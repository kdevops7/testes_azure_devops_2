# 🚀 Automação de Variable Groups no Azure DevOps via API

Este projeto documenta o processo de criação automatizada de **Variable Groups** (Library) no Azure DevOps utilizando chamadas de API REST e scripts JSON. A centralização dessas variáveis permite uma gestão mais eficiente de configurações de ambiente para pipelines de CI/CD.


## 📋 Visão Geral

O objetivo foi criar um grupo de variáveis dentro de um projeto específico, garantindo que todas as chamas como ao MongoDB, Service Bus e Storage estivessem prontas para uso em pipelines.


## 🛠️ Pré-requisitos

Antes de iniciar, certifique-se de ter:
* Uma Organização e um Projeto criados no Azure DevOps (Ex: `ellevo-next-test/Teste`).
* Um **PAT (Personal Access Token)** com permissões de "Variable Groups" (Read & Manage) ou "Full Access" para facilitar a requisição.
* Acesso ao **Azure CloudShell** (Bash) ou qualquer terminal com `curl` instalado.
* O **ID (GUID)** do projeto no Azure DevOps (Essencial, grupos de variáveis são sempre vinculados a um projeto específico).


## 📂 Estrutura do Arquivo JSON

O arquivo `variables.json` define a estrutura do grupo que irá ser criado. Abaixo está o modelo utilizado como exemplo com as variáveis de ambiente, strings de conexão e chaves de acesso:

```json
{
  "name": "EllevoDataImport",
  "description": "Grupo de variáveis para o serviço ellevo_data_import",
  "type": "Vsts",
  "variables": {
    "environment": { "value": "Production" },
    "App__InstanceId": { "value": "$(APPLICATION_INSTANCE_ID)" },
    "MongoDb__AppDatabase": { "value": "Ellevo" },
    "MessageBroker__Type": { "value": "ServiceBus" },
    "AzureBlobStorage__ConnectionString": { "value": "Seu_ConnectionString_Aqui" }
    // ... demais variáveis omitidas para brevidade
  },
  "variableGroupProjectReferences": [
    {
      "name": "Teste",
      "projectReference": {
        "id": "<GUID_DO_PROJETO>",
        "name": "Teste"
      }
    }
  ]
}


##🚀 Executando a Criação

Para realizar a chamada à API e criar o grupo, utilize o comando curl abaixo, substituindo os campos entre < >:

Bash
# Certifique-se de que seu PAT esteja completo, com todos os caracteres dados após criação do mesmo.
# Use "\" para o terminal Bash entendar a quebra de linhas

Comando:
curl -u :<PAT> \
  -X POST \
  -H "Content-Type: application/json" \
  https://dev.azure.com/ellevo-next-test/Teste/_apis/distributedtask/variablegroups?api-version=7.1-preview.2 \
  -d @variaveis.json -v

##Explicação do comando:
curl: Ferramenta utilizada para transferir dados e realizar requisições via protocolos de rede.
-u :<PAT>: Realiza a autenticação. O : antes do token indica ao sistema que o campo de "usuário" está vazio, enviando o Personal Access Token como credencial de acesso.
-X POST: Define o método HTTP como POST, indicando que a intenção da chamada é criar um novo recurso no servidor.
-H "Content-Type: application/json": Define o cabeçalho (header). Informa ao Azure que o corpo da requisição está formatado como JSON.
URL do Endpoint: Endereço completo que aponta para a organização, o projeto e o serviço específico de variablegroups na versão 7.1.
-d @variaveis.json: O parâmetro -d envia os dados (payload). O prefixo @ instrui o curl a ler o conteúdo diretamente do arquivo local chamado variaveis.json.
-v (Verbose): Ativa o modo detalhado. Essencial para o troubleshooting, pois exibe todo o log da transação, facilitando a identificação de erros de autenticação ou sintaxe.


##⚠️ Lições Aprendidas (Troubleshooting)

Durante a implementação, foram corrigidos os seguintes pontos críticos:

- Autenticação: Garantir que o PAT tenha permissão suficiente para interagir com a Library.
- Sintaxe JSON: Atenção especial à vírgulas e fechamento de chaves na estrutura de variáveis.
- Escopo do Projeto: Grupos de variáveis são vinculados a um projectReference; se o GUID do projeto estiver incorreto, a API retornará erro 404 ou 400.


##✨ Documentação gerada para facilitar o provisionamento de Variable Groups no Azure DevOps via API, através do Cloud Shell terminal Bash.
