Step 1: Install Pre-requisites
- Instructions for installing helm on windows*********************************************
- Instructions for adding a repo to helm on windows***************************************
- helm install cert-manager jetstack/cert-manager --namespace cert-manager --create-namespace --set installCRDs=true
- Link to instructions for installing & configuring istioctl******************************
- istioctl install

Step 2: Configure Pre-Requisites
- kubectl apply -f ./framework-config/cert-manager
- Explain config for Azure and how to configure azure-secret secret***********************
- kubectl apply -f ./framework-config/istio

Step 2: Install & Configure MySQL
- kubectl apply -f  ./framework-config/mysql-config
- Setup the schemas and users, then grant authority***************************************

Step 3: Install & Configure Keycloak
- kubectl apply -f ./framework-config/keycloak-config
- Import the .json file.  instructions to follow*****************************************