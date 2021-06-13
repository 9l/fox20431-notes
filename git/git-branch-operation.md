# Branch Operation

Git分支操作。

## 常用命令介绍

### 查看

```sh
git branch
# 冗长的(verbose)
git branch -v
# 查看所有
git branch -a
```

### 创建分支

```sh
git branch <branch_name>
```

### 删除分支

```sh
# 确保git不处于要删除的分支上
# 删除远程分支
git push origin -d <branch_name>
```

