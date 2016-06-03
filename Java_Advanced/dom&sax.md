# `DOM`&`SAX`
##`DOM`解析
* 特点

><u>***一次性***</u> 将整个`XML`文档加载到内存中，在内存中形成一个`DOM树`，那么操作（`CRUD`）`DOM树`实际是在操作内存中的对象（`Document`，`Element`，`Attr`，`Text`）要操作磁盘中的`XML`文件，那么还需要一个<u>***同步操作***</u>

* 缺点

>如果文件过大，那么可能造成内存溢出。

* 常用`API`

```java
// Node接口
// 获取当前元素的文本内容
String getTextContent();

// 为当前元素设置文本内容
void setTextContent(String text);

// 为当前的元素添加子元素
Node appendChild(Node newChild);
```

* 解析步骤

```java
// 1.确定XML文件的位置。
File f = new File("路径");
// 2.获取DocumentBuilderFactory对象
DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
// 3.通过工厂类获得DocumentBuilder对象
DocumentBuilder builder = dbf.newDocumentBuilder();
// 4.通过DocumentBuilder获取Document对象
```