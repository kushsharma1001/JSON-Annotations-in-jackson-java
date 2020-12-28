# JSON-Annotations-in-jackson-java
How to make good use of a Java class to handle json repsonses in an API.

public class Person {
    private String firstName;
    private String lastName;
    private String alias;
    private int age;
    private boolean isMale;
    private Address address;

    //getters and setters must be there
    //default constructor is must. we can leave it empty.
}

public class Address {
    private String street;
    private String city;
    private String state;
    private String zipCode;
    //getters and setters must be there
    //default constructor is must. we can leave it empty.
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
   
3) Above 2 points talk about if UI or user uses the api which returns Person object. But, what if we have a POST api exposed and we take some json from user/postman. Then, apart from the variables available in Person class, they may pass additional fields. By default, we POST call to such an API happens with additional fields, it throws exception. We can allow it to happen by using annotation: 

@JsonIgnoreProperties(ignoreUnknown = true)
public class Person {
    // ...
}

This, we can make a POST call to our api now even with additional fields. It will ignore the fields sent in Postman by user which are not available in our Person class and only map the fields available in Person class.
