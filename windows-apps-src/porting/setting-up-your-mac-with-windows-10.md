---
description: Utilisez votre ordinateur Mac actuel pour développer des applications pour Windows.
title: Configuration de votre Mac avec Windows 10
ms.assetid: 6D520610-5DE0-476E-A792-AA57E002D309
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b8ff6d6b3b15992f0598bfb371e6bfb1023d6dc1
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75685003"
---
# <a name="setting-up-your-mac-with-windows-10"></a>Configuration de votre Mac avec Windows 10


Utilisez votre ordinateur Mac actuel pour développer des applications pour Windows.

## <a name="run-windows-on-your-mac-and-use-visual-studio"></a>Exécuter Windows sur votre Mac et utiliser Visual Studio

Vous êtes prêt à développer des applications Windows universelles, mais vous n'avez pas de PC à portée de main ? Ce n’est pas un problème, vous pouvez utiliser votre Mac ! Avec des solutions tierces populaires comme Apple Boot Camp, Oracle VirtualBox, VMware Fusion et des ordinateurs parallèles, vous pouvez installer Windows 10 et Microsoft Visual Studio sur votre ordinateur Apple.

**Notez**  vous aurez besoin d’une image de démarrage Windows 10 sur disque ou un lecteur flash USB. Si vous êtes un abonné MSDN, vous pouvez télécharger l’image d’installation à partir du centre Téléchargements des abonnés MSDN. Si vous n’êtes pas abonné, le programme d’installation peut être acheté à partir de la [Microsoft Store](https://www.microsoft.com/store/apps). Vous pouvez également le télécharger à partir de [cet emplacement](https://www.microsoft.com/software-download/windows10) ; très utile si vous exécutez déjà Windows et que vous souhaitez effectuer une mise à jour.

Une fois que Windows est en cours d’exécution, vous pouvez installer la dernière version de Visual Studio à partir des [téléchargements pour Windows 10](https://developer.microsoft.com/windows/downloads) et commencer à écrire des applications.

**Remarque**  si vous envisagez d’utiliser les émulateurs de périphérique Visual Studio, vous **devez** installer une version 64 bits (x64) de Windows 10 professionnel ou une version ultérieure. Malheureusement, certains anciens modèles de Mac ne peuvent pas exécuter Windows en version 64 bits. Vérifiez auprès d’Apple si votre matériel est compatible sur cette[page de support Apple](https://support.apple.com/kb/HT5634).

## <a name="apple-boot-camp"></a>Apple Boot Camp

L’application Boot Camp Assistant est préinstallée sur chaque Mac récent et son lancement vous guide tout au long du processus d’installation de Windows 10. Tout ce dont vous avez besoin est une copie de Windows (obtenue à partir des sources énumérées ci-dessus) et au moins 30 Go d’espace disque libre. Une fois l’installation effectuée, vous pouvez choisir de démarrer dans Mac OSX ou dans Windows 10. Pour plus d'informations, voir la [page d’instructions Boot Camp](https://support.apple.com/HT201468) d'Apple.

## <a name="parallels-desktop"></a>Parallels Desktop

Avec Parallels Desktop 11, vous pouvez exécuter des applications Windows côte à côte avec les applications Mac existantes, y compris Visual Studio et Cortana. Une version pro est disponible et inclut des fonctionnalités supplémentaires pour les développeurs, notamment le débogage amélioré et la prise en charge de Docker et Jenkins. Pour plus d'informations et une version d'évaluation gratuite, voir [Parallels Desktop](https://www.parallels.com/download/desktop/).

## <a name="vmware-fusion"></a>VMWare Fusion

Fusion 8 de VMware vous permet d'exécuter Visual Studio directement sur votre bureau Mac. Une version pro est disponible pour offrir aux développeurs certaines fonctionnalités plus avancées, telles que la prise en charge de vSphere. Pour plus d'informations et une version d'évaluation gratuite, voir [VMware Fusion](http://www.vmware.com/products/fusion/).

## <a name="oracle-virtualbox"></a>Oracle VirtualBox

VirtualBox est une application gratuite qui permet d’exécuter des ordinateurs virtuels sur votre ordinateur. Elle prend en charge l’exécution de Windows sur Mac. C'est une option sans superflu, mais le prix est attrayant. Pour plus d’informations, voir [VirtualBox](https://www.virtualbox.org/wiki/Downloads).

