# Deploy a Simple Web Application Using 🐬 Docker with AWS ECR as the Private Docker Registry

### Prerequisites before pushing an image to ECR

The command docker pull yeasy/simple-web:latest is used to pull a Docker image from a Docker registry (in this case, Docker Hub) to your local machine. 
```
docker pull yeasy/simple-web:latest
```
### Expected results:
<pre>latest: Pulling from yeasy/simple-web
f2b6b4884fc8: Pull complete 
4fb899b4df21: Pull complete 
74eaa8be7221: Pull complete 
2d6e98fe4040: Pull complete 
414666f7554d: Pull complete 
bb0bcc8d7f6a: Pull complete 
ace2d3087f57: Pull complete 
da74659b9184: Pull complete 
834d72880162: Pull complete 
f05b81527a11: Pull complete 
779bb3fb81b2: Pull complete 
Digest: sha256:356de309052fe233ba08eb4c9ad85ab89398f31555e8777326d57307ac913727
Status: Downloaded newer image for yeasy/simple-web:latest
docker.io/yeasy/simple-web:latest

lawi@sys76:~$ docker images
REPOSITORY           TAG       IMAGE ID       CREATED        SIZE
hashicorp/vault      1.19      ffe2f6cea17f   2 weeks ago    503MB
hashicorp/boundary   0.19      8073fa2c1d5b   7 weeks ago    252MB
nginx                latest    4cad75abc83d   2 months ago   192MB
yeasy/simple-web     latest    172c78152bf6   7 years ago    679MB
</pre>


### 1. Creating an Amazon ECR private repository to store images

![image alt](https://github.com/minlawi/aws-ecr-private/blob/2d11b9b3520b1a0321bef554c5ec92ca5c213dee/Screenshot%20from%202025-04-20%2011-20-50.png)

![image lat](https://github.com/minlawi/aws-ecr-private/blob/2d11b9b3520b1a0321bef554c5ec92ca5c213dee/Screenshot%20from%202025-04-20%2011-19-53.png)

### 2. To authenticate Docker to an Amazon ECR private registry with get-login
* To authenticate Docker to an Amazon ECR registry with get-login-password, run the aws ecr get-login-password command. When passing the authentication token to the docker login command, use the value AWS for the username and specify the Amazon ECR registry URI you want to authenticate to. If authenticating to multiple registries, you must repeat the command for each registry.

## Important Notes

When working with AWS ECR (Elastic Container Registry), **make sure to replace the placeholders** in the following examples with your actual values:

- **`your_region`**: The AWS region where your ECR repository is located (e.g., `us-east-1`, `ap-northeast-1`).
- **`your_aws_account_id`**: Your AWS account ID, which you can find in your AWS Console.
- **`your_region.amazonaws.com`**: The full registry URL, which will look like `your_aws_account_id.dkr.ecr.your_region.amazonaws.com`.

### Example Command:

<pre>
aws ecr get-login-password --region your_region --profile your_profile_name | docker login --username AWS --password-stdin your_aws_account_id.dkr.ecr.your_region.amazonaws.com
</pre>
Important: Replace your_profile, your_region and your_aws_account_id.dkr.ecr.your_region.amazonaws.com with your actual region and AWS account ID when running the above command.
```
aws ecr get-login-password --region ap-southeast-1 --profile master-programmatic-admin | docker login --username AWS --password-stdin 571600835849.dkr.ecr.ap-southeast-1.amazonaws.com
```
### Expected result:
<pre>
lawi@sys76:~$ aws ecr get-login-password --region ap-southeast-1 --profile master-programmatic-admin | docker login --username AWS --password-stdin 571600835849.dkr.ecr.ap-southeast-1.amazonaws.com
Login Succeeded
</pre>

### 3. Tag Docker Image for Amazon ECR
This command is used to tag a Docker image with a new name, usually to prepare it for pushing to a specific Amazon Elastic Container Registry (ECR).
```
docker tag yeasy/simple-web:latest 571600835849.dkr.ecr.ap-southeast-1.amazonaws.com/yeasy/simple-web:latest
```
### Expected result:
<pre>lawi@sys76:~$ docker images
REPOSITORY                                                           TAG       IMAGE ID       CREATED        SIZE
hashicorp/vault                                                      1.19      ffe2f6cea17f   2 weeks ago    503MB
hashicorp/boundary                                                   0.19      8073fa2c1d5b   7 weeks ago    252MB
nginx                                                                latest    4cad75abc83d   2 months ago   192MB
yeasy/simple-web                                                     latest    172c78152bf6   7 years ago    679MB
571600835849.dkr.ecr.ap-southeast-1.amazonaws.com/yeasy/simple-web   latest    172c78152bf6   7 years ago    679MB
</pre>

### 4. Push Docker Image to Amazon ECR
The command is used to push a Docker image to an Amazon Elastic Container Registry (ECR) repository.
```
docker push 571600835849.dkr.ecr.ap-southeast-1.amazonaws.com/yeasy/simple-web:latest
```
### Expected result:
<pre>lawi@sys76:~$ docker push 571600835849.dkr.ecr.ap-southeast-1.amazonaws.com/yeasy/simple-web:latest
The push refers to repository [571600835849.dkr.ecr.ap-southeast-1.amazonaws.com/yeasy/simple-web]
bc8c0c984b54: Pushed 
a36d433a3808: Pushed 
6bae46c6ee76: Pushed 
6bdccf632521: Pushed 
62d47657687c: Pushed 
4e32c2de91a6: Pushed 
6e1b48dc2ccc: Pushed 
ff57bdb79ac8: Pushed 
6e5e20cbf4a7: Pushed 
86985c679800: Pushed 
8fad67424c4e: Pushed 
latest: digest: sha256:356de309052fe233ba08eb4c9ad85ab89398f31555e8777326d57307ac913727 size: 2633
</pre>

![image alt](https://github.com/minlawi/aws-ecr-private/blob/a60de3c7aa07bfa04ea318402bc26b773c41e75d/Screenshot%20from%202025-04-20%2011-26-10.png)

### 5. Build docker compose file 🐬
```
services:
  yeasy-web-1:
    image: 571600835849.dkr.ecr.ap-southeast-1.amazonaws.com/yeasy/simple-web:latest
    hostname: yeasy-web-1
    container_name: yeasy-web-1
    networks:
      - nginx-proxy-nw

  yeasy-web-2:
    image: 571600835849.dkr.ecr.ap-southeast-1.amazonaws.com/yeasy/simple-web:latest
    hostname: yeasy-web-2
    container_name: yeasy-web-2
    networks:
      - nginx-proxy-nw
    
networks:
  nginx-proxy-nw:
    driver: bridge
```
