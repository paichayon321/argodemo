# Openshift Blue/Green Deployment with ArgoCD
git clone https://github.com/paichayon321/argodemo.git

   1. Checkout Git and Copy Current version of Deployment.yaml and Service.yaml to New Version Deployment-new.yaml and Service-new.yaml
   
   ```
   git clone https://github.com/paichayon321/argodemo.git
   cp argodemo/env/test/deployment.yaml argodemo/env/test/deployment-new.yaml
   cp argodemo/env/test/service.yaml argodemo/env/test/service-new.yaml
   ```
   
   2. Change label and name on deployment-new.yaml
   ```
   yq -i '.metadata.name = "httpd-frontend-new"' argodemo/env/test/deployment-new.yaml
   yq -i '.metadata.labels.app = "httpd-frontend-new"' argodemo/env/test/deployment-new.yaml
   yq -i '.spec.selector.matchLabels.app = "httpd-frontend-new"' argodemo/env/test/deployment-new.yaml
   yq -i '.spec.template.metadata.labels.app = "httpd-frontend-new"' argodemo/env/test/deployment-new.yaml
   ```

   3. Change label and name on service-new.yaml

   ```
   yq -i '.metadata.name = "httpd-frontend-new"' argodemo/env/test/service-new.yaml
   yq -i '.metadata.labels.app = "httpd-frontend-new"' argodemo/env/test/service-new.yaml 
   yq -i '.spec.selector.app = "httpd-frontend-new"' argodemo/env/test/service-new.yaml
   ```

   5. Update new image version in deployment.yaml
   ```
   #yq -i '.spec.template.spec.containers[0].image = "registry.redhat.io/rhel8/httpd-24"' argodemo/env/test/deployment-new.yaml
   yq -i '.spec.template.spec.containers[0].image = "ghcr.io/rhobs/prometheus-example-app:0.4.1"' argodemo/env/test/deployment-new.yaml
   ```
   

   6. Modify route.yaml for incress weight to new version 10:90
   ```
   yq -i '.spec.to.weight = 90' argodemo/env/test/route.yaml
   yq -i '.spec.alternateBackends = [{"kind": "Service", "name": "httpd-frontend-new", "weight": 10}]' argodemo/env/test/route.yaml

   ```
   

   7. Push update to Git
   ```
   git add .
   git commit -m "Update BlueGreen"
   git push
   ```
      


# Switch Traffice 20:80
1. Checkout Git
2. Modify route.yaml for incress weight to new version 20:80
   ```
   yq -i '.spec.to.weight = 80' argodemo/env/test/route.yaml
   yq -i '.spec.alternateBackends = [{"kind": "Service", "name": "httpd-frontend-new", "weight": 20}]' argodemo/env/test/route.yaml
   ```
4. Push update to Git

# Switch Traffice 50:50
1. Checkout Git
   ```
   yq -i '.spec.to.weight = 50' argodemo/env/test/route.yaml
   yq -i '.spec.alternateBackends = [{"kind": "Service", "name": "httpd-frontend-new", "weight": 50}]' argodemo/env/test/route.yaml
   ```   
3. Modify route.yaml for incress weight to new version 50:50
4. Push update to Git

# Switch Traffice 0:100
1. Checkout Git
2. Remove deployment.yaml, service.yaml
   ```
   rm argodemo/env/test/deployment.yaml
   rm argodemo/env/test/service.yaml

   ```
4. Rename deploymment-new.yaml to deployment.yaml
   ```
   mv argodemo/env/test/deployment-new.yaml argodemo/env/test/deployment.yaml
   ```
    
6. Rename Service-new.yaml to service.yaml
   ```
   mv argodemo/env/test/service-new.yaml argodemo/env/test/service.yaml
   ```

   
7. Rename deployment
   ```
   yq -i '.metadata.name = "httpd-frontend"' argodemo/env/test/deployment.yaml
   yq -i '.metadata.labels.app = "httpd-frontend"' argodemo/env/test/deployment.yaml
   yq -i '.spec.selector.matchLabels.app = "httpd-frontend"' argodemo/env/test/deployment.yaml
   yq -i '.spec.template.metadata.labels.app = "httpd-frontend"' argodemo/env/test/deployment.yaml
   ```
   
