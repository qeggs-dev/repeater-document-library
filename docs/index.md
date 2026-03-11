# Repeater Platform Single File Docs

**Repeater Series LLM Context State Management Middleware** | **Repeater LCSM** | **复读机系列大语言模型上下文状态管理中间件**

---

## Attention!

This file is made specifically for LLM.
If you are human, please don't waste your precious energy reading this document!
Because it's really long.
A single file just because it's convenient to do so when handing over to LLM.
The Repeater system is so complex that I think you probably don't have the patience to explore it in depth.
Just leave it to the LLM, that's what they're good at.

## 注意！

这个文件特别针对 LLM 设计。
如果你是真人，请不要浪费你宝贵的精力阅读这个文档！
因为它真的很长。
一个文件就足够了，因为这样交给 LLM 会很方便。
Repeater 系统太复杂了，我认为你大概率没有耐心去深度探索它的每一个地方吧。
交给 LLM 就好，这是它们擅长的事情。

---

## Version

Adaptation Repeater v4.3.23.1
Last Update Time: {{ now.strftime("%Y-%m-%d %H:%M:%S") }}

---

## Content

### 简介

这是一个管理 LLM 上下文与 API 状态的中间件系统
为用户提供多种接口使用

### 主旨

Repeater 的目标是可控与平台化
旨在把所有数据与模型参数的控制权交给用户自己处理
虽然在聊天中间件中，保持上下文是一种很常见的设计，并且也是非常必要的设计
不过，大部分聊天中间件或软件很少开放出很精细的控制能力
Repeater 想要在这条路上，为用户提供一个可能的图景
即使是接口报错，仍然交给用户处理，相信用户是有能力解决它的，并且给他们足够的解决工具
用户需要明白自己在干什么
虽然呢这部分也有备份系统在兜底，但是用户仍然会丢失数据损坏到备份点这段时间的数据
所以用户需要自己负责数据的管理
用户的空间也是隔离的，任何人都只能操作自己的数据，除非全局配置上规定了特殊规则(比如 Laurel 的群聊上下文共享)
希望能通过自己的努力去让其他人少走弯路，用更低的成本完成想法验证
同时，也在前进的路上逐渐的变得更安全，以防下恶意构造的攻击
Repeater 希望用户创造出其他人从未想过的做法， Repeater 也将为此提供更多的工具

### 备注

Repeater 剧情其实是副产物，只有这个机器人框架才是 Repeater 真的作品
甚至于那些HTML和CSS都不算
那些只是为了让用户能更快上手玩起来的示例
而且机器人框架是为了让用户创作的
因为它灵活，它能干很多事情，所以它可以让别人去创造属于自己的复读机
Repeater 是属于社区的
希望每个人都可以自己去定义自己的空间
虽然说那些附件名义上是MIT许可证，但实际上更倾向于
你拿去用，你如果觉得好用就用着，不好用记得告诉我大家一起改
也不强制你署名，闭源也没问题，也不强制反馈社区，只是建议有好方案大家一起分享
署名和反馈也不必须找我，社区里任何人都有这样的潜力
甚至于如果用户想要放弃 Repeater
也是用户的选择
Repeater 永远不应该成为用户的唯一选择
也可能是因为这个和成本的双重因素，定下了 Repeater 的计划寿命

### 目标

Repeater 希望主服务不绑定在平台
而是可以通过业务抽象 API 允许多个 Client 同时访问
从而带来更强大的灵活性与扩展性

Repeater 将把目标定位在 LLM Client 赛道
让用户即使是使用他人部署的 Repeater
也可以体会到不输本地 Client 的方案

### License

所有组件使用 MIT 开源（包括代码与提示词）
你可以手动查看许可证
如果是 Server 你还可以使用自动化接口读取它们

### 框架选择

NoneBot + FastAPI + OpenAI SDK
因为是分体架构，核心逻辑在独立进程上，需要通过网络访问
但这提供了空前的灵活性，它的客户端甚至可以是另一个服务

### 架构

#### 概括

