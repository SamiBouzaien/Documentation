#  Microservices avec architecture clean

## Couche application

La couche d'application implémente les cas d'utilisation de l'application en fonction du domaine. Un cas d'utilisation peut être considéré comme une interaction utilisateur sur l'interface utilisateur (UI). Cette couche contient toute la logique d'application. **Il dépend de la couche de domaine**, mais ne dépend d'aucune autre couche ou projet. Cette couche définit les interfaces qui sont implémentées par des couches externes.
La couche application contient :

- Abstractions/Contracts/Interfaces
- Application Services/Handlers
- Commands and Queries
- Exceptions
- Models (DTOs)
- Mappers
- Validators
- Behaviors
- Specifications

**Bibliothèques utilisées :**

Nous avons utilisé Mediatr et FluentValidation pour appliquer CQRS pattern.

- Ajouter fluenValidation au la couche application
```sh
dotnet add package FluentValidation.DependencyInjectionExtensions --version 11.5.2
```
- Ajouter Mediatr a la couche application
```sh
dotnet add package MediatR.Extensions.Microsoft.DependencyInjection --version 11.1.0
```
pour le mappage de données nous avons utilise AutoMapper
```sh
dotnet add package AutoMapper.Extensions.Microsoft.DependencyInjection --version 12.0.1
```
## Couche domaine

La couche de domaine implémente la logique métier principale, indépendante des cas d'utilisation, du domaine/système.Cette couche est hautement abstraite et stable.Elle contient une quantité considérable d'entités de domaine et ne doit pas dépendre de bibliothèques et de frameworks externes. Idéalement, il devrait être faiblement couplé même au .NET Framework.

La couche de domaine contient :
- Entities
- Aggregates
- Value Objects
- Domain Events
- Enums
- Constants
## Couche infrastructure
LCette couche est responsable de la mise en œuvre des Contrats (Interfaces/Adaptateurs) définis au sein de la couche application aux Acteurs Secondaires. La couche d'infrastructure prend en charge d'autres couches en implémentant les abstractions et les intégrations à la bibliothèque et aux systèmes tiers.

La couche d'infrastructure contient la plupart des dépendances de votre application vis-à-vis des ressources externes telles que les systèmes de fichiers, les services Web, les API tierces, etc. La mise en œuvre des services **doit être basée sur des interfaces définies au sein de la couche application**.

La couche infrastructure  contient :

- Identity Services
- File Storage Services
- Queue Storage Services
- Message Bus Services
- Third-party Services
- Notifications
  - Email Service
  - Sms Service
- Data Context
- Repositories
- Data Seeding
- Data Migrations
- Caching (Distributed, In-Memory)

**Bibliothèques utilisées :**
Nous avons ajoute les bibliothèques de EntityFrameworkCore au projet infrastructure pour pouvoir se connecter aux bases de données.
```sh
dotnet add package Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore --version 7.0.5
dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 7.0.5
dotnet add package Microsoft.EntityFrameworkCore.InMemory --version 7.0.5
```

