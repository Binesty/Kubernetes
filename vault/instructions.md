## Install Helm chart

create namespace

````bash
kubectl create namespace vault
````

Vault state is inactive because it is unsealed 

## Initialize vault production

````bash
kubectl exec -ti vault-0 -n vault -- vault operator init
````

output
````bash
Unseal Key 1: [key-1]
Unseal Key 2: [key-2]
Unseal Key 3: [key-3]
Unseal Key 4: [key-4]
Unseal Key 5: [key-5]

Initial Root Token: [token]
````

## Unseal vault

use keys to sealed vault

Key-1
````bash
kubectl exec -ti [vault-pod] -n vault -- vault operator unseal [key-1]
````

Key-2
````bash
kubectl exec -ti [vault-pod] -n vault -- vault operator unseal [key-2]
````

Key-3
````bash
kubectl exec -ti [vault-pod] -n vault -- vault operator unseal [key-3]
````

## Create service and ingress to access vault

````yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  annotations:    
    kubernetes.io/ingress.class: nginx    
    nginx.ingress.kubernetes.io/rewrite-target: /
  name: vault-ingress
  namespace: vault
spec:
  rules:
  - host: vault.binesty.net
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: [vault-service]
            port:
              number: 8200
````

## Access webapp development

Access: http://vault.binesty.net/ui  
Initial Root Token: hvs.WhCerZdt30LAcCvf27CJXSCK