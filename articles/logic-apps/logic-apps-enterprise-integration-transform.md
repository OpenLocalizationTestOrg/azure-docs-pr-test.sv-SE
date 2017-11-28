---
title: Konvertera XML-data med transformeringar - Azure Logic Apps | Microsoft Docs
description: "Skapa transformeringar eller mapps att konvertera XML-data mellan formaten i logikappar med hjälp av Enterprise Integration-SDK"
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
ms.openlocfilehash: fb6027769377b3527b11f7831dab3bb8d7061c84
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="enterprise-integration-with-xml-transforms"></a><span data-ttu-id="5d440-103">Enterprise integration med XML-transformeringar</span><span class="sxs-lookup"><span data-stu-id="5d440-103">Enterprise integration with XML transforms</span></span>
## <a name="overview"></a><span data-ttu-id="5d440-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="5d440-104">Overview</span></span>
<span data-ttu-id="5d440-105">Enterprise integration transformeringen kopplingen konverterar data från ett format till ett annat format.</span><span class="sxs-lookup"><span data-stu-id="5d440-105">The Enterprise integration Transform connector converts data from one format to another format.</span></span> <span data-ttu-id="5d440-106">Du kan till exempel ha ett inkommande meddelande som innehåller det aktuella datumet i formatet YearMonthDay.</span><span class="sxs-lookup"><span data-stu-id="5d440-106">For example, you may have an incoming message that contains the current date in the YearMonthDay format.</span></span> <span data-ttu-id="5d440-107">Du kan använda en transformering formatera om datumet som ska vara i formatet MonthDayYear.</span><span class="sxs-lookup"><span data-stu-id="5d440-107">You can use a transform to reformat the date to be in the MonthDayYear format.</span></span>

## <a name="what-does-a-transform-do"></a><span data-ttu-id="5d440-108">Vad är en omvandling?</span><span class="sxs-lookup"><span data-stu-id="5d440-108">What does a transform do?</span></span>
<span data-ttu-id="5d440-109">En transformering som kallas även en karta, består av källa XML-schema (indata) och ett mål XML-schema (utdata).</span><span class="sxs-lookup"><span data-stu-id="5d440-109">A Transform, which is also known as a map, consists of a Source XML schema (the input) and a Target XML schema (the output).</span></span> <span data-ttu-id="5d440-110">Du kan använda olika inbyggda funktioner för att ändra eller kontrollera data, inklusive strängändringar, villkorlig tilldelningar, aritmetiska uttryck, datum tid formaterare och även slingor konstruktioner.</span><span class="sxs-lookup"><span data-stu-id="5d440-110">You can use different built-in functions to help manipulate or control the data, including string manipulations, conditional assignments, arithmetic expressions, date time formatters, and even looping constructs.</span></span>

