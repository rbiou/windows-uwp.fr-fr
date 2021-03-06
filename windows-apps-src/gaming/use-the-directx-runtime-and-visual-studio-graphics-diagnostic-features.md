---
title: Outils Graphics Diagnostics
description: Découvrez comment obtenir et utiliser les fonctionnalités de diagnostic de graphiques, notamment le débogage graphique, l’analyse des frames graphiques et l’utilisation du processeur graphique (GPU) dans Visual Studio.
ms.assetid: 629ea462-18ed-a333-07e9-cc87ea2dcd93
ms.date: 11/20/2019
ms.topic: article
keywords: Windows 10, uwp, jeux, graphiques, diagnostics, outils, directx
ms.localizationpriority: medium
ms.openlocfilehash: 14711a356f00ac027bf990554c90547017b9cc4d
ms.sourcegitcommit: f464e5157ca22cfe31075f2f1219b8227587bf03
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74263098"
---
# <a name="graphics-diagnostics-tools"></a>Outils Graphics Diagnostics

Les outils Graphics Diagnostics sont disponibles dans Windows 10 en tant que fonctionnalité facultative. Pour utiliser les fonctionnalités Graphics Diagnostics (fournies dans le runtime et Visual Studio) pour développer des applications ou des jeux DirectX, installez la fonctionnalité outils Graphics facultative.

1. Accédez à **paramètres** > **applications** > **applications & fonctionnalités/fonctionnalités facultatives**.
2. Si **Graphics Tools** est déjà listé sous **fonctionnalités installées**, cela signifie que vous avez terminé. Sinon, cliquez sur **Ajouter une fonctionnalité**.
3. Recherchez et/ou sélectionnez **Outils Graphics**, puis cliquez sur **installer**.

Les fonctionnalités de diagnostic de graphiques incluent la possibilité de créer des périphériques de débogage Direct3D (par le biais des couches SDK Direct3D) dans le runtime DirectX, ainsi que les fonctions de débogage graphique, d’analyse des frames graphiques et d’utilisation du GPU.

-   Le débogage graphique vous permet de tracer les appels Direct3D effectués par votre application. Vous pouvez ensuite réécouter ces appels, inspecter les paramètres, procéder à des opérations de débogage et à des expérimentations à l’aide de nuanceurs, et visualiser les ressources graphiques pour diagnostiquer les problèmes de rendu. Vous pouvez prendre des journaux sur des PC Windows, des simulateurs ou des appareils, puis les relire sur un autre matériel.
-   Analyse des frames graphiques dans Visual Studio s’exécute sur un journal de débogage graphique et collecte le minutage de base pour les appels de dessin Direct3D. Il effectue ensuite un ensemble d’expériences en modifiant différents paramètres graphiques et produit un tableau des résultats de minutage. Ces données peuvent vous aider à comprendre les problèmes de performances de rendu graphique dans votre application. Vous pouvez examiner les résultats de ces diverses expériences afin de trouver des moyens d’améliorer les performances.
-   La fonctionnalité d’utilisation du GPU dans Visual Studio vous permet de surveiller l’utilisation du GPU en temps réel. Il collecte et analyse les données de temporisation des charges de travail gérées par le processeur et le GPU, afin que vous puissiez déterminer où se trouvent les goulots d’étranglement.

## <a name="related-topics"></a>Rubriques connexes

[Vue d’ensemble de Graphics Diagnostics dans Visual Studio](/visualstudio/debugger/overview-of-visual-studio-graphics-diagnostics?view=vs-2015)