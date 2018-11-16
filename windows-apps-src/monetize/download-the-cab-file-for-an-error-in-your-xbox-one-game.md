---
author: Xansky
description: Utilisez cette méthode dans l’API d’analytique Microsoft Store pour télécharger le fichier CAB concernant une erreur dans votre jeu Xbox One.
title: Télécharger le fichier CAB concernant une erreur dans votre jeu Xbox One
ms.author: mhopkins
ms.date: 11/06/2018
ms.topic: article
keywords: windows 10, uwp, API d'analyse du MicrosoftStore, télécharger le fichier CAB
ms.localizationpriority: medium
ms.openlocfilehash: 517a1cbb8ec2cafe49ded53bce34e17537bc5efc
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6968213"
---
# <a name="download-the-cab-file-for-an-error-in-your-xbox-one-game"></a>Télécharger le fichier CAB concernant une erreur dans votre jeu Xbox One

Utilisez cette méthode dans l’API d’analytique Microsoft Store pour télécharger le fichier CAB associé à une erreur spécifique dans votre jeu Xbox One qui a été intégré via le portail de développeur de le Xbox (XDP) et disponible dans le tableau de bord du centre de développement Analytique XDP. Cette méthode ne peut télécharger le fichier CAB pour une erreur qui se sont produites au cours des 30 derniers jours.

Avant de pouvoir utiliser cette méthode, vous devez d’abord utiliser la méthode [d’obtenir des détails sur une erreur dans votre jeu Xbox One](get-details-for-an-error-in-your-xbox-one-game.md) pour récupérer l’ID du fichier CAB à télécharger.

## <a name="prerequisites"></a>Conditions préalables


Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du MicrosoftStore.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Obtenez l’ID du fichier CAB à télécharger. Pour obtenir cet ID, utilisez la méthode de [obtenir plus d’informations concernant une erreur dans votre console Xbox One jeu](get-details-for-an-error-in-your-xbox-one-game.md) pour récupérer les détails d’une erreur spécifique dans votre application et utiliser la valeur de **cabId** dans le corps de réponse de cette méthode.

## <a name="request"></a>Requête


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de la requête                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/cabdownload``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Paramètre        | Type   |  Description      |  Requis  |
|---------------|--------|---------------|------|
| applicationId | chaîne | L’ID de produit du jeu Xbox One pour lequel vous téléchargez le fichier CAB. Pour obtenir l’ID produit de votre jeu, accédez à votre jeu dans le portail de développement Xbox (XDP) et récupérez l’ID produit à partir de l’URL. Par ailleurs, si vous téléchargez vos données d’intégrité de l’état d’analytique du centre de développement Windows, l’ID de produit est inclus dans le fichier .tsv. |  Oui  |
| cabId | chaîne | L’ID unique du fichier CAB à télécharger. Pour obtenir cet ID, utilisez la méthode de [obtenir plus d’informations concernant une erreur dans votre console Xbox One jeu](get-details-for-an-error-in-your-xbox-one-game.md) pour récupérer les détails d’une erreur spécifique dans votre application et utiliser la valeur de **cabId** dans le corps de réponse de cette méthode. |  Oui  |

 
### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment télécharger un fichier CAB à l'aide de cette méthode. Remplacez les paramètres *applicationId* et *cabId* par les valeurs qui correspondent à votre application.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/cabdownload?applicationId=BRRT4NJ9B3D1&cabId=1336373323853 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse

Cette méthode renvoie un code de réponse 302 (redirection) et l'en-tête **Services de localisation** dans la réponse est attribué à l'URI de la signature d’accès partagé (SAS) du fichier CAB. L’appelant est redirigé vers cette URI pour télécharger automatiquement le fichier CAB.

## <a name="related-topics"></a>Rubriques associées

* [Accéder aux données d’analyse à l’aide des services du MicrosoftStore](access-analytics-data-using-windows-store-services.md)
* [Obtenir les données rapport d’erreurs pour votre console Xbox One jeu](get-error-reporting-data-for-your-xbox-one-game.md)
* [Obtenir les détails d’une erreur dans votre console Xbox One jeu](get-details-for-an-error-in-your-xbox-one-game.md)
* [Obtenir la trace de pile concernant une erreur dans votre console Xbox One du jeu](get-the-stack-trace-for-an-error-in-your-xbox-one-game.md)