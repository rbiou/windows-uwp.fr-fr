---
ms.assetid: C78176D6-47BB-4C63-92F8-426719A70F04
description: Utilisez cette méthode dans l’API de soumission au Microsoft Store pour obtenir l’état d’une soumission de version d’évaluation du package.
title: Obtenir l’état d’une soumission de version d’évaluation de package
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, API de soumission au Microsoft Store, soumission de version d'évaluation, état
ms.localizationpriority: medium
ms.openlocfilehash: e79e57353a729712da39431028282b90c0741bbd
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66358911"
---
# <a name="get-the-status-of-a-package-flight-submission"></a>Obtenir l’état d’une soumission de version d’évaluation de package

Utilisez cette méthode dans l’API de soumission au Microsoft Store pour obtenir l’état d’une soumission de version d’évaluation du package. Pour plus d’informations sur le processus de création d’une soumission de version d’évaluation de package à l’aide de l’API de soumission au Microsoft Store, voir [Gérer les soumissions de versions d’évaluation du package](manage-flight-submissions.md).

## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà le cas, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission au Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Créer une soumission de vol de package pour l’une de vos applications. Vous pouvez le faire dans le centre de partenaires, ou vous pouvez le faire à l’aide de la [créer une soumission de vol de package](create-a-flight-submission.md) (méthode).

## <a name="request"></a>Demande

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de requête                                                      |
|--------|------------------------------------------------------------------|
| GET   | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status` |


### <a name="request-header"></a>En-tête de requête

| Header        | type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Le jeton d’accès Azure AD sous la forme **PORTEUR** &lt; *jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Nom        | type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | chaîne | Obligatoire. ID Windows Store de l’application qui contient la soumission de version d’évaluation du package dont vous voulez obtenir l’état. Pour plus d’informations sur l’ID Windows Store, voir [Visualiser les informations d’identité des applications](https://docs.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| flightId | chaîne | Obligatoire. ID de la version d’évaluation du package qui contient la soumission dont vous voulez obtenir l’état. Cet ID est disponible dans les données de réponse des requêtes pour [créer une version d’évaluation du package](create-a-flight.md) ou [obtenir des versions d’évaluation du package pour une application](get-flights-for-an-app.md). Pour un vol a été créé dans le centre de partenaires, cet ID est également disponible dans l’URL de la page de vol de partenaires.  |
| submissionId | chaîne | Obligatoire. ID de la soumission dont vous voulez obtenir l’état. Cet ID est disponible dans les données de réponse des requêtes pour [créer une soumission de version d’évaluation de package](create-a-flight-submission.md). Pour la soumission qui a été créée dans le centre de partenaires, cet ID est également disponible dans l’URL de la page d’envoi dans l’espace partenaires.  |


### <a name="request-body"></a>Corps de la requête

Ne fournissez pas de corps de requête pour cette méthode.

### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment obtenir l’état d’une soumission de version d’évaluation du package.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243649/status HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse

L’exemple suivant illustre le corps de réponse JSON d’un appel réussi à cette méthode. Le corps de la réponse contient des informations sur la soumission spécifiée. Pour plus d’informations sur les valeurs figurant dans le corps de réponse, voir la section suivante.

```json
{
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
}
```

### <a name="response-body"></a>Corps de la réponse

| Value      | type   | Description                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| status           | chaîne  | État de la soumission. Les valeurs possibles sont les suivantes : <ul><li>Aucune</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publication</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | objet  |  Contient des détails supplémentaires sur l’état de la soumission, notamment des informations sur les éventuelles erreurs. Pour plus d’informations, voir [Ressource des détails d’état](manage-flight-submissions.md#status-details-object). |


## <a name="error-codes"></a>Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Error code |  Description   |
|--------|------------------|
| 404  | La soumission est introuvable. |
| 409  | L’application utilise une fonctionnalité de partenaires est [actuellement ne pas pris en charge par l’API de soumission de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported).  |


## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des envois à l’aide des services de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gérer les envois de vol de package](manage-flight-submissions.md)
* [Obtenir une soumission de l’application](get-an-app-submission.md)
* [Créer une soumission de l’application](create-an-app-submission.md)
* [Valider une soumission de l’application](commit-an-app-submission.md)
* [Mise à jour une soumission de l’application](update-an-app-submission.md)
* [Supprimer une soumission de l’application](delete-an-app-submission.md)
