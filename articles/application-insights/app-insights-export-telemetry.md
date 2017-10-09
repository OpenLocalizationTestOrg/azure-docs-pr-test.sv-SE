---
title: "aaaContinuous export av telemetri från Application Insights | Microsoft Docs"
description: "Exportera diagnostik- och användningsdata data toostorage i Microsoft Azure och ladda ned den därifrån."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 5b859200-b484-4c98-9d9f-929713f1030c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 02/23/2017
ms.author: bwren
ms.openlocfilehash: be9ed7e05922c1c8186df9ca4e642862adaa5fd0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="export-telemetry-from-application-insights"></a><span data-ttu-id="76527-103">Exportera telemetri från Application Insights</span><span class="sxs-lookup"><span data-stu-id="76527-103">Export telemetry from Application Insights</span></span>
<span data-ttu-id="76527-104">Ska tookeep din telemetri för längre än hello standard kvarhållningsperiod?</span><span class="sxs-lookup"><span data-stu-id="76527-104">Want tookeep your telemetry for longer than hello standard retention period?</span></span> <span data-ttu-id="76527-105">Eller bearbeta några särskilda sätt?</span><span class="sxs-lookup"><span data-stu-id="76527-105">Or process it in some specialized way?</span></span> <span data-ttu-id="76527-106">Löpande Export är idealisk för detta.</span><span class="sxs-lookup"><span data-stu-id="76527-106">Continuous Export is ideal for this.</span></span> <span data-ttu-id="76527-107">hello-händelser som du ser i hello Application Insights-portalen kan vara exporterade toostorage i Microsoft Azure i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="76527-107">hello events you see in hello Application Insights portal can be exported toostorage in Microsoft Azure in JSON format.</span></span> <span data-ttu-id="76527-108">Därifrån kan du hämta data och skriva oavsett kod som du behöver tooprocess den.</span><span class="sxs-lookup"><span data-stu-id="76527-108">From there you can download your data and write whatever code you need tooprocess it.</span></span>  

