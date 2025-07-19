**package qimo;**
**/***
    **图书表  书号、书名、价格、借阅量（0-100）**
    **定义数组保存借阅量数量，以数据量10为例，要求数据通过键盘输入，**
    **对输出可能存在的异常进行处理**
    **最后输出最高借阅量和最低借阅量**
 ***/**
**import com.sun.deploy.net.proxy.SunAutoProxyHandler;**
**import sun.util.resources.cldr.xh.CurrencyNames_xh;**

**import java.util.*;**

**public class Demo01 {**
    **public static void main(String[] args) {**
       **/* int[] arr = new int[10];**
        **int max,min;**
        **Scanner scanner = new Scanner(System.in);**
        **for (int i = 0; i < arr.length; i++) {**
            **try {**
                **arr[i] = scanner.nextInt();**
                **if(arr[i]<0){**
                    **throw new Exception();**
                **}**
            **}catch (Exception e){**
                **System.out.println("请输入合法数据：");**
            **}**
        **}**
        **max = min = arr[0];**
        **for (int i = 1; i <arr.length ; i++) {**
            **if(arr[i]>max)**
                **max = arr[i];**
            **if(arr[i]<min)**
                **min = arr[i];**
        **}**
        **System.out.println("最高借阅量"+max+",最低借阅量"+min);**
        **scanner.close();**
***/**

​        **Scanner scanner = new Scanner(System.*in*);**
​        **Book[]  books = new Book[3];**
​        **for (int i = 0; i < books.length; i++) {**
​            **int id;**
​            **String name;**
​            **float price;**
​            **int len;**
​            **String type;**
​            **try {**
​                **System.*out*.println("please input type:");**
​                **if(scanner.nextLine().equals("历史书")) {**
​                    **id = scanner.nextInt();**
​                    **scanner.nextLine();**
​                    **name = scanner.nextLine();**
​                    **price = scanner.nextFloat();**
​                    **scanner.nextLine();**
​                    **len = scanner.nextInt();**
​                    **scanner.nextLine();**
​                    **type = scanner.nextLine();**
​                    **String history = scanner.nextLine();**
​                    **books[i] = new HistoryBook(id, name, price, len,type,history);**
​                **}else if(scanner.nextLine().equals("专业书")){**
​                    **id = scanner.nextInt();**
​                    **scanner.nextLine();**
​                    **name = scanner.nextLine();**
​                    **price = scanner.nextFloat();**
​                    **scanner.nextLine();**
​                    **len = scanner.nextInt();**
​                    **scanner.nextLine();**
​                    **type = scanner.nextLine();**
​                    **String major = scanner.nextLine();**
​                    **books[i] = new MajorBook(id, name, price, len,type,major);**
​                **}**
​            **}catch (Exception e){**
​                **System.*out*.println("请输入合法数据：");**
​            **}**
​        **}**


    **}**
    **public  static void method(Book[] books,String type){**
        **TreeSet<Book> treeSet = new TreeSet<>(new Comparator<Book>() {**
            **@Override**
            **public int compare(Book o1, Book o2) {**
                **return o2.id-o1.id;**
            **}**
        **});**
        **for(Book book:books){**
            **treeSet.add(book);**
        **}**
        **//System.out.println(treeSet);**
       **// System.out.println("please input type:");**

​        **Iterator<Book> iterator = treeSet.iterator();**
​        **while(iterator.hasNext()){**
​            **Book next = iterator.next();**
​            **if (next.type.equals(type)){**
​                **System.*out*.println(next.name+","+next.type**
​                **);**
​            **}**
​        **}**
​    **}**
**}**

**interface Iun{**
    **Book search(String type);**

**}**
**class Book implements Iun{**
    **int id;**
    **String name;**
    **float price;**
    **int len;**
    **String type ;**

​    **public Book() {**
​    **}**

​    **public Book(int id, String name, float price, int len,String type) {**
​        **this.id = id;**
​        **this.name = name;**
​        **this.price = price;**
​        **this.len = len;**
​        **this.type = type;**
​    **}**

​    **public void getLen(){**
​        **System.*out*.println(1);**
​    **}**


    **@Override**
    **public Book search(String type) {**
        **return null;**
    **}**
**}**

