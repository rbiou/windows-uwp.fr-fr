---
title: Obtenir l’emplacement de l’utilisateur
description: Déterminez l’emplacement de l’utilisateur et réagissez aux changements d’emplacement. L’accès à l’emplacement de l’utilisateur est géré par les paramètres de confidentialité définis dans l’application Paramètres. Cet article indique également comment vérifier si votre application est autorisée à accéder à l’emplacement de l’utilisateur.
ms.assetid: 24DC9A41-8CC1-48B0-BC6D-24BF571AFCC8
ms.date: 11/28/2017
ms.topic: article
keywords: windows 10, uwp, carte, emplacement, fonctionnalité de localisation
ms.localizationpriority: medium
ms.openlocfilehash: 50f605164a496d00113b73ffeae669e3ff145535
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260392"
---
# <a name="get-the-users-location"></a>Obtenir l’emplacement de l’utilisateur




Déterminez l’emplacement de l’utilisateur et réagissez aux changements d’emplacement. L’accès à l’emplacement de l’utilisateur est géré par les paramètres de confidentialité définis dans l’application Paramètres. Cet article indique également comment vérifier si votre application est autorisée à accéder à l’emplacement de l’utilisateur.

**Conseil** Pour en savoir plus sur l’accès à l’emplacement de l’utilisateur dans votre application, téléchargez l’exemple suivant à partir du [référentiel Windows-universal-samples](https://github.com/Microsoft/Windows-universal-samples) sur GitHub.

-   [Exemple de carte pour UWP (plateforme Windows universelle)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)

## <a name="enable-the-location-capability"></a>Activer la fonctionnalité de localisation


1.  Dans l’**Explorateur de solutions**, double-cliquez sur **package.appxmanifest**, puis sélectionnez l’onglet **Capacités**.
2.  Dans la liste **Capacités**, cochez la case **Localisation**. Cette opération ajoute la fonctionnalité `location` de l’appareil au fichier manifeste du package.

```XML
  <Capabilities>
    <!-- DeviceCapability elements must follow Capability elements (if present) -->
    <DeviceCapability Name="location"/>
  </Capabilities>
```

## <a name="get-the-current-location"></a>Obtenir l’emplacement actuel


Cette section décrit comment détecter l’emplacement géographique de l’utilisateur à l’aide d’API dans l’espace de noms [**Windows.Devices.Geolocation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation).

### <a name="step-1-request-access-to-the-users-location"></a>Étape 1 : Demander l’accès à l’emplacement de l’utilisateur

À moins que votre application ait une fonctionnalité d’emplacement grossière (voir la remarque), vous devez demander l’accès à l’emplacement de l’utilisateur à l’aide de la méthode [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync) avant d’essayer d’accéder à l’emplacement. Vous devez appeler la méthode **RequestAccessAsync** à partir du thread de l’interface utilisateur et votre application doit être au premier plan. Votre application ne sera pas en mesure d’accéder aux informations relatives à l’emplacement de l’utilisateur tant que l’utilisateur n’aura pas accordé l’autorisation à votre application.\*

```csharp
using Windows.Devices.Geolocation;
...
var accessStatus = await Geolocator.RequestAccessAsync();
```



La méthode [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync) demande à l’utilisateur l’autorisation d’accéder à son emplacement. L’utilisateur est invité une fois seulement (par application). Une fois la première autorisation accordée ou refusée, cette méthode ne demande plus d’autorisation. Pour aider l’utilisateur à modifier les autorisations d’emplacement une fois qu’il a été invité, nous vous recommandons de fournir un lien vers les paramètres d’emplacement, comme illustré plus loin dans cette rubrique.

>Remarque : la fonctionnalité d’emplacement grossiste permet à votre application d’obtenir un emplacement masqué intentionnellement (non précis) sans obtenir l’autorisation explicite de l’utilisateur (le commutateur d’emplacement à l’ensemble du système doit néanmoins être **activé**). Pour savoir comment utiliser l’emplacement grossiste dans votre application, consultez la méthode [**AllowFallbackToConsentlessPositions**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.allowfallbacktoconsentlesspositions) dans la classe [**géolocator**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator) .

### <a name="step-2-get-the-users-location-and-register-for-changes-in-location-permissions"></a>Étape 2 : Obtenir l’emplacement de l’utilisateur et inscrire les changements d’autorisation de localisation

La méthode [**GetGeopositionAsync**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync) effectue une lecture unique de l’emplacement actuel. Ici, une instruction **switch** est utilisée avec l’élément **accessStatus** (de l’exemple précédent) afin d’agir uniquement lorsque l’accès à l’emplacement de l’utilisateur est autorisé. Si cette opération est autorisée, le code crée un objet [**Geolocator**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geolocator), inscrit les modifications dans les autorisations d’emplacement et demande l’emplacement de l’utilisateur.

```csharp
switch (accessStatus)
{
    case GeolocationAccessStatus.Allowed:
        _rootPage.NotifyUser("Waiting for update...", NotifyType.StatusMessage);

        // If DesiredAccuracy or DesiredAccuracyInMeters are not set (or value is 0), DesiredAccuracy.Default is used.
        Geolocator geolocator = new Geolocator { DesiredAccuracyInMeters = _desireAccuracyInMetersValue };

        // Subscribe to the StatusChanged event to get updates of location status changes.
        _geolocator.StatusChanged += OnStatusChanged;

        // Carry out the operation.
        Geoposition pos = await geolocator.GetGeopositionAsync();

        UpdateLocationData(pos);
        _rootPage.NotifyUser("Location updated.", NotifyType.StatusMessage);
        break;

    case GeolocationAccessStatus.Denied:
        _rootPage.NotifyUser("Access to location is denied.", NotifyType.ErrorMessage);
        LocationDisabledMessage.Visibility = Visibility.Visible;
        UpdateLocationData(null);
        break;

    case GeolocationAccessStatus.Unspecified:
        _rootPage.NotifyUser("Unspecified error.", NotifyType.ErrorMessage);
        UpdateLocationData(null);
        break;
}
```

### <a name="step-3-handle-changes-in-location-permissions"></a>Étape 3 : Gérer les modifications apportées aux autorisations d’emplacement

L’objet [**Geolocator**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geolocator) déclenche l’événement [**StatusChanged**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.statuschanged) afin d’indiquer que les paramètres d’emplacement de l’utilisateur ont changé. Cet événement transmet l’état correspondant par le biais de la propriété **Status** de l’argument (de type [**PositionStatus**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.PositionStatus)). Notez que cette méthode n’est pas appelée à partir du thread d’interface utilisateur et que l’objet [**Dispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) invoque les modifications de l’interface utilisateur.

```csharp
using Windows.UI.Core;
...
async private void OnStatusChanged(Geolocator sender, StatusChangedEventArgs e)
{
    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        // Show the location setting message only if status is disabled.
        LocationDisabledMessage.Visibility = Visibility.Collapsed;

        switch (e.Status)
        {
            case PositionStatus.Ready:
                // Location platform is providing valid data.
                ScenarioOutput_Status.Text = "Ready";
                _rootPage.NotifyUser("Location platform is ready.", NotifyType.StatusMessage);
                break;

            case PositionStatus.Initializing:
                // Location platform is attempting to acquire a fix.
                ScenarioOutput_Status.Text = "Initializing";
                _rootPage.NotifyUser("Location platform is attempting to obtain a position.", NotifyType.StatusMessage);
                break;

            case PositionStatus.NoData:
                // Location platform could not obtain location data.
                ScenarioOutput_Status.Text = "No data";
                _rootPage.NotifyUser("Not able to determine the location.", NotifyType.ErrorMessage);
                break;

            case PositionStatus.Disabled:
                // The permission to access location data is denied by the user or other policies.
                ScenarioOutput_Status.Text = "Disabled";
                _rootPage.NotifyUser("Access to location is denied.", NotifyType.ErrorMessage);

                // Show message to the user to go to location settings.
                LocationDisabledMessage.Visibility = Visibility.Visible;

                // Clear any cached location data.
                UpdateLocationData(null);
                break;

            case PositionStatus.NotInitialized:
                // The location platform is not initialized. This indicates that the application
                // has not made a request for location data.
                ScenarioOutput_Status.Text = "Not initialized";
                _rootPage.NotifyUser("No request for location is made yet.", NotifyType.StatusMessage);
                break;

            case PositionStatus.NotAvailable:
                // The location platform is not available on this version of the OS.
                ScenarioOutput_Status.Text = "Not available";
                _rootPage.NotifyUser("Location is not available on this version of the OS.", NotifyType.ErrorMessage);
                break;

            default:
                ScenarioOutput_Status.Text = "Unknown";
                _rootPage.NotifyUser(string.Empty, NotifyType.StatusMessage);
                break;
        }
    });
}
```

## <a name="respond-to-location-updates"></a>Répondre aux mises à jour de localisation


Cette section décrit comment utiliser l’événement [**PositionChanged**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.positionchanged) pour recevoir les mises à jour de l’emplacement de l’utilisateur pendant une période donnée. Étant donné que l’utilisateur peut révoquer l’accès à l’emplacement à tout moment, il est important d’appeler [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync) et d’utiliser l’événement [**StatusChanged**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.statuschanged), comme indiqué dans la section précédente.

Cette section suppose que vous avez déjà activé la fonctionnalité de localisation et appelé [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync) à partir du thread d’interface utilisateur de votre application au premier plan.

### <a name="step-1-define-the-report-interval-and-register-for-location-updates"></a>Étape 1 :Définir l’intervalle de rapport et inscrire les mises à jour d’emplacement

Dans cet exemple, une instruction **switch** est utilisée avec l’élément **accessStatus** (de l’exemple précédent) afin d’agir uniquement lorsque l’accès à l’emplacement de l’utilisateur est autorisé. Si cette opération est autorisée, le code crée un objet [**Geolocator**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geolocator), spécifie le type de suivi et inscrit les mises à jour d’emplacement.

L’objet [**Geolocator**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geolocator) peut déclencher l’événement [**PositionChanged**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.positionchanged) sur la base d’un changement de position (suivi basé sur la distance) ou d’un changement dans le temps (suivi basé sur le temps).

-   Pour le suivi basé sur la distance, définissez la propriété [**MovementThreshold**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.movementthreshold).
-   Pour le suivi basé sur le temps, définissez la propriété [**ReportInterval**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.reportinterval).

Si aucune des propriétés n’est définie, une position est renvoyée chaque seconde (équivalente à `ReportInterval = 1000`). Ici, un intervalle de rapport de 2 secondes (`ReportInterval = 2000`) est utilisé.
```csharp
using Windows.Devices.Geolocation;
...
var accessStatus = await Geolocator.RequestAccessAsync();

switch (accessStatus)
{
    case GeolocationAccessStatus.Allowed:
        // Create Geolocator and define perodic-based tracking (2 second interval).
        _geolocator = new Geolocator { ReportInterval = 2000 };

        // Subscribe to the PositionChanged event to get location updates.
        _geolocator.PositionChanged += OnPositionChanged;

        // Subscribe to StatusChanged event to get updates of location status changes.
        _geolocator.StatusChanged += OnStatusChanged;

        _rootPage.NotifyUser("Waiting for update...", NotifyType.StatusMessage);
        LocationDisabledMessage.Visibility = Visibility.Collapsed;
        StartTrackingButton.IsEnabled = false;
        StopTrackingButton.IsEnabled = true;
        break;

    case GeolocationAccessStatus.Denied:
        _rootPage.NotifyUser("Access to location is denied.", NotifyType.ErrorMessage);
        LocationDisabledMessage.Visibility = Visibility.Visible;
        break;

    case GeolocationAccessStatus.Unspecified:
        _rootPage.NotifyUser("Unspecificed error!", NotifyType.ErrorMessage);
        LocationDisabledMessage.Visibility = Visibility.Collapsed;
        break;
}
```

### <a name="step-2-handle-location-updates"></a>Étape 2 : Gérer les mises à jour d’emplacement

L’objet [**Geolocator**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geolocator) déclenche l’événement [**PositionChanged**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geolocator.positionchanged) pour indiquer que l’emplacement de l’utilisateur a changé ou qu’une certaine période s’est écoulée, selon la configuration que vous avez choisie. Cet événement transmet l’emplacement correspondant via la propriété de l’argument **Position** (de type [**Geoposition**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geoposition)). Dans cet exemple, la méthode n’est pas appelée à partir du thread d’interface utilisateur, et l’objet [**Dispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) invoque les modifications de l’interface utilisateur.

```csharp
using Windows.UI.Core;
...
async private void OnPositionChanged(Geolocator sender, PositionChangedEventArgs e)
{
    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        _rootPage.NotifyUser("Location updated.", NotifyType.StatusMessage);
        UpdateLocationData(e.Position);
    });
}
```

## <a name="change-the-location-privacy-settings"></a>Modifier les paramètres de confidentialité d’emplacement


Si les paramètres de confidentialité d’emplacement n’autorisent pas votre application à accéder à l’emplacement de l’utilisateur, nous vous recommandons de fournir un lien pratique vers les **paramètres de confidentialité d’emplacement** dans l’application **Paramètres**. Dans cet exemple, un contrôle de lien hypertexte est utilisé pour accéder à l’URI `ms-settings:privacy-location`.

```xml
<!--Set Visibility to Visible when access to location is denied -->  
<TextBlock x:Name="LocationDisabledMessage" FontStyle="Italic"
                 Visibility="Collapsed" Margin="0,15,0,0" TextWrapping="Wrap" >
          <Run Text="This app is not able to access Location. Go to " />
              <Hyperlink NavigateUri="ms-settings:privacy-location">
                  <Run Text="Settings" />
              </Hyperlink>
          <Run Text=" to check the location privacy settings."/>
</TextBlock>
```

Par ailleurs, votre application peut appeler la méthode[**LaunchUriAsync**](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync) pour lancer l’application **Paramètres** à partir du code. Pour plus d’informations, voir [Lancer l’application Paramètres Windows](https://docs.microsoft.com/windows/uwp/launch-resume/launch-settings-app).

```csharp
using Windows.System;
...
bool result = await Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-location"));
```

## <a name="troubleshoot-your-app"></a>Résoudre les problèmes de votre application


Pour que votre application puisse accéder à l’emplacement de l’utilisateur, l’option **Localisation** doit être activée sur l’appareil. Dans l’application **Paramètres**, vérifiez que les **paramètres de confidentialité d’emplacement** suivants sont bien activés :

-   **Emplacement de cet appareil...** est activé **(non** applicable dans Windows 10 mobile)
-   Le paramètre des services de localisation **Emplacement** est **activé**.
-   Sous **Choisir les applications qui peuvent utiliser votre emplacement**, votre application est **activée**.

## <a name="related-topics"></a>Rubriques connexes

* [Exemple de géolocalisation UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Geolocation)
* [Instructions de conception pour le géorepérage](https://docs.microsoft.com/windows/uwp/maps-and-location/guidelines-for-geofencing)
* [Recommandations de conception pour les applications prenant en charge la géolocalisation](https://docs.microsoft.com/windows/uwp/maps-and-location/guidelines-and-checklist-for-detecting-location)
