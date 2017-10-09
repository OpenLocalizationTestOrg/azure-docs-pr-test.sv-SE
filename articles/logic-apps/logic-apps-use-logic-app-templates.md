---
title: "aaaCreate arbetsflöden från mallar - Azure Logic Apps | Microsoft Docs"
description: "Komma igång - snabbt skapa arbetsflöden med hjälp av Azure Logikapp mallar tooconnect appar och integrera data."
author: kevinlam1
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 3656acfb-eefd-4e75-b5d2-73da56c424c9
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: LADocs; klam
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0127523662a12076772b52968f1e2f9cbad72a1b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-workflow-using-a-pre-built-template-or-pattern-tooget-started-quickly"></a><span data-ttu-id="61d43-103">Konfigurera ett arbetsflöde med en förskapad mall eller mönster tooget snabbt igång</span><span class="sxs-lookup"><span data-stu-id="61d43-103">Configure a workflow using a pre-built template or pattern tooget started quickly</span></span>

## <a name="what-are-logic-app-templates"></a><span data-ttu-id="61d43-104">Vad är logic app mallar</span><span class="sxs-lookup"><span data-stu-id="61d43-104">What are logic app templates</span></span>
<span data-ttu-id="61d43-105">En mall för logik-app är en förskapad logikapp som du kan använda tooquickly komma igång med din egen arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="61d43-105">A logic app template is a pre-built logic app that you can use tooquickly get started creating your own workflow.</span></span> 

<span data-ttu-id="61d43-106">Dessa mallar är ett bra sätt toodiscover olika mönster som kan skapas med hjälp av logic apps.</span><span class="sxs-lookup"><span data-stu-id="61d43-106">These templates are a good way toodiscover various patterns that can be built using logic apps.</span></span> <span data-ttu-id="61d43-107">Du kan antingen använda mallarna som-är eller ändra dem. toofit ditt scenario.</span><span class="sxs-lookup"><span data-stu-id="61d43-107">You can either use these templates as-is or modify them toofit your scenario.</span></span>

## <a name="overview-of-available-templates"></a><span data-ttu-id="61d43-108">Översikt över tillgängliga mallar</span><span class="sxs-lookup"><span data-stu-id="61d43-108">Overview of available templates</span></span>
<span data-ttu-id="61d43-109">Det finns många tillgängliga mallar som för närvarande är publicerade i hello logik app plattform.</span><span class="sxs-lookup"><span data-stu-id="61d43-109">There are many available templates currently published in hello logic app platform.</span></span> <span data-ttu-id="61d43-110">Vissa exempel kategorier, samt hello typ av kopplingar som används i dem, i listan nedan.</span><span class="sxs-lookup"><span data-stu-id="61d43-110">Some example categories, as well as hello type of connectors used in them, are listed below.</span></span>

### <a name="enterprise-cloud-templates"></a><span data-ttu-id="61d43-111">Företagsmallar i molnet</span><span class="sxs-lookup"><span data-stu-id="61d43-111">Enterprise cloud templates</span></span>
<span data-ttu-id="61d43-112">Mallar som integrerar Dynamics CRM, Salesforce, rutan, Azure Blob och övriga kopplingar för ditt företag moln behov.</span><span class="sxs-lookup"><span data-stu-id="61d43-112">Templates that integrate Dynamics CRM, Salesforce, Box, Azure Blob, and other connectors for your enterprise cloud needs.</span></span> <span data-ttu-id="61d43-113">Några exempel av vad som kan göras med dessa mallar innehåller ordna leads och säkerhetskopiering av filen för företagets data.</span><span class="sxs-lookup"><span data-stu-id="61d43-113">Some examples of what can be done with these templates include organizing your leads and backing up your corporate file data.</span></span>

### <a name="enterprise-integration-pack-templates"></a><span data-ttu-id="61d43-114">Företagsmallar integration pack</span><span class="sxs-lookup"><span data-stu-id="61d43-114">Enterprise integration pack templates</span></span>
<span data-ttu-id="61d43-115">Konfigurationer av VETER (validera, extrahera, transformera, utöka, vidarebefordra) pipelines, tar emot en X12 EDI-dokumentet via AS2 samt omvandla det tooXML, som X12 och AS2 meddelande hantering.</span><span class="sxs-lookup"><span data-stu-id="61d43-115">Configurations of VETER (validate, extract, transform, enrich, route) pipelines, receiving an X12 EDI document over AS2 and transforming it tooXML, as well as X12 and AS2 message handling.</span></span>

