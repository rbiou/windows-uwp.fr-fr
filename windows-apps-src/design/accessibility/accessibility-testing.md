---
Description: Procédures de test à suivre pour vous assurer que votre application Windows est accessible.
ms.assetid: 272D9C9E-B179-4F5A-8493-926D007A0225
title: Test de l’accessibilité
label: Accessibility testing
template: detail.hbs
ms.date: 05/18/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1144f6edfab73069a8645e464a2aa8de3e5e2ed6
ms.sourcegitcommit: e51f9489d8c977c3498afb1a75c91f96ac3a642b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/26/2020
ms.locfileid: "83854755"
---
# <a name="accessibility-testing"></a>Test de l’accessibilité  

Procédures de test à suivre pour vous assurer que votre application Windows est accessible.

## <a name="run-accessibility-testing-tools"></a>Exécuter les outils de test d’accessibilité

Le kit de développement logiciel (SDK) Windows comprend plusieurs outils de test d’accessibilité tels que [**AccScope**](/windows/desktop/WinAuto/accscope), [**Inspect**](/windows/desktop/WinAuto/inspect-objects) et [**l’outil de vérification de l’accessibilité de l’interface utilisateur**](/windows/desktop/WinAuto/ui-accessibility-checker). Ces outils peuvent vous aider à vérifier l’accessibilité de votre application. Veillez à vérifier l’ensemble des scénarios d’application et des éléments d’interface utilisateur.

Vous pouvez lancer les outils de test d’accessibilité à partir d’une invite de commandes Microsoft Visual Studio ou du dossier d’outils du Kit de développement logiciel (SDK) Windows (le sous-répertoire bin de l’emplacement où celui-ci est installé sur votre ordinateur de développement).
  
