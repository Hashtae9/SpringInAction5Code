----



### **๐จโ๐จโ๐ฆโ๐ฆ SpringInAction Group Study [์ด์ฑ๋ฏผ, ๊ถํ๊ตฌ, ์ต์ฌํธ, ๊ณฝํ๊ธฐ, ๊น์์ฒ ]**

**group - GIT : https://github.com/euncheol-kim/SpringInActionGroupStudy**

<br>

#### **์ด์ฑ๋ฏผ ๋ : https://github.com/CokeLee777**

#### **๊ถํ๊ตฌ ๋ : https://github.com/Hashtae9**

#### **์ต์ฌํธ ๋ : https://github.com/jaero0725**

#### **๊ณฝํ๊ธฐ ๋ : https://github.com/nicebyy**

#### **๊น์์ฒ  (๋ณธ์ธ): https://github.com/euncheol-kim**

<br>

#### ์์ฑ์ : ๊น์์ฒ  

<br>

-----

<br>

<br>

### goal

> jdbc ํ๋ ์์ํฌ์ธ mybatis์ Database(mySQL)์ ์ด์ฉํด ๊ธฐ๋ณธ์ ์ธ CRUD๋ฅผ ์ดํดํ๋ค.
>
> - mybatis ํ๋ก์ ํธ ์ค๋ช
> - mybatis_2 ํ๋ก์ ํธ ์ค๋ช
> - mybatis์ด๋ก 

<br>

<br> **Euncheol-Kim ์๊ฒฉ๋ธ๋์น ์ค์ต ํ๋ก์ ํธ ํ์ผ ๊ตฌ์ฑ**

| ํ๋ก์ ํธ๋ช | ์ค๋ช                                                         |
| ---------- | ------------------------------------------------------------ |
| mybatis    | class ๋ด๋ถ์์์ ๋ฐ์ดํฐ ์์ฑ/์ญ์ /์์ /์ ๊ฑฐ์ RestAPI <u>(JDBC ํ๋ก๊ทธ๋๋ฐ์ ์๋)</u> |
| mybatis_2  | ๋ฐ์ดํฐ๋ฒ ์ด์ค ์ฐ๊ฒฐ ๋ฐ mabatis๋ฅผ ์ด์ฉํ RestAPI CRUD           |

<br>

<br>

#  1. ํ์ผ๋ช : mybatis

## [1] ๊ฐ์

> <u>Database๋ฅผ ์ฐ๊ฒฐํ์ง ์๊ณ </u> **class ๋ด๋ถ์์์ ๋ฐ์ดํฐ ์์ฑ / ์ญ์  / ์์  / ์ ๊ฑฐ์ RestAPI๋ฅผ ์์ฑํ๋ค.**

<br>

## [2] ํ๋ก์ ํธ ์์ฑ์ ์ข์์ฑ ์ค์ 

- Spring Web
- Spring Data jdbc
- lombok
- Spring Boot DevTools
- MySQL Driver

<br>

## [3] ์ค์ ๊ตฌ์ฑ

| ํจํค์ง                  | ํ์ผ๋ช                     | ์ค๋ช                                |
| ----------------------- | -------------------------- | ----------------------------------- |
| src/main/java/resources | **application.properties** | mysql๋๋ผ์ด๋ฒ ์ค์  ๋ฐ mysql ์ฐ๊ฒฐ    |
|                         | **pom.xml**                | jdbc์์กด์ฑ์ค์ <br />mySQL์์กด์ฑ์ค์  |

<br>

- **pom.xml**

```xml
 <!-- mySQL JDBC ์ข์์ฑ -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jdbc</artifactId>
</dependency>

<!-- mySQL์ข์์ฑ -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <scope>runtime</scope>
</dependency>
```

<br>

- **application.properties**

