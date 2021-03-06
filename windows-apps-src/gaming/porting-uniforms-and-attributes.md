---
title: Mémoires tampons, uniformes, sommets Port OpenGL ES 2.0 vers Direct3D
description: Au cours du processus de portage vers Direct3D 11 depuis OpenGL ES 2.0, vous devez modifier la syntaxe et le comportement de l’API pour passer des données entre l’application et les programmes de nuanceurs.
ms.assetid: 9b215874-6549-80c5-cc70-c97b571c74fe
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jeux, opengl, direct3d, tampons, uniformes, attributs de sommets
ms.localizationpriority: medium
ms.openlocfilehash: 9d79a4573438aec49d4aa1b828c90e72c04150de
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368155"
---
# <a name="compare-opengl-es-20-buffers-uniforms-and-vertex-attributes-to-direct3d"></a>Comparer les tampons, les uniformes et les attributs de vertex OpenGL ES 2.0 à Direct3D




**API importantes**

-   [**ID3D11Device1::CreateBuffer**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1)
-   [**ID3D11Device1::CreateInputLayout**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createinputlayout)
-   [**ID3D11DeviceContext1::IASetInputLayout**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetinputlayout)

Au cours du processus de portage vers Direct3D 11 depuis OpenGL ES 2.0, vous devez modifier la syntaxe et le comportement de l’API pour passer des données entre l’application et les programmes de nuanceurs.

Dans OpenGL ES 2.0, les données sont passées vers et depuis des programmes de nuanceurs de quatre manières : en tant qu’uniformes pour les données constantes, en tant qu’attributs pour les données de vertex, en tant qu’objets de tampons pour les autres données de ressources (telles que les textures). Dans Direct3D 11, celles-ci correspondent grosso modo à des tampons constants, des mémoires tampons de vertex et des sous-ressources. Malgré la standardisation superficielle, elles sont gérées de manière assez différente dans l’usage.

Voici les principales correspondances.