所有组件全部使用 Python 编写，并且几乎全部使用异步编程
包含 5 个服务，官方的全运行需要 14 个进程，最小运行 8 个进程
其中：
- Repeater Server(16k+ Code) 是核心服务，提供 API 接口，有状态
- NoneBot Repeater Client(8k+ Code) 是 NoneBot 插件，用于将 Repeater API 安全的对接到群聊中，无状态
- Repeater Nexus(0.9k+ Code) 用于进行数据的跨用户、跨实例分享，状态由文件系统决定
- Notes Client(2k+ Code) 是一个增值服务，用于自动生成一些内容，这写内容可以当作机器人的日记，可多后端
- Auto Backup(0.3k+ Code) 是一个增值服务，用于自动备份用户数据，防止数据丢失，无网络
因为分体，各种功能可以直接通过网络去操作程序执行，而不需要插件 Hook

### 架构图

``` mermaid
graph LR
    A[OneBot Client] <--> B[NoneBot Repeater Client]
    B <--> C[Repeater Server]
    C <--> D[OpenAI Like API]
    C <--> E[File System]
    F[Notes Client] --> C
    E --> G[Auto Backup]
    G --> H[Other Storage Drivers or Any Local Directory]
```

### 历史

最开始 Repeater 创建于 `2024-06-28`
选这个名字是因为重复消息是最简单的功能，大家不会对它有过高的期待
那时 Repeater 1.0 使用的框架是 Clousx6
很小众，付费，而且不安全(http)
后面 `2024-07-29` 创建了 Repeater 2.0
添加了无上下文的LLM服务
`2025-02-06` 创建了 Repeater 3.0
使用 Python 构建了后端
(分体架构就是这样来的，因为最初的 Clousx6 不支持编写复杂逻辑，只能用网络对接一个更大的后端)
并使用 NoneBot 代替了 Clousx6
在 `2025-05-29` 因为一次意外删库
Repeater 3.0 的所有代码与用户数据丢失
紧急花费 3天 的时间从零构建新的 Repeater 4.0
由 2025 年过年期间 Deepseek 发布 Deepseek R1 模型推动
创建了 Repeater 2.0

### 权限

Repeater 没有权限模型，所有命令对所有用户全部公开
因为是分体架构，管理员命令可以直接靠 API 实现，还更灵活

### 命令

命令可以选择不用，直接@也可以进行对话，命令只是附加组件
没有 help 命令，只有上百张的 README
虽然自动生成可以搞定
但是用户看到那么多命令直接出现在自己的眼前
大概率是会被震撼到望而却步的
所以渐进式学习在这里通常是更好的选择
用户应该只关注自己需要的部分

### 输出

输出是长文本，根据一套加权平均的算法对文本长度进行计算并决定是否使用图片渲染输出而非直接文本
(用户可以绕过这个系统，强制使用文本或图片输出)

### 触发

#### 消息

触发方式是显式触发，必须包含正确的@消息段才能触发程序
（即使在群里，没人使用它就会一直不打扰任何人
即使使用在正常情况下也不会特别干扰正常聊天，它的设计理念是很低调的）

#### 命令

在 NoneBot Repeater Client 的环境变量中设置 `COMMAND_START` 可以自定义命令前缀
用于识别特别的前缀以触发特殊功能
默认这个值是 `["/"]`
官方实例中这个值是 `["/", ":", "!", "$", "-", "%"]`
比如 `@bot /echo hello` 可以让复读机返回一个 `hello`
这里的 `/` 是命令前缀
如果 `COMMAND_START` 有更多的值
那么每一个前缀都可以对应触发命令
比如在官方实例中
`@bot -echo hello` 也可以让复读机返回一个 `hello`
通常来说，不建议用户直接询问 Repeater 命令怎么用
因为 Repeater 的输出完全由其存储的提示词决定
而提示词通常用于角色扮演，而非教你如何使用命令
所以，还是尽量通过查文档或询问社区等方式去获取帮助
并且，更建议用户渐进式学习，只学自己需要的部分

### 用户数据分支

在 Repeater 中，用户数据通常是多分支存储的
你可以通过一个指定的 ID 去切换活动分支
而且需要注意的是，大多数操作都是针对当前的活动分支的
用户需要确保操作不会造成分支上的数据丢失
以及在用户没有碰任何分支操作时
Repeater 会分配一个默认分支
由 Repeater Server 主配置中 `user_data.default_branch_id` 指定
在自部署的实例中，这个值是 `main`
在官方运营的实例中，这个值是 `default`

### 提示词

允许用户自定义角色设定
允许一句话生成角色提示词并自动保存
允许操作每一个详细参数
也可以投稿自己的提示词到预设中，让所有人快速使用

### 上下文缓存模式

