---
title: Skapa en anpassad regel i Azure IoT Suite | Microsoft Docs
description: "Så här skapar du en anpassad regel i en IoT Suite förkonfigurerade lösning."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 562799dc-06ea-4cdd-b822-80d1f70d2f09
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: d58c27234ea05a82aaa3e8d72f70c1449980df09
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-custom-rule-in-the-remote-monitoring-preconfigured-solution"></a><span data-ttu-id="d8faf-103">Skapa en anpassad regel i fjärråtkomst övervakning förkonfigurerade lösningen</span><span class="sxs-lookup"><span data-stu-id="d8faf-103">Create a custom rule in the remote monitoring preconfigured solution</span></span>

## <a name="introduction"></a><span data-ttu-id="d8faf-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="d8faf-104">Introduction</span></span>

<span data-ttu-id="d8faf-105">Förkonfigurerade lösningar du kan konfigurera [regler som utlöses när en telemetri värdet för en enhet når ett visst tröskelvärde][lnk-builtin-rule].</span><span class="sxs-lookup"><span data-stu-id="d8faf-105">In the preconfigured solutions, you can configure [rules that trigger when a telemetry value for a device reaches a specific threshold][lnk-builtin-rule].</span></span> <span data-ttu-id="d8faf-106">[Använd dynamiska telemetri med fjärråtkomst övervakning förkonfigurerade lösningen] [ lnk-dynamic-telemetry] beskriver hur du lägger till anpassad telemetri värden som *ExternalTemperature* i lösningen.</span><span class="sxs-lookup"><span data-stu-id="d8faf-106">[Use dynamic telemetry with the remote monitoring preconfigured solution][lnk-dynamic-telemetry] describes how you can add custom telemetry values, such as *ExternalTemperature* to your solution.</span></span> <span data-ttu-id="d8faf-107">Den här artikeln visar hur du skapar en anpassad regel för dynamiskt telemetri typer i din lösning.</span><span class="sxs-lookup"><span data-stu-id="d8faf-107">This article shows you how to create custom rule for dynamic telemetry types in your solution.</span></span>

<span data-ttu-id="d8faf-108">Den här kursen använder en enkel Node.js simulerad enhet för att generera dynamiska telemetri för att skicka till förkonfigurerade lösningens serverdel.</span><span class="sxs-lookup"><span data-stu-id="d8faf-108">This tutorial uses a simple Node.js simulated device to generate dynamic telemetry to send to the preconfigured solution back end.</span></span> <span data-ttu-id="d8faf-109">Du sedan lägga till anpassade regler i den **RemoteMonitoring** Visual Studio-lösning och distribuera den här anpassade serverdel till din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="d8faf-109">You then add custom rules in the **RemoteMonitoring** Visual Studio solution and deploy this customized back end to your Azure subscription.</span></span>

<span data-ttu-id="d8faf-110">För att slutföra den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="d8faf-110">To complete this tutorial, you need:</span></span>

* <span data-ttu-id="d8faf-111">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="d8faf-111">An active Azure subscription.</span></span> <span data-ttu-id="d8faf-112">Om du inte har något konto kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="d8faf-112">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="d8faf-113">Mer information finns i [kostnadsfri utvärderingsversion av Azure][lnk_free_trial].</span><span class="sxs-lookup"><span data-stu-id="d8faf-113">For details, see [Azure Free Trial][lnk_free_trial].</span></span>
* <span data-ttu-id="d8faf-114">[Node.js] [ lnk-node] version 0.12.x eller senare för att skapa en simulerad enhet.</span><span class="sxs-lookup"><span data-stu-id="d8faf-114">[Node.js][lnk-node] version 0.12.x or later to create a simulated device.</span></span>
* <span data-ttu-id="d8faf-115">Visual Studio 2015 eller Visual Studio 2017 ändra förkonfigurerade lösningen tillbaka avslutas med din nya regler.</span><span class="sxs-lookup"><span data-stu-id="d8faf-115">Visual Studio 2015 or Visual Studio 2017 to modify the preconfigured solution back end with your new rules.</span></span>

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

