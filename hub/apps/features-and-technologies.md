---
Description: Cette section permet de comprendre de quelle manière les fonctionnalités Windows sont prises en charge sur les différentes plateformes d’applications et comment commencer à utiliser ces fonctionnalités dans votre code.
title: Fonctionnalités et technologies
ms.topic: article
ms.date: 05/08/2019
ms.localizationpriority: medium
ms.author: mcleans
author: mcleanbyron
ms.openlocfilehash: c214a31162b64853fd2115170ef1fd7766cafe96
ms.sourcegitcommit: e1104689fc1db5afb85701205c2580663522ee6d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997946"
---
# <a name="features-and-technologies-for-windows-apps"></a>Fonctionnalités et technologies des applications Windows

Quel que soit le type d’application que vous créez ou le périphérique que vous ciblez, Windows prend en charge de nombreuses fonctionnalités qui constituent des blocs de construction clés pour les scénarios d’application importants. Certaines de ces fonctionnalités sont exposées à la plateforme Windows universelle (UWP), Win32 (API Windows) et à d’autres plateformes d’application de différentes manières. Les articles suivants vous aident à comprendre comment certaines fonctionnalités de Windows sont prises en charge dans différentes plateformes d’application et comment commencer à utiliser les fonctionnalités de votre code.

Cet article fournit une liste personnalisée d’articles pour en savoir plus sur la façon dont vous pouvez accéder aux fonctionnalités et technologies Windows importantes sur les plateformes d’applications UWP, Win32 (API Windows), WPF et Windows Forms. Pour obtenir des informations complètes sur les fonctionnalités de développement de chaque plateforme, consultez les ressources suivantes :

