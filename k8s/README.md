# Imersão Full Stack && Full Cycle 15

Participe: https://imersao.fullcycle.com.br

## Criando as imagens para o k8s

Na pasta do codepix:

`docker build -t skyzenho/codepix-go:latest -f codepix/Dockerfile.prod`

subindo a imagem para o repositorio no dockerhub

`docker push skyzenho/codepix-go:latest`


`docker build -t skyzenho/imersao-bankapi:latest -f nestjs/Dockerfile.prod`

`docker push skyzenho/imersao-bankapi:latest`



`docker build -t skyzenho/imersao-bankfrontend:latest -f nextjs/Dockerfile.prod`

`docker push skyzenho/imersao-bankfrontend:latest`

Os secrets (secret.yaml) foram encodados em base64 através do site: base64encode.org


Para conectar o banco de dados foi utilizando o gerenciador helm para baixar images auxiliares pro k8s no nosso caso imagem da bitnami 

`helm repo add bitnami https://charts.bitnami.com/bitnami`

instalando postgresql:

`helm install postgresql bitnami/postgresql`

Entrar no pod com o comando listado após instalação

Criar tabelas:

`create database codepix;`
`create database bank001;`
`create database bank002;`

Para verificar o postgress password:
echo $POSTGRESS_PASSWORD

encodar base64 a seguinte string com modificando para o passwork encontrado acima

"dbname=codepix sslmode=disable user postgress password=*MODIFYHERE* host=postgres-postgresql"

### populando o bankapi

Entrar do pod do bankapi

`kubectl exec it -bankapi-copypast-randomvalue bash`

Rodar:

`npm run typeorm migration:run`
`npm run console fixtures`

instalando kafka

`helm install kafka bitnami/kafka`


Para aplicar as configurações no k8s utilizar o seguinte comando:

`kubectl apply -f k8s/bankapi/. `
`kubectl apply -f k8s/bankapi002/. `
`kubectl apply -f k8s/bankfrontend/. `
`kubectl apply -f k8s/bankfrontend002/. `
`kubectl apply -f k8s/codepix/. `

Para acessar o serviço sem utilizar as cloud providers é possivel fazer um redirecionamento das portas:

`kubectl port-forward svc/bankfrontend-service 9090:3000`

Algumas funcionalidade(Cadastrar chave) não podem ser acessadas devido a requisições feitas pelo navegador. O serviço ta rodando apenas na rede interna. seria necessário um ip externo