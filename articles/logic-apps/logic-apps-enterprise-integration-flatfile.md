---
title: Koda eller avkoda flat-filer i Azure logikappar | Microsoft Docs
description: "Hur du använder filen fil-kodaren eller avkodarens i Enterprise-Integrationspaket i dina logic apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 82152dab-c7ad-43df-b721-596559703be8
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; mandia
ms.openlocfilehash: bc3430624844cdeb92958433fba295f67a8ae0ff
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="overview-of-enterprise-integration-with-flat-files"></a><span data-ttu-id="045a3-103">Översikt över enterprise integration med flat-filer</span><span class="sxs-lookup"><span data-stu-id="045a3-103">Overview of enterprise integration with flat files</span></span>

<span data-ttu-id="045a3-104">Du kanske vill koda XML-innehåll innan du skickar det till ett partnerföretag i ett scenario för business-to-business (B2B).</span><span class="sxs-lookup"><span data-stu-id="045a3-104">You may want to encode XML content before you send it to a business partner in a business-to-business (B2B) scenario.</span></span> <span data-ttu-id="045a3-105">Du kan använda enkla filkodningen koppling för att göra detta i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="045a3-105">In a logic app, you can use the flat file encoding connector to do this.</span></span> <span data-ttu-id="045a3-106">Logikappen som du skapar kan hämta dess XML innehåll från en mängd olika källor, till exempel från en HTTP-begäran-utlösare, från ett annat program eller även från en av många [kopplingar](../connectors/apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="045a3-106">The logic app that you create can get its XML content from a variety of sources, including from an HTTP request trigger, from another application, or even from one of the many [connectors](../connectors/apis-list.md).</span></span> <span data-ttu-id="045a3-107">Mer information om logikappar finns i [logic apps dokumentationen](logic-apps-what-are-logic-apps.md "Lär dig mer om logikappar").</span><span class="sxs-lookup"><span data-stu-id="045a3-107">For more information about logic apps, see the [logic apps documentation](logic-apps-what-are-logic-apps.md "Learn more about Logic apps").</span></span>  

## <a name="create-the-flat-file-encoding-connector"></a><span data-ttu-id="045a3-108">Skapa enkla filkodningen connector</span><span class="sxs-lookup"><span data-stu-id="045a3-108">Create the flat file encoding connector</span></span>
<span data-ttu-id="045a3-109">Följ dessa steg för att lägga till en flat-fil kodning koppling till din logikapp.</span><span class="sxs-lookup"><span data-stu-id="045a3-109">Follow these steps to add a flat file encoding connector to your logic app.</span></span>

1. <span data-ttu-id="045a3-110">Skapa en logikapp och [länka till ditt konto integration](logic-apps-enterprise-integration-accounts.md "Lär dig hur du länkar ett integration konto till en logikapp").</span><span class="sxs-lookup"><span data-stu-id="045a3-110">Create a logic app and [link it to your integration account](logic-apps-enterprise-integration-accounts.md "Learn to link an integration account to a Logic app").</span></span> <span data-ttu-id="045a3-111">Det här kontot innehåller schema som du använder för att koda XML-data.</span><span class="sxs-lookup"><span data-stu-id="045a3-111">This account contains the schema you will use to encode the XML data.</span></span>  
2. <span data-ttu-id="045a3-112">Lägg till en **begäran - när en HTTP-begäran tas emot** utlösaren till din logikapp.</span><span class="sxs-lookup"><span data-stu-id="045a3-112">Add a **Request - When an HTTP request is received** trigger to your logic app.</span></span>  
   <span data-ttu-id="045a3-113">![Skärmbild av utlösare och välj](./media/logic-apps-enterprise-integration-b2b/flatfile-1.png)</span><span class="sxs-lookup"><span data-stu-id="045a3-113">![Screenshot of trigger to select](./media/logic-apps-enterprise-integration-b2b/flatfile-1.png)</span></span>    
3. <span data-ttu-id="045a3-114">Lägg till flat filkodningen åtgärd, enligt följande:</span><span class="sxs-lookup"><span data-stu-id="045a3-114">Add the flat file encoding action, as follows:</span></span>
   
    <span data-ttu-id="045a3-115">a.</span><span class="sxs-lookup"><span data-stu-id="045a3-115">a.</span></span> <span data-ttu-id="045a3-116">Välj den **plus** tecken.</span><span class="sxs-lookup"><span data-stu-id="045a3-116">Select the **plus** sign.</span></span>
   
    <span data-ttu-id="045a3-117">b.</span><span class="sxs-lookup"><span data-stu-id="045a3-117">b.</span></span> <span data-ttu-id="045a3-118">Välj den **lägga till en åtgärd** länk (visas när du har valt på plustecknet).</span><span class="sxs-lookup"><span data-stu-id="045a3-118">Select the **Add an action** link (appears after you have selected the plus sign).</span></span>
   
    <span data-ttu-id="045a3-119">c.</span><span class="sxs-lookup"><span data-stu-id="045a3-119">c.</span></span> <span data-ttu-id="045a3-120">I sökrutan anger *Flat* att filtrera alla åtgärder som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="045a3-120">In the search box, enter *Flat* to filter all the actions to the one that you want to use.</span></span>
   
    <span data-ttu-id="045a3-121">d.</span><span class="sxs-lookup"><span data-stu-id="045a3-121">d.</span></span> <span data-ttu-id="045a3-122">Välj den **Flat Filkodning** i listan.</span><span class="sxs-lookup"><span data-stu-id="045a3-122">Select the **Flat File Encoding** option from the list.</span></span>   
   <span data-ttu-id="045a3-123">![Skärmbild av Flat fil kodning](media/logic-apps-enterprise-integration-flatfile/flatfile-2.png)</span><span class="sxs-lookup"><span data-stu-id="045a3-123">![Screenshot of Flat File Encoding option](media/logic-apps-enterprise-integration-flatfile/flatfile-2.png)</span></span>   
4. <span data-ttu-id="045a3-124">På den **Flat Filkodning** dialogrutan markerar du den **innehåll** textruta.</span><span class="sxs-lookup"><span data-stu-id="045a3-124">On the **Flat File Encoding** dialog box, select the **Content** text box.</span></span>  
   <span data-ttu-id="045a3-125">![Skärmbild av innehåll textruta](media/logic-apps-enterprise-integration-flatfile/flatfile-3.png)</span><span class="sxs-lookup"><span data-stu-id="045a3-125">![Screenshot of Content text box](media/logic-apps-enterprise-integration-flatfile/flatfile-3.png)</span></span>  
5. <span data-ttu-id="045a3-126">Välj body-tagg som det innehåll som du vill koda.</span><span class="sxs-lookup"><span data-stu-id="045a3-126">Select the body tag as the content that you want to encode.</span></span> <span data-ttu-id="045a3-127">Body-taggen ska fylla i fältet content.</span><span class="sxs-lookup"><span data-stu-id="045a3-127">The body tag will populate the content field.</span></span>     
   ![Skärmbild av body-tagg](media/logic-apps-enterprise-integration-flatfile/flatfile-4.png)  
6. <span data-ttu-id="045a3-129">Välj den **schemanamnet** listruta och välj det schema som du vill använda för att koda indata innehållet.</span><span class="sxs-lookup"><span data-stu-id="045a3-129">Select the **Schema Name** list box, and choose the schema you want to use to encode the input content.</span></span>    
   <span data-ttu-id="045a3-130">![Skärmbild av schemanamnet listruta](media/logic-apps-enterprise-integration-flatfile/flatfile-5.png)</span><span class="sxs-lookup"><span data-stu-id="045a3-130">![Screenshot of Schema Name list box](media/logic-apps-enterprise-integration-flatfile/flatfile-5.png)</span></span>  
7. <span data-ttu-id="045a3-131">Spara ditt arbete.</span><span class="sxs-lookup"><span data-stu-id="045a3-131">Save your work.</span></span>   
   ![Skärmbild av spara ikon](media/logic-apps-enterprise-integration-flatfile/flatfile-6.png)  

<span data-ttu-id="045a3-133">Nu är du klar med att ställa in din kodning anslutningstjänst flat-fil.</span><span class="sxs-lookup"><span data-stu-id="045a3-133">At this point, you are finished setting up your flat file encoding connector.</span></span> <span data-ttu-id="045a3-134">I ett verkligt program kanske du vill lagra kodade data i en line-of-business-program, till exempel Salesforce.</span><span class="sxs-lookup"><span data-stu-id="045a3-134">In a real world application, you may want to store the encoded data in a line-of-business application, such as Salesforce.</span></span> <span data-ttu-id="045a3-135">Eller så kan du skicka att kodade data till en handel partner.</span><span class="sxs-lookup"><span data-stu-id="045a3-135">Or you can send that encoded data to a trading partner.</span></span> <span data-ttu-id="045a3-136">Du kan enkelt lägga till en åtgärd för att skicka utdata från åtgärden kodning till Salesforce eller till din handelspartner genom att använda någon av de kopplingar som tillhandahålls.</span><span class="sxs-lookup"><span data-stu-id="045a3-136">You can easily add an action to send the output of the encoding action to Salesforce, or to your trading partner, by using any one of the other connectors provided.</span></span>

<span data-ttu-id="045a3-137">Nu kan du testa din koppling genom att göra en begäran till HTTP-slutpunkten och inklusive XML-innehåll i brödtexten i begäran.</span><span class="sxs-lookup"><span data-stu-id="045a3-137">You can now test your connector by making a request to the HTTP endpoint, and including the XML content in the body of the request.</span></span>  

## <a name="create-the-flat-file-decoding-connector"></a><span data-ttu-id="045a3-138">Skapa flat fil avkoda connector</span><span class="sxs-lookup"><span data-stu-id="045a3-138">Create the flat file decoding connector</span></span>

> [!NOTE]
> <span data-ttu-id="045a3-139">Du måste ha en schemafil redan överförts till integration konto för att slutföra de här stegen.</span><span class="sxs-lookup"><span data-stu-id="045a3-139">To complete these steps, you need to have a schema file already uploaded into you integration account.</span></span>

1. <span data-ttu-id="045a3-140">Lägg till en **begäran - när en HTTP-begäran tas emot** utlösaren till din logikapp.</span><span class="sxs-lookup"><span data-stu-id="045a3-140">Add a **Request - When an HTTP request is received** trigger to your logic app.</span></span>  
   <span data-ttu-id="045a3-141">![Skärmbild av utlösare och välj](./media/logic-apps-enterprise-integration-b2b/flatfile-1.png)</span><span class="sxs-lookup"><span data-stu-id="045a3-141">![Screenshot of trigger to select](./media/logic-apps-enterprise-integration-b2b/flatfile-1.png)</span></span>    
2. <span data-ttu-id="045a3-142">Lägg till flat fil avkoda åtgärd, enligt följande:</span><span class="sxs-lookup"><span data-stu-id="045a3-142">Add the flat file decoding action, as follows:</span></span>
   
    <span data-ttu-id="045a3-143">a.</span><span class="sxs-lookup"><span data-stu-id="045a3-143">a.</span></span> <span data-ttu-id="045a3-144">Välj den **plus** tecken.</span><span class="sxs-lookup"><span data-stu-id="045a3-144">Select the **plus** sign.</span></span>
   
    <span data-ttu-id="045a3-145">b.</span><span class="sxs-lookup"><span data-stu-id="045a3-145">b.</span></span> <span data-ttu-id="045a3-146">Välj den **lägga till en åtgärd** länk (visas när du har valt på plustecknet).</span><span class="sxs-lookup"><span data-stu-id="045a3-146">Select the **Add an action** link (appears after you have selected the plus sign).</span></span>
   
    <span data-ttu-id="045a3-147">c.</span><span class="sxs-lookup"><span data-stu-id="045a3-147">c.</span></span> <span data-ttu-id="045a3-148">I sökrutan anger *Flat* att filtrera alla åtgärder som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="045a3-148">In the search box, enter *Flat* to filter all the actions to the one that you want to use.</span></span>
   
    <span data-ttu-id="045a3-149">d.</span><span class="sxs-lookup"><span data-stu-id="045a3-149">d.</span></span> <span data-ttu-id="045a3-150">Välj den **Flat fil avkoda** i listan.</span><span class="sxs-lookup"><span data-stu-id="045a3-150">Select the **Flat File Decoding** option from the list.</span></span>   
   <span data-ttu-id="045a3-151">![Skärmbild av Flat fil avkoda alternativet](media/logic-apps-enterprise-integration-flatfile/flatfile-2.png)</span><span class="sxs-lookup"><span data-stu-id="045a3-151">![Screenshot of Flat File Decoding option](media/logic-apps-enterprise-integration-flatfile/flatfile-2.png)</span></span>   
3. <span data-ttu-id="045a3-152">Välj den **innehåll** kontroll.</span><span class="sxs-lookup"><span data-stu-id="045a3-152">Select the **Content** control.</span></span> <span data-ttu-id="045a3-153">Detta genererar en lista med innehållet från tidigare steg som du kan använda som innehållet för att avkoda.</span><span class="sxs-lookup"><span data-stu-id="045a3-153">This produces a list of the content from earlier steps that you can use as the content to decode.</span></span> <span data-ttu-id="045a3-154">Observera att den *brödtext* från inkommande HTTP begäran är tillgängliga som ska användas som innehållet för att avkoda.</span><span class="sxs-lookup"><span data-stu-id="045a3-154">Notice that the *Body* from the incoming HTTP request is available to be used as the content to decode.</span></span> <span data-ttu-id="045a3-155">Du kan också ange innehåll att avkoda direkt i den **innehåll** kontroll.</span><span class="sxs-lookup"><span data-stu-id="045a3-155">You can also enter the content to decode directly into the **Content** control.</span></span>     
4. <span data-ttu-id="045a3-156">Välj den *brödtext* tagg.</span><span class="sxs-lookup"><span data-stu-id="045a3-156">Select the *Body* tag.</span></span> <span data-ttu-id="045a3-157">Observera body-taggen är nu i den **innehåll** kontroll.</span><span class="sxs-lookup"><span data-stu-id="045a3-157">Notice the body tag is now in the **Content** control.</span></span>
5. <span data-ttu-id="045a3-158">Välj namnet på det schema som du vill använda för att dekryptera innehållet.</span><span class="sxs-lookup"><span data-stu-id="045a3-158">Select the name of the schema that you want to use to decode the content.</span></span> <span data-ttu-id="045a3-159">Följande skärmbild visar att *OrderFile* är valda schemanamnet.</span><span class="sxs-lookup"><span data-stu-id="045a3-159">The following screenshot shows that *OrderFile* is the selected schema name.</span></span> <span data-ttu-id="045a3-160">Den här schemanamnet har överförts hänsyn integration tidigare.</span><span class="sxs-lookup"><span data-stu-id="045a3-160">This schema name had been uploaded into the integration account previously.</span></span>
   
   ![Skärmbild av Flat fil avkodning av dialogrutan](media/logic-apps-enterprise-integration-flatfile/flatfile-decode-1.png)    
6. <span data-ttu-id="045a3-162">Spara ditt arbete.</span><span class="sxs-lookup"><span data-stu-id="045a3-162">Save your work.</span></span>  
   ![Skärmbild av spara ikon](media/logic-apps-enterprise-integration-flatfile/flatfile-6.png)    

<span data-ttu-id="045a3-164">Nu är du klar med att ställa in din flat fil avkodning av anslutningen.</span><span class="sxs-lookup"><span data-stu-id="045a3-164">At this point, you are finished setting up your flat file decoding connector.</span></span> <span data-ttu-id="045a3-165">I ett verkligt program kanske du vill lagra kodade data i ett line-of-business-program, till exempel Salesforce.</span><span class="sxs-lookup"><span data-stu-id="045a3-165">In a real world application, you may want to store the decoded data in a line-of-business application such as Salesforce.</span></span> <span data-ttu-id="045a3-166">Du kan enkelt lägga till en åtgärd för att skicka utdata från åtgärden avkodning till Salesforce.</span><span class="sxs-lookup"><span data-stu-id="045a3-166">You can easily add an action to send the output of the decoding action to Salesforce.</span></span>

<span data-ttu-id="045a3-167">Nu kan du testa din koppling genom att göra en begäran för HTTP-slutpunkten och inkludera det XML-innehåll som du vill avkoda i brödtexten i begäran.</span><span class="sxs-lookup"><span data-stu-id="045a3-167">You can now test your connector by making a request to the HTTP endpoint and including the XML content you want to decode in the body of the request.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="045a3-168">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="045a3-168">Next steps</span></span>
* <span data-ttu-id="045a3-169">[Mer information om Enterprise-Integrationspaket](logic-apps-enterprise-integration-overview.md "Lär dig mer om Enterprise-Integrationspaket").</span><span class="sxs-lookup"><span data-stu-id="045a3-169">[Learn more about the Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "Learn about Enterprise Integration Pack").</span></span>  

