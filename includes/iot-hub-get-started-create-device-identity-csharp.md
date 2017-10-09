## <a name="create-a-device-identity"></a><span data-ttu-id="f7990-101">Skapa en enhetsidentitet</span><span class="sxs-lookup"><span data-stu-id="f7990-101">Create a device identity</span></span>
<span data-ttu-id="f7990-102">I det här avsnittet skapar du en .NET-konsolapp som skapar en enhetsidentitet i hello identitetsregistret i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="f7990-102">In this section, you create a .NET console app that creates a device identity in hello identity registry in your IoT hub.</span></span> <span data-ttu-id="f7990-103">En enhet kan inte ansluta tooIoT hubb om den inte har en post i registret för hello identitet.</span><span class="sxs-lookup"><span data-stu-id="f7990-103">A device cannot connect tooIoT hub unless it has an entry in hello identity registry.</span></span> <span data-ttu-id="f7990-104">Mer information finns i avsnittet hello ”identitetsregistret” av hello [IoT-hubb Utvecklarhandbok][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="f7990-104">For more information, see hello "Identity registry" section of hello [IoT Hub developer guide][lnk-devguide-identity].</span></span> <span data-ttu-id="f7990-105">När du kör den här konsolen appen genereras ett unikt enhets-ID och nyckel att enheten kan använda tooidentify själva när den skickar enhet till moln meddelanden tooIoT hubb.</span><span class="sxs-lookup"><span data-stu-id="f7990-105">When you run this console app, it generates a unique device ID and key that your device can use tooidentify itself when it sends device-to-cloud messages tooIoT Hub.</span></span> <span data-ttu-id="f7990-106">Enhets-ID är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="f7990-106">Device IDs are case sensitive.</span></span>

1. <span data-ttu-id="f7990-107">Lägga till en ny lösning för Visual C# klassiska skrivbordet projektet tooa i Visual Studio med hello **Konsolapp (.NET Framework)** projektmall.</span><span class="sxs-lookup"><span data-stu-id="f7990-107">In Visual Studio, add a Visual C# Windows Classic Desktop project tooa new solution by using hello **Console App (.NET Framework)** project template.</span></span> <span data-ttu-id="f7990-108">Se till att hello av .NET Framework 4.5.1 eller senare.</span><span class="sxs-lookup"><span data-stu-id="f7990-108">Make sure hello .NET Framework version is 4.5.1 or later.</span></span> <span data-ttu-id="f7990-109">Namnet hello projektet **CreateDeviceIdentity** och namnet hello lösningen **IoTHubGetStarted**.</span><span class="sxs-lookup"><span data-stu-id="f7990-109">Name hello project **CreateDeviceIdentity** and name hello solution **IoTHubGetStarted**.</span></span>
   
    ![Nytt Visual C# Windows Classic Desktop-projekt][10]
2. <span data-ttu-id="f7990-111">I Solution Explorer högerklickar du på hello **CreateDeviceIdentity** projektet och klicka sedan på **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="f7990-111">In Solution Explorer, right-click hello **CreateDeviceIdentity** project, and then click **Manage NuGet Packages**.</span></span>
3. <span data-ttu-id="f7990-112">I hello **NuGet Package Manager** väljer **Bläddra**, söka efter **microsoft.azure.devices**väljer **installera** tooinstall Hej **Microsoft.Azure.Devices** paketet och acceptera hello villkor för användning.</span><span class="sxs-lookup"><span data-stu-id="f7990-112">In hello **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** tooinstall hello **Microsoft.Azure.Devices** package, and accept hello terms of use.</span></span> <span data-ttu-id="f7990-113">Den här proceduren hämtar, installerar och lägger till en referens toohello [Azure IoT service SDK] [ lnk-nuget-service-sdk] NuGet-paketet och dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="f7990-113">This procedure downloads, installs, and adds a reference toohello [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>
   
    ![Fönstret för NuGet-pakethanteraren][11]
4. <span data-ttu-id="f7990-115">Lägg till följande hello `using` instruktioner överst hello i hello **Program.cs** fil:</span><span class="sxs-lookup"><span data-stu-id="f7990-115">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Common.Exceptions;
5. <span data-ttu-id="f7990-116">Lägg till följande fält toohello hello **programmet** klass.</span><span class="sxs-lookup"><span data-stu-id="f7990-116">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="f7990-117">Ersätt hello platshållarvärde med hello IoT-hubb anslutningssträngen för hello-hubb som du skapade i föregående avsnitt i hello.</span><span class="sxs-lookup"><span data-stu-id="f7990-117">Replace hello placeholder value with hello IoT Hub connection string for hello hub that you created in hello previous section.</span></span>
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
6. <span data-ttu-id="f7990-118">Lägg till följande metod toohello hello **programmet** klass:</span><span class="sxs-lookup"><span data-stu-id="f7990-118">Add hello following method toohello **Program** class:</span></span>
   
        private static async Task AddDeviceAsync()
        {
            string deviceId = "myFirstDevice";
            Device device;
            try
            {
                device = await registryManager.AddDeviceAsync(new Device(deviceId));
            }
            catch (DeviceAlreadyExistsException)
            {
                device = await registryManager.GetDeviceAsync(deviceId);
            }
            Console.WriteLine("Generated device key: {0}", device.Authentication.SymmetricKey.PrimaryKey);
        }
   
    <span data-ttu-id="f7990-119">Den här metoden skapar en enhetsidentitet med ID:t **myFirstDevice**.</span><span class="sxs-lookup"><span data-stu-id="f7990-119">This method creates a device identity with ID **myFirstDevice**.</span></span> <span data-ttu-id="f7990-120">(Om enhets-ID redan finns i hello identitetsregistret hello kod bara hämtar hello befintliga enhetsinformation.) hello app visar hello primärnyckel identitet.</span><span class="sxs-lookup"><span data-stu-id="f7990-120">(If that device ID already exists in hello identity registry, hello code simply retrieves hello existing device information.) hello app then displays hello primary key for that identity.</span></span> <span data-ttu-id="f7990-121">Du kan använda den här nyckeln i hello simulerade enheten app tooconnect tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="f7990-121">You use this key in hello simulated device app tooconnect tooyour IoT hub.</span></span>
[!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

7. <span data-ttu-id="f7990-122">Slutligen lägger du till följande rader toohello hello **Main** metoden:</span><span class="sxs-lookup"><span data-stu-id="f7990-122">Finally, add hello following lines toohello **Main** method:</span></span>
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddDeviceAsync().Wait();
        Console.ReadLine();
8. <span data-ttu-id="f7990-123">Kör det här programmet och anteckna hello enhetsnyckel.</span><span class="sxs-lookup"><span data-stu-id="f7990-123">Run this application, and make a note of hello device key.</span></span>
   
    ![Enhetsnyckel som genererats av hello program][12]

> [!NOTE]
> <span data-ttu-id="f7990-125">Hej IoT-hubb identitetsregistret lagrar bara enheten identiteter tooenable säker åtkomst toohello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="f7990-125">hello IoT Hub identity registry only stores device identities tooenable secure access toohello IoT hub.</span></span> <span data-ttu-id="f7990-126">Enheten ID och nycklar toouse lagras som säkerhetsreferenser och en aktiverat/inaktiverat flagga som du kan använda toodisable åtkomst för en enskild enhet.</span><span class="sxs-lookup"><span data-stu-id="f7990-126">It stores device IDs and keys toouse as security credentials, and an enabled/disabled flag that you can use toodisable access for an individual device.</span></span> <span data-ttu-id="f7990-127">Om ditt program måste toostore andra enhetsspecifika metadata, använder den en programspecifika butik.</span><span class="sxs-lookup"><span data-stu-id="f7990-127">If your application needs toostore other device-specific metadata, it should use an application-specific store.</span></span> <span data-ttu-id="f7990-128">Mer information finns i [utvecklarhandboken för IoT Hub][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="f7990-128">For more information, see [IoT Hub developer guide][lnk-devguide-identity].</span></span>
> 
> 

<!-- Images. -->
[10]: ./media/iot-hub-get-started-create-device-identity-csharp/create-identity-csharp1.png
[11]: ./media/iot-hub-get-started-create-device-identity-csharp/create-identity-csharp2.png
[12]: ./media/iot-hub-get-started-create-device-identity-csharp/create-identity-csharp3.png


<!-- Links -->
[lnk-devguide-identity]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
