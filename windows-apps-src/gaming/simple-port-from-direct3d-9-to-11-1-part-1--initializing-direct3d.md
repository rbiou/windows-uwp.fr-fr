---
title: Initialiser Direct3D 11
description: Montre comment convertir du code d’initialisation Direct3D 9 en Direct3D 11, notamment comment obtenir des handles vers le périphérique Direct3D et le contexte de périphérique, et comment utiliser DXGI pour configurer une chaîne d’échange.
ms.assetid: 1bd5e8b7-fd9d-065c-9ff3-1a9b1c90da29
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jeux, direct3d 11, initialisation, portage, direct3d 9
ms.localizationpriority: medium
ms.openlocfilehash: c5a7f33ddbc6d70af5293b92165892c2098e452d
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368033"
---
# <a name="initialize-direct3d-11"></a>Initialiser Direct3D 11



**Résumé**

-   Partie 1 : Initialiser Direct3D 11
-   [Partie 2 : Convertir le framework de rendu](simple-port-from-direct3d-9-to-11-1-part-2--rendering.md)
-   [Partie 3 : La boucle du jeu de port](simple-port-from-direct3d-9-to-11-1-part-3--viewport-and-game-loop.md)


Montre comment convertir du code d’initialisation Direct3D 9 en Direct3D 11, notamment comment obtenir des handles vers le périphérique Direct3D et le contexte de périphérique, et comment utiliser DXGI pour configurer une chaîne d’échange. Partie 1 de la procédure pas à pas [Porter une application Direct3D 9 simple vers DirectX 11 et la plateforme Windows universelle (UWP)](walkthrough--simple-port-from-direct3d-9-to-11-1.md).

## <a name="initialize-the-direct3d-device"></a>Initialiser le périphérique Direct3D


