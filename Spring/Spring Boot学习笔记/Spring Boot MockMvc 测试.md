## 简述：

之前就有看到大佬的 `blog` 中写过关于`Mockmvc`的使用方法，但是却没有系统的学一下，由于实在不想每次测试`http`请求都启动一下服务器，最终还是没忍住。

<center>
    <img style="border-radius: 0.5125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);"
    src="../../images/1546595420415.png">
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">enjoy coding！</div>
</center>

包含了四种请求方式的学习：

- GET
- POST
- PUT
- DELETE

## 环境准备（myself）：

- Jdk 8
- Springboot 2.0.5
- Lombok 1.18.2

## 添加依赖

```xml
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-web</artifactId>
</dependency>

<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-test</artifactId>
   <scope>test</scope>
</dependency>

<!-- lombok -->
<dependency>
   <groupId>org.projectlombok</groupId>
   <artifactId>lombok</artifactId>
   <version>1.18.2</version>
   <scope>provided</scope>
</dependency>
```

## 实体类

```java
/**
 * @author innerpeacez
 * @since 2019/1/7
 */
@Data
@AllArgsConstructor
public class Employee {

    private String firstName;
    private String lastName;
    private String email;
}
```

## Controller 控制器

定义好 CURD 的四种 http 请求方式。

RESTful Api 是这个样子的：

| method | url            | Description      |
| ------ | -------------- | ---------------- |
| GET    | /employee/{id} | 获取一个雇员     |
| GET    | /employee      | 获取所有雇员     |
| DELETE | /employee      | 删除一个雇员     |
| POST   | /employee      | 增加一个雇员     |
| PUT    | /employee      | 修改一个雇员信息 |



```java
/**
 * @author innerpeacez
 * @since 2019/1/8
 */
@RestController
@RequestMapping("/employee")
public class EmployeeController {

    /**
     * 保存雇员信息的容器
     */
    private static Map<Integer, Employee> employeeMap = new LinkedHashMap<>();

    /**
     * 静态方法初始化一些雇员信息，方便测试
     */
    static {
        Employee employee1 = new Employee("zhang", "san", "zhangsan@xx.com");
        Employee employee2 = new Employee("li", "si", "lisi@xx.com");
        employeeMap.put(1, employee1);
        employeeMap.put(2, employee2);
    }

    /**
     * 获取一个雇员
     * @param id
     * @return
     */
    @GetMapping("/{id}")
    public Employee getEmployee(@PathVariable Integer id) {
        return employeeMap.get(id);
    }

    /**
     * 获取所有雇员
     * @return
     */
    @GetMapping
    public Map<Integer, Employee> getEmployees() {
        return employeeMap;
    }

    /**
     * 增加一个雇员
     * @param employee
     * @return 增加的雇员信息
     */
    @PostMapping
    public Employee addEmployee(Employee employee) {
        return employeeMap.put(employeeMap.size() + 1, employee);
    }

    /**
     * 删除一个雇员
     * @param id
     * @return 删除的雇员信息
     */
    @DeleteMapping
    public Employee deleteEmployee(@RequestParam Integer id) {
        return employeeMap.remove(id);
    }

    /**
     * 修改一个雇员的信息
     * @param id
     * @param employee
     * @return 修改操作之前的雇员信息
     */
    @PutMapping
    public Employee updateEmployee(Integer id, Employee employee) {
        return employeeMap.replace(id, employee);
    }
}
```

## MockMvc测试类

`@RunWith`注解：`SpringRunner.class`使用 Junit 整合 Spring

`@WebMvcTest`注解：寻找测试的 Controller 中的 Mappling

`MockMvc`: 注入 MockMvc 模拟 Http 请求

### MockMvcRequestBuilders抽象类

类中分装了Http请求的Method，如：

- post()
- get()
- put()
- delete()
- ...

使用这些方法时可以设置请求头Header，入参方式，出参方式，验证返回条件，获取返回值等操作。

具体的操作通过下面三种方式实现：

- andExpect() // 匹配返回信息的预期状态
- andDo() // 处理返回值，有print(),log()等操作
- andReturn() // 获取返回值，MvcResult。

### 测试Http请求代码

```java
/**
 * @author innerpeacez
 * @since 2019/1/9
 */
@RunWith(SpringRunner.class)
@WebMvcTest(EmployeeController.class) // 需要测试的 Controller
public class EmployeeControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @Test
    public void testPost() throws Exception {
        MvcResult mvcResult = mockMvc.perform(post("/employee") //设置请求方式为 post 及 url
                .accept(MediaType.APPLICATION_JSON) // 设置入参方式为json
                .param("lastName", "wu")
                .param("email", "wangwu@xx.com")
                .contentType(MediaType.APPLICATION_JSON)) // 设置出参方式json
                .andExpect(status().isOk()) // 设置请求的状态码为 200，可以添加一些期望的响应的结果判断
                .andDo(print()) // print 方法打印出请求体，响应体
                .andReturn(); // 获取返回值MvcResult
        System.out.println(mvcResult.getResponse().getContentAsString());

        testGetAll();
    }

    @Test
    public void testGetOne() throws Exception {
        mockMvc.perform(get("/employee/1")
                .accept(MediaType.APPLICATION_JSON)
                .contentType(MediaType.APPLICATION_JSON))
                .andExpect(status().isOk())
                .andDo(print()) //
                .andReturn(); // 获取返回值
    }

    @Test
    public void testGetAll() throws Exception {
        MvcResult mvcResult = mockMvc.perform(get("/employee")
                .contentType(MediaType.APPLICATION_JSON))
                .andExpect(status().isOk())
                .andDo(print()).andReturn();
        System.out.println(mvcResult.getResponse().getContentAsString());
    }

    @Test
    public void testPut() throws Exception {
        MvcResult mvcResult = mockMvc.perform(put("/employee")
                .accept(MediaType.APPLICATION_JSON)
                .param("id", "1")
                .param("firstName", "张")
                .param("lastName", "三")
                .param("email", "zhangsan@xx.com")
                .contentType(MediaType.APPLICATION_JSON))
                .andExpect(status().isOk())
                .andDo(print())
                .andReturn();

        System.out.println("修改之前的值：" + mvcResult.getResponse().getContentAsString());
        testGetOne();
    }


    @Test
    public void testDelete() throws Exception {
        MvcResult mvcResult = mockMvc.perform(delete("/employee/1")
                .accept(MediaType.APPLICATION_JSON)
                .contentType(MediaType.APPLICATION_JSON))
                .andExpect(status().isOk())
                .andDo(print())
                .andReturn();

        System.out.println("删除之前的值：" + mvcResult.getResponse().getContentAsString());
        testGetOne();
    }
}
```

## 总结

本文简单的学习了一下 MockMvc 怎么使用，包含了主要的 CRUD 请求的方式，再也不用启动服务器来测试 http 请求啦。