* [Documentation UWP](/windows/uwp/index)
* [Documentation Win32 (API Windows)](/windows/desktop/index)
* [Documentation WPF](https://docs.microsoft.com/dotnet/framework/wpf/index)
* [Documentation Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/index)

## <a name="key-windows-features-and-technologies"></a>Principales fonctionnalités et technologies Windows

Les sections suivantes mettent en évidence plusieurs fonctionnalités et technologies importantes de Windows qui permettent de proposer des expériences modernes et attrayantes aux clients.

### <a name="windows-ink"></a>Windows Ink

![Stylet Surface](images/hero-small.png)  

La plateforme Windows Ink, associée à un stylet, permet de créer des notes manuscrites, des dessins et des annotations plus naturellement. La plateforme prend en charge la capture d’entrée du numériseur sous forme de données d’entrée manuscrite, la génération et la gestion de données d’entrée manuscrite, la restitution de ces données sous forme de traits et la conversion de l’encre en texte via la reconnaissance d’écriture manuscrite.

Pour plus d’informations sur les différentes façons d’utiliser Windows Ink dans les applications Windows, consultez [Windows Ink.](/windows/uwp/design/input/pen-and-stylus-interactions)

### <a name="speech-interactions"></a>Interactions vocales

![Écran initial de la reconnaissance vocale correspondant à une contrainte basée sur un fichier de grammaire SGRS](images/speech-listening-initial.png)

![Écran final de la reconnaissance vocale correspondant à une contrainte basée sur un fichier de grammaire SGRS](images/speech-listening-complete.png)

Windows propose différents moyens d’intégrer la reconnaissance vocale et la conversion de texte par synthèse vocale (également appelée TTS ou synthèse vocale) directement dans l’expérience utilisateur de votre application. Les fonctions vocales peuvent permettre aux utilisateurs d’interagir avec votre application de manière fiable et agréable. Elles peuvent aussi compléter, voire remplacer le clavier, la souris, l’écran tactile et les mouvements.

Pour plus d’informations sur les différentes façons d’utiliser les interactions vocales dans les applications Windows, consultez [Parole, voix et conversation dans Windows 10](speech.md).

### <a name="windows-ai"></a>Windows IA

![Windows IA](images/windows-ai.png)

Nous proposons plusieurs solutions IA différentes que vous pouvez utiliser pour améliorer vos applications Windows. Avec Windows Machine Learning, vous pouvez intégrer des modèles de Machine Learning entraînés à vos applications et les exécuter localement sur l’appareil. Windows Vision Skills permet d’utiliser des bibliothèques prédéfinies pour effectuer des tâches de traitement d’image courantes ou de créer vos propres solutions personnalisées. DirectML fournit des API de type DirectX de bas niveau qui vous permettent de tirer pleinement parti du matériel.

Pour plus d’informations sur les différentes façons d’utiliser Windows l’IA dans les applications Windows, consultez [IA Windows.](https://docs.microsoft.com/windows/ai/)

## <a name="features-and-technologies-by-platform"></a>Fonctionnalités et technologies par plateforme

Les sections suivantes fournissent des liens utiles pour en savoir plus sur l’intégration avec les fonctionnalités et technologies principales de Windows à partir de nos principales plateformes d’applications : UWP, Win32 (API Windows), WPF et Windows Forms.

### <a name="user-interface-and-accessibility"></a>Interface utilisateur et accessibilité

|  UWP  |  Win32 (API Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Concevez](/windows/uwp/design/basics/)<br/><br/>[Disposition](/windows/uwp/design/layout/)<br/><br/>[Contrôles](/windows/uwp/design/controls-and-patterns/)<br/><br/>[Entrée](/windows/uwp/design/input/)<br/><br/>[Vignettes](/windows/uwp/design/shell/tiles-and-notifications/creating-tiles)<br/><br/>[Couche visuelle](/windows/uwp/composition/visual-layer)<br/><br/>[Plateforme XAML](/windows/uwp/xaml-platform/)<br/><br/>[Lancement, reprise et tâches en arrière-plan](/windows/uwp/launch-resume/)<br/><br/>[Accessibilité de Windows](/windows/uwp/design/accessibility/accessibility)<br/><br/>[Interactions vocales](https://docs.microsoft.com/windows/uwp/design/input/speech-interactions)<br/><br/> |  [Interface utilisateur de bureau](/windows/desktop/windows-application-ui-development)<br/><br/>[Environnement et shell de bureau](/windows/desktop/user-interface)<br/><br/>[Contrôles Windows](/windows/desktop/controls/window-controls)<br/><br/>[Contrôles UWP dans les applications de bureau (XAML Islands)](/windows/apps/desktop/modernize/xaml-islands)<br/><br/>[Couche visuelle dans les applications de bureau](/windows/apps/desktop/modernize/visual-layer-in-desktop-apps)<br/><br/>[Windows et messages](/windows/desktop/winmsg/windowing)<br/><br/>[Menus et autres ressources](/windows/desktop/menurc/resources)<br/><br/>[Haute résolution](/windows/desktop/hidpi/high-dpi-desktop-application-development-on-windows)<br/><br/>[Accessibilité](/windows/desktop/accessibility)<br/><br/>[Microsoft Speech Platform - SDK (version 11)](https://www.microsoft.com/download/details.aspx?id=27226)<br/><br/>[SDK Microsoft Speech version 5.1](https://www.microsoft.com/download/details.aspx?id=10121)<br/><br/>  |  [Intégration du format Windows au format WPF](https://docs.microsoft.com/dotnet/framework/wpf/app-development/windows-in-wpf-applications)<br/><br/>[Vue d’ensemble de la navigation](https://docs.microsoft.com/dotnet/framework/wpf/app-development/navigation-overview)<br/><br/>[Intégration du format XAML au format WPF](https://docs.microsoft.com/dotnet/framework/wpf/advanced/xaml-in-wpf)<br/><br/>[Contrôles](https://docs.microsoft.com/dotnet/framework/wpf/controls/)<br/><br/>[Programmation de la couche visuelle](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/visual-layer-programming)<br/><br/>[Entrée](https://docs.microsoft.com/dotnet/framework/wpf/advanced/input-wpf)<br/><br/>[Accessibilité](https://docs.microsoft.com/dotnet/framework/ui-automation/)<br/><br/>[Guide de programmation System.Speech pour le .NET Framework](https://docs.microsoft.com/previous-versions/office/developer/speech-technologies/hh361625(v=office.14))<br/><br/>  | [Créer un formulaire Windows](https://docs.microsoft.com/dotnet/framework/winforms/creating-a-new-windows-form)<br/><br/>[Contrôles](https://docs.microsoft.com/dotnet/framework/winforms/controls/)<br/><br/>[Boîtes de dialogue](https://docs.microsoft.com/dotnet/framework/winforms/dialog-boxes-in-windows-forms)<br/><br/>[Entrée de l’utilisateur](https://docs.microsoft.com/dotnet/framework/winforms/user-input-in-windows-forms)<br/><br/>[Accessibilité de Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/advanced/windows-forms-accessibility)<br/><br/>[Guide de programmation System.Speech pour le .NET Framework](https://docs.microsoft.com/previous-versions/office/developer/speech-technologies/hh361625(v=office.14))<br/><br/> |

### <a name="audio-video-and-graphics"></a>Audio, vidéo et graphismes

|  UWP  |  Win32 (API Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Audio, vidéo et appareil photo](/windows/uwp/audio-video-camera/)<br/><br/>[Lecture de contenu multimédia](/windows/uwp/audio-video-camera/media-playback/)<br/><br/>[Couche visuelle](/windows/uwp/composition/visual-layer)<br/><br/>[Plateforme XAML](/windows/uwp/xaml-platform/) |  [Audio et vidéo](/windows/desktop/audio-and-video)<br/><br/>[Graphismes et jeux ](/windows/desktop/graphics-and-multimedia)<br/><br/>[DirectX](/windows/desktop/getting-started-with-directx-graphics)<br/><br/>[Direct2D](/windows/desktop/direct2d/direct2d-portal)<br/><br/>[Direct3D](/windows/desktop/direct3d)<br/><br/>[Windows GDI](/windows/desktop/gdi/windows-gdi)<br/><br/>[GDI+](/windows/desktop/gdiplus/-gdiplus-gdi-start)  |  [Graphismes](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/graphics)<br/><br/>[Mutimédia](https://docs.microsoft.com/dotnet/framework/wpf/graphics-multimedia/multimedia-overview)  |  [Graphismes et dessin](https://docs.microsoft.com/dotnet/framework/winforms/advanced/graphics-and-drawing-in-windows-forms)<br/><br/>[SoundPlayer, classe](https://docs.microsoft.com/dotnet/framework/winforms/controls/soundplayer-class-overview)  |

### <a name="data-access-and-app-resources"></a>Accès aux données et ressources de l’application

|  UWP  |  Win32 (API Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Accès aux données](/windows/uwp/data-access/)<br/><br/>[Liaison de données](/windows/uwp/data-binding/)<br/><br/>[Fichiers, dossiers et bibliothèques](/windows/uwp/files/)<br/><br/>[Ressources de l’application](/windows/uwp/app-resources/) |  [Accès aux données et stockage](/windows/desktop/data-access-and-storage)<br/><br/>[Systèmes de fichiers locaux](/windows/desktop/fileio/file-systems)<br/><br/>[Vues d’ensemble des ressources](/windows/desktop/menurc/resources-overviews)</li>  |  [Données et modélisation](https://docs.microsoft.com/dotnet/framework/data/)<br/><br/>[Liaison de données](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-wpf)<br/><br/>[Ressources dans les applications .NET](https://docs.microsoft.com/dotnet/framework/resources/)<br/><br/>[Ressource, contenu et fichiers de données d’une application](https://docs.microsoft.com/dotnet/framework/wpf/app-development/wpf-application-resource-content-and-data-files)  |  [Données et modélisation](https://docs.microsoft.com/dotnet/framework/data/)<br/><br/>[Liaison de données](https://docs.microsoft.com/dotnet/framework/winforms/windows-forms-data-binding)<br/><br/>[Ressources dans les applications .NET](https://docs.microsoft.com/dotnet/framework/resources/)<br/><br/>[Paramètres d’application](https://docs.microsoft.com/dotnet/framework/winforms/advanced/application-settings-for-windows-forms)  |

### <a name="devices-documents-and-printing"></a>Appareils, documents et impression

|  UWP  |  Win32 (API Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Activer les fonctionnalités d’un appareil](/windows/uwp/devices-sensors/enable-device-capabilities)<br/><br/>[Énumérer les appareils](/windows/uwp/devices-sensors/enumerate-devices)<br/><br/>[Détecteurs](/windows/uwp/devices-sensors/sensors)<br/><br/>[Bluetooth](/windows/uwp/devices-sensors/bluetooth)<br/><br/>[Impression et numérisation](/windows/uwp/devices-sensors/printing-and-scanning)<br/><br/>[NFC](/windows/uwp/devices-sensors/nfc) | [API de capteur](/windows/desktop/sensorsapi/portal)<br/><br/>[Impression](/desktop/printdocs/printdocs-printing)<br/><br/>[API UPnP](/desktop/upnp/universal-plug-and-play-start-page) |  [Impression et gestion du système d’impression](https://docs.microsoft.com/dotnet/framework/wpf/advanced/printing-and-print-system-management)  |  [Prise en charge de l’impression](https://docs.microsoft.com/dotnet/framework/winforms/advanced/windows-forms-print-support)  |

### <a name="system-network-and-power"></a>Système, réseau et alimentation

|  UWP  |  Win32 (API Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Énumérer les appareils](/windows/uwp/devices-sensors/enumerate-devices)<br/><br/>[Obtenir des informations sur la batterie](/windows/uwp/devices-sensors/get-battery-info)<br/><br/>[Threads et programmation asynchrone](/windows/uwp/threading-async/)<br/><br/>[Mise en réseau et services web](/windows/uwp/networking/) | [Services système](/windows/desktop/system-services)<br/><br/>[Gestion de la mémoire](/windows/desktop/memory/memory-management)<br/><br/>[Gestion de l’alimentation](/windows/desktop/power/power-management-portal)<br/><br/>[Processus et threads](/windows/desktop/procthread/processes-and-threads)<br/><br/>[Mise en réseau et Internet](/windows/desktop/networking)<br/><br/>[Informations système Windows](/windows/desktop/sysinfo/windows-system-information) |  [Modèle de thread](https://docs.microsoft.com/dotnet/framework/wpf/advanced/threading-model)<br/><br/>[Programmation réseau dans le .NET Framework](https://docs.microsoft.com/dotnet/framework/network-programming/)  |  [Informations système](https://docs.microsoft.com/dotnet/framework/winforms/advanced/system-information-and-windows-forms)<br/><br/>[Gestion de l’alimentation](https://docs.microsoft.com/dotnet/framework/winforms/advanced/power-management-in-windows-forms)<br/><br/>[Programmation réseau dans le .NET Framework](https://docs.microsoft.com/dotnet/framework/network-programming/)<br/><br/>[Fonction de mise en réseau des Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/advanced/networking-in-windows-forms-applications)  |

### <a name="debugging-and-performance"></a>Débogage et performances

|  UWP  |  Win32 (API Windows) |  WPF et Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Débogage, tests et analyse des performances](/windows/uwp/debug-test-perf)<br/><br/>[Déploiement et débogage des applications UWP](/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps)<br/><br/>[Kit de certification des applications Windows](/windows/uwp/debug-test-perf/windows-app-certification-kit)<br/><br/>[Performances](/windows/uwp/debug-test-perf/performance-and-xaml-ui)| [Débogage et gestion des erreurs](https://docs.microsoft.com/windows/win32/debugging-and-error-handling)<br/><br/>[Outils de débogage pour Windows](https://docs.microsoft.com/windows-hardware/drivers/debugger/)<br/><br/>[Suivi d’événements pour Windows (ETW)](https://docs.microsoft.com/windows/win32/etw/event-tracing-portal)<br/><br/>[API .NET TraceProcessing](/windows/apps/trace-processing/)<br/><br/>[TraceLogging](https://docs.microsoft.com/windows/win32/tracelogging/trace-logging-portal)<br/><br/>[Compteurs de performance](https://docs.microsoft.com/windows/win32/perfctrs/performance-counters-portal) |  [Débogage, traçage et profilage](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/)<br/><br/>[Traçage et instrumentation d’applications](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/tracing-and-instrumenting-applications)<br/><br/>[Diagnostiquer les erreurs avec les Assistants Débogage managé](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/diagnosing-errors-with-managed-debugging-assistants)<br/><br/>[Profilage de runtime](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/runtime-profiling)<br/><br/>[Compteurs de performance](https://docs.microsoft.com/dotnet/framework/debug-trace-profile/performance-counters)<br/><br/>[Déploiement ClickOnce pour les Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/clickonce-deployment-for-windows-forms)  |

### <a name="packaging-and-deployment"></a>Déploiement et packaging

|  UWP  |  Win32 (API Windows) |  WPF  |  Windows Forms  |
|-------|----------------------|-------|-----------------|
| [Empaquetage d’applications](/windows/uwp/packaging/)<br/><br/>[MSIX](https://docs.microsoft.com/windows/msix/)<br/><br/>[Schéma de manifeste du package de l’application](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root) | [Applications de bureau du package Windows (MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[Installation d’application et maintenance](/windows/desktop/application-installing-and-servicing)<br/><br/>[Windows Installer](/windows/desktop/msi/windows-installer-portal) |  [Applications de bureau du package Windows (MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[Déploiement d’applications et du .NET Framework](https://docs.microsoft.com/dotnet/framework/deployment/)<br/><br/>[Déploiement d’une application WPF](https://docs.microsoft.com/dotnet/framework/wpf/app-development/deploying-a-wpf-application-wpf)  |  [Applications de bureau du package Windows (MSIX)](/windows/msix/desktop/desktop-to-uwp-root)<br/><br/>[Déploiement d’applications et du .NET Framework](https://docs.microsoft.com/dotnet/framework/deployment/)<br/><br/>[Déploiement ClickOnce pour les Windows Forms](https://docs.microsoft.com/dotnet/framework/winforms/clickonce-deployment-for-windows-forms)  |
