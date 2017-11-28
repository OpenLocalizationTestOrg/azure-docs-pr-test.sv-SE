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
# <a name="tutorial-connect-logic-app-tooyour-azure-iot-suite-remote-monitoring-preconfigured-solution"></a><span data-ttu-id="c77ba-103">Självstudier: Anslut Logikapp tooyour Azure IoT Suite Fjärrövervaknings förkonfigurerade lösningen</span><span class="sxs-lookup"><span data-stu-id="c77ba-103">Tutorial: Connect Logic App tooyour Azure IoT Suite Remote Monitoring preconfigured solution</span></span>
<span data-ttu-id="c77ba-104">Hej [Microsoft Azure IoT Suite] [ lnk-internetofthings] fjärråtkomst övervakning förkonfigurerade lösningen är ett bra sätt tooget snabbt igång med en slutpunkt till slutpunkt funktionsuppsättning som är ett exempel på en IoT-lösning.</span><span class="sxs-lookup"><span data-stu-id="c77ba-104">hello [Microsoft Azure IoT Suite][lnk-internetofthings] remote monitoring preconfigured solution is a great way tooget started quickly with an end-to-end feature set that exemplifies an IoT solution.</span></span> <span data-ttu-id="c77ba-105">Den här självstudiekursen vägleder dig igenom hur tooadd Logikapp tooyour Microsoft Azure IoT Suite fjärrövervaknings förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="c77ba-105">This tutorial walks you through how tooadd Logic App tooyour Microsoft Azure IoT Suite remote monitoring preconfigured solution.</span></span> <span data-ttu-id="c77ba-106">Dessa steg visar hur du kan dra ytterligare IoT-lösningen genom att ansluta den tooa affärsprocess.</span><span class="sxs-lookup"><span data-stu-id="c77ba-106">These steps demonstrate how you can take your IoT solution even further by connecting it tooa business process.</span></span>

<span data-ttu-id="c77ba-107">*Om du letar efter en genomgång av hur tooprovision en fjärrövervaknings förkonfigurerade lösningen, se [Självstudier: Kom igång med hello IoT förkonfigurerade lösningar][lnk-getstarted].*</span><span class="sxs-lookup"><span data-stu-id="c77ba-107">*If you’re looking for a walkthrough on how tooprovision a remote monitoring preconfigured solution, see [Tutorial: Get started with hello IoT preconfigured solutions][lnk-getstarted].*</span></span>

<span data-ttu-id="c77ba-108">Innan du börjar den här kursen bör du:</span><span class="sxs-lookup"><span data-stu-id="c77ba-108">Before you start this tutorial, you should:</span></span>

