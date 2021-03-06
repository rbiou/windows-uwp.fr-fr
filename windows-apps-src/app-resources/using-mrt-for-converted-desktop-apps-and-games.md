---
title: Utilisation de MRT pour les jeux et applications de bureau converties
description: En empaquetant votre application ou votre jeu .NET ou Win32 sous la forme d’un package. msix ou. AppX, vous pouvez tirer parti du système de gestion des ressources pour charger les ressources d’application adaptées au contexte d’exécution. Cette rubrique détaillée décrit ces techniques.
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10, uwp, mrt, pri. ressources, jeux, centennial, desktop app converter, interface utilisateur multilingue, assembly satellite
ms.localizationpriority: medium
ms.openlocfilehash: c753e9437c76c89ac6af8cedcb1f954d1ce56fe3
ms.sourcegitcommit: 3e7a4f7605dfb4e87bac2d10b6d64f8b35229546
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/08/2020
ms.locfileid: "77089446"
---
# <a name="use-the-windows-10-resource-management-system-in-a-legacy-app-or-game"></a>Utiliser le système de gestion des ressources Windows 10 dans une application ou un jeu hérité

Les applications et les jeux .NET et Win32 sont souvent traduits dans différentes langues, agrandissant ainsi leur marché global. Pour plus d’informations sur la proposition de valeur de la localisation de votre application, consultez [Internationalisation et localisation](../design/globalizing/globalizing-portal.md). En empaquetant votre application ou votre jeu .NET ou Win32 sous la forme d’un package. msix ou. AppX, vous pouvez tirer parti du système de gestion des ressources pour charger les ressources d’application adaptées au contexte d’exécution. Cette rubrique détaillée décrit ces techniques.

Il existe plusieurs façons de localiser une application Win32 traditionnelle, mais Windows 8 a introduit un [nouveau système de gestion des ressources](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10)) qui fonctionne sur l’ensemble des langages de programmation et des types d’applications et fournit des fonctionnalités supplémentaires et allant au-delà de la simple localisation. Ce système est appelé « MRT » dans cette rubrique. Historiquement, cela signifiait « Modern Resource Technology », mais le terme « Modern » a été abandonné. Le gestionnaire de ressources peut également être appelé MRM (Gestionnaire de ressources modernes) ou PRI (Index de ressource de package).

Combiné au déploiement basé sur MSIX ou. AppX (par exemple, à partir de l’Microsoft Store), MRT peut automatiquement fournir les ressources les plus applicables pour un utilisateur/appareil donné, ce qui réduit le téléchargement et l’installation de la taille de votre application. Cette réduction de la taille peut être significative pour les applications contenant une grande quantité de contenu localisé, et peut relever de l'ordre de plusieurs *gigaoctets* pour les jeux AAA. MRT confère des avantages supplémentaires, comme des listes localisées dans l’interpréteur de commandes Windows et le Microsoft Store, la logique de secours automatique lorsque la langue par défaut d’un utilisateur ne correspond pas à vos ressources disponibles.

Ce document décrit l’architecture haut niveau de MRT et fournit un guide de portage pour vous aider à migrer des applications héritées Win32 vers MRT avec des modifications de code minimales. Une fois la migration vers MRT effectuée, les avantages supplémentaires (par exemple, la possibilité de segmenter les ressources par facteur de montée ou par thème de système) seront disponibles aux développeurs. Veuillez noter que la localisation basée sur MRT fonctionne à la fois pour les applications UWP et les applications Win32 traitées par le pont de bureau (également appelée « Centennial »).

Dans de nombreux cas, vous pouvez continuer d'utiliser vos formats de localisation et le code source existants, tout en intégrant MRT pour la résolution des ressources lors de l’exécution et la réduction des tailles de téléchargement : il s'agit d'une approche flexible. Le tableau suivant résume le travail et le rapport coût/bénéfice estimé de chaque étape. Ce tableau n’inclut pas les tâches non liées à la localisation, par exemple la fourniture d’icônes d’application haute résolution ou à contraste élevé. Pour plus d’informations sur la fourniture de plusieurs ressources pour les vignettes, les icônes, etc., voir [Personnaliser vos ressources pour la langue, l’échelle, le contraste élevé et d’autres qualificateurs](tailor-resources-lang-scale-contrast.md).

<table>
<tr>
<th>Travail</th>
<th>Avantage</th>
<th>Coût estimé</th>
</tr>
<tr>
<td>Localiser le manifeste du package</td>
<td>Travail minimal requis pour que votre contenu localisé apparaisse dans l’interpréteur de commandes Windows et dans le Microsoft Store</td>
<td>Petite</td>
</tr>
<tr>
<td>Utilisez MRT pour identifier et localiser les ressources</td>
<td>Condition préalable pour minimiser les tailles de téléchargement et de l’installation ; secours automatique de la langue</td>
<td>Moyenne</td>
</tr>
<tr>
<td>Créer des packs de ressources</td>
<td>Étape finale pour réduire les tailles de téléchargement et d'installation</td>
<td>Petite</td>
</tr>
<tr>
<td>Migration vers les API et les formats de ressources MRT</td>
<td>Tailles de fichiers beaucoup plus petites (selon la technologie de ressource existante)</td>
<td>Grande</td>
</tr>
</table>

## <a name="introduction"></a>Introduction

La plupart des applications non triviales contiennent des éléments d’interface utilisateur appelés *ressources* qui sont dissociés du code de l’application (contrairement aux *valeurs codées en dur* qui sont créées dans le code source proprement dit). Il existe plusieurs raisons de préférer les ressources aux valeurs codées en dur : elles sont plus faciles à éditer pour les utilisateurs qui ne sont pas des développeurs, par exemple, mais l'une des raisons essentielles est qu'un tel choix permet à l'application de sélectionner différentes représentations de la même ressource logique au moment de l'exécution. Par l’exemple, le texte à afficher sur un bouton (ou l’image à afficher dans une icône) peut différer selon les langues comprises par l’utilisateur, les caractéristiques du périphérique d’affichage, ou l'activation de technologies d’assistance.

Ainsi, l’objectif principal d’une technologie de gestion des ressources est donc de traduire, lors de l’exécution, une demande pour un *nom de la ressource* logique ou symbolique (tel que `SAVE_BUTTON_LABEL`) dans la *valeur* réelle la plus adaptée (par ex., « enregistrer ») parmi un ensemble de *candidats* possibles (par ex., « Enregistrer », « Speichern » ou « 저장 »). MRT fournit cette fonction, et permet aux applications d’identifier les candidats à la ressource à l’aide d’un large éventail d’attributs, appelés *qualificateurs*, tels que la langue de l’utilisateur, le facteur d’échelle de l'écran, le thème sélectionné par l’utilisateur et d’autres facteurs environnementaux. MRT prend même en charge les qualificateurs personnalisés pour les applications qui le requièrent (par exemple, une application peut fournir différentes ressources graphiques pour les utilisateurs qui ont ouvert une session avec un compte par opposition aux utilisateurs invités, sans ajouter explicitement cette vérification dans chaque partie de leur application). MRT fonctionne avec les ressources de chaîne et les ressources basées sur des fichiers, ces dernières étant implémentées en tant que références aux données externes (les fichiers eux-mêmes).

### <a name="example"></a>Exemple

Voici un exemple simple d’une application dotée d'étiquettes de texte sur deux boutons (`openButton` et `saveButton`) et d'un fichier PNG utilisé pour un logo (`logoImage`). Les étiquettes de texte sont localisées en anglais et en allemand et le logo est optimisé pour les écrans de bureau standard (facteur d’échelle de 100 %) et les téléphones à haute résolution (facteur d’échelle de 300 %). Notez que le schéma suivant présente une vue conceptuelle de haut niveau du modèle ; il ne correspond pas exactement à l’implémentation.

<p><img src="images\conceptual-resource-model.png"/></p>

