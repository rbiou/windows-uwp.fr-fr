---
title: Démarrage automatique avec lecture automatique
description: Vous pouvez utiliser la lecture automatique pour proposer votre application en tant qu’option lorsque l’utilisateur connecte un périphérique à son PC. Cela inclut les périphériques autres que les périphériques de volume, tels qu’un appareil photo ou un lecteur multimédia, ou les périphériques de volume tels qu’une clé USB, une carte mémoire SD ou un DVD.
ms.assetid: AD4439EA-00B0-4543-887F-2C1D47408EA7
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ab690fa3964fd9e9c517aedb6adb9e05154fad15
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259462"
---
# <a name="span-iddev_launch_resumeauto-launching_with_autoplayspanauto-launching-with-autoplay"></a><span id="dev_launch_resume.auto-launching_with_autoplay"></span>Lancement automatique avec exécution automatique

Vous pouvez utiliser la **lecture automatique** pour proposer votre application en tant qu’option lorsque l’utilisateur connecte un périphérique à son PC. Cela inclut les périphériques autres que les périphériques de volume, tels qu’un appareil photo ou un lecteur multimédia, ou les périphériques de volume tels qu’une clé USB, une carte mémoire SD ou un DVD. Vous pouvez également utiliser la **lecture automatique** pour proposer votre application en tant qu’option quand des utilisateurs partagent des fichiers entre deux PC à l’aide de la fonction de proximité (appui).

