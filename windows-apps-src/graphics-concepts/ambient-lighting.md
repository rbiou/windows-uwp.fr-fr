---
title: "Éclairage ambiant"
description: "L&quot;éclairage ambiant permet de donner à une scène un éclairage constant."
ms.assetid: C34FA65A-3634-4A4B-B183-4CDA89F4DC95
keywords:
- "Éclairage ambiant"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 77865a483f226fba912c03e2f9abe17eaa7fbee0
ms.lasthandoff: 02/07/2017

---

# <a name="ambient-lighting"></a>Éclairage ambiant


L'éclairage ambiant permet de donner à une scène un éclairage constant. Cette fonction permet d'éclairer tous les vertex d'objets de la même manière car elle ne dépend pas des autres facteurs d'éclairage, comme les normales de vertex, la direction de la lumière, la position de la lumière, la plage ou l'atténuation. L'éclairage ambiant est constant dans toutes les directions et colore tous les pixels d’un objet de la même façon. Il présente une grande rapidité de calcul, mais donne aux objets un aspect plat et irréaliste.

L'éclairage ambiant est le type d’éclairage le plus rapide, mais qui génère les résultats les moins réalistes. Direct3D contient une seule propriété d'éclairage ambiant global que vous pouvez utiliser sans avoir à créer la moindre lumière. Vous pouvez également définir n’importe quel objet clair pour apporter un éclairage ambiant.

L’éclairage ambiant pour une scène est décrit par l’équation suivante.

Éclairage ambiant = Cₐ\*\[Gₐ + sum(Atten<sub>i</sub>\*Spot<sub>i</sub>\*L<sub>ai</sub>)\]

Où :

| Paramètre         | Valeur par défaut | Type          | Description                                                                                                       |
|-------------------|---------------|---------------|-------------------------------------------------------------------------------------------------------------------|
| Cₐ                | (0,0,0,0)     | D3DCOLORVALUE | Couleur ambiante du matériau                                                                                            |
| Gₐ                | (0,0,0,0)     | D3DCOLORVALUE | Couleur ambiante globale                                                                                              |
| Atten<sub>i</sub> | (0,0,0,0)     | D3DCOLORVALUE | Atténuation lumineuse de la lumière ith. Voir [Atténuation et facteur de point lumineux](attenuation-and-spotlight-factor.md). |
| Spot<sub>i</sub>  | (0,0,0,0)     | D3DVECTOR     | Facteur de point lumineux de la lumière ith. Voir [Atténuation et facteur de point lumineux](attenuation-and-spotlight-factor.md).  |
| sum               | Non applicable           | Non applicable           | Somme de la lumière ambiante                                                                                          |
| L<sub>ai</sub>    | (0,0,0,0)     | D3DVECTOR     | Couleur ambiante lumineuse de la lumière ith                                                                              |

 

Cₐ possède l'une des valeurs suivantes :

-   couleur1 du vertex, si AMBIENTMATERIALSOURCE = D3DMCS\_COLOR1, et si la première couleur du vertex est indiquée dans la déclaration du vertex.
-   couleur2 du vertex, si AMBIENTMATERIALSOURCE = D3DMCS\_COLOR2, et si la deuxième couleur du vertex est indiquée dans la déclaration du vertex.
-   couleur ambiante du matériau.

**Remarque**   si l’une des options AMBIENTMATERIALSOURCE est utilisée et si la couleur du vertex n’est pas spécifiée, la couleur ambiante du matériau est utilisée.

 

Pour utiliser la couleur ambiante du matériau, utilisez SetMaterial comme indiqué dans l’exemple de code ci-dessous.

Gₐ est la couleur ambiante globale. Ce paramètre est défini à l’aide de SetRenderState(D3DRS\_AMBIENT). Il existe une seule couleur ambiante globale dans une scène Direct3D. Ce paramètre n’est pas associé à un objet lumineux Direct3D.

L<sub>ai</sub> est la couleur ambiante de la lumière ith dans la scène. Chaque lumière Direct3D possède un jeu de propriétés, parmi lesquelles figure la couleur ambiante. Le terme sum(L<sub>ai</sub>) correspond à la somme de toutes les couleurs ambiantes dans la scène.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Exemple


Dans cet exemple, la couleur de l'objet est définie à partir de la lumière ambiante de la scène et d'une couleur ambiante de matériau.

```
#define GRAY_COLOR  0x00bfbfbf

Ambient.r = 0.75f;
Ambient.g = 0.0f;
Ambient.b = 0.0f;
Ambient.a = 0.0f;
```

Selon l’équation, la couleur obtenue pour les vertex d’objet combine la couleur du matériau et la couleur de la lumière.

Les deux illustrations suivantes montrent la couleur du matériau (grise) et la couleur de la lumière (rouge vif).

![Illustration d’une sphère grise](images/amb1.jpg)![Illustration d’une sphère rouge](images/lightred.jpg)

La scène obtenue est présentée dans l’illustration suivante. Le seul objet de la scène est une sphère. La lumière ambiante éclaire tous les vertex d’objet de la même couleur. Elle ne dépend ni de la normale du vertex, ni de la direction de la lumière. La sphère apparaît donc comme un cercle 2D, car il n’y a aucune différence d'ombrage autour de la surface de l’objet.

![Illustration d’une sphère avec un éclairage ambiant](images/lighta.jpg)

Pour donner à des objets un aspect plus réaliste, appliquez un éclairage diffus ou spéculaire en plus de l’éclairage ambiant.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Formules mathématiques d’éclairage](mathematics-of-lighting.md)

 

 




