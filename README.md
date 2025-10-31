# ProjectHub API

API RESTful desenvolvida para gestão de projetos e freelancers, utilizando .NET 8, MySQL e arquitetura Clean Architecture com Domain-Driven Design (DDD).

## 📋 Descrição do Projeto

Este projeto implementa um sistema completo de gestão de projetos freelancers, permitindo:
- Cadastro e gestão de freelancers com especialidades e taxas horárias
- Criação e acompanhamento de projetos
- Sistema de avaliações e ratings para freelancers
- Controle de disponibilidade e status de projetos
- Relacionamento entre freelancers e projetos

## 🛠️ Tecnologias Utilizadas

- **.NET 8** - Framework principal
- **MySQL** - Banco de dados relacional
- **Entity Framework Core 8.0** - ORM para acesso a dados
- **AutoMapper** - Mapeamento de objetos
- **FluentValidation** - Validação de dados
- **Swagger/OpenAPI** - Documentação da API
- **Pomelo.EntityFrameworkCore.MySql** - Provider MySQL para EF Core

## 📁 Estrutura do Projeto

```
ProjectHub/
├── src/
│   ├── ProjectHub.Domain/              # Camada de Domínio
│   │   ├── Entities/                   # Entidades ricas do domínio
│   │   └── Common/                     # Classes base e exceções
│   ├── ProjectHub.Application/         # Camada de Aplicação
│   │   ├── DTOs/                       # Data Transfer Objects
│   │   ├── Interfaces/                 # Contratos de serviços e repositórios
│   │   ├── Services/                   # Lógica de negócio
│   │   ├── Validators/                 # Validações FluentValidation
│   │   └── Mappings/                   # Configurações AutoMapper
│   ├── ProjectHub.Infrastructure/      # Camada de Infraestrutura
│   │   ├── Data/                       # DbContext e configurações
│   │   └── Repositories/               # Implementação dos repositórios
│   └── ProjectHub.Presentation/        # Camada de Apresentação
│       └── Controllers/                # Controllers RESTful
└── ProjectHub.sln                      # Solução Visual Studio
```

## 🚀 Instruções de Execução

### Pré-requisitos

- .NET 8 SDK instalado
- MySQL Server instalado e rodando
- Visual Studio 2022 ou VS Code (recomendado)

### Configuração do Banco de Dados

1. Configure a string de conexão no arquivo `appsettings.json`:

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Port=3306;Database=ProjectHubDb;User=root;Password=sua_senha;CharSet=utf8mb4;"
  }
}
```

2. Crie o banco de dados no MySQL:

```sql
CREATE DATABASE ProjectHubDb CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

### Executando o Projeto

1. Clone o repositório:
```bash
git clone <url-do-repositorio>
cd cp2-thomas
```

2. Restaure as dependências:
```bash
dotnet restore
```

3. Crie as migrations (se necessário):
```bash
cd src/ProjectHub.Infrastructure
dotnet ef migrations add InitialCreate --startup-project ../ProjectHub.Presentation/ProjectHub.Presentation.csproj
dotnet ef database update --startup-project ../ProjectHub.Presentation/ProjectHub.Presentation.csproj
```

4. Execute o projeto:
```bash
cd src/ProjectHub.Presentation
dotnet run
```

5. Acesse a documentação Swagger:
```
http://localhost:5000/swagger
```

## 📚 Rotas Disponíveis

### Freelancers

| Método | Rota | Descrição |
|--------|------|-----------|
| GET | `/api/freelancers` | Lista todos os freelancers |
| GET | `/api/freelancers/active` | Lista freelancers ativos |
| GET | `/api/freelancers/{id}` | Obtém freelancer por ID |
| POST | `/api/freelancers` | Cria novo freelancer |
| PUT | `/api/freelancers/{id}` | Atualiza freelancer |
| DELETE | `/api/freelancers/{id}` | Remove freelancer |
| PATCH | `/api/freelancers/{id}/activate` | Ativa freelancer |
| PATCH | `/api/freelancers/{id}/deactivate` | Desativa freelancer |

### Projetos

| Método | Rota | Descrição |
|--------|------|-----------|
| GET | `/api/projects` | Lista todos os projetos |
| GET | `/api/projects/{id}` | Obtém projeto por ID |
| GET | `/api/projects/freelancer/{freelancerId}` | Lista projetos de um freelancer |
| POST | `/api/projects` | Cria novo projeto |
| PUT | `/api/projects/{id}` | Atualiza projeto |
| DELETE | `/api/projects/{id}` | Remove projeto |
| PATCH | `/api/projects/{id}/publish` | Publica projeto (Draft → Published) |
| POST | `/api/projects/{id}/assign` | Atribui projeto a freelancer |
| PATCH | `/api/projects/{id}/complete` | Marca projeto como concluído |
| PATCH | `/api/projects/{id}/cancel` | Cancela projeto |

## 📝 Exemplos de Requisições

### Criar Freelancer

```http
POST /api/freelancers
Content-Type: application/json

{
  "name": "João Silva",
  "cpf": "12345678901",
  "email": "joao.silva@email.com",
  "phone": "11987654321",
  "specialization": 1,
  "hourlyRate": 150.00
}
```

**Especializações disponíveis:**
- 1 = DesenvolvimentoWeb
- 2 = DesenvolvimentoMobile
- 3 = Design
- 4 = Marketing
- 5 = Redacao
- 6 = Traducao

### Criar Projeto

```http
POST /api/projects
Content-Type: application/json

{
  "clientName": "Maria Santos",
  "clientEmail": "maria@email.com",
  "title": "Desenvolvimento de Site Institucional",
  "description": "Desenvolvimento de site responsivo com WordPress, incluindo design personalizado e integração com formulários de contato.",
  "budget": 5000.00
}
```

### Atribuir Projeto a Freelancer

```http
POST /api/projects/{projectId}/assign
Content-Type: application/json

{
  "freelancerId": "guid-do-freelancer"
}
```

## 🏗️ Arquitetura

O projeto segue os princípios de **Clean Architecture** e **Domain-Driven Design (DDD)**:

- **Domain**: Entidades ricas com regras de negócio encapsuladas
- **Application**: Casos de uso, DTOs, validações e interfaces
- **Infrastructure**: Implementação de repositórios e acesso a dados
- **Presentation**: Controllers e configuração da API

### Princípios Aplicados

- ✅ Separação de responsabilidades
- ✅ Inversão de dependências
- ✅ Entidades ricas com validações de domínio
- ✅ DTOs para transferência de dados
- ✅ Validações com FluentValidation
- ✅ Mapeamento com AutoMapper
- ✅ Migrations para versionamento do banco

## 📊 Códigos de Resposta HTTP

- `200 OK` - Requisição bem-sucedida
- `201 Created` - Recurso criado com sucesso
- `204 No Content` - Recurso removido com sucesso
- `400 Bad Request` - Erro de validação ou regra de negócio
- `404 Not Found` - Recurso não encontrado

## 👥 Integrantes do Grupo

- RM557356 - Alex Ribeiro
- RM560649 - Thomas Baute
- RM560812 - Gabriel dos Santos

## 📄 Licença

Este projeto foi desenvolvido como parte do CP2 da disciplina Advanced Business Development with .NET - FIAP 2025.

---

*"Faça o teu melhor, na condição que você tem, enquanto você não tem condições melhores, para fazer melhor ainda."*  
— Mario Sergio Cortella
