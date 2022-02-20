---
order: 100
icon: code-square
title: Bien démarrer
---
# Démarrer

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

