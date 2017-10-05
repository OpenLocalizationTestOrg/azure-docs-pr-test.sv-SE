---
title: "Spåra flödet i Cloud Services-program med Azure-diagnostik | Microsoft Docs"
description: "Lägga till spårning av meddelanden till ett Azure-program för att felsöka mäta prestanda, övervakning, trafik analys och mer."
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
ms.openlocfilehash: 35b4a4270846c54a1ca760e803ef7adba60cf03b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="trace-the-flow-of-a-cloud-services-application-with-azure-diagnostics"></a><span data-ttu-id="6dcd7-103">Spåra flödet av ett Cloud Services-program med Azure-diagnostik</span><span class="sxs-lookup"><span data-stu-id="6dcd7-103">Trace the flow of a Cloud Services application with Azure Diagnostics</span></span>
<span data-ttu-id="6dcd7-104">Spårning är ett sätt att övervaka körning av ditt program när den körs.</span><span class="sxs-lookup"><span data-stu-id="6dcd7-104">Tracing is a way for you to monitor the execution of your application while it is running.</span></span> <span data-ttu-id="6dcd7-105">Du kan använda den [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx), [System.Diagnostics.Debug](https://msdn.microsoft.com/library/system.diagnostics.debug.aspx), och [System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx) klasser för att registrera information om fel och program som körs i loggar, textfiler eller andra enheter för senare analys.</span><span class="sxs-lookup"><span data-stu-id="6dcd7-105">You can use the [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx), [System.Diagnostics.Debug](https://msdn.microsoft.com/library/system.diagnostics.debug.aspx), and [System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx) classes to record information about errors and application execution in logs, text files, or other devices for later analysis.</span></span> <span data-ttu-id="6dcd7-106">Mer information om spårning finns [spårning och instrumentering program](https://msdn.microsoft.com/library/zs6s4h68.aspx).</span><span class="sxs-lookup"><span data-stu-id="6dcd7-106">For more information about tracing, see [Tracing and Instrumenting Applications](https://msdn.microsoft.com/library/zs6s4h68.aspx).</span></span>

## <a name="use-trace-statements-and-trace-switches"></a><span data-ttu-id="6dcd7-107">Använda trace-satser och spåra växlar</span><span class="sxs-lookup"><span data-stu-id="6dcd7-107">Use trace statements and trace switches</span></span>
<span data-ttu-id="6dcd7-108">Implementera spårning i tillämpningsprogrammet molntjänster genom att lägga till den [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) programkonfigurationen och anropar System.Diagnostics.Trace eller System.Diagnostics.Debug i din programkod.</span><span class="sxs-lookup"><span data-stu-id="6dcd7-108">Implement tracing in your Cloud Services application by adding the [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) to the application configuration and making calls to System.Diagnostics.Trace or System.Diagnostics.Debug in your application code.</span></span> <span data-ttu-id="6dcd7-109">Använda konfigurationsfilen *app.config* för arbetsroller och *web.config* för webbroller.</span><span class="sxs-lookup"><span data-stu-id="6dcd7-109">Use the configuration file *app.config* for worker roles and the *web.config* for web roles.</span></span> <span data-ttu-id="6dcd7-110">När du skapar en ny värdbaserad tjänst med en mall för Visual Studio Azure Diagnostics läggs automatiskt till projektet och DiagnosticMonitorTraceListener har lagts till i lämpliga konfigurationsfilen för de roller som du lägger till.</span><span class="sxs-lookup"><span data-stu-id="6dcd7-110">When you create a new hosted service using a Visual Studio template, Azure Diagnostics is automatically added to the project and the DiagnosticMonitorTraceListener is added to the appropriate configuration file for the roles that you add.</span></span>

<span data-ttu-id="6dcd7-111">Information om att placera trace-satser finns [så här: Lägg till Trace-satser för programkoden](https://msdn.microsoft.com/library/zd83saa2.aspx).</span><span class="sxs-lookup"><span data-stu-id="6dcd7-111">For information on placing trace statements, see [How to: Add Trace Statements to Application Code](https://msdn.microsoft.com/library/zd83saa2.aspx).</span></span>

<span data-ttu-id="6dcd7-112">Genom att placera [Trace växlar](https://msdn.microsoft.com/library/3at424ac.aspx) i koden, du kan styra om spårning sker och hur omfattande är.</span><span class="sxs-lookup"><span data-stu-id="6dcd7-112">By placing [Trace Switches](https://msdn.microsoft.com/library/3at424ac.aspx) in your code, you can control whether tracing occurs and how extensive it is.</span></span> <span data-ttu-id="6dcd7-113">På så sätt kan du övervaka status för ditt program i en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="6dcd7-113">This lets you monitor the status of your application in a production environment.</span></span> <span data-ttu-id="6dcd7-114">Detta är särskilt viktigt i ett affärsprogram som använder flera komponenter som körs på flera datorer.</span><span class="sxs-lookup"><span data-stu-id="6dcd7-114">This is especially important in a business application that uses multiple components running on multiple computers.</span></span> <span data-ttu-id="6dcd7-115">Mer information finns i [så här: konfigurera spårning växlar](https://msdn.microsoft.com/library/t06xyy08.aspx).</span><span class="sxs-lookup"><span data-stu-id="6dcd7-115">For more information, see [How to: Configure Trace Switches](https://msdn.microsoft.com/library/t06xyy08.aspx).</span></span>

## <a name="configure-the-trace-listener-in-an-azure-application"></a><span data-ttu-id="6dcd7-116">Konfigurera spårningslyssnaren i ett Azure-program</span><span class="sxs-lookup"><span data-stu-id="6dcd7-116">Configure the trace listener in an Azure application</span></span>
<span data-ttu-id="6dcd7-117">Spåra, felsökning och TraceSource, måste du ställa in ”lyssnare” för att samla in och registrera de meddelanden som skickas.</span><span class="sxs-lookup"><span data-stu-id="6dcd7-117">Trace, Debug and TraceSource, require you set up "listeners" to collect and record the messages that are sent.</span></span> <span data-ttu-id="6dcd7-118">Lyssnare samla in, lagra och vidarebefordra spårning av meddelanden.</span><span class="sxs-lookup"><span data-stu-id="6dcd7-118">Listeners collect, store, and route tracing messages.</span></span> <span data-ttu-id="6dcd7-119">De direkt spårning utdata till en lämplig mål, till exempel en logg, fönster eller textfil.</span><span class="sxs-lookup"><span data-stu-id="6dcd7-119">They direct the tracing output to an appropriate target, such as a log, window, or text file.</span></span> <span data-ttu-id="6dcd7-120">Azure Diagnostics använder den [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) klass.</span><span class="sxs-lookup"><span data-stu-id="6dcd7-120">Azure Diagnostics uses the [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) class.</span></span>

<span data-ttu-id="6dcd7-121">Innan du slutför följande procedur måste du initiera Azure diagnostikövervakare.</span><span class="sxs-lookup"><span data-stu-id="6dcd7-121">Before you complete the following procedure, you must initialize the Azure diagnostic monitor.</span></span> <span data-ttu-id="6dcd7-122">För att göra detta, se [aktiverar diagnostik i Microsoft Azure](cloud-services-dotnet-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="6dcd7-122">To do this, see [Enabling Diagnostics in Microsoft Azure](cloud-services-dotnet-diagnostics.md).</span></span>

<span data-ttu-id="6dcd7-123">Observera att om du använder mallar som tillhandahålls av Visual Studio, konfigurationen av lyssnaren läggs automatiskt åt dig.</span><span class="sxs-lookup"><span data-stu-id="6dcd7-123">Note that if you use the templates that are provided by Visual Studio, the configuration of the listener is added automatically for you.</span></span>

### <a name="add-a-trace-listener"></a><span data-ttu-id="6dcd7-124">Lägga till en spårningsavlyssningen</span><span class="sxs-lookup"><span data-stu-id="6dcd7-124">Add a trace listener</span></span>
1. <span data-ttu-id="6dcd7-125">Öppna filen web.config eller app.config för din roll.</span><span class="sxs-lookup"><span data-stu-id="6dcd7-125">Open the web.config or app.config file for your role.</span></span>
2. <span data-ttu-id="6dcd7-126">Lägg till följande kod i filen.</span><span class="sxs-lookup"><span data-stu-id="6dcd7-126">Add the following code to the file.</span></span> <span data-ttu-id="6dcd7-127">Ändra attributet Version om du vill använda versionsnumret för sammansättningen som du refererar till.</span><span class="sxs-lookup"><span data-stu-id="6dcd7-127">Change the Version attribute to use the version number of the assembly you are referencing.</span></span> <span data-ttu-id="6dcd7-128">Sammansättningens version ändras inte nödvändigtvis i varje version av Azure SDK om det finns uppdateringar till den.</span><span class="sxs-lookup"><span data-stu-id="6dcd7-128">The assembly version does not necessarily change with each Azure SDK release unless there are updates to it.</span></span>
   
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
   > <span data-ttu-id="6dcd7-129">Kontrollera att du har en projektreferens till sammansättningen Microsoft.WindowsAzure.Diagnostics.</span><span class="sxs-lookup"><span data-stu-id="6dcd7-129">Make sure you have a project reference to the Microsoft.WindowsAzure.Diagnostics assembly.</span></span> <span data-ttu-id="6dcd7-130">Uppdatera versionsnumret i xml-ovan för att matcha versionen av den refererade sammansättningen Microsoft.WindowsAzure.Diagnostics.</span><span class="sxs-lookup"><span data-stu-id="6dcd7-130">Update the version number in the xml above to match the version of the referenced Microsoft.WindowsAzure.Diagnostics assembly.</span></span>
   > 
   > 
3. <span data-ttu-id="6dcd7-131">Spara konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="6dcd7-131">Save the config file.</span></span>

<span data-ttu-id="6dcd7-132">Läs mer om lyssnare [Samlingsspårlyssnarna](https://msdn.microsoft.com/library/4y5y10s7.aspx).</span><span class="sxs-lookup"><span data-stu-id="6dcd7-132">For more information about listeners, see [Trace Listeners](https://msdn.microsoft.com/library/4y5y10s7.aspx).</span></span>

<span data-ttu-id="6dcd7-133">När du har slutfört stegen för att lägga till lyssnaren kan du lägga till trace-satser din kod.</span><span class="sxs-lookup"><span data-stu-id="6dcd7-133">After you complete the steps to add the listener, you can add trace statements to your code.</span></span>

### <a name="to-add-trace-statement-to-your-code"></a><span data-ttu-id="6dcd7-134">Att lägga till spårningsinstruktionen i koden</span><span class="sxs-lookup"><span data-stu-id="6dcd7-134">To add trace statement to your code</span></span>
1. <span data-ttu-id="6dcd7-135">Öppna en källfil för programmet.</span><span class="sxs-lookup"><span data-stu-id="6dcd7-135">Open a source file for your application.</span></span> <span data-ttu-id="6dcd7-136">Till exempel den <RoleName>.cs-fil för arbetsrollen eller webbroll.</span><span class="sxs-lookup"><span data-stu-id="6dcd7-136">For example, the <RoleName>.cs file for the worker role or web role.</span></span>
2. <span data-ttu-id="6dcd7-137">Lägg till följande med instruktionen om det inte redan lagts:</span><span class="sxs-lookup"><span data-stu-id="6dcd7-137">Add the following using statement if it has not already been added:</span></span>
    ```
        using System.Diagnostics;
    ```
3. <span data-ttu-id="6dcd7-138">Lägg till Trace-satser där du vill samla in information om tillståndet för programmet.</span><span class="sxs-lookup"><span data-stu-id="6dcd7-138">Add Trace statements where you want to capture information about the state of your application.</span></span> <span data-ttu-id="6dcd7-139">Du kan använda olika metoder för att formatera utdata från spårningsinstruktionen.</span><span class="sxs-lookup"><span data-stu-id="6dcd7-139">You can use a variety of methods to format the output of the Trace statement.</span></span> <span data-ttu-id="6dcd7-140">Mer information finns i [så här: Lägg till Trace-satser för programkoden](https://msdn.microsoft.com/library/zd83saa2.aspx).</span><span class="sxs-lookup"><span data-stu-id="6dcd7-140">For more information, see [How to: Add Trace Statements to Application Code](https://msdn.microsoft.com/library/zd83saa2.aspx).</span></span>
4. <span data-ttu-id="6dcd7-141">Spara källfilen.</span><span class="sxs-lookup"><span data-stu-id="6dcd7-141">Save the source file.</span></span>

