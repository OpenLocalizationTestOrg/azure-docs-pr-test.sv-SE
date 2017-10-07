---
title: aaaAzure IoT Suite och Logic Apps | Microsoft Docs
description: "En självstudiekurs om hur toohook in Logic Apps tooAzure IoT Suite för affärsprocess."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 4629a7af-56ca-4b21-a769-5fa18bc3ab07
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: corywink
ms.openlocfilehash: 6ef7311ac38f4e2ddb032cff0fb73591da5f76c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-connect-logic-app-tooyour-azure-iot-suite-remote-monitoring-preconfigured-solution"></a>Självstudier: Anslut Logikapp tooyour Azure IoT Suite Fjärrövervaknings förkonfigurerade lösningen
Hej [Microsoft Azure IoT Suite] [ lnk-internetofthings] fjärråtkomst övervakning förkonfigurerade lösningen är ett bra sätt tooget snabbt igång med en slutpunkt till slutpunkt funktionsuppsättning som är ett exempel på en IoT-lösning. Den här självstudiekursen vägleder dig igenom hur tooadd Logikapp tooyour Microsoft Azure IoT Suite fjärrövervaknings förkonfigurerade lösningen. Dessa steg visar hur du kan dra ytterligare IoT-lösningen genom att ansluta den tooa affärsprocess.

*Om du letar efter en genomgång av hur tooprovision en fjärrövervaknings förkonfigurerade lösningen, se [Självstudier: Kom igång med hello IoT förkonfigurerade lösningar][lnk-getstarted].*

Innan du börjar den här kursen bör du:

