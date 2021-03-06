---
ms.assetid: DAF92881-6AF6-44C7-B466-215F5226AE04
description: Utilisez cette méthode dans l’API de soumission de Microsoft Store pour récupérer des informations sur une application spécifique qui est inscrit pour votre compte espace partenaires.
title: Obtenir une application
ms.date: 02/28/2018
ms.topic: article
keywords: windows 10, uwp, API de soumission au Microsoft Store, app
ms.localizationpriority: medium
ms.openlocfilehash: 96efc6145d382d4f6a996e541d638f2a1d896e80
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372663"
---
# <a name="get-an-app"></a>Obtenir une application

Utilisez cette méthode dans l’API de soumission de Microsoft Store pour récupérer des informations sur une application spécifique qui est inscrit pour votre compte espace partenaires.

## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà le cas, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission au Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Demande

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de requête                                                      |
|--------|------------------------------------------------------------------|
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}` |


### <a name="request-header"></a>En-tête de requête

| Header        | type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Le jeton d’accès Azure AD sous la forme **PORTEUR** &lt; *jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Nom        | type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | chaîne | Obligatoire. ID Windows Store de l’application à récupérer. Pour plus d’informations sur l’ID Windows Store, voir [Visualiser les informations d’identité des applications](https://docs.microsoft.com/windows/uwp/publish/view-app-identity-details).  |


### <a name="request-body"></a>Corps de la requête

Ne fournissez pas de corps de requête pour cette méthode.

### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment récupérer des informations sur une app dont la valeur de l’ID Microsoft Store est 9NBLGGH4R315.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse

L’exemple suivant illustre le corps de réponse JSON d’un appel réussi à cette méthode. Pour plus d’informations sur les valeurs figurant dans le corps de la réponse, voir [Ressource d’application](get-app-data.md#application_object).

```json
{
  "id": "9NBLGGH4R315",
  "primaryName": "ApiTestApp",
  "packageFamilyName": "30481DevCenterAPITester.ApiTestAppForDevbox_ng6try80pwt52",
  "packageIdentityName": "30481DevCenterAPITester.ApiTestAppForDevbox",
  "publisherName": "CN=…",
  "firstPublishedDate": "1601-01-01T00:00:00Z",
  "lastPublishedApplicationSubmission": {
    "id": "1152921504621086517",
    "resourceLocation": "applications/9NBLGGH4R315/submissions/1152921504621086517"
  },
  "pendingApplicationSubmission": {
    "id": "1152921504621243487",
    "resourceLocation": "applications/9NBLGGH4R315/submissions/1152921504621243487"
  },
  "hasAdvancedListingPermission": false
}
```

## <a name="error-codes"></a>Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Error code |  Description   |
|--------|------------------|
| 404  | L’application spécifiée est introuvable. |
| 409  | L’application utilise une fonctionnalité de partenaires est [actuellement ne pas pris en charge par l’API de soumission de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported).  |


## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des envois à l’aide des services de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Obtenir toutes les applications](get-all-apps.md)
* [Obtenir les vols de package pour une application](get-flights-for-an-app.md)
* [Obtenir des modules complémentaires pour une application](get-add-ons-for-an-app.md)
