---
title: Introduction aux règles de rastérisation
description: Il est fréquent que les points spécifiés pour les vertex ne correspondent pas précisément aux pixels à l’écran. Dans ce cas, Direct3D applique des règles de rastérisation de triangle afin de déterminer les pixels qui s’appliquent à un triangle spécifique.
ms.assetid: 4232CDBA-F669-4417-9378-F9013E83462C
keywords:
- Introduction aux règles de rastérisation
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c635d0efad627734e0305c687e015ad183764717
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371054"
---
# <a name="introduction-to-rasterization-rules"></a>Introduction aux règles de rastérisation


Il est fréquent que les points spécifiés pour les vertex ne correspondent pas précisément aux pixels à l’écran. Dans ce cas, Direct3D applique des règles de rastérisation de triangle afin de déterminer les pixels qui s’appliquent à un triangle spécifique.

Il s’agit d’une présentation simplifiée des règles de rastérisation. Pour plus d’informations, voir [Règles de rastérisation](rasterization-rules.md). Voir également l’[étape du rastériseur (RS)](rasterizer-stage--rs-.md).

## <a name="span-idtrianglerasterizationrulesspanspan-idtrianglerasterizationrulesspanspan-idtrianglerasterizationrulesspantriangle-rasterization-rules"></a><span id="Triangle_Rasterization_Rules"></span><span id="triangle_rasterization_rules"></span><span id="TRIANGLE_RASTERIZATION_RULES"></span>Règles de rastérisation triangle


Pour renseigner les géométries, Direct3D commence par le coin supérieur gauche. C’est la convention utilisée pour les rectangles dans GDI et OpenGL. Dans Direct3D, le centre du pixel est le point déterminant. Si le centre se situe à l’intérieur d’un triangle, le pixel fait partie du triangle. Les centres de pixels se trouvent à des coordonnées d’entiers.

La description des règles de rastérisation des triangles utilisées par Direct3D ne s’applique pas nécessairement à l’ensemble du matériel disponible. Vos tests peuvent dévoiler des variations mineures dans l’implémentation de ces règles.

L’illustration suivante représente un rectangle dont le coin supérieur gauche se trouve aux coordonnées (0, 0) et le coin inférieur droit aux coordonnées (5, 5). Ce rectangle occupe 25 pixels, tel qu’attendu. La largeur du rectangle correspond à la soustraction « droite moins gauche ». La hauteur correspond à la soustraction « bas moins haut ».

![un carré numéroté divisé en six lignes et colonnes](images/pixmap.png)

Dans la convention de remplissage par le « coin supérieur gauche », le *haut* fait référence à l’emplacement vertical des portées horizontales, tandis que la *gauche* fait référence à l’emplacement horizontal des pixels d’une portée. Un bord ne peut pas être un bord supérieur, sauf s’il est horizontal. En général, la plupart des triangles présentent uniquement des bordures de droite et de gauche. L’illustration suivante représente des bordures supérieure et droite.

![carré numéroté comportant deux triangles](images/triedge.png)

La convention de remplissage par le « coin supérieur gauche » détermine l’action entreprise par Direct3D lorsqu’un triangle passe par le centre d’un pixel. L’illustration suivante représente deux triangles, l’un aux coordonnées (0, 0), (5, 0) et (5, 5), et l’autre aux coordonnées (0, 5), (0, 0) et (5, 5). Ici, le premier triangle occupe 15 pixels (en noir), tandis que le second occupe seulement 10 pixels (en gris), car la bordure partagée est la bordure gauche du premier triangle.

![carré numéroté représentant 2 triangles](images/twotris.png)

Si vous définissez un rectangle présentant un coin supérieur gauche aux coordonnées (0,5, 0,5) et un coin inférieur droit aux coordonnées (2,5, 4,5), le point central de ce rectangle se trouve aux coordonnées (1.5, 2.5). Lorsque le rastériseur Direct3D crénelle ce rectangle, le centre de chaque pixel se trouve explicitement à l’intérieur de chacun des 4 triangles ; la convention de « remplissage par le coin supérieur gauche » n’est pas requise. L’illustration suivante représente cette configuration. Les pixels du rectangle sont marqués en fonction du triangle dans lequel Direct3D les intègre.

![carré numéroté comportant un rectangle divisé en 4 triangles](images/noambig.png)

Si vous déplacez le rectangle de l’illustration précédente de façon à ce que le coin supérieur gauche se trouve aux coordonnées (1,0, 1,0), son coin inférieur droit se trouve aux coordonnées (3,0, 5,0) et son point central aux coordonnées (2,0, 3,0). Direct3D applique la convention du «remplissage par le coin supérieur gauche ». La bordure de la plupart des pixels de ce rectangle chevauche au moins 2 triangles, tel que représenté dans l’illustration suivante.

![carré numéroté comportant un rectangle divisé en 4 triangles](images/fillrule.png)

Pour les 2 rectangles, les mêmes pixels sont affectés, tel que représenté dans l’illustration suivante.

![pixels affectés par les 2 carrés numérotés précédents](images/samepix.png)

## <a name="span-idpointandlinerulesspanspan-idpointandlinerulesspanspan-idpointandlinerulesspanpoint-and-line-rules"></a><span id="Point_and_Line_Rules"></span><span id="point_and_line_rules"></span><span id="POINT_AND_LINE_RULES"></span>Point et des règles de ligne


Les points sont affichés comme les sprites de points, qui apparaissent comme des quadrilatères alignés sur l’écran. Ici donc, les règles d’affichage des polygones sont aussi respectées.

Les règles de rendu de ligne sans anti-crénelage sont exactement identiques aux règles dédiées aux [lignes GDI](https://docs.microsoft.com/windows/desktop/gdi/lines).

## <a name="span-idpointspriterulesspanspan-idpointspriterulesspanspan-idpointspriterulesspanpoint-sprite-rules"></a><span id="Point_Sprite_Rules"></span><span id="point_sprite_rules"></span><span id="POINT_SPRITE_RULES"></span>Règles du Sprite de point


Les sprites de points et les primitives de correctifs sont rastérisés comme si les primitives étaient préalablement crénelées en triangles et les triangles résultants rastérisés.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Appareils](devices.md)

[Étape du rastériseur (RS)](rasterizer-stage--rs-.md)

[Règles de rastérisation](rasterization-rules.md)

 

 




