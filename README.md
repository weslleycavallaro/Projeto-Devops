# Projeto-Devops-Jenkins

FASES DO PROJETO:

FASE 1

    Criar um repositório de código no Github para inserir a aplicação de exemplo;

    Criar conta no Docker Hub.

    Verificar acesso ao cluster Kubernetes local.

    Validar execução local com uvicorn.

        instalando as dependencias:

            pip install -r requirements.txt


        Validando a execucao com Uvicorn:

            uvicorn main:app --reload

FASE 2

    Para teste local e ver se está funcionando, pode criar docker-compose (opcional);

        docker compose up --build


    Fazer build: 
    
        docker build -t usuario/fastapi-hello:latest .


    Fazer push: 
    
        docker push usuario/fastapi-hello:latest
        

    Versionar o dockerfile junto com o código da aplicação no github.

FASE 3

    Criar o yaml de deployment e service da aplicação e aplicá-los no cluster

        kubectl apply -f deployment.yaml


FASE 4

FASE 5
