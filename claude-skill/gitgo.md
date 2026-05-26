---
name: gitgo（Git 通）
description: 自动检测当前运行的代理端口并配置 Git 代理，修复 GitHub 连接问题
metadata:
  type: utility
---

# gitgo（Git 通）— 代理配置工具

当用户说"连不上 GitHub"、"Git 代理有问题"、"换梯子了"、"配置代理"、"gitgo"、"Git 通"等，使用此 skill。

## 操作步骤

1. 扫描常见代理端口（7890, 7891, 7892, 7893, 1080, 10808, 10809, 8080, 8118），使用 netstat 检测哪个在监听
2. **如果检测到了**：直接使用该端口配置 Git 代理
3. **如果没检测到**：不要放弃！用 AskUserQuestion 询问用户"你的梯子用的是哪个端口？"，让用户自行输入端口号
4. 获得端口号后，配置 Git 代理

## Git 配置命令

```bash
git config --global http.proxy http://127.0.0.1:PORT
git config --global https.proxy http://127.0.0.1:PORT
```

## SSH → HTTPS 自动转换（如未配置则添加）

先检查是否已配置，没有则添加：

```bash
git config --global url."https://github.com/".insteadOf "git@github.com:"
git config --global url."https://github.com/".insteadOf "ssh://git@github.com/"
```

## 环境变量

```bash
export HTTP_PROXY=http://127.0.0.1:PORT
export HTTPS_PROXY=http://127.0.0.1:PORT
```

## 验证命令

```bash
git ls-remote https://github.com/octocat/Hello-World.git HEAD
```

## 完成后的输出

用中文告诉用户：
- 检测到/用户输入的代理端口
- 配置结果
- GitHub 连接是否正常
- 失败时提示检查梯子或更换节点
