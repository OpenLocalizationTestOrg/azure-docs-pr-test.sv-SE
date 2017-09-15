## <a name="create-a-device-identity"></a><span data-ttu-id="03e39-101">Skapa en enhetsidentitet</span><span class="sxs-lookup"><span data-stu-id="03e39-101">Create a device identity</span></span>

<span data-ttu-id="03e39-102">I det här avsnittet kan du använda den [Azure-portalen] [ lnk-azure-portal] att skapa en enhetsidentitet i identitetsregistret i IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="03e39-102">In this section, you use the [Azure portal][lnk-azure-portal] to create a device identity in the identity registry in your IoT hub.</span></span> <span data-ttu-id="03e39-103">En enhet kan inte ansluta till IoT Hub om den inte har en post i identitetsregistret.</span><span class="sxs-lookup"><span data-stu-id="03e39-103">A device cannot connect to IoT hub unless it has an entry in the identity registry.</span></span> <span data-ttu-id="03e39-104">Mer information finns i avsnittet om identitetsregistret i [utvecklarhandboken för IoT Hub][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="03e39-104">For more information, see the "Identity registry" section of the [IoT Hub developer guide][lnk-devguide-identity].</span></span> <span data-ttu-id="03e39-105">Den **enheten Explorer** i portalen kan du generera ett unikt enhets-ID och nyckel som enheten kan använda för att identifiera sig själv när den ansluter till IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="03e39-105">The **Device Explorer** in the portal helps you generate a unique device ID and key that your device can use to identify itself when it connects to IoT Hub.</span></span> <span data-ttu-id="03e39-106">Enhets-ID är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="03e39-106">Device IDs are case sensitive.</span></span>

1. <span data-ttu-id="03e39-107">Kontrollera att du är inloggad på den [Azure-portalen][lnk-azure-portal].</span><span class="sxs-lookup"><span data-stu-id="03e39-107">Make sure you are signed in to the [Azure portal][lnk-azure-portal].</span></span>

1. <span data-ttu-id="03e39-108">I Jumpbar klickar du på **alla resurser** och hitta din IoT-hubb-resurs.</span><span class="sxs-lookup"><span data-stu-id="03e39-108">In the Jumpbar, click **All resources** and find your IoT hub resource.</span></span>

    ![Navigera till din Iot-hubb][img-find-iothub]

1. <span data-ttu-id="03e39-110">När din IoT-hubb resurs öppnas, klickar du på den **enheten Explorer** verktyget och klicka sedan på **Lägg till** längst upp.</span><span class="sxs-lookup"><span data-stu-id="03e39-110">When your IoT hub resource is opened, click the **Device Explorer** tool, and then click **Add** at the top.</span></span> <span data-ttu-id="03e39-111">Ange namn för den nya enheten som **myDeviceId**, och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="03e39-111">Provide the name for your new device, such as **myDeviceId**, and click **Save**.</span></span>

    ![Skapa enhetsidentitet i portalen][img-create-device]

   <span data-ttu-id="03e39-113">Detta skapar en ny enhetsidentitet för din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="03e39-113">This creates a new device identity for your IoT hub.</span></span>

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

1. <span data-ttu-id="03e39-114">I den **enheten Explorer**'s listan över enheter klickar du på den nyligen skapade enheten och anteckna den **anslutningssträngen---primärnyckel**.</span><span class="sxs-lookup"><span data-stu-id="03e39-114">In the **Device Explorer**'s device list, click the newly created device and make note of the **Connection string---primary key**.</span></span> 

    ![Anslutningssträngen för enhet][img-connection-string]

> [!NOTE]
> <span data-ttu-id="03e39-116">IoT Hub-identitetsregistret lagrar bara enhetsidentiteter för att skydda åtkomsten till IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="03e39-116">The IoT Hub identity registry only stores device identities to enable secure access to the IoT hub.</span></span> <span data-ttu-id="03e39-117">Registret lagrar enhets-ID:n och enhetsnycklar som ska användas som säkerhetsreferenser och en aktiverad/inaktiverad-flagga som du kan använda för att inaktivera åtkomst för en enskild enhet.</span><span class="sxs-lookup"><span data-stu-id="03e39-117">It stores device IDs and keys to use as security credentials, and an enabled/disabled flag that you can use to disable access for an individual device.</span></span> <span data-ttu-id="03e39-118">Om ditt program behöver lagra andra enhetsspecifika metadata bör det använda ett programspecifikt datalager.</span><span class="sxs-lookup"><span data-stu-id="03e39-118">If your application needs to store other device-specific metadata, it should use an application-specific store.</span></span> <span data-ttu-id="03e39-119">Mer information finns i [utvecklarhandboken för IoT Hub][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="03e39-119">For more information, see [IoT Hub developer guide][lnk-devguide-identity].</span></span>

<!-- Images. -->
[img-find-iothub]: ./media/iot-hub-get-started-create-device-identity-portal/find-iothub.png
[img-create-device]: ./media/iot-hub-get-started-create-device-identity-portal/create-identity-portal.png
[img-connection-string]: ./media/iot-hub-get-started-create-device-identity-portal/device-connection-string.png


<!-- Links -->
[lnk-azure-portal]: https://portal.azure.com
[lnk-devguide-identity]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md

