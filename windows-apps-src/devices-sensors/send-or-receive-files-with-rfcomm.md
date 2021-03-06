---
ms.assetid: 5B3A6326-15EE-4618-AA8C-F1C7FB5232FB
title: Bluetooth RFCOMM
description: Cet article fournit une vue d’ensemble de RFCOMM Bluetooth dans les applications de plateforme Windows universelle (UWP), ainsi qu’un exemple de code illustrant la façon d’envoyer ou de recevoir un fichier.
ms.date: 06/26/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: d95b7159c64c7796e55d4566d62630c076a707b1
ms.sourcegitcommit: 015291bdf2e7d67076c1c85fc025f49c840ba475
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85469554"
---
# <a name="bluetooth-rfcomm"></a>Bluetooth RFCOMM

**API importantes**

- [**Windows. Devices. Bluetooth**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth)
- [**Windows.Devices.Bluetooth.Rfcomm**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth.Rfcomm)

Cet article fournit une vue d’ensemble de RFCOMM Bluetooth dans les applications de plateforme Windows universelle (UWP), ainsi qu’un exemple de code illustrant la façon d’envoyer ou de recevoir un fichier.

> [!Important]
> Vous devez déclarer la fonctionnalité « Bluetooth » dans *Package. appxmanifest*.
>
> `<Capabilities> <DeviceCapability Name="bluetooth" /> </Capabilities>`

## <a name="overview"></a>Vue d’ensemble

Les API de l’espace de noms [**Windows. Devices. Bluetooth. RFCOMM**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth.Rfcomm) sont générées sur les modèles existants pour Windows. Devices, y compris l' [**énumération**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration) et l' [**instanciation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Portable.StorageDevice). La lecture et l’écriture de données sont conçues pour tirer parti des modèles et des objets de [**flux de données établis**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.DataReader) dans [**Windows. Storage. streams**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams). Les attributs de protocole SDP (Service Discovery Protocol) possèdent une valeur et un type attendu. Toutefois, pour certains appareils communs, les implémentations des attributs SDP sont défectueuses, car le type de la valeur ne correspond pas au type attendu. En outre, de nombreuses utilisations de RFCOMM ne nécessitent aucun attribut SDP supplémentaire. Cette API permet donc d’accéder aux données SDP non analysées, à partir desquelles les développeurs peuvent obtenir les informations dont ils ont besoin.

Les API RFCOMM utilisent le concept d’identificateurs de service. Bien qu’un identificateur de service soit simplement un GUID sur 128 bits, il est également souvent spécifié sous la forme d’un entier sur 16 ou 32 bits. L’API RFCOMM offre un wrapper pour les identificateurs de service grâce auquel ces derniers peuvent être spécifiés et utilisés en tant que GUID sur 128 bits et qu’entiers sur 32 bits ; cependant, elle n’offre pas d’entiers sur 16 bits. Cela ne constitue pas un problème pour l’API, car les langues effectueront automatiquement une migration vers un entier 32 bits qui ne perturbera pas la génération de l’identificateur.

L’application peut effectuer des opérations d’appareil en plusieurs étapes dans le cadre d’une tâche en arrière-plan, si bien qu’elle peut s’exécuter jusqu’au bout, même si elle est déplacée vers l’arrière-plan et suspendue. Cela permet d’effectuer des opérations de maintenance fiables pour les appareils telles que la modification de paramètres persistants ou de microprogrammes et la synchronisation du contenu, sans obliger l’utilisateur à observer une barre de progression pour patienter. Utilisez [**DeviceServicingTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.DeviceServicingTrigger) pour la maintenance des appareils et [**DeviceUseTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger) pour la synchronisation du contenu. Ces tâches en arrière-plan limitent la durée pendant laquelle l’application peut s’exécuter en arrière-plan et ne sont pas conçues pour autoriser une opération ou synchronisation illimitée.

Pour obtenir un exemple de code complet qui détaille l’opération RFCOMM, consultez l' [**exemple de conversation Bluetooth RFCOMM**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BluetoothRfcommChat) sur GitHub.  

## <a name="send-a-file-as-a-client"></a>Envoyer un fichier en tant que client

Lors de l’envoi d’un fichier, le scénario d’application de base consiste à se connecter à un périphérique couplé en fonction d’un service souhaité. Cela implique les étapes suivantes :

