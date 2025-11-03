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
