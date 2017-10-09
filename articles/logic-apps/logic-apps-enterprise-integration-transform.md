---
title: aaaConvert XML-data med transformeringar - Azure Logic Apps | Microsoft Docs
description: "Skapa transformeringar eller mapps tooconvert XML-data mellan formaten i logikappar med hjälp av hello Enterprise Integration-SDK"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: add01429-21bc-4bab-8b23-bc76ba7d0bde
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: b56ec1072c5058d3aefc7f88ac9b2748ebe56456
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enterprise-integration-with-xml-transforms"></a><span data-ttu-id="7082c-103">Enterprise integration med XML-transformeringar</span><span class="sxs-lookup"><span data-stu-id="7082c-103">Enterprise integration with XML transforms</span></span>
## <a name="overview"></a><span data-ttu-id="7082c-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="7082c-104">Overview</span></span>
<span data-ttu-id="7082c-105">hello Enterprise integration transformeringen connector konverterar data från ett format tooanother format.</span><span class="sxs-lookup"><span data-stu-id="7082c-105">hello Enterprise integration Transform connector converts data from one format tooanother format.</span></span> <span data-ttu-id="7082c-106">Du kan till exempel ha ett inkommande meddelande som innehåller hello aktuellt datum hello YearMonthDay format.</span><span class="sxs-lookup"><span data-stu-id="7082c-106">For example, you may have an incoming message that contains hello current date in hello YearMonthDay format.</span></span> <span data-ttu-id="7082c-107">Du kan använda en transformering tooreformat hello datum toobe hello MonthDayYear format.</span><span class="sxs-lookup"><span data-stu-id="7082c-107">You can use a transform tooreformat hello date toobe in hello MonthDayYear format.</span></span>

## <a name="what-does-a-transform-do"></a><span data-ttu-id="7082c-108">Vad är en omvandling?</span><span class="sxs-lookup"><span data-stu-id="7082c-108">What does a transform do?</span></span>
<span data-ttu-id="7082c-109">En transformering som kallas även en karta, består av källa XML-schema (hello indata) och ett mål XML-schema (hello utdata).</span><span class="sxs-lookup"><span data-stu-id="7082c-109">A Transform, which is also known as a map, consists of a Source XML schema (hello input) and a Target XML schema (hello output).</span></span> <span data-ttu-id="7082c-110">Du kan använda olika inbyggda funktioner toohelp ändra eller kontrollera hello data, inklusive strängändringar, villkorlig tilldelningar, aritmetiska uttryck, datum tid formaterare och även slingor konstruktioner.</span><span class="sxs-lookup"><span data-stu-id="7082c-110">You can use different built-in functions toohelp manipulate or control hello data, including string manipulations, conditional assignments, arithmetic expressions, date time formatters, and even looping constructs.</span></span>

