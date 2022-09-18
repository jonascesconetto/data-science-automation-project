## Automação de projetos de Data Science com `Papermill`
  Este repositório contém alguns arquivos e códigos utilizados durante curso de `Data Pipelines com Apache Airflow `.
### Requisitos
Projeto consiste em desenvolver todo o processo de automatização para um projeto de `Data Science`, no qual possui as seguintes etapas:
  1. Carga de dados
  2. Limpeza
  3. Análise e exploraçào
  4. Pré-processamento e transformaçào
  5. Feature Engineering
  6. Ajustes e Avaliação
  7. Deploy
## 💻 Principais Tecnologias, Softwares e Bibliotecas
- Python
- Projeto Anaconda
- Docker 
- Apache AirFlow (Data Pipeline)
- Papermill
## ⚙ Instalação e configuração de ambiente (local)
### Projeto Anaconda
 - Seguir a [documentação](https://www.anaconda.com/) oficial.
### Papermill
  - Executar o seguinte comando no terminal do Anaconda.
    ```bash
    pip install papermill
    ```
  - Verificar se foi instalado corretamente (terminal do Anaconda)
    ```bash
    papermill -h
    ```
## ⚙ Instalação e configuração de ambiente (Docker)
### Docker
  - Seguir a [documentação](https://docs.docker.com/engine/install/) oficial.
### Container - Apache AirFlow
  - `Observação: `Abra o terminal em um diretório raiz para subir seu container com Airflow, dessa forma não perdemos os arquivos de DAGs gerados caso seja necessário algumas reinstalação ou configuração adicional do container.
  - Crie container com seguinte comando em seu terminal:
    ```bash
    docker run -d -p 8080:8080 -v "$PWD/airflow/dags:/opt/airflow/dags/" \
    --entrypoint=/bin/bash \
    --name airflow apache/airflow:2.3.4-python3.8 \
    -c '(\
    airflow db init && \
    airflow users create --username <nome de usuário> --password <sua senha> --firstname <Seu nome> --lastname <Seu Nome> --role Admin --email <Seu e-mail>); \
    airflow webserver & \
    airflow scheduler\
    '
    ```
  - Verificando o container em execução.
    ```bash
    docker container ls
    ```
  - Verificando os logs do container
    ```bash
    docker container logs airflow
    ```
  - Se nenhum erro aparecer, acesse a interface web do Apache Airflow pelo endereço:
    
    **https://localhost:8080**
### Provider para o Papermill no Airflow
  - Acessar o container do Airflow como root
    ```bash
    docker container exec -it --user root airflow bash
    ```
  - Instalar conjunto de bibliotecas necessárias
    ```bash
    pip install sklearn pandas numpy matplotlib **seaborn joblib
    ```
  - Instalar `papermill`
    ```bash
    pip install papermill
    ```
  - Instalar `Jupyter notebook`
    ```bash
    pip install jupyter
    ```
### Instalando o provider do `papermill` do Airflow
  - Acessar o container do Airflow como root
    ```bash
    docker container exec -it --user root airflow bash
    ```
  - Intalar o provider, dentro do container do Airflow
    ```python
    pip install apache-airflow-providers-papermill
    ```
## Executando 
### Testar o `notebook`do projeto
- Dentro do diretório `notebooks` executar o arquivo `regression-linear`.
### Executar o `notebook` com o `Papermill`
- Abrir o terminal do Anaconda
- Navegar até o diretório do notebook do projeto
- Incluir tags nas células para permitir a substituição dos parâmetros variáveis
  - `parameters`
- Rodar o seguinte comando:
  ```bash
  papermill regression-project.ipynb output.ipynb -p test_size_ 0.25 -p dataset "./../data/ecomerce.csv" -p model "./../model/" -p n_estimators 1000 -p max_depth 5 --stdout-file test.txt --stderr-file error.txt
  ```
  Parâmetros presente no projeto:
    - dataset = "./../data/ecomerce.csv"
    - model = "./../model/"
    - test_size_ = 0.3
    - n_estimators = 100
    - max_depth = 10
### Testar as `DAG's` do projeto
- Utilizando o PapermillOperator:
  - `pipeline_data_science_papermill_operator`
- Utiliando PythonOperator:
  - `pipeline_data_science_papermill_python`
## Vantagens e Desvantagens 
### Vantagens
- Rápida e simples
- Ideal para etapas de prototipaçào
- Desenvolvimento simples
- Sem aumentar a curva de aprendizado
### Desvantagens
- Difícil manutenção e controle de erros
- Todo processo é atômico. Difícil dividir as operações
- Todo o processamento é feito no nível do kernel;