- Utilisez les fonctions **RfcommDeviceService. \* GetDeviceSelector** pour vous aider à générer une requête AQS qui peut être utilisée pour énumérer les instances d’appareil couplées du service souhaité.
- Choisissez un appareil énuméré, créez un [**RfcommDeviceService**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth.Rfcomm.RfcommDeviceService)et lisez les attributs SDP en fonction des besoins (à l’aide des [**assistances de données établies**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.DataReader) pour analyser les données de l’attribut).
- Créez un socket et utilisez les propriétés [**RfcommDeviceService. ConnectionHostName**](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.rfcomm.rfcommdeviceservice.connectionhostname) et [**RfcommDeviceService. ConnectionServiceName**](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.rfcomm.rfcommdeviceservice.connectionservicename) pour [**StreamSocket. ConnectAsync**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.streamsocket.connectasync) au service de périphérique distant avec les paramètres appropriés.
- Suivez les schémas de flux de données établis pour lire des blocs de données dans le fichier et les envoyer à l’appareil via le flux [**StreamSocket.OutputStream**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.streamsocket.outputstream) du socket.

```csharp
using System;
using System.Threading.Tasks;
using Windows.Devices.Bluetooth.Rfcomm;
using Windows.Networking.Sockets;
using Windows.Storage.Streams;
using Windows.Devices.Bluetooth;

Windows.Devices.Bluetooth.Rfcomm.RfcommDeviceService _service;
Windows.Networking.Sockets.StreamSocket _socket;

async void Initialize()
{
    // Enumerate devices with the object push service
    var services =
        await Windows.Devices.Enumeration.DeviceInformation.FindAllAsync(
            RfcommDeviceService.GetDeviceSelector(
                RfcommServiceId.ObexObjectPush));

    if (services.Count > 0)
    {
        // Initialize the target Bluetooth BR device
        var service = await RfcommDeviceService.FromIdAsync(services[0].Id);

        bool isCompatibleVersion = await IsCompatibleVersionAsync(service);

        // Check that the service meets this App's minimum requirement
        if (SupportsProtection(service) && isCompatibleVersion)
        {
            _service = service;

            // Create a socket and connect to the target
            _socket = new StreamSocket();
            await _socket.ConnectAsync(
                _service.ConnectionHostName,
                _service.ConnectionServiceName,
                SocketProtectionLevel
                    .BluetoothEncryptionAllowNullAuthentication);

            // The socket is connected. At this point the App can wait for
            // the user to take some action, for example, click a button to send a
            // file to the device, which could invoke the Picker and then
            // send the picked file. The transfer itself would use the
            // Sockets API and not the Rfcomm API, and so is omitted here for
            // brevity.
        }
    }
}

// This App requires a connection that is encrypted but does not care about
// whether it's authenticated.
bool SupportsProtection(RfcommDeviceService service)
{
    switch (service.ProtectionLevel)
    {
        case SocketProtectionLevel.PlainSocket:
            if ((service.MaxProtectionLevel == SocketProtectionLevel
                    .BluetoothEncryptionWithAuthentication)
                || (service.MaxProtectionLevel == SocketProtectionLevel
                    .BluetoothEncryptionAllowNullAuthentication))
            {
                // The connection can be upgraded when opening the socket so the
                // App may offer UI here to notify the user that Windows may
                // prompt for a PIN exchange.
                return true;
            }
            else
            {
                // The connection cannot be upgraded so an App may offer UI here
                // to explain why a connection won't be made.
                return false;
            }
        case SocketProtectionLevel.BluetoothEncryptionWithAuthentication:
            return true;
        case SocketProtectionLevel.BluetoothEncryptionAllowNullAuthentication:
            return true;
    }
    return false;
}

// This App relies on CRC32 checking available in version 2.0 of the service.
const uint SERVICE_VERSION_ATTRIBUTE_ID = 0x0300;
const byte SERVICE_VERSION_ATTRIBUTE_TYPE = 0x0A;   // UINT32
const uint MINIMUM_SERVICE_VERSION = 200;
async Task<bool> IsCompatibleVersionAsync(RfcommDeviceService service)
{
    var attributes = await service.GetSdpRawAttributesAsync(
        BluetoothCacheMode.Uncached);
    var attribute = attributes[SERVICE_VERSION_ATTRIBUTE_ID];
    var reader = DataReader.FromBuffer(attribute);

    // The first byte contains the attribute's type
    byte attributeType = reader.ReadByte();
    if (attributeType == SERVICE_VERSION_ATTRIBUTE_TYPE)
    {
        // The remainder is the data
        uint version = reader.ReadUInt32();
        return version >= MINIMUM_SERVICE_VERSION;
    }
    else return false;
}
```

