---
ms.assetid: 414ACC73-2A72-465C-BD15-1B51CB2334F2
title: Télécharger et installer des mises à jour de package sur le Store
description: Apprenez à marquer des packages comme obligatoires dans l’Espace partenaires et à écrire du code dans votre application pour télécharger et installer des mises à jour de packages.
ms.date: 04/04/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: cb1ac05bdc5dcaaf31074f1b89e5bbb35e4f850d
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/29/2020
ms.locfileid: "68682727"
---
# <a name="download-and-install-package-updates-from-the-store"></a>Télécharger et installer des mises à jour de package sur le Store

À compter de la version 1607 de Windows 10, vous pouvez utiliser les méthodes de la classe [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) dans l’espace de noms [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) pour vérifier par programmation les mises à jour pour l’application actuelle à partir du Microsoft Store, puis télécharger et installer les packages mis à jour. Vous pouvez également rechercher les packages que vous avez marqués comme obligatoires dans l’Espace partenaires, et désactiver des fonctionnalités de l’application jusqu’à ce que la mise à jour obligatoire soit installée.

D’autres méthodes [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) introduites dans la version 1803 de Windows 10 vous permettent de télécharger et d’installer les mises à jour de package silencieusement (sans afficher de notification à l’utilisateur dans l’interface utilisateur), de désinstaller un [package facultatif](/windows/msix/package/optional-packages) et d’obtenir les informations relatives aux packages dans la file d’attente de téléchargement et d’installation de votre application.

Ces fonctionnalités vous permettent de maintenir à jour votre base d’utilisateurs avec la dernière version de votre application, des packages facultatifs et des services associés dans le Store.

## <a name="download-and-install-package-updates-with-the-users-permission"></a>Télécharger et installer les mises à jour de package avec l’autorisation de l’utilisateur

Cet exemple de code montre comment utiliser la méthode [GetAppAndOptionalStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getappandoptionalstorepackageupdatesasync) pour découvrir toutes les mises à jour de package disponibles depuis le Store, puis appeler la méthode [RequestDownloadAndInstallStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestdownloadandinstallstorepackageupdatesasync) pour télécharger et installer les mises à jour. Lors de l’utilisation de cette méthode de téléchargement et d’installation des mises à jour, le système d’exploitation affiche une boîte de dialogue qui invite l’utilisateur à donner son autorisation avant de télécharger les mises à jour.

