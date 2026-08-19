Sí. Si estás diseñando un CV para pasar filtros ATS para **.NET Developer, AI Engineer y DevOps Engineer**, yo separaría el problema en dos: **parseabilidad** y **matching de contenido**.

Importante: un ATS moderno no funciona necesariamente como un simple filtro de "tiene/no tiene palabra". Normalmente extrae información del CV y la compara/rankea contra la descripción del puesto. Por eso no conviene "rellenar" keywords que no podés defender. ([TechieCV](https://www.techiecv.com/resume-skills/dotnet-developer?utm_source=chatgpt.com ".NET Developer Resume Skills & ATS Keywords (2026) | TechieCV"))

# 1. TOP cosas que NO debe contener un CV ATS

### 🔴 Prioridad máxima

|Evitar|Problema|
|---|---|
|❌ Dos o tres columnas|El parser puede mezclar el orden del texto|
|❌ Tablas para estructurar el CV|Puede separar columnas/celdas incorrectamente|
|❌ Text boxes|El texto puede no entrar correctamente al parser|
|❌ Información importante en header/footer|Algunos parsers no la extraen bien|
|❌ Foto|No aporta al matching ATS|
|❌ Gráficos|No son texto interpretable|
|❌ Barras de nivel de skills|`C# ████████░░` no aporta información estructurada|
|❌ Logos de tecnologías|El ATS puede no identificar el logo|
|❌ Iconos para teléfono/email/LinkedIn|Puede generar caracteres basura|
|❌ CV completamente diseñado en Canva/Figma|Puede romper el parsing, especialmente si termina como imagen o PDF complejo|
|❌ PDF escaneado|Para el ATS puede ser prácticamente una imagen|
|❌ Texto dentro de imágenes|No es keyword-searchable de forma confiable|
|❌ Fuentes extravagantes|Pueden generar problemas de extracción/renderizado|
|❌ Skill rating `90%`, `★★★★★`|No demuestra experiencia y puede perjudicar el parsing|

Las recomendaciones actuales convergen especialmente en **una columna, headings estándar, sin tablas/text boxes/gráficos y PDF basado en texto o DOCX**, aunque el formato preferido depende del ATS/portal concreto. ([Syphon Labs](https://www.syphonlabs.com/blog/ats-friendly-resume-checklist-2026?utm_source=chatgpt.com "ATS-Friendly Resume Checklist for 2026 | Syphon Labs"))

---

# 2. Evitar títulos "creativos"

No pondría:

```text
MY JOURNEY
WHAT I DO
TECH JOURNEY
MY SUPER POWERS
WHERE I'VE BEEN
```

Mejor:

```text
PROFESSIONAL SUMMARY
TECHNICAL SKILLS
PROFESSIONAL EXPERIENCE
EDUCATION
CERTIFICATIONS
PROJECTS
LANGUAGES
```

Los headings convencionales facilitan la extracción estructurada. ([Syphon Labs](https://www.syphonlabs.com/blog/ats-friendly-resume-checklist-2026?utm_source=chatgpt.com "ATS-Friendly Resume Checklist for 2026 | Syphon Labs"))

---

# 3. No poner keywords solamente en "Skills"

Este es un error muy importante.

❌ Débil:

```text
SKILLS

C#
.NET
Azure
Docker
Kubernetes
SQL Server
```

Mejor:

```text
TECHNICAL SKILLS

C#, .NET 8, ASP.NET Core, Entity Framework Core,
REST APIs, SQL Server, Azure, Docker, Kubernetes,
Azure DevOps, Git, CI/CD
```

Pero todavía mejor:

```text
PROFESSIONAL EXPERIENCE

Senior .NET Developer

Developed REST APIs using C# and ASP.NET Core
running on .NET 8, with Entity Framework Core
and SQL Server.

Implemented CI/CD pipelines using Azure DevOps
and deployed containerized services using Docker.
```

La keyword aparece en **Skills + Experience**, donde además queda respaldada por contexto. ([Resume Captain](https://resumecaptain.com/ats-keywords/dotnet-developer?utm_source=chatgpt.com ".NET Developer ATS Keywords — Complete List (2026)"))

---

# 4. No poner keywords que realmente no dominás

Esto:

```text
C#
.NET
Java
Python
Go
Rust
C++
Kubernetes
Terraform
AWS
Azure
GCP
TensorFlow
PyTorch
OpenAI
Spark
Kafka
...
```

puede parecer impresionante, pero si después la experiencia no demuestra nada, es contraproducente.

La regla debería ser:

> **Keyword = tecnología que realmente utilizaste y podés defender técnicamente.**

---

# 5. No usar abreviaturas innecesarias

Por ejemplo, si la búsqueda dice:

```text
Amazon Web Services
```

no pondría solamente:

```text
AWS
```

Mejor:

```text
Amazon Web Services (AWS)
```

Lo mismo:

```text
Kubernetes (K8s)
Continuous Integration / Continuous Delivery (CI/CD)
Artificial Intelligence (AI)
Machine Learning (ML)
Application Programming Interface (API)
```

Después podés usar la abreviatura.

Esto aumenta las posibilidades de coincidir tanto con la forma completa como con la abreviada.

---

# 6. Perfil .NET Developer

Para **.NET Developer / Senior .NET Developer**, las keywords que más cuidaría son:

### 🔥 Core

```text
C#
.NET
.NET 8
.NET 9
ASP.NET Core
REST APIs
Web API
Entity Framework Core
SQL Server
LINQ
```

### 🔥 Backend

```text
ASP.NET Core
RESTful APIs
Microservices
Entity Framework Core
Dependency Injection
Middleware
Authentication
Authorization
JWT
OAuth 2.0
OpenAPI
Swagger
```

### 🔥 Arquitectura

```text
Microservices
Clean Architecture
SOLID
CQRS
MediatR
Design Patterns
Domain-Driven Design
Event-Driven Architecture
gRPC
```

### 🔥 Cloud / DevOps

```text
Microsoft Azure
Azure App Service
Azure Functions
Azure Service Bus
Azure DevOps
Docker
Kubernetes
CI/CD
Git
GitHub
GitHub Actions
```

### 🔥 Testing

```text
Unit Testing
Integration Testing
xUnit
NUnit
Moq
FluentAssertions
```

Las listas actuales de keywords para .NET ponen especialmente arriba **C#, .NET 8/9, ASP.NET Core, REST APIs, Entity Framework Core, SQL Server, Azure, Docker y CI/CD**. ([Resume Captain](https://resumecaptain.com/ats-keywords/dotnet-developer?utm_source=chatgpt.com ".NET Developer ATS Keywords — Complete List (2026)"))

### Ejemplo correcto

```text
Senior .NET Developer

Developed and maintained REST APIs using C#, .NET 8,
ASP.NET Core and Entity Framework Core.

Designed microservices using Clean Architecture,
Dependency Injection and CQRS.

Implemented CI/CD pipelines with Azure DevOps and
deployed containerized applications using Docker.

Optimized SQL Server queries, reducing average API
response time by 35%.
```

Eso es muchísimo mejor que:

```text
Skills:
C#, .NET, Azure, Docker, SQL, Microservices
```

---

# 7. Perfil AI Engineer

Acá hay que evitar algo muy común:

```text
AI Engineer
Machine Learning
Deep Learning
Artificial Intelligence
```

Eso es demasiado genérico.

Para **AI Engineer**, las keywords deberían reflejar el tipo de AI que realmente desarrollás.

### 🔥 Core AI

```text
Artificial Intelligence
Machine Learning
Deep Learning
Generative AI
Large Language Models (LLMs)
Natural Language Processing (NLP)
Computer Vision
Speech AI
```

### 🔥 Python / ML

```text
Python
PyTorch
TensorFlow
scikit-learn
NumPy
Pandas
Jupyter
```

### 🔥 LLM

Si realmente los utilizás:

```text
LLM
Large Language Models
OpenAI API
Azure OpenAI
Anthropic
Hugging Face
Transformers
Fine-tuning
Prompt Engineering
RAG
Retrieval-Augmented Generation
Embeddings
Vector Databases
Semantic Search
```

### 🔥 MLOps

```text
MLflow
Docker
Kubernetes
CI/CD
GitHub Actions
Azure ML
AWS SageMaker
Vertex AI
Model Deployment
Model Monitoring
```

### 🔥 Vector / RAG

```text
FAISS
Pinecone
Weaviate
Milvus
Qdrant
Elasticsearch
Azure AI Search
```

### 🔥 Data

```text
SQL
PostgreSQL
MongoDB
Data Pipelines
ETL
Feature Engineering
Data Preprocessing
```

### Ejemplo

```text
AI Engineer

Developed Generative AI applications using Python,
PyTorch, Large Language Models (LLMs), embeddings and
Retrieval-Augmented Generation (RAG).

Implemented document ingestion, chunking, embedding
generation and semantic search using vector databases.

Fine-tuned transformer-based models and deployed
inference services using Docker and Kubernetes.

Implemented ML pipelines and model monitoring using
MLflow and CI/CD.
```

Esto permite que el ATS encuentre:

```text
Python
PyTorch
LLM
RAG
Embeddings
Vector Database
Fine-tuning
Docker
Kubernetes
MLflow
CI/CD
```

y además demuestra que realmente los utilizaste.

---

# 8. Perfil DevOps Engineer

Para DevOps hay un conjunto de keywords muy claro.

### 🔥 Core

```text
DevOps
CI/CD
Linux
Git
Docker
Kubernetes
Infrastructure as Code
Terraform
```

### 🔥 Cloud

Elegir según tu experiencia:

```text
AWS
Azure
Google Cloud Platform (GCP)
```

Y después servicios concretos.

AWS:

```text
EC2
ECS
EKS
S3
Lambda
VPC
IAM
CloudWatch
RDS
ECR
```

Azure:

```text
Azure DevOps
AKS
Azure Kubernetes Service
Azure App Service
Azure Functions
Azure Container Registry
Azure Monitor
Azure Key Vault
```

### 🔥 Kubernetes

```text
Kubernetes
Docker
Helm
K8s
Ingress
ConfigMaps
Secrets
Deployments
Services
Namespaces
```

### 🔥 IaC

```text
Terraform
Ansible
CloudFormation
Infrastructure as Code
```

### 🔥 CI/CD

```text
Jenkins
GitHub Actions
GitLab CI/CD
Azure DevOps
Azure Pipelines
ArgoCD
GitOps
```

### 🔥 Monitoring

```text
Prometheus
Grafana
ELK Stack
Elasticsearch
Logstash
Kibana
OpenTelemetry
Datadog
```

Las listas actuales de DevOps destacan especialmente **Docker, Kubernetes, CI/CD, AWS, Terraform, Jenkins, Ansible, Linux, Git, monitoring, Infrastructure as Code, Helm, Prometheus y Grafana**. ([ATSAlign](https://www.atsalign.com/resume-checker/devops-engineer?utm_source=chatgpt.com "ATS Resume Checker for DevOps Engineers | ATSAlign"))

---

# 9. DevOps: ejemplo fuerte

En lugar de:

```text
DevOps Engineer

Responsible for DevOps activities.
Managed servers and deployments.
Worked with cloud technologies.
```

muchísimo mejor:

```text
DevOps Engineer

Designed and maintained CI/CD pipelines using GitHub
Actions and Azure DevOps for .NET microservices.

Containerized applications using Docker and deployed
workloads to Kubernetes (AKS).

Provisioned Azure infrastructure using Terraform,
implementing Infrastructure as Code.

Implemented observability using Prometheus, Grafana
and OpenTelemetry.

Automated deployment processes, reducing deployment
time by 60%.
```

Ahí aparecen:

```text
CI/CD
GitHub Actions
Azure DevOps
Docker
Kubernetes
AKS
Terraform
Infrastructure as Code
Prometheus
Grafana
OpenTelemetry
Automation
```

---

# 10. Las palabras que SÍ quiero en los tres CV

Independientemente del perfil, conviene que cada experiencia tenga:

```text
ACTION + TECHNOLOGY + WHAT + RESULT
```

Por ejemplo:

```text
Developed REST APIs using ASP.NET Core,
serving 500K+ requests per day and reducing
average response time by 30%.
```

No:

```text
Worked with ASP.NET Core.
```

---

# 11. Verbos ATS útiles

Usaría verbos como:

```text
Developed
Designed
Implemented
Architected
Built
Engineered
Automated
Optimized
Migrated
Integrated
Deployed
Configured
Refactored
Maintained
Scaled
Improved
Reduced
Increased
Led
Delivered
```

Pero no hay que meterlos artificialmente.

---

# 12. Métricas

Este es uno de los elementos que más diferencia un CV técnico bueno de uno mediocre.

❌

```text
Developed APIs with .NET.
```

✅

```text
Developed 25+ REST APIs using ASP.NET Core,
supporting 2M+ monthly requests.
```

❌

```text
Worked with Kubernetes.
```

✅

```text
Deployed 15+ microservices to Kubernetes,
automating releases through GitHub Actions.
```

❌

```text
Implemented an AI solution.
```

✅

```text
Developed a RAG-based AI assistant using Python,
LLMs, embeddings and Qdrant, reducing manual document
search time by 70%.
```

---

# 13. El Summary tiene que ser ATS-friendly

No:

```text
Passionate technology enthusiast who loves solving
complex problems and creating innovative solutions.
```

Eso consume espacio y aporta pocas keywords.

Para .NET:

```text
Senior .NET Developer with 8+ years of experience
developing backend systems using C#, .NET 8,
ASP.NET Core, REST APIs, Entity Framework Core,
SQL Server, Azure, Docker and microservices.
```

Para AI:

```text
AI Engineer with experience developing Generative AI,
LLM and RAG solutions using Python, PyTorch,
embeddings, vector databases, Docker and cloud
platforms.
```

Para DevOps:

```text
DevOps Engineer experienced in CI/CD, Kubernetes,
Docker, Terraform, Azure, Linux, GitHub Actions and
Infrastructure as Code, with a focus on automation
and cloud deployments.
```

---

# 14. Estructura ATS que usaría

Yo utilizaría esta estructura:

```text
NOMBRE APELLIDO
.NET Developer | AI Engineer | DevOps Engineer

Location | Phone | Email | LinkedIn | GitHub

PROFESSIONAL SUMMARY

TECHNICAL SKILLS

PROFESSIONAL EXPERIENCE

Company
Job Title
Location
MM/YYYY - MM/YYYY

- Achievement...
- Achievement...
- Achievement...

Company
Job Title
MM/YYYY - MM/YYYY

- Achievement...
- Achievement...

PROJECTS

Project
- ...

EDUCATION

Degree
University

CERTIFICATIONS

Certification

LANGUAGES
```

Una estructura de una sola columna y encabezados estándar es actualmente la opción más segura para el parsing. ([Syphon Labs](https://www.syphonlabs.com/blog/ats-friendly-resume-checklist-2026?utm_source=chatgpt.com "ATS-Friendly Resume Checklist for 2026 | Syphon Labs"))

---

# 15. El CV no debería ser uno solo

Para estos tres perfiles **no haría un único CV genérico**.

Haría:

```text
CV 01
.NET Developer
```

```text
CV 02
AI Engineer
```

```text
CV 03
DevOps Engineer
```

Y después, incluso mejor:

```text
CV .NET
     ↓
Job Description .NET
     ↓
CV .NET adaptado
```

```text
CV AI
     ↓
Job Description AI
     ↓
CV AI adaptado
```

```text
CV DevOps
     ↓
Job Description DevOps
     ↓
CV DevOps adaptado
```

La razón es que el ATS compara contra **la descripción concreta de la vacante**, así que no existe un conjunto universal de keywords que maximice el matching para todos los puestos. Las keywords exactas de la oferta deben reflejarse cuando realmente corresponden a tu experiencia. ([TechieCV](https://www.techiecv.com/resume-skills/dotnet-developer?utm_source=chatgpt.com ".NET Developer Resume Skills & ATS Keywords (2026) | TechieCV"))

## Mi "Top 10" de reglas ATS

Si tuviera que reducir todo a diez reglas:

1. **Una sola columna.**
    
2. **Sin tablas, text boxes ni sidebars.**
    
3. **Sin foto, logos, gráficos ni barras de skills.**
    
4. **Información de contacto como texto normal, no escondida en header/footer.**
    
5. **Headings estándar:** `Summary`, `Skills`, `Experience`, `Education`, etc.
    
6. **Keywords exactas de la job description**, siempre que sean verdaderas para vos.
    
7. **Las keywords deben aparecer también dentro de Experience**, no solamente en Skills.
    
8. **Usar nombre completo + abreviatura:** `Artificial Intelligence (AI)`, `Kubernetes (K8s)`, etc.
    
9. **Cada bullet debería demostrar impacto**, idealmente con números.
    
10. **PDF basado en texto o DOCX**, siguiendo lo que acepte el portal; nunca un PDF escaneado/imagen. ([Syphon Labs](https://www.syphonlabs.com/blog/ats-friendly-resume-checklist-2026?utm_source=chatgpt.com "ATS-Friendly Resume Checklist for 2026 | Syphon Labs"))
    

Y para los tres perfiles que mencionaste, el núcleo sería:

```text
.NET
C# | .NET 8/9 | ASP.NET Core | REST | EF Core |
SQL Server | Azure | Docker | Kubernetes | CI/CD
```

```text
AI
Python | PyTorch | LLM | Generative AI | RAG |
Embeddings | Transformers | Fine-tuning |
Vector Database | MLOps | Docker | Kubernetes
```

```text
DevOps
Linux | Git | Docker | Kubernetes | Terraform |
CI/CD | AWS/Azure/GCP | Helm | Ansible |
Prometheus | Grafana | GitOps | IaC
```

**Y una recomendación importante si estás construyendo un sistema automático de análisis/generación de CV:** no usaría un "ATS score" fijo del tipo `87% = pasa`. Es mucho más útil construir un **matcher contra la Job Description** que detecte: `required skills`, `preferred skills`, `years of experience`, `job titles`, `cloud`, `frameworks`, `certifications`, `education` y `missing keywords`, y luego genere una versión del CV **sin inventar experiencia**.