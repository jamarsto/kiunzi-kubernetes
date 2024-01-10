Step 1: Install Pre-requisites
- 
1. Install chocolatey
```
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```
2. Install helm
```
choco install kubernetes-helm
```
3. Add cert-manager repo to helm
```
helm repo add jetstack https://charts.jetstack.io
helm repo update
```
4. Install cert-manager
```
helm install cert-manager jetstack/cert-manager --namespace cert-manager --create-namespace --set installCRDs=true
```
5. Install istioctl
    - Download istioctl zip file from https://storage.googleapis.com/istio-release/releases/1.20.1/istio-1.20.1-win.zip
    - Extract the zip and add the bin directory to your path
6. Install istio
```
istioctl install
```

Step 2: Configure Pre-Requisites
- 
1. Provide your own values for the following placeholders in istio-system-azure-secret.yaml, and istio-system-letsencrypt-issuer.yaml as explained in https://cert-manager.io/docs/configuration/acme/dns01/azuredns/
    - <your_azure_client_secret>
    - <your_email_address>
    - <your_azure_client_id>
    - <your_azure_subscription_id>
    - <your_azure_tenant_id>
    - <your_dns_zone>
2. Configure cert-manager
```
kubectl apply -f ./framework-config/cert-manager
```
3. Configure istio
```
kubectl apply -f ./framework-config/istio
```  

Step 3: Install & Configure MySQL
- 
1. Provide your own values for the following placeholders in default-mysql-secrets.yaml, istio-system-keycloak-gateway.yaml, istio-system-mysql-virtualservice.yaml, and istio-system-keycloak-secrets.yaml
    - <mysql_root_password>
    - <your_keycloak_db_user>
    - <your_keycloak_db_password>
    - <your_keycloak_hostname>
2. Configure MySQL
```
kubectl apply -f  ./framework-config/mysql-config
```
3. Use a client like MySQL workbench to connect to your mysql instance on hostname <your_keyloak_hostname> on port 3306 and execute the following to create the user and schema for keyloak
```
CREATE USER '<your_keycloak_db_user>'@'%' IDENTIFIED BY '<your_keycloak_db_password>';
CREATE DATABASE keycloak CHARACTER SET utf8 COLLATE utf8_unicode_ci;
GRANT ALL PRIVILEGES ON quarkus.* TO '<your_keycloak_db_user>'@'%';
FLUSH PRIVILEGES;
```

Step 4: Install & Configure Keycloak
- 
1. Provide your own values for the following placeholders in default-keycloak-secrets.yaml, istio-system-keycloak-gateway.yaml, istio-system-keycloak-virtualservice.yaml, and istio-system-keycloak-secrets.yaml
    - <your_keycloak_hostname>
    - <your_keycloak_admin_username>
    - <your_keycloak_admin_password>
    - <your_keycloak_db_user>
    - <your_keycloak_db_password>
2. Configure Keycloak (The keycloak database could take several minutes to be fully initialised)
```
kubectl apply -f  ./framework-config/keycloak-config
```
3. Login to keycloak and import the quarkus-realm.json file
