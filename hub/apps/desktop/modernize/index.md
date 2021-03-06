---
Description: Ajoutez des interfaces utilisateur XAML modernes, créez des packages MSIX et incorporez d’autres composants modernes dans votre application de bureau.
title: Moderniser vos applications de bureau pour Windows
ms.topic: article
ms.date: 04/17/2019
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: c871149f764024de41309f21dbd036848633a2af
ms.sourcegitcommit: 720413d2053c8d5c5b34d6873740be6e913a4857
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88846729"
---
# <a name="modernize-your-desktop-apps"></a>Moderniser vos applications de bureau

Windows 10 et la plateforme universelle Windows (UWP) comportent de nombreuses fonctionnalités qui vous permettent de moderniser l’expérience utilisateur dans vos applications de bureau. La plupart de ces fonctionnalités sont disponibles sous forme de composants modulaires que vous pouvez incorporer dans vos applications de bureau, à votre rythme, sans avoir à réécrire le code des applications pour une autre plateforme. Enrichissez vos applications de bureau existantes avec les composants Windows 10 et UWP de votre choix.

Cet article décrit les fonctionnalités Windows 10 et UWP que vous pouvez utiliser dans vos applications de bureau dès aujourd’hui. Pour un didacticiel qui montre comment moderniser une application existante pour utiliser un grand nombre des fonctionnalités de cet article, consultez le didacticiel [Moderniser une application WPF](modernize-wpf-tutorial.md).

