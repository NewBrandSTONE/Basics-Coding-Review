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
// 需求：取出第二个联系人的名字
public void testGetName() throws Exception {
  // 1.获取Document对象
  DocumentBuilderFactory dfb = DocumentBuilderFactory.newInstance();
  DucumentBuilder db = dfb.newDocumentBuilder();
  File file = new File("contacts.xml");
  Document doc = builder.parse(file);
  System.out.println(doc);
  
  // 2.获取根元素
  ELement root = doc.getDocumentElement();
  
  // 3.获取第二个LinkMan
  NodeList list = root.getElementsByTagName("linkman");
  Element linkman = (Element) list.item(1);
  
  // 4.获取第二个联系人的<name>元素
  NodeList names = linkman.getElementsByTagName("name");
  node name = names.item(0);
  System.out.println(name);
  
  // 5.获取<name>中的文本
  String nameText = name.getTextContent();
  System.out.prinln(nameText);
}

File file = new File("contacts.xml");


/**
6.数据同步
获取Transformer对象的步骤
TransformerFactory fac = TransformerFactory.newInstacne();
Transformer tf = fac.newTransformer();
transform(Source xmlSource, Result outputTaget);
xmlSource:指定要同步的数据的位置
outputTarget:指定同步到的磁盘中的位置
*/
// 修改第一个联系人的邮箱内容
public void testUpdate() throws Exception {
  // 1.获取Document对象
  Document doc = DocumentBuilderFactory.newInstance().newDocumentBuilder().parse(file);
  // 2.获取根元素
  Element root = doc.getDocumentElement();
  // 3.获取第一个linkman元素
  NodeList list = root.getElementsByTagName("linkman");
  Element list = (Element) list.getElementsByTagName("linkman");
  Element linkman = list.item(0);
  // 4. 获取第一个联系人的<email>元素
  Element email = (Element) linkman.getElementsByTagName("email").item(0);
  // 5.修改<email>标签中的文本
  email.setTextContent("asd@qq.com");
  // 6.数据同步
  TransformerFactory fac = TransformerFactory.newInstance();
  Transformer tf = fac.newTransformer();
  Source xmlSource = new DOMSource(doc); // 注意不要忘记添加参数Document doc
  Result outputTarget = new StreamResult(file); // 注意不要忘记参数
  tf.transform(xmlSource, outputTaget);
  
  /**
  步骤：
  1.获取Document对象
  2.获取根元素
  3.创建新的<linkman>元素
  4.创建新的<name><email><address><group>
  5.给上面的四个元素设置文本
  6.将四个子元素添加到新的<linkman>元素中
  7.将新的<linkman>放到根元素中
  8.数据同步
  */
  // 添加一个联系人信息
  public void testAdd() throws Exception {
    // 1.获取Document对象
    Document doc = DocumentBuilderFactory().newInstance().newDocumentBuilder().parse(file);
    
    // 2.获取根元素
    Element root = doc.getDocumentElement();
    
    // 3.创建新的<linkman>元素
    Element linkman = doc.createELement("linkman");
    
    // 4.创建新的<name><email><address><group>
    Element address = doc.createElement("address");
    
    // ....其余标签省略了
    // 5.给上面的四个元素设置文本
    address.setTextContent("James");
    
    // ...其余设置省略
    // 6.将四个子元素添加到新的<linkman>元素中
    linkman.appendChild(address);
    // ...其他添加省略
    
    // 7.将新的<linkman>放到根元素中
    root.appendChild(linkman);
    
    // 8.数据同步
    Transformer tf = TransformerFactory.newInstance().newTransformer();
    tf.transform(new DOMSource(doc), new StreamResult(file));
  }
}
```

