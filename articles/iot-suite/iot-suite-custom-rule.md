---
title: aaaCreate en anpassad regel i Azure IoT Suite | Microsoft Docs
description: "Hur toocreate en anpassad regel i en IoT Suite förkonfigurerade lösningen."
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
ms.openlocfilehash: 6c5bb2ca54f3f17b99ad482e727c8e9fa28d7fe5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-rule-in-hello-remote-monitoring-preconfigured-solution"></a><span data-ttu-id="f4132-103">Skapa en anpassad regel i hello remote förkonfigurerade övervakningslösning</span><span class="sxs-lookup"><span data-stu-id="f4132-103">Create a custom rule in hello remote monitoring preconfigured solution</span></span>

## <a name="introduction"></a><span data-ttu-id="f4132-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="f4132-104">Introduction</span></span>

<span data-ttu-id="f4132-105">Hello förkonfigurerade lösningar du kan konfigurera [regler som utlöses när en telemetri värdet för en enhet når ett visst tröskelvärde][lnk-builtin-rule].</span><span class="sxs-lookup"><span data-stu-id="f4132-105">In hello preconfigured solutions, you can configure [rules that trigger when a telemetry value for a device reaches a specific threshold][lnk-builtin-rule].</span></span> <span data-ttu-id="f4132-106">[Använda dynamisk telemetri med hello remote förkonfigurerade övervakningslösning] [ lnk-dynamic-telemetry] beskriver hur du lägger till anpassad telemetri värden som *ExternalTemperature* tooyour lösning.</span><span class="sxs-lookup"><span data-stu-id="f4132-106">[Use dynamic telemetry with hello remote monitoring preconfigured solution][lnk-dynamic-telemetry] describes how you can add custom telemetry values, such as *ExternalTemperature* tooyour solution.</span></span> <span data-ttu-id="f4132-107">Den här artikeln visar hur toocreate anpassad regel för dynamiskt telemetri typer i din lösning.</span><span class="sxs-lookup"><span data-stu-id="f4132-107">This article shows you how toocreate custom rule for dynamic telemetry types in your solution.</span></span>

<span data-ttu-id="f4132-108">Den här kursen använder en enkel Node.js simulerade enheten toogenerate dynamiska telemetri toosend toohello förkonfigurerade lösningens serverdel.</span><span class="sxs-lookup"><span data-stu-id="f4132-108">This tutorial uses a simple Node.js simulated device toogenerate dynamic telemetry toosend toohello preconfigured solution back end.</span></span> <span data-ttu-id="f4132-109">Lägg till anpassade regler sedan i hello **RemoteMonitoring** Visual Studio-lösning och distribuera den här anpassade serverdel tooyour Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f4132-109">You then add custom rules in hello **RemoteMonitoring** Visual Studio solution and deploy this customized back end tooyour Azure subscription.</span></span>

<span data-ttu-id="f4132-110">toocomplete den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="f4132-110">toocomplete this tutorial, you need:</span></span>

* <span data-ttu-id="f4132-111">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f4132-111">An active Azure subscription.</span></span> <span data-ttu-id="f4132-112">Om du inte har något konto kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="f4132-112">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="f4132-113">Mer information finns i [kostnadsfri utvärderingsversion av Azure][lnk_free_trial].</span><span class="sxs-lookup"><span data-stu-id="f4132-113">For details, see [Azure Free Trial][lnk_free_trial].</span></span>
* <span data-ttu-id="f4132-114">[Node.js] [ lnk-node] version 0.12.x eller senare toocreate en simulerad enhet.</span><span class="sxs-lookup"><span data-stu-id="f4132-114">[Node.js][lnk-node] version 0.12.x or later toocreate a simulated device.</span></span>
* <span data-ttu-id="f4132-115">Visual Studio 2015 eller Visual Studio 2017 toomodify hello förkonfigurerade lösningen avslutas med din nya regler.</span><span class="sxs-lookup"><span data-stu-id="f4132-115">Visual Studio 2015 or Visual Studio 2017 toomodify hello preconfigured solution back end with your new rules.</span></span>

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

