3.Create a pod called multi-test with two containers.     
Container 1, name: one, image: nginx. env= name: alpha   
Container 2, name: two, image: busybox, command sleep 4800. env= name: beta 
----------------------------    
mutil Container 관련 문제.

https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/   

#apiVersion: v1
kind: Pod
metadata:
  name: envar-demo
  labels:
    purpose: demonstrate-envars
spec:
  containers:
  - name: envar-demo-container
    image: gcr.io/google-samples/node-hello:1.0
    env:
    - name: DEMO_GREETING
      value: "Hello from the environment"
    - name: DEMO_FAREWELL
      value: "Such a sweet sorrow"

