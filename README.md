# gitgo（Git 通）

切换梯子后自动配置 Git 代理，一条命令让 Git 重新通网。

## 解决了什么问题

用多个梯子软件（Clash Verge、v2rayN、Shadowsocks、山海等）来回切换时，每个软件的代理端口不一样。每次换梯子都得手动改 Git 配置，找端口、改设置、测试连通性，反复折腾。

`gitgo` 自动完成这一切：扫描你电脑上正在运行的代理端口 → 配置 Git → 验证 GitHub 是否连通。整个过程一条命令，不用动脑子。

另外，很多梯子的 HTTP 代理不支持转发 SSH（22 端口），导致 `git@github.com` 地址无法 clone。脚本会自动配置 SSH 地址转 HTTPS，从根本上解决这个问题。

## 安装

### 方式一：Claude Code Skill（推荐）

复制下面这句话，粘贴到 Claude Code 对话框即可自动安装：

> 帮我安装 gitgo（Git 通）skill，地址是 https://raw.githubusercontent.com/Homelander-Louis/gitgo/main/claude-skill/gitgo.md

安装后，在 Claude Code 里说「连不上 GitHub 了」或「配置代理」，Claude 会自动帮你检测端口并配置。检测不到代理时，还会主动问你端口号。

### 方式二：独立脚本

安装到系统 PATH，之后在任何终端里直接输入 `gitgo` 即可使用。

**macOS / Linux：**
```bash
sudo curl -o /usr/local/bin/gitgo https://raw.githubusercontent.com/Homelander-Louis/gitgo/main/gitgo && sudo chmod +x /usr/local/bin/gitgo
```

**Windows (Git Bash)：**
```bash
mkdir -p ~/bin && curl -o ~/bin/gitgo https://raw.githubusercontent.com/Homelander-Louis/gitgo/main/gitgo && chmod +x ~/bin/gitgo && echo 'export PATH="$HOME/bin:$PATH"' >> ~/.bashrc
```

## 使用

```bash
gitgo           # 自动扫描并配置
gitgo 7899      # 跳过扫描，直接用指定端口
```

脚本做了什么：
1. 扫描 `127.0.0.1` 上正在监听的代理端口（覆盖 7890、1080、10808 等常见端口）
2. 把 Git 全局代理设为该端口（`http.proxy` 和 `https.proxy`）
3. 配置 `url.insteadOf`，让 `git@github.com:` 地址自动走 HTTPS（解决 SSH 不通的问题）
4. 用 `git ls-remote` 实际测试 GitHub 连通性

## 常见代理软件默认端口

| 软件 | HTTP 代理端口 |
|------|-------------|
| Clash Verge | 7890 |
| 山海 VPN | 7890 |
| v2rayN / Xray | 10808 |
| Shadowsocks / SSR | 1080 |
| Clash（混合端口） | 7892 |
| Privoxy | 8118 |

如果你的梯子端口不在上述列表中，运行 `gitgo <你的端口>` 直接指定即可。

## 示例

```bash
$ gitgo
检测到代理: 127.0.0.1:7890
Git 代理已更新 → 127.0.0.1:7890
✓ GitHub 连接正常

$ gitgo 7899
检测到代理: 127.0.0.1:7899
Git 代理已更新 → 127.0.0.1:7899
✓ GitHub 连接正常
```
