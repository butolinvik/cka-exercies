###
```bash
kubectl create -f https://raw.githubusercontent.com/killer-sh/cks-course-environment/master/course-content/opa/gatekeeper.yaml
k get pod,svc -n gatekeeper-system 
#Deny all Policy

# Add to kube-apiserver.yaml
https://play.openpolicyagent.org/
https://github.com/BouweCeunen/gatekeeper-policies
