# 🚀 Projeto DevOps - Backend FastAPI com CI/CD Kubernetes

<div style="display: flex; justify-content: space-between; width: 100%;">
  <img src="images/icons/i-github.png" width="100"/>
  <img src="images/icons/i-python.png" width="100"/>
  <img src="images/icons/i-docker.png" width="100"/>
  <img src="images/icons/i-jenkins.png" width="100"/>
  <img src="images/icons/i-kubernetes.png" width="100"/>
</div>

## 📄 Visão Geral

Este projeto implementa um ciclo completo de DevOps para uma aplicação backend FastAPI, contemplando as etapas de desenvolvimento, conteinerização com Docker, versionamento com Git, build e deploy automatizado via Jenkins com deploy final no cluster Kubernetes local.

## 🛠️ Tecnologias Utilizadas

* Versionamento de Código: GitHub
* Linguagem: Python + FastAPI
* Container Runtime: Docker
* Registry de Imagens: Docker Hub
* Pipeline CI/CD: Jenkins
* Orquestração de Container: Kubernetes (Minikube local)

## 🗂️ Fase 1 - Preparação do Projeto

### Atividades

* Criação do repositório no GitHub
* Criação da conta no Docker Hub
* Configuração do cluster Kubernetes local (Minikube)
* Validação local da aplicação com Uvicorn

### 💻 Execução Local

### Estando na pasta do projeto /backend:

* Criação do ambiente virtual:

  ```
  python -m venv venv
  ```
* Ativação do ambiente:

  ```
  source venv/bin/activate
  ```
* Instalação das dependências:

  ```
  pip install requirements.txt
  ```

* Execução local:

  ```
  uvicorn main:app --reload
  ```

  ![UVICORN](images/1.png)

* Acesso:

  ```
  http://0.0.0.0:8000
  ```

  ![LOCAL](images/2.png)


## 🐳 Fase 2 - Conteinerização com Docker

### A partir do dockerfile criado:

### 🔨 Build da Imagem

```
 docker build -t usuario/projeto-devops:latest .
```

![BUILD](images/3.png)


### 🔬 Teste Local

```
docker run -d -p 8000:8000 usuario/projeto-devops:latest
```

ou

```
docker compose up --build
```

![LOCAL](images/2.png)

### 📤 Publicação no Docker Hub

```
docker login
```

```
docker push usuario/projeto-devops:latest
```

![PUSH](images/4.png)

## ☸️ Fase 3 - Deploy Manual no Kubernetes

### Deployment (deployment.yaml)

### 🔗 Aplicação no Cluster

```
kubectl apply -f deployment.yaml
```

![DEPLOYMENT](images/5.1.png)

### Acesso via NodePort

```
http://localhost:30001
```

![KUBERNETES](images/5.png)

## 🔄 Fase 4 - CI/CD Completo com Jenkins (Build, Push e Deploy)

### Pré-requisitos no Jenkins

* Jenkins com Docker, kubectl e kubeconfig configurados, rodando em:

  ```
  http://localhost:8080
  ```

* 🔐 Credenciais do Docker Hub armazenadas no Jenkins (dockerhub-credentials)

![CREDENTIALS](images/6.png)

* Expor a API local do Jenkins via Ngrok

  ```
  ngrok http http://localhost:8080
  ```

* 🪝 Webhook GitHub configurado apontando para:

  ```
  http://ngrok_url/github-webhook/
  ```

* Slack configurado para receber solicitações HTTP através do webhook

### Jenkinsfile

### 🔁 Fluxo Completo da Pipeline

![PIPELINE](images/7.png)

1. Git push no GitHub.
2. Webhook aciona o Jenkins.
3. Jenkins realiza o build da imagem Docker.
4. Jenkins realiza o push da imagem para o Docker Hub.
5. Trivy realiza o scan da imagem.
6. Jenkins aplica o deploy no cluster Kubernetes.
7. Chuck Norris da seu veredito final..

![FINAL](images/8.png)

## 🎯 Entrega Final Finais

* Código versionado no GitHub
* Imagem publicada no Docker Hub
* Aplicação rodando no Kubernetes via NodePort
* Pipeline Jenkins automatizada com CI/CD completo