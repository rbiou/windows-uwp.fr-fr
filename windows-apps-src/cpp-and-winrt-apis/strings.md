---
description: Avec C++/WinRT, vous pouvez appeler des API Windows Runtime à l’aide de types de chaînes étendues C++ standard, ou vous pouvez utiliser le type winrt::hstring.
title: Gestion des chaînes en C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, chaîne
ms.localizationpriority: medium
ms.openlocfilehash: 1771c3754e8e9580514f646ae8589b1982911fc7
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/29/2020
ms.locfileid: "79448565"
---
# <a name="string-handling-in-cwinrt"></a>Gestion des chaînes en C++/WinRT

Avec [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), vous pouvez appeler des API Windows Runtime à l’aide de types de chaînes étendues de la bibliothèque C++ standard comme **std::wstring** (remarque : pas avec des types de chaînes étroites comme **std::string**). C++/WinRT possède un type de chaîne personnalisée appelé [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) (défini dans la bibliothèque de base C++/WinRT, à savoir `%WindowsSdkDir%Include\<WindowsTargetPlatformVersion>\cppwinrt\winrt\base.h`). Et c’est en fait le type de chaîne que les constructeurs, fonctions et propriétés Windows Runtime prennent et renvoient. Mais, dans de nombreux cas &mdash; grâce aux constructeurs de conversion et aux opérateurs de conversion de **hstring** &mdash; vous pouvez choisir de tenir compte ou non de **hstring** dans votre code client. Si vous *créez* des API, vous devrez probablement connaître **hstring**.

Il existe de nombreux types de chaîne en C++. Des variantes existent dans de nombreuses bibliothèques en plus de **std::basic_string** de la bibliothèque C++ standard. C++17 possède des utilitaires de conversion de chaînes et **std::basic_string_view** pour combler les écarts entre tous les types de chaîne.  [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) fournit une convertibilité avec **std::wstring_view** afin de garantir l’interopérabilité pour laquelle **std::basic_string_view** a été conçu.

## <a name="using-stdwstring-and-optionally-winrthstring-with-uri"></a>Utilisation de **std::wstring** (et éventuellement **winrt::hstring**) avec **Uri**
[**Windows::Foundation::Uri**](/uwp/api/windows.foundation.uri) est construit à partir d’un [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring).

```cppwinrt
public:
    Uri(winrt::hstring uri) const;
```

