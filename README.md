# Roteiro para o desenvolvimento da atividade/desafio de projeto "Adicionando Segurança em APIs na AWS com Amazon Cognitoda" da DIO - Digital Innovation One referênte ao Bootcamp Cloud AwS

## Serviços AWS utilizados

- Amazon Cognito
- Amazon DynamoDB
- Amazon API Gateway
- AWS Lambda

## Etapas do desenvolvimento

### Criando uma API REST no Amazon API Gateway

- API Gateway Dashboard -> Create API -> REST API -> Build
- Protocol - REST -> Create new API -> API name [nome da sua api] -> Endpoint Type - Regional -> Create API
- Resources -> Actions -> Create Resource -> Resource Name [neste projeto foi usado o nome Items] -> Create Resource

### No Amazon DynamoDB

- DynamoDB Dashboard -> Tables -> Create table -> Table name [Items] -> Partition key [id] -> Create table

### No AWS Lambda

#### Função para inserir item

- Lambda Dashboard -> Create function -> Name [put_item_function] -> Create function
- Inserir código da função ```put_item_function.js``` disponível na pasta ```/src``` -> Deploy
- Configuration -> Execution role -> Abrir a Role no console do IAM
- IAM -> Roles -> Role criada no passo anterior -> Permissions -> Add inline policy
- Service - DynamoDB -> Manual actions -> add actions -> putItem
- Resources -> Add arn -> Selecionar o arn da tabela criada no DynamoDB -> Add
- Review policy -> Name [lambda_dynamodb_putItem_policy] -> Create policy

### Integrando o API Gateway com o Lambda backend

- API Gateway Dashboard -> Selecionar a API criada -> Resources -> Selecionar o resource criado -> Action -> Create method - POST
- Integration type -> Lambda function -> Use Lambda Proxy Integration -> Lambda function -> Selecionar a função Lambda criada -> Save
- Actions -> Deploy API -> Deployment Stage -> New Stage [dev] -> Deploy

### No POSTMAN

- Add Request -> Method POST -> Copiar o endpoint gerado no API Gateway
- Body -> Raw -> JSON -> Adicionar o seguinte body
```
{
  "id": "001",
  "price": 900
}
```
- Send

### No Amazon Cognito

- Cognito Dashboard -> Manage User Pools -> Create a User Pool -> Pool name [TestPool]
- How do you want your end users to sign in? - Email address or phone number -> Next Step
- What password strength do you want to require?
- Do you want to enable Multi-Factor Authentication (MFA)? Off -> Next Step
- Do you want to customize your email verification messages? -> Verification type - Link -> Next Step
- Which app clients will have access to this user pool? -> App client name [TestClient] -> Create App Client -> Next Step
- Create Pool

- App integration -> App client settings -> Enabled Identity Providers - Cognito User Pool
- Callback URL(s) [https://example.com/logout]
- OAuth 2.0 -> Allowed OAuth Flows - Authorization code grant -Implicit grant
- Allowed OAuth Scopes	- email	- openid
- Save Changes

- Domain name -> Domain prefix [nome da sua api] -> Save

### Criando um autorizador do Amazon Cognito para uma API REST no Amazon API Gateway

- API Gateway Dashboard -> Selecionar a API criada -> Authorizers -> Create New Authorizer
- Name [CognitoAuth] -> Type - Cognito -> Cognito User Pool [pool criada anteriormente] -> Token Source [Authorization]

- Resources -> selecionar o resource criado -> selecionar o método criado -> Method Request -> Authorization - Selecionar o autorizador criado

### No POSTMAN

- Add request -> Authorization
- Type - OAuth 2.0
- Callback URL [https://example.com/logout]
- Auth URL [https://nomedasuaapi.auth.sa-east-1.amazoncognito.com/login]
- Client ID - obter o Client ID do Cognito em App clients
- Scope [email - openid]
- Client Authentication [Send client credentials in body]
- Get New Acces Token
- Copiar o token gerado

- Selecionar a request para inserir item criada -> Authorization -> Type - Bearer Token -> Inserir o token copiado
- Send

# Desafio AWS - Autenticação, Autorização e Gerenciamento de Usuários

## Abaixo enuciado original na plataforma [dio.me](https://www.dio.me)

## Descrição do Desafio

Ofereça autenticação, autorização e gerenciamento de usuários para suas aplicações Web e Mobile com o Amazon Cognito. Esse serviço, totalmente gerenciado pela AWS, suporta os principais mecanismos de segurança do mercado, além da integração com terceiros, como Facebook, Google, Apple ou a própria Amazon. Nesse sentido, nosso Tech Hero apresenta os passos para tornar suas APIs mais seguras por meio dessa solução.

Neste desafio, você irá aprender ao mesmo tempo que desenvolve algo prático para o seu portfólio! Sendo assim, as seguintes tarefas serão realizadas:

- Utilizar os serviços Amazon Cognito, DynamoDB, API Gateway e AWS Lambda;
- Criar uma API REST no Amazon API Gateway;
- Criar tabela no Amazon DynamoDB;
- Criar funções no AWS Lambda;
- Integrar o API Gateway com o Lambda backend;
- Utilizar a ferramenta No POSTMAN;
- Criar um autorizador do Amazon Cognito para uma API REST no Amazon API Gateway.

## Instruções

1. Faça o login na sua conta AWS ou crie uma nova, caso ainda não tenha uma.
2. Crie um novo projeto na AWS Console e acesse o serviço Amazon Cognito para configurar a autenticação de usuários.
3. Configure as tabelas necessárias no Amazon DynamoDB para armazenar os dados dos usuários.
4. Crie as funções do AWS Lambda que irão processar as solicitações da API.
5. Crie uma API REST no Amazon API Gateway e integre com as funções Lambda criadas anteriormente.
6. Utilize o POSTMAN ou outra ferramenta de sua preferência para testar a API e verificar a autenticação e autorização dos usuários.
7. Implemente um autorizador do Amazon Cognito para garantir que apenas usuários autenticados tenham acesso à API REST no Amazon API Gateway.

## --------------------------------------FIM-----------------------------------------------------

## Recursos Adicionais

- Documentação oficial da AWS: [https://aws.amazon.com/documentation/](https://aws.amazon.com/documentation/)
- Documentação do Amazon Cognito: [https://aws.amazon.com/cognito/](https://aws.amazon.com/cognito/)
- Documentação do Amazon DynamoDB: [https://aws.amazon.com/dynamodb/](https://aws.amazon.com/dynamodb/)
- Documentação do Amazon API Gateway: [https://aws.amazon.com/api-gateway/](https://aws.amazon.com/api-gateway/)
- Documentação do AWS Lambda: [https://aws.amazon.com/lambda/](https://aws.amazon.com/lambda/)


### Lembrado que a AWS pode fazer alterações na sua interface gráfica e alguns passos, itens etc esterem em umaoutra sequência

