---
title: aaaStreaming loggar och konsolen
description: "Direktuppspelningsloggar och översikt över administratörskonsolen"
author: btardif
manager: erikre
editor: 
services: app-service\web
documentationcenter: 
ms.assetid: 3e50a287-781b-4c6a-8c53-eec261889d7a
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/12/2016
ms.author: byvinyal
ms.openlocfilehash: bb4b8ce5358da12041e164dfff8f43790dd67924
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="streaming-logs-and-hello-console"></a><span data-ttu-id="2d62c-103">Direktuppspelningsloggar och hello konsolen</span><span class="sxs-lookup"><span data-stu-id="2d62c-103">Streaming Logs and hello Console</span></span>
## <a name="streaming-logs"></a><span data-ttu-id="2d62c-104">Direktuppspelningsloggar</span><span class="sxs-lookup"><span data-stu-id="2d62c-104">Streaming Logs</span></span>
<span data-ttu-id="2d62c-105">Hej **Azure-portalen** ger en integrerad strömmande Loggvisaren där du kan visa spårninghändelser från din **Apptjänst** appar i realtid.</span><span class="sxs-lookup"><span data-stu-id="2d62c-105">hello **Azure portal** provides an integrated streaming log viewer that lets you view tracing events from your **App Service** apps in real time.</span></span>  

<span data-ttu-id="2d62c-106">Ställa in den här funktionen kräver några enkla steg:</span><span class="sxs-lookup"><span data-stu-id="2d62c-106">Setting up this feature requires a few simple steps:</span></span>

* <span data-ttu-id="2d62c-107">Skriva spårningar i koden</span><span class="sxs-lookup"><span data-stu-id="2d62c-107">Write traces in your code</span></span>
* <span data-ttu-id="2d62c-108">Aktivera programmet **diagnostikloggar** för din app</span><span class="sxs-lookup"><span data-stu-id="2d62c-108">Enable Application **Diagnostic Logs** for your app</span></span>
* <span data-ttu-id="2d62c-109">Visa hello ström från hello inbyggda **strömning loggar** Användargränssnittet hello **Azure-portalen**.</span><span class="sxs-lookup"><span data-stu-id="2d62c-109">View hello stream from hello built-in **Streaming Logs** UI in hello **Azure portal**.</span></span>

### <a name="how-toowrite-traces-in-your-code"></a><span data-ttu-id="2d62c-110">Hur toowrite spårar i koden</span><span class="sxs-lookup"><span data-stu-id="2d62c-110">How toowrite traces in your code</span></span>
<span data-ttu-id="2d62c-111">Det är enkelt att skriva spårningar i koden.</span><span class="sxs-lookup"><span data-stu-id="2d62c-111">Writing traces in your code is easy.</span></span>  <span data-ttu-id="2d62c-112">Det är lika enkelt som att skriva följande kod hello i C#:</span><span class="sxs-lookup"><span data-stu-id="2d62c-112">In C# it's as easy as writing hello following code:</span></span>

`````````````````````````
Trace.TraceInformation("My trace statement");
`````````````````````````

`````````````````````````
Trace.TraceWarning("My warning statement");
`````````````````````````

`````````````````````````
Trace.TraceError("My error statement");
`````````````````````````

<span data-ttu-id="2d62c-113">hello Trace klassen bor i hello System.Diagnostics namnområde.</span><span class="sxs-lookup"><span data-stu-id="2d62c-113">hello Trace class lives in hello System.Diagnostics namespace.</span></span>

<span data-ttu-id="2d62c-114">I en node.js-app kan du skriva koden tooachieve hello samma resultat:</span><span class="sxs-lookup"><span data-stu-id="2d62c-114">In a node.js app you can write this code tooachieve hello same result:</span></span>

`````````````````````````
console.log("My trace statement").
`````````````````````````

### <a name="how-tooenable-and-view-hello-streaming-logs"></a><span data-ttu-id="2d62c-115">Hur tooenable och visa hello direktuppspelningsloggar</span><span class="sxs-lookup"><span data-stu-id="2d62c-115">How tooenable and view hello streaming logs</span></span>
<span data-ttu-id="2d62c-116">![][BrowseSitesScreenshot]Diagnostik är aktiverade på grundval av per app.</span><span class="sxs-lookup"><span data-stu-id="2d62c-116">![][BrowseSitesScreenshot] Diagnostics are enabled on a per app basis.</span></span> <span data-ttu-id="2d62c-117">Starta genom att bläddra toohello plats som tooenable denna funktion på.</span><span class="sxs-lookup"><span data-stu-id="2d62c-117">Start by browsing toohello site you would like tooenable this feature on.</span></span>  

