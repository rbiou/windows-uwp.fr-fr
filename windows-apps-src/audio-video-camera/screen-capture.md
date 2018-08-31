---
author: eliotcowley
title: Capture d’écran
description: L’espace de noms Windows.Graphics.Capture fournit des API pour l’acquisition d’images à partir d’une fenêtre d’affichage ou d’application, afin de créer des flux vidéo ou des captures instantanées pour créer des expériences collaboratives et interactives.
ms.assetid: 349C959D-9C74-44E7-B5F6-EBDB5CA87B9F
ms.author: elcowle
ms.date: 5/21/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, capture d’écran
ms.localizationpriority: medium
ms.openlocfilehash: e407842711d1bfcac0a54fdf484a38d39bc2b237
ms.sourcegitcommit: f9690c33bb85f84466560efac6f23cca2daf5a02
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
ms.locfileid: "1912907"
---
# <a name="screen-capture"></a>Capture d'écran

À partir de Windows10, version1803, l’espace de noms [Windows.Graphics.Capture](https://docs.microsoft.com/uwp/api/windows.graphics.capture) fournit des API pour l’acquisition d’images à partir d’une fenêtre d’affichage ou d’application, afin de créer des flux vidéo ou des captures instantanées pour créer des expériences collaboratives et interactives.

Avec la capture d’écran, les développeurs appellent l’interface utilisateur sécurisée du système pour sélectionner la fenêtre d’affichage ou d’application à capturer, et une bordure de notification jaune est dessinée par le système autour de l’élément activement capturé. Dans le cas de plusieurs sessions de capture simultanée, une bordure jaune est dessinée autour de chaque élément capturé.

> [!NOTE]
> La capture d’écran API vous oblige à exécuter Windows10 Professionnel ou Entreprise.

## <a name="add-the-screen-capture-capability"></a>Ajouter la fonctionnalité de capture d’écran

Les API disponibles dans l’espace de noms **Windows.Graphics.Capture** nécessitent de déclarer une fonctionnalité générale dans le manifeste de votre application. Vous devez l’ajouter directement dans le fichier:
    
1. Cliquez avec le bouton droit sur **Package.appxmanifest** dans l’**Explorateur de solutions**. 
2. Sélectionnez **Ouvrir avec...** 
3. Choisissez **Éditeur (de texte) XML**. 
4. Sélectionnez **OK**.
5. Dans le nœud **Package**, ajoutez l’attribut suivant: `xmlns:uap6="http://schemas.microsoft.com/appx/manifest/uap/windows10/6"`
6. De même, dans le nœud **Package**, ajoutez l’élément suivant à l’attribut **IgnorableNamespaces**: `uap6`
7. Dans le nœud **Fonctionnalités**, ajoutez l’élément suivant: `<uap6:Capability Name="graphicsCapture"/>`

## <a name="launch-the-system-ui-to-start-screen-capture"></a>Lancer l’interface utilisateur du système pour démarrer la capture d’écran

Avant de lancer l’interface utilisateur du système, vous pouvez vérifier si votre application est actuellement en mesure d’effectuer des captures d’écran. Votre application n’est peut-être pas en mesure d’utiliser la fonction de capture d’écran pour plusieurs raisons, notamment si l’appareil ne répond pas à la configuration matérielle requise ou si l’application ciblée pour la capture bloque la capture d’écran. Utilisez la méthode **IsSupported** dans la classe [GraphicsCaptureSession](https://docs.microsoft.com/uwp/api/windows.graphics.capture.graphicscapturesession) pour déterminer si la capture d’écran UWP est prise en charge:

```cs
// This runs when the application starts.
public void OnInitialization()
{
    if (!GraphicsCaptureSession.IsSupported()) 
    {
        // Hide the capture UI if screen capture is not supported.
        CaptureButton.Visibility = Visibility.Collapsed; 
    }    
}
```

Une fois que vous avez vérifié que la capture d’écran est prise en charge, utilisez la classe [GraphicsCapturePicker](https://docs.microsoft.com/uwp/api/windows.graphics.capture.graphicscapturepicker) pour appeler l’interface utilisateur système du sélecteur. L’utilisateur final utilise cette interface utilisateur pour sélectionner la fenêtre d’affichage ou d’application à capturer. Le sélecteur retourne un objet [GraphicsCaptureItem](https://docs.microsoft.com/uwp/api/windows.graphics.capture.graphicscaptureitem) qui est utilisé pour créer un objet **GraphicsCaptureSession**:

```cs
public async Task StartCaptureAsync() 
{ 
    // The GraphicsCapturePicker follows the same pattern the 
    // file pickers do. 
    var picker = new GraphicsCapturePicker(); 
    GraphicsCaptureItem item = await picker.PickSingleItemAsync(); 

    // The item may be null if the user dismissed the 
    // control without making a selection or hit Cancel. 
    if (item != null) 
    {
        // We'll define this method later in the document.
        StartCaptureInternal(item); 
    } 
}
```

## <a name="create-a-capture-frame-pool-and-capture-session"></a>Créer un pool d’images de capture et une session de capture

À l’aide de **GraphicsCaptureItem**, vous allez créer un objet [Direct3D11CaptureFramePool](https://docs.microsoft.com/uwp/api/windows.graphics.capture.direct3d11captureframepool) avec le périphérique D3D, le format de pixel pris en charge (**DXGI\_FORMAT\_B8G8R8A8\_UNORM**), le nombre d’images souhaitées (qui peut être un entier) et la taille des images. La propriété **ContentSize** de la classe **GraphicsCaptureItem** peut être utilisée comme taille de votre image:

```cs
private GraphicsCaptureItem _item;
private Direct3D11CaptureFramePool _framePool;
private CanvasDevice _canvasDevice;
private GraphicsCaptureSession _session;

public void StartCaptureInternal(GraphicsCaptureItem item) 
{
    _item = item;

    _framePool = Direct3D11CaptureFramePool.Create( 
        _canvasDevice, // D3D device 
        DirectXPixelFormat.B8G8R8A8UIntNormalized, // Pixel format 
        2, // Number of frames 
        _item.Size); // Size of the buffers   
} 
```

Ensuite, obtenez une instance de la classe **GraphicsCaptureSession** pour votre **Direct3D11CaptureFramePool** en passant l’objet **GraphicsCaptureItem** à la méthode **CreateCaptureSession**:

```cs
_session = _framePool.CreateCaptureSession(_item);
```

Une fois que l’utilisateur a explicitement autorisé la capture d’une fenêtre d’application ou d’affichage dans l’interface utilisateur du système, l’objet **GraphicsCaptureItem** peut être associé à plusieurs objets **CaptureSession**. De cette façon, votre application peut choisir de capturer le même élément pour plusieurs expériences.

Pour capturer plusieurs éléments en même temps, votre application doit créer une session de capture pour chaque élément à capturer, ce qui nécessite d’appeler l’interface utilisateur du sélecteur pour chaque élément à capturer.

## <a name="acquire-capture-frames"></a>Acquérir des images de capture

Une fois votre pool d’images et votre session de capture créés, appelez la méthode **StartCapture** de votre instance **GraphicsCaptureSession** pour demander au système de commencer à envoyer des images de capture à votre application:

```cs
_session.StartCapture();
```

Pour acquérir ces images de capture, qui sont des objets [Direct3D11CaptureFrame](https://docs.microsoft.com/uwp/api/windows.graphics.capture.direct3d11captureframe), vous pouvez utiliser l’événement **Direct3D11CaptureFramePool.FrameArrived**:

```cs
_framePool.FrameArrived += (s, a) => 
{ 
    // The FrameArrived event fires for every frame on the thread that         
    // created the Direct3D11CaptureFramePool. This means we don't have to 
    // do a null-check here, as we know we're the only one  
    // dequeueing frames in our application.  

    // NOTE: Disposing the frame retires it and returns  
    // the buffer to the pool.
    using (var frame = _framePool.TryGetNextFrame()) 
    {
        // We'll define this method later in the document.
        ProcessFrame(frame); 
    }  
}; 
```

Il est recommandé de ne pas utiliser si possible le thread d’interface utilisateur pour **FrameArrived**, car cet événement est déclenché chaque fois qu’une nouvelle image est disponible, ce qui arrive souvent. Si vous choisissez d’écouter **FrameArrived** sur le thread d’interface utilisateur, prenez en compte la quantité de travail que vous effectuez chaque fois que l’événement est déclenché.

Sinon, vous pouvez extraire manuellement des images avec la méthode **Direct3D11CaptureFramePool.TryGetNextFrame** jusqu'à ce que vous obteniez toutes les images nécessaires.

L’objet **Direct3D11CaptureFrame** contient les propriétés **ContentSize**, **Surface** et **SystemRelativeTime**. L’objet **SystemRelativeTime** est la durée QPC ([QueryPerformanceCounter](https://msdn.microsoft.com/library/windows/desktop/ms644904)) qui peut être utilisée pour synchroniser d’autres éléments multimédias.

## <a name="processing-capture-frames"></a>Traitement des images de capture

Chaque image de l’objet **Direct3D11CaptureFramePool** est extraite lors de l’appel à **TryGetNextFrame** et archivée en fonction de la durée de vie de l’objet **Direct3D11CaptureFrame**. Pour les applications natives, il suffit de libérer l’objet **Direct3D11CaptureFrame** pour archiver l’image dans le pool d’images. Pour les applications gérées, il est recommandé d’utiliser la méthode **Direct3D11CaptureFrame.Dispose** (**Close** en C++). **Direct3D11CaptureFrame** implémente l’interface [IClosable](https://docs.microsoft.com/uwp/api/Windows.Foundation.IClosable), qui est projetée en tant que [IDisposable](https://msdn.microsoft.com/library/system.idisposable.aspx) pour les appelants en C#.

Les applications ne doivent pas enregistrer les références aux objets **Direct3D11CaptureFrame**, ni enregistrer les références à la surface Direct3D sous-jacente une fois que l’image a été archivée.

Lors du traitement d’une image, il est recommandé que les applications utilisent le verrouillage [ID3D11Multithread](https://msdn.microsoft.com/library/windows/desktop/mt644886) sur le périphérique associé à l’objet **Direct3D11CaptureFramePool**.

La surface Direct3D sous-jacente aura toujours la taille spécifiée lors de la création (ou de la recréation) de l’objet **Direct3D11CaptureFramePool**. Si le contenu est plus grand que l’image, il est ajusté à la taille de l’image. Si le contenu est plus petit que l’image, le reste de l’image contient des données non définies. Il est recommandé que les applications copient un sous-rectangle à l’aide de la propriété **ContentSize** pour cet objet **Direct3D11CaptureFrame** afin d’éviter l’affichage du contenu non défini.

## <a name="react-to-capture-item-resizing-or-device-lost"></a>Réagir au redimensionnement d’un élément de capture ou à une perte de périphérique

Pendant le processus de capture, les applications souhaitent peut-être modifier certains aspects de leur objet **Direct3D11CaptureFramePool**. Cela inclut l’ajout d’un nouveau périphérique Direct3D, la modification de la taille des mémoires tampons de trame ou même la modification du nombre de mémoires tampons dans le pool. Dans chacun de ces scénarios, la méthode **Recreate** de l’objet **Direct3D11CaptureFramePool** est l’outil recommandé.

Lorsque la méthode **Recreate** est appelée, toutes les images existantes sont ignorées. Cela permet d’éviter de distribuer des images dont les surfaces Direct3D sous-jacentes appartiennent à un appareil auquel l’application n’a peut-être plus accès. Pour cette raison, il est préférable de traiter toutes les images en attente avant d’appeler la méthode **Recreate**.

## <a name="putting-it-all-together"></a>Synthèse de tous les éléments

L’extrait de code suivant est un exemple de bout en bout de l’implémentation d’une capture d’écran dans une application UWP:

```cs
using Microsoft.Graphics.Canvas;
using System;
using System.Threading.Tasks;
using Windows.Graphics;
using Windows.Graphics.Capture;
using Windows.Graphics.DirectX;
using Windows.UI.Composition;

namespace CaptureSamples 
{
    class Sample
    {
        // Capture API objects.
        private SizeInt32 _lastSize; 
        private GraphicsCaptureItem _item; 
        private Direct3D11CaptureFramePool _framePool; 
        private GraphicsCaptureSession _session; 

        // Non-API related members.
        private CanvasDevice _canvasDevice; 
        private CompositionDrawingSurface _surface; 

        public async Task StartCaptureAsync() 
        { 
            // The GraphicsCapturePicker follows the same pattern the 
            // file pickers do. 
            var picker = new GraphicsCapturePicker(); 
            GraphicsCaptureItem item = await picker.PickSingleItemAsync(); 
 
            // The item may be null if the user dismissed the 
            // control without making a selection or hit Cancel. 
            if (item != null) 
            { 
                StartCaptureInternal(item); 
            }
        } 
 
        private void StartCaptureInternal(GraphicsCaptureItem item) 
        { 
             // Stop the previous capture if we had one.
            StopCapture(); 
 
            _item = item; 
            _lastSize = _item.Size; 
 
             _framePool = Direct3D11CaptureFramePool.Create( 
                _canvasDevice, // D3D device 
                DirectXPixelFormat.B8G8R8A8UIntNormalized, // Pixel format 
                2, // Number of frames 
                _item.Size); // Size of the buffers 
 
            _framePool.FrameArrived += (s, a) => 
            { 
                // The FrameArrived event is raised for every frame on the thread
                // that created the Direct3D11CaptureFramePool. This means we 
                // don't have to do a null-check here, as we know we're the only 
                // one dequeueing frames in our application.  
 
                // NOTE: Disposing the frame retires it and returns  
                // the buffer to the pool.
 
                using (var frame = _framePool.TryGetNextFrame()) 
                { 
                    ProcessFrame(frame); 
                }  
            }; 
 
            _item.Closed += (s, a) => 
            { 
                StopCapture(); 
            }; 
 
            _session = _framePool.CreateCaptureSession(_item); 
            _session.Start(); 
        } 
 
        public void StopCapture() 
        { 
            _session?.Dispose(); 
            _framePool?.Dispose(); 
            _item = null; 
            _session = null; 
            _framePool = null; 
        } 
 
        private void ProcessFrame(Direct3D11CaptureFrame frame) 
        { 
            // Resize and device-lost leverage the same function on the
            // Direct3D11CaptureFramePool. Refactoring it this way avoids 
            // throwing in the catch block below (device creation could always 
            // fail) along with ensuring that resize completes successfully and 
            // isn’t vulnerable to device-lost.   
            bool needsReset = false; 
            bool recreateDevice = false; 
 
            if ((frame.ContentSize.Width != _lastSize.Width) || 
                (frame.ContentSize.Height != _lastSize.Height)) 
            { 
                needsReset = true; 
                _lastSize = frame.ContentSize; 
            } 
            
            try 
            { 
                // Take the D3D11 surface and draw it into a  
                // Composition surface.
 
                // Convert our D3D11 surface into a Win2D object.
                var canvasBitmap = CanvasBitmap.CreateFromDirect3D11Surface( 
                    _canvasDevice, 
                    frame.Surface); 
 
                // Helper that handles the drawing for us, not shown. 
                FillSurfaceWithBitmap(_surface, canvasBitmap); 
            } 
            // This is the device-lost convention for Win2D.
            catch(Exception e) when (_canvasDevice.IsDeviceLost(e.HResult)) 
            { 
                // We lost our graphics device. Recreate it and reset 
                // our Direct3D11CaptureFramePool.  
                needsReset = true; 
                recreateDevice = true; 
            } 
 
            if (needsReset) 
            { 
                ResetFramePool(frame.ContentSize, recreateDevice); 
            }
        } 
 
        private void ResetFramePool(Vector2 size, bool recreateDevice) 
        { 
            do 
            { 
                try 
                { 
                    if (recreateDevice) 
                    { 
                        _canvasDevice = new CanvasDevice(); 
                    } 
 
                    _framePool.Recreate( 
                        _canvasDevice,  
                        DirectXPixelFormat.B8G8R8A8UIntNormalized,  
                        2, 
                        size); 
                } 
                // This is the device-lost convention for Win2D.
                catch(Exception e) when (_canvasDevice.IsDeviceLost(e.HResult)) 
                { 
                    _canvasDevice = null; 
                    recreateDevice = true; 
                } 
            } while (_canvasDevice == null); 
        } 
    } 
} 
```

## <a name="see-also"></a>Articles associés

* [Espace de noms Windows.Graphics.Capture](https://docs.microsoft.com/uwp/api/windows.graphics.capture)