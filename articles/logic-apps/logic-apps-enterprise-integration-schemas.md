---
title: "Scheman för XML-verifiering - Azure Logic Apps | Microsoft Docs"
description: "Validera XML-dokument med scheman för Logikappar i Azure och Enterprise-Integrationspaket"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: 56c5846c-5d8c-4ad4-9652-60b07aa8fc3b
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 4f58a587c1f10aea1cee89e46fa9ec340e0d21c6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="validate-xml-with-schemas-for-azure-logic-apps-and-the-enterprise-integration-pack"></a><span data-ttu-id="53e0b-103">Validera XML med scheman för Logikappar i Azure och Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="53e0b-103">Validate XML with schemas for Azure Logic Apps and the Enterprise Integration Pack</span></span>

<span data-ttu-id="53e0b-104">Scheman bekräfta att XML-dokument som du får är giltiga och har de förväntade data i ett fördefinierat format.</span><span class="sxs-lookup"><span data-stu-id="53e0b-104">Schemas confirm that the XML documents you receive are valid and have the expected data in a predefined format.</span></span> <span data-ttu-id="53e0b-105">Scheman kan också hjälpa Validera meddelanden som utbyts i ett B2B-scenario.</span><span class="sxs-lookup"><span data-stu-id="53e0b-105">Schemas also help validate messages that are exchanged in a B2B scenario.</span></span>

## <a name="add-a-schema"></a><span data-ttu-id="53e0b-106">Lägger till ett schema</span><span class="sxs-lookup"><span data-stu-id="53e0b-106">Add a schema</span></span>

1. <span data-ttu-id="53e0b-107">Välj i Azure-portalen **fler tjänster**.</span><span class="sxs-lookup"><span data-stu-id="53e0b-107">In the Azure portal, select **More services**.</span></span>

    ![Azure-portalen ”fler tjänster”](media/logic-apps-enterprise-integration-schemas/overview-11.png)

2. <span data-ttu-id="53e0b-109">Skriv i sökrutan filtrera **integrering**, och välj **Integrationskonton** från resultatlistan över.</span><span class="sxs-lookup"><span data-stu-id="53e0b-109">In the filter search box, enter **integration**, and select **Integration Accounts** from the results list.</span></span>

    ![Filtrera sökrutan](media/logic-apps-enterprise-integration-schemas/overview-21.png)

3. <span data-ttu-id="53e0b-111">Välj den **integrering konto** där du vill lägga till schemat.</span><span class="sxs-lookup"><span data-stu-id="53e0b-111">Select the **integration account** where you want to add the schema.</span></span>

    ![Lista över integrationskonton](media/logic-apps-enterprise-integration-schemas/overview-31.png)

4. <span data-ttu-id="53e0b-113">Välj den **scheman** panelen.</span><span class="sxs-lookup"><span data-stu-id="53e0b-113">Choose the **Schemas** tile.</span></span>

    ![Exempel integration kontot, ”scheman”](media/logic-apps-enterprise-integration-schemas/schema-11.png)

### <a name="add-a-schema-file-smaller-than-2-mb"></a><span data-ttu-id="53e0b-115">Lägga till en schemafil som är mindre än 2 MB</span><span class="sxs-lookup"><span data-stu-id="53e0b-115">Add a schema file smaller than 2 MB</span></span>

1. <span data-ttu-id="53e0b-116">I den **scheman** bladet som öppnas (från föregående steg), väljer **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="53e0b-116">In the **Schemas** blade that opens (from the preceding steps), choose **Add**.</span></span>

    ![Scheman bladet ”Lägg till”](media/logic-apps-enterprise-integration-schemas/schema-21.png)

2. <span data-ttu-id="53e0b-118">Ange ett namn för schemat.</span><span class="sxs-lookup"><span data-stu-id="53e0b-118">Enter a name for your schema.</span></span> <span data-ttu-id="53e0b-119">Överföra schemafilen genom att välja mappikonen bredvid den **schemat** rutan.</span><span class="sxs-lookup"><span data-stu-id="53e0b-119">Upload the schema file by selecting the folder icon next to the **Schema** box.</span></span> <span data-ttu-id="53e0b-120">När överföringen är klar väljer du **OK**.</span><span class="sxs-lookup"><span data-stu-id="53e0b-120">After the upload process completes, select **OK**.</span></span>

    ![Skärmbild av ”Lägg till schemat” med ”liten fil” markerat](media/logic-apps-enterprise-integration-schemas/schema-31.png)

