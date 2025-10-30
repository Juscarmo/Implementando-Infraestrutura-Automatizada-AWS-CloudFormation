# Implementando-Infraestrutura-Automatizada-AWS-CloudFormation


Desafio do curso Santander Code Girls onde aprendemos a trabahar com o CloudFormation 

🧠 O que é AWS CloudFormation?
CloudFormation atua como um motor de orquestração declarativa da AWS. Em vez de provisionar recursos manualmente pelo console, definimos a arquitetura completa do ambiente (servidores, redes, armazenamento, etc.) em um modelo único.

Eu utilizo esses templates (escritos em YAML ou JSON) para descrever o estado final que minha infraestrutura deve ter. O serviço, então, se encarrega de realizar todas as chamadas de API possíveis para construir, atualizar ou destruir essa arquitetura de forma segura.

Vantagens da Automação
Uma mudança para IaC com CloudFormation traz benefícios:

Padronização: Garantir que os ambientes de Desenvolvimento, Teste e Produção sejam idênticos, eliminando erros de configuração manual ( configuração drift ).
Gestão do Ciclo de Vida: Gerencia o ciclo de vida completo de um conjunto de recursos como uma única unidade.
Rollbacks Automáticos: Em caso de falha durante uma criação ou atualização, o CloudFormation reverte as alterações, restaurando o ambiente ao último estado funcional.
Versionamento: O código da infraestrutura pode ser armazenado e versionado no Git, permitindo auditorias e controle de mudanças.


🏗️ Stacks: A Unidade de Gerenciamento
O elemento central no CloudFormation é a Stack (Pilha) .

Uma Stack é o agrupamento lógico de todos os recursos definidos em um modelo. Ao submeter um template (por exemplo, meu-servico.yaml), o CloudFormation cria uma Stack com esse nome e provisiona todos os componentes nele definidos (ex: um S3 Bucket, um DynamoDB e uma Lambda Function).

Essa abordagem garante que todo o ambiente de uma aplicação possa ser gerenciado e, crucialmente, excluído com segurança através de uma única ação.
📋 Detalhes do Template Implementado
(Substitua o conteúdo abaixo pelo seu modelo e descrição)

Para esta prática, criei um modelo simples em YAML ( s3-bucket-template.yaml) para provisionar um recurso fundamental de armazenamento.
# s3-bucket-template.yaml
AWSTemplateFormatVersion: "2010-09-09"
Description: Template de S3 Bucket para o Desafio DIO com configurações de segurança.

Resources:
  DIOChallengeBucket:
    Type: AWS::S3::Bucket
    Properties:
      # Nome único garantido pelas variáveis intrínsecas
      BucketName: !Sub "dio-cf-project-${AWS::AccountId}-${AWS::Region}"
      AccessControl: Private
      
      # Adição para bloquear acesso público em conformidade com as melhores práticas de segurança
      PublicAccessBlockConfiguration:
        BlockPublicAcls: true
        BlockPublicPolicy: true
        IgnorePublicAcls: true
        RestrictPublicBuckets: true
        
      Tags:
        - Key: Environment
          Value: Teste-DIO

Outputs:
  BucketName:
    Description: Nome final do S3 Bucket criado
    Value: !Ref DIOChallengeBucket
