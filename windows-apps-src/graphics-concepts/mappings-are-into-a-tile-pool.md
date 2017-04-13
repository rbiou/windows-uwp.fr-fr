---
title: Mappages dans un pool de vignettes
description: "Lorsqu’une ressource est créée en tant que ressource de diffusion en continu, les vignettes qui la constituent proviennent du pointage dans des emplacements d’un pool de vignettes. Un pool de vignettes est un pool de mémoire (pris en charge par une ou plusieurs allocations masquées, auxquelles l’application n’a pas accès)."
ms.assetid: 58B8DBD5-62F5-4B94-8DD1-C7D57A812185
keywords:
- Mappages dans un pool de vignettes
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 9c8ba46cdd1968fd72307849005f91aa5e872260
ms.lasthandoff: 02/07/2017

---

# <a name="mappings-are-into-a-tile-pool"></a>Mappages dans un pool de vignettes


Lorsqu’une ressource est créée en tant que ressource de diffusion en continu, les vignettes qui la constituent proviennent du pointage dans des emplacements d’un pool de vignettes. Un pool de vignettes est un pool de mémoire (pris en charge par une ou plusieurs allocations masquées, auxquelles l’application n’a pas accès). Le système d’exploitation et le pilote d’affichage gèrent ce pool de mémoire ; l’encombrement de mémoire est facilement appréhendé par les applications. Les ressources de diffusion en continu mappent des régions de 64 Ko en pointant vers des emplacements d’un pool de vignettes. Cette configuration permet à plusieurs ressources de partager et de réutiliser les mêmes vignettes, et prend en charge la réutilisation des mêmes vignettes à différents emplacements d’une ressource, en fonction des besoins.

Avec cette méthode flexible d’intégration des vignettes d’une ressource à partir d’un pool de vignettes, la ressource est chargée de la définition et de la gestion des mappages des vignettes du pool requises. Les mappages de vignettes peuvent être modifiés. En outre, il n’est pas nécessaire de mapper simultanément l’ensemble des vignettes d’une ressource ; cette dernière peut présenter des mappages **NULL**. Un mappage **NULL** définit une vignette comme non disponible, du point de vue de la ressource qui y accède.

Plusieurs pools de vignettes peuvent être créés, et un nombre illimité de ressources de diffusion en continu peuvent mapper simultanément sur un pool de vignettes donné. Les pools de vignettes peuvent être augmentés ou réduits. Pour plus d’informations, voir [Redimensionnement d’un pool de vignettes](tile-pool-resizing.md). Pour que l’implémentation du disque d’affichage et de l’exécution puisse être simplifiée, une ressource donnée de diffusion en continu peut présenter des mappages dans un seul pool de vignettes simultanément.

Le volume de stockage associé à une ressource de diffusion en continu (mémoire indépendante du pool de vignettes) est plus ou moins proportionnel au nombre de vignettes mappées sur le pool à un moment donné. Avec le matériel, cela revient en fin de compte à mettre à l’échelle l’encombrement de mémoire associé au stockage de la table de page en fonction des vignettes mappées (par exemple, utiliser un schéma multiniveau approprié de table de page).

Le pool de vignettes peut être considéré comme une abstraction exclusivement logicielle permettant aux applications Direct3D de programmer les tables de page sur le processeur graphique (GPU), sans avoir à connaître les détails d’implémentation de niveau inférieur (ou traiter directement avec des adresses de pointeur). Les pools de vignettes n’appliquent aucun niveau supplémentaire d’indirection dans le matériel. Les optimisations d’une table de page de niveau unique exécutées à l’aide de constructions comme les répertoires de page sont indépendantes du concept de pool de vignettes.

Découvrons ensemble quel serait, dans le pire des cas, le besoin en stockage de la table de page (bien qu’en pratique les implémentations nécessitent uniquement un volume de stockage plus ou moins proportionnel au contenu mappé).

Supposons que chacune des entrées de la table de page représente 64 bits.

