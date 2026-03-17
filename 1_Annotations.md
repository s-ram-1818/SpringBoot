
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



