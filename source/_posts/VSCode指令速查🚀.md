---
title: "VSCode指令速查\U0001F680"
urlname: gieh2cbsygtk7ars
date: '2023-07-08 07:27:53 +0000'
tags: []
categories: []
---

![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/FmUabHtqOAPi4hA9p85mwe7O3n0i.jpeg)
工欲善其事，必先利其器。VSCode 作为一款集成开发工具具有轻量级，高度可定制化的特点。它拥有各种插件，并且免费，非常适合喜欢折腾的同学。而本篇文章就是为大家推荐一些 VSCode 使用过程中的最佳实践，通过这些实践，希望能帮助大家进步提供开发效率。下面我将从编辑器的前期设置和后期开发两个角度展开。

# 设置

首先在进行具体设置之前，我们要对 VSCode 设置有一个初步的认识。VSCode 的设置分为用户设置和工作区设置。

- 用户设置是一个全局范围的设置，会应用到所有的 Visual Studio Code 实例中。
- 工作区设置存放于每个项目`.vscode `文件夹下的 settings.json 文件中，只会对相应的工作区生效。一个工作区内是可以包含多个项目的。工作区的设置优先级高于用户设置，两者同时存在时，工作区设置会覆盖掉用户设置。此外，可以通过工作区设置在团队成员之间共享。一般来说，工作区设置也会被提交到 Git 仓库中。

设置可以使用快捷`Cmd+,` 的方式打开，也可以切换到 JSON 格式进行快速编辑。

## 快捷键

快捷能很大程度上提高我们的开发效率，那么对快捷键的配置也是不可少的部分，但快捷键的记忆却不是一件容易事情，下面将通过使用场景对快捷键进行分类整理方式，来提高快捷键的记忆效率。

### 常用快捷键</div>warning

打开快捷编辑面板：`Cmd+K→Cmd+S`</div>
| **Mac 快捷键** | **描述** | **来源** |
| --- | --- | --- |
| **通用快捷键** | | |
| `Cmd+,` | 打开用户设置 | |
| `Cmd+Shift+P或F1` | 打开命令面板 | |
| **窗口** | | |
| `Cmd+Shift+N` | 新建一个 Visual Studio Code 窗口 | |
| `Cmd+W` | 关闭当前窗口 | |
| `Cmd+Shift+T` | 重新打开最新关闭的窗口 | |
| **跳转** | | |
| `Cmd+P` | 文件跳转 | 用户 |
| `Option+←/→` | 向后/向前跳转 | |
| **基本编辑** | | |
| `Cmd+X` | 剪切当前行（当没有选定任何文本时） | |
| `Cmd+C` | 复制当前行（当没有选定任何文本时） | |
| `Option+↑/↓` | 把当前行的内容向上/下移动 | |
| `Shift+Option+↑/↓` | 把当前行的内容向上/下复制 | |
| `Cmd+/` | 添加或删除当前行的注释 | |
| **编程语言编辑** | | |
| `Cmd+Shift+F` | 格式化文档 | 用户 |
| `Cmd+K Cmd+F` | 格式化选定内容 | |
| **搜索与替换** | | |
| `Cmd+F` | 文件搜索 | |
| `Cmd+H` | 文件替换 | 用户 |
| `Cmd+Shift+F` | 文件搜索 | 用户 |
| `Cmd+Shift+H` | 全局替换 | |
| **多光标与选择** | | |
| `Option+Click` | 插入一个新的光标 | |
| `Shift+Option+→` | 扩大选中的范围 | |
| `Shift+Option+←` | 缩小选中的范围 | |
| `Option+←/→` | 光标移动到当前行的起始/末尾 | |
| **显示** | | |
| `Ctrl+Cmd+F` | 切换全屏模式 | |
| `Cmd+=` | 放大 | |
| `Cmd+-` | 缩小 | |
| **编辑器管理** | | |
| `Cmd+\\` | 分割编辑器 | |
| `Cmd+1/2/3` | 把焦点移动到不同的编辑器组 | |
| **文件管理** | | |
| `Cmd+N` | 新建文件 | |
| `Cmd+O` | 打开文件 | |
| `Cmd+S` | 保存 | |
| `Cmd+Shift+S` | 另存为 | |
| `Ctrl+Shift+←/→` | 打开前一个/下一个文件 | |

