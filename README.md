# 
## EventStorming Model
![image](https://github.com/Lsuyeon/food-delivery/assets/89675167/fc4b713a-9c22-4c4b-820c-9274aad5e63c)

## Saga (Pub / Sub)
![image](https://github.com/Lsuyeon/food-delivery/assets/89675167/d0d70cd2-5715-47f9-93ba-11836570014a)

## CQRS
![image](https://github.com/Lsuyeon/food-delivery/assets/89675167/de862618-6a1a-43f4-bbd3-5ab813335941)

## Compensation/Correlation
```
gitpod /workspace/food-delivery (main) $ http POST http://localhost:8081/orders foodId="케밥" address="서울시 동대문구" customerId="lsy"
HTTP/1.1 201 
Connection: keep-alive
Content-Type: application/json
Date: Thu, 11 May 2023 21:40:11 GMT
Keep-Alive: timeout=60
Location: http://localhost:8081/orders/7
Transfer-Encoding: chunked
Vary: Origin
Vary: Access-Control-Request-Method
Vary: Access-Control-Request-Headers

{
    "_links": {
        "order": {
            "href": "http://localhost:8081/orders/7"
        },
        "self": {
            "href": "http://localhost:8081/orders/7"
        }
    },
    "address": "서울시 동대문구",
    "customerId": "lsy",
    "foodId": "케밥",
    "options": null,
    "orderId": null,
    "storeId": null
}
```
```
gitpod /workspace/food-delivery (main) $ http PUT "http://localhost:8082/foodCookings/6/accept" accept=true
HTTP/1.1 200 
Connection: keep-alive
Content-Type: application/json;charset=UTF-8
Date: Thu, 11 May 2023 21:45:46 GMT
Keep-Alive: timeout=60
Transfer-Encoding: chunked

{
    "customerId": "lsy",
    "foodId": "케밥",
    "id": 6,
    "options": [],
    "orderId": "7",
    "status": "접수됨",
    "storeId": null
}
```
```
gitpod /workspace/food-delivery (main) $ http :8082/foodCookings/6
HTTP/1.1 200 
Connection: keep-alive
Content-Type: application/hal+json
Date: Thu, 11 May 2023 21:46:19 GMT
Keep-Alive: timeout=60
Transfer-Encoding: chunked
Vary: Origin
Vary: Access-Control-Request-Method
Vary: Access-Control-Request-Headers

{
    "_links": {
        "accept": {
            "href": "http://localhost:8082/foodCookings/6/accept"
        },
        "foodCooking": {
            "href": "http://localhost:8082/foodCookings/6"
        },
        "self": {
            "href": "http://localhost:8082/foodCookings/6"
        },
        "start": {
            "href": "http://localhost:8082/foodCookings/6/start"
        }
    },
    "customerId": "lsy",
    "foodId": "케밥",
    "options": [],
    "orderId": "7",
    "status": "접수됨",
    "storeId": null
}
```
## Model
www.msaez.io/#/storming/food-delivery-Lsy

## Before Running Services
### Make sure there is a Kafka server running
```
cd kafka
docker-compose up
```
- Check the Kafka messages:
```
cd kafka
docker-compose exec -it kafka /bin/bash
cd /bin
./kafka-console-consumer --bootstrap-server localhost:9092 --topic
```

## Run the backend micro-services
See the README.md files inside the each microservices directory:

- front
- store
- rider
- customer


## Run API Gateway (Spring Gateway)
```
cd gateway
mvn spring-boot:run
```

## Test by API
- front
```
 http :8088/orders id="id" foodId="foodId" options="options" address="address" customerId="customerId" orderId="orderId" storeId="storeId" 
 http :8088/payments id="id" orderId="orderId" status="status" 
```
- store
```
 http :8088/foodCookings id="id" status="status" foodId="foodId" orderId="orderId" options="options" storeId="storeId"  customerId=" customerId" 
```
- rider
```
 http :8088/deliveries id="id" status="status" orderid="orderid" address="address" 
```
- customer
```
```


## Run the frontend
```
cd frontend
npm i
npm run serve
```

## Test by UI
Open a browser to localhost:8088

## Required Utilities

- httpie (alternative for curl / POSTMAN) and network utils
```
sudo apt-get update
sudo apt-get install net-tools
sudo apt install iputils-ping
pip install httpie
```

- kubernetes utilities (kubectl)
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

- aws cli (aws)
```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

- eksctl 
```
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
```

