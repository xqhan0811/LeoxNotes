# Salesforce



## Salesforce执行顺序小口诀

1. 页面后台两不同，
2. 布局规则最优先，
3. 格式长度和必填。
4. Before Trigger触发前，
5. 后台只将外键检。
6. 批量插入有例外，
7. 验证规则提前验。
8. Before之后做验证，
9. 自定规则和必填，
10. 系统规则不二遍。
11. 验证之后跑去重，
12. 存入DB不提交。
13. After Trigger触发后，
14. 分配/回复/工作流，
15. 如果字段有更新，
16. 验证/去重不再做，
17. Trigger仅再跑一次。
18. PB/Flow依次跑，
19. 数据操作从头走，
20. Case规则在随后。
21. 父表汇总此时算，
22. 工作流把父表更，
23. 共享规则重计算。
24. 数据DB提交后，
25. 后续邮件才发送



## Trigger

 ###  Trigger类型

Trigger大体可以分为两类：

1、Before Triggers，Before Triggers是在DML（insert、update、delete、merge、upsert、undelete）数据之前，也就是说数据还没有Commit到数据库，我们可以在这个时间点编写Apex，常用的场景有验证数据合法性、验证业务场景等。
<font color="red">**注意在Before Triggers中如果删除或者修改触发该触发器的当前记录，系统会抛异常。**</font>
2、After Triggers，After Trigger是在DML数据之后，指的是数据库数据已经发生变化后，执行相应的Apex，常用的场景有关联对象更新、调用异步方法等。



我们在Salesforce写的Trigger默认是批处理的，比如Dataloader批量导入数据也会触发对应的触发器，所以在写代码逻辑的时候，应当考虑处理多条数据的情况。