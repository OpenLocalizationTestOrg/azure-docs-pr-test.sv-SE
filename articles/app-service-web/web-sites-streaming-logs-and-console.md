---
title: Direktuppspelningsloggar och konsolen
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
ms.openlocfilehash: 120ce6b115820728b9f853e9ff349beb0ef29c9d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="streaming-logs-and-the-console"></a><span data-ttu-id="688d2-103">Direktuppspelningsloggar och konsolen</span><span class="sxs-lookup"><span data-stu-id="688d2-103">Streaming Logs and the Console</span></span>
## <a name="streaming-logs"></a><span data-ttu-id="688d2-104">Direktuppspelningsloggar</span><span class="sxs-lookup"><span data-stu-id="688d2-104">Streaming Logs</span></span>
<span data-ttu-id="688d2-105">Den **Azure-portalen** ger en integrerad strömmande Loggvisaren där du kan visa spårninghändelser från din **Apptjänst** appar i realtid.</span><span class="sxs-lookup"><span data-stu-id="688d2-105">The **Azure portal** provides an integrated streaming log viewer that lets you view tracing events from your **App Service** apps in real time.</span></span>  

<span data-ttu-id="688d2-106">Ställa in den här funktionen kräver några enkla steg:</span><span class="sxs-lookup"><span data-stu-id="688d2-106">Setting up this feature requires a few simple steps:</span></span>

* <span data-ttu-id="688d2-107">Skriva spårningar i koden</span><span class="sxs-lookup"><span data-stu-id="688d2-107">Write traces in your code</span></span>
* <span data-ttu-id="688d2-108">Aktivera programmet **diagnostikloggar** för din app</span><span class="sxs-lookup"><span data-stu-id="688d2-108">Enable Application **Diagnostic Logs** for your app</span></span>
* <span data-ttu-id="688d2-109">Visa ström från inbyggt **strömning loggar** Användargränssnittet i den **Azure-portalen**.</span><span class="sxs-lookup"><span data-stu-id="688d2-109">View the stream from the built-in **Streaming Logs** UI in the **Azure portal**.</span></span>

### <a name="how-to-write-traces-in-your-code"></a><span data-ttu-id="688d2-110">Hur du skriver spårningar i koden</span><span class="sxs-lookup"><span data-stu-id="688d2-110">How to write traces in your code</span></span>
<span data-ttu-id="688d2-111">Det är enkelt att skriva spårningar i koden.</span><span class="sxs-lookup"><span data-stu-id="688d2-111">Writing traces in your code is easy.</span></span>  <span data-ttu-id="688d2-112">Det är lika enkelt som att skriva följande kod i C#:</span><span class="sxs-lookup"><span data-stu-id="688d2-112">In C# it's as easy as writing the following code:</span></span>

`````````````````````````
Trace.TraceInformation("My trace statement");
`````````````````````````

`````````````````````````
Trace.TraceWarning("My warning statement");
`````````````````````````

`````````````````````````
Trace.TraceError("My error statement");
`````````````````````````

<span data-ttu-id="688d2-113">Klassen Trace bor i namnområdet System.Diagnostics.</span><span class="sxs-lookup"><span data-stu-id="688d2-113">The Trace class lives in the System.Diagnostics namespace.</span></span>

<span data-ttu-id="688d2-114">Du kan skriva koden för att uppnå samma resultat i en node.js-app:</span><span class="sxs-lookup"><span data-stu-id="688d2-114">In a node.js app you can write this code to achieve the same result:</span></span>

`````````````````````````
console.log("My trace statement").
`````````````````````````

### <a name="how-to-enable-and-view-the-streaming-logs"></a><span data-ttu-id="688d2-115">Aktivera och visa strömning loggar</span><span class="sxs-lookup"><span data-stu-id="688d2-115">How to enable and view the streaming logs</span></span>
<span data-ttu-id="688d2-116">![][BrowseSitesScreenshot]Diagnostik är aktiverade på grundval av per app.</span><span class="sxs-lookup"><span data-stu-id="688d2-116">![][BrowseSitesScreenshot] Diagnostics are enabled on a per app basis.</span></span> <span data-ttu-id="688d2-117">Starta genom att bläddra till platsen som du vill aktivera den här funktionen på.</span><span class="sxs-lookup"><span data-stu-id="688d2-117">Start by browsing to the site you would like to enable this feature on.</span></span>  