<span data-ttu-id="f4132-116">Anteckna hello lösningens namn som du har valt för din distribution.</span><span class="sxs-lookup"><span data-stu-id="f4132-116">Make a note of hello solution name you chose for your deployment.</span></span> <span data-ttu-id="f4132-117">Du behöver den här lösningsnamn senare i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="f4132-117">You need this solution name later in this tutorial.</span></span>

[!INCLUDE [iot-suite-send-external-temperature](../../includes/iot-suite-send-external-temperature.md)]

<span data-ttu-id="f4132-118">Du kan stoppa hello Node.js-konsolprogram när du har verifierat att den skickar **ExternalTemperature** telemetri toohello förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="f4132-118">You can stop hello Node.js console app when you have verified that it is sending **ExternalTemperature** telemetry toohello preconfigured solution.</span></span> <span data-ttu-id="f4132-119">Behåll hello konsolfönstret öppna eftersom du kör den här Node.js-konsolprogram igen när du har lagt till hello anpassad regel toohello lösning.</span><span class="sxs-lookup"><span data-stu-id="f4132-119">Keep hello console window open because you run this Node.js console app again after you add hello custom rule toohello solution.</span></span>

## <a name="rule-storage-locations"></a><span data-ttu-id="f4132-120">Lagringsplatser för regeln</span><span class="sxs-lookup"><span data-stu-id="f4132-120">Rule storage locations</span></span>

<span data-ttu-id="f4132-121">Information om regler sparas på två platser:</span><span class="sxs-lookup"><span data-stu-id="f4132-121">Information about rules is persisted in two locations:</span></span>

* <span data-ttu-id="f4132-122">**DeviceRulesNormalizedTable** tabellen – den här tabellen lagras en normaliserade referera toohello regler som definierats av hello lösning portalen.</span><span class="sxs-lookup"><span data-stu-id="f4132-122">**DeviceRulesNormalizedTable** table – This table stores a normalized reference toohello rules defined by hello solution portal.</span></span> <span data-ttu-id="f4132-123">När hello lösning portal visas enheten regler, frågar den här tabellen för hello definitioner.</span><span class="sxs-lookup"><span data-stu-id="f4132-123">When hello solution portal displays device rules, it queries this table for hello rule definitions.</span></span>
* <span data-ttu-id="f4132-124">**DeviceRules** blob – den här blob lagrar alla hello regler som definierats för alla registrerade enheter och har definierats som en referens inkommande toohello Azure Stream Analytics-jobb.</span><span class="sxs-lookup"><span data-stu-id="f4132-124">**DeviceRules** blob – This blob stores all hello rules defined for all registered devices and is defined as a reference input toohello Azure Stream Analytics jobs.</span></span>
 
<span data-ttu-id="f4132-125">När du uppdaterar en befintlig regel eller definiera en ny regel i hello lösning portal är hello tabell- och blobbdata uppdaterade tooreflect hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="f4132-125">When you update an existing rule or define a new rule in hello solution portal, both hello table and blob are updated tooreflect hello changes.</span></span> <span data-ttu-id="f4132-126">hello regeln definition visas i hello portal kommer från hello tabell store och hello regeln definition som refereras av hello Stream Analytics-jobb kommer från hello-blob.</span><span class="sxs-lookup"><span data-stu-id="f4132-126">hello rule definition displayed in hello portal comes from hello table store, and hello rule definition referenced by hello Stream Analytics jobs comes from hello blob.</span></span> 

## <a name="update-hello-remotemonitoring-visual-studio-solution"></a><span data-ttu-id="f4132-127">Uppdatera hello RemoteMonitoring Visual Studio-lösning</span><span class="sxs-lookup"><span data-stu-id="f4132-127">Update hello RemoteMonitoring Visual Studio solution</span></span>

<span data-ttu-id="f4132-128">hello följande steg visar hur hello toomodify RemoteMonitoring Visual Studio-lösning tooinclude en ny regel som använder hello **ExternalTemperature** telemetri som skickas från hello simulerade enheten:</span><span class="sxs-lookup"><span data-stu-id="f4132-128">hello following steps show you how toomodify hello RemoteMonitoring Visual Studio solution tooinclude a new rule that uses hello **ExternalTemperature** telemetry sent from hello simulated device:</span></span>