### <a name="protocol-pattern-templates"></a><span data-ttu-id="61d43-116">Protokollet mönster mallar</span><span class="sxs-lookup"><span data-stu-id="61d43-116">Protocol pattern templates</span></span>
<span data-ttu-id="61d43-117">Dessa mallar består av logikappar med protokollet mönster, till exempel begäran och svar över HTTP samt integreringar över FTP- och SFTP.</span><span class="sxs-lookup"><span data-stu-id="61d43-117">These templates consist of logic apps that contain protocol patterns such as request-response over HTTP as well as integrations across FTP and SFTP.</span></span> <span data-ttu-id="61d43-118">Använd dessa eftersom de finns eller som bas för att skapa mer komplexa protocol-mönster.</span><span class="sxs-lookup"><span data-stu-id="61d43-118">Use these as they exist, or as a basis for creating more complex protocol patterns.</span></span>  

### <a name="personal-productivity-templates"></a><span data-ttu-id="61d43-119">Personlig produktivitet mallar</span><span class="sxs-lookup"><span data-stu-id="61d43-119">Personal productivity templates</span></span>
<span data-ttu-id="61d43-120">Mönster toohelp förbättra personlig produktivitet innehåller mallar som dagliga Påminn, göra viktiga arbetsobjekt till att göra-listor och automatisera tidskrävande uppgifter ned tooa enanvändarläge steg.</span><span class="sxs-lookup"><span data-stu-id="61d43-120">Patterns toohelp improve personal productivity include templates that set daily reminders, turn important work items into to-do lists, and automate lengthy tasks down tooa single user approval step.</span></span>

### <a name="consumer-cloud-templates"></a><span data-ttu-id="61d43-121">Konsumenten molnet mallar</span><span class="sxs-lookup"><span data-stu-id="61d43-121">Consumer cloud templates</span></span>
<span data-ttu-id="61d43-122">Enkel mallar som integreras med tjänster för social media som Twitter, Slack och e-post och slutligen kompatibla för att stärka sociala medier marknadsföringskampanjer.</span><span class="sxs-lookup"><span data-stu-id="61d43-122">Simple templates that integrate with social media services such as Twitter, Slack, and email, ultimately capable of strengthening social media marketing initiatives.</span></span> <span data-ttu-id="61d43-123">Dessa inkluderar även mallar, till exempel grumligt kopierar, som kan hjälpa dig att öka produktiviteten genom att spara tid på traditionellt återkommande uppgifter.</span><span class="sxs-lookup"><span data-stu-id="61d43-123">These also include templates such as cloudy copying, which can help increase productivity by saving time spent on traditionally repetitive tasks.</span></span> 

## <a name="how-toocreate-a-logic-app-using-a-template"></a><span data-ttu-id="61d43-124">Hur toocreate en logikapp med en mall</span><span class="sxs-lookup"><span data-stu-id="61d43-124">How toocreate a logic app using a template</span></span>
<span data-ttu-id="61d43-125">tooget igång med en app logikmall gå in hello logik app designer.</span><span class="sxs-lookup"><span data-stu-id="61d43-125">tooget started using a logic app template, go into hello logic app designer.</span></span> <span data-ttu-id="61d43-126">Om du anger hello designer genom att öppna en befintlig logikapp, laddas hello logikapp automatiskt i designern vyn.</span><span class="sxs-lookup"><span data-stu-id="61d43-126">If you're entering hello designer by opening an existing logic app, hello logic app automatically loads in your designer view.</span></span> <span data-ttu-id="61d43-127">Men om du skapar en ny logikapp kan se du hello-skärmen nedan.</span><span class="sxs-lookup"><span data-stu-id="61d43-127">However, if you're creating a new logic app, you see hello screen below.</span></span>  
 ![](../../includes/media/app-service-logic-templates/template7.png)  

<span data-ttu-id="61d43-128">Du kan antingen välja toostart med en tom logikapp eller en förskapad mall från den här skärmen.</span><span class="sxs-lookup"><span data-stu-id="61d43-128">From this screen, you can either choose toostart with a blank logic app or a pre-built template.</span></span> <span data-ttu-id="61d43-129">Om du väljer en av hello mallar tillhandahålls med ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="61d43-129">If you select one of hello templates, you are provided with additional information.</span></span> <span data-ttu-id="61d43-130">I det här exemplet använder vi hello *när en ny fil skapas i Dropbox, kopiera den tooOneDrive* mall.</span><span class="sxs-lookup"><span data-stu-id="61d43-130">In this example, we use hello *When a new file is created in Dropbox, copy it tooOneDrive* template.</span></span>  
 ![](../../includes/media/app-service-logic-templates/template2.png)  

