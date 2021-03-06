---
title: Interpolation de triangles
description: Pendant le rendu, le pipeline interpole les données des vertex pour chaque triangle.
ms.assetid: 1A76DD78-CED7-42BE-BA81-B9050CD3AF9B
keywords:
- Interpolation de triangles
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e8017cd75ed3dfd4129d6c15d668648792cc8d0a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592904"
---
# <a name="triangle-interpolation"></a>Interpolation de triangles


Pendant le rendu, le pipeline interpole les données des vertex pour chaque triangle. Les données de vertex peuvent représenter un large éventail de données et inclure (sans s'y limiter) les données suivantes : la couleur diffuse, la couleur spéculaire, l'alpha diffus (opacité du triangle), l'alpha spéculaire et un facteur brouillard. Pour le pipeline de vertex programmables, le facteur brouillard est pris dans le registre de brouillards. Pour le pipeline de vertex à fonction fixe, le facteur brouillard provient de l'alpha spéculaire.

Pour certaines données de vertex, l’interpolation dépend du mode de trame actuel, comme suit :

| Mode d’ombrage | Description                                                                                                                                                                 |
|--------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Plat         | Seul le facteur brouillard est interpolé en mode d’ombrage plat. Pour toutes les autres valeurs interpolées, la couleur du premier vertex du triangle est appliquée à tout le visage. |
| Gouraud      | Une interpolation linéaire est réalisée entre les trois vertex.                                                                                                               |

 

Les couleurs diffuse et spéculaire sont traitées différemment en fonction du modèle de couleurs. Dans le modèle de couleurs RVB, le système utilise les composants de couleur rouge, vert et bleu dans l’interpolation.

Le composant alpha d’une couleur est traité comme une valeur interpolée distincte, car les pilotes de périphériques peuvent implémenter la transparence de deux manières différentes : à l’aide de textures ou de maillage.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Systèmes de coordonnées et géométrie](coordinate-systems-and-geometry.md)

 

 




