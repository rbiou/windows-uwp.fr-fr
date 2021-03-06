---
Description: Répertorie les pratiques à éviter si vous souhaitez créer une application Windows accessible.
ms.assetid: 024A9B70-9821-45BB-93F1-61C0B2ECF53E
title: Pratiques d’accessibilité à éviter
label: Accessibility practices to avoid
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 75dad7eb676bd2d2a9d95fa57122085329e5e144
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83233939"
---
# <a name="accessibility-practices-to-avoid"></a>Pratiques d’accessibilité à éviter

Si vous souhaitez créer une application Windows accessible, consultez cette liste de pratiques à éviter : 

* **Évitez de créer des éléments d’interface utilisateur personnalisés si vous pouvez utiliser les contrôles Windows par défaut** ou des contrôles qui ont déjà implémenté la prise en charge de Microsoft UI Automation. Les contrôles Windows standard sont accessibles par défaut et ne nécessitent généralement que l’ajout de quelques attributs d’accessibilité propres à l’application. L’implémentation de la prise en charge de [**AutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) pour un vrai contrôle personnalisé nécessite quant à elle une charge de travail plus importante (voir [Pairs d’automatisation personnalisés](custom-automation-peers.md)).
* **Ne placez pas de texte statique ou d’autres éléments non interactifs dans l’ordre de tabulation** (par exemple, en définissant la propriété [**TabIndex**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.tabindex) pour un élément non interactif). La présence d’éléments non interactifs dans l’ordre de tabulation est contraire aux consignes d’accessibilité du clavier, car elle diminue l’efficacité de la navigation au clavier pour les utilisateurs. De nombreuses technologies d’assistance utilisent l’ordre de tabulation et la possibilité de mettre le focus sur un élément dans le cadre de leur logique pour présenter l’interface d’une application à l’utilisateur de technologie d’assistance. Les éléments de texte uniquement dans l’ordre de tabulation peuvent dérouter les utilisateurs qui s’attendent à rencontrer seulement des éléments interactifs dans l’ordre de tabulation (boutons, cases à cocher, champs de saisie de texte, zones de liste déroulante, listes, etc.).
* **Évitez d’utiliser un positionnement absolu pour les éléments d’interface utilisateur** (comme dans un élément [**Canvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas)), car l’ordre de présentation diffère souvent de l’ordre de déclaration des éléments enfants (qui est l’ordre logique de fait). Dans la mesure du possible, organisez les éléments d’interface utilisateur dans l’ordre du document ou dans l’ordre logique pour vérifier que les lecteurs d’écran peuvent lire ces éléments dans l’ordre correct. Si l’ordre visible des éléments d’interface utilisateur peut divergent du document ou de l’ordre logique, utilisez des valeurs d’index de tabulation explicites (Set [**TabIndex**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.tabindex)) pour définir l’ordre de lecture approprié.
* **N’utilisez pas la couleur comme seule façon de transmettre des informations.** Les utilisateurs qui ne distinguent pas les couleurs ne peuvent pas recevoir des informations qui sont transmises uniquement via la couleur, comme sur un indicateur d’état en couleur. Incluez d’autres signaux visuels, de préférence du texte, pour garantir l’accessibilité des informations.
* **N’actualisez pas automatiquement un canevas d’application entier**, à moins que cela ne soit absolument nécessaire pour la fonctionnalité de l’application. Si vous devez actualiser automatiquement le contenu d’une page, mettez à jour uniquement certaines zones de la page. Les technologies d’assistance doivent généralement supposer qu’un canevas d’application actualisé est une structure entièrement nouvelle, même si les modifications effectives sont minimes. Le coût pour l’utilisateur de technologie d’assistance est que tout affichage de document ou description de l’application actualisée doit maintenant être recréé et représenté à l’utilisateur.
  
  Une navigation de page délibérée initiée par l’utilisateur constitue un cas légitime pour l’actualisation de la structure d’application. Mais vous devez vérifier que l’élément d’interface utilisateur qui démarre la navigation est correctement identifié ou nommé afin de bien signaler que son appel aura pour conséquence un changement de contexte et un rechargement de page.

  > [!NOTE]
  > Si vous actualisez le contenu au sein d’une région, attribuez à la propriété d’accessibilité [**AccessibilityProperties.LiveSetting**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.livesettingproperty) de cet élément un paramètre autre que ceux par défaut (**Polite** ou **Assertive**). Certaines technologies d’assistance peuvent mapper ce paramètre au concept ARIA (Accessible Rich Internet Applications) des zones dynamiques et ainsi indiquer à l’utilisateur qu’une région de contenu a été modifiée.

* **N’utilisez pas d’éléments d’interface utilisateur qui clignotent plus de trois fois par seconde.** Les éléments qui clignotent peuvent provoquer des crises chez certaines personnes. Il est conseillé d’éviter d’utiliser des éléments d’interface utilisateur qui clignotent.
* **Ne modifiez pas le contexte utilisateur ou n’activez pas des fonctionnalités automatiquement.** Les modifications de contexte ou d’activation doivent se produire uniquement lorsque l’utilisateur entreprend une action directe sur un élément d’interface utilisateur qui a le focus. Les modifications apportées au contexte utilisateur comprennent le changement de focus, l’affichage d’un nouveau contenu et la navigation jusqu’à une autre page. Le fait d’apporter des changements de contexte sans impliquer l’utilisateur peut être désorientant pour les utilisateurs souffrant de handicaps. Les exceptions à cette exigence sont notamment l’affichage de sous-menus, la validation de formulaires, l’affichage de texte d’aide dans un autre contrôle et la modification du contexte en réponse à un événement asynchrone.

<span id="related_topics"/>

## <a name="related-topics"></a>Rubriques connexes  
* [Accessibilité](accessibility.md)
* [Accessibilité dans le Windows Store](accessibility-in-the-store.md)
* [Liste de vérification de l’accessibilité](accessibility-checklist.md)