1. <span data-ttu-id="f4132-129">Om du inte redan har gjort det klonade hello **azure iot-fjärr-övervakning** lämplig plats på den lokala datorn med hjälp av följande kommando i Git hello tooa i databasen:</span><span class="sxs-lookup"><span data-stu-id="f4132-129">If you have not already done so, clone hello **azure-iot-remote-monitoring** repository tooa suitable location on your local machine using hello following Git command:</span></span>

    ```
    git clone https://github.com/Azure/azure-iot-remote-monitoring.git
    ```

2. <span data-ttu-id="f4132-130">I Visual Studio öppnas hello RemoteMonitoring.sln från den lokala kopian av hello **azure iot-fjärr-övervakning** databasen.</span><span class="sxs-lookup"><span data-stu-id="f4132-130">In Visual Studio, open hello RemoteMonitoring.sln file from your local copy of hello **azure-iot-remote-monitoring** repository.</span></span>

3. <span data-ttu-id="f4132-131">Öppna filen hello Infrastructure\Models\DeviceRuleBlobEntity.cs och Lägg till en **ExternalTemperature** egenskapen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="f4132-131">Open hello file Infrastructure\Models\DeviceRuleBlobEntity.cs and add an **ExternalTemperature** property as follows:</span></span>

    ```csharp
    public double? Temperature { get; set; }
    public double? Humidity { get; set; }
    public double? ExternalTemperature { get; set; }
    ```

4. <span data-ttu-id="f4132-132">Hej samma fil, lägga till i en **ExternalTemperatureRuleOutput** egenskapen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="f4132-132">In hello same file, add an **ExternalTemperatureRuleOutput** property as follows:</span></span>

    ```csharp
    public string TemperatureRuleOutput { get; set; }
    public string HumidityRuleOutput { get; set; }
    public string ExternalTemperatureRuleOutput { get; set; }
    ```

5. <span data-ttu-id="f4132-133">Öppna filen hello Infrastructure\Models\DeviceRuleDataFields.cs och Lägg till följande hello **ExternalTemperature** egenskapen efter hello befintliga **fuktighet** egenskapen:</span><span class="sxs-lookup"><span data-stu-id="f4132-133">Open hello file Infrastructure\Models\DeviceRuleDataFields.cs and add hello following **ExternalTemperature** property after hello existing **Humidity** property:</span></span>

    ```csharp
    public static string ExternalTemperature
    {
        get { return "ExternalTemperature"; }
    }
    ```

6. <span data-ttu-id="f4132-134">I Hej samma fil, uppdatera hello **_availableDataFields** metoden tooinclude **ExternalTemperature** på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="f4132-134">In hello same file, update hello **_availableDataFields** method tooinclude **ExternalTemperature** as follows:</span></span>

    ```csharp
    private static List<string> _availableDataFields = new List<string>
    {                    
        Temperature, Humidity, ExternalTemperature
    };
    ```

7. <span data-ttu-id="f4132-135">Öppna filen hello Infrastructure\Repository\DeviceRulesRepository.cs och ändra hello **BuildBlobEntityListFromTableRows** metoden på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="f4132-135">Open hello file Infrastructure\Repository\DeviceRulesRepository.cs and modify hello **BuildBlobEntityListFromTableRows** method as follows:</span></span>

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

## <a name="rebuild-and-redeploy-hello-solution"></a><span data-ttu-id="f4132-136">Återskapa och omdistribuera hello lösning.</span><span class="sxs-lookup"><span data-stu-id="f4132-136">Rebuild and redeploy hello solution.</span></span>

<span data-ttu-id="f4132-137">Du kan nu distribuera hello uppdateras lösning tooyour Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f4132-137">You can now deploy hello updated solution tooyour Azure subscription.</span></span>

1. <span data-ttu-id="f4132-138">Öppna en upphöjd kommandotolk och navigera toohello roten för den lokala kopian av databasen för hello azure iot-fjärr-övervakning.</span><span class="sxs-lookup"><span data-stu-id="f4132-138">Open an elevated command prompt and navigate toohello root of your local copy of hello azure-iot-remote-monitoring repository.</span></span>

