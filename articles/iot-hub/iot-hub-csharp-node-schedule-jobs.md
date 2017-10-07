---
title: aaaSchedule jobb med Azure IoT Hub (.NET/nod) | Microsoft Docs
description: "Hur jobbet tooschedule en Azure IoT-hubb tooinvoke direkt metod på flera enheter. Du kan använda hello Azure IoT-enhet SDK för Node.js tooimplement hello simulerade enhetsappar och hello Azure IoT-tjänsten SDK för .NET tooimplement ett app toorun hello jobb."
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: 
ms.assetid: 2233356e-b005-4765-ae41-3a4872bda943
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/10/2017
ms.author: juanpere
ms.openlocfilehash: f6148b67129dde4580bfe9ccceafd6400fbc5976
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="schedule-and-broadcast-jobs-netnodejs"></a>Schemat och sändning jobb (.NET/Node.js)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Använd Azure IoT Hub tooschedule och spåra jobb som uppdaterar miljoner enheter. Använd jobb till:

* Uppdatera önskade egenskaper
* Uppdatera taggar
* Anropa direkt metoder

Ett jobb radbryts någon av följande åtgärder och spårar hello körning mot en uppsättning enheter som definieras av en enhet dubbla fråga. En backend-app kan till exempel använda en jobbet tooinvoke direkt metod på 10 000 enheter som startar om hello-enheter. Du anger hello uppsättning av enheter med en enhet dubbla fråga och schemalägga hello jobbet toorun vid ett senare tillfälle. hello spårar jobbförloppet som var och en av hello enheter tar emot och kör hello omstart direkt metod.

toolearn mer information om var och en av dessa funktioner finns:

* Enheten dubbla och egenskaper: [Kom igång med enheten twins] [ lnk-get-started-twin] och [Självstudier: hur toouse enheten dubbla egenskaper][lnk-twin-props]
* Dirigera metoder: [IoT-hubb Utvecklarhandbok - direkt metoder] [ lnk-dev-methods] och [kursen: direkta metoder][lnk-c2d-methods]

I den här självstudiekursen lär du dig att:

* Skapa en enhetsapp som implementerar en direkt metod som kallas **lockDoor** som kan anropas av hello backend-app. Hej enhetsapp tar också emot önskade egenskapsändringar från hello backend-app.
* Skapa en backend-app som skapar ett jobb toocall hello **lockDoor** direkt metod på flera enheter. Ett annat jobb skickar önskade egenskapen uppdaterar toomultiple enheter.

