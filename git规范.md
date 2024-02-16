# 仓库分支

1. 主分支(master):：这是项目的默认分支，通常包含了项目的稳定版本代码。在大多数情况下，主分支是受到保护的，不允许直接进行代码提交，而是通过合并其他分支的代码来更新。
2. 开发分支(develop):这是用于日常开发的分支，通常包含了项目的最新开发代码。开发者可以在这个分支上进行代码的修改、提交和测试，然后将代码合并到主分支中。
3. 功能分支(feature):当你的项目已经成形，但是要开发新功能，你不能影响之前的代码，就需要从开发分支中创建一个功能分支，，然后在其中进行代码的编写和测试。完成后，可以将功能分支合并回开发分支或主分支中。
4. 测试分支(test):测试人员用于测试的分支，对 develop 和 feature 分支进行测试
5. 预览分支(release):功能完成，测试也完成，一般由 test 合并
6. 修复分支(hotfix):当生产环境出现问题，需要紧急修复。由 master 创建，经过修复和测试最后合并到 master 和 develop

# 分支创建合并流程

1. 对于工作量很小的开发，可以直接在 develop 中进行，否则应该在 feature 中进行，然后合并到 develop。
2. 开发结束后需要合并到 test 分支
3. 测试通过后，需要将 test 合并到 release 分支
4. UAT 测试通过后，由 release 分支合并到 master 分支

### 创建分支：

1. 根据 master 创建新的分支 develop。使用`get branch`

```bash
git branch develop
```

2. 切换到开发分支开始工作

```bash
git checkout develop
```

使用一条命令创建并切换分支

```bash
git checkout -b develop
```

3. 需要开发新的功能，创建一个新的功能分支

```bash
git branch feature
```

4. 切换到功能分支开始工作

```bash
git checkout feature
```

### 合并分支

前面你已经将功能开发完成，现在你需要将功能分支合并到开发分支。(在此之前请确保你已经添加并提交了你的更改)

```bash
# 由功能分支切换到开发分支
git checkout develop
# 将功能分支合并到开发分支
git merge feature
```

然后拉取更新你的开发分支，你要知到项目是多人协同开发，你需要时刻保持你的开发分支是最新的
当开发结束要将开发分支合并到主分支

```bash
# 切换到开发分支
git checkout master
# 合并开发分支
git merge develop
```

### 删除功能分支

```bash
git branch -d feature
```

# Angular Git Commit 规范

## type 类型

- feat 新增功能
- fix 修复 bug
- docs 修改文档
- style 代码格式、注释
- refactor 普通更改既不修 bug 也不增加新功能
- perf 改进性能的更改
- test 修改或增加测试
- chore 对构建过程辅助工具和库的更改
- delete 删除功能或文件
- modify 修改功能
- ci 自动化流程配置
- revert 回滚到上一个版本

# git 提交流程

- Fork 你想要贡献的项目
- git clone 你 Fork 的项目到本地
- git checkout -b 创建并切换到贡献分支

1. git add 将更改的内容提交到暂存区
2. git commit 将暂存区的内容提交到本地仓库
3. git pull 拉取远程仓库并于本地仓库合并
4. 解决合并冲突，再 git add 和 git commit
5. git push 解决冲突后将最新的更改推送到远程

- 创建 Pull Request，提供详细的描述，解释你做了什么
- 项目维护者审查你的 Pull Request
- 审查通过接收合并
