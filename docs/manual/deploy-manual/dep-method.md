# 部署方法 <!-- {docsify-ignore} -->

## 本地预览 <!-- {docsify-ignore} -->

将项目文件夹 `docs` 移动至目标位置后，打开命令行工具，通过运行 `docsify serve` 命令启动一个本地服务，可以方便地实时预览效果。
默认访问地址 <http://localhost:3000> 。

```bash
docsify serve docs
```

## 远程部署 <!-- {docsify-ignore} -->

### GitHub Pages <!-- {docsify-ignore} -->

GitHub Pages支持从三个地方读取文件
* `docs/` 目录
* master 分支
* gh-pages 分支

推荐直接将文档放在`docs/` 目录下，在设置页面开启 GitHub Pages 功能并选择 `master branch /docs folder` 选项。

![github-pages部署](../_media/deploy-github-pages.png "github-pages部署")  

> 可以将文档放在根目录下，然后选择 master 分支 作为文档目录。你需要在部署位置下放一个 .nojekyll 文件（比如 /docs 目录或者 gh-pages 分支）

### VPS <!-- {docsify-ignore} -->

和部署所有静态网站一样，只需将服务器的访问根路径设为 `index.html` 文件。

例如 nginx 的配置

```nginx
server {
  listen 80;
  server_name  your.domain.com;

  location / {
    alias /path/to/dir/of/docs/;
    index index.html;
  }
}
```

windows下常用的nginx命令：
```
start nginx  //启动nginx

nginx -s stop //快速停止

nginx -s quit //停止nginx并保存相关信息

nginx -s reload //配置信息修改后，重新载入nginx

nginx -s reopen //重新打开日志文件

nginx -v //查看nginx版本
```