```properties

# Load the mysql driver (Database ์ข๋ฅ๋ง๋ค ์ฐ์ธก์ ๋ค์ด๊ฐ๋ ๋ด์ฉ์ด ์์ด)
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

# Connect the mysql database (Database ์ข๋ฅ๋ง๋ค ์ฐ์ธก์ ๋ค์ด๊ฐ๋ ๋ด์ฉ์ด ์์ด)
spring.datasource.url=jdbc:mysql://localhost:3306/user?serverTimezone=UTC&characterEncoding=UTF-8

# mysql username
spring.datasource.username=root 

# mysql password
spring.datasource.password=1234

```

<br>

<br> 

#### * ์ค์ ์ํ๋ฉฐ ํด๊ฒฐ์ด ๋์ง ์์๋ ๊ถ๊ธ์ฆ

mysql์ ์ฌ์ฉํ์ง ์๋ ์ค์ต์ด๋ผ `pom.xml`์ <u>mysql dependency ์ญ์ </u> `application.properties`์ jdbc๋ฅผ ์ ์ธํ ๋ชจ๋  ๋ด์ฉ์ ์ญ์ ํ๊ณ  ์คํํ๋๋ `error`๊ฐ ๋์์ต๋๋ค. 
<br><br>

์ ๊ฐ ์๊ฐํ๊ธฐ์ mysql๊ณผ ๊ด๋ จ๋ ๋ด์ฉ์ ์์กด์ฑ์ ์ง์์ฃผ๊ณ  ์๋น์ค๋ฅผ ์คํํ์ ๋, ๋ฌธ์  ์์ด ๋์ํด์ผํ๋ ๊ฒ ์๋๊ฐ์?



## [4] mybatis์ ํ์ผ ๊ตฌ์ฑ

