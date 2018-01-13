# Spring事务级别
---
## Propagation（传播属性）
属性名|作用
--|--
PROPAGATION_REQUIRED|如果要执行的事务外已经起了一个事务，那就不再起新的事务，否则要起新的事务。
PROPAGATION_SUPPORTS|如果在当前的事务中，以事务的形式执行，如果不在一个事务中，则以非事务的形式执行。
PROPAGATION_MANDATORY|强制的，如果在一个事务中，以事务的形式执行，如果没有事务，要抛出异常。
PROPAGATION_REQUIRES_NEW|如果已经存在一个事务中，会将当前事务挂起，新起一个事务。
PROPAGATION_NOT_SUPPORTED|不支持当前事务，如果当前有事务，则将事务挂起，以非事务的形式执行。
PROPAGATION_NEVER|不能在事务中执行，如果当前存在事务，会抛异常
PROPAGATION_NESTED|与PROPAGATION_REQUIRES_NEW相似，但是该类型是和父事务相依的，父事务回滚，它也要回滚。
