
## 비동기식 호출 / 시간적 디커플링 / 장애격리 / 최종 (Eventual) 일관성 테스트
**Req/Res 연동**

1. 주문 생성
http localhost:8081/orders productId=1 quantity=3 customerId="sunghan.lee@hanwha.com" customerName="Lee" customerAddr="seoul"
![image](https://user-images.githubusercontent.com/102270635/160507856-40a69811-e01d-430f-a2f5-e2313cc2608b.png)



2. 배송 확인
http localhost:8082/deliveries
![image](https://user-images.githubusercontent.com/102270635/160507815-d00e48e4-dcc1-47c6-acd3-750818c1047a.png)


3. 부하 툴을 사용하여 주문 생성
siege -c2 -t10S  -v --content-type "application/json" 'http://localhost:8081/orders POST {"productId":2, "quantity":1}

<부하 전>
![image](https://user-images.githubusercontent.com/102270635/160507930-4c679192-cbf2-419b-a275-30496384bcd1.png)

<부하 후>
![image](https://user-images.githubusercontent.com/102270635/160507945-1201381f-8d3e-4787-8c26-7810eded63b0.png)


4. fallback 처리를 하여 유연하게 대처
<fallback 처리 후>
![image](https://user-images.githubusercontent.com/102270635/160507989-f5f3462d-ca00-4aa8-8310-73dce9b3d419.png)


## Deploy
1. AWS CodeBuild 를 통한 서비스 배포
<img width="1338" alt="스크린샷 2022-03-29 오전 10 58 44" src="https://user-images.githubusercontent.com/54835264/160518739-c13292be-827d-4098-be67-6a05230f4c94.png">
<img width="1338" alt="스크린샷 2022-03-29 오전 10 57 59" src="https://user-images.githubusercontent.com/54835264/160518779-b748c2c3-3369-4d24-96fc-84de25ba10d3.png">




## AutoScale (HPA)
1. AWS CodeBuild 를 통한 서비스 배포

<img width="631" alt="스크린샷 2022-03-28 오후 9 33 54" src="https://user-images.githubusercontent.com/54835264/160518979-07cd3bc4-7453-4d68-8e96-914daf8ad46b.png">


<img width="631" alt="스크린샷 2022-03-28 오후 9 40 24" src="https://user-images.githubusercontent.com/54835264/160518959-acbdc790-ac32-45d3-9846-61fe35efffd5.png">


<img width="526" alt="스크린샷 2022-03-28 오후 9 41 25" src="https://user-images.githubusercontent.com/54835264/160518943-1d828460-eabc-443e-9dd8-4ac8eb4bc97b.png">


<img width="312" alt="스크린샷 2022-03-28 오후 9 43 28" src="https://user-images.githubusercontent.com/54835264/160518898-2e808e95-382e-41cb-a940-9a1fa9b5306f.png">





## Self-healing (Liveness Probe)
<img width="1143" alt="스크린샷 2022-03-28 오후 10 32 21" src="https://user-images.githubusercontent.com/54835264/160519188-d2a85efc-e847-41da-81d8-30da0b4ef9a4.png">


<img width="669" alt="스크린샷 2022-03-28 오후 10 33 29" src="https://user-images.githubusercontent.com/54835264/160519225-4a0b47f6-91c1-4edb-a643-7ef9ba0e9ba5.png">

<img width="1157" alt="스크린샷 2022-03-28 오후 10 35 15" src="https://user-images.githubusercontent.com/54835264/160519238-625d5083-d332-4a09-83ce-0f392fdfb847.png">



## Zero-Downtime Deploy (Readiness Probe)

<img width="379" alt="스크린샷 2022-03-28 오후 9 53 24" src="https://user-images.githubusercontent.com/54835264/160519270-2d68512b-2b95-40a7-aaf1-88347542b249.png">

<img width="981" alt="스크린샷 2022-03-28 오후 10 00 03" src="https://user-images.githubusercontent.com/54835264/160519277-39c82daa-3a59-4794-8670-eae92f55a328.png">