Mais **hstring** a des [constructeurs de conversion](/uwp/cpp-ref-for-winrt/hstring#hstringhstring-constructor) qui vous permettent de l’utiliser sans avoir besoin d’en tenir compte. Voici un exemple de code illustrant comment créer un **Uri** à partir d’un littéral de chaîne étendue, à partir d’une vue de chaîne étendue et à partir d’un **std::wstring**.

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <string_view>

using namespace winrt;
using namespace Windows::Foundation;

int main()
{
    using namespace std::literals;

    winrt::init_apartment();

    // You can make a Uri from a wide string literal.
    Uri contosoUri{ L"http://www.contoso.com" };

    // Or from a wide string view.
    Uri contosoSVUri{ L"http://www.contoso.com"sv };

    // Or from a std::wstring.
    std::wstring wideString{ L"http://www.adventure-works.com" };
    Uri awUri{ wideString };
}
```

L’accesseur de propriété [**Uri::Domain**](https://docs.microsoft.com/uwp/api/windows.foundation.uri.Domain) est de type **hstring**.

```cppwinrt
public:
    winrt::hstring Domain();
```

Mais, là encore, vous devez prendre conscience que ce détail est optionnel grâce à l’[opérateur de conversion vers **std::wstring_view**](/uwp/cpp-ref-for-winrt/hstring#hstringoperator-stdwstring_view) de **hstring**.

```cppwinrt
// Access a property of type hstring, via a conversion operator to a standard type.
std::wstring domainWstring{ contosoUri.Domain() }; // L"contoso.com"
domainWstring = awUri.Domain(); // L"adventure-works.com"

// Or, you can choose to keep the hstring unconverted.
hstring domainHstring{ contosoUri.Domain() }; // L"contoso.com"
domainHstring = awUri.Domain(); // L"adventure-works.com"
```

De même, [**IStringable::ToString**](https://docs.microsoft.com/windows/desktop/api/windows.foundation/nf-windows-foundation-istringable-tostring) renvoie hstring.

```cppwinrt
public:
    hstring ToString() const;
```

**Uri** implémente l’interface [**IStringable**](https://docs.microsoft.com/windows/desktop/api/windows.foundation/nn-windows-foundation-istringable).

```cppwinrt
// Access hstring's IStringable::ToString, via a conversion operator to a standard type.
std::wstring tostringWstring{ contosoUri.ToString() }; // L"http://www.contoso.com/"
tostringWstring = awUri.ToString(); // L"http://www.adventure-works.com/"

// Or you can choose to keep the hstring unconverted.
hstring tostringHstring{ contosoUri.ToString() }; // L"http://www.contoso.com/"
tostringHstring = awUri.ToString(); // L"http://www.adventure-works.com/"
```

Vous pouvez utiliser la fonction [**hstring::c_str**](/uwp/cpp-ref-for-winrt/hstring#hstringc_str-function) pour obtenir une chaîne étendue standard à partir d’un **hstring** (comme à partir d’un **std::wstring**).

```cppwinrt
#include <iostream>
std::wcout << tostringHstring.c_str() << std::endl;
```
Si vous avez un **hstring**, vous pouvez créer un **Uri** à partir de celui-ci.

```cppwinrt
Uri awUriFromHstring{ tostringHstring };
```

Envisagez d’une méthode qui prend un **hstring**.

```cppwinrt
public:
    Uri CombineUri(winrt::hstring relativeUri) const;
```

Toutes les options que vous venez de voir s’appliquent également dans de tels cas.

```cppwinrt
std::wstring contact{ L"contact" };
contosoUri = contosoUri.CombineUri(contact);
    
std::wcout << contosoUri.ToString().c_str() << std::endl;
```

**hstring** a un opérateur de conversion **std::wstring_view** membre, et la conversion est obtenue sans effort.

```cppwinrt
void legacy_print(std::wstring_view view);

void Print(winrt::hstring const& hstring)
{
    legacy_print(hstring);
}
```

## <a name="winrthstring-functions-and-operators"></a>Fonctions et opérateurs **winrt::hstring**
Une série de constructeurs, d’opérateurs, de fonctions et d’itérateurs sont implémentés pour [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring).

Un **hstring** étant une plage, vous pouvez l’utiliser avec `for` basé sur les plages, ou avec `std::for_each`. Il fournit également des opérateurs de comparaison pour effectuer des comparaisons naturelles et efficaces à ses homologues dans la bibliothèque C++ standard. Et il inclut tout ce dont vous avez besoin pour utiliser **hstring** comme clé pour les conteneurs associatifs.

Nous reconnaissons que de nombreuses bibliothèques C++ utilisent **std::string** et fonctionnent exclusivement avec du texte UTF-8. Pour votre commodité, nous fournissons des programmes d’assistance, tels que [**winrt::to_string**](/uwp/cpp-ref-for-winrt/to-string) et [**winrt::to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring), pour la conversion arrière et dans les deux sens.

`WINRT_ASSERT` est une définition de macro qui s’étend à [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros).

```cppwinrt
winrt::hstring w{ L"Hello, World!" };

std::string c = winrt::to_string(w);
WINRT_ASSERT(c == "Hello, World!");

w = winrt::to_hstring(c);
WINRT_ASSERT(w == L"Hello, World!");
```

Pour obtenir plus d’informations et des exemples sur les fonctions et opérateurs **hstring**, consultez la rubrique sur les informations de référence API [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring).

## <a name="the-rationale-for-winrthstring-and-winrtparamhstring"></a>Logique pour **winrt::hstring** et **winrt::param::hstring**
Windows Runtime est implémenté en termes de caractères **wchar_t**, mais l’interface binaire d’application (ABI) de Windows Runtime n’est pas un sous-ensemble de ce que fournit **std::wstring** ou **std::wstring_view**. L’utilisation de ceux-ci entraînerait des inefficacités importantes. Au lieu de cela, C++/WinRT fournit **winrt::hstring**, qui représente une chaîne immuable cohérente avec le [HSTRING](https://docs.microsoft.com/windows/desktop/WinRT/hstring) sous-jacent et implémenté derrière une interface similaire à celle de **std::wstring**. 

Vous pouvez remarquer que les paramètres d’entrée C++/WinRT qui doivent accepter logiquement **winrt::hstring** attendent en fait **winrt::param::hstring**. L’espace de nom **param** contient un ensemble de types utilisés exclusivement pour optimiser les paramètres d’entrée afin d’effectuer la liaison naturelle aux types de la bibliothèque C++ standard et d’éviter des copies et autres inefficacités. Vous ne devez pas utiliser ces types directement. Si vous voulez utiliser une optimisation pour vos propres fonctions, utilisez **std::wstring_view**. Consultez également [Passage de paramètres à la frontière ABI](/windows/uwp/cpp-and-winrt-apis/pass-parms-to-abi).

Le résultat est que vous pouvez ignorer les spécificités de la gestion des chaînes Windows Runtime, et travailler efficacement avec ce que vous connaissez. Et c’est important étant donné l’utilisation intensive des chaînes dans Windows Runtime.

## <a name="formatting-strings"></a>Mise en forme des chaînes
Une des options de mise en forme des chaînes est **std::wostringstream**. Voici un exemple qui met en forme et affiche un message de suivi de débogage simple.

```cppwinrt
#include <sstream>
#include <winrt/Windows.UI.Input.h>
#include <winrt/Windows.UI.Xaml.Input.h>
...
void MainPage::OnPointerPressed(winrt::Windows::UI::Xaml::Input::PointerRoutedEventArgs const& e)
{
    winrt::Windows::Foundation::Point const point{ e.GetCurrentPoint(nullptr).Position() };
    std::wostringstream wostringstream;
    wostringstream << L"Pointer pressed at (" << point.X << L"," << point.Y << L")" << std::endl;
    ::OutputDebugString(wostringstream.str().c_str());
}
```

## <a name="the-correct-way-to-set-a-property"></a>La bonne méthode pour définir une propriété

Vous définissez une propriété en passant une valeur à une fonction setter. Voici un exemple.

```cppwinrt
// The right way to set the Text property.
myTextBlock.Text(L"Hello!");
```

Le code ci-dessous est incorrect. Il se compile, mais il ne fait que modifier la **winrt::hstring** temporaire retournée par la fonction d’accesseur **Text()** , puis supprimer le résultat.

```cppwinrt
// *Not* the right way to set the Text property.
myTextBlock.Text() = L"Hello!";
```

## <a name="important-apis"></a>API importantes
* [Structure winrt::hstring](/uwp/cpp-ref-for-winrt/hstring)
* [Fonction winrt::to_hstring](/uwp/cpp-ref-for-winrt/to-hstring)
* [Fonction winrt::to_string](/uwp/cpp-ref-for-winrt/to-string)
