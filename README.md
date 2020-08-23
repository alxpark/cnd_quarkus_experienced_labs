# Cloud Native Development with Quarkus - Experienced - Labs

This repository contains the _Cloud Native Development with Quarkus - Experienced_ lab exercises.

mvn quarkus:add-extension -Dextensions="health"

mvn clean quarkus:dev -DskipTests

http://localhost:8080


curl http://localhost:8080/health/ready
```
{
    "status": "UP",
    "checks": [
        {
            "name": "Simple health check",
            "status": "UP"
        }
    ]
}
```

curl -i http://localhost:8080/health/live
```
{
    "status": "DOWN",
    "checks": [
        {
            "name": "Health check with data",
            "status": "UP",
            "data": {
                "bar": "barValue",
                "foo": "fooValue"
            }
        },
        {
            "name": "Database connection health check",
            "status": "DOWN",
            "data": {
                "error": "Cannot contact database"
            }
        }
    ]
}
```

application.properties
database.up=true

curl -i http://localhost:8080/health/live
```
{
    "status": "UP",
    "checks": [
        {
            "name": "Database connection health check",
            "status": "UP"
        },
        {
            "name": "Health check with data",
            "status": "UP",
            "data": {
                "bar": "barValue",
                "foo": "fooValue"
            }
        }
    ]
}
```

mvn clean quarkus:dev -DskipTests
curl http://localhost:8080/hello

mvn clean test

mvn quarkus:generate-config



# Build Executable JAR
mvn clean package
ls -1 target/*.jar
java -Dquarkus.http.port=8081 -jar target/*-runner.jar


# redeploy
mvn clean package -DskipTests
oc start-build people --from-file target/*-runner.jar --follow
oc rollout status -w dc/people

PEOPLE_ROUTE_URL=$(oc get route people -o=template --template='{{.spec.host}}')
curl http://${PEOPLE_ROUTE_URL}/person/birth/before/2000

echo; echo "http://${PEOPLE_ROUTE_URL}/datatable.html" ; echo



# Event driven
mvn quarkus:add-extension -Dextensions="vertx"
mvn clean quarkus:dev
curl -i -X POST http://localhost:8080/person/joe
curl http://localhost:8080/person/name/joe
