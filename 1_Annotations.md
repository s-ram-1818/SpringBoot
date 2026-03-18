
# @Controller vs @RestController

**@Controller**
- Used for **Spring MVC**
- Returns **View (HTML/JSP)**

```java
@Controller
public class HomeController {

    @GetMapping("/home")
    public String home() {
        return "home"; // returns home.html
    }
}
````

---

**@RestController**

* Used for **REST APIs**
* Returns **JSON directly**

```java
@RestController
public class UserController {

    @GetMapping("/user")
    public String getUser() {
        return "Ram";
    }
}
```

**Note:**
`@RestController = @Controller + @ResponseBody`


# @RequestParam

`@RequestParam` is used to **get query parameters from the URL** in a Spring Boot controller.

### Example

```java
@RestController
public class UserController {

    @GetMapping("/user")
    public String getUser(@RequestParam String name) {
        return "Hello " + name;
    }
}
````

**Request URL**

```
/user?name=Ram
```

**Response**

```
Hello Ram
```


# @PathVariable

`@PathVariable` is used to **get values from the URL path** in a Spring Boot controller.

### Example

```java
@RestController
public class UserController {

    @GetMapping("/user/{id}")
    public String getUser(@PathVariable int id) {
        return "User ID: " + id;
    }
}
````

**Request URL**

```
/user/10
```

**Response**

```
User ID: 10
```


# @RequestBody

`@RequestBody` is used to **get data from the request body (JSON)** and convert it into a Java object.

### Example

```java
@RestController
public class UserController {

    @PostMapping("/user")
    public String createUser(@RequestBody User user) {
        return user.getName();
    }
}
````

**Request Body (JSON)**

```
{
  "name": "Ram",
  "age": 22
}
```

```
```

# @JsonProperty

`@JsonProperty` is used to **map a JSON field name to a Java class field**.

### Example

```java
public class User {

    @JsonProperty("user_name")
    private String name;

}
````

**JSON**

```
{
  "user_name": "Ram"
}
```

Here JSON field **user_name** maps to Java field **name**.

```
```

# Spring Core Short Notes

## Bean
A **Bean** is an object that is **created and managed by the Spring IoC container**.

---

## `@Component`
- Class level annotation.
- Used to **automatically create a bean** using component scanning.

Example:
```java
@Component
public class UserService {}
````

---

## `@Bean`

* Method level annotation.
* Used to **manually create and register a bean** inside a `@Configuration` class.

Example:

```java
@Configuration
public class AppConfig {

    @Bean
    public UserService userService(){
        return new UserService();
    }
}
```

---

## Difference: `@Component` vs `@Bean`

| `@Component`            | `@Bean`                      |
| ----------------------- | ---------------------------- |
| Class level             | Method level                 |
| Automatic bean creation | Manual bean creation         |
| Used for own classes    | Used for third-party classes |

---

## Dependency Injection (DI)

Spring **automatically provides required objects to a class instead of the class creating them**.

Example:

```java
@Autowired
Engine engine;
```

---

## Types of Injection

1. Constructor Injection (Best)
2. Setter Injection
3. Field Injection

---

## `@PostConstruct`

Runs **after bean creation and dependency injection**.

Example:

```java
@PostConstruct
public void init(){}
```

---

## Bean Lifecycle

```
Bean Creation
↓
Dependency Injection
↓
@PostConstruct (Initialization)
↓
Bean Ready
↓
@PreDestroy (Before destruction)
```

---

## Bean Creation Time

Default → **Eager initialization** (created at startup)

Use `@Lazy` → **Created only when needed**

```
```


# `@ConditionalOnProperty` in Spring Boot

## Definition
`@ConditionalOnProperty` is used to **create a bean only if a specific property is present (or has a specific value)** in `application.properties` or `application.yml`.

---

## Syntax

```java
@ConditionalOnProperty(
    name = "property.name",
    havingValue = "value",
    matchIfMissing = false
)
````

---

## Example

### Step 1: Define Property

```properties
feature.payment.enabled=true
```

---

### Step 2: Use Annotation

```java
@Component
@ConditionalOnProperty(name = "feature.payment.enabled", havingValue = "true")
public class PaymentService {

