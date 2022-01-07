# poc-wiremock
This PoC was developed to test the Forward Carllback feature of the WireMock (http://wiremock.org/).

### Follow the steps below to run the PoC:

1. Build the poc-webhook


The poc-webhook is a simple application to simulate an application that needs to receive a request from Mock-Server, it will start at port 9191
```bash
$ ./gradlew build

$ java -jar build/libs/poc-webhook-0.1-all.jar
```

2. Run MockServer as docker container

```bash
$ docker run --rm --network=host -p 8080:8080 -v $PWD/stubs:/home/wiremock -v $PWD/extensions:/var/wiremock/extensions wiremock/wiremock:2.32.0 --extensions com.opentable.extension.BodyTransformer,org.wiremock.weboks.Webhooks --global-response-templating
```

### For testing use this curl command
```bash
curl --location --request POST 'http://localhost:8080/payment/register' \
--header 'Content-Type: application/json' \
--data-raw '{
    "card": "1111222233334444"
}'
```
The WireMock will generate an ID and Date to return as response, but by default the webhook extension can generate newer data to be send and/or get data from the original request. To have the same data from original response send to webhook a custom extension needs to be implemented.
