---
title: Étape Output Merger (OM)
description: L’étape de fusion de sortie (OM, Output Merger) combine plusieurs types de données de sortie (valeurs du nuanceur de pixels, informations de profondeur et gabarit) avec le contenu de la cible de rendu et des tampons de profondeur/gabarit pour générer le résultat final du pipeline.
ms.assetid: ED2DC4A0-2B92-47AF-884A-BFC2183C78B8
keywords:
- Étape Output Merger (OM)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 177d5a8fed47396fa694bd8fb88baea8d8b7bbb3
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371184"
---
# <a name="output-merger-om-stage"></a>Étape Output Merger (OM)


L’étape de fusion de sortie (OM, Output Merger) combine plusieurs types de données de sortie (valeurs du nuanceur de pixels, informations de profondeur et gabarit) avec le contenu de la cible de rendu et des tampons de profondeur/gabarit pour générer le résultat final du pipeline.

## <a name="span-idpurpose-and-usesspanspan-idpurpose-and-usesspanspan-idpurpose-and-usesspanpurpose-and-uses"></a><span id="Purpose-and-uses"></span><span id="purpose-and-uses"></span><span id="PURPOSE-AND-USES"></span>Rôle et les utilisations


L’étape de fusion de sortie (OM) est la dernière étape pour déterminer quels pixels sont visibles (avec un test de profondeur/gabarit) et pour fusionner couleurs de pixel finales.

L’étape OM génère la couleur de pixel du rendu final à partir d’une combinaison des éléments suivants :

-   État du pipeline
-   Données de pixels générées par les nuanceurs de pixels
-   Contenu des cibles de rendu
-   Contenu des tampons de profondeur/gabarit.

### <a name="span-idblending-overviewspanspan-idblending-overviewspanspan-idblending-overviewspanblending-overview"></a><span id="Blending-overview"></span><span id="blending-overview"></span><span id="BLENDING-OVERVIEW"></span>Vue d’ensemble de la fusion

La fusion combine une ou plusieurs valeurs de pixels pour créer une couleur de pixel finale. Le diagramme suivant illustre le processus de fusion des données de pixel.

![Diagramme illustrant le fonctionnement de la fusion des données](images/d3d10-blend-state.png)

De façon conceptuelle, vous pouvez imaginer que ce processus est mené deux fois à l’étape de fusion de sortie : une première fois pour fusionner les données RVB et une seconde fois pour fusionner les données alpha. Pour découvrir comment créer et définir un état de fusion à l’aide de l’API, voir [la configuration de la fonctionnalité de fusion](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-blend-state).

La fusion à fonction fixe peut être activée individuellement pour chaque cible de rendu. Toutefois, il n’y a qu’un seul ensemble de contrôles de fusion. Toutes les cibles RenderTargets pour lesquelles la fusion est activée sont donc fusionnées de la même manière. Les valeurs de fusion (BlendFactor compris) sont toujours limitées à l’intervalle du format de la cible de rendu avant la fusion. Cette limitation est effectuée par cible de rendu en fonction de leur type. Seuls les formats float16, float11 et float10 ne sont pas limités, afin que les opérations de fusion puissent être réalisées avec une précision ou un intervalle au moins égal au format de sortie. Il s’agit de la seule exception. Les valeurs NaN et zéro signé sont propagées dans tous les cas (y compris les intensités de fusion de 0.0).

Lorsque vous utilisez des cibles de rendu sRVB, au moment de l’exécution, la couleur de la cible de rendu est convertie dans un espace linéaire avant la fusion. Ensuite, la valeur fusionnée finale est de nouveau convertie dans l’espace sRVB, puis la valeur est retransmise à la cible de rendu.

### <a name="span-iddual-source-color-blendingspanspan-iddual-source-color-blendingspanspan-iddual-source-color-blendingspandual-source-color-blending"></a><span id="Dual-source-color-blending"></span><span id="dual-source-color-blending"></span><span id="DUAL-SOURCE-COLOR-BLENDING"></span>Couleur de double-source de fusion

Avec cette fonctionnalité, l’étape de fusion de sortie utilise simultanément les deux sorties du nuanceur de pixels (o0 et o1) en entrée pour l’opération de fusion, avec la cible de rendu unique à l’emplacement 0. Les opérations de fusion valide incluent : l’addition (Add), la soustraction (Subtract) et la soustraction inversée (RevSubtract) L’équation de fusion et le masque d’écriture de sortie indiquent les composants de sortie du nuanceur de pixels. Les composants supplémentaires sont ignorés.

L’écriture sur les autres sorties du nuanceur de pixels (o2, o3 etc.) n’est pas définie. Vous ne pouvez pas écrire sur une cible de rendue qui n’est pas liée à l’emplacement 0. L’écriture dans oDepth est possible pendant une fusion de couleur avec deux sources.

### <a name="span-iddepth-stencil-testspanspan-iddepth-stencil-testspanspan-iddepth-stencil-testspandepth-stencil-testing-overview"></a><span id="Depth-Stencil-Test"></span><span id="depth-stencil-test"></span><span id="DEPTH-STENCIL-TEST"></span>Vue d’ensemble du test de stencil de profondeur

