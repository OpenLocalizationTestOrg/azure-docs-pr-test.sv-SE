---
title: aaaTroubleshoot Application Insights i ett Java-webbprojekt
description: "Felsökningsguide för - övervakning av live Java-appar med Application Insights."
services: application-insights
documentationcenter: java
author: CFreemanwa
manager: carmonm
ms.assetid: ef602767-18f2-44d2-b7ef-42b404edd0e9
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/16/2016
ms.author: bwren
ms.openlocfilehash: c274c01b1992971fae194c3e510512ca06ab76b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-and-q-and-a-for-application-insights-for-java"></a><span data-ttu-id="8a9f7-103">Felsökning och vanliga frågor och svar för Application Insights för Java</span><span class="sxs-lookup"><span data-stu-id="8a9f7-103">Troubleshooting and Q and A for Application Insights for Java</span></span>
<span data-ttu-id="8a9f7-104">Frågor eller problem med [Azure Application Insights i Java][java]?</span><span class="sxs-lookup"><span data-stu-id="8a9f7-104">Questions or problems with [Azure Application Insights in Java][java]?</span></span> <span data-ttu-id="8a9f7-105">Här följer några tips.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-105">Here are some tips.</span></span>

## <a name="build-errors"></a><span data-ttu-id="8a9f7-106">Skapa fel</span><span class="sxs-lookup"><span data-stu-id="8a9f7-106">Build errors</span></span>
<span data-ttu-id="8a9f7-107">**I Eclipse när lägger till hello Application Insights SDK via Maven eller Gradle, visas build eller kontrollsumma verifieringsfel.**</span><span class="sxs-lookup"><span data-stu-id="8a9f7-107">**In Eclipse, when adding hello Application Insights SDK via Maven or Gradle, I get build or checksum validation errors.**</span></span>

* <span data-ttu-id="8a9f7-108">Om hello beroende <version> elementet använder ett mönster med jokertecken (t.ex. (Maven) `<version>[1.0,)</version>` eller (Gradle) `version:'1.0.+'`), pröva en specifik version i stället som `1.0.2`.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-108">If hello dependency <version> element is using a pattern with wildcard characters (e.g. (Maven) `<version>[1.0,)</version>` or (Gradle) `version:'1.0.+'`), try specifying a specific version instead like `1.0.2`.</span></span> <span data-ttu-id="8a9f7-109">Se hello [viktig information](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) hello senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-109">See hello [release notes](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) for hello latest version.</span></span>

## <a name="no-data"></a><span data-ttu-id="8a9f7-110">Inga data</span><span class="sxs-lookup"><span data-stu-id="8a9f7-110">No data</span></span>
<span data-ttu-id="8a9f7-111">**Jag har lagts till Application Insights och körde min app, men aldrig sett data i hello-portalen.**</span><span class="sxs-lookup"><span data-stu-id="8a9f7-111">**I added Application Insights successfully and ran my app, but I've never seen data in hello portal.**</span></span>