### 显示用户快捷键

很多时候默认的快捷键，并不能很好的满足我们的使用习惯，就需要对快捷进行调整，具体哪些快捷键进行了修改就可以通过下面的筛选显示出来。
![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/Fn0QyQvmIwHUa7uznVRiqNZSfZ0q.png)

### 快捷键大全</div>warning

打开快捷键大全：`Cmd+K→Cmd+R`</div>

## 插件

VSCode 的[插件市场](https://marketplace.visualstudio.com/vscode)纷繁复杂，种类繁多，但不是每一个插件都是我们需要的，下面就为大家列举出了一些常用并且好用的插件供大家选择。

| **插件**                                                                                           | **描述**                                                                                        | **备注** |
| -------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- | -------- |
| `Chinese (Simplified)`                                                                             | 简体中文                                                                                        |          |
| **HTML 必备**                                                                                      |                                                                                                 |          |
| `Auto Close Tag`                                                                                   | 标签自动闭合                                                                                    |          |
| `Auto Rename Tag`                                                                                  | 标签自动重命名                                                                                  |          |
| `Highlight Matching Tag`                                                                           | 高亮对应 HTML 标签 & 表示对应括号                                                               |          |
| `Live Server`                                                                                      | 代码保存后，浏览器自动更新                                                                      |          |
| **Vue 必备**                                                                                       |                                                                                                 |          |
| `[Vue Language Features (Volar)](https://marketplace.visualstudio.com/items?itemName=Vue.volar)`🌟 | `Vetur`的升级版。提供语法高亮，Emmet 支持，格式化代码，错误检查，自动补全等功能，以及高级功能。 |
| [Volar - vue 终极开发神器！ - 掘金](https://juejin.cn/post/6966106927990308872)                    |

- [vuejs.org](https://vuejs.org/) 出品
  |
  | [TypeScript Vue Plugin (Volar)](https://marketplace.visualstudio.com/items?itemName=Vue.vscode-typescript-vue-plugin)🌟 | | |
  | `[Vetur](https://marketplace.visualstudio.com/items?itemName=octref.vetur)` | 提供语法高亮，Emmet 支持，格式化代码，错误检查，自动补全等功能 | |
  | `[Vue VSCode Snippets](https://marketplace.visualstudio.com/items?itemName=sdras.vue-vscode-snippets)` | | |
  | **代码检查** | | |
  | `[ESlint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)` | JavaScript 代码检查 | [一文彻底读懂 ESLint - 掘金](https://juejin.cn/post/6959825653029928968) |
  | `[stylelint](https://marketplace.visualstudio.com/items?itemName=stylelint.vscode-stylelint)` | css/sass/less 代码检查 | |
  | **代码格式化** |
  | |
  | `[Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)`🌟 | 格式化插件
  ![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/FodBLJYMqXmNo2Y-I-aUvTMfC-8y.png) | |
  | `Beautify` | 格式化插件
  支持语言：Javascript, JSON, CSS, Sass, and HTML | |
  | **智能提示** | | |
  | `npm Intellisense` | npm 智能提示 | |
  | `Path Intellisense` | 路径智能提示，自动导入 | |
  | `path prompt` | 路径图片预览 | |
  | **Git** | | |
  | `[GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens)`🌟 | 预览每一行的提交记录以及代码提交树 |
- [gitkraken.com](https://gitkraken.com/) 出品
  |
  | `[Git History](https://marketplace.visualstudio.com/items?itemName=donjayamanne.githistory)` | 文件提交历史
  | |
  | `[Git Graph](https://marketplace.visualstudio.com/items?itemName=mhutchie.git-graph)` | 可视化显示 Git 提交树
  ![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/FiHlwVkc14NgOGjiXMI77K44ojVO.gif) | |
  | **Github** |
  | |
  | `[GitHub Actions](https://marketplace.visualstudio.com/items?itemName=GitHub.vscode-github-actions)` | | |
  | `GitHub Copilot` | | 收费 |
  | **AI** | | |
  | [ChatGPT - 中文版](https://marketplace.visualstudio.com/items?itemName=WhenSunset.chatgpt-china) | | |
  | **主题** | | |
  | [Catppuccin Icons for VSCode](https://marketplace.visualstudio.com/items?itemName=Catppuccin.catppuccin-vsc-icons) 🌟 | 文件图标
  ![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/Fj6r3ym70-24GVQYX1ZiMsaKmrZM.png) | |
  | `[vscode-icons](https://marketplace.visualstudio.com/items?itemName=vscode-icons-team.vscode-icons)` | 文件图标
  ![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/Fpc4XocfmcQRVAhTLY-9Z7awzK7B.gif) | |
  | `[Bracket Pair Colorizer](https://marketplace.visualstudio.com/items?itemName=CoenraadS.bracket-pair-colorizer-2)`🌟 | 彩色的括号
  ![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/FvAnIUgBzX957oTMiX8O7SlY4J7r.png) |
- 已经内置，只需要开启
  |
  | `[Webstorm IntelliJ Darcula Theme](https://marketplace.visualstudio.com/items?itemName=xr0master.webstorm-intellij-darcula-themehttps://marketplace.visualstudio.com/items?itemName=xr0master.webstorm-intellij-darcula-theme)`🌟 | Webstorm 一样的主题
  ![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/FiCBqMi2xDUc2BlPfWsfwIbM0txs.png) | |
  | **其他** | | |
  | `[background](https://marketplace.visualstudio.com/items?itemName=shalldie.background)`🌟 | 界面右下角有个小人 | |
  | `carbon-now-sh` | 截获代码为 PNG，Ctrl+Shift+P => Carbon | |
  | `Settings Sync` | 设置同步 | |

## 同步设置

对于开发来说一般都会有两台电脑，一台在公司，一台在家里，当我们辛苦的在公司电脑上的配置好了快捷键和插件，难道回到家中还需要再重新配置一次吗？当然是不需要的，我们可以通过官方账号的同步机制来进行多台设备间配置的同步，具体步骤如下。

1. 首先登录账号并打开设置同步功能。

![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/Fq4r1p4Vnom5fGQ9_uK-2amxfziP.png)

2. 然后选择要需要同步的内容。

![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/FoSiG5lJ_-eNJ8DfF4xR1S21TPAH.png)</div>warning
【**选项含义**】

- **Settings**（配置）
- **Keyboard Shortcuts**（快捷键）
- **Extensions**（插件）
- **User Snippets**（用户代码片段）
- **UI State**（界面状态）</div>

3. 最后，在另外一台电脑上登录同一个账号，就可以同步电脑上的所有配置内容。

[VSCode 官方的配置同步方案 - 掘金](https://juejin.cn/post/7066622158184644621)

## 统一环境

当团队成员之间进行协同开发的时候，往往会遇到大家的配置不同问题，比如格式化的方式不同，就会导致最后代码大面积冲突。为了解决这个问题就可以采用上文提到的工作区设置来实现不同团队成员间配置的同步。具体步骤如下。

1. 首先，在 `.vscode`下创建文件`extensions.json`和` settings.json`，并提交代码到 Git 仓库。

![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/FjSuc42FQacUBzzKlK6XlkxltVmr.png)

```json
// 示例代码
{
  "recommendations": [
    "dbaeumer.vscode-eslint",
    "esbenp.prettier-vscode",
    "octref.vetur",
    "formulahendry.auto-close-tag",
    "formulahendry.auto-rename-tag"
  ]
}
```

```json
// 示例代码
{
  "eslint.autoFixOnSave": true
}
```

2. 然后项目成员拉取代码后，在搜索框中输入`@recommended`就会显示工作区推荐的插件，项目成员下载并安装好这些插件，就能实现不同成员之间开发环境的统一。

![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/FsVd2-8e-YYq34wWo_xFmsRNSlqP.png)
[如何在 vscode 中共享团队间的项目配置和插件配置](https://lleohao.github.io/2019/09/23/%E5%A6%82%E4%BD%95%E5%9C%A8-vscode-%E4%B8%AD%E5%85%B1%E4%BA%AB%E5%9B%A2%E9%98%9F%E9%97%B4%E7%9A%84%E9%A1%B9%E7%9B%AE%E9%85%8D%E7%BD%AE%E5%92%8C%E6%8F%92%E4%BB%B6%E9%85%8D%E7%BD%AE/)

# 开发

## 自定义代码模版

在开发的过程中，往往我们会遇到一些重复的代码，比如创建一个初始的 Vue 文件，如果每次都手敲那是很慢的，我们就可以把那些经常使用的代码封装乘代码片段，通过命令来快速创建。下面是定义代码片段的具体步骤。

1. 首先，执行`Ctrl+Shift+P` 打开命令面板，输入`Snippets`，并选择创建代码片段。

![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/FuGFe-i5kAdbV5ptIYI8eHYCaIqT.png)
![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/FpYWAEf2pqBvSan_Y_oJgcXjgj3F.png)

2. 然后输入代码片段并保存。

````json
{
	"vue template": {
		"prefix": "vuem",
		"body": [
			"<template>",
			" <div>",
			" ",
			" </div>",
			"</template>",
			"<script>",
			" export default {",
			"   name:'$1',",
			"   data () {",
			"     return {",
			" ",
			"     }",
			"   },",
			"   computed:{",
			" ",
			"   },",
			"   methods:{",
			" ",
			"   },",
			"   components: {",
			" ",
			"   },",
			" }",
			"</script>",
			"<style scoped>",
			" ",
			"</style>",
			""
		],
		"description": "vue template"
	}
}
```</div>warning
【**字段含义**】

- vue template ：当前snippet名字。
- prefix ：前缀，代码块使用快捷方式；键入前缀，按tab键，代码块就会被使用。
- body ：代码块内容；换行使用\r\n。
- description ：键入前缀，vscode 感知到前缀，显示的说明内容。
- $1,$2,$0 ：指定代码模块生成后，编辑光标出现位置; 使用Tab键进行切换(编辑光标按$1,$2,$3...$0的顺序跳转)，$0是光标最后可切换位置。</div>

3. 最后，新建.vue文件，输入 `vuem+tab` 或 `vuem+enter` 即生成模板。

[Snippet语法](https://juejin.cn/post/6844903912068104199)

## Emmet
Emmet 语法对于开发HTML代码是非常高效的，但它不能直接在VSCode中使用，需要安装具体的插件。`[Vue Language Features (Volar)](https://marketplace.visualstudio.com/items?itemName=Vue.volar)`插件就能很好的支持Emmet语法。

1. 首先，我们要在VSCode中安装一下`[Vue Language Features (Volar)](https://marketplace.visualstudio.com/items?itemName=Vue.volar)`。
2. 其次，我们需要认真学一下Emmet常用语法。

[Emmet](https://www.yuque.com/songxingguo/devhints/emmet)

## 多项目工作区
在很多时候我们都会面临同时开发多个项目情况，比如前端开发既要开发小程序端又要开发Web端这两个项目。如果我们打开两个VSCode窗口进行开发，在不同窗口间切法，显然会降低我们开发的效率。于是我们就可以考虑VSCode的多项目工作区，将多个项目同时放到一个工作区里面进行开发，减少了来回切换的麻烦。下面是具体的做法。

1. 可以通过拖拽或选择的方式将文件夹添加都工作区。
2. 也可以将文件夹移出工作区。

![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/FlW36F0Wu9ys3wZk7v3UC5Gfs6pA.png)

## 终端
在开发的过程中，很多事情我们都需要通过终端进行处理，比如安装依赖，提交代码等。所以能快速唤起终端并且能对终端进行一些基本的调整就显得很有必要了。

| **Windows快捷键** | **Mac快捷键** | **描述** |
| --- | --- | --- |
| `Ctrl+Shift+C` | `Cmd+Shift+C` | 打开新的外部终端 |
|
 | `ctrl+`` | 显示集成终端 |
|  | `ctrl+Shift+`` | 创建新的集成终端 |

![](https://raw.githubusercontent.com/songxingguo/songxingguo.github.io/hexo/static/images/FtCAGIQE7OFvfxv3GjgAOlM1XkqI.png)

## //调试与运行

## //重构代码

# 扩展阅读

- [配置前端环境](https://medium.com/@thoamsy/%E4%BD%BF%E7%94%A8-vs-code-%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E5%BE%88%E7%88%BD%E7%9A%84%E5%89%8D%E7%AB%AF%E7%8E%AF%E5%A2%83-2d393ba5cc45)
- [快捷键配置](https://juejin.im/post/6844903826063884296)
- [keyboard-shortcuts-macos.pdf](https://www.yuque.com/attachments/yuque/0/2023/pdf/394169/1688802091554-3f696b9a-257b-4b0f-a329-5aea195cda5a.pdf?_lake_card=%7B%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2023%2Fpdf%2F394169%2F1688802091554-3f696b9a-257b-4b0f-a329-5aea195cda5a.pdf%22%2C%22name%22%3A%22keyboard-shortcuts-macos.pdf%22%2C%22size%22%3A206761%2C%22ext%22%3A%22pdf%22%2C%22source%22%3A%22%22%2C%22status%22%3A%22done%22%2C%22download%22%3Afalse%2C%22taskId%22%3A%22uf72ca31a-19b2-468d-8083-ed87052d835%22%2C%22taskType%22%3A%22transfer%22%2C%22type%22%3A%22application%2Fpdf%22%2C%22mode%22%3A%22title%22%2C%22id%22%3A%22O4kBW%22%2C%22card%22%3A%22file%22%7D)
- [vsCode 配置](https://www.cnblogs.com/chenguangliang/p/12034297.html)
- [vscode-哔哩哔哩_Bilibili](https://search.bilibili.com/all?vt=03553703&keyword=vscode&from_source=webtop_search&spm_id_from=333.1007&search_source=5)
- [Vscode真香插件，前端效率神器！！_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1kb4y1y7zr/?spm_id_from=333.337.search-card.all.click&vd_source=19a3d7390d7927695ad503a1d8bcb0ad)
- [001.课程介绍_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1G24y177Um?p=1&vd_source=19a3d7390d7927695ad503a1d8bcb0ad)
- [Emmet-HTML/CSS代码自动补全语法](https://code.z01.com/Emmet/)
- [Visual Studio Code 权威指南](https://book.douban.com/subject/35125617/)
- [Visual Studio Code 权威指南-韩骏-微信读书](https://weread.qq.com/web/bookDetail/7bc32db071f02f257bc2a8a)
- [Documentation for Visual Studio Code](https://code.visualstudio.com/docs)
- [《Visual Studio Code 权威指南》](https://www.yuque.com/songxingguo/devhints/ngrliymei9ng7nm4?view=doc_embed)

---

最后，重学前端，将思考固定下来。

- 笔记和记忆力是提高开发效率的最好方法。
- 如果没有对旧事物进行大量练习，你不太可能发现新事物。
- 努力学习最感兴趣的东西。
>  ©️版权申明：版权所有@宋玉，本文内容仅供学习，欢迎指正、交流，转载请注明出处！[原文地址-语雀](https://www.yuque.com/songxingguo/devhints/gieh2cbsygtk7ars)

````
