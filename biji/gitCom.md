## GIT 命令

**初始化** 					`git init`

**设置邮箱用户名**	 `git config [--global] user.name or user.email` [全局]

**工作区->暂存区** 	` git add filename`

**暂存区->工作区** 	`git rm --cached filename`

**暂存区->存储区** 	`git commit -m Message`

**日志**		`git log`和`git log --oneline`

**恢复文件** 			`git restore filename`

**重置节点** 	`git resrt --request` 	直接回到目标节点，不保存提交

**恢复节点**  	`git revert --address`		直接回到目标节点之前，创建一个提交

**创建并切换分支**		`git checkout -b user` b-> branch  [][][ -v ]`[ -d ]

**分支合并**     `git merge 分支` 将分支与当前分支合并