<span data-ttu-id="d8faf-116">Anteckna lösningens namn som du har valt för din distribution.</span><span class="sxs-lookup"><span data-stu-id="d8faf-116">Make a note of the solution name you chose for your deployment.</span></span> <span data-ttu-id="d8faf-117">Du behöver den här lösningsnamn senare i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="d8faf-117">You need this solution name later in this tutorial.</span></span>

[!INCLUDE [iot-suite-send-external-temperature](../../includes/iot-suite-send-external-temperature.md)]

<span data-ttu-id="d8faf-118">Du kan stoppa Node.js-konsolprogram när du har verifierat att den skickar **ExternalTemperature** telemetri till förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="d8faf-118">You can stop the Node.js console app when you have verified that it is sending **ExternalTemperature** telemetry to the preconfigured solution.</span></span> <span data-ttu-id="d8faf-119">Behåll konsolfönstret öppna eftersom du kör den här Node.js-konsolprogram igen när du lägger till anpassad regel i lösningen.</span><span class="sxs-lookup"><span data-stu-id="d8faf-119">Keep the console window open because you run this Node.js console app again after you add the custom rule to the solution.</span></span>

## <a name="rule-storage-locations"></a><span data-ttu-id="d8faf-120">Lagringsplatser för regeln</span><span class="sxs-lookup"><span data-stu-id="d8faf-120">Rule storage locations</span></span>

<span data-ttu-id="d8faf-121">Information om regler sparas på två platser:</span><span class="sxs-lookup"><span data-stu-id="d8faf-121">Information about rules is persisted in two locations:</span></span>

* <span data-ttu-id="d8faf-122">**DeviceRulesNormalizedTable** tabellen – den här tabellen lagras en normaliserade referens till de regler som definierats av lösningen portalen.</span><span class="sxs-lookup"><span data-stu-id="d8faf-122">**DeviceRulesNormalizedTable** table – This table stores a normalized reference to the rules defined by the solution portal.</span></span> <span data-ttu-id="d8faf-123">När lösningen portal visas enheten regler, frågar den här tabellen för Regeldefinitioner.</span><span class="sxs-lookup"><span data-stu-id="d8faf-123">When the solution portal displays device rules, it queries this table for the rule definitions.</span></span>
* <span data-ttu-id="d8faf-124">**DeviceRules** blob – den här blob lagrar alla regler som definierats för alla registrerade enheter och har definierats som en referens som indata till Azure Stream Analytics-jobb.</span><span class="sxs-lookup"><span data-stu-id="d8faf-124">**DeviceRules** blob – This blob stores all the rules defined for all registered devices and is defined as a reference input to the Azure Stream Analytics jobs.</span></span>
 
<span data-ttu-id="d8faf-125">När du uppdaterar en befintlig regel eller definiera en ny regel i lösningen portal, har tabell- och blobbdata uppdaterats för att återspegla ändringarna.</span><span class="sxs-lookup"><span data-stu-id="d8faf-125">When you update an existing rule or define a new rule in the solution portal, both the table and blob are updated to reflect the changes.</span></span> <span data-ttu-id="d8faf-126">Regeldefinitionen visas i portalen hämtas från arkivet tabell och Regeldefinitionen refererar till Stream Analytics-jobb kommer från blob.</span><span class="sxs-lookup"><span data-stu-id="d8faf-126">The rule definition displayed in the portal comes from the table store, and the rule definition referenced by the Stream Analytics jobs comes from the blob.</span></span> 

## <a name="update-the-remotemonitoring-visual-studio-solution"></a><span data-ttu-id="d8faf-127">Uppdatera RemoteMonitoring Visual Studio-lösning</span><span class="sxs-lookup"><span data-stu-id="d8faf-127">Update the RemoteMonitoring Visual Studio solution</span></span>

