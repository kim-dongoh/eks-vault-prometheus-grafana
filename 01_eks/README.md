# EKS Cluster 구성



# 1. 사전 준비 사항

## 1.1. AWS CLI Install

> https://docs.aws.amazon.com/ko_kr/cli/latest/userguide/getting-started-install.html

[Command]

```bash
sudo apt install -y unzip
```

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
```

```bash
unzip awscliv2.zip
```

```bash
sudo ./aws/install
```

```bash
aws --version
```



[Output]

```
aws-cli/2.11.6 Python/3.11.2 Linux/5.4.0-137-generic exe/x86_64.ubuntu.20 prompt/off
```



## 1.2. AWS Configration

[Command]

```bash
aws configure
```

```
AWS Access Key ID [None]: AKIA...WH5
AWS Secret Access Key [None]: 8Jep...Y/p
Default region name [None]: ap-northeast-2
Default output format [None]: <Enter>
```

```bash
aws sts get-caller-identity
```



[Output]

```
{
    "UserId": "AID...IEP",
    "Account": "070...981",
    "Arn": "arn:aws:iam::070...981:user/<Name>"
}
```



## 1.3. EKSCTL Install

> https://docs.aws.amazon.com/ko_kr/eks/latest/userguide/eksctl.html

[Command]

```bash
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
```

```bash
sudo mv /tmp/eksctl /usr/local/bin
```

```bash
eksctl version
```



[Output]

```
0.135.0
```



## 1.4. KUBECTL Install

> https://docs.aws.amazon.com/ko_kr/eks/latest/userguide/install-kubectl.html

[Command]

```bash
curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.24.10/2023-01-30/bin/linux/amd64/kubectl
```

```bash
chmod +x ./kubectl
```

```bash
mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$PATH:$HOME/bin
```

```bash
echo 'export PATH=$PATH:$HOME/bin' >> ~/.bashrc
```

```bash
kubectl version --short --client
```



[Output]

```
Flag --short has been deprecated, and will be removed in the future. The --short output will become the default.
Client Version: v1.24.10-eks-48e63af
Kustomize Version: v4.5.4
```



## 1.5. SSH Key 생성

[Command]

```bash
ssh-keygen
```

```bash
Generating public/private rsa key pair.
Enter file in which to save the key (/home/<User>/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again
```



[Output]

```
Your identification has been saved in /home/<User>/.ssh/id_rsa
Your public key has been saved in /home/<User>/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:Rhz...hQQ
The key's randomart image is:
+---[RSA 3072]----+
| ..  o.oE=o+.    |
|. ..oo...o+      |
| . ooo oo.       |
|  . + =..        |
| . o + +S        |
|  o.+....        |
| o oo+.          |
|  o.Boo. .       |
|  .*BO+.o        |
+----[SHA256]-----+
```



# 2. EKS Cluster 생성

[Command]

```bash
eksctl create cluster --name <Cluster Name> --region ap-northeast-2 --version 1.24 \
--nodegroup-name <Node Group Name> --node-type t3.medium --nodes 3 --nodes-min 3 --nodes-max 6 \
--with-oidc --node-volume-size=20 --ssh-access --ssh-public-key ~/.ssh/id_rsa.pub
```
