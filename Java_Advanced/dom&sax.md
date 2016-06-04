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
1.TransformerFactory fac = TransformerFactory.newInstacne();
2.Transformer tf = fac.newTransformer();
3.transform(Source xmlSource, Result outputTaget);
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
  
  /**
  删除第三个联系人的信息
  1.获取Document对象
  2.获取根元素
  3.获取到第三个联系人元素
  4.让根元素删掉第三个联系人
  5.同步数据
  */
  public void testDelete() throws Exception {
    // 1.获取Document对象
    Document doc = DocumentBuilderFactory().newInstance().newDocumentBuilder().parse(file);
    // 2.获取根元素
    Element root = doc.getDocumentElement();
    // 3.获取到第三个联系人元素
    Node linkman = root.getElementByTagName("linkman").item(2);
    // 4.让根元素删除第三个联系人
    root.removeChild(linkman);
    // 5.数据同步
    Transformer tf = TransformerFactory.newInstance().newTransformer();
    tf.transform(new DOMSource(doc), new StreamResult(file));
  }
}
```

##`SAX`解析

* 特点

> `SAX`逐行扫描文档，一边扫描一边解析；相比于`DOM`解析，`SAX`可以在解析文档的任意时刻停止；但是`SAX`解析的操作比较复杂

* `SAX`解析产生以下类型的事件

>1.在文档的开始和结束时触发文档处理事件。
>
>2.在文档内每一个`XML`元素接受解析的前后触发元素事件。
>
>3.任何元数据通常由单独的事件处理
>
>4.在处理文档的`DTD`或者`Schema`时产生`DTD`或`Schema`事件。
>
>5.产生错误事件用来通知主机应用程序解析错误。

* 补充

>`Android`中常用的`XML`解析`PULL`解析器的运行方式与`SAX`类似，不同的是，在`pull`解析过程中，我们需要自己获取产生的事件然后做响应的操作

* `SAX`解析`XML`的操作步骤：

> 两个核心类`SAXParser`和`DefaultHandler` 

1.创建`SAXParser`对象

```java
SAXParserFacotry factory = SAXParserFactory.newInstance();
SAXParser parser = factory.newSAXParser();
```

2.通过SAXParser中的parser方法解析一个指定的XML文件

```java
void parse(File file, DefaultHandle handle);
attributes.parse(new File("student.xml"), new SAXHandler);
// 这里的SAXHandler是我们自定义的一个XML解析器
```

3.创建一个SAXHandler类，并继承自DefaultHandler

* DefaultHandler中常用的方法

```java
// 接受文档开始的通知
void startDocument(); 
// 接受元素开始的通知
void startElement(String uri, String localName, String qName, Attributes attributes);
// 接受元素中字符数据的通知
void characters(char[] ch, int start, int length);
// 接受元素结束的通知
void endElement(String uri, String localName, String qName);
// 接受文档结束的通知
void endDocument();
```

* 示例代码

```java
public class extends DefaultHandler {
  // 存放联系人的集合
  private List<Contact> list;
  // 联系人对象
  private Contact con;
  // 记录上一个标签名
  private Sring preTag;
  
  public List<Contact> getList() {
    return list;
  }
  
  // 当解析到文档开始的时候调用该方法
  public void startDocument() throws Exception {
    list = new ArrayList<>();
  }
  
  // 当解析到文档结束的时候调用该方法
  public void endDocument() throws Exception {
  }
  // 当解析到文档结束的时候调用到该方法
  public void endDocument() throws Exception {
  }
  // 当解析到文本内容的时候调用该方法
  public void characters(char[] ch, int start, int lenth) throws Exception {
    if ("name".equals(preTag)) {
      String name = new String(ch, start, length);
      con.setName(name);
    } else if ("email".equals(preTag)) {
      String email = new String(ch, start, length);
      con.setEmail(email);
    } else if ("group".equals(preTag)) {
      String group = new String(ch, start, length);
      con.setGroup(group);
      // 一个联系人的信息解析完成，将其放入到集合中
      list.add(con);
    }
  }
}
```