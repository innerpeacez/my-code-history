@Configuration

@ComponentScan

@RestController 是一个组合注解，包含了@Controller和@ResponseBody，使用了这个注解的类中的每一个方法上都会有一个@ResponseBody注解。

@Transactional

@RequestMapping 匹配所有HTTP方法，可以定义为class-level

@GetMapping

@PostMapping

@PutMapping

@DeleteMapping

@PatchMapping


URL pattern 

? 匹配一个字符

* 匹配路径段中的零个或多个字符

** 匹配零个或多个路径段