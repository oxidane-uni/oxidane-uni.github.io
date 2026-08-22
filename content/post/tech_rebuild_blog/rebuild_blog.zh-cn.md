---
title: 使用 Hugo 对博客的重建与 Stack 主题优化记录
date: 2023-04-22T14:56:05+08:00
description: 你不觉得图文无关很酷么？
image: ./images/4featureimg/azl.jpg
comments: true
math: true
categories: 
  - 技术
tags:
  - Hugo
draft: false
---

## 为何再次重建博客？  
是，我知道我上次更新博客是 2021 年，但是这个博客我的确是没有忘的。~~（主播只是入狱了）~~  
实际上这个博客是我在 21 年暑假用Hugo建的，使用的是 Eureka 主题。整体效果确实非常不错，但遗憾的是配置过程极其奇怪：虽说有专门的[说明文档](https://www.wangchucheng.com/zh/docs/hugo-eureka/),但是于我而言这玩意写得实在是不太友好，所以当时我的配置过程纯粹靠蒙，虽说最后还是搞出来了，但确实是一种折磨。  
正好最近几天又想写博客了，就把 Hugo 又下了回来，结果升级 Eureka 主题的时候又折磨了好久，还是没整明白。

> 使用git submodule下来的Eureka配置后存在的大量报错，不断提示module未定义，反正也看不懂，干脆换主题了。  
  
因此本次的折腾就再走一遍之前的路，从零开始重建博客，顺带记录下以防之后忘记。  

## Hugo之安装
> “Hugo是由Go语言实现的静态网站生成器。简单、易用、高效、易扩展、快速部署。 ” 

本次我使用的是 Hugo v0.111.2 与 git v2.38.0 ，操作系统为 Win 11。在 Hugo 的[releases](https://github.com/gohugoio/hugo/releases)和 Git 的[官网](https://git-scm.com/)都能很方便地下载。我选择的是 hugo_extended_0.111.2_windows-amd64.zip
版本，以便后续折腾。下载完成后将压缩包里的东西解压至你喜欢的目录，再将该目录的路径添加到环境变量中以便后续调用。至于 Git 的安装，我就不再赘述了。  
![PATH](./images/4post/230422/path.png)  

## Hugo建站

### 新建网站
运行 Hugo.exe 后，输入:  
`hugo new site ./path   //比如我这里就是./exnadio.github.io `  
运行后便会输出一个网站目录，其结构为（引用自[炸鸡人博客](https://zhajiman.github.io/post/rebuild_blog)）：
```
.
├── archetypes      # 存放文章模板
├── config.toml     # 简单的配置文件
├── content         # 存放文章
├── data            # 存放生成静态页面时的配置文件
├── layouts         # 存放页面布局的模板
├── static          # 存放图片等静态内容
└── themes          # 存放下载的主题
```
此时便搭好了一个基础框架，可以进行主题的导入了。

### 导入主题  
这一步你就可以打开 [Hugo Themes](https://themes.gohugo.io/) 这个网站，选择一个你喜欢的主题，然后跟随着它的说明一步一步配置就大功告成了。~~那么我们下期再见~~  
实际上我认为这步是整个 Hugo 建站中最复杂的一步。且每个人选择的主题不尽相同，很难以一个完善的教程（何况我这篇是否算教程也有待考虑）通杀一切，我就分享下我这次的经历好了。
安装主题一般而言存在三种方式：  
- git submodule 安装
- go module 安装（需要安装 Go 语言）
- 本地安装

我个人更推荐第一种方式，考虑到后续升级的难易，这算是最均衡的一种方式。具体的安装方法可以在各主题的说明中找到，我这里安装的是[ Stack](https://stack.jimmycai.com/)。 
在网站目录下，输入： 
```
git init
git submodule add https://github.com/CaiJimmy/hugo-theme-stack/ themes/hugo-theme-stack
```  
等待下载完成后，便可以进行[配置](##主题配置历程)了。假如你想用其他方式安装，也可以参考[这里](https://stack.jimmycai.com/guide/getting-started)。  
Stack本身有全英文的[说明文档](https://stack.jimmycai.com/config/)，我建议是将`./themes/hugo-theme-stack/exampleSite/content`和`./themes/hugo-theme-stack/config.yaml`直接夺舍，根据说明与需求修改，会剩下很多时间。  
> 根据 Stack 的说明文档，Stack 后续将改用.toml格式的 Config 文件，不过其配置步骤基本相同。 

### 预览网站  
很多教程会先教创建文章，不过我觉得配置完主题后还是先预览网站为好。Hugo的一大好处就是可以进行即时预览，你可以看到你每一步修改所产生的变化，也可以看到你是如何把一切搞崩溃的。
输入:  
```
hugo server
``` 
即可建立一个本地服务器。  
![Hugo server](./images/4post/230422/hugoserver.png)  
当然你也可以加上一些 flag 来达到一些其他目的：比如指定主题的`--theme=Stack`和关闭快速渲染的`--disableFastRender`等，具体可以查阅文档。  
之后便可以在浏览器里打开`http://localhost:1313`预览。

### 创建文章  
Hugo 中的文章采用 Markdown 格式，通过以下命令你可以在`./content/post`路径下创建一篇文章：
`hugo new post/rebuild_blog.md`
打开后你会发现生成的Markdown文档带有一段FrontMatter部分，具体的意思可以在[这里](https://stack.jimmycai.com/writing/frontmatter)找到。大体上可以认为是文档的一些属性。  
![frontmatter](./images/4post/230422/frontmatter.png)
虽然大部分的主题都会帮你配置好 FrontMatter 部分，但是也是有部分主题是需要自行配置的，很不幸，Stack 就是这类主题之一。不过这也没啥关系，修改之前提到的`./archetypes/default.md/`下的`default.md`即可。

### 生成静态页面  
只需输入：  
```
hugo
```
即可。默认生成位置为`./public`，你可以在配置文档中使用`publishDir`参数指定，也可以直接使用`-d`参数指定。  

### 发布网站
首先，我不会 git。   
其次，本部分全文抄袭自[炸鸡人博客](https://zhajiman.github.io/post/rebuild_blog)。  
> 这里用 Github Pages 来部署博客。首先在`config.yaml`里指定:
> 1. publishDir: docs
> 
> 然后再一个`hugo`命令，这样就把静态页面输出到`docs`目录下了。
> 
> 接着在 Github 上以 ZhaJiMan.github.io 的名字（根据自己的用户名而定）新建一>个空仓库，进行下面的 Git 命令:
> 1. git add .
> 2. git commit -m "first commit"
> 3. git branch -M main
> 4. git remote add origin https://github.com/ZhaJiMan/ZhaJiMan.github.io.git
> 5. git push -u origin main  
> 
> 这段改编自空仓库页面出现的提示，大意是:
> - 将网站目录下的所有内容暂存。
> - 把暂存的内容提交给版本库。
> - 把主分支的名字从 master 改为 main。
> - 添加远程仓库。
> - 把本地内容推送到远程仓库里。  
> 
> 推送成功后，进入仓库的设置页面，点击侧栏的 Pages，再把 Source 选项改为 main 分支下的`docs`目录，这样 Github Pages 就会根据我们推送上去的`docs`目录里的静态页面来显示网站。这里指定`docs`的好处是还可以把网站的所有文件都备份到仓库里（不包含以 submodule 形式添加主题，详见参考链接）。最后在与仓库同名的网站 https://zhajiman.github.io/ 上看看自己的博客吧！  

我补充几个细节：
1. `git push -u origin master //我采用了master分支`后是要验证 github 账号的（也许只有首次有？），假如你没有，那么我建议你读第2点。
2. Github 是有带 GUI 的客户端的，git 苦手可以考虑用这个。  
 
## 主题配置历程  
由于主题的配置是个个体差异极大的过程，因此我不会事无巨细地说明每一个过程，而是说明几个小点。

### 网站图标更改  
其实很简单，将图片生成的 favicon 文件夹放在`./static`文件夹下，然后在`cofig.yaml`下指定：`favicon: /favicon/favicon.ico `就行。

### 评论系统之接入 
~~忘了Uttrances怎么配置的了啊嗯，鸽了。~~

已将评论系统改为 giscus，配置过程见[这里]()

## Stack主题自定义  
首先放一下对比图：  
![](./images/4post/230422/lightmode_default.png)    
![](./images/4post/230422/lightmode_modified.png)  

### 改善浅色模式可读性
配置完 Stack 后，我并不是很满意，假如说满分100的话我只能打个70分左右。最大的不满在于其浅色模式下可读性实在是过于糟糕,你可以在 [ Demo 网站](https://demo.stack.jimmycai.com/) 上感受一下，我不清楚为什么作者采用了`#bababa`这个颜色，导致其与背景的对比度来到了可怜的1.94，简直就是一场彻头彻尾的灾难。  
不过好在 Stack 主题预留了自定义的空间，详见[官网的说明](https://stack.jimmycai.com/guide/modify-theme)。这里具体介绍下自定义的方法：  
在Stack以及很多主题中，主题文件夹下的`assets/scss`下都提供了一个供用户自定义的`custom.scss`文件。  
> 原理便是在最后引入这个文件，使其位于最终css文件的末尾，从而覆盖原先的属性，达到“自定义”的效果。  

因此，想要自定义就很简单了：找到你希望修改的元素和它对应的选择器，重新定义这个选择器即可。  
万幸，目前的浏览器为我们提供了好用的开发者工具。以我现在使用的Edge为例，按下`F12`弹出开发者工具，使用左上角的小箭头使你的鼠标变成一个查看器(你也可以使用`SHIFT`+`CTRL`+`C`的组合键)，只要将光标置于你想修改的元素上，就能找到该元素的源代码。  
例如我现在想改浅色模式下的文本颜色，只需选择一段文本,就能在右下角找到它对应的css:  
![](./images/4post/230422/css_selected.png)  
![](./images/4post/230422/css_txt.png)  
可以看到文本被指定为`---body-text-color`。  
再顺藤摸瓜找到对应的颜色控制：  
![](./images/4post/230422/css_color.png)  
把要修改的部分直接复制出来放进`custom.scss`文件中，修改下即可。比如我使用的这套：
```scss
:root {
  --body-background: #EBEBEB;
  --accent-color: #1B365D;
  --accent-color-darker: #202A44;
  --accent-color-text: #FFF;
  --body-text-color: #202A44;
}

:root {
  --card-background: #FFF;
  --card-background-selected: #EBEBEB;
  --card-text-color-main: #202A44;
  --card-text-color-secondary: #53565A;
  --card-text-color-tertiary: #888B8D;
}
```  
此时就能达到这种效果了：  
![](./images/4post/230422/lightmode_modified.png)  

Tag的修改我也说下，用上面的方法找到代码，发现：  
![](./images/4post/230422/tagcolorcode.png)  
> 何意味?  

然后把能找到这些颜色的地方全图图了：  
```css
.article-list article:nth-child(n) .article-category a {
  background: #1B365D;
  color: #fff;
}

.article-category a, .article-tags a {
  background-color: #1B365D;
  color: #fff;
}

```

**Note:**  
改完之后我发现分类的 Tag 还是存在问题，虽说前几个颜色是对的，但是越往后走居然开始变色了：  
![](./images/4post/230422/case.png)  
此时即使在custom.scss中指定了颜色也没啥用，看了下这玩意居然在是 element.style 写死的，多少有点搞：  
![](./images/4post/230422/element_style.png)  
于是便需要在代码后面加上`!important`
现代码如下：
```css
.article-list article:nth-child(n) .article-category a {
  background: #1B365D!important;
  color: #fff;
}

.article-category a, .article-tags a {
  background-color: #1B365D!important;
  color: #fff;
}

```

### 修改网站布局  
原先挤满所有空间的布局我不喜欢，遂改。
这里使用[仙贝的代码](https://xrg.fj.cn/p/hugo-stack%E4%B8%BB%E9%A2%98%E6%9B%B4%E6%96%B0%E5%B0%8F%E8%AE%B0/#%E4%B8%BB%E9%A1%B5%E5%B8%83%E5%B1%80)，一样是在`custom.scss`中添加：
```css
.container {
    margin-left: auto;
    margin-right: auto;

    &.extended {
        /* range: 768-1024 */
        @include respond(md) {
            max-width: 1024px;
            --left-sidebar-max-width: 25%;
            --right-sidebar-max-width: 30%;
        }

        /* range: 1024-1280 */
        @include respond(lg) {
            max-width: 1280px;
            --left-sidebar-max-width: 25%;
            --right-sidebar-max-width: 22%;
        }
    }

    &.compact {
        @include respond(md) {
            --left-sidebar-max-width: 25%;
            max-width: 768px;
        }

        @include respond(lg) {
            max-width: 1024px;
            --left-sidebar-max-width: 20%;
        }

        @include respond(xl) {
            max-width: 1280px;
        }
    }
}
```  
即可。

### 滚动条修改  
同上，不喜欢，[仙贝代码](https://xrg.fj.cn/p/hugo-stack%E4%B8%BB%E9%A2%98%E6%9B%B4%E6%96%B0%E5%B0%8F%E8%AE%B0/#%E6%BB%9A%E5%8A%A8%E6%9D%A1%E7%BE%8E%E5%8C%96)，添加：  
```scss
html{
    ::-webkit-scrollbar {
        width: 20px;
      }
      
      ::-webkit-scrollbar-track {
        background-color: transparent;
      }
      
      ::-webkit-scrollbar-thumb {
        background-color: #d6dee1;
        border-radius: 20px;
        border: 6px solid transparent;
        background-clip: content-box;
      }
      
      ::-webkit-scrollbar-thumb:hover {
        background-color: #a8bbbf;
      }
}
```

### 图标添加与修改  
Stack 主题带了几个很好看的 Tabler 图标，可惜并不全，部分缺失的图标要手动添加。  
例如我想添加一个比比汗丽丽的图标，在[ Tabler 官网](https://tabler-icons.io/)搜索发现居然真有：  
![](./images/4post/230422/searchresult.png)  
将svg文件其下载到`.\themes\hugo-theme-stack\assets\icons`中，再调用即可。  
![](./images/4post/230422/implement.png)
  
另外，我也不是很喜欢自带的深色模式切换开关，以我个人的的观点来看，这玩意太不直观了：  
![](./images/4post/230422/toggle.png)  
图标的添加方式就不再赘述，我这里选的是"sun-high"和"moon-stars"这两个图标。
> 观察网页的CSS可以发现：深色模式下图标的切换就是“一个显示，一个隐藏”，在相关文件中指定该用的图标即可。

作者将这个开关做在了侧边栏里，因而直接在有关的`.\assets\scss\partials\sidebar.scss`和`\layouts\partials\sidebar\left.html`里指定图标即可:
``
```css
/* .\assets\scss\partials\sidebar.scss Line 154*/
[data-scheme="dark"] {
    #dark-mode-toggle {
        color: var(--accent-color);
        font-weight: 700;

        .icon-tabler-sun-high {
            display: none;
        }

        .icon-tabler-moon-stars {
            display: unset;
        }
    }
}

#dark-mode-toggle {
    margin-top: auto;
    color: var(--body-text-color);
    display: flex;
    align-items: center;
    cursor: pointer;
    gap: var(--menu-icon-separation);

    .icon-tabler-moon-stars {
        display: none;
    }
}
```  

```html  
<!---.\layouts\partials\sidebar\left.html Line 91--->
{{ if (default false .Site.Params.colorScheme.toggle) }}
  <li id="dark-mode-toggle">
    {{ partial "helper/icon" "sun-high" }}
    {{ partial "helper/icon" "moon-stars" }}
    <span>{{ T "darkMode" }}</span>
  </li>
{{ end }}
```
效果如下：  
![](./images/4post/230422/sun.png)  
顺带一提，想改中文翻译直接去`\i18n\zh-cn.yaml`中修改即可（暗色模式这翻译也太怪了）。

### 添加字数统计  
Stack 本身是不带字数统计的，但是Hugo本身是支持进行字数统计的。
已知，Stack 的文章页面是由三个 html 控制的：  
```
.  
├── details.html        
├── content.html     
└── footer.html   
```
所以在 `details.html` 中加入相应的字数统计代码就行了。
在互联网上一番搜索后，我发现之前就有人写过[相关代码](https://mantyke.icu/posts/2021/f9f0ec87/)。  
```html
{{ if .Site.Params.article.readingTime }} 
  <div>
    {{ partial "helper/icon" "brush" }} 
      <time class="article-words">
        {{ .WordCount }}字
      </time>
  </div>
```
然而这段代码没有多语言支持，所以我决定让事情变得更复杂：全面照抄 Stack 实现阅读时长的方式。  
首先在`congfig.yaml`中的`.params.article`中添加：
```yaml
wordCount: true
```
然后参考前面的和readTime的代码，在`detail.html`中加上下面的代码：
```html
{{ if .Site.Params.article.readingTime }}
  <div>
    {{ partial "helper/icon" "file-description" }}
      <time class="article-words">
        {{ T "article.wordCount" .WordCount }}
      </time>
   </div>
 {{ end }}
```
之后只要添加多语言支持，也就是在`i18n`文件夹中修改对应语言的.yaml文件。以英文为例，在`.\i18n\en.yaml`中加上：
```yaml
wordCount:
  other: "{{.Count}} words"
```
便完事大成了。反正一切主打的就是一个 readTime 在哪我在哪 
> 顺带改了下图标，参考[这里](###图标添加与修改 )

### 文章修改时间显示  
修改时间就没有字数统计这么麻烦了。Stack 本身自带修改时间显示，不过这玩意放在了最底下，不太直观，我就把它提上来了。  
源代码看位置就在`.\layouts\partials\article\components\footer.html`里，果不其然：
```html
{{- if ne .Lastmod .Date -}}
  <section class="article-lastmod">
   {{ partial "helper/icon" "clock" }}
    <span>
      {{ T "article.lastUpdatedOn" }} {{ .Lastmod.Format ( or .Site.Params.dateFormat.lastUpdated "Jan 02, 2006 15:04 MST" ) }}
      </span>
  </section>
 {{- end -}}
```
前面也说了，日期、阅读时间之类被放在了`.\layouts\partials\article\components\footer.html`里，因此在参考日期的代码和源代码基础上做点修改，添加进去就行：
```html
{{- if ne .Lastmod .Date -}}
  <div>
    {{ partial "helper/icon" "edit" }}
      <time class="article-lastmod">
        {{- .Lastmod.Format (or .Site.Params.dateFormat.published "Jan 02, 2006") -}}
      </time>
  </div>
{{ end }}
```
顺带，为了自动更改修改时间，在`config.yaml`中添加：
```yaml
frontmatter:
  lastmod: [":fileModTime", "lastmod"]
```

### 友情链接改为双列显示  
这里我用的是[大佬的代码](https://tech.randomwaves.space/posts/21-12-08-make-hugo-stack-theme-links-display-in-two-columns/)，修改过程和之前一样，在`custom.scss`添上以下css即可：
```scss
@media (min-width: 1024px) {
  .article-list--compact.links {
      display: grid;
      grid-template-columns: 1fr 1fr;
      background: none;
      box-shadow: none;
      
      article {
          background: var(--card-background);
          border: none;
          box-shadow: var(--shadow-l2);
          margin-bottom: 8px;
          border-radius: 10px;
          &:nth-child(odd) {
              margin-right: 8px;
          }
      }
  }
}
```


## 目前仍未完成的部分  
当然，目前这个网站还是有一些问题的：  
1. ~~过往文章未恢复~~  
2. ~~评论系统未接入~~  
3. ~~部分界面还未配置好~~ 
4. ~~网站图标未设置（其实是还没做）~~  
5. ~~没有文章修改时间显示~~
6. ~~洋文站点还没完成(已躺平)~~
 


## 参考链接
[用 Hugo 重新搭建博客](https://zhajiman.github.io/post/rebuild_blog/)  
[Hugo Stack主题更新小记](https://xrg.fj.cn/p/hugo-stack%E4%B8%BB%E9%A2%98%E6%9B%B4%E6%96%B0%E5%B0%8F%E8%AE%B0)  
[将Hugo Stack主题友情链接改为双列显示](https://tech.randomwaves.space/posts/21-12-08-make-hugo-stack-theme-links-display-in-two-columns/)  
[Hugo | 看中 Stack 主题的归档功能，搬家并做修改](https://mantyke.icu/posts/2021/f9f0ec87/)  
[Page Variables](https://gohugo.io/variables/page/)