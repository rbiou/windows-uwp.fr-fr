---
title: Porter les mémoires tampons et données de vertex
description: Lors de cette étape, vous allez définir les mémoires tampons de vertex qui contiendront vos maillages ainsi que les mémoires tampons d’index qui permettront aux nuanceurs de parcourir les vertex dans l’ordre indiqué.
ms.assetid: 9a8138a5-0797-8532-6c00-58b907197a25
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jeux , portage, tampons de sommets, données, direct3d
ms.localizationpriority: medium
ms.openlocfilehash: 8445339d442fb740e9e2aba5e9d1cb0388c746ef
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368243"
---
# <a name="port-the-vertex-buffers-and-data"></a>Porter les mémoires tampons et données de vertex




**API importantes**

-   [**ID3DDevice::CreateBuffer**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createbuffer)
-   [**ID3DDeviceContext::IASetVertexBuffers**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetvertexbuffers)
-   [**ID3D11DeviceContext::IASetIndexBuffer**](https://docs.microsoft.com/windows/desktop/api/d3d10/nf-d3d10-id3d10device-iasetindexbuffer)

Lors de cette étape, vous allez définir les mémoires tampons de vertex qui contiendront vos maillages ainsi que les mémoires tampons d’index qui permettront aux nuanceurs de parcourir les vertex dans l’ordre indiqué.

Commençons par examiner le modèle codé en dur correspondant au maillage de cube de notre exemple. Dans les deux représentations, les vertex sont organisés sous forme de liste de triangles (à la différence d’une bande de triangles ou de toute autre disposition de triangles plus appropriée). Tous les vertex de ces représentations sont également associés à des valeurs d’index et de couleur. Une grande partie du code Direct3D utilisé dans cette rubrique fait référence à des variables et des objets déjà définis dans le projet Direct3D.

Voici le cube qui sera traité par OpenGL ES 2.0. Dans l’exemple d’implémentation, chaque vertex est de 7 valeurs float : coordonnées de position suivies des valeurs de couleurs RVBA 4 3.

```cpp
#define CUBE_INDICES 36
#define CUBE_VERTICES 8

GLfloat cubeVertsAndColors[] = 
{
  -0.5f, -0.5f,  0.5f, 0.0f, 0.0f, 1.0f, 1.0f,
  -0.5f, -0.5f, -0.5f, 0.0f, 0.0f, 0.0f, 1.0f,
  -0.5f,  0.5f,  0.5f, 0.0f, 1.0f, 1.0f, 1.0f,
  -0.5f,  0.5f, -0.5f, 0.0f, 1.0f, 0.0f, 1.0f,
  0.5f, -0.5f,  0.5f, 1.0f, 0.0f, 1.0f, 1.0f,
  0.5f, -0.5f, -0.5f, 1.0f, 0.0f, 0.0f, 1.0f,  
  0.5f,  0.5f,  0.5f, 1.0f, 1.0f, 1.0f, 1.0f,
  0.5f,  0.5f, -0.5f, 1.0f, 1.0f, 0.0f, 1.0f
};

GLuint cubeIndices[] = 
{
  0, 1, 2, // -x
  1, 3, 2,

  4, 6, 5, // +x
  6, 7, 5,

  0, 5, 1, // -y
  5, 6, 1,

  2, 6, 3, // +y
  6, 7, 3,

  0, 4, 2, // +z
  4, 6, 2,

  1, 7, 3, // -z
  5, 7, 1
};
```

Et voici le même cube qui sera cette fois traité par Direct3D 11.

```cpp
VertexPositionColor cubeVerticesAndColors[] = 
// struct format is position, color
{
  {XMFLOAT3(-0.5f, -0.5f, -0.5f), XMFLOAT3(0.0f, 0.0f, 0.0f)},
  {XMFLOAT3(-0.5f, -0.5f,  0.5f), XMFLOAT3(0.0f, 0.0f, 1.0f)},
  {XMFLOAT3(-0.5f,  0.5f, -0.5f), XMFLOAT3(0.0f, 1.0f, 0.0f)},
  {XMFLOAT3(-0.5f,  0.5f,  0.5f), XMFLOAT3(0.0f, 1.0f, 1.0f)},
  {XMFLOAT3( 0.5f, -0.5f, -0.5f), XMFLOAT3(1.0f, 0.0f, 0.0f)},
  {XMFLOAT3( 0.5f, -0.5f,  0.5f), XMFLOAT3(1.0f, 0.0f, 1.0f)},
  {XMFLOAT3( 0.5f,  0.5f, -0.5f), XMFLOAT3(1.0f, 1.0f, 0.0f)},
  {XMFLOAT3( 0.5f,  0.5f,  0.5f), XMFLOAT3(1.0f, 1.0f, 1.0f)},
};
        
unsigned short cubeIndices[] = 
{
  0, 2, 1, // -x
  1, 2, 3,

  4, 5, 6, // +x
  5, 7, 6,

  0, 1, 5, // -y
  0, 5, 4,

  2, 6, 7, // +y
  2, 7, 3,

  0, 4, 6, // -z
  0, 6, 2,

  1, 3, 7, // +z
  1, 7, 5
};
```

Si vous analysez ce code plus en détail, vous remarquerez que le cube contenu dans le code OpenGL ES 2.0 est représenté dans un système de coordonnées orienté main droite, alors que le cube défini dans le code Direct3D est représenté dans un système de coordonnées orienté main gauche. Lorsque vous importez vos propres données de maillage, vous devez inverser les coordonnées de l’axe z de votre modèle, et modifier les index de chaque maillage de façon adéquate pour parcourir les triangles dans le nouvel ordre du système de coordonnées.

Supposons que nous venons de transposer le maillage du cube du système de coordonnées orienté main droite d’OpenGL ES 2.0 dans le système de coordonnées orienté main gauche de Direct3D. Voyons maintenant comment faire pour charger les données du cube en vue de leur traitement dans les deux modèles.

## <a name="instructions"></a>Instructions

### <a name="step-1-create-an-input-layout"></a>Étape 1 : Créer une disposition d’entrée

Dans OpenGL ES 2.0, vos données de vertex sont fournies sous forme d’attributs, qui seront ensuite transmis aux objets nuanceur pour lecture. En règle générale, vous fournissez à l’objet programme du nuanceur une chaîne contenant le nom d’attribut utilisé dans le code GLSL du nuanceur, puis vous obtenez en retour un emplacement mémoire à transmettre au nuanceur. Dans cet exemple, un objet mémoire tampon de vertex stocke une liste de structures de vertex personnalisées, qui ont été définies et mises en forme comme suit :

OpenGL ES 2.0 : Configurez les attributs qui contiennent les informations par vertex.

``` syntax
typedef struct 
{
  GLfloat pos[3];        
  GLfloat rgba[4];
} Vertex;
```

Dans OpenGL ES 2.0, les dispositions d’entrée sont implicites ; vous prenez un usage général GL\_élément\_tableau\_mémoire tampon et fournir le stride et décalage de telle sorte que le nuanceur de sommets peut interpréter les données après son téléchargement. Avant l’étape du rendu, vous indiquez au nuanceur quels attributs il doit mapper aux différentes parties de chaque bloc de données de vertex, avec **glVertexAttribPointer**.

Dans Direct3D, vous devez fournir la disposition d’entrée qui décrit la structure des données de vertex dans la mémoire tampon de vertex au moment où vous créez cette dernière (et pas avant de dessiner la géométrie). Pour cela, utilisez une disposition d’entrée qui correspond à la disposition des données de chaque vertex en mémoire. Les informations que vous indiquez ici doivent être précises et exactes. C’est très important !

Ici, vous créez une description d’entrée sous forme de tableau de [ **D3D11\_d’entrée\_élément\_DESC** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_input_element_desc) structures.

Direct3D : Définir une description de la disposition d’entrée.

``` syntax
struct VertexPositionColor
{
  DirectX::XMFLOAT3 pos;
  DirectX::XMFLOAT3 color;
};

// ...

const D3D11_INPUT_ELEMENT_DESC vertexDesc[] = 
{
  { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0,  D3D11_INPUT_PER_VERTEX_DATA, 0 },
  { "COLOR",    0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
};

```

Cette description de disposition d’entrée définit un vertex à l’aide d’une paire de vecteurs à trois coordonnées : le premier vecteur 3D indique la position du vertex dans le système de coordonnées du modèle et le second vecteur 3D contient la valeur de couleur RVB associée au vertex. Dans cet exemple, vous utilisez trois valeurs à virgule flottante 32 bits, représentées ainsi dans le code : `XMFLOAT3(X.Xf, X.Xf, X.Xf)`. Vous devez utiliser des types de la bibliothèque [DirectXMath](https://docs.microsoft.com/windows/desktop/dxmath/ovw-xnamath-reference)si vous gérez des données qui seront utilisées par un nuanceur car ils garantissent un packaging et un alignement corrects de ces données. (Par exemple, choisissez le type [**XMFLOAT3**](https://docs.microsoft.com/windows/desktop/api/directxmath/ns-directxmath-xmfloat3) ou [**XMFLOAT4**](https://docs.microsoft.com/windows/desktop/api/directxmath/ns-directxmath-xmfloat4) pour les données de vecteur et le type [**XMFLOAT4X4**](https://docs.microsoft.com/windows/desktop/api/directxmath/ns-directxmath-xmfloat4x4) pour les matrices.)

Pour obtenir la liste de tous les types de format possibles, reportez-vous à [ **DXGI\_FORMAT**](https://docs.microsoft.com/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format).

Créez l’objet disposition sur la base de la disposition d’entrée du vertex que vous venez de définir. Dans le code suivant, vous écrivez à **m\_inputLayout**, une variable de type **ComPtr** (qui pointe vers un objet de type [ **ID3D11InputLayout**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11inputlayout)). **fileData** contient l’objet nuanceur de vertex compilé à l’étape précédente, [Porter les nuanceurs](port-the-shader-config.md).

Direct3D : Créer la disposition d’entrée utilisée par la mémoire tampon vertex.

``` syntax
Microsoft::WRL::ComPtr<ID3D11InputLayout>      m_inputLayout;

// ...

m_d3dDevice->CreateInputLayout(
  vertexDesc,
  ARRAYSIZE(vertexDesc),
  fileData->Data,
  fileShaderData->Length,
  &m_inputLayout
);
```

Nous avons terminé la définition de la disposition d’entrée. À présent, créons une mémoire tampon qui utilise cette disposition et chargeons-la avec les données de maillage du cube.

### <a name="step-2-create-and-load-the-vertex-buffers"></a>Étape 2 : Créer et charger le tampon de vertex

Dans OpenGL ES 2.0, vous créez deux mémoires tampons : l’une pour les données de position, l’autre pour les valeurs de couleur. (Vous pouvez également créer un struct qui contient à la fois et une seule mémoire tampon.) Vous liez chaque mémoire tampon et écrivez des données de couleur et de position dans les. Plus tard, lors de l’exécution de votre fonction de rendu, vous devrez de nouveau lier les mémoires tampons et indiquer au nuanceur le format des données contenues dans chacune d’elles afin qu’il sache comment les interpréter.

OpenGL ES 2.0 : Lier les mémoires tampons de vertex

``` syntax
// upload the data for the vertex position buffer
glGenBuffers(1, &renderer->vertexBuffer);    
glBindBuffer(GL_ARRAY_BUFFER, renderer->vertexBuffer);
glBufferData(GL_ARRAY_BUFFER, sizeof(VERTEX) * CUBE_VERTICES, renderer->vertices, GL_STATIC_DRAW);   
```

Dans Direct3D, accessible via le nuanceur de mémoires tampons sont représentés en tant que [ **D3D11\_SUBRESOURCE\_données** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_subresource_data) structures. Pour lier l’emplacement de cette mémoire tampon à l’objet de nuanceur, vous devez créer un CD3D11\_tampon\_structure DESC pour chaque mémoire tampon avec [ **ID3DDevice::CreateBuffer**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createbuffer), puis définissez la mémoire tampon du contexte de périphérique Direct3D en appelant une méthode set spécifique au type de la mémoire tampon, comme [ **ID3DDeviceContext::IASetVertexBuffers**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetvertexbuffers).

Lorsque vous définissez la mémoire tampon, vous devez configurer le stride (la taille de l’élément de données pour un vertex) ainsi que le décalage (là où commence le tableau des données de vertex) à partir du début de la mémoire.

Notez que nous attribuons le pointeur vers le **vertexIndices** de tableau à la **pSysMem** champ la [ **D3D11\_SUBRESOURCE\_données** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_subresource_data) structure. Si cela n’est pas défini correctement, votre maillage sera endommagé ou vide.

Direct3D : Créer et définir la mémoire tampon vertex

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
        
// ...

UINT stride = sizeof(VertexPositionColor);
UINT offset = 0;
m_d3dContext->IASetVertexBuffers(
  0,
  1,
  m_vertexBuffer.GetAddressOf(),
  &stride,
  &offset);
```

### <a name="step-3-create-and-load-the-index-buffer"></a>Étape 3 : Créer et charger le tampon d’index

Les mémoires tampons d’index sont très utiles, car elles permettent au nuanceur de vertex de rechercher des vertex spécifiques. Nous avons intégré ces mémoires dans notre exemple de convertisseur, mais ce n’est pas une obligation. À l’instar des mémoires tampons de vertex dans OpenGL ES 2.0, vous créez et liez une mémoire tampon d’index en tant que mémoire tampon générale, puis vous y copiez les index de vertex que vous aviez précédemment créés.

Lorsque vous êtes prêt à dessiner, vous liez de nouveau les deux mémoires tampons (index et vertex) et vous appelez **glDrawElements**.

OpenGL ES 2.0 : Envoyer l’ordre d’index à l’appel de dessin.

``` syntax
GLuint indexBuffer;

// ...

glGenBuffers(1, &renderer->indexBuffer);    
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, renderer->indexBuffer);   
glBufferData(GL_ELEMENT_ARRAY_BUFFER, 
  sizeof(GLuint) * CUBE_INDICES, 
  renderer->vertexIndices, 
  GL_STATIC_DRAW);

// ...
// Drawing function

// Bind the index buffer
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, renderer->indexBuffer);
glDrawElements (GL_TRIANGLES, renderer->numIndices, GL_UNSIGNED_INT, 0);
```

Dans Direct3D, le processus est quasiment similaire, bien qu’un peu plus technique. Fournissez la mémoire tampon d’index sous forme de sous-ressource Direct3D à l’objet [**ID3D11DeviceContext**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext) que vous avez créé lors de la configuration de Direct3D. Pour cela, appelez [**ID3D11DeviceContext::IASetIndexBuffer**](https://docs.microsoft.com/windows/desktop/api/d3d10/nf-d3d10-id3d10device-iasetindexbuffer) avec la sous-ressource configurée pour le tableau d’index, comme ci-dessous. (Là encore, notez que vous affectez le pointeur vers le **cubeIndices** de tableau à la **pSysMem** champ la [ **D3D11\_SUBRESOURCE\_dedonnées** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_subresource_data) structure.)

Direct3D : Créer le tampon d’index.

``` syntax
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

// ...

m_d3dContext->IASetIndexBuffer(
  m_indexBuffer.Get(),
  DXGI_FORMAT_R16_UINT,
  0);
```

Vous allez ensuite dessiner les triangles en appelant [**ID3D11DeviceContext::DrawIndexed**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed) (ou [**ID3D11DeviceContext::Draw**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-draw) pour les vertex non indexés), comme expliqué ci-après. (Pour plus d’informations, voir l’étape suivante [Dessiner à l’écran](draw-to-the-screen.md).)

Direct3D : Dessiner les sommets indexées.

``` syntax
m_d3dContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
m_d3dContext->IASetInputLayout(m_inputLayout.Get());

// ...

m_d3dContext->DrawIndexed(
  m_indexCount,
  0,
  0);
```

## <a name="previous-step"></a>Étape précédente


[Les objets de nuanceur de port](port-the-shader-config.md)

## <a name="next-step"></a>Étape suivante

[Port du GLSL](port-the-glsl.md)

## <a name="remarks"></a>Notes

Lorsque vous définissez vos structures Direct3D, séparez le code qui appelle les méthodes sur [**ID3D11Device**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11device) du code de la méthode qui est appelée lorsque les ressources de périphérique doivent être recréées. Dans le modèle de projet Direct3D, ce code se trouve dans les méthodes **CreateDeviceResource** de l’objet convertisseur. Le code employé pour la mise à jour du contexte de périphérique ([**ID3D11DeviceContext**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)) est placé dans la méthode **Render**, car c’est l’endroit où vous élaborez les étapes du nuanceur et liez les données requises.

## <a name="related-topics"></a>Rubriques connexes


* [Comment : port un simple convertisseur OpenGL ES 2.0 vers Direct3D 11](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)
* [Les objets de nuanceur de port](port-the-shader-config.md)
* [Les mémoires tampons de vertex et les données de port](port-the-vertex-buffers-and-data-config.md)
* [Port du GLSL](port-the-glsl.md)

 

 




