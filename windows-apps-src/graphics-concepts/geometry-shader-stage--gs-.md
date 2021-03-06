---
title: Étape Geometry Shader (GS)
description: L’étape du nuanceur de géométrie (GS, Geometry Shader) traite des primitives complètes (triangles, lignes et points), ainsi que leurs vertex adjacents.
ms.assetid: 8A1350DD-B006-488F-9DAF-14CD2483BA4E
keywords:
- Étape Geometry Shader (GS)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 0ea3e7ec73b042eeef560af3d88754afdfa5b441
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370459"
---
# <a name="geometry-shader-gs-stage"></a>Étape Geometry Shader (GS)


L’étape du nuanceur de géométrie (GS, Geometry Shader) traite des primitives complètes (triangles, lignes et points), ainsi que leurs vertex adjacents. Cette étape se révèle utile pour les algorithmes tels que l’expansion de point-sprites, les systèmes de particules dynamiques et la génération de volumes d’ombre. Cette étape prend en charge l’amplification et le filtrage de géométrie.

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Rôle et les utilisations


L’étape du nuanceur de géométrie traite des primitives complètes : triangles (3 vertex présentant jusqu’à 3 vertex adjacents), lignes (2 vertex présentant jusqu’à 2 vertex adjacents) et points (1 vertex).

![illustration d’un triangle et d’une ligne avec des vertex adjacents](images/d3d10-gs.png)

Le nuanceur de géométrie prend également en charge l’amplification et le filtrage de géométrie limités. Pour une primitive d’entrée donnée, le nuanceur de géométrie peut ignorer la primitive ou émettre une ou plusieurs nouvelles primitives.

L’étape du nuanceur de géométrie (GS) est une étape de nuanceur programmable ; elle apparaît sous la forme d’un bloc arrondi dans le diagramme du [pipeline graphique](graphics-pipeline.md). Cette étape du nuanceur expose sa propre fonctionnalité unique, qui repose sur les modèles de nuanceur (voir l’article [Noyau de nuanceur commun](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-common-core)).

L’étape du nuanceur de géométrie est bien adaptée à différents algorithmes, notamment :

-   Expansion de point-sprites
-   Systèmes de particules dynamiques
-   Génération de fourrure
-   Génération de volumes d’ombre
-   Rendu sous forme de cubemap en une seule passe
-   Échange de matériaux par primitive
-   Configuration de matériaux par primitive : cette fonctionnalité inclut la génération de coordonnées barycentriques en tant que données de primitive pour permettre à un nuanceur de pixels d’effectuer une interpolation d’attributs personnalisés.

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>entrée


L’étape du nuanceur de géométrie exécute le code du nuanceur spécifié par l’application avec des primitives complètes en guise d’entrée et la possibilité de générer des vertex en sortie. Contrairement aux nuanceurs de vertex, qui opèrent sur un seul vertex, les entrées du nuanceur de géométrie correspondent aux vertex d’une primitive complète (trois vertex pour les triangles, deux vertex pour les lignes ou un seul vertex pour les points). Les nuanceurs de géométrie peuvent également introduire en tant qu’entrée les données de vertex relatives aux primitives adjacentes aux arêtes (trois vertex supplémentaires pour un triangle, et deux vertex supplémentaires pour une ligne).

L’étape du nuanceur de géométrie peut consommer le **SV\_PrimitiveID** valeur générée par le système qui est généré automatiquement par le [étape de l’assembleur d’entrée (IA)](input-assembler-stage--ia-.md). Ceci permet d’extraire ou de calculer les données par primitive selon les besoins.

