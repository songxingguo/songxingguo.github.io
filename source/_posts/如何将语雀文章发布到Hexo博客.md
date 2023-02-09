---
title: 如何将语雀文章发布到Hexo博客
urlname: gwpsg4dq6tp27z3s
date: '2023-02-07 05:15:12 +0000'
tags: []
categories: []
---

## 写在前面

实现语雀上的文章自动化发布到个人博客，首先是因为语雀本身是一个优秀的创作的平台，能方便的编辑和管理文章，但缺点是它本身是一个封闭的系统，就像微信公众号一样，不利于 SEO。其次个人博客是由自己搭建，没有任何限制，也能更好的进行 SEO。基于这两点原因，语雀+个人博客就是一个完美的组合，个人博客作为前台，语雀作为后台，既能能很好的创造和管理文章，有能够将需要共享的文章发布到个人博客，供大家搜索。下面要做的就是建立起两者的桥梁。

## 传送门

- [项目地址](https://github.com/songxingguo/songxingguo.github.io)
- 使用效果（视频）

## 设计思路

![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/Fv4SHStOd0NQNbe-P78SUpwLcqdJ.jpeg)
整体的设计思路是每次语雀更新文档都通过 Webhook 发送钉钉消息，当手动点击【发布】的时候再触发 GithubAction 将 slug 对应的文章从语雀拉取下去并推送到博客仓库，最后部署博客。触发流程虽然是从语雀到 Github 的过程，但开发的过程是优先处理 Github 部分然后再由语雀触发 Webhook，开发过程主要包括以下几个步骤。

1. 配置`yuque-heox-publish`
2. 配置`.github/workflows/publish.yml`
3. 使用云函数触发 GithubAction
4. 使用云函数推送钉钉消息
5. 配置语雀 Webhook

<!-- more -->

### `yuque-hexo-publish`是什么

`[yuque-hexo](https://github.com/x-cold/yuque-hexo)`是一个很优秀的插件，能将语雀的文章完全**同步**到个人博客上。但这并不满足我的需求，我需要的是发布而不是同步，我并不想把所有文章都**同步**到个人博客上，也不希望只能有一个知识库里面的文章能**发布**到个人博客，而是希望能把我想发布的文章都能**发布**到个人博客上。基于这个这个考虑我在`yuque-hexo`的基础上新增了发布功能，`yuque-hexo-publish`就此诞生了。yuque-hexo-publish 在功能上和 yuque-hexo 是完全一致的，只是新增了命令 publish。

## 具体实施

### Hexo 博客开发

#### 项目配置

1. `package.json`中配置 yuque-hexo-publish。

```yaml
  "yuqueConfig": {
    "postPath": "source/_posts", //文章目录
    "baseUrl": "https://www.yuque.com/api/v2",
    "login": "songxingguo",//用户名
    "repo": "devhints", //知识库名称
    "onlyPublished": false,
    "onlyPublic": false,
    "imgCdn": {
      "concurrency": 1,
      "imageBed": "github", //图床类型
      "enabled": true,
      "bucket": "songxingguo.github.io", //仓库地址
      "prefixKey": "static/images" //图片文件上传到仓库后的目录
    }
  }
```

更详细的配置说明可以查看`[yuque-hexo-publish](https://github.com/songxingguo/yuque-hexo/tree/feature-publish)`。

2. `package.json`中添加任务脚本。

```bash
  "scripts": {
    "build": "hexo generate",
    "pulish": "yuque-hexo publish",
  },
```

3. 根目录下新建` .github/workflows/publish.yml`。

```yaml
name: Deploy To Github Pages
on: [repository_dispatch]
jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout 🛎️
        uses: actions/checkout@v3
        with:
          persist-credentials: false

      - name: Setup Node.js
        uses: actions/setup-node@master
        with:
          node-version: "12.0.0"

      - name: Install and Build 🔧
        env:
          YUQUE_TOKEN: ${{ secrets.YUQUE_TOKEN }}
          SLUG: ${{ github.event.client_payload.slug }}
          SECRET_ID: songxingguo
          SECRET_KEY: ${{ secrets.ACCESS_TOKEN }}
        run: |
          npm i
          npm run publish
          npm run build

      - name: 配置Git用户名邮箱
        run: |
          git config --global user.name "songxingguo"
          git config --global user.email "1328989942@qq.com"

      - name: 提交yuque拉取的文章到GitHub仓库
        run: |
          git pull
          git add .
          git commit -m "feat:提交文章" -a

      - name: 推送文章到仓库
        uses: ad-m/github-push-action@master
        with:
          github_token: ${{ secrets.ACCESS_TOKEN }}

      - name: Deploy 🚀
        uses: JamesIves/github-pages-deploy-action@v4
        with:
          ACCESS_TOKEN: ${{ secrets.ACCESS_TOKEN }}
          BRANCH: master
          FOLDER: public
          CLEAN: true
```

`YUQUE_TOKE`和`ACCESS_TOKEN`都是 Github 的环境变量，`github.event.client_payload.slug`是`[repository_dispatch](https://docs.github.com/zh/actions/using-workflows/events-that-trigger-workflows#repository_dispatch)`事件中传递的参数。

#### 如何本地调试

```bash
YUQUE_TOKEN=xxx SLUG=xxx yuque-hexo publish
```

`yuque-hexo publish `需要两个环境变量，`YUQUE_TOKEN`是语雀的[授权码](https://www.yuque.com/settings/tokens)，`SLUG`是语雀文章的 id。

### 阿里云函数开发

#### 初始化云函数

1. 创建一个处理 http 请求的 node.js 云函数，[云函数创建地址](https://fcnext.console.aliyun.com/cn-hangzhou/tasks)。

![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/FmgXhivkd1GDSXivmeXk4GFgJ5mW.png)

2. 创建好后的效果如下。

![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/Fn9FBWVuiHojlCDdNBudsnalwEXs.png)

#### 调试云函数

1. 配置测试请求。

![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/FrNF5Qqv5Z8c-GdIRsTEUU9fR6Rq.png)

2. 部署代码之后，点击【测试函数】，就可以看到日志输出。

![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/Fm13C_KkS6MOfXCgdToHQqHVS7-p.png)

#### 使用云函数触发 GithubAction

这个函数是通过 Get 请求进行调用的。

```javascript
https://xx-xx-opbwmocbhe.cn-hangzhou.fcapp.run?slug=${slug}`
```

首先需要从参数中获取 slug。

```javascript
const slug = params.queries.slug;
```

然后使用`[repository_dispatch](https://docs.github.com/zh/actions/using-workflows/events-that-trigger-workflows#repository_dispatch)`事件触发 GithubAction，并将`slug`传给`client_payload`。

```javascript
const axios = require("axios");

const options = {
  method: "POST",
  url: "https://api.github.com/repos/songxingguo/songxingguo.github.io/dispatches",
  headers: {
    "content-type": "application/json",
    Accept: "application/vnd.github+json",
    Authorization: "Bearer GITHUB_TOKEN",
  },
  data: { event_type: "publish", client_payload: { slug } },
};

axios
  .request(options)
  .then(function (response) {
    console.log(response.data);
  })
  .catch(function (error) {
    console.error(error);
  });
```

其中`GITHUB_TOKEN`是 github 的授权码，获取和配置可以参考[使用 Github Action 部署静态网站](https://www.yuque.com/songxingguo/devhints/tfub27hk86lsdrpb)。`event_type`是活动类型这个和`.github/workflows/publish.yml`里面的`types`是一一对应的，如果`.github/workflows/publish.yml`中没有指定具体`types`，那这儿填任何值都是可以。具体信息可以查看[详细配置](https://docs.github.com/zh/developers/webhooks-and-events/webhooks/webhook-events-and-payloads#repository_dispatch)。
![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/FveEzFT5TggObDiemqVIn5zlRzAd.png)
完整代码如下。

> 需要注意 ⚠️ 异步执行的问题，可以通过`async-await`保证请求是在`resp.send`之前执行的。

```javascript
var axios = require("axios");
var getRawBody = require("raw-body");
var getFormBody = require("body/form");
var body = require("body");

/*
To enable the initializer feature (https://help.aliyun.com/document_detail/156876.html)
please implement the initializer function as below：
exports.initializer = (context, callback) => {
  console.log('initializing');
  callback(null, '');
};
*/

exports.handler = (req, resp, context) => {
  var params = {
    path: req.path,
    queries: req.queries,
    headers: req.headers,
    method: req.method,
    requestURI: req.url,
    clientIP: req.clientIP,
  };

  getRawBody(req, async (err, body) => {
    for (var key in req.queries) {
      var value = req.queries[key];
      resp.setHeader(key, value);
    }
    resp.setHeader("Content-Type", "text/plain");
    params.body = body.toString();
    const slug = params.queries.slug;
    console.log("slug", slug);

    const options = {
      method: "POST",
      url: "https://api.github.com/repos/songxingguo/songxingguo.github.io/dispatches",
      headers: {
        "content-type": "application/json",
        Accept: "application/vnd.github+json",
        Authorization: "Bearer xxx",
      },
      data: { event_type: "publish", client_payload: { slug } },
    };

    await axios
      .request(options)
      .then(function (response) {
        console.log(response.data);
      })
      .catch(function (error) {
        console.error(error);
      });

    resp.send(JSON.stringify(params, null, "    "));
  });
};
```

#### 使用云函数推送钉钉消息

##### 调试钉钉消息类型及数据格式

1. 创建钉钉机器人，并复制 webhook 地址。

![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/FhiBRGA1W_Z4-GzqzjiH5esIggKG.png)

2. 打开[在线接口调试工具](https://hoppscotch.io/)，输入 Webhook 地址，配置钉钉[消息类型及数据格式](https://open.dingtalk.com/document/robots/custom-robot-access#title-72m-8ag-pqw)。

![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/FqEYt6cimbEW4pJDET2YEsevO3bQ.png)

3. 调试到自己想要的效果后复制请求的代码。

![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/FqxbJqsBt0_l1REj_8luZiqzFBxH.png)

---

这个函数是通过 post 请求进行调用，首先需要从请求体中获取`slug`和`title`。

```javascript
const data = JSON.parse(params.body);
const { slug, title } = data.data;
```

然后再请求钉钉机器人的 Webhook 地址。

```javascript
const axios = require("axios");

const options = {
  method: "POST",
  url: "https://oapi.dingtalk.com/robot/send",
  params: {
    access_token: "",
  },
  headers: { "content-type": "application/json" },
  data: {
    msgtype: "actionCard",
    actionCard: {
      title: "文档发布",
      text: `${title}`,
      btnOrientation: "0",
      btns: [
        {
          title: "发布",
          actionURL: `https://xx-xx-xx.cn-hangzhou.fcapp.run?slug=${slug}`,
        }, // 触发GithubAction云函数的公网地址
        {
          title: "Actions",
          actionURL:
            "https://github.com/songxingguo/songxingguo.github.io/actions",
        },
      ],
    },
  },
};

axios
  .request(options)
  .then(function (response) {
    console.log(response.data);
  })
  .catch(function (error) {
    console.error(error);
  });
```

其中`access_token `为钉钉 webhook 地址中的授权码。

```javascript
https://oapi.dingtalk.com/robot/send?access_token=xx
```

最终的效果如下所示。
![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/FgwZMGzVe6xjsIMMsYyDrVxql0ku.png)
完整代码如下。

```javascript
var getRawBody = require("raw-body");
var getFormBody = require("body/form");
var body = require("body");
var axios = require("axios");

/*
To enable the initializer feature (https://help.aliyun.com/document_detail/156876.html)
please implement the initializer function as below：
exports.initializer = (context, callback) => {
  console.log('initializing');
  callback(null, '');
};
*/

exports.handler = (req, resp, context) => {
  console.log("hello world");

  var params = {
    path: req.path,
    queries: req.queries,
    headers: req.headers,
    method: req.method,
    requestURI: req.url,
    clientIP: req.clientIP,
  };

  getRawBody(req, async (err, body) => {
    for (var key in req.queries) {
      var value = req.queries[key];
      resp.setHeader(key, value);
    }
    resp.setHeader("Content-Type", "text/plain");
    params.body = body.toString();

    const data = JSON.parse(params.body);
    const { slug, title } = data.data;
    console.log("slug", slug);

    const options = {
      method: "POST",
      url: "https://oapi.dingtalk.com/robot/send",
      params: {
        access_token: "",
      },
      headers: { "content-type": "application/json" },
      data: {
        msgtype: "actionCard",
        actionCard: {
          title: "文档发布",
          text: `${title}`,
          btnOrientation: "0",
          btns: [
            {
              title: "发布",
              actionURL: `https://xx-xx-xx.cn-hangzhou.fcapp.run?slug=${slug}`,
            },
            {
              title: "Actions",
              actionURL:
                "https://github.com/songxingguo/songxingguo.github.io/actions",
            },
          ],
        },
      },
    };

    await axios
      .request(options)
      .then(function (response) {
        console.log(response.data);
      })
      .catch(function (error) {
        console.error(error);
      });

    resp.send(JSON.stringify(params, null, "    "));
  });
};
```

#### 配置语雀 Webhook

进入语雀[webhook 配置页面](https://www.yuque.com/songxingguo/devhints/settings/webhooks)，填写名称和**推送钉钉消息**云函数的公网地址，并选择**更新文档**时触发。
![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/Fmtzly3_0hghg_uvqaK6eDgmsT-M.png)

## 其他

### [Hoppscotch](https://hoppscotch.io/)

Hoppscotch 和 postman 的功能是一样的，但这个是线上的版本，更加的方便。有时请求会出现请求无法到达的问题，需要配置插件代理，将请求的域名加入到插件中。
![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/FkPke4qlMP1Zv2PALiW8f-dAATr5.png)

## [拓展阅读](https://www.yuque.com/songxingguo/devhints/gwpsg4dq6tp27z3s)

[Github Actions 持续集成 Docker 构建并部署 Node 项目到云服务器](https://www.yuque.com/1874w/1874.cool/ovugli?view=doc_embed)

[yueque-hexo 图床配置](https://github.com/LetTTGACO/yuque-hexo-example)

[语雀云端写作 Hexo+Github Actions+COS 持续集成](https://www.yuque.com/1874w/1874.cool/roeayv?view=doc_embed)
[Webhooks](https://www.yuque.com/yuque/developer/doc-webhook?view=doc_embed)