| OpenGL ES 2.0             | Direct3D 11                                                                                                                                                                         |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| uniforme                   | Champ de tampon constant (**cbuffer**).                                                                                                                                                |
| attribut                 | Champ d’élément de tampon de vertex, désigné par un schéma d’entrée et marqué à l’aide d’une sémantique HLSL spécifique.                                                                                |
| objet de tampon             | mémoire tampon ; Consultez [ **D3D11\_SUBRESOURCE\_données** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_subresource_data) et [ **D3D11\_tampon\_DESC** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_buffer_desc)et pour les définitions de mémoire tampon usage général. |
| objet de tampon de trame (FBO) | Cible(s) de rendu ; voir [**ID3D11RenderTargetView**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) avec [**ID3D11Texture2D**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d).                                       |
| mémoire tampon d’arrière-plan               | Chaîne de permutation avec surface de « mémoire tampon d’arrière-plan » ; voir [**IDXGISwapChain1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1) avec [**IDXGISurface1**](https://docs.microsoft.com/windows/desktop/api/dxgi/nn-dxgi-idxgisurface1) attaché.                       |

 

## <a name="port-buffers"></a>Mémoires tampons de port


Dans OpenGL ES 2.0, le processus de création et de liaison de tout type de tampon suit généralement ce modèle :

-   Appelez glGenBuffers pour générer un ou plusieurs tampons et leur renvoyer les handles.
-   Appelez glBindBuffer pour définir la disposition d’une mémoire tampon, tels que GL\_élément\_tableau\_mémoire tampon.
-   Appelez glBufferData pour renseigner le tampon à l’aide de données spécifiques (telles que les structures de vertex, les données d’index ou les données de couleurs) dans une disposition spécifique.

Le type le plus courant de mémoire tampon est la mémoire tampon de vertex, qui contient au minimum les positions des vertex dans un système de coordonnées. Dans le cadre d’une utilisation typique, un vertex est représenté par une structure qui contient les coordonnées de position, un vecteur normal vers la position du vertex, un vecteur tangent à la position du vertex et des coordonnées (uv) de recherche de texture. La mémoire tampon contient la liste contiguë de ces vertex, dans le même ordre (comme une liste de triangles, une bande ou un éventail), représentant collectivement les polygones visibles dans votre scène. (Dans Direct3D 11 et OpenGL ES 2.0, il n’est pas efficace d’avoir plusieurs mémoires tampons de vertex par appel de dessin.)

Voici un exemple de mémoire tampon de vertex et de tampon d’index créés avec OpenGL ES 2.0 :

OpenGL ES 2.0 : Création et remplissage d’une mémoire tampon vertex et un mémoire tampon d’index.

``` syntax
glGenBuffers(1, &renderer->vertexBuffer);
glBindBuffer(GL_ARRAY_BUFFER, renderer->vertexBuffer);
glBufferData(GL_ARRAY_BUFFER, sizeof(Vertex) * CUBE_VERTICES, renderer->vertices, GL_STATIC_DRAW);

glGenBuffers(1, &renderer->indexBuffer);
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, renderer->indexBuffer);
glBufferData(GL_ELEMENT_ARRAY_BUFFER, sizeof(int) * CUBE_INDICES, renderer->vertexIndices, GL_STATIC_DRAW);
```

D’autres tampons incluent les tampons de pixels et les pixmaps, comme les textures. Le pipeline nuanceur peut générer un rendu en tampons de texture (pixmaps) ou générer le rendu d’objets de tampons et utiliser ces tampons lors des futures passes du nuanceur. Dans le cas le plus simple, le flux des appels est le suivant :

-   Appelez glGenFramebuffers pour générer un objet de tampon de trame.
-   Appelez glBindFramebuffer pour lier l’objet de tampon de trame pour l’écriture.
-   Appelez glFramebufferTexture2D pour dessiner dans une carte de texture spécifiée.

Dans Direct3D 11, les éléments de données des tampons sont considérés comme des « sous-ressources » et peuvent varier des éléments de données de vertex individuels aux textures de carte MIP.

-   Remplir un [ **D3D11\_SUBRESOURCE\_données** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_subresource_data) structure avec la configuration d’un élément de données de mémoire tampon.
-   Remplir un [ **D3D11\_tampon\_DESC** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_buffer_desc) structure avec la taille des éléments individuels dans la mémoire tampon, ainsi que le type de tampon.
-   Appelez [**ID3D11Device1::CreateBuffer**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) avec ces deux structures.

Direct3D 11 : Création et remplissage d’une mémoire tampon vertex et un mémoire tampon d’index.

``` syntax
D3D11_SUBRESOURCE_DATA vertexBufferData = {0};
vertexBufferData.pSysMem = cubeVertices;
vertexBufferData.SysMemPitch = 0;
vertexBufferData.SysMemSlicePitch = 0;
CD3D11_BUFFER_DESC vertexBufferDesc(sizeof(cubeVertices), D3D11_BIND_VERTEX_BUFFER);

m_d3dDevice->CreateBuffer(
  &vertexBufferDesc,
  &vertexBufferData,
  &m_vertexBuffer);

m_indexCount = ARRAYSIZE(cubeIndices);

D3D11_SUBRESOURCE_DATA indexBufferData = {0};
indexBufferData.pSysMem = cubeIndices;
indexBufferData.SysMemPitch = 0;
indexBufferData.SysMemSlicePitch = 0;
CD3D11_BUFFER_DESC indexBufferDesc(sizeof(cubeIndices), D3D11_BIND_INDEX_BUFFER);

m_d3dDevice->CreateBuffer(
  &indexBufferDesc,
  &indexBufferData,
  &m_indexBuffer);
    
```

Il est possible de créer des tampons de pixels ou pixmaps, tels qu’un tampon de trame, sous forme d’objets [**ID3D11Texture2D**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d). Ceux-ci peuvent être liés en tant que ressources à un [**ID3D11RenderTargetView**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) ou [**ID3D11ShaderResourceView**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11shaderresourceview), lequel, une fois dessiné, peut s’afficher avec la chaîne de permutation associée ou être passé à un nuanceur, respectivement.

Direct3D 11 : Création d’un objet de mémoire tampon de trame.

``` syntax
ComPtr<ID3D11RenderTargetView> m_d3dRenderTargetViewWin;
// ...
ComPtr<ID3D11Texture2D> frameBuffer;

m_swapChainCoreWindow->GetBuffer(0, IID_PPV_ARGS(&frameBuffer));
m_d3dDevice->CreateRenderTargetView(
  frameBuffer.Get(),
  nullptr,
  &m_d3dRenderTargetViewWin);
```

## <a name="change-uniforms-and-uniform-buffer-objects-to-direct3d-constant-buffers"></a>Changer les uniformes et les objets de tampons uniformes en tampons constants Direct3D


Dans Open GL ES 2.0, les uniformes correspondent au mécanisme qui fournit des données constantes aux programmes de nuanceurs individuels. Ces données ne peuvent pas être modifiées par les nuanceurs.

Un uniforme généralement implique fournissant un de la glUniform\* méthodes avec l’emplacement de téléchargement dans le GPU, ainsi qu’un pointeur vers les données en mémoire de l’application. Après avoir Ile glUniform\* méthode s’exécute, les données uniformes soient dans la mémoire du GPU et accessible par les nuanceurs ont déclarés autrement uniformes. Vous devez veiller à ce que les données soient compressées de manière à ce que le nuanceur puisse les interpréter selon la déclaration uniforme incluse dans le nuanceur (à l’aide de types compatibles).

OpenGL ES 2.0 : création d’un uniforme et téléchargement de données dans cet uniforme

``` syntax
renderer->mvpLoc = glGetUniformLocation(renderer->programObject, "u_mvpMatrix");

// ...

glUniformMatrix4fv(renderer->mvpLoc, 1, GL_FALSE, (GLfloat*) &renderer->mvpMatrix.m[0][0]);
```

Dans le langage GLSL d’un nuanceur, la déclaration uniforme correspondante ressemble à ceci :

Open GL ES 2.0 : Déclaration uniforme GLSL

``` syntax
uniform mat4 u_mvpMatrix;
```

Direct3D désigne les données uniformes comme étant des « tampons constants », lesquels, à l’instar des uniformes, contiennent les données constantes fournies aux nuanceurs individuels. Comme avec les tampons uniformes, il est important de compresser les données de tampons constants dans la mémoire de la même manière que celle avec laquelle le nuanceur s’attend à les interpréter. À l’aide des types de DirectXMath (tel que [ **XMFLOAT4**](https://docs.microsoft.com/windows/desktop/api/directxmath/ns-directxmath-xmfloat4)) au lieu de types de plateformes (telles que **float\***  ou **float\[4\]** ) garantit l’alignement de l’élément de données.

Les tampons constants doivent avoir un registre de GPU associé utilisé pour référencer ces données sur le GPU. Les données sont compressées dans l’emplacement du registre comme indiqué par la disposition du tampon.

Direct3D 11 : Création d’un tampon constant et télécharger des données à celle-ci

``` syntax
struct ModelViewProjectionConstantBuffer
{
     DirectX::XMFLOAT4X4 mvp;
};

// ...

ModelViewProjectionConstantBuffer   m_constantBufferData;

// ...

XMStoreFloat4x4(&m_constantBufferData.mvp, mvpMatrix);

CD3D11_BUFFER_DESC constantBufferDesc(sizeof(ModelViewProjectionConstantBuffer), D3D11_BIND_CONSTANT_BUFFER);
m_d3dDevice->CreateBuffer(
  &constantBufferDesc,
  nullptr,
  &m_constantBuffer);
```

Dans le langage HLSL d’un nuanceur, la déclaration de tampon constant correspondante ressemble à ceci :

Direct3D 11 : Mémoire tampon constante déclaration de HLSL

``` syntax
cbuffer ModelViewProjectionConstantBuffer : register(b0)
{
  matrix mvp;
};
```

Notez qu’un registre doit être déclaré pour chaque tampon constant. Les différents niveaux de fonctionnalités Direct3D possèdent des registres disponibles maximaux différents, alors ne dépassez pas le nombre maximal pour le niveau de fonctionnalité le plus bas que vous ciblez.

## <a name="port-vertex-attributes-to-a-direct3d-input-layouts-and-hlsl-semantics"></a>Porter des attributs de vertex vers des schémas d’entrée Direct3D et une sémantique HLSL


Comme les données de vertex peuvent être modifiées par le pipeline nuanceur, OpenGL ES 2.0 requiert que vous les spécifiiez en tant qu’« attributs » plutôt qu’« uniformes ». (Cela a changé dans les versions ultérieures de OpenGL et GLSL.) Données de vertex spécifiques telle la position du sommet, normales, les tangentes et les valeurs de couleur sont fournis pour les nuanceurs comme valeurs d’attribut. Ces valeurs d’attributs correspondent à des décalages spécifiques pour chaque élément dans les données de vertex ; par exemple, le premier attribut peut pointer vers le composant de position d’un vertex individuel et le deuxième vers la normale, etc.

Le processus de base permettant de déplacer les données de mémoire tampon de vertex depuis la mémoire principale vers le GPU ressemble à ceci :

-   Chargez les données de vertex avec glBindBuffer.
-   Obtenez l’emplacement des attributs sur le GPU avec glGetAttribLocation. Appelez-le pour chaque attribut dans l’élément de données de vertex.
-   Appelez glVertexAttribPointer pour définir la taille et le décalage corrects de l’attribut à l’intérieur d’un élément de données de vertex individuel. Procédez ainsi pour chaque attribut.
-   Activez les informations de disposition des données de vertex avec glEnableVertexAttribArray.

OpenGL ES 2.0 : Chargement des données de mémoire tampon de vertex à l’attribut de nuanceur

``` syntax
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, renderer->vertexBuffer);
loc = glGetAttribLocation(renderer->programObject, "a_position");

glVertexAttribPointer(loc, 3, GL_FLOAT, GL_FALSE, 
  sizeof(Vertex), 0);
loc = glGetAttribLocation(renderer->programObject, "a_color");
glEnableVertexAttribArray(loc);

glVertexAttribPointer(loc, 4, GL_FLOAT, GL_FALSE, 
  sizeof(Vertex), (GLvoid*) (sizeof(float) * 3));
glEnableVertexAttribArray(loc);
```

Maintenant, dans votre nuanceur de vertex, vous déclarez les attributs à l’aide des mêmes noms que ceux que vous avez définis dans votre appel à glGetAttribLocation.

OpenGL ES 2.0 : Déclaration d’un attribut dans GLSL

``` syntax
attribute vec4 a_position;
attribute vec4 a_color;                     
```

D’une certaine manière, le même processus est valable pour Direct3D. Au lieu des attributs, les données de vertex sont fournies dans des tampons d’entrée, qui incluent des mémoires tampons de vertex et les tampons d’index correspondants. Toutefois, étant donné que Direct3D ne possède pas la déclaration des « attributs », vous devez spécifier un schéma d’entrée qui déclare le composant individuel des éléments de données dans la mémoire tampon de vertex et la sémantique HLSL qui indiquent où et comment ces composants doivent être interprétés par le nuanceur de vertex. La sémantique HLSL exige que vous définissiez l’usage de chaque composant avec une chaîne spécifique qui informe le moteur de nuanceur de son objectif. Par exemple, les données de position de vertex sont marquées comme POSITION, les données normales sont marquées comme NORMAL et les données de couleurs de vertex sont marquées comme COLOR. (Autres étapes du nuanceur nécessitent également la sémantique spécifique, et cette sémantique avoir des interprétations différentes en fonction de l’étape du nuanceur.) Pour plus d’informations sur la sémantique du langage HLSL, consultez [Port votre pipeline de nuanceur](change-your-shader-loading-code.md) et [HLSL sémantique](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dcl-usage---ps).

Collectivement, le processus de définition de la mémoire tampon de vertex et du tampon d’index et de définition du schéma d’entrée est appelé stade « d’assembly d’entrée » (stade IA) du pipeline graphique Direct3D.

Direct3D 11 : Configuration de l’étape de l’assembly d’entrée

``` syntax
// Set up the IA stage corresponding to the current draw operation.
UINT stride = sizeof(VertexPositionColor);
UINT offset = 0;
m_d3dContext->IASetVertexBuffers(
        0,
        1,
        m_vertexBuffer.GetAddressOf(),
        &stride,
        &offset);

m_d3dContext->IASetIndexBuffer(
        m_indexBuffer.Get(),
        DXGI_FORMAT_R16_UINT,
        0);

m_d3dContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
m_d3dContext->IASetInputLayout(m_inputLayout.Get());
```

Un schéma d’entrée est déclaré et associé avec un nuanceur de vertex en déclarant le format de l’élément de données de vertex et la sémantique utilisée pour chaque composant. La disposition des données vertex élément décrite dans la D3D11\_entrée\_élément\_DESC que vous créez doit correspondre à la disposition de la structure correspondante. Ici, vous créez un schéma pour les données de vertex qui comporte deux composants :

-   Une coordonnée de position de vertex, représentée dans la mémoire principale sous forme de XMFLOAT3, un tableau aligné de 3 valeurs à virgule flottante 32 bits pour les coordonnées (x, y, z).
-   Une valeur de couleur de vertex, représentée sous forme de XMFLOAT4, un tableau aligné de 4 valeurs à virgule flottante 32 bits pour la couleur (RVBA).

Vous attribuez une sémantique pour chacun, ainsi qu’un type de format. Vous passez ensuite la description à la méthode [**ID3D11Device1::CreateInputLayout**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createinputlayout). Le schéma d’entrée est utilisé quand nous appelons la méthode [**ID3D11DeviceContext1::IASetInputLayout**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetinputlayout) quand vous configurez l’assembly d’entrée pendant notre méthode de rendu.

Direct3D 11 : Décrivant une disposition d’entrée avec une sémantique spécifique

``` syntax
ComPtr<ID3D11InputLayout> m_inputLayout;

// ...

const D3D11_INPUT_ELEMENT_DESC vertexDesc[] = 
{
  { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0,  D3D11_INPUT_PER_VERTEX_DATA, 0 },
  { "COLOR",    0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
};

m_d3dDevice->CreateInputLayout(
  vertexDesc,
  ARRAYSIZE(vertexDesc),
  fileData->Data,
  fileData->Length,
  &m_inputLayout);

// ...
// When we start the drawing process...

m_d3dContext->IASetInputLayout(m_inputLayout.Get());
```

Enfin, vous vérifiez que le nuanceur peut comprendre les données d’entrée en déclarant l’entrée. La sémantique que vous avez attribuée dans le schéma est utilisée pour sélectionner les emplacements corrects dans la mémoire GPU.

Direct3D 11 : Déclaration des données d’entrée avec la sémantique de HLSL du nuanceur

``` syntax
struct VertexShaderInput
{
  float3 pos : POSITION;
  float3 color : COLOR;
};
```

 

 




