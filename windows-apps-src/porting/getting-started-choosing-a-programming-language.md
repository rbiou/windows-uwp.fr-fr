---
title: Choix d’un langage de programmation
ms.assetid: 6CA46432-BF03-4B20-9187-565B3503B497
description: Choix d’un langage de programmation
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 10673184b44f9c4c52e2ee5ad3cf4ba4a1238497
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493304"
---
# <a name="getting-started-choosing-a-programming-language"></a>Bien démarrer : Choix d’un langage de programmation

## <a name="choosing-a-programming-language"></a>Choix d’un langage de programmation

Avant d’aller plus loin, vous devez connaître les langages de programmation permettant de développer des applications pour la plateforme Windows universelle (UWP). Bien que les procédures pas à pas du présent article utilisent le langage C#, vous pouvez développer des applications pour la plateforme Windows universelle (UWP) à l’aide d’un ou de plusieurs langages de programmation (voir [Langages, outils et infrastructures](https://docs.microsoft.com/previous-versions/windows/apps/dn465799(v=win.10))).

Vous pouvez développer à l’aide des langages C++, C#, Microsoft Visual Basic et JavaScript. JavaScript utilise le balisage HTML5 pour la disposition de l’interface utilisateur et les autres langages utilisent un langage de balisage appelé *XAML (Extensible Application Markup Language)* pour décrire leur interface utilisateur.

Bien que l’article se concentre ici sur C#, les autres langages offrent des avantages uniques qu’il peut être intéressant d’explorer. Par exemple, si la performance de votre application est une priorité, surtout pour des composants graphiques complexes, C++ peut s’avérer le meilleur choix. La version Microsoft .NET de Visual Basic donne de très bons résultats pour les développeurs d’applications Visual Basic. JavaScript avec HTML5 convient parfaitement pour les professionnels spécialisés dans le développement Web. Pour plus d’informations, voir les rubriques suivantes :

-   [Créer une application « Hello, World ! » à l’aide de C# ou de Visual Basic](../get-started/create-a-hello-world-app-xaml-universal.md)
-   [Créer une application « Hello, World ! » à l’aide de C++/WinRT](/windows/uwp/get-started/create-a-basic-windows-10-app-in-cppwinrt)
-   [Créer une application « Hello, World ! » à l’aide de C++/CX](/windows/uwp/get-started/create-a-basic-windows-10-app-in-cpp)
-   [Créer une application « Hello, World ! » à l’aide de JavaScript](../get-started/create-a-hello-world-app-js-uwp.md)

**Remarque**    Pour les applications qui utilisent des graphiques 3D, les normes OpenGL et OpenGL ES ne sont pas disponibles en mode natif pour les applications UWP. Si vous préférez ne pas réécrire votre code OpenGL ES dans Microsoft DirectX, vous pourriez être intéressé par **Angle**. Angle est un projet en cours conçu pour convertir OpenGL en DirectX en transformant des appels d’API OpenGL en appels d’API DirectX. Pour en savoir plus, consultez les articles suivants :
-   [Angle](https://bugs.chromium.org/p/angleproject/)
-   [Créer votre première application UWP à l’aide de DirectX](https://docs.microsoft.com/previous-versions/windows/apps/br229580(v=win.10))
-   [Exemples d’applications UWP utilisant DirectX](https://docs.microsoft.com/samples/browse/?expanded=windows&products=windows-uwp&terms=directx)
-   [Où est le kit SDK DirectX ?](https://docs.microsoft.com/windows/desktop/directx-sdk--august-2009-)

## <a name="giving-c-a-go"></a>Essayer C#

En tant que développeur iOS, vous êtes accoutumé à Objective-C et à Swift. Le langage de programmation Microsoft le plus proche des deux est C#. Pour la majorité des développeurs et des applications, nous pensons que C# est le langage le plus simple et le plus rapide à apprendre et à utiliser. C’est pourquoi les informations et procédures fournies dans cet article se concentrent sur ce langage. Pour en savoir plus sur le langage C#, voir les rubriques suivantes :

-   [Créez votre première application UWP à l’aide de C# ou de Visual Basic](../get-started/create-a-hello-world-app-xaml-universal.md)
-   [Exemples d’applications UWP utilisant C #](https://docs.microsoft.com/samples/browse/?expanded=windows&products=windows-uwp&languages=csharp)
-   [Visual C#](https://msdn.microsoft.com/library/kx37x362.aspx)

Voici une classe écrite avec les deux langages, Objective-C et C#. La version Objective-C apparaît en premier, suivie de la version C#.

```obj-c
// Objective-C header: SampleClass.h.

#import <Foundation/Foundation.h>

@interface SampleClass : NSObject {
    BOOL localVariable;
}

@property (nonatomic) BOOL localVariable;

-(int) addThis: (int) firstNumber andThis: (int) secondNumber;

@end
```

```obj-c
// Objective-C implementation.

#import "SampleClass.h"

@implementation SampleClass

@synthesize localVariable = _localVariable;

- (id)init {
    self = [super init];
    if (self) {
        localVariable = true;
    }
    return self;
}

-(int) addThis: (int) firstNumber andThis: (int) secondNumber {
    return firstNumber + secondNumber;
}

@end
```

```obj-c
// Objective-C usage.

SampleClass *mySampleClass = [[SampleClass alloc] init];
mySampleClass.localVariable = false;
int result = [mySampleClass addThis:1 andThis:2];
```

Voyons la version C#. Vous remarquerez que, comme dans Swift, l’en-tête et l’implémentation ne figurent pas dans des fichiers distincts.

```csharp
// C# header and implementation.

using System;

namespace MyApp  // Defines this code' s scope.
{
    class SampleClass
    {
        private bool localVariable;

        public SampleClass() // Constructor.
        {
            localVariable = true;
        }

        public bool myLocalVariable // Property.
        {
            get
            {
                return localVariable;
            }
            set
            {
                localVariable = value; 
            }
        }

        public int AddTwoNumbers(int numberOne, int numberTwo)
        {
            return numberOne + numberTwo;
        }        
    }
}
```

```csharp
// C# usage.

SampleClass mySampleClass = new SampleClass();
mySampleClass.myLocalVariable = false;
int result = mySampleClass.AddTwoNumbers(1, 2);
```

C# est un langage facile à comprendre. Il est fourni avec les nombreuses classes et infrastructures de prise en charge de .NET. En un clin de œil, vous écrirez avec bonheur votre code sans le moindre crochet !

## <a name="next-step"></a>Étape suivante

[Bien démarrer : Savoir maîtriser Visual Studio](getting-started-getting-around-in-visual-studio.md)