* <span data-ttu-id="c77ba-109">Etablera hello fjärrövervaknings förkonfigurerade lösning i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="c77ba-109">Provision hello remote monitoring preconfigured solution in your Azure subscription.</span></span>
* <span data-ttu-id="c77ba-110">Skapa en SendGrid konto tooenable toosend ett e-postmeddelande som utlöser dina affärsprocesser.</span><span class="sxs-lookup"><span data-stu-id="c77ba-110">Create a SendGrid account tooenable you toosend an email that triggers your business process.</span></span> <span data-ttu-id="c77ba-111">Du kan registrera dig för ett kostnadsfritt utvärderingskonto på [SendGrid](https://sendgrid.com/) genom att klicka på **försök gratis**.</span><span class="sxs-lookup"><span data-stu-id="c77ba-111">You can sign up for a free trial account at [SendGrid](https://sendgrid.com/) by clicking **Try for Free**.</span></span> <span data-ttu-id="c77ba-112">När du har registrerat för din kostnadsfritt konto, måste toocreate en [API-nyckeln](https://sendgrid.com/docs/User_Guide/Settings/api_keys.html) i SendGrid som ger behörighet toosend e-post.</span><span class="sxs-lookup"><span data-stu-id="c77ba-112">After you have registered for your free trial account, you need toocreate an [API key](https://sendgrid.com/docs/User_Guide/Settings/api_keys.html) in SendGrid that grants permissions toosend mail.</span></span> <span data-ttu-id="c77ba-113">Du behöver den här API-nyckeln senare i självstudiekursen hello.</span><span class="sxs-lookup"><span data-stu-id="c77ba-113">You need this API key later in hello tutorial.</span></span>

<span data-ttu-id="c77ba-114">toocomplete den här kursen behöver du Visual Studio 2015 eller Visual Studio 2017 toomodify hello åtgärder i hello förkonfigurerade lösningens serverdel.</span><span class="sxs-lookup"><span data-stu-id="c77ba-114">toocomplete this tutorial, you need Visual Studio 2015 or Visual Studio 2017 toomodify hello actions in hello preconfigured solution back end.</span></span>

<span data-ttu-id="c77ba-115">Under förutsättning att du redan har etablerat din fjärrövervaknings förkonfigurerade lösningen, navigera toohello resursgruppen för lösningen i hello [Azure-portalen][lnk-azureportal].</span><span class="sxs-lookup"><span data-stu-id="c77ba-115">Assuming you’ve already provisioned your remote monitoring preconfigured solution, navigate toohello resource group for that solution in hello [Azure portal][lnk-azureportal].</span></span> <span data-ttu-id="c77ba-116">hello resursgruppen har hello samma namn som hello lösning servernamnet du valde när du har etablerat din fjärranslutna övervakningslösning.</span><span class="sxs-lookup"><span data-stu-id="c77ba-116">hello resource group has hello same name as hello solution name you chose when you provisioned your remote monitoring solution.</span></span> <span data-ttu-id="c77ba-117">Du kan se alla hello etablerade Azure-resurser för din lösning förutom hello Azure Active Directory-program som du hittar i hello klassiska Azure-portalen i hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="c77ba-117">In hello resource group, you can see all hello provisioned Azure resources for your solution except for hello Azure Active Directory application that you can find in hello Azure Classic Portal.</span></span> <span data-ttu-id="c77ba-118">hello följande skärmbild visar ett exempel **resursgruppen** bladet för en fjärransluten övervakning förkonfigurerade lösningen:</span><span class="sxs-lookup"><span data-stu-id="c77ba-118">hello following screenshot shows an example **Resource group** blade for a remote monitoring preconfigured solution:</span></span>

![](media/iot-suite-logic-apps-tutorial/resourcegroup.png)

<span data-ttu-id="c77ba-119">toobegin, konfigurera hello logik app toouse med hello förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="c77ba-119">toobegin, set up hello logic app toouse with hello preconfigured solution.</span></span>

## <a name="set-up-hello-logic-app"></a><span data-ttu-id="c77ba-120">Ställ in hello Logikapp</span><span class="sxs-lookup"><span data-stu-id="c77ba-120">Set up hello Logic App</span></span>
1. <span data-ttu-id="c77ba-121">Klicka på **Lägg till** hello överst i dina blad för resursgrupp i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c77ba-121">Click **Add** at hello top of your resource group blade in hello Azure portal.</span></span>
2. <span data-ttu-id="c77ba-122">Sök efter **Logikapp**markerar du den och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="c77ba-122">Search for **Logic App**, select it and then click **Create**.</span></span>
3. <span data-ttu-id="c77ba-123">Fyll i hello **namn** och Använd hello samma **prenumeration** och **resursgruppen** som du använde när du har etablerat din fjärranslutna övervakningslösning.</span><span class="sxs-lookup"><span data-stu-id="c77ba-123">Fill out hello **Name** and use hello same **Subscription** and **Resource group** that you used when you provisioned your remote monitoring solution.</span></span> <span data-ttu-id="c77ba-124">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="c77ba-124">Click **Create**.</span></span>
   
    ![](media/iot-suite-logic-apps-tutorial/createlogicapp.png)
4. <span data-ttu-id="c77ba-125">När distributionen är klar kan du se hello Logikapp visas som en resurs i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="c77ba-125">When your deployment completes, you can see hello Logic App is listed as a resource in your resource group.</span></span>
5. <span data-ttu-id="c77ba-126">Klicka på hello Logikapp toonavigate toohello Logikapp bladet välj hello **tom Logikapp** mallen tooopen hello **Logic Apps Designer**.</span><span class="sxs-lookup"><span data-stu-id="c77ba-126">Click hello Logic App toonavigate toohello Logic App blade, select hello **Blank Logic App** template tooopen hello **Logic Apps Designer**.</span></span>
   
    ![](media/iot-suite-logic-apps-tutorial/logicappsdesigner.png)
6. <span data-ttu-id="c77ba-127">Välj **begära**.</span><span class="sxs-lookup"><span data-stu-id="c77ba-127">Select **Request**.</span></span> <span data-ttu-id="c77ba-128">Den här åtgärden anger att en inkommande HTTP-begäran med en viss JSON formateras nyttolast fungerar som en utlösare.</span><span class="sxs-lookup"><span data-stu-id="c77ba-128">This action specifies that an incoming HTTP request with a specific JSON formatted payload acts as a trigger.</span></span>
7. <span data-ttu-id="c77ba-129">Klistra in följande kod i hello begära brödtext JSON-Schema hello:</span><span class="sxs-lookup"><span data-stu-id="c77ba-129">Paste hello following code into hello Request Body JSON Schema:</span></span>
   
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
   > <span data-ttu-id="c77ba-130">Du kan kopiera hello URL: en för hello HTTP post när du sparar hello logikapp, men först måste du lägga till en åtgärd.</span><span class="sxs-lookup"><span data-stu-id="c77ba-130">You can copy hello URL for hello HTTP post after you save hello logic app, but first you must add an action.</span></span>
   > 
   > 
8. <span data-ttu-id="c77ba-131">Klicka på **+ nytt steg** under manuell utlösaren.</span><span class="sxs-lookup"><span data-stu-id="c77ba-131">Click **+ New step** under your manual trigger.</span></span> <span data-ttu-id="c77ba-132">Klicka på **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="c77ba-132">Then click **Add an action**.</span></span>
   
    ![](media/iot-suite-logic-apps-tutorial/logicappcode.png)
9. <span data-ttu-id="c77ba-133">Sök efter **SendGrid - skicka e-post** och klicka på den.</span><span class="sxs-lookup"><span data-stu-id="c77ba-133">Search for **SendGrid - Send email** and click it.</span></span>
   
    ![](media/iot-suite-logic-apps-tutorial/logicappaction.png)
10. <span data-ttu-id="c77ba-134">Ange ett namn för hello anslutning som **SendGridConnection**, ange hello **SendGrid API-nyckel** du skapade när du konfigurerar ditt SendGrid-konto och klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="c77ba-134">Enter a name for hello connection, such as **SendGridConnection**, enter hello **SendGrid API Key** you created when you set up your SendGrid account, and click **Create**.</span></span>
    
    ![](media/iot-suite-logic-apps-tutorial/sendgridconnection.png)
11. <span data-ttu-id="c77ba-135">Lägg till-e-postadresser egna tooboth hello **från** och **till** fält.</span><span class="sxs-lookup"><span data-stu-id="c77ba-135">Add email addresses you own tooboth hello **From** and **To** fields.</span></span> <span data-ttu-id="c77ba-136">Lägg till **Remote övervakningsavisering [DeviceId]** toohello **ämne** fältet.</span><span class="sxs-lookup"><span data-stu-id="c77ba-136">Add **Remote monitoring alert [DeviceId]** toohello **Subject** field.</span></span> <span data-ttu-id="c77ba-137">I hello **e-postmeddelandets brödtext** fältet, lägga till **enheten [DeviceId] har rapporterat [measurementName] med [measuredValue]-värde.**</span><span class="sxs-lookup"><span data-stu-id="c77ba-137">In hello **Email Body** field, add **Device [DeviceId] has reported [measurementName] with value [measuredValue].**</span></span> <span data-ttu-id="c77ba-138">Du kan lägga till **[DeviceId]**, **[measurementName]**, och **[measuredValue]** genom att klicka i hello **du kan infoga data från föregående steg**avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c77ba-138">You can add **[DeviceId]**, **[measurementName]**, and **[measuredValue]** by clicking in hello **You can insert data from previous steps** section.</span></span>
    
    ![](media/iot-suite-logic-apps-tutorial/sendgridaction.png)
12. <span data-ttu-id="c77ba-139">Klicka på **spara** i hello huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="c77ba-139">Click **Save** in hello top menu.</span></span>
13. <span data-ttu-id="c77ba-140">Klicka på hello **begära** utlösare och kopiera hello **Http Post toothis URL** värde.</span><span class="sxs-lookup"><span data-stu-id="c77ba-140">Click hello **Request** trigger and copy hello **Http Post toothis URL** value.</span></span> <span data-ttu-id="c77ba-141">URL: en måste senare i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="c77ba-141">You need this URL later in this tutorial.</span></span>

> [!NOTE]
> <span data-ttu-id="c77ba-142">Logic Apps aktivera toorun [många olika typer av åtgärder] [ lnk-logic-apps-actions] inklusive åtgärder i Office 365.</span><span class="sxs-lookup"><span data-stu-id="c77ba-142">Logic Apps enable you toorun [many different types of action][lnk-logic-apps-actions] including actions in Office 365.</span></span> 
> 
> 

## <a name="set-up-hello-eventprocessor-web-job"></a><span data-ttu-id="c77ba-143">Ställ in hello EventProcessor Web jobb</span><span class="sxs-lookup"><span data-stu-id="c77ba-143">Set up hello EventProcessor Web Job</span></span>
<span data-ttu-id="c77ba-144">Anslut din förkonfigurerade lösning toohello Logikappen som du skapade i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="c77ba-144">In this section, you connect your preconfigured solution toohello Logic App you created.</span></span> <span data-ttu-id="c77ba-145">toocomplete den här uppgiften som du lägger till hello URL tootrigger hello Logikapp toohello åtgärd som utlöses när en enhet sensor värdet överskrider ett tröskelvärde.</span><span class="sxs-lookup"><span data-stu-id="c77ba-145">toocomplete this task, you add hello URL tootrigger hello Logic App toohello action that fires when a device sensor value exceeds a threshold.</span></span>

1. <span data-ttu-id="c77ba-146">Använd din git klienten tooclone hello senaste versionen av hello [azure iot-fjärr-övervakning github-lagringsplatsen][lnk-rmgithub].</span><span class="sxs-lookup"><span data-stu-id="c77ba-146">Use your git client tooclone hello latest version of hello [azure-iot-remote-monitoring github repository][lnk-rmgithub].</span></span> <span data-ttu-id="c77ba-147">Exempel:</span><span class="sxs-lookup"><span data-stu-id="c77ba-147">For example:</span></span>
   
    ```cmd
    git clone https://github.com/Azure/azure-iot-remote-monitoring.git
    ```
2. <span data-ttu-id="c77ba-148">Öppna i Visual Studio hello **RemoteMonitoring.sln** från hello lokal kopia av hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="c77ba-148">In Visual Studio, open hello **RemoteMonitoring.sln** from hello local copy of hello repository.</span></span>
3. <span data-ttu-id="c77ba-149">Öppna hello **ActionRepository.cs** filen i hello **infrastruktur\\databasen** mapp.</span><span class="sxs-lookup"><span data-stu-id="c77ba-149">Open hello **ActionRepository.cs** file in hello **Infrastructure\\Repository** folder.</span></span>
4. <span data-ttu-id="c77ba-150">Uppdatera hello **actionIds** ordlista med hello **Http Post toothis URL** du antecknade från din Logikapp på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="c77ba-150">Update hello **actionIds** dictionary with hello **Http Post toothis URL** you noted from your Logic App as follows:</span></span>
   
    ```csharp
    private Dictionary<string,string> actionIds = new Dictionary<string, string>()
    {
        { "Send Message", "<Http Post toothis URL>" },
        { "Raise Alarm", "<Http Post toothis URL>" }
    };
    ```
5. <span data-ttu-id="c77ba-151">Spara hello ändringar i lösningen och avsluta Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c77ba-151">Save hello changes in solution and exit Visual Studio.</span></span>

## <a name="deploy-from-hello-command-line"></a><span data-ttu-id="c77ba-152">Distribuera från hello kommandorad</span><span class="sxs-lookup"><span data-stu-id="c77ba-152">Deploy from hello command line</span></span>
<span data-ttu-id="c77ba-153">I det här avsnittet kan du distribuera den uppdaterade versionen av hello övervakning lösning tooreplace hello fjärrversionen för närvarande körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="c77ba-153">In this section, you deploy your updated version of hello remote monitoring solution tooreplace hello version currently running in Azure.</span></span>

1. <span data-ttu-id="c77ba-154">Följande hello [dev ställa in] [ lnk-devsetup] instruktioner tooset upp din miljö för distribution.</span><span class="sxs-lookup"><span data-stu-id="c77ba-154">Following hello [dev set-up][lnk-devsetup] instructions tooset up your environment for deployment.</span></span>
2. <span data-ttu-id="c77ba-155">toodeploy lokalt, följ hello [lokala distributionen] [ lnk-localdeploy] instruktioner.</span><span class="sxs-lookup"><span data-stu-id="c77ba-155">toodeploy locally, follow hello [local deployment][lnk-localdeploy] instructions.</span></span>
3. <span data-ttu-id="c77ba-156">toodeploy toohello moln och uppdatera den befintliga distributionen i molnet, följ hello [cloud deployment] [ lnk-clouddeploy] instruktioner.</span><span class="sxs-lookup"><span data-stu-id="c77ba-156">toodeploy toohello cloud and update your existing cloud deployment, follow hello [cloud deployment][lnk-clouddeploy] instructions.</span></span> <span data-ttu-id="c77ba-157">Använd hello namnet på din ursprungliga distribution som hello distributionens namn.</span><span class="sxs-lookup"><span data-stu-id="c77ba-157">Use hello name of your original deployment as hello deployment name.</span></span> <span data-ttu-id="c77ba-158">Till exempel om den ursprungliga distributionen av hello anropades **demologicapp**, använda hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c77ba-158">For example if hello original deployment was called **demologicapp**, use hello following command:</span></span>
   
   ```cmd
   build.cmd cloud release demologicapp
   ```
   
   <span data-ttu-id="c77ba-159">När hello skapa skriptet körs, ska du toouse hello samma Azure-konto, prenumeration, region och Active Directory-instans som du använde när du har etablerat hello lösning.</span><span class="sxs-lookup"><span data-stu-id="c77ba-159">When hello build script runs, be sure toouse hello same Azure account, subscription, region, and Active Directory instance you used when you provisioned hello solution.</span></span>

## <a name="see-your-logic-app-in-action"></a><span data-ttu-id="c77ba-160">Se din Logikapp i praktiken</span><span class="sxs-lookup"><span data-stu-id="c77ba-160">See your Logic App in action</span></span>
<span data-ttu-id="c77ba-161">hello remote förkonfigurerade övervakningslösning har två regler som ställts in som standard när du etablerar en lösning.</span><span class="sxs-lookup"><span data-stu-id="c77ba-161">hello remote monitoring preconfigured solution has two rules set up by default when you provision a solution.</span></span> <span data-ttu-id="c77ba-162">Båda innehåller inte hello **SampleDevice001** enhet:</span><span class="sxs-lookup"><span data-stu-id="c77ba-162">Both rules are on hello **SampleDevice001** device:</span></span>

* <span data-ttu-id="c77ba-163">Temperatur > 38.00</span><span class="sxs-lookup"><span data-stu-id="c77ba-163">Temperature > 38.00</span></span>
* <span data-ttu-id="c77ba-164">Fuktighet > 48.00</span><span class="sxs-lookup"><span data-stu-id="c77ba-164">Humidity > 48.00</span></span>

<span data-ttu-id="c77ba-165">hello temperatur regel utlöser hello **höja larm** åtgärd och hello fuktighet regel utlöser hello **SendMessage** åtgärd.</span><span class="sxs-lookup"><span data-stu-id="c77ba-165">hello temperature rule triggers hello **Raise Alarm** action and hello Humidity rule triggers hello **SendMessage** action.</span></span> <span data-ttu-id="c77ba-166">Under förutsättning att du har använt hello samma URL för båda åtgärder hello **ActionRepository** class, logik app-utlösare för antingen regeln.</span><span class="sxs-lookup"><span data-stu-id="c77ba-166">Assuming you used hello same URL for both actions hello **ActionRepository** class, your logic app triggers for either rule.</span></span> <span data-ttu-id="c77ba-167">Båda reglerna använder SendGrid toosend en e-toohello **till** adress med information om hello avisering.</span><span class="sxs-lookup"><span data-stu-id="c77ba-167">Both rules use SendGrid toosend an email toohello **To** address with details of hello alert.</span></span>

> [!NOTE]
> <span data-ttu-id="c77ba-168">Hej Logikapp fortsätter tootrigger varje gång hello tröskelvärdet har uppnåtts.</span><span class="sxs-lookup"><span data-stu-id="c77ba-168">hello Logic App continues tootrigger every time hello threshold is met.</span></span> <span data-ttu-id="c77ba-169">tooavoid onödiga e-postmeddelanden, kan du antingen inaktivera hello regler i din lösning portalen eller inaktivera hello Logikapp i hello [Azure-portalen][lnk-azureportal].</span><span class="sxs-lookup"><span data-stu-id="c77ba-169">tooavoid unnecessary emails, you can either disable hello rules in your solution portal or disable hello Logic App in hello [Azure portal][lnk-azureportal].</span></span>
> 
> 

<span data-ttu-id="c77ba-170">Dessutom tooreceiving e-postmeddelanden, du kan också se när hello Logikapp som körs i hello portal:</span><span class="sxs-lookup"><span data-stu-id="c77ba-170">In addition tooreceiving emails, you can also see when hello Logic App runs in hello portal:</span></span>

![](media/iot-suite-logic-apps-tutorial/logicapprun.png)

## <a name="next-steps"></a><span data-ttu-id="c77ba-171">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c77ba-171">Next steps</span></span>
<span data-ttu-id="c77ba-172">Nu när du har använt en Logikapp tooconnect hello förkonfigurerade lösningen tooa affärsprocess, kan du läsa mer om hello alternativ för att anpassa hello förkonfigurerade lösningar:</span><span class="sxs-lookup"><span data-stu-id="c77ba-172">Now that you've used a Logic App tooconnect hello preconfigured solution tooa business process, you can learn more about hello options for customizing hello preconfigured solutions:</span></span>

* <span data-ttu-id="c77ba-173">[Använda dynamisk telemetri med hello remote förkonfigurerade övervakningslösning][lnk-dynamic]</span><span class="sxs-lookup"><span data-stu-id="c77ba-173">[Use dynamic telemetry with hello remote monitoring preconfigured solution][lnk-dynamic]</span></span>
* <span data-ttu-id="c77ba-174">[Enhetens information metadata i hello remote förkonfigurerade övervakningslösning][lnk-devinfo]</span><span class="sxs-lookup"><span data-stu-id="c77ba-174">[Device information metadata in hello remote monitoring preconfigured solution][lnk-devinfo]</span></span>

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
