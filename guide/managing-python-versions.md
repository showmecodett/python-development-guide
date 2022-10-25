# 介绍
在做python开发的时候，会遇到要切换不同的python版本开发的情况。比如老的项目是python2的，新的项目是python3的。那么可以使用`pyenv`来维护不同版本的python

# 安装
MacOS的用户可以直接`brew update && brew install pyenv`
安装参考：[安装pyenv](https://github.com/pyenv/pyenv#installation)

# 设置环境变量
我使用的是zsh，环境变量如下
```bash
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
echo 'eval "$(pyenv init -)"' >> ~/.zshrc
```
[环境变量设置参考](https://github.com/pyenv/pyenv#set-up-your-shell-environment-for-pyenv)

# 使用

## 查看当前可用的版本
`pyenv versions`

## 查看当前可以安装的python版本（如果你看到的python版本不全，可能是你的pyenv版本较旧）
`pyenv install -l`

## 安装python 3.10.4版本(3.10.4是个变量，可以替换为你想装的版本)
`pyenv install 3.10.4`
![安装python 3.10.4](images/managing-python-versions-3104.png)

## 再次查看版本列表，可以看到python 3.10.4版本了
`pyenv versions`
![python列表](images/managing-python-versions-list.png)

## 场景

### 1. 我想测试python某个版本的一个功能
python 3.10优化了错误提示，比如代码中丢失了字典的花括号，之前的版本提示是`SyntaxError: invalid syntax`, python3.10 会提示到具体的位置，并提示具体的报错原因。

#### 示例代码
[better-error-messages.py](example/managing-python-versions-better-error-messages.py)

#### 执行
```bash
# 查看当前终端的python版本
python --version
# 在当前终端使用python 3.10的版本
pyenv shell 3.10.4
# 在查看当前python的版本
python --version
# 使用python 3.10.4执行
python example/managing-python-versions-better-error-messages.py
# 使用python 3.8.14执行
pyenv shell 3.8.14
python --version
python example/managing-python-versions-better-error-messages.py
```
#### python 3.10执行报错
![3.10-better-error-messages.png](images/managing-python-versions-3.10-better-error-messages.png)
#### python 3.8执行报错
![3.8-better-error-messages.png](images/managing-python-versions-3.8-better-error-messages.png)

#### pyenv shell命令解释

查看当前终端使用的python版本
`pyenv version`
![pyenv-version.png](images/managing-python-versions-pyenv-version.png)
可以看到使用环境变量的方式设置的python 版本，那么我们新打开个终端，在进入到当前目录，看下python版本
![pyenv-shell-new-terminal.png](images/managing-python-versions-pyenv-shell-new-terminal.png)
如上图所示，可以看到python版本已经变成了默认的版本了
综上，说明通过`pyenv shell <version>`修改的python版本只对当前的终端session生效。

### 2. 使用python3.8版本维护某python工程
我们有个`Django3.2`版本的项目，只能使用`python3.8`版本的开发，但我只要在`Django3.2`的项目中使用`python3.8`版本，不影响我全局的python版本
```bash
#查看当前环境的python版本
python --version
pyenv version

# 创建Django3.2版本的项目
mkdir be-django3.2
cd be-django3.2

# 查看项目下的python版本
pyenv version
```
![images/managing-python-versions-create-django3.2.png](images/managing-python-versions-create-django3.2.png)

打开新的终端，验证是否在当前工程中依旧是`python3.8`版本
![images/managing-python-versions-django3.2-new-terminal.png](images/managing-python-versions-django3.2-new-terminal.png)
如上图所示，`Django3.2`项目的父目录使用的还是系统版本python，在`Django3.2`中使用的是`python3.8`版本，说明是永久生效的。

#### pyenv local命令解释
![images/managing-python-versions-pyenv-local.png](images/managing-python-versions-pyenv-local.png)
如上图所示，我们可以看到`Django3.2`项目的当前目录下多了个`.python-version`文件，说明pyenv是通过这种方式保存当前目录的python版本的

综上所述，`pyenv local <version>`可以对某个目录设置相应python版本的环境

### 3. 修改全局python版本
`pyenv global <version>`

# 附录
[pyenv](https://github.com/pyenv/pyenv)