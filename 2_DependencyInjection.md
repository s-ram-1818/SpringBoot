
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

# Unsatisfied Dependency Problem (Spring)

## Definition
**Unsatisfied Dependency** occurs when **Spring cannot find or inject a required bean** into another bean.

This usually happens during **application startup**, and Spring throws an error like:

```

No qualifying bean of type 'Engine' available

````

---

## Example (Problem)

```java
@Component
class Car {

    @Autowired
    private Engine engine; // dependency

}
````

```java
class Engine {
    // Not a Spring Bean
}
```

### Problem

* `Engine` is **not registered as a bean**
* Spring cannot inject it into `Car`

---

## Causes of Unsatisfied Dependency

### 1. Bean Not Defined

```java
class Engine {} // Missing @Component
```

---

### 2. Wrong Package Scanning

Spring cannot detect beans outside the scan path.

---

### 3. Multiple Beans (Ambiguity)

```java
@Component
class PetrolEngine implements Engine {}

@Component
class DieselEngine implements Engine {}
```

```java id="u2ye9g"
@Autowired
private Engine engine; // Which one?
```

---

### 4. Missing `@Autowired` or Injection

Dependency is declared but not properly injected.

---

## Solutions

### 1. Register Bean

```java
@Component
class Engine {}
```

OR

```java
@Bean
public Engine engine(){
    return new Engine();
}
```

---

### 2. Fix Package Scanning

Ensure main class scans correct package:

```java
@SpringBootApplication(scanBasePackages = "com.example")
```

---

### 3. Resolve Multiple Beans

Use `@Qualifier`:

```java
@Autowired
@Qualifier("petrolEngine")
private Engine engine;
```

OR use `@Primary`:

```java
@Primary
@Component
class PetrolEngine implements Engine {}
```

---

### 4. Optional Dependency

```java
@Autowired(required = false)
private Engine engine;
```

---

## Short Note

**Unsatisfied Dependency:** When Spring **fails to inject a required bean because it cannot find or resolve it**.

```
```

