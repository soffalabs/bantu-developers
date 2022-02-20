---
order: 95
icon: note
title: Applications
---
# Vos applications

Bantu est une solution qui vous permet de créer plusieurs applications pour servir des besoins fonctionnels différents.<br />
Nous publions chaque mois de nouvelles API pour disposer d'un riche catalogue.


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

## Consommer l'Api Todos

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

<img src="/static/img/bantu_todos_keys.png" alt="" width="500" />

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

