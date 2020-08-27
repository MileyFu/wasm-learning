# 主要成分分析 (PCA) + SVG 案例

这个例子中，我们演示了如何对一个二维数据数组进行主成分分析(PCA)，然后在一个 SVG 图中绘制结果。看看[这里的实例](https://www.secondstate.io/demo/2020-tencentcloud.html).

> 用Rust绘制 SVG 图 , 我们使用了[Cetra]在[本文中](https://cetra3.github.io/blog/drawing-svg-graphs-rust/)描述的技巧(https://cetra3.github.io/blog/).

## 前期准备

如果还未安装，按照下面的简单教程在用于开发的电脑上 [安装 ssvmup](https://www.secondstate.io/articles/ssvmup/) 。

## 创建 WASM 字节码

```
$ ssvmup build
```

## 用于部署的包

```
$ cp pkg/pca_bg.wasm cloud/
$ cd cloud
$ zip pca.zip *
```

## 在腾讯云上部署 

[按照这里的教程进行](https://github.com/second-state/ssvm-tencent-starter/blob/master/README.md#deploy-on-tencentcloud)。确保在 web 控制台中的“执行处理程序”设置为 `pca_bg.wasm` 。

## 创建 web 服务

点击触发器管理，创建触发器。从列表中选择 API 网关触发器并提交.

![创建一个 web API trigger](docs/create.png)

一旦创建了 web API 触发器，就向下复制访问路径 URL。

![URL 以访问 web API](docs/access.png)

## Test测试

接下来，转到“ test”文件夹并使用“ curl”将 CSV 数据文件发布到访问路径 URL。它应该返回一个绘制输入2D 点以及两个主要组件的 SVG 图。

```
$ cd test
$ curl -d @iris.csv -X POST https://service-m9pxktbc-1302315972.hk.apigw.tencentcs.com/release/PCASVG
```

## Web app

通过访问路径 URL 提供的这个无服务器函数，我们现在可以为它构建一个网页前端。网页只是 HTML 和 JavaScript，可以托管在任何计算机上，包括你的本地笔记本电脑，因此是真正的无服务器。该网页使用 JavaScript 从云函数中请求 PCA 计算和 SVG 绘图。

[上线的网页](https://www.secondstate.io/demo/2020-tencentcloud.html) | [HTML 源代码](https://github.com/second-state/www/blob/master/themes/hugo-notepadium/static/demo/2020-tencentcloud.html)

注意: 你必须在腾讯无服务器云函数的 API 网关上 [启用 CORS](https://www.secondstate.io/articles/tencentcloud-api-gateway-cors/) ，从而让 JavaScript AJAX 呼叫成功。

