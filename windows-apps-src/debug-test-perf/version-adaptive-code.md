---
author: jwmsft
title: Code adaptatif de version
description: "Découvrez comment tirer parti des nouvelles API tout en conservant la compatibilité avec les versions précédentes"
translationtype: Human Translation
ms.sourcegitcommit: 3f81d80cef0fef6d24cad1b42ce9726b03857b5a
ms.openlocfilehash: db6b9c83d36ac876661197dce81e5724e44bb640

---

# Code adaptatif de version &#58; Utilisez de nouvelles API tout en conservant la compatibilité avec les versions précédentes

Chaque version du Kit de développement logiciel Windows 10 apporte d’incroyables fonctionnalités qui vous raviront. Toutefois, vos clients ne mettent pas tous en même temps leurs appareils à jour vers la dernière version de Windows 10, et vous voulez vous assurer que votre application fonctionne sur le plus large éventail possible d’appareils. Nous allons vous expliquer comment concevoir votre application afin qu’elle s’exécute sur les versions antérieures de Windows 10, mais tire également parti des nouvelles fonctionnalités lorsque votre application s’exécute sur un appareil disposant de la dernière mise à jour.

Vous devez suivre 2 étapes pour vous assurer que votre application prend en charge la plus large gamme d’appareils Windows 10. Tout d’abord, configurez votre projet Visual Studio afin de cibler les dernières API. Cela influe sur les résultats de la compilation de votre application. Ensuite, effectuez des vérifications à l’exécution pour vous assurer que vous appelez uniquement les API présentes sur l’appareil sur lequel votre application s’exécute.

> [!NOTE] 
> Cet article utilise des exemples du Kit de développement logiciel Windows Insider Preview pour Windows 10, version 1607 (mise à jour anniversaire). Le Kit de développement logiciel Preview est une version préliminaire et ne peut pas être utilisé dans un environnement de production. Installez uniquement le Kit de développement logiciel sur votre ordinateur de test. Le Kit de développement logiciel Preview contient des résolutions de bogues et les modifications de développement apportées à la surface d’exposition de l’API. Si vous travaillez sur une application que vous devrez soumettre au Store, vous ne devez pas installer la version d’évaluation.

## Configurer votre projet Visual Studio

La première étape de la prise en charge de plusieurs versions de Windows 10 consiste à spécifier les versions de système d’exploitation et de Kit de développement logiciel *Cible* et *Minimum* prises en charge dans votre projet Visual Studio.
- *Cible* : version du Kit de développement logiciel pour laquelle Visual Studio compile le code de votre application et exécute tous les outils. Toutes les API et les ressources de ce Kit de développement logiciel sont disponibles dans le code de votre application au moment de la compilation.
- *Minimum* : version du Kit de développement logiciel qui prend en charge  la version la plus ancienne du système d’exploitation sur laquelle votre application peut s’exécuter (et qui sera déployée par le Windows Store) et la version pour laquelle Visual Studio compile le code de balisage de votre application. 

Pendant son exécution, votre application s’exécute par rapport à la version du système d’exploitation déployée : votre application lève donc des exceptions si vous utilisez des ressources ou si vous appelez des API qui ne sont pas disponibles dans cette version. Nous allons vous montrer comment effectuer des vérifications à l’exécution pour appeler les API appropriées dans la suite de cet article.

Les paramètres Cible et Minimum spécifient les limites de plage des versions de système d’exploitation et de Kit de développement logiciel. Toutefois, si vous testez votre application sur la version Minimum, vous pouvez être sûr qu’elle s’exécutera sur toutes les versions comprises entre la version Minimum et la version Cible.

> [!TIP]
> Visual Studio ne donne aucune information sur la compatibilité des API. Il vous incombe de tester et de vous assurer que votre application fonctionne comme prévu sur toutes les versions de système d’exploitation, de la version Minimum à la version Cible incluses.

Lorsque vous créez un projet dans Visual Studio 2015, Update 2 ou version ultérieure, vous êtes invité à définir les versions Cible et Minimum prises en charge par votre application. Par défaut, la version Cible est la version du Kit de développement logiciel installée la plus élevée, tandis que la version Minimum est la version du Kit de développement logiciel installée la moins élevée. Vous pouvez choisir les versions Cible et Minimum uniquement parmi les versions du Kit de développement logiciel installées sur votre ordinateur. 

