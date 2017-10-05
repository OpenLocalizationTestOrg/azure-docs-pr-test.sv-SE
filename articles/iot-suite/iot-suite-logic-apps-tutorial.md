---
title: Azure IoT Suite och Logic Apps | Microsoft Docs
description: "En självstudiekurs om hur du koppla samman Logic Apps till Azure IoT Suite för affärsprocess."
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
ms.openlocfilehash: 2e7997e2a8bdeeec6083d40acb55e653f87e140b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-connect-logic-app-to-your-azure-iot-suite-remote-monitoring-preconfigured-solution"></a><span data-ttu-id="94cac-103">Självstudier: Anslut Logikappen i Azure IoT Suite Fjärrövervaknings förkonfigurerade lösningen</span><span class="sxs-lookup"><span data-stu-id="94cac-103">Tutorial: Connect Logic App to your Azure IoT Suite Remote Monitoring preconfigured solution</span></span>
<span data-ttu-id="94cac-104">Den [Microsoft Azure IoT Suite] [ lnk-internetofthings] fjärråtkomst övervakning förkonfigurerade lösningen är ett bra sätt att snabbt komma igång med en slutpunkt till slutpunkt funktionsuppsättning som är ett exempel på en IoT-lösning.</span><span class="sxs-lookup"><span data-stu-id="94cac-104">The [Microsoft Azure IoT Suite][lnk-internetofthings] remote monitoring preconfigured solution is a great way to get started quickly with an end-to-end feature set that exemplifies an IoT solution.</span></span> <span data-ttu-id="94cac-105">Den här självstudiekursen vägleder dig igenom hur du lägger till Logikapp till din Microsoft Azure IoT Suite remote förkonfigurerade övervakningslösning.</span><span class="sxs-lookup"><span data-stu-id="94cac-105">This tutorial walks you through how to add Logic App to your Microsoft Azure IoT Suite remote monitoring preconfigured solution.</span></span> <span data-ttu-id="94cac-106">Dessa steg visar hur du kan dra ytterligare IoT-lösningen genom att ansluta till en affärsprocess.</span><span class="sxs-lookup"><span data-stu-id="94cac-106">These steps demonstrate how you can take your IoT solution even further by connecting it to a business process.</span></span>

<span data-ttu-id="94cac-107">*Om du letar efter en genomgång av hur du etablerar en fjärrövervaknings förkonfigurerade lösningen, se [Självstudier: Kom igång med förkonfigurerade IoT-lösningar][lnk-getstarted].*</span><span class="sxs-lookup"><span data-stu-id="94cac-107">*If you’re looking for a walkthrough on how to provision a remote monitoring preconfigured solution, see [Tutorial: Get started with the IoT preconfigured solutions][lnk-getstarted].*</span></span>

<span data-ttu-id="94cac-108">Innan du börjar den här kursen bör du:</span><span class="sxs-lookup"><span data-stu-id="94cac-108">Before you start this tutorial, you should:</span></span>

