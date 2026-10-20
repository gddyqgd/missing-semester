# 第一课总结

## 1. <mark>工作流</mark>

```mermaid
graph LR

A[团队仓库] --->|fork| B[个人远程仓库]
B[个人远程仓库] --->|clone| C[本地仓库]
C[本地仓库] --->|branch| D[编辑代码]
D[编辑代码] --->|add| E[暂存区]
E[暂存区] --->|commit| C[本地仓库]
C[本地仓库] --->|push| B[个人远程仓库]
B[个人远程仓库] --->|PR| A[团队仓库]
```
## 2. Linux

- Linux是操作系统内核，Ubuntu(Ros兼容性好)是发行版。

```mermaid
graph LR

A[终端] --->|输入命令| B[shell]
B[shell] --->|解释命令| C[Linux]
```

## 3. Vim

- Vim是文本编辑器，用来编辑内容。

## 4. shell的命令结构
|命令  |选项  |参数     |
|:---|:---|:---|
|ls|-la|~/project|
|rm|-r|project|
|...|...|...|

## 5.
