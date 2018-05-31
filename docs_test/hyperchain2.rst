Hyperchain1.2 FAQ
================
1.概述
------



2.共识机制
----------

共识机制是保证区块链中所有共识节点（即验证节点：validating peer，
VP）按照相同顺序执行交易、写入账本的基础，而记账节点（即非验证节点：non-validating
peer， NVP）只需要从其所连接的共识节点中同步账本信息，因此无需参与共识。

Hyperchain平台支持可插拔的共识机制，可以针对区块链的不同应用场景提供不同的共识算法，当前版本已经实现了\ `PBFT算法 <http://www.usenix.net/legacy/publications/library/proceedings/osdi2000/castro/castro.pdf>`__\ 的改良算法：高鲁棒拜占庭容错算法RBFT（Robust
Byzantine Fault
Tolerance），其算法构思来源于多篇论文(尤其是\ `Aardvark <https://www.usenix.org/legacy/event/nsdi09/tech/full_papers/clement/clement.pdf>`__)，后续将陆续支持RAFT等共识算法。

客户端发送交易到Hyperchain平台，API层解析出交易后转发给共识模块，共识模块接收并缓存交易到本地的交易池（TxPool）中，交易池承担着缓存交易与打包区块的作用，因此是作为共识模块的子模块实现的。另外，共识模块还需要维护一个共识的数据库，用于存储算法所需的变量以便宕机后的自主恢复，例如RBFT算法需要维护视图，PrePrepare，Prepare，Commit等共识信息。

###1 Q:**共识过程中，区块链平台如何切换主节点？**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
### A:
Hyperchain平台的节点都有自己的ID，正常共识情况下，平台会根据ID的顺序定时轮换主节点。
当发生`viewchange`时,其他共识节点认为主节点异常，此时将平台将主动切换主节点。

###2 Q:**共识过程中，何时会发生viewchange？**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
