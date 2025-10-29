# Projeto Micro-SaaS Escolar

Este projeto implementa um sistema de gerenciamento escolar utilizando uma arquitetura de microsserviços em Flask, conforme proposto na atividade AP2.

O sistema é dividido em três serviços independentes, cada um com seu próprio banco de dados SQLite e documentação Swagger.

## Arquitetura de Microsserviços

O ecossistema é composto por três serviços principais que se comunicam de forma síncrona:

1.  **Serviço de Gerenciamento** (`:5001`)
    * Responsável pelo cadastro e gerenciamento de Professores, Turmas e Alunos.
    * Serve como a fonte central de dados para as outras entidades.

2.  **Serviço de Reservas** (`:5002`)
    * Responsável por gerenciar as reservas de salas e laboratórios.
    * Consome o serviço de **Gerenciamento** para validar a existência de uma `turma_id` ao criar ou atualizar uma reserva.

3.  **Serviço de Atividades e Notas** (`:5003`)
    * Responsável por gerenciar Atividades (criadas por professores para turmas) e Notas (atribuídas a alunos).
    * Consome o serviço de **Gerenciamento** para validar a existência de `turma_id`, `professor_id` e `aluno_id` ao realizar operações.

## Como Executar o Projeto (com Docker)

A forma mais simples de executar todo o ecossistema é utilizando o Docker Compose.

**Pré-requisitos:**
* [Docker](https://docs.docker.com/get-docker/)
* [Docker Compose](https://docs.docker.com/compose/install/)

**Instruções:**

1.  **Clonar o repositório:**
    ```bash
    git clone <https://github.com/JoaoVictor2201/micro-saas.git>
    cd micro-saas
    ```

2.  **Subir os containers:**
    No diretório raiz (onde está o `docker-compose.yml`), execute:
    ```bash
    docker-compose up --build
    ```
    O comando `--build` força a reconstrução das imagens, o que é útil se fizeres alterações no código.

3.  **Criar os Bancos de Dados (Primeira Execução):**
    Como cada serviço tem um banco de dados SQLite separado, precisamos criá-los. Abra **três novos terminais** e execute os seguintes comandos, um em cada terminal:

    * **Terminal 1 (Gerenciamento):**
        ```bash
        docker exec -it gerenciamento flask shell
        ```
        Dentro do shell, digite:
        ```python
        from app import db, create_app
        app = create_app()
        with app.app_context():
            db.create_all()
        exit()
        ```

    * **Terminal 2 (Reservas):**
        ```bash
        docker exec -it reservas flask shell
        ```
        Dentro do shell, digite:
        ```python
        from app import db, create_app
        app = create_app()
        with app.app_context():
            db.create_all()
        exit()
        ```

    * **Terminal 3 (Atividades):**
        ```bash
        docker exec -it atividades flask shell
        ```
        Dentro do shell, digite:
        ```python
        from app import db, create_app
        app = create_app()
        with app.app_context():
            db.create_all()
        exit()
        ```

4.  **Pronto!** Os serviços estão a funcionar.

## Endpoints e Documentação (Swagger)

Após executar `docker-compose up`, os serviços estarão disponíveis nos seguintes endereços:

* **Serviço de Gerenciamento:**
    * **API:** `http://localhost:5001/`
    * **Swagger Docs:** `http://localhost:5001/apidocs`

* **Serviço de Reservas:**
    * **API:** `http://localhost:5002/`
    * **Swagger Docs:** `http://localhost:5002/apidocs`

* **Serviço de Atividades e Notas:**
    * **API:** `http://localhost:5003/`
    * **Swagger Docs:** `http://localhost:5003/apidocs`

 
## Integrantes:

- João Victor - 2402779
- Vitor Daniel - 2403060
- Anderson Alberissi - 2403321
- Gabriel Gonçalves - 2402912
