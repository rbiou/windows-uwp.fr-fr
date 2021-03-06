---
title: Applications adaptatives de version
description: Découvrez comment tirer parti des nouvelles API tout en conservant la compatibilité avec les versions précédentes
ms.date: 05/17/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b947d0b6cc83dc6bca45efb7103a933e79972e3b
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/29/2020
ms.locfileid: "67317452"
---
# <a name="version-adaptive-apps-use-new-apis-while-maintaining-compatibility-with-previous-versions"></a>Applications adaptatives de version : Utiliser les nouvelles API tout en préservant la compatibilité avec les versions précédentes

Chaque version du Kit de développement logiciel Windows 10 apporte d’incroyables fonctionnalités qui vous raviront. Toutefois, vos clients ne mettent pas tous en même temps leurs appareils à jour vers la dernière version de Windows 10, et vous voulez vous assurer que votre application fonctionne sur le plus large éventail possible d’appareils. Nous allons vous expliquer comment concevoir votre application afin qu’elle s’exécute sur les versions antérieures de Windows 10, mais tire également parti des nouvelles fonctionnalités lorsque votre application s’exécute sur un appareil disposant de la dernière mise à jour.

Vous devez effectuer trois étapes pour faire en sorte que votre application prenne en charge le plus large éventail d’appareils Windows 10.

- Tout d’abord, configurez votre projet Visual Studio afin de cibler les dernières API. Cela influe sur les résultats de la compilation de votre application.
- Ensuite, effectuez des vérifications à l’exécution pour vérifier que vous appelez uniquement les API présentes sur l’appareil sur lequel votre application s’exécute.
- Enfin, testez votre application sur la version minimale et la version cible de Windows 10.

## <a name="configure-your-visual-studio-project"></a>Configurer votre projet Visual Studio

La première étape de la prise en charge de plusieurs versions de Windows 10 consiste à spécifier les versions de système d’exploitation et de Kit de développement logiciel *Cible* et *Minimum* prises en charge dans votre projet Visual Studio.

- *Cible* : Version du kit SDK pour laquelle Visual Studio compile le code de votre application et exécute tous les outils. Toutes les API et les ressources de ce Kit de développement logiciel sont disponibles dans le code de votre application au moment de la compilation.
- *Minimum* : Version du kit SDK qui prend en charge la version la plus ancienne du système d’exploitation sur laquelle votre application peut s’exécuter (et qui sera déployée par le store) et la version pour laquelle Visual Studio compile le code de balisage de votre application. 

Pendant son exécution, votre application s’exécute par rapport à la version du système d’exploitation déployée : votre application lève donc des exceptions si vous utilisez des ressources ou si vous appelez des API qui ne sont pas disponibles dans cette version. Nous allons vous montrer comment effectuer des vérifications à l’exécution pour appeler les API appropriées dans la suite de cet article.

Les paramètres Cible et Minimum spécifient les limites de plage des versions de système d’exploitation et de Kit de développement logiciel. Toutefois, si vous testez votre application sur la version Minimum, vous pouvez être sûr qu’elle s’exécutera sur toutes les versions comprises entre la version Minimum et la version Cible.

> [!TIP]
> Visual Studio ne donne aucune information sur la compatibilité des API. Il vous incombe de tester et de vous assurer que votre application fonctionne comme prévu sur toutes les versions de système d’exploitation, de la version Minimum à la version Cible incluses.

Lorsque vous créez un projet dans Visual Studio 2015, Update 2 ou version ultérieure, vous êtes invité à définir les versions Cible et Minimum prises en charge par votre application. Par défaut, la version Cible est la version du Kit de développement logiciel installée la plus élevée, tandis que la version Minimum est la version du Kit de développement logiciel installée la moins élevée. Vous pouvez choisir les versions Cible et Minimum uniquement parmi les versions du Kit de développement logiciel installées sur votre ordinateur. 

![Définir le Kit de développement logiciel cible dans Visual Studio](images/vs-target-sdk-1.png)

Nous vous recommandons généralement de conserver les valeurs par défaut. Toutefois, si vous avez installé une version d’évaluation du Kit de développement et si vous écrivez du code de production, vous devez modifier la version Cible en passant du Kit de développement logiciel d’évaluation à la dernière version officielle du Kit de développement logiciel. 

Pour modifier la version Minimum et la version Cible d’un projet déjà créé dans Visual Studio, accédez à Projet -&gt; Propriétés -&gt; onglet Application -&gt; Ciblage.

![Modifier le Kit de développement logiciel cible dans Visual Studio](images/vs-target-sdk-2.png)

À titre de référence, le tableau suivant indique les numéros de build pour chaque kit SDK.

| Nom convivial | Version | Build OS/SDK |
| ---- | ---- | ---- |
| RTM | 1507 | 10240 |
| Mise à jour de novembre | 1511 | 10586 |
| Mise à jour anniversaire | 1607 | 14393 |
| Creators Update | 1703 | 15063 |
| Fall Creators Update | 1709 | 16299 |
| Mise à jour d’avril 2018 | 1803 | 17134 |
| Mise à jour d’octobre 2018 | 1809 | 17763 |
| Mise à jour de mai 2019 | 1903 | 18362 |

