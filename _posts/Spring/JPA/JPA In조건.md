# JPA In조건 예제 



## 1. Overview

In this article we will learn, Spring JPA query IN clause example or Spring JPA IN or NOT IN query with an example.  In SQL query, when we require to select multiple records which based on multiple values IN cause is used. and same ways we want to ignore multiple records based on multiple values at that NOT IN query is used,  For example:

```
SELECT * FROM employee WHERE id IN (1,2,3...)
```

Above query will select all those employees whose id is 1 or 2 or 3.

While working with spring data JPA we require `List` or `Collection` of those records that we require to pass in IN clause. Spring data JPA support IN queries using method name, @Query annotation, or native query.

Here is a complete example of [spring JPA with the database.](https://javadeveloperzone.com/spring-boot/spring-boot-restful-web-services-with-jpa-example/)

## 2. Spring JPA query IN and NOT IN query examples

### 2.1 Spring JPA query IN clause example

In bellow repository, we have defined three methods:

1. In the first method, we have fetched records using the method name, When we define a method with a specified naming conversation then spring JPA will automatically generate the query at runtime and return the result.
2. In the second method, We have used `@Query` and passed `names` as a parameter `List`.
3. In the third and last method, We have used the native query in which, nativeQuery = true means `value` contains the native query.



```java
@Repository
@Transactional
public interface EmployeeDAO extends JpaRepository<Employee,Integer> {

    List<Employee> findByEmployeeNameIn(List<String> names);                // 1. Spring JPA In cause using method name

    @Query("SELECT e FROM Employee e WHERE e.employeeName IN (:names)")     // 2. Spring JPA In cause using @Query
    List<Employee> findByEmployeeNames(@Param("names")List<String> names);

    @Query(nativeQuery =true,value = "SELECT * FROM Employee as e WHERE e.employeeName IN (:names)")   // 3. Spring JPA In cause using native query
    List<Employee> findByEmployeeName(@Param("names") List<String> names);
}
```

#### Output:

```java
java.util.List<String> names = new ArrayList<>();
names.add("Jone");
names.add("Harry");
List<Employee> employees = employeeDAO.findByEmployeeNameIn(names);
```

#### Query:

```
SELECT * FROM employee WHERE employeeName IN (? , ?)
```

### 2.2 Spring JPA query NOT IN clause example

Three different ways to write `NOT IN` query in Spring JPA as below:

```java
@Repository
@Transactional
public interface EmployeeDAO extends JpaRepository<Employee,Integer> {

    List<Employee> findByEmployeeNameNotIn(List<String> names);                // Spring JPA In cause using method name

    @Query("SELECT e FROM Employee e WHERE e.employeeName NOT IN (:names)")     // Spring JPA In cause using @Query
    List<Employee> findByEmployeeNamesNot(@Param("names")List<String> names);

    @Query(nativeQuery =true,value = "SELECT * FROM Employee as e WHERE e.employeeName NOT IN (:names)")   // Spring JPA In cause using native query
    List<Employee> findByEmployeeNameNot(@Param("names") List<String> names);

}
```

#### Output:

```java
java.util.List<String> names = new ArrayList<>();
names.add(“Harry”);
List<Employee> employees = employeeDAO.findByEmployeeNameNotIn(names);
```

#### Query:

```
SELECT * FROM employee WHERE employeeName NOT IN (? , ?)
```

### 2.3 Spring JPA dynamic IN Query

Here we have defined ways to write dynamic IN query, for example, if we passed `List` of names then it will return result based on those names and if we pass the empty or null value in list parameter then it will fetch all the records. [Here are more examples of Spring JPA dynamic query](https://javadeveloperzone.com/spring/spring-jpa-dynamic-query-example/).

```java
public List<Employee> findByInCriteria(List<String> names){
        return employeeDAO.findAll(new Specification<Employee>() {
            @Override
            public Predicate toPredicate(Root<Employee> root, CriteriaQuery<?> query, CriteriaBuilder criteriaBuilder) {
                List<Predicate> predicates = new ArrayList<>();
                if(names!=null && !names.isEmpty()) {
                    predicates.add(root.get("employeeName").in(names));
                }
                return criteriaBuilder.and(predicates.toArray(new Predicate[predicates.size()]));
            }
        });
    }
```

### 2.4 Spring JPA dynamic NOT IN Query

Here we have defined the way to write dynamic `NOT IN` query. Means if list contains any value then it will be considered as where cause otherwise it will be ignored. [Here are more examples of Spring JPA dynamic query](https://javadeveloperzone.com/spring/spring-jpa-dynamic-query-example/).

```java
public List<Employee> findByInCriteria(List<String> names){
        return employeeDAO.findAll(new Specification<Employee>() {
            @Override
            public Predicate toPredicate(Root<Employee> root, CriteriaQuery<?> query, CriteriaBuilder criteriaBuilder) {
                List<Predicate> predicates = new ArrayList<>();
                if(names!=null && !names.isEmpty()) {
                    predicates.add(criteriaBuilder.not(root.get("employeeName").in(names)));
                }
                return criteriaBuilder.and(predicates.toArray(new Predicate[predicates.size()]));
            }
        });
}
```

## 3. Conclusion

In this article, We learn that ways write IN and NOT IN query in spring JPA, We learn three ways to write JPA query like using IN and NOT IN query using method name, using @Query parameter and using native query. We also learn to write dynamic IN and NOT IN query in spring data JPA.

## 4. References

- [Spring data JPA dynamic query](https://spring.io/blog/2011/04/26/advanced-spring-data-jpa-specifications-and-querydsl/)
- [Spring JPA documentation ](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/)