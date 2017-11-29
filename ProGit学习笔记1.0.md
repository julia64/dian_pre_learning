# ProGit 学习笔记
## 第一章 起步
### 关于版本控制
版本控制是一种记录若干文件内容变化，以便将来查阅特定版本修订情况的系统。
控制系统：本地版本->集中化版本->分布式版本（客户端提取原始代码的镜像）
### Git基础要点
1. 不比较内容差异，而是纵览文件的指纹信息并作快照，保存一个指向快照的索引。若文件没有变化，则Git不再保存，只对上次保存的快照作链接。
2. 近乎所有操作都可本地执行，因此速度很快。
3. Git能通过校验指纹子串保持数据完整性。
4. git目录：Git用来保存元数据和对象数据库的地方。克隆镜像仓库拷贝的就是其中的数据。
5. 文件在Git中的状态 
	* 已提交：该文件已被安全地保存在本地数据库中。
	* 已修改：修改了某个文件，但还没有提交保存。
	* 已暂存：把已修改的文件放在下次提交时要保存的清单中。

### 第一次运行前的配置
* /etc/gitconfig文件：系统文件中对所有用户都普适的配置 （--system）
* ~/.gitconfig文件：用户目录下的配置文件只适用于该用户（--global）
#### 用户信息
```
$ git config --global user.name "Julia64"
$ git config --global user.email 945159252@qq.com
```
#### 文本编辑器
```
$ git config -- global core.editor emacs （将默认编辑器换成Emacs）
```
#### 差异分析工具
```
$ git config -- global merge.tool vimdiff（在解决合并冲突时使用vimdiff）
```
#### 查看配置信息
```
$ git config -- list
```
#### 获取帮助
```
$ git help <verb>
```
## 第二章 Git基础
### 取得项目的Git仓库
1. 从当前目录初始化
对现有的某个项目开始用Git管理，只需到此项目所在的目录，执行：
```
$ git init
```
如果想将某些文件纳入版本控制
```
$ git add *.c
$ git add README
$ git commit -m 'initial project version'
```
2. 从现有仓库克隆
```
$ git clone git://github.com/schacon/grit.git (mygrit)
```
在当前目录创建一个名为“grit”（或“mygrit”）的目录，内含一个.git目录，并已从仓库取出最新版本的文件拷贝。
### 更新记录到仓库

##### 检查当前文件状态

```
$ git status
```
##### 跟踪新文件
```
$ git add README
```
##### 暂存已修改文件
```
$ get add <verb>
```
git add命令：开始跟踪新文件、把已跟踪的文件放到暂存区、合并时把有冲突的文件标记为已解决状态。
xgwang@hust.edu.cn