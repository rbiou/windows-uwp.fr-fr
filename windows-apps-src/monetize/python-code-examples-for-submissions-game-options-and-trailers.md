---
description: Servez-vous des exemples de code Python présentés dans cette section pour en savoir plus sur la soumission d'options de jeu et de bandes-annonces à l'aide de l’API de soumission au Microsoft Store.
title: 'Exemple de code Python : soumission d’applications avec options de jeu et bandes-annonces'
ms.date: 07/10/2017
ms.topic: article
keywords: windows 10, uwp, API de soumission au Microsoft Store, exemples de code, options de jeu, bandes-annonces, descriptions avancées, python
ms.localizationpriority: medium
ms.openlocfilehash: 5d68058d1bf53d936c9d88a3725c7c1dd9a3f443
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334987"
---
# <a name="python-sample-app-submission-with-game-options-and-trailers"></a>Exemple de code Python : soumission d’application avec options de jeu et bandes-annonces

Cet article fournit des exemples de code Python qui décrivent comment utiliser l’[API de soumission au Microsoft Store](create-and-manage-submissions-using-windows-store-services.md) pour les tâches suivantes :

* Obtenir un jeton d’accès Azure AD à utiliser avec l’API de soumission au Microsoft Store.
* Créer une soumission d’applications
* Configurer des données de description dans le Store pour la soumission d’applications, y compris des options de description avancées de [jeux](manage-app-submissions.md#gaming-options-object) et de [bandes-annonces](manage-app-submissions.md#trailer-object).
* Télécharger le fichier ZIP contenant les packages, les images de description et les fichiers de bande-annonce pour la soumission d’applications.
* Valider la soumission d’applications.

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>Créer une soumission d’applications

Ce code appelle d'autres exemples de classes et de fonctions pour utiliser l’API de soumission au Microsoft Store afin de créer et valider une soumission d’apps contenant des options de jeu et une bande-annonce. Adaptez ce code en fonction de vos besoins, comme suit :

* Affectez la variable `tenant` à l’ID de locataire pour votre application et affectez les variables `client` et `secret` à l'ID client et à la clé de votre application. Pour plus d’informations, consultez [comment associer une application Azure AD à votre compte espace partenaires](create-and-manage-submissions-using-windows-store-services.md#how-to-associate-an-azure-ad-application-with-your-partner-center-account)
* Affectez la variable `application_id` à l'[ID Store](in-app-purchases-and-trials.md#store-ids) de l’application pour laquelle vous voulez créer une soumission.

> [!div class="tabbedCodeSnippets"]
[!code-python[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/python/CreateAndSubmitAppSubmissionExample.py#L1-L74)]

<span id="token" />

## <a name="obtain-an-azure-ad-access-token-and-invoke-the-submission-api"></a>Obtenir un jeton d’accès Azure AD et appeler l’API de soumission

L’exemple suivant définit les classes suivantes :

* La classe `DevCenterAccessTokenClient` définit une méthode d’assistance qui utilise vos valeurs `tenantId`, `clientId` et `clientSecret` pour créer un jeton d’accès Azure AD à utiliser avec l’API de soumission au Microsoft Store.
* La classe `DevCenterClient` définit des méthodes d’assistance qui appellent plusieurs méthodes dans l’API de soumission au Microsoft Store et chargent le fichier ZIP contenant les packages, les images de description et les fichiers de bande-annonce pour la soumission d’apps.

> [!div class="tabbedCodeSnippets"]
[!code-python[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/python/devcenterclient.py#L1-L126)]

<span id="token" />

## <a name="get-app-submission-listing-data"></a>Obtenir les données de description d'une soumission d’applications

L’exemple suivant définit les fonctions d’assistance qui renvoient des données de description au format JSON pour un nouvel exemple de soumission d’applications.

> [!div class="tabbedCodeSnippets"]
[!code-python[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/python/submissiondatasamples.py#L1-L170)]

## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des envois à l’aide des services de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