* <span data-ttu-id="8a9f7-112">Vänta en minut och klicka på Uppdatera.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-112">Wait a minute and click Refresh.</span></span> <span data-ttu-id="8a9f7-113">hello diagram uppdatera själva regelbundet, men du kan också uppdatera manuellt.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-113">hello charts refresh themselves periodically, but you can also refresh manually.</span></span> <span data-ttu-id="8a9f7-114">intervall för uppdatering av hello beror på hello tidsintervall i hello-diagram.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-114">hello refresh interval depends on hello time range of hello chart.</span></span>
* <span data-ttu-id="8a9f7-115">Kontrollera att du har en instrumentation nyckel som definierats i hello ApplicationInsights.xml-filen (i mappen hello resurser i ditt projekt)</span><span class="sxs-lookup"><span data-stu-id="8a9f7-115">Check that you have an instrumentation key defined in hello ApplicationInsights.xml file (in hello resources folder in your project)</span></span>
* <span data-ttu-id="8a9f7-116">Kontrollera att det finns inga `<DisableTelemetry>true</DisableTelemetry>` nod i hello XML-fil.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-116">Verify that there is no `<DisableTelemetry>true</DisableTelemetry>` node in hello xml file.</span></span>
* <span data-ttu-id="8a9f7-117">Du kan ha tooopen TCP-portarna 80 och 443 för utgående trafik toodc.services.visualstudio.com i brandväggen. Se hello [fullständig lista över brandväggsundantag](app-insights-ip-addresses.md)</span><span class="sxs-lookup"><span data-stu-id="8a9f7-117">In your firewall, you might have tooopen TCP ports 80 and 443 for outgoing traffic toodc.services.visualstudio.com. See hello [full list of firewall exceptions](app-insights-ip-addresses.md)</span></span>
* <span data-ttu-id="8a9f7-118">Starta board i hello Microsoft Azure, titta på hello tjänstkarta status.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-118">In hello Microsoft Azure start board, look at hello service status map.</span></span> <span data-ttu-id="8a9f7-119">Om det finns vissa uppgifter som avisering, vänta tills de returnerade tooOK och Stäng och öppna bladet din Application Insights-programmet igen.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-119">If there are some alert indications, wait until they have returned tooOK and then close and re-open your Application Insights application blade.</span></span>
* <span data-ttu-id="8a9f7-120">Aktivera loggning toohello IDE konsolfönstret, genom att lägga till en `<SDKLogger />` hello rotnoden i hello ApplicationInsights.xml fil (i mappen hello resurser i ditt projekt) och Sök efter poster som inleds med [fel]-element.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-120">Turn on logging toohello IDE console window, by adding an `<SDKLogger />` element under hello root node in hello ApplicationInsights.xml file (in hello resources folder in your project), and check for entries prefaced with [Error].</span></span>
* <span data-ttu-id="8a9f7-121">Kontrollera att rätt ApplicationInsights.xml filen har har lästs in av hello Java SDK genom att titta på hello konsolens Utdatameddelanden för instruktionen ”konfigurationsfilen har har hittat” Hej.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-121">Make sure that hello correct ApplicationInsights.xml file has been successfully loaded by hello Java SDK, by looking at hello console's output messages for a "Configuration file has been successfully found" statement.</span></span>
* <span data-ttu-id="8a9f7-122">Om hello config-fil inte hittas, kontrollera hello utgående meddelanden toosee där hello konfigurationsfilen genomsöks efter och se till att hello ApplicationInsights.xml finns på någon av dessa platser.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-122">If hello config file is not found, check hello output messages toosee where hello config file is being searched for, and make sure that hello ApplicationInsights.xml is located in one of those search locations.</span></span> <span data-ttu-id="8a9f7-123">Du kan placera hello konfigurationsfilen nära hello Application Insights SDK JAR: er som en tumregel.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-123">As a rule of thumb, you can place hello config file near hello Application Insights SDK JARs.</span></span> <span data-ttu-id="8a9f7-124">Exempel: i Tomcat, detta betyder att hello WEB-INF/lib mapp.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-124">For example: in Tomcat, this would mean hello WEB-INF/lib folder.</span></span>

