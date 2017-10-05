---
title: Transformera XML med XSLT-maps - Azure Logic Apps | Microsoft Docs
description: "Lägg till XSLT mappar för att omvandla XML-data med Azure Logikappar och Enterprise-Integrationspaket"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: 90f5cfc4-46b2-4ef7-8ac4-486bb0e3f289
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 4445a84a6c6425110e7d705019a28b5cc5447046
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="add-maps-for-xml-data-transform"></a><span data-ttu-id="8abf2-103">Lägga till mappningar för transformering av XML-data</span><span class="sxs-lookup"><span data-stu-id="8abf2-103">Add maps for XML data transform</span></span>

<span data-ttu-id="8abf2-104">Enterprise integration använder maps för att omvandla XML-data mellan format.</span><span class="sxs-lookup"><span data-stu-id="8abf2-104">Enterprise integration uses maps to transform XML data between formats.</span></span> <span data-ttu-id="8abf2-105">En karta är ett XML-dokument som definierar data i ett dokument som ska omvandlas till ett annat format.</span><span class="sxs-lookup"><span data-stu-id="8abf2-105">A map is an XML document that defines the data in a document that should be transformed into another format.</span></span> 

## <a name="why-use-maps"></a><span data-ttu-id="8abf2-106">Varför använda maps?</span><span class="sxs-lookup"><span data-stu-id="8abf2-106">Why use maps?</span></span>

<span data-ttu-id="8abf2-107">Anta att du regelbundet ta emot B2B order eller fakturor från en kund som använder YYYMMDD format för datum.</span><span class="sxs-lookup"><span data-stu-id="8abf2-107">Suppose that you regularly receive B2B orders or invoices from a customer who uses the YYYMMDD format for dates.</span></span> <span data-ttu-id="8abf2-108">Du kan dock lagra datum i formatet MMDDYYY i din organisation.</span><span class="sxs-lookup"><span data-stu-id="8abf2-108">However, in your organization, you store dates in the MMDDYYY format.</span></span> <span data-ttu-id="8abf2-109">Du kan använda en karta till *transformera* YYYMMDD datumformat i MMDDYYY innan de lagras informationen order eller faktura i kunddatabasen för aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="8abf2-109">You can use a map to *transform* the YYYMMDD date format into the MMDDYYY before storing the order or invoice details in your customer activity database.</span></span>

## <a name="how-do-i-create-a-map"></a><span data-ttu-id="8abf2-110">Hur skapar jag en karta</span><span class="sxs-lookup"><span data-stu-id="8abf2-110">How do I create a map?</span></span>

<span data-ttu-id="8abf2-111">Du kan skapa projekt BizTalk-integrering med den [Enterprise-Integrationspaket](logic-apps-enterprise-integration-overview.md "Lär dig mer om enterprise-integrationspaket") för Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="8abf2-111">You can create BizTalk Integration projects with the [Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "Learn about the enterprise integration pack") for Visual Studio 2015.</span></span> <span data-ttu-id="8abf2-112">Du kan sedan skapa en Integrationskarta-fil som kan du mappa visuellt objekt mellan två XML-schemafiler.</span><span class="sxs-lookup"><span data-stu-id="8abf2-112">You can then create an Integration Map file that lets you visually map items between two XML schema files.</span></span> <span data-ttu-id="8abf2-113">När du skapar det här projektet har du en XSLT-dokument.</span><span class="sxs-lookup"><span data-stu-id="8abf2-113">After you build this project, you will have an XSLT document.</span></span>

## <a name="how-do-i-add-a-map"></a><span data-ttu-id="8abf2-114">Hur lägger jag till en karta?</span><span class="sxs-lookup"><span data-stu-id="8abf2-114">How do I add a map?</span></span>

1. <span data-ttu-id="8abf2-115">Välj i Azure-portalen **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="8abf2-115">In the Azure portal, select **Browse**.</span></span>

    ![](./media/logic-apps-enterprise-integration-overview/overview-1.png)

2. <span data-ttu-id="8abf2-116">Skriv i sökrutan filtrera **integrering**och välj **Integrationskonton** från resultatlistan över.</span><span class="sxs-lookup"><span data-stu-id="8abf2-116">In the filter search box, enter **integration**, then select **Integration Accounts** from the results list.</span></span>

    ![](./media/logic-apps-enterprise-integration-overview/overview-2.png)

3. <span data-ttu-id="8abf2-117">Välj integration konto där du vill lägga till kartan.</span><span class="sxs-lookup"><span data-stu-id="8abf2-117">Select the integration account where you want to add the map.</span></span>

    ![](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. <span data-ttu-id="8abf2-118">Välj den **Maps** panelen.</span><span class="sxs-lookup"><span data-stu-id="8abf2-118">Select the **Maps** tile.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/map-1.png)

5. <span data-ttu-id="8abf2-119">När det öppnas bladet Maps, Välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="8abf2-119">After the Maps blade opens, choose **Add**.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/map-2.png)  

