3.Create a pod called multi-test with two containers.     
Container 1, name: one, image: nginx. env= name: alpha   
Container 2, name: two, image: busybox, command sleep 4800. env= name: beta 
----------------------------    
mutil Container 관련 문제.
https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/   

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
    env:   
    -  name: name   
       value: beta   
