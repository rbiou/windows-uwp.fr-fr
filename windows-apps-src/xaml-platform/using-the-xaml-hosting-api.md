---
author: mcleans
description: Cet article décrit comment héberger UWP XAML d’interface utilisateur dans votre application de bureau.
title: À l’aide de l’API d’hébergement dans une application de bureau UWP XAML
ms.author: mcleans
ms.date: 09/21/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp, windows forms, wpf
keywords: Windows 10, uwp, WinForms, wpf, win32
ms.localizationpriority: medium
ms.openlocfilehash: 860e515d013046ef77d0aee38eb5d42c9c3e2dc9
ms.sourcegitcommit: e4f3e1b2d08a02b9920e78e802234e5b674e7223
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/26/2018
ms.locfileid: "4209929"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-desktop-application"></a>À l’aide de l’API d’hébergement dans une application de bureau UWP XAML

> [!NOTE]
> Le XAML UWP hébergeant des API est actuellement disponible en tant qu’un version préliminaire pour développeurs. Bien que nous vous encourageons à tester cette API dans votre propre code prototype maintenant, nous ne recommandons pas que vous l’utiliser dans le code de production pour l’instant. Cette API continuera à mûrir et stabiliser dans les futures versions de Windows. Microsoft ne donne aucune garantie, expresse ou implicite, concernant les informations fournies ici.

À compter de Windows 10 Insider Preview SDK build 17709, les applications de bureau non UWP (y compris les applications C++ Win32, Windows Forms et WPF) peuvent utiliser l' *API d’hébergement de XAML UWP* pour héberger des contrôles UWP dans n’importe quel élément d’interface utilisateur qui est associé à un handle de fenêtre) HWND). Cette API permet aux applications de bureau de non UWP à utiliser les dernières fonctionnalités de l’interface utilisateur de Windows 10 qui sont uniquement disponibles via les contrôles UWP. Par exemple, les applications de bureau non UWP peuvent utiliser cette API pour héberger des contrôles UWP qui utilisent le [Système Fluent Design](../design/fluent-design-system/index.md) et prennent en charge [Windows Ink](../design/input/pen-and-stylus-interactions.md).

Le XAML UWP API d’hébergement fournit les bases pour un plus large ensemble de contrôles que nous fournissons pour permettre aux développeurs de donner à non UWP interface utilisateur Fluent applications de bureau. Ce scénario est parfois appelé *îles XAML*. Pour plus d’informations sur ce scénario de développeur, voir [les contrôles UWP dans les applications de bureau](xaml-host-controls.md).

## <a name="is-the-uwp-xaml-hosting-api-right-for-your-desktop-application"></a>Le XAML UWP héberge API adaptée à votre application de bureau?

Le XAML UWP API d’hébergement fournit l’infrastructure de bas niveau pour l’hébergement de contrôles UWP dans les applications de bureau. Certains types d’applications de bureau ont la possibilité d’utiliser des API de remplacement et plus pratique pour atteindre cet objectif.  

* Si vous avez une application de bureau Win32 C++ et que vous souhaitez pour héberger des contrôles UWP dans votre application, vous devez utiliser l’API d’hébergement de XAML UWP. Il n’existe aucune alternative pour ces types d’applications.