<span data-ttu-id="688d2-118">![][DiagnosticsLogs]Från inställningsmenyn, bläddra till den **övervakning** avsnittet och klicka på **(1) diagnostikloggar**.</span><span class="sxs-lookup"><span data-stu-id="688d2-118">![][DiagnosticsLogs] From settings menu, scroll down to the **Monitoring** section and click on **(1) Diagnostic Logs**.</span></span> <span data-ttu-id="688d2-119">Sedan **(2) Aktivera** **programloggning (filsystem)** eller **programloggning (blob)** den **nivå** alternativet kan du ändra den allvarlighetsgraden spårningar att avbilda.</span><span class="sxs-lookup"><span data-stu-id="688d2-119">Then **(2) enable** **Application Logging (Filesystem)** or **Application Logging (blob)** The **Level** option lets you change the severity level of traces to capture.</span></span> <span data-ttu-id="688d2-120">Om du försöker bara bekanta dig med funktionen, ställa in på **utförlig** så alla trace-satser som samlas in.</span><span class="sxs-lookup"><span data-stu-id="688d2-120">If you're just trying to get familiar with the feature, set the level to **Verbose** to ensure all of your trace statements are collected.</span></span>

<span data-ttu-id="688d2-121">Klicka på **spara** längst upp på bladet och du är redo att visa loggar.</span><span class="sxs-lookup"><span data-stu-id="688d2-121">Click **SAVE** at the top of the blade and you're ready to view logs.</span></span>

> [!NOTE]
> <span data-ttu-id="688d2-122">Det högre av **allvarlighetsgrad** mer spåren produceras och mer resurser används för att logga.</span><span class="sxs-lookup"><span data-stu-id="688d2-122">The higher the **severity level** the more resources are consumed to log and the more traces are produced.</span></span> <span data-ttu-id="688d2-123">Kontrollera att **allvarlighetsgrad** konfigureras till att rätt detaljnivå för en produktion eller hög belastning på platsen.</span><span class="sxs-lookup"><span data-stu-id="688d2-123">Make sure **severity level** is configured to the correct verbosity for a production or high traffic site.</span></span> 
> 
> 

<span data-ttu-id="688d2-124">![][StreamingLogsScreenshot]Visa den **direktuppspelningsloggar** från inom Azure-portalen klickar du på **(1) loggström** även i den **övervakning** avsnitt i inställningsmenyn.</span><span class="sxs-lookup"><span data-stu-id="688d2-124">![][StreamingLogsScreenshot] To view the **streaming logs** from within the Azure portal, click on **(1) Log Stream** also in the **Monitoring** section of the settings menu.</span></span> <span data-ttu-id="688d2-125">Om din app aktivt skriver trace-satser, så du bör se dem i den **(2) strömning loggar UI** i nära realtid.</span><span class="sxs-lookup"><span data-stu-id="688d2-125">If your app is actively writing trace statements, then you should see them in the **(2) streaming logs UI** in near real time.</span></span>

## <a name="console"></a><span data-ttu-id="688d2-126">Konsolen</span><span class="sxs-lookup"><span data-stu-id="688d2-126">Console</span></span>
<span data-ttu-id="688d2-127">Den **Azure-portalen** ger åtkomst till din app.</span><span class="sxs-lookup"><span data-stu-id="688d2-127">The **Azure portal** provides console access to your app.</span></span> <span data-ttu-id="688d2-128">Du kan utforska appens filsystemet och köra powershell/cmd-skript.</span><span class="sxs-lookup"><span data-stu-id="688d2-128">You can explore your app's file system and run powershell/cmd scripts.</span></span> <span data-ttu-id="688d2-129">Du är bundna av samma behörigheter som din Appkod som körs när konsolkommandon.</span><span class="sxs-lookup"><span data-stu-id="688d2-129">You are bound by the same permissions set as your running app code when executing console commands.</span></span> <span data-ttu-id="688d2-130">Åtkomst till skyddade kataloger eller skript som körs som kräver förhöjd behörighet har blockerats.</span><span class="sxs-lookup"><span data-stu-id="688d2-130">Access to protected directories or running scripts that require elevated permissions is blocked.</span></span>  

<span data-ttu-id="688d2-131">![][ConsoleScreenshot]Rulla ned till Inställningar-menyn **utvecklingsverktyg** avsnittet och klicka på **(1) konsolen** och **(2) konsolen** UI öppnas till höger.</span><span class="sxs-lookup"><span data-stu-id="688d2-131">![][ConsoleScreenshot] From settings menu, scroll down to **Development Tools** section and click on **(1) Console** and the **(2) console** UI opens to the right.</span></span>

<span data-ttu-id="688d2-132">Att bekanta dig med de **konsolen**, försök grundläggande kommandon som:</span><span class="sxs-lookup"><span data-stu-id="688d2-132">To get familiar with the **console**, try basic commands like:</span></span>

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