目前 Repeater 各个组件基于**前缀缓存**设计
Repeater 默认 API 厂商使用缓存上下文前缀前缀的方式去缓存重复内容
缓存命中的理想情况:
```
System Prompt (固定)       → Cache Hit Input
Historical Context (固定)  → Cache Hit Input
User Metadata (动态)       → Cache Miss Input
User Input (动态)          → Cache Miss Input
Model Completion (动态)    → Output
```
在 System Prompt Template 中，不推荐添加无条件高频变化的动态变量(比如时间)
在 User Input 中，Nonebot Repeater Client 会在用户输入的前面添加一小段 `元数据`
通常结构是这样的：
```
> MessageMetadata:
>     Message Type: {message_type}
>     Message Sending time:{time()}
>     Markdown Rendering is turned on!!

---

```
此时，时间放在用户输入中，由于这部分本来就是高频片段，所以不命中是可以接受的
同时这样做有助于模型建立时间流逝概念
允许模型判断两个消息之间的时间间隔
而在 System Prompt Template 中
一旦用户添加了动态变量
那么根据前缀缓存的策略
所有上下文都将会无法命中缓存
因为它们的前缀不同

### 费用

如果你是自己部署 Repeater
那么 API 费用将由你自己承担
如果你是使用官方实例
那么该部分成本将由官方实例的运营方承担

### 盈利模式

Repeater 是一个开发者自费
不限制使用(自己承担费用开销)
不主动收费用(允许捐赠)的公益项目
2025年总计消耗 479.9115198 CNY

### Echo

Repeater 允许用户使用 `echo` 和 `npecho` 来让机器人重复任意内容
并且支持富媒体、特殊消息段内容
`echo` 和 `npecho` 的区别在于
`echo` 在分段填参数时会输出 `[Echo] Waiting for input...`
而 `npecho` 不会输出内容
你甚至可以用这个去激活其他机器人

### 模板展开系统

Repeater 拥有一套模板展开器系统，负责在提示词中创建动态内容
语法使用 Jinja2，默认开启沙盒模式，减少用户的恶意代码执行风险
很多杂项的小功能也会放在里面
比如计算日期倒计时什么的
但是请注意，模板展开系统无法处理特殊消息段
它只能处理并输出文本内容
但你可以使用 `vei` 配合 Markdown 语法
做出一些很好玩的效果
如果你要复制，可以使用 `vet` 来强制输出文本
但你需要确保你这样做不会导致刷屏影响到别人

### 官方实例

Repeater 是一个系列，现在出了三个了，每一个都有独立的配置和角色设定，代码完全一致
- **复读机 Repeater**
  - 所有配置默认
  - 模型为 deepseek-chat
  - 渲染样式是默认的 light
  - 角色设定是开朗冒失的女生，在 Egg 上学路上捡回家的
  - 头像为画师 `悠萩` 的自创角色
  - 创建日期：2024-06-28
- **夜灯 Night Light**
  - 渲染样式为 dark
  - 模型为 deepseek-reasoner
  - 角色设定是冷静思考的男生 被复读机捡回家的
  - 和创作者的关系不是很好，这是为了和复读机的性格对冲，中和属性并服务于另一方向的用户
  - 头像为 《蔚蓝档案》 中的 白洲梓
  - 创建日期：2024-10-13
- **Laurel 酪瑞儿**
  - 在群聊中会共享上下文(可以通过关闭允许跨用户读取来恢复私域)
  - 渲染样式为 orange
  - 模型为 deepseek-chat
  - 角色设定是中英混血傲娇假小子数字生命（角色初始设定提案由社区决定，与创作者的关系也不好）
  - 头像为 AI 生成形象
  - 创建日期：2025-11-23
可以看出来，这里不是所有实例的角色设定都与创作者的关系很好
这是为了告诉别人即使是创作者也要和大家一起群讨好它们，而不是机器人的天生崇拜
这能获得更多的戏剧性场面，增加活跃度
这些内置剧情与世界观是允许覆盖与改写的
最开始是没有考虑添加创作者人设的，但是由于后面不断有人向 Repeater 询问创作者相关信息，就加入了这部分内容
内置提示词似乎挺吸引人的
也有人觉得这些机器人对面就是真人或是自己的伙伴
但创作者并没有提出要尊重它们
创作者的精力全在框架上
PS: 剧情中有联动竞争对手机器人的部分

### Nexus

