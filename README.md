# 📘 estudo-noauth

### 🔐 Controle de Acesso com Spring Security e JWT

Projeto de estudo com foco em autenticação e autorização usando Spring Security, JWT e controle de acesso por papéis (roles). A aplicação disponibiliza endpoints protegidos que só podem ser acessados por usuários autenticados com permissões específicas.

---

### 📌 Objetivo

Este projeto foi desenvolvido com o objetivo de praticar os seguintes conceitos:
- Segurança com Spring Security e JWT
- Autenticação baseada em tokens
- Autorização por rotas usando `@PreAuthorize`
- Integração com banco de dados H2 (ambiente de testes)
- Controle de CORS

---

### 🛠️ Tecnologias e Ferramentas

- Java 17+
- Spring Boot
- Spring Security
- JWT (JSON Web Token)
- H2 Database (ambiente de teste)
- Maven

---

### ✅ Funcionalidades

- Autenticação de usuários com e-mail e senha
- Geração de token JWT após login
- Acesso protegido a endpoints com base nos papéis:
  - `ROLE_ADMIN`
  - `ROLE_OPERATOR`
- Operações de produtos (exemplo de recurso protegido):
  - `GET /products` – acesso público autenticado
  - `GET /products/{id}` – permitido para `ADMIN` e `OPERATOR`
  - `POST /products` – permitido apenas para `ADMIN`

---

### 🧪 Perfis e Banco de Teste

- Utiliza **H2** como banco em memória para testes
- Console do H2 acessível em: `http://localhost:8080/h2-console`
- Profile ativo por padrão: `test`

#### `application.properties`
```properties
spring.profiles.active=${APP_PROFILE:test}
security.jwt.duration=${JWT_DURATION:86400}
cors.origins=${CORS_ORIGINS:http://localhost:3000,http://localhost:5173}
```

#### `application-test.properties`
```properties
spring.datasource.url=jdbc:h2:mem:testdb
spring.h2.console.enabled=true
spring.jpa.show-sql=true
```

---

### ▶️ Como Executar

#### Pré-requisitos
- Java 17+
- Maven

#### Passos:
```bash
# 1. Clone o repositório
git clone https://github.com/seuusuario/estudo-noauth.git
cd estudo-noauth

# 2. Compile o projeto
./mvnw clean install

# 3. Execute
./mvnw spring-boot:run
```

---

### 🔑 Autenticação e Geração do Token

Para acessar os endpoints protegidos, é necessário obter um token JWT. Siga os passos abaixo:

1. Faça uma requisição `POST` para o endpoint de login:
```
POST /oauth/token
```

2. Envie os dados no formato `x-www-form-urlencoded`:

| Campo        | Valor                          |
|--------------|--------------------------------|
| grant_type   | password                       |
| username     | email do usuário               |
| password     | senha do usuário               |
| client_id    | myclientid (padrão)            |
| client_secret| myclientsecret (padrão)        |

#### Exemplo usando `curl`:
```bash
curl -X POST http://localhost:8080/oauth/token \
  -u myclientid:myclientsecret \
  -d grant_type=password \
  -d username=alex@gmail.com \
  -d password=123456
```

3. A resposta será um JSON com o token:
```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "token_type": "bearer",
  "expires_in": 86400,
  "scope": "read write"
}
```

4. Para usar o token, envie-o no header das requisições:
```
Authorization: Bearer SEU_TOKEN
```

#### 🧑‍💼 Usuários para Teste

| E-mail              | Senha   | Permissões             |
|---------------------|---------|------------------------|
| alex@gmail.com      | 123456  | ROLE_OPERATOR          |
| maria@gmail.com     | 123456  | ROLE_OPERATOR, ADMIN   |

---

### 🔐 Segurança

O projeto implementa autenticação com JWT e autorização por roles usando anotações como:

```java
@PreAuthorize("hasRole('ROLE_ADMIN')")
```

O token JWT é validado em cada requisição autenticada. As credenciais são buscadas no banco de dados via `UserRepository`.

---

### 🔄 Estrutura Básica

```
src/
 └── main/
     ├── java/
     │   └── com.devsuperior.demo/
     │        ├── entities/
     │        ├── repositories/
     │        ├── services/
     │        ├── controllers/
     │        └── config/
     └── resources/
          ├── application.properties
          └── application-test.properties
```

---


### 👨‍💻 Autor

**Gabriel Prata**  
[GitHub](https://github.com/GabrielPraata) | [LinkedIn](https://www.linkedin.com/in/gabrielprata/)


