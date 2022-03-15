3.Create a pod called multi-test with two containers.     
Container 1, name: one, image: nginx. env= name: alpha   
Container 2, name: two, image: busybox, command sleep 4800. env= name: beta 
----------------------------    
mutil Container 관련 문제.    

참고문서 : https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/    

kubectl run multi-test --image=nginx --dry-run=client -o yaml > mult-test.yaml   

YAML 파일 리디렉션 후 작업. 하나의 파드 안에 두개의 컨테이너 정의.   

환경변수 설정시  'name=alpha'처럼 설정하기 위해선 -name 부분과 value 값을 따로 정의해야 한다.   

mult-test.yaml   
---
apiVersion: v1   
kind: Pod   
metadata:   
  name: multi-test   
spec:   
  containers:   
  - name: one   
    image: nginx   
    env:   
    - name: name   
      value: alpha   
  containers:   
  - name: two    
    image: busybox   
    command: ["sleep", "4800"]   
    env:   
    -  name: name   
       value: beta   