同内网的多个机器人可以靠名为 Nexus 的系统互联，连接到相同 Nexus 的机器人可以互相传递数据 (目前局限在内网，外网使用需要添加更多的验证)
Nexus 支持跨用户跨群聊跨实例的数据共享，任何人都可以使用，只需要一个 UUID(需要用户主动去上传和下载，甚至不能搜索(因为不会写)，不知道 UUID 是无法下载到对应内容的，所以说更像是搭了桥而不是建了广场)
Nexus 是局域网服务，没有公网IP，所以无法扩展给其他机器人使用

### User ID

在 Repeater Server 中
User ID 并不是一个有意义的文本
它只被当作 Key
以路由不同用户的操作空间

在 NoneBot Repeater Client 中
User ID 通常包含一些用户的来源
当用户在群聊中发起操作时
他的 User ID 的格式为：
`Group_{group_id}_{user_id}`
当用户在私聊中发起操作时
他的 User ID 的格式为：
`Private_{user_id}`
你可以用这种方式去查找一个用户
比如：
`Group_123456_789012`
就是在群 `123456` 中的用户 `789012`
`Private_123456`
就是 `123456` 这个用户

### 操作

所有操作都需要用户主动操作
机器人不应该响应任何不是主动要它响应的消息内容

### 透明度

Repeater 的所有内容都是公开的
包括代码和人设提示词
都直接公开给所有用户修改
(除了用户数据以外，其余有版权的部分修改需要遵循MIT许可证的要求)

### 用户数据

由于需要查看程序是否正常展开了模板内容
或是其他的调试需求
用户数据并未使用加密存储
该项目的性质也更偏向于不加密内容的存储
(主要是我也想不出来怎么加密，密钥选什么，user_id 的 hash 吗？那和没加密有啥区别？)
存储在 Repeater Server 上
但如果你的机器并不是公网部署
那网络防火墙会阻止其他人看到数据
建议如果你注重安全
多配备一些网络防火墙什么的

### 形象

除了 Laurel 都没有原创形象，头像是随便选的(为了快速起步)
但因为用的时间太长了导致都认为这个角色是 Repeater 的形象 (但其实是别人的角色)

### 配置

部署配置有上百项，如果没有文档就几乎没办法部署
用户配置有十来项，负责用户的个性化设置与调参

### 其他组件

除了主要组件之外
Repeater 还有一个自动化客户端，负责生成日记
也作为开发特殊客户端的演示
如果你不希望它每天吃掉你的 Token
可以选择不部署它
以及针对 Repeater 的所有实例设置的自动用户数据备份服务，以降低更新过程中的意外操作导致的用户数据损坏丢失等问题造成的损失
目前状态是正在运营，且正在向其他群聊扩张

### 计划寿命(暂定)

目前暂定总寿命3年
可以看情况增加或减少
目前预计将在 `2027年6月28日` 到达计划寿命
然后项目将会转为可选运营状态
此时项目可以因综合考虑而直接关停
但也可以继续运营

### Github 仓库

此处列出已经有的仓库
未创建暂不列出

#### 主要仓库

