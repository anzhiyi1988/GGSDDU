第一种方式

<plugin>

<groupId>org.apache.maven.plugins</groupId>

<artifactId>maven-jar-plugin</artifactId>

<version>2.4</version>

<configuration>

<archive>

<manifest>

<addClasspath>true</addClasspath>

<classpathPrefix>lib/</classpathPrefix>

<mainClass>com.sysware.HelloWorld</mainClass>

</manifest>

</archive>

</configuration>

</plugin>  

 

mvn clean package

 

第二种方式

<plugin>  

<groupId>org.apache.maven.plugins</groupId>

<artifactId>maven-assembly-plugin</artifactId>

<version>2.3</version>

<configuration>

<appendAssemblyId>false</appendAssemblyId>

<descriptorRefs>

<descriptorRef>jar-with-dependencies</descriptorRef>

</descriptorRefs>

<archive>

<manifest>

<mainClass>com.juvenxu.mvnbook.helloworld.HelloWorld</mainClass>

</manifest>

</archive>

</configuration>

</plugin>  

 

mvn assembly:assembly