# Passo a passo criação e configuração EKS

## 1 - Criação da VPC

- Para realizar a criação da vpc que será usada pelo aks pode ser utilizado o template do CloudFormation. Segue o link:
    - https://s3.us-west-2.amazonaws.com/amazon-eks/cloudformation/2020-10-29/amazon-eks-vpc-private-subnets.yaml

## 2 - Criando roles para acesso ao EKS.

Após a criação(ou durante) da VPC precisamos criar roles para permitir nosso acesso ao EKS. Precisamos criar duas roles que nomeei da seguinte forma:
- cluster-eks: com a permissão AmazonEKSClusterPolicy
- ec2-eks-role: com as permissões AmazonEKSWorkerNodePolicy, AmazonEKS_CNI_Policy e AmazonEC2ContainerRegistryReadOnly.

## 3 - Criando o EKS

- Realizei a criação do eks com as configurações default, ficando atento nos seguintes passos:
    - Configuração da vpc. Utilizar a vpc criada
    - Configuração da role. Utilizar a role cluster-eks devido as suas permissões de administração do eks.
- O eks leva entre 10 e 15 minutos para sua criação
- Após a criação do eks, é necessário configurar os nodes na guia Compute - Add node group
- Utilizei duas máquinas EC2 t3.xlarger
- Ter atenção na role, adicionando a role ec2-eks-role, criada anteriormente
- Escolher o tier das máquinas EC2
- Escolher as subnets, já vem configuradas como default

## 4 - Acessando o cluster EKS

Para acessar o cluster necessário realizar a configuração do kubeconfig na máquina local:
- aws eks update-kubeconfig --name <nome do eks>

- testando o acesso: kubectl get nodes

## 5 - Instalando o Terraform

- Para instalar o terraform no Windows ou Linux pode seguir o passo a passo o link abaixo:
    https://spacelift.io/blog/how-to-install-terraform

## 6 - Executando o Terraform

- No diretório do projeto, dentro do diretório terraform executar os seguintes comandos:
    - terraform init: inicializar o terraform
    - terraform validate: validar a sintaxe do terraform (Opcional)
    - terraform plan -out plan.out: planeja o que vai ser deployado e gera um arquivo de plano
    - terraform apply plan.out: aplica o que foi planejado realizando o deploy
