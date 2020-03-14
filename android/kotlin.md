# Kotlin notes

#### Infos related to classes
- **public** is the default visibility operator
 - **private**: visible in the same file
 - **internal**: visible in the same module
 - **protected**: visible in the class and subclasses
- if **open** stands in front of class definition, it means inheriting from that particular class is possible.
- if **val/var** is used in class definition they are meant to be considered as class properties. Variables used in class definition WITHOUT **val/var** they serve as only parameters, which can be accessed in **init** block, or use to initialize other class property.
```java
class User(val id: Long, email: String) {
    val hasEmail = email.isNotBlank()    //email can be accessed here
    init {
        //email can be accessed here
    }

    fun getEmail(){
        //email can't be accessed here
    }
}
```
- **init blocks** run before secondary constructor
- classes have default **getters/setters**
- **abstract** classes can not be instantieted
- **interface methods** can have a default implementation
- **interface properties** must be overwritten
