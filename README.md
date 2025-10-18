☁️ Guia Completo de AWS para Desenvolvedores Python

> Autor: DevGege
Versão: 1.2
Descrição: Um guia prático e visual sobre AWS, com foco em uso real por engenheiros de software e desenvolvedores Python.




---

🧭 Sumário

1. Introdução


2. Principais serviços


3. Formas de acesso


4. Conceitos fundamentais


5. Fluxos e diagramas visuais


6. Configuração inicial passo a passo


7. AWS CLI — comandos úteis


8. Boto3 — exemplos práticos em Python


9. Mini-projeto: Backup Automático para S3


10. Automação & CI/CD


11. Observabilidade e segurança


12. Boas práticas de arquitetura


13. Recursos e certificações


14. Licença e contribuição




---

Introdução

A Amazon Web Services (AWS) é a maior plataforma de computação em nuvem do mundo.
Permite criar, implantar e escalar aplicações sem precisar gerenciar infraestrutura física.
Você paga apenas pelo uso e pode controlar tudo via painel web, CLI ou SDKs (como o Boto3 para Python).


---

Principais serviços

Categoria	Serviço	Descrição

💻 Computação	EC2	Servidores virtuais escaláveis
⚙️ Serverless	Lambda	Execução de funções sob demanda
🗃️ Armazenamento	S3	Bucket para objetos, arquivos e backups
🧩 Banco de Dados	RDS, DynamoDB	SQL gerenciado e NoSQL
🌐 Rede	VPC, Route 53, CloudFront	Infraestrutura, DNS e CDN
🔐 Segurança	IAM, KMS, Secrets Manager	Controle de acesso e criptografia
🧠 IA & ML	SageMaker, Comprehend	Modelos de machine learning e NLP
📊 Monitoramento	CloudWatch, X-Ray	Logs e métricas
🧰 DevOps	CodeBuild, CodeDeploy, CodePipeline	CI/CD nativo



---

Formas de acesso

1. Console Web

Interface gráfica, ideal para iniciantes e debug visual.

2. AWS CLI

Ferramenta de linha de comando:

aws configure
aws s3 ls
aws ec2 describe-instances

3. SDK (Boto3)

Integração com Python:

import boto3
s3 = boto3.client('s3')
for bucket in s3.list_buckets()['Buckets']:
    print(bucket['Name'])

4. Infraestrutura como Código (IaC)

Com Terraform, AWS CDK ou CloudFormation.


---

Conceitos fundamentais

Regiões: Localização física dos datacenters (ex: sa-east-1 → São Paulo)

Zonas de disponibilidade (AZ): Subdivisões dentro das regiões.

IAM Roles/Policies: Controle de acesso baseado em permissão.

Buckets S3: Armazenamento de objetos (com versionamento e criptografia).

Elastic Load Balancer (ELB): Distribui tráfego entre servidores EC2.

Auto Scaling Group: Cria/destroi instâncias conforme demanda.



---

Fluxos e diagramas visuais

🧱 Arquitetura Web Simplificada

graph TD;
  A[Usuário] -->|HTTP/HTTPS| B[CloudFront / CDN];
  B --> C[Load Balancer];
  C --> D[Instâncias EC2];
  D --> E[RDS - Banco de Dados];
  D --> F[S3 - Arquivos Estáticos];
  D --> G[CloudWatch - Logs];

🪣 Fluxo de Backup Diário com Lambda e S3

graph LR;
  A[Servidor Local / App] --> B[Lambda Function];
  B --> C[S3 Bucket];
  C --> D[CloudWatch - Notificação];

Esses diagramas podem ser renderizados automaticamente pelo GitHub, GitLab e MkDocs.


---

Configuração inicial passo a passo

1. Criar conta na AWS


2. Criar um usuário no IAM com permissões de administrador


3. Gerar chaves de acesso (Access Key e Secret Key)


4. Configurar via CLI:

aws configure


5. Testar conexão:

aws sts get-caller-identity




---

AWS CLI — comandos úteis

Tarefa	Comando

Listar buckets	aws s3 ls
Criar bucket	aws s3 mb s3://meu-bucket
Fazer upload	aws s3 cp arquivo.txt s3://meu-bucket/
Ver instâncias EC2	aws ec2 describe-instances
Parar instância	aws ec2 stop-instances --instance-ids i-xxxx



---

Boto3 — exemplos práticos em Python

Upload para S3

import boto3

s3 = boto3.client('s3')
s3.upload_file('dados.zip', 'meu-bucket', 'backups/dados.zip')
print('✅ Upload concluído!')

Listar instâncias EC2

import boto3

ec2 = boto3.resource('ec2')
for instance in ec2.instances.all():
    print(instance.id, instance.state)

Executar Lambda

lambda_client = boto3.client('lambda')
response = lambda_client.invoke(
    FunctionName='meu_lambda',
    InvocationType='Event',
    Payload=b'{}'
)


---

Mini-projeto: Backup Automático para S3

Estrutura de pastas

backup_project/
├── backup.py
├── requirements.txt
└── .env

Exemplo backup.py

import boto3, os, datetime

s3 = boto3.client('s3')
BUCKET = 'meu-backup'
FOLDER = '/var/data'

for file in os.listdir(FOLDER):
    path = os.path.join(FOLDER, file)
    if os.path.isfile(path):
        dest = f"{datetime.date.today()}/{file}"
        s3.upload_file(path, BUCKET, dest)
        print(f'✅ Enviado: {dest}')

Automação com cron

0 2 * * * /usr/bin/python3 /home/ubuntu/backup.py


---

Automação & CI/CD

Exemplo com GitHub Actions

name: Deploy para AWS Lambda
on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: sa-east-1
      - run: zip -r function.zip .
      - run: aws lambda update-function-code --function-name MeuLambda --zip-file fileb://function.zip


---

Observabilidade e segurança

Use CloudWatch Logs e X-Ray para rastrear erros e latência.

Ative MFA no console AWS.

Utilize IAM Roles em vez de chaves fixas.

Criptografe dados com KMS.

Configure S3 Block Public Access por padrão.



---

Boas práticas de arquitetura

1. Divida workloads por contas e regiões.


2. Use Infraestrutura como Código (IaC).


3. Configure monitoramento automatizado.


4. Utilize Auto Scaling para custo dinâmico.


5. Evite chaves hardcoded no código-fonte.




---

Recursos e certificações

Nível	Certificação	Foco

🎓 Iniciante	AWS Certified Cloud Practitioner	Fundamentos gerais
🧠 Intermediário	AWS Certified Developer – Associate	Desenvolvimento
⚙️ Avançado	AWS Certified Solutions Architect	Arquitetura


Recursos

AWS Training

Documentação do Boto3

Labs práticos AWS Skill Builder



---

Licença e contribuição

Este guia é open source sob a licença MIT.
Contribuições são bem-vindas via pull requests no GitHub.

> “Automatizar é multiplicar tempo.” — DevGege



