## <img align="center" width="40px" src="https://hermes.digitalinnovation.one/assets/diome/logo-minimized.png"> Este repositório foi criado como parte dos estudos e desafios do **Santander Code Girls** - Implementando Infraestrutura Automatizada com AWS CloudFormation
# 🛠️ Implementando IaC na AWS: Provisionamento com CloudFormation

Este repositório documenta minha experiência prática com o AWS CloudFormation, a ferramenta nativa de Infraestrutura como Código (IaC) da Amazon Web Services. O objetivo foi automatizar o *deployment* de recursos, garantindo consistência e rastreabilidade na arquitetura.

## 🧠 O que é AWS CloudFormation?

CloudFormation atua como um **motor de orquestração declarativa** da AWS. Em vez de provisionar recursos manualmente pelo console, nós definimos a arquitetura completa do ambiente (servidores, redes, storage, etc.) em um *template* único.

Eu utilizo esses templates (escritos em YAML ou JSON) para descrever o estado final que a minha infraestrutura deve ter. O serviço, então, se encarrega de realizar todas as chamadas de API necessárias para construir, atualizar ou destruir essa arquitetura de forma segura.

### Vantagens da Automação

A mudança para a IaC com CloudFormation proporciona ganhos significativos:

* **Padronização:** Garante que os ambientes de Desenvolvimento, Teste e Produção sejam idênticos, eliminando erros de configuração manual (*configuration drift*).
* **Gestão do Ciclo de Vida:** Gerencia o ciclo de vida completo de um conjunto de recursos como uma única unidade.
* **Rollbacks Automáticos:** Em caso de falha durante a criação ou atualização, o CloudFormation reverte as alterações, restaurando o ambiente ao último estado funcional.
* **Versionamento:** O código da infraestrutura pode ser armazenado e versionado no Git, permitindo auditoria e controle de mudanças.

## 🏗️ Stacks: A Unidade de Gerenciamento

O elemento central no CloudFormation é a **Stack (Pilha)**.

Uma Stack é o **agrupamento lógico** de todos os recursos que são definidos em um template. Ao submeter um template (por exemplo, `meu-servico.yaml`), o CloudFormation cria uma Stack com esse nome e provisiona todos os componentes definidos nele (ex: um S3 Bucket, um DynamoDB e uma Lambda Function).

Essa abordagem garante que todo o ambiente de uma aplicação possa ser gerenciado e, crucialmente, **excluído** com segurança através de uma única ação.

## 📋 Detalhes do Template Implementado

**(Substitua o conteúdo abaixo pelo seu template e descrição)**

Para esta prática, criei um template simples em YAML (`s3-bucket-template.yaml`) para provisionar um recurso fundamental de armazenamento.

```yaml
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
```
## 🛠️ Comparativo Rápido com o Terraform

Como engenheiro, é importante conhecer as alternativas. O Terraform é uma escolha popular, mas possui um escopo diferente do CloudFormation:

| Característica | AWS CloudFormation | Terraform |
| :--- | :--- | :--- |
| **Escopo de Cloud** | Focado e integrado exclusivamente na AWS | Agnosticidade de Cloud (Multi-Cloud) |
| **Linguagem Utilizada** | YAML / JSON | HCL (HashiCorp Configuration Language) |
| **Gerenciamento de Estado** | Integrado e gerenciado internamente pela AWS | Requer o gerenciamento de um arquivo de estado (State File) |
| **Modelo de Custos** | Gratuito (paga-se apenas pelos recursos AWS criados) | Open Source (gratuito) |

 ### 🔗 Desafio proposto por [Digital Innovation One - DIO](https://www.dio.me/)
