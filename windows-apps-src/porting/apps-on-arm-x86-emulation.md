---
title: Fonctionnement du système x86 et de l'émulation ARM32 sur ARM
author: msatranjr
description: Une présentation de l'émulation des applications x86 sur ARM.
ms.author: misatran
ms.date: 02/15/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10s, toujours connecté, x86, émulation sur ARM
ms.localizationpriority: medium
ms.openlocfilehash: d8304bb49b1eeeea8b0aa17f7548197a360cbcf8
ms.sourcegitcommit: 11edca90aaf7856c762e68903483079d30ad3877
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2018
ms.locfileid: "1595123"
---
# <a name="how-x86-emulation-works-on-arm"></a>Fonctionnement de l'émulation x86 sur ARM
Pour les application x86, l'émulation enrichit l'écosystème des applications Win32 sur ARM. Ainsi, l'utilisateur profite d'une expérience sans pareil dans l'exécution d'une application Win32 x86 existante, sans que l'application ne souffre de modification. L'application n'a pas conscience d'être exécutée sur un PC ARM avec Windows, à moins qu'elle n'appelle les API spécifiques ([IsWoW64Process2](https://msdn.microsoft.com/en-us/library/windows/desktop/mt804318.aspx)).

La couche [WOW64](https://msdn.microsoft.com/en-us/library/windows/desktop/aa384249(v=vs.85).aspx) de Windows10 permet au code x86 de s'exécuter sur une version ARM64 de Windows10. L'émulation x86 fonctionne en compilant les blocs d'instructions x86 en instructions ARM64 avec des optimisations permettant d'améliorer la performance. Un service met en cache ces blocs de cde traduits afin de réduire le traitement de la traduction des instructions et de permette une optimisation lorsque le code s'exécute à nouveau. Les caches sont produits pour chaque module afin que d'autres applications soient en mesure de les utiliser lors de leur premier lancement. 

Pour plus d’informations sur ces technologies, visionnez la vidéo Channel9 [Windows10 sur ARM](https://channel9.msdn.com/Events/Build/2017/P4171) 