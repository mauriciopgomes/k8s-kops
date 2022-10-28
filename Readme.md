# Cluster Kubernetes

___

## Create Cluster

Export S3 Bucket

    export KOPS_STATE_STORE=s3://k8s-mauriciogomes

Create Cluster Single-Master

    kops create cluster --zones=us-east-1c k8s.mauriciogomes.com.br

Create Cluster Multi-Master

    kops create cluster \
    --node-count 3 \
    --zones us-east-1a,us-east-1b,us-east-1c \
    --master-zones us-east-1a,us-east-1b,us-east-1c \
    k8s.mauriciogomes.com.br

Edit Cluster to Add Metrics

    kops edit cluster k8s.mauriciogomes.com.br

Add this options

    spec:
      containerRuntime: containerd
      kubelet:
        anonymousAuth: false 
        authenticationTokenWebhook: true
        authorizationMode: Webhook

Run create command script

    kops update cluster --name k8s.mauriciogomes.com.br --yes

Wait until ready

    kops validate cluster --wait 10m

Create Cert Manager

    kubectl apply -f k8s/00-cert-manager.yaml
    kubectl apply -f k8s/01-cert-manager.crds.yaml
    kubectl apply -f k8s/02-letsencrypt.yaml

Create Ingress Nginx

    kubectl apply -f k8s/03-ingress-nginx.yaml

Create Metric Server

    kubectl apply -f k8s/04-metric-server.yaml

## Delete Cluster

Export S3 Bucket

    export KOPS_STATE_STORE=s3://k8s-mauriciogomes

Delete cluster command

    kops delete cluster k8s.mauriciogomes.com.br --yes
    
    
##O que é gerenciamento de clusters do Kubernetes?
##O gerenciamento de cluster Kubernetes é a forma como uma equipe de TI gerencia um grupo de clusters do Kubernetes. 

##Com as aplicações nativas em nuvem modernas, os ambientes do Kubernetes estão se tornando altamente distribuídos. Eles podem ser implantados em vários datacenters ##on-premise, na nuvem pública e na borda.

##As organizações que optarem por usar o Kubernetes em escala ou na produção terão vários clusters, por exemplo, para desenvolvimento, teste e produção, distribuídos ##dentre os ambientes. Assim, elas precisarão ser capazes de gerenciá-los de maneira efetiva.