<span data-ttu-id="d8faf-128">Följande steg visar hur du ändrar RemoteMonitoring Visual Studio-lösning för att inkludera en ny regel som använder den **ExternalTemperature** telemetri som skickats från den simulerade enheten:</span><span class="sxs-lookup"><span data-stu-id="d8faf-128">The following steps show you how to modify the RemoteMonitoring Visual Studio solution to include a new rule that uses the **ExternalTemperature** telemetry sent from the simulated device:</span></span>

1. <span data-ttu-id="d8faf-129">Om du inte redan har gjort det klona den **azure iot-fjärr-övervakning** databasen till en lämplig plats på den lokala datorn med följande Git-kommando:</span><span class="sxs-lookup"><span data-stu-id="d8faf-129">If you have not already done so, clone the **azure-iot-remote-monitoring** repository to a suitable location on your local machine using the following Git command:</span></span>

    ```
    git clone https://github.com/Azure/azure-iot-remote-monitoring.git
    ```

2. <span data-ttu-id="d8faf-130">Öppna filen RemoteMonitoring.sln i Visual Studio från den lokala kopian av den **azure iot-fjärr-övervakning** databasen.</span><span class="sxs-lookup"><span data-stu-id="d8faf-130">In Visual Studio, open the RemoteMonitoring.sln file from your local copy of the **azure-iot-remote-monitoring** repository.</span></span>

3. <span data-ttu-id="d8faf-131">Öppna filen Infrastructure\Models\DeviceRuleBlobEntity.cs och lägga till en **ExternalTemperature** egenskapen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="d8faf-131">Open the file Infrastructure\Models\DeviceRuleBlobEntity.cs and add an **ExternalTemperature** property as follows:</span></span>

    ```csharp
    public double? Temperature { get; set; }
    public double? Humidity { get; set; }
    public double? ExternalTemperature { get; set; }
    ```

4. <span data-ttu-id="d8faf-132">Lägg till i samma fil, en **ExternalTemperatureRuleOutput** egenskapen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="d8faf-132">In the same file, add an **ExternalTemperatureRuleOutput** property as follows:</span></span>

    ```csharp
    public string TemperatureRuleOutput { get; set; }
    public string HumidityRuleOutput { get; set; }
    public string ExternalTemperatureRuleOutput { get; set; }
    ```

5. <span data-ttu-id="d8faf-133">Öppna filen Infrastructure\Models\DeviceRuleDataFields.cs och Lägg till följande **ExternalTemperature** egenskapen när den befintliga **fuktighet** egenskapen:</span><span class="sxs-lookup"><span data-stu-id="d8faf-133">Open the file Infrastructure\Models\DeviceRuleDataFields.cs and add the following **ExternalTemperature** property after the existing **Humidity** property:</span></span>

    ```csharp
    public static string ExternalTemperature
    {
        get { return "ExternalTemperature"; }
    }
    ```

6. <span data-ttu-id="d8faf-134">Uppdatera i samma fil i **_availableDataFields** metod för att inkludera **ExternalTemperature** på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="d8faf-134">In the same file, update the **_availableDataFields** method to include **ExternalTemperature** as follows:</span></span>

    ```csharp
    private static List<string> _availableDataFields = new List<string>
    {                    
        Temperature, Humidity, ExternalTemperature
    };
    ```

7. <span data-ttu-id="d8faf-135">Öppna filen Infrastructure\Repository\DeviceRulesRepository.cs och ändra den **BuildBlobEntityListFromTableRows** metoden på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="d8faf-135">Open the file Infrastructure\Repository\DeviceRulesRepository.cs and modify the **BuildBlobEntityListFromTableRows** method as follows:</span></span>

    ```csharp
    else if (rule.DataField == DeviceRuleDataFields.Humidity)
    {
        entity.Humidity = rule.Threshold;
        entity.HumidityRuleOutput = rule.RuleOutput;
    }
    else if (rule.DataField == DeviceRuleDataFields.ExternalTemperature)
    {
      entity.ExternalTemperature = rule.Threshold;
      entity.ExternalTemperatureRuleOutput = rule.RuleOutput;
    }
    ```

