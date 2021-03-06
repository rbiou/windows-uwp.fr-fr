---
description: Utilisez cette méthode dans l’API d’analyse du Microsoft Store pour obtenir les informations concernant une erreur spécifique de votre application de bureau.
title: Obtenir les informations sur une erreur de votre application de bureau
ms.date: 06/05/2018
ms.topic: article
keywords: windows 10, uwp, services du Microsoft Store, API d'analyse du Microsoft Store, erreurs, détails, application de bureau
ms.localizationpriority: medium
ms.openlocfilehash: 986e4a0c11430517872e6f0b21e429ef168529b7
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372671"
---
# <a name="get-details-for-an-error-in-your-desktop-application"></a>Obtenir les informations sur une erreur de votre application de bureau

Utilisez cette méthode dans l’API d’analyse du Microsoft Store pour obtenir les informations concernant une erreur spécifique de votre app, au format JSON. Cette méthode ne récupère que les informations concernant les erreurs survenues dans les 30 derniers jours. Données d’erreur détaillés sont également disponibles dans le [rapport d’intégrité](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program) pour les applications de bureau dans l’espace partenaires.

Pour utiliser cette méthode, vous devez d’abord utiliser la méthode [Obtenir les données de rapport d’erreurs](get-error-reporting-data.md) afin de récupérer l’ID de l’erreur sur laquelle vous souhaitez des informations détaillées.

