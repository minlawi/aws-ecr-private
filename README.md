# Prerequisites before pushing an image to ECR

The command docker pull yeasy/simple-web:latest is used to pull a Docker image from a Docker registry (in this case, Docker Hub) to your local machine. 
```
docker pull yeasy/simple-web:latest
```
Expected results:
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
The command docker tag yeasy/simple-web private-ecr/simple-web is used to tag a Docker image with a new name, typically to prepare it for pushing to a private Docker registry, like Amazon ECR (Elastic Container Registry).
```
docker tag yeasy/simple-web private-ecr/simple-web
```
Expected result:
<pre>lawi@sys76:~$ docker images
REPOSITORY               TAG       IMAGE ID       CREATED        SIZE
hashicorp/vault          1.19      ffe2f6cea17f   2 weeks ago    503MB
hashicorp/boundary       0.19      8073fa2c1d5b   7 weeks ago    252MB
nginx                    latest    4cad75abc83d   2 months ago   192MB
private-ecr/simple-web   latest    172c78152bf6   7 years ago    679MB
yeasy/simple-web         latest    172c78152bf6   7 years ago    679MB
</pre>

# 1. Creating an Amazon ECR private repository to store images

![image alt](https://github.com/minlawi/aws-ecr-private/blob/2499f526a9b43b0aeeee7edab33703dd4d019dcd/Screenshot%20from%202025-04-20%2010-05-54.png)

![image lat](https://github.com/minlawi/aws-ecr-private/blob/280fcf865986e08fae5c9e4ee1233cac9c735398/Screenshot%20from%202025-04-20%2010-09-39.png)