> [!NOTE]
> Avez-vous besoin d’aide pour migrer vos applications de bureau vers Windows 10 ? Le service [Soutien aux applications du bureau](https://docs.microsoft.com/FastTrack/win-10-desktop-app-assure) fournit gratuitement un support direct aux développeurs souhaitant migrer leurs applications vers Windows 10. Ce programme est mis à la disposition de tous les ISV et toutes les entreprises éligibles. Pour plus d’informations sur les critères d’éligibilité et sur le programme lui-même, consultez [https://docs.microsoft.com/fasttrack/win-10-app-assure-assistance-offered](https://docs.microsoft.com/fasttrack/win-10-app-assure-assistance-offered). Pour démarrer maintenant, [envoyez votre demande](https://fasttrack.microsoft.com/dl/daa).

## <a name="windows-ui-library"></a>Bibliothèque d’IU Windows

La bibliothèque d’interface utilisateur Windows est un ensemble de packages NuGet qui fournissent des contrôles et autres éléments d’interface utilisateur pour les applications Windows 10. WinUI était initialement une boîte à outils qui offrait des versions nouvelles et mises à jour des contrôles UWP pour les applications UWP ciblant des versions de bas niveau de Windows 10. WinUI s’est depuis étendue et est devenue la plateforme d’interface utilisateur native moderne pour les applications Windows 10 sur UWP, .NET et Win32.

Vous pouvez utiliser WinUI des manières suivantes dans les applications de bureau :

* Vous pouvez mettre à jour des applications WPF, Windows Forms et C++/Win32 existantes pour utiliser [XAML Islands](xaml-islands.md) afin d’héberger des contrôles WinUI 2.x dans les applications.
* À compter de [WinUi 3.0 Preview 1](../../winui/winui3/index.md), vous pouvez créer des [applications .NET et C++/Win32 qui utilisent une interface utilisateur entièrement basée sur WinUI](../../winui/winui3/get-started-winui3-for-desktop.md).

Consultez [Bibliothèque d’interface utilisateur Windows (WinUI)](../../winui/index.md).

## <a name="msix-packages"></a>Packages MSIX

MSIX est un format de package d’application Windows moderne qui permet de créer des packages universels pour toutes les applications Windows, notamment les applications UWP, WPF, Windows Forms et Win32. MSIX réunit les meilleurs aspects des technologies d’installation MSI, .appx, App-V et ClickOnce pour offrir une expérience d’empaquetage moderne et fiable.

En empaquetant vos applications de bureau Windows dans des packages MSIX, vous avez accès à une expérience d’installation et de mise à jour fiable, à un modèle de sécurité managé avec un système de capacité flexible, à un support pour le Microsoft Store, à la gestion d’entreprise et à de nombreux modèles de distribution personnalisés.

Pour plus d’informations, consultez [Empaqueter des applications de bureau](/windows/msix/desktop/desktop-to-uwp-root) dans la documentation MSIX.

## <a name="net-core-3"></a>.NET Core 3

.NET Core 3 est la dernière version majeure de .NET Core. La grande nouveauté de cette version est la prise en charge des applications de bureau Windows, notamment les applications Windows Forms et WPF. Vous pouvez exécuter vos applications de bureau Windows, nouvelles et existantes, sur .NET Core 3 et tirer pleinement parti de cette nouvelle version de .NET Core. De plus, vous pourrez utiliser les contrôles UWP hébergés dans [XAML Islands](xaml-islands.md) dans toutes les applications Windows Forms et WPF qui ciblent .NET Core 3.

Pour plus d’informations, consultez [Nouveautés de .NET Core 3.0](https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0).

## <a name="windows-runtime-apis"></a>API Windows Runtime

Vous pouvez appeler de nombreuses API Windows Runtime directement dans votre application de bureau WPF, Windows Forms ou Win32 C++ afin d’apporter aux utilisateurs de Windows 10 des expériences modernes. Par exemple, vous pouvez appeler des API Windows Runtime pour ajouter des notifications toast à votre application de bureau.

Pour plus d’informations, consultez [Utiliser des API Windows Runtime dans les applications de bureau](desktop-to-uwp-enhance.md).

## <a name="host-uwp-controls-xaml-islands"></a>Héberger des contrôles UWP (XAML Islands)

À partir de Windows 10, version 1903, vous pouvez ajouter des [contrôles XAML UWP](/windows/uwp/design/controls-and-patterns/controls-by-function) directement dans n’importe quel élément d’interface utilisateur d’une application WPF, Windows Forms ou Win32 C++ qui est associé à un handle de fenêtre (HWND). Cela vous permet d’intégrer entièrement les dernières fonctionnalités UWP comme [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions) et les contrôles qui prennent en charge le [système Fluent Design](/windows/uwp/design/fluent-design-system/index) dans les fenêtres et autres aires d’affichage dans vos applications de bureau. Ce scénario de développement est parfois appelé *XAML Islands*.

Pour plus d’informations, consultez [Contrôles UWP dans les applications de bureau](xaml-islands.md).

## <a name="use-the-visual-layer-in-desktop-apps"></a>Utiliser la couche Visuel dans les applications de bureau

Vous pouvez maintenant utiliser des API Windows Runtime dans des applications de bureau non conçues pour UWP. Ces API vous permettent d’améliorer l’apparence, le comportement et les fonctionnalités de vos applications WPF, Windows Forms et Win32 C++, mais aussi de bénéficier des toutes dernières fonctionnalités d’interface utilisateur de Windows 10 qui sont disponibles uniquement par le biais d’UWP. C’est utile pour créer des expériences utilisateur personnalisées plus avancées que les contrôles UWP intégrés que vous pouvez héberger en utilisant XAML Islands.

Pour plus d’informations, consultez [Moderniser votre application de bureau à l’aide de la couche Visuel](visual-layer-in-desktop-apps.md).

## <a name="additional-features-available-to-apps-with-package-identity"></a>Fonctionnalités supplémentaires disponibles pour les applications avec une identité de package

Certaines expériences Windows 10 modernes sont uniquement offertes dans les applications de bureau qui ont une [identité de package](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity). Ces fonctionnalités incluent certaines API Windows Runtime, extensions de package et composants UWP. Pour plus d’informations, consultez [Fonctionnalités nécessitant l’identité du package](modernize-packaged-apps.md).

Il existe plusieurs façons d’accorder une identité à une application de bureau :

* Empaquetez-la dans un package [MSIX](/windows/msix/desktop/desktop-to-uwp-root). MSIX est un format de package d’application moderne qui permet de créer des packages universels pour toutes les applications Windows, WPF, Windows Forms et Win32. Il offre une expérience d’installation et de mise à jour fiable, un modèle de sécurité managé avec un système de capacité flexible, une prise en charge du Microsoft Store, une gestion d’entreprise et de nombreux modèles de distribution personnalisés. Pour plus d’informations, consultez [Empaqueter des applications de bureau](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root) dans la documentation MSIX.
* Si vous ne parvenez pas à adopter l’empaquetage MSIX pour le déploiement de votre application de bureau, à compter de Windows 10 version 2004, vous pouvez accorder une identité de package en créant un *package MSIX partiel* contenant uniquement un manifeste de package. Pour plus d’informations, consultez [Accorder une identité à des applications de bureau non empaquetées](grant-identity-to-nonpackaged-apps.md).

<a id="desktop-uwp-controls"></a>

## <a name="uwp-controls-optimized-for-desktop-apps"></a>Contrôles UWP optimisés pour les applications de bureau

Que vous développiez une application UWP ciblant exclusivement la famille d’appareils de bureau ou que vous souhaitiez utiliser des contrôles UWP dans vos applications de bureau WPF, Windows Forms ou Win32 C++, les contrôles UWP nouveaux et mis à jour suivants sont conçus pour offrir des expériences de bureau optimisées avec le système [Fluent Design](/windows/uwp/design/fluent-design-system/index). Ces contrôles ont été introduits dans Windows 10, version 1809 (mise à jour d’octobre 2018 ou version 10.0.17763).

| Contrôler |  Description |
|------ |--------------|
| [MenuBar](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/menus#create-a-menu-bar) | Fournit un moyen rapide et simple d’exposer un ensemble de commandes dans les applications nécessitant un niveau d’organisation et de regroupement plus avancé que celui fourni par **CommandBar**. |
| [DropDownButton](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/buttons#create-a-drop-down-button) | Affiche une flèche permettant de développer un menu volant attaché qui contient des options supplémentaires.  |
| [SplitButton](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/buttons#create-a-split-button) | Fournit un bouton à deux composants, qui peuvent être appelés séparément. Un composant se comporte comme un bouton standard et appelle une action immédiate. L’autre composant appelle un menu volant qui propose des options supplémentaires à l’utilisateur.|
| [ToggleSplitButton](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/buttons#create-a-toggle-split-button) | Fournit un bouton à deux composants, qui peuvent être appelés séparément. Un composant se comporte comme un bouton bascule qui peut être activé ou désactivé. L’autre composant appelle un menu volant qui propose des options supplémentaires à l’utilisateur. |
| [CommandBarFlyout](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/command-bar-flyout) |  Vous permet d’afficher les tâches utilisateur courantes dans le contexte d’un élément sur la zone de dessin de l’interface utilisateur. |
| [ComboBox](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/combo-box#make-a-combo-box-editable) | Vous pouvez maintenant utiliser une zone de liste modifiable qui permet à l’utilisateur d’entrer d’autres valeurs que celles proposées dans le contrôle.  |
| [TreeView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/tree-view) | Vous pouvez désormais configurer une arborescence pour activer la liaison de données, les modèles d’élément et la fonction glisser-déplacer.  |
| [DataGridView](https://docs.microsoft.com/windows/communitytoolkit/controls/datagrid) |   Offre un moyen flexible d’afficher une collection de données en lignes et en colonnes. Ce contrôle est disponible dans le kit de ressources [Windows Community Toolkit](https://docs.microsoft.com/windows/uwpcommunitytoolkit/).  |

## <a name="other-technologies-for-modern-desktop-apps"></a>Autres technologies pour les applications de bureau modernes

### <a name="microsoft-graph"></a>Microsoft Graph

Microsoft Graph est une collection d’API avec lesquelles vous pouvez créer des applications destinées aux organisations ou aux consommateurs qui interagissent avec les données de millions d’utilisateurs. Microsoft Graph expose des API REST et des bibliothèques clientes pour accéder aux données des sources suivantes :
* Azure Active Directory
* Applications Microsoft 365 Office : SharePoint, OneDrive, Outlook/Exchange, Microsoft Teams, OneNote, Planner et Excel
* Services Enterprise Mobility + Security : Identity Manager, Intune, Advanced Threat Analytics et Advanced Threat Protection.
* Services Windows 10 : appareils et activités

Pour plus d’informations, consultez la [documentation Microsoft Graph](https://developer.microsoft.com/graph/docs/concepts/overview).

### <a name="adaptive-cards"></a>Cartes adaptatives

Cartes adaptatives est un framework ouvert multiplateforme qui vous permet d’échanger du contenu d’interface utilisateur au format carte de façon commune et cohérente sur l’ensemble des appareils et des plateformes.

Pour plus d’informations, consultez la [documentation Cartes adaptatives](https://docs.microsoft.com/adaptive-cards/).
