# 开源之路第一步--Github项目同步

> 最近在一群老师的帮助下，加入到了Github上某开源项目的贡献队伍中.  
> 拉代码，远程关联同步的时候，遇到了一些问题，
> 目测适用于多数的Maven项目.  

## 项目和工具说明  

```txt
项目管理工具: Git
项目类型: Maven工程
JDK版本: 1.8+
```

## 具体步骤  

### 1.拉取代码

```txt
给开源项目提交代码不能直接提交上去, 都需要先fork到自己的仓库中, 然后和源工程进行关联    
这样做的目的是为了保证不污染源项目本身(一般我们也没有直接修改源项目的权限, 都是通过提issues和PR).  
```

#### 1.1开启长路径创建功能

因为这个项目的文件名比较长, 导致代码路径较长, 在 Windows 上就会产生一个问题,   
Git在创建路径的时候是通过 Windows API 实现的, 路径最大长度被限制在 260,   
所以需要在git开启设置来让超长路径名能正确的省生成, 命令如下.
`git config --global core.longpaths true`    
如果不开启此设置, 项目在下载完成后会因无法生成正确的路径而被系统自动删除,导致编译时出现文件缺失的问题.  

#### 1.2拉取代码

- 先将源项目 fork 到码云(Gitee)  
- 直接克隆下代码到本地  
- 下载完成后需要, ip 变了, git 远程仓库的地址也变了, 这个时候就要自己手动修改项目连接到的远程仓库地址  
- 修改 remote 地址: 在你本地的项目位置，右键 --> git bash here    
  然后输入命令`git remote set-url origin xxx.git`,   
  这里的 xxx.git 是自己 Github 的项目地址, 这一步主要是将代码远程地址设置成你的 Github 地址.   
- 设置 upstream 地址 & fork 同步:
  查看远程分支和路径 `git remote -v`, 确认自己的 GitHub 地址确实已经添加上去了.  
  添加源项目地址`git remote add upstream yyy.git`    
  主要是为了让本地项目和源项目进行关联, 这样方便直接更新到源项目的最新代码.  
  `git remote -v`验证是否添加成功.    
- 将源项目代码同步到本地
  `git fetch upstream` 默认将源项目`main`分支代码拉取到本地, 如果需要在命令的最后添加指定分支名即可.
  `git rebase upstream/main` 
  `git push` 完成提交同步
  `git log` 查看提交日志
  如果需要贡献代码, 需要执行`git merge upstream`

### 2 静候编译完成

使用如下 maven 命令完成编译

`mvn clean package install -Dmaven.test.skip=true -Dmaven.javadoc.skip=true -Drat.skip=true -Dcheckstyle.skip=true`

