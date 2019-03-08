---
Description: Pour faciliter la saisie de données par les utilisateurs au moyen du clavier tactile, ou panneau de saisie, définissez l’étendue des entrées du contrôle de texte de sorte qu’elle corresponde au type de données attendu de la part de l’utilisateur.
MS-HAID: dev\_ctrl\_layout\_txt.use\_input\_scope\_to\_change\_the\_touch\_keyboard
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Utiliser l’étendue des entrées pour modifier le clavier tactile
ms.assetid: 6E5F55D7-24D6-47CC-B457-B6231EDE2A71
template: detail.hbs
keywords: clavier, accessibilité, navigation, focus, texte, entrées, interaction utilisateur
ms.date: 02/08/2017
ms.topic: article
ms.openlocfilehash: 1350c6e0eae057386fb721a358f71acb19c4efc1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57591764"
---
# <a name="use-input-scope-to-change-the-touch-keyboard"></a>Utiliser l’étendue des entrées pour modifier le clavier tactile

Pour faciliter la saisie de données par les utilisateurs au moyen du clavier tactile, ou panneau de saisie, définissez l’étendue des entrées du contrôle de texte de sorte qu’elle corresponde au type de données attendu de la part de l’utilisateur.

### <a name="important-apis"></a>API importantes
- [InputScope](https://msdn.microsoft.com/library/windows/apps/hh702632)
- [InputScopeNameValue](https://msdn.microsoft.com/library/windows/apps/hh702028)


Le clavier tactile permet d’entrer du texte lorsque l’application est exécutée sur un appareil disposant d’un écran tactile. Le clavier tactile est appelé lorsque l’utilisateur appuie sur un champ d’entrée modifiable, tel qu’un élément **[TextBox](https://msdn.microsoft.com/library/windows/apps/br209683)** ou **[RichEditBox](https://msdn.microsoft.com/library/windows/apps/br227548)**. Vous pouvez considérablement faciliter et accélérer la saisie de données par les utilisateurs dans votre application en définissant l’*étendue des entrées* du contrôle de texte afin qu’elle corresponde au type de données attendu de la part de l’utilisateur. L’étendue des entrées fournit au système une indication sur le type d’entrée de texte attendu par le contrôle, afin que le système puisse fournir une disposition de clavier tactile spécialisée pour le type d’entrée.

Par exemple, si une zone de texte est utilisée uniquement pour saisir un code confidentiel à 4 chiffres, définissez la propriété [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702632) sur **Number**. Cela indique au système qu’il doit afficher la disposition du pavé numérique, ce qui permet à l’utilisateur d’entrer plus facilement le code confidentiel.

> [!IMPORTANT]
> - Ces informations s’appliquent uniquement au clavier virtuel. Elles ne concernent pas les claviers matériels ou visuels figurant dans les Options d’ergonomie de Windows.
> - L’étendue des entrées n’entraîne l’exécution d’aucune validation des entrées et n’empêche pas l’utilisateur de saisir des données par le biais d’un clavier matériel ou d’un autre dispositif du même ordre. Vous restez responsable de la validation d’une entrée dans votre code, si nécessaire.

## <a name="changing-the-input-scope-of-a-text-control"></a>Modification de l’étendue des entrées d’un contrôle de texte

Les étendues d’entrées disponibles pour votre application appartiennent à l’énumération **[InputScopeNameValue](https://msdn.microsoft.com/library/windows/apps/hh702028)**. Vous pouvez attribuer l’une de ces valeurs à la propriété **InputScope** d’un élément **[TextBox](https://msdn.microsoft.com/library/windows/apps/br209683)** ou **[RichEditBox](https://msdn.microsoft.com/library/windows/apps/br227548)**.

> [!IMPORTANT]
> Le **[InputScope](https://msdn.microsoft.com/library/windows/apps/dn996570)** propriété sur **[PasswordBox](https://msdn.microsoft.com/library/windows/apps/br227519)** prend uniquement en charge la **mot de passe** et  **NumericPin** valeurs. Toute autre valeur est ignorée.

Ici, vous modifiez l’étendue des entrées de plusieurs zones de texte afin qu’elle corresponde aux données attendues pour chaque zone de texte.

**Pour modifier l’étendue des entrées dans XAML**

1.  Dans le fichier XAML de votre page, recherchez la balise correspondant au contrôle de texte que vous voulez modifier.
2.  Ajoutez l’attribut [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702632) à la balise et spécifiez la valeur [**InputScopeNameValue**](https://msdn.microsoft.com/library/windows/apps/hh702028) qui correspond à l’entrée attendue.

    Voici certaines zones de texte pouvant apparaître dans un formulaire de contact client courant. Lorsque [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702632) est défini, un clavier tactile dont la disposition est appropriée aux données s’affiche pour chaque zone de texte.

    ```xaml
    <StackPanel Width="300">
        <TextBox Header="Name" InputScope="Default"/>
        <TextBox Header="Email Address" InputScope="EmailSmtpAddress"/>
        <TextBox Header="Telephone Number" InputScope="TelephoneNumber"/>
        <TextBox Header="Web site" InputScope="Url"/>
    </StackPanel>
    ```

**Pour modifier l’étendue d’entrée dans le code**

1.  Dans le fichier XAML de votre page, recherchez la balise correspondant au contrôle de texte que vous voulez modifier. Le cas échéant, définissez l’[attribut x:Name](https://msdn.microsoft.com/library/windows/apps/mt204788) de façon à pouvoir référencer le contrôle dans votre code.

    ```csharp
    <TextBox Header="Telephone Number" x:Name="phoneNumberTextBox"/>
    ```

2.  Instanciez un nouvel objet [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702025).

    ```csharp
    InputScope scope = new InputScope();
    ```

3.  Instanciez un nouvel objet [**InputScopeName**](https://msdn.microsoft.com/library/windows/apps/hh702027).
    
    ```csharp
    InputScopeName scopeName = new InputScopeName();
    ```

4.  Définissez la propriété [**NameValue**](https://msdn.microsoft.com/library/windows/apps/hh702032) de l’objet [**InputScopeName**](https://msdn.microsoft.com/library/windows/apps/hh702027) sur une valeur de l’énumération [**InputScopeNameValue**](https://msdn.microsoft.com/library/windows/apps/hh702028).

    ```csharp
    scopeName.NameValue = InputScopeNameValue.TelephoneNumber;
    ```

5.  Ajoutez l’objet [**InputScopeName**](https://msdn.microsoft.com/library/windows/apps/hh702027) à la collection [**Names**](https://msdn.microsoft.com/library/windows/apps/hh702034) de l’objet [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702025).

    ```csharp
    scope.Names.Add(scopeName);
    ```

6.  Définissez l’objet [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702025) en tant que valeur de la propriété [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702632) du contrôle de texte.

    ```csharp
    phoneNumberTextBox.InputScope = scope;
    ```

Voici le code dans son ensemble.

```CSharp
InputScope scope = new InputScope();
InputScopeName scopeName = new InputScopeName();
scopeName.NameValue = InputScopeNameValue.TelephoneNumber;
scope.Names.Add(scopeName);
phoneNumberTextBox.InputScope = scope;
```

Toutes ces étapes peuvent être regroupées dans ce code raccourci.

```CSharp
phoneNumberTextBox.InputScope = new InputScope() 
{
    Names = {new InputScopeName(InputScopeNameValue.TelephoneNumber)}
};
```

## <a name="text-prediction-spell-checking-and-auto-correction"></a>Prédiction de texte, vérification orthographique et correction automatique

Les contrôles [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) et [**RichEditBox**](https://msdn.microsoft.com/library/windows/apps/br227548) disposent de plusieurs propriétés influant sur le comportement du panneau de saisie. Pour fournir la meilleure expérience possible à vos utilisateurs, il est important de comprendre de quelle manière ces propriétés affectent la saisie tactile de texte.

-   [**IsSpellCheckEnabled**](https://msdn.microsoft.com/library/windows/apps/br209688)— lors de la vérification orthographique est activée pour un contrôle de texte, le contrôle interagit avec le moteur de vérification orthographique du système pour marquer les mots qui ne sont pas reconnus. Vous pouvez appuyer sur un mot pour afficher une liste de suggestions de corrections. La vérification orthographique est activée par défaut.

    Pour l’étendue d’entrée **Default**, cette propriété permet également la mise en majuscules automatique du premier mot dans une phrase, ainsi que la correction automatique des mots au fur et à mesure de la saisie. Ces fonctionnalités de correction automatique peuvent être désactivées dans les autres zones d’entrées. Pour en savoir plus, consultez les tableaux présentés plus loin dans cette rubrique.

-   [**IsTextPredictionEnabled**](https://msdn.microsoft.com/library/windows/apps/br209690)— lors de la prédiction de texte est activée pour un contrôle de texte, le système affiche une liste de mots qui vous pourrez commencer à taper. Vous pouvez sélectionner un élément dans la liste. Ainsi, vous n’avez pas besoin de saisir le mot entier. La prédiction de texte est activée par défaut.

    La prédiction de texte peut être désactivée si l’étendue des entrées est différente de **Default**, même si la propriété [**IsTextPredictionEnabled**](https://msdn.microsoft.com/library/windows/apps/br209690) présente la valeur **true**. Pour en savoir plus, consultez les tableaux présentés plus loin dans cette rubrique.

-   [**PreventKeyboardDisplayOnProgrammaticFocus**](https://msdn.microsoft.com/library/windows/apps/dn299273)— lorsque cette propriété a **true**, il empêche le système d’afficher le panneau SIP lorsque le focus est défini par programme sur un contrôle de texte. Au lieu de cela, le clavier s’affiche uniquement lorsque l’utilisateur interagit avec le contrôle.

## <a name="touch-keyboard-index-for-windows"></a>Index de clavier tactile pour Windows

Les tableaux suivants présentent les dispositions de panneau d’entrée logicielle (SIP) Windows pour les valeurs d’étendue des entrées courantes. L’effet de l’étendue des entrées sur les fonctionnalités activées par les propriétés **IsSpellCheckEnabled** et **IsTextPredictionEnabled** est répertorié pour chaque étendue des entrées. Cette liste des étendues des entrées disponibles n’est pas complète.

> [!Tip] 
> Vous pouvez activer/désactiver la plupart des claviers tactile entre une disposition alphabétique et une disposition de chiffres et symboles en appuyant sur la **& 123** clé pour modifier la disposition de chiffres et symboles, et appuyez sur la **abcd** clé pour Remplacez par la mise en page alphabétique.

### <a name="default"></a>Default

`<TextBox InputScope="Default"/>`

Les valeur par défaut Windows le clavier tactile.

![Clavier tactile Windows par défaut](images/input-scopes/default.png)
- Vérification orthographique : activée si **IsSpellCheckEnabled** = **true** ; désactivée si **IsSpellCheckEnabled** = **false**.
- Correction automatique : activée si **IsSpellCheckEnabled** = **true** ; désactivée si **IsSpellCheckEnabled** = **false**.
- Mise en majuscule automatique : activée si **IsSpellCheckEnabled** = **true** ; désactivée si **IsSpellCheckEnabled** = **false**.
- Prédiction de texte : activée si **IsTextPredictionEnabled** = **true** ; désactivée si **IsTextPredictionEnabled** = **false**.

### <a name="currencyamountandsymbol"></a>CurrencyAmountAndSymbol

`<TextBox InputScope="CurrencyAmountAndSymbol"/>`

Disposition de clavier numérique et symbolique par défaut

![Clavier tactile Windows pour les devises](images/input-scopes/currencyamountandsymbol.png)

- Inclut les touches gauche/droite de page pour afficher les symboles plus
- Vérification orthographique : activée par défaut ; peut être désactivée
- Vérification orthographique : activée par défaut ; peut être désactivée
- Mise en majuscules automatique : toujours désactivée
- Prédiction de texte : activée par défaut ; peut être désactivée
 
### <a name="url"></a>URL

`<TextBox InputScope="Url"/>`

![Clavier tactile Windows pour les URL](images/input-scopes/url.png)

- Inclut les touches **.com** et ![touche OK](images/input-scopes/kbdgokey.png) (OK). Maintenez la **.com** clé pour afficher des options supplémentaires (**.org**, **.net**et les suffixes spécifiques à la région)
- Inclut le **:**, **-**, et **/** clés
- Vérification orthographique : désactivée par défaut ; peut être activée
- Correction automatique : désactivée par défaut ; peut être activée
- Mise en majuscules automatique : désactivée par défaut ; peut être activée
- Prédiction de texte : désactivée par défaut ; peut être activée


### <a name="emailsmtpaddress"></a>EmailSmtpAddress

`<TextBox InputScope="EmailSmtpAddress"/>`

![Clavier tactile Windows pour les adresses de messagerie](images/input-scopes/emailsmtpaddress.png)
- Inclut les touches **@** et **.com**. Maintenez la **.com** clé pour afficher des options supplémentaires (**.org**, **.net**et les suffixes spécifiques à la région)
- Inclut le **_** et **-** clés
- Vérification orthographique : désactivée par défaut ; peut être activée
- Correction automatique : désactivée par défaut ; peut être activée
- Mise en majuscules automatique : désactivée par défaut ; peut être activée
- Prédiction de texte : désactivée par défaut ; peut être activée


### <a name="number"></a>Numéro

`<TextBox InputScope="Number"/>`

![Clavier tactile Windows pour les nombres](images/input-scopes/number.png)
- Vérification orthographique : activée par défaut ; peut être désactivée
- Vérification orthographique : activée par défaut ; peut être désactivée
- Mise en majuscules automatique : toujours désactivée
- Prédiction de texte : activée par défaut ; peut être désactivée

### <a name="telephonenumber"></a>TelephoneNumber

`<TextBox InputScope="TelephoneNumber"/>`

![Clavier tactile Windows pour les numéros de téléphone](images/input-scopes/telephonenumber.png)
- Vérification orthographique : activée par défaut ; peut être désactivée
- Vérification orthographique : activée par défaut ; peut être désactivée
- Mise en majuscules automatique : toujours désactivée
- Prédiction de texte : activée par défaut ; peut être désactivée

### <a name="search"></a>Recherche

`<TextBox InputScope="Search"/>`

![Clavier tactile Windows pour la recherche](images/input-scopes/search.png)
- Inclut le **recherche** clés au lieu du **entrée** clé
- Vérification orthographique : activée par défaut ; peut être désactivée
- Vérification orthographique : activée par défaut ; peut être désactivée
- Mise en majuscules automatique : toujours désactivée
- Prédiction de texte : activée par défaut ; peut être désactivée

### <a name="searchincremental"></a>SearchIncremental

`<TextBox InputScope="SearchIncremental"/>`

![Clavier tactile de Windows pour la recherche incrémentielle](images/input-scopes/searchincremental.png)
- Même disposition que **par défaut**
- Vérification orthographique : désactivée par défaut ; peut être activée
- Correction automatique : toujours désactivée
- Mise en majuscules automatique : toujours désactivée
- Prédiction de texte : toujours désactivée

### <a name="formula"></a>Formule

`<TextBox InputScope="Formula"/>`

![Clavier tactile de Windows pour la formule](images/input-scopes/formula.png)
- Inclut le **=** clé
- Inclut également le **%**, **$**, et **+** clés
- Vérification orthographique : activée par défaut ; peut être désactivée
- Vérification orthographique : activée par défaut ; peut être désactivée
- Mise en majuscules automatique : toujours désactivée
- Prédiction de texte : activée par défaut ; peut être désactivée

### <a name="chat"></a>Conversation

`<TextBox InputScope="Chat"/>`

![Clavier tactile Windows par défaut](images/input-scopes/default.png)
- Même disposition que **par défaut**
- Vérification orthographique : activée par défaut ; peut être désactivée
- Vérification orthographique : activée par défaut ; peut être désactivée
- Mise en majuscules automatique : activée par défaut ; peut être désactivée
- Prédiction de texte : activée par défaut ; peut être désactivée

### <a name="nameorphonenumber"></a>NameOrPhoneNumber

`<TextBox InputScope="NameOrPhoneNumber"/>`

![Clavier tactile Windows par défaut](images/input-scopes/default.png)
- Même disposition que **par défaut**
- Vérification orthographique : désactivée par défaut ; peut être activée
- Correction automatique : désactivée par défaut ; peut être activée
- Mise en majuscules automatique : désactivée par défaut, peut être est activée (première lettre de chaque mot est en majuscules)
- Prédiction de texte : désactivée par défaut ; peut être activée