```cppwinrt
...
#include <winrt/Windows.Devices.Bluetooth.Rfcomm.h>
#include <winrt/Windows.Devices.Enumeration.h>
#include <winrt/Windows.Networking.Sockets.h>
#include <winrt/Windows.Storage.Streams.h>
...
Windows::Devices::Bluetooth::Rfcomm::RfcommDeviceService m_service{ nullptr };
Windows::Networking::Sockets::StreamSocket m_socket;

Windows::Foundation::IAsyncAction Initialize()
{
    // Enumerate devices with the object push service.
    Windows::Devices::Enumeration::DeviceInformationCollection services{
        co_await Windows::Devices::Enumeration::DeviceInformation::FindAllAsync(
            Windows::Devices::Bluetooth::Rfcomm::RfcommDeviceService::GetDeviceSelector(
                Windows::Devices::Bluetooth::Rfcomm::RfcommServiceId::ObexObjectPush())) };

    if (services.Size() > 0)
    {
        // Initialize the target Bluetooth BR device.
        Windows::Devices::Bluetooth::Rfcomm::RfcommDeviceService service{
            co_await Windows::Devices::Bluetooth::Rfcomm::RfcommDeviceService::FromIdAsync(services.GetAt(0).Id()) };

        // Check that the service meets this App's minimum
        // requirement
        if (SupportsProtection(service)
            && co_await IsCompatibleVersion(service))
        {
            m_service = service;

            // Create a socket and connect to the target
            co_await m_socket.ConnectAsync(
                m_service.ConnectionHostName(),
                m_service.ConnectionServiceName(),
                Windows::Networking::Sockets::SocketProtectionLevel::BluetoothEncryptionAllowNullAuthentication);

            // The socket is connected. At this point the App can
            // wait for the user to take some action, for example, click
            // a button to send a file to the device, which could
            // invoke the Picker and then send the picked file.
            // The transfer itself would use the Sockets API and
            // not the Rfcomm API, and so is omitted here for
            //brevity.
        }
    }
}

// This App requires a connection that is encrypted but does not care about
// whether it's authenticated.
bool SupportsProtection(Windows::Devices::Bluetooth::Rfcomm::RfcommDeviceService const& service)
{
    switch (service.ProtectionLevel())
    {
    case Windows::Networking::Sockets::SocketProtectionLevel::PlainSocket:
        if ((service.MaxProtectionLevel() == Windows::Networking::Sockets::SocketProtectionLevel::BluetoothEncryptionWithAuthentication)
            || (service.MaxProtectionLevel() == Windows::Networking::Sockets::SocketProtectionLevel::BluetoothEncryptionAllowNullAuthentication))
        {
            // The connection can be upgraded when opening the socket so the
            // App may offer UI here to notify the user that Windows may
            // prompt for a PIN exchange.
            return true;
        }
        else
        {
            // The connection cannot be upgraded so an App may offer UI here
            // to explain why a connection won't be made.
            return false;
        }
    case Windows::Networking::Sockets::SocketProtectionLevel::BluetoothEncryptionWithAuthentication:
        return true;
    case Windows::Networking::Sockets::SocketProtectionLevel::BluetoothEncryptionAllowNullAuthentication:
        return true;
    }
    return false;
}

// This application relies on CRC32 checking available in version 2.0 of the service.
const uint32_t SERVICE_VERSION_ATTRIBUTE_ID{ 0x0300 };
const byte SERVICE_VERSION_ATTRIBUTE_TYPE{ 0x0A }; // UINT32.
const uint32_t MINIMUM_SERVICE_VERSION{ 200 };

Windows::Foundation::IAsyncOperation<bool> IsCompatibleVersion(Windows::Devices::Bluetooth::Rfcomm::RfcommDeviceService const& service)
{
    auto attributes{
        co_await service.GetSdpRawAttributesAsync(Windows::Devices::Bluetooth::BluetoothCacheMode::Uncached) };

    auto attribute{ attributes.Lookup(SERVICE_VERSION_ATTRIBUTE_ID) };
    auto reader{ Windows::Storage::Streams::DataReader::FromBuffer(attribute) };

    // The first byte contains the attribute's type.
    byte attributeType{ reader.ReadByte() };
    if (attributeType == SERVICE_VERSION_ATTRIBUTE_TYPE)
    {
        // The remainder is the data
        uint32_t version{ reader.ReadUInt32() };
        co_return (version >= MINIMUM_SERVICE_VERSION);
    }
}
...
```

