---
title: Partenaires
icon: dash
---


Un partenaire est un service utilisé pour effectuer un paiement auprès d'un utilisateur en passant par un canal spécifique.

## Activer un partenaire

Cette action est exécuter pour activer un partenaire ou changer les informations d'un partenaire.

!!!warning
Une apiKey de type **account-admin** est requise pour accéder à cette ressource
!!!

```graphql

@baseUrl = https://sandbox.bantu.dev/payments
@apikey = rak_00000000000

# ConfigureGateway
PUT {{baseUrl}}/v1/partners/:partner-id
Content-Type: application/json
Authorization: Basic {{apikey}}:

{
    "url": "https://url-partner"
}

```

Les `partner-id` supportés actuellement sont:

- [x] `orange-money`
- [ ] `sama-money`
- [ ] `paypal`

## Statut d'un partenaire

Pour un partenaire activé, l'opération suivante permettre de vérifier si la communication (réseau) avec ce partenaire est fonctionnelle.

!!!
Cette ressource est accessible sans authentification
!!!

```graphql
@baseUrl = https://sandbox.bantu.dev/payments
GET {{baseUrl}}/v1/partners/:partner-id/status
```


## Configurer un partenaire

Pour intégrer des paiements à votre application, il est nécessaire de configurer un partenaire (préalablement activé).
Cette configuration consite à fournir les informations d'authentification propres à votre usage.

En effet, Si **Bantu** permet aux développeurs d'effectuer des paiements via Paypal par exemple, il est de la responsabilité
du développeur (ou de l'entreprise) de créer le compte Paypal et de renseigner les informations nécessaires pour communiquer
avec l'API Paypal.

Cette ressource `ConfigurePartner` vous permet de fournir ces informations en question.


!!!
Cette ressource est accessible avec votre cle `skTest` ou `skLive` (clés d'application)
!!!


```graphql

@baseUrl = https://sandbox.bantu.dev/payments
@apikey = sk_xxxxxxxxxxxxxxxx

# ConfigureGateway
PUT {{baseUrl}}/v1/partners/:partner-id/config
Content-Type: application/json
Authorization: Basic {{apikey}}:

{
    clientId: "xxxxxxx", 
    clientSecret: "xxxxxxxx", 
    merchantId: "xxxxxxxxx",
    token: "xxxxxxxxxxxx"
}

```

| Configuration | orange-money  | sama-money    |  paypal   |
|-              |-              |-              |-          |
| `clientId`    | Obligatoire   |  Obligatoire  |           |
| `clientSecret`| Obligatoire   |  Obligatoire  |           |
| `merchantId`  | Obligatoire   |  Obligatoire  |           |
| `token`       |               |  Obligatoire  |           |


Les informations transmises sont validées avant d'être enregistrées.
En cas de donnée absente, vous recevrez donc un message explicite.

## Vérifier la configuration 

Une fois le partnaire configurée pour votre instance applicative, vous pouvez configurer un monitoring en
consommant la ressource suivante avec votre clé publique.

!!!
Cette ressource est accessible avec votre cle `pkTest` ou `pkLive` (clés d'application)
!!!


```graphql

@baseUrl = https://sandbox.bantu.dev/payments
@apikey = pk_xxxxxxxxxxxxxxxx

# CheckPartnerStatus
GET {{baseUrl}}/v1/partners/:partner-id/check
Content-Type: application/json
Authorization: Basic {{apikey}}:

```