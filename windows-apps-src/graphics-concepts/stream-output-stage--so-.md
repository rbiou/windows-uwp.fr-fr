---
title: Étape Stream Output
description: L'étape de sortie de flux (SO, Stream Output) sort (diffuse) en continu les données de vertex de l'étape active précédente et les stocke dans une ou plusieurs mémoires tampon en mémoire. Les données diffusées dans la mémoire peuvent être renvoyées dans le pipeline en tant que données d’entrée ou lues par le processeur.
ms.assetid: DE89E99F-39BC-4B34-B80F-A7D373AA7C0A
keywords:
- Étape Stream Output
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e3614b7bde3a87c8f5fa6fdc0eada560fd7bbcdc
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370962"
---
# <a name="stream-output-so-stage"></a>Étape Stream Output


L'étape de sortie de flux (SO, Stream Output) sort (diffuse) en continu les données de vertex de l'étape active précédente et les stocke dans une ou plusieurs mémoires tampon en mémoire. Les données diffusées dans la mémoire peuvent être renvoyées dans le pipeline en tant que données d’entrée ou lues par le processeur.

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Rôle et les utilisations


![diagramme de l’emplacement de l’étape de sortie de flux dans le pipeline](images/d3d10-pipeline-stages-so.png)

Cette étape diffuse des données primitives du pipeline dans la mémoire vers le rastériseur. Les données de l’étape précédente peuvent être diffusées dans la mémoire et/ou transférées vers le rastériseur. Les données diffusées dans la mémoire peuvent être renvoyées dans le pipeline en tant que données d’entrée ou lues par le processeur.

Les données diffusées dans la mémoire peuvent relues dans le pipeline dans le cadre d'une passe de rendu ultérieure, ou peuvent être copiées vers une ressource intermédiaire (afin qu’elles puissent être lues par le processeur). La quantité de données diffusées peut varier ; Direct3D est conçu pour gérer les données sans avoir besoin de requête (GPU) sur la quantité de données écrites.--&gt;

Il existe deux façons de transmettre les données de sortie de flux dans le pipeline :

-   Les données de sortie de flux de données peuvent être transmises lors de l'étape de l'assembleur d'entrée (IA).
-   Les données de sortie de flux peuvent être lues par les nuanceurs programmables à l’aide des fonctions [Charger](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-to-load).

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>entrée


Les données de vertex issues d'une étape précédente du nuanceur.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Sortie


L’étape de sortie de flux (SO) sort (diffuse) en continu les données de vertex de l’étape active précédente, telle que l’étape du nuanceur de géométrie (GS), et les stocke dans une ou plusieurs mémoires tampon en mémoire. Si l’étape de nuanceur de géométrie (GS) est inactif, l’étape de sortie Stream (ainsi) génère en permanence des données de vertex à partir de l’étape de nuanceur de domaine (DS) dans les mémoires tampons en mémoire (ou si DS est également inactif, à partir de la phase de Vertex Shader (VS)).

Lorsqu’un triangle ou une bande de ligne est associé(e) à l’étape d’assembleur d’entrée (IA), chaque bande est convertie en liste avant d'être diffusée. Les vertex sont toujours écrits sous forme de primitives complètes (par exemple, 3 vertex à la fois pour les triangles) ; les primitives incomplètes ne sont jamais diffusées. Les types de primitives avec voisinage ignorent les données de voisinage avant la diffusion en continu des données.

L’étape de sortie de flux peut prendre en charge simultanément jusqu'à 4 mémoires tampons.

-   Si vous diffusez des données en continu dans plusieurs mémoires tampons, chaque tampon peut capturer uniquement un seul élément (jusqu'à 4 composants) de données de vertex, avec un stride de données implicite égal à la largeur de l’élément dans chaque tampon (compatible avec la manière dont les tampons à seul élément peuvent être liés pour la saisie dans les étapes de nuanceur). En outre, si les mémoires tampons sont de différentes tailles, l'écriture s’arrête dès que l’un des tampons est plein.
-   Si vous diffusez des données en continu dans une seule mémoire tampon, le tampon peut capturer jusqu'à 64 composants scalaires de données par vertex (256 octets ou moins) ou la capacité du stride de vertex peut atteindre 2 048 octets.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Pipeline graphique](graphics-pipeline.md)

 

 