### <a name="add-a-schema-file-larger-than-2-mb-up-to-8-mb-maximum"></a><span data-ttu-id="53e0b-122">Lägga till en schemafil som är större än 2 MB (upp till 8 MB maximalt)</span><span class="sxs-lookup"><span data-stu-id="53e0b-122">Add a schema file larger than 2 MB (up to 8 MB maximum)</span></span>

<span data-ttu-id="53e0b-123">De här stegen variera beroende på åtkomstnivå för blob-behållare: **offentliga** eller **ingen anonym åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="53e0b-123">These steps differ based on the blob container access level: **Public** or **No anonymous access**.</span></span>

<span data-ttu-id="53e0b-124">**Att fastställa den här åtkomstnivån**</span><span class="sxs-lookup"><span data-stu-id="53e0b-124">**To determine this access level**</span></span>

1.  <span data-ttu-id="53e0b-125">Öppna **Azure Lagringsutforskaren**.</span><span class="sxs-lookup"><span data-stu-id="53e0b-125">Open **Azure Storage Explorer**.</span></span> 

2.  <span data-ttu-id="53e0b-126">Under **Blobbbehållare**, Välj blob-behållare som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="53e0b-126">Under **Blob Containers**, select the blob container you want.</span></span> 

3.  <span data-ttu-id="53e0b-127">Välj **säkerhet**, **åtkomstnivå**.</span><span class="sxs-lookup"><span data-stu-id="53e0b-127">Select **Security**, **Access Level**.</span></span>

<span data-ttu-id="53e0b-128">Om blob-säkerhetsåtkomstnivå **offentliga**, Följ dessa steg.</span><span class="sxs-lookup"><span data-stu-id="53e0b-128">If the blob security access level is **Public**, follow these steps.</span></span>

![Azure Lagringsutforskaren med ”Blob-behållare”, ”säkerhet” och ”offentliga” markerat](media/logic-apps-enterprise-integration-schemas/blob-public.png)

1. <span data-ttu-id="53e0b-130">Överför schemat till ditt lagringskonto och kopiera URI: N.</span><span class="sxs-lookup"><span data-stu-id="53e0b-130">Upload the schema to your storage account, and copy the URI.</span></span>

    ![Storage-konto med URI markerat](media/logic-apps-enterprise-integration-schemas/schema-blob.png)

2. <span data-ttu-id="53e0b-132">I **Lägg till Schema**väljer **stora filer**, och ange URI i den **innehåll URI** textruta.</span><span class="sxs-lookup"><span data-stu-id="53e0b-132">In **Add Schema**, select **Large file**, and provide the URI in the **Content URI** text box.</span></span>

    ![Scheman med knappen ”Lägg till” och ”stora filer” markerat](media/logic-apps-enterprise-integration-schemas/schema-largefile.png)

<span data-ttu-id="53e0b-134">Om blob-säkerhetsåtkomstnivå **ingen anonym åtkomst**, Följ dessa steg.</span><span class="sxs-lookup"><span data-stu-id="53e0b-134">If the blob security access level is **No anonymous access**, follow these steps.</span></span>

![Azure Lagringsutforskaren med ”Blob-behållare”, ”säkerhet” och ”ingen anonym åtkomst” markerat](media/logic-apps-enterprise-integration-schemas/blob-1.png)

1. <span data-ttu-id="53e0b-136">Överför schemat till ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="53e0b-136">Upload the schema to your storage account.</span></span>

    ![Lagringskonto](media/logic-apps-enterprise-integration-schemas/blob-3.png)

2. <span data-ttu-id="53e0b-138">Generera en signatur för delad åtkomst för schemat.</span><span class="sxs-lookup"><span data-stu-id="53e0b-138">Generate a shared access signature for the schema.</span></span>

    ![Storage-konto med delad åtkomst signaturer fliken markerat](media/logic-apps-enterprise-integration-schemas/blob-2.png)

3. <span data-ttu-id="53e0b-140">I **Lägg till Schema**väljer **stora filer**, och ange signatur för delad åtkomst URI i den **innehåll URI** textruta.</span><span class="sxs-lookup"><span data-stu-id="53e0b-140">In **Add Schema**, select **Large file**, and provide the shared access signature URI in the **Content URI** text box.</span></span>

    ![Scheman med knappen ”Lägg till” och ”stora filer” markerat](media/logic-apps-enterprise-integration-schemas/schema-largefile.png)

