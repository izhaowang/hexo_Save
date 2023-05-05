---
title: issue
tags:
  - issue
categories:
  - issue
date: 2023-03-03 02:25:48
---
# 当我们项目中添加了个多个git仓库。当我们在当前仓库push的时候，它不会提交你clone的内部仓库。
就是外部仓库无法 track 一个嵌套仓库，外部仓库所有的 add，commit，push 都与嵌套仓库没有任何关系。
## 首先你需要移除这个仓库
git rm -f --cahche themes/next  // clone 的地址是 themes/next
## 1. 强制将next仓库嵌套进你当前的仓库
删除掉themes/next 中 .git 文件，这样next就是一个普通的文件，不和任何仓库有关系。 在外部文件可以直接push
## 2. 使用 git submodule add 地址  子模块目录
但是子模块的代码不由我们控制，也就是源仓库修改，我们这边受到牵连
命令行中输入 
```
$ git submodule add https://github.com/theme-next/hexo-theme-next themes/next
```
执行完成后，会在站点根目录下生成 .gitmodules 文件，内容如下：
{% raw %}
[submodule "themes/next"]
    path = themes/next
    url = https://github.com/theme-next/hexo-theme-next
{% endraw %}
配置文件保存了项目 URL 与已经拉取的本地目录之间的映射，如果有多个子模块，该文件中就会有多条记录。要重点注意的是，该文件应像 .gitignore 文件一样受到（通过）版本控制，和该项目的其他部分一同被拉取推送。有了映射关系，克隆该项目的人就知道去哪获得子模块了

## fork别人的仓库到自己的仓库中去
重复第二部的命令行
```
$ git submodule add 自己仓库的地址 themes/next
```
{% raw %}
[submodule "themes/next"]
    path = themes/next
    url = https://github.com/izhaowang/hexo-theme-next
    ignore = all 
{% endraw %}

+ all：子模块永远不会被视为已修改（但仍将显示在状态输出中并在提交时提交）。
+ dirty：将忽略对子模块工作树的所有更改，仅考虑子模块的HEAD与其在超级项目中的记录状态之间的已提交差异。
+ untracked：只有子模块中未跟踪的文件才会被忽略。将显示对跟踪文件的提交的差异和修改。
+ none：默认选项，不会忽略对子模块的修改，显示所有已提交的差异以及对已跟踪和未跟踪文件的修

## 去修改子模块的git 地址

```
1. 在.gitmoudles中修改新的url地址
2. 在themes/next目录下 命令输入 git submodule sync --recursive 即可
```

# 关于设置markdown的代码片段后不生效问题
// 在vscode的setting.json中进行如下配置
```javascript
  "[markdown]": {
      "editor.quickSuggestions": {
          "other": "on",
          "comments": "on",
          "strings": "on"
      },
  },
```