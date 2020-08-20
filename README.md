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