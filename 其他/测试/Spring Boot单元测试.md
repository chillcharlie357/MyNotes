
`spring-boot-starter-test`继承了`JUnit`等多个测试库。

# 测试类结构

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class UnitTest1 {
	@Test
    public void contextLoads(){
        
    }
}
```

- `@Runwith`：JUnit中的注解，用来通知JUnit单元测试框架不要使用内置的方式进行单元测试。
- `@SpringBootTest`：用于Spring Boot应用的测试，默认会分局报名逐级往上查找Spring Boot主程序，也就是`@SpringBootApplocation`注解，并在单元测试启动的时候启动该类来创建Spring上下文。

[Getting Started | Testing the Web Layer](https://spring.io/guides/gs/testing-web/)

# 测试MVC

[Spring Boot单元测试入门实战 - 掘金](https://juejin.cn/post/6992922963691929637)

```java
@SpringBootTest
@AutoConfigureMockMvc
public class TestingWebApplicationTest {

	@Autowired
	private MockMvc mockMvc;

	@Test
	public void shouldReturnDefaultMessage() throws Exception {
		this.mockMvc.perform(get("/")).andDo(print()).andExpect(status().isOk())
				.andExpect(content().string(containsString("Hello, World")));
	}
}
```


# Jenkins自动测试

[【Jenkins使用之九】jenkins自动测试 - cac2020 - 博客园](https://www.cnblogs.com/cac2020/p/13691505.html)