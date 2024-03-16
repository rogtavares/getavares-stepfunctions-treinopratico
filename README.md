# TREINO  AWS Step Functions 


Treino pratico, para desenvolver uma aplicaçao baseada em servicos  da
AWS orquestrada por meio do  Serviços AWS utilizados 
AWS Step Functions

- 1 AWS Step Functions
- 2 AWS Lambda
- 3 Amazon S3
- 4 Amazon Rekognition
- 5 Amazon DynamoDB
- 6 Amazon Cloudwatch
- 7 Amazon Cloudtrail

## Requisitos

- Conta ativa na AWS, cuidado com custo final.


## Roteiro de desenvolvimento

- Criar uma tabela no Amazon DynamoDB
- Criar duas funções AWS Lambda
- Criar um bucket no Amazon S3
- Criar uma máquina de estado no AWS Step Functions
- Configurar uma trail no AWS Cloudtrail
- Criar uma regra de evento no AWS Cloudwatch
- Testar

## Passos do desenvolvimento...faça com ( muita) calma .


### Criar bucket no Amazon S3

- S3 Console -> Create bucket -> Bucket name [seu_bucket] -> Create bucket 

### Criar tabela no Amazon DynamoDB

 - DynamoDB Console -> Create table - > Table name [nome_da_sua_tabela] -> Partition key [filename] -> Create table

### Criar funções lambda

#### Função de extração de metadados do arquivo no Amazon S3

 - Lambda Console -> Create function -> Author from scratch -> Function name [extract-metadata-function] -> Runtime [Python3.8] -> Role [Create new role with basic Lambda permissions] -> Create function
 - Inserir o [código](src/lambda_file_metadata.py) no editor de código -> Deploy

#### Função para chamada da API do Amazon Rekognition  ( parte q mais curto)) 

- Lambda Console -> Create function -> Author from scratch -> Function name [rekognition-api-function] -> Runtime [Python3.8] -> Role [Create new role with basic Lambda permissions] -> Create function
 - Inserir o [código](src/lambda_rekognition_api.py) no editor de código -> Deploy

### Configurar políticas de acesso

- Selecionar a função criada -> Configuration -> Permissions -> Execution role 
- Adicionar as seguintes Policies

  - CloudWatchFullAccess
  - AmazonRekognitionFullAccess
  - AmazonS3FullAccess
  - AmazonDynamoDBFullAccess
  - AWSCloudTrail_FullAccess 

### Criar máquina de estado no AWS Step Functions    

 - Step Functions Console -> State machines -> Create state machine -> Write your workflow in code -> Standard -> Inserir o [código](src/state_machine.json) no editor -> Next
 - Name [nome_da_sua_state_machine] -> Execution role [Create_new_role] -> Create state machine

### Configurar o CloudTrail

 - CloudTrail Console -> Trails -> Trail name [nome_da_trail] -> Storage location [Create new S3 bucket] -> Next
 - Events -> Event type -> Data events -> Switch to basic event selectors -> Individual bucket selection [read, write] -> Choose bucket -> Next
 - Create trail

### Configurar regra de evento no CloudWatch

 - CloudWatch Console -> Rules -> Create Rule -> Event Pattern -> Service Name [Simple Storage Service] -> Event Type [Object Level Operations] -> Specific Operations [PutObject] -> Specific bucket by name [bucket_criado]
 - Targets -> Add target -> Step Functions state machine -> State machine [sua_state_machine] -> Create a new role for this specific resource
 - Configure details -> Name [nome_da_rule]

### ENCERRA SERVIÇO AWS. (( para não gerar custos ))

Rogerio Tavares.  ((16 MARCO 24)) 

![diagrama arquitetura 2.jpg](diagrama%20arquitetura%202.jpg)