* Etablera hello fjärrövervaknings förkonfigurerade lösning i din Azure-prenumeration.
* Skapa en SendGrid konto tooenable toosend ett e-postmeddelande som utlöser dina affärsprocesser. Du kan registrera dig för ett kostnadsfritt utvärderingskonto på [SendGrid](https://sendgrid.com/) genom att klicka på **försök gratis**. När du har registrerat för din kostnadsfritt konto, måste toocreate en [API-nyckeln](https://sendgrid.com/docs/User_Guide/Settings/api_keys.html) i SendGrid som ger behörighet toosend e-post. Du behöver den här API-nyckeln senare i självstudiekursen hello.

toocomplete den här kursen behöver du Visual Studio 2015 eller Visual Studio 2017 toomodify hello åtgärder i hello förkonfigurerade lösningens serverdel.

Under förutsättning att du redan har etablerat din fjärrövervaknings förkonfigurerade lösningen, navigera toohello resursgruppen för lösningen i hello [Azure-portalen][lnk-azureportal]. hello resursgruppen har hello samma namn som hello lösning servernamnet du valde när du har etablerat din fjärranslutna övervakningslösning. Du kan se alla hello etablerade Azure-resurser för din lösning förutom hello Azure Active Directory-program som du hittar i hello klassiska Azure-portalen i hello resursgruppen. hello följande skärmbild visar ett exempel **resursgruppen** bladet för en fjärransluten övervakning förkonfigurerade lösningen:

![](media/iot-suite-logic-apps-tutorial/resourcegroup.png)

toobegin, konfigurera hello logik app toouse med hello förkonfigurerade lösningen.

## <a name="set-up-hello-logic-app"></a>Ställ in hello Logikapp
1. Klicka på **Lägg till** hello överst i dina blad för resursgrupp i hello Azure-portalen.
2. Sök efter **Logikapp**markerar du den och klicka sedan på **skapa**.
3. Fyll i hello **namn** och Använd hello samma **prenumeration** och **resursgruppen** som du använde när du har etablerat din fjärranslutna övervakningslösning. Klicka på **Skapa**.
   
    ![](media/iot-suite-logic-apps-tutorial/createlogicapp.png)
4. När distributionen är klar kan du se hello Logikapp visas som en resurs i resursgruppen.
5. Klicka på hello Logikapp toonavigate toohello Logikapp bladet välj hello **tom Logikapp** mallen tooopen hello **Logic Apps Designer**.
   
    ![](media/iot-suite-logic-apps-tutorial/logicappsdesigner.png)
6. Välj **begära**. Den här åtgärden anger att en inkommande HTTP-begäran med en viss JSON formateras nyttolast fungerar som en utlösare.
7. Klistra in följande kod i hello begära brödtext JSON-Schema hello:
   
    ```json
    {
      "$schema": "http://json-schema.org/draft-04/schema#",
      "id": "/",
      "properties": {
        "DeviceId": {
          "id": "DeviceId",
          "type": "string"
        },
        "measuredValue": {
          "id": "measuredValue",
          "type": "integer"
        },
        "measurementName": {
          "id": "measurementName",
          "type": "string"
        }
      },
      "required": [
        "DeviceId",
        "measurementName",
        "measuredValue"
      ],
      "type": "object"
    }
    ```
   
   > [!NOTE]
   > Du kan kopiera hello URL: en för hello HTTP post när du sparar hello logikapp, men först måste du lägga till en åtgärd.
   > 
   > 
8. Klicka på **+ nytt steg** under manuell utlösaren. Klicka på **lägga till en åtgärd**.
   
    ![](media/iot-suite-logic-apps-tutorial/logicappcode.png)
9. Sök efter **SendGrid - skicka e-post** och klicka på den.
   
    ![](media/iot-suite-logic-apps-tutorial/logicappaction.png)
10. Ange ett namn för hello anslutning som **SendGridConnection**, ange hello **SendGrid API-nyckel** du skapade när du konfigurerar ditt SendGrid-konto och klicka på **skapa**.
    
    ![](media/iot-suite-logic-apps-tutorial/sendgridconnection.png)
11. Lägg till-e-postadresser egna tooboth hello **från** och **till** fält. Lägg till **Remote övervakningsavisering [DeviceId]** toohello **ämne** fältet. I hello **e-postmeddelandets brödtext** fältet, lägga till **enheten [DeviceId] har rapporterat [measurementName] med [measuredValue]-värde.** Du kan lägga till **[DeviceId]**, **[measurementName]**, och **[measuredValue]** genom att klicka i hello **du kan infoga data från föregående steg**avsnitt.
    
    ![](media/iot-suite-logic-apps-tutorial/sendgridaction.png)
12. Klicka på **spara** i hello huvudmenyn.
13. Klicka på hello **begära** utlösare och kopiera hello **Http Post toothis URL** värde. URL: en måste senare i den här kursen.

> [!NOTE]
> Logic Apps aktivera toorun [många olika typer av åtgärder] [ lnk-logic-apps-actions] inklusive åtgärder i Office 365. 
> 
> 

## <a name="set-up-hello-eventprocessor-web-job"></a>Ställ in hello EventProcessor Web jobb
Anslut din förkonfigurerade lösning toohello Logikappen som du skapade i det här avsnittet. toocomplete den här uppgiften som du lägger till hello URL tootrigger hello Logikapp toohello åtgärd som utlöses när en enhet sensor värdet överskrider ett tröskelvärde.

1. Använd din git klienten tooclone hello senaste versionen av hello [azure iot-fjärr-övervakning github-lagringsplatsen][lnk-rmgithub]. Exempel:
   
    ```cmd
    git clone https://github.com/Azure/azure-iot-remote-monitoring.git
    ```
2. Öppna i Visual Studio hello **RemoteMonitoring.sln** från hello lokal kopia av hello-databasen.
3. Öppna hello **ActionRepository.cs** filen i hello **infrastruktur\\databasen** mapp.
4. Uppdatera hello **actionIds** ordlista med hello **Http Post toothis URL** du antecknade från din Logikapp på följande sätt:
   
    ```csharp
    private Dictionary<string,string> actionIds = new Dictionary<string, string>()
    {
        { "Send Message", "<Http Post toothis URL>" },
        { "Raise Alarm", "<Http Post toothis URL>" }
    };
    ```
5. Spara hello ändringar i lösningen och avsluta Visual Studio.

## <a name="deploy-from-hello-command-line"></a>Distribuera från hello kommandorad
I det här avsnittet kan du distribuera den uppdaterade versionen av hello övervakning lösning tooreplace hello fjärrversionen för närvarande körs i Azure.

1. Följande hello [dev ställa in] [ lnk-devsetup] instruktioner tooset upp din miljö för distribution.
2. toodeploy lokalt, följ hello [lokala distributionen] [ lnk-localdeploy] instruktioner.
3. toodeploy toohello moln och uppdatera den befintliga distributionen i molnet, följ hello [cloud deployment] [ lnk-clouddeploy] instruktioner. Använd hello namnet på din ursprungliga distribution som hello distributionens namn. Till exempel om den ursprungliga distributionen av hello anropades **demologicapp**, använda hello följande kommando:
   
   ```cmd
   build.cmd cloud release demologicapp
   ```
   
   När hello skapa skriptet körs, ska du toouse hello samma Azure-konto, prenumeration, region och Active Directory-instans som du använde när du har etablerat hello lösning.

## <a name="see-your-logic-app-in-action"></a>Se din Logikapp i praktiken
hello remote förkonfigurerade övervakningslösning har två regler som ställts in som standard när du etablerar en lösning. Båda innehåller inte hello **SampleDevice001** enhet:

* Temperatur > 38.00
* Fuktighet > 48.00

hello temperatur regel utlöser hello **höja larm** åtgärd och hello fuktighet regel utlöser hello **SendMessage** åtgärd. Under förutsättning att du har använt hello samma URL för båda åtgärder hello **ActionRepository** class, logik app-utlösare för antingen regeln. Båda reglerna använder SendGrid toosend en e-toohello **till** adress med information om hello avisering.

> [!NOTE]
> Hej Logikapp fortsätter tootrigger varje gång hello tröskelvärdet har uppnåtts. tooavoid onödiga e-postmeddelanden, kan du antingen inaktivera hello regler i din lösning portalen eller inaktivera hello Logikapp i hello [Azure-portalen][lnk-azureportal].
> 
> 

Dessutom tooreceiving e-postmeddelanden, du kan också se när hello Logikapp som körs i hello portal:

![](media/iot-suite-logic-apps-tutorial/logicapprun.png)

## <a name="next-steps"></a>Nästa steg
Nu när du har använt en Logikapp tooconnect hello förkonfigurerade lösningen tooa affärsprocess, kan du läsa mer om hello alternativ för att anpassa hello förkonfigurerade lösningar:

* [Använda dynamisk telemetri med hello remote förkonfigurerade övervakningslösning][lnk-dynamic]
* [Enhetens information metadata i hello remote förkonfigurerade övervakningslösning][lnk-devinfo]

[lnk-dynamic]: iot-suite-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[lnk-internetofthings]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-getstarted]: iot-suite-getstarted-preconfigured-solutions.md
[lnk-azureportal]: https://portal.azure.com
[lnk-logic-apps-actions]: ../connectors/apis-list.md
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-devsetup]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/dev-setup.md
[lnk-localdeploy]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/local-deployment.md
[lnk-clouddeploy]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/cloud-deployment.md
