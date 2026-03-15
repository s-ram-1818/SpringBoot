
# Dependency Injection (DI) – Short Notes

## Definition
**Dependency Injection (DI)** is a design pattern where **Spring provides required objects (dependencies) to a class instead of the class creating them itself**.

This reduces **tight coupling** between classes.

---

## Example

```java
@Component
class Engine {}

@Component
class Car {

    @Autowired
    private Engine engine;

}
````

Here Spring **creates the Engine bean and injects it into Car**.

---

# Types of Dependency Injection

## 1. Constructor Injection (Recommended)

Dependency injected through constructor.

```java
@Component
class Car {

    private final Engine engine;

    public Car(Engine engine){
        this.engine = engine;
    }

}
```

Advantages:

* Best practice
* Supports immutability
* Easier testing

---

## 2. Setter Injection

Dependency injected through setter method.

```java
@Component
class Car {

    private Engine engine;

    @Autowired
    public void setEngine(Engine engine){
        this.engine = engine;
    }

}
```

Used when dependency is **optional**.

---

## 3. Field Injection

Dependency injected directly into class field.

```java
@Component
class Car {

    @Autowired
    private Engine engine;

}
```

Problems:

* Hard to test
* Hidden dependencies
* Not recommended

---

# Benefits of Dependency Injection

* Reduces **tight coupling**
* Improves **maintainability**
* Easier **unit testing**
* More **flexible design**

---

# Quick Summary

| Injection Type        | Description                    |
| --------------------- | ------------------------------ |
| Constructor Injection | Recommended method             |
| Setter Injection      | Used for optional dependencies |
| Field Injection       | Simple but not recommended     |

---

## One Line Definition

**Dependency Injection:** A mechanism where **Spring automatically provides required objects to a class.**

```
```
