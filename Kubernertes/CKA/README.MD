3.Create a pod called multi-test with two containers.     
Container 1, name: one, image: nginx. env= name: alpha   
Container 2, name: two, image: busybox, command sleep 4800. env= name: beta 
----------------------------    
mutil Container 관련 문제.    

참고문서 : https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/    

kubectl run multi-test --image=nginx --dry-run=client -o yaml > mult-test.yaml   

YAML 파일 리디렉션 후 작업  

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