## <a name="how-toocreate-a-transform"></a><span data-ttu-id="7082c-111">Hur toocreate en omvandling?</span><span class="sxs-lookup"><span data-stu-id="7082c-111">How toocreate a transform?</span></span>
<span data-ttu-id="7082c-112">Du kan skapa en transformering/karta med hello Visual Studio [Enterprise Integration SDK](https://aka.ms/vsmapsandschemas).</span><span class="sxs-lookup"><span data-stu-id="7082c-112">You can create a transform/map by using hello Visual Studio [Enterprise Integration SDK](https://aka.ms/vsmapsandschemas).</span></span> <span data-ttu-id="7082c-113">När du är klar med att skapa och testa hello transformeringen överför hello transformeringen till ditt konto för integrering.</span><span class="sxs-lookup"><span data-stu-id="7082c-113">When you are finished creating and testing hello transform, you upload hello transform into your integration account.</span></span> 

## <a name="how-toouse-a-transform"></a><span data-ttu-id="7082c-114">Hur toouse en transformering</span><span class="sxs-lookup"><span data-stu-id="7082c-114">How toouse a transform</span></span>
<span data-ttu-id="7082c-115">När du har överfört hello transformering/mappa till kontot integration kan du använda den toocreate en logikapp.</span><span class="sxs-lookup"><span data-stu-id="7082c-115">After you upload hello transform/map into your integration account, you can use it toocreate a Logic app.</span></span> <span data-ttu-id="7082c-116">Hej logikapp körs din transformationer när hello logikapp utlöses (och det finns indata innehåll som måste omvandlas toobe).</span><span class="sxs-lookup"><span data-stu-id="7082c-116">hello Logic app runs your transformations whenever hello Logic app is triggered (and there is input content that needs toobe transformed).</span></span>

<span data-ttu-id="7082c-117">**Här följer hello steg toouse en transformering**:</span><span class="sxs-lookup"><span data-stu-id="7082c-117">**Here are hello steps toouse a transform**:</span></span>

### <a name="prerequisites"></a><span data-ttu-id="7082c-118">Krav</span><span class="sxs-lookup"><span data-stu-id="7082c-118">Prerequisites</span></span>

* <span data-ttu-id="7082c-119">Skapa ett konto för integrering och lägga till en karta tooit</span><span class="sxs-lookup"><span data-stu-id="7082c-119">Create an integration account and add a map tooit</span></span>  

<span data-ttu-id="7082c-120">Nu när du har åtgärdat hello krav, är det tid toocreate logikappen:</span><span class="sxs-lookup"><span data-stu-id="7082c-120">Now that you've taken care of hello prerequisites, it's time toocreate your Logic app:</span></span>  

1. <span data-ttu-id="7082c-121">Skapa en logikapp och [länka det tooyour integrering konto](../logic-apps/logic-apps-enterprise-integration-accounts.md "Läs toolink en logikapp integrering konto tooa") som innehåller hello kartan.</span><span class="sxs-lookup"><span data-stu-id="7082c-121">Create a Logic app and [link it tooyour integration account](../logic-apps/logic-apps-enterprise-integration-accounts.md "Learn toolink an integration account tooa Logic app") that contains hello map.</span></span>
2. <span data-ttu-id="7082c-122">Lägg till en **begära** utlösaren tooyour logikapp</span><span class="sxs-lookup"><span data-stu-id="7082c-122">Add a **Request** trigger tooyour Logic app</span></span>  
   ![](./media/logic-apps-enterprise-integration-transforms/transform-1.png)    
3. <span data-ttu-id="7082c-123">Lägg till hello **transformera XML** åtgärden genom att först välja **lägga till en åtgärd** </span><span class="sxs-lookup"><span data-stu-id="7082c-123">Add hello **Transform XML** action by first selecting **Add an action** </span></span>  
   ![](./media/logic-apps-enterprise-integration-transforms/transform-2.png)   
4. <span data-ttu-id="7082c-124">Ange hello word *transformera* i hello Sök rutan toofilter alla hello åtgärder toohello något som du vill toouse</span><span class="sxs-lookup"><span data-stu-id="7082c-124">Enter hello word *transform* in hello search box toofilter all hello actions toohello one that you want toouse</span></span>  
   ![](./media/logic-apps-enterprise-integration-transforms/transform-3.png)  
5. <span data-ttu-id="7082c-125">Välj hello **transformera XML** åtgärd</span><span class="sxs-lookup"><span data-stu-id="7082c-125">Select hello **Transform XML** action</span></span>   
6. <span data-ttu-id="7082c-126">Lägg till hello XML **innehåll** som du omvandla.</span><span class="sxs-lookup"><span data-stu-id="7082c-126">Add hello XML **CONTENT** that you transform.</span></span> <span data-ttu-id="7082c-127">Du kan använda alla XML-data som visas i hello HTTP-begäran som hello **innehåll**.</span><span class="sxs-lookup"><span data-stu-id="7082c-127">You can use any XML data you receive in hello HTTP request as hello **CONTENT**.</span></span> <span data-ttu-id="7082c-128">I det här exemplet väljer du hello brödtext hello HTTP-begäran som utlöste hello logikapp.</span><span class="sxs-lookup"><span data-stu-id="7082c-128">In this example, select hello body of hello HTTP request that triggered hello Logic app.</span></span>
7. <span data-ttu-id="7082c-129">Välj hello namnet på hello **KARTAN** som du vill toouse tooperform hello omvandling.</span><span class="sxs-lookup"><span data-stu-id="7082c-129">Select hello name of hello **MAP** that you want toouse tooperform hello transformation.</span></span> <span data-ttu-id="7082c-130">hello karta måste redan finnas i ditt konto för integrering.</span><span class="sxs-lookup"><span data-stu-id="7082c-130">hello map must already be in your integration account.</span></span> <span data-ttu-id="7082c-131">I ett tidigare steg gav du redan din logik app tooyour integrering åtkomstkonto som innehåller kartan.</span><span class="sxs-lookup"><span data-stu-id="7082c-131">In an earlier step, you already gave your Logic app access tooyour integration account that contains your map.</span></span>      
   ![](./media/logic-apps-enterprise-integration-transforms/transform-4.png) 
8. <span data-ttu-id="7082c-132">Spara ditt arbete</span><span class="sxs-lookup"><span data-stu-id="7082c-132">Save your work</span></span>  
    ![](./media/logic-apps-enterprise-integration-transforms/transform-5.png) 

<span data-ttu-id="7082c-133">Nu är du klar med att ställa in kartan.</span><span class="sxs-lookup"><span data-stu-id="7082c-133">At this point, you are finished setting up your map.</span></span> <span data-ttu-id="7082c-134">Du kanske vill toostore hello omvandlas data i en LOB-program, till exempel SalesForce i ett verkligt program.</span><span class="sxs-lookup"><span data-stu-id="7082c-134">In a real world application, you may want toostore hello transformed data in an LOB application such as SalesForce.</span></span> <span data-ttu-id="7082c-135">Du kan enkelt som åtgärden toosend hello utdata för hello Omforma tooSalesforce.</span><span class="sxs-lookup"><span data-stu-id="7082c-135">You can easily as an action toosend hello output of hello transform tooSalesforce.</span></span> 

<span data-ttu-id="7082c-136">Nu kan du testa din transformeringen genom att göra en förfrågan toohello HTTP-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="7082c-136">You can now test your transform by making a request toohello HTTP endpoint.</span></span>  

## <a name="features-and-use-cases"></a><span data-ttu-id="7082c-137">Funktioner och användningsområden</span><span class="sxs-lookup"><span data-stu-id="7082c-137">Features and use cases</span></span>
* <span data-ttu-id="7082c-138">hello-transformation som skapats i en karta kan vara enkel, till exempel kopiera ett namn och adress från ett dokument tooanother.</span><span class="sxs-lookup"><span data-stu-id="7082c-138">hello transformation created in a map can be simple, such as copying a name and address from one document tooanother.</span></span> <span data-ttu-id="7082c-139">Eller så kan du skapa mer komplexa omformningar med hjälp av hello out box kartan åtgärder.</span><span class="sxs-lookup"><span data-stu-id="7082c-139">Or, you can create more complex transformations using hello out-of-the-box map operations.</span></span>  
* <span data-ttu-id="7082c-140">Flera kartan operations eller funktioner är tillgängliga, inklusive strängar, datum tidsfunktioner och så vidare.</span><span class="sxs-lookup"><span data-stu-id="7082c-140">Multiple map operations or functions are readily available, including strings, date time functions, and so on.</span></span>  
* <span data-ttu-id="7082c-141">Du kan göra en kopia direkt mellan hello scheman.</span><span class="sxs-lookup"><span data-stu-id="7082c-141">You can do a direct data copy between hello schemas.</span></span> <span data-ttu-id="7082c-142">I hello Mapper ingår i hello SDK, är så enkelt som att rita en linje som sammanbinder hello element i hello datakällans schema med deras motsvarigheter i hello mål schemat.</span><span class="sxs-lookup"><span data-stu-id="7082c-142">In hello Mapper included in hello SDK, this is as simple as drawing a line that connects hello elements in hello source schema with their counterparts in hello destination schema.</span></span>  
* <span data-ttu-id="7082c-143">När du skapar en karta, kan du visa en grafisk representation av hello karta som visar alla hello relationer och länkar som du skapar.</span><span class="sxs-lookup"><span data-stu-id="7082c-143">When creating a map, you view a graphical representation of hello map, which shows all hello relationships and links you create.</span></span>
* <span data-ttu-id="7082c-144">Använd hello Test kartan funktionen tooadd ett exempelmeddelande XML.</span><span class="sxs-lookup"><span data-stu-id="7082c-144">Use hello Test Map feature tooadd a sample XML message.</span></span> <span data-ttu-id="7082c-145">Du kan testa hello karta du skapat och se hello genererade utdata med en enkel klickning.</span><span class="sxs-lookup"><span data-stu-id="7082c-145">With a simple click, you can test hello map you created, and see hello generated output.</span></span>  
* <span data-ttu-id="7082c-146">Överför befintliga mappningar</span><span class="sxs-lookup"><span data-stu-id="7082c-146">Upload existing maps</span></span>  
* <span data-ttu-id="7082c-147">Innehåller stöd för hello XML-format.</span><span class="sxs-lookup"><span data-stu-id="7082c-147">Includes support for hello XML format.</span></span>

## <a name="learn-more"></a><span data-ttu-id="7082c-148">Läs mer</span><span class="sxs-lookup"><span data-stu-id="7082c-148">Learn more</span></span>
* [<span data-ttu-id="7082c-149">Mer information om hello Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="7082c-149">Learn more about hello Enterprise Integration Pack</span></span>](../logic-apps/logic-apps-enterprise-integration-overview.md "Lär dig mer om Enterprise-Integrationspaket")  
* [<span data-ttu-id="7082c-150">Mer information om maps</span><span class="sxs-lookup"><span data-stu-id="7082c-150">Learn more about maps</span></span>](../logic-apps/logic-apps-enterprise-integration-maps.md "Lär dig mer om enterprise integration maps")  