2. <span data-ttu-id="f4132-139">toodeploy uppdaterade lösningen, kör följande kommando ersätter hello **{distributionsnamnet}** med hello namnet på din förkonfigurerade lösningsdistribution som du antecknade tidigare:</span><span class="sxs-lookup"><span data-stu-id="f4132-139">toodeploy your updated solution, run hello following command substituting **{deployment name}** with hello name of your preconfigured solution deployment that you noted previously:</span></span>

    ```
    build.cmd cloud release {deployment name}
    ```

## <a name="update-hello-stream-analytics-job"></a><span data-ttu-id="f4132-140">Uppdatera hello Stream Analytics-jobbet</span><span class="sxs-lookup"><span data-stu-id="f4132-140">Update hello Stream Analytics job</span></span>

<span data-ttu-id="f4132-141">När hello distributionen är klar, kan du uppdatera hello Stream Analytics-jobbet toouse hello nya definitioner.</span><span class="sxs-lookup"><span data-stu-id="f4132-141">When hello deployment is complete, you can update hello Stream Analytics job toouse hello new rule definitions.</span></span>

1. <span data-ttu-id="f4132-142">Navigera toohello resursgruppen som innehåller din förkonfigurerade lösning resurser i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f4132-142">In hello Azure portal, navigate toohello resource group that contains your preconfigured solution resources.</span></span> <span data-ttu-id="f4132-143">Den här resursgruppen har samma namn du angav för hello hello lösningen under hello distributionen.</span><span class="sxs-lookup"><span data-stu-id="f4132-143">This resource group has hello same name you specified for hello solution during hello deployment.</span></span>

2. <span data-ttu-id="f4132-144">Navigera toohello {distributionsnamnet}-regler Stream Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="f4132-144">Navigate toohello {deployment name}-Rules Stream Analytics job.</span></span> 

3. <span data-ttu-id="f4132-145">Klicka på **stoppa** toostop hello Stream Analytics-jobbet körs.</span><span class="sxs-lookup"><span data-stu-id="f4132-145">Click **Stop** toostop hello Stream Analytics job from running.</span></span> <span data-ttu-id="f4132-146">(Du måste vänta tills hello streaming job toostop innan du kan redigera hello fråga).</span><span class="sxs-lookup"><span data-stu-id="f4132-146">(You must wait for hello streaming job toostop before you can edit hello query).</span></span>

4. <span data-ttu-id="f4132-147">Klicka på **frågan**.</span><span class="sxs-lookup"><span data-stu-id="f4132-147">Click **Query**.</span></span> <span data-ttu-id="f4132-148">Redigera hello frågan tooinclude hello **Välj** -uttrycket för **ExternalTemperature**.</span><span class="sxs-lookup"><span data-stu-id="f4132-148">Edit hello query tooinclude hello **SELECT** statement for **ExternalTemperature**.</span></span> <span data-ttu-id="f4132-149">hello följande exempel visar hello fullständig fråga med hello nya **Välj** instruktionen:</span><span class="sxs-lookup"><span data-stu-id="f4132-149">hello following sample shows hello complete query with hello new **SELECT** statement:</span></span>

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

5. <span data-ttu-id="f4132-150">Klicka på **spara** toochange hello uppdatera regel för frågan.</span><span class="sxs-lookup"><span data-stu-id="f4132-150">Click **Save** toochange hello updated rule query.</span></span>

6. <span data-ttu-id="f4132-151">Klicka på **starta** toostart hello Stream Analytics-jobbet körs igen.</span><span class="sxs-lookup"><span data-stu-id="f4132-151">Click **Start** toostart hello Stream Analytics job running again.</span></span>

## <a name="add-your-new-rule-in-hello-dashboard"></a><span data-ttu-id="f4132-152">Lägg till din nya regel hello instrumentpanelen</span><span class="sxs-lookup"><span data-stu-id="f4132-152">Add your new rule in hello dashboard</span></span>

<span data-ttu-id="f4132-153">Nu kan du lägga till hello **ExternalTemperature** regeln tooa enhet i hello lösning instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="f4132-153">You can now add hello **ExternalTemperature** rule tooa device in hello solution dashboard.</span></span>

1. <span data-ttu-id="f4132-154">Navigera toohello lösning portal.</span><span class="sxs-lookup"><span data-stu-id="f4132-154">Navigate toohello solution portal.</span></span>

2. <span data-ttu-id="f4132-155">Navigera toohello **enheter** panelen.</span><span class="sxs-lookup"><span data-stu-id="f4132-155">Navigate toohello **Devices** panel.</span></span>