## <a name="rebuild-and-redeploy-the-solution"></a><span data-ttu-id="d8faf-136">Återskapa och distribuera lösningen igen.</span><span class="sxs-lookup"><span data-stu-id="d8faf-136">Rebuild and redeploy the solution.</span></span>

<span data-ttu-id="d8faf-137">Du kan nu distribuera den uppdaterade lösningen till din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="d8faf-137">You can now deploy the updated solution to your Azure subscription.</span></span>

1. <span data-ttu-id="d8faf-138">Öppna en upphöjd kommandotolk och navigera till roten i den lokala kopian av databasen azure iot-fjärr-övervakning.</span><span class="sxs-lookup"><span data-stu-id="d8faf-138">Open an elevated command prompt and navigate to the root of your local copy of the azure-iot-remote-monitoring repository.</span></span>

2. <span data-ttu-id="d8faf-139">För att distribuera din uppdaterade lösning, kör du följande kommando ersätter **{distributionsnamnet}** med namnet på din förkonfigurerade lösningsdistribution som du antecknade tidigare:</span><span class="sxs-lookup"><span data-stu-id="d8faf-139">To deploy your updated solution, run the following command substituting **{deployment name}** with the name of your preconfigured solution deployment that you noted previously:</span></span>

    ```
    build.cmd cloud release {deployment name}
    ```

## <a name="update-the-stream-analytics-job"></a><span data-ttu-id="d8faf-140">Uppdatera Stream Analytics-jobbet</span><span class="sxs-lookup"><span data-stu-id="d8faf-140">Update the Stream Analytics job</span></span>

<span data-ttu-id="d8faf-141">När installationen är klar, kan du uppdatera Stream Analytics-jobbet om du vill använda de nya definitionerna.</span><span class="sxs-lookup"><span data-stu-id="d8faf-141">When the deployment is complete, you can update the Stream Analytics job to use the new rule definitions.</span></span>

1. <span data-ttu-id="d8faf-142">Navigera till den resursgrupp som innehåller din förkonfigurerade lösning resurser i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d8faf-142">In the Azure portal, navigate to the resource group that contains your preconfigured solution resources.</span></span> <span data-ttu-id="d8faf-143">Den här resursgruppen har samma namn som du angav för lösningen under distributionen.</span><span class="sxs-lookup"><span data-stu-id="d8faf-143">This resource group has the same name you specified for the solution during the deployment.</span></span>

2. <span data-ttu-id="d8faf-144">Navigera till {distributionsnamnet}-regler Stream Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="d8faf-144">Navigate to the {deployment name}-Rules Stream Analytics job.</span></span> 

3. <span data-ttu-id="d8faf-145">Klicka på **stoppa** att stoppa Stream Analytics-jobbet körs.</span><span class="sxs-lookup"><span data-stu-id="d8faf-145">Click **Stop** to stop the Stream Analytics job from running.</span></span> <span data-ttu-id="d8faf-146">(Du måste vänta tills direktuppspelningsjobbet att stoppa innan du kan redigera frågan).</span><span class="sxs-lookup"><span data-stu-id="d8faf-146">(You must wait for the streaming job to stop before you can edit the query).</span></span>

