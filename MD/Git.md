# Git

### …or create a new repository on the command line

```echo "# VueDemoR" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:MourinhoLove/VueDemoR.git
git push -u origin main```
```

### …or push an existing repository from the command line

```
git remote add origin git@github.com:MourinhoLove/VueDemoR.git
git branch -M main
git push -u origin main
```

git remote add origin git@github.com:MourinhoLove/NetCoreDemo.git



## 常用指令

- `git init`

  建立新的本地端 Repository。

- `git clone [Repository URL]`

  複製遠端的 Repository 檔案到本地端。

- `git status`

  檢查本地端檔案異動狀態。

- `git add [檔案或資料夾]`

  將指定的檔案（或資料夾）加入版本控制。用 `git add .` 可加入全部。

- `git commit`

  提交（commit）目前的異動。

- `git commit -m "提交說明內容"`

  提交（commit）目前的異動並透過 `-m` 參數設定摘要說明文字。

- `git remote add origin git@github.com:tianqixin/runoob-git-test.git`

  关联github的远程仓库

- `git push -u origin master`或者 main

  提交到远程仓库