<span data-ttu-id="61d43-131">Om du väljer toouse hello mallen kan bara välja hello *använder den här mallen* knappen.</span><span class="sxs-lookup"><span data-stu-id="61d43-131">If you choose toouse hello template, just select hello *use this template* button.</span></span> <span data-ttu-id="61d43-132">Du ombeds toosign i tooyour konton baserat på vilka kopplingar hello-mallen använder.</span><span class="sxs-lookup"><span data-stu-id="61d43-132">You'll be asked toosign in tooyour accounts based on which connectors hello template utilizes.</span></span> <span data-ttu-id="61d43-133">Eller om du har redan en anslutning med dessa kopplingar kan du välja fortsätta som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="61d43-133">Or, if you've previously established a connection with these connectors, you can select continue as seen below.</span></span>  
 ![](../../includes/media/app-service-logic-templates/template3.png)  

<span data-ttu-id="61d43-134">Efter hello anslutning och välja *fortsätta*, hello logikappen öppnas i designern.</span><span class="sxs-lookup"><span data-stu-id="61d43-134">After establishing hello connection and selecting *continue*, hello logic app opens in designer view.</span></span>  
 ![](../../includes/media/app-service-logic-templates/template4.png)  

<span data-ttu-id="61d43-135">I hello-exemplet ovan, vilket är fallet hello med många mallar kan hello obligatorisk egenskap fälten fyllas i inom hello kopplingar; vissa kan dock fortfarande kräver ett värde innan du kan tooproperly distribuera hello logikapp.</span><span class="sxs-lookup"><span data-stu-id="61d43-135">In hello example above, as is hello case with many templates, some of hello mandatory property fields may be filled out within hello connectors; however, some might still require a value before being able tooproperly deploy hello logic app.</span></span> <span data-ttu-id="61d43-136">Om du försöker toodeploy utan att ange några hello fält saknas, meddelas du med ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="61d43-136">If you try toodeploy without entering some of hello missing fields, you'll be notified with an error message.</span></span>

<span data-ttu-id="61d43-137">Om du inte vill tooreturn toohello mall viewer, Välj hello *mallar* knapp i hello övre navigeringsfältet.</span><span class="sxs-lookup"><span data-stu-id="61d43-137">If you wish tooreturn toohello template viewer, select hello *Templates* button in hello top navigation bar.</span></span> <span data-ttu-id="61d43-138">Genom att växla tillbaka toohello mall viewer förlorar du alla osparade pågår.</span><span class="sxs-lookup"><span data-stu-id="61d43-138">By switching back toohello template viewer, you lose any unsaved progress.</span></span> <span data-ttu-id="61d43-139">Tidigare tooswitching tillbaka till mallen viewer visas ett meddelande om detta varningsmeddelande.</span><span class="sxs-lookup"><span data-stu-id="61d43-139">Prior tooswitching back into template viewer, you'll see a warning message notifying you of this.</span></span>  
 ![](../../includes/media/app-service-logic-templates/template5.png)  

## <a name="how-toodeploy-a-logic-app-created-from-a-template"></a><span data-ttu-id="61d43-140">Hur toodeploy en logikapp skapas från en mall</span><span class="sxs-lookup"><span data-stu-id="61d43-140">How toodeploy a logic app created from a template</span></span>
<span data-ttu-id="61d43-141">När du har lästs in din mall och gjort önskade ändringar, kan du välja hello spara knappen i hello övre vänstra hörnet.</span><span class="sxs-lookup"><span data-stu-id="61d43-141">Once you have loaded your template and made any desired changes, select hello save button in hello upper left corner.</span></span> <span data-ttu-id="61d43-142">Detta sparar och publicerar din logikapp.</span><span class="sxs-lookup"><span data-stu-id="61d43-142">This saves and publishes your logic app.</span></span>  
 ![](../../includes/media/app-service-logic-templates/template6.png)  

<span data-ttu-id="61d43-143">Om du skulle t.ex. Mer information om hur tooadd fler steg i en befintlig mall för logik app eller göra ändringar i allmänhet kan du läsa mer i [skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="61d43-143">If you would like more information on how tooadd more steps into an existing logic app template, or make edits in general, read more at [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

