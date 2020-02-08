---
 title:  "Dockerfile常见指令" 
--- 
# Dockerfile常见的指令

### FROM

有效的Dockerfile必须以 **FROM** 指令开始，该指令初始化一个新的编译阶段，为后面的指令指定一个基础的镜像(Base image)。（ **ARG** 指令可以）

```dockerfile
FROM <image>[:<tag>] [AS <name>]

# examples 
FROM alpine as builder
```

### ARG 与 ENV

**ARG** 指令是用来指定Dockerfile中需要使用的参数，Dockerfile中可以用 **${varaiable name}**  或 **\$<varaiable name>** 来引用该指令指定的参数，该参数只能在 **build** 阶段使用，容器是不能访问到这个参数的，与此不同的就是 **ENV** 指令，该指令指定的 **环境变量** 既能在Dockerfile中被引用，也能够在容器中被访问，**ARG** 和 **ENV** 具体的指令语法如下：

```Dockerfile
    ARG <name>[=<default value>]
    
    ENV <name> [default value]]
```

### COPY

文件复制功能，常常用于把文件从主机目录复制到容器目录里面，语法格式如下：
```
COPY [--chown=<user>:<group>] <src>... <dest>
```
从源目录**src**复制到目的目录**dest**，*\-\-chown* 参数改变文件所属的用户和用户组，此外，**COPY**指令还有个特殊的用法。可以在 不同**构建阶段(build stage)** 的目录之间复制文件，这个功能可以通过 *--from* 参数来实现：
```
COPY --from=<name|index> <src> <dest>
```
通过 *--from* 指定之前构建阶段的名称（as 后面的参数）或者构建阶段的id

> **提示**：

### LABEL

标签用来描述镜像的基本信息，一个 **LABEL**指令可以指定多个key=value的标签数据,我们可以通过命令 **docker inspect** 来显示镜像的基本信息。
```Dockerfile
LABEL <key>=<value> <key>=<value> <key>=<value> ...

# example

LABEL maintainer="SvenDowideit@home.org.au"
```