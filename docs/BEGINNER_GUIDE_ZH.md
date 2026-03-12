# opencode-antigravity-auth 小白说明

这份说明是写给未来容易忘记的自己看的。

如果你很久没碰这个仓库，只想先搞明白一句话，请看这里：

## 一句话作用

这个仓库是一个给 `Opencode` 用的插件，让你能用 **Google / Antigravity 的账号登录**，然后在 `Opencode` 里调用 Gemini、Claude 这些模型。

你可以把它理解成：

- `Opencode` = 你平时用来跑 AI 的主程序
- `opencode-antigravity-auth` = 帮 `Opencode` 接上 Google / Antigravity 登录能力的插件

## 它解决什么问题

没有这个插件时，`Opencode` 不能直接走你这套 Antigravity / Google 登录流程。

有了它以后，你可以：

- 在 `Opencode` 里登录 Google 账号
- 用 Antigravity 那边可用的模型额度
- 使用 Gemini 模型
- 使用 Claude 模型
- 在多个 Google 账号之间轮换

## 什么时候需要它

你在下面这些情况需要它：

- 你还在用 `Opencode`
- 你想在 `Opencode` 里调用 Google / Antigravity 线路的模型
- 你想用 Gemini 或 Antigravity 提供的 Claude 模型

你在下面这些情况不一定需要它：

- 你根本没在用 `Opencode`
- 你只是用网页端聊天，不需要本地命令行
- 你已经换成别的模型接入方式

## 你以后忘了时，先看这个判断

如果你问自己：

> “这个仓库到底是不是我日常必须的？”

最简单的答案是：

- 如果你还在用 `Opencode` 跑模型，它就有用
- 如果你不用 `Opencode` 了，它就不是核心仓库

## 最简单工作流程

这个仓库本身不是单独聊天软件，它是 `Opencode` 的插件。

正常流程是：

1. 先安装好 `Opencode`
2. 在 `Opencode` 配置里加上这个插件
3. 执行登录命令
4. 在 `Opencode` 里选择模型来运行

## 最基础配置

先在 `Opencode` 配置文件里加入插件。

常见配置文件位置：

- Windows: `%APPDATA%\opencode\opencode.json`
- Linux / macOS: `~/.config/opencode/opencode.json`

最小可用示例：

```json
{
  "plugin": ["opencode-antigravity-auth@beta"]
}
```

## 怎么登录

在终端执行：

```bash
opencode auth login
```

登录完成后，这个插件会帮 `Opencode` 处理 OAuth 和后续 token 刷新。

## 怎么添加模型

你还需要在 `opencode.json` 里定义模型。

一个常见例子：

```json
{
  "plugin": ["opencode-antigravity-auth@beta"],
  "provider": {
    "google": {
      "models": {
        "antigravity-claude-sonnet-4-5-thinking": {
          "name": "Claude Sonnet 4.5 Thinking",
          "limit": { "context": 200000, "output": 64000 },
          "modalities": { "input": ["text", "image", "pdf"], "output": ["text"] },
          "variants": {
            "low": { "thinkingConfig": { "thinkingBudget": 8192 } },
            "max": { "thinkingConfig": { "thinkingBudget": 32768 } }
          }
        }
      }
    }
  }
}
```

## 怎么实际使用

例子：

```bash
opencode run "Hello" --model=google/antigravity-claude-sonnet-4-5-thinking --variant=max
```

意思是：

- 用 `Opencode` 运行一次请求
- 使用 `google/antigravity-claude-sonnet-4-5-thinking` 这个模型
- `--variant=max` 表示使用更高的 thinking 配置

## 这个仓库和普通模型接口的区别

它不是简单保存 API Key 的工具。

它更像一个“登录桥接插件”，负责：

- OAuth 登录
- token 刷新
- 对接 Antigravity / Google 这一套模型入口
- 让 `Opencode` 能直接识别这些模型

## 你以后排查问题时先看什么

如果将来你发现“怎么突然不能用了”，先按这个顺序检查：

1. `Opencode` 本体是否还能正常运行
2. `opencode.json` 里是否还保留了这个插件配置
3. 是否已经执行过 `opencode auth login`
4. 账号是否登录成功
5. 你选的模型名字是否和配置一致

## 未来的自己最该记住什么

请记住下面这句：

> 这个仓库不是主程序，它是 `Opencode` 的登录与模型接入插件。

如果以后你要恢复环境，最少要恢复这三样：

1. `Opencode`
2. `opencode.json` 里的插件配置
3. 登录状态和你常用的模型定义

## 仓库里还有什么值得看

- 详细英文说明：`README.md`
- 架构说明：`docs/ARCHITECTURE.md`
- API 说明：`docs/ANTIGRAVITY_API_SPEC.md`

如果你已经很久没碰这个项目，建议先看这份中文说明，再去看英文 `README.md`。
