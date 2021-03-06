---
ms.assetid: E8751EBF-AE0F-4107-80A1-23C186453B1C
description: Utilisez cette méthode de l’API de soumission au Microsoft Store pour mettre à jour une soumission d’applications existante.
title: Mettre à jour une soumission d’application
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, API de soumission au Microsoft Store, soumission d’applications, mise à jour
ms.localizationpriority: medium
ms.openlocfilehash: 77c033ff09d56448f42d1f8084265ac0aa5d5212
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371429"
---
# <a name="update-an-app-submission"></a>Mettre à jour une soumission d’application

Utilisez cette méthode de l’API de soumission au Microsoft Store pour mettre à jour une soumission d’applications existante. Après avoir mis à jour une soumission à l’aide de cette méthode, vous devez [valider la soumission](commit-an-app-submission.md) en vue de son intégration et de sa publication.

Pour plus d’informations sur la façon dont cette méthode s’inscrit dans le processus de création d’une soumission d’apps à l’aide de l’API de soumission au Microsoft Store, voir [Gérer les soumissions d’apps](manage-app-submissions.md).

## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà le cas, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission au Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Créer une soumission pour l’une de vos applications. Vous pouvez le faire dans le centre de partenaires, ou vous pouvez le faire à l’aide de la [créer une soumission de l’application](create-an-app-submission.md) (méthode).

