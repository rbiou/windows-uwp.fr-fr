---
title: /untrustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}
assetID: f7de98c3-e6d1-2c40-00f0-d45e418af8bf
permalink: en-us/docs/xboxlive/rest/uri-untrustedplatformusersxuidscidssciddatapathandfilenametype.html
description: " /untrustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jeux, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: cdb1d9d96d28e5aadc9c017f6f13e51bdf066f73
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656284"
---
# <a name="untrustedplatformusersxuidxuidscidssciddatapathandfilenametype"></a>/untrustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}
Téléchargements, télécharge ou supprime un fichier. Le domaine pour ces URI est `titlestorage.xboxlive.com`.
 
  * [Paramètres d’URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Paramètres d’URI
 
| Paramètre| Type| Description| 
| --- | --- | --- | 
| xuid| entier non signé 64 bits| La Xbox ID de l’utilisateur (XUID) du lecteur qui effectue la demande.| 
| scid| GUID| ID de la configuration de service à rechercher.| 
| pathAndFileName| chaîne| Chemin d’accès et le nom de l’élément accessible. Caractères valides dans la partie chemin d’accès (jusqu'à et y compris la barre oblique finale), notamment des lettres majuscules (A-Z), lettres minuscules (a-z), chiffres (0-9), un trait de soulignement (_) et de la barre oblique (/). La partie chemin d’accès peut être vide. Caractères valides dans la partie du nom de fichier (tout ce qui suit la barre oblique finale), notamment des lettres majuscules (A-Z), lettres minuscules (a-z), chiffres (0-9), un trait de soulignement (_), point (.) et trait d’union (-). Le nom de fichier ne peut pas être vide, se terminer par un point ou contenir deux points consécutifs.| 
| type| chaîne| Le format des données. Les valeurs possibles sont binaire ou json.| 
  
<a id="ID4EOC"></a>

 
## <a name="valid-methods"></a>Méthodes valides

[SUPPRIMER](uri-untrustedplatformusersxuidscidssciddatapathandfilenametype-delete.md)

&nbsp;&nbsp;Supprime un fichier. 

[GET](uri-untrustedplatformusersxuidscidssciddatapathandfilenametype-get.md)

&nbsp;&nbsp;Télécharge un fichier.

[PUT](uri-untrustedplatformusersxuidscidssciddatapathandfilenametype-put.md)

&nbsp;&nbsp;Télécharge un fichier. Les données peuvent être téléchargées dans un téléchargement complet dans lequel les données et les métadonnées sont envoyés dans un seul message, ou comme un téléchargement de bloc multiples dans lequel les données et les métadonnées sont envoyés dans une série de blocs plus petits. Uniquement les fichiers qui sont plus petits que quatre mégaoctets peuvent être envoyés en tant qu’un seul message. Téléchargement de bloc multiples n’est pas pris en charge pour les données de type json. 
 
<a id="ID4E5C"></a>

 
## <a name="see-also"></a>Voir également
 
<a id="ID4EAD"></a>

 
##### <a name="parent"></a>Parent 

[URI de stockage de titre](atoc-reference-storagev2.md)

   