## <a name="prerequisites"></a>Prérequis


Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà fait, remplissez toutes les [conditions préalables](access-analytics-data-using-windows-store-services.md#prerequisites) relatives à l’API d’analyse du Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Récupérez l’ID de l’erreur sur laquelle vous souhaitez des informations détaillées. Pour ce faire, utilisez la méthode [Obtenir les données de rapport d’erreurs](get-error-reporting-data.md) et utilisez la valeur **failureHash** dans le corps de la réponse de cette méthode.

## <a name="request"></a>Demande


### <a name="request-syntax"></a>Syntaxe de la requête

| Méthode | URI de requête                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failuredetails``` |


### <a name="request-header"></a>En-tête de requête

| Header        | type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Le jeton d’accès Azure AD sous la forme **PORTEUR** &lt; *jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Paramètre        | type   |  Description      |  Obligatoire  
|---------------|--------|---------------|------|
| applicationId | chaîne | L’ID produit d'une application de bureau pour laquelle vous souhaitez récupérer les détails de l'erreur. Pour obtenir l’ID de produit d’une application de bureau, ouvrez un [analytique de rapports pour votre application de bureau partenaires](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program) (telles que la **rapport d’intégrité**) et récupérer l’ID de produit dans l’URL. |  Oui  |
| failureHash | chaîne | ID de l’erreur sur laquelle vous souhaitez des informations détaillées. Pour obtenir la valeur correspondant à l’erreur qui vous intéresse, utilisez la méthode [Obtenir les données de rapport d’erreurs](get-error-reporting-data.md) et utilisez la valeur **failureHash** dans le corps de la réponse de cette méthode. |  Oui  |
| startDate | date | Date de début des données à récupérer concernant l’erreur. La valeur par défaut est de 30 jours avant la date actuelle.<p/><p/>**Remarque :** &nbsp;&nbsp;cette méthode peut récupérer uniquement les détails des erreurs qui se sont produites au cours des 30 derniers jours. |  Non  |
| endDate | date | Date de fin des données à récupérer concernant l’erreur. La valeur par défaut est la date actuelle. |  Non  |
| top | entier | Le nombre de lignes de données à renvoyer dans la requête. La valeur maximale et la valeur par défaut en l’absence de définition est 10000. Si la requête comporte davantage de lignes, le corps de la réponse inclut un lien sur lequel vous cliquez pour solliciter la page suivante de données. |  Non  |
| skip | entier | Le nombre de lignes à ignorer dans la requête. Utilisez ce paramètre pour parcourir de grands ensembles de données. Par exemple, indiquez top=10 et skip=0 pour obtenir les 10 premières lignes de données, top=10 et skip=10 pour obtenir les 10 lignes suivantes, et ainsi de suite. |  Non  |
| Filter |chaîne  | Une ou plusieurs instructions qui filtrent les lignes de la réponse. Chaque instruction comporte un champ Nom dans le corps de la réponse et une valeur, qui sont associés aux opérateurs **eq** ou **ne**, et les instructions peuvent être combinées à l’aide des opérateurs **and** ou **or**. Les valeurs de chaîne doivent être entourées par des guillemets dans le paramètre *filter*. Vous pouvez spécifier les champs suivants dans le corps de réponse :<p/><ul><li><strong>market</strong></li><li><strong>date</strong></li><li><strong>cabIdHash</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>deviceType</strong></li><li><strong>deviceModel</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>applicationVersion</strong></li><li><strong>osBuild</strong></li><li><strong>fileName</strong></li></ul> | Non   |
| orderby | chaîne | Instruction commandant les valeurs des données de résultats. Syntaxe : <em>orderby=field [order],field [order],...</em>. Le paramètre <em>field</em> peut comporter l’une des chaînes suivantes :<ul><li><strong>market</strong></li><li><strong>date</strong></li><li><strong>cabIdHash</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>deviceType</strong></li><li><strong>deviceModel</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>applicationVersion</strong></li><li><strong>osBuild</strong></li><li><strong>fileName</strong></li></ul><p>Le paramètre <em>order</em>, facultatif, peut comporter les valeurs <strong>asc</strong> ou <strong>desc</strong> afin de spécifier l’ordre croissant ou décroissant pour chaque champ. La valeur par défaut est <strong>asc</strong>.</p><p>Voici un exemple de chaîne <em>orderby</em> : <em>orderby=date,market</em></p> |  Non  |


### <a name="request-example"></a>Exemple de requête

Les exemples suivants fournissent plusieurs requêtes permettant de récupérer des données d’erreur. Remplacez la valeur *applicationId* par l'ID produit pour votre application de bureau.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failuredetails?applicationId=10238467886765136388&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failuredetails?applicationId=10238467886765136388&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse


### <a name="response-body"></a>Corps de la réponse

| Value      | type    | Description    |
|------------|---------|------------|
| Value      | tableau   | Tableau d’objets comportant des données d’erreur détaillées. Pour plus d’informations sur les données de chaque objet, consultez la section [Valeurs des informations d’erreur](#error-detail-values) ci-dessous.          |
| @nextLink  | chaîne  | S’il existe des pages supplémentaires de données, cette chaîne comporte un URI que vous pouvez utiliser pour solliciter la page suivante de données. Par exemple, cette valeur est renvoyée si le paramètre **top** de la requête est défini sur 10, mais que plus de 10 lignes d’erreur sont associées à la requête. |
| TotalCount | entier | Nombre total de lignes dans les résultats de la requête.        |


<span id="error-detail-values"/>

### <a name="error-detail-values"></a>Valeurs des informations d’erreur

Les éléments du tableau *Value* comportent les valeurs suivantes :

| Value           | type    | Description     |
|-----------------|---------|----------------------------|
| applicationId   | chaîne  | L'ID produit de l’application de bureau dont vous souhaitez récupérer des détails de l’erreur.      |
| failureHash     | chaîne  | Identificateur unique de l’erreur.     |
| failureName     | chaîne  | Le nom de l'échec, qui est constitué de quatre parties : une ou plusieurs classes de problème, un code de vérification d’exception/d’erreur, le nom de l'image où l’échec s’est produit et le nom de la fonction associée.           |
| date            | chaîne  | Date de début des données d’erreur. Si la requête spécifiait un jour précis, cette valeur correspond à la date. Si la requête était relative à une semaine, un mois ou toute autre plage de dates, cette valeur correspond à la première date de la plage de dates. |
| cabIdHash           | chaîne  | Le hachage de l'ID unique du fichier CAB associé à cette erreur.   |
| cabExpirationTime  | chaîne  | Date et heure auxquelles le fichier CAB est arrivé à expiration et n’est plus téléchargeable au format ISO 8601.   |
| market          | chaîne  | Code pays ISO 3166 du marché des appareils.     |
| osBuild         | chaîne  | Numéro de version du système d’exploitation sur lequel l’erreur s’est produite.       |
| applicationVersion         | chaîne  |   La version du fichier exécutable de l’application dans laquelle l’erreur s’est produite.     |
| deviceModel           | chaîne  | Chaîne identifiant le modèle d’appareil sur lequel l’application s’exécutait lorsque l’erreur s’est produite.   |
| osVersion       | chaîne  | Une des chaînes suivantes qui spécifie la version du système d'exploitation sur laquelle l’application de bureau est installée :<p/><ul><li><strong>Windows 7</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Windows Server 2016</strong></li><li><strong>Windows Server 1709</strong></li><li><strong>Inconnu</strong></li></ul>    |
| osRelease       | chaîne  |  L'une des chaînes suivantes qui spécifie la version du système d’exploitation ou l'anneau de distribution de version d’évaluation (comme une sous-population dans la version du système d’exploitation) sur laquelle/lequel l'erreur s'est produite.<p/><p>Pour Windows 10 :</p><ul><li><strong>Version 1507</strong></li><li><strong>Version 1511</strong></li><li><strong>Version 1607</strong></li><li><strong>Version 1703</strong></li><li><strong>Version 1709</strong></li><li><strong>Version 1803</strong></li><li><strong>Release Preview</strong></li><li><strong>Insider rapide</strong></li><li><strong>Insider lente</strong></li></ul><p/><p>Pour Windows Server 1709 :</p><ul><li><strong>RTM</strong></li></ul><p>Pour Windows Server 2016 :</p><ul><li><strong>Version 1607</strong></li></ul><p>Pour Windows 8.1 :</p><ul><li><strong>Mise à jour 1</strong></li></ul><p>Pour Windows 7 :</p><ul><li><strong>Service Pack 1</strong></li></ul><p>Si la version du système d’exploitation ou l'anneau de distribution de version d’évaluation est inconnu(e), ce champ comporte la valeur <strong>Inconnu</strong>.</p>    |
| deviceType      | chaîne  | Une des chaînes suivantes indiquant le type d’appareil sur lequel l’erreur s’est produite : <p/><ul><li><strong>PC</strong></li><li><strong>Serveur</strong></li><li><strong>Inconnu</strong></li></ul>     |
| cabDownloadable           | Booléen  | Indique si le fichier CAB est téléchargeable par cet utilisateur.   |
| fileName           | chaîne  | Le nom du fichier exécutable de l’application de bureau dont vous souhaitez récupérer des détails de l’erreur.  |


### <a name="response-example"></a>Exemple de réponse

L’exemple suivant représente un corps de réponse JSON pour cette requête.

```json
{
  "Value": [
    {
      "applicationId": "10238467886765136388",
      "failureHash": "012345-5dbc9-b12f-c124-9d9810f05d8b",
      "failureName": "NULL_CLASS_PTR_WRITE_c0000005_contoso.exe!unknown_error_in_process",
      "date": "2018-01-28 23:55:29",
      "cabIdHash": "54ffb83a-e159-41d2-8158-f36f306cc01e",
      "cabExpirationTime": "2018-02-27 23:55:29",
      "market": "US",
      "osBuild": "10.0.10240",
      "applicationVersion": "2.2.2.0",
      "deviceModel": "Contoso All-in-one",
      "osVersion": "Windows 10",
      "osRelease": "Version 1703",
      "deviceType": "PC",
      "cabDownloadable": false,
      "fileName": "contosodemo.exe"
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Rapport d’intégrité](../publish/health-report.md)
* [Accéder aux données d’analytique à l’aide des services de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtenir des données pour votre application de bureau de signalement d’erreurs](get-desktop-application-error-reporting-data.md)
* [Obtenir la trace de pile pour une erreur dans votre application de bureau](get-the-stack-trace-for-an-error-in-your-desktop-application.md)
* [Télécharger le fichier CAB pour une erreur dans votre application de bureau](download-the-cab-file-for-an-error-in-your-desktop-application.md)
