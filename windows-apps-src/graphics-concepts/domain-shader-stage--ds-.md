---
title: "Étape du nuanceur de domaine (DS)"
description: "L’étape du nuanceur de domaine (DS, Domain Shader) calcule la position de vertex d’un point subdivisé dans le patch de sortie ; ce calcul porte sur la position de vertex correspondant à chaque échantillon de domaine."
ms.assetid: 673CC04A-A74F-495F-AFB7-49157538749C
keywords:
- "Étape du nuanceur de domaine (DS)"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 8bb2b3c731f3612b7142bfd136b3d57d277c2e03
ms.lasthandoff: 02/07/2017

---

# <a name="domain-shader-ds-stage"></a>Étape du nuanceur de domaine (DS)


L’étape du nuanceur de domaine (DS, Domain Shader) calcule la position de vertex d’un point subdivisé dans le patch de sortie ; ce calcul porte sur la position de vertex correspondant à chaque échantillon de domaine. Un nuanceur de domaine est exécuté une fois par point de sortie de l’étape du paveur et dispose d’un accès en lecture seule au patch de sortie et aux constantes de patch de sortie du nuanceur de coque, ainsi qu’aux coordonnées UV de sortie de l’étape du paveur.

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Objectif et cas d’utilisation


L’étape du nuanceur de domaine (DS) produit en sortie la position de vertex d’un point subdivisé dans le patch de sortie, en fonction de l’entrée provenant de l’[étape du nuanceur de coque (HS)](hull-shader-stage--hs-.md) et de l’[étape du paveur (TS)](tessellator-stage--ts-.md).

![diagramme de l’étape du nuanceur de domaine](images/d3d11-domain-shader.png)

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Entrée


-   Un nuanceur de domaine consomme les points de contrôle de sortie de l’[étape du nuanceur de coque (HS)](hull-shader-stage--hs-.md). Les sorties du nuanceur de coque sont les suivantes :
    -   Points de contrôle.
    -   Données de constante de patch.
    -   Facteurs de pavage. Les facteurs de pavage peuvent inclure les valeurs utilisées par le paveur à fonction fixe, ainsi que les valeurs brutes (avant l’arrondissement sous forme d’entier par le pavage), ce qui facilite notamment la géomorphose.
-   Un nuanceur de domaine est invoqué une fois par coordonnée de sortie à partir de l’[étape du paveur (TS)](tessellator-stage--ts-.md).

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Sortie


-   L’étape du nuanceur de domaine (DS) produit en sortie la position de vertex d’un point subdivisé dans le patch de sortie.

Une fois le nuanceur de domaine exécuté, le pavage prend fin, et les données du pipeline progressent vers l’étape suivante du pipeline, telle que l’[étape du nuanceur de géométrie (GS)](geometry-shader-stage--gs-.md) et l’[étape du nuanceur de pixels (PS)](pixel-shader-stage--ps-.md). Un nuanceur de géométrie qui attend des primitives avec une contiguïté (par exemple, 6 vertex par triangle) n’est pas valide lorsque le pavage est actif (cela entraîne un comportement indéfini qui sera signalé par la couche de débogage).

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Exemple


```
void main( out    MyDSOutput result, 
           float2 myInputUV : SV_DomainPoint, 
           MyDSInput DSInputs,
           OutputPatch<MyOutPoint, 12> ControlPts, 
           MyTessFactors tessFactors)
{
     ...

     result.Position = EvaluateSurfaceUV(ControlPoints, myInputUV);
}
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Articles connexes


[Pipeline graphique](graphics-pipeline.md)

 

 




