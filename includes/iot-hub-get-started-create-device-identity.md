## <a name="create-a-device-identity"></a>Skapa en enhetsidentitet

I det här avsnittet kan du använda verktyget Node.js [iothub explorer] [ iot-hub-explorer] toocreate en enhetsidentitet för den här självstudiekursen. Enhets-ID är skiftlägeskänsliga.

1. Kör följande hello i kommandoradsverktyget miljön:

    `npm install -g iothub-explorer@latest`

1. Kör sedan följande kommando toologin tooyour hubb hello. Ersätt `{iot hub connection string}` med hello IoT-hubb anslutningssträngen som du kopierade tidigare:

    `iothub-explorer login "{iot hub connection string}"`

1. Skapa en ny enhetsidentitet som kallas slutligen `myDeviceId` med hello-kommando:

    `iothub-explorer create myDeviceId --connection-string`

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

Anteckna anslutningssträngen för hello enheten från hello resultat. Den här enheten anslutningssträngen används av hello enheten app tooconnect tooyour IoT-hubb som en enhet.

![][img-identity]

Se för[komma igång med IoT-hubb] [ lnk-getstarted] tooprogrammatically skapa enheten identiteter.

<!-- images and links -->
[img-identity]: media/iot-hub-get-started-create-device-identity/devidentity.png

[iot-hub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md

[lnk-getstarted]: ../articles/iot-hub/iot-hub-csharp-csharp-getstarted.md
