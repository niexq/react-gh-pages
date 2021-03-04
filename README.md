## 将React应用部署到GitHub Pages
参考[react-gh-pages](https://github.com/gitname/react-gh-pages)以及[create-react-app-Deployment-GitHub Pages](https://create-react-app.dev/docs/deployment/#github-pages)

![react-gh-pages](/react-gh-pages.png)
<!-- # <img src="/logo.png" title="react-gh-pages" alt="react-gh-pages logo" width="530"> -->

## 步骤
前提：本地已安装nodejs，已有github账户

- [1.在GitHub上创建一个空的repository](#step1)
- [2.使用create-react-app创建新的React应用](#step2)
- [3.重要，添加homepage到package.json](#step3)
- [4.重要，安装gh-pages并添加deploy到package.json的scripts中](#step4)
- [5.重要，将GitHub存储库添加为本地git存储库中的remote](#step5)
- [6.重要，通过运行```npm run deploy```部署到GitHub Pages](#step6)
- [7.对于项目页面，请确保项目设置使用```gh-pages```](#step7)
- [8.可选，配置域](#step8)
- [9.可选，将本地源代码提交推送到GitHub的```master```分支](#step9)
- [10.可选，故障排除](#step10)

### <span id="step1">1.在GitHub上[创建一个空的repository](https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/creating-a-new-repository)</span>
  + 输入自定义的Repository name
  + 其他为空，不用初始化README.md，.gitignore，license，保持repository干净无文件

### <span id="step2">2.使用[create-react-app创建新的React应用](https://github.com/facebook/create-react-app)</span>

```js
// npx
npx create-react-app react-gh-pages

// or npm
npm init react-app react-gh-pages

// or yarn
yarn create react-app react-gh-pages
```
### <span id="step3">3.**重要**，添加homepage到package.json</span>

打开项目，然后为项目package.json添加一个homepage字段：
```js

"homepage": "https://myusername.github.io/react-gh-pages",

// 或GitHub用户页面
"homepage": "https://myusername.github.io",

// 或自定义域页面：
"homepage": "https://mywebsite.com",

```
Create React App使用该homepage字段来确定所构建的HTML文件中的根URL。

### <span id="step4">4.**重要**，安装gh-pages并添加deploy到package.json的scripts中</span>

进入项目，安装gh-pages
```js
cd react-gh-pages
npm install gh-pages --save-dev
```

添加以下deploy脚本到项目的package.json中：
```js
  "scripts": {
+   "predeploy": "npm run build",
+   "deploy": "gh-pages -d build",
    "start": "react-scripts start",
    "build": "react-scripts build",
```

该predeploy脚本将自动运行在deploy运行之前。

（可忽略）如果要部署到GitHub用户页面而不是项目页面，则需要进行其他修改：

调整package.json脚本以将部署推送到master：
```js
  "scripts": {
    "predeploy": "npm run build",
-   "deploy": "gh-pages -d build",
+   "deploy": "gh-pages -b master -d build",
```

### <span id="step5">5.**重要**，将GitHub存储库添加为本地git存储库中的remote</span>

```js
git remote add origin https://github.com/myusername/react-gh-pages.git
```

### <span id="step6">6.**重要**，通过运行```npm run deploy```部署到GitHub Pages</span>

```js
npm run deploy
```


### <span id="step7">7.对于项目页面，请确保项目设置使用```gh-pages```</span>
确保将GitHub项目设置中的```GitHub Pages```选项设置为使用```gh-pages```分支：

![gh-pages-settings](/gh-pages-settings.gif)
![gh-pages-settings](/gh-pages-settings.png)
### <span id="step8">8.**可选**，[配置域](https://docs.github.com/en/github/working-with-github-pages/about-custom-domains-and-github-pages#supported-custom-domains)</span>
可以通过新增```CNAME```文件到```public/```目录来使用GitHub Pages配置自定义域。

CNAME文件应如下所示：
```js
mywebsite.com
```

### <span id="step9">9.**可选**，将本地源代码提交推送到GitHub的```master```分支</span>

```js
git add .
git commit -m "feat: Create a React app and publish it to GitHub Pages"
git push origin master
```

### <span id="step10">10.**可选**，故障排除</span>

+ 如果在部署时遇到```/dev/tty: No such a device or address```错误或类似错误，请尝试以下操作：
  + 创建一个新的[个人访问令牌](https://github.com/settings/tokens)
  + ```git remote set-url origin https://<user>:<token>@github.com/<user>/<repo>```
  + 再运行```npm run deploy```

+ 如果在部署时遇到```Cannot read property 'email' of null```，请尝试以下操作：
  + ```git config --global user.name '<your_name>'```
  + ```git config --global user.email '<your_email>'```
  + 再运行```npm run deploy```

+ 如果在部署时遇到```fatal: A branch named 'gh-pages' already exists.```，请尝试以下操作：
  + 命令行或手动删除node_modules/.cache/gh-pages
  + 再运行**npm run deploy**
  ```js
  rm -rf node_modules/.cache/gh-pages
  npm run deploy
  ```

### <span>最后</span>

如果操作了第7步配置了自定义域名域名，可访问：https://mywebsite.com/react-gh-pages

如未配置域名，访问：https://myusername.github.io/react-gh-pages

也可通过GitHub-settings-GitHub Pages查看：

![website](/website.png)