Pour la prévision la plus pessimiste de taille de table de page pour une surface unique, compte tenu des limites des ressources dans Direct3D 11, supposons qu’une ressource de diffusion en continu soit créée avec un format de 128 bits par élément (par exemple, une valeur flottante RVBA) ; une vignette de 64 Ko comporte alors uniquement 4 096 pixels. La taille maximale prise en charge de [**Texture2DArray**](https://msdn.microsoft.com/library/windows/desktop/ff471526), de 16 384\*16 384\*2 048 (avec uniquement une seule image bitmap), nécessite environ 1 Go de stockage dans la table de page si les éléments sont renseignés exclusivement (sans mipmap) avec des entrées de table de 64 bits. Dans le pire des cas, l’ajout de mipmaps provoque l’accroissement du stockage de la table de page entièrement mappé d’environ un tiers ; il est alors d’environ 1,3 Go.

Dans cette configuration, vous aurez accès à environ 10 6 téraoctets de mémoire adressable. Toutefois, il peut exister une limite de quantité de mémoire adressable. Le cas échéant, ces volumes sont réduits, éventuellement autour d’un téraoctet.

Il est également pertinent d’étudier la ressource de diffusion en continu [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) de 16 384\*16 384 avec un format de 32 bits par élément, avec des mipmaps. L’espace requis pour une table de page entièrement renseignée sera d’environ 170 Ko, avec des entrées de table de 64 bits;

Enfin, considérons un exemple valorisant le format BC, par exemple BC7, avec 128 bits par vignette de 4x4 pixels. Un pixel comporte un octet. Une ressource [**Texture2DArray**](https://msdn.microsoft.com/library/windows/desktop/ff471526) de 16 384\*16 384\*2 048 comprenant des mipmaps nécessiterait environ 85 Mo d’espace pour renseigner entièrement cette mémoire dans une table de page. Cela n’est pas si mal, car cela permet à une ressource de diffusion en continu de couvrir 550 gigapixels (512 Go de mémoire dans ce cas).

Dans la pratique, la définition de ces mappages intégraux est impossible, dans la mesure où la quantité de mémoire physique disponible est bien insuffisante pour des opérations de mappage et de référencement simultanées d’un tel volume. Cependant, avec un pool de vignettes, les applications pourraient opter pour la réutilisation des vignettes (prenons l’exemple simple de réutilisation d’une vignette de couleur noire pour les régions noires d’une image) via le pool de vignettes (mappages de table de page) comme outil pour la compression de mémoire.

L’ensemble des entrées de la table de page sont initialement définies sur **NULL**. Par ailleurs, les applications ne peuvent transmettre aucune donnée initiale dans la mémoire de la surface, car cette dernière ne comporte à l’origine aucune sauvegarde de mémoire.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>Dans cette section


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Rubrique</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[Création d’un pool de vignettes](tile-pool-creation.md)</p></td>
<td align="left"><p>Les applications peuvent créer un ou plusieurs pools de vignettes par appareil Direct3D. La taille totale de chaque pool de vignettes est limitée à la taille maximale de la ressource Direct3D 11, qui est d’environ 1/4 de RAM du processeur graphique (GPU).</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Redimensionnement d’un pool de vignettes](tile-pool-resizing.md)</p></td>
<td align="left"><p>Redimensionnez un pool de vignettes pour l’augmenter si l’application nécessite plus de plages de travail pour les ressources de diffusion en continu mappées ou le réduire si moins d’espace est nécessaire.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Suivi des risques ou ressources d’un pool de vignettes](hazard-tracking-versus-tile-pool-resources.md)</p></td>
<td align="left"><p>Pour les ressources autres que les instances de diffusion en continu, Direct3D peut pallier certaines problématiques durant le rendu. Parallèlement, le suivi des risques se déroulant au niveau de la vignette pour les ressources de diffusion en continu, cette activité pourrait s’avérer trop coûteuse sur ces dernières.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Création de ressources de diffusion en continu](creating-streaming-resources.md)

 

 