* <span data-ttu-id="94cac-109">Etablera fjärråtkomst övervakning förkonfigurerade lösningen i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="94cac-109">Provision the remote monitoring preconfigured solution in your Azure subscription.</span></span>
* <span data-ttu-id="94cac-110">Skapa ett SendGrid-konto så att du kan skicka ett e-postmeddelande som utlöser dina affärsprocesser.</span><span class="sxs-lookup"><span data-stu-id="94cac-110">Create a SendGrid account to enable you to send an email that triggers your business process.</span></span> <span data-ttu-id="94cac-111">Du kan registrera dig för ett kostnadsfritt utvärderingskonto på [SendGrid](https://sendgrid.com/) genom att klicka på **försök gratis**.</span><span class="sxs-lookup"><span data-stu-id="94cac-111">You can sign up for a free trial account at [SendGrid](https://sendgrid.com/) by clicking **Try for Free**.</span></span> <span data-ttu-id="94cac-112">När du har registrerat för din kostnadsfritt konto, måste du skapa en [API-nyckeln](https://sendgrid.com/docs/User_Guide/Settings/api_keys.html) i SendGrid som ger behörighet att skicka e-post.</span><span class="sxs-lookup"><span data-stu-id="94cac-112">After you have registered for your free trial account, you need to create an [API key](https://sendgrid.com/docs/User_Guide/Settings/api_keys.html) in SendGrid that grants permissions to send mail.</span></span> <span data-ttu-id="94cac-113">Du behöver den här API-nyckeln senare under kursen.</span><span class="sxs-lookup"><span data-stu-id="94cac-113">You need this API key later in the tutorial.</span></span>

<span data-ttu-id="94cac-114">Den här kursen behöver du Visual Studio 2015 eller Visual Studio-2017 för att ändra åtgärder i förkonfigurerade lösningens serverdel.</span><span class="sxs-lookup"><span data-stu-id="94cac-114">To complete this tutorial, you need Visual Studio 2015 or Visual Studio 2017 to modify the actions in the preconfigured solution back end.</span></span>

<span data-ttu-id="94cac-115">Under förutsättning att du redan har etablerat din fjärrövervaknings förkonfigurerade lösningen måste du gå till resursgruppen för lösningen i den [Azure-portalen][lnk-azureportal].</span><span class="sxs-lookup"><span data-stu-id="94cac-115">Assuming you’ve already provisioned your remote monitoring preconfigured solution, navigate to the resource group for that solution in the [Azure portal][lnk-azureportal].</span></span> <span data-ttu-id="94cac-116">Resursgruppen har samma namn som lösningens namn du angav när du har etablerat din fjärranslutna övervakningslösning.</span><span class="sxs-lookup"><span data-stu-id="94cac-116">The resource group has the same name as the solution name you chose when you provisioned your remote monitoring solution.</span></span> <span data-ttu-id="94cac-117">Du kan se alla etablerade Azure-resurser för din lösning förutom Azure Active Directory-program som du hittar i den klassiska Azure-portalen i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="94cac-117">In the resource group, you can see all the provisioned Azure resources for your solution except for the Azure Active Directory application that you can find in the Azure Classic Portal.</span></span> <span data-ttu-id="94cac-118">Följande skärmbild visar ett exempel **resursgruppen** bladet för en fjärransluten övervakning förkonfigurerade lösningen:</span><span class="sxs-lookup"><span data-stu-id="94cac-118">The following screenshot shows an example **Resource group** blade for a remote monitoring preconfigured solution:</span></span>

![](media/iot-suite-logic-apps-tutorial/resourcegroup.png)

<span data-ttu-id="94cac-119">Börja genom att ställer in logikappen för användning med förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="94cac-119">To begin, set up the logic app to use with the preconfigured solution.</span></span>

## <a name="set-up-the-logic-app"></a><span data-ttu-id="94cac-120">Ställ in Logikappen</span><span class="sxs-lookup"><span data-stu-id="94cac-120">Set up the Logic App</span></span>
1. <span data-ttu-id="94cac-121">Klicka på **Lägg till** längst upp i din blad för resursgrupp i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="94cac-121">Click **Add** at the top of your resource group blade in the Azure portal.</span></span>
2. <span data-ttu-id="94cac-122">Sök efter **Logikapp**markerar du den och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="94cac-122">Search for **Logic App**, select it and then click **Create**.</span></span>
3. <span data-ttu-id="94cac-123">Fyll i den **namn** och använda samma **prenumeration** och **resursgruppen** som du använde när du har etablerat din fjärranslutna övervakningslösning.</span><span class="sxs-lookup"><span data-stu-id="94cac-123">Fill out the **Name** and use the same **Subscription** and **Resource group** that you used when you provisioned your remote monitoring solution.</span></span> <span data-ttu-id="94cac-124">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="94cac-124">Click **Create**.</span></span>
   
    ![](media/iot-suite-logic-apps-tutorial/createlogicapp.png)
4. <span data-ttu-id="94cac-125">När distributionen är klar kan du se Logikappen har listats som en resurs i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="94cac-125">When your deployment completes, you can see the Logic App is listed as a resource in your resource group.</span></span>
5. <span data-ttu-id="94cac-126">Klicka på Logikappen att navigera till bladet Logikapp väljer den **tom Logikapp** mall för att öppna den **Logic Apps Designer**.</span><span class="sxs-lookup"><span data-stu-id="94cac-126">Click the Logic App to navigate to the Logic App blade, select the **Blank Logic App** template to open the **Logic Apps Designer**.</span></span>
   
    ![](media/iot-suite-logic-apps-tutorial/logicappsdesigner.png)
6. <span data-ttu-id="94cac-127">Välj **begära**.</span><span class="sxs-lookup"><span data-stu-id="94cac-127">Select **Request**.</span></span> <span data-ttu-id="94cac-128">Den här åtgärden anger att en inkommande HTTP-begäran med en viss JSON formateras nyttolast fungerar som en utlösare.</span><span class="sxs-lookup"><span data-stu-id="94cac-128">This action specifies that an incoming HTTP request with a specific JSON formatted payload acts as a trigger.</span></span>
7. <span data-ttu-id="94cac-129">Klistra in följande kod i begäran brödtext JSON schemat:</span><span class="sxs-lookup"><span data-stu-id="94cac-129">Paste the following code into the Request Body JSON Schema:</span></span>
   
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
   > <span data-ttu-id="94cac-130">Du kan kopiera URL: en för HTTP-post när du sparar logikappen, men först måste du lägga till en åtgärd.</span><span class="sxs-lookup"><span data-stu-id="94cac-130">You can copy the URL for the HTTP post after you save the logic app, but first you must add an action.</span></span>
   > 
   > 
8. <span data-ttu-id="94cac-131">Klicka på **+ nytt steg** under manuell utlösaren.</span><span class="sxs-lookup"><span data-stu-id="94cac-131">Click **+ New step** under your manual trigger.</span></span> <span data-ttu-id="94cac-132">Klicka på **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="94cac-132">Then click **Add an action**.</span></span>
   
    ![](media/iot-suite-logic-apps-tutorial/logicappcode.png)
9. <span data-ttu-id="94cac-133">Sök efter **SendGrid - skicka e-post** och klicka på den.</span><span class="sxs-lookup"><span data-stu-id="94cac-133">Search for **SendGrid - Send email** and click it.</span></span>
   
    ![](media/iot-suite-logic-apps-tutorial/logicappaction.png)
10. <span data-ttu-id="94cac-134">Ange ett namn för anslutningen, till exempel **SendGridConnection**, ange den **SendGrid API-nyckel** du skapade när du konfigurerar ditt SendGrid-konto och klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="94cac-134">Enter a name for the connection, such as **SendGridConnection**, enter the **SendGrid API Key** you created when you set up your SendGrid account, and click **Create**.</span></span>
    
    ![](media/iot-suite-logic-apps-tutorial/sendgridconnection.png)
11. <span data-ttu-id="94cac-135">Lägg till e-postadresser som du äger för båda de **från** och **till** fält.</span><span class="sxs-lookup"><span data-stu-id="94cac-135">Add email addresses you own to both the **From** and **To** fields.</span></span> <span data-ttu-id="94cac-136">Lägg till **Remote övervakningsavisering [DeviceId]** till den **ämne** fältet.</span><span class="sxs-lookup"><span data-stu-id="94cac-136">Add **Remote monitoring alert [DeviceId]** to the **Subject** field.</span></span> <span data-ttu-id="94cac-137">I den **e-postmeddelandets brödtext** fältet, lägga till **enheten [DeviceId] har rapporterat [measurementName] med [measuredValue]-värde.**</span><span class="sxs-lookup"><span data-stu-id="94cac-137">In the **Email Body** field, add **Device [DeviceId] has reported [measurementName] with value [measuredValue].**</span></span> <span data-ttu-id="94cac-138">Du kan lägga till **[DeviceId]**, **[measurementName]**, och **[measuredValue]** genom att klicka i den **du kan infoga data från föregående steg** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="94cac-138">You can add **[DeviceId]**, **[measurementName]**, and **[measuredValue]** by clicking in the **You can insert data from previous steps** section.</span></span>
    
    ![](media/iot-suite-logic-apps-tutorial/sendgridaction.png)
12. <span data-ttu-id="94cac-139">Klicka på **spara** på huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="94cac-139">Click **Save** in the top menu.</span></span>
13. <span data-ttu-id="94cac-140">Klicka på den **begära** utlösare och kopiera den **Http Post till denna URL** värde.</span><span class="sxs-lookup"><span data-stu-id="94cac-140">Click the **Request** trigger and copy the **Http Post to this URL** value.</span></span> <span data-ttu-id="94cac-141">URL: en måste senare i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="94cac-141">You need this URL later in this tutorial.</span></span>

> [!NOTE]
> <span data-ttu-id="94cac-142">Logic Apps gör det möjligt att köra [många olika typer av åtgärder] [ lnk-logic-apps-actions] inklusive åtgärder i Office 365.</span><span class="sxs-lookup"><span data-stu-id="94cac-142">Logic Apps enable you to run [many different types of action][lnk-logic-apps-actions] including actions in Office 365.</span></span> 
> 
> 

## <a name="set-up-the-eventprocessor-web-job"></a><span data-ttu-id="94cac-143">Ställ in EventProcessor Web-jobb</span><span class="sxs-lookup"><span data-stu-id="94cac-143">Set up the EventProcessor Web Job</span></span>
<span data-ttu-id="94cac-144">I det här avsnittet kan ansluta du din förkonfigurerade lösning till Logikappen som du skapade.</span><span class="sxs-lookup"><span data-stu-id="94cac-144">In this section, you connect your preconfigured solution to the Logic App you created.</span></span> <span data-ttu-id="94cac-145">Lägg till URL: en för att utlösa Logikappen på den åtgärd som utlöses när en enhet sensor värdet överskrider ett tröskelvärde för att slutföra den här uppgiften.</span><span class="sxs-lookup"><span data-stu-id="94cac-145">To complete this task, you add the URL to trigger the Logic App to the action that fires when a device sensor value exceeds a threshold.</span></span>

1. <span data-ttu-id="94cac-146">Använda git-klienten för att klona den senaste versionen av den [azure iot-fjärr-övervakning github-lagringsplatsen][lnk-rmgithub].</span><span class="sxs-lookup"><span data-stu-id="94cac-146">Use your git client to clone the latest version of the [azure-iot-remote-monitoring github repository][lnk-rmgithub].</span></span> <span data-ttu-id="94cac-147">Exempel:</span><span class="sxs-lookup"><span data-stu-id="94cac-147">For example:</span></span>
   
    ```cmd
    git clone https://github.com/Azure/azure-iot-remote-monitoring.git
    ```
2. <span data-ttu-id="94cac-148">Öppna i Visual Studio den **RemoteMonitoring.sln** från den lokala kopian av databasen.</span><span class="sxs-lookup"><span data-stu-id="94cac-148">In Visual Studio, open the **RemoteMonitoring.sln** from the local copy of the repository.</span></span>
3. <span data-ttu-id="94cac-149">Öppna den **ActionRepository.cs** filen i den **infrastruktur\\databasen** mapp.</span><span class="sxs-lookup"><span data-stu-id="94cac-149">Open the **ActionRepository.cs** file in the **Infrastructure\\Repository** folder.</span></span>
4. <span data-ttu-id="94cac-150">Uppdatering av **actionIds** ordlista med den **Http Post till denna URL** du antecknade från din Logikapp på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="94cac-150">Update the **actionIds** dictionary with the **Http Post to this URL** you noted from your Logic App as follows:</span></span>
   
    ```csharp
    private Dictionary<string,string> actionIds = new Dictionary<string, string>()
    {
        { "Send Message", "<Http Post to this URL>" },
        { "Raise Alarm", "<Http Post to this URL>" }
    };
    ```
5. <span data-ttu-id="94cac-151">Spara ändringarna i lösningen och avsluta Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="94cac-151">Save the changes in solution and exit Visual Studio.</span></span>

## <a name="deploy-from-the-command-line"></a><span data-ttu-id="94cac-152">Distribuera från kommandoraden</span><span class="sxs-lookup"><span data-stu-id="94cac-152">Deploy from the command line</span></span>
<span data-ttu-id="94cac-153">I det här avsnittet kan du distribuera den uppdaterade versionen av den fjärranslutna övervakningslösning att ersätta den version som för närvarande körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="94cac-153">In this section, you deploy your updated version of the remote monitoring solution to replace the version currently running in Azure.</span></span>

1. <span data-ttu-id="94cac-154">Efter den [dev ställa in] [ lnk-devsetup] instruktioner för att konfigurera din miljö för distribution.</span><span class="sxs-lookup"><span data-stu-id="94cac-154">Following the [dev set-up][lnk-devsetup] instructions to set up your environment for deployment.</span></span>
2. <span data-ttu-id="94cac-155">Om du vill distribuera lokalt följer den [lokala distributionen] [ lnk-localdeploy] instruktioner.</span><span class="sxs-lookup"><span data-stu-id="94cac-155">To deploy locally, follow the [local deployment][lnk-localdeploy] instructions.</span></span>
3. <span data-ttu-id="94cac-156">Om du vill distribuera till molnet och uppdatera den befintliga distributionen i molnet, följer du de [cloud deployment] [ lnk-clouddeploy] instruktioner.</span><span class="sxs-lookup"><span data-stu-id="94cac-156">To deploy to the cloud and update your existing cloud deployment, follow the [cloud deployment][lnk-clouddeploy] instructions.</span></span> <span data-ttu-id="94cac-157">Använd namnet på din ursprungliga distribution som distributionens namn.</span><span class="sxs-lookup"><span data-stu-id="94cac-157">Use the name of your original deployment as the deployment name.</span></span> <span data-ttu-id="94cac-158">Till exempel om den ursprungliga distributionen av anropades **demologicapp**, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="94cac-158">For example if the original deployment was called **demologicapp**, use the following command:</span></span>
   
   ```cmd
   build.cmd cloud release demologicapp
   ```
   
   <span data-ttu-id="94cac-159">När build-skriptet körs, måste du använda samma Azure-konto, prenumeration, region och Active Directory-instans som du använde när du har etablerat lösningen.</span><span class="sxs-lookup"><span data-stu-id="94cac-159">When the build script runs, be sure to use the same Azure account, subscription, region, and Active Directory instance you used when you provisioned the solution.</span></span>

## <a name="see-your-logic-app-in-action"></a><span data-ttu-id="94cac-160">Se din Logikapp i praktiken</span><span class="sxs-lookup"><span data-stu-id="94cac-160">See your Logic App in action</span></span>
<span data-ttu-id="94cac-161">Fjärråtkomst övervakning förkonfigurerade lösningen har två regler som ställts in som standard när du etablerar en lösning.</span><span class="sxs-lookup"><span data-stu-id="94cac-161">The remote monitoring preconfigured solution has two rules set up by default when you provision a solution.</span></span> <span data-ttu-id="94cac-162">Båda innehåller inte den **SampleDevice001** enhet:</span><span class="sxs-lookup"><span data-stu-id="94cac-162">Both rules are on the **SampleDevice001** device:</span></span>

* <span data-ttu-id="94cac-163">Temperatur > 38.00</span><span class="sxs-lookup"><span data-stu-id="94cac-163">Temperature > 38.00</span></span>
* <span data-ttu-id="94cac-164">Fuktighet > 48.00</span><span class="sxs-lookup"><span data-stu-id="94cac-164">Humidity > 48.00</span></span>

<span data-ttu-id="94cac-165">Utlösare för temperatur-regeln den **höja larm** åtgärden och fuktigheten regel utlösare i **SendMessage** åtgärd.</span><span class="sxs-lookup"><span data-stu-id="94cac-165">The temperature rule triggers the **Raise Alarm** action and the Humidity rule triggers the **SendMessage** action.</span></span> <span data-ttu-id="94cac-166">Under förutsättning att du använder samma URL för båda åtgärder i **ActionRepository** class, logik app-utlösare för antingen regeln.</span><span class="sxs-lookup"><span data-stu-id="94cac-166">Assuming you used the same URL for both actions the **ActionRepository** class, your logic app triggers for either rule.</span></span> <span data-ttu-id="94cac-167">Båda reglerna använder SendGrid för att skicka ett e-postmeddelande till den **till** adress med information om aviseringen.</span><span class="sxs-lookup"><span data-stu-id="94cac-167">Both rules use SendGrid to send an email to the **To** address with details of the alert.</span></span>

> [!NOTE]
> <span data-ttu-id="94cac-168">Logikappen fortsätter att utlösa varje gång tröskeln uppfylls.</span><span class="sxs-lookup"><span data-stu-id="94cac-168">The Logic App continues to trigger every time the threshold is met.</span></span> <span data-ttu-id="94cac-169">För att undvika onödiga e-postmeddelanden, kan du antingen inaktivera reglerna i din lösning portal eller inaktivera Logikappen i den [Azure-portalen][lnk-azureportal].</span><span class="sxs-lookup"><span data-stu-id="94cac-169">To avoid unnecessary emails, you can either disable the rules in your solution portal or disable the Logic App in the [Azure portal][lnk-azureportal].</span></span>
> 
> 

<span data-ttu-id="94cac-170">Förutom e-postmeddelanden, kan du också se när Logikappen som körs i portalen:</span><span class="sxs-lookup"><span data-stu-id="94cac-170">In addition to receiving emails, you can also see when the Logic App runs in the portal:</span></span>

![](media/iot-suite-logic-apps-tutorial/logicapprun.png)

## <a name="next-steps"></a><span data-ttu-id="94cac-171">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="94cac-171">Next steps</span></span>
<span data-ttu-id="94cac-172">Nu när du har använt en Logikapp för att ansluta den förkonfigurerade lösningen till en affärsprocess, du kan lära dig mer om alternativen för att anpassa förkonfigurerade lösningar:</span><span class="sxs-lookup"><span data-stu-id="94cac-172">Now that you've used a Logic App to connect the preconfigured solution to a business process, you can learn more about the options for customizing the preconfigured solutions:</span></span>

* <span data-ttu-id="94cac-173">[Använd dynamiska telemetri med fjärråtkomst övervakning förkonfigurerade lösningen][lnk-dynamic]</span><span class="sxs-lookup"><span data-stu-id="94cac-173">[Use dynamic telemetry with the remote monitoring preconfigured solution][lnk-dynamic]</span></span>
* <span data-ttu-id="94cac-174">[Enhetens information metadata i fjärråtkomst övervakning förkonfigurerade lösningen][lnk-devinfo]</span><span class="sxs-lookup"><span data-stu-id="94cac-174">[Device information metadata in the remote monitoring preconfigured solution][lnk-devinfo]</span></span>

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