<span data-ttu-id="2d62c-118">![][DiagnosticsLogs]Från inställningsmenyn rullar toohello **övervakning** avsnittet och klicka på **(1) diagnostikloggar**.</span><span class="sxs-lookup"><span data-stu-id="2d62c-118">![][DiagnosticsLogs] From settings menu, scroll down toohello **Monitoring** section and click on **(1) Diagnostic Logs**.</span></span> <span data-ttu-id="2d62c-119">Sedan **(2) Aktivera** **programloggning (filsystem)** eller **programloggning (blob)** hello **nivå** alternativet kan du ändra hello allvarlighetsgraden spårningar toocapture.</span><span class="sxs-lookup"><span data-stu-id="2d62c-119">Then **(2) enable** **Application Logging (Filesystem)** or **Application Logging (blob)** hello **Level** option lets you change hello severity level of traces toocapture.</span></span> <span data-ttu-id="2d62c-120">Om du försöker bara tooget bekant med hello funktion, ange hello nivå för**utförlig** tooensure alla trace-satser som samlas in.</span><span class="sxs-lookup"><span data-stu-id="2d62c-120">If you're just trying tooget familiar with hello feature, set hello level too**Verbose** tooensure all of your trace statements are collected.</span></span>

<span data-ttu-id="2d62c-121">Klicka på **spara** hello överst på bladet hello och du är redo tooview loggar.</span><span class="sxs-lookup"><span data-stu-id="2d62c-121">Click **SAVE** at hello top of hello blade and you're ready tooview logs.</span></span>

> [!NOTE]
> <span data-ttu-id="2d62c-122">hello högre hello **allvarlighetsgrad** hello mer resurser är förbrukade toolog och hello mer spårningar produceras.</span><span class="sxs-lookup"><span data-stu-id="2d62c-122">hello higher hello **severity level** hello more resources are consumed toolog and hello more traces are produced.</span></span> <span data-ttu-id="2d62c-123">Kontrollera att **allvarlighetsgrad** är konfigurerade toohello rätt detaljnivå för en produktion eller hög belastning på platsen.</span><span class="sxs-lookup"><span data-stu-id="2d62c-123">Make sure **severity level** is configured toohello correct verbosity for a production or high traffic site.</span></span> 
> 
> 

<span data-ttu-id="2d62c-124">![][StreamingLogsScreenshot]tooview hello **direktuppspelningsloggar** från inom hello Azure-portalen klickar du på **(1) loggström** även i hello **övervakning** hello Inställningar-menyn.</span><span class="sxs-lookup"><span data-stu-id="2d62c-124">![][StreamingLogsScreenshot] tooview hello **streaming logs** from within hello Azure portal, click on **(1) Log Stream** also in hello **Monitoring** section of hello settings menu.</span></span> <span data-ttu-id="2d62c-125">Om din app aktivt skriver trace-satser, så du bör se dem i hello **(2) strömning loggar UI** i nära realtid.</span><span class="sxs-lookup"><span data-stu-id="2d62c-125">If your app is actively writing trace statements, then you should see them in hello **(2) streaming logs UI** in near real time.</span></span>

## <a name="console"></a><span data-ttu-id="2d62c-126">Konsolen</span><span class="sxs-lookup"><span data-stu-id="2d62c-126">Console</span></span>
<span data-ttu-id="2d62c-127">Hej **Azure-portalen** ger tooyour åtkomst-konsolapp.</span><span class="sxs-lookup"><span data-stu-id="2d62c-127">hello **Azure portal** provides console access tooyour app.</span></span> <span data-ttu-id="2d62c-128">Du kan utforska appens filsystemet och köra powershell/cmd-skript.</span><span class="sxs-lookup"><span data-stu-id="2d62c-128">You can explore your app's file system and run powershell/cmd scripts.</span></span> <span data-ttu-id="2d62c-129">Du är bundna av hello samma behörigheter som angetts som din Appkod som körs när konsolkommandon.</span><span class="sxs-lookup"><span data-stu-id="2d62c-129">You are bound by hello same permissions set as your running app code when executing console commands.</span></span> <span data-ttu-id="2d62c-130">Åtkomst tooprotected kataloger eller skript som körs som kräver utökade behörigheter har blockerats.</span><span class="sxs-lookup"><span data-stu-id="2d62c-130">Access tooprotected directories or running scripts that require elevated permissions is blocked.</span></span>  

<span data-ttu-id="2d62c-131">![][ConsoleScreenshot]Från inställningsmenyn rulla nedåt för**utvecklingsverktyg** avsnittet och klicka på **(1) konsolen** och hello **(2) konsolen** UI öppnar toohello höger.</span><span class="sxs-lookup"><span data-stu-id="2d62c-131">![][ConsoleScreenshot] From settings menu, scroll down too**Development Tools** section and click on **(1) Console** and hello **(2) console** UI opens toohello right.</span></span>

<span data-ttu-id="2d62c-132">Bekanta dig med hello tooget **konsolen**, försök grundläggande kommandon som:</span><span class="sxs-lookup"><span data-stu-id="2d62c-132">tooget familiar with hello **console**, try basic commands like:</span></span>

`````````````````````````
dir
`````````````````````````

`````````````````````````
cd
`````````````````````````

<!-- Images. -->
[DiagnosticsLogs]: ./media/web-sites-streaming-logs-and-console/diagnostic-logs.png
[BrowseSitesScreenshot]: ./media/web-sites-streaming-logs-and-console/browse-sites.png
[StreamingLogsScreenshot]: ./media/web-sites-streaming-logs-and-console/streaming-logs.png
[ConsoleScreenshot]: ./media/web-sites-streaming-logs-and-console/console.png
