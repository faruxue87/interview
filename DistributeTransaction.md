# 分布式事务
* JTA事务(金融领域特殊方案，强一致性)
* 容器级事务：weblogic，jboss等基于JTA
* TCC柔性事务：try catch cancle
* MQ事务 消息，确认

# 银行交易数据一致性方案(强一致性，操作完毕之后客户端可以立即)
* 应用程序
* 事务管理器(协调者)
* 资源管理器(DB)
* 通信资源管理器(消息)

# Atomikos JTA实现方案
Spring+Tomcat+Atomikos
* Datasource:com.alibaba.druid.pool.xa.DruidXADatasource 多个
* TransactionManager:Atomikos 一个

# 2PC
phase1 准备阶段 写undo/redo日志，加锁
phase2 提交/回滚

# 3pc
