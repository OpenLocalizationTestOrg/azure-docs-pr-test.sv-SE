---
title: Distribuera Azure-funktionen med Azure IoT-Edge | Microsoft Docs
description: Distribuera Azure-funktion som en modul till en insticksenhet
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: v-jamebr
ms.date: 11/15/2017
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 1dfe46d307a076ae02362c4bba292602001ed915
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/09/2017
---
# <a name="deploy-azure-function-as-an-iot-edge-module---preview"></a>Distribuera Azure-funktion som en gräns för IoT-modul - förhandsgranskning
Du kan använda Azure Functions för att distribuera kod som implementerar affärslogiken direkt till IoT Edge-enheter. Den här självstudiekursen vägleder dig genom att skapa och distribuera en Azure-funktion som filtrerar sensordata på simulerade IoT gränsenheten som du skapade i distribuera Azure IoT kanten på en simulerad enhet på [Windows] [ lnk-tutorial1-win]eller [Linux] [ lnk-tutorial1-lin] självstudier. I den här guiden får du lära dig hur man:     

> [!div class="checklist"]
> * Använd Visual Studio-koden för att skapa en Azure-funktion
> * Använd VS-kod och Docker att skapa en Docker-avbildning och publicera i registret 
> * Distribuera modulen till din IoT-Edge-enhet
> * Visa genererade data


Azure-funktion som du skapar i den här självstudiekursen filtrerar temperatur data som genereras av enheten och endast skickar meddelanden uppströms till Azure IoT Hub när temperaturen är högre än angivet tröskelvärde. 

## <a name="prerequisites"></a>Krav

