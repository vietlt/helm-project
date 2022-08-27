Provision the infrastructure with Terraform
‘terraform init’
 ![image](https://user-images.githubusercontent.com/95838672/187014645-421dbc96-0fc2-4e64-ba77-bb23eb5bcf28.png)
‘terraform apply’
 ![image](https://user-images.githubusercontent.com/95838672/187014650-53c8c5e7-70bd-4201-8497-96f5fb633b3e.png)
 ![image](https://user-images.githubusercontent.com/95838672/187014663-74599ed3-e5a8-42b6-93b0-22bada6bca29.png)
*The whole provisioning process may take more than 20 minutes.




Confirm terraform with EKS cluster 2 worker nodes on the public subnet
 ![image](https://user-images.githubusercontent.com/95838672/187014668-43340583-f0a1-4c5f-b209-60c1a6383e57.png)
Package ChatApp application to a Docker image and upload to the container registry ECR 
‘aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin 817735295857.dkr.ecr.ap-southeast-1.amazonaws.com’
 ![image](https://user-images.githubusercontent.com/95838672/187014677-0062e5dc-3076-48ce-ad20-1816003b0c92.png)
Change dir into folder main and run ‘docker build -t chatapp .’
 ![image](https://user-images.githubusercontent.com/95838672/187014709-90c324ae-eb25-4c4d-9e6e-0556f16037b3.png)
 ![image](https://user-images.githubusercontent.com/95838672/187014718-ab92c788-e89f-4cb0-b8cb-a3ff2553bdb6.png)
‘docker tag chatapp:latest 817735295857.dkr.ecr.ap-southeast-1.amazonaws.com/demo-repo:latest’
‘docker push 817735295857.dkr.ecr.ap-southeast-1.amazonaws.com/demo-repo:latest’
 ![image](https://user-images.githubusercontent.com/95838672/187014721-965bc9a0-3ca2-4dec-9f75-72f89254634d.png)
Confirm the image had been pushed to the ECR repository.
 ![image](https://user-images.githubusercontent.com/95838672/187014724-28c5f4b2-8166-4705-bfa0-0126b82d7fcd.png)




Create chart repository
Change dir back to the outside directory, then run commands below
‘helm create chatapp’
‘helm create mysql’
 ![image](https://user-images.githubusercontent.com/95838672/187014733-68b6e3db-927e-43cf-8fd6-ff365e047bd2.png)
Write chart files and deploy the application on EKS
Make necessary configuration to the values.yaml and development.yaml files in each chart repository
 ![image](https://user-images.githubusercontent.com/95838672/187014741-a8778cb6-425e-4dd0-be69-0e2197d6aeb2.png)
![image](https://user-images.githubusercontent.com/95838672/187014746-2fa14be3-0940-4282-b4db-d273afe43d74.png)
Update config to connect to aws eks
‘aws eks --region ap-southeast-1 update-kubeconfig --name k8s’
 ![image](https://user-images.githubusercontent.com/95838672/187014753-056a65c1-0f4c-4884-bcea-e413d5e5c0a6.png)
Deploying the application using Helm chart
‘helm install mysql mysql/’
 ![image](https://user-images.githubusercontent.com/95838672/187014757-eedf8c80-1b4e-4719-9880-6a819f306aab.png)
Confirm chart has been deploy
 ![image](https://user-images.githubusercontent.com/95838672/187014758-cd88cfc0-8333-469d-82dd-8981bd83197e.png)
Run ‘helm install chatapp chatapp/’
 ![image](https://user-images.githubusercontent.com/95838672/187014760-89507027-0466-4c51-8ae4-13bc452aa364.png)
 ![image](https://user-images.githubusercontent.com/95838672/187014766-c5198562-9bb5-4bd4-b9e9-a40a96d65947.png)




Showing the application works
Get load balancer
 ![image](https://user-images.githubusercontent.com/95838672/187014773-948d7d5a-2c31-4f6e-854d-542435115e62.png)
Show the result
 ![image](https://user-images.githubusercontent.com/95838672/187014776-1d31d9b5-a84e-4099-a3d3-3f13358705b6.png)
Packaging chart and pushing to the repository
‘helm package ./chatapp/ -d charts/’
‘helm package ./mysql/ -d charts/’
 ![image](https://user-images.githubusercontent.com/95838672/187014781-8335d031-ee35-4b6c-8315-1890cb88c3f8.png)
Upload to artifacthub.io
 ![image](https://user-images.githubusercontent.com/95838672/187014784-23c18a59-d435-4ab2-b22f-98ca402a8df4.png)
