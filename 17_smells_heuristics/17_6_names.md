## 17.6 名称（Names）

### N1: 采用描述性名称（Choose Descriptive Names）



### N2: 名称应与抽象层级相符（Choose Names at the Appropriate Level of Abstraction）

Don’t pick names that communicate implementation; choose names the reflect the level of abstraction of the class or function you are working in.

命名不要暴露实现细节，只用描述功能抽象。

### N3: 尽可能使用标准命名法（Use Standard Nomenclature Where Possible）

如果用装饰者模式，就以Decorator结尾。

### N4: 无歧义的命名（Unambiguous Names）

反例

```java
private String doRename() throws Exception {
  if(refactorReferences) 
    renameReferences();
  renamePage();
  
  pathToRename.removeNameFromEnd(); 
  pathToRename.addNameToEnd(newName); 
  return PathParser.render(pathToRename);
}
```

正例

```java
private String renamePageAndOptionallyAllReferences() throws Exception {
  ...
}
```



### N5: 为较大作用范围选用较长名称（Use Long Names for Long Scopes） 

如果变量作用范围很小，例如5行之内，那么取名为`i` `j` 是没问题的；但是如果作为范围较大，则应该使用较长的名称。

### N6: 避免编码（Avoid Encodings） 

不要在名称中包含类型或作用域信息。


### N7: 名称应该说明副作用（Names Should Describe Side-Effects）

反例

```java
public ObjectOutputStream getOos() throws IOException { 
  if (m_oos == null) {
    m_oos = new ObjectOutputStream(m_socket.getOutputStream()); 
  }
  
  return m_oos; 
}
```



正例

```java
public ObjectOutputStream createOrReturnOos() throws IOException { 
 ... 
}
```

