---
author: mcleanbyron
ms.assetid: dc632a4c-ce48-400b-8e6e-1dddbd13afff
description: "Utilisez cette méthode dans l’API des promotions du Windows Store pour gérer les chaînes de distribution des campagnes publicitaires."
title: "Gérer les chaînes de distribution des campagnes publicitaires"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, API des promotions du Windows Store, campagnes publicitaires
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 5f9ede6a2e645a644e4650f3af7e5476bd52dd53
ms.lasthandoff: 02/08/2017

---

# <a name="manage-delivery-lines-for-ad-campaigns"></a>Gérer les chaînes de distribution des campagnes publicitaires

Appliquez ces méthodes dans l’API des promotions du Windows Store afin de créer une ou plusieurs *chaînes de distribution* vous permettant d’acquérir du stock et de fournir le contenu des campagnes publicitaires. Pour chaque chaîne de distribution, vous pouvez définir le ciblage, définir votre prix de soumission et définir le montant que vous souhaitez dépenser en spécifiant le budget et en lui associant les contenus que vous souhaitez utiliser.

Pour plus d’informations sur la relation entre les profils de ciblage et les campagnes publicitaires, les chaînes de distribution et les contenus, consultez la page [Exécuter des campagnes publicitaires à l’aide des services du Windows Store](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api).

## <a name="prerequisites"></a>Prérequis