* Azure IoT gränsenheten som du skapade i Snabbstart eller tidigare kursen.
* [Visual Studio Code](https://code.visualstudio.com/). 
* [C# för Visual Studio Code (drivs av OmniSharp) tillägg](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). 
* [Azure IoT kant-tillägget för Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). 
* [Docker](https://docs.docker.com/engine/installation/). Community Edition (CE) för din plattform är tillräcklig för den här kursen. 
* [.NET core 2.0 SDK](https://www.microsoft.com/net/core#windowscmd). 

## <a name="create-a-container-registry"></a>Skapa ett behållarregister
I de här självstudierna använder Azure IoT kant-tillägget för VS-kod att skapa en modul och skapa en **behållaren image** från filer. Sedan du överför avbildningen till en **registret** som lagrar och hanterar dina bilder. Slutligen kan distribuera du avbildningen från registret ska köras på enheten IoT kant.  

Du kan använda alla Docker-kompatibel registret för den här självstudiekursen. Två populära Docker registret tjänster som är tillgängliga i molnet är [Azure Container registret](https://docs.microsoft.com/azure/container-registry/) och [Docker-hubb](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). Den här kursen används Azure Container registret. 

1. I den [Azure-portalen](https://portal.azure.com)väljer **skapar du en resurs** > **behållare** > **Azure Container registret** .
2. Namnge din registret, Välj en prenumeration, välja en resursgrupp och ange SKU: N till **grundläggande**. 
3. Välj **Skapa**.
4. När du har skapat behållaren registret går du till den och väljer **åtkomstnycklar**. 
5. Växla **administratörsanvändare** till **aktivera**.
6. Kopiera värdena för **inloggningsserver**, **användarnamn**, och **lösenord**. Senare under kursen ska du använda dessa värden. 

## <a name="create-a-function-project"></a>Skapa ett projekt med funktionen
Följande steg visar hur du skapar en IoT-Edge-funktionen med hjälp av Visual Studio Code och Azure IoT kant-tillägget.
1. Öppna Visual Studio-koden.
2. Markera för att öppna VS koden integrerad terminal **visa** > **integrerad Terminal**.
3. Installera (eller uppdatera) på **AzureIoTEdgeFunction** mallen i dotnet, kör följande kommando i en integrerad terminal:

    ```cmd/sh
    dotnet new -i Microsoft.Azure.IoT.Edge.Function
    ```
2. Skapa ett projekt för den nya modulen. Följande kommando skapar projektmappen **FilterFunction**, i den aktuella arbetsmappen:

    ```cmd/sh
    dotnet new aziotedgefunction -n FilterFunction
    ```

3. Välj **filen** > **öppna mappen**, bläddra sedan till den **FilterFunction** mapp och öppna projektet i VS-kod.
4. I VS kod explorer expanderar den **EdgeHubTrigger Csharp** mapp, öppna den **run.csx** fil.
5. Ersätt innehållet i filen med följande kod:

   ```csharp
   #r "Microsoft.Azure.Devices.Client"
   #r "Newtonsoft.Json"

   using System.IO;
   using Microsoft.Azure.Devices.Client;
   using Newtonsoft.Json;

   // Filter messages based on the temperature value in the body of the message and the temperature threshold value.
   public static async Task Run(Message messageReceived, IAsyncCollector<Message> output, TraceWriter log)
   {
        const int temperatureThreshold = 25;
        byte[] messageBytes = messageReceived.GetBytes();
        var messageString = System.Text.Encoding.UTF8.GetString(messageBytes);

        if (!string.IsNullOrEmpty(messageString))
        {
            // Get the body of the message and deserialize it
            var messageBody = JsonConvert.DeserializeObject<MessageBody>(messageString);

            if (messageBody != null && messageBody.machine.temperature > temperatureThreshold)
            {
                // Send the message to the output as the temperature value is greater than the threashold
                var filteredMessage = new Message(messageBytes);
                // Copy the properties of the original message into the new Message object
                foreach (KeyValuePair<string, string> prop in messageReceived.Properties)
                {
                    filteredMessage.Properties.Add(prop.Key, prop.Value);
                }
                // Add a new property to the message to indicate it is an alert
                filteredMessage.Properties.Add("MessageType", "Alert");
                // Send the message        
                await output.AddAsync(filteredMessage);
                log.Info("Received and transferred a message with temperature above the threshold");
            }
        }
    }

    //Define the expected schema for the body of incoming messages
    class MessageBody
    {
        public Machine machine {get;set;}
        public Ambient ambient {get; set;}
        public string timeCreated {get; set;}
    }
    class Machine
    {
       public double temperature {get; set;}
       public double pressure {get; set;}         
    }
    class Ambient
    {
       public double temperature {get; set;}
       public int humidity {get; set;}         
    }
   ```

11. Spara filen.

## <a name="publish-a-docker-image"></a>Publicera en Docker-bild

1. Skapa Docker-bild.
    1. I VS kod explorer expanderar den **Docker** mapp. Expandera mappen för din plattform för behållare, antingen **linux x64** eller **windows nano**. 
    2. Högerklicka på den **Dockerfile** fil och klicka på **skapa IoT kant modulen Docker bild**. 
    3. Navigera till den **FilterFunction** projektmappen, och klicka på **Välj mapp som EXE_DIR**. 
    4. Ange avbildningens namn i popup-textrutan längst upp i fönstret VS-kod. Till exempel: `<your container registry address>/filterfunction:latest`. Behållaren registret adressen är samma som den inloggningsserver som du kopierade från registret. Det bör vara i form av `<your container registry name>.azurecr.io`.
 
4. Logga in på Docker. Ange följande kommando i en integrerad terminal: 

   ```csh/sh
   docker login -u <username> -p <password> <Login server>
   ```
        
   Användarnamn, lösenord och logga in server som ska användas i det här kommandot finns [Azure-portalen] (https://portal.azure.com). Från **alla resurser**, klickar du på panelen för din Azure-behållaren registret för att öppna dess egenskaper och klicka sedan på **åtkomstnycklar**. Kopiera värdena i den **användarnamn**, **lösenord**, och **inloggningsserver** fält. 

3. Skicka bilden till Docker-databasen. Välj **visa** > **kommandot paletten...**  Sök sedan efter **kant: Push-gräns för IoT-modulen Docker bild**.
4. Ange samma avbildningens namn som du använde i steg i textrutan popup 1.d.

## <a name="add-registry-credentials-to-your-edge-device"></a>Lägg till registret autentiseringsuppgifter i Edge-enhet
Lägg till autentiseringsuppgifterna för registret Edge körningsmiljön på datorn där du kör Edge-enhet. Detta ger runtime-åtkomst till pull-behållaren. 

- För Windows, kör du följande kommando:
    
    ```cmd/sh
    iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

- Kör följande kommando för Linux:
    
    ```cmd/sh
    sudo iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

## <a name="run-the-solution"></a>Köra lösningen

1. I den **Azure-portalen**, navigera till din IoT-hubb.
2. Gå till **IoT kant (förhandsgranskning)** och välj IoT-Edge-enhet.
1. Välj **ange moduler**. 
2. Om du redan har distribuerat den **tempSensor** modul till den här enheten, det kan vara fylls automatiskt. Om inte, Följ dessa steg om du vill lägga till den:
    1. Välj **lägga till IoT kant modul**.
    2. I den **namn** anger `tempSensor`.
    3. I den **avbildningen URI** anger `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview`.
    4. Lämna de andra inställningarna oförändrad och klicka på **spara**.
1. Lägg till den **filterFunction** modul.
    1. Välj **Lägg till IoT kant modul** igen.
    2. I den **namn** anger `filterFunction`.
    3. I den **bild** , ange din adress bild, till exempel `<docker registry address>/filterfunction:latest`.
    74. Klicka på **Spara**.
2. Klicka på **Nästa**.
3. I den **ange vägar** steg, kopiera JSON nedan i textrutan. Första vägen transporter meddelanden från temperatursensorn till filtermodul via ”input1”-slutpunkten. Andra vägen transporter meddelanden från modulen filter till IoT-hubb. I den här vägen `$upstream` är särskilda mål som talar om kant-hubb för att skicka meddelanden till IoT-hubb. 

    ```json
    {
       "routes":{
          "sensorToFilter":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filterFunction/inputs/input1\")",
          "filterToIoTHub":"FROM /messages/modules/filterFunction/outputs/* INTO $upstream"
       }
    }
    ```

4. Klicka på **Nästa**.
5. I den **granska mallen** , klicka på **skicka**. 
6. Gå tillbaka till sidan IoT kant enheten information och klickar på **uppdatera**. Du bör se den nya **filterfunction** modulen körs tillsammans med den **tempSensor** modulen och **IoT kant runtime**. 

## <a name="view-generated-data"></a>Visa genererade data

Så här övervakar enhet till moln-meddelanden som skickas från IoT-Edge-enhet till din IoT-hubb:
1. Konfigurera Azure IoT Toolkit-tillägget med anslutningssträngen för din IoT-hubb: 
    1. Navigera till din IoT-hubb i Azure-portalen och välj **principer för delad åtkomst**. 
    2. Välj **iothubowner** Kopiera värdet för **sträng-primära anslutningsnyckel**.
    1. Klicka i koden VS-explorer **IOT HUB-enheter** och klicka sedan på **...** . 
    1. Välj **ange anslutningssträngen för IoT-hubb** och ange anslutningssträngen för Iot-hubb i popup-fönstret. 

1. Om du vill övervaka data som inkommer till IoT-hubben, Välj den **visa** > **kommandot paletten...**  och Sök efter **IoT: börja övervaka D2C meddelandet**. 
2. Om du vill stoppa övervakningen av data, Använd den **IoT: stoppa övervakningen D2C meddelandet** i paletten kommando. 

## <a name="next-steps"></a>Nästa steg

I kursen får skapat du en Azure-funktion som innehåller koden för att filtrera rådata som genereras av enheten IoT kant. Om du vill behålla utforska Azure IoT kant, lär du dig hur du använder en IoT-enhet som en gateway. 

> [!div class="nextstepaction"]
> [Skapa en IoT-Edge-gateway](how-to-create-transparent-gateway.md)

<!--Links-->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
