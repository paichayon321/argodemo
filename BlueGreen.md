# Openshift Blue/Green Deployment with ArgoCD and Kustomize
git clone https://github.com/paichayon321/argodemo.git

1. Checkout Git and Copy Current version of Deployment.yaml and Service.yaml to New Version Deployment-new.yaml and Service-new.yaml
   ```
   git clone https://github.com/paichayon321/argodemo.git
   cp argodemo/env/test/deployment.yaml argodemo/env/test/deployment-new.yaml
   cp argodemo/env/test/service.yaml argodemo/env/test/service-new.yaml
   ```
2. Change label and name on deployment-new.yaml
3. Change label and name on service-new.yaml
4. Update new image version in deployment.yaml
5. Modify route.yaml for incress weight to new version 10:90
6. Push update to Git


# Switch Traffice 20:80
1. Checkout Git
2. Modify route.yaml for incress weight to new version 20:80
3. Push update to Git

# Switch Traffice 50:50
1. Checkout Git
2. Modify route.yaml for incress weight to new version 50:50
3. Push update to Git

# Switch Traffice 0:100
1. Checkout Git
2. Remove deployment.yaml, service.yaml
3. Rename deploymment-new.yaml to deployment.yaml
4. Rename Service-new.yaml to service.yaml
5. Modify route.yaml for incress weight to new version 0:100
6. Push update to Git
