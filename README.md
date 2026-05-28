# ⚡ AWS Lambda com .NET 10 (Boilerplate & Lab)

[![.NET 10](https://img.shields.io/badge/.NET-10.0-blueviolet.svg)](https://dotnet.microsoft.com/download/dotnet/10.0)
[![AWS Lambda](https://img.shields.io/badge/AWS-Lambda-orange.svg)](https://aws.amazon.com/lambda/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Este repositório serve como um guia prático e um modelo de inicialização (boilerplate) para o desenvolvimento de funções **AWS Lambda nativas em .NET 10**, utilizando as melhores práticas de mercado, injeção de dependência simplificada e testes locais eficientes sem a necessidade de emular o ambiente AWS completo via Docker.

A função contida aqui é um processador de strings simples, ideal para entender o ciclo de vida de uma request/response em arquiteturas Serverless.

---

## 🏗️ Arquitetura do Projeto

O projeto é estruturado de forma minimalista, focando em performance (Cold Start reduzido) e manutenibilidade.

* **Runtime:** `.NET 10`
* **Serializador:** `System.TextJson` (Otimizado via Source Generators implícitos para ambientes Serverless).
* **Architecture:** `x86_64` (Suporta alteração para `arm64` no deploy).

### Fluxo de Execução

```text
[ Client / Event Source ] ──( JSON String )──> [ AWS Lambda Handler ]
                                                       │
                                            ( Log: context.Logger )
                                                       │
[ Client ] <──( JSON String em MAIÚSCULO ) ────────────┘

```

---

## 🛠️ Pré-requisitos

Para rodar e modificar este projeto, você precisará de:

1. **.NET 10 SDK** instalado.
2. **AWS .NET Mock Lambda Test Tool** (versão para .NET 10).
3. Uma IDE de sua preferência (VS Code, Visual Studio ou Rider).

Se ainda não tem a ferramenta de testes instalada globalmente, execute:

```bash
dotnet tool install --global Amazon.Lambda.TestTool --version 0.10.3

```

---

## 🚀 Como Executar e Testar Localmente

### 1. Clonar e Buildar

Primeiro, clone o repositório e compile o projeto para gerar os artefatos necessários na pasta `bin`:

```bash
git clone [https://github.com/danhpaiva/aws-lambda-dotnet10-exemplo-funcao.git](https://github.com/danhpaiva/aws-lambda-dotnet10-exemplo-funcao.git)
cd aws-lambda-dotnet10-exemplo-funcao
dotnet build

```

### 2. Iniciar a Ferramenta de Teste

Execute o comando específico da ferramenta para o runtime do .NET 10:

```bash
dotnet-lambda-test-tool-10.0

```

Isso abrirá uma interface web em seu navegador padrão no endereço `http://localhost:5050`.

### 3. Executar o Payload de Teste

Na interface gráfica do browser, configure os seguintes campos:

* **Function Handler:** `ExemploFuncaoLambda::ExemploFuncaoLambda.Function::FunctionHandler`
* **Function Input:** Selecione `Custom` e envie uma string envelopada em aspas duplas:
```json
"olá mundo, testando a lambda em .net 10"

```



Clique em **Execute Function** para ver o output em letras maiúsculas e os logs de execução em tempo real.

---

## 📦 Estrutura de Arquivos Importantes

* `Function.cs`: Contém o ponto de entrada da Lambda (`FunctionHandler`) e o atributo de assembly que define o serializador JSON global.
* `ExemploFuncaoLambda.csproj`: Configurações do SDK da AWS, propriedades de otimização de build como `CopyLocalLockFileAssemblies` e `PublishReadyToRun`.
* `aws-lambda-tools-defaults.json`: Arquivo de configuração que automatiza os parâmetros do deploy via CLI da AWS.

---

## 🚀 Deploy na AWS (via CLI)

Se você tiver o `Amazon.Lambda.Tools` instalado no seu CLI do .NET, o deploy pode ser feito com um único comando a partir da raiz do projeto:

```bash
dotnet lambda deploy-function NomeDaSuaLambda

```

Este comando lerá o arquivo `aws-lambda-tools-defaults.json` automaticamente para definir o runtime, a memória allocated (`512MB`), o timeout (`30s`) e o handler.

---

## 🧠 Dicas Sênior de Otimização Incorporadas

* **`PublishReadyToRun = true`**: Reduz drasticamente o tempo de **Cold Start** da Lambda na AWS, compilando o código em formato nativo antes do upload.
* **`CopyLocalLockFileAssemblies = true`**: Garante que o Mock Test Tool local consiga rastrear todas as dependências do NuGet sem falhas de carregamento de tipo.

---

Feito com .NET 10 💜 por [Daniel](https://www.google.com/search?q=https://github.com/danhpaiva)

```

---