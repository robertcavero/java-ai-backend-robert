# API de Gerenciamento de Transações Financeiras integrado à Inteligência Artificial.


## 💡 Sobre o Projeto
> ℹ️ **Nota:** Este projeto é um *fork* aprimorado com foco em otimização e clareza.

Este projeto consiste em uma API RESTful de gerenciamento de transações financeiras integrado à IA. A aplicação foi desenvolvida utilizando Java 25, Spring Boot e Spring AI.

Seu diferencial é a possibilidade de interagir com o sistema por meio de voz. O usuário envia um áudio descrevendo uma transação ou fazendo uma consulta, o sistema transforma esse áudio em texto, envia a informação para a ser interpretada.

Após processar a solicitação, a resposta também pode ser convertida em áudio e devolvida ao usuário."

---

## 🛠️ Tecnologias Utilizadas

- **Backend / Core:** Java 25, Spring Boot, Spring AI.
- **Persistência / Banco de Dados:** Spring Data JPA, Hibernate, MySQL
- **Inteligência Artificial:** OpenAI API (`gpt-4o-mini`, `Whisper-1`, `gpt-4o-mini-tts`)
- **Build / Gerenciamento:** Gradle

---

## 🚀 Melhoria Implementada

1. **Correção do Mapeamento de UUID no MySQL:**
   - Antes, o ID das transações gerado como `UUID` aparecia mapeado como `BLOB` no MySQL. A melhoria consistiu na alteração das annotations da entidade (`TransactionEntity`) utilizando `@JdbcTypeCode(SqlTypes.VARCHAR)` e definindo o `@Column(length = 36)`. Assim o Hibernate gerou o identificador corretamente como texto legível. (Veja a diferença abaixo)
       ```javascript
      [
      	{
      		"amount" : 12000,
      		"id" : "3fde5d4c-8e5f-4ac0-96bb-27ba80369af5",
      		"description" : "Entrega de comida",
      		"category" : "RESTAURANTS"
      	},
      	{
      		"amount" : 9000,
      		"id" : ?,
      		"description" : "combo de pipoca no cinema",
      		"category" : "ENTERTAINMENT"
      	}
      ]
      ```

2. **Documentação de Uso Alternativa (Curl)**
   - Inclusão de exemplos práticos via terminal para desenvolvedores que optam por testar os endpoints da API diretamente via requisições HTTP (curl) sem depender de ferramentas gráficas:
  
      ```bash
      curl -X POST http://localhost:8080/transactions/ai \
      -F "file=@./src/test/resources/audio/recording-1.m4a" \
      -o ./testes_sucesso/exemplo_1.mp3
       ```
     
---


## ⚙️ Como Executar a Aplicação

Siga os passos abaixo para configurar e rodar o projeto em seu ambiente local.

### Pré-requisitos

Certifique-se de ter instalado em sua máquina:
- Git
- Java JDK (versão compatível com Java 25)
- Docker Desktop

### Passo a Passo

1. **Clone o repositório:**
   ```bash
   git clone https://github.com/robertcavero/java-ai-backend-robert.git
   cd spring-ai
   ```

2. **Inicie a aplicação:**
   ```bash
   ./gradlew bootRun
   ```

---
## 🧪 Como Testar o Fluxo Principal
## 1. Pré-requisitos
* Mantenha o **Docker Desktop** em execução no seu computador.
* Tenha sua **API Key da OpenAI**.

## 2. Configuração do Projeto
1. Acesse o arquivo `application.properties` localizado na pasta `spring-ai/src/main/resources/`.
2. Adicione sua chave da API da OpenAI nas configurações do projeto (você pode fazer isso diretamente no IntelliJ).
3. No mesmo arquivo `application.properties`, configure a propriedade do banco de dados como `create` para gerar a estrutura inicial:

```properties
spring.jpa.hibernate.ddl-auto=create
```

---

## 3. Conexão com o Banco de Dados
1. Conecte o banco ao MySQL adicionando-o como *Data Source*.
2. Utilize as informações geradas para instanciar e visualizar o banco no **MySQL Workbench**.

---

## 4. Primeira Execução 
1. Abra o terminal e execute o comando abaixo para iniciar a aplicação e criar o banco de dados (o Docker Desktop estar em execução é essencial para o sucesso dessa etapa):

```bash
./gradlew bootRun
```

2. Após a primeira execução bem-sucedida, volte ao arquivo `application.properties` e mude a propriedade para `update`, pois o banco já foi criado:

```properties
spring.jpa.hibernate.ddl-auto=update
```

3. Execute o projeto novamente pelo terminal:

```bash
./gradlew bootRun
```

---

## 5. Executando os Testes
Com a aplicação rodando na porta `8080`, abra o Git Bash e envie um comando `curl` para processar um áudio e gerar um arquivo `.mp3`:

```bash
curl -X POST http://localhost:8080/transactions/ai \
  -F "file=@./src/test/resources/audio/recording-1.m4a" \
  -o ./testes_sucesso/exemplo_1.mp3
```
---

## 🧠 O que aprendi durante o desafio

- Mapeamento com Hibernate: Compreensão de como o Spring Data lida com tipos modernos (como UUID) em bancos relacionais tradicionais como o MySQL utilizando SqlTypes.

- Integração com Spring AI: Orquestração de modelos multimodais da OpenAI para transformar dados não estruturados de áudio em objetos de domínio tipados e persistidos.

