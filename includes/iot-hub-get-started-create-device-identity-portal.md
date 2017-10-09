## <a name="create-a-device-identity"></a><span data-ttu-id="f09a8-101">Skapa en enhetsidentitet</span><span class="sxs-lookup"><span data-stu-id="f09a8-101">Create a device identity</span></span>

<span data-ttu-id="f09a8-102">I det här avsnittet kan du använda hello [Azure-portalen] [ lnk-azure-portal] toocreate en enhetsidentitet i hello identitetsregistret i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="f09a8-102">In this section, you use hello [Azure portal][lnk-azure-portal] toocreate a device identity in hello identity registry in your IoT hub.</span></span> <span data-ttu-id="f09a8-103">En enhet kan inte ansluta tooIoT hubb om den inte har en post i registret för hello identitet.</span><span class="sxs-lookup"><span data-stu-id="f09a8-103">A device cannot connect tooIoT hub unless it has an entry in hello identity registry.</span></span> <span data-ttu-id="f09a8-104">Mer information finns i avsnittet hello ”identitetsregistret” av hello [IoT-hubb Utvecklarhandbok][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="f09a8-104">For more information, see hello "Identity registry" section of hello [IoT Hub developer guide][lnk-devguide-identity].</span></span> <span data-ttu-id="f09a8-105">Hej **enheten Explorer** i hello portal hjälper dig att generera ett unikt enhets-ID och nyckel att enheten kan använda tooidentify själva när den ansluter tooIoT hubb.</span><span class="sxs-lookup"><span data-stu-id="f09a8-105">hello **Device Explorer** in hello portal helps you generate a unique device ID and key that your device can use tooidentify itself when it connects tooIoT Hub.</span></span> <span data-ttu-id="f09a8-106">Enhets-ID är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="f09a8-106">Device IDs are case sensitive.</span></span>

1. <span data-ttu-id="f09a8-107">Kontrollera att du är inloggad toohello [Azure-portalen][lnk-azure-portal].</span><span class="sxs-lookup"><span data-stu-id="f09a8-107">Make sure you are signed in toohello [Azure portal][lnk-azure-portal].</span></span>

1. <span data-ttu-id="f09a8-108">I hello Jumpbar klickar du på **alla resurser** och hitta din IoT-hubb-resurs.</span><span class="sxs-lookup"><span data-stu-id="f09a8-108">In hello Jumpbar, click **All resources** and find your IoT hub resource.</span></span>

    ![Navigera tooyour Iot-hubb][img-find-iothub]

1. <span data-ttu-id="f09a8-110">När din IoT-hubb resurs öppnas, klickar du på hello **enheten Explorer** verktyget och klicka sedan på **Lägg till** hello överst.</span><span class="sxs-lookup"><span data-stu-id="f09a8-110">When your IoT hub resource is opened, click hello **Device Explorer** tool, and then click **Add** at hello top.</span></span> <span data-ttu-id="f09a8-111">Ange hello namn för den nya enheten som **myDeviceId**, och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="f09a8-111">Provide hello name for your new device, such as **myDeviceId**, and click **Save**.</span></span>

    ![Skapa enhetsidentitet i portalen][img-create-device]

   <span data-ttu-id="f09a8-113">Detta skapar en ny enhetsidentitet för din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="f09a8-113">This creates a new device identity for your IoT hub.</span></span>

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

1. <span data-ttu-id="f09a8-114">I hello **enheten Explorer**'s listan över enheter klickar du på hello nyskapad enheten och anteckna hello **anslutningssträngen---primärnyckel**.</span><span class="sxs-lookup"><span data-stu-id="f09a8-114">In hello **Device Explorer**'s device list, click hello newly created device and make note of hello **Connection string---primary key**.</span></span> 

    ![Anslutningssträngen för enhet][img-connection-string]

> [!NOTE]
> <span data-ttu-id="f09a8-116">Hej IoT-hubb identitetsregistret lagrar bara enheten identiteter tooenable säker åtkomst toohello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="f09a8-116">hello IoT Hub identity registry only stores device identities tooenable secure access toohello IoT hub.</span></span> <span data-ttu-id="f09a8-117">Enheten ID och nycklar toouse lagras som säkerhetsreferenser och en aktiverat/inaktiverat flagga som du kan använda toodisable åtkomst för en enskild enhet.</span><span class="sxs-lookup"><span data-stu-id="f09a8-117">It stores device IDs and keys toouse as security credentials, and an enabled/disabled flag that you can use toodisable access for an individual device.</span></span> <span data-ttu-id="f09a8-118">Om ditt program måste toostore andra enhetsspecifika metadata, använder den en programspecifika butik.</span><span class="sxs-lookup"><span data-stu-id="f09a8-118">If your application needs toostore other device-specific metadata, it should use an application-specific store.</span></span> <span data-ttu-id="f09a8-119">Mer information finns i [utvecklarhandboken för IoT Hub][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="f09a8-119">For more information, see [IoT Hub developer guide][lnk-devguide-identity].</span></span>

<!-- Images. -->
[img-find-iothub]: ./media/iot-hub-get-started-create-device-identity-portal/find-iothub.png
[img-create-device]: ./media/iot-hub-get-started-create-device-identity-portal/create-identity-portal.png
[img-connection-string]: ./media/iot-hub-get-started-create-device-identity-portal/device-connection-string.png


<!-- Links -->
[lnk-azure-portal]: https://portal.azure.com
[lnk-devguide-identity]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md