3. <span data-ttu-id="f4132-156">Leta upp hello anpassade du skapade som skickar **ExternalTemperature** telemetri och på hello **enhetsinformation** klickar du på **Lägg till regel**.</span><span class="sxs-lookup"><span data-stu-id="f4132-156">Locate hello custom device you created that sends **ExternalTemperature** telemetry and on hello **Device Details** panel, click **Add Rule**.</span></span>

4. <span data-ttu-id="f4132-157">Välj **ExternalTemperature** i **datafält**.</span><span class="sxs-lookup"><span data-stu-id="f4132-157">Select **ExternalTemperature** in **Data Field**.</span></span>

5. <span data-ttu-id="f4132-158">Ange **tröskelvärdet** too56.</span><span class="sxs-lookup"><span data-stu-id="f4132-158">Set **Threshold** too56.</span></span> <span data-ttu-id="f4132-159">Klicka på **spara och visa regler**.</span><span class="sxs-lookup"><span data-stu-id="f4132-159">Then click **Save and view rules**.</span></span>

6. <span data-ttu-id="f4132-160">Returnera toohello instrumentpanelen tooview hello larm historik.</span><span class="sxs-lookup"><span data-stu-id="f4132-160">Return toohello dashboard tooview hello alarm history.</span></span>

7. <span data-ttu-id="f4132-161">Hello konsolfönstret du lämnat öppna börja hello Node.js konsolen app toobegin skicka **ExternalTemperature** telemetridata.</span><span class="sxs-lookup"><span data-stu-id="f4132-161">In hello console window you left open, start hello Node.js console app toobegin sending **ExternalTemperature** telemetry data.</span></span>

8. <span data-ttu-id="f4132-162">Observera att hello **larm historik** tabell visas nya larm när hello ny regel utlöses.</span><span class="sxs-lookup"><span data-stu-id="f4132-162">Notice that hello **Alarm History** table shows new alarms when hello new rule is triggered.</span></span>
 
## <a name="additional-information"></a><span data-ttu-id="f4132-163">Ytterligare information</span><span class="sxs-lookup"><span data-stu-id="f4132-163">Additional information</span></span>

<span data-ttu-id="f4132-164">Ändra hello operatorn  **>**  är mer komplexa och utöver hello steg som beskrivs i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="f4132-164">Changing hello operator **>** is more complex and goes beyond hello steps outlined in this tutorial.</span></span> <span data-ttu-id="f4132-165">Du kan ändra hello Stream Analytics-jobbet toouse oavsett operator som du vill, avspeglar den operatorn i hello lösning portal är en mer komplicerad uppgift.</span><span class="sxs-lookup"><span data-stu-id="f4132-165">While you can change hello Stream Analytics job toouse whatever operator you like, reflecting that operator in hello solution portal is a more complex task.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="f4132-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f4132-166">Next steps</span></span>
<span data-ttu-id="f4132-167">Nu när du har sett hur toocreate anpassade regler, du kan lära dig mer om hello förkonfigurerade lösningar:</span><span class="sxs-lookup"><span data-stu-id="f4132-167">Now that you've seen how toocreate custom rules, you can learn more about hello preconfigured solutions:</span></span>

- <span data-ttu-id="f4132-168">[Ansluta Logikapp tooyour Azure IoT Suite Fjärrövervaknings förkonfigurerade lösningen][lnk-logic-app]</span><span class="sxs-lookup"><span data-stu-id="f4132-168">[Connect Logic App tooyour Azure IoT Suite Remote Monitoring preconfigured solution][lnk-logic-app]</span></span>
- <span data-ttu-id="f4132-169">[Enhetens information metadata i hello fjärrövervaknings förkonfigurerade lösningen][lnk-devinfo].</span><span class="sxs-lookup"><span data-stu-id="f4132-169">[Device information metadata in hello remote monitoring preconfigured solution][lnk-devinfo].</span></span>

[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
[lnk-builtin-rule]: iot-suite-getstarted-preconfigured-solutions.md#view-alarms
[lnk-dynamic-telemetry]: iot-suite-dynamic-telemetry.md
[lnk-logic-app]: iot-suite-logic-apps-tutorial.md