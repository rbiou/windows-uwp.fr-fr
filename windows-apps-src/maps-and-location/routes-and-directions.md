---
title: Afficher des itinéraires et indications sur une carte
description: Demandez des itinéraires et indications, et affichez-les dans votre app.
ms.assetid: BBB4C23A-8F10-41D1-81EA-271BE01AED81
ms.date: 09/20/2017
ms.topic: article
keywords: windows 10, uwp, itinéraire, carte, localisation, indications
ms.localizationpriority: medium
ms.openlocfilehash: e9e464f9a3b49d3a94edbc8593df58e1e7c24515
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259339"
---
# <a name="display-routes-and-directions-on-a-map"></a>Afficher des itinéraires et indications sur une carte



Demandez des itinéraires et indications, et affichez-les dans votre app.

>[!Note]
>Pour plus d’informations sur l’utilisation de cartes dans votre app, téléchargez l’[exemple de carte pour plateforme Windows universelle (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl).
>Si la cartographie n’est pas une fonction essentielle de votre app, envisagez de lancer l’app Cartes Windows à la place. Vous pouvez utiliser les schémas d’URI `bingmaps:`, `ms-drive-to:` et `ms-walk-to:` afin de lancer l’application Cartes Windows en accédant à des cartes, itinéraires et indications étape par étape spécifiques. Pour plus d’informations, voir [Lancer l’app Cartes Windows](https://docs.microsoft.com/windows/uwp/launch-resume/launch-maps-app).

 
## <a name="an-intro-to-maproutefinder-results"></a>Introduction aux résultats de MapRouteFinder


Les classes pour les itinéraires et les indications sont associées comme suit :

* La classe [**MapRouteFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteFinder) dispose de méthodes qui permettent d’obtenir des itinéraires et des indications. Ces méthodes retournent un [**MapRouteFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteFinderResult).

* La classe [**MapRouteFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteFinderResult) contient un objet [**MapRoute**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRoute). Accédez à cet objet via la propriété [**Route**](https://docs.microsoft.com/uwp/api/windows.services.maps.maproutefinderresult.route) de la classe **MapRouteFinderResult**.

* La classe [**MapRoute**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRoute) contient une collection d’objets [**MapRouteLeg**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteLeg). Accédez à cette collection via la propriété [**Legs**](https://docs.microsoft.com/uwp/api/windows.services.maps.maproute.legs) de la classe **MapRoute**.

* Chaque classe [**MapRouteLeg**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteLeg) contient une collection d’objets [**MapRouteManeuver**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteManeuver). Accédez à cette collection via la propriété [**Maneuvers**](https://docs.microsoft.com/uwp/api/windows.services.maps.maprouteleg.maneuvers) de la classe **MapRouteLeg**.

Obtenez des itinéraires et des indications pour des déplacements en voiture ou à pied en appelant les méthodes de la classe [**MapRouteFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteFinder). Par exemple, [**GetDrivingRouteAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maproutefinder.getdrivingrouteasync) ou [**GetWalkingRouteAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maproutefinder.getwalkingrouteasync).

Lorsque vous demandez un itinéraire :

* Vous pouvez indiquer uniquement un point de départ et un point d’arrivée, ou une série de positions pour calculer l’itinéraire.

    Les points de position *Arrêt* permettent d'ajouter des trajets supplémentaires, dotés de leur propre itinéraire. Pour spécifier des points de position *arrêt*, utilisez l'une des surcharges [**GetDrivingRouteFromWaypointsAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maproutefinder.getwalkingroutefromwaypointsasync).

    Les points de position *Via* définissent des emplacements intermédiaires entre les points de position *Arrêt*. Ils n’ajoutent pas de trajets.  Il s'agit simplement de points de position traversés par un itinéraire. Pour spécifier des points de position *via*, utilisez l'une des surcharges [**GetDrivingRouteFromEnhancedWaypointsAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maproutefinder.getdrivingroutefromenhancedwaypointsasync).

* Vous pouvez indiquer des paramètres d’optimisation (par exemple, réduction de la distance).

* Vous pouvez spécifier des restrictions (par exemple, aucun trajet par autoroute).

## <a name="display-directions"></a>Afficher des indications

L’objet [**MapRouteFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteFinderResult) contient un objet [**MapRoute**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRoute) accessible via sa propriété [**Route**](https://docs.microsoft.com/uwp/api/windows.services.maps.maproutefinderresult.route).

L’objet [**MapRoute**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRoute) calculé a des propriétés qui indiquent le temps pour parcourir l’itinéraire, la longueur de celui-ci et la collection d’objets [**MapRouteLeg**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteLeg) qui contiennent les étapes de l’itinéraire. Chaque objet **MapRouteLeg** contient une collection d’objets [**MapRouteManeuver**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteManeuver). L’objet **MapRouteManeuver** contient des indications accessibles via sa propriété [**InstructionText**](https://docs.microsoft.com/uwp/api/windows.services.maps.maproutemaneuver.instructiontext).

>[!IMPORTANT]
>Vous devez spécifier une clé d’authentification pour Cartes avant de pouvoir utiliser les services de carte. Pour plus d’informations, voir [Demander une clé d’authentification pour Cartes](authentication-key.md).

 

```csharp
using System;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.Services.Maps;
using Windows.Devices.Geolocation;
...
private async void button_Click(object sender, RoutedEventArgs e)
{
   // Start at Microsoft in Redmond, Washington.
   BasicGeoposition startLocation = new BasicGeoposition() {Latitude=47.643,Longitude=-122.131};

   // End at the city of Seattle, Washington.
   BasicGeoposition endLocation = new BasicGeoposition() {Latitude = 47.604,Longitude= -122.329};

   // Get the route between the points.
   MapRouteFinderResult routeResult =
         await MapRouteFinder.GetDrivingRouteAsync(
         new Geopoint(startLocation),
         new Geopoint(endLocation),
         MapRouteOptimization.Time,
         MapRouteRestrictions.None);

   if (routeResult.Status == MapRouteFinderStatus.Success)
   {
      System.Text.StringBuilder routeInfo = new System.Text.StringBuilder();

      // Display summary info about the route.
      routeInfo.Append("Total estimated time (minutes) = ");
      routeInfo.Append(routeResult.Route.EstimatedDuration.TotalMinutes.ToString());
      routeInfo.Append("\nTotal length (kilometers) = ");
      routeInfo.Append((routeResult.Route.LengthInMeters / 1000).ToString());

      // Display the directions.
      routeInfo.Append("\n\nDIRECTIONS\n");

      foreach (MapRouteLeg leg in routeResult.Route.Legs)
      {
         foreach (MapRouteManeuver maneuver in leg.Maneuvers)
         {
            routeInfo.AppendLine(maneuver.InstructionText);
         }
      }

      // Load the text box.
      tbOutputText.Text = routeInfo.ToString();
   }
   else
   {
      tbOutputText.Text =
            "A problem occurred: " + routeResult.Status.ToString();
   }
}
```

Cet exemple montre les résultats suivants dans la zone de texte `tbOutputText`.

``` syntax
Total estimated time (minutes) = 18.4833333333333
Total length (kilometers) = 21.847

DIRECTIONS
Head north on 157th Ave NE.
Turn left onto 159th Ave NE.
Turn left onto NE 40th St.
Turn left onto WA-520 W.
Enter the freeway WA-520 from the right.
Keep left onto I-5 S/Portland.
Keep right and leave the freeway at exit 165A towards James St..
Turn right onto James St.
You have reached your destination.
```

## <a name="display-routes"></a>Afficher des itinéraires


Pour afficher un [**MapRoute**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRoute) sur un [**MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl), créez un [**MapRouteView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapRouteView) avec **MapRoute**. Ajoutez ensuite **MapRouteView** à la collection [**Routes**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.routes) de **MapControl**.

>[!IMPORTANT]
>Pour pouvoir utiliser les services de carte ou le contrôle de carte, vous devez spécifier une clé d’authentification de cartes. Pour plus d’informations, voir [Demander une clé d’authentification pour Cartes](authentication-key.md).

 

```csharp
using System;
using Windows.Devices.Geolocation;
using Windows.Services.Maps;
using Windows.UI;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Maps;
...
private async void ShowRouteOnMap()
{
   // Start at Microsoft in Redmond, Washington.
   BasicGeoposition startLocation = new BasicGeoposition() { Latitude = 47.643, Longitude = -122.131 };

   // End at the city of Seattle, Washington.
   BasicGeoposition endLocation = new BasicGeoposition() { Latitude = 47.604, Longitude = -122.329 };


   // Get the route between the points.
   MapRouteFinderResult routeResult =
         await MapRouteFinder.GetDrivingRouteAsync(
         new Geopoint(startLocation),
         new Geopoint(endLocation),
         MapRouteOptimization.Time,
         MapRouteRestrictions.None);

   if (routeResult.Status == MapRouteFinderStatus.Success)
   {
      // Use the route to initialize a MapRouteView.
      MapRouteView viewOfRoute = new MapRouteView(routeResult.Route);
      viewOfRoute.RouteColor = Colors.Yellow;
      viewOfRoute.OutlineColor = Colors.Black;

      // Add the new MapRouteView to the Routes collection
      // of the MapControl.
      MapWithRoute.Routes.Add(viewOfRoute);

      // Fit the MapControl to the route.
      await MapWithRoute.TrySetViewBoundsAsync(
            routeResult.Route.BoundingBox,
            null,
            Windows.UI.Xaml.Controls.Maps.MapAnimationKind.None);
   }
}
```

Cet exemple affiche ce qui suit sur un [**MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) nommé **MapWithRoute**.

![Contrôle de carte avec l’itinéraire affiché.](images/routeonmap.png)

Voici une version de cet exemple utilisant un point de position *via* entre deux points de position *arrêt* :

```csharp
using System;
using Windows.Devices.Geolocation;
using Windows.Services.Maps;
using Windows.UI;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Maps;
...
private async void ShowRouteOnMap()
{
  Geolocator locator = new Geolocator();
  locator.DesiredAccuracyInMeters = 1;
  locator.PositionChanged += Locator_PositionChanged;

  BasicGeoposition point1 = new BasicGeoposition() { Latitude = 47.649693, Longitude = -122.144908 };
  BasicGeoposition point2 = new BasicGeoposition() { Latitude = 47.6205, Longitude = -122.3493 };
  BasicGeoposition point3 = new BasicGeoposition() { Latitude = 48.649693, Longitude = -122.144908 };

  // Get Driving Route from point A  to point B thru point C
  var path = new List<EnhancedWaypoint>();

  path.Add(new EnhancedWaypoint(new Geopoint(point1), WaypointKind.Stop));
  path.Add(new EnhancedWaypoint(new Geopoint(point2), WaypointKind.Via));
  path.Add(new EnhancedWaypoint(new Geopoint(point3), WaypointKind.Stop));

  MapRouteFinderResult routeResult =  await MapRouteFinder.GetDrivingRouteFromEnhancedWaypointsAsync(path);

  if (routeResult.Status == MapRouteFinderStatus.Success)
  {
      MapRouteView viewOfRoute = new MapRouteView(routeResult.Route);
      viewOfRoute.RouteColor = Colors.Yellow;
      viewOfRoute.OutlineColor = Colors.Black;

      myMap.Routes.Add(viewOfRoute);

      await myMap.TrySetViewBoundsAsync(
            routeResult.Route.BoundingBox,
            null,
            Windows.UI.Xaml.Controls.Maps.MapAnimationKind.None);
  }
}
```

## <a name="related-topics"></a>Rubriques connexes

* [Espace partenaires Bing Cartes](https://www.bingmapsportal.com/)
* [Exemple de carte UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
* [Recommandations de conception pour les cartes](https://docs.microsoft.com/windows/uwp/maps-and-location/controls-map)
* [Build 2015 Video : utilisation des cartes et de l’emplacement sur les téléphones, les tablettes et les PC dans vos applications Windows](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Exemple d’application de trafic UWP](https://github.com/Microsoft/Windows-appsample-trafficapp)
