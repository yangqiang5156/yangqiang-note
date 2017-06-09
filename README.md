## 风林火山的笔记
gitbook使用

```bash
npm install gitbook-cli -g
mkdir my-note
cd my-note
gitbook init
gitbook serve
然后在localhost:4000就可以看到
```

```bash
项目中添加了npm scripts
发布项目可通过下面命令
npm run build
npm run deploy
运行后会通过gh-pages同时push到github的ph-pages 分支
浏览器输入yangqiang5156.github.io/yangqiang-note 即可查看
```

 gh-pages插件用于把gh-pages文件下的内容快速push到github的gh-pages分支，方便外网访问预览