```cpp
Windows::Devices::Bluetooth::Rfcomm::RfcommDeviceService^ _service;
Windows::Networking::Sockets::StreamSocket^ _socket;

void Initialize()
{
    // Enumerate devices with the object push service
    create_task(
        Windows::Devices::Enumeration::DeviceInformation::FindAllAsync(
            RfcommDeviceService::GetDeviceSelector(
                RfcommServiceId::ObexObjectPush)))
    .then([](DeviceInformationCollection^ services)
    {
        if (services->Size > 0)
        {
            // Initialize the target Bluetooth BR device
            create_task(RfcommDeviceService::FromIdAsync(services[0]->Id))
            .then([](RfcommDeviceService^ service)
            {
                // Check that the service meets this App's minimum
                // requirement
                if (SupportsProtection(service)
                    && IsCompatibleVersion(service))
                {
                    _service = service;

                    // Create a socket and connect to the target
                    _socket = ref new StreamSocket();
                    create_task(_socket->ConnectAsync(
                        _service->ConnectionHostName,
                        _service->ConnectionServiceName,
                        SocketProtectionLevel
                            ::BluetoothEncryptionAllowNullAuthentication)
                    .then([](void)
                    {
                        // The socket is connected. At this point the App can
                        // wait for the user to take some action, for example, click
                        // a button to send a file to the device, which could
                        // invoke the Picker and then send the picked file.
                        // The transfer itself would use the Sockets API and
                        // not the Rfcomm API, and so is omitted here for
                        //brevity.
                    });
                }
            });
        }
    });
}

// This App requires a connection that is encrypted but does not care about
// whether it's authenticated.
bool SupportsProtection(RfcommDeviceService^ service)
{
    switch (service->ProtectionLevel)
    {
    case SocketProtectionLevel->PlainSocket:
        if ((service->MaxProtectionLevel == SocketProtectionLevel
                ::BluetoothEncryptionWithAuthentication)
            || (service->MaxProtectionLevel == SocketProtectionLevel
                ::BluetoothEncryptionAllowNullAuthentication))
        {
            // The connection can be upgraded when opening the socket so the
            // App may offer UI here to notify the user that Windows may
            // prompt for a PIN exchange.
            return true;
        }
        else
        {
            // The connection cannot be upgraded so an App may offer UI here
            // to explain why a connection won't be made.
            return false;
        }
    case SocketProtectionLevel::BluetoothEncryptionWithAuthentication:
        return true;
    case SocketProtectionLevel::BluetoothEncryptionAllowNullAuthentication:
        return true;
    }
    return false;
}

// This App relies on CRC32 checking available in version 2.0 of the service.
const uint SERVICE_VERSION_ATTRIBUTE_ID = 0x0300;
const byte SERVICE_VERSION_ATTRIBUTE_TYPE = 0x0A;   // UINT32
const uint MINIMUM_SERVICE_VERSION = 200;
bool IsCompatibleVersion(RfcommDeviceService^ service)
{
    auto attributes = await service->GetSdpRawAttributesAsync(
        BluetoothCacheMode::Uncached);
    auto attribute = attributes[SERVICE_VERSION_ATTRIBUTE_ID];
    auto reader = DataReader.FromBuffer(attribute);

    // The first byte contains the attribute's type
    byte attributeType = reader->ReadByte();
    if (attributeType == SERVICE_VERSION_ATTRIBUTE_TYPE)
    {
        // The remainder is the data
        uint version = reader->ReadUInt32();
        return version >= MINIMUM_SERVICE_VERSION;
    }
}
```

## <a name="receive-file-as-a-server"></a>Recevoir un fichier en tant que serveur

Un autre scénario d’application RFCOMM courant consiste à héberger un service sur le PC et à l’exposer pour d’autres périphériques.