> **Remarque**  si vous êtes fabricant d’appareils et que vous souhaitez associer votre [application d’appareil Microsoft Store](https://msdn.microsoft.com/library/windows/hardware/Dn265154(v=vs.85).aspx) en tant que gestionnaire d' **exécution automatique** pour votre appareil, vous pouvez identifier cette application dans les métadonnées de l’appareil. Pour en savoir plus, voir [Lecture automatique pour les applications pour périphériques du Microsoft Store](https://msdn.microsoft.com/library/windows/hardware/dn265136(v=vs.85).aspx).

## <a name="register-for-autoplay-content"></a>S’inscrire pour le contenu de lecture automatique

Vous pouvez inscrire des applications en tant qu’options pour les événements de contenu de **lecture automatique**. Les événements de contenu de **lecture automatique** se déclenchent lorsqu’un périphérique de volume, tel que la carte mémoire d’un appareil photo, une clé USB ou un DVD, est inséré dans le PC. Voici comment identifier votre application en tant qu’option de **lecture automatique** lorsqu’un périphérique de volume d’un appareil photo est inséré.

Dans ce didacticiel, vous avez créé une application qui affiche des fichiers image ou les copie dans les images. Vous avez inscrit l’application pour l’événement de contenu de lecture automatique **ShowPicturesOnArrival**.

La lecture automatique déclenche également des événements de contenu pour du contenu partagé entre des PC en utilisant la fonction de proximité (action d’appuyer). Vous pouvez utiliser les étapes et le code de cette section pour gérer des fichiers qui sont partagés entre des PC utilisant la fonction de proximité. Le tableau suivant répertorie les événements de contenu de lecture automatique disponibles pour le partage de contenu via la fonction de proximité.

| Action         | Événement de contenu de lecture automatique  |
|----------------|-------------------------|
| Partage de musique  | PlayMusicFilesOnArrival |
| Partage de vidéos | PlayVideoFilesOnArrival |

 
Lorsque des fichiers sont partagés à l’aide de la fonction de proximité, la propriété **Files** de l’objet **FileActivatedEventArgs** contient une référence à un dossier racine reprenant l’intégralité des fichiers partagés.

### <a name="step-1-create-a-new-project-and-add-autoplay-declarations"></a>Étape 1 : Créer un projet et ajouter les déclarations de lecture automatique

1.  Ouvrez Microsoft Visual Studio et sélectionnez **Nouveau projet** dans le menu **Fichier**. Dans la section **Visual C#** , sous **Windows**, sélectionnez **Application vide (Windows universel)** . Attribuez à l’application le nom **AutoPlayDisplayOrCopyImages**, puis cliquez sur **OK**.
2.  Ouvrez le fichier Package.appxmanifest, puis sélectionnez l'onglet **Fonctionnalités**. Choisissez les fonctionnalités **Stockage amovible** et **Bibliothèque d’images**. Cela permet à l’application d’accéder à des périphériques de stockage amovibles pour la mémoire de l’appareil photo, et d’accéder aux images locales.
3.  Dans le fichier manifeste, sélectionnez l’onglet **Déclarations**. Dans la liste déroulante **Déclarations disponibles**, sélectionnez **Contenu de lecture automatique**, puis cliquez sur **Ajouter**. Sélectionnez le nouvel élément **Contenu de lecture automatique** ajouté à la liste **Déclarations prises en charge**.
4.  Une déclaration **Contenu de lecture automatique** identifie votre application en tant qu’option lorsque la lecture automatique déclenche un événement de contenu. L’événement est basé sur le contenu d’un périphérique de volume tel qu’un DVD ou une clé USB. La lecture automatique examine le contenu du périphérique de volume et détermine l’événement de contenu à déclencher. Si la racine du volume contient un dossier DCIM, AVCHD ou PRIVATE\\ACHD, ou si un utilisateur a activé **choisir quoi faire avec chaque type de média** dans le panneau de configuration de l’exécution automatique et que les images se trouvent à la racine du volume, l’exécution automatique déclenche l’événement **ShowPicturesOnArrival** . Dans la section **Actions de lancement**, entrez les valeurs du tableau 1 ci-dessous pour la première action de lancement.
5.  Dans la section **Actions de lancement** correspondant à l’élément **Contenu de lecture automatique**, cliquez sur **Ajouter nouveau** pour ajouter une deuxième action de lancement. Entrez les valeurs suivantes pour la deuxième action de lancement.
6.  Dans la liste déroulante **Déclarations disponibles**, sélectionnez **Associations de types de fichier**, puis cliquez sur **Ajouter**. Dans les propriétés de la nouvelle Déclaration **associations de types de fichier** , définissez le champ **nom complet** sur **lecture automatique ou afficher les images** et le champ **nom** sur **image\_association1**. Dans la section **Types de fichier pris en charge**, cliquez sur **Ajouter nouveau**. Attribuez au champ **Type de fichier** la valeur **.jpg**. Dans la section **Types de fichier pris en charge**, attribuez au champ **Type de fichier** de la nouvelle association de fichiers la valeur **.png**. Pour les événements de contenu, la lecture automatique filtre les types de fichiers qui ne sont pas explicitement associés à votre application.
7.  Enregistrez et fermez le fichier manifeste.

**Tableau 1**

| Paramètre             | Valeur                 |
|---------------------|-----------------------|
| Verb                | show                  |
| Nom complet de l’action | Show Pictures         |
| Événement de contenu       | ShowPicturesOnArrival |

Le paramètre **Nom complet de l’action** identifie la chaîne que la lecture automatique affiche pour votre application. Le paramètre **Verbe** identifie une valeur qui est transmise à votre application pour l’option sélectionnée. Vous pouvez spécifier plusieurs options de lancement pour un événement de lecture automatique et utiliser le paramètre **Verbe** pour déterminer l’option sélectionnée par l’utilisateur pour votre application. Vous pouvez vérifier quelle option a été sélectionnée par l’utilisateur par le biais de la propriété **verb** des arguments d’événement de démarrage transmis à votre application. Vous pouvez attribuer n’importe quelle valeur au paramètre **Verbe**, sauf la valeur **open** qui est réservée.

**Tableau 2**  

| Paramètre             | Valeur                      |
|--------------------:|----------------------------|
| Verb                | copy                       |
| Nom complet de l’action | Copy Pictures Into Library |
| Événement de contenu       | ShowPicturesOnArrival      |

### <a name="step-2-add-xaml-ui"></a>Étape 2 : Ajouter une interface utilisateur XAML

Ouvrez le fichier MainPage.xaml et ajoutez le code XAML suivant à la section &lt;Grid&gt; par défaut.

```xml
<TextBlock FontSize="18">File List</TextBlock>
<TextBlock x:Name="FilesBlock" HorizontalAlignment="Left" TextWrapping="Wrap"
           VerticalAlignment="Top" Margin="0,20,0,0" Height="280" Width="240" />
<Canvas x:Name="FilesCanvas" HorizontalAlignment="Left" VerticalAlignment="Top"
        Margin="260,20,0,0" Height="280" Width="100"/>
```

### <a name="step-3-add-initialization-code"></a>Étape 3 : Ajouter du code d’initialisation

Le code de cette étape vérifie la valeur du verbe dans la propriété **Verb**, qui est l’un des arguments de démarrage transmis à l’application pendant l’événement **OnFileActivated**. Le code appelle alors une méthode associée à l’option que l’utilisateur a sélectionnée. Pour un événement de mémoire d’appareil photo, la lecture automatique passe le dossier racine du stockage de l’appareil photo à l’application. Vous pouvez récupérer ce dossier à partir du premier élément de la propriété **Files**.

Ouvrez le fichier App.xaml.cs et ajoutez le code suivant à la classe **App**.

```cs
protected override void OnFileActivated(FileActivatedEventArgs args)
{
    if (args.Verb == "show")
    {
        Frame rootFrame = (Frame)Window.Current.Content;
        MainPage page = (MainPage)rootFrame.Content;

        // Call DisplayImages with root folder from camera storage.
        page.DisplayImages((Windows.Storage.StorageFolder)args.Files[0]);
    }

    if (args.Verb == "copy")
    {
        Frame rootFrame = (Frame)Window.Current.Content;
        MainPage page = (MainPage)rootFrame.Content;

        // Call CopyImages with root folder from camera storage.
        page.CopyImages((Windows.Storage.StorageFolder)args.Files[0]);
    }

    base.OnFileActivated(args);
}
```

> **Notez**  les méthodes `DisplayImages` et `CopyImages` sont ajoutées dans les étapes suivantes.

### <a name="step-4-add-code-to-display-images"></a>Étape 4 : Ajouter du code pour afficher des images

Dans le fichier MainPage.xaml.cs, ajoutez le code suivant à la classe **MainPage**.

```cs
async internal void DisplayImages(Windows.Storage.StorageFolder rootFolder)
{
    // Display images from first folder in root\DCIM.
    var dcimFolder = await rootFolder.GetFolderAsync("DCIM");
    var folderList = await dcimFolder.GetFoldersAsync();
    var cameraFolder = folderList[0];
    var fileList = await cameraFolder.GetFilesAsync();
    for (int i = 0; i < fileList.Count; i++)
    {
        var file = (Windows.Storage.StorageFile)fileList[i];
        WriteMessageText(file.Name + "\n");
        DisplayImage(file, i);
    }
}

async private void DisplayImage(Windows.Storage.IStorageItem file, int index)
{
    try
    {
        var sFile = (Windows.Storage.StorageFile)file;
        Windows.Storage.Streams.IRandomAccessStream imageStream =
            await sFile.OpenAsync(Windows.Storage.FileAccessMode.Read);
        Windows.UI.Xaml.Media.Imaging.BitmapImage imageBitmap =
            new Windows.UI.Xaml.Media.Imaging.BitmapImage();
        imageBitmap.SetSource(imageStream);
        var element = new Image();
        element.Source = imageBitmap;
        element.Height = 100;
        Thickness margin = new Thickness();
        margin.Top = index * 100;
        element.Margin = margin;
        FilesCanvas.Children.Add(element);
    }
    catch (Exception e)
    {
       WriteMessageText(e.Message + "\n");
    }
}

// Write a message to MessageBlock on the UI thread.
private Windows.UI.Core.CoreDispatcher messageDispatcher = Window.Current.CoreWindow.Dispatcher;

private async void WriteMessageText(string message, bool overwrite = false)
{
    await messageDispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
        () =>
        {
            if (overwrite)
                FilesBlock.Text = message;
            else
                FilesBlock.Text += message;
        });
}
```

### <a name="step-5-add-code-to-copy-images"></a>Étape 5 : Ajouter du code pour copier des images

Dans le fichier MainPage.xaml.cs, ajoutez le code suivant à la classe **MainPage**.

```cs
async internal void CopyImages(Windows.Storage.StorageFolder rootFolder)
{
    // Copy images from first folder in root\DCIM.
    var dcimFolder = await rootFolder.GetFolderAsync("DCIM");
    var folderList = await dcimFolder.GetFoldersAsync();
    var cameraFolder = folderList[0];
    var fileList = await cameraFolder.GetFilesAsync();

    try
    {
        var folderName = "Images " + DateTime.Now.ToString("yyyy-MM-dd HHmmss");
        Windows.Storage.StorageFolder imageFolder = await
            Windows.Storage.KnownFolders.PicturesLibrary.CreateFolderAsync(folderName);

        foreach (Windows.Storage.IStorageItem file in fileList)
        {
            CopyImage(file, imageFolder);
        }
    }
    catch (Exception e)
    {
        WriteMessageText("Failed to copy images.\n" + e.Message + "\n");
    }
}

async internal void CopyImage(Windows.Storage.IStorageItem file,
                              Windows.Storage.StorageFolder imageFolder)
{
    try
    {
        Windows.Storage.StorageFile sFile = (Windows.Storage.StorageFile)file;
        await sFile.CopyAsync(imageFolder, sFile.Name);
        WriteMessageText(sFile.Name + " copied.\n");
    }
    catch (Exception e)
    {
        WriteMessageText("Failed to copy file.\n" + e.Message + "\n");
    }
}
```

### <a name="step-6-build-and-run-the-app"></a>Étape 6 : Générer et exécuter l’application

1.  Appuyez sur F5 pour créer et déployer l’application (en mode débogage).
2.  Pour exécuter votre application, insérez une carte mémoire d’appareil photo ou un autre périphérique de stockage d’un appareil photo dans votre PC. Sélectionnez ensuite l’une des options d’événement de contenu que vous avez spécifiées dans votre fichier package.appxmanifest à partir de la liste d’options de la lecture automatique. Cet exemple de code affiche ou copie uniquement les images du dossier DCIM de la carte mémoire d’un appareil photo. Si la carte mémoire de votre appareil photo stocke des images dans un dossier AVCHD ou privé\\ACHD, vous devez mettre à jour le code en conséquence.
    **Remarque**  si vous n’avez pas de carte mémoire pour appareil photo, vous pouvez utiliser un lecteur flash si celui-ci contient un dossier nommé **DCIM** à la racine et si le dossier DCIM contient un sous-dossier qui contient des images.

## <a name="register-for-an-autoplay-device"></a>S’inscrire pour un appareil de lecture automatique


Vous pouvez inscrire des applications en tant qu’options pour les événements de périphérique de **lecture automatique**. Les événements de périphérique de **lecture automatique** sont déclenchés lorsqu’un périphérique est connecté à un PC.

Voici comment identifier votre application en tant qu’option de **lecture automatique** lorsqu’un appareil photo est connecté à un PC. L’application s’inscrit en tant que gestionnaire pour l’événement **WPD\\ImageSourceAutoPlay** . Il s’agit d’un événement courant que le système WPD (Windows Portable Device) déclenche lorsque des appareils photo et d’autres périphériques d’acquisition d’images l’avertissent qu’ils sont une source d’image (ImageSource) utilisant MTP. Pour plus d’informations, voir [Appareils mobiles Windows](https://docs.microsoft.com/previous-versions/ff597729(v=vs.85)).

**Important**  les API [**Windows. Devices. portable. StorageDevice**](https://docs.microsoft.com/uwp/api/Windows.Devices.Portable.StorageDevice) font partie de la [famille des appareils de bureau](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide). Les applications peuvent utiliser ces API uniquement sur les appareils Windows 10 de la famille des appareils de bureau, tels que les PC.

 

### <a name="step-1-create-a-new-project-and-add-autoplay-declarations"></a>Étape 1 : Créer un projet et ajouter les déclarations de lecture automatique

1.  Ouvrez Visual Studio et sélectionnez **Nouveau projet** dans le menu **Fichier**. Dans la section **Visual C#** , sous **Windows**, sélectionnez **Application vide (Windows universel)** . Nommez l’application **AutoPlayDevice\_Camera** , puis cliquez sur **OK.**
2.  Ouvrez le fichier Package.appxmanifest, puis sélectionnez l'onglet **Fonctionnalités**. Choisissez la fonctionnalité **Stockage amovible**. Cela permet à l’application d’accéder aux données situées sur l’appareil photo en tant que périphérique de volume de stockage amovible.
3.  Dans le fichier manifeste, sélectionnez l’onglet **Déclarations**. Dans la liste déroulante **Déclarations disponibles**, sélectionnez **Périphérique de lecture automatique**, puis cliquez sur **Ajouter**. Sélectionnez le nouvel élément **Périphérique de lecture automatique** ajouté à la liste **Déclarations prises en charge**.
4.  Une déclaration **Périphérique de lecture automatique** identifie votre application en tant qu’option lorsque la lecture automatique déclenche un événement de périphérique pour des événements connus. Dans la section **Actions de lancement**, entrez les valeurs suivantes pour la première action de lancement.
5.  Dans la liste déroulante **Déclarations disponibles**, sélectionnez **Associations de types de fichier**, puis cliquez sur **Ajouter**. Dans les propriétés de la nouvelle Déclaration **associations de types de fichier** , définissez le champ **nom complet** sur **afficher les images de l’appareil photo** et le champ **nom** sur **caméra\_association1**. Dans la section **Types de fichier pris en charge**, cliquez sur **Ajouter nouveau** (si nécessaire). Attribuez au champ **Type de fichier** la valeur **.jpg**. Dans la section **Types de fichier pris en charge**, cliquez à nouveau sur **Ajouter nouveau**. Attribuez au champ **Type de fichier** de la nouvelle association de fichiers la valeur **.png**. Pour les événements de contenu, la lecture automatique filtre les types de fichiers qui ne sont pas explicitement associés à votre application.
6.  Enregistrez et fermez le fichier manifeste.

| Paramètre             | Valeur            |
|---------------------|------------------|
| Verb                | show             |
| Nom complet de l’action | Show Pictures    |
| Événement de contenu       | WPD\\ImageSource |

Le paramètre **Nom complet de l’action** identifie la chaîne que la lecture automatique affiche pour votre application. Le paramètre **Verbe** identifie une valeur qui est transmise à votre application pour l’option sélectionnée. Vous pouvez spécifier plusieurs options de lancement pour un événement de lecture automatique et utiliser le paramètre **Verbe** pour déterminer l’option sélectionnée par l’utilisateur pour votre application. Vous pouvez vérifier quelle option a été sélectionnée par l’utilisateur par le biais de la propriété **verb** des arguments d’événement de démarrage transmis à votre application. Vous pouvez attribuer n’importe quelle valeur au paramètre **Verbe**, sauf la valeur **open** qui est réservée. Pour obtenir un exemple d’utilisation de plusieurs verbes dans une même application, voir [S’inscrire pour le contenu de lecture automatique](#register-for-autoplay-content).

### <a name="step-2-add-assembly-reference-for-the-desktop-extensions"></a>Étape 2 : Ajouter la référence d’assembly pour les extensions de bureau

Les API nécessaires pour accéder au stockage d’un appareil mobile Windows, [**Windows.Devices.Portable.StorageDevice**](https://docs.microsoft.com/uwp/api/Windows.Devices.Portable.StorageDevice), font partie de la [famille des appareils de bureau](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide). Cela signifie qu’un assembly spécial est requis pour utiliser les API et ces appels fonctionneront uniquement sur un appareil de la famille des appareils de bureau (par exemple un PC).

1.  Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur **Références**, puis cliquez sur **Ajouter une référence...** .
2.  Développez **Windows universel** et cliquez sur **Extensions**.
3.  Sélectionnez ensuite **Extensions de bureau Windows pour UWP** et cliquez sur **OK**.

### <a name="step-3-add-xaml-ui"></a>Étape 3 : Ajouter une interface utilisateur XAML

Ouvrez le fichier MainPage.xaml et ajoutez le code XAML suivant à la section &lt;Grid&gt; par défaut.

```xml
<StackPanel Orientation="Vertical" Margin="10,0,-10,0">
    <TextBlock FontSize="24">Device Information</TextBlock>
    <StackPanel Orientation="Horizontal">
        <TextBlock x:Name="DeviceInfoTextBlock" FontSize="18" Height="400" Width="400" VerticalAlignment="Top" />
        <ListView x:Name="ImagesList" HorizontalAlignment="Left" Height="400" VerticalAlignment="Top" Width="400">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <StackPanel Orientation="Vertical">
                        <Image Source="{Binding Path=Source}" />
                        <TextBlock Text="{Binding Path=Name}" />
                    </StackPanel>
                </DataTemplate>
            </ListView.ItemTemplate>
            <ListView.ItemsPanel>
                <ItemsPanelTemplate>
                    <WrapGrid Orientation="Horizontal" ItemHeight="100" ItemWidth="120"></WrapGrid>
                </ItemsPanelTemplate>
            </ListView.ItemsPanel>
        </ListView>
    </StackPanel>
</StackPanel>
```

### <a name="step-4-add-activation-code"></a>Étape 4 : Ajouter du code d’activation

Le code de cette étape fait référence à l’appareil photo en tant que [**StorageDevice**](https://docs.microsoft.com/uwp/api/Windows.Devices.Portable.StorageDevice) en transmettant l’ID des informations de périphérique de l’appareil photo à la méthode [**FromId**](https://docs.microsoft.com/uwp/api/windows.devices.portable.storagedevice.fromid). L’ID des informations de périphérique de l’appareil photo est obtenu en diffusant tout d’abord des arguments de l’événement en tant que [**DeviceActivatedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.DeviceActivatedEventArgs), puis en obtenant la valeur de la propriété [**DeviceInformationId**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.deviceactivatedeventargs.deviceinformationid).

Ouvrez le fichier App.xaml.cs et ajoutez le code suivant à la classe **App**.

```cs
protected override void OnActivated(IActivatedEventArgs args)
{
   if (args.Kind == ActivationKind.Device)
   {
      Frame rootFrame = null;
      // Ensure that the current page exists and is activated
      if (Window.Current.Content == null)
      {
         rootFrame = new Frame();
         rootFrame.Navigate(typeof(MainPage));
         Window.Current.Content = rootFrame;
      }
      else
      {
         rootFrame = Window.Current.Content as Frame;
      }
      Window.Current.Activate();

      // Make sure the necessary APIs are present on the device
      bool storageDeviceAPIPresent =
      Windows.Foundation.Metadata.ApiInformation.IsTypePresent("Windows.Devices.Portable.StorageDevice");

      if (storageDeviceAPIPresent)
      {
         // Reference the current page as type MainPage
         var mPage = rootFrame.Content as MainPage;

         // Cast the activated event args as DeviceActivatedEventArgs and show images
         var deviceArgs = args as DeviceActivatedEventArgs;
         if (deviceArgs != null)
         {
            mPage.ShowImages(Windows.Devices.Portable.StorageDevice.FromId(deviceArgs.DeviceInformationId));
         }
      }
      else
      {
         // Handle case where APIs are not present (when the device is not part of the desktop device family)
      }

   }

   base.OnActivated(args);
}
```

> **Notez**  la méthode `ShowImages` est ajoutée à l’étape suivante.

### <a name="step-5-add-code-to-display-device-information"></a>Étape 5 : Ajouter du code pour afficher des informations de périphérique

Vous pouvez obtenir des informations sur l’appareil photo à partir des propriétés de la classe [**StorageDevice**](https://docs.microsoft.com/uwp/api/Windows.Devices.Portable.StorageDevice). Le code de cette étape affiche le nom du périphérique et d’autres informations pour l’utilisateur au moment de l’exécution de l’application. Le code appelle alors les méthodes GetImageList et GetThumbnail, que vous ajouterez à l’étape suivante, pour afficher des miniatures des images stockées dans l’appareil photo.

Dans le fichier MainPage.xaml.cs, ajoutez le code suivant à la classe **MainPage**.

```cs
private Windows.Storage.StorageFolder rootFolder;

internal async void ShowImages(Windows.Storage.StorageFolder folder)
{
    DeviceInfoTextBlock.Text = "Display Name = " + folder.DisplayName + "\n";
    DeviceInfoTextBlock.Text += "Display Type =  " + folder.DisplayType + "\n";
    DeviceInfoTextBlock.Text += "FolderRelativeId = " + folder.FolderRelativeId + "\n";

    // Reference first folder of the device as the root
    rootFolder = (await folder.GetFoldersAsync())[0];
    var imageList = await GetImageList(rootFolder);

    foreach (Windows.Storage.StorageFile img in imageList)
    {
        ImagesList.Items.Add(await GetThumbnail(img));
    }
}
```

> **Notez**  les méthodes `GetImageList` et `GetThumbnail` sont ajoutées à l’étape suivante.

### <a name="step-6-add-code-to-display-images"></a>Étape 6 : Ajouter du code pour afficher des images

Le code de cette étape affiche des miniatures des images stockées dans l’appareil photo. Le code effectue des appels asynchrones à l’appareil photo pour obtenir l’image miniature. Toutefois, l’appel asynchrone suivant ne se produit qu’une fois l’appel asynchrone précédent terminé. Cela garantit qu’une seule demande à la fois est effectuée auprès de l’appareil photo.

Dans le fichier MainPage.xaml.cs, ajoutez le code suivant à la classe **MainPage**.

```cs
async private System.Threading.Tasks.Task<List<Windows.Storage.StorageFile>> GetImageList(Windows.Storage.StorageFolder folder)
{
    var result = await folder.GetFilesAsync();
    var subFolders = await folder.GetFoldersAsync();
    foreach (Windows.Storage.StorageFolder f in subFolders)
        result = result.Union(await GetImageList(f)).ToList();

    return (from f in result orderby f.Name select f).ToList();
}

async private System.Threading.Tasks.Task<Image> GetThumbnail(Windows.Storage.StorageFile img)
{
    // Get the thumbnail to display
    var thumbnail = await img.GetThumbnailAsync(Windows.Storage.FileProperties.ThumbnailMode.SingleItem,
                                                100,
                                                Windows.Storage.FileProperties.ThumbnailOptions.UseCurrentScale);

    // Create a XAML Image object bind to on the display page
    var result = new Image();
    result.Height = thumbnail.OriginalHeight;
    result.Width = thumbnail.OriginalWidth;
    result.Name = img.Name;
    var imageBitmap = new Windows.UI.Xaml.Media.Imaging.BitmapImage();
    imageBitmap.SetSource(thumbnail);
    result.Source = imageBitmap;

    return result;
}
```

### <a name="step-7-build-and-run-the-app"></a>Étape 7 : Générer et exécuter l’application

1.  Appuyez sur F5 pour créer et déployer l’application (en mode débogage).
2.  Pour exécuter votre application, connectez un appareil photo à votre ordinateur. Sélectionnez ensuite l’application dans la liste d’options de la lecture automatique.
    **Notez**  les caméras ne sont pas publiées pour l’événement d’appareil de lecture automatique **wpd\\ImageSource** .

## <a name="configure-removable-storage"></a>Configurer le stockage amovible

Vous pouvez identifier un périphérique de volume, tel qu’une carte mémoire ou une clé USB, comme périphérique de **lecture automatique** lorsque le périphérique de volume est connecté à un PC. Cette fonctionnalité est particulièrement utile lorsque vous voulez associer une application spécifique pour la **lecture automatique** à présenter à l’utilisateur pour votre périphérique de volume.

Voici comment identifier votre périphérique de volume comme périphérique de **lecture automatique**.

Pour identifier votre périphérique de volume comme périphérique de **lecture automatique**, ajoutez un fichier autorun.inf au lecteur racine de votre périphérique. Dans le fichier autorun.inf, ajoutez une clé **CustomEvent** à la section **AutoRun**. Lorsque votre périphérique de volume se connecte à un PC, la **lecture automatique** trouve le fichier autorun.inf et traite votre volume comme un périphérique. La **lecture automatique** crée un événement de **lecture automatique** à l’aide du nom que vous avez fourni pour la clé **CustomEvent**. Vous pouvez alors créer une application et l’inscrire en tant que gestionnaire de cet événement de **lecture automatique**. Lorsque le périphérique est connecté au PC, la **lecture automatique** affiche votre application comme gestionnaire de votre périphérique de volume. Pour plus d’informations sur les fichiers autorun.inf, voir [Entrées du fichier Autorun.inf](https://docs.microsoft.com/windows/desktop/shell/autorun-cmds).

### <a name="step-1-create-an-autoruninf-file"></a>Étape 1 : Créer un fichier autorun.inf

Dans le lecteur racine de votre périphérique de volume, ajoutez un fichier nommé autorun.inf. Ouvrez le fichier autorun.inf et ajoutez le texte suivant.

``` syntax
[AutoRun]
CustomEvent=AutoPlayCustomEventQuickstart
```

### <a name="step-2-create-a-new-project-and-add-autoplay-declarations"></a>Étape 2 : Créer un projet et ajouter les déclarations de lecture automatique

1.  Ouvrez Visual Studio et sélectionnez **Nouveau projet** dans le menu **Fichier**. Dans la section **Visual C#** , sous **Windows**, sélectionnez **Application vide (Windows universel)** . Nommez l’application **AutoPlayCustomEvent** et cliquez sur **OK**.
2.  Ouvrez le fichier Package.appxmanifest, puis sélectionnez l'onglet **Fonctionnalités**. Choisissez la fonctionnalité **Stockage amovible**. Cela permet à l’application d’accéder aux fichiers et dossiers situés sur les périphériques de stockage amovibles.
3.  Dans le fichier manifeste, sélectionnez l’onglet **Déclarations**. Dans la liste déroulante **Déclarations disponibles**, sélectionnez **Contenu de lecture automatique**, puis cliquez sur **Ajouter**. Sélectionnez le nouvel élément **Contenu de lecture automatique** ajouté à la liste **Déclarations prises en charge**.

    **Notez**  également, vous pouvez également choisir d’ajouter une déclaration d' **appareil de lecture automatique** pour votre événement d’exécution automatique personnalisé.  
4.  Dans la section **Actions de lancement** de votre déclaration d’événement **Contenu de lecture automatique**, entrez les valeurs suivantes pour la première action de lancement.
5.  Dans la liste déroulante **Déclarations disponibles**, sélectionnez **Associations de types de fichier**, puis cliquez sur **Ajouter**. Dans les propriétés de la nouvelle Déclaration **associations de types de fichier** , définissez le champ nom d' **affichage** sur **afficher les fichiers. ms** et le champ **nom** sur **MS\_Association**. Dans la section **Types de fichier pris en charge**, cliquez sur **Ajouter nouveau**. Attribuez au champ **Type de fichier** la valeur **.ms**. Pour les événements de contenu, la lecture automatique filtre les types de fichiers qui ne sont pas explicitement associés à votre application.
6.  Enregistrez et fermez le fichier manifeste.

| Paramètre             | Valeur                         |
|---------------------|-------------------------------|
| Verb                | show                          |
| Nom complet de l’action | Afficher les fichiers                    |
| Événement de contenu       | AutoPlayCustomEventQuickstart |

La valeur **Événement de contenu** correspond au texte que vous avez fourni pour la clé **CustomEvent** dans votre fichier autorun.inf. Le paramètre **Nom complet de l’action** identifie la chaîne que la lecture automatique affiche pour votre application. Le paramètre **Verbe** identifie une valeur qui est transmise à votre application pour l’option sélectionnée. Vous pouvez spécifier plusieurs options de lancement pour un événement de lecture automatique et utiliser le paramètre **Verbe** pour déterminer l’option sélectionnée par l’utilisateur pour votre application. Vous pouvez vérifier quelle option a été sélectionnée par l’utilisateur par le biais de la propriété **verb** des arguments d’événement de démarrage transmis à votre application. Vous pouvez attribuer n’importe quelle valeur au paramètre **Verbe**, sauf la valeur **open** qui est réservée.

### <a name="step-3-add-xaml-ui"></a>Étape 3 : Ajouter une interface utilisateur XAML

Ouvrez le fichier MainPage.xaml et ajoutez le code XAML suivant à la section &lt;Grid&gt; par défaut.

```xml
<StackPanel Orientation="Vertical">
    <TextBlock FontSize="28" Margin="10,0,800,0">Files</TextBlock>
    <TextBlock x:Name="FilesBlock" FontSize="22" Height="600" Margin="10,0,800,0" />
</StackPanel>
```

### <a name="step-4-add-activation-code"></a>Étape 4 : Ajouter du code d’activation

Dans les étapes suivantes, le code appelle une méthode destinée à afficher les dossiers présents dans le lecteur racine de votre périphérique de volume. Pour les événements de contenu de lecture automatique, la lecture automatique transmet le dossier racine du dispositif de stockage aux arguments de démarrage qui sont transmis à l’application pendant l’événement **OnFileActivated**. Vous pouvez récupérer ce dossier à partir du premier élément de la propriété **Files**.

Ouvrez le fichier App.xaml.cs et ajoutez le code suivant à la classe **App**.

```cs
protected override void OnFileActivated(FileActivatedEventArgs args)
{
    var rootFrame = Window.Current.Content as Frame;
    var page = rootFrame.Content as MainPage;

    // Call ShowFolders with root folder from device storage.
    page.DisplayFiles(args.Files[0] as Windows.Storage.StorageFolder);

    base.OnFileActivated(args);
}
```

> **Notez**  la méthode `DisplayFiles` est ajoutée à l’étape suivante.

 

### <a name="step-5-add-code-to-display-folders"></a>Étape 5 : Ajouter du code pour afficher des dossiers

Dans le fichier MainPage.xaml.cs, ajoutez le code suivant à la classe **MainPage**.

```cs
internal async void DisplayFiles(Windows.Storage.StorageFolder folder)
{
    foreach (Windows.Storage.StorageFile f in await ReadFiles(folder, ".ms"))
    {
        FilesBlock.Text += "  " + f.Name + "\n";
    }
}

internal async System.Threading.Tasks.Task<IReadOnlyList<Windows.Storage.StorageFile>>
    ReadFiles(Windows.Storage.StorageFolder folder, string fileExtension)
{
    var options = new Windows.Storage.Search.QueryOptions();
    options.FileTypeFilter.Add(fileExtension);
    var query = folder.CreateFileQueryWithOptions(options);
    var files = await query.GetFilesAsync();

    return files;
}
```

### <a name="step-6-build-and-run-the-qpp"></a>Étape 6 : Générer et exécuter l’application

1.  Appuyez sur F5 pour créer et déployer l’application (en mode débogage).
2.  Pour exécuter votre application, insérez une carte mémoire ou un autre périphérique de stockage dans votre PC. Sélectionnez ensuite votre application dans la liste des options de gestionnaire de lecture automatique.

## <a name="autoplay-event-reference"></a>Référence des événements de lecture automatique


Le système de **lecture automatique** permet aux applications de s’inscrire pour de nombreux événements d’arrivée de périphérique et de volume (disque). Pour pouvoir les inscrire à des événements de contenu de **lecture automatique**, vous devez activer la fonctionnalité **Stockage amovible** dans le manifeste du package. Ce tableau affiche les événements auxquels vous pouvez vous inscrire au moment où ils sont déclenchés.

| Scénario                                                           | Événement                              | Description   |
|--------------------------------------------------------------------|------------------------------------|---------------|
| Utilisation de photos sur un appareil photo                                           | **WPD\ImageSource**                | Événement déclenché pour des appareils photo identifiés comme des appareils mobiles Windows et dotés de la fonctionnalité ImageSource. |
| Utilisation de musique sur un lecteur audio                                     | **WPD\AudioSource**                | Événement déclenché pour des lecteurs multimédias identifiés comme des appareils mobiles Windows et dotés de la fonctionnalité AudioSource. |
| Utilisation de vidéos dans une caméra vidéo                                     | **WPD\VideoSource**                | Événement déclenché pour des caméras vidéo identifiées comme des appareils mobiles Windows et dotées de la fonctionnalité VideoSource. |
| Accéder à un disque mémoire flash ou un disque dur externe              | **StorageOnArrival**               | Événement déclenché lorsqu’un lecteur ou un volume est connecté au PC.   Si le lecteur ou le volume contient un dossier DCIM, AVCHD ou PRIVATE\ACHD à la racine du disque, l’événement **ShowPicturesOnArrival** est déclenché à la place. |
| Utilisation de photos situées sur un dispositif de stockage de masse (hérité)                            | **ShowPicturesOnArrival**          | Événement déclenché lorsqu’un lecteur ou un volume contient un dossier DCIM, AVCHD ou PRIVATE\ACHD à la racine du disque. Si un utilisateur a activé l’option **Choisir l’action pour chaque type de média** dans la section Lecture automatique du Panneau de configuration, la lecture automatique examine un volume connecté au PC pour déterminer le type de contenu du disque. Si des images sont trouvées, un événement **ShowPicturesOnArrival** est déclenché. |
| Réception de photos via le partage de proximité (appui et envoi)             | **ShowPicturesOnArrival**          | Lorsque des utilisateurs transmettent du contenu à l’aide de la fonction de proximité (appui et envoi), la lecture automatique examine les fichiers partagés pour déterminer le type de contenu. Si des images sont trouvées, un événement **ShowPicturesOnArrival** est déclenché. |
| Utilisation de musique située sur un dispositif de stockage de masse (hérité)                             | **PlayMusicFilesOnArrival**        | Si un utilisateur a activé l’option **Choisir l’action pour chaque type de média** dans la section Lecture automatique du Panneau de configuration, la lecture automatique examine un volume connecté au PC pour déterminer le type de contenu du disque.  Si des fichiers de musique sont trouvés, un événement **PlayMusicFilesOnArrival** est déclenché. |
| Réception de musique via le partage de proximité (appui et envoi)              | **PlayMusicFilesOnArrival**        | Lorsque des utilisateurs transmettent du contenu à l’aide de la fonction de proximité (appui et envoi), la lecture automatique examine les fichiers partagés pour déterminer le type de contenu. Si des fichiers de musique sont trouvés, un événement **PlayMusicFilesOnArrival** est déclenché. |
| Utilisation de vidéos situées sur un dispositif de stockage de masse (hérité)                            | **PlayVideoFilesOnArrival**        | Si un utilisateur a activé l’option **Choisir l’action pour chaque type de média** dans la section Lecture automatique du Panneau de configuration, la lecture automatique examine un volume connecté au PC pour déterminer le type de contenu du disque. Si des fichiers vidéo sont trouvés, un événement **PlayVideoFilesOnArrival** est déclenché. |
| Réception de vidéos via le partage de proximité (appui et envoi)             | **PlayVideoFilesOnArrival**        | Lorsque des utilisateurs transmettent du contenu à l’aide de la fonction de proximité (appui et envoi), la lecture automatique examine les fichiers partagés pour déterminer le type de contenu. Si des fichiers vidéo sont trouvés, un événement **PlayVideoFilesOnArrival** est déclenché. |
| Gestion d’ensembles mixtes de fichiers depuis un périphérique connecté               | **MixedContentOnArrival**          | Si un utilisateur a activé l’option **Choisir l’action pour chaque type de média** dans la section Lecture automatique du Panneau de configuration, la lecture automatique examine un volume connecté au PC pour déterminer le type de contenu du disque. Si aucun type de contenu spécifique n’est trouvé (par exemple, des images), un événement **MixedContentOnArrival** est déclenché. |
| Gestion d’ensembles mixtes de fichiers via le partage de proximité (appui et envoi) | **MixedContentOnArrival**          | Lorsque des utilisateurs transmettent du contenu à l’aide de la fonction de proximité (appui et envoi), la lecture automatique examine les fichiers partagés pour déterminer le type de contenu. Si aucun type de contenu spécifique n’est trouvé (par exemple, des images), un événement **MixedContentOnArrival** est déclenché. |
| Gérer de la vidéo depuis un média optique                                    | **PlayDVDMovieOnArrival**<br />**PlayBluRayOnArrival**<br />**PlayVideoCDMovieOnArrival**<br />**PlaySuperVideoCDMovieOnArrival** | Lorsqu’un disque est inséré dans le lecteur optique, la lecture automatique examine les fichiers pour déterminer le type de contenu. Si des fichiers vidéos sont trouvés, l’événement correspondant au type de disque optique est déclenché. |
| Gérer de la musique depuis un média optique                                    | **PlayCDAudioOnArrival**<br />**PlayDVDAudioOnArrival** | Lorsqu’un disque est inséré dans le lecteur optique, la lecture automatique examine les fichiers pour déterminer le type de contenu. Si des fichiers de musique sont trouvés, l’événement correspondant au type de disque optique est déclenché. |
| Lire des disques optimisés                                                | **PlayEnhancedCDOnArrival**<br />**PlayEnhancedDVDOnArrival** | Lorsqu’un disque est inséré dans le lecteur optique, la lecture automatique examine les fichiers pour déterminer le type de contenu. Si un disque amélioré est trouvé, l’événement correspondant au type de disque optique est déclenché. |
| Gérer des disques optiques inscriptibles                                     | **HandleCDBurningOnArrival** <br />**HandleDVDBurningOnArrival** <br />**HandleBDBurningOnArrival** | Lorsqu’un disque est inséré dans le lecteur optique, la lecture automatique examine les fichiers pour déterminer le type de contenu. Si un disque inscriptible est trouvé, l’événement correspondant au type de disque optique est déclenché. |
| Gérer un autre appareil ou une connexion de volume                       | **UnknownContentOnArrival**        | Événement déclenché pour tous les événements si du contenu ne correspondant à aucun événement de contenu de lecture automatique est trouvé. L’utilisation de cet événement n’est pas recommandée. Vous devez seulement inscrire votre application aux événements de lecture automatique spécifiques qu’elle est capable de gérer. |

Vous pouvez spécifier que la lecture automatique déclenche un événement de contenu de lecture automatique personnalisé à l’aide de l’entrée **CustomEvent** dans le fichier autorun.inf d’un volume. Pour plus d’informations, voir [Entrées du fichier Autorun.inf](https://docs.microsoft.com/windows/desktop/shell/autorun-cmds).

Vous pouvez inscrire votre application en tant que gestionnaire d’événements Contenu de lecture automatique ou Périphérique de lecture automatique en ajoutant une extension au fichier package.appxmanifest de votre application. Si vous utilisez Visual Studio, vous pouvez ajouter une déclaration **Contenu de lecture automatique** ou **Périphérique de lecture automatique** dans l'onglet **Déclarations**. Si vous modifiez le fichier package.appxmanifest de votre application directement, ajoutez un élément [**Extension**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-1-extension) au manifeste de votre package qui indique **windows.autoPlayContent** ou **windows.autoPlayDevice** en guise de **Catégorie** . Par exemple, l’entrée suivante dans le manifeste du package ajoute une extension **Contenu de lecture automatique** pour inscrire l’application en tant que gestionnaire de l’événement **ShowPicturesOnArrival**.

```xml
  <Applications>
    <Application Id="AutoPlayHandlerSample.App">
      <Extensions>
        <Extension Category="windows.autoPlayContent">
          <AutoPlayContent>
            <LaunchAction Verb="show" ActionDisplayName="Show Pictures"
                          ContentEvent="ShowPicturesOnArrival" />
          </AutoPlayContent>
        </Extension>
      </Extensions>
    </Application>
  </Applications>
```

 

 
