---
Description: Étendre votre application de bureau avec des interfaces utilisateur et des composants Windows
title: Étendre votre application avec une interface utilisateur et des composants Windows
ms.date: 06/08/2018
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 73e867071058dfde71979318d6d711d79460f30b
ms.sourcegitcommit: 4df8c04fc6c22ec76cdb7bb26f327182f2dacafa
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85334554"
---
# <a name="extend-your-desktop-app-with-modern-uwp-components"></a>Étendre votre application de bureau avec des composants UWP modernes

Certaines expériences Windows 10 (par exemple, une page d’interface utilisateur tactile) doivent s’exécuter à l’intérieur d’un conteneur d’application moderne. Si vous souhaitez ajouter ces expériences, étendez votre application de bureau avec des projets UWP et des composants Windows Runtime.

Dans de nombreux cas, vous pouvez appeler des API d’exécution Windows directement à partir de votre application de bureau. Par conséquent, avant de lire ce guide, consultez [Optimiser pour Windows 10](desktop-to-uwp-enhance.md).

> [!NOTE]
> Les fonctionnalités décrites dans cet article nécessitent que votre application de bureau comporte une [identité de package](modernize-packaged-apps.md), soit en [empaquetant votre application de bureau dans un package MSIX](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root), soit en [accordant l’identité de votre application à l’aide d’un package partiellement alloué](grant-identity-to-nonpackaged-apps.md).

Si vous êtes prêt, commençons.

<a id="setup"></a>

## <a name="first-setup-your-solution"></a>Commencer par configurer la Solution

Ajoutez un ou plusieurs projets UWP et composants d’exécution à votre solution.

Commencez avec une solution qui contient un **Projet de création de package d’application Windows** avec une référence à votre application de bureau.

Cette image illustre un exemple de solution.

![Démarrer un projet de façon étendue](images/desktop-to-uwp/extend-start-project.png)

Si votre solution ne contient aucun projet de création de package, consultez [Empaqueter votre application à l’aide de Visual Studio](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net).

### <a name="configure-the-desktop-application"></a>Configurer l’application de bureau

Assurez-vous que votre application de bureau contient des références aux fichiers dont vous avez besoin pour appeler les API d’exécution Windows.

