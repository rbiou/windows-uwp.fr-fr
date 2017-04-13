---
author: mcleanbyron
ms.assetid: 7b07a6ca-4be1-497c-a901-0a2da3762555
description: "Utilisez cette méthode dans l’API des promotions du Windows Store pour créer, modifier et obtenir des campagnes publicitaires."
title: "Gérer les campagnes publicitaires"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, API des promotions du Windows Store, campagnes publicitaires
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: bde9588176c1e52ccab169ad3f51ad15781e06ee
ms.lasthandoff: 02/08/2017

---

# <a name="manage-ad-campaigns"></a>Gérer les campagnes publicitaires

Appliquez ces méthodes dans l’[API des promotions du Windows Store](run-ad-campaigns-using-windows-store-services.md) pour créer, modifier et obtenir des campagnes publicitaires pour votre application. Chaque campagne créée via cette méthode peut être associée à une application uniquement.

>**Remarque**&nbsp;&nbsp;Il est également possible de créer et de gérer des campagnes publicitaires via le tableau de bord du Centre de développement Windows. Le cas échéant, les campagnes générées par programme sont accessibles dans ledit tableau de bord. Pour plus d’informations sur la gestion des campagnes publicitaires dans le tableau de bord, consultez la section [Création d’une campagne de publicité pour votre application](../publish/create-an-ad-campaign-for-your-app.md).

Lorsque vous appliquez ces méthodes pour créer ou mettre à jour une campagne, vous appelez généralement l’une des méthodes suivantes pour gérer les *chaînes de distribution*, les *profils de ciblage* et les *contenus* associés à la campagne. Pour plus d’informations sur la relation entre les profils de ciblage et les campagnes publicitaires, les chaînes de distribution et les contenus, consultez la page [Exécuter des campagnes publicitaires à l’aide des services du Windows Store](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api).

* [Gérer les chaînes de distribution des campagnes publicitaires](manage-delivery-lines-for-ad-campaigns.md)
* [Gérer les profils de ciblage pour les campagnes publicitaires](manage-targeting-profiles-for-ad-campaigns.md)
* [Gérer les contenus des campagnes publicitaires](manage-creatives-for-ad-campaigns.md)

## <a name="prerequisites"></a>Prérequis

