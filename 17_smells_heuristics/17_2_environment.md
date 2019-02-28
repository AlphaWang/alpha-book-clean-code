## 17.2 环境（Environment）

### E1: 需要多步才能实现的构建（Build Requires More Than One Step）
You should be able to check out the system with one simple command and then issue one other simple command to build it.
```
svn get mySystem 
cd mySystem
ant all
```

### E2: 需要多步才能做到的测试（Tests Require More Than One Step）
You should be able to run all the unit tests with just one command. 