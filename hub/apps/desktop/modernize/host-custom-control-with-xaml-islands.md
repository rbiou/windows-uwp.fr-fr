---
description: Cet article explique comment héberger un contrôle UWP personnalisé dans une application WPF à l’aide d’îlots XAML.
title: Héberger un contrôle UWP personnalisé dans une application WPF avec XAML Islands
ms.date: 01/24/2020
ms.topic: article
keywords: windows 10, uwp, formulaires windows, wpf, xaml islands, contrôles personnalisés, contrôles utilisateur, contrôles hôtes
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 5f3e4eee486edd47901fc2b97a6e10c880cb04b1
ms.sourcegitcommit: 2571af6bf781a464a4beb5f1aca84ae7c850f8f9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/30/2020
ms.locfileid: "82606298"
---
# <a name="host-a-custom-uwp-control-in-a-wpf-app-using-xaml-islands"></a>Héberger un contrôle UWP personnalisé dans une application WPF avec XAML Islands

Cet article montre comment utiliser le contrôle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) du Kit de ressources Communauté Windows pour héberger un contrôle UWP personnalisé dans une application WPF qui cible .NET Core 3. Le contrôle personnalisé contient plusieurs contrôles UWP internes du SDK Windows et lie une propriété de l’un des contrôles UWP à une chaîne dans l’application WPF. Cet article montre également comment héberger un contrôle UWP à partir de la [bibliothèque WinUI](https://docs.microsoft.com/uwp/toolkits/winui/).

Bien que cet article montre comment effectuer cette opération dans une application WPF, le processus est similaire pour une application Windows Forms. Pour une vue d’ensemble de l’hébergement de contrôles UWP dans des applications WPF et Windows Forms, consultez [cet article](xaml-islands.md#wpf-and-windows-forms-applications).

## <a name="required-components"></a>Composants requis

Pour héberger un contrôle UWP Personnalisé dans une application WPF (ou Windows Forms), vous avez besoin des composants suivants dans votre solution. Cet article fournit des instructions sur la création de chacun de ces composants.

* **Le projet et le code source de votre application**. L’utilisation du contrôle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) pour héberger des contrôles UWP personnalisés est uniquement prise en charge dans les applications ciblant .NET Core 3. Ce scénario n’est pas pris en charge dans les applications qui ciblent le .NET Framework.

* **Le contrôle UWP personnalisé**. Vous aurez besoin du code source du contrôle UWP personnalisé que vous souhaitez héberger pour pouvoir le compiler avec votre application. En général, le contrôle personnalisé est défini dans un projet de bibliothèque de classes UWP que vous référencez dans la même solution que votre projet WPF ou Windows Forms.

* **Un projet d’application UWP qui définit une classe Application racine dérivant de XamlApplication**. Votre projet WPF ou Windows Forms doit avoir accès à une instance de la classe [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) fournie par le Windows Community Toolkit afin qu’il puisse découvrir et charger des contrôles XAML UWP personnalisés. À cette fin, la méthode recommandée consiste à définir cet objet dans un projet d’application UWP distinct qui fait partie de la solution pour votre application WPF ou Windows Forms. 

    > [!NOTE]
    > Votre solution ne peut contenir qu’un seul projet qui définit un objet `XamlApplication`. Tous les contrôles UWP personnalisés de votre application partagent le même objet `XamlApplication`. Le projet qui définit l’objet `XamlApplication` doit inclure des références à tous les autres projets et bibliothèques UWP utilisés pour héberger les contrôles UWP sur l’îlot XAML.

## <a name="create-a-wpf-project"></a>Créer un projet WPF

Avant de commencer, suivez ces instructions pour créer un projet WPF et le configurer pour héberger des îlots XAML. Si vous disposez déjà d’un projet WPF, vous pouvez adapter ces étapes et exemples de code à votre projet.

> [!NOTE]
> Si vous avez un projet existant qui cible le .NET Framework, vous devez migrer votre projet vers .NET Core 3. Pour plus d’informations, consultez [cette série de publications de blog](https://devblogs.microsoft.com/dotnet/migrating-a-sample-wpf-app-to-net-core-3-part-1/).

1. Si vous ne l’avez pas déjà fait, installez la dernière version du [Kit de développement logiciel (SDK) .NET Core 3](https://dotnet.microsoft.com/download/dotnet-core/3.0).

2. Dans Visual Studio 2019, créez un projet **Application WPF (.NET Core)** .

3. Assurez-vous que les [références de package](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files) sont activées :

    1. Dans Visual Studio, cliquez sur **Outils -> Gestionnaire de package NuGet-> Paramètres du Gestionnaire de package**.
    2. Assurez-vous que **PackageReference** est sélectionné pour **Format de gestion de package par défaut**.

4. Cliquez avec le bouton droit sur votre projet WPF dans l’**Explorateur de solutions**, puis choisissez **Gérer les packages NuGet**.

5. Dans la fenêtre **Gestionnaire de package NuGet**, assurez-vous que l’option **Inclure la version préliminaire** est activée.

6. Sélectionnez l’onglet **Parcourir**, recherchez le package [Microsoft.Toolkit.Wpf.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost) (version v6.0.0 ou ultérieure), puis installez le package. Ce package fournit tout ce dont vous avez besoin pour utiliser le contrôle **WindowsXamlHost** afin d’héberger un contrôle UWP, y compris d’autres packages NuGet associés.
    > [!NOTE]
    > Les applications Windows Forms doivent utiliser le package [Microsoft.Toolkit.Forms.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost) (version v 6.0.0 ou ultérieure).

7. Configurez votre solution afin qu’elle cible une plateforme spécifique, telle que x86 ou x64. Les contrôles UWP personnalisés ne sont pas pris en charge dans les projets qui ciblent **N’importe quelle UC**.

    1. Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le nœud de la solution, puis sélectionnez **Propriétés** -> **Propriétés de la configuration** -> **Gestionnaire de configurations**.
    2. Sous **Plateforme de la solution active**, sélectionnez **Nouveau**. 
    3. Dans la boîte de dialogue **Nouvelle plateforme de solution**, sélectionnez **x64** ou **x86**, puis appuyez sur **OK**. 
    4. Fermez les boîtes de dialogue ouvertes.

## <a name="define-a-xamlapplication-class-in-a-uwp-app-project"></a>Définir une classe XamlApplication dans un projet d’application UWP

Ensuite, ajoutez un projet d’application UWP à votre solution et modifiez la classe `App` par défaut de ce projet afin de la dériver de la classe [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) fournie par le Windows Community Toolkit. Cette classe prend en charge l’interface [IXamlMetadaraProvider](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Markup.IXamlMetadataProvider), qui permet à votre application de découvrir et de charger des métadonnées pour les contrôles XAML UWP personnalisés dans les assemblys du répertoire actif de votre application au moment de l’exécution. Cette classe initialise également le framework XAML UWP pour le thread actuel. 

1. Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le nœud de la solution, puis sélectionnez **Ajouter** -> **Nouveau projet**.
2. Ajoutez un projet **Application vide (Windows universelle)** à votre solution. Assurez-vous que la version cible et la version minimale sont toutes les deux définies sur **Windows 10, version 1903** ou ultérieure.
3. Dans le projet d’application UWP, installez le package NuGet [Microsoft.Toolkit.Win32.UI.XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) (version v6.0.0 ou ultérieure).
4. Ouvrez le fichier **App.xaml** et remplacez le contenu de ce fichier par le code XAML suivant. Remplacez `MyUWPApp` par l’espace de noms de votre projet d’application UWP.

    ```xml
    <xaml:XamlApplication
        x:Class="MyUWPApp.App"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:xaml="using:Microsoft.Toolkit.Win32.UI.XamlHost"
        xmlns:local="using:MyUWPApp">
    </xaml:XamlApplication>
    ```

5. Ouvrez le fichier **App.xaml.cs** et remplacez le contenu de ce fichier par le code suivant. Remplacez `MyUWPApp` par l’espace de noms de votre projet d’application UWP.

    ```csharp
    namespace MyUWPApp
    {
        public sealed partial class App : Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication
        {
            public App()
            {
                this.Initialize();
            }
        }
    }
    ```

6. Supprimez le fichier **MainPage.xaml** du projet d’application UWP.
7. Nettoyez le projet d’application UWP, puis générez-le.
8. Dans votre projet WPF, cliquez avec le bouton droit sur le nœud **Dépendances** et ajoutez une référence à votre projet d’application UWP.

## <a name="instantiate-the-xamlapplication-object-in-the-entry-point-of-your-wpf-app"></a>Instancier l’objet XamlApplication dans le point d’entrée de votre application WPF

Ensuite, ajoutez du code au point d’entrée de votre application WPF pour créer une instance de la classe `App` que vous venez de définir dans le projet UWP (il s’agit de la classe qui dérive maintenant de `XamlApplication`).

1. Dans votre projet WPF, cliquez avec le bouton droit sur le nœud du projet, sélectionnez **Ajouter** -> **Nouvel élément**, puis sélectionnez **Classe**. Nommez la classe **Program**, puis cliquez sur **Ajouter**.

2. Remplacez la classe `Program` générée par le code suivant, puis enregistrez le fichier. Remplacez `MyUWPApp` par l’espace de noms de votre projet d’application UWP, puis remplacez `MyWPFApp` par l’espace de noms de votre projet d’application WPF.

    ```csharp
    public class Program
    {
        [System.STAThreadAttribute()]
        public static void Main()
        {
            using (new MyUWPApp.App())
            {
                MyWPFApp.App app = new MyWPFApp.App();
                app.InitializeComponent();
                app.Run();
            }
        }
    }
    ```

3. Cliquez avec le bouton droit sur le nœud du projet, puis choisissez **Propriétés**.

4. Sous l’onglet **Application** des propriétés, cliquez sur la liste déroulante **Objet de démarrage** et choisissez le nom qualifié complet de la classe `Program` que vous avez ajoutée à l’étape précédente. 
    > [!NOTE]
    > Par défaut, les projets WPF définissent une fonction de point d’entrée `Main` dans un fichier de code généré qui n’est pas destiné à être modifié. Cette étape remplace le point d’entrée de votre projet par la méthode `Main` de la nouvelle classe `Program`, ce qui vous permet d’ajouter du code qui s’exécute aussi tôt que possible dans le processus de démarrage de l’application. 

5. Enregistrez les modifications apportées aux propriétés du projet.

## <a name="create-a-custom-uwp-control"></a>Créer un contrôle UWP personnalisé

Pour héberger un contrôle UWP personnalisé dans votre application WPF, vous devez disposer du code source du contrôle pour pouvoir le compiler avec votre application. En général, les contrôles personnalisés sont définis dans un projet de bibliothèque de classe UWP pour faciliter la portabilité.

Dans cette section, vous allez définir un contrôle UWP personnalisé simple dans un nouveau projet de bibliothèque de classe. Vous pouvez également définir le contrôle UWP personnalisé dans le projet d’application UWP que vous avez créé dans la section précédente. Toutefois, ces étapes sont effectuées dans un projet de bibliothèque de classe distinct à des fins d’illustration, car il s’agit généralement de la façon dont les contrôles personnalisés sont implémentés pour la portabilité.

Si vous disposez déjà d’un contrôle personnalisé, vous pouvez l’utiliser à la place du contrôle présenté ici. Toutefois, vous devrez quand même configurer le projet qui contient le contrôle, comme indiqué dans ces étapes.

1. Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le nœud de la solution, puis sélectionnez **Ajouter** -> **Nouveau projet**.
2. Ajoutez un projet **Bibliothèque de classe (Windows universelle)** à votre solution. Assurez-vous que la version cible et la version minimale sont toutes les deux définies sur **Windows 10, version 1903** ou ultérieure.
3. Cliquez avec le bouton droit sur le fichier projet, puis sélectionnez **Décharger le projet**. Cliquez à nouveau avec le bouton droit sur le fichier projet, puis sélectionnez **Modifier**.
4. Avant l’élément `</Project>` de fermeture, ajoutez le code XML suivant pour désactiver plusieurs propriétés, puis enregistrez le fichier projet. Ces propriétés doivent être activées pour héberger le contrôle UWP personnalisé dans une application WPF (ou Windows Forms).

    ```xml
    <PropertyGroup>
      <EnableTypeInfoReflection>false</EnableTypeInfoReflection>
      <EnableXBindDiagnostics>false</EnableXBindDiagnostics>
    </PropertyGroup>
    ```

5. Cliquez avec le bouton droit sur le fichier projet, puis sélectionnez **Recharger le projet**.
6. Supprimez le fichier **Class1.cs** par défaut et ajoutez un nouvel élément **Contrôle utilisateur** au projet.
7. Dans le fichier XAML du contrôle utilisateur, ajoutez l’élément `StackPanel` suivant comme enfant de l’élément `Grid`par défaut. Cet exemple ajoute un contrôle ``TextBlock``, puis lie l’attribut ``Text`` de ce contrôle au champ ``XamlIslandMessage``.

    ```xml
    <StackPanel Background="LightCoral">
        <TextBlock>This is a simple custom UWP control</TextBlock>
        <Rectangle Fill="Blue" Height="100" Width="100"/>
        <TextBlock Text="{x:Bind XamlIslandMessage}" FontSize="50"></TextBlock>
    </StackPanel>
    ```

8. Dans le fichier code-behind du contrôle utilisateur, ajoutez le champ `XamlIslandMessage` à la classe de contrôle utilisateur, comme indiqué ci-dessous.

    ```csharp
    public sealed partial class MyUserControl : UserControl
    {
        public string XamlIslandMessage { get; set; }

        public MyUserControl()
        {
            this.InitializeComponent();
        }
    }
    ```

9. Générez le projet de bibliothèque de classes UWP.
10. Dans votre projet WPF, cliquez avec le bouton droit sur le nœud **Dépendances** et ajoutez une référence à votre projet de bibliothèque de classe UWP.
11. Dans le projet d’application UWP que vous avez configuré précédemment, cliquez avec le bouton droit sur le nœud **Références** et ajoutez une référence au projet de bibliothèque de classe UWP.
12. Régénérez l’ensemble de la solution et assurez-vous que tous les projets ont été créés avec succès.

## <a name="host-the-custom-uwp-control-in-your-wpf-app"></a>Héberger le contrôle UWP personnalisé dans une application WPF

1. Dans l’**Explorateur de solutions**, développez le projet WPF et ouvrez le fichier MainWindow.xaml ou une autre fenêtre dans laquelle vous souhaitez héberger le contrôle personnalisé.
2. Dans le fichier XAML, ajoutez la déclaration d’espace de noms suivante à l’élément `<Window>`.

    ```xml
    xmlns:xaml="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

3. Dans le même fichier, ajoutez le contrôle suivant à l’élément `<Grid>`. Remplacez l’attribut `InitialTypeName` par le nom qualifié complet du contrôle utilisateur dans votre projet de bibliothèque de classe UWP.

    ```xml
    <xaml:WindowsXamlHost InitialTypeName="UWPClassLibrary.MyUserControl" ChildChanged="WindowsXamlHost_ChildChanged" />
    ```

4. Ouvrez le fichier code-behind et ajoutez le code suivant à la classe `Window`. Ce code définit un gestionnaire d’événements `ChildChanged` qui affecte la valeur du champ ``XamlIslandMessage`` du contrôle personnalisé UWP à la valeur du champ `WPFMessage` dans l’application WPF. Remplacez `UWPClassLibrary.MyUserControl` par le nom qualifié complet du contrôle utilisateur dans votre projet de bibliothèque de classe UWP.

    ```csharp
    private void WindowsXamlHost_ChildChanged(object sender, EventArgs e)
    {
        // Hook up x:Bind source.
        global::Microsoft.Toolkit.Wpf.UI.XamlHost.WindowsXamlHost windowsXamlHost =
            sender as global::Microsoft.Toolkit.Wpf.UI.XamlHost.WindowsXamlHost;
        global::UWPClassLibrary.MyUserControl userControl =
            windowsXamlHost.GetUwpInternalObject() as global::UWPClassLibrary.MyUserControl;

        if (userControl != null)
        {
            userControl.XamlIslandMessage = this.WPFMessage;
        }
    }

    public string WPFMessage
    {
        get
        {
            return "Binding from WPF to UWP XAML";
        }
    }
    ```

6. Générez et exécutez votre application, puis vérifiez que le contrôle utilisateur UWP s’affiche comme prévu.

## <a name="add-a-control-from-the-winui-library-to-the-custom-control"></a>Ajouter au contrôle personnalisé un contrôle de la bibliothèque WinUI

Traditionnellement, les contrôles UWP ont été publiés dans le cadre du système d’exploitation Windows 10 et mis à la disposition des développeurs via le SDK Windows. La [bibliothèque WinUI](https://docs.microsoft.com/uwp/toolkits/winui/) est une autre approche, dans laquelle les versions mises à jour des contrôles UWP du SDK Windows sont distribuées dans un package NuGet qui n’est pas lié aux versions de SDK Windows. Cette bibliothèque comprend également de nouveaux contrôles qui ne font pas partie du SDK Windows et de la plateforme UWP par défaut. Pour plus d’informations, consultez notre [feuille de route de la bibliothèque WinUI](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md).

Cette section montre comment ajouter un contrôle UWP de la bibliothèque WinUI à votre contrôle utilisateur afin de pouvoir héberger ce contrôle dans votre application WPF.

1. Dans le projet d’application UWP, installez la dernière version publiée ou prépubliée du package NuGet [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml).

    > [!NOTE]
    > Si votre application de bureau est empaquetée dans un [package MSIX](https://docs.microsoft.com/windows/msix), vous pouvez utiliser une version prépubliée ou publiée du package NugGet [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml). Si votre application de bureau n’est pas empaquetée à l’aide de MSIX, vous devez installer une version préliminaire du package NuGet [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml).

2. Dans le fichier App.xaml de ce projet, ajoutez l’élément enfant suivant à l’élément `<xaml:XamlApplication>`.

    ```xml
    <Application.Resources>
        <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
    </Application.Resources>
    ```

    Après avoir ajouté cet élément, le contenu de ce fichier devrait maintenant ressembler à ce qui suit.

    ```xml
    <xaml:XamlApplication
        x:Class="MyUWPApp.App"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:xaml="using:Microsoft.Toolkit.Win32.UI.XamlHost"
        xmlns:local="using:MyUWPApp">
        <Application.Resources>
            <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
        </Application.Resources>
    </xaml:XamlApplication>
    ```

3. Dans le projet de bibliothèque de classe UWP, installez la dernière version du package NuGet [Microsof.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml) (la même version que celle que vous avez installée dans le projet d’application UWP).

4. Dans le même projet, ouvrez le fichier XAML pour le contrôle utilisateur, puis ajoutez la déclaration d’espace de noms suivante à l’élément `<UserControl>`.

    ```xml
    xmlns:winui="using:Microsoft.UI.Xaml.Controls"
    ```

5. Dans le même fichier, ajoutez un élément `<winui:RatingControl />` en tant qu’enfant de l’élément `<StackPanel>`. Cet élément ajoute une instance de la classe [RatingControl](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.ratingcontrol) issue de la bibliothèque WinUI. Après avoir ajouté cet élément, l’élément `<StackPanel>` devrait maintenant ressembler à ce qui suit.

    ```xml
    <StackPanel Background="LightCoral">
        <TextBlock>This is a simple custom UWP control</TextBlock>
        <Rectangle Fill="Blue" Height="100" Width="100"/>
        <TextBlock Text="{x:Bind XamlIslandMessage}" FontSize="50"></TextBlock>
        <winui:RatingControl />
    </StackPanel>
    ```

6. Générez et exécutez votre application, puis vérifiez que le nouveau contrôle d’évaluation s’affiche comme prévu.

## <a name="package-the-app"></a>Empaqueter l’application

Vous pouvez empaqueter l’application WPF dans un [package MSIX](https://docs.microsoft.com/windows/msix) pour la déployer. MSIX est une technologie d’empaquetage moderne pour Windows, basée sur une combinaison des technologies d’installation MSI, .appx, App-V et ClickOnce.

Les instructions suivantes montrent comment empaqueter tous les composants de la solution dans un package MSIX en utilisant le [Projet de création de package d’application Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) dans Visual Studio 2019. Ces étapes sont nécessaires uniquement si vous souhaitez empaqueter l’application WPF dans un package MSIX. Notez que ces étapes incluent actuellement des solutions de contournement spécifiques au scénario d’hébergement de contrôles UWP personnalisés.

> [!NOTE]
> Si vous choisissez de ne pas empaqueter votre application dans un [package MSIX](https://docs.microsoft.com/windows/msix) pour la déployer, [Visual C++ Runtime](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) doit être installé sur les ordinateurs qui exécutent votre application.

1. Ajoutez un nouveau [projet d’empaquetage d’application Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) à votre solution. À mesure que vous créez le projet, sélectionnez **Windows 10, version 1903 (10.0; Build 18362)** pour la **Version cible** et la **Version minimale**.

2. Dans le projet d’empaquetage, cliquez avec le bouton droit sur le nœud **Applications**, puis choisissez **Ajouter une référence**. Dans la liste des projets, sélectionnez le projet WPF dans votre solution, puis cliquez sur **OK**.

3. Modifiez le fichier projet WPF. Ces modifications sont actuellement requises pour l’empaquetage d’applications WPF qui hébergent des contrôles UWP personnalisés.

    1. Dans l'Explorateur de solutions, cliquez avec le bouton droit sur le nœud du projet WPF et sélectionnez **Décharger le projet**.
    2. Cliquez avec le bouton droit sur le nœud de projet WPF, puis sélectionnez **Modifier**.
    3. Localisez la dernière balise de fermeture `</PropertyGroup>` dans le fichier, puis ajoutez le code XML suivant immédiatement après cette balise.

        ``` xml
        <PropertyGroup>
          <AssetTargetFallback>uap10.0.18362</AssetTargetFallback>
        </PropertyGroup>
        ```

    4. Enregistrez le fichier projet et fermez-le.
    5. Cliquez avec le bouton droit sur le nœud de projet WPF et choisissez **Recharger le projet**.

4. Générez et exécutez le projet d’empaquetage. Vérifiez que l’application WPF s’exécute et que le contrôle personnalisé UWP s’affiche comme prévu.

## <a name="related-topics"></a>Rubriques connexes

* [Héberger des contrôles XAML UWP dans des applications de bureau (îlots XAML)](xaml-islands.md)
* [Exemples de code XAML Islands](https://github.com/microsoft/Xaml-Islands-Samples)
* [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)