Pour utiliser ces méthodes, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà le cas, remplissez toutes les [conditions préalables](run-ad-campaigns-using-windows-store-services.md#prerequisites) relatives à l’API des promotions du Windows Store.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de ces méthodes. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête

Ces méthodes présentent les URI suivants.

| Type de méthode | URI de la requête         |  Description  |
|--------|---------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/line``` |  Crée une nouvelle chaîne de distribution.  |
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/line/{lineId}``` |  Modifie la chaîne de distribution définie par *lineId*.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/line/{lineId}``` |  Obtient la chaîne de distribution définie par *lineId*.  |

<span/> 
### <a name="header"></a>En-tête

| En-tête        | Type   | Description         |
|---------------|--------|---------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |
| Tracking ID   | GUID   | Facultatif. ID de suivi du flux d’appels.                                  |

<span/>
### <a name="request-body"></a>Corps de la requête

Les méthodes POST et PUT nécessitent un corps de requête JSON avec les champs requis d’un objet de [chaîne de distribution](#line) et tous les champs que vous souhaitez définir ou modifier.
 
<span/>
### <a name="request-examples"></a>Exemples de requête

L’exemple suivant vous explique comment appeler la méthode POST pour créer une chaîne de distribution.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/promotion/line HTTP/1.1
Authorization: Bearer <your access token>

{
    "name": "Contoso App Campaign - Paid Line",
    "configuredStatus": "Active",
    "startDateTime": "2017-01-19T12:09:34Z",
    "endDateTime": "2017-01-31T23:59:59Z",
    "bidAmount": 0.4,
    "dailyBudget": 20,
    "targetProfileId": {
        "id": 310021746
    },
    "creatives": [
        {
            "id": 106851
        }
    ],
    "campaignId": 31043481,
    "minMinutesPerImp ": 1
}
```

L’exemple suivant vous explique comment appeler la méthode GET pour récupérer une chaîne de distribution.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/line/31019990  HTTP/1.1
Authorization: Bearer <your access token>
```

<span/> 
## <a name="response"></a>Réponse

Ces méthodes renvoient un corps de réponse JSON avec un objet de [chaîne de distribution](#line) qui comporte les informations sur la chaîne de distribution créée, mise à jour ou récupérée. L’exemple suivant représente un corps de réponse associé à ces méthodes.

```json
{
    "Data": {
        "id": 31043476,
        "name": "Contoso App Campaign - Paid Line",
        "configuredStatus": "Active",
        "effectiveStatus": "Active",
        "effectiveStatusReasons": [
            "{\"ValidationStatusReasons\":null}"
        ],
        "startDateTime": "2017-01-19T12:09:34Z",
        "endDateTime": "2017-01-31T23:59:59Z",
        "createdDateTime": "2017-01-17T10:28:34Z",
        "bidType": "CPM",
        "bidAmount": 0.4,
        "dailyBudget": 20,
        "targetProfileId": {
            "id": 310021746
        },
        "creatives": [
            {
                "id": 106126
            }
        ],
        "campaignId": 31043481,
        "minMinutesPerImp ": 1,
        "pacingType ": "SpendEvenly",
        "currencyId ": 732
    }
}

```

<span id="line"/>
## <a name="delivery-line-object"></a>Objet de chaîne de distribution

Les corps de requête et de réponse associés à ces méthodes comportent les champs suivants. Ce tableau signale les champs en lecture seule (qui ne peuvent être modifiés dans la méthode PUT) et les champs requis dans le corps de requête associé aux méthodes POST et PUT.

| Champ        | Type   |  Description      |  Lecture seule  | Valeur par défaut  | Requis pour les méthodes POST/PUT |   
|--------------|--------|---------------|------|-------------|------------|
|  id   |  entier   |  L’ID de la chaîne de distribution.     |   Oui    |      |  Non      |    
|  name   |  chaîne   |   Nom de la chaîne de distribution.    |    Non   |      |  POST     |     
|  configuredStatus   |  chaîne   |  L’une des valeurs suivantes qui spécifie le statut de la chaîne de distribution définie par le développeur : <ul><li>**Active**</li><li>**Inactive**</li></ul>     |  Non     |      |   POST    |       
|  effectiveStatus   |  chaîne   |   L’une des valeurs suivantes qui spécifie le statut effectif de la chaîne de distribution, suivant la validation du système : <ul><li>**Active**</li><li>**Inactive**</li><li>**Processing**</li><li>**Failed**</li></ul>    |    Oui   |      |  Non      |      
|  effectiveStatusReasons   |  tableau   |  L’une ou plusieurs des valeurs suivantes qui spécifient le motif du statut effectif de la chaîne de distribution : <ul><li>**AdCreativesInactive**</li><li>**ValidationFailed**</li></ul>      |  Oui     |     |    Non    |           
|  startDatetime   |  chaîne   |  L’heure et la date de début de la chaîne de distribution, au format ISO 8601. Cette valeur ne peut pas être modifiée si elle se situe dans le passé.     |    Non   |      |    POST, PUT     |
|  endDatetime   |  chaîne   |  L’heure et la date de fin de la chaîne de distribution, au format ISO 8601. Cette valeur ne peut pas être modifiée si elle se situe dans le passé.     |   Non    |      |  POST, PUT     |
|  createdDatetime   |  chaîne   |  L’heure et la date de la chaîne de distribution créée, au format ISO 8601.      |    Oui   |      |  Non      |
|  bidType   |  chaîne   |  Une valeur spécifiant le type d’enchères de la chaîne de distribution. Actuellement, la seule valeur prise en charge est **CPM**.      |    Non   |  CPM    |  Non     |
|  bidAmount   |  décimale   |  Le montant misé sur une requête publicitaire.      |    Non   |  La valeur CPM moyenne suivant les marchés cibles (cette valeur est régulièrement revue).    |    Non    |  
|  dailyBudget   |  décimale   |  Le budget quotidien de la chaîne de distribution. Les valeurs *dailyBudget* ou *lifetimeBudget* doivent être définies.      |    Non   |      |   POST, PUT (si *lifetimeBudget* n’est pas définie)       |
|  lifetimeBudget   |  décimale   |   Le budget à vie de la chaîne de distribution. Les valeurs lifetimeBudget* ou *dailyBudget* doivent être définies.      |    Non   |      |   POST, PUT (si *dailyBudget* n’est pas définie)    |
|  targetingProfileId   |  objet   |  Sur l’objet identifiant le [profil de ciblage](manage-targeting-profiles-for-ad-campaigns.md#targeting-profile) décrivant les utilisateurs, les zones géographiques et les types d’inventaire que vous souhaitez cibler pour cette chaîne de distribution. Cet objet est composé d’un seul champ *id* qui définit l’ID du profil de ciblage.     |    Non   |      |  Non      |  
|  creatives   |  tableau   |  Un ou plusieurs objets représentant les [contenus](manage-creatives-for-ad-campaigns.md#creative) associés à la chaîne de distribution. Chacun des objets de ce champ est composé d’un seul champ *id* définissant l’ID d’un contenu.      |    Non   |      |   Non     |  
|  campaignId   |  entier   |  L’ID de la campagne publicitaire parente.      |    Non   |      |   Non     |  
|  minMinutesPerImp   |  entier   |  Spécifie l’intervalle minimum (en minutes) entre 2 impressions communiquées à un utilisateur de cette chaîne de distribution.      |    Non   |  4000    |  Non      |  
|  pacingType   |  chaîne   |  Une des valeurs suivantes spécifiant le type d’allure : <ul><li>**SpendEvenly**</li><li>**SpendAsFastAsPossible**</li></ul>      |    Non   |  SpendEvenly    |  Non      |
|  currencyId   |  entier   |  ID de la devise de la campagne.      |    Oui   |  Devise du compte développeur (il n’est pas nécessaire de renseigner ce champ dans les appels POST et PUT).    |   Non     |      |


## <a name="related-topics"></a>Rubriques connexes

* [Exécuter des campagnes publicitaires à l’aide des services du Windows Store](run-ad-campaigns-using-windows-store-services.md)
* [Gérer les campagnes publicitaires](manage-ad-campaigns.md)
* [Gérer les profils de ciblage pour les campagnes publicitaires](manage-targeting-profiles-for-ad-campaigns.md)
* [Gérer les contenus des campagnes publicitaires](manage-creatives-for-ad-campaigns.md)
* [Obtenir les données relatives aux performances des campagnes publicitaires](get-ad-campaign-performance-data.md)
