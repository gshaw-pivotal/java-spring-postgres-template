## To build docker

From root: `docker compose build`

## To start docker image

From root: `docker compose up -d`

## To stop docker image

From root: `docker compose down`

## To access Postgres within the docker image / container

Access the command line within the container (via a terminal):
`docker exec -it backend-db bash`

Log into Postgres DB:
`psql -U postgres -d backend-db`

List tables in the DB/schema:
`\dt`

## Development Notes

### Database / Repository

The Spring Boot application can interact with the Postgres DB utilizing the following:

- For data objects, a class can be created to represent a record in a given DB table. For example:
```java
@Builder
@Data
@Entity
@Table(name = "<name_of_db_table>")
@NoArgsConstructor
@AllArgsConstructor
public class Foo {

    @Id
    private UUID id; //The primary key

    //A variable for each column in the table
    //private <data_type> <column_name>
}
```
- This enables Lombok support and JPA support and allows a row in the DB to be mapped into this POJO.
- For repositories using the CRUDRepository approach should work:
```java
@Repository
public interface FooRepository extends CrudRepository<Foo, UUID> {
}
```
- Here the two parameters associated with the CrudRepository is the POJO that matches with the table and the type of the Primary Key. In this example the POJO for the table is Foo and the primary key is a UUID.

### Controller / Rest API

Annotate class with `@RestController`, then for each endpoint create a public method with the annotation to support the Rest method/type.

Sample endpoints include:
```java
@GetMapping(value = "/api/foo/{id}")
public ResponseEntity<Foo> getFoo(@PathVariable Foo foo) {
    return ResponseEntity.ok(...);
}
```
```java
@PostMapping(value = "/api/foo")
public ResponseEntity<Foo> getFoo(@RequestBody Foo foo) {
    return ResponseEntity.ok(...);
}
```

### Configuring Beans

To have control over beans you can create a configuration class and explicitly bean up classes.
```java
@Configuration
public class Configuration {

    @Bean
    public FooService fooService(...) {
        return new FooService(...);
    }
}
```
And the associated class being `beaned` in the configuration class has a constructor method.
```java
public class FooService {
    public FooService(...) {
        this.a = a;
        ...
    }
}
```