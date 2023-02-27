# FUCK-GFW

> I do not want waste time in GFW again.

为了避免愚蠢的防火长城浪费我们的时间，原作者和各位贡献者为你呈上 FUCK-GFW。

## ⚡报告问题

如果你发现某个部分对你不生效，或者有错误，可以在项目的 [Discussion](https://github.com/Taskeren/ForK-GFW/discussions) 里报告。

## pip

~/.config/pip/pip.conf

```
[global]
proxy=http://localhost:1087
```

注意不支持 socks5

### 参考

https://my.oschina.net/tangshi/blog/699190

## Git

### 使用 SSH Clone

在文件 `~/.ssh/config` 后添加

```bash
Host github.com
# Mac下
ProxyCommand nc -X 5 -x 127.0.0.1:1080 %h %p
# Linux下
ProxyCommand nc --proxy-type socks5 --proxy 127.0.0.1:1080 %h %p
```

注意 Linux 和 Mac 下 ncat/netcat 区别，详见 <https://unix.stackexchange.com/questions/368155/what-are-the-differences-between-ncat-nc-and-netcat>

### 使用 HTTP Clone

```
git config --global http.proxy http://127.0.0.1:1087
```

建议使用 http, 因为 socks5 在使用 git-lfs 时会报错 `proxyconnect tcp: dial tcp: lookup socks5: no such host`

### 参考

https://gist.github.com/laispace/666dd7b27e9116faece6

## Cargo

Cargo 会依次检查以下位置

1. 环境变量 `CARGO_HTTP_PROXY`

```bash
export CARGO_HTTP_PROXY=http://127.0.0.1:1080
```

2. 任意 `config.toml`[\*](https://doc.rust-lang.org/cargo/参考/config.html#hierarchical-structure) 中的 `http.proxy`

```toml
[http]
proxy = "127.0.0.1:1080"
```

3. 环境变量 `HTTPS_PROXY` & `https_proxy` & `http_proxy`

```bash
export https_proxy=http://127.0.0.1:1080
export http_proxy=http://127.0.0.1:1080
```

`http_proxy` 一般来讲没必要，除非使用基于 HTTP 的 Crate Repository

Cargo 使用 libcurl，故可接受任何符合 [libcurl format](https://everything.curl.dev/usingcurl/proxies) 的地址与协议

`127.0.0.1:1080`，`http://127.0.0.1:1080`，`socks5://127.0.0.1:1080` 均可

### 参考

https://doc.rust-lang.org/cargo/参考/config.html#httpproxy

## apt (apt-get)

在 `/etc/apt/apt.conf.d/` 目录下新增 `proxy.conf` 文件，添加

```
Acquire::http::Proxy "http://127.0.0.1:8080/";
Acquire::https::Proxy "http://127.0.0.1:8080/";
```

注：无法使用 socks5 代理。

### 参考

https://askubuntu.com/a/349765/883355

## curl

在 `~/.curlrc` 中添加

```bash
socks5 = "127.0.0.1:1080"
```

### 参考

https://www.zhihu.com/question/31360766

## Gradle

在 `~/.gradle/gradle.properties` 中添加

```properties
systemProp.http.proxyHost=127.0.0.1
systemProp.http.proxyPort=1087
systemProp.https.proxyHost=127.0.0.1
systemProp.https.proxyPort=1087
```

### 参考

https://stackoverflow.com/questions/5991194/gradle-proxy-configuration

## go get

```
HTTP_PROXY=socks5://localhost:1080 go get
```

测试了下 HTTPS_PROXY 和 ALL_PROXY 都不起作用

或者使用[goproxy.io](https://goproxy.io/)

## npm

```
npm config set proxy http://127.0.0.1:1087
npm config set https-proxy http://127.0.0.1:1087
```

用 socks5 就报错

推荐使用 yarn，npm 是真的慢

### 参考

-   https://stackoverflow.com/questions/7559648/is-there-a-way-to-make-npm-install-the-command-to-work-behind-proxy
-   有些包要在 postinstall 阶段下载内容的还需要配置环境变量：https://antfu.me/posts/npm-binary-mirrors

## rustup

```bash
export https_proxy=http://127.0.0.1:1080
```

## yarn

```
yarn config set proxy http://127.0.0.1:1087
yarn config set https-proxy http://127.0.0.1:1087
```

不支持 socks5

### 参考

https://github.com/yarnpkg/yarn/issues/3418

## yarn2

### 配置代理

```sh
yarn config set httpProxy http://127.0.0.1:1087
yarn config set httpsProxy http://127.0.0.1:1087
```

不支持全局设置，支持 socks5

这个命令会修改项目目录下的 `.yarnrc.yml` 文件, 请留意不要把带有环境变量的代码提交到仓库, 以免造成麻烦，如下

```yml
httpsProxy: "socks5://127.0.0.1:1080"
```

### 使用 NPM 镜像

```sh
yarn config set npmRegistryServer https://127.0.0.1:1087
```

注意: 此方法不适用于下载 yarn 官方插件!  
yarn 的官方插件默认会从 GitHub(raw.githubusercontent.com)上下载  
您可能依旧需要配置代理

### 参考

-   [yarn doc - httpProxy](https://yarnpkg.com/configuration/yarnrc#httpProxy)
-   [yarn doc - httpsProxy](https://yarnpkg.com/configuration/yarnrc#httpsProxy)

## gem

`~/.gemrc`

```
---
# See 'gem help env' for additional options.
http_proxy: http://localhost:1087
```

### 参考

Google

## Homebrew

```
ALL_PROXY=socks5://localhost:1080
```

## wget

`~/.wgetrc`

```
use_proxy=yes
http_proxy=127.0.0.1:1087
https_proxy=127.0.0.1:1087
```

### 参考

https://stackoverflow.com/questions/11211705/how-to-set-proxy-for-wget

## snap

```bash
sudo snap set system proxy.http="http://127.0.0.1:1087"
sudo snap set system proxy.https="http://127.0.0.1:1087"
```

### 参考

https://askubuntu.com/questions/764610/how-to-install-snap-packages-behind-web-proxy-on-ubuntu-16-04#answer-1146047

## docker

```
$ sudo mkdir -p /etc/systemd/system/docker.service.d
$ sudo vim /etc/systemd/system/docker.service.d/proxy.conf

[Service]
Environment="ALL_PROXY=socks5://localhost:1080"

$ sudo systemctl daemon-reload
$ sudo systemctl restart docker
```

必须是 socks5，http 不生效

## Electron Dev Dependency

设置环境变量

```
ELECTRON_GET_USE_PROXY=true
GLOBAL_AGENT_HTTPS_PROXY=http://localhost:1080
```

### 参考

-   https://www.electronjs.org/docs/latest/tutorial/installation#proxies
-   https://github.com/gajus/global-agent/blob/v2.1.5/README.md#environment-variables

## Visual Studio Code Remote (WSL2)

WSL2 环境下可以通过设置 `~/.vscode-server/server-env-setup` 脚本文件，设置开发环境的环境变量，使用代理。

WSL2 内环境访问 Win 下的代理程序端口代理(例子代码中 http 代理端口监听 17070)，因为子网地址每次启动都不一样，需要动态处理。

新建 `~/.vscode-server/server-env-setup` 文件，该文件会在 VSCode 启动 WSL 环境后被`source`。

```
WSL_HOST=$(sed -n '/^nameserver/p' /etc/resolv.conf | cut -d' ' -f2)
export http_proxy=http://${WSL_HOST}:17070
export https_proxy=$http_proxy
export all_proxy=$http_proxy
```

### 参考

https://code.visualstudio.com/docs/remote/wsl

## Visual Studio Code Remote (SSH)

VSCode SSH 后的环境不会使用本地界面 VSCode 内的代理设置，如果 SSH 主机没有默认网络链接或在墙内，会导致问题。

### SSH 主机无网络

需要手动下载 vscode 的 server 端传输部署。详情见链接

-   https://stackoverflow.com/questions/56671520/how-can-i-install-vscode-server-in-linux-offline
-   https://gist.github.com/b01/0a16b6645ab7921b0910603dfb85e4fb

### SSH 主机在墙内

虽然文档未提及，但是可以使用 WSL 模式的方案，配置 `~/.vscode-server/server-env-setup` 文件设置代理。

SSH 主机有代理程序监听在 17070 端口：

新建 `~/.vscode-server/server-env-setup` 文件，该文件会在 VSCode 启动 WSL 环境后被 source。

```
export http_proxy=http://127.0.0.1:17070
export https_proxy=$http_proxy
export all_proxy=$http_proxy
```

### 参考

https://code.visualstudio.com/docs/remote/ssh

<!--
## Tips
推荐使用Clash,[windows版](https://github.com/Fndroid/clash_for_windows_pkg)，[mac版](https://github.com/yichengchen/clashX)（注意：[ClashX Pro](https://install.appcenter.ms/users/clashx/apps/clashx-pro/distribution_groups/public) 包含TUN功能，ClashX不包含），开启Tun mode可以解决大部分的GFW问题

### 参考
https://docs.cfw.lbyczf.com/contents/tun.html
-->

## Scoop

```bash
scoop config proxy 127.0.0.1:1080
```

### 参考

https://github.com/ScoopInstaller/Scoop/wiki/Using-Scoop-behind-a-proxy#configuring-scoop-to-use-your-proxy

## OpenWRT opkg

在 LUCI 面版菜单配置或者/etc/opkg.conf 末尾追加

```
option http_proxy http://localhost:1080/
```

### 参考

https://openwrt.org/docs/guide-user/additional-software/opkg

## Maven

`~/.m2/settings.xml`

```xml
<settings>
  ...
  <proxies>
   <proxy>
      <id>example-proxy</id>
      <active>true</active>
      <!-- 代理协议 -->
      <protocol>http</protocol>
      <!-- 代理地址 -->
      <host>proxy.example.com</host>
      <!-- 代理端口 -->
      <port>8080</port>
      <!-- 如果代理不需要登陆，必须删除 username 和 password -->
      <username>proxyuser</username>
      <password>somepassword</password>
      <!-- 不使用代理的站点，可以删除 -->
      <nonProxyHosts>www.google.com|*.example.com</nonProxyHosts>
    </proxy>
  </proxies>
  ...
</settings>
```

### 参考

-   https://maven.apache.org/guides/mini/guide-proxies.html
-   https://stackoverflow.com/questions/1251192/how-do-i-use-maven-through-a-proxy

## SSH with `connect`

`~/.ssh/config`

```
Host github.com
  Hostname ssh.github.com
  Port 443
  User git
  # 代理设置
  ProxyCommand connect -H localhost:7890 %h %p
```

这个方式你需要在执行 SSH 的环境里有 connect 这个程序。你可以从 Git for Windows 里去获取一份（`C:\Program Files\Git\mingw64\bin`），复制到或者添加到 Path 环境变量里。
