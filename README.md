# JSON-in-java
How to make good use of a Java class to handle json repsonses in an API.

public class Person {
    private String firstName;
    private String lastName;
    private String alias;
    private int age;
    private boolean isMale;
    private Address address;

    //getters and setters
}

public class Address {
    private String street;
    private String city;
    private String state;
    private String zipCode;
        //getters and setters
}

1) By default, if you return an object where instance variable is not initialized, then, in UI, it receives null values also.
   {
   "firstName":"John",
   "lastName":"Doe",
   "alias":null,
   "age":29,
   "address":{
      "street":"100 Elm Way",
      "city":"Foo City",
      "state":"NJ",
      "zipCode":"01234"
   },
   "Male":true
}
   
  Hence, Use annotation: @JsonInclude(Include.NON_NULL) as class level to ignore null fileds as shown below:
  
  @JsonInclude(Include.NON_NULL)
public class Person {
    // ...
}

output will be: {
   "firstName":"John",
   "lastName":"Doe",
   "age":29,
   "address":{
      "street":"100 Elm Way",
      "city":"Foo City",
      "state":"NJ",
      "zipCode":"01234"
   },
   "Male":true
}

2) Also, observe in above example that boolean variable "isMale" is recieved in UI as "Male". This is correct and happens like this only. So, incase we wish to receive a boolean variable with name "isMale", then use annotation at setter level like this:

@JsonInclude(Include.NON_NULL)
public class Person{

@JsonProperty("isMale") 
    public boolean isMale() {
        return isMale;
    } 
//.....
}

output will be skiping null values and boolean isMale will be shown as isMale only: 

{
   "firstName":"John",
   "lastName":"Doe",
   "age":29,
   "address":{
      "street":"100 Elm Way",
      "city":"Foo City",
      "state":"NJ",
      "zipCode":"01234"
   },
   "isMale":true
}
   
   
