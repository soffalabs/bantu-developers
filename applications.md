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
    "pkTest": "pk_test_75UcmFeohlyQmwPlSd--w", # Clé privée de production
    "skTest": "sk_test_woNOBFe94pe5_ErHkyWqKE", # Clé publique de production
    "pkLive": "pk_live_uyO9mdn_kGi9ov0yJVD6v", # Clé privée de test
    "skLive": "sk_live_85XknhLMcGS67A3ch6p91", # Clé publique de test
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
2. Rajouter *une* TODO (si l'on considère que TODO est une action/tâche)
3. Marquer une TODO comme terminée