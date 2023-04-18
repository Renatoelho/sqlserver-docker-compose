# Requisitos

Docker 23.0.3 (Host)

Docker-Compose 1.25.0 (Host)


# Criando um servidor SQL Server com Docker-compose

Neste repositório, você vai aprender como criar um servidor ***SQL Server*** utilizando ***Docker-compose***. Com o Docker-compose, é possível criar e configurar o ambiente de desenvolvimento rapidamente, facilitando o processo de desenvolvimento e evitando problemas de compatibilidade com outras versões do software.

O Docker-compose é uma ferramenta que permite gerenciar e orquestrar vários containers Docker ao mesmo tempo. Ele é usado para definir e executar aplicativos multi-container Docker. O Docker-compose usa um ***arquivo YAML*** para definir os serviços, volumes e redes necessários para cada container. Com o Docker-compose, é possível criar um ambiente de desenvolvimento completo com vários serviços, como bancos de dados, servidores de aplicativos e outros, sem precisar configurar cada um individualmente.

O SQL Server em contêineres é uma forma popular de implantar e gerenciar bancos de dados SQL Server. O SQL Server é um dos sistemas de gerenciamento de banco de dados mais utilizados em todo o mundo, e a ***implantação em containers*** oferece muitos benefícios para os usuários.


# Subindo um serviço SQL Server com Docker-compose

```yaml
version: "3.3"
services:
  sqlserver:
    image: mcr.microsoft.com/mssql/server:2022-latest
    environment:
      SA_PASSWORD: "<Crie uma Senha Forte>"
      ACCEPT_EULA: "Y"
    ports:
      - "1433:1433"
    volumes:
      - volume-sqlserver:/var/opt/mssql/data
      - volume-sqlserver:/var/opt/mssql/log
      - volume-sqlserver:/var/opt/mssql/secrets
    restart: always

volumes:
  volume-sqlserver:
    external: true
```

Esta representação YAML contém a definição de um serviço de banco de dados SQL Server.

Na seção ***services***, a imagem do SQL Server 2022 mais recente é usada (mcr.microsoft.com/mssql/server:2022-latest). As ***variáveis*** de ambiente são definidas para configurar o SQL Server: a senha do usuário 'SA' ***SA_PASSWORD*** e o contrato de licença ***ACCEPT_EULA***. A ***porta*** 1433 é mapeada para permitir a conexão ao SQL Server a partir de outras aplicações externas.

A seção ***volumes*** define os pontos de montagem para os volumes externos que são compartilhados entre o host e o container. São usados três volumes que são nomeados como "***volume-sqlserver***" e são mapeados para as pastas ***/var/opt/mssql/data***, ***/var/opt/mssql/log*** e ***/var/opt/mssql/secrets***.

Por fim, a seção ***restart*** define que o serviço será sempre reiniciado após falhas ou erros inesperados.

A última seção, ***volumes***, define o nome do volume compartilhado que é referenciado na seção services. O volume volume-sqlserver é definido como um volume externo "***external: true***", o que significa que ele deve ser criado antes de executar o serviço do SQL Server.

> ***Obs.:*** Crie um arquivo com esse conteúdo ou utilize o disponível no repositório.

# Implementando o Serviço SQL Server

#### Crie o volume no Docker

```bash
docker volume create --name=volume-sqlserver
```


#### Configurações no volume

- Inspecione o volume criado e pegue o diretório existente em “Mountpoint”.

```bash
docker volume inspect volume-sqlserver
```

- Alterando as permissões do diretório da montagem.

```bash
sudo chmod 777 /var/lib/docker/volumes/volume-sqlserver/_data
```

> ***Obs.:*** Essa abordagem foi a opção que encontrei para evitar erros de ***permissão negada*** na ativação do serviço quando o SQL Server tenta gravar nos volumes.


#### Ativando o serviço

```bash
docker-compose -f docker-compose.yaml --compatibility up -d
```


#### Desativando o serviço

```bash
docker-compose -f docker-compose.yaml --compatibility down
```

> ***Atenção:*** Quando o serviço é ativo é criada uma rede exclusiva, e na desativação essa rede é deletada.


# Referências:

Install Docker Desktop on Ubuntu, ***docs.docker.com***. Disponível em: <https://docs.docker.com/desktop/install/ubuntu/>. Acesso em: 15 de abr. 2023.

The Compose file, ***docs.docker.com***. Disponível em: <https://docs.docker.com/compose/compose-file/03-compose-file/>. Acesso em: 15 de abr. 2023.

Início Rápido: Executar imagens de contêiner do SQL Server Linux com o Docker, ***learn.microsoft.com***. Disponível em: <https://learn.microsoft.com/pt-br/sql/linux/quickstart-install-connect-docker?view=sql-server-ver15&tabs=ubuntu&pivots=cs1-bash>. Acesso em: 15 de abr. 2023.