4. <span data-ttu-id="d8faf-147">Klicka på **frågan**.</span><span class="sxs-lookup"><span data-stu-id="d8faf-147">Click **Query**.</span></span> <span data-ttu-id="d8faf-148">Redigera en fråga som inkluderar den **Välj** -uttrycket för **ExternalTemperature**.</span><span class="sxs-lookup"><span data-stu-id="d8faf-148">Edit the query to include the **SELECT** statement for **ExternalTemperature**.</span></span> <span data-ttu-id="d8faf-149">I följande exempel visar slutförd fråga med det nya **Välj** instruktionen:</span><span class="sxs-lookup"><span data-stu-id="d8faf-149">The following sample shows the complete query with the new **SELECT** statement:</span></span>

    ```
    WITH AlarmsData AS 
    (
    SELECT
         Stream.IoTHub.ConnectionDeviceId AS DeviceId,
         'Temperature' as ReadingType,
         Stream.Temperature as Reading,
         Ref.Temperature as Threshold,
         Ref.TemperatureRuleOutput as RuleOutput,
         Stream.EventEnqueuedUtcTime AS [Time]
    FROM IoTTelemetryStream Stream
    JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
    WHERE
         Ref.Temperature IS NOT null AND Stream.Temperature > Ref.Temperature
     
    UNION ALL
     
    SELECT
         Stream.IoTHub.ConnectionDeviceId AS DeviceId,
         'Humidity' as ReadingType,
         Stream.Humidity as Reading,
         Ref.Humidity as Threshold,
         Ref.HumidityRuleOutput as RuleOutput,
         Stream.EventEnqueuedUtcTime AS [Time]
    FROM IoTTelemetryStream Stream
    JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
    WHERE
         Ref.Humidity IS NOT null AND Stream.Humidity > Ref.Humidity
     
    UNION ALL
     
    SELECT
         Stream.IoTHub.ConnectionDeviceId AS DeviceId,
         'ExternalTemperature' as ReadingType,
         Stream.ExternalTemperature as Reading,
         Ref.ExternalTemperature as Threshold,
         Ref.ExternalTemperatureRuleOutput as RuleOutput,
         Stream.EventEnqueuedUtcTime AS [Time]
    FROM IoTTelemetryStream Stream
    JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
    WHERE
         Ref.ExternalTemperature IS NOT null AND Stream.ExternalTemperature > Ref.ExternalTemperature
    )
     
    SELECT *
    INTO DeviceRulesMonitoring
    FROM AlarmsData
     
    SELECT *
    INTO DeviceRulesHub
    FROM AlarmsData
    ```

5. <span data-ttu-id="d8faf-150">Klicka på **spara** att ändra frågan uppdaterade regeln.</span><span class="sxs-lookup"><span data-stu-id="d8faf-150">Click **Save** to change the updated rule query.</span></span>

6. <span data-ttu-id="d8faf-151">Klicka på **starta** att starta Stream Analytics-jobbet körs igen.</span><span class="sxs-lookup"><span data-stu-id="d8faf-151">Click **Start** to start the Stream Analytics job running again.</span></span>

## <a name="add-your-new-rule-in-the-dashboard"></a><span data-ttu-id="d8faf-152">Lägg till din nya regel i instrumentpanelen</span><span class="sxs-lookup"><span data-stu-id="d8faf-152">Add your new rule in the dashboard</span></span>

<span data-ttu-id="d8faf-153">Du kan nu lägga till den **ExternalTemperature** regeln till en enhet i instrumentpanelen för lösningen.</span><span class="sxs-lookup"><span data-stu-id="d8faf-153">You can now add the **ExternalTemperature** rule to a device in the solution dashboard.</span></span>

1. <span data-ttu-id="d8faf-154">Gå till portalen för lösningen.</span><span class="sxs-lookup"><span data-stu-id="d8faf-154">Navigate to the solution portal.</span></span>

2. <span data-ttu-id="d8faf-155">Navigera till den **enheter** panelen.</span><span class="sxs-lookup"><span data-stu-id="d8faf-155">Navigate to the **Devices** panel.</span></span>

3. <span data-ttu-id="d8faf-156">Leta upp den anpassa enhet som du skapade som skickar **ExternalTemperature** telemetri och på den **enhetsinformation** klickar du på **Lägg till regel**.</span><span class="sxs-lookup"><span data-stu-id="d8faf-156">Locate the custom device you created that sends **ExternalTemperature** telemetry and on the **Device Details** panel, click **Add Rule**.</span></span>

