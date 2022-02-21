---
order: 100
icon: code-square
title: Bien démarrer
---

Les points d'entrées suivants sont mis à votre disposition :

- https://sandbox.bantu.dev  - Sandbox (Cet environnement de test est réinitialisé de façon régulière)
- https://api.bantu.dev  - Production (Disponible plus tard)


Pour développer avec Bantu, les étapes suivantes sont à suivre:

1. Créer un compte développeur pour récupérer une clé d'API de type **Account** (`ak_`)
2. Créer une application pour récupérer des clés d'API de type **Application** (`sk_` ou `pk_`)
3. Utiliser les clés d'api de type application pour exploiter le reste du catalogue d'APIs

!!!
Les différents exemples de requêtes  sont construits avec <a href="https://code.visualstudio.com" target="_blank" alt="">Visual Studio Code</a> et
le plugin <a href="https://marketplace.visualstudio.com/items?itemName=humao.rest-client" target="_blank">RestClient</a>
!!!



## Compte développeur

La création d'un compte développeur se fait grâce au service <a href="https://sandbox.bantu.dev/accounts/swagger-ui.html" target="_blank">Accounts</a>.



```graphql
# rest client (vscode)
@baseUrl = https://sandbox.bantu.dev

## Créer votre compte développeur

POST {{baseUrl}}/accounts
Content-Type: application/json

{
    "name": "account-name", # Nom du compte développeur
    "email": "developer@email.com" # Email utilisée pour vous transmettre votre clé d'API.
}
```


Si votre requête est valide, vous recevrez une réponse JSON avec la structure suivante:

```json
{
  "accountId": "acc_1ib0yvl75q92b", # Identifiant unique de votre compte
  "message": "The api-key was sent to the provided email address"
}
```

!!! 
Nous transmettons la clé d'API à votre email pour en effectuer la vérification.
!!!

Lorsque la création du comtpe développeur aboutit, le mail transmis contient les informations suivantes:


``` Exemple d'email
Your account has been created.
Your account id is acc_t4htyy32n1va
Your api key is ak_VrDi2KTxKQeR5Zf9DV6GX
```

> Le template de mail sera plus user-friendly avant notre passage en production :)

Il est donc important de bien conserver ces informations. En cas d'oubli, il est possible demander un renvoi de votre clé d'API Account par email (la même fournie lors de la création).


!!!warning
Arpès création de votre compte développeur, si la clé d'API n'est pas utilisée pour créer une application au bout de 7 jours, le compte est
automatiquement détruit avec un email de notification quelques jours à l'avance (offre cloud uniquement).
!!!

### Unicité de l'email

Parce que l'email utilisée lors de la création du compte est unique, si vous tentez de réutiliser une adresse existante,
la demande sera rejetée avec la réponse suivante :

```json
{
  "timestamp": "2022-02-08T21:23:04.653+00:00",
  "timestamp": "2022-02-08T21:23:04.653+00:00",
  "source": "bantu-accounts",
  "kind": "ConflictException",
  "status": 409,
  "message": "account.email.exists",
  "prod": true,
  "traceId": "aaa1ec78-ddcc-4b01-b8e8-f4436fd967db", 
  "spanId": "b779a13d-4d2d-46f5-b68e-1c783b6b441e",
  "tenant": "default"
}
```



## Créer une application

Un compte ne vous permet pas de faire grand chose sur la plateforme. 
L'exploitation commence par la création d'une application qui a pour effet de :

1. Créer de deux schémas de données (production + test) dédiés à votre projet
2. Générer des clés applicatives pour vous permettre de manipuler les APIs du catalogue en modes **live** (production) et **test**.


!!!warning
Lorsqu'un compte ou une application est supprimé.e, toutes les données associées sont supprimées au bout de 7 jours.
Durant cette période de grâce, il est toujours possible de revenir sur votre décision.
!!!


```graphql
@baseUrl = https://sandbox.bantu.dev
@apikey = ak_00000000000 # remplacer par votre api key (Account)

### ------------------------------------------------------------------------------
### Créer une application
### ------------------------------------------------------------------------------

POST {{baseUrl}}/accounts/v1/applications
Content-Type: application/json
Authorization: Basic {{apikey}}:

{
    "name": "my-application", # Nom de l'application (obligatoire en minuscules)
    "description": "Application description" # Courte description (obligatoire)
}
```


Si votre requête est valide, la création de l'application retourne une réponse semblale à la suivante:

```json
{
  "application": {
    "id": "app_1ib9893gdtlcz", # Identifiant unique de votre application
    "accountId": "acc_1iukwdd80pjxs", # ID du compte auquel est lié cette application
    "name": "test-app", # Nom de l'application
    "description": "Hello world", # Description de l'application
    "pkTest": "pk_test_75UcmFeohlyQmwPlSd--w", # Clé publique de test
    "skTest": "sk_test_woNOBFe94pe5_ErHkyWqKE", # Clé privée de test
    "pkLive": "pk_live_uyO9mdn_kGi9ov0yJVD6v", # Clé publique de production
    "skLive": "sk_live_85XknhLMcGS67A3ch6p91", # Clé privée de production
    "createdAt": "2022-02-08T21:58:10.689+00:00" # Date de création du compte
  }
}
```

## Consommer un Produit

Nous avons mis en place un service **Todos** à des fins purement éducatives.
Ce simple service vous permet de comprendre rapidement les principes de bases qui s'appliquent  à tout le catalogue de services.

La documentation (OpenAPI) du service est <a href="https://sandbox.bantu.dev/todos/swagger-ui.html" target="_blank">disponible ici</a>.

Trois opérations sont disponibles :

1. Visualiser les TODO
2. Rajouter *une TODO (si l'on considère que TODO est une action/tâche)
3. Marquer une TODO comme terminée


Dans les informations qui sont retournées après création de l'application, les couples suivants sont les plus importants:

1. `id + skTest` vous permet d'accéder à votre environnement de **test**
2. `id + skLive` vous permet d'accéder à votre environnement de **live**

<picture>
  <source
    srcset="/static/img/bantu_todos_keys_dark.png"
    media="(prefers-color-scheme: dark)">
  <img src="/static/img/bantu_todos_keys.png" alt="" width="780" />
</picture>

Ainsi, pour récupér la liste des TODOS ou rajouter une TODO dans votre environnement de test, la requête suivante est valide :

```graphql
@baseUrl = https://sandbox.bantu.dev/todos
@appId = app_1ib9893gdtlcz
@secret = sk_test_woNOBFe94pe5_ErHkyWqKE

### GetTodoList 
GET {{baseUrl}}/v1
Authorization: Basic {{appId}}:{{secret}}

### AddTodo

POST {{baseUrl}}/v1
Authorization: Basic {{appId}}:{{secret}}
Content-Type: application/json

{
    "content": "Product roadmap for the next quarter"
}

```

Pour travailler dans votre environnement de production (live), vous l'aurez compris, la seule ligne à changer est 
```graphql
@secret = sk_live_85XknhLMcGS67A3ch6p91
```

!!!
À ce stade vous devriez disposer de suffisamment d'information pour créer un compte, une application et commencer à manipuler
l'API Todos. En cas d'anomalie
!!!

