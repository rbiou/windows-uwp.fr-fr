---
Description: Un affichage Calendrier permet à un utilisateur d’afficher un calendrier qu’il peut parcourir par mois, par année ou par décennie, et d’interagir avec celui-ci.
title: Affichage Calendrier
ms.assetid: d8ec5ba8-7a9d-405d-a1a5-5a1b502b9e64
label: Calendar view
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 8536446cb3406420b75da163f1479c027a3fba36
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82968974"
---
# <a name="calendar-view"></a>Affichage Calendrier

Un affichage Calendrier permet à un utilisateur d’afficher un calendrier qu’il peut parcourir par mois, par année ou par décennie, et d’interagir avec celui-ci. Un utilisateur peut sélectionner une seule date ou une plage de dates. Il n’y a pas de surface de sélection et le calendrier est toujours visible.

**Obtenir la bibliothèque d’interface utilisateur Windows**

|  |  |
| - | - |
| ![Logo WinUI](images/winui-logo-64x64.png) | La bibliothèque d’interface utilisateur Windows version 2.2 ou ultérieure inclut pour ce contrôle un nouveau modèle qui utilise des angles arrondis. Pour plus d’informations, consultez [Rayons des angles](/windows/uwp/design/style/rounded-corner). WinUI est un package NuGet qui contient de nouveaux contrôles et des fonctionnalités d’interface utilisateur pour les applications Windows. Pour plus d’informations, notamment des instructions d’installation, consultez la [bibliothèque d’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

> **API de plateforme :**  [classe CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView), [événement SelectedDatesChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.selecteddateschanged)

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?
Utilisez un affichage Calendrier pour permettre à un utilisateur de sélectionner une date unique ou une plage de dates à partir d’un calendrier toujours visible.

Si vous avez besoin de permettre à un utilisateur de sélectionner plusieurs dates en une seule fois, vous devez utiliser un affichage Calendrier. Si vous avez besoin de permettre à un utilisateur de sélectionner une seule date sans que le calendrier soit toujours visible, envisagez d’utiliser un contrôle de type [sélecteur de dates du calendrier](calendar-date-picker.md) ou [sélecteur de date](date-picker.md).

Pour plus d’informations sur le choix du contrôle approprié, voir l’article [Contrôles de date et d’heure](date-and-time.md).

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l’application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/CalendarView">ouvrir l’application et voir l’objet CalendarView en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

L’affichage Calendrier se compose de 3 affichages distincts : l’affichage mensuel, l’affichage annuel et l’affichage décennal. Par défaut, il s’ouvre avec l’affichage mensuel. Vous pouvez spécifier un affichage de démarrage en définissant la propriété [DisplayMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.displaymode).

![Les 3 affichages d’un affichage Calendrier](images/calendar-view-3-views.png)

Les utilisateurs cliquent sur l’en-tête dans l’affichage mensuel pour ouvrir l’affichage annuel, et cliquent sur l’en-tête dans l’affichage annuel pour ouvrir l’affichage décennal. Les utilisateurs sélectionnent une année dans l’affichage décennal pour revenir à l’affichage annuel, et sélectionnent un mois dans l’affichage annuel pour revenir à l’affichage mensuel. Les deux flèches sur le côté de l’en-tête permettent d’avancer ou de reculer par mois, par année ou par décennie.

## <a name="create-a-calendar-view"></a>Créer un affichage Calendrier

Cet exemple montre comment créer un simple affichage calendrier.

```xaml
<CalendarView/>
```

L’affichage Calendrier obtenu se présente comme suit :

![Exemple d’affichage Calendrier](images/controls_calendar_monthview.png)

### <a name="selecting-dates"></a>Sélection de dates

Par défaut, la propriété [SelectionMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.selectionmode) est définie sur **Single**. Cela permet à l’utilisateur de sélectionner une date unique dans le calendrier. Définissez SelectionMode sur **None** pour désactiver la sélection de date.

Définissez SelectionMode sur **Multiple** pour permettre à un utilisateur de sélectionner plusieurs dates. Vous pouvez sélectionner plusieurs dates par programmation en ajoutant des objets [DateTime](https://docs.microsoft.com/dotnet/api/system.datetime)/[DateTimeOffset](https://docs.microsoft.com/dotnet/api/system.datetimeoffset) à la collection [SelectedDates](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.selecteddates), comme illustré ici :

```csharp
calendarView1.SelectedDates.Add(DateTimeOffset.Now);
calendarView1.SelectedDates.Add(new DateTime(1977, 1, 5));
```

Un utilisateur peut désélectionner une date sélectionnée en cliquant ou en appuyant sur la grille du calendrier.

Vous pouvez gérer l’événement [SelectedDatesChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.selecteddateschanged) pour être averti quand la collection [SelectedDates](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.selecteddates) a changé.

> [!NOTE]
> Pour obtenir des informations importantes sur les valeurs de date, consultez [Valeurs DateTime et Calendar](date-and-time.md#datetime-and-calendar-values) dans l’article Contrôles de date et d’heure.

### <a name="customizing-the-calendar-views-appearance"></a>Personnalisation de l’apparence de l’affichage du calendrier

L’affichage Calendrier est constitué d’éléments XAML définis dans le ControlTemplate et d’éléments visuels rendus directement par le contrôle.
- Les éléments XAML définis dans le modèle de contrôle incluent la bordure qui encadre le contrôle, l’en-tête, les boutons Précédent et Suivant et les éléments DayOfWeek. À l’instar de tout contrôle XAML, vous pouvez appliquer un style et redéfinir le modèle de ces éléments.
- La grille du calendrier se compose d’objets [CalendarViewDayItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarViewDayItem). Vous ne pouvez pas appliquer un style ou redéfinir le modèle de ces éléments, mais diverses propriétés sont fournies pour vous permettre de personnaliser leur apparence.

Ce schéma suivant présente les éléments qui constituent l’affichage mensuel du calendrier. Pour plus d’nformations, consultez les remarques sur la classe [CalendarViewDayItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarViewDayItem).

![Éléments d’un affichage de calendrier mensuel](images/calendar-view-month-elements.png)

Ce tableau répertorie les propriétés que vous pouvez modifier pour modifier l’apparence des éléments de calendrier.

Élément | Propriétés
--------|-----------
DayOfWeek | [DayOfWeekFormat](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.dayofweekformat)
CalendarItem | [CalendarItemBackground](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.calendaritembackground), [CalendarItemBorderBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.calendaritemborderbrush), [CalendarItemBorderThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.calendaritemborderthickness), [CalendarItemForeground](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.calendaritemforeground)
DayItem | [DayItemFontFamily](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.dayitemfontfamily), [DayItemFontSize](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.dayitemfontsize), [DayItemFontStyle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.dayitemfontstyle), [DayItemFontWeight](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.dayitemfontweight), [HorizontalDayItemAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.horizontaldayitemalignment), [VerticalDayItemAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.verticaldayitemalignment), [CalendarViewDayItemStyle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.calendarviewdayitemstyle)
MonthYearItem (dans les affichages annuel et décennal, équivalent à DayItem) | [MonthYearItemFontFamily](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.monthyearitemfontfamily), [MonthYearItemFontSize](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.monthyearitemfontsize), [MonthYearItemFontStyle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.monthyearitemfontstyle), [MonthYearItemFontWeight](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.monthyearitemfontweight)
FirstOfMonthLabel | [FirstOfMonthLabelFontFamily](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.firstofmonthlabelfontfamily), [FirstOfMonthLabelFontSize](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.firstofmonthlabelfontsize), [FirstOfMonthLabelFontStyle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.firstofmonthlabelfontstyle), [FirstOfMonthLabelFontWeight](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.firstofmonthlabelfontweight), [HorizontalFirstOfMonthLabelAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.horizontalfirstofmonthlabelalignment), [VerticalFirstOfMonthLabelAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.verticalfirstofmonthlabelalignment), [IsGroupLabelVisible](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.isgrouplabelvisible)
FirstofYearDecadeLabel (dans les affichages annuel et décennal, équivalent à FirstOfMonthLabel) | [FirstOfYearDecadeLabelFontFamily](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.firstofyeardecadelabelfontfamily), [FirstOfYearDecadeLabelFontSize](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.firstofyeardecadelabelfontsize), [FirstOfYearDecadeLabelFontStyle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.firstofyeardecadelabelfontstyle), [FirstOfYearDecadeLabelFontWeight](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.firstofyeardecadelabelfontweight)
Bordures d’état visuel | [FocusBorderBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.focusborderbrush), [HoverBorderBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.hoverborderbrush), [PressedBorderBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.pressedborderbrush), [SelectedBorderBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.selectedborderbrush), [SelectedForeground](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.selectedforeground), [SelectedHoverBorderBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.selectedhoverborderbrush), [SelectedPressedBorderBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.selectedpressedborderbrush)
OutofScope | [IsOutOfScopeEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.isoutofscopeenabled), [OutOfScopeBackground](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.outofscopebackground), [OutOfScopeForeground](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.outofscopeforeground)
Aujourd'hui | [IsTodayHighlighted](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.istodayhighlighted), [TodayFontWeight](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.todayfontweight), [TodayForeground](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.todayforeground)

 Par défaut, l’affichage mensuel affiche 6 semaines à la fois. Vous pouvez modifier le nombre de semaines affiché en définissant la propriété [NumberOfWeeksInView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.numberofweeksinview). Le nombre de semaines minimum à afficher est 2 ; le nombre maximum est 8.

Par défaut, les affichages annuel et décennal s’affichent dans une grille 4x4. Pour modifier le nombre de lignes ou de colonnes, appelez [SetYearDecadeDisplayDimensions](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.setyeardecadedisplaydimensions) avec le nombre souhaité de lignes et de colonnes. Cela modifiera la grille pour les affichages annuel et décennal.

Ici, les affichages annuel et décennal sont définis pour apparaître dans une grille 3x4.

```csharp
calendarView1.SetYearDecadeDisplayDimensions(3, 4);
```

Par défaut, la date minimale affichée dans l’affichage Calendrier est 100 ans avant la date actuelle, et la date maximale affichée est 100 ans après la date actuelle. Vous pouvez changer les dates minimale et maximale qui s’affichent dans le calendrier en définissant les propriétés [MinDate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.mindate) et [MaxDate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.maxdate).

```csharp
calendarView1.MinDate = new DateTime(2000, 1, 1);
calendarView1.MaxDate = new DateTime(2099, 12, 31);
```

### <a name="updating-calendar-day-items"></a>Mise à jour des éléments Jour du calendrier

Dans le calendrier, chaque jour est représenté par un objet [CalendarViewDayItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarViewDayItem). Pour accéder à un élément Jour individuel et utiliser ses propriétés et ses méthodes, gérez l’événement [CalendarViewDayItemChanging](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarview.calendarviewdayitemchanging) et utilisez la propriété Item des arguments d’événement pour accéder à CalendarViewDayItem.

Vous pouvez rendre un jour non sélectionnable dans l’affichage Calendrier, en définissant sa propriété [CalendarViewDayItem.IsBlackout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarviewdayitem.isblackout) sur **true**.

Vous pouvez afficher des informations contextuelles sur la densité des événements d’une journée en appelant la méthode [CalendarViewDayItem.SetDensityColors](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.calendarviewdayitem.setdensitycolors). Vous pouvez afficher de 0 à 10 barres de densité pour chaque jour et définir la couleur de chaque barre.

Voici quelques éléments Jour dans un calendrier. Les jours 1 et 2 sont grisés. Les jours 2, 3 et 4 ont différentes barres de densité définies.

![Jours du calendrier avec des barres de densité](images/calendar-view-density-bars.png)

### <a name="phased-rendering"></a>Rendu progressif

Un affichage Calendrier peut contenir un grand nombre d’objets CalendarViewDayItem. Pour assurer la réactivité de l’interface utilisateur et permettre une navigation fluide dans le calendrier, l’affichage Calendrier prend en charge le rendu progressif. Vous pouvez ainsi répartir le traitement d’un élément Jour en plusieurs phases. Si un jour est déplacé en dehors de l’affichage avant que toutes les phases soient terminées, aucun temps supplémentaire n’est consacré à essayer de traiter et de rendre de cet élément.

Cet exemple montre le rendu progressif d’un affichage Calendrier pour la planification de rendez-vous.
- Dans la phase 0, l’élément Jour par défaut est rendu.
- Dans la phase 1, vous grisez les dates qui ne peuvent pas être réservées. Cela inclut les dates passées, les dimanches et les dates déjà entièrement réservées.
- Dans la phase 2, vous vérifiez chaque rendez-vous réservé pour la journée. Vous affichez une barre de densité verte pour chaque rendez-vous confirmé et une barre de densité bleue pour chaque rendez-vous provisoire.

Dans cet exemple, la classe `Bookings` est tirée d’une application fictive de prise de rendez-vous et n’est pas affichée.

```xaml
<CalendarView CalendarViewDayItemChanging="CalendarView_CalendarViewDayItemChanging"/>
```

```csharp
private void CalendarView_CalendarViewDayItemChanging(CalendarView sender,
                                   CalendarViewDayItemChangingEventArgs args)
{
    // Render basic day items.
    if (args.Phase == 0)
    {
        // Register callback for next phase.
        args.RegisterUpdateCallback(CalendarView_CalendarViewDayItemChanging);
    }
    // Set blackout dates.
    else if (args.Phase == 1)
    {
        // Blackout dates in the past, Sundays, and dates that are fully booked.
        if (args.Item.Date < DateTimeOffset.Now ||
            args.Item.Date.DayOfWeek == DayOfWeek.Sunday ||
            Bookings.HasOpenings(args.Item.Date) == false)
        {
            args.Item.IsBlackout = true;
        }
        // Register callback for next phase.
        args.RegisterUpdateCallback(CalendarView_CalendarViewDayItemChanging);
    }
    // Set density bars.
    else if (args.Phase == 2)
    {
        // Avoid unnecessary processing.
        // You don't need to set bars on past dates or Sundays.
        if (args.Item.Date > DateTimeOffset.Now &&
            args.Item.Date.DayOfWeek != DayOfWeek.Sunday)
        {
            // Get bookings for the date being rendered.
            var currentBookings = Bookings.GetBookings(args.Item.Date);

            List<Color> densityColors = new List<Color>();
            // Set a density bar color for each of the days bookings.
            // It's assumed that there can't be more than 10 bookings in a day. Otherwise,
            // further processing is needed to fit within the max of 10 density bars.
            foreach (booking in currentBookings)
            {
                if (booking.IsConfirmed == true)
                {
                    densityColors.Add(Colors.Green);
                }
                else
                {
                    densityColors.Add(Colors.Blue);
                }
            }
            args.Item.SetDensityColors(densityColors);
        }
    }
}
```

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemple de Galerie de contrôles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery) : tous les contrôles XAML dans un format interactif.

## <a name="related-articles"></a>Articles connexes

- [Contrôles de date et d’heure](date-and-time.md)
- [Sélecteur de dates du calendrier](calendar-date-picker.md)
- [Sélecteur de dates](date-picker.md)
- [Sélecteur d’heure](time-picker.md)