**class HistoryBook extends Book{**
    **//static final String type = "lishishu" ;**
    **String history;**
    **public HistoryBook(String type) {**
        **this.type = type;**
    **}**

​    **public HistoryBook(int id, String name, float price, int len, String type,String history) {**
​        **super(id, name, price, len,type);**
​        **this.history = history;**
​    **}**
​    **public HistoryBook() {**
​    **}**
​    **public void getLen(){**
​        **System.*out*.println(2);**
​    **}**
**}**
**class MajorBook extends Book{**
​    **String major ;**

​    **public MajorBook(String major) {**
​        **this.major = major;**
​    **}**

​    **public MajorBook(int id, String name, float price, int len, String type,String major) {**
​        **super(id, name, price, len,type);**
​        **this.major = major;**
​    **}**

​    **public MajorBook() {**
​    **}**
​    **public void getLen(){**
​        **System.*out*.println(3);**
​    **}**
**}**





**//-------------------------------**

**package qimo;**

**import java.sql.*;**

**public class Demo02 {**
    **public static <Strings> void main(String[] args) throws Exception {**
        **Statement stmt = null;**
        **ResultSet rs = null;**
        **Connection conn = null;**
        **try {**
            **//1.注册数据库的驱动**
            **Class.*forName*("com.mysql.cj.jdbc.Driver");**
            **String url = "jdbc:mysql://localhost:3306/jdbc?serverTimezone=GMT%2BG&useSSl=false";**
            **String username = "root";**
            **String password = "root";**
            **conn = DriverManager.*getConnection*(url, username, password);**
            **stmt = (Statement) conn.createStatement();**
            **String sql = "select*from users";
            rs = stmt.executeQuery(sql);
            System.*out*.println("id | name | passeord | email | birthday");
            while (rs.next()) {
                int id = rs.getInt("id");
                String name = rs.getString("name");
                Strings psw = (Strings) rs.getString("password");
                String email = rs.getString("email");
                Date birthday = rs.getDate("birthday");
                System.*out*.println(id + "   |    " + name + "   |   " + psw + "   |   " + email + "   |   " + birthday + "   |   ");**
            **}**
        **} catch (Exception e) {**
            **e.printStackTrace();**
        **} finally {**
            **if (rs != null) {**
                **try {**
                    **rs.close();**
                **} catch (SQLException e) {**
                    **e.printStackTrace();**
                **}**
                **rs = null;**
            **}**
            **if (stmt != null) {**
                **try {**
                    **stmt.close();**
                **} catch (SQLException e) {**
                    **e.printStackTrace();**
                **}**
                **stmt = null;**
            **}**
            **if (conn != null) {**
                **try {**
                    **conn.close();**
                **} catch (SQLException e) {**
                    **e.printStackTrace();**
                **}**
            **}**
            **conn = null;**
        **}**
    **}**
**}**



**//----------------------**

**package qimo;**

**import chap07.Student;**

**import java.util.Comparator;**
**import java.util.Set;**
**import java.util.TreeMap;**

**public class Demo03 {**
    **public static void main(String[] args) {**
        **TreeMap<Student,String> treeMap = new TreeMap<>(new Comparator<Student>() {**
            **@Override**
            **public int compare(Student o1, Student o2) {**

​                **return o1.age - o2.age;**
​            **}**
​        **});**
​        **TreeMap<Student,String> treeMap1 = new TreeMap<>(new Comparator<Student>() {**
​            **@Override**
​            **public int compare(Student o1, Student o2) {**

​                **return o2.age - o1.age;**
​            **}**
​        **});**
​        **treeMap.put(new Student("С1",18),"计科222");**
​        **treeMap.put(new Student("С2",17),"计科221");**
​        **treeMap.put(new Student("С3",19),"计科223");**
​        **System.*out*.println(treeMap);**

​        **Set<Student> students = treeMap.keySet();**
​        **for(Student student:students){**
​            **String values = treeMap.get(student);**
​            **if (values.equals("计科本223")) {**
​                **System.*out*.println(student + "=" + treeMap.get(student));**
​            **}**
​        **}**
​    **}**
​    **}**