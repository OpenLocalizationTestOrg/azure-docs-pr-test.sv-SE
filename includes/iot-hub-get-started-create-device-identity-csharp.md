## <a name="create-a-device-identity"></a>Skapa en enhetsidentitet
I det här avsnittet skapar du en .NET-konsolapp som skapar en enhetsidentitet i hello identitetsregistret i din IoT-hubb. En enhet kan inte ansluta tooIoT hubb om den inte har en post i registret för hello identitet. Mer information finns i avsnittet hello ”identitetsregistret” av hello [IoT-hubb Utvecklarhandbok][lnk-devguide-identity]. När du kör den här konsolen appen genereras ett unikt enhets-ID och nyckel att enheten kan använda tooidentify själva när den skickar enhet till moln meddelanden tooIoT hubb. Enhets-ID är skiftlägeskänsliga.

1. Lägga till en ny lösning för Visual C# klassiska skrivbordet projektet tooa i Visual Studio med hello **Konsolapp (.NET Framework)** projektmall. Se till att hello av .NET Framework 4.5.1 eller senare. Namnet hello projektet **CreateDeviceIdentity** och namnet hello lösningen **IoTHubGetStarted**.
   
    ![Nytt Visual C# Windows Classic Desktop-projekt][10]
2. I Solution Explorer högerklickar du på hello **CreateDeviceIdentity** projektet och klicka sedan på **hantera NuGet-paket**.
3. I hello **NuGet Package Manager** väljer **Bläddra**, söka efter **microsoft.azure.devices**väljer **installera** tooinstall Hej **Microsoft.Azure.Devices** paketet och acceptera hello villkor för användning. Den här proceduren hämtar, installerar och lägger till en referens toohello [Azure IoT service SDK] [ lnk-nuget-service-sdk] NuGet-paketet och dess beroenden.
   
    ![Fönstret för NuGet-pakethanteraren][11]
4. Lägg till följande hello `using` instruktioner överst hello i hello **Program.cs** fil:
   
        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Common.Exceptions;
5. Lägg till följande fält toohello hello **programmet** klass. Ersätt hello platshållarvärde med hello IoT-hubb anslutningssträngen för hello-hubb som du skapade i föregående avsnitt i hello.
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
6. Lägg till följande metod toohello hello **programmet** klass:
   
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
   
    Den här metoden skapar en enhetsidentitet med ID:t **myFirstDevice**. (Om enhets-ID redan finns i hello identitetsregistret hello kod bara hämtar hello befintliga enhetsinformation.) hello app visar hello primärnyckel identitet. Du kan använda den här nyckeln i hello simulerade enheten app tooconnect tooyour IoT-hubb.
[!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

7. Slutligen lägger du till följande rader toohello hello **Main** metoden:
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddDeviceAsync().Wait();
        Console.ReadLine();
8. Kör det här programmet och anteckna hello enhetsnyckel.
   
    ![Enhetsnyckel som genererats av hello program][12]

> [!NOTE]
> Hej IoT-hubb identitetsregistret lagrar bara enheten identiteter tooenable säker åtkomst toohello IoT-hubb. Enheten ID och nycklar toouse lagras som säkerhetsreferenser och en aktiverat/inaktiverat flagga som du kan använda toodisable åtkomst för en enskild enhet. Om ditt program måste toostore andra enhetsspecifika metadata, använder den en programspecifika butik. Mer information finns i [utvecklarhandboken för IoT Hub][lnk-devguide-identity].
> 
> 

<!-- Images. -->
[10]: ./media/iot-hub-get-started-create-device-identity-csharp/create-identity-csharp1.png
[11]: ./media/iot-hub-get-started-create-device-identity-csharp/create-identity-csharp2.png
[12]: ./media/iot-hub-get-started-create-device-identity-csharp/create-identity-csharp3.png


<!-- Links -->
[lnk-devguide-identity]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