> [!NOTE]
> Ces méthodes prennent en charge les [packages facultatifs](/windows/msix/package/optional-packages) et obligatoires pour votre application. Les packages facultatifs sont utiles pour les modules complémentaires de contenu téléchargeable (DLC), la division d’une application de grande taille en réponse à des contraintes de taille, et la livraison de contenu supplémentaire distinct de votre application principale. Pour obtenir l’autorisation de soumettre une application utilisant des packages facultatifs (notamment les modules complémentaires DLC) sur le Store, consultez [Support technique pour les développeurs Windows](https://developer.microsoft.com/windows/support).

Cet exemple de code se base sur les hypothèses suivantes :

* Le code s’exécute dans le contexte d’une [Page](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page).
* La **Page** contient un élément [ProgressBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.progressbar) nommé ```downloadProgressBar``` pour fournir l’état de l’opération de téléchargement.
* Le fichier de code contient une instruction **using** pour les espaces de noms **Windows.Services.Store**, **Windows.Threading.Tasks** et **Windows.UI.Popups**.
* Cette application mono-utilisateur ne s’exécute que dans le contexte de l’utilisateur qui l’a lancée. Pour une [application multi-utilisateur](https://docs.microsoft.com/windows/uwp/xbox-apps/multi-user-applications), utilisez la méthode [GetForUser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.User) plutôt que la méthode [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.GetDefault) pour obtenir un objet **StoreContext**.

```csharp
private StoreContext context = null;

public async Task DownloadAndInstallAllUpdatesAsync()
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    // Get the updates that are available.
    IReadOnlyList<StorePackageUpdate> updates =
        await context.GetAppAndOptionalStorePackageUpdatesAsync();

    if (updates.Count > 0)
    {
        // Alert the user that updates are available and ask for their consent
        // to start the updates.
        MessageDialog dialog = new MessageDialog(
            "Download and install updates now? This may cause the application to exit.", "Download and Install?");
        dialog.Commands.Add(new UICommand("Yes"));
        dialog.Commands.Add(new UICommand("No"));
        IUICommand command = await dialog.ShowAsync();

        if (command.Label.Equals("Yes", StringComparison.CurrentCultureIgnoreCase))
        {
            // Download and install the updates.
            IAsyncOperationWithProgress<StorePackageUpdateResult, StorePackageUpdateStatus> downloadOperation =
                context.RequestDownloadAndInstallStorePackageUpdatesAsync(updates);

            // The Progress async method is called one time for each step in the download
            // and installation process for each package in this request.
            downloadOperation.Progress = async (asyncInfo, progress) =>
            {
                await this.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
                () =>
                {
                    downloadProgressBar.Value = progress.PackageDownloadProgress;
                });
            };

            StorePackageUpdateResult result = await downloadOperation.AsTask();
        }
    }
}
```

> [!NOTE]
> Pour télécharger uniquement (sans installer) les mises à jour de package disponibles, utilisez la méthode [RequestDownloadStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestdownloadstorepackageupdatesasync).

### <a name="display-download-and-install-progress-info"></a>Afficher les informations de progression pour le téléchargement et l’installation

Lorsque vous appelez la méthode [RequestDownloadStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestdownloadstorepackageupdatesasync) ou [RequestDownloadAndInstallStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestdownloadandinstallstorepackageupdatesasync), vous pouvez affecter un gestionnaire [Progress](https://docs.microsoft.com/uwp/api/windows.foundation.iasyncoperationwithprogress-2.progress) qui est appelé à chaque étape du téléchargement (ou du processus de téléchargement et d’installation) pour chacun des packages figurant dans cette demande. Le gestionnaire reçoit un objet [StorePackageUpdateStatus](https://docs.microsoft.com/uwp/api/windows.services.store.storepackageupdatestatus) qui fournit des informations sur le package de mise à jour qui a déclenché la notification de progression. L’exemple précédent utilise le champ **PackageDownloadProgress** de l’objet **StorePackageUpdateStatus** pour afficher la progression du processus de téléchargement et d’installation.

Notez que lorsque vous appelez **RequestDownloadAndInstallStorePackageUpdatesAsync** pour télécharger et installer les mises à jour de package dans une opération unique, la valeur du champ **PackageDownloadProgress** passe de 0,0 à 0,8 durant le processus de téléchargement d’un package, puis de 0,8 à 1,0 durant l’installation. Par conséquent, si vous mappez le pourcentage affiché dans votre boîte de dialogue d’avancement personnalisée directement sur la valeur du champ **PackageDownloadProgress**, votre interface affiche une progression de 80 % à l’issue du téléchargement, tandis que le système d’exploitation affiche la boîte de dialogue d’installation. Si vous souhaitez que votre boîte de dialogue d’avancement personnalisée affiche une valeur de 100 % quand le package est téléchargé et prêt à être installé, vous pouvez modifier votre code afin d’affecter la valeur de 100 % à votre boîte de dialogue d’avancement quand le champ **PackageDownloadProgress** atteint la valeur de 0,8.

## <a name="download-and-install-package-updates-silently"></a>Télécharger et installer les mises à jour de package silencieusement

À partir de la version 1803 de Windows 10, vous pouvez utiliser les méthodes [TrySilentDownloadStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.trysilentdownloadstorepackageupdatesasync) et [TrySilentDownloadAndInstallStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.trysilentdownloadandinstallstorepackageupdatesasync) pour télécharger et installer les mises à jour de package silencieusement sans afficher d’interface utilisateur de notification. Cette opération réussit uniquement si l’utilisateur a activé le paramètre **Mettre à jour les applications automatiquement** dans le Store, et n’utilise pas une connexion réseau limitée. Avant d’appeler ces méthodes, vous devez vérifier la propriété [CanSilentlyDownloadStorePackageUpdates](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.cansilentlydownloadstorepackageupdates) pour déterminer si ces conditions sont actuellement réunies.

Cet exemple de code montre comment utiliser la méthode [GetAppAndOptionalStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getappandoptionalstorepackageupdatesasync) pour découvrir toutes les mises à jour de package disponibles, puis appeler les méthodes [TrySilentDownloadStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.trysilentdownloadstorepackageupdatesasync) et [TrySilentDownloadAndInstallStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.trysilentdownloadandinstallstorepackageupdatesasync) pour télécharger et installer les mises à jour silencieusement.

Cet exemple de code se base sur les hypothèses suivantes :
* Le fichier de code contient une instruction **using** pour les espaces de noms **Windows.Services.Store** et **System.Threading.Tasks**.
* Cette application mono-utilisateur ne s’exécute que dans le contexte de l’utilisateur qui l’a lancée. Pour une [application multi-utilisateur](https://docs.microsoft.com/windows/uwp/xbox-apps/multi-user-applications), utilisez la méthode [GetForUser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.User) plutôt que la méthode [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.GetDefault) pour obtenir un objet **StoreContext**.

> [!NOTE]
> Les méthodes **IsNowAGoodTimeToRestartApp**, **RetryDownloadAndInstallLater** et **RetryInstallLater** appelées par le code dans cet exemple sont des méthodes d’espace réservé conçues pour être implémentées si nécessaire, en fonction de la conception de votre propre application.

```csharp
private StoreContext context = null;

public async Task DownloadAndInstallAllUpdatesInBackgroundAsync()
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    // Get the updates that are available.
    IReadOnlyList<StorePackageUpdate> storePackageUpdates =
        await context.GetAppAndOptionalStorePackageUpdatesAsync();

    if (storePackageUpdates.Count > 0)
    {

        if (!context.CanSilentlyDownloadStorePackageUpdates)
        {
            return;
        }

        // Start the silent downloads and wait for the downloads to complete.
        StorePackageUpdateResult downloadResult =
            await context.TrySilentDownloadStorePackageUpdatesAsync(storePackageUpdates);

        switch (downloadResult.OverallState)
        {
            case StorePackageUpdateState.Completed:
                // The download has completed successfully. At this point, confirm whether your app
                // can restart now and then install the updates (for example, you might only install
                // packages silently if your app has been idle for a certain period of time). The
                // IsNowAGoodTimeToRestartApp method is not implemented in this example, you should
                // implement it as needed for your own app.
                if (IsNowAGoodTimeToRestartApp())
                {
                    await InstallUpdate(storePackageUpdates);
                }
                else
                {
                    // Retry/reschedule the installation later. The RetryInstallLater method is not  
                    // implemented in this example, you should implement it as needed for your own app.
                    RetryInstallLater();
                    return;
                }
                break;
            // If the user cancelled the download or you can't perform the download for some other
            // reason (for example, Wi-Fi might have been turned off and the device is now on
            // a metered network) try again later. The RetryDownloadAndInstallLater method is not  
            // implemented in this example, you should implement it as needed for your own app.
            case StorePackageUpdateState.Canceled:
            case StorePackageUpdateState.ErrorLowBattery:
            case StorePackageUpdateState.ErrorWiFiRecommended:
            case StorePackageUpdateState.ErrorWiFiRequired:
            case StorePackageUpdateState.OtherError:
                RetryDownloadAndInstallLater();
                return;
            default:
                break;
        }
    }
}

private async Task InstallUpdate(IReadOnlyList<StorePackageUpdate> storePackageUpdates)
{
    // Start the silent installation of the packages. Because the packages have already
    // been downloaded in the previous method, the following line of code just installs
    // the downloaded packages.
    StorePackageUpdateResult downloadResult =
        await context.TrySilentDownloadAndInstallStorePackageUpdatesAsync(storePackageUpdates);

    switch (downloadResult.OverallState)
    {
        // If the user cancelled the installation or you can't perform the installation  
        // for some other reason, try again later. The RetryInstallLater method is not  
        // implemented in this example, you should implement it as needed for your own app.
        case StorePackageUpdateState.Canceled:
        case StorePackageUpdateState.ErrorLowBattery:
        case StorePackageUpdateState.OtherError:
            RetryInstallLater();
            return;
        default:
            break;
    }
}
```

## <a name="mandatory-package-updates"></a>Mises à jour de packages obligatoires

Quand vous créez une soumission de package dans l’Espace partenaires pour une application qui cible Windows 10, version 1607 ou ultérieure, vous pouvez [marquer le package comme obligatoire](../publish/upload-app-packages.md#mandatory-update), ainsi que spécifier la date et l’heure auxquelles il le devient. Lorsque cette propriété est définie et que votre application détecte que la mise à jour du package est disponible, votre application peut déterminer si le package de mise à jour est obligatoire et modifier son comportement jusqu’à ce que la mise à jour soit installée (par exemple, votre application peut désactiver certaines fonctionnalités).

> [!NOTE]
> Le statut obligatoire d’une mise à jour de package n’est pas appliqué par Microsoft et le système d’exploitation ne fournit pas d’interface utilisateur pour indiquer aux utilisateurs qu’une mise à jour d’application obligatoire doit être installée. Les développeurs doivent utiliser le paramètre obligatoire pour appliquer des mises à jour d’application obligatoires dans leur propre code.  

Pour marquer une soumission de package comme obligatoire :

1. Connectez-vous à l’[Espace partenaires](https://partner.microsoft.com/dashboard) et accédez à la page de présentation de votre application.
2. Cliquez sur le nom de la soumission contenant la mise à jour de package que vous voulez rendre obligatoire.
3. Accédez à la page **Packages** de la soumission. Au bas de cette page, sélectionnez **Rendre obligatoire cette mise à jour**, puis choisissez le jour et l’heure auxquels la mise à jour du package devient obligatoire. Cette option s’applique à tous les packages UWP de la soumission.

Pour plus d’informations, consultez [Chargement des packages d’application](../publish/upload-app-packages.md).

> [!NOTE]
> Si vous créez une [version d’évaluation du package](../publish/package-flights.md), vous pouvez marquer les packages comme obligatoires à l’aide d’une interface utilisateur similaire dans la page **Packages** de la version d’évaluation. Dans ce cas, la mise à jour de package obligatoire s’applique uniquement aux clients qui font partie du groupe de versions d’évaluation.

### <a name="code-example-for-mandatory-packages"></a>Exemple de code pour des packages obligatoires

L’exemple de code suivant montre comment déterminer si des packages mis à jour sont obligatoires ou non. En général, vous devez rétrograder l’expérience d’utilisation de votre application normalement pour l’utilisateur si le téléchargement ou l’installation d’une mise à jour de package obligatoire échoue.

```csharp
private StoreContext context = null;

// Downloads and installs package updates in separate steps.
public async Task DownloadAndInstallAllUpdatesAsync()
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }  

    // Get the updates that are available.
    IReadOnlyList<StorePackageUpdate> updates =
        await context.GetAppAndOptionalStorePackageUpdatesAsync();

    if (updates.Count != 0)
    {
        // Download the packages.
        bool downloaded = await DownloadPackageUpdatesAsync(updates);

        if (downloaded)
        {
            // Install the packages.
            await InstallPackageUpdatesAsync(updates);
        }
    }
}

// Helper method for downloading package updates.
private async Task<bool> DownloadPackageUpdatesAsync(IEnumerable<StorePackageUpdate> updates)
{
    bool downloadedSuccessfully = false;

    IAsyncOperationWithProgress<StorePackageUpdateResult, StorePackageUpdateStatus> downloadOperation =
        this.context.RequestDownloadStorePackageUpdatesAsync(updates);

    // The Progress async method is called one time for each step in the download process for each
    // package in this request.
    downloadOperation.Progress = async (asyncInfo, progress) =>
    {
        await this.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
        () =>
        {
            downloadProgressBar.Value = progress.PackageDownloadProgress;
        });
    };

    StorePackageUpdateResult result = await downloadOperation.AsTask();

    switch (result.OverallState)
    {
        case StorePackageUpdateState.Completed:
            downloadedSuccessfully = true;
            break;
        default:
            // Get the failed updates.
            var failedUpdates = result.StorePackageUpdateStatuses.Where(
                status => status.PackageUpdateState != StorePackageUpdateState.Completed);

            // See if any failed updates were mandatory
            if (updates.Any(u => u.Mandatory && failedUpdates.Any(
                failed => failed.PackageFamilyName == u.Package.Id.FamilyName)))
            {
                // At least one of the updates is mandatory. Perform whatever actions you
                // want to take for your app: for example, notify the user and disable
                // features in your app.
                HandleMandatoryPackageError();
            }
            break;
    }

    return downloadedSuccessfully;
}

// Helper method for installing package updates.
private async Task InstallPackageUpdatesAsync(IEnumerable<StorePackageUpdate> updates)
{
    IAsyncOperationWithProgress<StorePackageUpdateResult, StorePackageUpdateStatus> installOperation =
        this.context.RequestDownloadAndInstallStorePackageUpdatesAsync(updates);

    // The package updates were already downloaded separately, so this method skips the download
    // operatation and only installs the updates; no download progress notifications are provided.
    StorePackageUpdateResult result = await installOperation.AsTask();

    switch (result.OverallState)
    {
        case StorePackageUpdateState.Completed:
            break;
        default:
            // Get the failed updates.
            var failedUpdates = result.StorePackageUpdateStatuses.Where(
                status => status.PackageUpdateState != StorePackageUpdateState.Completed);

            // See if any failed updates were mandatory
            if (updates.Any(u => u.Mandatory && failedUpdates.Any(failed => failed.PackageFamilyName == u.Package.Id.FamilyName)))
            {
                // At least one of the updates is mandatory, so tell the user.
                HandleMandatoryPackageError();
            }
            break;
    }
}

// Helper method for handling the scenario where a mandatory package update fails to
// download or install. Add code to this method to perform whatever actions you want
// to take, such as notifying the user and disabling features in your app.
private void HandleMandatoryPackageError()
{
}
```

## <a name="uninstall-optional-packages"></a>Désinstaller des packages obligatoires

À compter de la version 1803 de Windows 10, vous pouvez utiliser la méthode [RequestUninstallStorePackageAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestuninstallstorepackageasync) ou [RequestUninstallStorePackageByStoreIdAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestuninstallstorepackagebystoreidasync) pour désinstaller un [package facultatif](/windows/msix/package/optional-packages) (notamment un package DLC) de l’application actuelle. Par exemple, si votre application comprend du contenu installé via des packages facultatifs, vous pouvez fournir une interface utilisateur permettant aux utilisateurs de désinstaller les packages facultatifs et d’ainsi libérer de l’espace disque.

L’exemple de code suivant montre comment appeler la méthode [RequestUninstallStorePackageAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.requestuninstallstorepackageasync). Cet exemple se base sur les hypothèses suivantes :
* Le fichier de code contient une instruction **using** pour les espaces de noms **Windows.Services.Store** et **System.Threading.Tasks**.
* Cette application mono-utilisateur ne s’exécute que dans le contexte de l’utilisateur qui l’a lancée. Pour une [application multi-utilisateur](https://docs.microsoft.com/windows/uwp/xbox-apps/multi-user-applications), utilisez la méthode [GetForUser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.User) plutôt que la méthode [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.GetDefault) pour obtenir un objet **StoreContext**.

```csharp
public async Task UninstallPackage(Windows.ApplicationModel.Package package)
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    var storeContext = StoreContext.GetDefault();
    IAsyncOperation<StoreUninstallStorePackageResult> uninstallOperation =
        storeContext.RequestUninstallStorePackageAsync(package);

    // At this point, you can update your app UI to show that the package
    // is installing.

    uninstallOperation.Completed += (asyncInfo, status) =>
    {
        StoreUninstallStorePackageResult result = uninstallOperation.GetResults();
        switch (result.Status)
        {
            case StoreUninstallStorePackageStatus.Succeeded:
                {
                    // Update your app UI to show the package as uninstalled.
                    break;
                }
            default:
                {
                    // Update your app UI to show that the package uninstall failed.
                    break;
                }
        }
    };
}
```

## <a name="get-download-queue-info"></a>Obtenir les informations de file d’attente de téléchargement

À compter de la version 1803 de Windows 10, vous pouvez utiliser les méthodes [GetAssociatedStoreQueueItemsAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getassociatedstorequeueitemsasync) et [GetStoreQueueItemsAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getstorequeueitemsasync) pour obtenir des informations concernant les packages se trouvant dans la file d’attente de téléchargement et d’installation actuelle du Store. Ces méthodes sont utiles si votre application ou jeu prend en charge de vastes packages facultatifs (notamment des DLC) dont le téléchargement et l’installation peuvent prendre des heures ou des jours, et que vous souhaitez traiter avec élégance le où un client ferme l’application ou jeu avant la fin du processus de téléchargement et d’installation. Lorsque le client redémarre votre application ou jeu, votre code peut utiliser ces méthodes pour obtenir des informations à propos de l’état des packages qui se trouvent toujours dans la file d’attente de téléchargement et d’installation. Ainsi, vous pouvez afficher l’état de chaque package au client.

L’exemple de code suivant montre comment appeler la méthode [GetAssociatedStoreQueueItemsAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getassociatedstorequeueitemsasync) pour obtenir la liste des mises à jour du package en cours de l’application actuelle et récupérer les informations d’état pour chaque package. Cet exemple se base sur les hypothèses suivantes :
* Le fichier de code contient une instruction **using** pour les espaces de noms **Windows.Services.Store** et **System.Threading.Tasks**.
* Cette application mono-utilisateur ne s’exécute que dans le contexte de l’utilisateur qui l’a lancée. Pour une [application multi-utilisateur](https://docs.microsoft.com/windows/uwp/xbox-apps/multi-user-applications), utilisez la méthode [GetForUser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.User) plutôt que la méthode [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.GetDefault) pour obtenir un objet **StoreContext**.

> [!NOTE]
> Les méthodes **MarkUpdateInProgressInUI**, **RemoveItemFromUI**, **MarkInstallCompleteInUI**, **MarkInstallErrorInUI** et **MarkInstallPausedInUI** appelées par le code dans cet exemple sont des méthodes d’espace réservé conçues pour être implémentées si nécessaire en fonction de la conception de votre propre application.

```csharp
private StoreContext context = null;

private async Task GetQueuedInstallItemsAndBuildInitialStoreUI()
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    // Get the Store packages in the install queue.
    IReadOnlyList<StoreQueueItem> storeUpdateItems = await context.GetAssociatedStoreQueueItemsAsync();

    foreach (StoreQueueItem storeItem in storeUpdateItems)
    {
        // In this example we only care about package updates.
        if (storeItem.InstallKind != StoreQueueItemKind.Update)
            continue;

        StoreQueueItemStatus currentStatus = storeItem.GetCurrentStatus();
        StoreQueueItemState installState = currentStatus.PackageInstallState;
        StoreQueueItemExtendedState extendedInstallState =
            currentStatus.PackageInstallExtendedState;

        // Handle the StatusChanged event to display current status to the customer.
        storeItem.StatusChanged += StoreItem_StatusChanged;

        switch (installState)
        {
            // Download and install are still in progress, so update the status for this  
            // item and provide the extended state info. The following methods are not
            // implemented in this example; you should implement them as needed for your
            // app's UI.
            case StoreQueueItemState.Active:
                MarkUpdateInProgressInUI(storeItem, extendedInstallState);
                break;
            case StoreQueueItemState.Canceled:
                RemoveItemFromUI(storeItem);
                break;
            case StoreQueueItemState.Completed:
                MarkInstallCompleteInUI(storeItem);
                break;
            case StoreQueueItemState.Error:
                MarkInstallErrorInUI(storeItem);
                break;
            case StoreQueueItemState.Paused:
                MarkInstallPausedInUI(storeItem, installState, extendedInstallState);
                break;
        }
    }
}

private void StoreItem_StatusChanged(StoreQueueItem sender, object args)
{
    StoreQueueItemStatus currentStatus = sender.GetCurrentStatus();
    StoreQueueItemState installState = currentStatus.PackageInstallState;
    StoreQueueItemExtendedState extendedInstallState = currentStatus.PackageInstallExtendedState;

    switch (installState)
    {
        // Download and install are still in progress, so update the status for this  
        // item and provide the extended state info. The following methods are not
        // implemented in this example; you should implement them as needed for your
        // app's UI.
        case StoreQueueItemState.Active:
            MarkUpdateInProgressInUI(sender, extendedInstallState);
            break;
        case StoreQueueItemState.Canceled:
            RemoveItemFromUI(sender);
            break;
        case StoreQueueItemState.Completed:
            MarkInstallCompleteInUI(sender);
            break;
        case StoreQueueItemState.Error:
            MarkInstallErrorInUI(sender);
            break;
        case StoreQueueItemState.Paused:
            MarkInstallPausedInUI(sender, installState, extendedInstallState);
            break;
    }
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Création de packages facultatifs et d’ensembles associés](/windows/msix/package/optional-packages)