## <a name="request"></a>Demande

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de requête                                                      |
|--------|------------------------------------------------------------------|
| PUT   | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}  ``` |


### <a name="request-header"></a>En-tête de requête

| Header        | type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Le jeton d’accès Azure AD sous la forme **PORTEUR** &lt; *jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Nom        | type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | chaîne | Obligatoire. ID Windows Store de l’application pour laquelle vous voulez mettre à jour une soumission. Pour plus d’informations sur l’ID Windows Store, voir [Visualiser les informations d’identité des applications](https://docs.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| submissionId | chaîne | Obligatoire. ID de la soumission à mettre à jour. Cet ID est disponible dans les données de réponse des requêtes pour [créer une soumission d’apps](create-an-app-submission.md). Pour la soumission qui a été créée dans le centre de partenaires, cet ID est également disponible dans l’URL de la page d’envoi dans l’espace partenaires.  |


### <a name="request-body"></a>Corps de la requête

Le corps de la requête contient les paramètres suivants.

| Value      | type   | Description                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| applicationCategory           | chaîne  |   Chaîne qui spécifie la [catégorie et/ou sous-catégorie](https://docs.microsoft.com/windows/uwp/publish/category-and-subcategory-table) pour votre application. Les catégories et sous-catégories sont combinées en une seule chaîne à l’aide du caractère trait de soulignement « _ », par exemple **BooksAndReference_EReader**.      |  
| pricing           |  objet  | Objet qui contient les informations de tarification pour l’application. Pour plus d’informations, voir la section relative à la [ressource de tarification](manage-app-submissions.md#pricing-object).       |   
| visibility           |  chaîne  |  Visibilité de l’application. Les valeurs possibles sont les suivantes : <ul><li>Hidden</li><li>Public</li><li>Private</li><li>NotSet</li></ul>       |   
| targetPublishMode           | chaîne  | Mode de publication pour la soumission. Les valeurs possibles sont les suivantes : <ul><li>Immediate</li><li>Manuelle</li><li>SpecificDate</li></ul> |
| targetPublishDate           | chaîne  | Date de publication de la soumission au format ISO 8601, si le paramètre *targetPublishMode* a la valeur SpecificDate.  |  
| listings           |   objet  |  Dictionnaire de paires clé/valeur, où chaque clé est un code de pays et chaque valeur est un objet de [ressource de référencement](manage-app-submissions.md#listing-object) qui contient des informations de référencement pour l’application.       |   
| hardwarePreferences           |  tableau  |   Tableau de chaînes qui définissent les [préférences matérielles](https://docs.microsoft.com/windows/uwp/publish/enter-app-properties) pour votre application. Les valeurs possibles sont les suivantes : <ul><li>Touch</li><li>Clavier</li><li>Souris</li><li>Appareil photo</li><li>NfcHce</li><li>NFC</li><li>BluetoothLE</li><li>Telephony</li></ul>     |   
| automaticBackupEnabled           |  booléenne  |   Indique si Windows peut inclure les données de votre application dans les sauvegardes automatiques sur OneDrive. Pour plus d’informations, voir [Déclarations d’application](https://docs.microsoft.com/windows/uwp/publish/app-declarations).   |   
| canInstallOnRemovableMedia           |  booléenne  |   Indique si les clients peuvent installer votre application sur un stockage amovible. Pour plus d’informations, voir [Déclarations d’application](https://docs.microsoft.com/windows/uwp/publish/app-declarations).     |   
| isGameDvrEnabled           |  booléenne |   Indique si les jeux DVR sont activés pour l’application.    |   
| gamingOptions           |  objet |   Un tableau contenant une [ressource d’options de jeu](manage-app-submissions.md#gaming-options-object) qui définit les paramètres associés aux jeux pour l'app.     |   
| hasExternalInAppProducts           |     booléenne          |   Indique si votre app permet aux utilisateurs d’effectuer des achats hors du système de commerce du Microsoft Store. Pour plus d’informations, voir [Déclarations d’application](https://docs.microsoft.com/windows/uwp/publish/app-declarations).     |   
| meetAccessibilityGuidelines           |    booléenne           |  Indique si votre application a fait l’objet de tests pour voir si elle est conforme aux recommandations d’accessibilité. Pour plus d’informations, voir [Déclarations d’application](https://docs.microsoft.com/windows/uwp/publish/app-declarations).      |   
| notesForCertification           |  chaîne  |   Contient des [notes de certification](https://docs.microsoft.com/windows/uwp/publish/notes-for-certification) pour votre application.    |    
| applicationPackages           |   tableau  | Contient des objets qui fournissent des détails sur chaque package de la soumission. Pour plus d’informations, voir la section [Package d’application](manage-app-submissions.md#application-package-object). Quand vous appelez cette méthode pour mettre à jour une soumission d’application, seules les valeurs *fileName*, *fileStatus*, *minimumDirectXVersion* et *minimumSystemRam* de ces objets sont nécessaires dans le corps de la requête. Les autres valeurs sont remplies par les partenaires.   |    
| packageDeliveryOptions    | objet  | Contient les paramètres de déploiement de package progressif et de mise à jour obligatoire de la soumission. Pour plus d’informations, consultez [Objet options de remise du package](manage-app-submissions.md#package-delivery-options-object).  |
| enterpriseLicensing           |  chaîne  |  Une des [valeur de gestion des licences d’entreprise](manage-app-submissions.md#enterprise-licensing) qui indiquent le comportement de la gestion des licences d’entreprise pour l’application.  |    
| allowMicrosftDecideAppAvailabilityToFutureDeviceFamilies           |  booléenne   |  Indique si Microsoft est autorisé à [rendre l’application disponible pour les futures familles d’appareils Windows 10](https://docs.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability).    |    
| allowTargetFutureDeviceFamilies           | booléenne   |  Indique si votre application est autorisée à [cibler les futures familles d’appareils Windows 10](https://docs.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability).     |   
| trailers           |  tableau |   Un tableau contenant jusqu'à [ressources de bandes-annonces](manage-app-submissions.md#trailer-object), qui représentent les bandes-annonces vidéos du listing de l'app.   |   


### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment mettre à jour une soumission d’application.

```json
PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions/1152921504621230023 HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
{
  "applicationCategory": "BooksAndReference_EReader",
  "pricing": {
    "trialPeriod": "FifteenDays",
    "marketSpecificPricings": {},
    "sales": [],
    "priceId": "Tier2"
  },
  "visibility": "Public",
  "targetPublishMode": "Manual",
  "targetPublishDate": "1601-01-01T00:00:00Z",
  "listings": {
    "en-us": {
      "baseListing": {
        "copyrightAndTrademarkInfo": "",
        "keywords": [
              "epub"
            ],
        "licenseTerms": "",
        "privacyPolicy": "",
        "supportContact": "",
        "websiteUrl": "",
        "description": "Description",
        "features": [
              "Free ebook reader"
            ],
        "releaseNotes": "",
        "images": [
          {
            "fileName": "contoso.png",
            "fileStatus": "Uploaded",
            "id": "1152921504672272757",
            "imageType": "Screenshot"
          }
        ],
        "recommendedHardware": [],
        "title": "Contoso ebook reader"
      },
      "platformOverrides": {
        "Windows81": {
          "description": "Ebook reader for Windows 8.1"
        }
      }
    }
  },
  "hardwarePreferences": [
    "Touch"
  ],
  "automaticBackupEnabled": false,
  "canInstallOnRemovableMedia": true,
  "isGameDvrEnabled": false,
  "gamingOptions": [],
  "hasExternalInAppProducts": false,
  "meetAccessibilityGuidelines": true,
  "notesForCertification": "",
  "applicationPackages": [
    {
      "fileName": "contoso_app.appx",
      "fileStatus": "PendingUpload",
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None"
    }
  ],
  "packageDeliveryOptions": {
    "packageRollout": {
        "isPackageRollout": false,
        "packageRolloutPercentage": 0.0,
        "packageRolloutStatus": "PackageRolloutNotStarted",
        "fallbackSubmissionId": "0"
    },
    "isMandatoryUpdate": false,
    "mandatoryUpdateEffectiveDate": "1601-01-01T00:00:00.0000000Z"
  },
  "enterpriseLicensing": "Online",
  "allowMicrosoftDecideAppAvailabilityToFutureDeviceFamilies": true,
  "allowTargetFutureDeviceFamilies": {
    "Desktop": false,
    "Mobile": true,
    "Holographic": true,
    "Xbox": false,
    "Team": true
  },
  "trailers": []
}
```

## <a name="response"></a>Réponse

L’exemple suivant illustre le corps de réponse JSON d’un appel réussi à cette méthode. Le corps de la réponse contient des informations sur la soumission mise à jour. Pour plus d’informations sur les valeurs figurant dans le corps de la réponse, voir la [ressource de soumission d’application](manage-app-submissions.md#app-submission-object).

```json
{
  "id": "1152921504621243540",
  "applicationCategory": "BooksAndReference_EReader",
  "pricing": {
    "trialPeriod": "FifteenDays",
    "marketSpecificPricings": {},
    "sales": [],
    "priceId": "Tier2"
  },
  "visibility": "Public",
  "targetPublishMode": "Manual",
  "targetPublishDate": "1601-01-01T00:00:00Z",
  "listings": {
    "en-us": {
      "baseListing": {
        "copyrightAndTrademarkInfo": "",
        "keywords": [
           "epub"
        ],
        "licenseTerms": "",
        "privacyPolicy": "",
        "supportContact": "",
        "websiteUrl": "",
        "description": "Description",
        "features": [
          "Free ebook reader"
        ],
        "releaseNotes": "",
        "images": [
          {
            "fileName": "contoso.png",
            "fileStatus": "Uploaded",
            "id": "1152921504672272757",
            "imageType": "Screenshot"
          }
        ],
        "recommendedHardware": [],
        "title": "Contoso ebook reader"
      },
      "platformOverrides": {
        "Windows81": {
          "description": "Ebook reader for Windows 8.1",
        }
      }
    }
  },
  "hardwarePreferences": [
    "Touch"
  ],
  "automaticBackupEnabled": false,
  "canInstallOnRemovableMedia": true,
  "isGameDvrEnabled": false,
  "gamingOptions": [],
  "hasExternalInAppProducts": false,
  "meetAccessibilityGuidelines": true,
  "notesForCertification": "",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/387a9ea8-a412-43a9-8fb3-a38d03eb483d?sv=2014-02-14&sr=b&sig=sdd12JmoaT6BhvC%2BZUrwRweA%2Fkvj%2BEBCY09C2SZZowg%3D&se=2016-06-17T18:32:26Z&sp=rwl",
  "applicationPackages": [
    {
      "fileName": "contoso_app.appx",
      "fileStatus": "PendingUpload",
      "id": "1152921504620138797",
      "version": "1.0.0.0",
      "architecture": "ARM",
      "languages": [
        "en-US"
      ],
      "capabilities": [
        "ID_RESOLUTION_HD720P",
        "ID_RESOLUTION_WVGA",
        "ID_RESOLUTION_WXGA"
      ],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None",
      "targetDeviceFamilies": [
        "Windows.Mobile min version 10.0.10240.0"
      ]
    }
  ],
  "packageDeliveryOptions": {
    "packageRollout": {
        "isPackageRollout": false,
        "packageRolloutPercentage": 0.0,
        "packageRolloutStatus": "PackageRolloutNotStarted",
        "fallbackSubmissionId": "0"
    },
    "isMandatoryUpdate": false,
    "mandatoryUpdateEffectiveDate": "1601-01-01T00:00:00.0000000Z"
  },
  "enterpriseLicensing": "Online",
  "allowMicrosoftDecideAppAvailabilityToFutureDeviceFamilies": true,
  "allowTargetFutureDeviceFamilies": {
    "Desktop": false,
    "Mobile": true,
    "Holographic": true,
    "Xbox": false,
    "Team": true
  },
  "friendlyName": "Submission 2",
  "trailers": []
}
```

## <a name="error-codes"></a>Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Error code |  Description   |
|--------|------------------|
| 400  | Impossible de mettre à jour la soumission, car la requête n’est pas valide. |
| 409  | L’envoi n’a pas pu être chargé en raison de l’état actuel de l’application, ou l’application utilise une fonctionnalité de partenaires est [actuellement ne pas pris en charge par l’API de soumission de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   


## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des envois à l’aide des services de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Obtenir une soumission de l’application](get-an-app-submission.md)
* [Créer une soumission de l’application](create-an-app-submission.md)
* [Valider une soumission de l’application](commit-an-app-submission.md)
* [Supprimer une soumission de l’application](delete-an-app-submission.md)
* [Obtenir l’état d’une soumission de l’application](get-status-for-an-app-submission.md)
