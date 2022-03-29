## Deploy
1. AWS CodeBuild 를 통한 서비스 배포
<img width="1338" alt="스크린샷 2022-03-29 오전 10 58 44" src="https://user-images.githubusercontent.com/54835264/160518739-c13292be-827d-4098-be67-6a05230f4c94.png">
<img width="1338" alt="스크린샷 2022-03-29 오전 10 57 59" src="https://user-images.githubusercontent.com/54835264/160518779-b748c2c3-3369-4d24-96fc-84de25ba10d3.png">


## AutoScale (HPA)
1. 컨터이너 리소스 CPU 200m 설정
<img width="312" alt="스크린샷 2022-03-28 오후 9 43 28" src="https://user-images.githubusercontent.com/54835264/160518898-2e808e95-382e-41cb-a940-9a1fa9b5306f.png">

2. HPA(Horizonal Pod Autoscale)를 cpu임계치=20%, 최대Pod수=3, 최저Pod수=1 로 설정
```
kubectl autoscale deployment team4-store --cpu-percent=20 --min=1 --max=3
```

3. siege 컨테이너 배포 후 siege 컨테이너 내부에서 동시사용자 1명, 20초간 부하 테스트를 실행
```
kubectl apply -f - <<EOF
apiVersion: v1
kind: Pod
metadata:
  name: siege
spec:
  containers:
  - name: siege
    image: apexacme/siege-nginx
EOF
```

```
kubectl exec -it siege -- /bin/bash
siege -c1 -t10S -v http://team4-store:8080/profile/products
```

<img width="526" alt="스크린샷 2022-03-28 오후 9 41 25" src="https://user-images.githubusercontent.com/54835264/160518943-1d828460-eabc-443e-9dd8-4ac8eb4bc97b.png">

4. CPU 리소스가 20%를 넘어서자 HPA 이벤트가 발생하고 Replicas=3 으로 AutoScale 실행 
<img width="631" alt="스크린샷 2022-03-28 오후 9 33 54" src="https://user-images.githubusercontent.com/54835264/160518979-07cd3bc4-7453-4d68-8e96-914daf8ad46b.png">

5. 쿨타임 이후 다시 3->1 로 Replica Set 변경
<img width="631" alt="스크린샷 2022-03-28 오후 9 40 24" src="https://user-images.githubusercontent.com/54835264/160518959-acbdc790-ac32-45d3-9846-61fe35efffd5.png">


## Self-healing (Liveness Probe)
1. Liveness Probe 설정 후 재배포 실행
```
livenessProbe:
  httpGet:
    path: /actuator/health
    port: 8080
  initialDelaySeconds: 120
  timeoutSeconds: 2
  periodSeconds: 5
  failureThreshold: 5
```

2. Spring-Boot 설정문제로 재배포 된 컨테이너 내부 오류 발생
<img width="1157" alt="스크린샷 2022-03-28 오후 10 35 15" src="https://user-images.githubusercontent.com/54835264/160519238-625d5083-d332-4a09-83ce-0f392fdfb847.png">
<img width="1143" alt="스크린샷 2022-03-28 오후 10 32 21" src="https://user-images.githubusercontent.com/54835264/160519188-d2a85efc-e847-41da-81d8-30da0b4ef9a4.png">

3. Liveness Probe 실패로 컨테이너 재시작 이벤트 발생
<img width="669" alt="스크린샷 2022-03-28 오후 10 33 29" src="https://user-images.githubusercontent.com/54835264/160519225-4a0b47f6-91c1-4edb-a643-7ef9ba0e9ba5.png">

## Zero-Downtime Deploy (Readiness Probe)
1. Readiness Probe 설정 후 재배포 실행
```
readinessProbe:
  httpGet:
    path: /actuator/health
    port: 8080
  initialDelaySeconds: 10
  timeoutSeconds: 2
  periodSeconds: 5
  failureThreshold: 10
```

2. 
<img width="379" alt="스크린샷 2022-03-28 오후 9 53 24" src="https://user-images.githubusercontent.com/54835264/160519270-2d68512b-2b95-40a7-aaf1-88347542b249.png">

<img width="981" alt="스크린샷 2022-03-28 오후 10 00 03" src="https://user-images.githubusercontent.com/54835264/160519277-39c82daa-3a59-4794-8670-eae92f55a328.png">



