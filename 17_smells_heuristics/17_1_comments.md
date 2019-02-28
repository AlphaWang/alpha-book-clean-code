## 17.1 注释（Comments）

### C1: 不恰当的信息（Inappropriate Information）
注释只应该描述有关代码和设计的技术性信息。

反例：修改历史记录

### C2: 废弃的注释（Obsolete Comment）
过时、无关、不正确的注释就是废弃的注释。

问题：造成误导

### C3: 冗余注释（Redundant Comment）
注释应该谈及代码自身 ~~没有~~（不能）提到的东西。
Comments should say things that the code cannot say for itself.

反例：
```
i++; // increment i
```

```
/**
* @param sellRequest
* @return
* @throws ManagedComponentException */
public SellResponse beginSellItem(SellRequest sellRequest) throws ManagedComponentException
```

### C4: 糟糕的注释（Poorly Written Comment）
如果注释值得写，那就值得好好写。
- 注意语法、拼写；
- 不画蛇添足、保持简洁。

### C5: 注释掉的代码（Commented-Out Code）
看到注释掉的代码，就删掉它！