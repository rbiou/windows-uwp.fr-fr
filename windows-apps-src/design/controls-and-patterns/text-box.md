---
ms.assetid: CC1BF51D-3DAC-4198-ADCB-1770B901C2FC
Description: Le contrôle TextBox permet à un utilisateur d’entrer du texte dans une application.
title: Zone de texte
label: Text box
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 825f2cec4723139f187da6e9ea0d4b2dbb14457c
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970674"
---
# <a name="text-box"></a>Zone de texte

Le contrôle TextBox permet à un utilisateur de taper du texte dans une application. Il est généralement utilisé pour capturer une seule ligne de texte, mais peut être configuré pour la saisie de plusieurs lignes. Le texte s’affiche sous la forme d’un texte brut uniforme simple.

Le contrôle TextBox comporte différentes fonctionnalités destinées à simplifier la saisie de texte. Il intègre un menu contextuel familier prenant en charge la copie et le collage de texte. Le bouton Effacer tout permet à un utilisateur de supprimer rapidement tout le texte qui a été entré. Le contrôle intègre également des fonctionnalités de vérification de l’orthographe qui sont activées par défaut.

**Obtenir la bibliothèque d’interface utilisateur Windows**

|  |  |
| - | - |
| ![Logo WinUI](images/winui-logo-64x64.png) | La bibliothèque d’interface utilisateur Windows version 2.2 ou ultérieure inclut pour ce contrôle un nouveau modèle qui utilise des angles arrondis. Pour plus d’informations, consultez [Rayons des angles](/windows/uwp/design/style/rounded-corner). WinUI est un package NuGet qui contient de nouveaux contrôles et des fonctionnalités d’interface utilisateur pour les applications Windows. Pour plus d’informations, notamment des instructions d’installation, consultez la [bibliothèque d’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

> **API de plateforme** : [classe TextBox class](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox), [propriété Text](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.text)

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

Utilisez un contrôle **TextBox** si vous voulez permettre à un utilisateur de saisir et de modifier du texte sans mise en forme, par exemple dans un formulaire. Vous pouvez utiliser la propriété [Text](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.text) pour obtenir et définir le texte d’un contrôle TextBox.

Vous avez la possibilité de définir un contrôle TextBox en lecture seule, mais il doit s’agir d’un état conditionnel temporaire. Si le texte ne doit jamais être modifiable, envisagez plutôt d’utiliser un contrôle [TextBlock](text-block.md).

Si vous voulez recueillir un mot de passe ou d’autres données confidentielles, comme un numéro de sécurité sociale, utilisez un contrôle [PasswordBox](password-box.md). Une zone de mot de passe ressemble à une zone de saisie de texte, à la différence qu’elle affiche des puces à la place du texte entré.

Pour permettre à l’utilisateur d’entrer des termes de recherche ou pour lui présenter une liste de suggestions dans laquelle il peut effectuer un choix pendant qu’il saisit du texte, utilisez un contrôle [AutoSuggestBox](auto-suggest-box.md).

Pour afficher et modifier des fichiers de texte enrichi, utilisez un contrôle [RichEditBox](rich-edit-box.md).

Pour plus d’informations sur le choix du contrôle de texte approprié, consultez l’article [Contrôles de texte](text-controls.md).

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l’application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/TextBox">ouvrir l’application et voir le contrôle TextBox en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

![Une zone de texte](images/text-box.png)

## <a name="create-a-text-box"></a>Créer une zone de texte

Voici le code XAML pour une zone de texte simple avec un texte d’en-tête et un texte d’espace réservé.

```xaml
<TextBox Width="500" Header="Notes" PlaceholderText="Type your notes here"/>
```

```csharp
TextBox textBox = new TextBox();
textBox.Width = 500;
textBox.Header = "Notes";
textBox.PlaceholderText = "Type your notes here";
// Add the TextBox to the visual tree.
rootGrid.Children.Add(textBox);
```

Voici la zone de texte résultant de ce code XAML.

![Une zone de texte simple](images/text-box-ex1.png)

### <a name="use-a-text-box-for-data-input-in-a-form"></a>Utiliser une zone de texte pour l’entrée de données dans un formulaire

Il est courant d’utiliser une zone de texte pour accepter l’entrée de données dans un formulaire et d’utiliser la propriété [Text](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.text) pour obtenir la chaîne de texte complète de la zone de texte. Vous utilisez généralement un événement tel qu’un clic sur un bouton d’envoi pour accéder à la propriété Text, mais vous pouvez gérer l’événement [TextChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.textchanged) ou [TextChanging](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.textchanging) si vous avez besoin d’effectuer une opération spécifique quand le texte change.

Cet exemple montre comment obtenir et définir le contenu actuel d’une zone de texte.

```xaml
<TextBox name="SampleTextBox" Text="Sample Text"/>
```

```csharp
string sampleText = SampleTextBox.Text;
...
SampleTextBox.Text = "Sample text retrieved";
```

Vous pouvez ajouter un contrôle [Header](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.header) (ou une étiquette) et un contrôle [PlaceholderText](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.placeholdertext) (ou un filigrane) à la zone de texte pour indiquer à l’utilisateur le rôle de cette zone. Si vous voulez personnaliser l’aspect de l’en-tête, vous pouvez définir la propriété [HeaderTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.headertemplate) au lieu de la propriété Header. *Pour plus d’informations sur la conception, consultez Recommandations pour les étiquettes*.

Vous pouvez restreindre le nombre de caractères que l’utilisateur peut taper en définissant la propriété [MaxLength](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.maxlength). La propriété MaxLength ne limite cependant pas la longueur du texte collé. Si cet aspect a de l’importance pour votre application, utilisez l’événement [Paste](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.paste) pour modifier le texte collé.

La zone de texte inclut un bouton Effacer tout (« X ») qui apparaît quand du texte est entré dans la zone. Quand un utilisateur clique sur le bouton « X », le texte figurant dans la zone de texte est effacé. Cette dernière se présente comme suit.

![Une zone de texte avec un bouton Effacer tout](images/text-box-clear-all.png)

Le bouton Effacer tout s’affiche seulement pour les zones de texte modifiables d’une seule ligne qui contiennent du texte et qui ont le focus.

Le bouton Effacer tout n’apparaît pas dans les cas suivants :

- **IsReadOnly** a la valeur **true**.
- **AcceptsReturn** a la valeur **true**.
- **TextWrap** a une valeur différente de **NoWrap**.

Cet exemple montre comment obtenir et définir le contenu actuel d’une zone de texte.

```xaml
<TextBox name="SampleTextBox" Text="Sample Text"/>
```

```csharp
string sampleText = SampleTextBox.Text;
...
SampleTextBox.Text = "Sample text retrieved";
```


### <a name="make-a-text-box-read-only"></a>Configurer une zone de texte en lecture seule

Vous pouvez configurer une zone de texte comme étant en lecture seule en définissant la propriété [IsReadOnly](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.isreadonly) sur **true**. En règle générale, vous activez/désactivez cette propriété dans le code de votre application en fonction de conditions applicables à cette dernière. Si vous avez besoin que le texte soit toujours en lecture seule, envisagez plutôt d’utiliser un contrôle TextBlock.

Pour configurer le contrôle TextBox en lecture seule, définissez la propriété IsReadOnly sur true. Par exemple, vous pouvez avoir besoin de définir un contrôle TextBox permettant à un utilisateur d’entrer des commentaires seulement sous certaines conditions. Vous pouvez configurer le contrôle TextBox comme étant en lecture seule jusqu’à ce que ces conditions soient remplies. Si vous voulez simplement afficher du texte, envisagez plutôt d’utiliser un contrôle TextBlock ou RichTextBlock.

Une zone de texte en lecture seule présente le même aspect qu’une zone de texte en lecture/écriture, ce qui peut dérouter l’utilisateur.
Un utilisateur peut sélectionner et copier du texte.
IsEnabled

### <a name="enable-multi-line-input"></a>Activer la saisie multiligne

Deux propriétés vous permettent d’indiquer si le contrôle de zone de texte peut ou non afficher du texte sur plusieurs lignes. En règle générale, vous définissez ces deux propriétés pour configurer une zone de texte multiligne.

- Pour que la zone de texte autorise et affiche les caractères de saut de ligne ou de retour chariot, définissez la propriété [AcceptsReturn](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.acceptsreturn) sur **true**.
- Pour activer la fonctionnalité de renvoi de texte à la ligne, définissez la propriété [TextWrapping](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.textwrapping) sur **Wrap**. Le texte est alors automatiquement renvoyé à la ligne quand il atteint le bord de la zone de texte, quels que soient les caractères séparateurs de ligne.

> **Remarque**&nbsp;&nbsp;Les contrôles TextBox et RichEditBox ne prennent pas en charge la valeur **WrapWholeWords** pour leur propriété TextWrapping. Si vous tentez d’utiliser WrapWholeWords comme valeur pour TextBox.TextWrapping ou pour RichEditBox.TextWrapping, une exception d’argument non valide est levée.

Une zone de texte multiligne s’agrandit verticalement à mesure que l’utilisateur entre du texte, sauf si le contrôle est restreint par ses propriétés [Height](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.height) ou [MaxHeight](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.maxheight) ou par un conteneur parent. Vous devez vérifier qu’une zone de texte multiligne ne dépasse pas les limites de sa zone visible et limiter son extension dans le cas contraire. Nous vous recommandons de configurer systématiquement une zone de texte multiligne sur une hauteur appropriée, et de ne pas autoriser l’augmentation progressive de cette hauteur à mesure que l’utilisateur tape du texte.

Le défilement à l’aide d’une roulette de défilement ou d’une interaction tactile est automatiquement activé s’il y a lieu. Les barres de défilement verticales ne sont cependant pas visibles par défaut. Vous pouvez afficher les barres de défilement verticales en définissant [ScrollViewer.VerticalScrollBarVisibility](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollbarvisibility) sur **Auto** sur le contrôle ScrollViewer incorporé, comme indiqué ci-après.

```xaml
<TextBox AcceptsReturn="True" TextWrapping="Wrap"
         MaxHeight="172" Width="300" Header="Description"
         ScrollViewer.VerticalScrollBarVisibility="Auto"/>
```

```csharp
TextBox textBox = new TextBox();
textBox.AcceptsReturn = true;
textBox.TextWrapping = TextWrapping.Wrap;
textBox.MaxHeight = 172;
textBox.Width = 300;
textBox.Header = "Description";
ScrollViewer.SetVerticalScrollBarVisibility(textBox, ScrollBarVisibility.Auto);
```

Voici à quoi ressemble la zone de texte après un ajout de texte.

![Une zone de texte multiligne](images/text-box-multi-line.png)

### <a name="format-the-text-display"></a>Mettre en forme l’affichage du texte

Pour aligner le texte figurant dans une zone de texte, utilisez la propriété [TextAlignment](/uwp/api/windows.ui.xaml.controls.textbox.textalignment). Pour aligner la zone de texte dans la disposition de la page, utilisez les propriétés [HorizontalAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.horizontalalignment) et [VerticalAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.verticalalignment).

Bien que la zone de texte prenne en charge seulement du texte sans mise en forme, vous pouvez personnaliser l’affichage du texte dans la zone de texte pour l’adapter à vos besoins. Vous pouvez définir les propriétés [Control](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control) standard, comme [FontFamily](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.fontfamily), [FontSize](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.fontsize), [FontStyle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.fontstyle), [Background](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.background), [Foreground](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.foreground) et [CharacterSpacing](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.characterspacing) pour modifier l’apparence du texte. Ces propriétés affectent seulement la façon dont le texte s’affiche localement dans la zone de texte. Par conséquent, si vous devez copier et coller ce texte par exemple dans un contrôle de texte enrichi, aucune mise en forme n’est appliquée.

Cet exemple montre un contrôle de zone de texte en lecture seule dont plusieurs propriétés sont définies pour personnaliser l’apparence du texte.

```xaml
<TextBox Text="Sample Text" IsReadOnly="True"
         FontFamily="Verdana" FontSize="24"
         FontWeight="Bold" FontStyle="Italic"
         CharacterSpacing="200" Width="300"
         Foreground="Blue" Background="Beige"/>
```

```csharp
TextBox textBox = new TextBox();
textBox.Text = "Sample Text";
textBox.IsReadOnly = true;
textBox.FontFamily = new FontFamily("Verdana");
textBox.FontSize = 24;
textBox.FontWeight = Windows.UI.Text.FontWeights.Bold;
textBox.FontStyle = Windows.UI.Text.FontStyle.Italic;
textBox.CharacterSpacing = 200;
textBox.Width = 300;
textBox.Background = new SolidColorBrush(Windows.UI.Colors.Beige);
textBox.Foreground = new SolidColorBrush(Windows.UI.Colors.Blue);
// Add the TextBox to the visual tree.
rootGrid.Children.Add(textBox);
```

La zone de texte résultante ressemble à ce qui suit.

![Une zone de texte mise en forme](images/text-box-formatted.png)

### <a name="modify-the-context-menu"></a>Modifier le menu contextuel

Par défaut, les commandes figurant dans le menu contextuel de la zone de texte dépendent de l’état de cette dernière. Par exemple, le menu contextuel peut présenter les commandes suivantes quand la zone de texte est modifiable.

Commande | Condition d’affichage
------- | -------------
Copier | L’utilisateur a sélectionné du texte.
Couper | L’utilisateur a sélectionné du texte.
Coller | Le Presse-papiers contient du texte.
Tout sélectionner | Le contrôle TextBox contient du texte.
Annuler | L’utilisateur a modifié du texte.

Pour modifier les commandes figurant dans le menu contextuel, gérez l’événement [ContextMenuOpening](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.contextmenuopening). Pour obtenir un exemple, consultez l’exemple **Personnalisation de l’élément CommandBarFlyout de RichEditBox - Ajout de « Share »** dans la <a href="xamlcontrolsgallery:/item/RichEditBox">Galerie de contrôles XAML</a>. Pour plus d’informations sur la conception, consultez Recommandations pour les [menus contextuels](menus.md).

### <a name="select-copy-and-paste"></a>Sélectionner, copier et coller

Vous pouvez obtenir ou définir le texte sélectionné dans une zone de texte à l’aide de la propriété [SelectedText](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.selectedtext). Utilisez les propriétés [SelectionStart](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.selectionstart) et [SelectionLength](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.selectionlength), et les méthodes [Select](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.select) et [SelectAll](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.selectall), pour manipuler la sélection de texte. Pour exécuter une action spécifique quand l’utilisateur sélectionne ou désélectionne du texte, gérez l’événement [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.selectionchanged). Vous pouvez modifier la couleur de mise en surbrillance du texte sélectionné en définissant la propriété [SelectionHighlightColor](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.selectionhighlightcolor).

Par défaut, le contrôle TextBox prend en charge les opérations de copie et de collage. Vous pouvez appliquer une gestion personnalisée de l’événement [Paste](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.paste) aux contrôles de texte modifiables de votre application. Par exemple, vous pouvez supprimer les sauts de ligne d’une adresse multiligne quand vous collez cette dernière dans une zone de recherche d’une seule ligne. Vous pouvez également vérifier la longueur du texte collé et avertir l’utilisateur si elle dépasse la longueur maximale pouvant être enregistrée dans une base de données. Pour plus d’informations et d’exemples, consultez l’événement [Paste](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.paste).

Vous trouverez ci-dessous un exemple d’utilisation de ces propriétés et méthodes. Quand vous sélectionnez du texte dans la première zone de texte, ce texte s’affiche dans la seconde zone de texte, qui est en lecture seule. Les valeurs des propriétés [SelectionLength](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.selectionlength) et [SelectionStart](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.selectionstart) apparaissent sous la forme de deux blocs de texte. Cette opération est effectuée à l’aide de l’événement [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.selectionchanged).

```xaml
<StackPanel>
   <TextBox x:Name="textBox1" Height="75" Width="300" Margin="10"
         Text="The text that is selected in this TextBox will show up in the read only TextBox below."
         TextWrapping="Wrap" AcceptsReturn="True"
         SelectionChanged="TextBox1_SelectionChanged" />
   <TextBox x:Name="textBox2" Height="75" Width="300" Margin="5"
         TextWrapping="Wrap" AcceptsReturn="True" IsReadOnly="True"/>
   <TextBlock x:Name="label1" HorizontalAlignment="Center"/>
   <TextBlock x:Name="label2" HorizontalAlignment="Center"/>
</StackPanel>
```

```csharp
private void TextBox1_SelectionChanged(object sender, RoutedEventArgs e)
{
    textBox2.Text = textBox1.SelectedText;
    label1.Text = "Selection length is " + textBox1.SelectionLength.ToString();
    label2.Text = "Selection starts at " + textBox1.SelectionStart.ToString();
}
```

Voici le résultat de ce code.

![Texte sélectionné dans une zone de texte](images/text-box-selection.png)

## <a name="choose-the-right-keyboard-for-your-text-control"></a>Choisir le clavier adapté à votre contrôle de texte

Pour faciliter la saisie de données par les utilisateurs au moyen du clavier tactile, ou panneau de saisie, définissez l’étendue des entrées du contrôle de texte de sorte qu’elle corresponde au type de données attendu de la part de l’utilisateur.

Le clavier tactile permet d’entrer du texte quand l’application est exécutée sur un appareil disposant d’un écran tactile. Le clavier tactile est appelé quand l’utilisateur appuie sur un champ d’entrée modifiable, comme un contrôle TextBox ou RichEditBox. Vous pouvez grandement faciliter et accélérer la saisie de données par les utilisateurs dans votre application, en définissant l’étendue des entrées du contrôle de texte afin qu’elle corresponde au type de données attendu de la part de l’utilisateur. L’étendue des entrées fournit au système une indication sur le type d’entrée de texte attendu par le contrôle, afin que le système puisse fournir une disposition de clavier tactile spécialisée pour le type d’entrée.

Par exemple, si une zone de texte est utilisée uniquement pour la saisie d’un code PIN à 4 chiffres, définissez la propriété [InputScope](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.inputscope) sur **Number**. Ceci indique au système qu’il doit afficher la disposition du pavé numérique, ce qui permet à l’utilisateur d’entrer plus facilement le code PIN.

> **Important**&nbsp;&nbsp;L’étendue des entrées ne déclenche aucune validation et n’empêche pas l’utilisateur d’entrer des données via un clavier matériel ou un autre dispositif d’entrée. Vous restez responsable de la validation d’une entrée dans votre code, si nécessaire.

Les autres propriétés qui affectent le clavier tactile sont [IsSpellCheckEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.isspellcheckenabled), [IsTextPredictionEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.istextpredictionenabled) et [PreventKeyboardDisplayOnProgrammaticFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.preventkeyboarddisplayonprogrammaticfocus). (La propriété IsSpellCheckEnabled affecte également le contrôle TextBox quand un clavier matériel est utilisé.)

Pour plus d’informations et d’exemples, consultez [Utiliser l’étendue des entrées pour modifier le clavier tactile](https://docs.microsoft.com/windows/uwp/design/input/use-input-scope-to-change-the-touch-keyboard) et la documentation relative aux propriétés.

## <a name="recommendations"></a>Recommandations

- Utilisez un texte d’étiquette ou d’espace réservé si l’objectif de la zone de texte n’est pas clair. Une étiquette reste toujours visible, qu’il y ait ou non une valeur dans la zone de saisie de texte. Le texte d’espace réservé s’affiche initialement dans la zone de saisie de texte, mais disparaît après qu’une valeur a été entrée.
- Attribuez à la zone de texte une largeur appropriée pour la plage de valeurs qui peuvent être entrées. La longueur des mots varie selon la langue. Par conséquent, tenez compte de la localisation si vous destinez votre application au marché international.
- Une zone de saisie de texte se compose généralement d’une seule ligne (`TextWrap = "NoWrap"`). Si les utilisateurs doivent entrer ou modifier une longue chaîne de caractères, utilisez une zone de saisie de texte multiligne (`TextWrap = "Wrap"`).
- En règle générale, une zone de saisie de texte est utilisée pour du texte modifiable. Vous pouvez cependant utiliser une zone de saisie de texte en lecture seule. Le contenu de la zone pourra être lu, sélectionné et copié, mais pas modifié.
- Pour réduire l’encombrement de l’écran, vous pouvez choisir d’afficher certaines zones de saisie de texte seulement quand la case de contrôle associée est cochée. Vous pouvez également lier l’état activé d’une zone de saisie de texte à un contrôle de type case à cocher.
- Déterminez comment doit se comporter une zone de saisie de texte quand l’utilisateur appuie dessus alors qu’elle contient déjà une valeur. Le comportement par défaut est approprié pour la modification de la valeur plutôt que pour son remplacement. Le point d’insertion est placé entre deux mots et rien n’est sélectionné. Si la valeur d’une zone de saisie de texte a toutes les chances d’être remplacée par l’utilisateur, modifiez le comportement de la zone afin que tout le texte existant soit sélectionné quand le contrôle reçoit le focus, et que la sélection soit remplacée par la nouvelle valeur entrée.

### <a name="single-line-input-boxes"></a>Zones de saisie d’une seule ligne

- Utilisez plusieurs zones de texte d’une ligne pour la saisie de plusieurs éléments de texte de petite taille. Si les zones de texte sont de nature apparentées, regroupez-les.

- Faites en sorte que la taille des zones de texte d’une ligne soit légèrement supérieure à l’entrée la plus longue prévue. Si ceci élargit le contrôle de manière excessive, divisez ce dernier en deux contrôles. Par exemple, vous pouvez diviser une entrée d’adresse en « Ligne d’adresse 1 » et « Ligne d’adresse 2 ».
- Définissez un nombre maximal de caractères pouvant être entrés. Si la source de données de stockage n’accepte pas les chaînes d’entrée longues, limitez la longueur des entrées et utilisez une fenêtre contextuelle de validation pour indiquer à l’utilisateur qu’il a atteint la limite.
- Utilisez des contrôles de saisie de texte d’une ligne pour rassembler de petits éléments de texte.

    L’exemple suivant montre une zone de texte d’une ligne destinée à saisir une réponse à une question de sécurité. La réponse devant être brève, il convient d’utiliser ici une zone de texte d’une seule ligne.

    ![Saisie de données élémentaires](images/guidelines_and_checklist_for_singleline_text_input_type_text.png)

- Utilisez un ensemble de contrôles de saisie de texte d’une ligne courts et de taille fixe pour entrer des données ayant un format spécifique.

    ![Saisie de données mises en forme](images/textinput-example-productkey.png)

- Utilisez un contrôle de saisie de texte d’une ligne sans contraintes pour entrer ou modifier des chaînes, en association avec un bouton de commande qui permet aux utilisateurs de sélectionner des valeurs valides.

    ![Saisie de données assistée](images/textinput-example-assisted.png)

### <a name="multi-line-text-input-controls"></a>Zones de saisie de texte multiligne

- Quand vous créez une zone de texte enrichi, proposez des boutons d’application de style et implémentez leurs actions.
- Utilisez une police cohérente avec le style de votre application.
- Faites en sorte que la hauteur du contrôle de texte soit suffisante pour recevoir des entrées classiques.
- Pour la saisie de chaînes de texte longues dont le nombre de mots ou de caractères est limité, utilisez une zone de texte brut et ajoutez un compteur en temps réel qui montre à l’utilisateur le nombre de caractères ou de mots autorisés restants avant d’atteindre la limite. Vous devez créer le compteur vous-même. Placez-le sous la zone de texte en question et mettez-le à jour de façon dynamique à mesure que l’utilisateur entre chaque mot ou caractère.

    ![Texte long](images/guidelines_and_checklist_for_multiline_text_input_text_limits.png)

- Ne laissez pas les contrôles de saisie de texte s’allonger en hauteur quand les utilisateurs entrent le texte.
- N’utilisez pas de zone de texte de plusieurs lignes quand les utilisateurs n’ont besoin que d’une seule ligne.
- N’utilisez pas de contrôle de texte enrichi si un contrôle de texte brut est approprié.

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemple de Galerie de contrôles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery) : tous les contrôles XAML dans un format interactif.

## <a name="related-articles"></a>Articles connexes

- [Contrôles de texte](text-controls.md)
- [Recommandations sur la vérification orthographique](text-controls.md)
- [Ajout de la fonctionnalité de recherche](https://docs.microsoft.com/previous-versions/windows/apps/hh465231(v=win.10))
- [Recommandations sur la saisie de texte](text-controls.md)
- [TextBox, classe](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)
- [PasswordBox, classe](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PasswordBox)
- [String.Length, propriété](https://docs.microsoft.com/dotnet/api/system.string.length)