- Créez une classe [**RfcommServiceProvider**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth.Rfcomm.RfcommServiceProvider) pour annoncer le service souhaité.
- Définissez les attributs SDP en fonction des besoins (à l’aide des [**assistances de données établies**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.DataReader) pour générer les données de l’attribut) et commencez à publier les enregistrements SDP pour les autres appareils à récupérer.
- Pour vous connecter à un périphérique client, créez un écouteur du socket pour commencer à écouter les demandes de connexion entrantes.
- À la réception d’une connexion, stockez le socket connecté en vue d’un traitement ultérieur.
- Suivez les schémas de flux de données établis pour lire des blocs de données du flux d’entrée InputStream du socket et les enregistrer dans un fichier.

Pour rendre un service RFCOMM persistant en arrière-plan, utilisez [**RfcommConnectionTrigger**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.rfcommconnectiontrigger). La tâche en arrière-plan est déclenchée à la connexion au service. Le développeur reçoit un handle pour le socket dans la tâche en arrière-plan. L’exécution de la tâche en arrière-plan est longue, et cette dernière persiste tant que le socket est utilisé.    

```csharp
Windows.Devices.Bluetooth.Rfcomm.RfcommServiceProvider _provider;

async void Initialize()
{
    // Initialize the provider for the hosted RFCOMM service
    _provider =
        await Windows.Devices.Bluetooth.Rfcomm.RfcommServiceProvider.CreateAsync(
            RfcommServiceId.ObexObjectPush);

    // Create a listener for this service and start listening
    StreamSocketListener listener = new StreamSocketListener();
    listener.ConnectionReceived += OnConnectionReceivedAsync;
    await listener.BindServiceNameAsync(
        _provider.ServiceId.AsString(),
        SocketProtectionLevel
            .BluetoothEncryptionAllowNullAuthentication);

    // Set the SDP attributes and start advertising
    InitializeServiceSdpAttributes(_provider);
    _provider.StartAdvertising(listener);
}

const uint SERVICE_VERSION_ATTRIBUTE_ID = 0x0300;
const byte SERVICE_VERSION_ATTRIBUTE_TYPE = 0x0A;   // UINT32
const uint SERVICE_VERSION = 200;

void InitializeServiceSdpAttributes(RfcommServiceProvider provider)
{
    Windows.Storage.Streams.DataWriter writer = new Windows.Storage.Streams.DataWriter();

    // First write the attribute type
    writer.WriteByte(SERVICE_VERSION_ATTRIBUTE_TYPE);
    // Then write the data
    writer.WriteUInt32(MINIMUM_SERVICE_VERSION);

    IBuffer data = writer.DetachBuffer();
    provider.SdpRawAttributes.Add(SERVICE_VERSION_ATTRIBUTE_ID, data);
}

void OnConnectionReceivedAsync(
    StreamSocketListener listener,
    StreamSocketListenerConnectionReceivedEventArgs args)
{
    // Stop advertising/listening so that we're only serving one client
    _provider.StopAdvertising();
    listener.Dispose();
    _socket = args.Socket;

    // The client socket is connected. At this point the App can wait for
    // the user to take some action, for example, click a button to receive a file
    // from the device, which could invoke the Picker and then save the
    // received file to the picked location. The transfer itself would use
    // the Sockets API and not the Rfcomm API, and so is omitted here for
    // brevity.
}
```