> [!VIDEO https://www.youtube.com/embed/ce0hKQfY9B8]
  
### <a name="accscope"></a>**Visionneuse de l’étendue de l’ergonomie (AccScope)**  

L’outil [**AccScope**](/windows/desktop/WinAuto/accscope) permet aux développeurs et testeurs d’évaluer l’accessibilité de leur application pendant sa conception et son développement, potentiellement dans les premières phases de prototype, plutôt que dans les dernières phases de test du cycle de développement de l’application. Il est en particulier destiné à tester les scénarios d’accessibilité du Narrateur avec votre application.

### <a name="inspect"></a>**Inspecter**  

[**Inspect**](/windows/desktop/WinAuto/inspect-objects) vous permet de sélectionner n’importe quel élément d’interface utilisateur et d’afficher ses données d’accessibilité. Vous pouvez afficher les propriétés et modèles de contrôles Microsoft UI Automation, et tester la structure de navigation des éléments d’automatisation dans l’arborescence UI Automation. Utilisez **inspecter** au fur et à mesure que vous développez l’interface utilisateur pour vérifier la façon dont les attributs d’accessibilité sont exposés dans UI Automation. Dans certains cas, les attributs proviennent de la prise en charge UI Automation déjà implémentée pour les contrôles XAML par défaut. Dans d’autres cas, les attributs proviennent de valeurs spécifiques que vous avez définies dans votre balisage XAML, en tant que propriétés jointes [**AutomationProperties**](/uwp/api/windows.ui.xaml.automation.automationproperties) .

L’image suivante illustre l’outil [**Inspect**](/windows/desktop/WinAuto/inspect-objects) qui interroge les propriétés UI Automation de l’élément de menu **Modifier** dans le Bloc-notes.

![Capture d’écran de l’outil Inspect.](./images/inspect.png)

### <a name="ui-accessibility-checker"></a>Vérificateur d’accessibilité de l’interface utilisateur

**UI Accessibility Checker (AccChecker)** vous permet d’identifier les problèmes d’accessibilité au moment de l’exécution. Lorsque votre interface utilisateur est complète et fonctionnelle, utilisez **AccChecker** pour tester différents scénarios, vérifier l’exactitude des informations d’accessibilité à l’exécution et identifier les problèmes à l’exécution. Vous pouvez exécuter **AccChecker** en mode d’interface utilisateur ou de ligne de commande. Pour exécuter l’outil en mode d’interface utilisateur, ouvrez le répertoire **AccChecker** dans le répertoire bin du Kit de développement logiciel (SDK) Windows, exécutez acccheckui.exe, puis cliquez sur le menu **Aide**.

### <a name="ui-automation-verify"></a>UI Automation Verify

**UI Automation Verify (UIA Verify)** est une infrastructure de vérification et de test automatisée pour les implémentations UI Automation. **UIA Verify** peut être intégré au code de test et effectuer des tests réguliers et automatisés ou des vérifications ponctuelles de scénarios UI Automation. Pour exécuter **UIA Verify**, exécutez VisualUIAVerifyNative.exe à partir du sous-répertoire UIAVerify.

### <a name="accessible-event-watcher"></a>Accessible Event Watcher

L' [**Observateur d’événements accessible (AccEvent)**](/windows/desktop/WinAuto/accessible-event-watcher) teste si les éléments d’interface utilisateur d’une application déclenchent des événements UI Automation et Microsoft Active Accessibility corrects lorsque des modifications de l’interface utilisateur se produisent. De telles modifications peuvent se produire quand le focus change ou quand un élément d’interface utilisateur est appelé, sélectionné ou subit une modification d’état ou de propriété.

> [!NOTE]
> La plupart des outils de test d’accessibilité mentionnés dans la documentation s’exécutent sur un PC, et non sur un téléphone. Vous pouvez exécuter certains des outils tout en développant et en utilisant un émulateur, mais la plupart d’entre eux ne peuvent pas exposer l’arborescence UI Automation au sein de l’émulateur.

## <a name="test-keyboard-accessibility"></a>Tester l’accessibilité du clavier

Le meilleur moyen de tester l’accessibilité de votre clavier consiste à débrancher la souris et à utiliser le Clavier visuel si vous utilisez une tablette. Testez la navigation de l’accessibilité du clavier à l’aide de la touche _Tab_. Vous devez pouvoir parcourir tous les éléments d’interface utilisateur interactifs à l’aide de la touche _Tab_. Pour les éléments d’interface utilisateur composites, vérifiez que vous pouvez naviguer entre les parties des éléments à l’aide des touches de direction. Par exemple, vous devriez pouvoir naviguer parmi des listes d’éléments à l’aide des touches du clavier. Pour finir, vérifiez que vous pouvez appeler tous les éléments d’interface utilisateur interactifs avec le clavier une fois que ces éléments ont le focus, généralement à l’aide de la touche Entrée ou Espace.

## <a name="verify-the-contrast-ratio-of-visible-text"></a>Vérifier le coefficient de contraste du texte visible

Utilisez des outils de contraste des couleurs pour vérifier que le coefficient de contraste du texte visible est acceptable. Les exceptions comprennent les éléments d’interface utilisateur inactifs, ainsi que les logos et le texte décoratif qui ne transmettent pas d’informations et peuvent être réorganisés sans modifier la signification. Pour plus d’informations sur le coefficient de contraste et les exceptions, voir [Exigences de texte accessible](accessible-text-requirements.md). Pour connaître les outils permettant de tester les coefficients de contraste, voir la spécification [Techniques for WCAG 2.0 G18 (section Resources)](https://www.w3.org/TR/WCAG20-TECHS/G18.html#G18-resources).

> [!NOTE]
> Certains outils répertoriés par la spécification Techniques for WCAG 2.0 G18 ne peuvent pas être utilisés de manière interactive avec une application UWP. Vous pouvez être amené à entrer des valeurs de couleur de premier plan et d’arrière-plan manuellement dans l’outil, ou à effectuer des captures d’écran de l’interface utilisateur de l’application puis à exécuter l’outil de coefficient de contraste sur l’image de capture d’écran, ou à exécuter l’outil tout en ouvrant des fichiers bitmap sources dans un programme d’édition d’images plutôt que pendant que cette image est chargée par l’application.

## <a name="verify-your-app-in-high-contrast"></a>Vérifier votre application en contraste élevé

Utilisez votre application lorsqu’un thème à contraste élevé est actif pour vérifier que tous les éléments d’interface utilisateur s’affichent correctement. Tout le texte doit être lisible, et toutes les images doivent être claires. Ajustez les modèles de contrôles ou ressources de dictionnaires de thèmes XAML pour résoudre les éventuels problèmes de thème dus à des contrôles. Dans les cas où les problèmes de contraste élevé ne sont pas dus aux thèmes ou aux contrôles (par exemple s’ils sont dus à des fichiers image), vous devez fournir des versions distinctes à utiliser lorsqu’un thème à contraste élevé est actif.

## <a name="verify-your-app-with-display-settings"></a>Vérifier votre application avec des paramètres d’affichage  

Utilisez les options d’affichage système qui ajustent la valeur en haute résolution de l’affichage et assurez-vous que l’interface utilisateur de votre application est correctement mise à l’échelle lorsque cette valeur est modifiée. (Certains utilisateurs modifient les valeurs ppp en tant qu’option d’accessibilité. Celle-ci est disponible dans **Options d’ergonomie**, ainsi que les propriétés d’affichage.) Si vous détectez des problèmes, suivez [Recommandations en matière d’expérience utilisateur pour la disposition et la mise à l’échelle](https://developer.microsoft.com/windows/apps/design) et fournissez des ressources supplémentaires pour les différents facteurs de mise à l’échelle.

## <a name="verify-main-app-scenarios-by-using-narrator"></a>Vérifier les scénarios d’application principaux à l’aide du Narrateur

Utilisez le narrateur pour tester l’expérience de lecture d’écran pour votre application.

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-Narrator-and-Dev-Mode/player]

**Procédez comme suit pour tester votre application à l’aide du Narrateur avec une souris et le clavier :**

1. Démarrez le narrateur en appuyant sur la _touche Windows + Ctrl + Entrée_. Dans les versions antérieures à la version 1607 de Windows 10, utilisez la _touche Windows + entrée_ pour démarrer le narrateur.
2. Naviguez dans votre application à l’aide du clavier en utilisant la touche _Tab_, les touches de direction et la _touche Verr. maj + les touches de direction_.
3. À mesure que vous naviguez dans votre application, écoutez le Narrateur lire les éléments de votre interface utilisateur et vérifiez les points suivants :
    - Pour chaque contrôle, vérifiez que le Narrateur lit tout le contenu visible. Vérifiez également qu’il lit le nom de chaque contrôle, tout état applicable (coché, sélectionné, etc.) et le type du contrôle (bouton, case à cocher, élément de liste, etc.).
    - Si l’élément est interactif, vérifiez que vous pouvez utiliser le Narrateur pour appeler son action en appuyant sur _Verr. maj + Entrée_.
    - Pour chaque tableau, vérifiez que le Narrateur lit correctement le nom du tableau, sa description (le cas échéant) et les en-têtes de lignes et de colonnes.
4. Appuyez sur _Verr. maj + Maj + Entrée_ pour effectuer des recherches dans votre application et vérifier que tous vos contrôles apparaissent dans la liste de recherche, et que les noms des contrôles sont localisés et lisibles.
5. Éteignez votre moniteur et essayez d’accomplir les scénarios d’application principaux en utilisant uniquement le clavier et le Narrateur. Pour obtenir la liste complète des commandes et des raccourcis du Narrateur, appuyez sur _Verr. maj + F1_.

À compter de Windows 10 version 1607, nous avons introduit un nouveau mode développeur dans le Narrateur. Activez le mode développeur quand Narrator est déjà en cours d’exécution en appuyant sur _Ctrl + Maj + F12_. Quand le mode développeur est activé, l’écran est masqué et met en évidence uniquement les objets accessibles et le texte associé exposé par programmation au Narrateur. Vous avez ainsi une bonne représentation visuelle des informations qui sont exposées au Narrateur.

**Procédez comme suit pour tester votre application à l’aide du mode tactile du Narrateur :**

> [!NOTE]
> Le Narrateur passe automatiquement en mode tactile sur les appareils qui prennent en charge les contacts 4+. Le Narrateur ne prend pas en charge les scénarios à plusieurs moniteurs ou les numériseurs d’interaction tactile multipoint sur l’écran principal.

1. Familiarisez-vous avec l’interface utilisateur et explorez la disposition.
    - **Naviguez dans l’interface utilisateur en effectuant des mouvements de balayage à l’aide d’un seul doigt.** Effectuez des mouvements de balayage vers la gauche ou la droite pour naviguer entre les éléments et vers le haut ou le bas pour changer la catégorie des éléments parmi lesquels vous naviguez. Les catégories incluent tous les éléments, liens, tableaux, en-têtes, etc. La navigation par mouvements de balayage à l’aide d’un seul doigt est similaire à la navigation avec _Verr. maj + les touches de direction_.
    - **Utilisez les mouvements d’insertion d’une tabulation pour naviguer entre les éléments pouvant être actifs.** Un balayage à trois doigts vers la droite ou la gauche est équivalent à l’utilisation de la touche _Tab_ et des touches _Maj + Tab_ sur un clavier.
    - **Explorez spatialement l’interface utilisateur à l’aide d’un seul doigt.** Déplacez un seul doigt vers le haut et le bas, ou la gauche et la droite, pour que le Narrateur lise les éléments placés sous votre doigt. Vous pouvez utiliser la souris comme alternative, car elle utilise la même logique de test de positionnement avancé que le déplacement d’un seul doigt.
    - **Lisez la fenêtre entière et tout son contenu en balayant l’écran vers le haut avec trois doigts**. Ceci est équivalent à l’utilisation de _Verr. maj + W_.

    Si des éléments d’interface utilisateur importants sont inaccessibles, il s’agit peut-être d’un problème d’accessibilité.

2. Interagissez avec un contrôle pour tester ses actions principales et secondaires, ainsi que son comportement de défilement.

    Les actions principales incluent des choses telles que l’activation d’un bouton, le positionnement d’un curseur de texte et le placement du focus sur le contrôle. Les actions secondaires incluent des actions telles que la sélection d’un élément de liste ou le développement d’un bouton qui propose plusieurs options.

    - Pour tester une action principale : appuyez deux fois ou effectuez un appui prolongé avec un doigt et appuyez avec un autre doigt.
    - Pour tester une action secondaire : appuyez trois fois ou effectuez un appui prolongé avec un doigt et appuyez deux fois avec un autre doigt.
    - Pour tester le comportement de défilement : balayez l’écran avec deux doigts pour effectuer un défilement dans la direction de votre choix.

    Certains contrôles fournissent des actions supplémentaires. Pour afficher la liste complète, touchez l’écran avec quatre doigts.

    Si un contrôle réagit à la souris ou au clavier mais ne réagit pas à une interaction tactile principale ou secondaire, le contrôle doit peut-être implémenter des modèles de contrôles [UI Automation](/windows/desktop/WinAuto/entry-uiauto-win32) supplémentaires.

Vous devez également envisager d’utiliser l’outil [**AccScope**](/windows/desktop/WinAuto/accscope) pour tester les scénarios d’accessibilité du narrateur avec votre application. La [**rubrique relative à l’outil AccScope**](/windows/desktop/WinAuto/accscope) décrit comment configurer **AccScope** pour tester les scénarios du Narrateur.

## <a name="examine-the-ui-automation-representation-for-your-app"></a>Examiner la représentation UI Automation de votre application

Plusieurs des outils de test d’UI Automation mentionnés précédemment permettent d’afficher votre application d’une façon qui, délibérément, ne prend pas en compte son apparence, mais la représente sous la forme d’une structure d’éléments UI Automation. C’est ainsi que les clients UI Automation, principalement les technologies d’assistance, vont interagir avec votre application dans les scénarios d’accessibilité.

L’outil [**AccScope**](/windows/desktop/WinAuto/accscope) fournit une vue particulièrement intéressante de votre application, car vous pouvez voir les éléments UI Automation en tant que représentation visuelle ou en tant que liste. Si vous utilisez la visualisation, vous pouvez explorer les différents éléments d’une façon qui vous permettra de les mettre en corrélation avec l’apparence visuelle de l’interface utilisateur de votre application. Vous pouvez même tester l’accessibilité de vos premiers prototypes d’interface utilisateur avant d’avoir affecté toute la logique à l’interface utilisateur, ce qui garantit que l’interaction visuelle et la navigation dans les scénarios d’accessibilité dans votre application sont équilibrées.

La présence d’éléments apparaissant à tort dans l’affichage des éléments UI Automation est l’un des aspects que vous pouvez tester. Si vous trouvez des éléments que vous souhaitez omettre de la vue, ou inversement, si des éléments sont manquants, vous pouvez utiliser la propriété jointe XAML [**AutomationProperties. AccessibilityView**](/uwp/api/windows.ui.xaml.automation.automationproperties.accessibilityviewproperty) pour ajuster le mode d’affichage des contrôles XAML dans les vues d’accessibilité. Après avoir examiné les affichages d’accessibilité de base, il est également conseillé de revérifier l’ordre de vos tabulations ou la navigation spatiale tel que le permettent les touches de direction. Vous vous assurez ainsi que les utilisateurs peuvent atteindre chacun des éléments interactifs qui sont exposés dans l’affichage de contrôle.

## <a name="related-topics"></a>Rubriques connexes

- [Accessibilité](accessibility.md)
- [Pratiques à éviter](practices-to-avoid.md)
- [Automatisation de l’interface utilisateur](/windows/desktop/WinAuto/entry-uiauto-win32)
- [Accessibilité dans Windows](https://www.microsoft.com/accessibility/)
- [Prise en main du narrateur](https://support.microsoft.com/help/22798/windows-10-complete-guide-to-narrator)
