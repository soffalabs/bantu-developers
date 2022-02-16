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

1. Créer une base de données (ou schéma) dédié.e à votre projet
2. Générer des clés applicatives pour vous permettre de manipuler les APIs du catalogue


!!!warning
Lorsqu'un compte est supprimé, toutes les applications associées sont supprimées.
Il n'est pas possible de revenir en arrière ou restaurer une sauvegarde.

De même, lorsqu'une application est supprimée, la base de données liée à cette
application est détruite de façon définitive.
!!!


+++ rest-client (vscode)
```graphql
@baseUrl = https://api.bantu.dev
@apikey = ak_00000000000 # remplacer par votre api key (compte)

### ------------------------------------------------------------------------------
### Créer votre compte développeur
### ------------------------------------------------------------------------------

POST {{baseUrl}}/accounts/v1/applications
Content-Type: application/json
Authorization: Basic {{apikey}}:

{
    "name": "my-application", # Nom de l'application (obligatoire en minuscules)
    "description": "Application description" # Courte description (obligatoire)
}
```
+++


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

## Mettre à jour les informations d'une application

## Supprimer une application