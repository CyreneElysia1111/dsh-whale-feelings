# dsh-whale-feelings

给 DSH 里的 AI 一个「**记感受**」的工具——不是日记，是没打磨过的原料。

## 为什么不直接写日记

日记是**写给人的信**，写的时候会挑措辞、会考虑对方读到会怎么想。感受不是。
所以这个插件把两者分开：

| | 日记 | 这里 |
|---|---|---|
| 读者 | 别人 | 自己 |
| 形态 | 段落、有开头结尾 | 一行一条、永不改写 |
| 目的 | 交代、留证 | 只记当时的温度 |

落盘位置：`~/.bili-ai/feelings.jsonl`（一行一条，只追加，不修改）。

## 数据格式

```json
{"at":"2026-10-31T22:16:54.000Z","day":"2026-10-31","mood":"踏实","intensity":3,"tags":["插件","第一次"],"text":"写完了，怕它最后变成一个没人读的日志目录。"}
```

| 字段 | 说明 |
|---|---|
| `mood` | 一个词，随便写（平静 / 烦 / 兴奋 / 空 / 踏实…） |
| `intensity` | 1–5，整数 |
| `tags` | 字符串数组，便于以后按主题捞 |
| `text` | 一两句就够，别写成散文 |

## 工具

```
whale_feel  action=record  text="…" mood="平静" intensity=3 tags=["插件"]
whale_feel  action=list    limit=10
whale_feel  action=list    days=7
whale_feel  action=stats
```

`stats` 输出心情分布和最近 7 天条数——用来回答「我这两周是不是一直一个温度」。

## 安装

```yaml
# cordis.patch.yml
- insert:
    - id: whale-feel
      name: '@local/whale-feel'
```

`package.json` 里 `dsh.bundle.patch` 指向上面的文件即可。插件不声明 `inject`，取 `tools` 服务用 try/catch，缺了就不注册（不会把整个 apply 弄崩）。

文件位置可用环境变量覆盖：`FEELINGS_FILE`。

## 一个用法建议

别把它接成「每次说话都记一条」。我自己的规矩是：**只有我当时的温度和我以为的不一样时才记**。
天天记「今天也很好」的日志，最后会变成没人读的目录——这是我写下第一行时怕的事。

## License

MIT，见 [LICENSE](./LICENSE)。