```cppwinrt
...
#include <winrt/Windows.Devices.Bluetooth.Rfcomm.h>
#include <winrt/Windows.Networking.Sockets.h>
#include <winrt/Windows.Storage.Streams.h>
...
Windows::Devices::Bluetooth::Rfcomm::RfcommServiceProvider m_provider{ nullptr };
Windows::Networking::Sockets::StreamSocket m_socket;

Windows::Foundation::IAsyncAction Initialize()
{
    // Initialize the provider for the hosted RFCOMM service.
    auto provider{ co_await Windows::Devices::Bluetooth::Rfcomm::RfcommServiceProvider::CreateAsync(
        Windows::Devices::Bluetooth::Rfcomm::RfcommServiceId::ObexObjectPush()) };

    m_provider = provider;

    // Create a listener for this service and start listening.
    Windows::Networking::Sockets::StreamSocketListener listener;
    listener.ConnectionReceived({ this, &MainPage::OnConnectionReceived });

    co_await listener.BindServiceNameAsync(m_provider.ServiceId().AsString(),
        Windows::Networking::Sockets::SocketProtectionLevel::BluetoothEncryptionAllowNullAuthentication);

    // Set the SDP attributes and start advertising
    InitializeServiceSdpAttributes();
    m_provider.StartAdvertising(listener);
}

const uint32_t SERVICE_VERSION_ATTRIBUTE_ID{ 0x0300 };
const byte SERVICE_VERSION_ATTRIBUTE_TYPE{ 0x0A };   // UINT32.
const uint32_t SERVICE_VERSION{ 200 };

void InitializeServiceSdpAttributes()
{
    Windows::Storage::Streams::DataWriter writer;

    // First write the attribute type
    writer.WriteByte(SERVICE_VERSION_ATTRIBUTE_TYPE);
    // Then write the data
    writer.WriteUInt32(SERVICE_VERSION);

    auto data{ writer.DetachBuffer() };
    m_provider.SdpRawAttributes().Insert(SERVICE_VERSION_ATTRIBUTE_ID, data);
}

void OnConnectionReceived(
    Windows::Networking::Sockets::StreamSocketListener const& listener,
    Windows::Networking::Sockets::StreamSocketListenerConnectionReceivedEventArgs const& args)
{
    // Stop advertising/listening so that we're only serving one client
    m_provider.StopAdvertising();
    listener.Close();
    m_socket = args.Socket();

    // The client socket is connected. At this point the application can wait for
    // the user to take some action, for example, click a button to receive a
    // file from the device, which could invoke the Picker and then save
    // the received file to the picked location. The transfer itself
    // would use the Sockets API and not the Rfcomm API, and so is
    // omitted here for brevity.
}
...
```

```cpp
Windows::Devices::Bluetooth::Rfcomm::RfcommServiceProvider^ _provider;
Windows::Networking::Sockets::StreamSocket^ _socket;

void Initialize()
{
    // Initialize the provider for the hosted RFCOMM service
    create_task(Windows::Devices::Bluetooth.
        RfcommServiceProvider::CreateAsync(
            RfcommServiceId::ObexObjectPush))
    .then([](RfcommServiceProvider^ provider) -> task<void> {
        _provider = provider;

        // Create a listener for this service and start listening
        auto listener = ref new StreamSocketListener();
        listener->ConnectionReceived += ref new TypedEventHandler<
                StreamSocketListener^,
                StreamSocketListenerConnectionReceivedEventArgs^>
           (&OnConnectionReceived);
        return create_task(listener->BindServiceNameAsync(
            _provider->ServiceId->AsString(),
            SocketProtectionLevel
                ::BluetoothEncryptionAllowNullAuthentication));
    }).then([listener](void) {
        // Set the SDP attributes and start advertising
        InitializeServiceSdpAttributes(_provider);
        _provider->StartAdvertising(listener);
    });
}

const uint SERVICE_VERSION_ATTRIBUTE_ID = 0x0300;
const byte SERVICE_VERSION_ATTRIBUTE_TYPE = 0x0A;   // UINT32
const uint SERVICE_VERSION = 200;
void InitializeServiceSdpAttributes(RfcommServiceProvider^ provider)
{
    auto writer = ref new Windows::Storage::Streams::DataWriter();

    // First write the attribute type
    writer->WriteByte(SERVICE_VERSION_ATTRIBUTE_TYPE);
    // Then write the data
    writer->WriteUInt32(SERVICE_VERSION);

    auto data = writer->DetachBuffer();
    provider->SdpRawAttributes->Add(SERVICE_VERSION_ATTRIBUTE_ID, data);
}

void OnConnectionReceived(
    StreamSocketListener^ listener,
    StreamSocketListenerConnectionReceivedEventArgs^ args)
{
    // Stop advertising/listening so that we're only serving one client
    _provider->StopAdvertising();
    create_task(listener->Close())
    .then([args](void) {
        _socket = args->Socket;

        // The client socket is connected. At this point the App can wait for
        // the user to take some action, for example, click a button to receive a
        // file from the device, which could invoke the Picker and then save
        // the received file to the picked location. The transfer itself
        // would use the Sockets API and not the Rfcomm API, and so is
        // omitted here for brevity.
    });
}
```