* Pour les applications WPF et Windows Forms, nous vous recommandons d’utiliser les [contrôles encapsulés](xaml-host-controls.md#wrapped-controls) et [hébergez des contrôles](xaml-host-controls.md#host-controls) dans le Kit de ressources de la Communauté Windows au lieu du XAML UWP hébergeant des API. Ces contrôles utilisent le XAML UWP API d’hébergement en interne et fournissent une expérience de développement plus simple. Toutefois, vous pouvez utiliser l’API directement dans ces types d’applications d’hébergement si vous choisissez de XAML UWP.

## <a name="related-samples"></a>Exemples connexes

La manière d’utiliser le XAML UWP hébergeant des API dans votre code dépend de votre type d’application, la conception de votre application et d’autres facteurs. Pour illustrer comment utiliser cette API dans le contexte d’une application complète, cet article fait référence au code dans les exemples suivants.

### <a name="c-win32"></a>Win32 C++

Il existe plusieurs exemples sur GitHub qui montrent comment utiliser le XAML UWP API d’hébergement dans une application Win32 C++:

  * [XamlHostingSample](https://github.com/Microsoft/Windows-appsample-Xaml-Hosting). Cet exemple montre comment ajouter des contrôles UWP [InkCanvas](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas), [InkToolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar)et [MediaPlayerElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) à une application Win32 C++.
  * [XamlIslands32](https://github.com/clarkezone/cppwinrt/tree/master/Desktop/XamlIslandsWin32). Cet exemple montre comment ajouter plusieurs contrôles UWP de base pour une application Win32 C++ et gérer les modifications PPP.

### <a name="wpf-and-windows-forms"></a>WPF et Windows Forms

Le contrôle [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) dans le Kit de ressources de la Communauté Windows agit comme un exemple de référence pour l’utilisation de l’UWP, API d’hébergement dans les applications WPF et Windows Forms. Le code source est disponible aux emplacements suivants:

  * Pour la version WPF du contrôle, [Cliquez ici](https://github.com/Microsoft/WindowsCommunityToolkit/tree/master/Microsoft.Toolkit.Win32/Microsoft.Toolkit.Wpf.UI.XamlHost). La version WPF dérive de [**System.Windows.Interop.HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost).
  * La version Windows Forms du contrôle, [Cliquez ici](https://github.com/Microsoft/WindowsCommunityToolkit/tree/master/Microsoft.Toolkit.Win32/Microsoft.Toolkit.Forms.UI.XamlHost). La version Windows Forms dérive de [**System.Windows.Forms.Control**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control).

## <a name="prerequisites"></a>Conditions préalables

Le XAML UWP API d’hébergement a ces conditions préalables.

* Windows 10 Insider Preview build 17709 (ou une version ultérieure) et la build Insider Preview correspondante du SDK Windows. Dans la mesure où il s’agit d’une fonctionnalité en constante évolution, pour une expérience optimale nous vous recommandons d’utiliser la dernière version disponible.

* Pour utiliser le XAML UWP hébergeant des API dans votre application de bureau, vous devez configurer votre projet afin que vous pouvez appeler des API UWP:

    * **Win32 C++:** Nous vous recommandons de configurer votre projet pour utiliser [C++ / WinRT](../cpp-and-winrt-apis/index.md). Téléchargez et installez le [C++ / Extension WinRT Visual Studio (VSIX)](https://aka.ms/cppwinrt/vsix) à partir de Visual Studio Marketplace, puis ajoutez le ```<CppWinRTEnabled>true</CppWinRTEnabled>``` propriété à votre fichier .vcxproj comme décrit [ici](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

    * **Windows Forms et WPF:** Suivez [ces instructions](../porting/desktop-to-uwp-enhance.md#modify-a-net-project-to-use-uwp-apis).

## <a name="architecture-of-xaml-islands"></a>Architecture des îles XAML

Le XAML UWP API d’hébergement inclut [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource), [**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager)et autres types associés dans l’espace de noms [**Windows.UI.Xaml.Hosting**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting) . Les applications de bureau peuvent utiliser cette API pour rendre les contrôles UWP et la navigation en mode focus clavier dans et en dehors des éléments de l’itinéraire. Les applications de bureau peuvent également dimensionner et positionner les contrôles UWP selon vos besoins.

Lorsque vous créez une île XAML à l’aide du code XAML qui héberge des API dans une application de bureau, vous aurez la hiérarchie d’objets suivante:

* Au niveau de base est l’élément d’interface utilisateur dans votre application dans laquelle vous souhaitez héberger l’île XAML. Cet élément d’interface utilisateur doit avoir un handle de fenêtre (HWND). Éléments d’interface utilisateur dans laquelle vous pouvez héberger une île XAML incluent [**System.Windows.Interop.HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) pour les applications WPF, [**System.Windows.Forms.Control**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) pour les applications Windows Forms et une [fenêtre](https://docs.microsoft.com/windows/desktop/winmsg/about-windows) pour les applications Win32 C++.

* À la prochaine niveau est un objet **DesktopWindowXamlSource** . Cet objet fournit l’infrastructure pour héberger l’île XAML. Votre code est responsable de la création de cet objet, puis association à l’élément d’interface utilisateur parent.

* Lorsque vous créez un **DesktopWindowXamlSource**, cet objet crée automatiquement une fenêtre enfant natif pour héberger votre contrôle UWP. Cette fenêtre enfant native est principalement employés par votre code, mais vous pouvez accéder à son handle (HWND) si nécessaire.

* Enfin, au niveau supérieur est le contrôle UWP que vous souhaitez héberger dans votre application de bureau. Cela peut être n’importe quel objet UWP qui dérive de [**Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), y compris n’importe quel contrôle UWP fourni par le Kit de développement Windows, ainsi que des contrôles utilisateur personnalisés.

Le diagramme suivant illustre la hiérarchie d’objets dans une île XAML.

![Architecture DesktopWindowXamlSource](images/xaml-hosting-api-rev2.png)

## <a name="how-to-host-uwp-xaml-controls"></a>Comment héberger les contrôles XAML UWP

Voici les principales étapes pour héberger un contrôle UWP dans votre application à l’aide de l’API d’hébergement de XAML UWP.

1. Initialisez l’infrastructure XAML UWP pour le thread actuel avant que votre application crée un des objets [**Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) qu’il hébergera dans le [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource).

    * Si votre application crée l’objet **DesktopWindowXamlSource** avant qu’il crée les objets **Windows.UI.Xaml.UIElement** , cette infrastructure sera initialisée pour vous lorsque vous instanciez l’objet **DesktopWindowXamlSource** . Dans ce scénario, vous n’avez pas besoin d’ajouter du code de votre choix pour initialiser l’infrastructure.

    * Toutefois, si votre application crée les objets **Windows.UI.Xaml.UIElement** avant de créer l’objet **DesktopWindowXamlSource** qui hébergera les, votre application doit appeler la méthode statique [** WindowsXamlManager.InitializeForCurrentThread**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) méthode d’initialiser explicitement l’infrastructure XAML UWP avant que les objets **Windows.UI.Xaml.UIElement** sont instanciées. Votre application doit généralement doit appeler cette méthode lorsque l’élément d’interface utilisateur parent qui héberge le **DesktopWindowXamlSource** est instancié.

    ```cppwinrt
    Windows::UI::Xaml::Hosting::WindowsXamlManager windowsXamlManager =
        Windows::UI::Xaml::Hosting::WindowsXamlManager::InitializeForCurrentThread();
    ```

    ```csharp
    global::Windows.UI.Xaml.Hosting.WindowsXamlManager windowsXamlManager =
        global::Windows.UI.Xaml.Hosting.WindowsXamlManager.InitializeForCurrentThread();
    ```

    > [!NOTE]
    > Cette méthode renvoie un objet [**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) qui contient une référence à l’infrastructure XAML UWP. Vous pouvez créer autant d’objets **WindowsXamlManager** que vous le souhaitez sur un thread donné. Toutefois, étant donné que chaque objet conserve une référence à l’infrastructure UWP XAML, vous devez supprimer les objets pour vous assurer que les ressources XAML sont finalement publiées.

2. Créez un objet [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) et l’associer à un élément d’interface utilisateur parent dans votre application qui est associé à un handle de fenêtre.

    Pour ce faire, vous devez procédez comme suit:

    1. Créez un objet **DesktopWindowXamlSource** et effectuer un cast vers l’interface **IDesktopWindowXamlSourceNative** COM. Cette interface est déclarée dans le ```windows.ui.xaml.hosting.desktopwindowxamlsource.h``` fichier d’en-tête dans le SDK Windows. Dans un projet C++ Win32, vous pouvez référencer directement ce fichier d’en-tête. Dans un projet WPF ou Windows Forms, vous devez déclarer cette interface dans votre code d’application à l’aide de l’attribut [**ComImport**](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.comimportattribute) . Assurez-vous que votre déclaration d’interface correspond exactement à la déclaration d’interface dans ```windows.ui.xaml.hosting.desktopwindowxamlsource.h```.

    2. Appelez la méthode **AttachToWindow** de l’interface **IDesktopWindowXamlSourceNative** et transmettre le handle de fenêtre de l’élément d’interface utilisateur parent dans votre application.

    3. Définissez la taille initiale de la fenêtre enfant interne contenue dans le **DesktopWindowXamlSource**. Par défaut, cette fenêtre enfant interne est définie sur une largeur et une hauteur de 0. Si vous ne définissez pas la taille de la fenêtre, tous les contrôles UWP que vous ajoutez à la **DesktopWindowXamlSource** ne sera pas visibles. Pour accéder à la fenêtre enfant interne dans le **DesktopWindowXamlSource**, utilisez la propriété **WindowHandle** de l’interface **IDesktopWindowXamlSourceNative** . Les exemples suivants utilisent la fonction [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) pour définir la taille de la fenêtre.

    Voici quelques exemples de code qui illustrent ce processus.

    ```cppwinrt
    // This example assumes you already have an HWND variable named 'parentHwnd' that
    // contains the handle of the parent window.
    Windows::UI::Xaml::Hosting::DesktopWindowXamlSource desktopWindowXamlSource;
    auto interop = desktopWindowXamlSource.as<IDesktopWindowXamlSourceNative>();
    check_hresult(interop->AttachToWindow(parentHwnd));

    HWND childInteropHwnd = nullptr;
    interop->get_WindowHandle(&childInteropHwnd);

    SetWindowPos(childInteropHwnd, 0, 0, 0, 300, 300, SWP_SHOWWINDOW);
    ```

    ```csharp
    // This WPF example assumes you already have an HwndHost named 'parentHwndHost'
    // that will act as the parent UI elemnt for your XAML island. It also assumes
    // you have used the DllImport attribute to import SetWindowPos from user32.dll
    // as a static method into a class named NativeMethods.
    Windows.UI.Xaml.Hosting.DesktopWindowXamlSource desktopWindowXamlSource =
        new Windows.UI.Xaml.Hosting.DesktopWindowXamlSource();

    IntPtr iUnknownPtr = System.Runtime.InteropServices.Marshal.GetIUnknownForObject(
        desktopWindowXamlSource);
    IDesktopWindowXamlSourceNative desktopWindowXamlSourceNative =
        System.Runtime.InteropServices.Marshal.Marshal.GetTypedObjectForIUnknown(
            iUnknownPtr, typeof(IDesktopWindowXamlSourceNative))
            as IDesktopWindowXamlSourceNative;

    desktopWindowXamlSourceNative.AttachToWindow(parentHwndHost.Handle);

    var childInteropHwnd = desktopWindowXamlSourceNative.WindowHandle;
    NativeMethods.SetWindowPos(childInteropHwnd, HWND_TOP, 0, 0, 300, 300, SWP_SHOWWINDOW);
    ```

3. Définir le **Windows.UI.Xaml.UIElement** que vous souhaitez héberger à la propriété de [**contenu**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) de votre objet **DesktopWindowXamlSource** . L’exemple suivant définit un [**Windows.UI.Xaml.Controls.Grid**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) nommé ```myGrid``` à la propriété de **contenu** .

   ```cppwinrt
   desktopWindowXamlSource.Content(myGrid);
   ```

   ```csharp
   desktopWindowXamlSource.Content = myGrid;
   ```

Pour obtenir des exemples complets qui illustrent les tâches dans le contexte d’un exemple d’application de travail, voir les fichiers de code suivants:

  * **Win32 C++:** Consultez le fichier [Main.cpp](https://github.com/Microsoft/Windows-appsample-Xaml-Hosting/blob/master/XamlHostingSample/Main.cpp) dans l’exemple de [XamlHostingSample](https://github.com/Microsoft/Windows-appsample-Xaml-Hosting) ou le fichier [Desktop.cpp](https://github.com/clarkezone/cppwinrt/blob/master/Desktop/XamlIslandsWin32/Desktop.cpp) dans l’exemple [XamlIslands32](https://github.com/clarkezone/cppwinrt/tree/master/Desktop/XamlIslandsWin32) .
  * **WPF:** Consultez les fichiers [WindowsXamlHostBase.cs](https://github.com/Microsoft/WindowsCommunityToolkit/blob/master/Microsoft.Toolkit.Win32/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs) et [WindowsXamlHost.cs](https://github.com/Microsoft/WindowsCommunityToolkit/blob/master/Microsoft.Toolkit.Win32/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs) dans le Kit de ressources de la Communauté Windows.  
  * **Windows Forms:** Consultez les fichiers [WindowsXamlHostBase.cs](https://github.com/Microsoft/WindowsCommunityToolkit/blob/master/Microsoft.Toolkit.Win32/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs) et [WindowsXamlHost.cs](https://github.com/Microsoft/WindowsCommunityToolkit/blob/master/Microsoft.Toolkit.Win32/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs) dans le Kit de ressources de la Communauté Windows.


## <a name="how-to-host-custom-uwp-xaml-controls"></a>Hôte personnalisé XAML UWP de contrôles

> [!IMPORTANT]
> Actuellement, les contrôles XAML UWP personnalisés à partir de parties 3e sont uniquement pris en charge dans les applications c# WPF et Windows Forms. Vous devez disposer le code source pour les contrôles afin que vous pouvez compiler contre elles dans votre application.

Si vous souhaitez héberger un contrôle XAML UWP personnalisé (un contrôle que vous définissez vous-même ou un contrôle fourni par un 3e tiers), vous devez effectuer les tâches supplémentaires suivantes en plus de la procédure décrite dans la [section précédente](#how-to-host-uwp-xaml-controls).

1. Définissez un type personnalisé qui dérive de [**Windows.UI.Xaml.Application**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application) et implémente également [**IXamlMetadataProvider**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.ixamlmetadataprovider). Ce type agit comme un fournisseur de métadonnées racine pour le chargement des métadonnées pour les types UWP XAML personnalisés dans les assemblys dans le répertoire actif de votre application.

    Pour obtenir un exemple illustrant comment procéder, consultez le fichier de code [XamlApplication.cs](https://github.com/Microsoft/WindowsCommunityToolkit/tree/master/Microsoft.Toolkit.Win32/Microsoft.Windows.Interop.WindowsXamlHost.Shared/XamlApplication.cs) dans le Kit de ressources de la Communauté Windows. Ce fichier fait partie de l’implémentation des classes **WindowsXamlHost** partagée pour WPF et Windows Forms, qui illustrent comment utiliser les API dans ces types d’applications d’hébergement de XAML UWP.

2. Appelez la méthode de [**GetXamlType**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.ixamlmetadataprovider.getxamltype) de votre fournisseur de métadonnées racine lorsque le nom du type du contrôle XAML UWP est affecté (Cela peut être affecté dans le code en cours d’exécution, ou vous pouvez choisir d’activer cette option pour être affectés dans la fenêtre de propriétés de Visual Studio).

    Pour obtenir un exemple illustrant comment procéder, consultez le fichier de code [UWPTypeFactory.cs](https://github.com/Microsoft/WindowsCommunityToolkit/tree/master/Microsoft.Toolkit.Win32/Microsoft.Windows.Interop.WindowsXamlHost.Shared/UWPTypeFactory.cs) dans le Kit de ressources de la Communauté Windows. Ce fichier fait partie de l’implémentation des classes **WindowsXamlHost** partagée pour WPF et Windows Forms.

3. Intégrer le code source pour le contrôle XAML UWP personnalisé à votre solution d’application hôte, la génération du contrôle personnalisé et l’utiliser dans votre application en suivant [les instructions ci-après](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost#add-a-custom-uwp-control).

## <a name="how-to-handle-keyboard-focus-navigation"></a>Comment gérer la navigation en mode focus clavier

Lorsque l’utilisateur navigue dans les éléments d’interface utilisateur dans votre application à l’aide du clavier (par exemple, en appuyant sur touche **Tab** ou flèche/de direction), vous aurez besoin programmer le déplacement du focus dans et en dehors de l’objet **DesktopWindowXamlSource** . Lors de la navigation au clavier de l’utilisateur atteint le **DesktopWindowXamlSource**, déplacer le focus dans le premier objet [**Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) dans l’ordre de navigation pour votre interface utilisateur, continuer à déplacer le focus vers la commande suivante ** Windows.UI.Xaml.UIElement** objets en tant que les cycles de l’utilisateur sur les éléments, puis déplacer le focus précédent en dehors de la **DesktopWindowXamlSource** et dans l’élément d’interface utilisateur parent.  

Le XAML UWP API d’hébergement fournit plusieurs types et membres pour vous aider à accomplir ces tâches.

1. Lorsque la navigation au clavier saisit votre **DesktopWindowXamlSource**, l’événement [**GotFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus) est déclenché. Gérez cet événement et par programmation déplacer le focus vers le premier hébergé **Windows.UI.Xaml.UIElement** à l’aide de la méthode [**NavigateFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus) .

2. Lorsque l’utilisateur se trouve sur le dernier élément pouvant être actif dans votre **DesktopWindowXamlSource** et appuie sur la touche **Tab** ou sur une touche de direction, l’événement [**TakeFocusRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested) est déclenché. Gérez cet événement et par programmation déplacer le focus vers le prochain élément dans l’application hôte. Par exemple, dans une application WPF où le **DesktopWindowXamlSource** est hébergé dans un [**System.Windows.Interop.HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost), vous pouvez utiliser la méthode [**MoveFocus**](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus) pour transférer le focus vers le prochain élément dans l’application hôte.

Pour obtenir des exemples qui montrent comment effectuer cette opération dans le contexte d’un exemple d’application de travail, voir les fichiers de code suivants:
  * **WPF:** Consultez le fichier [WindowsXamlHostBase.Focus.cs](https://github.com/Microsoft/WindowsCommunityToolkit/blob/master/Microsoft.Toolkit.Win32/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs) dans le Kit de ressources de la Communauté Windows.  
  * **Windows Forms:** Consultez le fichier [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/Microsoft/WindowsCommunityToolkit/blob/master/Microsoft.Toolkit.Win32/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs) dans le Kit de ressources de la Communauté Windows.

## <a name="how-to-handle-layout-changes"></a>Comment gérer les modifications de disposition

Lorsque l’utilisateur modifie la taille de l’élément d’interface utilisateur parent, vous devez gérer les modifications de disposition nécessaires pour vous assurer que vos contrôles UWP afficher comme prévu. Voici quelques scénarios importants à prendre en compte.

1. Lorsque l’élément d’interface utilisateur parent doit obtenir la taille de la zone rectangulaire nécessaire pour s’adapter à la **Windows.UI.Xaml.UIElement** que vous hébergez sur le **DesktopWindowXamlSource**, appelez la méthode de [**mesure**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure) de la **Windows.UI.Xaml.UIElement **. Exemple :
    * Dans une application WPF vous ferez cela à partir de la méthode [**MeasureOverride**](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride) de l' [**HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) qui héberge le **DesktopWindowXamlSource**.
    * Dans une application Windows Forms vous ferez cela à partir de la méthode [**GetPreferredSize**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize) du [**contrôle**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) qui héberge le **DesktopWindowXamlSource**.

2. Lorsque les dimensions de l’élément UI parent, appelez la méthode de [**disposition**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange) de la racine **Windows.UI.Xaml.UIElement** qui vous hébergez sur le **DesktopWindowXamlSource**. Exemple :
    * Dans une application WPF vous ferez cela à partir de la méthode [**ArrangeOverride**](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride) de l’objet [**HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) qui héberge le **DesktopWindowXamlSource**.
    * Dans une application Windows Forms vous ferez cela à partir du gestionnaire pour l’événement [**SizeChanged**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged) du [**contrôle**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) qui héberge le **DesktopWindowXamlSource**.

Pour obtenir des exemples qui montrent comment effectuer cette opération dans le contexte d’un exemple d’application de travail, voir les fichiers de code suivants:
  * **WPF:** Consultez le fichier [WindowsXamlHost.Layout.cs](https://github.com/Microsoft/WindowsCommunityToolkit/blob/master/Microsoft.Toolkit.Win32/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) dans le Kit de ressources de la Communauté Windows.  
  * **Windows Forms:** Consultez le fichier [WindowsXamlHost.Layout.cs](https://github.com/Microsoft/WindowsCommunityToolkit/blob/master/Microsoft.Toolkit.Win32/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) dans le Kit de ressources de la Communauté Windows.

## <a name="how-to-handle-dpi-changes"></a>Comment gérer les modifications PPP

Si vous souhaitez gérer les modifications PPP dans la fenêtre qui héberge votre UWP contrôler (par exemple, si l’utilisateur fait glisser la fenêtre entre les deux moniteurs avec divers écrans haute résolution), vous devez configurer le contrôle UWP avec une transformation de rendu, écouter les modifications de PPP de votre application , transformation du contrôle UWP en réponse aux modifications PPP de rendu et mettre à jour la position de la fenêtre.

Les étapes suivantes illustrent un moyen de gérer ce processus dans le contexte d’une application Win32 C++. Pour obtenir un exemple complet, consultez les fichiers de code [Desktop.cpp](https://github.com/clarkezone/cppwinrt/blob/master/Desktop/XamlIslandsWin32/Desktop.cpp) et [Desktop.h](https://github.com/clarkezone/cppwinrt/blob/master/Desktop/XamlIslandsWin32/Desktop.h) dans l’exemple [XamlIslands32](https://github.com/clarkezone/cppwinrt/tree/master/Desktop/XamlIslandsWin32) sur GitHub.

1. Mettre à jour d’un objet [**ScaleTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform) dans votre application et affectez-le à la méthode [**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform) de votre contrôle UWP. L’exemple suivant effectue cette opération pour un contrôle [**Windows.UI.Xaml.Controls.Grid**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) dans une application Win32 C++.

    ```cppwinrt
    // Private fields maintained by your app, such as in a window class you have defined.
    Windows::UI::Xaml::Media::ScaleTransform m_scale;
    Windows::UI::Xaml::Controls::Grid m_rootGrid;

    // Code that runs during initialization, such as the constructor for a window class you have defined.
    m_rootGrid.RenderTransform(m_scale);
    ```

2. Dans votre fonction [**WindowProc**](https://msdn.microsoft.com/library/windows/desktop/ms633573.aspx) , écoutez le message [**WM_DPICHANGED**](https://docs.microsoft.com/windows/desktop/hidpi/wm-dpichanged) . En réponse à ce message:
    * Utilisez la fonction [**SetWindowPos**](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) pour redimensionner la fenêtre qui contient le contrôle UWP pour le rectangle passé au message.
    * Mettre à jour les facteurs d’échelle axes x et y de votre objet **ScaleTransform** en fonction de la nouvelle valeur PPP.
    * Apporter les modifications nécessaires à l’apparence et la disposition de votre contrôle UWP. L’exemple de code suivant ajuste le [**remplissage**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid.padding) du contrôle **Windows.UI.Xaml.Controls.Grid** hébergé en réponse aux changements de PPP.

    ```cppwinrt
    LRESULT HandleDpiChange(HWND hWnd, WPARAM wParam, LPARAM lParam)
    {
        HWND hWndStatic = GetWindow(hWnd, GW_CHILD);
        if (hWndStatic != nullptr)
        {
            UINT uDpi = HIWORD(wParam);

            // Resize the window
            auto lprcNewScale = reinterpret_cast<RECT*>(lParam);

            SetWindowPos(hWnd, nullptr, lprcNewScale->left, lprcNewScale->top,
                lprcNewScale->right - lprcNewScale->left, lprcNewScale->bottom - lprcNewScale->top,
                SWP_NOZORDER | SWP_NOACTIVATE);

            NewScale(uDpi);
          }
          return 0;
    }

    void NewScale(UINT dpi) {

        auto scaleFactor = (float)dpi / 100;

        if (m_scale != nullptr) {
            m_scale.ScaleX(scaleFactor);
            m_scale.ScaleY(scaleFactor);
        }

        ApplyCorrection(scaleFactor);
    }

    void ApplyCorrection(float scaleFactor) {
        float rightCorrection = (m_rootGrid.Width() * scaleFactor - m_rootGrid.Width()) / scaleFactor;
        float bottomCorrection = (m_rootGrid.Height() * scaleFactor - m_rootGrid.Height()) / scaleFactor;

        m_rootGrid.Padding(Windows::UI::Xaml::ThicknessHelper::FromLengths(0, 0, rightCorrection, bottomCorrection));
    }
    ```

2. Pour configurer votre application soit PPP par moniteur prenant en charge, ajoutez un [manifeste de l’assembly côte-à-côte](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) à votre projet et définissez ```<dpiAwareness>``` à un élément de ```PerMonitorV2```. Pour plus d’informations sur cette valeur, voir la description de [**DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2**](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context).

    ```xml
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">
        <application xmlns="urn:schemas-microsoft-com:asm.v3">
            <windowsSettings>
                <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitorV2</dpiAwareness>
            </windowsSettings>
        </application>
    </assembly>
    ```

    Pour un manifeste d’assembly côte-à-côte exemple complet, consultez le fichier [XamlIslandsWin32.exe.manifest](https://github.com/clarkezone/cppwinrt/blob/master/Desktop/XamlIslandsWin32/XamlIslandsWin32.exe.manifest) dans l’exemple [XamlIslands32](https://github.com/clarkezone/cppwinrt/tree/master/Desktop/XamlIslandsWin32) sur GitHub.

## <a name="limitations"></a>Limitations

Le code XAML qui héberge API partage les mêmes limites que tous les autres types de contrôles d’hôte XAML pour Windows 10. Pour une liste détaillée, voir les [limites du contrôle hôte XAML](xaml-host-controls.md#limitations).

## <a name="troubleshooting"></a>Résolution des problèmes

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>Erreur à l’aide de XAML UWP API d’hébergement dans une application UWP

| Problème | Résolution |
|-------|------------|
| Votre application reçoit une **exception COMException** avec le message suivant: «Impossible d’activer DesktopWindowXamlSource. Ce type ne peut pas être utilisé dans une application UWP.» ou «Impossible d’activer WindowsXamlManager. Ce type ne peut pas être utilisé dans une application UWP.» | Cette erreur indique que vous essayez d’utiliser l’API d’hébergement de XAML UWP (plus précisément, vous essayez d’instancier les types [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) ou [**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) ) dans une application UWP. Le XAML UWP API d’hébergement est destiné uniquement à être utilisés dans les applications de bureau non UWP, telles que les applications WPF, Windows Forms et Win32 C++. |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>Erreur pendant la liaison à une fenêtre sur un thread différent

| Problème | Résolution |
|-------|------------|
| Votre application reçoit une **exception COMException** avec le message suivant: «AttachToWindow méthode a échoué car le HWND spécifié a été créé sur un thread différent.» | Cette erreur indique que votre application a appelé la méthode **IDesktopWindowXamlSourceNative.AttachToWindow** et lui passé le HWND d’une fenêtre qui a été créé sur un thread différent. Vous devez transmettre à cette méthode le HWND d’une fenêtre qui a été créé sur le même thread que le code à partir duquel vous appelez la méthode. |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>Erreur pendant la liaison à une fenêtre sur une autre fenêtre de niveau supérieur

| Problème | Résolution |
|-------|------------|
| Votre application reçoit une **exception COMException** avec le message suivant: «AttachToWindow méthode a échoué car le HWND spécifié descend à partir d’une fenêtre de niveau supérieur différente de HWND qui a été précédemment passé à AttachToWindow sur le même thread.» | Cette erreur indique que votre application a appelé la méthode **IDesktopWindowXamlSourceNative.AttachToWindow** et lui passé le HWND d’une fenêtre qui descend à partir d’une fenêtre de niveau supérieur différents qu’une fenêtre que vous avez spécifié dans un appel précédent à cette méthode sur le même thread.</p></p>Une fois que votre application appelle **IDesktopWindowXamlSourceNative.AttachToWindow** sur un thread spécifique, tous les autres objets [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) sur le même thread peuvent joindre uniquement à windows qui sont les descendants de la même fenêtre de niveau supérieur qui a été transmis dans le premier appel à **IDesktopWindowXamlSourceNative.AttachToWindow**. Lorsque tous les objets **DesktopWindowXamlSource** sont fermés pour un thread particulier, la prochaine **DesktopWindowXamlSource** est ensuite libre attacher à n’importe quelle fenêtre à nouveau.</p></p>Pour résoudre ce problème, fermez tous les objets **DesktopWindowXamlSource** qui sont liés aux autres fenêtres de niveau supérieur sur ce thread, ou créent un nouveau thread pour cette **DesktopWindowXamlSource**. |

## <a name="related-topics"></a>Rubriques associées

* [Contrôles UWP dans les applications de bureau](xaml-host-controls.md)