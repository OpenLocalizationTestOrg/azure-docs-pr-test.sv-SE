---
title: aaaTransform XML med XSLT-maps - Azure Logic Apps | Microsoft Docs
description: "Lägg till XSLT mappar tootransform XML-data med Azure Logikappar och hello Enterprise-Integrationspaket"
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
ms.openlocfilehash: 720e6f988b8542136dfcc402c3c463fcfb2f23cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-maps-for-xml-data-transform"></a><span data-ttu-id="0601f-103">Lägga till mappningar för transformering av XML-data</span><span class="sxs-lookup"><span data-stu-id="0601f-103">Add maps for XML data transform</span></span>

<span data-ttu-id="0601f-104">Enterprise integration använder mappar tootransform XML-data mellan format.</span><span class="sxs-lookup"><span data-stu-id="0601f-104">Enterprise integration uses maps tootransform XML data between formats.</span></span> <span data-ttu-id="0601f-105">En karta är ett XML-dokument som definierar hello data i ett dokument som ska omvandlas till ett annat format.</span><span class="sxs-lookup"><span data-stu-id="0601f-105">A map is an XML document that defines hello data in a document that should be transformed into another format.</span></span> 

## <a name="why-use-maps"></a><span data-ttu-id="0601f-106">Varför använda maps?</span><span class="sxs-lookup"><span data-stu-id="0601f-106">Why use maps?</span></span>

<span data-ttu-id="0601f-107">Anta att du regelbundet ta emot B2B order eller fakturor från en kund som använder hello YYYMMDD format för datum.</span><span class="sxs-lookup"><span data-stu-id="0601f-107">Suppose that you regularly receive B2B orders or invoices from a customer who uses hello YYYMMDD format for dates.</span></span> <span data-ttu-id="0601f-108">Du kan dock lagra datum hello MMDDYYY formatet i din organisation.</span><span class="sxs-lookup"><span data-stu-id="0601f-108">However, in your organization, you store dates in hello MMDDYYY format.</span></span> <span data-ttu-id="0601f-109">Du kan använda en karta för*transformera* hello YYYMMDD datumformat i hello MMDDYYY innan de lagras hello order eller faktura information i kunddatabasen för aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="0601f-109">You can use a map too*transform* hello YYYMMDD date format into hello MMDDYYY before storing hello order or invoice details in your customer activity database.</span></span>

## <a name="how-do-i-create-a-map"></a><span data-ttu-id="0601f-110">Hur skapar jag en karta</span><span class="sxs-lookup"><span data-stu-id="0601f-110">How do I create a map?</span></span>

<span data-ttu-id="0601f-111">Du kan skapa projekt BizTalk-integrering med hello [Enterprise-Integrationspaket](logic-apps-enterprise-integration-overview.md "Lär dig mer om hello enterprise-integrationspaket") för Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="0601f-111">You can create BizTalk Integration projects with hello [Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "Learn about hello enterprise integration pack") for Visual Studio 2015.</span></span> <span data-ttu-id="0601f-112">Du kan sedan skapa en Integrationskarta-fil som kan du mappa visuellt objekt mellan två XML-schemafiler.</span><span class="sxs-lookup"><span data-stu-id="0601f-112">You can then create an Integration Map file that lets you visually map items between two XML schema files.</span></span> <span data-ttu-id="0601f-113">När du skapar det här projektet har du en XSLT-dokument.</span><span class="sxs-lookup"><span data-stu-id="0601f-113">After you build this project, you will have an XSLT document.</span></span>

## <a name="how-do-i-add-a-map"></a><span data-ttu-id="0601f-114">Hur lägger jag till en karta?</span><span class="sxs-lookup"><span data-stu-id="0601f-114">How do I add a map?</span></span>

1. <span data-ttu-id="0601f-115">Välj i hello Azure-portalen, **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="0601f-115">In hello Azure portal, select **Browse**.</span></span>

    ![](./media/logic-apps-enterprise-integration-overview/overview-1.png)

2. <span data-ttu-id="0601f-116">Skriv i sökrutan för hello filter **integrering**och välj **Integrationskonton** från hello resultatlistan.</span><span class="sxs-lookup"><span data-stu-id="0601f-116">In hello filter search box, enter **integration**, then select **Integration Accounts** from hello results list.</span></span>

    ![](./media/logic-apps-enterprise-integration-overview/overview-2.png)

3. <span data-ttu-id="0601f-117">Välj hello integration konto där du vill att tooadd hello kartan.</span><span class="sxs-lookup"><span data-stu-id="0601f-117">Select hello integration account where you want tooadd hello map.</span></span>

    ![](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. <span data-ttu-id="0601f-118">Välj hello **Maps** panelen.</span><span class="sxs-lookup"><span data-stu-id="0601f-118">Select hello **Maps** tile.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/map-1.png)

5. <span data-ttu-id="0601f-119">När hello Maps blad öppnas, välja **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="0601f-119">After hello Maps blade opens, choose **Add**.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/map-2.png)  