4. <span data-ttu-id="53e0b-142">I den **scheman** bladet för ditt konto integration nytillagda schemat ska visas.</span><span class="sxs-lookup"><span data-stu-id="53e0b-142">In the **Schemas** blade of your integration account, your newly added schema should appear.</span></span>

    ![Kontot integrering med ”scheman” och det nya schemat markerat](media/logic-apps-enterprise-integration-schemas/schema-41.png)

## <a name="edit-schemas"></a><span data-ttu-id="53e0b-144">Redigera scheman</span><span class="sxs-lookup"><span data-stu-id="53e0b-144">Edit schemas</span></span>

1. <span data-ttu-id="53e0b-145">Välj den **scheman** panelen.</span><span class="sxs-lookup"><span data-stu-id="53e0b-145">Choose the **Schemas** tile.</span></span>

2. <span data-ttu-id="53e0b-146">Efter den **scheman** blad öppnas väljer du det schema som du vill redigera.</span><span class="sxs-lookup"><span data-stu-id="53e0b-146">After the **Schemas** blade opens, select the schema that you want to edit.</span></span>

3. <span data-ttu-id="53e0b-147">På den **scheman** bladet välj **redigera**.</span><span class="sxs-lookup"><span data-stu-id="53e0b-147">On the **Schemas** blade, choose **Edit**.</span></span>

    ![Scheman bladet](media/logic-apps-enterprise-integration-schemas/edit-12.png)

4. <span data-ttu-id="53e0b-149">Välj schemafilen som du vill redigera och välj sedan **öppna**.</span><span class="sxs-lookup"><span data-stu-id="53e0b-149">Select the schema file that you want to edit, then select **Open**.</span></span>

    ![Öppna schemafilen redigera](media/logic-apps-enterprise-integration-schemas/edit-31.png)

<span data-ttu-id="53e0b-151">Azure visar ett meddelande att schemat har överförts.</span><span class="sxs-lookup"><span data-stu-id="53e0b-151">Azure shows a message that the schema uploaded successfully.</span></span>

## <a name="delete-schemas"></a><span data-ttu-id="53e0b-152">Ta bort scheman</span><span class="sxs-lookup"><span data-stu-id="53e0b-152">Delete schemas</span></span>

1. <span data-ttu-id="53e0b-153">Välj den **scheman** panelen.</span><span class="sxs-lookup"><span data-stu-id="53e0b-153">Choose the **Schemas** tile.</span></span>

2. <span data-ttu-id="53e0b-154">Efter den **scheman** öppnas bladet Välj det schema som du vill ta bort.</span><span class="sxs-lookup"><span data-stu-id="53e0b-154">After the **Schemas** blade opens, select the schema you want to delete.</span></span>

3. <span data-ttu-id="53e0b-155">På den **scheman** bladet välj **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="53e0b-155">On the **Schemas** blade, choose **Delete**.</span></span>

    ![Scheman bladet](media/logic-apps-enterprise-integration-schemas/delete-12.png)

4. <span data-ttu-id="53e0b-157">För att bekräfta att du vill ta bort det aktuella schemat, Välj **Ja**.</span><span class="sxs-lookup"><span data-stu-id="53e0b-157">To confirm that you want to delete the selected schema, choose **Yes**.</span></span>

    ![Schemat bekräftelse-meddelande](media/logic-apps-enterprise-integration-schemas/delete-21.png)

    <span data-ttu-id="53e0b-159">I den **scheman** bladet listan schemat uppdateras och innehåller inte längre det schema som du har tagit bort.</span><span class="sxs-lookup"><span data-stu-id="53e0b-159">In the **Schemas** blade, the schema list refreshes  and no longer includes the schema that you deleted.</span></span>

    ![Din integrering konto med ”scheman” markerat](media/logic-apps-enterprise-integration-schemas/delete-31.png)

## <a name="next-steps"></a><span data-ttu-id="53e0b-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="53e0b-161">Next steps</span></span>
* <span data-ttu-id="53e0b-162">[Mer information om Enterprise-Integrationspaket](logic-apps-enterprise-integration-overview.md "Lär dig mer om enterprise-integrationspaket").</span><span class="sxs-lookup"><span data-stu-id="53e0b-162">[Learn more about the Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "Learn about the enterprise integration pack").</span></span>  

