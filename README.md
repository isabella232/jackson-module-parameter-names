[Jackson](../../../jackson) module
that adds support for accessing parameter names; a feature added in JDK 8.

## Status

This is a new, experimental module; 2.4 is the first official release.

[[![Build Status](https://travis-ci.org/FasterXML/jackson-module-parameter-names.svg)](https://travis-ci.org/FasterXML/jackson-module-parameter-names)](https://travis-ci.org/FasterXML/jackson-module-parameter-names)
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.fasterxml.jackson.module/jackson-module-parameter-names/badge.svg)](https://maven-badges.herokuapp.com/maven-central/com.fasterxml.jackson.module/jackson-module-parameter-names)


## Usage

### Maven dependency

For maven dependency definition, click on the second badge in the previous, Status section.

### Registering module

Like all standard Jackson modules (libraries that implement Module interface), registration is done as follows:

```java
ObjectMapper mapper = new ObjectMapper();
mapper.registerModule(new ParameterNamesModule());
```

after which functionality is available for all normal Jackson operations.

### Usage example

Java 8 API adds support for accessing parameter names at runtime in order to enable clients to abandon the JavaBeans standard if they want to without forcing them to use annotations (such as [JsonProperty][1]).

So, after registering the module as described above, you will be able to use data binding with a class like

```java
class Person {

    // mandatory fields
    private final String name;
    private final String surname;
    
    // optional fields
    private String nickname;

    // no annotations are required if preconditions are met (details below)
    public Person(String name, String surname) {

        this.name = name;
        this.surname = surname;
    }

    public String getName() {
        return name;
    }

    public String getSurname() {
        return surname;
    }

    public String getNickname() {

        return nickname;
    }

    public void setNickname(String nickname) {

        this.nickname = nickname;
    }
}
```

Preconditions:

  - class Person must be compiled with Java 8 compliant compiler with option to store formal parameter names turned on (`-parameters` option). For more information about Java 8 API for accessing parameter names at runtime see [this][2]
  - if there are multiple visible constructors and there is no default constructor, `@JsonCreator` is required to select constructor for deserialization
  - if used with `jackson-databind` lower than  `2.6.0`, `@JsonCreator` is required

## More

See [Wiki](../../wiki) for more information (javadocs, downloads).

[1]: http://jackson.codehaus.org/1.1.2/javadoc/org/codehaus/jackson/annotate/JsonProperty.html
[2]: http://docs.oracle.com/javase/tutorial/reflect/member/methodparameterreflection.html
