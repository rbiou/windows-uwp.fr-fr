---
ms.assetid: E59FB6FE-5318-46DF-B050-73F599C3972A
description: Utilisez cette méthode dans l’API de soumission de Microsoft Store pour récupérer des informations sur les achats dans l’application pour une application inscrite auprès de vos partenaires.
title: Obtenir des extensions pour une application
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, API de soumission au Microsoft Store, extensions, produit in-app, PIA
ms.localizationpriority: medium
ms.openlocfilehash: 35b30d5760cb734fcdbd2df552ca5c5609414709
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372170"
---
# <a name="get-add-ons-for-an-app"></a>Obtenir des extensions pour une application

Utilisez cette méthode dans l’API de soumission de Microsoft Store pour répertorier les modules complémentaires pour une application qui est inscrit pour votre compte espace partenaires.

## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà le cas, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission au Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Demande

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de requête                                                      |
|--------|------------------------------------------------------------------|
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listinappproducts` |


### <a name="request-header"></a>En-tête de requête

| Header        | type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Le jeton d’accès Azure AD sous la forme **PORTEUR** &lt; *jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête


|  Nom  |  type  |  Description  |  Obligatoire  |
|------|------|------|------|
|  applicationId  |  chaîne  |  ID Windows Store de l’application pour laquelle vous voulez récupérer les extensions. Pour plus d’informations sur l’ID Windows Store, voir [Visualiser les informations d’identité des applications](https://docs.microsoft.com/windows/uwp/publish/view-app-identity-details).  |  Oui  |
|  top  |  entier  |  Nombre d’éléments à retourner dans la requête (autrement dit, nombre d’extensions à retourner). Si l’application comporte plus d’extensions que la valeur que vous spécifiez dans la requête, le corps de la réponse comprend un chemin d’URI relatif que vous pouvez ajouter à l’URI de la méthode pour solliciter la page suivante de données.  |  Non  |
|  skip |  entier  | Nombre d’éléments à ignorer dans la requête avant de retourner les éléments restants. Utilisez ce paramètre pour parcourir des ensembles de données. Par exemple, top=10 et skip=0 permettent de récupérer les éléments 1 à 10, top=10 et skip=10 permettent de récupérer les éléments 11 à 20, et ainsi de suite.   |  Non  |


### <a name="request-body"></a>Corps de la requête

Ne fournissez pas de corps de requête pour cette méthode.

### <a name="request-examples"></a>Exemples de demande

L’exemple suivant montre comment répertorier toutes les extensions pour une application.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/listinappproducts HTTP/1.1
Authorization: Bearer <your access token>
```

L’exemple suivant montre comment répertorier les 10 premières extensions pour une application.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/listinappproducts?top=10 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse

L’exemple suivant montre le corps de réponse JSON renvoyé par une requête réussie sur les 10 premières extensions d’une application qui en possède 53 au total. Pour des raisons de concision, cet exemple affiche uniquement les données des trois premières extensions retournées par la requête. Pour plus d’informations sur les valeurs figurant dans le corps de réponse, voir la section suivante.

```json
{
  "@nextLink": "applications/9NBLGGH4R315/listinappproducts/?skip=10&top=10",
  "value": [
    {
      "inAppProductId": "9NBLGGH4TNMP"
    },
    {
      "inAppProductId": "9NBLGGH4TNMN"
    },
    {
      "inAppProductId": "9NBLGGH4TNNR"
    },
    // Next 7 add-ons  are omitted for brevity ...
  ],
  "totalCount": 53
}
```

### <a name="response-body"></a>Corps de la réponse

| Value      | type   | Description                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| @nextLink  | chaîne | S’il existe des pages supplémentaires de données, cette chaîne contient un chemin relatif que vous pouvez ajouter à l’URI de requête `https://manage.devcenter.microsoft.com/v1.0/my/` de base pour solliciter la page suivante de données. Par exemple, si le paramètre *top* du corps de requête initial a la valeur 10, mais qu’il existe 50 extensions pour l’application, le corps de réponse comprendra une valeur @nextLink de `applications/{applicationid}/listinappproducts/?skip=10&top=10`, ce qui indique que vous pouvez appeler `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationid}/listinappproducts/?skip=10&top=10` pour solliciter les 10 extensions suivantes. |
| valeur      | tableau  | Tableau d’objets qui répertorie l’ID Windows Store de chaque extension pour l’application spécifiée. Pour plus d’informations sur les données incluses dans chaque objet, voir [ressource d’extension](get-app-data.md#add-on-object).                                                                                                                           |
| totalCount | entier    | Nombre total de lignes dans les résultats de données pour la requête (autrement dit, nombre total d’extensions pour l’application spécifiée).    |


## <a name="error-codes"></a>Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Error code |  Description   |
|--------|------------------|
| 404  | Aucune extension n’a été trouvée. |
| 409  | Les modules complémentaires utilisent les fonctionnalités de partenaires qui sont [actuellement ne pas pris en charge par l’API de soumission de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported).  |


## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des envois à l’aide des services de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Obtenir toutes les applications](get-all-apps.md)
* [Obtenez une application](get-an-app.md)
* [Obtenir les vols de package pour une application](get-flights-for-an-app.md)
