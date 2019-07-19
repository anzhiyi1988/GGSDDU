数据库建表

create table txttest(

n_id  integer,

txt  text

);

 

create table imgtest(

n_id  integer,

img  bytea

);

 

Hibernate映射

<class name=*"Txttest"* table=*"txttest"* schema=*"public"*>

<id name=*"nid"* column=*"n_id"* type=*"int"* />

<property name=*"txt"* column=*"txt"* type=*"string"* />

</class>

 

 

<class name=*"Imgtest"* table=*"imgtest"* schema=*"public"*>

<id name=*"nid"* column=*"n_id"* type=*"int"* />

<property name=*"img"* column=*"img"* type=*"byte[]"* />

</class>

 



```java
 public void saveTxt() throws IOException {
 
        File f = new File(
                "D:/workspace/study/hibernatePOSTGRE/src/com/thunisoft/HibernateSessionFactory.java");
        BufferedReader br = new BufferedReader(new FileReader(f));
        String temp = null;
        StringBuffer sb = new StringBuffer();
        temp = br.readLine();
        while (temp != null) {
            sb.append(temp + " ");
            temp = br.readLine();
        }
 
        Txttest a = new Txttest();
        a.setNid(1);
        a.setTxt(sb.toString());
 
        Session session = HibernateSessionFactory.getSession();
        session.beginTransaction();
        session.save(a);
        session.getTransaction().commit();
        HibernateSessionFactory.closeSession();
 
    }

public void saveImg() throws IOException {
 
        FileImageInputStream fs = new FileImageInputStream(new File(
                "D:/picture/1.jpg"));
        byte[] byteArr = new byte[(int) fs.length()];
 
        fs.read(byteArr);
 
        Imgtest a = new Imgtest();
        a.setNid(1);
        a.setImg(byteArr);
 
        Session session = HibernateSessionFactory.getSession();
        session.beginTransaction();
        session.save(a);
        session.getTransaction().commit();
        HibernateSessionFactory.closeSession();
 
    }

```