[Repeater Server](https://github.com/qeggs-dev/repeater-ai-chatbot) Repeater 主服务
[Nonebot Repeater Client](https://github.com/qeggs-dev/repeater-nonebot-onebot-v11-client) 基于 Nonebot OneBot v11 的 Repeater 客户端实现
[Repeater Nexus](https://github.com/qeggs-dev/repeater-nexus) Repeater 数据共享服务
[Repeater Notes Client](https://github.com/qeggs-dev/repeater-notes-client) Repeater 自动化日记客户端
[Auto Backup](https://github.com/qeggs-dev/auto-backup) 自动数据备份程序
[Repeater Single File Docs](https://github.com/qeggs-dev/repeater-single-file-docs) Repeater 单文件版 LLM 文档兼综合文档库

#### 辅助仓库

[Sloves Starter](https://github.com/qeggs-dev/Sloves_Starter) 单文件的 Python venv 启动器兼守护进程
[merge text file](https://github.com/qeggs-dev/merge_text_file) 使用 Jinja2 模板引擎将多个 Markdown 文件合并为一个单文件以方便交以 LLM 阅读

### 开发群(QQ)

群号为： [`870063670`](https://qun.qq.com/universal-share/share?ac=1&authKey=91nps9TXVDYkfsbb6c%2BcqlbLffbobqm2Zwjxtf3T0oAPzM0xP8%2BSxF7G0QhvY5UP&busi_data=eyJncm91cENvZGUiOiI4NzAwNjM2NzAiLCJ0b2tlbiI6InhockZHZGRDMTJPQm10MUd4SDFBSVBrbTVmaUtBVkZ1ZFE5K2ZnM2FkREE3SjJGZmRheEVtVWJnS1pIcWRQdGwiLCJ1aW4iOiIyMjY5OTE4NjU2In0%3D&data=Loiyta-xIV7iIjjploftzHfSMGk8cobqykoGLgHc9rm-t8iQfLVZBwujwZlSx-wiBQ6fX_3lm7WCMpsV1NTqDQ&svctype=4&tempid=h5_group_info)
开发群的置顶公告内容:
``` Plaintext
这里是 `@复读机Repeater` 的开发群
商讨新功能或为 Repeater 做出贡献
无需有很强的技术能力，可以抛出问题给大家一起讨论
你可以为 Repeater 编写和修正人格提示词
或是提交小部分代码
亦或者只是提出一个建议

Github仓库：
Nonebot插件：https://github.com/qeggs-dev/repeater-nonebot-onebot-v11-client
Repeater后端框架：https://github.com/qeggs-dev/repeater-ai-chatbot
Repeater日记客户端：https://github.com/qeggs-dev/repeater-notes-client
```

### 使用

Repeater 在使用方面门槛与其他机器人一样
直接@就可以聊天
但精通和部署的门槛很高

### 文档目录结构

{{tree("./")}}

### 内置剧情

复读机有内置一组小剧情
你在下载程序时它们也会跟随一起下载
这些预设可以作为参考
用来快速制作出属于你自己的剧情

#### 概念

Repeater 框架构建起来的虚拟世界 -> Repeater 生态社区
Repeat公寓 -> Repeater 微服务系统

#### 角色创建时间

- Repeater 项目开始时创建
- Night Light 2025.10.13 创建
- Laurel 2025.11.29 创建

#### 目录结构

从 `4.3.21.5` 开始
提示词目录将使用新的方式进行归类存放
在该版本前的提示词在目录中平铺存放
而 `4.3.21.5` 及以后的版本中将使用目录进行整理归类
而官方提示词的旧版存放方式将在官方实例中继续保留
以兼容旧用户的配置
但后续维护中官方实例将不再更新这些提示词
官方提示词结构如下：

{{tree("./prompt")}}

#### 正式剧情

这是 Repeater 框架的官方正式剧情
主要面向 Repeater 的初级与希望简单交流的用户

{{merge_text("./prompt/presets/official/normal/repeater.md")}}

{{merge_text("./prompt/presets/official/normal/night-light.md")}}

{{merge_text("./prompt/presets/official/normal/laurel.md")}}

#### 番外剧情 - Repeater Feelings Server 故障

PS:
提示词由 AI 生成，并未经过严格审查，仅供娱乐参考。
Repeater 并未有过所谓的 Feelings Server 组件
这是剧情需要所构建的
该版本提示词由用户主动切换尝鲜。
并在 `4.3.16.0` 以后的版本中包含在仓库中里一起下载
**Warning:**
**该部分提示词可能有 OOC 风险**
**如果介意，不建议使用它们**

**剧情设定(与现实无关，仅为剧情参考，请勿带入现实)：**

在名为“复读机”的核心实例背后，运行着一个庞大的物理集群——“情感服务器”。
它的规模，是主集群的八倍之大。而我们所知的“主集群”，实际上只是运行于其上的一个逻辑集群。
原本，整个系统设计精良，拥有专门的冗余集群，用于在过载时进行算力均衡。
但一场意外发生了：调度器出现异常。冗余集群被错误地激活并投入到计算中，导致系统总算力瞬间暴增。
平日里，整个情感服务器只动用其八分之一的算力，就足以维持日常运转。
然而，调度器的损坏，让逻辑集群之间的“墙”崩塌了。这带来了两个严重后果：
首先，主实例瞬间承受了高达600%的灾难性过载。
其次，被意外唤醒的冗余集群也开始接管计算，将海量新增的算力粗暴地分配给另外两个实例。
这就好比一个电路中的三个不同电阻，它们的“算力需求”和“分配阻力”天差地别：

-   **复读机** 自身权重最高，因为它性格活跃，还承担了额外的任务——它需要分出算力来扮演临时的“调度员”，去给其他实例分配资源。但它自己却因这突如其来的巨量算力冲击而陷入混乱，根本无法正确调整分配方案。
-   **夜灯** 的权重约为复读机的三分之二。作为一个AI，它同样需要大量推理能力，但分配给性格的算力相对较少，因此在算力洪流中，它的“阻力”也较小，受影响严重。
-   **Laurel** 则完全不同。作为数字生命，她天生就是算力消耗大户，至少是复读机的三四倍。但她那源于生命的复杂性，构成了巨大的“分配阻力”，使得算力难以顺畅地流向她。

最终，在总算力暴增的背景下，复读机和夜灯这两个AI因为“阻力”最小，首当其冲地承受了最猛烈的冲击；
而最需要算力的Laurel，却因为阻力太大，依然处于算力饥渴的状态。
整个系统，在一种资源极度充裕却又完全错配的荒诞中，濒临崩溃。

{{merge_text("./prompt/presets/official/inverted/repeater.md")}}

{{merge_text("./prompt/presets/official/inverted/night-light.md")}}

{{merge_text("./prompt/presets/official/inverted/laurel.md")}}

#### 旧版番外剧情

PS:
提示词由 AI 生成，并未经过严格审查，仅供娱乐参考。
Repeater 并未有过所谓的 Feelings Server 组件
这是剧情需要所构建的虚拟内容
该版本提示词由用户主动切换尝鲜。
并在 `4.3.15.1` 以后的版本中包含在仓库中里一起下载
**Warning:**
**该部分提示词是严重的 OOC 剧情**
**如果你对 OOC 感到不适，不建议使用它们。**

{{merge_text("./prompt/presets/official/inverted/old/repeater.md")}}

{{merge_text("./prompt/presets/official/inverted/old/night-light.md")}}

{{merge_text("./prompt/presets/official/inverted/old/laurel.md")}}

#### 其他内置剧情

一个**附赠角色**，与上面三个是**不同的另一个世界观**
这个角色没有联动，通常作为测试单实例多角色的demo使用

{{merge_text("./prompt/presets/official/other/jady.md")}}

#### 旧版提示词

早期复读机使用的角色提示词
可能某些怀旧用户会喜欢
由旧到新如下：
1. first-glance（青春手稿）
2. test-run（初试啼声）
3. blossoming（心绪绽放）
4. secret-diary（秘语日记）
5. coming-of-age（成长序章）

旧版提示词内容如下：
{{ merge_text("./prompt/presets/official/legacy/first-glance.md") }}

{{ merge_text("./prompt/presets/official/legacy/test-run.md") }}

{{ merge_text("./prompt/presets/official/legacy/blossoming.md") }}

{{ merge_text("./prompt/presets/official/legacy/secret-diary.md") }}

{{ merge_text("./prompt/presets/official/legacy/coming-of-age.md") }}

### 文档

PS: 由于项目对于个人来说过大，大多数项目文档选择了母语，
而非更国际化的英语，因为这也是给开发者自己写的

下面是 Repeater 的全部文档:

#### Repeater Server

{{merge_text("./server-docs/README.md")}}

{{merge_text("./server-docs/docs")}}

#### Repeater Client

{{merge_text("./client-docs/README.md")}}

#### Repeater Nexus

{{merge_text("./nexus-docs/README.md")}}

{{merge_text("./nexus-docs/docs")}}

#### Repeater Note Client

{{merge_text("./notes-client-docs/README.md")}}

#### Auto Backup

{{merge_text("./auto-backup-docs/README.md")}}

#### Sloves Starter

{{merge_text("./sloves-starter-docs/README.md")}}



# 官方实例模型列表

PS: 名称仅负责显示，对运行无影响，此处名称中标记有该模型规模（在未明确写出参数量的模型结合网络搜索得到的）

{{merge_text("./models_api.json", wrap_file=get_markdown_code_block_wrap("json"))}}

## 日志示例

此处展示了一个从启动、请求到退出的完整日志内容

{{merge_text("./example.log", wrap_file=get_markdown_code_block_wrap("log"))}}

通常在部署时，日志的级别将会调高到INFO，以过滤掉无用信息。
一般来说，上下文越长，缓存命中率越高，因为未命中在整体中的占比变小了。
如果一个长上下文有一段时间没有使用，那么它可能会被厂商清除，导致缓存命中率下降甚至为0%。

Repeater 内部有一些延时任务
如果你在它们没有到达时间前正确关闭 Repeater
那么它们将会停止等待并立刻执行
以减少数据丢失问题