# Load Balancer 구성

# 1. 사전 준비 사항

## 1.1. Helm Install

> https://helm.sh/docs/intro/install/

[Command]

```bash
curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
```

```bash
sudo apt-get install apt-transport-https --yes
```

```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] 
```

```bash
https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
```

```bash
sudo apt-get update
```

```bash
sudo apt-get install helm
```

```bash
helm version
```



[Output]

```
version.BuildInfo{Version:"v3.11.2", GitCommit:"912ebc1cd10d38d340f048efaf0abda047c3468e", GitTreeState:"clean", GoVersion:"go1.18.10"}
```



## 1.2. IAM 정책 생성

> https://docs.aws.amazon.com/ko_kr/eks/latest/userguide/aws-load-balancer-controller.html

AWS Load Balancer Controller는 Kubernetes 클러스터의 AWS Elastic Load Balancer를 관리

[Command]

```bash
curl -O https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.4.7/docs/install/iam_policy.json
```

```bash
aws iam create-policy \
    --policy-name AWSLoadBalancerControllerIAMPolicy \
    --policy-document file://iam_policy.json
```

