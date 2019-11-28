# 环银河拉力赛

感谢大家参加PlatON环银河拉力赛！

PlatON是专注于可扩展性和隐私保护的高性能分布式计算网络，是隐私计算和分布式经济的公共基础设施。PlatON于2018年12月18日发布了测试网络Baleyworld，经进十个月的迭代，目前已经升级到0.7.3，本次拉力赛将在该版本基础上开展。
## 活动目的

邀请广大社区加入PlatON生态网络，了解和熟悉PlatON Network 节点运行管理和链上治理，全面模拟主网线上运行的各种情况，为主网上线做充分准备。

## 比赛时间

- 开始时间：预计北京时间11月15日开始部署底层链，后续节点接入事宜，敬请关注gitlab和gitter公告
- 结束时间：计划北京时间12月2日完成本次拉力赛

  注：大赛组委会可能会视情况随时调整比赛内容和时间，最终解释权归大赛组委会所有。

## 比赛规则和奖励

请各参赛队伍部署并运营PlatON节点，可通过各种可行策略方法来提高节点收益排名，同时积极进行各种攻防手段来降低其他节点的收益，保护个人节点稳定。

每个参赛节点可通过各赛站的节点收益排名、签名率排名、成功举报双签及参与闯关赛获取积分，最终将按积分情况奖励表现突出的小组和个人（相同积分以节点加入时间优先原则！）。
- **收益排名前20的节点可获得如下所示积分（节点收益包括质押奖励、手续费奖励、出块奖励的总收益）。**

| 排名              | 奖励的积分 |
| ----------------- | ---------- |
| 第一名            | 1000       |
| 第二名            | 600        |
| 第三名            | 400        |
| 第四名~第十名     | 200        |
| 第十一名~第二十名 | 100        |

​      注： 相同节点收益，以节点加入时间优先排名。

- **举报双签，每举报成功一次，可获得500积分。**
  限制说明：只能用节点的质押钱包和收益钱包进行举报，否则无法判断举报用户是哪个。

- **签名率排名前20的节点可获得如下所示积分。**

| **排名**          | **奖励的积分** |
| ----------------- | -------------- |
| 第一名            | 800            |
| 第二名            | 600            |
| 第三名            | 400            |
| 第四名~第十名     | 200            |
| 第十一名~第二十名 | 100            |
- **在线率排名前20的节点可获得如下所示积分。**

| **排名**          | **奖励的积分** |
| ----------------- | -------------- |
| 第一名            | 600            |
| 第二名            | 400            |
| 第三名            | 200            |
| 第四名~第十名     | 100            |
| 第十一名~第二十名 | 50             |

​    注：在线率是当节点处于在线可连接状态的比率（需要参赛节点提供节点IP和端口）；相同节点收益，以节点加入时间优先排名。

- **参与闯关赛的节点可获得积分**


| **闯关赛**       | **要求**                           | 奖励的积分 |
| ---------------- | ---------------------------------- | ---------- |
| 链上治理升级演练 | 压测过程中处于节点列表中且保持在线 | 200        |
| 全网压测         | 压测过程中处于节点列表中且保持在线 | 200        |
| DB回滚演练       | 参与DB回滚的节点                   | 400        |

注：如有特殊情况（如比赛异常中断、分叉、出现纠纷等），积分和奖励规则可能会有调整，最终解释权归大赛组委会所有。

## 赛前准备
- 机器资源：


  由PlatON运维组统一提供。

- 请在拉力赛底层链部署成功后尽快前将以下信息发送到[rally@platon.network](mailto:rally@platon.network)，由组委会统一协调节点转账。

  ```
  参赛节点信息：节点ID、节点名称、节点IP和端口（对外公开的P2P端口）、节点质押账户地址、节点收益账户地址
  ```

- 本次比赛的代码分支：
  https://github.com/PlatONnetwork/PlatON-Go/tree/v0.7.3.2

- 下载并安装ATON钱包
  Android版下载地址：

  版本验证中，敬请关注后续拉力赛公告

  iOS版下载地址：
  
  版本验证中，敬请关注后续拉力赛公告
  
- 下载节点管理工具   

## 讨论区

如有任何问题，请通过以下渠道向组委会反馈，或向社区提问：
- Gitlab： http://192.168.9.66/PlatON-GalaxyRally/GalaxyRally
  您可以在这里找到本次拉力赛的所有相关文档、SDK、比赛进展公告，及所有的问题讨论。

- 拉力赛支持邮箱：[rally@platon.network](mailto:rally@platon.network)

- gitter社区：由于是内部赛，gitter room成员由组委会定向邀请加入

如对比赛规则、结果有异议，请发送邮件给PlatON运营支持。

## 文档库

以下资料已上传到GitLab，部分内容将实时更新。

### 节点

- [PlatON验证节点介绍](http://192.168.9.66/PlatON-GalaxyRally/GalaxyRally/blob/master/Phase1/PlatON验证节点介绍.md)
- [PlatON节点安装部署手册](http://192.168.9.66/PlatON-GalaxyRally/GalaxyRally/blob/master/Phase1/PlatON节点安装部署手册.md)
- [在线MTool使用手册](http://192.168.9.66/PlatON-GalaxyRally/GalaxyRally/blob/master/Phase1/在线MTool使用手册.md)
- [离线MTool使用手册](http://192.168.9.66/PlatON-GalaxyRally/GalaxyRally/blob/master/Phase1/离线MTool使用手册.md)

### 钱包

- [ATON使用手册](http://192.168.9.66/PlatON-GalaxyRally/GalaxyRally/blob/master/Phase1/ATON钱包用户使用手册.md)

### 治理与升级

- [PlatON链上治理升级指南](http://192.168.9.66/PlatON-GalaxyRally/GalaxyRally/blob/master/Phase1/链上治理升级指南.md)
- [PlatON链下数据回滚升级指南](http://192.168.9.66/PlatON-GalaxyRally/GalaxyRally/blob/master/Phase1/链下数据回滚升级指南.md)

### 开发指南

- [JAVA SDK](http://192.168.9.66/PlatON-GalaxyRally/GalaxyRally/blob/master/Phase1/Java-SDK.md)
- [JS SDK](http://192.168.9.66/PlatON-GalaxyRally/GalaxyRally/blob/master/Phase1/JavaScript-API.md)
- [JSON RPC](http://192.168.9.66/PlatON-GalaxyRally/GalaxyRally/blob/master/Phase1/JSON-RPC.md) 

### 拉力赛FAQ

- [FAQ](http://192.168.9.66/PlatON-GalaxyRally/GalaxyRally/blob/master/FAQ.md)