## <a name="how-to-create-a-transform"></a><span data-ttu-id="5d440-111">Så här skapar du en omvandling?</span><span class="sxs-lookup"><span data-stu-id="5d440-111">How to create a transform?</span></span>
<span data-ttu-id="5d440-112">Du kan skapa en transformering/karta med hjälp av Visual Studio [Enterprise Integration SDK](https://aka.ms/vsmapsandschemas).</span><span class="sxs-lookup"><span data-stu-id="5d440-112">You can create a transform/map by using the Visual Studio [Enterprise Integration SDK](https://aka.ms/vsmapsandschemas).</span></span> <span data-ttu-id="5d440-113">När du är klar att skapa och testa transformeringen överför transformeringen till ditt konto för integrering.</span><span class="sxs-lookup"><span data-stu-id="5d440-113">When you are finished creating and testing the transform, you upload the transform into your integration account.</span></span> 

## <a name="how-to-use-a-transform"></a><span data-ttu-id="5d440-114">Hur du använder en transformering</span><span class="sxs-lookup"><span data-stu-id="5d440-114">How to use a transform</span></span>
<span data-ttu-id="5d440-115">Du kan använda det för att skapa en logikapp när du har överfört transformering/mappa till ditt konto för integrering.</span><span class="sxs-lookup"><span data-stu-id="5d440-115">After you upload the transform/map into your integration account, you can use it to create a Logic app.</span></span> <span data-ttu-id="5d440-116">Logikappen körs din transformationer när logikappen utlöses (och det finns indata innehåll som måste omvandlas).</span><span class="sxs-lookup"><span data-stu-id="5d440-116">The Logic app runs your transformations whenever the Logic app is triggered (and there is input content that needs to be transformed).</span></span>

<span data-ttu-id="5d440-117">**Här följer stegen för att använda en transformering**:</span><span class="sxs-lookup"><span data-stu-id="5d440-117">**Here are the steps to use a transform**:</span></span>

### <a name="prerequisites"></a><span data-ttu-id="5d440-118">Krav</span><span class="sxs-lookup"><span data-stu-id="5d440-118">Prerequisites</span></span>

* <span data-ttu-id="5d440-119">Skapa ett konto för integrering och Lägg till en karta</span><span class="sxs-lookup"><span data-stu-id="5d440-119">Create an integration account and add a map to it</span></span>  

<span data-ttu-id="5d440-120">Nu när du har åtgärdat förutsättningarna, är det dags att skapa din logikapp:</span><span class="sxs-lookup"><span data-stu-id="5d440-120">Now that you've taken care of the prerequisites, it's time to create your Logic app:</span></span>  

1. <span data-ttu-id="5d440-121">Skapa en logikapp och [länka till ditt konto integration](../logic-apps/logic-apps-enterprise-integration-accounts.md "Lär dig hur du länkar ett integration konto till en logikapp") som innehåller mappningen.</span><span class="sxs-lookup"><span data-stu-id="5d440-121">Create a Logic app and [link it to your integration account](../logic-apps/logic-apps-enterprise-integration-accounts.md "Learn to link an integration account to a Logic app") that contains the map.</span></span>
2. <span data-ttu-id="5d440-122">Lägg till en **begära** utlösaren till din logikapp</span><span class="sxs-lookup"><span data-stu-id="5d440-122">Add a **Request** trigger to your Logic app</span></span>  
   ![](./media/logic-apps-enterprise-integration-transforms/transform-1.png)    
3. <span data-ttu-id="5d440-123">Lägg till den **transformera XML** åtgärden genom att först välja **lägga till en åtgärd** </span><span class="sxs-lookup"><span data-stu-id="5d440-123">Add the **Transform XML** action by first selecting **Add an action** </span></span>  
   ![](./media/logic-apps-enterprise-integration-transforms/transform-2.png)   
4. <span data-ttu-id="5d440-124">Ange ordet *transformera* i sökrutan för att filtrera vilka åtgärder som det som du vill använda</span><span class="sxs-lookup"><span data-stu-id="5d440-124">Enter the word *transform* in the search box to filter all the actions to the one that you want to use</span></span>  
   ![](./media/logic-apps-enterprise-integration-transforms/transform-3.png)  
5. <span data-ttu-id="5d440-125">Välj den **transformera XML** åtgärd</span><span class="sxs-lookup"><span data-stu-id="5d440-125">Select the **Transform XML** action</span></span>   
6. <span data-ttu-id="5d440-126">Lägga till XML **innehåll** som du omvandla.</span><span class="sxs-lookup"><span data-stu-id="5d440-126">Add the XML **CONTENT** that you transform.</span></span> <span data-ttu-id="5d440-127">Du kan använda en XML-data som visas i HTTP-begäran som den **innehåll**.</span><span class="sxs-lookup"><span data-stu-id="5d440-127">You can use any XML data you receive in the HTTP request as the **CONTENT**.</span></span> <span data-ttu-id="5d440-128">I det här exemplet väljer du innehållet i HTTP-begäran som utlöste logikappen.</span><span class="sxs-lookup"><span data-stu-id="5d440-128">In this example, select the body of the HTTP request that triggered the Logic app.</span></span>
7. <span data-ttu-id="5d440-129">Välj namnet på den **KARTAN** som du vill använda för att utföra omvandlingen.</span><span class="sxs-lookup"><span data-stu-id="5d440-129">Select the name of the **MAP** that you want to use to perform the transformation.</span></span> <span data-ttu-id="5d440-130">Kartan måste redan finnas i ditt konto för integrering.</span><span class="sxs-lookup"><span data-stu-id="5d440-130">The map must already be in your integration account.</span></span> <span data-ttu-id="5d440-131">I ett tidigare steg gav du redan din logik appåtkomst till ditt konto för integrering med kartan.</span><span class="sxs-lookup"><span data-stu-id="5d440-131">In an earlier step, you already gave your Logic app access to your integration account that contains your map.</span></span>      
   ![](./media/logic-apps-enterprise-integration-transforms/transform-4.png) 
8. <span data-ttu-id="5d440-132">Spara ditt arbete</span><span class="sxs-lookup"><span data-stu-id="5d440-132">Save your work</span></span>  
    ![](./media/logic-apps-enterprise-integration-transforms/transform-5.png) 

<span data-ttu-id="5d440-133">Nu är du klar med att ställa in kartan.</span><span class="sxs-lookup"><span data-stu-id="5d440-133">At this point, you are finished setting up your map.</span></span> <span data-ttu-id="5d440-134">I ett verkligt program kanske du vill lagra omvandlade data i en LOB-program, till exempel SalesForce.</span><span class="sxs-lookup"><span data-stu-id="5d440-134">In a real world application, you may want to store the transformed data in an LOB application such as SalesForce.</span></span> <span data-ttu-id="5d440-135">Du kan enkelt som en åtgärd för att skicka utdata från transformeringen till Salesforce.</span><span class="sxs-lookup"><span data-stu-id="5d440-135">You can easily as an action to send the output of the transform to Salesforce.</span></span> 

<span data-ttu-id="5d440-136">Nu kan du testa din transformeringen genom att göra en begäran till HTTP-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="5d440-136">You can now test your transform by making a request to the HTTP endpoint.</span></span>  

## <a name="features-and-use-cases"></a><span data-ttu-id="5d440-137">Funktioner och användningsområden</span><span class="sxs-lookup"><span data-stu-id="5d440-137">Features and use cases</span></span>
* <span data-ttu-id="5d440-138">Transformering som skapats i en karta kan vara enkel, till exempel kopiera ett namn och adress från ett dokument till en annan.</span><span class="sxs-lookup"><span data-stu-id="5d440-138">The transformation created in a map can be simple, such as copying a name and address from one document to another.</span></span> <span data-ttu-id="5d440-139">Eller så kan du skapa mer komplexa omformningar med hjälp av åtgärderna out box-karta.</span><span class="sxs-lookup"><span data-stu-id="5d440-139">Or, you can create more complex transformations using the out-of-the-box map operations.</span></span>  
* <span data-ttu-id="5d440-140">Flera kartan operations eller funktioner är tillgängliga, inklusive strängar, datum tidsfunktioner och så vidare.</span><span class="sxs-lookup"><span data-stu-id="5d440-140">Multiple map operations or functions are readily available, including strings, date time functions, and so on.</span></span>  
* <span data-ttu-id="5d440-141">Du kan göra en kopia direkt mellan scheman.</span><span class="sxs-lookup"><span data-stu-id="5d440-141">You can do a direct data copy between the schemas.</span></span> <span data-ttu-id="5d440-142">I Mapper ingår i SDK, är så enkelt som att rita en linje som sammanbinder elementen i datakällans schema med deras motsvarigheter i mål-schemat.</span><span class="sxs-lookup"><span data-stu-id="5d440-142">In the Mapper included in the SDK, this is as simple as drawing a line that connects the elements in the source schema with their counterparts in the destination schema.</span></span>  
* <span data-ttu-id="5d440-143">När du skapar en karta, kan du visa en grafisk representation av kartan som visar alla relationer och länkar som du skapar.</span><span class="sxs-lookup"><span data-stu-id="5d440-143">When creating a map, you view a graphical representation of the map, which shows all the relationships and links you create.</span></span>
* <span data-ttu-id="5d440-144">Med funktionen testa kartan att lägga till en XML-exempelmeddelande.</span><span class="sxs-lookup"><span data-stu-id="5d440-144">Use the Test Map feature to add a sample XML message.</span></span> <span data-ttu-id="5d440-145">Du kan testa kartan som du skapade och se utdata skapas med en enkel klickning.</span><span class="sxs-lookup"><span data-stu-id="5d440-145">With a simple click, you can test the map you created, and see the generated output.</span></span>  
* <span data-ttu-id="5d440-146">Överför befintliga mappningar</span><span class="sxs-lookup"><span data-stu-id="5d440-146">Upload existing maps</span></span>  
* <span data-ttu-id="5d440-147">Innehåller stöd för XML-format.</span><span class="sxs-lookup"><span data-stu-id="5d440-147">Includes support for the XML format.</span></span>

## <a name="learn-more"></a><span data-ttu-id="5d440-148">Läs mer</span><span class="sxs-lookup"><span data-stu-id="5d440-148">Learn more</span></span>
* [<span data-ttu-id="5d440-149">Mer information om Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="5d440-149">Learn more about the Enterprise Integration Pack</span></span>](../logic-apps/logic-apps-enterprise-integration-overview.md "Lär dig mer om Enterprise-Integrationspaket")  
* [<span data-ttu-id="5d440-150">Mer information om maps</span><span class="sxs-lookup"><span data-stu-id="5d440-150">Learn more about maps</span></span>](../logic-apps/logic-apps-enterprise-integration-maps.md "Lär dig mer om enterprise integration maps")  