Hello slutet av den här självstudiekursen har du en enhet för Node.js konsolapp och en serverdel för .NET (C#) konsolapp:

**simDevice.js** som ansluter tooyour IoT-hubb, implementerar hello **lockDoor** direkt metod och hanterar behövs egenskapsändringar.

**ScheduleJob** som använder jobb toocall hello **lockDoor** direkta metoden och uppdatera hello enheten dubbla önskade egenskaper på flera enheter.

toocomplete den här kursen behöver du hello följande:

* Visual Studio 2015 eller Visual Studio 2017.
* Node.js version 0.12.x eller senare. hello artikel [förbereda din utvecklingsmiljö] [ lnk-dev-setup] beskriver hur tooinstall Node.js för den här självstudiekursen på Windows- eller Linux.
* Ett aktivt Azure-konto. Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="schedule-jobs-for-calling-a-direct-method-and-sending-device-twin-updates"></a>Schemaläggning för direkta metoden och skicka dubbla uppdateringar

I det här avsnittet skapar du en .NET-konsolapp (med C#) som använder jobb toocall hello **lockDoor** direkt metod och skicka önskade egenskapen uppdaterar toomultiple enheter.

1. I Visual Studio, lägger du till en Visual C# klassiska skrivbordet projektet toohello aktuella lösning med hjälp av hello **konsolprogram** projektmall. Namnet hello projektet **ScheduleJob**.

    ![Nytt Visual C# Windows Classic Desktop-projekt][img-createapp]

1. I Solution Explorer högerklickar du på hello **ScheduleJob** projektet och klicka sedan på **hantera NuGet-paket...** .
1. I hello **NuGet Package Manager** väljer **Bläddra**, söka efter **microsoft.azure.devices**väljer **installera** tooinstall Hej **Microsoft.Azure.Devices** paketet och acceptera hello villkor för användning. Det här steget hämtar, installerar och lägger till en referens toohello [Azure IoT service SDK] [ lnk-nuget-service-sdk] NuGet-paketet och dess beroenden.

    ![Fönstret för NuGet-pakethanteraren][img-servicenuget]
1. Lägg till följande hello `using` instruktioner överst hello i hello **Program.cs** fil:
    
    ```csharp
    using Microsoft.Azure.Devices;
    using Microsoft.Azure.Devices.Shared;
    ```

1. Lägg till följande hello `using` instruktionen om den inte redan finns i hello standard instruktioner.

    ```csharp
    using System.Threading.Tasks;
    ```

1. Lägg till följande fält toohello hello **programmet** klass. Ersätt platshållaren hello med hello IoT-hubb anslutningssträngen för hello-hubb som du skapade i föregående avsnitt i hello.

    ```csharp
    static string connString = "{iot hub connection string}";
    static ServiceClient client;
    static JobClient jobClient;
    ```

1. Lägg till följande metod toohello hello **programmet** klass:

    ```csharp
    public static async Task MonitorJob(string jobId)
    {
        JobResponse result;
        do
        {
            result = await jobClient.GetJobAsync(jobId);
            Console.WriteLine("Job Status : " + result.Status.ToString());
            Thread.Sleep(2000);
        } while ((result.Status != JobStatus.Completed) && (result.Status != JobStatus.Failed));
    }
    ```

1. Lägg till följande metod toohello hello **programmet** klass:

    ```csharp
    public static async Task StartMethodJob(string jobId)
    {
        CloudToDeviceMethod directMethod = new CloudToDeviceMethod("lockDoor", TimeSpan.FromSeconds(5), TimeSpan.FromSeconds(5));

        JobResponse result = await jobClient.ScheduleDeviceMethodAsync(jobId,
            "deviceId='myDeviceId'",
            directMethod,
            DateTime.Now,
            10);

        Console.WriteLine("Started Method Job");
    }
    ```

1. Lägg till följande metod toohello hello **programmet** klass:

    ```csharp
    public static async Task StartTwinUpdateJob(string jobId)
    {
        var twin = new Twin();
        twin.Properties.Desired["Building"] = "43";
        twin.Properties.Desired["Floor"] = "3";
        twin.ETag = "*";

        JobResponse result = await jobClient.ScheduleTwinUpdateAsync(jobId,
            "deviceId='myDeviceId'",
            twin,
            DateTime.Now,
            10);

        Console.WriteLine("Started Twin Update Job");
    }
    ```

1. Slutligen lägger du till följande rader toohello hello **Main** metoden:

    ```csharp
    jobClient = JobClient.CreateFromConnectionString(connString);

    string methodJobId = Guid.NewGuid().ToString();

    StartMethodJob(methodJobId);
    MonitorJob(methodJobId).Wait();
    Console.WriteLine("Press ENTER toorun hello next job.");
    Console.ReadLine();

    string twinUpdateJobId = Guid.NewGuid().ToString();

    StartTwinUpdateJob(twinUpdateJobId);
    MonitorJob(twinUpdateJobId).Wait();
    Console.WriteLine("Press ENTER tooexit.");
    Console.ReadLine();
    ```

1. Hello Solution Explorer, öppna hello **ange Startprojekt...**  och se till att hello **åtgärd** för **ScheduleJob** projektet är **starta**. Skapa hello lösning.

## <a name="create-a-simulated-device-app"></a>Skapa en simulerad enhetsapp

I det här avsnittet skapar du en Node.js-konsolprogram som svarar tooa direkta metoden anropas av hello moln, vilket utlöser simulerade enheten startas om och använder hello rapporterade egenskaper tooenable enheter dubbla frågor tooidentify enheter och när de senast startas om.

1. Skapa en ny tom mapp som kallas **simDevice**.  I hello **simDevice** mapp, skapa en package.json-fil med hello följande kommando vid en kommandotolk.  Acceptera alla standardvärden för hello:

    ```cmd/sh
    npm init
    ```

1. Vid en kommandotolk i hello **simDevice** mapp, kör följande kommando tooinstall hello hello **azure iot-enhet** och **azure-iot-enhet – mqtt** paket:

    ```cmd/sh
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. Med hjälp av en textredigerare, skapa en ny **simDevice.js** filen i hello **simDevice** mapp.

1. Lägg till följande hello 'krävs, instruktioner hello början av hello **simDevice.js** fil:

    ```nodejs
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

1. Lägg till en **connectionString** variabel och använda den toocreate en **klienten** instans. Kontrollera att tooreplace hello platshållarna med värdena lämpliga tooyour installationen.

    ```nodejs
    var connectionString = 'HostName={youriothostname};DeviceId={yourdeviceid};SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

1. Lägg till följande funktion toohandle hello hello **lockDoor** metod.

    ```nodejs
    var onLockDoor = function(request, response) {
   
        // Respond hello cloud app for hello direct method
        response.send(200, function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response toomethod \'' + request.methodName + '\' sent successfully.');
            }
        });
   
        console.log('Locking Door!');
    };
    ```

1. Lägg till följande kod tooregister hello hanterare för hello hello **lockDoor** metod.

    ```nodejs
    client.open(function(err) {
        if (err) {
            console.error('Could not connect tooIotHub client.');
        }  else {
            console.log('Client connected tooIoT Hub.  Waiting for lockDoor direct method.');
            client.onDeviceMethod('lockDoor', onLockDoor);
        }
    });
    ```

1. Spara och Stäng hello **simDevice.js** fil.

> [!NOTE]
> enkel tookeep saker, den här självstudiekursen implementerar inte några återförsöksprincip. I produktionskod, bör du implementera försök principer (till exempel en exponentiell backoff) enligt förslaget i hello MSDN-artikel [hantering av tillfälliga fel][lnk-transient-faults].

## <a name="run-hello-apps"></a>Köra hello appar

Du är nu redo toorun hello appar.

1. Kommandotolken hello i hello **simDevice** mapp, kör följande kommando toobegin lyssnar efter hello omstart direkta metoden hello.

    ```cmd/sh
    node simDevice.js
    ```

1. Kör hello C#-konsolapp **ScheduleJob** genom att högerklicka på hello **ScheduleJob** projekt, sedan välja **felsöka** och **Starta ny instans**.

1. Du kan se hello utdata från både enheten och backend-appar.

    ![Köra hello appar tooschedule jobb][img-schedulejobs]

## <a name="next-steps"></a>Nästa steg

I den här kursen används ett jobb tooschedule en direkta metoden tooa enhet och hello uppdatering av hello enheten dubbla egenskaper.

komma igång med IoT-hubb och device management-mönster, till exempel remote över hello luften firmware-uppdatering, läsa toocontinue [Självstudier: hur toodo en inbyggd programvara uppdatera][lnk-fwupdate].

komma igång med IoT-hubb toocontinue finns [komma igång med IoT kant][lnk-iot-edge].

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-schedule-jobs/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-schedule-jobs/createnetapp.png
[img-schedulejobs]: media/iot-hub-csharp-node-schedule-jobs/schedulejobs.png

[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-props]: iot-hub-node-node-twin-how-to-configure.md
[lnk-c2d-methods]: iot-hub-node-node-direct-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-fwupdate]: iot-hub-node-node-firmware-update.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