    public PaymentService() {
        System.out.println("PaymentService Bean Created");
    }

}
```

### Behavior

* If `feature.payment.enabled=true` → Bean is created ✅
* If `false` or missing → Bean NOT created ❌

---

## Another Example (with `@Bean`)

```java
@Configuration
public class AppConfig {

    @Bean
    @ConditionalOnProperty(name = "feature.email.enabled", havingValue = "true")
    public EmailService emailService(){
        return new EmailService();
    }
}
```

---

## Important Attributes

| Attribute      | Meaning                                        |
| -------------- | ---------------------------------------------- |
| name           | Property name to check                         |
| havingValue    | Required value to match                        |
| matchIfMissing | If true, bean created when property is missing |

---

## Example with `matchIfMissing`

```java
@ConditionalOnProperty(
    name = "feature.sms.enabled",
    havingValue = "true",
    matchIfMissing = true
)
```

* If property missing → Bean created ✅
* If false → Not created ❌

---

## Use Cases

* Enable/disable features (**feature toggles**)
* Environment-based configuration
* Optional services (email, payment, logging)

---

## Short Note

**`@ConditionalOnProperty` → Creates a bean only when a specific property value matches the condition.**

```
```
[
# `@Profile` Annotation in Spring Boot

---

## 1. Definition

`@Profile` is used to **load or activate beans only for specific environments (profiles)**.

👉 Common profiles:
- `dev`
- `test`
- `prod`

---

## 2. Why Use It?

- Different configurations for different environments  
- Enable/disable beans based on environment  
- Avoid manual code changes  

---

## 3. Basic Syntax

```java
@Profile("dev")
@Component
public class DevService {}
````

This bean will be created **only when `dev` profile is active**.

---

## 4. Activate Profile

### Using `application.properties`

```properties
spring.profiles.active=dev
```

---

### Using Command Line

```bash
--spring.profiles.active=prod
```

---

## 5. Example

### Dev Bean

```java id="x7p4mv"
@Component
@Profile("dev")
class DevDatabase {

    public DevDatabase(){
        System.out.println("Dev DB Connected");
    }
}
```

---

### Prod Bean

```java id="6l4v9n"
@Component
@Profile("prod")
class ProdDatabase {

    public ProdDatabase(){
        System.out.println("Prod DB Connected");
    }
}
```

---

### Behavior

| Active Profile | Bean Created |
| -------------- | ------------ |
| dev            | DevDatabase  |
| prod           | ProdDatabase |

---

## 6. Multiple Profiles

```java id="j7sq0z"
@Profile({"dev", "test"})
@Component
class TestService {}
```

Bean is created if **any profile matches**.

---

## 7. Negation (NOT Profile)

```java id="8gdf1k"
@Profile("!prod")
@Component
class NonProdService {}
```

Bean is created in **all profiles except prod**.

---

## 8. With `@Bean`

```java id="c0l4h9"
@Configuration
public class AppConfig {

    @Bean
    @Profile("dev")
    public DataSource devDataSource(){
        return new DevDataSource();
    }
}
```

---

## 9. Internal Working

```text id="tx9l2a"
Spring Boot Starts
↓
Reads active profile
↓
Checks @Profile condition
↓
Match?
   → YES → Bean Created
   → NO  → Bean Skipped
```

---

## 10. Difference: `@Profile` vs `@ConditionalOnProperty`

| Feature  | `@Profile`                  | `@ConditionalOnProperty` |
| -------- | --------------------------- | ------------------------ |
| Based on | Environment (dev/test/prod) | Property value           |
| Use case | Environment-specific config | Feature toggle           |

---

## 11. Best Practices

* Use for **environment-based configs**
* Combine with config classes
* Keep profiles clean (`dev`, `prod`, etc.)
* Avoid too many profiles

---

## 12. Short Summary

* Loads beans **based on active environment**
* Helps manage **multiple environments**
* Works with `@Component` and `@Bean`

---

## One Line Definition

**`@Profile` → Creates a bean only when a specific environment profile is active.**

```
```