Un tampon de profondeur/gabarit, créé comme ressource de texture, peut contenir les données de profondeur et les données de gabarit. Les données de profondeur sont utilisées pour identifier les pixels les plus proches de la caméra, et les données de gabarit sont utilisées pour masquer les pixels pouvant être mis à jour. Pour finir, les données des valeurs de profondeur et de gabarit sur utilisées à l’étape de fusion de sortie pour déterminer si un pixel doit ou non être visible. Le diagramme suivant illustre le fonctionnement général du test de profondeur/gabarit.

![Diagramme du fonctionnement général du test de profondeur/gabarit](images/d3d10-depth-stencil-test.png)

Pour configurer le test de profondeur/gabarit, voir [Configuration de la fonctionnalité de profondeur-gabarit](configuring-depth-stencil-functionality.md). L’état profondeur/gabarit est encapsulé dans un objet profondeur/gabarit. Cet état peut être spécifié par une application. Sinon, l’étape OM utilise les valeurs par défaut. Si l’échantillonnage multiple est désactivé, les opérations de fusion sont effectuées pixel par pixel. Si l’échantillonnage multiple est activé, la fusion est effectuée par échantillon multiple.

Le processus d’utilisation du tampon de profondeur afin de déterminer les pixels visibles est appelé la mise en mémoire tampon de profondeur (tampon z).

Une fois que les valeurs de profondeur ont atteint l’étape de fusion de sortie (qu’elles proviennent de l’interpolation ou d’un nuanceur de pixels), elles sont toujours limitées d’après le format/la précision du tampon de profondeur, selon les règles en matière de virgule flottante : z = min(Viewport.MaxDepth,max(Viewport.MinDepth,z)). Une fois la limitation effectuée, la valeur de profondeur est comparée (avec DepthFunc) à la valeur du tampon de profondeur existante. Si aucun tampon de profondeur n’a été lié, le test de profondeur est toujours positif.

Si aucun composant de gabarit n’est inclus dans le format du tampon de profondeur, ou si aucun tampon de profondeur n’a été lié, le test de gabarit est toujours positif.

Il n’est possible d’activer qu’un seul tampon de profondeur/gabarit à la fois. Toutes les vues de ressources liées doivent correspondre à la vue de profondeur/gabarit (même taille et même dimensions). La taille des ressources n’a pas d’importance. Seule la taille de la vue doit être identique.

### <a name="span-idsample-maskspanspan-idsample-maskspanspan-idsample-maskspansample-mask-overview"></a><span id="Sample-Mask"></span><span id="sample-mask"></span><span id="SAMPLE-MASK"></span>Présentation de masque l’exemple

Un masque d’échantillon est un masque de couverture 32 bits à plusieurs  échantillons qui détermine les échantillons à mettre à jour dans les cibles de rendu actives. Vous ne pouvez définir qu’un seul masque d’échantillon. Le mappage des bits dans un masque d’échantillon aux échantillons d’une ressource est défini par un utilisateur. Pour un rendu à n échantillons, les n premiers bits (parmi les bits de poids faible) du masque d’échantillon sont utilisés (le nombre maximum étant de 32 bits).

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>entrée


L’étape de fusion de sortie (OM) génère la couleur de pixel du rendu final à partir d’une combinaison des éléments suivants :

-   État du pipeline
-   Données de pixels générées par les nuanceurs de pixels
-   Contenu des cibles de rendu
-   Contenu des tampons de profondeur/gabarit.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Sortie


### <a name="span-idoutput-write-mask-overviewspanspan-idoutput-write-mask-overviewspanspan-idoutput-write-mask-overviewspanoutput-write-mask-overview"></a><span id="Output-write-mask-overview"></span><span id="output-write-mask-overview"></span><span id="OUTPUT-WRITE-MASK-OVERVIEW"></span>Vue d’ensemble de masque d’écriture de sortie

Le masque d’écriture de sortie permet de contrôler (pour chaque composant) les données à écrire sur une cible de rendu.

### <a name="span-idmultiple-render-targets-overviewspanspan-idmultiple-render-targets-overviewspanspan-idmultiple-render-targets-overviewspanmultiple-render-targets-overview"></a><span id="Multiple-render-targets-overview"></span><span id="multiple-render-targets-overview"></span><span id="MULTIPLE-RENDER-TARGETS-OVERVIEW"></span>Vue d’ensemble des cibles de rendu plusieurs

Un nuanceur de pixels doit être utilisé pour le rendu d’au moins 8 cibles distinctes ayant toutes le même type (buffer, Texture1D, Texture1DArray, etc.). Par ailleurs, toutes les cibles de rendu doivent avoir les mêmes valeurs de dimensions (largeur, hauteur, profondeur, taille de tableau et nombres d’échantillons). Chaque cible de rendu peut avoir un format de données différent.

Vous pouvez utiliser n’importe quelle combinaison d’emplacements cibles de rendu (jusqu’à 8). Toutefois, une vue de ressource ne peut pas être liée à plusieurs emplacements de cible de rendu simultanément. Une vue peut être réutilisée tant que les ressources ne sont pas utilisées simultanément.

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
<td align="left"><p><a href="configuring-depth-stencil-functionality.md">Configuration de la fonctionnalité du stencil de profondeur</a></p></td>
<td align="left"><p>Cette section décrit les étapes de configuration de la mémoire tampon de profondeur-gabarit et l’état de profondeur-gabarit pour l’étape de fusion/sortie.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Pipeline graphique](graphics-pipeline.md)

 

 




