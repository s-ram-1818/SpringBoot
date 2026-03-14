````md
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



