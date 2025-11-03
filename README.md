# Infraestrutura automatizada com AWS CloudFormation
Em vez de provisionar e configurar recursos da AWS manualmente através do console ou CLI, o CloudFormation permite que você defina toda a sua infraestrutura (como instâncias EC2, bancos de dados RDS, redes VPC, grupos de segurança e mais) em arquivos de texto.
# 📝 Conteúdo Essencial dos Templates CloudFormation
O foco aqui é na modularidade e na definição clara dos componentes.

## 1. Template Principal (Orquestrador) - templates/main-application.yml
Este template usará o conceito de Nested Stacks (Stacks Aninhadas) para juntar todos os componentes. Isso permite que você gerencie a aplicação inteira como uma única unidade, mas mantenha os arquivos de infraestrutura pequenos e reutilizáveis.

#### Recurso Principal: 
AWS::CloudFormation::Stack

#### Função:
Ele referencia os templates menores (VPC, EC2, RDS) e passa parâmetros entre eles.

# 💻 Automatizando o Deploy
Para facilitar o uso do repositório, inclua um script simples de automação:

Exemplo de Script de Deploy (scripts/deploy.sh)
Este script usará o AWS CLI para criar ou atualizar o Stack principal:

```Bash

#!/bin/bash

# Define o nome do Stack e o ambiente (para carregar o arquivo de parâmetros correto)
STACK_NAME="MyProject-AppStack"
ENV="dev" # Pode ser alterado para 'prod'

# Carrega a região da AWS
REGION="sa-east-1" 

# Caminho para o template principal e parâmetros
TEMPLATE_BODY="templates/main-application.yml"
PARAMETERS_FILE="parameters/${ENV}-params.json"

echo "Iniciando deploy do Stack ${STACK_NAME} para a região ${REGION}..."

# Comando CloudFormation para criar ou atualizar o Stack (create-change-set)
aws cloudformation deploy \
    --template-file ${TEMPLATE_BODY} \
    --stack-name ${STACK_NAME} \
    --parameter-overrides $(cat ${PARAMETERS_FILE} | jq -r 'to_entries|map("\(.key)=\(.value)")|.[]') \
    --capabilities CAPABILITY_IAM \
    --region ${REGION}

if [ $? -eq 0 ]; then
    echo "Deploy concluído com sucesso!"
else
    echo "Erro no deploy. Verifique o console do CloudFormation para detalhes."```
fi
Nota: Você precisará do jq para processar o arquivo de parâmetros JSON de forma mais limpa.

