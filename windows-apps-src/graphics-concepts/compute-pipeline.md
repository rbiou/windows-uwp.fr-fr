---
title: Pipeline de calcul
description: Le pipeline de calcul Direct 3D est conçu pour traiter les calculs pouvant être effectués principalement en parallèle du pipeline graphique.
ms.assetid: 355B66C6-C0DF-47BA-A9C9-7AFA50B5B614
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 911546f1c2973a79aea4b597a47352149a4e4210
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651114"
---
# <a name="compute-pipeline"></a>Pipeline de calcul


\[Certaines informations sont relatives à la version préliminaire du produit qui peut être substantiellement modifié avant sa commercialisation. Microsoft exclut toute garantie, expresse ou implicite, concernant les informations fournies ici.\]


Le pipeline de calcul Direct 3D est conçu pour traiter les calculs pouvant être effectués principalement en parallèle du pipeline graphique. Le pipeline de calcul comporte seulement quelques étapes, où des données sont transmises de l’entrée vers la sortie via l’étape du nuanceur de calcul programmable.

| | |
|-|-|
|Objectif|Comme les autres nuanceurs programmables, l'[étape Compute Shader (CS)](compute-shader-stage--cs-.md) est conçue et implémentée avec HLSL. Un nuanceur de calcul assure des opérations de calcul général à haut débit et tire parti du grand nombre de processeurs parallèles sur le processeur graphique (GPU). Le nuanceur de calcul fournit des fonctionnalités de partage de mémoire et de synchronisation de threads pour prendre en charge des méthodes de programmation parallèles plus efficaces.|
|Entrée|Contrairement à d’autres nuanceurs programmables, la définition d’entrée est abstraite. L’entrée peut, par nature, avoir une, deux ou trois dimensions, qui déterminent le nombre d’appels du nuanceur de calcul à exécuter. Il est possible de définir des données partagées pour un ensemble d’appels à lire.|
|Sortie|Les données de sortie du nuanceur de calcul, qui peuvent être extrêmement variées, peuvent être synchronisées avec le pipeline de rendu graphique lorsque les données calculées sont requises.|
| | |




<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">Like other programmable shaders, <a href="#compute-shader-stage--cs-.md">Compute Shader (CS) stage</a> is designed and implemented with HLSL. A compute shader provides high-speed general purpose computing and takes advantage of the large numbers of parallel processors on the graphics processing unit (GPU). The compute shader provides memory sharing and thread synchronization features to allow more effective parallel programming methods.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left">Unlike other programmable shaders, the definition of input is abstract. The input can be one, two or three-dimensional in nature, determining the number of invocations of the compute shader to execute. It is possible to define shared data for one set of invocations to read.</td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">Output data from the compute shader, which can be highly varied, can be synchronized with the graphics rendering pipeline when the computed data is required.</td>
</tr>
</tbody>
</table>
-->

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Guide d’apprentissage graphiques Direct3D](index.md)

 

 
