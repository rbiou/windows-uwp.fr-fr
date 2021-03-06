---
description: Cette rubrique décrit les stratégies de gestion des erreurs lors de la programmation avec C++/WinRT.
title: Gestion des erreurs avec C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, erreur, gestion, exception
ms.localizationpriority: medium
ms.openlocfilehash: 1092427659cfbf2fb7d1b5dbfc9cb8802dcfeccd
ms.sourcegitcommit: 1e8f51d5730fe748e9fe18827895a333d94d337f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87296157"
---
# <a name="error-handling-with-cwinrt"></a>Gestion des erreurs avec C++/WinRT

Cette rubrique décrit les stratégies de gestion des erreurs lors de la programmation avec [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt). Pour obtenir des informations plus générales et contextuelles, consultez [Gestion des erreurs et des exceptions (C++ moderne)](/cpp/cpp/errors-and-exception-handling-modern-cpp).

## <a name="avoid-catching-and-throwing-exceptions"></a>Éviter d’intercepter et de lever des exceptions
Nous vous recommandons de continuer à écrire du [code pour parer à toute exception](/cpp/cpp/how-to-design-for-exception-safety), mais d’éviter d’intercepter et de lever des exceptions chaque fois que possible. S’il n’existe aucun gestionnaire pour une exception, Windows génère automatiquement un rapport d’erreurs (notamment un fichier minidump de l’incident), ce qui vous permettra d’identifier la cause du problème.

Ne levez pas une exception que vous prévoyez d’intercepter. Et n’utilisez pas les exceptions pour les échecs attendus. Levez une exception *uniquement lorsqu’une erreur d’exécution inattendue se produit* et gérez toutes les autres erreurs avec des codes d’erreur/de résultat&mdash;directement et à proximité de la source de l’échec. De cette façon, lorsqu’une exception *est* levée, vous savez que la cause est soit un bogue dans votre code, soit un état d’erreur exceptionnel dans le système.

Envisagez le scénario d’accès au Registre Windows. Si votre application ne parvient pas à lire une valeur du Registre, c’est normal et vous devez gérer cela correctement. Ne levez pas d’exception ; retournez plutôt une valeur `bool` ou `enum` indiquant que (et peut-être pourquoi) la valeur n’a pas été lue. Un échec de l’*écriture* d’une valeur dans le Registre est, en revanche, susceptible d’indiquer qu’il existe un problème plus important que vous pouvez gérer judicieusement dans votre application. Dans un cas comme celui-ci, vous ne devez pas laisser votre application continuer, donc une exception qui produit un rapport d’erreurs est le moyen le plus rapide d’empêcher votre application de causer des dommages.

