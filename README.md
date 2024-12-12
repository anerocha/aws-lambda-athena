# Processamento de Mensagens do Telegram com AWS Lambda e Athena

Este projeto tem como objetivo coletar, transformar e armazenar as mensagens do Telegram em formato otimizado para consultas analíticas. Utilizamos AWS Lambda para o processamento dos dados, S3 para o armazenamento e AWS Athena para consulta via SQL. O processo é automatizado para ser executado diariamente, processando os dados do dia anterior.

## Arquitetura de Dados

1. **Coleta de Dados:** As mensagens do Telegram são armazenadas em arquivos JSON em um bucket S3, no formato bruto.
2. **Processamento Diário:** Uma função Lambda é executada automaticamente todos os dias, realizando o download das mensagens, transformando os dados (de JSON para Parquet) e armazenando os arquivos no S3, em um bucket enriquecido.
3. **Consulta de Dados:** O AWS Athena fornece uma interface SQL para consultar os dados armazenados no S3, permitindo análises eficientes sobre as mensagens.
4. **Automatização:** O processo de execução da função Lambda é agendado utilizando o **AWS EventBridge**, garantindo que os dados do dia anterior sejam processados automaticamente todos os dias.

## Tecnologias Utilizadas

- **AWS Lambda**: Função serverless para processamento de dados.
- **Amazon S3**: Armazenamento de dados brutos e enriquecidos.
- **PyArrow**: Biblioteca para manipulação de dados no formato Parquet.
- **AWS Athena**: Ferramenta de consulta SQL para análise de dados.
- **AWS EventBridge**: Serviço de agendamento de tarefas para executar a função Lambda automaticamente.

## Como Configurar e Executar

### 1. Variáveis de Ambiente

Este projeto depende de duas variáveis de ambiente que devem ser configuradas no console do AWS Lambda:

- **AWS_S3_BUCKET**: Nome do bucket S3 onde os dados brutos (JSON) são armazenados.
- **AWS_S3_ENRICHED**: Nome do bucket S3 onde os dados enriquecidos (Parquet) serão armazenados.

### 2. Permissões

A função Lambda precisa de permissões adequadas para acessar e modificar os buckets S3. Certifique-se de que a função tenha a permissão `s3:PutObject` para o bucket de dados enriquecidos e `s3:GetObject` para o bucket de dados brutos.

### 3. Configuração da Função Lambda

- Faça o upload do código da função Lambda.
- Instale a biblioteca `PyArrow` criando uma camada (Layer) ou faça o upload do pacote no S3 e adicione a camada ao Lambda.
- Aumente o tempo de execução do Lambda para pelo menos **5 minutos** para garantir que o processamento de grandes volumes de dados seja concluído sem interrupções.

### 4. Agendamento com AWS EventBridge

Utilize o **AWS EventBridge** para agendar a execução diária da função Lambda. Configure o evento para ser acionado em um horário fixo todos os dias, garantindo que os dados do dia anterior sejam processados.

### 5. Consulta no AWS Athena

Após o processamento, você pode consultar os dados no **AWS Athena**. 
