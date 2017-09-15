## <a name="create-a-device-identity"></a><span data-ttu-id="36cbc-101">Skapa en enhetsidentitet</span><span class="sxs-lookup"><span data-stu-id="36cbc-101">Create a device identity</span></span>
<span data-ttu-id="36cbc-102">I det här avsnittet ska du skapa en .NET-konsolapp som skapar en enhetsidentitet i identitetsregistret i IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="36cbc-102">In this section, you create a .NET console app that creates a device identity in the identity registry in your IoT hub.</span></span> <span data-ttu-id="36cbc-103">En enhet kan inte ansluta till IoT Hub om den inte har en post i identitetsregistret.</span><span class="sxs-lookup"><span data-stu-id="36cbc-103">A device cannot connect to IoT hub unless it has an entry in the identity registry.</span></span> <span data-ttu-id="36cbc-104">Mer information finns i avsnittet om identitetsregistret i [utvecklarhandboken för IoT Hub][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="36cbc-104">For more information, see the "Identity registry" section of the [IoT Hub developer guide][lnk-devguide-identity].</span></span> <span data-ttu-id="36cbc-105">När du kör den här konsolappen genererar det ett unikt enhets-ID och en nyckel som din enhet kan använda för att identifiera sig själv när den skickar ”enhet-till-molnet”-meddelanden till IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="36cbc-105">When you run this console app, it generates a unique device ID and key that your device can use to identify itself when it sends device-to-cloud messages to IoT Hub.</span></span> <span data-ttu-id="36cbc-106">Enhets-ID är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="36cbc-106">Device IDs are case sensitive.</span></span>

1. <span data-ttu-id="36cbc-107">I Visual Studio lägger du till ett Visual C# Classic Desktop-projekt i den nya lösningen med hjälp av projektmallen **Konsolapp (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="36cbc-107">In Visual Studio, add a Visual C# Windows Classic Desktop project to a new solution by using the **Console App (.NET Framework)** project template.</span></span> <span data-ttu-id="36cbc-108">Kontrollera att .NET Framework-versionen är 4.5.1 eller senare.</span><span class="sxs-lookup"><span data-stu-id="36cbc-108">Make sure the .NET Framework version is 4.5.1 or later.</span></span> <span data-ttu-id="36cbc-109">Ge projektet namnet **CreateDeviceIdentity** och lösningen namnet **IoTHubGetStarted**.</span><span class="sxs-lookup"><span data-stu-id="36cbc-109">Name the project **CreateDeviceIdentity** and name the solution **IoTHubGetStarted**.</span></span>
   
    ![Nytt Visual C# Windows Classic Desktop-projekt][10]
2. <span data-ttu-id="36cbc-111">Högerklicka i Solution Explorer på projektet **CreateDeviceIdentity** och klicka sedan på **Hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="36cbc-111">In Solution Explorer, right-click the **CreateDeviceIdentity** project, and then click **Manage NuGet Packages**.</span></span>
3. <span data-ttu-id="36cbc-112">Välj **Bläddra** i fönstret för **NuGet-pakethanteraren**, leta upp **microsoft.azure.devices**, välj **Installera** för att installera **Microsoft.Azure.Devices**-paketet och godkänn användningsvillkoren.</span><span class="sxs-lookup"><span data-stu-id="36cbc-112">In the **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** to install the **Microsoft.Azure.Devices** package, and accept the terms of use.</span></span> <span data-ttu-id="36cbc-113">Denna procedur hämtar, installerar och lägger till en referens för [NuGet-paketetet SDK för Azure IoT-tjänster][lnk-nuget-service-sdk] och dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="36cbc-113">This procedure downloads, installs, and adds a reference to the [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>
   
    ![Fönstret för NuGet-pakethanteraren][11]
4. <span data-ttu-id="36cbc-115">Lägg till följande `using`-uttryck överst i **Program.cs**-filen:</span><span class="sxs-lookup"><span data-stu-id="36cbc-115">Add the following `using` statements at the top of the **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Common.Exceptions;
5. <span data-ttu-id="36cbc-116">Lägg till följande fält i klassen **Program**.</span><span class="sxs-lookup"><span data-stu-id="36cbc-116">Add the following fields to the **Program** class.</span></span> <span data-ttu-id="36cbc-117">Ersätt platshållarvärdet med IoT Hub-anslutningssträngen som du skapade i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="36cbc-117">Replace the placeholder value with the IoT Hub connection string for the hub that you created in the previous section.</span></span>
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
6. <span data-ttu-id="36cbc-118">Lägg till följande metod i klassen **Program**:</span><span class="sxs-lookup"><span data-stu-id="36cbc-118">Add the following method to the **Program** class:</span></span>
   
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
   
    <span data-ttu-id="36cbc-119">Den här metoden skapar en enhetsidentitet med ID:t **myFirstDevice**.</span><span class="sxs-lookup"><span data-stu-id="36cbc-119">This method creates a device identity with ID **myFirstDevice**.</span></span> <span data-ttu-id="36cbc-120">(Om enhets-ID:t redan finns i identitetsregistret hämtar koden bara den befintliga enhetsinformationen.) Appen visar sedan den primära nyckeln för den identiteten.</span><span class="sxs-lookup"><span data-stu-id="36cbc-120">(If that device ID already exists in the identity registry, the code simply retrieves the existing device information.) The app then displays the primary key for that identity.</span></span> <span data-ttu-id="36cbc-121">Du använder den här nyckeln i den simulerade enhetsappen för att ansluta till din IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="36cbc-121">You use this key in the simulated device app to connect to your IoT hub.</span></span>
[!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

7. <span data-ttu-id="36cbc-122">Slutligen lägger du till följande rader till **Main**-metoden:</span><span class="sxs-lookup"><span data-stu-id="36cbc-122">Finally, add the following lines to the **Main** method:</span></span>
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddDeviceAsync().Wait();
        Console.ReadLine();
8. <span data-ttu-id="36cbc-123">Kör det här programmet och notera enhetsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="36cbc-123">Run this application, and make a note of the device key.</span></span>
   
    ![Enhetsnyckel som genereras av programmet][12]

> [!NOTE]
> <span data-ttu-id="36cbc-125">IoT Hub-identitetsregistret lagrar bara enhetsidentiteter för att skydda åtkomsten till IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="36cbc-125">The IoT Hub identity registry only stores device identities to enable secure access to the IoT hub.</span></span> <span data-ttu-id="36cbc-126">Registret lagrar enhets-ID:n och enhetsnycklar som ska användas som säkerhetsreferenser och en aktiverad/inaktiverad-flagga som du kan använda för att inaktivera åtkomst för en enskild enhet.</span><span class="sxs-lookup"><span data-stu-id="36cbc-126">It stores device IDs and keys to use as security credentials, and an enabled/disabled flag that you can use to disable access for an individual device.</span></span> <span data-ttu-id="36cbc-127">Om ditt program behöver lagra andra enhetsspecifika metadata bör det använda ett programspecifikt datalager.</span><span class="sxs-lookup"><span data-stu-id="36cbc-127">If your application needs to store other device-specific metadata, it should use an application-specific store.</span></span> <span data-ttu-id="36cbc-128">Mer information finns i [utvecklarhandboken för IoT Hub][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="36cbc-128">For more information, see [IoT Hub developer guide][lnk-devguide-identity].</span></span>
> 
> 

<!-- Images. -->
[10]: ./media/iot-hub-get-started-create-device-identity-csharp/create-identity-csharp1.png
[11]: ./media/iot-hub-get-started-create-device-identity-csharp/create-identity-csharp2.png
[12]: ./media/iot-hub-get-started-create-device-identity-csharp/create-identity-csharp3.png


<!-- Links -->
[lnk-devguide-identity]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
