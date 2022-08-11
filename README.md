# nginx-ingress-controller-with-eks
This repo walks you through how to enable cookie-based sticky session with Nginx Ingress Controller on Amazon EKS.


## 1. Create an EKS cluster in a specific region
eksctl create cluster --region ap-east-1

## 2. Deploy Nginx Ingress Controller 
kubectl apply -f ./nginx-ingress-controller.yaml

## 3. Deploy the sample app deployment and service
kubectl apply -f ./deployment.yaml && kubectl apply -f ./svc.yaml

## 4. Verify that the sticky session is working
for i in {0..5}; do curl --cookie cookie.txt --cookie-jar cookie.txt http://ababd4e58fc4b4cde933c52ebd007efc-e3ff84bdc1aef180.elb.ap-east-1.amazonaws.com/; echo ""; done

The output of the above command should return something like this(showing that each curl request with the same cookie value is served by the same pod): 
Hello "what-is-my-pod-deployment-666d84dbf5-zmhcr"!
Hello "what-is-my-pod-deployment-666d84dbf5-zmhcr"!
Hello "what-is-my-pod-deployment-666d84dbf5-zmhcr"!
Hello "what-is-my-pod-deployment-666d84dbf5-zmhcr"!
Hello "what-is-my-pod-deployment-666d84dbf5-zmhcr"!
Hello "what-is-my-pod-deployment-666d84dbf5-zmhcr"!
