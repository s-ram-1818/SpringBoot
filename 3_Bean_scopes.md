
# Bean Scopes in Spring

Bean Scope defines **how many instances of a bean Spring creates and how long they live in the container**.

---

## Types of Bean Scopes

- Singleton
- Prototype
- Request
- Session

---

# 1. Singleton Scope (Default)

### Definition
Only **one instance of the bean is created per Spring container** and shared across the entire application.

### Example

```java
@Component
@Scope("singleton")
public class UserService {

    public UserService(){
        System.out.println("Singleton Bean Created");
    }

}
````

### Behavior

```
Application Start
↓
Spring creates one bean
↓
Same bean used everywhere
```

---

# 2. Prototype Scope

### Definition

Spring creates **a new instance every time the bean is requested**.

### Example

```java
@Component
@Scope("prototype")
public class PaymentService {

    public PaymentService(){
        System.out.println("Prototype Bean Created");
    }

}
```

### Behavior

```
Bean requested
↓
New object created
↓
Bean requested again
↓
Another new object created
```

---

# 3. Request Scope

### Definition

A **new bean is created for each HTTP request**.

Used only in **web applications**.

### Example

```java
@Component
@Scope("request")
public class RequestService {

}
```

### Behavior

```
User Request 1 → New Bean
User Request 2 → New Bean
```

---

# 4. Session Scope

### Definition

A **new bean is created for each user session**.

### Example

```java
@Component
@Scope("session")
public class SessionService {

}
```

### Behavior

```
User Login → Bean Created
Same Session → Same Bean Used
Session Ends → Bean Destroyed
```

---

# Summary Table

| Scope     | Instances Created      | Use Case               |
| --------- | ---------------------- | ---------------------- |
| Singleton | One per container      | Default for services   |
| Prototype | New instance each time | Stateful beans         |
| Request   | One per HTTP request   | Web request processing |
| Session   | One per user session   | User-specific data     |

---

# Quick Revision

```
Singleton → One bean for entire application
Prototype → New bean each time requested
Request → One bean per HTTP request
Session → One bean per user session
```

```
```
