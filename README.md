# k8s-redmine-2021

### Install
```sh
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
helm install redmine bitnami/redmine --namespace public-service --set master.persistence.storageClass=oci --set service.type=NodePort
kubectl get secret --namespace public-service redmine-redmine -o jsonpath={.data.redmine-password} | base64 --decode 
```

### NOTES:
1. Get the Redmine URL:

  NOTE: It may take a few minutes for the LoadBalancer IP to be available.
        Watch the status with: 'kubectl get svc --namespace public-service -w redmine'


  export SERVICE_IP=$(kubectl get svc --namespace public-service redmine --template "{{ range (index .status.loadBalancer.ingress 0) }}{{ . }}{{ end }}")
  echo "Redmine URL: http://$SERVICE_IP/"

2. Login with the following credentials

  export REDMINE_USERNAME=user
  export REDMINE_PASSWORD=$(kubectl get secret --namespace public-service redmine -o jsonpath="{.data.redmine-password}" | base64 --decode)

  echo Username: $REDMINE_USERNAME
  echo Password: $REDMINE_PASSWORD

   You can access the DB using the following password:
   export MARIADB_PASSWORD=$(kubectl get secret --namespace public-service redmine-mariadb -o jsonpath="{.data.mariadb-password}" | base64 --decode)