Comme autre exemple, envisagez de récupérer une image miniature d’un appel à [**StorageFile.GetThumbnailAsync**](/uwp/api/windows.storage.storagefile.getthumbnailasync#Windows_Storage_StorageFile_GetThumbnailAsync_Windows_Storage_FileProperties_ThumbnailMode_), puis de transmettre cette miniature à [**BitmapSource.SetSourceAsync**](/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync#Windows_UI_Xaml_Media_Imaging_BitmapSource_SetSourceAsync_Windows_Storage_Streams_IRandomAccessStream_). Si cette séquence d’appels vous oblige à passer `nullptr` à **SetSourceAsync** (le fichier image ne peut pas être lu ; son extension de fichier fait peut-être croire qu’il contient des données d’image, mais en réalité ce n’est pas le cas), vous allez générer la levée d’une exception de pointeur non valide. Si vous découvrez un cas semblable à celui-ci dans votre code, au lieu d’intercepter et de gérer ce cas comme une exception, vérifiez à la place si la valeur `nullptr` a été renvoyée par **GetThumbnailAsync**.

La levée d’exceptions a tendance à être plus lente que l’utilisation de codes d’erreur. Si vous ne levez une exception que lorsqu’une erreur irrécupérable se produit, si tout se passe bien, ce ne sera jamais au détriment des performances.

Mais un gain de performance plus probable implique que la surcharge du runtime s’assure que les destructeurs appropriés sont appelés dans l’éventualité peu probable de la levée d’une exception. Cette assurance est coûteuse, qu’une exception soit en réalité levée ou non. Par conséquent, vous devez vous assurer que le compilateur a une idée claire des fonctions susceptibles de lever des exceptions. Si le compilateur peut prouver qu’il n’y aura aucune exception à partir de certaines fonctions (spécification `noexcept`), il peut alors optimiser le code généré.

## <a name="catching-exceptions"></a>Interception des exceptions
Une condition d’erreur qui survient au niveau de la couche [Windows Runtime ABI](interop-winrt-abi.md#what-is-the-windows-runtime-abi-and-what-are-abi-types) est renvoyée sous la forme d’une valeur HRESULT. Mais vous n’avez pas besoin de gérer des HRESULT dans votre code. Le code de projection C++/WinRT généré pour une API du côté de la consommation détecte un code d’erreur HRESULT au niveau de la couche ABI et le convertit en exception [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error), que vous pouvez capturer et gérer. Si vous voulez *vraiment* gérer des HRESULTS, alors utilisez le type **winrt::hresult**.

Par exemple, si l’utilisateur supprime une image de la bibliothèque d’images pendant que votre application parcourt cette collection, la projection lève une exception. Dans ce cas, vous devez intercepter et gérer cette exception. Voici un exemple de code illustrant ce cas.

```cppwinrt
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Storage.h>
#include <winrt/Windows.UI.Xaml.Media.Imaging.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Storage;
using namespace Windows::UI::Xaml::Media::Imaging;

IAsyncAction MakeThumbnailsAsync()
{
    auto imageFiles{ co_await KnownFolders::PicturesLibrary().GetFilesAsync() };

    for (StorageFile const& imageFile : imageFiles)
    {
        BitmapImage bitmapImage;
        try
        {
            auto thumbnail{ co_await imageFile.GetThumbnailAsync(FileProperties::ThumbnailMode::PicturesView) };
            if (thumbnail) bitmapImage.SetSource(thumbnail);
        }
        catch (winrt::hresult_error const& ex)
        {
            winrt::hresult hr = ex.code(); // HRESULT_FROM_WIN32(ERROR_FILE_NOT_FOUND).
            winrt::hstring message = ex.message(); // The system cannot find the file specified.
        }
    }
}
```

Utilisez ce même modèle dans une coroutine lorsque vous appelez une fonction faisant l’objet d’une instruction `co_await`. Un autre exemple de cette conversion de HRESULT en exception est le cas où une API du composant retourne E_OUTOFMEMORY, ce qui entraîne la levée d’un **std::bad_alloc**.

Préférez [**winrt::hresult_error::code**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errorcode-function) quand vous jetez juste un coup d’œil à un code HRESULT. La fonction [**winrt::hresult_error::to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errorto_abi-function), en revanche, est convertie en objet d’erreur COM et envoie (push) l’état dans le stockage local des threads COM.

## <a name="throwing-exceptions"></a>Levée des exceptions
Dans certains cas, vous déciderez que, si votre appel à une fonction donnée échoue, votre application ne pourra pas être restaurée (vous ne pourrez plus être sûr qu’elle fonctionnera de manière prévisible). L’exemple de code ci-dessous utilise une valeur [**winrt::handle**](/uwp/cpp-ref-for-winrt/handle) comme wrapper autour du HANDLE renvoyé par [**CreateEvent**](https://docs.microsoft.com/windows/desktop/api/synchapi/nf-synchapi-createeventa). Il passe ensuite le handle (en créant une valeur `bool` à partir de celui-ci) au modèle de fonction [**winrt::check_bool**](/uwp/cpp-ref-for-winrt/error-handling/check-bool). **winrt::check_bool** fonctionne avec une valeur `bool` ou avec n’importe quelle valeur convertible en valeur `false` (condition d’erreur) ou `true` (condition de réussite).

```cppwinrt
winrt::handle h{ ::CreateEvent(nullptr, false, false, nullptr) };
winrt::check_bool(bool{ h });
winrt::check_bool(::SetEvent(h.get()));
```

Si la valeur que vous passez à [**winrt::check_bool**](/uwp/cpp-ref-for-winrt/error-handling/check-bool) est false, la séquence d’actions suivante se produit.

- **winrt::check_bool** appelle la fonction [**winrt::throw_last_error**](/uwp/cpp-ref-for-winrt/error-handling/throw-last-error).
- **winrt::throw_last_error** appelle [**GetLastError**](https://docs.microsoft.com/windows/desktop/api/errhandlingapi/nf-errhandlingapi-getlasterror) pour récupérer la valeur du dernier code d’erreur du thread appelant et appelle ensuite la fonction [**winrt::throw_hresult**](/uwp/cpp-ref-for-winrt/error-handling/throw-hresult).
- **winrt::throw_hresult** lève une exception à l’aide d’un objet [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) (ou d’un objet standard) qui représente le code d’erreur.

Étant donné que les API Windows signalent des erreurs d’exécution à l’aide de différents types de valeur de retour, il existe en plus de **winrt::check_bool** de nombreuses autres fonctions d’assistance pour vérifier les valeurs et lever des exceptions.

- [**winrt::check_hresult**](/uwp/cpp-ref-for-winrt/error-handling/check-hresult). Vérifie si le code HRESULT représente une erreur et, si tel est le cas, appelle **winrt::throw_hresult**.
- [**winrt::check_nt**](/uwp/cpp-ref-for-winrt/error-handling/check-nt). Vérifie si un code représente une erreur et, si tel est le cas, appelle **winrt::throw_hresult**.
- [**winrt::check_pointer**](/uwp/cpp-ref-for-winrt/error-handling/check-pointer). Vérifie si un pointeur est null et, si tel est le cas, appelle **winrt::throw_last_error**.
- [**winrt::check_win32**](/uwp/cpp-ref-for-winrt/error-handling/check-win32). Vérifie si un code représente une erreur et, si tel est le cas, appelle **winrt::throw_hresult**.

Vous pouvez utiliser ces fonctions d’assistance pour les types de code de retour courants, ou vous pouvez répondre à n’importe quelle condition d’erreur et appeler [**winrt::throw_last_error**](/uwp/cpp-ref-for-winrt/error-handling/throw-last-error) ou [**winrt::throw_hresult**](/uwp/cpp-ref-for-winrt/error-handling/throw-hresult). 

## <a name="throwing-exceptions-when-authoring-an-api"></a>Levée d’exceptions lors de la création d’une API
Toutes les limites de l’[interface binaire d’application Windows Runtime](interop-winrt-abi.md#what-is-the-windows-runtime-abi-and-what-are-abi-types) (ou limites ABI) doivent être *noexcept*, ce qui signifie que les exceptions ne doivent jamais en sortir. Lorsque vous créez une API, vous devez toujours marquer la limite ABI avec le mot clé `noexcept` en C++. `noexcept` a un comportement spécifique en C++. Si une exception en C++ atteint une limite `noexcept`, le processus échouera rapidement et le message **std::terminate** s’affichera. Ce comportement est généralement souhaitable, car une exception non prise en charge implique presque toujours un état inconnu dans le processus.

Puisque les exceptions ne doivent pas franchir la limite ABI, une condition d’erreur qui survient dans une implémentation est renvoyée via la couche ABI sous la forme d’un code d’erreur HRESULT. Lorsque vous créez une API à l’aide de C++/WinRT, le code est généré pour vous afin de convertir en HRESULT les exceptions que vous levez *effectivement* dans votre implémentation. La fonction [**winrt::to_hresult**](/uwp/cpp-ref-for-winrt/error-handling/to-hresult) est utilisée dans ce code généré dans un modèle comme celui-ci.

```cppwinrt
HRESULT DoWork() noexcept
{
    try
    {
        // Shim through to your C++/WinRT implementation.
        return S_OK;
    }
    catch (...)
    {
        return winrt::to_hresult(); // Convert any exception to an HRESULT.
    }
}
```

[**winrt::to_hresult**](/uwp/cpp-ref-for-winrt/error-handling/to-hresult) gère les exceptions dérivées de **std::exception** et [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) et ses types dérivés. Dans votre implémentation, vous devez préférer **winrt::hresult_error**, ou un type dérivé, afin que les consommateurs de votre API reçoivent des informations d’erreur complètes. **std::exception** (qui correspond à E_FAIL) est pris en charge en cas d’exceptions survenant en raison de l’utilisation de la bibliothèque de modèles standard.

### <a name="debuggability-with-noexcept"></a>Capacité de débogage avec noexcept
Comme nous l’avons mentionné précédemment, une exception en C++ qui atteint une limite `noexcept` échoue rapidement en affichant le message **std::terminate**. Ce n’est pas idéal pour le débogage, car **std::terminate** perd souvent une grande partie, voire la totalité de l’erreur ou du contexte de l’exception levée, en particulier lorsque des coroutines sont impliquées.

Par conséquent, cette section traite de la situation où votre méthode ABI (que vous avez correctement annotée avec `noexcept`) utilise `co_await` pour appeler le code de projection C++/WinRT asynchrone. Nous vous recommandons d’encapsuler les appels au code de projection C++/WinRT dans une classe **winrt::fire_and_forget**. Cela fournit ainsi un emplacement approprié où les exceptions non prises en charge sont correctement enregistrées en tant qu’exceptions stowed, ce qui aide considérablement le débogage.

```cppwinrt
HRESULT MyWinRTObject::MyABI_Method() noexcept
{
    winrt::com_ptr<Foo> foo{ get_a_foo() };

    [/*no captures*/](winrt::com_ptr<Foo> foo) -> winrt::fire_and_forget
    {
        co_await winrt::resume_background();

        foo->ABICall();

        AnotherMethodWithLotsOfProjectionCalls();
    }(foo);

    return S_OK;
}
```

**winrt::fire_and_forget** a une application d’assistance de méthode `unhandled_exception` intégrée qui appelle **winrt::terminate**, laquelle appelle à son tour **RoFailFastWithErrorContext**. Cela garantit que tous les contextes (exception stowed, code d’erreur, message d’erreur, trace de pile, etc.) sont conservés pour le débogage en direct ou pour une sauvegarde post-mortem. Pour plus de commodité, vous pouvez factoriser la partie fire-and-forget dans une fonction distincte qui retourne **winrt::fire_and_forget** et l’appelle ensuite.

### <a name="synchronous-code"></a>Code synchrone
Dans certains cas, votre méthode ABI (que vous avez correctement annotée avec `noexcept`, une fois de plus) appelle uniquement du code synchrone. En d’autres termes, elle n’utilise jamais `co_await`, que ce soit pour appeler une méthode Windows Runtime asynchrone ou pour basculer entre les threads de premier plan et d’arrière-plan. Alors, la technique fire_and_forget continue de fonctionner, mais elle n’est pas efficace. Au lieu de cela, vous pouvez procéder comme suit.

```cppwinrt
HRESULT abi() noexcept try
{
    // ABI code goes here.
} catch (...) { winrt::terminate(); }
```

### <a name="fail-fast"></a>Échouer rapidement
Le code de la section précédente échoue toujours rapidement. Comme nous l’avons écrit, ce code ne gère pas les exceptions. Toute exception non prise en charge entraîne l’arrêt du programme.

Mais ce formulaire surmonte ce problème, car il assure la capacité de débogage. Dans de rares cas, vous souhaiterez peut-être `try/catch` et prendre en charge certaines exceptions. Comme expliqué dans cette rubrique, cette situation reste rare, car nous vous déconseillons d’utiliser des exceptions comme mécanisme de contrôle des flux pour les conditions prévues.

N’oubliez pas qu’il est déconseillé de laisser une exception non prise en charge s’échapper d’un contexte `noexcept` de type naked. Dans cette condition, le runtime C++ va terminer le processus avec **std::terminate**, ce qui vous fera perdre toutes les informations sur les exceptions stowed soigneusement enregistrées par C++/WinRT.

## <a name="assertions"></a>Assertions
Pour les hypothèses internes dans votre application, il existe des assertions. Privilégiez autant que possible **static_assert** pour la validation au moment de la compilation. Pour les conditions d’exécution, utilisez `WINRT_ASSERT` avec une expression booléenne. `WINRT_ASSERT` est une définition de macro qui s’étend à [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros).

```cppwinrt
WINRT_ASSERT(pos < size());
```

WINRT_ASSERT est compilé à part dans les builds Release ; dans une version de débogage, il arrête l’application dans le débogueur sur la ligne de code où se trouve l’assertion.

Vous ne devez pas utiliser d’exception dans vos destructeurs. Par conséquent, au moins dans les versions de débogage, vous pouvez déclarer le résultat de l’appel d’une fonction à partir d’un destructeur à l’aide de WINRT_VERIFY (avec une expression booléenne) et de WINRT_VERIFY_ (avec un résultat attendu et une expression booléenne).

```cppwinrt
WINRT_VERIFY(::CloseHandle(value));
WINRT_VERIFY_(TRUE, ::CloseHandle(value));
```

## <a name="important-apis"></a>API importantes
* [Modèle de fonction winrt::check_bool](/uwp/cpp-ref-for-winrt/error-handling/check-bool)
* [Fonction winrt::check_hresult](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)
* [Modèle de fonction winrt::check_nt](/uwp/cpp-ref-for-winrt/error-handling/check-nt)
* [Modèle de fonction winrt::check_pointer](/uwp/cpp-ref-for-winrt/error-handling/check-pointer)
* [Modèle de fonction winrt::check_win32](/uwp/cpp-ref-for-winrt/error-handling/check-win32)
* [Struct winrt::handle](/uwp/cpp-ref-for-winrt/handle)
* [Struct winrt::hresult_error](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)
* [Fonction winrt::throw_hresult](/uwp/cpp-ref-for-winrt/error-handling/throw-hresult)
* [Fonction winrt::throw_last_error](/uwp/cpp-ref-for-winrt/error-handling/throw-last-error)
* [Fonction winrt::to_hresult](/uwp/cpp-ref-for-winrt/error-handling/to-hresult)

## <a name="related-topics"></a>Rubriques connexes
* [Gestion des erreurs et des exceptions (C++ moderne)](/cpp/cpp/errors-and-exception-handling-modern-cpp)
* [Procédure : conception pour parer à toute exception](/cpp/cpp/how-to-design-for-exception-safety)
