---
ms.assetid: ba2ac5f5-1e0d-4f1d-a6f8-6a65b4cff501
description: Cette section explique comment effectuer le portage de votre application sur la plateforme UWP (plateforme Windows universelle), qui permet de créer un seul package d’application Windows 10 que vos clients peuvent installer sur tous les types d’appareil. Votre application tirera ainsi profit d’un nouveau matériel formidable, de remarquables opportunités de monétisation, d’un ensemble d’API modernes, de contrôles d’interface utilisateur adaptatifs et de multiples modalités d’entrée, incluant la souris, le clavier, les fonctions tactiles et vocales.
title: Portage d’applications sur Windows 10
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 937d165d9305a3f4909383e872f49fcf08a3115c
ms.sourcegitcommit: f727b68e86a86c94eff00f67ed79a1c12666e7bc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/29/2020
ms.locfileid: "66371588"
---
# <a name="porting-apps-to-windows10"></a>Portage d’applications sur Windows 10


Cette section explique comment effectuer le portage de votre application sur la plateforme UWP (plateforme Windows universelle), qui permet de créer un seul package d’application Windows 10 que vos clients peuvent installer sur tous les types d’appareil. Votre application tirera ainsi profit d’un nouveau matériel formidable, de remarquables opportunités de monétisation, d’un ensemble d’API modernes, de contrôles d’interface utilisateur adaptatifs et de multiples modalités d’entrée, incluant la souris, le clavier, les fonctions tactiles et vocales.

Windows Runtime (WinRT) est la technologie qui vous permet de générer des applications pour la plateforme Windows universelle (UWP). Pour plus d’informations sur WinRT et les applications pour UWP, voir [Qu’est-ce qu’une application pour la plateforme Windows Universelle (UWP) ?](https://docs.microsoft.com/windows/uwp/get-started/whats-a-uwp).

Ce guide relatif au portage explique les différences entre la technologie de votre application actuelle et celle de la plateforme Windows universelle (UWP). Une fois que vous aurez compris comment effectuer la transition entre ces deux technologies, vous pourrez vous plonger dans le reste du Centre de développement, qui constitue une ressource complète pour le développement d’applications UWP. Lorsque vous serez prêt, l’idéal est de commencer par lire la rubrique [Comment développer une application du Windows Store](https://docs.microsoft.com/previous-versions/windows/apps/dn726537(v=win.10)).

| Rubrique | Description |
|-------|-------------|
| [Passer des applications de bureau aux applications UWP](desktop-to-uwp-migrate.md) | Choisissez l’une des options disponibles pour apporter des fonctionnalités UWP à vos applications de bureau .NET et Win32. |
| [Passer de Windows Runtime 8.x à UWP](w8x-to-uwp-root.md) | Si vous disposez d’une application 8.1 universelle (qu’elle cible Windows 8.1, Windows Phone 8.1 ou les deux), vous constaterez que le portage de votre code source et de vos compétences vers Windows 10 s’effectue en toute transparence. Grâce à Windows 10, vous pouvez créer une application UWP, à savoir un package d’application unique que vos clients peuvent installer sur tout type d’appareil. |
| [Mappage de concepts d’applications Windows pour développeurs Android et iOS](android-ios-uwp-map.md) | Si vous êtes un développeur doté de compétences relatives aux systèmes d’exploitation Android ou iOS ou au code et que vous souhaitez migrer vers Windows 10 et la plateforme Windows universelle, cette ressource vous permettra de mapper les fonctionnalités de la plateforme, et vos connaissances, entre les trois plateformes. |
| [Passer d’iOS à UWP](ios-to-uwp-root.md) | Êtes-vous un développeur iOS vous demandant comment faire pour passer à Windows 10 et UWP ? Cette opération n’est pas aussi effrayante que vous l’imaginez. Nous avons les outils, les techniques et les informations nécessaires pour créer des applications fantastiques qui fonctionnent aussi bien sur Windows que sur vos appareils iOS, et même peut-être mieux ! |
| [Migrer de Silverlight pour Windows Phone vers UWP](wpsl-to-uwp-root.md) | Si vous êtes un développeur disposant d’une application Silverlight pour Windows Phone, vous pouvez tirer parti de vos compétences et de votre code source pour le passage à Windows 10. Grâce à Windows 10, vous pouvez créer une application UWP, à savoir un package d’application unique que vos clients peuvent installer sur tout type d’appareil. |
| [Convertir votre application web en PWA](https://docs.microsoft.com/microsoft-edge/progressive-web-apps) | Vous pouvez désormais convertir votre application web en PWA (application web progressive) qui fonctionnera sur toutes les plateformes, notamment la plateforme UWP ! L’[Outil PWA Builder](https://www.pwabuilder.com) génère le manifeste nécessaire à votre place. Ceci remplace le pont HWA (Hosted Web Apps). |

## <a name="related-topics"></a>Rubriques connexes

* [Passer de WPF et Silverlight à WinRT](https://docs.microsoft.com/previous-versions/windows/apps/dn263237(v=win.10))
* [Passer d’Android à WinRT](https://docs.microsoft.com/previous-versions/windows/apps/jj945421(v=win.10))
* [Passer du web à WinRT](https://docs.microsoft.com/previous-versions/windows/apps/hh465151(v=win.10))
