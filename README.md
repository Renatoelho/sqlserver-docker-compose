# Criando um servidor SQL Server com Docker-compose

Neste repositório, você vai aprender como criar um servidor ***SQL Server*** utilizando ***Docker-compose***. Com o Docker-compose, é possível criar e configurar o ambiente de desenvolvimento rapidamente, facilitando o processo de desenvolvimento e evitando problemas de compatibilidade com outras versões do software.

O Docker-compose é uma ferramenta que permite gerenciar e orquestrar vários containers Docker ao mesmo tempo. Ele é usado para definir e executar aplicativos multi-container Docker. O Docker-compose usa um ***arquivo YAML*** para definir os serviços, volumes e redes necessários para cada container. Com o Docker-compose, é possível criar um ambiente de desenvolvimento completo com vários serviços, como bancos de dados, servidores de aplicativos e outros, sem precisar configurar cada um individualmente.

O SQL Server em contêineres é uma forma popular de implantar e gerenciar bancos de dados SQL Server. O SQL Server é um dos sistemas de gerenciamento de banco de dados mais utilizados em todo o mundo, e a ***implantação em containers*** oferece muitos benefícios para os usuários.


# Requisitos

+ ![Docker](https://img.shields.io/badge/Docker-23.0.3-E3E3E3)

+ ![Docker-compose](https://img.shields.io/badge/Docker--compose-1.25.0-E3E3E3)

+ ![Git](https://img.shields.io/badge/Git-2.25.1%2B-E3E3E3)

+ ![Ubuntu](https://img.shields.io/badge/Ubuntu-20.04-E3E3E3)


# Exemplo de um arquivo YAML no Docker-compose com um serviço SQL Server

```yaml
version: "3.3"
services:
  sqlserver:
    hostname: sqlserver
    container_name: sqlserver
    image: mcr.microsoft.com/mssql/server:2022-latest
    environment:
      - SA_PASSWORD="<Crie uma Senha Forte>"
      - ACCEPT_EULA=Y
      - TZ=America/Sao_Paulo
    ports:
      - "1433:1433"
    volumes:
      - ./volumes/sqlserver/sqlserver_data:/var/opt/mssql/data
      - ./volumes/sqlserver/sqlserver_log:/var/opt/mssql/log
      - ./volumes/sqlserver/sqlserver_secrets:/var/opt/mssql/secrets
    deploy:
      resources:
        limits:
          memory: 2G
    restart: always
    networks:
      - rede-sqlserver

networks:
  rede-sqlserver:
    driver: bridge
```

# Implantando o Serviço SQL Server

### Clonando o repositório

Para clonar o repositório execute o seguinte comando:

```bash
git clone https://github.com/Renatoelho/sqlserver-docker-compose.git sqlserver-docker-compose
```


### Ativando o serviço

>***IMPORTANTE***: Lembre-se de alterar o valor da variável 'SA_PASSWORD' no arquivo ```docker-compose.yaml```para uma senha complexa, para que o contêiner seja criado sem problemas e você consiga acessá-lo.

```bash
cd sqlserver-docker-compose
```

```bash
docker-compose -f docker-compose.yaml --compatibility up -d
```


### Ajustando permissões dos volumes

Depois de subir o contâiner altere as permissões do diretório dos volumes.

```bash
sudo chmod -R 777 volumes/
```


### Acessando o SQL Server

|Parâmetro         |Valor         |
|------------------|--------------|
|Usuário           |SA|
|Senha             |\<Sua Senha Forte\>|
|Database          |master|
|Host              |localhost|
|Porta             |1433|


### Desativando o serviço

```bash
docker-compose -f docker-compose.yaml --compatibility down
```

# Referências:

Install Docker Desktop on Ubuntu, ***docs.docker.com***. Disponível em: <https://docs.docker.com/desktop/install/ubuntu/>. Acesso em: 15 de abr. 2023.

The Compose file, ***docs.docker.com***. Disponível em: <https://docs.docker.com/compose/compose-file/03-compose-file/>. Acesso em: 15 de abr. 2023.

Início Rápido: Executar imagens de contêiner do SQL Server Linux com o Docker, ***learn.microsoft.com***. Disponível em: <https://learn.microsoft.com/pt-br/sql/linux/quickstart-install-connect-docker?view=sql-server-ver15&tabs=ubuntu&pivots=cs1-bash>. Acesso em: 15 de abr. 2023.