#### <a name="i-used-toosee-data-but-it-has-stopped"></a><span data-ttu-id="8a9f7-125">Jag använde toosee data, men den har stoppats</span><span class="sxs-lookup"><span data-stu-id="8a9f7-125">I used toosee data, but it has stopped</span></span>
* <span data-ttu-id="8a9f7-126">Kontrollera hello [status blogg](http://blogs.msdn.com/b/applicationinsights-status/).</span><span class="sxs-lookup"><span data-stu-id="8a9f7-126">Check hello [status blog](http://blogs.msdn.com/b/applicationinsights-status/).</span></span>
* <span data-ttu-id="8a9f7-127">Har du uppnått den månatliga kvoten för datapunkter?</span><span class="sxs-lookup"><span data-stu-id="8a9f7-127">Have you hit your monthly quota of data points?</span></span> <span data-ttu-id="8a9f7-128">Öppna inställningar/kvoten och priser toofind ut. I så fall, kan du uppgradera din plan eller betala för extra kapacitet.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-128">Open Settings/Quota and Pricing toofind out. If so, you can upgrade your plan, or pay for additional capacity.</span></span> <span data-ttu-id="8a9f7-129">Se hello [priser schemat](https://azure.microsoft.com/pricing/details/application-insights/).</span><span class="sxs-lookup"><span data-stu-id="8a9f7-129">See hello [pricing scheme](https://azure.microsoft.com/pricing/details/application-insights/).</span></span>

#### <a name="i-dont-see-all-hello-data-im-expecting"></a><span data-ttu-id="8a9f7-130">Alla hello data jag förväntade visas inte</span><span class="sxs-lookup"><span data-stu-id="8a9f7-130">I don't see all hello data I'm expecting</span></span>
* <span data-ttu-id="8a9f7-131">Öppna hello kvoter och prissättning bladet och kontrollera om [provtagning](app-insights-sampling.md) är i drift.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-131">Open hello Quotas and Pricing blade and check whether [sampling](app-insights-sampling.md) is in operation.</span></span> <span data-ttu-id="8a9f7-132">(100% transmission innebär att provtagning inte är i drift.) hello Application Insights-tjänsten kan vara set tooaccept en bråkdel av hello telemetri som tas emot från din app.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-132">(100% transmission means that sampling isn't in operation.) hello Application Insights service can be set tooaccept only a fraction of hello telemetry that arrives from your app.</span></span> <span data-ttu-id="8a9f7-133">Detta gör att inom din månatliga kvoten av telemetri.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-133">This helps you keep within your monthly quota of telemetry.</span></span> 

## <a name="no-usage-data"></a><span data-ttu-id="8a9f7-134">Inga användningsdata</span><span class="sxs-lookup"><span data-stu-id="8a9f7-134">No usage data</span></span>
<span data-ttu-id="8a9f7-135">**Information om begäranden och svarstider, men inga vyn sida, webbläsare eller användardata visas.**</span><span class="sxs-lookup"><span data-stu-id="8a9f7-135">**I see data about requests and response times, but no page view, browser, or user data.**</span></span>

<span data-ttu-id="8a9f7-136">Du konfigurerat din app toosend telemetri från hello-servern.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-136">You successfully set up your app toosend telemetry from hello server.</span></span> <span data-ttu-id="8a9f7-137">Nu är nästa steg för[ställa in din webbsidor toosend telemetri från hello webbläsare][usage].</span><span class="sxs-lookup"><span data-stu-id="8a9f7-137">Now your next step is too[set up your web pages toosend telemetry from hello web browser][usage].</span></span>

<span data-ttu-id="8a9f7-138">Alternativt, om klienten är en app i en [telefonnummer eller andra][platforms], du kan skicka telemetri därifrån.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-138">Alternatively, if your client is an app in a [phone or other device][platforms], you can send telemetry from there.</span></span> 

<span data-ttu-id="8a9f7-139">Använd hello samma instrumentation viktiga tooset in telemetri om både klienten och servern.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-139">Use hello same instrumentation key tooset up both your client and server telemetry.</span></span> <span data-ttu-id="8a9f7-140">hello data visas i hello samma Application Insights-resurs, och du kan toocorrelate händelser från klienten och servern.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-140">hello data will appear in hello same Application Insights resource, and you'll be able toocorrelate events from client and server.</span></span>


## <a name="disabling-telemetry"></a><span data-ttu-id="8a9f7-141">Inaktivera telemetri</span><span class="sxs-lookup"><span data-stu-id="8a9f7-141">Disabling telemetry</span></span>
<span data-ttu-id="8a9f7-142">**Hur inaktiverar jag telemetri samlingen?**</span><span class="sxs-lookup"><span data-stu-id="8a9f7-142">**How can I disable telemetry collection?**</span></span>

<span data-ttu-id="8a9f7-143">I koden:</span><span class="sxs-lookup"><span data-stu-id="8a9f7-143">In code:</span></span>

```Java

    TelemetryConfiguration config = TelemetryConfiguration.getActive();
    config.setTrackingIsDisabled(true);
```

<span data-ttu-id="8a9f7-144">**Eller**</span><span class="sxs-lookup"><span data-stu-id="8a9f7-144">**Or**</span></span> 

<span data-ttu-id="8a9f7-145">Uppdatera ApplicationInsights.xml (i mappen hello resurser i ditt projekt).</span><span class="sxs-lookup"><span data-stu-id="8a9f7-145">Update ApplicationInsights.xml (in hello resources folder in your project).</span></span> <span data-ttu-id="8a9f7-146">Lägg till följande hello under hello root-noden:</span><span class="sxs-lookup"><span data-stu-id="8a9f7-146">Add hello following under hello root node:</span></span>

```XML

    <DisableTelemetry>true</DisableTelemetry>
```

<span data-ttu-id="8a9f7-147">Metoden hello XML, har du toorestart hello program när du ändrar hello värde.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-147">Using hello XML method, you have toorestart hello application when you change hello value.</span></span>

## <a name="changing-hello-target"></a><span data-ttu-id="8a9f7-148">Ändra hello mål</span><span class="sxs-lookup"><span data-stu-id="8a9f7-148">Changing hello target</span></span>
<span data-ttu-id="8a9f7-149">**Hur kan jag ändra vilka Azure-resurs projektet skickar data till?**</span><span class="sxs-lookup"><span data-stu-id="8a9f7-149">**How can I change which Azure resource my project sends data to?**</span></span>

* <span data-ttu-id="8a9f7-150">[Hämta hello instrumentation nyckel för hello ny resurs.][java]</span><span class="sxs-lookup"><span data-stu-id="8a9f7-150">[Get hello instrumentation key of hello new resource.][java]</span></span>
* <span data-ttu-id="8a9f7-151">Om du har lagt till Application Insights tooyour projektet med hello Azure Toolkit för Eclipse webbprojektet Högerklicka, Välj **Azure**, **konfigurera Application Insights**, och ändra hello nyckel.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-151">If you added Application Insights tooyour project using hello Azure Toolkit for Eclipse, right click your web project, select **Azure**, **Configure Application Insights**, and change hello key.</span></span>
* <span data-ttu-id="8a9f7-152">Annars uppdatera hello nyckeln i ApplicationInsights.xml i hello resurser mappen i projektet.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-152">Otherwise, update hello key in ApplicationInsights.xml in hello resources folder in your project.</span></span>

## <a name="debug-data-from-hello-sdk"></a><span data-ttu-id="8a9f7-153">Felsöka data från hello SDK</span><span class="sxs-lookup"><span data-stu-id="8a9f7-153">Debug data from hello SDK</span></span>

<span data-ttu-id="8a9f7-154">**Hur kan jag ta reda vilka hello SDK göra?**</span><span class="sxs-lookup"><span data-stu-id="8a9f7-154">**How can I find out what hello SDK is doing?**</span></span>

<span data-ttu-id="8a9f7-155">tooget mer information om vad som händer i hello API, lägga till `<SDKLogger/>` under hello rotnoden hello ApplicationInsights.xml konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-155">tooget more information about what's happening in hello API, add `<SDKLogger/>` under hello root node of hello ApplicationInsights.xml configuration file.</span></span>

<span data-ttu-id="8a9f7-156">Du kan också instruera hello loggaren toooutput tooa fil:</span><span class="sxs-lookup"><span data-stu-id="8a9f7-156">You can also instruct hello logger toooutput tooa file:</span></span>

```XML

    <SDKLogger type="FILE">
      <enabled>True</enabled>
      <UniquePrefix>JavaSDKLog</UniquePrefix>
    </SDKLogger>
```

<span data-ttu-id="8a9f7-157">hello-filer kan hittas `%temp%\javasdklogs` eller `java.io.tmpdir` vid Tomcat-server.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-157">hello files can be found under `%temp%\javasdklogs` or `java.io.tmpdir` in case of Tomcat server.</span></span>


## <a name="hello-azure-start-screen"></a><span data-ttu-id="8a9f7-158">hello Azure startskärmen</span><span class="sxs-lookup"><span data-stu-id="8a9f7-158">hello Azure start screen</span></span>
<span data-ttu-id="8a9f7-159">**Jag tittar på [hello Azure-portalen](https://portal.azure.com). Hello kartan berätta något om min app?**</span><span class="sxs-lookup"><span data-stu-id="8a9f7-159">**I'm looking at [hello Azure portal](https://portal.azure.com). Does hello map tell me something about my app?**</span></span>

<span data-ttu-id="8a9f7-160">Nej, den visar hello hälsotillståndet för Azure-servrar hello världen.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-160">No, it shows hello health of Azure servers around hello world.</span></span>

<span data-ttu-id="8a9f7-161">*Från hello Azure start board (startsidan), hur hittar jag information om min app?*</span><span class="sxs-lookup"><span data-stu-id="8a9f7-161">*From hello Azure start board (home screen), how do I find data about my app?*</span></span>

<span data-ttu-id="8a9f7-162">Om du [konfigurera din app för Application Insights][java], klicka på Bläddra, Välj Application Insights och välj hello app resurs du skapat för din app.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-162">Assuming you [set up your app for Application Insights][java], click Browse, select Application Insights, and select hello app resource you created for your app.</span></span> <span data-ttu-id="8a9f7-163">tooget det snabbare i framtiden, du kan fästa din app toohello start-kort.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-163">tooget there faster in future, you can pin your app toohello start board.</span></span>

## <a name="intranet-servers"></a><span data-ttu-id="8a9f7-164">Intranätservrar</span><span class="sxs-lookup"><span data-stu-id="8a9f7-164">Intranet servers</span></span>
<span data-ttu-id="8a9f7-165">**Övervakar jag en server i intranätet?**</span><span class="sxs-lookup"><span data-stu-id="8a9f7-165">**Can I monitor a server on my intranet?**</span></span>

<span data-ttu-id="8a9f7-166">Ja, förutsatt att servern kan skicka telemetri toohello Application Insights-portalen via hello offentliga internet.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-166">Yes, provided your server can send telemetry toohello Application Insights portal through hello public internet.</span></span> 

<span data-ttu-id="8a9f7-167">I brandväggen kanske tooopen TCP-portarna 80 och 443 för utgående trafik toodc.services.visualstudio.com och f5.services.visualstudio.com.</span><span class="sxs-lookup"><span data-stu-id="8a9f7-167">In your firewall, you might have tooopen TCP ports 80 and 443 for outgoing traffic toodc.services.visualstudio.com and f5.services.visualstudio.com.</span></span>

## <a name="data-retention"></a><span data-ttu-id="8a9f7-168">Datakvarhållning</span><span class="sxs-lookup"><span data-stu-id="8a9f7-168">Data retention</span></span>
<span data-ttu-id="8a9f7-169">**Hur länge sparas data i hello portal? Är det säkra?**</span><span class="sxs-lookup"><span data-stu-id="8a9f7-169">**How long is data retained in hello portal? Is it secure?**</span></span>

<span data-ttu-id="8a9f7-170">Se [datalagring och sekretess][data].</span><span class="sxs-lookup"><span data-stu-id="8a9f7-170">See [Data retention and privacy][data].</span></span>

## <a name="next-steps"></a><span data-ttu-id="8a9f7-171">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8a9f7-171">Next steps</span></span>
<span data-ttu-id="8a9f7-172">**Jag konfigurera Application Insights för Min server Java-app. Vad kan jag göra?**</span><span class="sxs-lookup"><span data-stu-id="8a9f7-172">**I set up Application Insights for my Java server app. What else can I do?**</span></span>

* <span data-ttu-id="8a9f7-173">[Övervaka tillgängligheten för webbsidor][availability]</span><span class="sxs-lookup"><span data-stu-id="8a9f7-173">[Monitor availability of your web pages][availability]</span></span>
* <span data-ttu-id="8a9f7-174">[Övervaka användningen av webbsida][usage]</span><span class="sxs-lookup"><span data-stu-id="8a9f7-174">[Monitor web page usage][usage]</span></span>
* <span data-ttu-id="8a9f7-175">[Spåra användning och diagnostisera problem i dina enhetsappar][platforms]</span><span class="sxs-lookup"><span data-stu-id="8a9f7-175">[Track usage and diagnose issues in your device apps][platforms]</span></span>
* <span data-ttu-id="8a9f7-176">[Skriva koden tootrack användning av din app][track]</span><span class="sxs-lookup"><span data-stu-id="8a9f7-176">[Write code tootrack usage of your app][track]</span></span>
* <span data-ttu-id="8a9f7-177">[Samla in diagnostikloggar][javalogs]</span><span class="sxs-lookup"><span data-stu-id="8a9f7-177">[Capture diagnostic logs][javalogs]</span></span>

## <a name="get-help"></a><span data-ttu-id="8a9f7-178">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="8a9f7-178">Get help</span></span>
* [<span data-ttu-id="8a9f7-179">Stack Overflow</span><span class="sxs-lookup"><span data-stu-id="8a9f7-179">Stack Overflow</span></span>](http://stackoverflow.com/questions/tagged/ms-application-insights)

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[data]: app-insights-data-retention-privacy.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[platforms]: app-insights-platforms.md
[track]: app-insights-api-custom-events-metrics.md
[usage]: app-insights-javascript.md

