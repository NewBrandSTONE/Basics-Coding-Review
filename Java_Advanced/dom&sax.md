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
// 删除当前元素中指定的子元素(老爹干掉儿子)
Node removeChild(Node oldChild);

// Document接口
// 获取根元素
Element getDocumentElement();
// 创建指定名称新元素
Element createElement(String tagName);

// Element接口
// 根据指定的标签名称获取到指定的标签
NodeList getElementsByTagName(String name);
// 为当前元素设置属性值
void setAttribute(String name, String value);
// 获取指定标签的属性
String getAttribute(String tagName);
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

 * 示例代码

```java
// 需求：得到某个具体的文本节点的内容：去除第二个联系人的名字

```