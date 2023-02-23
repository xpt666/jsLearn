
1. git status查看当前仓库状态，红色代表未add，绿色代表未commit
```
On branch note
Your branch is up to date with 'origin/note'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   chapter01.md        这个代表修改了文件chapter01.md还未add

no changes added to commit (use "git add" and/or "git commit -a")
```

2. git add将有修改的文件提交，需要加文件名，.代表所有已修改文件
3. 