Lorsqu’un nuanceur de géométrie est actif, il est invoqué une fois par primitive transmise ou générée précédemment dans le pipeline. Chaque invocation du nuanceur de géométrie voit en tant qu’entrée les données de la primitive invoquant le nuanceur, qu’il s’agisse d’un point unique, d’une ligne unique ou d’un triangle unique. Une bande de triangles provenant d’une étape précédente du pipeline entraînerait une invocation du nuanceur de géométrie pour chacun des triangles de la bande (comme si la bande était développée sous la forme d’une liste de triangles). Toutes les données d’entrée pour chaque vertex de la primitive concernée sont disponibles (autrement dit, 3 vertex pour un triangle), ainsi que les données des vertex adjacents si elles sont applicables et disponibles.

Abréviations de vertex courantes :

|     |                 |
|-----|-----------------|
| TV  | Vertex de triangle |
| LV  | Vertex de ligne     |
| AV  | Vertex adjacent |

 

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Sortie


L’étape du nuanceur de géométrie (GS) peut produire en sortie plusieurs vertex formant une seule topologie sélectionnée. Les topologies de sortie du nuanceur de géométrie disponibles sont **tristrip**, **linestrip** et **pointlist**. Le nombre de primitives émises peut varier librement au sein d’une invocation du nuanceur de géométrie, bien que le nombre maximal de vertex pouvant être émis doive être déclaré de manière statique. Les longueurs de bande émises à partir d’une invocation du nuanceur de géométrie peuvent être arbitraires, et d’autres bandes peuvent être créées par le biais de la fonction HLSL [RestartStrip](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-so-restartstrip).

L’exécution d’une instance du nuanceur de géométrie est effectuée de manière atomique à partir d’autres invocations, à ceci près que les données ajoutées aux flux sont des données en série. Les sorties d’une invocation donnée d’un nuanceur de géométrie sont indépendantes des autres invocations (bien que l’ordre soit respecté). Un nuanceur de géométrie générant des bandes de triangles démarrera une nouvelle bande à chaque invocation.

La sortie du nuanceur de géométrie peut être transmise à l’étape du rastériseur et/ou à un tampon de vertex en mémoire par le biais de l’étape de sortie de flux. La sortie transmise à la mémoire est développée sous forme de listes de points/lignes/triangles (exactement comme si ces dernières étaient transmises au rastériseur).

Un nuanceur de géométrie produit en sortie les données d’un seul vertex à la fois en ajoutant des vertex à un objet de flux de sortie. La topologie des flux est déterminée par une déclaration fixe, choisissant un élément **TriangleStream**, **LineStream** et **PointStream** en tant que sortie pour l’étape GS.

Il existe trois types d’objets de flux disponibles : **TriangleStream**, **LineStream** et **PointStream**, qui sont tous basés sur des modèles objets. La topologie de la sortie est déterminée par leurs types d’objets respectifs, tandis que le format des vertex ajoutés au flux est déterminé par le type de modèle.

Quand une sortie du nuanceur de géométrie est identifié en tant que système interprété valeur (par exemple, **SV\_RenderTargetArrayIndex** ou **SV\_Position**), matériel examine ces données et effectue un comportement dépendant de la valeur, en plus de pouvoir transmettre les données elles-mêmes à l’étape suivante de nuanceur pour l’entrée. Lorsque ces données issues du nuanceur de géométrie a une signification pour le matériel sur une base par primitive (tel que **SV\_RenderTargetArrayIndex** ou **SV\_ViewportArrayIndex**), plutôt que sur une base par vertex (tel que **SV\_ClipDistance\[n\]**  ou **SV\_Position**), les données par primitive sont obtenue à partir du début vertex émis pour la primitive.

Si le nuanceur de géométrie prend fin et qu’une primitive est incomplète, le nuanceur peut générer une primitive partiellement complétée. Les primitives incomplètes sont ignorées sans avertissement. Ce comportement est comparable à la façon dont l’étape IA traite les primitives partiellement complétées.

Le nuanceur de géométrie peut exécuter des opérations de chargement et d’échantillonnage de texture lorsque des dérivés d’espace à l’écran ne sont pas requis (**samplelevel**, **samplecmplevelzero**, **samplegrad**).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Pipeline graphique](graphics-pipeline.md)

 

 




