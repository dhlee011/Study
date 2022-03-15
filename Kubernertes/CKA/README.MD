1.Create a new service account with the name Fest.   
Grant this Service account access to list all PersistentVolumes in the cluster by creating an   
appropriate cluster role called Fest-role and ClusterRoleBinding called Fest-role-binding.   
Next, create a pod called Fest with the image: redis and serviceAccount: Fest in the default namespace   
------   

참고 문서:    
https://kubernetes.io/docs/reference/access-authn-authz/rbac/   
https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/   
          
1.Fest 라는 이름의 Service account 생성.   

2.PersistentVolumes list에 접근할 수 있는 Fest-role 이라는 ClusterRole 생성.   

3.Fest(Service account)와 Fest-role(ClusterRole)을 바인딩하는, Fest-role-binding 이라는 ClusterRoleBinding 생성.   

4.redis 이미지를 사용하고 Fest serviceAccount를 사용하는 Fest 라는 파드를 default namespae에 생성.    

     
---   
kubectl create serviceaccount Fest   

kubectl create clusterrole Fest-role --verb=list --resource=pv   

kubectl create clusterrolebinding Fest-role-binding --clusterrole=Fest-role --serviceaccount=default:Fest   

kubectl run Fest --image=redis --dry-run=client -o yaml > Fest.yaml

Fest.yaml
---
apiVersion: v1   
kind: Pod   
metadata:   
  name: Fest   
spec:   
  containers:   
  - name: Fest      
    image: redis      
  serviceAccountName: Fest   
  automountServiceAccountToken: false   
   
   
============================================================================================      
   
2.List the InternalIP of all nodes of the cluster. Save the result to a file /root/CKA/node_ips   
Answer should be in the format: InternalIP of master<space>InternalIP of node1<space>InternalIP of node2<space>InternalIP of node3 (in a single line)   
---
참고문서 :
https://kubernetes.io/docs/reference/kubectl/cheatsheet/   

Cluster Nodes의 IP list를 /root/CKA/node_ips에 InternalIP of master<space>InternalIP 형식으로 저장. Jasonpath 방식으로 출력.
       
---
          
kubectl get nodes -o jsonpath='{.items[*].status.addresses[?(@.type=="InternalIP")].address}' > /root/CKA/node_ips   
          

============================================================================================
     
3.Create a pod called multi-test with two containers.     
Container 1, name: one, image: nginx. env= name: alpha   
Container 2, name: two, image: busybox, command sleep 4800. env= name: beta 
----------------------------    

참고문서 :   
https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/    

mutil Container 관련 문제.   

YAML 파일 리디렉션 후 작업. 하나의 파드 안에 두개의 컨테이너 정의.   

환경변수 설정시  'name=alpha'처럼 설정하기 위해선 name 부분과 value 값을 따로 정의해야 한다.   

---


kubectl run multi-test --image=nginx --dry-run=client -o yaml > mult-test.yaml   

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
          
         
============================================================================================   
          
4.Create a Pod called non-pod , image: redis:alpine   
runAsUser: 1000   
fsGroup: 2000   
---   
참고문서 :   
https://kubernetes.io/docs/concepts/policy/pod-security-policy/   
https://kubernetes.io/blog/2020/12/14/kubernetes-release-1.20-fsgroupchangepolicy-fsgrouppolicy/   
          
Pod Security Policy 문제.   
   
---   
kubectl run non-pod --image=redis:alpine --dry-run=client -o yaml > non-pod.yaml   

non-pod.yaml   
---   
apiVersion: v1   
kind: Pod   
metadata:   
  name: non-pod   
spec:   
  containers:    
  - name: non-pod   
    image: redis:alpine   
  securityContext:   
    runAsUser: 1000      
    fsGroup: 2000   
