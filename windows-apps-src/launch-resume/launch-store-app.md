---
title: Lancer l’application Microsoft Store
description: Cette rubrique décrit le schéma d’URI ms-windows-store. Votre application peut utiliser ce schéma d’URI pour lancer l’application Microsoft Store à des pages spécifiques dans le Store.
ms.assetid: 9A9C6576-1637-47D1-AC3B-D1A20D49E0FF
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: fd0e7137f31a8f1620f7937b52efe1ca84a6b99a
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370783"
---
# <a name="launch-the-microsoft-store-app"></a>Lancer l’application Microsoft Store



Cette rubrique décrit la **ms-windows-store :** Schéma d’URI. Votre application peut utiliser ce schéma d’URI pour lancer l’application Microsoft Store à des pages spécifiques dans le magasin à l’aide de la [ **LaunchUriAsync** ](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync) (méthode).

Cet exemple montre comment ouvrir le Store à la page Jeux :

```cs
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-windows-store://navigatetopage/?Id=Games"));
```

## <a name="ms-windows-store-uri-scheme-reference"></a>MS-windows-store : Référence de schéma d’URI

<table>
<tr><th>Description</th><th></th><th>Schéma d’URI</th></tr>
<tr><td>Lance la page d’accueil du Store.</td><td /><td>ms-windows-store://home</td></tr>
<tr><td>Ouvre une verticale de niveau supérieur dans le Store.<p>Remarque: Certains utilisateurs n’ont accès à tous les marchés verticaux.</p>
</td><td /><td>
<p>ms-windows-store://navigatetopage/?Id=Apps </p>
<p>ms-windows-store://navigatetopage/?Id=Games</p>
<p>ms-windows-store://navigatetopage/?Id=Music</p>
<p>ms-windows-store://navigatetopage/?Id=Video</p>
<p>ms-windows-store://navigatetopage/?Id=LOB</p>
</td>
</tr>
<tr>
<td rowspan="4">Lance la page de détails d’un produit (PDP). <p>ID de Store est recommandée pour les clients sur Windows 10 et fonctionne sur toutes les versions de système d’exploitation, mais dans les méthodes précédentes de les créer (ex : NFP) qui ne sont toujours pris en charge.</p>
<p>Vous trouverez ces valeurs dans <a href="https://partner.microsoft.com/dashboard">partenaires</a> sur le <a href="https://docs.microsoft.com/windows/uwp/publish/view-app-identity-details">identité de l’application</a> page dans la section de gestion d’application pour chaque application.</p>
</td>
<td>
ID Windows Store <p>(recommandé)</p>
</td>
<td>
<p>ms-windows-store://pdp/?ProductId=9WZDNCRFHVJL</p>
</td>
</tr>
<tr>
<td>Nom de la famille de packages (PFN)</td>
<td>ms-windows-store://pdp/?PFN= Microsoft.Office.OneNote_8wekyb3d8bbwe
</td>
</tr>
<tr>
<td>ID de produit (Windows Phone 7.x/8.x)</td>
<td>ms-windows-store://pdp/?PhoneAppId=ca05b3ab-f157-450c-8c49-a1f127f5e71d</td>
</tr>
<tr>
<td>ID de produit (Windows 8.x)</td>
<td>ms-windows-store://pdp/?AppId=f022389f-f3a6-417e-ad23-704fbdf57117
</td>
</tr>
<tr>
<td rowspan="4">Lance la procédure d’écriture d’un avis pour un produit.</td>
<td>ID Windows Store <p>(recommandé)</p></td>
<td>ms-windows-store://review/?ProductId=9WZDNCRFHVJL </td>
</tr>
<tr>
<td>Nom de la famille de packages (PFN)</td>
<td>ms-windows-store://review/?PFN= Microsoft.Office.OneNote_8wekyb3d8bbwe
</td>
</tr>
<tr>
<td>ID de produit (Windows Phone 7.x/8.x)</td>
<td>ms-windows-store://reviewapp/?AppId=ca05b3ab-f157-450c-8c49-a1f127f5e71d </td>
</tr>
<tr>
<td>ID de produit (Windows 8.x)</td>
<td>ms-windows-store://review/?AppId=f022389f-f3a6-417e-ad23-704fbdf57117 </td>
</tr>
<tr>
<td>Lance une recherche de produits associés à une extension de fichier. </td>
<td />
<td>ms-windows-store://assoc/?FileExt=pdf
</td>
</tr>
<tr>
<td>Lance une recherche de produits associés à un protocole.</td>
<td />
<td>ms-windows-store://assoc/?Protocol=ms-word </td>
</tr>
<tr>
<td>Lance une recherche de produits associés à une ou plusieurs balises. Les balises doivent être séparées par des virgules.
</td>
<td />
<td>
<p>ms-windows-store://assoc/?Tags=Photos_Rich_Media_Edit </p>
<p>ms-windows-store://assoc/?Tags=Photos_Rich_Media_Edit, Camera_Capture_App</p>
</td>
</tr>
<tr>
<td>
Lance une recherche pour la requête spécifiée. Les espaces sont autorisées dans la requête.
</td>
<td />
<td>ms-windows-store://search/?query=OneNote </td>
</tr>
<tr>
<td>Lance une recherche de produits d’une catégorie.</td>
<td />
<td>
<p>ms-windows-store://browse/?type=Apps&amp;cat=Productivity</p>
<p>ms-windows-store://browse/?type=Apps&amp;cat=Health+%26+fitness </p>
</td>
</tr>
<tr>
<td>Lance une recherche de produits de l’éditeur spécifié. Les espaces sont autorisées dans le nom.
</td>
<td />
<td>ms-windows-store://publisher/?name=Microsoft Corporation
</td>
</tr>
<tr><td>Lance les téléchargement et mises à jour de page.</td>
<td />
<td>ms-windows-store://downloadsandupdates </td>
</tr>
<tr>
<td>Lance la page Paramètres du Store.</td>
<td />
<td>ms-windows-store://settings </td>
</tr>
</table>

 

 
