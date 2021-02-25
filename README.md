## Clone the Craftdemo Repository
```
git clone https://github.com/Neru007/craftdemo.git
```

# Building Docker Image and Pushing it to dockerhub

Build the image using the following command

```bash
$ cd craftdemo/app
```

```bash
$ docker build -t neru007/craftdemo:v1 .
```

Run the Docker container using the command shown below.

```bash
$ docker run -d -p 5000:5000 craftdemo
```

The application will be accessible at http://127.0.0.1:5000

Push the Application to dockerhub 

```bash
$ docker push neru007/craftdemo:v1
```


# Setting up EKS cluster using Terraform and Deploying Python Flask Application that is created using above steps.




## Amazon CLI

You can get the Amazon CLI on [Docker-Hub](https://hub.docker.com/r/amazon/aws-cli) <br/>
We'll need the Amazon CLI to gather information so we can build our Terraform file.

```
# Run Amazon CLI
docker run -it --rm -v ${PWD}:/craftdemo -w /craftdemo --entrypoint /bin/sh amazon/aws-cli:2.0.43

# some handy tools :)
yum install -y tar git unzip wget

```

## Login to Amazon

```
# Access your "My Security Credentials" section in your profile. 
# Create an access key

aws configure

Default region name: ap-southeast-2
Default output format: json
```

# Terraform CLI 

```
# Get Terraform

curl -o /tmp/terraform.zip -LO https://releases.hashicorp.com/terraform/0.14.7/terraform_0.14.7_linux_amd64.zip
unzip /tmp/terraform.zip
chmod +x terraform && mv terraform /usr/local/bin/
terraform
```

## Terraform Amazon Kubernetes Provider 

```
cd craftdemo/infra/

terraform init

terraform plan
terraform apply

```

As soon as cluster is ready, you should find a kubeconfig_{clustername} kubeconfig file in the current directory.


## Install AWS IAM authenticator so that Kubernetes can authenticate to cluster via IAM

```bash
curl -o aws-iam-authenticator https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/aws-iam-authenticator
chmod +x ./aws-iam-authenticator
mv aws-iam-authenticator /usr/local/bin/
```

# Deploy Kubectl and Sample Application that is created by docker file in app folder

```


# Get kubectl

curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
chmod +x ./kubectl
mv ./kubectl /usr/local/bin/kubectl


export KUBECONFIG=${PWD}/kubeconfig_{clustername}

kubectl apply -f ../manifest

kubectl get nodes
kubectl get deploy -n craftdemo
kubectl get pods -n craftdemo
kubectl get svc -n craftdemo
kubectl get ingress -n craftdemo
```

Application will be available over ingress 


# Clean up 

```
terraform destroy
```