4. <span data-ttu-id="d8faf-157">Välj **ExternalTemperature** i **datafält**.</span><span class="sxs-lookup"><span data-stu-id="d8faf-157">Select **ExternalTemperature** in **Data Field**.</span></span>

5. <span data-ttu-id="d8faf-158">Ange **tröskelvärdet** -56.</span><span class="sxs-lookup"><span data-stu-id="d8faf-158">Set **Threshold** to 56.</span></span> <span data-ttu-id="d8faf-159">Klicka på **spara och visa regler**.</span><span class="sxs-lookup"><span data-stu-id="d8faf-159">Then click **Save and view rules**.</span></span>

6. <span data-ttu-id="d8faf-160">Gå tillbaka till instrumentpanelen för att visa larm historiken.</span><span class="sxs-lookup"><span data-stu-id="d8faf-160">Return to the dashboard to view the alarm history.</span></span>

7. <span data-ttu-id="d8faf-161">I konsolträdet du lämnat öppna börjar Node.js-konsolprogram om du vill börja skicka **ExternalTemperature** telemetridata.</span><span class="sxs-lookup"><span data-stu-id="d8faf-161">In the console window you left open, start the Node.js console app to begin sending **ExternalTemperature** telemetry data.</span></span>

8. <span data-ttu-id="d8faf-162">Observera att den **larm historik** tabell visas nya larm när den nya regeln utlöses.</span><span class="sxs-lookup"><span data-stu-id="d8faf-162">Notice that the **Alarm History** table shows new alarms when the new rule is triggered.</span></span>
 
## <a name="additional-information"></a><span data-ttu-id="d8faf-163">Ytterligare information</span><span class="sxs-lookup"><span data-stu-id="d8faf-163">Additional information</span></span>

<span data-ttu-id="d8faf-164">Ändra operatorn  **>**  är mer komplexa och utöver de steg som beskrivs i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="d8faf-164">Changing the operator **>** is more complex and goes beyond the steps outlined in this tutorial.</span></span> <span data-ttu-id="d8faf-165">Du kan ändra Stream Analytics-jobbet om du vill använda den operator som du vill, avspeglar den operatorn i lösningen portal är en mer komplicerad uppgift.</span><span class="sxs-lookup"><span data-stu-id="d8faf-165">While you can change the Stream Analytics job to use whatever operator you like, reflecting that operator in the solution portal is a more complex task.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="d8faf-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d8faf-166">Next steps</span></span>
<span data-ttu-id="d8faf-167">Nu när du har lärt dig hur du skapar egna regler, kan du lära dig mer om förkonfigurerade lösningar:</span><span class="sxs-lookup"><span data-stu-id="d8faf-167">Now that you've seen how to create custom rules, you can learn more about the preconfigured solutions:</span></span>

- <span data-ttu-id="d8faf-168">[Ansluta Logikappen i Azure IoT Suite Fjärrövervaknings förkonfigurerade lösningen][lnk-logic-app]</span><span class="sxs-lookup"><span data-stu-id="d8faf-168">[Connect Logic App to your Azure IoT Suite Remote Monitoring preconfigured solution][lnk-logic-app]</span></span>
- <span data-ttu-id="d8faf-169">[Enhetens information metadata för fjärranslutna övervakningen förkonfigurerade lösningen][lnk-devinfo].</span><span class="sxs-lookup"><span data-stu-id="d8faf-169">[Device information metadata in the remote monitoring preconfigured solution][lnk-devinfo].</span></span>

[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
[lnk-builtin-rule]: iot-suite-getstarted-preconfigured-solutions.md#view-alarms
[lnk-dynamic-telemetry]: iot-suite-dynamic-telemetry.md
[lnk-logic-app]: iot-suite-logic-apps-tutorial.md