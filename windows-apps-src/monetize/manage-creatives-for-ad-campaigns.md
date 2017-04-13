---
author: mcleanbyron
ms.assetid: c5246681-82c7-44df-87e1-a84a926e6496
description: "Utilisez cette méthode dans l’API des promotions du Windows Store pour gérer les contenus des campagnes publicitaires."
title: "Gérer les contenus des campagnes publicitaires"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, API des promotions du Windows Store, campagnes publicitaires
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 1e0755134a47b6acfb48f735ea56c4aa3c46be14
ms.lasthandoff: 02/08/2017

---

# <a name="manage-creatives-for-ad-campaigns"></a>Gérer les contenus des campagnes publicitaires

Appliquez ces méthodes dans l’API des promotions du Windows Store pour charger vos propres contenus personnalisés à utiliser dans les campagnes publicitaires ou pour obtenir des contenus existants. Un contenu peut être associé à plusieurs chaînes de distribution, dans plusieurs campagnes, sous réserve qu’il représente toujours la même application.

Pour plus d’informations sur la relation entre les contenus et les campagnes publicitaires, les chaînes de distribution et les profils de ciblage, consultez la page [Exécuter des campagnes publicitaires à l’aide des services du Windows Store](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api).

## <a name="prerequisites"></a>Prérequis

Pour utiliser ces méthodes, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà le cas, remplissez toutes les [conditions préalables](run-ad-campaigns-using-windows-store-services.md#prerequisites) relatives à l’API des promotions du Windows Store.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de ces méthodes. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête

Ces méthodes présentent les URI suivants.

| Type de méthode | URI de la requête     |  Description  |
|--------|-----------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative``` |  Crée un nouveau contenu.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative/{creativeId}``` |  Obtient le contenu défini par *creativeId*.  |

>**Remarque**&nbsp;&nbsp;Cette API ne prend actuellement pas en charge de méthode PUT.

<span/> 
### <a name="header"></a>En-tête

| En-tête        | Type   | Description         |
|---------------|--------|---------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |
| Tracking ID   | GUID   | Facultatif. ID de suivi du flux d’appels.                                  |


<span/>
### <a name="request-body"></a>Corps de requête

La méthode POST nécessite un corps de requête JSON avec les champs requis d’un objet de [contenu](#creative).

<span/>
### <a name="request-examples"></a>Exemples de requête

L’exemple suivant vous explique comment appeler la méthode POST pour créer un élément de contenu. Dans cet exemple, la valeur *content* a été raccourcie, par souci de concision.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative HTTP/1.1
Authorization: Bearer <your access token>

{
  "name": "Contoso App Campaign - Creative 1",
  "content": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEAAQABAAD/2wBDAAgGB...other base64 data shortened for brevity...",
  "height": 80,
  "width": 480,
  "imageAttributes":
  {
    "imageExtension": "PNG"
  }
}
```

L’exemple suivant vous explique comment appeler la méthode GET pour récupérer un élément de contenu.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative/106851  HTTP/1.1
Authorization: Bearer <your access token>
```

<span/>
## <a name="response"></a>Réponse

Ces méthodes renvoient un corps de réponse JSON avec un objet de [contenu](#creative) qui comporte des informations sur le contenu créé ou récupéré. L’exemple suivant représente un corps de réponse associé à ces méthodes. Dans cet exemple, la valeur *content* a été raccourcie, par souci de concision.

```json
{
    "Data": {
        "id": 106126,
        "name": "Contoso App Campaign - Creative 2",
        "content": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEAAQABAAD/2wBDAAgGB...other base64 data shortened for brevity...",
        "height": 50,
        "width": 300,
        "format": "Banner",
        "imageAttributes":
        {
          "imageExtension": "PNG"
        },
        "storeProductId": "9nblggh42cfd"
    }
}
```

<span id="creative"/>
## <a name="creative-object"></a>Objet de contenu

Les corps de requête et de réponse associés à ces méthodes comportent les champs suivants. Ce tableau signale les champs en lecture seule (qui ne peuvent être modifiés dans la méthode PUT) et les champs requis dans le corps de requête associé à la méthode POST.

| Champ        | Type   |  Description      |  Lecture seule  | Valeur par défaut  |  Requis pour la méthode POST |  
|--------------|--------|---------------|------|-------------|------------|
|  id   |  entier   |  ID de l’élément de contenu.     |   Oui    |      |    Non   |       
|  name   |  chaîne   |   Nom de l’élément de contenu.    |    Non   |      |  Oui     |       
|  content   |  chaîne   |  Le contenu de l’image, encodé en base 64.     |  Non     |      |   Oui    |       
|  height   |  entier   |   La hauteur du contenu.    |    Non    |      |   Oui    |       
|  width   |  entier   |  La largeur du contenu.     |  Non    |     |    Oui   |       
|  landingUrl   |  chaîne   |  L’URL de destination du contenu (cette valeur doit être un URI valide).     |  Non    |     |   Oui    |       
|  format   |  chaîne   |   Format de la publicité. Actuellement, la seule valeur prise en charge est **Banner**.    |   Non    |  Banner   |  Non     |       
|  imageAttributes   | [ImageAttributes](#image-attributes)    |   Fournit des attributs pour l’élément de contenu.     |   Non    |      |   Oui    |       
|  storeProductId   |  chaîne   |   L’[ID du Windows Store](in-app-purchases-and-trials.md#store-ids) pour l’application à laquelle est associée cette campagne publicitaire. Un produit peut par exemple présenter l’ID 9nblggh42cfd du Windows Store.    |   Non    |    |  Non     |   |  

<span id="image-attributes"/>
## <a name="imageattributes-object"></a>Objet ImageAttributes

| Champ        | Type   |  Description      |  Lecture seule  | Valeur par défaut  | Requis pour la méthode POST |  
|--------------|--------|---------------|------|-------------|------------|
|  imageExtension   |   chaîne  |   Extension de l’image (PNG ou JPG).    |    Non   |      |   Oui    |       |


## <a name="related-topics"></a>Rubriques connexes

* [Exécuter des campagnes publicitaires à l’aide des services du Windows Store](run-ad-campaigns-using-windows-store-services.md)
* [Gérer les campagnes publicitaires](manage-ad-campaigns.md)
* [Gérer les chaînes de distribution des campagnes publicitaires](manage-delivery-lines-for-ad-campaigns.md)
* [Gérer les profils de ciblage pour les campagnes publicitaires](manage-targeting-profiles-for-ad-campaigns.md)
* [Obtenir les données relatives aux performances des campagnes publicitaires](get-ad-campaign-performance-data.md)
