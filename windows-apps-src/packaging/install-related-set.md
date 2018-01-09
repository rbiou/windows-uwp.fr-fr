---
author: laurenhughes
title: "Installer un ensemble connexe à l’aide d’un fichier du Programme d’installation d’application"
description: "Dans cette section, nous allons examiner les étapes à suivre pour autoriser l’installation d’un ensemble connexe via le Programme d’installation d’application. Nous effectuerons également les étapes nécessaires pour créer un fichier *.appinstaller qui définira votre ensemble connexe."
ms.author: lahugh
ms.date: 10/10/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows10, uwp, programme d’installation d’application, appinstaller, charger une version test, ensemble connexe, packages facultatifs"
ms.localizationpriority: medium
ms.openlocfilehash: 484d69f5f9f0b15b765f6f92eaad787af37134cf
ms.sourcegitcommit: f9a4854b6aecfda472fb3f8b4a2d3b271b327800
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="install-a-related-set-using-an-app-installer-file"></a>Installer un ensemble connexe à l’aide d’un fichier du Programme d’installation d’application

Si vous commencez juste à utiliser des packages facultatifs UWP ou des ensembles connexes, les articles suivants constituent de bonnes ressources pour commencer. 

1.  [Étendre votre application à l’aide de packages facultatifs](https://blogs.msdn.microsoft.com/appinstaller/2017/04/05/uwpoptionalpackages/)
2.  [Créer votre premier package facultatif](https://blogs.msdn.microsoft.com/appinstaller/2017/05/09/build-your-first-optional-package/)
3.  [Code de chargement à partir d’un package facultatif](https://blogs.msdn.microsoft.com/appinstaller/2017/05/11/loading-code-from-an-optional-package/)
4.  [Outils pour créer un ensemble connexe](https://blogs.msdn.microsoft.com/appinstaller/2017/05/12/tooling-to-create-a-related-set/)
5.  [Packages facultatifs et création d’ensembles connexes](https://docs.microsoft.com/windows/uwp/packaging/optional-packages)

Avec Windows10FallCreatorsUpdate, les ensembles connexes peuvent désormais être installés via le Programme d’installation d’application. Cela permet la distribution et le déploiement de packages d’application d'ensembles connexes pour les utilisateurs. 

## <a name="how-to-install-a-related-set"></a>Comment installer un ensemble connexe 
Un ensemble connexe n’est pas une entité, mais plutôt une combinaison d’un package principal et de packages facultatifs. Pour être en mesure d’installer un ensemble connexe comme une seule entité, nous devons être en mesure de spécifier le package principal et le package facultatif comme un seul package. Pour ce faire, nous devons créer un fichier XML avec une extension «.appinstaller» pour définir un ensemble connexe. Le Programme d’installation d’application utilise le fichier *.appinstaller et autorise l’utilisateur à installer tous les packages définis d’un seul clic. 

Avant d’entrer dans les détails, voici un exemple complet de fichier *.appinstaller:

```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017"
    Version="1.0.0.0" 
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" > 
   
    <MainBundle 
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.appxbundle" />
        
    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.appxbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.appxbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.appx" />

    </OptionalPackages>

</AppInstaller>
```

Au cours du déploiement, le fichier du Programme d’installation d’application est validé par rapport aux packages de l’application référencés dans l’élément `Uri`. Par conséquent, `Name`, `Publisher` et `Version` doivent correspondre à l’élément [Package/Identity](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) dans le manifeste du package de l’application. 

## <a name="how-to-create-an-app-installer-file"></a>Comment créer un fichier du Programme d’installation d’application 

Pour distribuer votre ensemble connexe comme une seule entité, vous devez créer un fichier du Programme d’installation d’application qui contient les éléments requis par ce [schéma appinstaller](https://docs.microsoft.com/uwp/schemas/appinstallerschema/app-installer-file).

### <a name="step-1-create-the-appinstaller-file"></a>Étape1: Créer le fichier *.appinstaller
À l’aide d’un éditeur de texte, créez un fichier (qui contiendra le code XML) et nommez-le &lt;nomdefichier&gt;.appinstaller 

### <a name="step-2-add-the-basic-template"></a>Étape2: Ajouter le modèle de base
Le modèle de base comprend les informations du fichier du Programme d’installation d’application. 
```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017"
    Version="1.0.0.0" 
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" > 
</AppInstaller>
```

### <a name="step-3-add-the-main-package-information"></a>Étape3: Ajouter les informations du package principal 
Si le package principal de l’application est un fichier .appxbundle, utilisez alors `<MainBundle>` illustré ci-dessous. Si le package principal de l’application est un fichier .appx, utilisez alors `<MainPackage>` à la place de `<MainBundle>` dans l’extrait de code. 

```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017"
    Version="1.0.0.0" 
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" > 
   
    <MainBundle 
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.appxbundle" />

</AppInstaller>
```
Les informations contenues dans l’attribut `<MainBundle>` ou `<MainPackage>` doivent correspondre à l’élément [Package/Identity](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) dans le manifeste de l'ensemble d'applications ou dans le manifeste du package de l’application. 

### <a name="step-4-add-the-optional-packages"></a>Étape4: Ajouter les packages facultatifs 
Similaire à l’attribut du package principal de l’application, si le package facultatif peut être soit un package de l’application, soit un ensemble d’applications, l’élément enfant au sein de l’attribut `<OptionalPackages>` doit être `<Package>` ou `<Bundle>` respectivement. Les informations du package dans les éléments enfants doivent correspondre à l’élément d’identité dans le manifeste de l’ensemble d’applications ou du package. 

``` xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017"
    Version="1.0.0.0" 
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" > 
   
    <MainBundle 
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.appxbundle" />
        
    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.appxbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.appxbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.appx" />

    </OptionalPackages>

</AppInstaller>
```

### <a name="step-5-add-dependencies"></a>Étape5: Ajouter des dépendances 
Dans l’élément de dépendances, vous pouvez spécifier les packages d’infrastructure requis pour le package principal ou les packages facultatifs. 

``` xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017"
    Version="1.0.0.0" 
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" > 
   
    <MainBundle 
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.appxbundle" />
        
    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.appxbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.appxbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            ProcessorArchitecture="x86"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.appx" />

    </OptionalPackages>

    <Dependencies>
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x86" Uri="http://foobarbaz.com/fwkx86.appx" />
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x64" Uri="http://foobarbaz.com/fwkx64.appx" />
    </Dependencies>

</AppInstaller>
```

### <a name="step-6-add-update-setting"></a>Étape6: Ajouter le paramètre de mise à jour 
Le fichier du Programme d'installation d'application permet également de spécifier le paramètre de mise à jour afin que les ensembles connexes puissent être automatiquement mis à jour lorsqu’un fichier du Programme d’installation d’application plus récent est publié. **<UpdateSettings>** est un élément facultatif. 
``` xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017"
    Version="1.0.0.0" 
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" > 
   
    <MainBundle 
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.appxbundle" />
        
    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.appxbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.appxbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            ProcessorArchitecture="x86"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.appx" />

    </OptionalPackages>

    <Dependencies>
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x86" Uri="http://foobarbaz.com/fwkx86.appx" />
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x64" Uri="http://foobarbaz.com/fwkx64.appx" />
    </Dependencies>
    
    <UpdateSettings>
        <OnLaunch />
    </UpdateSettings>

</AppInstaller>
```

Pour obtenir tous les détails sur le schéma XML, voir [Informations de référence sur le fichier du Programme d’installation d’application](https://docs.microsoft.com/uwp/schemas/appinstallerschema/app-installer-file).

> [!NOTE]
> 
> Le type du fichier du Programme d’installation d’application est nouveau dans Windows10 FallCreatorsUpdate. Il n’existe aucune prise en charge pour le déploiement des applicationsUWP utilisant un fichier du Programme d’installation d’application sur les versions précédentes de Windows10. 