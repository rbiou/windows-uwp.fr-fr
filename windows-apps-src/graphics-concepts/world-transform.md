---
title: Transformation du monde
description: Une transformation universelle change les coordonnées à partir de l'espace du modèle, dans lequel les sommets sont définis par rapport à l’origine locale d’un modèle, vers l'espace universel.
ms.assetid: 767B032C-69D0-4583-8FEB-247F4C41E31D
keywords:
- Transformation du monde
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: af694ed32d845e518f4189f75309f1f371743f90
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662414"
---
# <a name="world-transform"></a>Transformation du monde


Une transformation universelle change les coordonnées à partir de l'espace du modèle, dans lequel les sommets sont définis par rapport à l’origine locale d’un modèle, vers l'espace universel. Dans l’espace universel, les sommets sont définis par rapport à l’origine commune de tous les objets d'une scène. La transformation universelle place un modèle dans le monde.

## <span id="What_Is_a_World_Transform"></span><span id="what_is_a_world_transform"></span><span id="WHAT_IS_A_WORLD_TRANSFORM"></span>


Le diagramme suivant montre la relation entre le système de coordonnées universelles et le système de coordonnées local d’un modèle.

![Diagramme indiquant la liaison entre les coordonnées universelles et les coordonnées locales](images/worldcrd.png)

La transformation universelle peut inclure n’importe quelle combinaison de translations, rotations et mises à l'échelle.

## <a name="span-idsettingupaworldmatrixxmlspansetting-up-a-world-matrix"></a><span id="SETTING_UP_A_WORLD_MATRIX.XML"></span>Configuration d’une matrice du monde


Comme pour toute autre transformation, créez la transformation universelle en concaténant une série de matrices en une matrice unique qui contient la somme totale de leurs effets. Dans le cas le plus simple, lorsqu’un modèle est à l’origine du monde et que ses axes de coordonnées locales sont orientés de façon identique à l’espace universel, la matrice universelle est la matrice d’identité. Plus généralement, la matrice universelle est une combinaison d’une translation en espace universel et éventuellement d'une ou plusieurs rotations pour faire tourner le modèle en fonction des besoins.

Direct3D utilise les matrices universelles et d'affichage que vous définissez pour configurer plusieurs structures de données internes. Chaque fois que vous définissez une nouvelle matrice universelle ou d'affichage, le système recalcule les structures internes associées. Définir ces matrices fréquemment, par exemple, des milliers de fois par image, nécessite un temps de calcul important. Vous pouvez réduire le nombre de calculs requis en concaténant vos matrices universelles et d'affichage dans une matrice universelle-d'affichage que vous définissez comme matrice universelle, puis en définissant la matrice d'affichage sur l’identité. Conserver des copies mises en cache de chaque matrice universelle et d'affichage, afin de pouvoir modifier, concaténer et réinitialiser la matrice universelle en fonction des besoins.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Transformations](transforms.md)

 

 




