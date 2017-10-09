## <a name="create-a-device-identity"></a>Skapa en enhetsidentitet

I det här avsnittet kan du använda hello [Azure-portalen] [ lnk-azure-portal] toocreate en enhetsidentitet i hello identitetsregistret i din IoT-hubb. En enhet kan inte ansluta tooIoT hubb om den inte har en post i registret för hello identitet. Mer information finns i avsnittet hello ”identitetsregistret” av hello [IoT-hubb Utvecklarhandbok][lnk-devguide-identity]. Hej **enheten Explorer** i hello portal hjälper dig att generera ett unikt enhets-ID och nyckel att enheten kan använda tooidentify själva när den ansluter tooIoT hubb. Enhets-ID är skiftlägeskänsliga.

1. Kontrollera att du är inloggad toohello [Azure-portalen][lnk-azure-portal].

1. I hello Jumpbar klickar du på **alla resurser** och hitta din IoT-hubb-resurs.

    ![Navigera tooyour Iot-hubb][img-find-iothub]

1. När din IoT-hubb resurs öppnas, klickar du på hello **enheten Explorer** verktyget och klicka sedan på **Lägg till** hello överst. Ange hello namn för den nya enheten som **myDeviceId**, och klicka på **spara**.

    ![Skapa enhetsidentitet i portalen][img-create-device]

   Detta skapar en ny enhetsidentitet för din IoT-hubb.

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

1. I hello **enheten Explorer**'s listan över enheter klickar du på hello nyskapad enheten och anteckna hello **anslutningssträngen---primärnyckel**. 

    ![Anslutningssträngen för enhet][img-connection-string]

> [!NOTE]
> Hej IoT-hubb identitetsregistret lagrar bara enheten identiteter tooenable säker åtkomst toohello IoT-hubb. Enheten ID och nycklar toouse lagras som säkerhetsreferenser och en aktiverat/inaktiverat flagga som du kan använda toodisable åtkomst för en enskild enhet. Om ditt program måste toostore andra enhetsspecifika metadata, använder den en programspecifika butik. Mer information finns i [utvecklarhandboken för IoT Hub][lnk-devguide-identity].

<!-- Images. -->
[img-find-iothub]: ./media/iot-hub-get-started-create-device-identity-portal/find-iothub.png
[img-create-device]: ./media/iot-hub-get-started-create-device-identity-portal/create-identity-portal.png
[img-connection-string]: ./media/iot-hub-get-started-create-device-identity-portal/device-connection-string.png


<!-- Links -->
[lnk-azure-portal]: https://portal.azure.com
[lnk-devguide-identity]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md