Dans le graphique, le code d’application fait référence à trois noms de ressources logiques. Lors de l’exécution, la pseudo-fonction `GetResource` utilise MRT pour rechercher les noms des ressources dans le tableau de ressources (appelé fichier PRI) et trouver le candidat le plus approprié, sur la base des conditions ambiantes (langue de l’utilisateur et facteur d’échelle de l’affichage). Dans le cas des étiquettes, les chaînes sont utilisées directement. Dans le cas de l’image du logo, les chaînes sont interprétées comme noms de fichiers et les fichiers sont lus sur disque. 

Si l’utilisateur parle une langue autre que l’anglais ou l’allemand, ou a un facteur d’échelle d’affichage autre que 100% ou 300%, MRT choisit le candidat correspondant « le plus proche » en fonction d’un ensemble de règles de secours (voir [système de gestion des ressources](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10)) pour plus d’informations sur l’arrière-plan).

Veuillez noter que MRT prend en charge les ressources qui sont adaptées à plusieurs qualificateurs, par exemple, si l’image du logo contient du texte incorporé qui doit également être localisé, le logo a quatre candidats : EN/Scale-100, DE/Scale-100, EN/Scale-300 et DE/Scale-300.

### <a name="sections-in-this-document"></a>Sections dans ce document

Les sections suivantes décrivent les tâches principales nécessaires à l’intégration de MRT avec votre application.

#### <a name="phase-0-build-an-application-package"></a>Phase 0 : Créer un package d’application

Cette section explique comment obtenir votre création d'application de bureau en tant que package d’application. Aucune fonctionnalité MRT n'est utilisée à ce stade.

#### <a name="phase-1-localize-the-application-manifest"></a>Phase 1 : Localiser le manifeste d’application

Cette section décrit comment localiser le manifeste de votre application (de sorte que celle-ci s’affiche correctement dans l’interpréteur de commandes Windows), tout en continuer d'utiliser votre format de ressource hérité et les API pour empaqueter et localiser les ressources. 

#### <a name="phase-2-use-mrt-to-identify-and-locate-resources"></a>Phase 2 : Utilisation de MRT pour identifier et localiser les ressources

Cette section décrit comment modifier votre code d’application (et éventuellement la disposition de la ressource) pour localiser les ressources à l’aide de MRT, tout en utilisant les API et les formats de ressource existants pour charger et consommer des ressources. 

#### <a name="phase-3-build-resource-packs"></a>Phase 3 : Créer des packs de ressources

