

联盟链的应用
==========

1. 概述
-------

共识机制是保证区块链中所有共识节点（即验证节点：validating peer，
VP）按照相同顺序执行交易、写入账本的基础，而记账节点（即非验证节点：non-validating
peer， NVP）只需要从其所连接的共识节点中同步账本信息，因此无需参与共识。

Hyperchain平台支持可插拔的共识机制，可以针对区块链的不同应用场景提供不同的共识算法，当前版本已经实现了\ `PBFT算法 <http://www.usenix.net/legacy/publications/library/proceedings/osdi2000/castro/castro.pdf>`__\ 的改良算法：高鲁棒拜占庭容错算法RBFT（Robust
Byzantine Fault
Tolerance），其算法构思来源于多篇论文(尤其是\ `Aardvark <https://www.usenix.org/legacy/event/nsdi09/tech/full_papers/clement/clement.pdf>`__)，后续将陆续支持RAFT等共识算法。

客户端发送交易到Hyperchain平台，API层解析出交易后转发给共识模块，共识模块接收并缓存交易到本地的交易池（TxPool）中，交易池承担着缓存交易与打包区块的作用，因此是作为共识模块的子模块实现的。另外，共识模块还需要维护一个共识的数据库，用于存储算法所需的变量以便宕机后的自主恢复，例如RBFT算法需要维护视图，PrePrepare，Prepare，Commit等共识信息。



2. 应用场景
---------------



3. 架构设计
---------------

RBFT的常规流程保证了区块链各共识节点以相同的顺序处理来自客户端的交易。RBFT同PBFT的容错能力相同，需要至少3f+1个节点才能容忍f个拜占庭错误。下图为最少集群节点数下的共识流程，其N=4，f=1。图中的Primary1为共识节点动态选举出来的主节点，负责对客户端发来的交易进行排序打包，Replica2，3，4为从节点。所有节点执行交易的逻辑相同并能够在主节点失效时参与新主节点的选举。

常规流程
~~~~~~~~



