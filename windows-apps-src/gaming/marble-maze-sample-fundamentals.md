---
title: Notions de base de l’exemple Marble Maze
description: Ce document présente les principales caractéristiques du projet Marble Maze, notamment la façon dont il utilise Visual C++ dans l’environnement Windows Runtime, mais également la façon dont il est créé, structuré et généré.
ms.assetid: 73329b29-62e3-1b36-01db-b7744ee5b4c3
ms.date: 08/22/2017
ms.topic: article
keywords: windows 10, uwp, jeux, exemples, directx, principes de base
ms.localizationpriority: medium
ms.openlocfilehash: ff39abadc82cc3e0a5d0296ed499baa3b85f2714
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258487"
---
# <a name="marble-maze-sample-fundamentals"></a>Notions de base de l’exemple Marble Maze




Cette rubrique présente les principales caractéristiques du projet Marble Maze, notamment la façon dont il utilise Visual C++ dans l’environnement Windows Runtime, mais également la façon dont il est créé, structuré et généré. Cette rubrique décrit également plusieurs des conventions utilisées dans le code.

> [!NOTE]
> L’exemple de code correspondant à ce document est disponible dans [l’exemple de jeu Marble Maze en DirectX](https://github.com/microsoft/Windows-appsample-marble-maze).

Ce document traite certains points importants relatifs à la planification et au développement de votre jeu de plateforme Windows universelle (UWP).

-   Utilisez le modèle d' **application DirectX 11 (Universal C++Windows-/CX)** dans Visual Studio pour créer votre jeu DirectX UWP.
-   Windows Runtime fournit des classes et des interfaces vous permettant de développer des applications UWP grâce à une approche orientée objet plus moderne.
-   Utilisez des références d’objet avec le symbole Hat (^) pour gérer la durée de vie des variables de Windows Runtime, [Microsoft :: WRL :: ComPtr](https://docs.microsoft.com/cpp/windows/comptr-class) pour gérer la durée de vie des objets com et [std :: Shared\_PTR](https://docs.microsoft.com/cpp/standard-library/shared-ptr-class) ou [std :: unique\_PTR](https://docs.microsoft.com/cpp/standard-library/unique-ptr-class) pour gérer la C++ durée de vie de tous les autres objets alloués par tas.
-   Dans la majorité des cas, utilisez la gestion des exceptions plutôt que les codes de résultats pour gérer les erreurs inattendues.
-   Utilisez les [annotations SAL](https://docs.microsoft.com/visualstudio/code-quality/using-sal-annotations-to-reduce-c-cpp-code-defects) et les outils d’analyse du code pour découvrir les erreurs présentes dans votre application.

## <a name="creating-the-visual-studio-project"></a>Création du projet Visual Studio


Si vous avez téléchargé et extrait l’exemple, vous pouvez ouvrir le fichier **MarbleMaze_VS2017.sln** (dans le dossier **C++** ) dans Visual Studio pour visualiser le code.

Nous sommes partis d’un projet existant pour la création du projet Visual Studio pour Marble Maze. Toutefois, si vous n’avez pas encore de projet existant qui fournit les fonctionnalités de base requises par votre jeu UWP DirectX, nous vous recommandons de créer un projet basé sur le modèle **application Visual Studio DirectX 11 (Universal C++Windows-/CX)** , car il fournit une application 3D de base. Pour cela, procédez comme suit:

1. Dans Visual Studio 2019, sélectionnez **fichier > nouveau projet de >...**

2. Dans la fenêtre **créer un nouveau projet** , sélectionnez **application DirectX 11 (Universal Windows- C++/CX)** . Si vous ne voyez pas cette option, vous ne disposez peut-être pas des composants requis installés&mdash;consultez [modifier Visual Studio 2019 en ajoutant ou en supprimant des charges de travail et des composants](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) pour plus d’informations sur l’installation de composants supplémentaires.

![Nouveau projet](images/vs2019-marble-maze-sample-fundamentals-1.png)

3. Sélectionnez **suivant**, puis entrez un **nom de projet**, un **emplacement** pour les fichiers à stocker et un nom de **solution**, puis sélectionnez **créer**.



Un paramètre de projet important dans le modèle d' **application DirectX 11 ( C++Universal Windows-/CX)** est l’option **/ZW** , qui permet au programme d’utiliser les extensions de langage Windows Runtime. Cette option est activée par défaut lorsque vous utilisez le modèle Visual Studio. Voir [Définition des options du compilateur](https://docs.microsoft.com/cpp/build/reference/setting-compiler-options) pour plus d’informations sur la définition des options du compilateur dans Visual Studio.

> **Attention**   l’option **/ZW** n’est pas compatible avec les options telles que **/CLR**. Dans le cas de **/clr**, cela signifie que vous ne pouvez pas cibler .NET Framework et Windows Runtime à partir du même projet Visual C++.

 

Chaque application UWP que vous obtenez à partir de la Microsoft Store est fournie sous la forme d’un package d’application. Un package d’application comprend un manifeste de package qui contient des informations sur votre application. Par exemple, vous pouvez spécifier les capacités (autrement dit, l’accès requis aux ressources système ou aux données utilisateur protégées) de votre application. Si vous déterminez que votre application a besoin de certaines fonctionnalités, utilisez le manifeste du package pour les déclarer. Le manifeste vous permet également de spécifier des propriétés de projet telles que les rotations prises en charge de l’appareil, les images de la vignette et l’écran de démarrage. Vous pouvez modifier le manifeste en ouvrant **Package.appxmanifest** dans votre projet. Pour plus d’informations sur les packages d’application, voir [Création de packages d’application](https://docs.microsoft.com/windows/uwp/packaging/index).

##  <a name="building-deploying-and-running-the-game"></a>Génération, déploiement et exécution du jeu

Dans les menus déroulants situés dans la partie supérieure de Visual Studio, à gauche du bouton vert de lecture, sélectionnez votre configuration de déploiement. Nous vous recommandons de la définir sur **Déboguer** en ciblant l’architecture de votre appareil (**x86** pour 32 bits, **x64** pour 64 bits) et sur votre **machine locale**. Vous pouvez également effectuer un test sur une **machine distante**, ou sur un **appareil** connecté via USB. Cliquez ensuite sur le bouton vert de lecture pour générer et effectuer le déploiement sur votre appareil.

![Débogage ; x64 ; ordinateur local](images/marble-maze-sample-fundamentals-2.png)

###  <a name="controlling-the-game"></a>Contrôle du jeu

Vous pouvez utiliser les fonctions tactiles, l’accéléromètre, la manette Xbox One ou la souris pour contrôler le jeu Marble Maze.

-   Utilisez la croix directionnelle de la manette pour modifier l’élément de menu actif.
-   Utilisez les fonctions tactiles, la touche A ou le bouton Démarrer de la manette, ou la souris pour sélectionner un élément de menu.
-   Utilisez les fonctions tactiles, l’accéléromètre, le stick analogique gauche ou la souris pour incliner le jeu.
-   Utilisez les fonctions tactiles, la touche A ou le bouton Démarrer de la manette ou la souris pour fermer les menus, tels que le tableau des scores.
-   Utilisez le bouton Démarrer de la manette ou la touche P du clavier pour interrompre ou reprendre le jeu.
-   Utilisez le bouton Retour de la manette ou la touche Début du clavier pour redémarrer le jeu.
-   Quand le tableau des scores est visible, utilisez le bouton Retour de la manette ou la touche Début du clavier pour effacer tous les scores.

##  <a name="code-conventions"></a>Conventions de code


Windows Runtime est une interface de programmation permettant de créer des applications pour UWP qui ne peuvent être exécutées que dans un environnement d’application particulier. De telles applications utilisent des fonctions, des types de données et des appareils autorisés, et sont distribuées à partir de la Microsoft Store. Au niveau le plus bas, Windows Runtime est composé d’une interface binaire d’application. L’interface binaire d’application est un contrat binaire de bas niveau qui rend les API Windows Runtime accessibles à plusieurs langages de programmation tels que les langages JavaScript, .NET et Visual C++.

Afin d’appeler les API Windows Runtime à partir des langages JavaScript et .NET, ces langages nécessitent des projections spécifiques à chaque environnement de langage. Quand vous appelez une API Windows Runtime à partir du langage JavaScript ou .NET, vous invoquez la projection, laquelle appelle à son tour la fonction ABI sous-jacente. Bien que vous puissiez également appeler les fonctions ABI directement à partir du langage C++, Microsoft fournit également des projections pour C++, car elles permettent de consommer nettement plus facilement les API Windows Runtime, tout en maintenant de hautes performances. Microsoft fournit également des extensions de langage pour Visual C++ qui prennent en charge spécifiquement les projections Windows Runtime. Plusieurs de ces extensions de langage présentent une syntaxe similaire à celle du langage C++/CLI. Toutefois, au lieu de cibler l’environnement CLR (Common Langage Runtime), les applications natives utilisent cette syntaxe pour cibler Windows Runtime. Le modificateur de référence d’objet, ou accent circonflexe (^), est un élément important de cette nouvelle syntaxe, car il permet l’effacement automatique des objets d’exécution au moyen du décompte de références. Au lieu d’appeler des méthodes telles que [AddRef](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-addref) et [Release](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-release) pour gérer la durée de vie d’un objet Windows Runtime, le runtime supprime l’objet quand aucun autre composant ne le référence, par exemple lorsqu’il quitte l’étendue ou que vous attribuez la valeur **nullptr** à toutes les références. Une autre part importante de l’utilisation de Visual C++ pour créer des applications UWP relève du mot-clé **ref new**. Utilisez **ref new** à la place de **new** pour créer des objets Windows Runtime avec décompte des références. Pour plus d’informations, voir [Système de types (C++/CX)](https://docs.microsoft.com/cpp/cppcx/type-system-c-cx).

> [!IMPORTANT]
> Vous devez uniquement utiliser **^** et **ref new** quand vous créez des objets ou des composants Windows Runtime. La syntaxe C++ standard peut vous servir à écrire du code de l’application principale qui n’utilise pas Windows Runtime.

Marble Maze utilise **^** et **Microsoft::WRL::ComPtr** pour gérer les objets alloués par segment de mémoire et limiter les fuites de mémoire. Nous vous recommandons d’utiliser ^ pour gérer la durée de vie des variables de Windows Runtime, **ComPtr** pour gérer la durée de vie des variables com (par exemple, lorsque vous utilisez DirectX) et **std :: Shared\_PTR** ou **std :: unique\_PTR** pour gérer la durée C++ de vie de tous les autres objets alloués par tas.

 

Pour plus d’informations sur les extensions de langage qui sont à la disposition d’une application UWP en C++, voir [Informations de référence sur le langage Visual C++ (C++/CX)](https://docs.microsoft.com/cpp/cppcx/visual-c-language-reference-c-cx).

###  <a name="error-handling"></a>Gestion des erreurs

Le jeu Marble Maze utilise la gestion des exceptions comme principal moyen de traiter les erreurs inattendues. Même si le code de jeu utilise généralement des codes de journalisation ou des codes d’erreur, tels que les valeurs **HRESULT** pour indiquer des erreurs, la gestion des exceptions présente deux principaux avantages. En premier lieu, elle facilite la lecture et la maintenance du code. Du point de vue du code, la gestion des exceptions constitue un moyen plus efficace de propager une erreur vers une routine qui se chargera de sa gestion. L’utilisation de codes d’erreur exige généralement que chaque fonction propage explicitement les erreurs. L’autre avantage est que vous pouvez configurer le débogueur Visual Studio de façon à s’arrêter immédiatement au niveau de l’emplacement et du contexte de l’erreur quand une exception est levée. Windows Runtime a largement recours à la gestion des exceptions. Par conséquent, en utilisant la gestion des exceptions dans votre code, vous pouvez combiner toute la gestion des exceptions en un seul modèle.

Nous vous recommandons d’utiliser les conventions suivantes dans votre modèle de gestion des erreurs :

-   Utilisez des exceptions pour communiquer des erreurs inattendues.
-   N’utilisez pas les exceptions pour contrôler le flux de code.
-   Interceptez uniquement les exceptions que vous pouvez gérer sans risque et à partir desquelles une récupération est possible. Sinon, n’interceptez pas l’exception et laissez l’application s’arrêter.
-   Quand vous appelez une routine DirectX qui renvoie **HRESULT**, utilisez la fonction **DX::ThrowIfFailed**. Cette fonction est définie dans [DirectXHelper.h](https://github.com/Microsoft/Windows-appsample-marble-maze/blob/master/C%2B%2B/Shared/DirectXHelper.h). **ThrowIfFailed** lève une exception si la valeur **HRESULT** fournie est un code d’erreur. Par exemple, **E\_pointeur** force **ThrowIfFailed** à lever [Platform :: NullReferenceException](https://docs.microsoft.com/cpp/cppcx/platform-nullreferenceexception-class).

    Quand vous utilisez **ThrowIfFailed**, placez l’appel DirectX sur une ligne distincte pour améliorer la lisibilité du code, comme illustré dans l’exemple suivant.

    ```cpp
    // Identify the physical adapter (GPU or card) this device is running on.
    ComPtr<IDXGIAdapter> dxgiAdapter;
    DX::ThrowIfFailed(
        dxgiDevice->GetAdapter(&dxgiAdapter)
        );
    ```

-   Même si l’utilisation de **HRESULT** pour les erreurs inattendues est à éviter, il est encore plus important d’éviter celle de la gestion des exceptions pour contrôler le flux de code. Utilisez plutôt une valeur de retour **HRESULT**, si nécessaire, pour contrôler le flux de code.

###  <a name="sal-annotations"></a>Annotations SAL

Utilisez les annotations SAL et les outils d’analyse du code pour découvrir les erreurs présentes dans votre application.

Le langage SAL de Microsoft vous permet d’annoter (ou décrire) la façon dont une fonction utilise ses paramètres. Les annotations SAL décrivent également des valeurs de retour. Les annotations SAL peuvent être utilisées conjointement à l’outil d’analyse du code C/C++ pour découvrir les éventuelles erreurs du code source C ou C++. Les erreurs de codage courantes signalées par l’outil sont notamment les dépassements de mémoire tampon, une mémoire non initialisée, les déréférencements du pointeur Null et les fuites de mémoire et de ressources.

Utilisez plutôt la méthode **BasicLoader::LoadMesh** qui est déclarée dans [BasicLoader.h](https://github.com/Microsoft/Windows-appsample-marble-maze/blob/e62d68a85499e208d591d2caefbd9df62af86809/C%2B%2B/Shared/BasicLoader.h). Cette méthode utilise `_In_` pour spécifier que *filename* est un paramètre d’entrée (et qu’il ne peut donc faire l’objet que d’une lecture), `_Out_` pour indiquer que *vertexBuffer* et *indexBuffer* sont des paramètres de sortie (et qu’ils feront donc uniquement l’objet d’une écriture) et `_Out_opt_` pour spécifier que *vertexCount* et *indexCount* sont des paramètres de sortie facultatifs (pouvant faire l’objet d’une écriture). Étant donné que *vertexCount* et *indexCount* sont des paramètres de sortie optionnels, ils peuvent avoir la valeur **nullptr**. L’outil d’analyse du code C/C++ examine les appels à cette méthode pour s’assurer que les paramètres qu’elle transmet répondent à ces critères.

```cpp
void LoadMesh(
    _In_ Platform::String^ filename,
    _Out_ ID3D11Buffer** vertexBuffer,
    _Out_ ID3D11Buffer** indexBuffer,
    _Out_opt_ uint32* vertexCount,
    _Out_opt_ uint32* indexCount
    );
```

Pour effectuer l’analyse du code de votre application, dans la barre de menus, sélectionnez **Générer > Exécuter l’analyse du code sur la solution**. Pour plus d’informations sur l’analyse de code, voir [Analyse de la qualité du code C/C++ à l’aide de l’analyse du code](https://docs.microsoft.com/visualstudio/code-quality/analyzing-c-cpp-code-quality-by-using-code-analysis).

La liste complète des annotations disponibles est définie dans le fichier sal.h. Pour plus d’informations, voir [Annotations SAL](https://docs.microsoft.com/cpp/c-runtime-library/sal-annotations).

## <a name="next-steps"></a>Étapes suivantes


Pour plus d’informations sur la structure du code de l’application Marble Maze, ainsi que sur les différences entre la structure d’une application UWP en DirectX et celle d’une application de bureau classique, voir [Structure de l’application Marble Maze](marble-maze-application-structure.md).

## <a name="related-topics"></a>Rubriques connexes


* [Structure d’application labyrinthe de marbre](marble-maze-application-structure.md)
* [Développement de labyrinthe, un jeu UWP dans C++ et DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 




