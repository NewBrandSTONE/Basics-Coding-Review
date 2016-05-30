# DOM&SAX
##DOM解析
特点
><u>***一次性***</u> 将整个XML文档加载到内存中，在内存中形成一个DOM树，那么操作（CRUD）DOM树实际是在操作内存中的对象（Document，Element，Attr，Text）要操作磁盘中的XML文件，那么还需要一个<u>***同步操作***</u>

缺点
>如果文件过大，那么可能造成内存溢出。

解析步骤
```java
// 1.确定XML文件的位置。
File f = new File("路径");
// 2.获取DocumentBuilderFactory对象
DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
// 3.通过工厂类获得DocumentBuilder对象
DocumentBuilder builder = dbf.newDocumentBuilder();
// 4.通过DocumentBuilder获取Document对象
```