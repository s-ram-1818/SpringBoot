
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


# Singleton Bean with Request Scope in Spring

## Problem

A **Singleton bean** is created **once per application**, while a **Request-scoped bean** is created **once per HTTP request**.

If a singleton directly injects a request-scoped bean, the request bean is created **only once at startup**, which breaks request scope behavior.

Example (Problem):

```java
@Component
@Scope("singleton")
public class OrderService {

    @Autowired
    private RequestBean requestBean; // wrong behavior
}
````

```java
@Component
@Scope("request")
public class RequestBean {}
```

Here:

* `OrderService` is created **once**
* `RequestBean` is injected **once**
* It will **not change per request**

---

# Solution: Use Scoped Proxy

Spring creates a **proxy object** that fetches the correct request bean **for every HTTP request**.

Example:

```java
@Component
@Scope(value = "request", proxyMode = ScopedProxyMode.TARGET_CLASS)
public class RequestBean {

}
```

Now inject normally:

```java
@Component
public class OrderService {

    @Autowired
    private RequestBean requestBean;

}
```

---

# How It Works

```
Singleton Bean
     ↓
Proxy Object
     ↓
Actual Request Bean (created per request)
```

Each request gets its **own RequestBean instance**, while the singleton holds only the **proxy reference**.

---

# Summary

| Bean Type | Instances            |
| --------- | -------------------- |
| Singleton | One per application  |
| Request   | One per HTTP request |

To use **request scope inside singleton**, use **Scoped Proxy**.

---

# One Line

**Scoped Proxy allows a singleton bean to safely use a request-scoped bean by injecting a proxy instead of the real object.**

```
```


