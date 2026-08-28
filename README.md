# Desafio Prático: CI e Infraestrutura (Node.js + Jenkins + Docker)

Este repositório contém a solução para o desafio de automação de infraestrutura e Integração Contínua (CI). O projeto orquestra uma API Node.js conectada a um banco de dados PostgreSQL, juntamente com um servidor Jenkins para execução automatizada da esteira de CI.

## 🏗️ Arquitetura e Tecnologias

A infraestrutura foi totalmente containerizada utilizando **Docker** e orquestrada com **Docker Compose**. O ambiente conta com três serviços interconectados:

1. **API Node.js**: Imagem otimizada baseada em Alpine, com dependências em cache.
2. **Banco de Dados (PostgreSQL)**: Com volume persistente de dados e banco de testes.
3. **Jenkins Server**: Imagem LTS customizada, rodando como usuário não-privilegiado, porém configurada para utilizar a CLI do Docker do host (Docker in Docker - DinD).

## 🚀 Como Executar o Projeto Localmente

### 1. Iniciar os serviços

Certifique-se de ter o Docker e o Docker Compose instalados na sua máquina. Na raiz do projeto, execute:

```bash
docker compose up -d
```
*(O parâmetro `--build` pode ser adicionado caso queira forçar a recriação das imagens locais).*

### 2. Acessar o Jenkins

Para acessar a interface do Jenkins, será necessário a senha de administrador inicial. Recupere-a com o comando:

```bash
docker exec jenkins-server cat /var/jenkins_home/secrets/initialAdminPassword
```

Em seguida, acesse [http://localhost:8080](http://localhost:8080), cole a senha e siga com a instalação recomendada dos plugins. 

**Importante:** Vá em *Gerenciar Jenkins* -> *Tools* (Ferramentas globais), adicione uma instalação automática do NodeJS e dê a ela exatamente o nome **`node`**.

### 3. Acessar a API

A aplicação Node.js estará acessível através da porta `3000` e já integrada à rede do banco de dados:
[http://localhost:3000](http://localhost:3000)

## 🔄 Esteira de Integração Contínua (Pipeline)

O arquivo `Jenkinsfile` na raiz do repositório define a pipeline declarativa para o projeto. Ela engloba os seguintes estágios:

- **Checkout:** Clonagem do código fonte.
- **Build / Instalação:** Download de dependências via NPM.
- **SAST (Segurança):** Auditoria de pacotes com `npm audit`.
- **Lint & Quality:** Análise estática do código utilizando `ESLint`.
- **Testes:** Execução da suíte de testes automatizados com `Jest`.
- **Post Actions:** Limpeza dinâmica do workspace de execução e notificação de status da esteira.
