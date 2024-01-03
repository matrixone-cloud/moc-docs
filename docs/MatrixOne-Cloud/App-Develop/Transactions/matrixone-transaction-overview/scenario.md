# 应用场景

## 实际场景中的应用

在一个财务系统中，不同用户之间的转账是非常常见的场景，而转账在数据库中的实际操作，通常是两个步骤，首先是对一个用户的账面金额抵扣之后，然后是对另一个用户的账面金额进行增加。只有利用事务的原子性，才能确保总账面资金没有变化，同时两个用户之间的账户都完成了各自的抵扣与增加，例如 A 用户此时对 B 用户转账 50：

```
start transaction;
update accounts set balance=balance-50 where name='A';
update accounts set balance=balance+50 where name='B';
commit;
```

当两次 `update` 都成功并且最终提交，整个转账才算真正完成，任何一步的失败，必须让整个事务回滚，才能确保原子性。

此外，两个账户转账的过程中，在没有提交之前，无论是 A 或者 B，看到的都是尚未转账完成的账面余额，这是事务的隔离性。

在转账过程中，数据库会检查 A 的账面资金是否大于 50，A 和 B 在系统中是否确有其人，只有都满足这些约束，才能保证事务的一致性。

完成转账后，无论系统是否重启，数据已经完成了持久化，体现了事务的持久性。

## MatrixOne Cloud 的悲观事务与读已提交

MatrixOne Cloud 默认**悲观事务**与**读已提交**隔离级别，这种组合方式使性能达到最优。

悲观事务 (Pessimistic Transaction) 是指在事务期间，持有资源的过程中，将资源锁定，以避免其他并发事务对该资源的修改或读取。悲观事务假定并发事务可能会对资源进行操作，并防止这种情况发生。

在 MatrixOne Cloud 中，可以使用 `UPDATE ... WHERE...` 语句来实现悲观锁定，该语句会锁定符合条件的记录，直到事务提交或回滚。例如，下面的 SQL 语句锁定了 `user` 表中 `id=1` 的记录：

```sql
START TRANSACTION;
UPDATE user WHERE id=1;
-- 在事务期间执行其他操作，例如修改该记录
COMMIT;
```

读已提交 (Read Committed) 是一种隔离级别，它确保在事务提交之前，其他事务所做的修改不会对当前事务可见。在读已提交隔离级别下，事务只能看到已经提交的数据，无法看到未提交的数据。因此，在该隔离级别下，可以避免脏读 (Dirty Read)。

在 SQL 中，可以使用 `SET TRANSACTION ISOLATION LEVEL READ COMMITTED;` 语句设置隔离级别为读已提交。例如，下面的 SQL 语句查询了 user 表中 id=1 的记录，但如果其他事务正在修改该记录，则当前事务无法看到未提交的修改：

```sql
BEGIN TRANSACTION;
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;
-- 如果其他事务正在修改 user 表中 id=1 的记录，下面的语句将会等待，直到锁定被释放
SELECT * FROM user WHERE id = 1;
COMMIT;
```

需要注意的是，使用悲观事务和读已提交隔离级别可以避免一些并发问题，但也可能会带来一些额外的开销，因此需要权衡利弊并根据实际需求来选择使用。