6. <span data-ttu-id="0601f-120">Ange en **namn** på kartan.</span><span class="sxs-lookup"><span data-stu-id="0601f-120">Enter a **Name** for your map.</span></span> <span data-ttu-id="0601f-121">tooupload hello kartan fil, Välj hello mappikon hello höger på hello **kartan** textruta.</span><span class="sxs-lookup"><span data-stu-id="0601f-121">tooupload hello map file, choose hello folder icon on hello right side of hello **Map** text box.</span></span> <span data-ttu-id="0601f-122">När hello överföringen är klar väljer **OK**.</span><span class="sxs-lookup"><span data-stu-id="0601f-122">After hello upload process completes, choose **OK**.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/map-3.png)

7. <span data-ttu-id="0601f-123">När Azure lägger till hello kartan tooyour integrering konto, få skärmen som visar om din mappningsfilen har lagts till eller inte.</span><span class="sxs-lookup"><span data-stu-id="0601f-123">After Azure adds hello map tooyour integration account, you get an onscreen message that shows whether your map file was added or not.</span></span> <span data-ttu-id="0601f-124">När du får detta meddelande, Välj hello **Maps** panelen så att du kan visa hello nyligen lagt till mappning.</span><span class="sxs-lookup"><span data-stu-id="0601f-124">After you get this message, choose hello **Maps** tile so you can view hello newly added map.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/map-4.png)

## <a name="how-do-i-edit-a-map"></a><span data-ttu-id="0601f-125">Hur redigerar en karta?</span><span class="sxs-lookup"><span data-stu-id="0601f-125">How do I edit a map?</span></span>

<span data-ttu-id="0601f-126">Du måste överföra en ny fil karta med hello ändringar som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="0601f-126">You must upload a new map file with hello changes that you want.</span></span> <span data-ttu-id="0601f-127">Du kan först hämta hello kartan för redigering.</span><span class="sxs-lookup"><span data-stu-id="0601f-127">You can first download hello map for editing.</span></span>

<span data-ttu-id="0601f-128">tooupload en ny mappning som ersätter hello karta, Följ dessa steg.</span><span class="sxs-lookup"><span data-stu-id="0601f-128">tooupload a new map that replaces hello existing map, follow these steps.</span></span>

1. <span data-ttu-id="0601f-129">Välj hello **Maps** panelen.</span><span class="sxs-lookup"><span data-stu-id="0601f-129">Choose hello **Maps** tile.</span></span>

2. <span data-ttu-id="0601f-130">När hello Maps blad öppnas, Välj hello karta som du vill tooedit.</span><span class="sxs-lookup"><span data-stu-id="0601f-130">After hello Maps blade opens, select hello map that you want tooedit.</span></span>

3. <span data-ttu-id="0601f-131">På hello **Maps** bladet välj **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="0601f-131">On hello **Maps** blade, choose **Update**.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/edit-1.png)

4. <span data-ttu-id="0601f-132">Välj hello mappningsfilen du vill tooupload och sedan välja i hello filväljaren **öppna**.</span><span class="sxs-lookup"><span data-stu-id="0601f-132">In hello file picker, select hello map file that you want tooupload, then select **Open**.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/edit-2.png)

## <a name="how-toodelete-a-map"></a><span data-ttu-id="0601f-133">Hur toodelete en karta?</span><span class="sxs-lookup"><span data-stu-id="0601f-133">How toodelete a map?</span></span>

1. <span data-ttu-id="0601f-134">Välj hello **Maps** panelen.</span><span class="sxs-lookup"><span data-stu-id="0601f-134">Choose hello **Maps** tile.</span></span>

2. <span data-ttu-id="0601f-135">När hello Maps blad öppnas, väljer du hello-mappning som du vill toodelete.</span><span class="sxs-lookup"><span data-stu-id="0601f-135">After hello Maps blade opens, select hello map you want toodelete.</span></span>

3. <span data-ttu-id="0601f-136">Välj **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="0601f-136">Choose **Delete**.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/delete.png)

4. <span data-ttu-id="0601f-137">Bekräfta att du vill toodelete hello kartan.</span><span class="sxs-lookup"><span data-stu-id="0601f-137">Confirm that you want toodelete hello map.</span></span>

    ![](./media/logic-apps-enterprise-integration-maps/delete-confirmation-1.png)

## <a name="next-steps"></a><span data-ttu-id="0601f-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0601f-138">Next Steps</span></span>
* [<span data-ttu-id="0601f-139">Mer information om hello Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="0601f-139">Learn more about hello Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md "Lär dig mer om Enterprise-Integrationspaket")  
* [<span data-ttu-id="0601f-140">Mer information om avtalen</span><span class="sxs-lookup"><span data-stu-id="0601f-140">Learn more about agreements</span></span>](../logic-apps/logic-apps-enterprise-integration-agreements.md "Lär dig mer om enterprise integration-avtal")  
* [<span data-ttu-id="0601f-141">Mer information om transformeringar</span><span class="sxs-lookup"><span data-stu-id="0601f-141">Learn more about transforms</span></span>](logic-apps-enterprise-integration-transform.md "Lär dig mer om enterprise integration transformeringar")  