6. <span data-ttu-id="8abf2-120">Ange en **namn** på kartan.</span><span class="sxs-lookup"><span data-stu-id="8abf2-120">Enter a **Name** for your map.</span></span> <span data-ttu-id="8abf2-121">Om du vill överföra filen karta, väljer du mappikonen på höger sida av den **kartan** textruta.</span><span class="sxs-lookup"><span data-stu-id="8abf2-121">To upload the map file, choose the folder icon on the right side of the **Map** text box.</span></span> <span data-ttu-id="8abf2-122">När överföringen är klar väljer **OK**.</span><span class="sxs-lookup"><span data-stu-id="8abf2-122">After the upload process completes, choose **OK**.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/map-3.png)

7. <span data-ttu-id="8abf2-123">När Azure läggs mappningen till ditt konto integration, få skärmen som visar om din mappningsfilen har lagts till eller inte.</span><span class="sxs-lookup"><span data-stu-id="8abf2-123">After Azure adds the map to your integration account, you get an onscreen message that shows whether your map file was added or not.</span></span> <span data-ttu-id="8abf2-124">När du får detta meddelande, välja den **Maps** panelen så att du kan visa nytillagda kartan.</span><span class="sxs-lookup"><span data-stu-id="8abf2-124">After you get this message, choose the **Maps** tile so you can view the newly added map.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/map-4.png)

## <a name="how-do-i-edit-a-map"></a><span data-ttu-id="8abf2-125">Hur redigerar en karta?</span><span class="sxs-lookup"><span data-stu-id="8abf2-125">How do I edit a map?</span></span>

<span data-ttu-id="8abf2-126">Du måste överföra en ny fil karta med ändringarna som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="8abf2-126">You must upload a new map file with the changes that you want.</span></span> <span data-ttu-id="8abf2-127">Du kan först hämta kartan för redigering.</span><span class="sxs-lookup"><span data-stu-id="8abf2-127">You can first download the map for editing.</span></span>

<span data-ttu-id="8abf2-128">Följ dessa steg för att ladda upp en ny mappning som ersätter den befintliga mappningen.</span><span class="sxs-lookup"><span data-stu-id="8abf2-128">To upload a new map that replaces the existing map, follow these steps.</span></span>

1. <span data-ttu-id="8abf2-129">Välj den **Maps** panelen.</span><span class="sxs-lookup"><span data-stu-id="8abf2-129">Choose the **Maps** tile.</span></span>

2. <span data-ttu-id="8abf2-130">När det öppnas bladet Maps, Välj kartan som du vill redigera.</span><span class="sxs-lookup"><span data-stu-id="8abf2-130">After the Maps blade opens, select the map that you want to edit.</span></span>

3. <span data-ttu-id="8abf2-131">På den **Maps** bladet välj **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="8abf2-131">On the **Maps** blade, choose **Update**.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/edit-1.png)

4. <span data-ttu-id="8abf2-132">Välj mappningsfilen som du vill ladda upp och välj sedan i filväljaren, **öppna**.</span><span class="sxs-lookup"><span data-stu-id="8abf2-132">In the file picker, select the map file that you want to upload, then select **Open**.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/edit-2.png)

## <a name="how-to-delete-a-map"></a><span data-ttu-id="8abf2-133">Hur du tar bort en karta?</span><span class="sxs-lookup"><span data-stu-id="8abf2-133">How to delete a map?</span></span>

1. <span data-ttu-id="8abf2-134">Välj den **Maps** panelen.</span><span class="sxs-lookup"><span data-stu-id="8abf2-134">Choose the **Maps** tile.</span></span>

2. <span data-ttu-id="8abf2-135">När det öppnas bladet Maps, Välj kartan som du vill ta bort.</span><span class="sxs-lookup"><span data-stu-id="8abf2-135">After the Maps blade opens, select the map you want to delete.</span></span>

3. <span data-ttu-id="8abf2-136">Välj **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="8abf2-136">Choose **Delete**.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/delete.png)

4. <span data-ttu-id="8abf2-137">Bekräfta att du vill ta bort mappningen.</span><span class="sxs-lookup"><span data-stu-id="8abf2-137">Confirm that you want to delete the map.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/delete-confirmation-1.png)

## <a name="next-steps"></a><span data-ttu-id="8abf2-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8abf2-138">Next Steps</span></span>
* [<span data-ttu-id="8abf2-139">Mer information om Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="8abf2-139">Learn more about the Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md "Lär dig mer om Enterprise-Integrationspaket")  
* [<span data-ttu-id="8abf2-140">Mer information om avtalen</span><span class="sxs-lookup"><span data-stu-id="8abf2-140">Learn more about agreements</span></span>](../logic-apps/logic-apps-enterprise-integration-agreements.md "Lär dig mer om enterprise integration-avtal")  
* [<span data-ttu-id="8abf2-141">Mer information om transformeringar</span><span class="sxs-lookup"><span data-stu-id="8abf2-141">Learn more about transforms</span></span>](logic-apps-enterprise-integration-transform.md "Lär dig mer om enterprise integration transformeringar")  