9. Rename service
   ```
   yq -i '.metadata.name = "httpd-frontend"' argodemo/env/test/service.yaml
   yq -i '.metadata.labels.app = "httpd-frontend"' argodemo/env/test/service.yaml
   yq -i '.spec.selector.app = "httpd-frontend"' argodemo/env/test/service.yaml
   ```
    
10. Modify route.yaml weight to 100 and remove alternateBackends
    ```
    yq -i '.spec.to.weight = 100' argodemo/env/test/route.yaml
    yq -i 'del(.spec.alternateBackends)' argodemo/env/test/route.yaml
    ```
    
11. Push update to Git

---
Full Script Here
```
# BlueGreen Deployment with yq

git clone https://github.com/paichayon321/argodemo.git

# Duplicate Deployment and service
cp argodemo/env/test/deployment.yaml argodemo/env/test/deployment-new.yaml
cp argodemo/env/test/service.yaml argodemo/env/test/service-new.yaml

# Change to new deployment name
yq -i '.metadata.name = "httpd-frontend-new"' argodemo/env/test/deployment-new.yaml
yq -i '.metadata.labels.app = "httpd-frontend-new"' argodemo/env/test/deployment-new.yaml
yq -i '.spec.selector.matchLabels.app = "httpd-frontend-new"' argodemo/env/test/deployment-new.yaml
yq -i '.spec.template.metadata.labels.app = "httpd-frontend-new"' argodemo/env/test/deployment-new.yaml

# Change to new service name
yq -i '.metadata.name = "httpd-frontend-new"' argodemo/env/test/service-new.yaml
yq -i '.metadata.labels.app = "httpd-frontend-new"' argodemo/env/test/service-new.yaml
yq -i '.spec.selector.app = "httpd-frontend-new"' argodemo/env/test/service-new.yaml


# Change Image 
yq -i '.spec.template.spec.containers[0].image = "registry.redhat.io/rhel8/httpd-24"' argodemo/env/test/deployment-new.yaml


# Set 10:90

yq -i '.spec.to.weight = 90' argodemo/env/test/route.yaml
yq -i '.spec.alternateBackends = [{"kind": "Service", "name": "httpd-frontend-new", "weight": 10}]' argodemo/env/test/route.yaml



# 20:80

yq -i '.spec.to.weight = 80' argodemo/env/test/route.yaml
yq -i '.spec.alternateBackends = [{"kind": "Service", "name": "httpd-frontend-new", "weight": 20}]' argodemo/env/test/route.yaml


# 50:50

yq -i '.spec.to.weight = 50' argodemo/env/test/route.yaml
yq -i '.spec.alternateBackends = [{"kind": "Service", "name": "httpd-frontend-new", "weight": 50}]' argodemo/env/test/route.yaml


# 0:100

rm argodemo/env/test/deployment.yaml
rm argodemo/env/test/service.yaml
mv argodemo/env/test/deployment-new.yaml argodemo/env/test/deployment.yaml
mv argodemo/env/test/service-new.yaml argodemo/env/test/service.yaml

yq -i '.metadata.name = "httpd-frontend"' argodemo/env/test/deployment.yaml
yq -i '.metadata.labels.app = "httpd-frontend"' argodemo/env/test/deployment.yaml
yq -i '.spec.selector.matchLabels.app = "httpd-frontend"' argodemo/env/test/deployment.yaml
yq -i '.spec.template.metadata.labels.app = "httpd-frontend"' argodemo/env/test/deployment.yaml


yq -i '.metadata.name = "httpd-frontend"' argodemo/env/test/service.yaml
yq -i '.metadata.labels.app = "httpd-frontend"' argodemo/env/test/service.yaml
yq -i '.spec.selector.app = "httpd-frontend"' argodemo/env/test/service.yaml


yq -i '.spec.to.weight = 100' argodemo/env/test/route.yaml
yq -i 'del(.spec.alternateBackends)' argodemo/env/test/route.yaml

```