Vous pouvez télécharger les versions finales du SDK à partir des [archives de Windows SDK et de l’émulateur](https://developer.microsoft.com/windows/downloads/sdk-archive). Vous pouvez télécharger la dernière version du Kit de développement logiciel Windows Insider Preview dans la section du site [Windows Insider](https://insider.windows.com/for-developers/) dédiée aux développeurs.

 Pour plus d’informations sur les mises à jour Windows 10, consultez [Informations sur les versions Windows 10](https://www.microsoft.com/itpro/windows-10/release-information). Pour obtenir des informations importantes sur le cycle de vie du support Windows 10, consultez [Infos-clés sur le cycle de vie Windows](https://support.microsoft.com/help/13853/windows-lifecycle-fact-sheet).

## <a name="perform-api-checks"></a>Effectuer des vérifications d’API

La clé pour les applications adaptatives de version est la combinaison des contrats API et de la classe [ApiInformation](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation). Cette classe vous permet de détecter si un membre, un type ou un contrat API spécifié est présent pour vous permettre d’effectuer des appels d’API sur divers appareils et versions de système d’exploitation de manière sécurisée.

### <a name="api-contracts"></a>Contrats API

L’ensemble des API au sein d’une famille d’appareils est subdivisé en contrats API. La méthode **ApiInformation.IsApiContractPresent** permet de tester la présence d’un contrat API. C’est utile si vous voulez tester la présence de nombreuses API qui existent toutes dans la même version d’un contrat API.

```csharp
    bool isScannerDeviceContract_1_Present =
        Windows.Foundation.Metadata.ApiInformation.IsApiContractPresent
            ("Windows.Devices.Scanners.ScannerDeviceContract", 1);
```

Qu’est-ce qu’un contrat API ? Un contrat API représente essentiellement une fonctionnalité – un ensemble d’API associées qui offrent ensemble une fonctionnalité particulière. Un contrat API hypothétique pourrait représenter un ensemble d’API contenant deux classes, cinq interfaces, une structure, deux énumérations, et ainsi de suite.

Les types logiquement associés sont regroupés dans un contrat API, et depuis Windows 10, chaque API Windows Runtime est membre d’un contrat API. Avec les contrats API, vous recherchez la disponibilité d’une fonctionnalité ou d’une API spécifique sur l’appareil. Autrement dit, vous vérifiez les fonctionnalités d’un appareil ; vous ne recherchez pas la présence d’un appareil ou d’un système d’exploitation spécifique. Une plateforme qui implémente les API d’un contrat API est nécessaire à l’implémentation de chaque API de ce contrat. Cela signifie que vous pouvez tester si le système d’exploitation en cours d’exécution prend en charge un contrat API particulier et, si tel est le cas, appeler n’importe quelle API de ce contrat API sans avoir à vérifier chacune d’elle individuellement.

Le contrat API le plus important et le plus utilisé est **Windows.Foundation.UniversalApiContract**. Il contient la majorité des API de la plateforme Windows universelle. La documentation relative aux [kits SDK d’extension de familles d’appareils et contrats API](https://docs.microsoft.com/uwp/extension-sdks/) décrit la diversité des contrats API disponibles. Vous verrez que la plupart d’entre eux représentent un ensemble d’API associées sur un plan fonctionnel.

> [!NOTE]
> Si vous avez installé une préversion du Kit de développement logiciel (SDK) Windows qui n’est pas encore documentée, vous trouverez également des informations sur la prise en charge des contrats API dans le fichier « Platform.xml » situé dans le dossier d’installation du SDK sur « \(Program Files (x86))\Windows Kits\10\Platforms\<plateforme>\<Version du SDK>\Platform.xml ».

### <a name="version-adaptive-code-and-conditional-xaml"></a>Code adaptatif de version et XAML conditionnel

Dans toutes les versions de Windows 10, vous pouvez utiliser la classe ApiInformation dans une condition à l’intérieur de votre code pour tester la présence de l’API que vous voulez appeler. Dans votre code adaptatif, vous pouvez utiliser différentes méthodes de la classe, notamment IsTypePresent, IsEventPresent, IsMethodPresent et IsPropertyPresent, pour tester la présence d’API au niveau de précision dont vous avez besoin.

Pour obtenir des informations et des exemples supplémentaires, consultez **[Code adaptatif de version](version-adaptive-code.md)** .

Si la version minimale de vos applications est la build 15063 (Creators Update) ou ultérieure, vous pouvez utiliser le *XAML conditionnel* pour définir des propriétés et instancier des objets dans le balisage sans avoir à utiliser le code-behind. Le XAML conditionnel permet d’utiliser la méthode ApiInformation.IsApiContractPresent dans le balisage.

Pour obtenir des informations et des exemples supplémentaires, consultez **[XAML conditionnel](conditional-xaml.md)** .

## <a name="test-your-version-adaptive-app"></a>Tester votre application adaptative de version

Quand vous utilisez du code adaptatif de version ou du XAML conditionnel pour écrire une application adaptative de version, vous devez le tester sur un appareil exécutant la version minimale et sur un appareil exécutant la version cible de Windows 10.

Vous ne pouvez pas tester tous les chemins de code conditionnel sur un seul appareil. Pour faire en sorte que tous les chemins de code soient testés, vous devez déployer et tester votre application sur un appareil distant (ou une machine virtuelle) exécutant la version minimale du système d’exploitation pris en charge.
Pour plus d’informations sur le débogage à distance, consultez [Déploiement et débogage des applications UWP](deploying-and-debugging-uwp-apps.md).

## <a name="related-articles"></a>Articles connexes

- [Qu’est-ce qu’une application UWP ?](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)
- [Dynamically detecting features with API contracts](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/)
- [API Contracts](https://channel9.msdn.com/Events/Build/2015/3-733) (Vidéo Build 2015)