<span data-ttu-id="76527-109">Med hjälp av löpande Export kan innebära en extra kostnad.</span><span class="sxs-lookup"><span data-stu-id="76527-109">Using Continuous Export may incur an additional charge.</span></span> <span data-ttu-id="76527-110">Kontrollera din [Prismodell](http://azure.microsoft.com/pricing/details/application-insights/).</span><span class="sxs-lookup"><span data-stu-id="76527-110">Check your [pricing model](http://azure.microsoft.com/pricing/details/application-insights/).</span></span>

<span data-ttu-id="76527-111">Innan du konfigurerar löpande export finns det några alternativ som du kanske vill tooconsider:</span><span class="sxs-lookup"><span data-stu-id="76527-111">Before you set up continuous export, there are some alternatives you might want tooconsider:</span></span>

* <span data-ttu-id="76527-112">hello exportera-knappen hello överst på ett mått eller Sök blad kan du överföra tabeller och diagram tooan Excel-kalkylblad.</span><span class="sxs-lookup"><span data-stu-id="76527-112">hello Export button at hello top of a metrics or search blade lets you transfer tables and charts tooan Excel spreadsheet.</span></span>

* <span data-ttu-id="76527-113">[Analytics](app-insights-analytics.md) tillhandahåller kraftfulla frågespråk för telemetri.</span><span class="sxs-lookup"><span data-stu-id="76527-113">[Analytics](app-insights-analytics.md) provides a powerful query language for telemetry.</span></span> <span data-ttu-id="76527-114">Det kan också exportera resultaten.</span><span class="sxs-lookup"><span data-stu-id="76527-114">It can also export results.</span></span>
* <span data-ttu-id="76527-115">Om du vill ha för[Utforska dina data i Power BI](app-insights-export-power-bi.md), kan du göra det utan att använda löpande Export.</span><span class="sxs-lookup"><span data-stu-id="76527-115">If you're looking too[explore your data in Power BI](app-insights-export-power-bi.md), you can do that without using Continuous Export.</span></span>
* <span data-ttu-id="76527-116">Hej [REST API för dataåtkomst](https://dev.applicationinsights.io/) kan du komma åt din telemetri programmässigt.</span><span class="sxs-lookup"><span data-stu-id="76527-116">hello [Data access REST API](https://dev.applicationinsights.io/) lets you access your telemetry programmatically.</span></span>

<span data-ttu-id="76527-117">När löpande Export kopierar din data toostorage (där den kan hålla för så länge du vill), är det fortfarande tillgängliga i Application Insights för hello vanliga [kvarhållningsperioden](app-insights-data-retention-privacy.md).</span><span class="sxs-lookup"><span data-stu-id="76527-117">After Continuous Export copies your data toostorage (where it can stay for as long as you like), it's still available in Application Insights for hello usual [retention period](app-insights-data-retention-privacy.md).</span></span>

## <span data-ttu-id="76527-118"><a name="setup"></a>Skapa en löpande Export</span><span class="sxs-lookup"><span data-stu-id="76527-118"><a name="setup"></a> Create a Continuous Export</span></span>
1. <span data-ttu-id="76527-119">Öppna löpande Export i hello Application Insights-resurs för din app, och välj **Lägg till**:</span><span class="sxs-lookup"><span data-stu-id="76527-119">In hello Application Insights resource for your app, open Continuous Export and choose **Add**:</span></span>

    ![Bläddra nedåt och klicka på löpande Export](./media/app-insights-export-telemetry/01-export.png)

2. <span data-ttu-id="76527-121">Välj hello telemetri datatyper du vill tooexport.</span><span class="sxs-lookup"><span data-stu-id="76527-121">Choose hello telemetry data types you want tooexport.</span></span>

3. <span data-ttu-id="76527-122">Skapa eller välj en [Azure storage-konto](../storage/common/storage-introduction.md) där du vill toostore hello data.</span><span class="sxs-lookup"><span data-stu-id="76527-122">Create or select an [Azure storage account](../storage/common/storage-introduction.md) where you want toostore hello data.</span></span>

    > [!Warning]
    > <span data-ttu-id="76527-123">Som standard hello lagringsplatsen anges toohello samma geografiska region som Application Insights-resurs.</span><span class="sxs-lookup"><span data-stu-id="76527-123">By default, hello storage location will be set toohello same geographical region as your Application Insights resource.</span></span> <span data-ttu-id="76527-124">Om du sparar i en annan region, kan det utgå avgifter för överföring.</span><span class="sxs-lookup"><span data-stu-id="76527-124">If you store in a different region, you may incur transfer charges.</span></span>

    ![Klicka på Lägg till, exportera mål, Storage-konto och sedan skapa en ny lagring eller välj en befintlig butik](./media/app-insights-export-telemetry/02-add.png)

4. <span data-ttu-id="76527-126">Skapa eller välj en behållare i hello lagring:</span><span class="sxs-lookup"><span data-stu-id="76527-126">Create or select a container in hello storage:</span></span>

    ![Klicka på Välj händelsetyper](./media/app-insights-export-telemetry/create-container.png)

<span data-ttu-id="76527-128">När du har skapat din export, startar det ska.</span><span class="sxs-lookup"><span data-stu-id="76527-128">Once you've created your export, it starts going.</span></span> <span data-ttu-id="76527-129">Du kan bara hämta data som tas emot när du har skapat hello export.</span><span class="sxs-lookup"><span data-stu-id="76527-129">You only get data that arrives after you create hello export.</span></span>

<span data-ttu-id="76527-130">Det kan finnas en fördröjning på ungefär en timme innan data visas i hello lagring.</span><span class="sxs-lookup"><span data-stu-id="76527-130">There can be a delay of about an hour before data appears in hello storage.</span></span>

### <a name="tooedit-continuous-export"></a><span data-ttu-id="76527-131">tooedit löpande export</span><span class="sxs-lookup"><span data-stu-id="76527-131">tooedit continuous export</span></span>

<span data-ttu-id="76527-132">Om du vill toochange hello händelsetyper senare kan du bara redigera hello exporten:</span><span class="sxs-lookup"><span data-stu-id="76527-132">If you want toochange hello event types later, just edit hello export:</span></span>

![Klicka på Välj händelsetyper](./media/app-insights-export-telemetry/05-edit.png)

### <a name="toostop-continuous-export"></a><span data-ttu-id="76527-134">toostop löpande export</span><span class="sxs-lookup"><span data-stu-id="76527-134">toostop continuous export</span></span>

<span data-ttu-id="76527-135">toostop hello export, klicka på Inaktivera.</span><span class="sxs-lookup"><span data-stu-id="76527-135">toostop hello export, click Disable.</span></span> <span data-ttu-id="76527-136">När du klickar på Aktivera igen startas hello export med nya data.</span><span class="sxs-lookup"><span data-stu-id="76527-136">When you click Enable again, hello export will restart with new data.</span></span> <span data-ttu-id="76527-137">Du kommer inte hello data som anlänt på hello portalen medan export har inaktiverats.</span><span class="sxs-lookup"><span data-stu-id="76527-137">You won't get hello data that arrived in hello portal while export was disabled.</span></span>

<span data-ttu-id="76527-138">toostop hello exportera, ta bort den permanent.</span><span class="sxs-lookup"><span data-stu-id="76527-138">toostop hello export permanently, delete it.</span></span> <span data-ttu-id="76527-139">Gör det bort inte data från lagring.</span><span class="sxs-lookup"><span data-stu-id="76527-139">Doing so doesn't delete your data from storage.</span></span>

### <a name="cant-add-or-change-an-export"></a><span data-ttu-id="76527-140">Det går inte att lägga till eller ändra en export?</span><span class="sxs-lookup"><span data-stu-id="76527-140">Can't add or change an export?</span></span>
* <span data-ttu-id="76527-141">tooadd eller ändra export måste ägare, deltagare eller Application Insights deltagare åtkomsträttigheter.</span><span class="sxs-lookup"><span data-stu-id="76527-141">tooadd or change exports, you need Owner, Contributor or Application Insights Contributor access rights.</span></span> <span data-ttu-id="76527-142">[Lär dig mer om roller][roles].</span><span class="sxs-lookup"><span data-stu-id="76527-142">[Learn about roles][roles].</span></span>

## <span data-ttu-id="76527-143"><a name="analyze"></a>Vilka händelser får du?</span><span class="sxs-lookup"><span data-stu-id="76527-143"><a name="analyze"></a> What events do you get?</span></span>
<span data-ttu-id="76527-144">hello är exporterade data hello rådata telemetri som vi får från programmet, förutom att vi lägga till lokaliseringsuppgifter som vi beräkna från hello klientens IP-adress.</span><span class="sxs-lookup"><span data-stu-id="76527-144">hello exported data is hello raw telemetry we receive from your application, except that we add location data which we calculate from hello client IP address.</span></span>

<span data-ttu-id="76527-145">Data som har tagits bort av [provtagning](app-insights-sampling.md) ingår inte i hello exporterade data.</span><span class="sxs-lookup"><span data-stu-id="76527-145">Data that has been discarded by [sampling](app-insights-sampling.md) is not included in hello exported data.</span></span>

<span data-ttu-id="76527-146">Det ingår inte andra beräknade mått.</span><span class="sxs-lookup"><span data-stu-id="76527-146">Other calculated metrics are not included.</span></span> <span data-ttu-id="76527-147">Till exempel vi exportera inte Genomsnittlig CPU-användning, men vi exportera hello rådata telemetri som hello medelvärde beräknas.</span><span class="sxs-lookup"><span data-stu-id="76527-147">For example, we don't export average CPU utilisation, but we do export hello raw telemetry from which hello average is computed.</span></span>

<span data-ttu-id="76527-148">hello data även hello resultat för alla [tillgänglighet webbtester](app-insights-monitor-web-app-availability.md) som du har skapat.</span><span class="sxs-lookup"><span data-stu-id="76527-148">hello data also includes hello results of any [availability web tests](app-insights-monitor-web-app-availability.md) that you have set up.</span></span>

> [!NOTE]
> <span data-ttu-id="76527-149">**Sampling.**</span><span class="sxs-lookup"><span data-stu-id="76527-149">**Sampling.**</span></span> <span data-ttu-id="76527-150">Om ditt program skickar stora mängder data, hello provtagning funktionen fungerar och skicka en bråkdel av hello genereras telemetri.</span><span class="sxs-lookup"><span data-stu-id="76527-150">If your application sends a lot of data, hello sampling feature may operate and send only a fraction of hello generated telemetry.</span></span> [<span data-ttu-id="76527-151">Läs mer om sampling.</span><span class="sxs-lookup"><span data-stu-id="76527-151">Learn more about sampling.</span></span>](app-insights-sampling.md)
>
>

## <span data-ttu-id="76527-152"><a name="get"></a>Inspektera hello data</span><span class="sxs-lookup"><span data-stu-id="76527-152"><a name="get"></a> Inspect hello data</span></span>
<span data-ttu-id="76527-153">Du kan inspektera hello lagring direkt i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="76527-153">You can inspect hello storage directly in hello portal.</span></span> <span data-ttu-id="76527-154">Klicka på **Bläddra**, Välj ditt lagringskonto och öppna sedan **behållare**.</span><span class="sxs-lookup"><span data-stu-id="76527-154">Click **Browse**, select your storage account, and then open **Containers**.</span></span>

<span data-ttu-id="76527-155">Öppna tooinspect Azure storage i Visual Studio **visa**, **Cloud Explorer**.</span><span class="sxs-lookup"><span data-stu-id="76527-155">tooinspect Azure storage in Visual Studio, open **View**, **Cloud Explorer**.</span></span> <span data-ttu-id="76527-156">(Om du inte har det kommandot du behöver tooinstall hello Azure SDK: öppna hello **nytt projekt** dialogrutan Expandera Visual C# / molnet och välj **hämta Microsoft Azure SDK för .NET**.)</span><span class="sxs-lookup"><span data-stu-id="76527-156">(If you don't have that menu command, you need tooinstall hello Azure SDK: Open hello **New Project** dialog, expand Visual C#/Cloud and choose **Get Microsoft Azure SDK for .NET**.)</span></span>

<span data-ttu-id="76527-157">När du öppnar din blobstore, visas en behållare med en uppsättning blob-filer.</span><span class="sxs-lookup"><span data-stu-id="76527-157">When you open your blob store, you'll see a container with a set of blob files.</span></span> <span data-ttu-id="76527-158">hello URI: N för varje fil som härletts från din Application Insights-resurs med instrumentation nyckel, telemetri-typ/datum/tid.</span><span class="sxs-lookup"><span data-stu-id="76527-158">hello URI of each file derived from your Application Insights resource name, its instrumentation key, telemetry-type/date/time.</span></span> <span data-ttu-id="76527-159">(hello resursnamnet är gemener och hello instrumentation nyckeln utesluter bindestreck.)</span><span class="sxs-lookup"><span data-stu-id="76527-159">(hello resource name is all lowercase, and hello instrumentation key omits dashes.)</span></span>

![Inspektera hello blobstore med ett lämpligt verktyg](./media/app-insights-export-telemetry/04-data.png)

<span data-ttu-id="76527-161">hello datum och tid är UTC och när hello telemetri har deponerats i hello store - inte hello tid den genererades.</span><span class="sxs-lookup"><span data-stu-id="76527-161">hello date and time are UTC and are when hello telemetry was deposited in hello store - not hello time it was generated.</span></span> <span data-ttu-id="76527-162">Så om du skriva kod toodownload hello data den flytta linjärt hello-informationen.</span><span class="sxs-lookup"><span data-stu-id="76527-162">So if you write code toodownload hello data, it can move linearly through hello data.</span></span>

<span data-ttu-id="76527-163">Här är hello form av hello-sökväg:</span><span class="sxs-lookup"><span data-stu-id="76527-163">Here's hello form of hello path:</span></span>

    $"{applicationName}_{instrumentationKey}/{type}/{blobDeliveryTimeUtc:yyyy-MM-dd}/{ blobDeliveryTimeUtc:HH}/{blobId}_{blobCreationTimeUtc:yyyyMMdd_HHmmss}.blob"

<span data-ttu-id="76527-164">där</span><span class="sxs-lookup"><span data-stu-id="76527-164">Where</span></span>

* <span data-ttu-id="76527-165">`blobCreationTimeUtc`tid då blob skapades i hello interna är Förproduktion storage</span><span class="sxs-lookup"><span data-stu-id="76527-165">`blobCreationTimeUtc` is time when blob was created in hello internal staging storage</span></span>
* <span data-ttu-id="76527-166">`blobDeliveryTimeUtc`är hello tid när blob kopierade toohello export mållagringskontot</span><span class="sxs-lookup"><span data-stu-id="76527-166">`blobDeliveryTimeUtc` is hello time when blob is copied toohello export destination storage</span></span>

## <span data-ttu-id="76527-167"><a name="format"></a>Dataformatet</span><span class="sxs-lookup"><span data-stu-id="76527-167"><a name="format"></a> Data format</span></span>
* <span data-ttu-id="76527-168">Varje blobb är en textfil som innehåller flera ' \n'-separated rader.</span><span class="sxs-lookup"><span data-stu-id="76527-168">Each blob is a text file that contains multiple '\n'-separated rows.</span></span> <span data-ttu-id="76527-169">Den innehåller hello telemetri bearbetas under en period av ungefär en halv minut.</span><span class="sxs-lookup"><span data-stu-id="76527-169">It contains hello telemetry processed over a time period of roughly half a minute.</span></span>
* <span data-ttu-id="76527-170">Varje rad representerar en telemetri data, till exempel en vy för begäran eller sidan.</span><span class="sxs-lookup"><span data-stu-id="76527-170">Each row represents a telemetry data point such as a request or page view.</span></span>
* <span data-ttu-id="76527-171">Varje rad är ett oformaterat JSON-dokument.</span><span class="sxs-lookup"><span data-stu-id="76527-171">Each row is an unformatted JSON document.</span></span> <span data-ttu-id="76527-172">Om du vill toosit och vårvädret den, öppna den i Visual Studio och väljer Redigera, Avancerat formatfilen:</span><span class="sxs-lookup"><span data-stu-id="76527-172">If you want toosit and stare at it, open it in Visual Studio and choose Edit, Advanced, Format File:</span></span>

![Visa hello telemetri med ett lämpligt verktyg](./media/app-insights-export-telemetry/06-json.png)

<span data-ttu-id="76527-174">Tidsvaraktigheter finns i tick, där 10 000 markeringar = 1ms.</span><span class="sxs-lookup"><span data-stu-id="76527-174">Time durations are in ticks, where 10 000 ticks = 1ms.</span></span> <span data-ttu-id="76527-175">Till exempel värdena visa taget 1ms toosend en begäran från hello webbläsare, 3ms tooreceive och 1.8s tooprocess hello sidan i hello webbläsare:</span><span class="sxs-lookup"><span data-stu-id="76527-175">For example, these values show a time of 1ms toosend a request from hello browser, 3ms tooreceive it, and 1.8s tooprocess hello page in hello browser:</span></span>

    "sendRequest": {"value": 10000.0},
    "receiveRequest": {"value": 30000.0},
    "clientProcess": {"value": 17970000.0}

[<span data-ttu-id="76527-176">Detaljerad datamodell referens för hello egenskapstyper och värden.</span><span class="sxs-lookup"><span data-stu-id="76527-176">Detailed data model reference for hello property types and values.</span></span>](app-insights-export-data-model.md)

## <a name="processing-hello-data"></a><span data-ttu-id="76527-177">Hello databearbetning</span><span class="sxs-lookup"><span data-stu-id="76527-177">Processing hello data</span></span>
<span data-ttu-id="76527-178">I liten skala, skriva vissa kod toopull isär dina data, läsa in den i ett kalkylblad och så vidare.</span><span class="sxs-lookup"><span data-stu-id="76527-178">On a small scale, you can write some code toopull apart your data, read it into a spreadsheet, and so on.</span></span> <span data-ttu-id="76527-179">Exempel:</span><span class="sxs-lookup"><span data-stu-id="76527-179">For example:</span></span>

    private IEnumerable<T> DeserializeMany<T>(string folderName)
    {
      var files = Directory.EnumerateFiles(folderName, "*.blob", SearchOption.AllDirectories);
      foreach (var file in files)
      {
         using (var fileReader = File.OpenText(file))
         {
            string fileContent = fileReader.ReadToEnd();
            IEnumerable<string> entities = fileContent.Split('\n').Where(s => !string.IsNullOrWhiteSpace(s));
            foreach (var entity in entities)
            {
                yield return JsonConvert.DeserializeObject<T>(entity);
            }
         }
      }
    }

<span data-ttu-id="76527-180">En större kodexemplet finns [med hjälp av en arbetsroll][exportasa].</span><span class="sxs-lookup"><span data-stu-id="76527-180">For a larger code sample, see [using a worker role][exportasa].</span></span>

## <span data-ttu-id="76527-181"><a name="delete"></a>Ta bort gamla data</span><span class="sxs-lookup"><span data-stu-id="76527-181"><a name="delete"></a>Delete your old data</span></span>
<span data-ttu-id="76527-182">Observera att du är ansvarig för att hantera lagringskapaciteten och ta bort hello gamla data om det behövs.</span><span class="sxs-lookup"><span data-stu-id="76527-182">Please note that you are responsible for managing your storage capacity and deleting hello old data if necessary.</span></span>

## <a name="if-you-regenerate-your-storage-key"></a><span data-ttu-id="76527-183">Om du återskapar nyckeln för säkerhetslagring...</span><span class="sxs-lookup"><span data-stu-id="76527-183">If you regenerate your storage key...</span></span>
<span data-ttu-id="76527-184">Om du ändrar hello viktiga tooyour lagring att löpande export sluta fungera.</span><span class="sxs-lookup"><span data-stu-id="76527-184">If you change hello key tooyour storage, continuous export will stop working.</span></span> <span data-ttu-id="76527-185">Du ser ett meddelande i ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="76527-185">You'll see a notification in your Azure account.</span></span>

<span data-ttu-id="76527-186">Öppna hello löpande Export bladet och redigera exporten.</span><span class="sxs-lookup"><span data-stu-id="76527-186">Open hello Continuous Export blade and edit your export.</span></span> <span data-ttu-id="76527-187">Redigera hello exportera mål, men lämna hello samma lagringsutrymme som valts.</span><span class="sxs-lookup"><span data-stu-id="76527-187">Edit hello Export Destination, but just leave hello same storage selected.</span></span> <span data-ttu-id="76527-188">Klicka på OK tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="76527-188">Click OK tooconfirm.</span></span>

![Redigera hello kontinuerlig exportera, öppna och Stäng dig exportera.](./media/app-insights-export-telemetry/07-resetstore.png)

<span data-ttu-id="76527-190">hello löpande export startas om.</span><span class="sxs-lookup"><span data-stu-id="76527-190">hello continuous export will restart.</span></span>

## <a name="export-samples"></a><span data-ttu-id="76527-191">Export-exempel</span><span class="sxs-lookup"><span data-stu-id="76527-191">Export samples</span></span>

* <span data-ttu-id="76527-192">[Exportera tooSQL med Stream Analytics][exportasa]</span><span class="sxs-lookup"><span data-stu-id="76527-192">[Export tooSQL using Stream Analytics][exportasa]</span></span>
* [<span data-ttu-id="76527-193">Stream Analytics-exempel 2</span><span class="sxs-lookup"><span data-stu-id="76527-193">Stream Analytics sample 2</span></span>](app-insights-export-stream-analytics.md)

<span data-ttu-id="76527-194">Överväg att i större skala [HDInsight](https://azure.microsoft.com/services/hdinsight/) -Hadoop-kluster i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="76527-194">On larger scales, consider [HDInsight](https://azure.microsoft.com/services/hdinsight/) - Hadoop clusters in hello cloud.</span></span> <span data-ttu-id="76527-195">HDInsight tillhandahåller en mängd olika tekniker för att hantera och analys av stordata och du kan använda den tooprocess data som har exporterats från Application Insights.</span><span class="sxs-lookup"><span data-stu-id="76527-195">HDInsight provides a variety of technologies for managing and analyzing big data, and you could use it tooprocess data that has been exported from Application Insights.</span></span>

## <a name="q--a"></a><span data-ttu-id="76527-196">Frågor och svar</span><span class="sxs-lookup"><span data-stu-id="76527-196">Q & A</span></span>
* <span data-ttu-id="76527-197">*Men jag vill bara en gång hämtning av ett diagram.*</span><span class="sxs-lookup"><span data-stu-id="76527-197">*But all I want is a one-time download of a chart.*</span></span>  

    <span data-ttu-id="76527-198">Ja, kan du göra det.</span><span class="sxs-lookup"><span data-stu-id="76527-198">Yes, you can do that.</span></span> <span data-ttu-id="76527-199">Hello överkant hello-bladet, klickar du på **exportera Data**.</span><span class="sxs-lookup"><span data-stu-id="76527-199">At hello top of hello blade, click **Export Data**.</span></span>
* <span data-ttu-id="76527-200">*Jag har skapat en export, men det finns inga data i mitt Arkiv.*</span><span class="sxs-lookup"><span data-stu-id="76527-200">*I set up an export, but there's no data in my store.*</span></span>

    <span data-ttu-id="76527-201">Application Insights erhöll någon telemetri från din app eftersom du ställer in hello export?</span><span class="sxs-lookup"><span data-stu-id="76527-201">Did Application Insights receive any telemetry from your app since you set up hello export?</span></span> <span data-ttu-id="76527-202">Du får endast nya data.</span><span class="sxs-lookup"><span data-stu-id="76527-202">You'll only receive new data.</span></span>
* <span data-ttu-id="76527-203">*Jag försökte tooset upp en export men nekas åtkomst*</span><span class="sxs-lookup"><span data-stu-id="76527-203">*I tried tooset up an export, but was denied access*</span></span>

    <span data-ttu-id="76527-204">Om hello konto ägs av organisationen, har du toobe en medlem av hello ägare eller deltagare.</span><span class="sxs-lookup"><span data-stu-id="76527-204">If hello account is owned by your organization, you have toobe a member of hello owners or contributors groups.</span></span>
* <span data-ttu-id="76527-205">*Kan jag exportera raka toomy egna lokala store?*</span><span class="sxs-lookup"><span data-stu-id="76527-205">*Can I export straight toomy own on-premises store?*</span></span>

    <span data-ttu-id="76527-206">Nej, tyvärr.</span><span class="sxs-lookup"><span data-stu-id="76527-206">No, sorry.</span></span> <span data-ttu-id="76527-207">Vår export-motorn fungerar för tillfället bara med Azure storage just nu.</span><span class="sxs-lookup"><span data-stu-id="76527-207">Our export engine currently only works with Azure storage at this time.</span></span>  
* <span data-ttu-id="76527-208">*Finns det någon gräns toohello mängden data som du lägger till i mitt Arkiv?*</span><span class="sxs-lookup"><span data-stu-id="76527-208">*Is there any limit toohello amount of data you put in my store?*</span></span>

    <span data-ttu-id="76527-209">Nej.</span><span class="sxs-lookup"><span data-stu-id="76527-209">No.</span></span> <span data-ttu-id="76527-210">Vi kommer att skicka data tills du tar bort hello export.</span><span class="sxs-lookup"><span data-stu-id="76527-210">We'll keep pushing data in until you delete hello export.</span></span> <span data-ttu-id="76527-211">Vi ska avbrytas om vi träffar hello yttre gränser för blob-lagring, men det är ganska stor.</span><span class="sxs-lookup"><span data-stu-id="76527-211">We'll stop if we hit hello outer limits for blob storage, but that's pretty huge.</span></span> <span data-ttu-id="76527-212">Är det upp tooyou toocontrol hur mycket lagringsutrymme som du använder.</span><span class="sxs-lookup"><span data-stu-id="76527-212">It's up tooyou toocontrol how much storage you use.</span></span>  
* <span data-ttu-id="76527-213">*Hur många blobbar ska visas i hello storage?*</span><span class="sxs-lookup"><span data-stu-id="76527-213">*How many blobs should I see in hello storage?*</span></span>

  * <span data-ttu-id="76527-214">För varje datatyp skapas varje minut för valda tooexport, en ny blob (om det finns data).</span><span class="sxs-lookup"><span data-stu-id="76527-214">For every data type you selected tooexport, a new blob is created every minute (if data is available).</span></span>
  * <span data-ttu-id="76527-215">Dessutom för program med hög belastning på ytterligare partition enheter allokeras.</span><span class="sxs-lookup"><span data-stu-id="76527-215">In addition, for applications with high traffic, additional partition units are allocated.</span></span> <span data-ttu-id="76527-216">I det här fallet skapar varje enhet en blob varje minut.</span><span class="sxs-lookup"><span data-stu-id="76527-216">In this case each unit creates a blob every minute.</span></span>
* <span data-ttu-id="76527-217">*Jag återskapas hello viktiga toomy lagring eller ändrats hello namnet på hello behållare och nu fungerar inte hello export.*</span><span class="sxs-lookup"><span data-stu-id="76527-217">*I regenerated hello key toomy storage or changed hello name of hello container, and now hello export doesn't work.*</span></span>

    <span data-ttu-id="76527-218">Redigera hello exporten bladet och öppna hello export mål.</span><span class="sxs-lookup"><span data-stu-id="76527-218">Edit hello export and open hello export destination blade.</span></span> <span data-ttu-id="76527-219">Lämna hello samma lagringsutrymme som tidigare valt och klicka på OK tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="76527-219">Leave hello same storage selected as before, and click OK tooconfirm.</span></span> <span data-ttu-id="76527-220">Exportera startas om.</span><span class="sxs-lookup"><span data-stu-id="76527-220">Export will restart.</span></span> <span data-ttu-id="76527-221">Om hello ändringen var inom hello efter några dagar kan förlora du inte data.</span><span class="sxs-lookup"><span data-stu-id="76527-221">If hello change was within hello past few days, you won't lose data.</span></span>
* <span data-ttu-id="76527-222">*Kan jag pausa hello export?*</span><span class="sxs-lookup"><span data-stu-id="76527-222">*Can I pause hello export?*</span></span>

    <span data-ttu-id="76527-223">Ja.</span><span class="sxs-lookup"><span data-stu-id="76527-223">Yes.</span></span> <span data-ttu-id="76527-224">Klicka på Inaktivera.</span><span class="sxs-lookup"><span data-stu-id="76527-224">Click Disable.</span></span>

## <a name="code-samples"></a><span data-ttu-id="76527-225">Kodexempel</span><span class="sxs-lookup"><span data-stu-id="76527-225">Code samples</span></span>

* [<span data-ttu-id="76527-226">Stream Analytics-exempel</span><span class="sxs-lookup"><span data-stu-id="76527-226">Stream Analytics sample</span></span>](app-insights-export-stream-analytics.md)
* <span data-ttu-id="76527-227">[Exportera tooSQL med Stream Analytics][exportasa]</span><span class="sxs-lookup"><span data-stu-id="76527-227">[Export tooSQL using Stream Analytics][exportasa]</span></span>
* [<span data-ttu-id="76527-228">Detaljerad datamodell referens för hello egenskapstyper och värden.</span><span class="sxs-lookup"><span data-stu-id="76527-228">Detailed data model reference for hello property types and values.</span></span>](app-insights-export-data-model.md)

<!--Link references-->

[exportasa]: app-insights-code-sample-export-sql-stream-analytics.md
[roles]: app-insights-resources-roles-access-control.md