Dans Direct3D 9, nous avons créé un handle vers le périphérique Direct3D en appelant la méthode [**IDirect3D9::CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3d9-createdevice). Nous avons commencé en obtenant un pointeur vers l’[**interface IDirect3D9**](https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3d9) et nous avons spécifié plusieurs paramètres pour contrôler la configuration du périphérique Direct3D et de la chaîne d’échange. Avant cela, nous avons appelé la fonction [**GetDeviceCaps**](https://docs.microsoft.com/windows/desktop/api/wingdi/nf-wingdi-getdevicecaps) pour vérifier que nous n’étions pas en train de demander au périphérique quelque chose qu’il ne pourrait pas faire.

Direct3D 9

```cpp
UINT32 AdapterOrdinal = 0;
D3DDEVTYPE DeviceType = D3DDEVTYPE_HAL;
D3DCAPS9 caps;
m_pD3D->GetDeviceCaps(AdapterOrdinal, DeviceType, &caps); // caps bits

D3DPRESENT_PARAMETERS params;
ZeroMemory(&params, sizeof(D3DPRESENT_PARAMETERS));

// Swap chain parameters:
params.hDeviceWindow = m_hWnd;
params.AutoDepthStencilFormat = D3DFMT_D24X8;
params.BackBufferFormat = D3DFMT_X8R8G8B8;
params.MultiSampleQuality = D3DMULTISAMPLE_NONE;
params.MultiSampleType = D3DMULTISAMPLE_NONE;
params.SwapEffect = D3DSWAPEFFECT_DISCARD;
params.Windowed = true;
params.PresentationInterval = 0;
params.BackBufferCount = 2;
params.BackBufferWidth = 0;
params.BackBufferHeight = 0;
params.EnableAutoDepthStencil = true;
params.Flags = 2;

m_pD3D->CreateDevice(
    0,
    D3DDEVTYPE_HAL,
    m_hWnd,
    64,
    &params,
    &m_pd3dDevice
    );
```

Dans Direct3D 11, le contexte de périphérique et l’infrastructure graphique est considérée comme étant distincte du périphérique lui-même. L’initialisation se divise en plusieurs étapes.

Pour commencer, nous créons le périphérique. Nous obtenons la liste des niveaux de fonctionnalité que le périphérique prend en charge : celle-ci nous informe de presque tout ce que nous devons savoir sur l’unité de traitement graphique (GPU). En outre, nous n’avons pas besoin de créer une interface juste pour accéder à Direct3D. Nous utilisons plutôt l’API principale [**D3D11CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice). Cela nous permet d’obtenir un handle vers le périphérique et le contexte immédiat de périphérique. Le contexte de périphérique sert à définir l’état du pipeline et à générer des commandes de rendu.

Après avoir créé le périphérique Direct3D 11 et le contexte, nous pouvons exploiter la fonctionnalité de pointeur COM pour obtenir la version la plus récente des interfaces, lesquelles incluent des fonctionnalités supplémentaires et sont toujours recommandées.

> **Remarque**    D3D\_fonctionnalité\_niveau\_9\_1 (ce qui correspond au modèle de nuanceur 2.0) est le niveau minimal votre jeu de Microsoft Store est requis pour prendre en charge. (Les packages de votre jeu ARM échouera certification si vous ne prend pas en charge 9\_1.) Si votre jeu inclut également un chemin d’accès de rendu pour les fonctionnalités du nuancier modèle 3, vous devez inclure D3D\_fonctionnalité\_niveau\_9\_3 dans le tableau.

 

Direct3D 11

```cpp
// This flag adds support for surfaces with a different color channel 
// ordering than the API default. It is required for compatibility with
// Direct2D.
UINT creationFlags = D3D11_CREATE_DEVICE_BGRA_SUPPORT;

#if defined(_DEBUG)
// If the project is in a debug build, enable debugging via SDK Layers.
creationFlags |= D3D11_CREATE_DEVICE_DEBUG;
#endif

// This example only uses feature level 9.1.
D3D_FEATURE_LEVEL featureLevels[] = 
{
    D3D_FEATURE_LEVEL_9_1
};

// Create the Direct3D 11 API device object and a corresponding context.
ComPtr<ID3D11Device> device;
ComPtr<ID3D11DeviceContext> context;
D3D11CreateDevice(
    nullptr, // Specify nullptr to use the default adapter.
    D3D_DRIVER_TYPE_HARDWARE,
    nullptr,
    creationFlags,
    featureLevels,
    ARRAYSIZE(featureLevels),
    D3D11_SDK_VERSION, // UWP apps must set this to D3D11_SDK_VERSION.
    &device, // Returns the Direct3D device created.
    nullptr,
    &context // Returns the device immediate context.
    );

// Store pointers to the Direct3D 11.2 API device and immediate context.
device.As(&m_d3dDevice);

context.As(&m_d3dContext);
```

## <a name="create-a-swap-chain"></a>Créer une chaîne d’échange


Direct3D 11 inclut une API de périphérique appelée DXGI (infrastructure DirectX Graphics). L’interface DXGI nous permet (par exemple) de contrôler la manière dont la chaîne d’échange est configurée et de configurer des périphériques partagés. À cette étape de l’initialisation de Direct3D, nous allons utiliser DXGI pour créer une chaîne d’échange. Puisque nous avons créé le périphérique, nous pouvons suivre une chaîne d’interface vers la carte DXGI.

Le périphérique Direct3D implémente une interface COM pour DXGI. Tout d’abord, nous avons besoin d’obtenir cette interface, puis de l’utiliser pour demander la carte DXGI hébergeant le périphérique. Ensuite, nous utilisons la carte DXGI pour créer une fabrique DXGI.

> **Remarque**    il s’agit des interfaces COM pour votre première réponse peut consister à utiliser [ **QueryInterface**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)). Vous devez utiliser des pointeurs intelligents [**Microsoft::WRL::ComPtr**](https://docs.microsoft.com/cpp/windows/comptr-class) à la place. Ensuite, il suffit d’appeler la méthode [**As()** ](https://docs.microsoft.com/previous-versions/br230426(v=vs.140)) en fournissant un pointeur COM vide du type d’interface correct.

 

**Direct3D 11**

```cpp
ComPtr<IDXGIDevice2> dxgiDevice;
m_d3dDevice.As(&dxgiDevice);

// Then, the adapter hosting the device;
ComPtr<IDXGIAdapter> dxgiAdapter;
dxgiDevice->GetAdapter(&dxgiAdapter);

// Then, the factory that created the adapter interface:
ComPtr<IDXGIFactory2> dxgiFactory;
dxgiAdapter->GetParent(
    __uuidof(IDXGIFactory2),
    &dxgiFactory
    );
```

Maintenant que nous avons la fabrique DXGI, nous pouvons l’utiliser pour créer la chaîne d’échange. Définissons à présent les paramètres de cette chaîne d’échange. Nous devons spécifier le format de surface ; Nous allons choisir [ **DXGI\_FORMAT\_B8G8R8A8\_UNORM** ](https://docs.microsoft.com/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format) , car il est compatible avec Direct2D. Nous désactivons l’échelle d’affichage, l’échantillonnage multiple et le rendu stéréo car ils ne sont pas utilisés dans cet exemple. Étant donné que l’exécution est directement effectuée dans un CoreWindow, nous pouvons laisser les valeurs 0 de la largeur et de la hauteur et obtenir automatiquement les valeurs de plein écran.

> **Remarque**    toujours ensemble la *SDKVersion* paramètre D3D11\_SDK\_VERSION pour les applications UWP.

 

**Direct3D 11**

```cpp
ComPtr<IDXGISwapChain1> swapChain;
dxgiFactory->CreateSwapChainForCoreWindow(
    m_d3dDevice.Get(),
    reinterpret_cast<IUnknown*>(window),
    &swapChainDesc,
    nullptr,
    &swapChain
    );
swapChain.As(&m_swapChain);
```

Pour garantir que nous ne rendu plus souvent que l’écran peut s’afficher en fait, nous définissons la latence de trame à 1 et utilisez [ **DXGI\_échange\_effet\_FLIP\_séquentiel** ](https://docs.microsoft.com/windows/desktop/api/dxgi/ne-dxgi-dxgi_swap_effect). Cela permet d’économiser de l’énergie et constitue une exigence de certification du Windows Store. Nous en saurons plus sur la présentation à l’écran dans la partie 2 de cette procédure pas à pas.

> **Remarque**    vous pouvez utiliser le multithreading (par exemple, [ **ThreadPool** ](https://docs.microsoft.com/uwp/api/Windows.System.Threading) des éléments de travail) pour continuer d’autres tâches pendant que le thread de rendu est bloqué.

 

**Direct3D 11**

```cpp
dxgiDevice->SetMaximumFrameLatency(1);
```

Nous pouvons maintenant configurer la mémoire tampon d’arrière-plan pour le rendu.

## <a name="configure-the-back-buffer-as-a-render-target"></a>Configurer la mémoire tampon d’arrière-plan en tant que cible de rendu


Nous devons d’abord obtenir un handle vers la mémoire tampon d’arrière-plan. (Notez que la mémoire tampon d’arrière-plan est détenue par la chaîne de permutation DXGI, tandis que dans DirectX 9, il est détenu par le périphérique Direct3D.) Puis nous indiquons le périphérique Direct3D pour l’utiliser comme la cible de rendu en créant une cible de rendu *vue* à l’aide de la mémoire tampon d’arrière-plan.

**Direct3D 11**

```cpp
ComPtr<ID3D11Texture2D> backBuffer;
m_swapChain->GetBuffer(
    0,
    __uuidof(ID3D11Texture2D),
    &backBuffer
    );

// Create a render target view on the back buffer.
m_d3dDevice->CreateRenderTargetView(
    backBuffer.Get(),
    nullptr,
    &m_renderTargetView
    );
```

À présent, le contexte de périphérique entre en jeu. Nous demandons à Direct3D d’utiliser la vue de cible de rendu que nous venons de créer à l’aide de l’interface du contexte de périphérique. Nous allons récupérer la largeur et la hauteur de la mémoire tampon d’arrière-plan afin de pouvoir cibler toute la fenêtre en tant que fenêtre d’affichage. Notez que la mémoire tampon d’arrière-plan est liée à la chaîne d’échange, alors si la taille de la fenêtre change (par exemple, quand l’utilisateur fait glisser la fenêtre de jeu vers un autre moniteur), la mémoire tampon d’arrière-plan a besoin d’être redimensionnée, ce qui requiert une reconfiguration.

**Direct3D 11**

```cpp
D3D11_TEXTURE2D_DESC backBufferDesc = {0};
backBuffer->GetDesc(&backBufferDesc);

CD3D11_VIEWPORT viewport(
    0.0f,
    0.0f,
    static_cast<float>(backBufferDesc.Width),
    static_cast<float>(backBufferDesc.Height)
    );

m_d3dContext->RSSetViewports(1, &viewport);
```

Maintenant que nous avons un handle de périphérique et une cible de rendu en plein écran, nous sommes prêts à charger et dessiner la géométrie. Continuer à [partie 2 : Rendu](simple-port-from-direct3d-9-to-11-1-part-2--rendering.md).

 

 




