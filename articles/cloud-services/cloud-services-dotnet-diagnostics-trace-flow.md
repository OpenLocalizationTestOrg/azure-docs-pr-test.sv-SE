---
title: "aaaTrace hello flödet i Cloud Services-program med Azure-diagnostik | Microsoft Docs"
description: "Lägg till spårning meddelanden tooan Azure-program toohelp felsökning, mäta prestanda, övervakning, trafik analys och mer."
services: cloud-services
documentationcenter: .net
author: rboucher
manager: jwhit
editor: 
ms.assetid: 09934772-cc07-4fd2-ba88-b224ca192f8e
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/20/2016
ms.author: robb
ms.openlocfilehash: d2ed7b5997ae1d298115b4ce593bb5051a9a0c75
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="trace-hello-flow-of-a-cloud-services-application-with-azure-diagnostics"></a><span data-ttu-id="369f5-103">Spåra hello flödet av ett Cloud Services-program med Azure-diagnostik</span><span class="sxs-lookup"><span data-stu-id="369f5-103">Trace hello flow of a Cloud Services application with Azure Diagnostics</span></span>
<span data-ttu-id="369f5-104">Spårning är ett sätt för du toomonitor hello körningen av program när den körs.</span><span class="sxs-lookup"><span data-stu-id="369f5-104">Tracing is a way for you toomonitor hello execution of your application while it is running.</span></span> <span data-ttu-id="369f5-105">Du kan använda hello [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx), [System.Diagnostics.Debug](https://msdn.microsoft.com/library/system.diagnostics.debug.aspx), och [System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx) klasserna toorecord information om fel och program som körs i loggar, textfiler eller andra enheter för senare analys.</span><span class="sxs-lookup"><span data-stu-id="369f5-105">You can use hello [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx), [System.Diagnostics.Debug](https://msdn.microsoft.com/library/system.diagnostics.debug.aspx), and [System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx) classes toorecord information about errors and application execution in logs, text files, or other devices for later analysis.</span></span> <span data-ttu-id="369f5-106">Mer information om spårning finns [spårning och instrumentering program](https://msdn.microsoft.com/library/zs6s4h68.aspx).</span><span class="sxs-lookup"><span data-stu-id="369f5-106">For more information about tracing, see [Tracing and Instrumenting Applications](https://msdn.microsoft.com/library/zs6s4h68.aspx).</span></span>

## <a name="use-trace-statements-and-trace-switches"></a><span data-ttu-id="369f5-107">Använda trace-satser och spåra växlar</span><span class="sxs-lookup"><span data-stu-id="369f5-107">Use trace statements and trace switches</span></span>
<span data-ttu-id="369f5-108">Implementera spårning i tillämpningsprogrammet molntjänster genom att lägga till hello [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) toohello programkonfigurationen och gör anrop tooSystem.Diagnostics.Trace eller System.Diagnostics.Debug i din programkod.</span><span class="sxs-lookup"><span data-stu-id="369f5-108">Implement tracing in your Cloud Services application by adding hello [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) toohello application configuration and making calls tooSystem.Diagnostics.Trace or System.Diagnostics.Debug in your application code.</span></span> <span data-ttu-id="369f5-109">Använd hello konfigurationsfilen *app.config* för arbetsroller och hello *web.config* för webbroller.</span><span class="sxs-lookup"><span data-stu-id="369f5-109">Use hello configuration file *app.config* for worker roles and hello *web.config* for web roles.</span></span> <span data-ttu-id="369f5-110">När du skapar en ny värdbaserad tjänst med en mall för Visual Studio toohello projektet läggs automatiskt till Azure-diagnostik och hello DiagnosticMonitorTraceListener läggs toohello lämplig konfigurationsfilen för hello roller som du lägger till.</span><span class="sxs-lookup"><span data-stu-id="369f5-110">When you create a new hosted service using a Visual Studio template, Azure Diagnostics is automatically added toohello project and hello DiagnosticMonitorTraceListener is added toohello appropriate configuration file for hello roles that you add.</span></span>

<span data-ttu-id="369f5-111">Information om att placera trace-satser finns [så här: Lägg till Trace-satser tooApplication koden](https://msdn.microsoft.com/library/zd83saa2.aspx).</span><span class="sxs-lookup"><span data-stu-id="369f5-111">For information on placing trace statements, see [How to: Add Trace Statements tooApplication Code](https://msdn.microsoft.com/library/zd83saa2.aspx).</span></span>

<span data-ttu-id="369f5-112">Genom att placera [Trace växlar](https://msdn.microsoft.com/library/3at424ac.aspx) i koden, du kan styra om spårning sker och hur omfattande är.</span><span class="sxs-lookup"><span data-stu-id="369f5-112">By placing [Trace Switches](https://msdn.microsoft.com/library/3at424ac.aspx) in your code, you can control whether tracing occurs and how extensive it is.</span></span> <span data-ttu-id="369f5-113">På så sätt kan du övervaka hello status för ditt program i en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="369f5-113">This lets you monitor hello status of your application in a production environment.</span></span> <span data-ttu-id="369f5-114">Detta är särskilt viktigt i ett affärsprogram som använder flera komponenter som körs på flera datorer.</span><span class="sxs-lookup"><span data-stu-id="369f5-114">This is especially important in a business application that uses multiple components running on multiple computers.</span></span> <span data-ttu-id="369f5-115">Mer information finns i [så här: konfigurera spårning växlar](https://msdn.microsoft.com/library/t06xyy08.aspx).</span><span class="sxs-lookup"><span data-stu-id="369f5-115">For more information, see [How to: Configure Trace Switches](https://msdn.microsoft.com/library/t06xyy08.aspx).</span></span>

## <a name="configure-hello-trace-listener-in-an-azure-application"></a><span data-ttu-id="369f5-116">Konfigurera hello spårningslyssnaren i ett Azure-program</span><span class="sxs-lookup"><span data-stu-id="369f5-116">Configure hello trace listener in an Azure application</span></span>
<span data-ttu-id="369f5-117">Spåra, felsökning och TraceSource, måste du ställa in ”lyssnare” toocollect och registrera hello-meddelanden som skickas.</span><span class="sxs-lookup"><span data-stu-id="369f5-117">Trace, Debug and TraceSource, require you set up "listeners" toocollect and record hello messages that are sent.</span></span> <span data-ttu-id="369f5-118">Lyssnare samla in, lagra och vidarebefordra spårning av meddelanden.</span><span class="sxs-lookup"><span data-stu-id="369f5-118">Listeners collect, store, and route tracing messages.</span></span> <span data-ttu-id="369f5-119">De direkt hello spårning utdata tooan lämpliga mål, till exempel en logg, fönster eller textfil.</span><span class="sxs-lookup"><span data-stu-id="369f5-119">They direct hello tracing output tooan appropriate target, such as a log, window, or text file.</span></span> <span data-ttu-id="369f5-120">Azure Diagnostics använder hello [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) klass.</span><span class="sxs-lookup"><span data-stu-id="369f5-120">Azure Diagnostics uses hello [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) class.</span></span>

<span data-ttu-id="369f5-121">Innan du slutför hello följer proceduren måste du initiera hello Azure diagnostikövervakare.</span><span class="sxs-lookup"><span data-stu-id="369f5-121">Before you complete hello following procedure, you must initialize hello Azure diagnostic monitor.</span></span> <span data-ttu-id="369f5-122">toodo detta, se [aktiverar diagnostik i Microsoft Azure](cloud-services-dotnet-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="369f5-122">toodo this, see [Enabling Diagnostics in Microsoft Azure](cloud-services-dotnet-diagnostics.md).</span></span>

<span data-ttu-id="369f5-123">Observera att om du använder hello-mallar som tillhandahålls av Visual Studio hello konfigurationen av hello lyssnare läggs automatiskt åt dig.</span><span class="sxs-lookup"><span data-stu-id="369f5-123">Note that if you use hello templates that are provided by Visual Studio, hello configuration of hello listener is added automatically for you.</span></span>

### <a name="add-a-trace-listener"></a><span data-ttu-id="369f5-124">Lägga till en spårningsavlyssningen</span><span class="sxs-lookup"><span data-stu-id="369f5-124">Add a trace listener</span></span>
1. <span data-ttu-id="369f5-125">Öppna hello web.config eller app.config-filen för din roll.</span><span class="sxs-lookup"><span data-stu-id="369f5-125">Open hello web.config or app.config file for your role.</span></span>
2. <span data-ttu-id="369f5-126">Lägg till följande kod toohello hello.</span><span class="sxs-lookup"><span data-stu-id="369f5-126">Add hello following code toohello file.</span></span> <span data-ttu-id="369f5-127">Ändra hello attributet toouse hello versionsnummer hello-paketet som du refererar till.</span><span class="sxs-lookup"><span data-stu-id="369f5-127">Change hello Version attribute toouse hello version number of hello assembly you are referencing.</span></span> <span data-ttu-id="369f5-128">hello Sammansättningsversionen ändras inte nödvändigtvis i varje version av Azure SDK om det finns uppdateringar tooit.</span><span class="sxs-lookup"><span data-stu-id="369f5-128">hello assembly version does not necessarily change with each Azure SDK release unless there are updates tooit.</span></span>
   
    ```
    <system.diagnostics>
        <trace>
            <listeners>
                <add type="Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitorTraceListener,
                  Microsoft.WindowsAzure.Diagnostics,
                  Version=2.8.0.0,
                  Culture=neutral,
                  PublicKeyToken=31bf3856ad364e35"
                  name="AzureDiagnostics">
                    <filter type="" />
                </add>
            </listeners>
        </trace>
    </system.diagnostics>
    ```
   > [!IMPORTANT]
   > <span data-ttu-id="369f5-129">Kontrollera att du har ett projekt referens toohello Microsoft.WindowsAzure.Diagnostics sammansättning.</span><span class="sxs-lookup"><span data-stu-id="369f5-129">Make sure you have a project reference toohello Microsoft.WindowsAzure.Diagnostics assembly.</span></span> <span data-ttu-id="369f5-130">Uppdatera hello versionsnumret i hello xml ovan toomatch hello version av hello refereras Microsoft.WindowsAzure.Diagnostics sammansättning.</span><span class="sxs-lookup"><span data-stu-id="369f5-130">Update hello version number in hello xml above toomatch hello version of hello referenced Microsoft.WindowsAzure.Diagnostics assembly.</span></span>
   > 
   > 
3. <span data-ttu-id="369f5-131">Spara hello config-fil.</span><span class="sxs-lookup"><span data-stu-id="369f5-131">Save hello config file.</span></span>

<span data-ttu-id="369f5-132">Läs mer om lyssnare [Samlingsspårlyssnarna](https://msdn.microsoft.com/library/4y5y10s7.aspx).</span><span class="sxs-lookup"><span data-stu-id="369f5-132">For more information about listeners, see [Trace Listeners](https://msdn.microsoft.com/library/4y5y10s7.aspx).</span></span>

<span data-ttu-id="369f5-133">Du kan lägga till Spårningskod instruktioner tooyour när du har slutfört hello steg tooadd hello lyssnare.</span><span class="sxs-lookup"><span data-stu-id="369f5-133">After you complete hello steps tooadd hello listener, you can add trace statements tooyour code.</span></span>

### <a name="tooadd-trace-statement-tooyour-code"></a><span data-ttu-id="369f5-134">tooadd Spårningskod instruktionen tooyour</span><span class="sxs-lookup"><span data-stu-id="369f5-134">tooadd trace statement tooyour code</span></span>
1. <span data-ttu-id="369f5-135">Öppna en källfil för programmet.</span><span class="sxs-lookup"><span data-stu-id="369f5-135">Open a source file for your application.</span></span> <span data-ttu-id="369f5-136">Till exempel hello <RoleName>.cs filen för hello worker-rollen eller webbroll.</span><span class="sxs-lookup"><span data-stu-id="369f5-136">For example, hello <RoleName>.cs file for hello worker role or web role.</span></span>
2. <span data-ttu-id="369f5-137">Lägg till hello följande med instruktionen om det inte redan lagts:</span><span class="sxs-lookup"><span data-stu-id="369f5-137">Add hello following using statement if it has not already been added:</span></span>
    ```
        using System.Diagnostics;
    ```
3. <span data-ttu-id="369f5-138">Lägg till Trace-satser där du vill toocapture information om hello tillståndet för programmet.</span><span class="sxs-lookup"><span data-stu-id="369f5-138">Add Trace statements where you want toocapture information about hello state of your application.</span></span> <span data-ttu-id="369f5-139">Du kan använda olika metoder tooformat hello utdata från hello spårningsinstruktionen.</span><span class="sxs-lookup"><span data-stu-id="369f5-139">You can use a variety of methods tooformat hello output of hello Trace statement.</span></span> <span data-ttu-id="369f5-140">Mer information finns i [så här: Lägg till Trace-satser tooApplication koden](https://msdn.microsoft.com/library/zd83saa2.aspx).</span><span class="sxs-lookup"><span data-stu-id="369f5-140">For more information, see [How to: Add Trace Statements tooApplication Code](https://msdn.microsoft.com/library/zd83saa2.aspx).</span></span>
4. <span data-ttu-id="369f5-141">Spara hello källfilen.</span><span class="sxs-lookup"><span data-stu-id="369f5-141">Save hello source file.</span></span>

