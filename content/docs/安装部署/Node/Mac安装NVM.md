# 1.卸载Node.js

## 1.1 通过Homebrew安装

```sh
brew uninstall node 
```

## 1.2 通过官方安装包安装

1. 删除 `/usr/local/lib` 下的任意 `node` 和 `node_modules` 的文件或目录
2. 删除 `/usr/local/include` 下的任意 `node` 和 `node_modules` 的文件或目录
3. 删除 `Home` 目录下的任意 `node` 和 `node_modules` 的文件或目录
4. 删除 `/usr/local/bin` 下的任意 `node` 的可执行文件

以下三种任选其一。测试 `nvm`、`node`、`npm` 三个命令是否还在

```bash
sudo rm -rf /opt/local/bin/node /opt/local/include/node /opt/local/lib/node_modules
sudo rm -rf /usr/local/bin/npm /usr/local/share/man/man1/node.1 /usr/local/lib/dtrace/node.d
```



```bash
sudo rm -rf /usr/local/{bin/{node,npm},lib/node_modules/npm,lib/node,share/man/*/node.*}
```



```bash
sudo rm -rf ~/.npm
sudo rm -rf ~/node_modules
sudo rm -rf ~/.node-gyp
sudo rm /usr/local/bin/node
sudo rm /usr/local/bin/npm
sudo rm /usr/local/lib/dtrace/node.d
```

# 2. 安装版本管理Nvm

```sh
brew install nvm
```

安装完成后，在 `~/.bash_profile` 或 `~/.zshrc` 中添加以下内容

```sh
# Node.js
source $(brew --prefix nvm)/nvm.sh
```

安装完成后，查看是否安装成功

```sh
$ nvm --version
0.39.1
```

```sh
$ nvm list
```



# 3. 通过 nvm 安装管理Node.js

安装最新版本node.js

```bash
nvm install node
```

安装指定版本node.js

```sh
nvm install 10.22.0
```

列出所有版本

```sh
$ nvm list
->     v10.22.0
        v17.4.0
default -> node (-> v17.4.0)
iojs -> N/A (default)
unstable -> N/A (default)
node -> stable (-> v17.4.0) (default)
stable -> 17.4 (-> v17.4.0) (default)
lts/* -> lts/gallium (-> N/A)
lts/argon -> v4.9.1 (-> N/A)
lts/boron -> v6.17.1 (-> N/A)
lts/carbon -> v8.17.0 (-> N/A)
lts/dubnium -> v10.24.1 (-> N/A)
lts/erbium -> v12.22.9 (-> N/A)
lts/fermium -> v14.18.3 (-> N/A)
lts/gallium -> v16.13.2 (-> N/A)
```

显示当前版本

```sh
$ nvm current
v10.22.0
```

切换使用指定的版本node

```sh
nvm use 17.4.0
```

# 4. 安装npm包

```sh
nvm use 10.22.0
```

```sh
npm install -g gitbook-cli
```

