
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
![image](https://user-images.githubusercontent.com/102270635/160507856-40a69811-e01d-430f-a2f5-e2313cc2608b.png)
![image](https://user-images.githubusercontent.com/102270635/160507856-40a69811-e01d-430f-a2f5-e2313cc2608b.png)




## AutoScale (HPA)
1. AWS CodeBuild 를 통한 서비스 배포




## Self-healing (Liveness Probe)




## Zero-Downtime Deploy (Readiness Probe)