![Définir le Kit de développement logiciel cible dans Visual Studio](images/vs-target-sdk-1.png)

Nous vous recommandons généralement de conserver les valeurs par défaut. Toutefois, si vous avez installé une version d’évaluation du Kit de développement et si vous écrivez du code de production, vous devez modifier la version Cible en passant du Kit de développement logiciel d’évaluation à la dernière version officielle du Kit de développement logiciel. 

Pour modifier la version Minimum et la version Cible d’un projet déjà créé dans Visual Studio, accédez à Projet -&gt; Propriétés -&gt; onglet Application -&gt; Ciblage.

![Modifier le Kit de développement logiciel cible dans Visual Studio](images/vs-target-sdk-2.png) 

À titre de référence, voici les numéros de version pour chaque Kit de développement logiciel :
- Windows 10, version 1506 : SDK version 10240
- Windows 10, version 1511 (mise à jour de novembre) : SDK version 10586
- Windows 10, version 1607 Insider Preview (mise à jour anniversaire) : à l’heure où nous écrivons ce document, [la dernière version du Kit de développement logiciel Insider Preview est 14332](https://blogs.windows.com/buildingapps/2016/04/28/windows-10-anniversary-sdk-preview-build-14332-released/).

Vous pouvez télécharger les versions finales du Kit de développement logiciel dans les [archives de Kits de développement logiciel Windows et d’émulateurs](https://developer.microsoft.com/downloads/sdk-archive). Vous pouvez télécharger la dernière version du Kit de développement logiciel Windows Insider Preview dans la section du site [Windows Insider](https://insider.windows.com/) dédiée aux développeurs.

## Écrire du code adaptatif

Vous pouvez considérer l’écriture de code adaptatif de la même façon que vous envisagez la [création d’une interface utilisateur adaptative](https://msdn.microsoft.com/windows/uwp/layout/layouts-with-xaml). Vous pouvez concevoir une interface utilisateur de base qui s’exécute sur l’écran le plus petit, puis déplacer ou ajouter des éléments lorsque vous détectez un écran plus grand pendant l’exécution de votre application. Avec le code adaptatif, vous écrivez votre code de base pour qu’il s’exécute sur la version du système d’exploitation minimale, et vous pouvez ajouter des fonctionnalités choisies lorsque vous détectez une version supérieure pour laquelle la nouvelle fonctionnalité est disponible.

### Vérification des API à l’exécution

Vous utilisez la classe [Windows.Foundation.Metadata.ApiInformation](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.apiinformation.aspx) dans une condition à l’intérieur de votre code pour tester la présence de l’API que vous voulez appeler. Cette condition est évaluée à chaque exécution de votre application, mais sa valeur est **true** uniquement sur les appareils sur lesquels l’API est présente et donc disponible pour un appel. Cela vous permet d’écrire du code adaptatif de version afin de créer des applications utilisant des API disponibles uniquement sur certaines versions de système d’exploitation.

Nous allons examiner des exemples spécifiques de ciblage des nouvelles fonctionnalités dans Windows Insider Preview. Pour obtenir une vision globale de l’utilisation d’**ApiInformation**, voir [Guide des applications UWP](https://msdn.microsoft.com/windows/uwp/get-started/universal-application-platform-guide) et le billet de blog [Détection dynamique des fonctionnalités avec les contrats API](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/).

> [!TIP]
> De nombreuses vérifications des API à l’exécution peuvent affecter les performances de votre application. Les vérifications sont incluses dans ces exemples. Dans le code de production, vous devez effectuer la vérification une seule fois et mettre en cache les résultats, puis utiliser le résultat mis en cache dans toute votre application. 

### Scénarios non pris en charge

Dans la plupart des cas, vous pouvez conserver la version Minimum du Kit de développement logiciel version 10240 pour votre application et utiliser les vérifications à l’exécution pour activer toutes les nouvelles API lorsque votre application s’exécute par la suite sur une autre version. Cependant, vous devez parfois choisir une version supérieure à la version Minimum de votre application pour utiliser de nouvelles fonctionnalités.

Vous devez choisir une version supérieure à la version Minimum de votre application si vous utilisez :
- une nouvelle API nécessitant une fonctionnalité qui n’est pas disponible dans une version antérieure. Vous devez choisir une version supérieure qui inclut cette fonctionnalité. Pour plus d’informations, voir [Déclarations des fonctionnalités d’application](../packaging/app-capability-declarations.md).
- toutes les nouvelles clés de ressources ajoutées à generic.xaml et qui ne sont pas disponibles dans une version antérieure. La version de generic.xaml utilisée lors de l’exécution est déterminée par la version du système d’exploitation sur laquelle l’appareil s’exécute. Vous ne pouvez pas utiliser les vérifications API à l’exécution pour déterminer la présence de ressources XAML. Par conséquent, vous devez uniquement utiliser des clés de ressources disponibles dans la version Minimum que votre application prend en charge, sans quoi [XAMLParseException](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.markup.xamlparseexception.aspx) entraînera le blocage de votre application à l’exécution.

### Options de code adaptatif

Il existe deux méthodes pour créer du code adaptatif. Dans la plupart des cas, vous écrivez votre balisage d’application pour une exécution sur la version Minimum, puis vous utilisez le code de votre application pour accéder aux nouvelles fonctionnalités du système d’exploitation disponibles. Toutefois, si vous devez mettre à jour une propriété dans un état visuel, et s’il n’y a qu’une modification de propriété ou de valeur d’énumération entre les versions de système d’exploitation, vous pouvez créer un déclencheur d’état extensible dont l’activation dépend de la présence d’une API.

Nous allons comparer les différentes options.

**Code de l’application**

Quand l’utiliser :
- Recommandé pour tous les scénarios de code adaptatif à l’exception des cas spécifiques définis ci-dessous pour les déclencheurs extensibles.

Avantages :
- Évite la surcharge pour le développeur/la complexité des relations avec les API dans le balisage.

Inconvénients :
- Aucune prise en charge du concepteur.

**Déclencheurs d’état**

Quand l’utiliser :
- Quand il n’y a qu’une modification de propriété ou de valeur d’énumération entre les versions de système d’exploitation, qui ne nécessite pas de modification logique et qui est associée à un état visuel.

Avantages :
- Vous permet de créer des états visuels spécifiques déclenchés en fonction de la présence d’une API.
- Prise en charge disponible pour certains concepteurs.

Inconvénients :
- L’utilisation de déclencheurs personnalisés se limite aux états visuels et ne se prête pas aux dispositions adaptatives compliquées.
- Doit utiliser la propriété Setters pour spécifier les modifications de valeur : seules les modifications simples sont donc possibles.
- Les déclencheurs d’état personnalisés sont très détaillés pour la configuration et l’utilisation.

## Exemples de code adaptatif

Cette section présente plusieurs exemples de code adaptatif qui utilisent des API nouvelles dans Windows 10, version 1607 (Windows Insider Preview).

### Exemple 1 : Nouvelle valeur d’énumération

Windows 10, version 1607 ajoute une nouvelle valeur à l’énumération [InputScopeNameValue](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.inputscopenamevalue.aspx) : **ChatWithoutEmoji**. Cette nouvelle étendue des entrées affiche le même comportement d’entrée que l’étendue des entrées **Chat** (vérification de l’orthographe, saisie semi-automatique, mise en majuscules automatique), mais elle est mappée à un clavier tactile sans bouton emoji. Elle est utile si vous créez votre propre sélecteur d’emoji et souhaitez désactiver le bouton emoji du clavier tactile. 

Cet exemple explique comment vérifier la présence de la valeur d’énumération **ChatWithoutEmoji** et définit la propriété [InputScope](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textbox.inputscope.aspx) d’une classe **TextBox** le cas échéant. En cas d’absence sur le système sur lequel l’application est exécutée, **InputScope** prend alors la valeur **Chat**. Le code présenté peut être placé dans un constructeur de page ou un gestionnaire d’événements Page.Loaded.

> [!TIP]
> Lorsque vous vérifiez une API, utilisez des chaînes statiques au lieu de vous appuyer sur les fonctionnalités de langage .NET. Sinon, votre application pourrait tenter d’accéder à un type qui n’est pas défini et se bloquer au moment de l’exécution.

**C#**
```csharp
// Create a TextBox control for sending messages 
// and initialize an InputScope object.
TextBox messageBox = new TextBox();
messageBox.AcceptsReturn = true;
messageBox.TextWrapping = TextWrapping.Wrap;
InputScope scope = new InputScope();
InputScopeName scopeName = new InputScopeName();

// Check that the ChatWithEmoji value is present.
// (It's present starting with Windows 10, version 1607,
//  the Target version for the app. This check returns false on earlier versions.)         
if (ApiInformation.IsEnumNamedValuePresent("Windows.UI.Xaml.Input.InputScopeNameValue", "ChatWithoutEmoji"))
{
    // Set new ChatWithoutEmoji InputScope if present.
    scopeName.NameValue = InputScopeNameValue.ChatWithoutEmoji;
}
else
{
    // Fall back to Chat InputScope.
    scopeName.NameValue = InputScopeNameValue.Chat;
}

// Set InputScope on messaging TextBox.
scope.Names.Add(scopeName);
messageBox.InputScope = scope;

// For this example, set the TextBox text to show the selected InputScope.
messageBox.Text = messageBox.InputScope.Names[0].NameValue.ToString();

// Add the TextBox to the XAML visual tree (rootGrid is defined in XAML).
rootGrid.Children.Add(messageBox);
```

Dans l’exemple précédent, la classe TextBox est créée et toutes les propriétés sont définies dans le code. Toutefois, si un code XAML existe déjà et si vous souhaitez simplement modifier la propriété InputScope sur les systèmes pour lesquels la nouvelle valeur est prise en charge, vous pouvez le faire sans modifier votre code XAML, comme illustré ici. Vous définissez la valeur par défaut **Chat** dans le code XAML, mais vous la remplacez dans le code si la valeur **ChatWithoutEmoji** est présente.

**XAML**
```xaml
<TextBox x:Name="messageBox"
         AcceptsReturn="True" TextWrapping="Wrap"
         InputScope="Chat"
         Loaded="messageBox_Loaded"/>
```

**C#**
```csharp
private void messageBox_Loaded(object sender, RoutedEventArgs e)
{
    if (ApiInformation.IsEnumNamedValuePresent("Windows.UI.Xaml.Input.InputScopeNameValue", "ChatWithoutEmoji"))
    {
        // Check that the ChatWithEmoji value is present.
        // (It's present starting with Windows 10, version 1607,
        //  the Target version for the app. This code is skipped on earlier versions.)
        InputScope scope = new InputScope();
        InputScopeName scopeName = new InputScopeName();
        scopeName.NameValue = InputScopeNameValue.ChatWithoutEmoji;
        // Set InputScope on messaging TextBox.
        scope.Names.Add(scopeName);
        messageBox.InputScope = scope;
    }

    // For this example, set the TextBox text to show the selected InputScope.
    // This is outside of the API check, so it will happen on all OS versions.
    messageBox.Text = messageBox.InputScope.Names[0].NameValue.ToString();
}
```

Maintenant que nous avons un exemple concret, nous allons voir comment s’appliquent les paramètres de version Cible et Minimum.

Dans ces exemples, vous pouvez utiliser la valeur d’énumération Chat dans le code XAML, ou dans un code sans vérification, car elle est présente dans la version Minimum du système d’exploitation pris en charge. 

Si vous utilisez la valeur ChatWithoutEmoji dans un code XAML, ou dans un code sans vérification, la compilation se fera sans erreur, car elle est présente dans la version Cible du système d’exploitation. Elle s’exécute également sans erreur sur un système disposant de la version du système d’exploitation Cible. Toutefois, lorsque l’application s’exécute sur un système doté de la version Minimum du système d’exploitation, elle se bloque lors de l’exécution en raison de l’absence de la valeur d’énumération ChatWithoutEmoji. Par conséquent, vous devez utiliser cette valeur uniquement dans le code et l’incorporer dans une vérification API à l’exécution afin qu’elle soit appelée uniquement si elle est prise en charge sur le système en cours d’exécution.

### Exemple 2 : Nouveau contrôle

Une nouvelle version de Windows offre en règle générale de nouveaux contrôles à la surface d’API UWP apportant de nouvelles fonctionnalités à la plateforme. Pour tirer parti d’un nouveau contrôle, utilisez la méthode [ApiInformation.IsTypePresent](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.apiinformation.istypepresent.aspx).

Windows 10, version 1607 introduit un nouveau contrôle multimédia appelé [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx). Ce contrôle est basé sur la classe [MediaPlayer](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.aspx) et apporte donc des fonctionnalités telles que la capacité à s’adapter à la lecture audio en arrière-plan, et elle se sert des améliorations architecturales de Media stack.

Toutefois, si l’application s’exécute sur un appareil exécutant lui-même une version de Windows 10 antérieure à la version 1607, vous devez choisir le contrôle [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaelement.aspx) et non le nouveau contrôle **MediaPlayerElement**. Vous pouvez vous servir de la méthode [**ApiInformation.IsTypePresent**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.apiinformation.istypepresent.aspx) pour vérifier la présence du contrôle MediaPlayerElement lors de l’exécution, puis charger les contrôles adaptés au système dans lequel l’application est en cours d’exécution.

Cet exemple montre comment créer une application qui utilise le nouveau contrôle MediaPlayerElement ou l’ancien contrôle MediaElement en fonction de la présence du type MediaPlayerElement. Dans ce code, vous utilisez la classe [UserControl](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.usercontrol.aspx) pour agencer les contrôles et leur code et interface utilisateur associés pour être en mesure de passer de l’un à l’autre en fonction de la version du système d’exploitation. Vous pouvez aussi utiliser un contrôle personnalisé qui offrira plus de fonctionnalités et de personnalisations que nécessaire dans cet exemple simple.
 
**MediaPlayerUserControl** 

`MediaPlayerUserControl` encapsule un contrôle **MediaPlayerElement** et plusieurs boutons permettant d’ignorer le média image par image. UserControl vous permet de traiter ces contrôles sous forme d’entité unique et de passer plus facilement à un contrôle MediaElement sur les systèmes plus anciens. Ce contrôle utilisateur doit être utilisé uniquement sur les systèmes où MediaPlayerElement est présent, afin que vous n’utilisiez pas les vérifications ApiInformation dans le code situé à l’intérieur de ce contrôle utilisateur.

> [!NOTE]
> Pour que cet exemple reste simple et ciblé, les boutons d’étape d’image sont placés en dehors du lecteur multimédia. Pour améliorer l’expérience utilisateur, vous devez personnaliser MediaTransportControls afin d’inclure des boutons personnalisés. Voir [Contrôles de transport personnalisés](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/custom-transport-controls) pour plus d’informations. 

**XAML**
```xaml
<UserControl
    x:Class="MediaApp.MediaPlayerUserControl"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MediaApp"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    d:DesignHeight="300"
    d:DesignWidth="400">

    <Grid x:Name="MPE_grid>
        <Grid.RowDefinitions>
            <RowDefinition/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
        <StackPanel Orientation="Horizontal" 
                    HorizontalAlignment="Center" Grid.Row="1">
            <RepeatButton Click="StepBack_Click" Content="Step Back"/>
            <RepeatButton Click="StepForward_Click" Content="Step Forward"/>
        </StackPanel>
    </Grid>
</UserControl>
```

**C#**
```csharp
using System;
using Windows.Media.Core;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;

namespace MediaApp
{
    public sealed partial class MediaPlayerUserControl : UserControl
    {
        public MediaPlayerUserControl()
        {
            this.InitializeComponent();
            
            // The markup code compiler runs against the Minimum OS version so MediaPlayerElement must be created in app code
            MPE = new MediaPlayerElement();
            Uri videoSource = new Uri("ms-appx:///Assets/UWPDesign.mp4");
            MPE.Source = MediaSource.CreateFromUri(videoSource);
            MPE.AreTransportControlsEnabled = true;
            MPE.MediaPlayer.AutoPlay = true;

            // Add MediaPlayerElement to the Grid
            MPE_grid.Children.Add(MPE);

        }

        private void StepForward_Click(object sender, RoutedEventArgs e)
        {
            // Step forward one frame, only available using MediaPlayerElement.
            MPE.MediaPlayer.StepForwardOneFrame();
        }

        private void StepBack_Click(object sender, RoutedEventArgs e)
        {
            // Step forward one frame, only available using MediaPlayerElement.
            MPE.MediaPlayer.StepForwardOneFrame();
        }
    }
}
```

**MediaElementUserControl**
 
`MediaElementUserControl` encapsule un contrôle **MediaElement**.

**XAML**
```xaml
<UserControl
    x:Class="MediaApp.MediaElementUserControl"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MediaApp"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    d:DesignHeight="300"
    d:DesignWidth="400">

    <Grid>
        <MediaElement AreTransportControlsEnabled="True" 
                      Source="Assets/UWPDesign.mp4"/>
    </Grid>
</UserControl>
```

> [!NOTE]
> La page de code de `MediaElementUserControl` contient uniquement du code généré : il n’est donc pas affiché.

**Initialiser un contrôle basé sur IsTypePresent**

Lors de l’exécution, vous appelez la méthode **ApiInformation.IsTypePresent** afin de vérifier MediaPlayerElement. S’il est présent, vous chargez `MediaPlayerUserControl`. Dans le cas contraire, vous chargez `MediaElementUserControl`.

**C#**
```csharp
public MainPage()
{
    this.InitializeComponent();

    UserControl mediaControl;

    // Check for presence of type MediaPlayerElement.
    if (ApiInformation.IsTypePresent("Windows.UI.Xaml.Controls.MediaPlayerElement"))
    {
        mediaControl = new MediaPlayerUserControl();
    }
    else
    {
        mediaControl = new MediaElementUserControl();
    }

    // Add mediaControl to XAML visual tree (rootGrid is defined in XAML).
    rootGrid.Children.Add(mediaControl);
}
```

> [!IMPORTANT]
> N’oubliez pas que cette vérification définit uniquement l’objet `mediaControl` sur la valeur `MediaPlayerUserControl` ou `MediaElementUserControl`. Vous devez effectuer les vérifications conditionnelles suivantes dans votre code, partout où vous devez déterminer s’il convient d’utiliser les API MediaPlayerElement ou MediaElement. Vous devez effectuer la vérification une seule fois et mettre en cache les résultats, puis utiliser le résultat mis en cache dans toute votre application.

## Exemples de déclencheur d’état

Les déclencheurs d’état extensibles permettent d’utiliser conjointement le balisage et le code afin de déclencher des changements d’état visuel basés sur une condition que vous vérifiez dans le code. Ici, il s’agit de la présence d’une API spécifique. Nous ne recommandons pas l’utilisation de déclencheurs d’état pour les scénarios courants de code adaptatif en raison de la surcharge impliquée, et de la restriction aux seuls états visuels. 

Vous devez utiliser des déclencheurs d’état pour le code adaptatif uniquement en cas de modifications mineures d’interface utilisateur entre les différentes versions de système d’exploitation, qui n’affecteront pas le reste de l’interface utilisateur, comme un changement de propriété ou de valeur d’énumération sur un contrôle.

### Exemple 1 : Nouvelle propriété

La première étape de la configuration d’un déclencheur d’état extensible consiste à sous-classer la classe [StateTriggerBase](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.statetriggerbase.aspx) afin de créer un déclencheur personnalisé dont l’activité dépendra de la présence d’une API. Cet exemple montre un déclencheur qui s’active si la présence de la propriété correspond à la variable `_isPresent` définie dans le code XAML.

**C#**
```csharp
class IsPropertyPresentTrigger : StateTriggerBase
{
    public string TypeName { get; set; }
    public string PropertyName { get; set; }

    private Boolean _isPresent;
    private bool? _isPropertyPresent = null;

    public Boolean IsPresent
    {
        get { return _isPresent; }
        set
        {
            _isPresent = value;
            if (_isPropertyPresent == null)
            {
                // Call into ApiInformation method to determine if property is present.
                _isPropertyPresent =
                ApiInformation.IsPropertyPresent(TypeName, PropertyName);
            }

            // If the property presence matches _isPresent then the trigger will be activated;
            SetActive(_isPresent == _isPropertyPresent);
        }
    }
}
```

L’étape suivante consiste à configurer le déclencheur d’état visuel dans XAML pour obtenir deux états visuels différents en fonction de la présence de l’API. 

Windows 10, version 1607 introduit une nouvelle propriété sur la classe [FrameworkElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.aspx), appelée [AllowFocusOnInteraction](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.allowfocusoninteraction.aspx), qui détermine si un contrôle prend le focus lorsque l’utilisateur interagit. Cette fonction est utile si vous souhaitez conserver le focus sur une zone de texte pour l’entrée de données (et garder le clavier tactile visible) pendant que l’utilisateur clique sur un bouton.

Dans cet exemple, le déclencheur vérifie si la propriété est présente. Si la propriété est présente, il définit la propriété **AllowFocusOnInteraction** d’un Bouton sur **false**. Mais si la propriété n’est pas présente, le bouton conserve son état d’origine. La zone de texte est incluse pour permettre de mieux visualiser les effets de cette propriété lorsque vous exécutez le code.

**XAML**
```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <StackPanel>
        <TextBox Width="300" Height="36"/>
        <!-- Button to set the new property on. -->
        <Button x:Name="testButton" Content="Test" Margin="12"/>
    </StackPanel>

    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="propertyPresentStateGroup">
            <VisualState>
                <VisualState.StateTriggers>
                    <!--Trigger will activate if the AllowFocusOnInteraction property is present-->
                    <local:IsPropertyPresentTrigger 
                        TypeName="Windows.UI.Xaml.FrameworkElement" 
                        PropertyName="AllowFocusOnInteraction" IsPresent="True"/>
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="testButton.AllowFocusOnInteraction" 
                            Value="False"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Grid>
```

### Exemple 2 : Nouvelle valeur d’énumération

Cet exemple explique comment définir des valeurs d’énumération différentes en fonction de la présence d’une valeur. Il utilise un déclencheur d’état personnalisé pour obtenir le même résultat que l’exemple précédent de Chat. Dans cet exemple, vous utilisez la nouvelle étendue des entrées ChatWithoutEmoji si l’appareil exécute Windows 10, version 1607. Sinon, c’est l’étendue des entrées **Chat** qui est utilisée. Les états visuels qui utilisent ce déclencheur sont configurés dans un style *if-else*, où l’étendue des entrées est choisie en fonction de la présence de la nouvelle valeur d’énumération.

**C#**
```csharp
class IsEnumPresentTrigger : StateTriggerBase
{
    public string EnumTypeName { get; set; }
    public string EnumValueName { get; set; }

    private Boolean _isPresent;
    private bool? _isEnumValuePresent = null;

    public Boolean IsPresent
    {
        get { return _isPresent; }
        set
        {
            _isPresent = value;

            if (_isEnumValuePresent == null)
            {
                // Call into ApiInformation method to determine if value is present.
                _isEnumValuePresent =
                ApiInformation.IsEnumNamedValuePresent(EnumTypeName, EnumValueName);
            }

            // If the property presence matches _isPresent then the trigger will be activated;
            SetActive(_isPresent == _isEnumValuePresent);
        }
    }
}
```

**XAML**
```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <TextBox x:Name="messageBox"
     AcceptsReturn="True" TextWrapping="Wrap"/>


    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="EnumPresentStates">
            <!--if-->
            <VisualState x:Name="isPresent">
                <VisualState.StateTriggers>
                    <local:IsEnumPresentTrigger 
                        EnumTypeName="Windows.UI.Xaml.Input.InputScopeNameValue" 
                        EnumValueName="ChatWithoutEmoji" IsPresent="True"/>
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="messageBox.InputScope" Value="ChatWithoutEmoji"/>
                </VisualState.Setters>
            </VisualState>
            <!--else-->
            <VisualState x:Name="isNotPresent">
                <VisualState.StateTriggers>
                    <local:IsEnumPresentTrigger 
                        EnumTypeName="Windows.UI.Xaml.Input.InputScopeNameValue" 
                        EnumValueName="ChatWithoutEmoji" IsPresent="False"/>
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="messageBox.InputScope" Value="Chat"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Grid>
```
## Articles connexes

- [Guide des applications UWP](https://msdn.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)
- [Détection dynamique des fonctionnalités avec les contrats d’API](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/)



<!--HONumber=Jun16_HO4-->