| ํจํค์ง                                       | ํ์ผ                | ์ค๋ช                             |
| -------------------------------------------- | ------------------- | -------------------------------- |
| src/main/java/**<u>com.example.mybatis</u>** | UserController.java | RestFull CRUD์ ๊ธฐ๋ฅ์ ๋ด์ ํ์ผ |
| src/main/java/**<u>mybatis.model</u>**       | User                | Model                            |

<br>

- User.java

```java
package mybatis.model;

import lombok.Data;
import lombok.RequiredArgsConstructor;

@Data // lombok annotation : ๋ณ์์ ๋ํ getter & setter์์ฑ
public class User {
    private String id;
    private String name;
    private String phone;
    private String address;

    public User(String id, String name, String phone, String address) {
        super();
        this.id = id;
        this.name = name;
        this.phone = phone;
        this.address = address;
    }
}
```

<br>

- UserController.java

```java
package com.example.mybatis;

import mybatis.model.User;
import org.springframework.web.bind.annotation.*;

import javax.annotation.PostConstruct;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;


// RestAPI ์์ฑ - class๋ด๋ถ์์ ๋ฐ์ดํฐ ์์ฑ/์ญ์ /์์ /์ ๊ฑฐ

@RestController
public class UserController {
    private Map<String, User> userMap;

    @PostConstruct // ์ด๊ธฐํ ์งํ
    public void init() {
        userMap = new HashMap<String, User>();
        userMap.put("1", new User("1", "ํ๊ธธ๋", "111-1111", "์์ธ์ ๊ฐ๋จ๊ตฌ ๋์น1๋"));
        userMap.put("2", new User("2", "ํ๊ธธ์", "111-1112", "์์ธ์ ๊ฐ๋จ๊ตฌ ๋์น2๋"));
        userMap.put("3", new User("3", "ํ๊ธธ์", "111-1113", "์์ธ์ ๊ฐ๋จ๊ตฌ ๋์น3๋"));
    }

    @GetMapping("/user/{id}") // id๊ฐ์ ํตํ ๋ฐ์ดํฐ ๋ฐํ
    public User getUser(@PathVariable("id") String id) {
        return userMap.get(id);
    }

    @GetMapping("/user/all") // ๋ชจ๋  ๋ฐ์ดํฐ ๋ฐํ
    public List<User> getUserList() {
        return new ArrayList<User>(userMap.values()); // https://docs.oracle.com/javase/8/docs/api/java/util/Map.html -> Map์ ๋ฐ์ดํฐ๋ค์ ArrayList๋ก ๋ฐ๊พผ๋ค.
    }

    @PutMapping("/user/{id}") //๋ฐ์ดํฐ ์์ฑ
    public void putUser(@PathVariable("id") String id, @RequestParam("name") String name, @RequestParam("phone") String phone, @RequestParam("address") String address){
        User user = new User(id, name, phone, address);
        userMap.put(id, user);
    }

    @PostMapping("/user/{id}") //๋ฐ์ดํฐ ์์ 
    public void postUser(@PathVariable("id") String id, @RequestParam("name") String name, @RequestParam("phone") String phone, @RequestParam("address") String address){
        User user = userMap.get(id);
        user.setName(name);
        user.setPhone(phone);
        user.setAddress(address);
    }

    @DeleteMapping("/user/{id}") //๋ฐ์ดํฐ ์ญ์ 
    public void deleteUser(@PathVariable("id") String id){
        userMap.remove(id);
    }

}

```

<br>

- **@PathVariable** 

> (ex) -> @PathVariable "id" String id
>
> ์ฌ์ฉ์ ์๋ ฅ("id"์ ํด๋น)์ id๋งค๊ฐ๋ณ์๋ก ๋ฐ์์ ๋ด๋ถ์ ์ผ๋ก ์ฒ๋ฆฌ

<br>

- **@RequestParam**

> (ex) -> @RequestParam("address") String address
>
> ๊ฐ์ฒด์ ๋ฐ์ดํฐ๋ฅผ ๋ฐ์์ค๊ธฐ ์ํด ์ฌ์ฉํ๋ค.

<br>

<br>

#  2. ํ์ผ๋ช : mybatis_2 

## [1] ๊ฐ์

> <u>mysql์ spring๊ณผ ์ฐ๊ฒฐ</u>ํ์ฌ **mybatis framework๋ฅผ ํตํด CRUD๋ฅผ ์งํํ๋ค.**

- mybatis frameowrk = ORM framework (๊ฐ์ฒด์ ๊ด๊ณํ ๋ฐ์ดํฐ๋ฅผ ๋งคํํ๊ธฐ ์ํ ๊ธฐ์ )
- mabatis๋ ์๋ฐ ์ค๋ธ์ ํธ์ SQL์ฌ์ด์ ์๋ ๋งคํ ๊ธฐ๋ฅ์ ์ง์ํ๋ค.

<br>

## [2] ํ๋ก์ ํธ ์์ฑ์ ์ข์์ฑ ์ค์ 

- Spring Web
- Spring Data jdbc
- lombok
- Spring Boot DevTools
- MySQL Driver

<br>

## [3] ์ค์ ๊ตฌ์ฑ

| ํจํค์ง                  | ํ์ผ๋ช                     | ์ค๋ช                                |
| ----------------------- | -------------------------- | ----------------------------------- |
| src/main/java/resources | **application.properties** | mysql๋๋ผ์ด๋ฒ ์ค์  ๋ฐ mysql ์ฐ๊ฒฐ    |
|                         | **pom.xml**                | jdbc์์กด์ฑ์ค์ <br />mySQL์์กด์ฑ์ค์  |

<br>

- **pom.xml**

```xml
 <!-- mySQL JDBC ์ข์์ฑ -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jdbc</artifactId>
</dependency>

<!-- mySQL์ข์์ฑ -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <scope>runtime</scope>
</dependency>
```

<br>

- **application.properties** <u>\<enviroment></u>

```properties
# Load the mysql driver (Database ์ข๋ฅ๋ง๋ค ์ฐ์ธก์ ๋ค์ด๊ฐ๋ ๋ด์ฉ์ด ์์ด)
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

# Connect the mysql database (Database ์ข๋ฅ๋ง๋ค ์ฐ์ธก์ ๋ค์ด๊ฐ๋ ๋ด์ฉ์ด ์์ด)
spring.datasource.url=jdbc:mysql://localhost:3306/user?serverTimezone=UTC&characterEncoding=UTF-8

# mysql username
spring.datasource.username=root 

# mysql password
spring.datasource.password=1234

```

<br>

<br> <br>

## [4] mybatis_2์ ํ์ผ ๊ตฌ์ฑ

| ํจํค์ง                                       | ํ์ผ                | ์ค๋ช                                        |
| -------------------------------------------- | ------------------- | ------------------------------------------- |
| src/main/java/**<u>com.example.mybatis</u>** | UserController.java | RestFull CRUD์ ๊ธฐ๋ฅ์ ๋ด์ ํ์ผ            |
| src/main/java/**<u>mybatis.model</u>**       | User                | Model                                       |
| src/main/java/**<u>mapper</u>**              | UserMapper          | mybatis framework๋ฅผ ์ด์ฉํด ๊ตฌ์ฑํ interface |

<br>

<br>

- User.java <u>\<typeAlias ์ค์ ></u>

```java
package mybatis.model;

import lombok.Data;

@Data // lombok annotation : ๋ณ์์ ๋ํ getter & setter์์ฑ
public class User {
    private String id;
    private String name;
    private String phone;
    private String address;

    public User(String id, String name, String phone, String address) {
        super();
        this.id = id;
        this.name = name;
        this.phone = phone;
        this.address = address;
    }
}

```

<br>

- UserMapper.java <u><mapper๋ฑ๋ก></u>

```java
package com.example.mybatis_2.mapper;

import mybatis.model.User;
import org.apache.ibatis.annotations.*;

import java.util.List;

@Mapper //Mapper ์ธํฐํ์ด์ค
public interface UserMapper {

    @Select("SELECT * FROM User WHERE id=#{id}")
    User getUser(@Param("id") String id); //์ ๋ฌ๋ id๋ฅผ ๊ฐ์ง๊ณ  database์์ ์กฐํ๋ฅผํด์ User ๊ฐ์ฒด๋ก ๋ฐํํ๋ api
    // @Param์ด๋ธํ์ด์ ๋๋ถ์ ๋งค๊ฐ๋ณ์ id๊ฐ #{id}์ ๋งคํ์ด ๋๋ค.

    @Select("SELECT * FROM User")
    List<User> getUserList();

    @Insert("INSERT INTO User VALUES(#{id}, #{name}, #{phone}, #{address})")
    int insertUser(@Param("id") String id, @Param("name") String name, @Param("phone") String phone, @Param("address") String address);

    @Update("UPDATE User SET name=#{name}, phone=#{phone}, address=#{address} WHERE id=#{id}")
    int updateUser(@Param("id") String id, @Param("name") String name, @Param("phone") String phone, @Param("address") String address);

    @Delete("DELETE FROM User WHERE id=#{id}")
    int deleteUser(@Param("id") String id);
}

```

- @Mapper

> ๊ด๊ณํ ๋ฐ์ดํฐ๋ฒ ์ด์ค(RDBMS)๋ฅผ ์๋ฐ์ ๊ฐ์ฒด ์งํฅ ๋ชจ๋ธ๋ก ๋งคํํ๊ฒ ๋์์ฃผ๋ ์ธํฐํ์ด์ค๋ค. 

<br>

- #{....}

> ์ฌ์ฉ์์ ์๋ ฅ๊ฐ์ ๊ฐ์ฒด์ ๋ฐ์ดํฐ๋ฒ ์ด์ค๋ฅผ ๋งคํ์ํค๊ธฐ ์ํด์ ์ฌ์ฉํ๋ค.

<br>

- @Param

> (ex) ->  @Select("SELECT * FROM User WHERE id=#{id}")
>                User getUser(@Param("id") String id);
>
> 
>
> @Param์ด๋ธํ์ด์ ๋๋ถ์ ๋งค๊ฐ๋ณ์ id๊ฐ #{id}์ ๋งคํ์ด ๋๋ค.

<br>

- UserController.java

```java
package com.example.mybatis_2;

import com.example.mybatis_2.mapper.UserMapper;
import mybatis.model.User;
import org.springframework.web.bind.annotation.*;
import java.util.List;

@RestController
public class UserController {
    private UserMapper mapper;

    public UserController(UserMapper mapper) {
        this.mapper = mapper;
    }

    @GetMapping("/user/{id}")
    public User getUser(@PathVariable("id") String id) {
        return mapper.getUser(id);
    }

    @GetMapping("/user/all")
    public List<User> getUserList() {
        return mapper.getUserList();
    }

    @PutMapping("/user/{id}") //๋ฐ์ดํฐ ์์ฑ
    public void putUser(@PathVariable("id") String id, @RequestParam("name") String name, @RequestParam("phone") String phone, @RequestParam("address") String address){
        mapper.insertUser(id, name, phone, address);
    }

    @PostMapping("/user/{id}") //๋ฐ์ดํฐ ์์ 
    public void postUser(@PathVariable("id") String id, @RequestParam("name") String name, @RequestParam("phone") String phone, @RequestParam("address") String address){
        mapper.updateUser(id, name, phone, address);
    }

    @DeleteMapping("/user/{id}") //๋ฐ์ดํฐ ์ญ์ 
    public void deleteUser(@PathVariable("id") String id){
        mapper.deleteUser(id);
    }

}

```

- @PathVariable

> mabatis ํ๋ก์ ํธ์ ์ค๋ช



<br><br><br>

# 3. mybatis3 framework ์ด๋ก 

## [1] mybatis3 ๋?

๋ฐ์ดํฐ๋ฒ ์ด์ค ํ๋ก๊ทธ๋๋ฐ์ ์ข ๋ ์ฝ๊ฒ ํ  ์ ์๊ฒ ๋์์ฃผ๋ ํ๋ ์์ํฌ

- ๊ธฐ์กด JDBC(์๋ฐ ํ๋ก๊ทธ๋จ์ด ๋ฐ์ดํฐ๋ฒ ์ด์ค์ ์ฐ๊ฒฐ๋์ด ๋ฐ์ดํฐ๋ฅผ ์ฃผ๊ณ  ๋ฐ์ ์ ์๊ฒ ํด์ฃผ๋ ํ๋ก๊ทธ๋๋ฐ ์ธํฐํ์ด์ค)๋ฅผ ์กฐ๊ธ ๋ ์ฝ๊ณ  ์ ์ฐํ๊ณ  ํธํ๊ฒ ์ฌ์ฉํ๊ธฐ ์ํด ๊ฐ๋ฐ๋์๋ค.



## [2] mybatis3 ์ ์ฅ์ 

- ํ๋ก๊ทธ๋จ ์ฝ๋์ <u>SQL์ฟผ๋ฆฌ์ ๋ถ๋ฆฌ</u>๋ก ์ฝ๋์ ๊ฐ๊ฒฐ์ฑ ๋ฐ ์ ์ง๋ณด์์ฑ ํฅ์
- ๋น ๋ฅธ ๊ฐ๋ฐ์ด ๊ฐ๋ฅํ๋ฉฐ ์์ฐ์ฑ์ด ํฅ์๋๋ค.



## [3] mybatis3 ๊ตฌ์กฐ

![image](https://user-images.githubusercontent.com/72078208/169668693-bfdace9a-ac87-4330-98e6-7093fdaafad4.png)

#### mybatis3์ ๊ตฌ์ฑ

![image](https://user-images.githubusercontent.com/72078208/169668855-b5e1933c-dcb4-4ea7-ae7f-9da9172a17e4.png)

<br>

<br>

**<u>mybatis3์ ๊ตฌ์ฑ</u>**์ ๊ดํ ๋ง๋ง ์ด๋ ค์ด๊ฒ ๊ฐ์ต๋๋ค.
 [typeAlias](./1), [enviroment](./2), [mapper](./3) ์ค๋ช์ ํด๋น๋๋ ์ฝ๋๋ค์ด๋ ํ์ธํ์๋ฉด ์ข์ ๊ฒ ๊ฐ์ต๋๋ค.
