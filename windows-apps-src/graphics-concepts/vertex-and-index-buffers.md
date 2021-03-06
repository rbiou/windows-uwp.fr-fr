---
title: Mémoires tampons de sommets et d’index
description: Les mémoires tampons de vertex comportent des données de vertex ; les vertex sont traités dans le cadre des opérations de transformation, d'éclairage et de découpage.
ms.assetid: 8A39CD23-85FB-4424-9AC3-37919704CD68
keywords:
- Mémoires tampons de sommets et d’index
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d2ea6ce4060957ade5dd1007389be51176440f04
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593574"
---
# <a name="vertex-and-index-buffers"></a>Mémoires tampons de sommets et d’index


Les *mémoires tampons de vertex* comportent des données de vertex ; les vertex sont traités dans le cadre des opérations de transformation, d'éclairage et de découpage. Les *mémoires tampons d’index* sont des mémoires tampons qui contiennent des données d’index, lesquelles sont des décalages entiers dans des mémoires tampons de vertex, utilisés pour le rendu des primitives.

Les mémoires tampons de vertex peuvent contenir tout type de vertex (transformé ou non transformé, allumé ou éteint) qui peut être rendu. Vous pouvez traiter les sommets dans une mémoire tampon de vertex pour effectuer des opérations telles que la transformation, l'éclairage, ou la génération d'indicateurs de découpage. La transformation est toujours effectuée.

La flexibilité des mémoires tampons de vertex en fait des points d’arrêt parfaits pour réutiliser une géométrie transformée. Vous pouvez créer une mémoire tampon de vertex unique, transformer, éclairer et découper les sommets dedans, puis restituer le modèle dans la scène autant de fois que nécessaire, sans avoir à le retransformer, même avec des changements d’état de rendu entrelacés. Cela se révèle particulièrement utile lors du rendu de modèles qui utilisent plusieurs textures : la géométrie est transformée une seule fois, puis des parties de celle-ci peuvent être rendues en fonction des besoins, entrelacées avec les modifications de texture requises. Les changements d’état de rendu effectués après le traitement des sommets prennent effet au prochain traitement des sommets.

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
<td align="left"><p><a href="introduction-to-buffers.md">Introduction aux mémoires tampons</a></p></td>
<td align="left"><p>Une ressource de mémoire tampon est un ensemble de données dont le type a été intégralement spécifié, regroupées en éléments. Les mémoires tampons stockent des données, telles que les coordonnées de texture dans une <em>mémoire tampon de vertex</em>, les index dans une <em>mémoire tampon d’index</em>, les données constantes de nuanceur dans une <em>mémoire tampon constante</em>, les vecteurs de position, les vecteurs normaux ou l’état de l’appareil.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="index-buffers.md">Tampons d’index</a></p></td>
<td align="left"><p>Les <em>mémoires tampons d’index</em> sont des mémoires tampons qui contiennent des données d’index, lesquelles sont des décalages entiers dans des mémoires tampons de vertex, utilisés pour le rendu des primitives.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Guide d’apprentissage graphiques Direct3D](index.md)

 

 




