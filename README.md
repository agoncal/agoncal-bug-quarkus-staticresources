# No main manifest attribute when packaging static resources in an executable JAR with a Main

## Dev mode

This example is about serving static resources[1] (an `index.html` page) when packaged in an executable JAR.
If you look at the `pom.xml` there is just an Undertow dependency[2].
If you execute Quarkus in dev mode (`./mvnw clean quarkus:dev`) and go to http://localhost:8080/ you will see the `index.html` page.

## Production mode

Now, let's package the app with `./mvnw clean package`.
It creates an executable JAR.
When we execute it, it doesn't work:

```
$ java -jar target/static-resources-1.0.0-SNAPSHOT.jar

no main manifest attribute, in target/static-resources-1.0.0-SNAPSHOT.jar
```

So I've created a `Main` class as explained in[3] and added a `META-INF/MANIFEST.MF` file with the following content:

```
Main-Class: org.agoncal.bug.quarkus.Main
```

But I still get the same problem. It looks like Quarkus is not even reading the `MANIFEST.MF` file.


## References

* [1] https://quarkus.io/guides/http-reference#serving-static-resources
* [2] https://quarkus.io/guides/http-reference#servlet-config
* [3] https://quarkus.io/guides/lifecycle#the-main-method