Pour ce faire, consultez la section [Configurer votre projet](desktop-to-uwp-enhance.md#set-up-your-project).

### <a name="add-a-uwp-project"></a>Ajouter un projet UWP

Ajoutez une **Application vide (Windows universelle)** à votre solution.

C’est ici que vous générez une interface utilisateur XAML moderne, ou que vous utilisez vos API uniquement exécutées dans un processus UWP.

![Projet UWP](images/desktop-to-uwp/add-uwp-project-to-solution.png)

Dans votre projet de création de package, cliquez avec le bouton droit sur le nœud **Applications**, puis cliquez sur **Ajouter une référence**.

![Référencer un projet UWP](images/desktop-to-uwp/add-uwp-project-reference.png)

Ajoutez ensuite une référence au projet UWP.

![Référencer un projet UWP](images/desktop-to-uwp/choose-uwp-project.png)

Votre solution doit ressembler à ceci :

![Solution avec un projet UWP](images/desktop-to-uwp/uwp-project-reference.png)

### <a name="optional-add-a-windows-runtime-component"></a>(Facultatif) Ajoutez un composant Windows Runtime

Pour accomplir certains scénarios, vous devez ajouter un code à un composant Windows Runtime.

![service d’application composant d’exécution](images/desktop-to-uwp/add-runtime-component.png)

Ensuite, à partir de votre projet UWP, ajoutez une référence au composant d’exécution. Votre solution doit ressembler à ceci :

![Référence de composant d’exécution](images/desktop-to-uwp/runtime-component-reference.png)

### <a name="build-your-solution"></a>Générer votre solution

Générez votre solution pour vous assurer qu’aucune erreur n’apparaît. Si vous recevez des erreurs, ouvrez **Configuration Manager** et vérifiez que vos projets ciblent la même plateforme.

![Configuration Manager](images/desktop-to-uwp/config-manager.png)

Examinons quelques-unes des possibilités concernant vos projets UWP et composants d’exécution.

## <a name="show-a-modern-xaml-ui"></a>Afficher une interface utilisateur XAML moderne

Dans le cadre du flux de votre application, vous pouvez incorporer des interfaces utilisateur XAML modernes dans votre application de bureau. Ces interfaces utilisateur s’adaptent naturellement à différentes résolutions et tailles d’écran et prennent en charge des modèles interactifs modernes tels que la fonctionnalité tactile et l’entrée manuscrite.

Par exemple, avec une petite quantité de balisage XAML, vous pouvez offrir aux utilisateurs des fonctionnalités puissantes de visualisation cartographique.

Cette image montre une application Windows Forms qui ouvre une interface utilisateur XAML moderne contenant un contrôle de carte.

![adaptive-design](images/desktop-to-uwp/extend-xaml-ui.png)

>[!NOTE]
>Cet exemple affiche une interface utilisateur XAML en ajoutant un projet UWP à la solution. Il s’agit de l’approche prise en charge stable pour afficher des interfaces utilisateur XAML dans une application de bureau. L’alternative à cette approche consiste à ajouter des contrôles XAML UWP directement à votre application de bureau à l’aide d’un îlot XAML. Des îlots XAML sont actuellement disponibles en préversion pour développeurs. Nous vous encourageons à les essayer dès maintenant dans votre prototype de code, mais nous vous déconseillons de les utiliser dans le code de production pour le moment. Ces API et contrôles continueront à mûrir et à se stabiliser dans les futures versions de Windows. Pour en savoir plus sur les îlots XAML, consultez [Contrôles UWP dans les applications de bureau](xaml-islands.md)

### <a name="the-design-pattern"></a>Modèle de conception

Pour afficher une interface utilisateur XAML, procédez comme suit :

:one: [Configurer votre solution](#solution-setup)

:two: [Créer une interface utilisateur XAML](#xaml-UI)

:three: [Ajouter une extension de protocole au projet UWP](#add-a-protocol-extension)

:four: [Démarrer l’application UWP à partir de votre application de bureau](#start)

:five: [Dans le projet UWP, afficher la page souhaitée](#parse)

<a id="solution-setup"></a>

### <a name="setup-your-solution"></a>Configurer votre solution

Pour des instructions générales sur la façon de configurer votre solution, consultez la section [Commencer par configurer la Solution](#setup) au début de ce guide.

Votre solution ressemblera à ceci :

![Solution d’interface utilisateur XAML](images/desktop-to-uwp/xaml-ui-solution.png)

Dans cet exemple, le projet Windows Forms est nommé **Landmarks** et le projet UWP qui contient l’interface utilisateur XAML est nommé **MapUI**.

<a id="xaml-UI"></a>

### <a name="create-a-xaml-ui"></a>Créer une interface utilisateur avec XAML

Ajoutez une interface utilisateur XAML à votre projet UWP. Voici le code XAML d’une carte de base.

```xml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" Margin="12,20,12,14">
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition Width="*"/>
    </Grid.ColumnDefinitions>
    <maps:MapControl x:Name="myMap" Grid.Column="0" Width="500" Height="500"
                     ZoomLevel="{Binding ElementName=zoomSlider,Path=Value, Mode=TwoWay}"
                     Heading="{Binding ElementName=headingSlider,Path=Value, Mode=TwoWay}"
                     DesiredPitch="{Binding ElementName=desiredPitchSlider,Path=Value, Mode=TwoWay}"
                     HorizontalAlignment="Left"
                     MapServiceToken="<Your Key Goes Here" />
    <Grid Grid.Column="1" Margin="12">
        <StackPanel>
            <Slider Minimum="1" Maximum="20" Header="ZoomLevel" Name="zoomSlider" Value="17.5"/>
            <Slider Minimum="0" Maximum="360" Header="Heading" Name="headingSlider" Value="0"/>
            <Slider Minimum="0" Maximum="64" Header=" DesiredPitch" Name="desiredPitchSlider" Value="32"/>
        </StackPanel>
    </Grid>
</Grid>
```

### <a name="add-a-protocol-extension"></a>Ajouter une extension de protocole

Dans l’**Explorateur de solutions**, ouvrez le fichier **package.appxmanifest** du projet UWP dans votre solution et ajoutez cette extension.

```xml
<Extensions>
  <uap:Extension Category="windows.protocol" Executable="MapUI.exe" EntryPoint="MapUI.App">
    <uap:Protocol Name="xamluidemo" />
  </uap:Extension>
</Extensions>
```

Nommez le protocole, indiquez le nom de l’exécutable généré par le projet UWP et le nom de la classe de point d’entrée.

Vous pouvez également ouvrir le **package.appxmanifest** dans le concepteur, choisir l’onglet **Déclarations**, puis ajouter l’extension ici.

![declarations-tab](images/desktop-to-uwp/protocol-properties.png)

> [!NOTE]
> Les contrôles de carte téléchargent des données à partir d’internet, donc, si vous en utilisez un, vous devez également ajouter la fonctionnalité « client internet » à votre manifeste.

<a id="start"></a>

### <a name="start-the-uwp-app"></a>Démarrer l’application UWP

Tout d’abord, à partir de votre application de bureau créez un [Uri](https://docs.microsoft.com/dotnet/api/system.uri) qui inclut le nom du protocole et les paramètres que vous souhaitez passer à l’application UWP. Appelez ensuite la méthode [LaunchUriAsync](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync).

```csharp

private void Statue_Of_Liberty_Click(object sender, EventArgs e)
{
    ShowMap(40.689247, -74.044502);
}

private async void ShowMap(double lat, double lon)
{
    string str = "xamluidemo://";

    Uri uri = new Uri(str + "location?lat=" +
        lat.ToString() + "&?lon=" + lon.ToString());

    var success = await Windows.System.Launcher.LaunchUriAsync(uri);

}
```

<a id="parse"></a>

### <a name="parse-parameters-and-show-a-page"></a>Analyser les paramètres et afficher une page

Dans la classe **Application** de votre projet UWP, remplacez le gestionnaire d’événements **OnActivated**. Si l’application est activée par votre protocole, analysez les paramètres, puis ouvrez la page que vous voulez.

```csharp
protected override void OnActivated(Windows.ApplicationModel.Activation.IActivatedEventArgs e)
{
    if (e.Kind == ActivationKind.Protocol)
    {
        ProtocolActivatedEventArgs protocolArgs = (ProtocolActivatedEventArgs)e;
        Uri uri = protocolArgs.Uri;
        if (uri.Scheme == "xamluidemo")
        {
            Frame rootFrame = new Frame();
            Window.Current.Content = rootFrame;
            rootFrame.Navigate(typeof(MainPage), uri.Query);
            Window.Current.Activate();
        }
    }
}
```

Dans le code derrière votre page XAML, remplacez la méthode ``OnNavigatedTo`` afin d’utiliser les paramètres passés dans la page. Dans ce cas, nous allons utiliser la latitude et la longitude qui ont été transmises à cette page pour afficher un emplacement dans une carte.

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
 {
     if (e.Parameter != null)
     {
         WwwFormUrlDecoder decoder = new WwwFormUrlDecoder(e.Parameter.ToString());

         double lat = Convert.ToDouble(decoder[0].Value);
         double lon = Convert.ToDouble(decoder[1].Value);

         BasicGeoposition pos = new BasicGeoposition();

         pos.Latitude = lat;
         pos.Longitude = lon;

         myMap.Center = new Geopoint(pos);

         myMap.Style = MapStyle.Aerial3D;

     }

     base.OnNavigatedTo(e);
 }
```

## <a name="making-your-desktop-application-a-share-target"></a>Faire de votre application de bureau une cible de partage

Vous pouvez faire de votre application de bureau une cible de partage afin que les utilisateurs puissent facilement partager des données (par exemple, des images) à partir d’autres applications qui prennent en charge le partage.

Par exemple, les utilisateurs peuvent choisir votre application pour partager des photos à partir de Microsoft Edge ou l’application Photos. Voici un exemple d’application WPF disposant de cette fonctionnalité.

![cible de partage](images/desktop-to-uwp/share-target.png).

Voir l’exemple complet [ici](https://github.com/Microsoft/Windows-Packaging-Samples/tree/master/ShareTarget)

### <a name="the-design-pattern"></a>Modèle de conception

Pour faire de votre application une cible de partage, procédez comme suit :

:one: [Ajouter une extension de cible de partage](#share-extension)

:two: [Remplacer le gestionnaire d’événements OnShareTargetActivated](#override)

:three: [Ajouter des extensions de bureau au projet UWP](#desktop-extensions)

:four: [Ajouter l’extension de processus de confiance totale](#full-trust)

:five: [Modifier l’application de bureau pour récupérer le fichier partagé](#modify-desktop)

<a id="share-extension"></a>

Les étapes suivantes  

### <a name="add-a-share-target-extension"></a>Ajouter une extension cible de partage

Dans l’**Explorateur de solutions**, ouvrez le fichier **package.appxmanifest** du projet UWP dans votre solution et ajoutez l’extension cible de partage.

```xml
<Extensions>
      <uap:Extension
          Category="windows.shareTarget"
          Executable="ShareTarget.exe"
          EntryPoint="App">
        <uap:ShareTarget>
          <uap:SupportedFileTypes>
            <uap:SupportsAnyFileType />
          </uap:SupportedFileTypes>
          <uap:DataFormat>Bitmap</uap:DataFormat>
        </uap:ShareTarget>
      </uap:Extension>
</Extensions>  
```

Indiquez le nom de l’exécutable généré par le projet UWP et le nom de la classe de point d’entrée. Ce balisage part du principe que le nom de l’exécutable de votre application UWP est `ShareTarget.exe`.

Vous devez également spécifier les types de fichiers qui peuvent être partagés avec votre application. Dans cet exemple, nous faisons de l’application de bureau [WPF PhotoStoreDemo](https://github.com/Microsoft/WPF-Samples/tree/master/Sample%20Applications/PhotoStoreDemo) une cible de partage pour les images. Nous spécifions donc `Bitmap` comme type de fichier pris en charge.

<a id="override"></a>

### <a name="override-the-onsharetargetactivated-event-handler"></a>Remplacer le gestionnaire d’événements OnShareTargetActivated

Remplacez le gestionnaire d’événements **OnShareTargetActivated** dans la classe **App** de votre projet UWP.

Ce gestionnaire d’événements est appelé lorsque des utilisateurs choisissent votre application pour partager leurs fichiers.

```csharp

protected override void OnShareTargetActivated(ShareTargetActivatedEventArgs args)
{
    shareWithDesktopApplication(args.ShareOperation);
}

private async void shareWithDesktopApplication(ShareOperation shareOperation)
{
    if (shareOperation.Data.Contains(StandardDataFormats.StorageItems))
    {
        var items = await shareOperation.Data.GetStorageItemsAsync();
        StorageFile file = items[0] as StorageFile;
        IRandomAccessStreamWithContentType stream = await file.OpenReadAsync();

        await file.CopyAsync(ApplicationData.Current.LocalFolder);
            shareOperation.ReportCompleted();

        await FullTrustProcessLauncher.LaunchFullTrustProcessForCurrentAppAsync();
    }
}
```

Dans ce code, nous enregistrons l’image partagée par l’utilisateur dans un dossier de stockage local d’applications. Plus tard, nous modifierons l’application de bureau pour extraire des images de ce même dossier. L’application de bureau faire cela parce qu’elle est incluse dans le même package que l’application UWP.

<a id="desktop-extensions"></a>

### <a name="add-desktop-extensions-to-the-uwp-project"></a>Ajouter des extensions de bureau au projet UWP

Ajoutez les **Extensions de bureau Windows pour UWP** au projet d’application UWP.

![extension de bureau](images/desktop-to-uwp/desktop-extensions.png)

<a id="full-trust"></a>

### <a name="add-the-full-trust-process-extension"></a>Ajouter l’extension de processus de confiance totale

Dans **Explorateur de solutions**, ouvrez le fichier **package.appxmanifest** du projet d’empaquetage dans votre solution, puis ajoutez l’extension de processus de confiance totale en regard de l’extension cible de partage que vous avez ajoutée à ce fichier précédemment.

```xml
<Extensions>
  ...
      <desktop:Extension Category="windows.fullTrustProcess" Executable="PhotoStoreDemo\PhotoStoreDemo.exe" />
  ...
</Extensions>  
```

Cette extension permet à l’application UWP de démarrer l’application de bureau dans laquelle vous souhaitez partager un fichier. Dans l’exemple, nous faisons référence au fichier exécutable de l’application de bureau [WPF PhotoStoreDemo](https://github.com/Microsoft/WPF-Samples/tree/master/Sample%20Applications/PhotoStoreDemo).

<a id="modify-desktop"></a>

### <a name="modify-the-desktop-application-to-get-the-shared-file"></a>Modifier l’application de bureau pour récupérer le fichier partagé

Modifiez votre application de bureau pour rechercher et traiter le fichier partagé. Dans cet exemple, l’application UWP a stocké le fichier partagé dans le dossier de données des applications locales. Par conséquent, nous allons modifier l’application de bureau [WPF PhotoStoreDemo](https://github.com/Microsoft/WPF-Samples/tree/master/Sample%20Applications/PhotoStoreDemo) pour extraire des photos de ce dossier.

```csharp
Photos.Path = Windows.Storage.ApplicationData.Current.LocalFolder.Path;
```

Pour des instances de l’application de bureau déjà ouvertes par l’utilisateur, nous pouvons également gérer l’événement [FileSystemWatcher](https://docs.microsoft.com/dotnet/api/system.io.filesystemwatcher) et transmettre le chemin d’accès à l’emplacement du fichier. Ainsi, toutes les instances ouvertes de l’application de bureau affichent la photo partagée.

```csharp
...

   FileSystemWatcher watcher = new FileSystemWatcher(Photos.Path);

...

private void Watcher_Created(object sender, FileSystemEventArgs e)
{
    // new file got created, adding it to the list
    Dispatcher.BeginInvoke(System.Windows.Threading.DispatcherPriority.Normal, new Action(() =>
    {
        if (File.Exists(e.FullPath))
        {
            ImageFile item = new ImageFile(e.FullPath);
            Photos.Insert(0, item);
            PhotoListBox.SelectedIndex = 0;
            CurrentPhoto.Source = (BitmapSource)item.Image;
        }
    }));
}

```

## <a name="create-a-background-task"></a>Créer une tâche en arrière-plan

Vous ajouter une tâche en arrière-plan pour exécuter le code, même lorsque l’application est suspendue. Les tâches en arrière-plan sont idéales pour les petites tâches qui ne nécessitent pas d’interaction d’utilisateur. Par exemple, votre tâche peut télécharger des e-mails, afficher une notification toast à propos d’un message de chat entrant, ou réagir à un changement dans une condition du système.

Voici l’exemple d’une application WPF qui inscrit une tâche en arrière-plan.

![tâche en arrière-plan](images/desktop-to-uwp/sample-background-task.png)

La tâche réalise une requête HTTP et mesure le délai nécessaire à l’obtention d’une réponse. Vos tâches seront probablement plus intéressantes, mais cet exemple est idéal pour apprendre les mécanismes de base d’une tâche en arrière-plan.

Consultez l’exemple complet [ici](https://github.com/Microsoft/Windows-Packaging-Samples/tree/master/BGTask).

### <a name="the-design-pattern"></a>Modèle de conception

Pour créer un service en arrière-plan, procédez comme suit :

:one: [Implémenter la tâche en arrière-plan](#implement-task)

:two: [Configurer la tâche en arrière-plan](#configure-background-task)

:three: [Inscrire la tâche en arrière-plan](#register-background-task)

<a id="implement-task"></a>

### <a name="implement-the-background-task"></a>Implémenter la tâche en arrière-plan

Implémentez la tâche en arrière-plan en ajoutant le code à un projet de composant Windows Runtime.

```csharp
public sealed class SiteVerifier : IBackgroundTask
{
    public async void Run(IBackgroundTaskInstance taskInstance)
    {

        taskInstance.Canceled += TaskInstance_Canceled;
        BackgroundTaskDeferral deferral = taskInstance.GetDeferral();
        var msg = await MeasureRequestTime();
        ShowToast(msg);
        deferral.Complete();
    }

    private async Task<string> MeasureRequestTime()
    {
        string msg;
        try
        {
            var url = ApplicationData.Current.LocalSettings.Values["UrlToVerify"] as string;
            var http = new HttpClient();
            Stopwatch clock = Stopwatch.StartNew();
            var response = await http.GetAsync(new Uri(url));
            response.EnsureSuccessStatusCode();
            var elapsed = clock.ElapsedMilliseconds;
            clock.Stop();
            msg = $"{url} took {elapsed.ToString()} ms";
        }
        catch (Exception ex)
        {
            msg = ex.Message;
        }
        return msg;
    }
```

<a id="configure-background-task"></a>

### <a name="configure-the-background-task"></a>Configurer la tâche en arrière-plan

Dans le concepteur de manifeste, ouvrez le fichier **package.appxmanifest** du projet d’empaquetage dans votre solution.

Dans l’onglet **Déclarations**, ajoutez une déclaration **Tâches en arrière-plan**.

![Option de tâche en arrière-plan](images/desktop-to-uwp/background-task-option.png)

Ensuite, choisissez les propriétés souhaitées. Notre exemple utilise la propriété **Minuteur**.

![Propriété Minuteur](images/desktop-to-uwp/timer-property.png)

Fournissez le nom complet de la classe dans votre composant Windows Runtime qui implémente la tâche en arrière-plan.

![Propriété Minuteur](images/desktop-to-uwp/background-task-entry-point.png)

<a id="register-background-task"></a>

### <a name="register-the-background-task"></a>Inscrire la tâche en arrière-plan

Ajoutez le code à votre projet d’application de bureau qui inscrit la tâche en arrière-plan.

```csharp
public void RegisterBackgroundTask(String triggerName)
{
    var current = BackgroundTaskRegistration.AllTasks
        .Where(b => b.Value.Name == triggerName).FirstOrDefault().Value;

    if (current is null)
    {
        BackgroundTaskBuilder builder = new BackgroundTaskBuilder();
        builder.Name = triggerName;
        builder.SetTrigger(new MaintenanceTrigger(15, false));
        builder.TaskEntryPoint = "HttpPing.SiteVerifier";
        builder.Register();
        System.Diagnostics.Debug.WriteLine("BGTask registered:" + triggerName);
    }
    else
    {
        System.Diagnostics.Debug.WriteLine("Task already:" + triggerName);
    }
}
```

## <a name="find-answers-to-your-questions"></a>Trouvez des réponses à vos questions

Des questions ? Contactez-nous sur Stack Overflow. Notre équipe supervise ces [étiquettes](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Vous pouvez également nous poser vos questions [ici](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).
