## <a name="create-a-device-identity"></a><span data-ttu-id="bafff-101">Skapa en enhetsidentitet</span><span class="sxs-lookup"><span data-stu-id="bafff-101">Create a device identity</span></span>

<span data-ttu-id="bafff-102">I det här avsnittet kan du använda verktyget Node.js [iothub explorer] [ iot-hub-explorer] toocreate en enhetsidentitet för den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="bafff-102">In this section, you use a Node.js tool called [iothub-explorer][iot-hub-explorer] toocreate a device identity for this tutorial.</span></span> <span data-ttu-id="bafff-103">Enhets-ID är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="bafff-103">Device IDs are case sensitive.</span></span>

1. <span data-ttu-id="bafff-104">Kör följande hello i kommandoradsverktyget miljön:</span><span class="sxs-lookup"><span data-stu-id="bafff-104">Run hello following in your command-line environment:</span></span>

    `npm install -g iothub-explorer@latest`

1. <span data-ttu-id="bafff-105">Kör sedan följande kommando toologin tooyour hubb hello.</span><span class="sxs-lookup"><span data-stu-id="bafff-105">Then, run hello following command toologin tooyour hub.</span></span> <span data-ttu-id="bafff-106">Ersätt `{iot hub connection string}` med hello IoT-hubb anslutningssträngen som du kopierade tidigare:</span><span class="sxs-lookup"><span data-stu-id="bafff-106">Substitute `{iot hub connection string}` with hello IoT Hub connection string you previously copied:</span></span>

    `iothub-explorer login "{iot hub connection string}"`

1. <span data-ttu-id="bafff-107">Skapa en ny enhetsidentitet som kallas slutligen `myDeviceId` med hello-kommando:</span><span class="sxs-lookup"><span data-stu-id="bafff-107">Finally, create a new device identity called `myDeviceId` with hello command:</span></span>

    `iothub-explorer create myDeviceId --connection-string`

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

<span data-ttu-id="bafff-108">Anteckna anslutningssträngen för hello enheten från hello resultat.</span><span class="sxs-lookup"><span data-stu-id="bafff-108">Make a note of hello device connection string from hello result.</span></span> <span data-ttu-id="bafff-109">Den här enheten anslutningssträngen används av hello enheten app tooconnect tooyour IoT-hubb som en enhet.</span><span class="sxs-lookup"><span data-stu-id="bafff-109">This device connection string is used by hello device app tooconnect tooyour IoT Hub as a device.</span></span>

![][img-identity]

<span data-ttu-id="bafff-110">Se för[komma igång med IoT-hubb] [ lnk-getstarted] tooprogrammatically skapa enheten identiteter.</span><span class="sxs-lookup"><span data-stu-id="bafff-110">Refer too[Getting started with IoT Hub][lnk-getstarted] tooprogrammatically create device identities.</span></span>

<!-- images and links -->
[img-identity]: media/iot-hub-get-started-create-device-identity/devidentity.png

[iot-hub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md

[lnk-getstarted]: ../articles/iot-hub/iot-hub-csharp-csharp-getstarted.md