Cette section décrit les modifications finales nécessaires pour séparer vos ressources en *packs de ressources* distincts, réduisant ainsi la taille de téléchargement (et l'installation) de votre application.

### <a name="not-covered-in-this-document"></a>Non traitée dans ce document.

Une fois les phases 0-3 ci-dessus terminées, vous disposerez d’une application « bundle » qui peut être soumise à la Microsoft Store et qui réduira la taille de téléchargement et d’installation des utilisateurs en omettant les ressources dont ils n’ont pas besoin (par exemple, les langues qu’ils ne parlent pas). Vous pouvez apporter des améliorations supplémentaires au niveau de la taille et des fonctionnalités de l’application en effectuant une étape finale.

#### <a name="phase-4-migrate-to-mrt-resource-formats-and-apis"></a>Phase 4 : Migrer vers les API et les formats de ressources MRT

Cette phase n'est pas abordée dans ce document ; elle implique le transfert de vos ressources (notamment les chaînes) à partir des formats hérités, tels que des assemblys de ressources DLL d’interface utilisateur multilingue ou .NET vers des fichiers PRI. Cela peut permettre de gagner davantage d'espace au niveau des tailles de téléchargement et d'installation. Cela permet également d’utiliser d'autres fonctionnalités MRT, par exemple, la réduction des tailles de téléchargement et d’installation de l’image de fichiers en fonction du facteur d’échelle, des paramètres d’accessibilité, etc.

## <a name="phase-0-build-an-application-package"></a>Phase 0 : Créer un package d’application

Avant d’apporter des modifications aux ressources de votre application, vous devez tout d’abord remplacer votre technologie de mise en package et d’installation actuelle par la technologie de création de packages et de déploiement UWP standard. Il existe trois façons de procéder :

* Si vous disposez d’une application de bureau de grande taille avec un programme d’installation complexe ou que vous utilisez un grand nombre de points d’extensibilité de système d’exploitation, vous pouvez utiliser l’outil Desktop App Converter pour générer les informations de mise en page et de manifeste du fichier UWP à partir de votre programme d’installation d’application existant (par exemple, un MSI).
* Si vous disposez d’une application de bureau plus petite avec relativement peu de fichiers ou un simple programme d’installation et aucun hook d’extensibilité, vous pouvez créer manuellement la disposition des fichiers et les informations du manifeste.
* Si vous procédez à une reconstruction à partir de la source et que vous souhaitez mettre à jour votre application pour qu’elle soit une application UWP pure, vous pouvez créer un projet dans Visual Studio et vous fier à l’IDE pour effectuer une grande partie du travail.

Si vous souhaitez utiliser le convertisseur d’application de [Bureau](https://www.microsoft.com/store/p/desktopappconverter/9nblggh4skzw), consultez [empaqueter une application de bureau à l’aide du convertisseur](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-run-desktop-app-converter) d’application de bureau pour plus d’informations sur le processus de conversion. Vous trouverez un ensemble complet d’exemples de convertisseurs Desktop sur [le pont de bureau pour les exemples UWP GitHub référentiel](https://github.com/Microsoft/DesktopBridgeToUWP-Samples).

Si vous souhaitez créer manuellement le package, vous devez créer une structure de répertoires qui comprend tous les fichiers de votre application (fichiers exécutables et contenu, mais pas le code source) et un fichier manifeste de package (. appxmanifest). Vous trouverez un exemple dans [l’exemple Hello, World GitHub](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/blob/master/Samples/HelloWorldSample/CentennialPackage/AppxManifest.xml), mais un fichier manifeste de package de base qui exécute l’exécutable de bureau nommé `ContosoDemo.exe` se présente comme suit, où le <span style="background-color: yellow">texte mis en surbrillance</span> serait remplacé par vos propres valeurs.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
         xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
         xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
         xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
         IgnorableNamespaces="uap mp rescap">
    <Identity Name="Contoso.Demo"
              Publisher="CN=Contoso.Demo"
              Version="1.0.0.0" />
    <Properties>
    <DisplayName>Contoso App</DisplayName>
    <PublisherDisplayName>Contoso, Inc</PublisherDisplayName>
    <Logo>Assets\StoreLogo.png</Logo>
  </Properties>
    <Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14393.0" 
                        MaxVersionTested="10.0.14393.0" />
  </Dependencies>
    <Resources>
    <Resource Language="en-US" />
  </Resources>
    <Applications>
    <Application Id="ContosoDemo" Executable="ContosoDemo.exe" 
                 EntryPoint="Windows.FullTrustApplication">
    <uap:VisualElements DisplayName="Contoso Demo" BackgroundColor="#777777" 
                        Square150x150Logo="Assets\Square150x150Logo.png" 
                        Square44x44Logo="Assets\Square44x44Logo.png" 
        Description="Contoso Demo">
      </uap:VisualElements>
    </Application>
  </Applications>
    <Capabilities>
    <rescap:Capability Name="runFullTrust" />
  </Capabilities>
</Package>
```

Pour plus d’informations sur le fichier manifeste du package et la disposition des packages, consultez [manifeste du package d’application](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest).

Enfin, si vous utilisez Visual Studio pour créer un nouveau projet et migrer votre code existant dans, consultez [créer une application « Hello, World »](https://docs.microsoft.com/windows/uwp/get-started/create-a-hello-world-app-xaml-universal). Vous pouvez inclure votre code existant dans le nouveau projet, mais vous devrez probablement apporter des modifications de code significatives (en particulier dans l’interface utilisateur) afin de l’exécuter en tant qu’application UWP pure. Ces modifications ne sont pas abordées dans ce document.

## <a name="phase-1-localize-the-manifest"></a>Phase 1 : localisation du manifeste

### <a name="step-11-update-strings--assets-in-the-manifest"></a>Étape 1,1 : mettre à jour les chaînes & les ressources dans le manifeste

En phase 0, vous avez créé un fichier manifeste du package de base (. appxmanifest) pour votre application (en fonction des valeurs fournies au convertisseur, extraites du MSI ou entrées manuellement dans le manifeste), mais il ne contient pas d’informations localisées et ne prend pas en charge fonctionnalités supplémentaires telles que les composants de la mosaïque de démarrage haute résolution, etc.

Pour vous assurer que le nom et la description de votre application sont correctement localisés, vous devez définir certaines ressources dans un ensemble de fichiers de ressources et mettre à jour le manifeste du package pour y faire référence.

#### <a name="creating-a-default-resource-file"></a>Création d’un fichier de ressources par défaut

La première étape consiste à créer un fichier de ressources par défaut dans votre langue par défaut (par ex., anglais américain). Pour ce faire, vous pouvez procéder manuellement avec un éditeur de texte, ou via le Concepteur de ressources dans Visual Studio.

Si vous souhaitez créer les ressources manuellement :

1. Créez un fichier XML appelé `resources.resw` et placez-le dans un sous-dossier `Strings\en-us` de votre projet. Utilisez le code BCP-47 approprié si votre langue par défaut n’est pas l’anglais (États-Unis).
2. Dans le fichier XML, ajoutez le contenu suivant, le <span style="background-color: yellow">texte mis en surbrillance</span> étant remplacé par le texte correspondant à votre application, dans votre langue par défaut.

> [!NOTE]
> Il existe des restrictions sur les longueurs de certaines de ces chaînes. Pour plus d’informations, voir [VisualElements](/uwp/schemas/appxpackage/appxmanifestschema/element-visualelements).

```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
  <data name="ApplicationDescription">
    <value>Contoso Demo app with localized resources (English)</value>
  </data>
  <data name="ApplicationDisplayName">
    <value>Contoso Demo Sample (English)</value>
  </data>
  <data name="PackageDisplayName">
    <value>Contoso Demo Package (English)</value>
  </data>
  <data name="PublisherDisplayName">
    <value>Contoso Samples, USA</value>
  </data>
  <data name="TileShortName">
    <value>Contoso (EN)</value>
  </data>
</root>
```

Si vous souhaitez utiliser le concepteur dans Visual Studio :

1. Créez le dossier `Strings\en-us` (ou un autre langage approprié) dans votre projet et ajoutez un **nouvel élément** au dossier racine de votre projet, en utilisant le nom par défaut de `resources.resw`. Veillez à choisir le **fichier de ressources (. resw)** et non le **dictionnaire de ressources** . un dictionnaire de ressources est un fichier utilisé par les applications XAML.
2. À l’aide du concepteur, entrez les chaînes suivantes (utilisent les mêmes `Names` mais remplacez les `Values` par le texte approprié à votre application) :

<img src="images\editing-resources-resw.png"/>

> [!NOTE]
> Si vous démarrez avec le concepteur Visual Studio, vous pouvez toujours modifier directement le code XML en appuyant sur `F7`. Toutefois, si vous démarrez avec un fichier XML minimal, *le concepteur ne reconnaîtra pas le fichier*, car un grand nombre de métadonnées supplémentaires n'y figureront pas ; vous pouvez remédier à ce problème en copiant l’informations XSD réutilisable à partir d’un fichier généré par le concepteur dans votre fichier XML modifié manuellement.

#### <a name="update-the-manifest-to-reference-the-resources"></a>Mise à jour du manifeste pour référencer les ressources

Une fois que les valeurs sont définies dans le fichier `.resw`, l’étape suivante consiste à mettre à jour le manifeste pour référencer les chaînes de ressources. Là encore, vous pouvez modifier directement un fichier XML, ou utiliser le Concepteur de manifeste de Visual Studio.

Si vous modifiez directement le fichier XML, ouvrez le fichier `AppxManifest.xml` et apportez les modifications suivantes aux <span style="background-color: lightgreen">valeurs mises en surbrillance</span> ; utilisez *exactement* ce texte, et non pas un texte spécifique à votre application. Il n’est pas obligatoire d’utiliser ces noms de ressource exacts, vous pouvez choisir le vôtre, mais tout ce que vous choisissez doit correspondre exactement à l’ensemble du contenu du fichier &mdash;. Ces noms doivent correspondre aux `Names` que vous avez créés dans le fichier `.resw`, avec le préfixe `ms-resource:` et l’espace de noms `Resources/`. 

> [!NOTE]
> De nombreux éléments du manifeste ont été omis de cet extrait de code. ne supprimez rien.

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package>
  <Properties>
    <DisplayName>ms-resource:Resources/PackageDisplayName</DisplayName>
    <PublisherDisplayName>ms-resource:Resources/PublisherDisplayName</PublisherDisplayName>
  </Properties>
  <Applications>
    <Application>
      <uap:VisualElements DisplayName="ms-resource:Resources/ApplicationDisplayName"
        Description="ms-resource:Resources/ApplicationDescription">
        <uap:DefaultTile ShortName="ms-resource:Resources/TileShortName">
          <uap:ShowNameOnTiles>
            <uap:ShowOn Tile="square150x150Logo" />
          </uap:ShowNameOnTiles>
        </uap:DefaultTile>
      </uap:VisualElements>
    </Application>
  </Applications>
</Package>
```

Si vous utilisez le concepteur de manifeste Visual Studio, ouvrez le fichier. appxmanifest et modifiez les valeurs des <span style="background-color: lightgreen">valeurs en surbrillance</span> dans l’onglet **application* et l’onglet *Packaging* :

<img src="images\editing-application-info.png"/>
<img src="images\editing-packaging-info.png"/>

### <a name="step-12-build-pri-file-make-an-msix-package-and-verify-its-working"></a>Étape 1,2 : générer un fichier PRI, créer un package MSIX et vérifier qu’il fonctionne

Vous devez maintenant être en mesure de générer le fichier `.pri` et déployer l’application pour vérifier que les informations correctes (dans votre langue par défaut) s’affiche dans le Menu Démarrer.

Si vous utilisez Visual Studio pour la création, appuyez simplement sur `Ctrl+Shift+B` pour générer le projet, puis faites un clic droit sur le projet et choisissez `Deploy` dans le menu contextuel.

Si vous effectuez la génération manuellement, procédez comme suit pour créer un fichier de configuration pour `MakePRI` outil et pour générer le fichier `.pri` lui-même (vous trouverez plus d’informations dans la section [packages d’applications manuelles](/windows/msix/package/manual-packaging-root)) :

1. Ouvrez une invite de commandes développeur à partir du dossier **Visual studio 2017** ou **Visual Studio 2019** dans le menu Démarrer.
2. Basculez vers le répertoire racine du projet (celui qui contient le fichier. appxmanifest et le dossier **Strings** ).
3. Saisissez la commande suivante, en remplaçant « contoso_demo.xml » par un nom approprié pour votre projet, et « en-US » par la langue par défaut de votre application (ou conservez en-US le cas échéant). Notez que le fichier XML est créé dans le répertoire parent (et**non** dans le répertoire du projet), car il ne fait pas partie de l’application (vous pouvez choisir n’importe quel autre répertoire, mais veillez à le remplacer dans les commandes ultérieures).

    ```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US /pv 10.0 /o
    ```

    Vous pouvez taper `makepri createconfig /?` pour voir ce que fait chaque paramètre, mais pour résumer :
      * `/cf` définit le nom de fichier de configuration (la sortie de cette commande)
      * `/dq` définit les qualificateurs par défaut, dans ce cas la langue `en-US`
      * `/pv` définit la version de la plateforme, dans ce cas Windows 10
      * `/o` le définit pour remplacer le fichier de sortie s’il existe

4. Maintenant que vous disposez d'un fichier de configuration, exécutez à nouveau `MakePRI` pour rechercher le disque afin de trouver les ressources et les inclure dans un fichier PRI. Remplacez « contoso_demop.xml » par le nom du fichier XML que vous avez utilisé dans l’étape précédente et veillez à spécifier le répertoire parent pour l’entrée et la sortie : 

    ```CMD
    makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o
    ```

    Vous pouvez saisir `makepri new /?` pour voir ce que fait chaque paramètre, mais pour résumer :
      * `/pr` définit la racine du projet (dans ce cas, le répertoire actif)
      * `/cf` définit le nom du fichier de configuration, créé à l’étape précédente.
      * `/of` définit le fichier de sortie 
      * `/mf` crée un fichier de mappage (afin que nous puissions exclure des fichiers du package dans une étape ultérieure)
      * `/o` le définit pour remplacer le fichier de sortie s’il existe

5. Vous disposez désormais d'un fichier `.pri` avec les ressources de langue par défaut (par ex., en-US). Pour vérifier que celui-ci fonctionne correctement, vous pouvez exécuter la commande suivante :

    ```CMD
    makepri dump /if ..\resources.pri /of ..\resources /o
    ```

    Vous pouvez saisir `makepri dump /?` pour voir ce que fait chaque paramètre, mais pour résumer :
      * `/if` définit le nom du fichier d’entrée 
      * `/of` définit le nom du fichier de sortie (`.xml` sera ajouté automatiquement)
      * `/o` le définit pour remplacer le fichier de sortie s’il existe

6. Enfin, vous pouvez ouvrir `..\resources.xml` dans un éditeur de texte et vérifiez qu’il répertorie vos valeurs `<NamedResource>` (comme `ApplicationDescription` et `PublisherDisplayName`) ainsi que les valeurs `<Candidate>` pour la langue par défaut que vous avez choisie (il y aura d'autres contenus au début du fichier ; ignorez cela pour le moment).

Vous pouvez ouvrir le fichier de mappage `..\resources.map.txt` pour vérifier qu’il contient les fichiers nécessaires à votre projet (y compris le fichier PRI, qui ne fait pas partie du répertoire du projet). Plus important encore, le fichier de mappage n'inclura *pas* une référence à votre fichier `resources.resw` car le contenu de ce fichier est déjà intégré au fichier PRI. Toutefois, il contiendra d'autres ressources comme les noms de fichiers de vos images.

#### <a name="building-and-signing-the-package"></a>Création et signature du package 

Maintenant que le fichier PRI est généré, vous pouvez créer et signer le package :

1. Pour créer le package d’application, exécutez la commande suivante en remplaçant `contoso_demo.appx` par le nom du fichier. msix/. AppX que vous souhaitez créer et en veillant à choisir un autre répertoire pour le fichier (cet exemple utilise le répertoire parent ; il peut se trouver n’importe où, mais **ne doit pas** être le répertoire du projet).

    ```CMD
    makeappx pack /m AppXManifest.xml /f ..\resources.map.txt /p ..\contoso_demo.appx /o
    ```

    Vous pouvez saisir `makeappx pack /?` pour voir ce que fait chaque paramètre, mais pour résumer :
      * `/m` définit le fichier manifeste à utiliser
      * `/f` définit le fichier de mappage à utiliser (créé à l’étape précédente) 
      * `/p` définit le nom du package de sortie
      * `/o` le définit pour remplacer le fichier de sortie s’il existe

2. Une fois le package créé, il doit être signé. Le moyen le plus simple d’obtenir un certificat de signature consiste à créer un projet Windows universel vide dans Visual Studio et à copier le fichier `.pfx` qu’il crée, mais vous pouvez en créer un manuellement à l’aide des utilitaires `MakeCert` et `Pvk2Pfx`, comme décrit dans [comment créer un certificat de signature de package d’application](https://docs.microsoft.com/windows/desktop/appxpkg/how-to-create-a-package-signing-certificate).

    > [!IMPORTANT]
    > Si vous créez manuellement un certificat de signature, assurez-vous que vous placez les fichiers dans un répertoire différent de celui de votre projet source ou de votre source de package. sinon, il peut être inclus dans le package, y compris la clé privée !

3. Pour signer le package, utilisez la commande suivante. Notez que le `Publisher` spécifié dans l'élément `Identity` du `AppxManifest.xml` doit correspondre au `Subject` du certificat (il ne s’agit **pas** de l'élément `<PublisherDisplayName>`, qui est le nom complet localisé à afficher aux utilisateurs). Comme toujours, remplacez les noms de fichiers `contoso_demo...` par les noms correspondants à votre projet, et (**très important**) vérifiez que le fichier `.pfx` ne se trouve pas dans le répertoire actif (sinon, il sera créé et inclus dans votre package, y compris la clé de signature privée !) :

    ```CMD
    signtool sign /fd SHA256 /a /f ..\contoso_demo_key.pfx ..\contoso_demo.appx
    ```

    Vous pouvez saisir `signtool sign /?` pour voir ce que fait chaque paramètre, mais pour résumer :
      * `/fd` définit l’algorithme de synthèse de fichiers (SHA256 est la valeur par défaut pour. AppX)
      * `/a` sélectionne automatiquement le meilleur certificat
      * `/f` spécifie le fichier d’entrée qui contient le certificat de signature

Enfin, vous pouvez maintenant double-cliquer sur le fichier `.appx` pour l’installer, ou, si vous préférez la ligne de commande, vous pouvez ouvrir une invite PowerShell, basculer sur le répertoire contenant le package et saisir ce qui suit (en remplaçant `contoso_demo.appx` par votre nom de package) :

```CMD
add-appxpackage contoso_demo.appx
```

Si vous recevez des messages d'erreur indiquant que le certificat n'est pas approuvé, assurez-vous qu’il a été ajouté au magasin de l’ordinateur (et **non pas** au magasin de l’utilisateur). Pour ajouter le certificat au magasin de l’ordinateur, vous pouvez utiliser la ligne de commande ou l’Explorateur Windows.

Pour utiliser la ligne de commande :

1. Exécutez une invite de commandes Visual Studio 2017 ou Visual Studio 2019 en tant qu’administrateur.
2. Basculez vers le répertoire qui contient le fichier `.cer` (n’oubliez pas de vous assurer que celui-ci se trouve en dehors de vos répertoires source ou de projet !)
3. Saisissez la commande suivante, en remplaçant `contoso_demo.cer` par votre nom de fichier :
    ```CMD
    certutil -addstore TrustedPeople contoso_demo.cer
    ```
    
    Vous pouvez exécuter `certutil -addstore /?` pour voir ce que fait chaque paramètre, mais pour résumer :
      * `-addstore` ajoute un certificat à un magasin de certificats
      * `TrustedPeople` indique le magasin dans lequel le certificat est placé

Pour utiliser Windows Explorer :

1. Accédez au dossier qui contient le fichier `.pfx`
2. Double-cliquez sur le fichier `.pfx`et l'**Assistant d'importation de Certificat** doit apparaître
3. Choisissez `Local Machine`, puis cliquez sur `Next`
4. Acceptez l’invite d’élévation de l’administrateur du contrôle de compte d’utilisateur, si elle apparaît, puis cliquez sur `Next`
5. Entrez le mot de passe de la clé privée, le cas échéant, puis cliquez sur `Next`
6. Sélectionnez `Place all certificates in the following store`
7. Cliquez sur `Browse`, choisissez le dossier `Trusted People` (et **non pas** « Éditeurs approuvés »)
8. Cliquez sur `Next`, puis `Finish`

Après avoir ajouté le certificat au magasin `Trusted People`, réessayez d’installer le package.

Vous devez maintenant voir votre application apparaître dans la liste « Toutes les applications » du menu Démarrer, avec les informations appropriées extraites du fichier `.resw` / `.pri`. Si vous voyez une chaîne vide ou la chaîne `ms-resource:...`, cela signifie qu'une erreur est survenue : vérifiez vos modifications pour vous assurer qu’elles sont correctes. Si vous cliquez avec le bouton droit de la souris sur votre application dans le menu Démarrer, vous pouvez l’épingler sous forme de vignette et vérifier que les informations correctes s’y affichent également.

### <a name="step-13-add-more-supported-languages"></a>Étape 1.3 : Ajouter d’autres langues prises en charge

Une fois les modifications apportées au manifeste du package et le fichier de `resources.resw` initial créé, il est facile d’ajouter des langues supplémentaires.

#### <a name="create-additional-localized-resources"></a>Créer d’autres ressources localisées

Tout d’abord, créez les valeurs de ressources localisées supplémentaires. 

Dans le dossier `Strings`, créez des dossiers supplémentaires pour chaque langue que vous prenez en charge à l’aide du code BCP-47 approprié (par exemple, `Strings\de-DE`). Dans chacun de ces dossiers, créez un fichier `resources.resw` (à l’aide d’un éditeur XML ou du concepteur Visual Studio) qui inclut les valeurs de ressource traduites. Il est supposé que vos chaînes localisées sont d'ores et déjà disponibles quelque part, et qu'il vous suffit de les copier dans le fichier `.resw` ; ce document ne couvre pas l’étape de la traduction elle-même. 

Par exemple, le fichier `Strings\de-DE\resources.resw` peut ressembler à ceci, avec le <span style="background-color: yellow">texte mis en surbrillance</span> modifié à partir de `en-US` :

```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
  <data name="ApplicationDescription">
    <value>Contoso Demo app with localized resources (German)</value>
  </data>
  <data name="ApplicationDisplayName">
    <value>Contoso Demo Sample (German)</value>
  </data>
  <data name="PackageDisplayName">
    <value>Contoso Demo Package (German)</value>
  </data>
  <data name="PublisherDisplayName">
    <value>Contoso Samples, DE</value>
  </data>
  <data name="TileShortName">
    <value>Contoso (DE)</value>
  </data>
</root>
```

Les étapes suivantes supposent que vous avez ajouté des ressources pour `de-DE` et `fr-FR`, mais le même modèle peut être appliqué à n’importe quelle langue.

#### <a name="update-the-package-manifest-to-list-supported-languages"></a>Mettre à jour le manifeste du package pour répertorier les langues prises en charge

Le manifeste du package doit être mis à jour pour répertorier les langues prises en charge par l’application. Desktop App Converter ajoute la langue par défaut, mais les autres doivent être ajoutées explicitement. Si vous modifiez le fichier `AppxManifest.xml` directement, mettez à jour le nœud `Resources` comme suit, en ajoutant autant d’éléments que nécessaire et en remplaçant les <span style="background-color: yellow"> langues appropriées prises en charge</span>, tout en vous assurant que la première entrée dans la liste est la langue par défaut (de secours). Dans cet exemple, la valeur par défaut est l'anglais (États-Unis) avec une prise en charge supplémentaire pour l’allemand (Allemagne) et le français (France) :

```xml
<Resources>
  <Resource Language="EN-US" />
  <Resource Language="DE-DE" />
  <Resource Language="FR-FR" />
</Resources>
```

Si vous utilisez Visual Studio, aucune action n'est requise de votre part ; si vous examinez `Package.appxmanifest` vous devez voir la valeur <span style="background-color: yellow">x-generate</span> spéciale, ce qui permet au processus de génération d’insérer les langues qu’il trouve dans votre projet (sur la base des dossiers nommés avec des codes BCP-47). Notez qu’il ne s’agit pas d’une valeur valide pour un manifeste de package réel. elle fonctionne uniquement pour les projets Visual Studio :

```xml
<Resources>
  <Resource Language="x-generate" />
</Resources>
```

#### <a name="re-build-with-the-localized-values"></a>Générez à nouveau l'application avec les valeurs localisées

Vous pouvez maintenant générer et déployer votre application, à nouveau, et si vous modifiez votre préférence de langue dans Windows, vous devriez voir les valeurs qui viennent d’être localisées s’afficher dans le menu Démarrer (les instructions permettant de modifier la langue se trouvent ci-dessous).

Pour Visual Studio, vous pouvez simplement utiliser `Ctrl+Shift+B` pour générer l'application et cliquer sur le projet pour `Deploy`.

Si vous créez manuellement le projet, suivez les mêmes étapes susmentionnées, mais ajoutez des langues supplémentaires, séparées par des traits de soulignement, à la liste des qualificateurs par défaut (`/dq`) lors de la création du fichier de configuration. Par exemple, pour prendre en charge les ressources en anglais, allemand et français ajoutées à l’étape précédente :

```CMD
makepri createconfig /cf ..\contoso_demo.xml /dq en-US_de-DE_fr-FR /pv 10.0 /o
```

Cela crée un fichier PRI qui contient toutes les langues spécifiées que vous pouvez facilement utiliser en vue d'un test. Si la taille totale de vos ressources est peu importante, ou si vous ne prenez en charge qu'un nombre réduit de langues uniquement, cela peut être acceptable pour votre application d'expédition ; cela est utile si vous souhaitez uniquement bénéficier des avantages de réduction des tailles d’installation ou de téléchargement des ressources dont vous avez besoin pour effectuer le travail supplémentaire de conception de modules linguistiques distincts.

#### <a name="test-with-the-localized-values"></a>Test avec les valeurs localisées

Pour tester les nouvelles modifications localisées, ajoutez simplement une nouvelle langue d’interface utilisateur par défaut à Windows. Il est inutile de télécharger les modules linguistiques, de redémarrer le système ou que votre interface utilisateur Windows entière s'affiche dans une langue étrangère. 

1. Exécutez l'application `Settings` (`Windows + I`)
2. Accédez à `Time & language`
3. Accédez à `Region & language`
4. Cliquez sur `Add a language`
5. Saisissez (ou sélectionnez) la langue souhaitée (par ex. `Deutsch` ou `German`)
 * S’il existe des sous-langues, choisissez celle que vous souhaitez (par ex., `Deutsch / Deutschland`)
6. Sélectionnez la langue dans la liste des langues
7. Cliquez sur `Set as default`

Maintenant, ouvrez le menu Démarrer et recherchez votre application : vous verrez des valeurs localisées pour la langue sélectionnée (les autres applications peuvent également apparaître localisées). Si vous ne voyez pas le nom localisé immédiatement, patientez quelques minutes jusqu'à ce que le cache du menu Démarrer soit réactualisé. Pour revenir à votre langue maternelle, définissez-la en tant que langue par défaut dans la liste des langues. 

### <a name="step-14-localizing-more-parts-of-the-package-manifest-optional"></a>Étape 1,4 : localisation d’autres parties du manifeste du package (facultatif)

D’autres sections du manifeste de package peuvent être localisées. Par exemple, si votre application gère les extensions de fichier, le manifeste `windows.fileTypeAssociation` doit être doté d'une extension, à l’aide du <span style="background-color: lightgreen">texte mis en surbrillance en vert</span> exactement comme indiqué (dans la mesure où il fait référence à des ressources) et en remplaçant le <span style="background-color: yellow">texte surligné en jaune</span> avec des informations spécifiques à votre application :

```xml
<Extensions>
  <uap:Extension Category="windows.fileTypeAssociation">
    <uap:FileTypeAssociation Name="default">
      <uap:DisplayName>ms-resource:Resources/FileTypeDisplayName</uap:DisplayName>
      <uap:Logo>Assets\StoreLogo.png</uap:Logo>
      <uap:InfoTip>ms-resource:Resources/FileTypeInfoTip</uap:InfoTip>
      <uap:SupportedFileTypes>
        <uap:FileType ContentType="application/x-contoso">.contoso</uap:FileType>
      </uap:SupportedFileTypes>
    </uap:FileTypeAssociation>
  </uap:Extension>
</Extensions>
```

Vous pouvez également ajouter ces informations à l’aide du Concepteur de manifeste de Visual Studio, via l'onglet `Declarations`, en prenant note des <span style="background-color: lightgreen">valeurs mises en surbrillance</span> :

<p><img src="images\editing-declarations-info.png"/></p>

Ajoutez maintenant les noms de ressources correspondant à chacun de vos fichiers `.resw`, en remplaçant le <span style="background-color: yellow">texte mis en surbrillance</span> par le texte approprié à votre application (n’oubliez pas de faire cela pour *chaque langue prise en charge !* ) :

```xml
... existing content...
<data name="FileTypeDisplayName">
  <value>Contoso Demo File</value>
</data>
<data name="FileTypeInfoTip">
  <value>Files used by Contoso Demo App</value>
</data>
```

Cela apparaîtra ensuite dans certaines parties de l’interface Windows, par exemple, dans l’Explorateur de fichiers :

<p><img src="images\file-type-tool-tip.png"/></p>

Générez et testez le package comme avant, en testant les nouveaux scénarios qui doivent désormais afficher les nouvelles chaînes d’interface utilisateur.

## <a name="phase-2-use-mrt-to-identify-and-locate-resources"></a>Phase 2 : Utilisation de MRT pour identifier et localiser les ressources

La section précédente vous a montré comment utiliser MRT pour localiser le fichier manifeste de votre application afin que l’interpréteur de commandes Windows puisse afficher correctement le nom de l’application et d'autres métadonnées. Aucune modification du code n’est requise pour cet objet ; seule l’utilisation de fichiers `.resw` est requise ainsi que certains outils supplémentaires. Cette section vous montre comment utiliser MRT pour localiser les ressources dans vos formats de ressource existants, à l’aide de votre code de gestion des ressources existant, le tout avec un minimum de modifications.

### <a name="assumptions-about-existing-file-layout--application-code"></a>Hypothèses concernant la disposition de fichier et le code d'application existants

Dans la mesure où il existe de nombreuses façons de localiser vos applications de bureau Win32, ce livre blanc fera certaines hypothèses simplifiant la structure de l’application existante que vous devez mapper sur votre environnement spécifique. Vous devrez peut-être apporter quelques modifications à votre code base existant ou à la disposition des ressources pour vous conformer aux exigences de MRT et ces dernières ne sont pas, pour la majeure partie, traitées dans ce document.

#### <a name="resource-file-layout"></a>Mise en page du fichier de ressources

Cet article suppose que vos ressources localisées ont toutes les mêmes noms de fichiers (par exemple, `contoso_demo.exe.mui` ou `contoso_strings.dll` ou `contoso.strings.xml`), mais qu’elles sont placées dans des dossiers différents avec des noms BCP-47 (`en-US`, `de-DE`, etc.). Quel que soit le nombre de fichiers de ressources dont vous disposez, leurs noms, leurs formats et les API associées, etc. La seule chose qui importe est que chaque ressource *logique*ait le même nom de fichier (mais soit placée dans un autre répertoire *physique*). 

À titre de contre-exemple, si votre application utilise une structure de fichiers plate avec un seul répertoire `Resources` contenant les fichiers `english_strings.dll` et `french_strings.dll`, elle ne sera  pas mappée correctement sur MRT. Une meilleure structure serait un répertoire `Resources` avec des sous-répertoires et des fichiers `en\strings.dll` et `fr\strings.dll`. Il est également possible d’utiliser le même nom de fichier de base mais avec des qualificateurs incorporés, tel que `strings.lang-en.dll` et `strings.lang-fr.dll`, mais l’utilisation de répertoires avec des codes de langue est plus simple sur le plan conceptuel, aussi nous allons nous concentrer sur cette méthode.

>[!NOTE]
> Il est toujours possible d’utiliser le programme MRT et les avantages de l’empaquetage même si vous ne pouvez pas suivre cette Convention d’affectation de noms de fichiers. Cela nécessite simplement plus de travail.

Par exemple, l’application peut disposer d'un ensemble de commandes d’interface utilisateur personnalisées (utilisées pour les étiquettes des boutons etc.) dans un fichier texte simple nommé <span style="background-color: yellow">ui.txt</span>, figurant sous un dossier <span style="background-color: yellow">UICommands</span> :

<blockquote>
<pre>
+ ProjectRoot
|--+ Strings
|  |--+ en-US
|  |  \--- resources.resw
|  \--+ de-DE
|     \--- resources.resw
|--+ <span style="background-color: yellow">UICommands</span>
|  |--+ en-US
|  |  \--- <span style="background-color: yellow">ui.txt</span>
|  \--+ de-DE
|     \--- <span style="background-color: yellow">ui.txt</span>
|--- AppxManifest.xml
|--- ...rest of project...
</pre>
</blockquote>

#### <a name="resource-loading-code"></a>Code de chargement de ressources

Cet article suppose que, à un moment donné dans votre code, vous souhaitez localiser le fichier qui contient une ressource localisée, le charger, puis l’utiliser. Les API utilisées pour charger les ressources, les API utilisées pour extraire les ressources, etc. ne sont pas importantes. Dans le pseudo-code, il existe trois étapes :

<blockquote>
<pre>
set userLanguage = GetUsersPreferredLanguage()
set resourceFile = FindResourceFileForLanguage(MY_RESOURCE_NAME, userLanguage)
set resource = LoadResource(resourceFile) 
    
// now use 'resource' however you want
</pre>
</blockquote>

MRT requiert uniquement une modification des deux premières étapes de ce processus : déterminer les meilleures ressources candidat et comment les localiser. Vous n'avez pas besoin de modifier la façon dont vous chargez ou utilisez les ressources (bien que des fonctionnalités pour cette opération soient à disposition si vous souhaitez utiliser ces dernières).

Par exemple, l’application peut utiliser l’API Win32 `GetUserPreferredUILanguages`, la fonction CRT `sprintf`et les API Win32 `CreateFile` pour remplacer les trois fonctions de pseudo-code ci-dessus, puis analyser manuellement le fichier texte à la recherche de paires `name=value`. (Les détails ne sont pas importants ; il s’agit simplement d’illustrer le fait que MRT n’a aucun impact sur les techniques utilisées pour gérer les ressources une fois qu’elles ont été détectées).

### <a name="step-21-code-changes-to-use-mrt-to-locate-files"></a>Étape 2.1 : Modifications apportées à l'utilisation de MRT pour localiser les fichiers

Il est très simple de basculer sur votre code pour utiliser MRT afin de rechercher des ressources. Cela nécessite d'utiliser quelques types WinRT et certaines lignes de code. Les principaux types que vous utiliserez sont les suivants :

* [ResourceContext](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceContext), qui englobe l’ensemble des valeurs de qualificateur (langue, facteur d’échelle, etc.) en cours
* [ResourceManager](https://docs.microsoft.com/uwp/api/windows.applicationmodel.resources.core.resourcemanager) (la version WinRT, pas la version de .NET), qui permet d’accéder à toutes les ressources à partir du fichier PRI
* [ResourceMap](https://docs.microsoft.com/uwp/api/windows.applicationmodel.resources.core.resourcemap), qui représente un sous-ensemble spécifique des ressources dans le fichier PRI (dans cet exemple, les ressources basées sur les fichiers par rapport aux ressources de chaîne)
* [NamedResource](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources.Core.NamedResource), qui représente une ressource logique et tous les candidats possibles
* [ResourceCandidate](https://docs.microsoft.com/uwp/api/windows.applicationmodel.resources.core.resourcecandidate), qui représente une ressource candidat concrète unique 

Dans le pseudo-code, vous corrigez un nom de fichier de ressources donné (comme `UICommands\ui.txt` dans l’exemple ci-dessus) de la manière suivante :

<blockquote>
<pre>
// Get the ResourceContext that applies to this app
set resourceContext = ResourceContext.GetForViewIndependentUse()
    
// Get the current ResourceManager (there's one per app)
set resourceManager = ResourceManager.Current
    
// Get the "Files" ResourceMap from the ResourceManager
set fileResources = resourceManager.MainResourceMap.GetSubtree("Files")
    
// Find the NamedResource with the logical filename we're looking for,
// by indexing into the ResourceMap
set desiredResource = fileResources["UICommands\ui.txt"]
    
// Get the ResourceCandidate that best matches our ResourceContext
set bestCandidate = desiredResource.Resolve(resourceContext)
   
// Get the string value (the filename) from the ResourceCandidate
set absoluteFileName = bestCandidate.ValueAsString
</blockquote>
</pre>

Veuillez noter que le code ne demande **pas** un dossier de langue spécifique - comme `UICommands\en-US\ui.txt` - même si les fichiers existent de la sorte sur le disque. Au lieu de cela, il demande le nom de fichier *logique*`UICommands\ui.txt` et utilise MRT pour rechercher le fichier sur le disque approprié dans un des répertoires de langue.

À partir d’ici, l’exemple d’application peut continuer à utiliser `CreateFile` pour charger le `absoluteFileName` et analyser les paires `name=value` exactement comme précédemment ; cette logique ne nécessite aucun changement dans l’application. Si vous écrivez en C# ou C++ / CX, le code n’est pas beaucoup plus compliqué (de plus, la plupart des variables intermédiaires peuvent être élidés) - consultez la section sur le **Chargement des ressources .NET**ci-dessous. Les applications basées C++ / WRL seront plus complexes en raison des API COM de bas niveau utilisées pour activer et appeler les API WinRT, toutefois les étapes fondamentales sont les mêmes ; consultez la section **Chargement des ressources d’interface utilisateur multilingue Win32**ci-dessous.

#### <a name="loading-net-resources"></a>Chargement des ressources .NET

Étant donné que .NET dispose d’un mécanisme intégré pour rechercher et charger des ressources (appelées « Assemblys satellites »), aucun code explicite n'est à remplacer comme dans l’exemple synthétique ci-dessus : dans .NET, vous devez simplement placer vos DLL de ressource dans les répertoires appropriés et ils seront automatiquement recherchés. Quand une application est empaquetée en tant que MSIX ou. AppX à l’aide de packs de ressources, la structure de répertoires est quelque peu différente, plutôt que de faire en sorte que les répertoires de ressources soient des sous-répertoires du répertoire principal de l’application, mais qu’ils ne soient pas du tout présents si l’utilisateur n’a pas la langue indiquée dans ses préférences). 

Par exemple, imaginons une application .NET avec la disposition suivante, où tous les fichiers existent sous le dossier `MainApp` :

<blockquote>
<pre>
+ MainApp
|--+ en-us
|  \--- MainApp.resources.dll
|--+ de-de
|  \--- MainApp.resources.dll
|--+ fr-fr
|  \--- MainApp.resources.dll
\--- MainApp.exe
</pre>
</blockquote>

Après la conversion en. AppX, la disposition ressemblera à ce qui suit, en supposant `en-US` était la langue par défaut et que l’utilisateur a à la fois l’allemand et le français répertoriés dans leur liste de langues :

<blockquote>
<pre>
+ WindowsAppsRoot
|--+ MainApp_neutral
|  |--+ en-us
|  |  \--- <span style="background-color: yellow">MainApp.resources.dll</span>
|  \--- MainApp.exe
|--+ MainApp_neutral_resources.language_de
|  \--+ de-de
|     \--- <span style="background-color: yellow">MainApp.resources.dll</span>
\--+ MainApp_neutral_resources.language_fr
   \--+ fr-fr
      \--- <span style="background-color: yellow">MainApp.resources.dll</span>
</pre>
</blockquote>

Étant donné que les ressources localisées n’existent plus dans les sous-répertoires sous l’emplacement d’installation de l’exécutable principal, la résolution de ressource intégrée .NET échoue. Heureusement, .NET est doté d'un mécanisme bien défini pour la gestion des tentatives de charge assembly ayant échoué - l'événement `AssemblyResolve`. Une application .NET à l’aide de MRT doit s’inscrire pour cet événement et fournir l’assembly manquant pour le sous-système de ressource .NET. 

Voici un exemple concis montrant comment utiliser les API WinRT pour localiser les assemblys satellite utilisés par .NET : le code présenté comme tel est compressé intentionnellement pour afficher une implémentation minimale, bien que vous puissiez voir qu’il correspond étroitement au pseudo-code ci-dessus, le `ResolveEventArgs` fournissant le nom de l’assembly dont nous avons besoin pour localiser. Vous trouverez une version exécutable de ce code (avec les commentaires détaillés et la gestion des erreurs) dans le fichier `PriResourceRsolver.cs` dans [les exemples **programmes de résolution d’Assembly .NET** sur GitHub](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DotNetSatelliteAssemblyDemo).

```csharp
static class PriResourceResolver
{
  internal static Assembly ResolveResourceDll(object sender, ResolveEventArgs args)
  {
    var fullAssemblyName = new AssemblyName(args.Name);
    var fileName = string.Format(@"{0}.dll", fullAssemblyName.Name);

    var resourceContext = ResourceContext.GetForViewIndependentUse();
    resourceContext.Languages = new[] { fullAssemblyName.CultureName };

    var resource = ResourceManager.Current.MainResourceMap.GetSubtree("Files")[fileName];

    // Note use of 'UnsafeLoadFrom' - this is required for apps installed with .appx, but
    // in general is discouraged. The full sample provides a safer wrapper of this method
    return Assembly.UnsafeLoadFrom(resource.Resolve(resourceContext).ValueAsString);
  }
}
```

Étant donné la classe ci-dessus, vous ajoutez le code suivant au début du code de démarrage de votre application (avant de devoir charger les ressources localisées) :

```csharp
void EnableMrtResourceLookup()
{
  AppDomain.CurrentDomain.AssemblyResolve += PriResourceResolver.ResolveResourceDll;
}
```

L’exécution .NET déclenchera l’événement `AssemblyResolve` chaque fois qu’elle ne parvient pas à trouver les DLL de ressource ; à ce moment-là, le gestionnaire d’événement fourni localisera le fichier souhaité via MRT et retournera l’assembly.

> [!NOTE]
> Si votre application a déjà un gestionnaire de `AssemblyResolve` à d’autres fins, vous devrez intégrer le code de résolution des ressources avec votre code existant.

#### <a name="loading-win32-mui-resources"></a>Chargement des ressources de l’interface utilisateur multilingue Win32

Le chargement des ressources d’interface utilisateur multilingue Win32 est un processus quasiment identique au chargement d’assemblys Satellite de l'environnement .NET, à ceci près qu'il utilise un code + C c++ / CX ou C++ / WRL à la place. C++ / CX permet de créer un code beaucoup plus simple, qui est étroitement mis en correspondance avec le code C# ci-dessus, mais utilise les extensions de langage C++, les commutateurs de compilation et requiert une exécution plus longue que vous souhaiterez peut-être éviter. Si tel est le cas, C++ / WRL fournit une solution doté d'un impact beaucoup faible avec plus de code en clair. Néanmoins, si vous êtes familiarisé avec la programmation ATL (ou COM en général), vous devriez être à l'aise avec WRL. 

L’exemple de fonction suivant montre comment utiliser C++ / WRL pour charger une DLL de ressource spécifique et renvoyer une `HINSTANCE` qui peut être utilisée pour charger davantage de ressources à l’aide des API de ressource Win32 habituelles. Notez que, contrairement à l'exemple C#, qui initialise explicitement le `ResourceContext` avec la langue demandée par l'exécution .NET, ce code s’appuie sur langue actuelle de l’utilisateur.

```cpp
#include <roapi.h>
#include <wrl\client.h>
#include <wrl\wrappers\corewrappers.h>
#include <Windows.ApplicationModel.resources.core.h>
#include <Windows.Foundation.h>
   
#define IF_FAIL_RETURN(hr) if (FAILED((hr))) return hr;
    
HRESULT GetMrtResourceHandle(LPCWSTR resourceFilePath,  HINSTANCE* resourceHandle)
{
  using namespace Microsoft::WRL;
  using namespace Microsoft::WRL::Wrappers;
  using namespace ABI::Windows::ApplicationModel::Resources::Core;
  using namespace ABI::Windows::Foundation;
    
  *resourceHandle = nullptr;
  HRESULT hr{ S_OK };
  RoInitializeWrapper roInit{ RO_INIT_SINGLETHREADED };
  IF_FAIL_RETURN(roInit);
    
  // Get Windows.ApplicationModel.Resources.Core.ResourceManager statics
  ComPtr<IResourceManagerStatics> resourceManagerStatics;
  IF_FAIL_RETURN(GetActivationFactory(
    HStringReference(
    RuntimeClass_Windows_ApplicationModel_Resources_Core_ResourceManager).Get(),
    &resourceManagerStatics));
    
  // Get .Current property
  ComPtr<IResourceManager> resourceManager;
  IF_FAIL_RETURN(resourceManagerStatics->get_Current(&resourceManager));
    
  // get .MainResourceMap property
  ComPtr<IResourceMap> resourceMap;
  IF_FAIL_RETURN(resourceManager->get_MainResourceMap(&resourceMap));
    
  // Call .GetValue with supplied filename
  ComPtr<IResourceCandidate> resourceCandidate;
  IF_FAIL_RETURN(resourceMap->GetValue(HStringReference(resourceFilePath).Get(),
    &resourceCandidate));
    
  // Get .ValueAsString property
  HString resolvedResourceFilePath;
  IF_FAIL_RETURN(resourceCandidate->get_ValueAsString(
    resolvedResourceFilePath.GetAddressOf()));
    
  // Finally, load the DLL and return the hInst.
  *resourceHandle = LoadLibraryEx(resolvedResourceFilePath.GetRawBuffer(nullptr),
    nullptr, LOAD_LIBRARY_AS_DATAFILE | LOAD_LIBRARY_AS_IMAGE_RESOURCE);
    
  return S_OK;
}
```

## <a name="phase-3-building-resource-packs"></a>Phase 3 : Création de packages de ressources

Maintenant que vous avez un « fat pack » qui contient toutes les ressources, il existe deux manières de créer un package principal et des packages de ressources distincts afin de réduire les tailles de téléchargement et d'installation :

* Utilisez un fat pack existant, exécutez-le par le biais de [l’outil Générateur de lot](https://www.microsoft.com/store/apps/9nblggh43pmq) pour créer automatiquement des packs de ressources. Il s’agit de l’approche à privilégier si vous disposez d’un système de génération qui produit déjà un fat pack que vous souhaitez post-traiter afin de générer les packs de ressources.
* Produire directement les packages de ressource individuels et les intégrer dans un lot. Il s’agit de l’approche à privilégier si vous avez davantage de contrôle sur votre système de génération et que vous pouvez créer les packages directement.

### <a name="step-31-creating-the-bundle"></a>Étape 3.1 : Création du lot

#### <a name="using-the-bundle-generator-tool"></a>À l’aide de l’outil de génération de lot

Pour pouvoir utiliser l’outil de génération de lot, le fichier de configuration PRI créé pour le package doit être mis à jour manuellement pour supprimer la section `<packaging>`.

Si vous utilisez Visual Studio, reportez-vous à la rubrique pour vous [assurer que les ressources sont installées sur un appareil, qu’un appareil en ait besoin](https://docs.microsoft.com/previous-versions/dn482043(v=vs.140)) pour obtenir des informations sur la façon de générer toutes les langues dans le package principal en créant les fichiers `priconfig.packaging.xml` et `priconfig.default.xml`.

Si vous modifiez manuellement les fichiers, procédez comme suit : 

1. Créez le fichier de configuration de la même manière que précédemment, en renseignant le chemin d’accès correct, le nom de fichier et les langues :

    ```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US_de-DE_es-MX /pv 10.0 /o
    ```

2. Ouvrez manuellement le fichier `.xml` créé et supprimez l’intégralité de la section `&lt;packaging&rt;` (mais conservez tout le reste intact) :

    ```xml
    <?xml version="1.0" encoding="UTF-8" standalone="yes" ?> 
    <resources targetOsVersion="10.0.0" majorVersion="1">
      <!-- Packaging section has been deleted... -->
      <index root="\" startIndexAt="\">
        <default>
        ...
        ...
    ```

3. Générez le fichier `.pri` et le package `.appx` comme avant, à l’aide du fichier de configuration à jour, du répertoire approprié du package et des fichiers des noms (voir ci-dessus pour plus d’informations sur ces commandes) :

    ```CMD
    makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o
    makeappx pack /m AppXManifest.xml /f ..\resources.map.txt /p ..\contoso_demo.appx /o
    ```

4. Une fois le package créé, utilisez la commande suivante pour créer le bundle, à l’aide des noms de répertoire et de fichier appropriés :

    ```CMD
    BundleGenerator.exe -Package ..\contoso_demo.appx -Destination ..\bundle -BundleName contoso_demo
    ```

Vous pouvez maintenant passer à l’étape finale, signature (voir ci-dessous).

#### <a name="manually-creating-resource-packages"></a>Création manuelle de packages de ressources

La création manuelle de packages de ressources nécessite l’exécution d'un ensemble de commandes légèrement différent pour créer des fichiers `.pri` et `.appx` distincts ; ces derniers sont tous semblables aux commandes utilisées précédemment, par conséquent seules des explications minimales sont fournies. Remarque : toutes les commandes supposent que le répertoire actif est le répertoire contenant le fichier `AppXManifest.xml`, mais que tous les fichiers sont placés dans le répertoire parent (vous pouvez utiliser un autre répertoire, si nécessaire, mais vous ne devez pas polluer le répertoire du projet avec l'un de ces fichiers). Comme toujours, remplacez les noms de fichier « Contoso » par vos propres noms de fichiers.

1. Utilisez la commande suivante pour créer un fichier de configuration qui nomme **uniquement** la langue par défaut en tant que qualificateur par défaut - dans ce cas, `en-US` :

    ```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US /pv 10.0 /o
    ```

2. Créez un fichier par défaut `.pri` et `.map.txt` pour le package principal, ainsi qu’un ensemble supplémentaire de fichiers pour chaque langue trouvée dans votre projet, avec la commande suivante :

    ```CMD
    makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o
    ```

3. Utilisez la commande suivante pour créer le package principal (qui contient les ressources de code exécutable et les langues par défaut). Comme toujours, renommez le fichier comme vous le souhaitez, bien que vous deviez placer le package dans un répertoire séparé pour faciliter la création du lot ultérieurement (cet exemple utilise le répertoire `..\bundle`) :

    ```CMD
    makeappx pack /m .\AppXManifest.xml /f ..\resources.map.txt /p ..\bundle\contoso_demo.main.appx /o
    ```

4. Une fois le package principal créé, utilisez la commande suivante une fois pour chaque langue supplémentaire (c'est-à-dire, répétez cette commande pour chaque fichier de mappage de langue généré à l’étape précédente). Là encore, la sortie doit se trouver dans un répertoire séparé (le même que celui du package principal). Notez que le langue est spécifiée **à la fois** dans l'option `/f` et dans l'option `/p` et utilisez le nouvel argument `/r` (qui indique qu'un package de ressources est requis) :

    ```CMD
    makeappx pack /r /m .\AppXManifest.xml /f ..\resources.language-de.map.txt /p ..\bundle\contoso_demo.de.appx /o
    ```

5. Combinez tous les packages à partir du répertoire de lot dans un seul fichier `.appxbundle`. La nouvelle option `/d` spécifie le répertoire à utiliser pour tous les fichiers dans le lot (c’est pourquoi les fichiers `.appx` sont placés dans un répertoire séparé à l’étape précédente) :

    ```CMD
    makeappx bundle /d ..\bundle /p ..\contoso_demo.appxbundle /o
    ```

La dernière étape de la création du package consiste à signer.

### <a name="step-32-signing-the-bundle"></a>Étape 3.2 : Signature du lot

Une fois que vous avez créé le fichier `.appxbundle` (soit par le biais de l’outil de génération de lot, soit manuellement) vous disposerez d'un seul fichier qui contiendra le package principal, ainsi que tous les packages de ressources. La dernière étape consiste à signer le fichier afin que Windows l'installe :

```CMD
signtool sign /fd SHA256 /a /f ..\contoso_demo_key.pfx ..\contoso_demo.appxbundle
```

Cela produira un fichier `.appxbundle` signé qui contiendra le package principal, ainsi que tous les packages de ressources linguistiques. Vous pouvez double-cliquer dessus, comme un fichier de package, pour installer l’application, ainsi que toutes les langues appropriées, en fonction des préférences de langue Windows de l'utilisateur.

## <a name="related-topics"></a>Rubriques connexes

* [Personnaliser vos ressources pour la langue, l’échelle, le contraste élevé et d’autres qualificateurs](tailor-resources-lang-scale-contrast.md)