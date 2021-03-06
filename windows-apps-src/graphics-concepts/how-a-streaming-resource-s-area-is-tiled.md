---
title: Restitution de la surface d’une ressource de diffusion en continu sous forme de mosaïque
description: Lorsque vous créez une ressource de diffusion en continu, les dimensions, la taille des éléments de format et le nombre de mipmaps et/ou de sections de tableau (le cas échéant) déterminent le nombre de vignettes requises pour couvrir toute la zone de la surface.
ms.assetid: 3485FD8D-2A06-4B0A-8810-ECF37736F94B
keywords:
- Restitution de la surface d’une ressource de diffusion en continu sous forme de mosaïque
- zone de ressource, découpage en vignettes
- tableaux de tailles, ressources, découpage en vignettes
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 28fac411ad814167dcef3b02424c866bd3453344
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370596"
---
# <a name="how-a-streaming-resources-area-is-tiled"></a>Restitution de la surface d’une ressource de diffusion en continu sous forme de mosaïque


Lorsque vous créez une ressource de diffusion en continu, les dimensions, la taille des éléments de format et le nombre de mipmaps et/ou de sections de tableau (le cas échéant) déterminent le nombre de vignettes requises pour couvrir toute la zone de la surface. La disposition des pixels/octets au sein des vignettes est déterminée par l’implémentation. Le nombre de pixels qui tiennent dans une vignette, selon la taille des éléments de format, est fixe et identique, que vous effectuiez ou non un réagencement standard des données en surface.

Le nombre de vignettes qui seront utilisées par une taille de surface et une largeur d’élément de format spécifiques est parfaitement défini et prévisible grâce aux tableaux figurant dans les articles répertoriés ci-dessous. Pour les ressources qui contiennent des mipmaps ou pour les cas dans lesquels les dimensions de surface ne remplissent pas une vignette, il existe certaines contraintes ; pour plus d’informations, consultez l’article [Compression de mipmaps](mipmap-packing.md).

Différentes ressources de diffusion en continu peuvent pointer sur une mémoire identique présentant différents formats, à condition que les applications ne s’appuient pas sur les résultats d’écriture en mémoire dans un format spécifique et sur les résultats de lecture en mémoire dans un autre format. Toutefois, les applications peuvent se fonder sur les résultats d’écriture en mémoire dans un format et sur les résultats de lecture dans un autre format si ces formats appartiennent à la même famille (autrement dit, s’ils ont le même format parent sans type). Par exemple, DXGI\_FORMAT\_R8G8B8A8\_UNORM et DXGI\_FORMAT\_R8G8B8A8\_UINT sont compatibles entre eux, mais pas avec DXGI\_FORMAT\_R16G16\_UNORM.

Il existe une exception bien définie à cette règle dans le cas où des données d’un format sont fusionnées dans un autre format : si tous les bits d’une vignette se composent uniquement de 0, cette vignette peut être utilisée avec n’importe quel format interprétant ce contenu en mémoire en tant que 0 (quelle que soit la configuration mémoire). Par conséquent, une vignette ont pu être effacée à 0 x 00 au format DXGI\_FORMAT\_R8\_UNORM, puis utilisé avec un format tel que DXGI\_FORMAT\_R32G32\_FLOAT et il apparaîtrait est le contenu toujours (0.0f, 0.0f).

La disposition des données dans une vignette ne dépend pas de l’emplacement de mappage de la vignette dans une ressource globale. Ceci permet donc par exemple de réutiliser une vignette à différents emplacements d’une surface en une seule opération et d’obtenir systématiquement un comportement cohérent.

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
<td align="left"><p><a href="texture2d-and-texture2darray-subresource-tiling.md">Mosaïque subresource Texture2D et Texture2DArray</a></p></td>
<td align="left"><p>Les tableaux suivants montrent comment des sous-ressources <a href="https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture2d"><strong>Texture2D</strong></a> et <a href="https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture2darray"><strong>Texture2DArray</strong></a> sont restituées sous forme de tuiles.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="texture3d-subresource-tiling.md">Mosaïque subresource Texture3D</a></p></td>
<td align="left"><p>Ce tableau montre comment des sous-ressources <a href="https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture3d"><strong>Texture3D</strong></a> sont restituées sous forme de tuiles.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="buffer-tiling.md">Mosaïque de la mémoire tampon</a></p></td>
<td align="left"><p>Une ressource de <a href="introduction-to-buffers.md">mémoire tampon</a> est divisée en vignettes de 64 Ko, avec un espace vide dans la dernière vignette si la taille n’est pas un multiple de 64 Ko.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="mipmap-packing.md">Compression de mipmap</a></p></td>
<td align="left"><p>Il est possible de compresser plusieurs mips (par section de tableau) dans différentes vignettes, en fonction des dimensions et du format de la ressource de diffusion en continu, du nombre de mipmaps et des sections de tableau.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Création de ressources de diffusion en continu](creating-streaming-resources.md)

 

 