Pour utiliser ces méthodes, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà le cas, remplissez toutes les [conditions préalables](run-ad-campaigns-using-windows-store-services.md#prerequisites) relatives à l’API des promotions du Windows Store.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de ces méthodes. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

<span/> 
## <a name="request"></a>Requête

Ces méthodes présentent les URI suivants.

| Type de méthode | URI de la requête                                                      |  Description  |
|--------|------------------------------------------------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign``` |  Crée une campagne publicitaire.  |
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign/{campaignId}``` |  Modifie la campagne publicitaire définie par *campaignId*.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign/{campaignId}``` |  Obtient la campagne publicitaire définie par *campaignId*.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign``` |  Recherche des campagnes publicitaires. Consultez la section [Paramètres](#parameters) pour en savoir plus sur les paramètres de requête pris en charge.  |

<span/> 
### <a name="header"></a>En-tête

| En-tête        | Type   | Description         |
|---------------|--------|---------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |
| ID de suivi   | GUID   | Facultatif. ID de suivi du flux d’appels.                                  |

<span id="parameters"/> 
### <a name="parameters"></a>Paramètres

La méthode GET d’interrogation des campagnes publicitaires prend en charge les paramètres de requête facultatifs suivants.

| Nom        | Type   |  Description      |    
|-------------|--------|---------------|------|
| skip  |  entier   | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir des ensembles de données. Par exemple, indiquez fetch=10 et skip=0 pour obtenir les 10 premières lignes de données, top=10 et skip=10 pour obtenir les 10 lignes suivantes, et ainsi de suite.    |       
| fetch  |  int   | Le nombre de lignes de données à renvoyer dans la requête.    |       
| campaignSetSortColumn  |  chaîne   | Commande les objets [Campaign](#campaign) dans le corps de la réponse, via le champ spécifié. La syntaxe est <em>CampaignSetSortColumn=field</em>, où le paramètre <em>field</em> peut être l’une des chaînes suivantes :</p><ul><li><strong>id</strong></li><li><strong>createdDateTime</strong></li></ul><p>La valeur par défaut est **createdDateTime**.     |     
| isDescending  |  valeur booléenne   | Trie les objets [Campaign](#campaign) dans le corps de la réponse, par ordre croissant ou décroissant.   |         
| applicationId  |  chaîne   | Utilisez cette valeur pour renvoyer uniquement les campagnes publicitaires associées à l’application présentant l’[ID du Windows Store](in-app-purchases-and-trials.md#store-ids) spécifié. Un produit peut par exemple présenter l’ID 9nblggh42cfd du Windows Store.   |         
| label  |  chaîne   | Cette valeur renvoie uniquement les campagnes publicitaires comportant l’élément *label* spécifié dans l’objet [Campaign](#campaign).    |       |    

<span/>
### <a name="request-body"></a>Corps de demande

Les méthodes POST et PUT nécessitent un corps de requête JSON avec les champs requis d’un objet [Campaign](#campaign) et les champs supplémentaires que vous souhaitez définir ou modifier.

<span/>
### <a name="request-examples"></a>Exemples de requête

L’exemple suivant représente l’appel d’une méthode POST pour créer une campagne publicitaire.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign HTTP/1.1
Authorization: Bearer <your access token>

{
    "name": "Contoso App Campaign",
    "applicationId": "9nblggh42cfd",
    "configuredStatus": "Active",
    "objective": "DriveInstalls",
    "type": "Community"
}
```

L’exemple suivant représente l’appel d’une méthode GET pour récupérer une campagne publicitaire spécifique.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign/31043481  HTTP/1.1
Authorization: Bearer <your access token>
```

L’exemple suivant représente l’appel de la méthode GET pour rechercher un ensemble de campagnes publicitaires, triées par dates de création.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign?applicationId=9nblggh42cfd&fetch=100&skip=0&campaignSetSortColumn=createdDateTime HTTP/1.1
Authorization: Bearer <your access token>
```

<span/> 
## <a name="response"></a>Réponse

Ces méthodes renvoient un corps de réponse JSON avec un ou plusieurs objets [Campaign](#campaign), suivant la méthode appelée. L’exemple suivant représente un corps de réponse pour la méthode GET d’une campagne spécifique.

```json
{
    "Data": {
        "id": 31043481,
        "name": "Contoso App Campaign",
        "createdDate": "2017-01-17T10:12:15Z",
        "applicationId": "9nblggh42cfd",
        "configuredStatus": "Active",
        "effectiveStatus": "Active",
        "effectiveStatusReasons": [
            "{\"ValidationStatusReasons\":null}"
        ],
        "labels": [],
        "objective": "DriveInstalls",
        "type": "Paid",
        "lines": [
            {
                "id": 31043476,
                "name": "Contoso App Campaign - Paid Line"
            }
        ]
    }
}
```

<span id="campaign"/>
## <a name="campaign-object"></a>Objet de campagne

Les corps de requête et de réponse associés à ces méthodes comportent les champs suivants. Ce tableau signale les champs en lecture seule (qui ne peuvent être modifiés dans la méthode PUT) et les champs requis dans le corps de requête associé à la méthode POST.

| Champ        | Type   |  Description      |  Lecture seule  | Valeur par défaut  | Requis pour la méthode POST |  
|--------------|--------|---------------|------|-------------|------------|
|  id   |  entier   |  L’ID de la campagne publicitaire.     |   Oui    |      |  Non     |       
|  name   |  chaîne   |   Nom de la campagne publicitaire.    |    Non   |      |  Oui     |       
|  configuredStatus   |  chaîne   |  L’une des valeurs suivantes qui spécifie le statut de la campagne publicitaire définie par le développeur : <ul><li>**Active**</li><li>**Inactive**</li></ul>     |  Non     |  Active    |   Oui    |       
|  effectiveStatus   |  chaîne   |   L’une des valeurs suivantes qui spécifie le statut effectif de la campagne publicitaire, suivant la validation du système : <ul><li>**Active**</li><li>**Inactive**</li><li>**Processing**</li></ul>    |    Oui   |      |   Non      |       
|  effectiveStatusReasons   |  tableau   |  L’une ou plusieurs des valeurs suivantes qui spécifient le motif du statut effectif de la campagne publicitaire : <ul><li>**AdCreativesInactive**</li><li>**BillingFailed**</li><li>**AdLinesInactive**</li><li>**ValidationFailed**</li><li>**Failed**</li></ul>      |  Oui     |     |    Non     |       
|  applicationId   |  chaîne   |  L’[ID du Windows Store](in-app-purchases-and-trials.md#store-ids) pour l’application à laquelle est associée cette campagne publicitaire. Un produit peut par exemple présenter l’ID 9nblggh42cfd du Windows Store.     |   Oui    |      |  Oui     |       
|  labels   |  tableau   |   Une ou plusieurs chaînes représentant les légendes personnalisées de la campagne. Ces légendes sont utilisées pour rechercher et marquer des campagnes.    |   Non    |  null    |    Non     |       
|  type   | chaîne    |  L’une des valeurs suivantes spécifiant le type de campagne : <ul><li>**Paid**</li><li>**House**</li><li>**Community**</li></ul>      |   Oui    |      |   Oui    |       
|  objective   |  chaîne   |  L’une des valeurs suivantes spécifiant l’objectif de la campagne : <ul><li>**DriveInstall**</li><li>**DriveReengagement**</li><li>**DriveInAppPurchase**</li></ul>     |   Non    |  DriveInstall    |   Oui    |       
|  lines   |  tableau   |   Un ou plusieurs objets identifiant les [chaînes de distribution](manage-delivery-lines-for-ad-campaigns.md#line) associées à la campagne publicitaire. Les objets de ce champ sont constitués de champs *id* et *name* définissant l’ID et le nom de la chaîne de distribution.     |   Non    |      |    Non     |       
|  createdDate   |  chaîne   |  La date et l’heure de création de la campagne publicitaire, au format ISO 8601.     |  Oui     |      |     Non    |       |

## <a name="related-topics"></a>Rubriques connexes

* [Exécuter des campagnes publicitaires à l’aide des services du Windows Store](run-ad-campaigns-using-windows-store-services.md)
* [Gérer les chaînes de distribution des campagnes publicitaires](manage-delivery-lines-for-ad-campaigns.md)
* [Gérer les profils de ciblage pour les campagnes publicitaires](manage-targeting-profiles-for-ad-campaigns.md)
* [Gérer les contenus des campagnes publicitaires](manage-creatives-for-ad-campaigns.md)
* [Obtenir les données relatives aux performances des campagnes publicitaires](get-ad-campaign-performance-data.md)
