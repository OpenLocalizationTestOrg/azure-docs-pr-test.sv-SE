---
title: "aaaSchemas för XML-verifiering - Azure Logic Apps | Microsoft Docs"
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
ms.openlocfilehash: 87cf92741e10ff7cccd260f27442909e34928903
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="validate-xml-with-schemas-for-azure-logic-apps-and-hello-enterprise-integration-pack"></a><span data-ttu-id="02eb0-103">Validera XML med scheman för Logikappar i Azure och hello Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="02eb0-103">Validate XML with schemas for Azure Logic Apps and hello Enterprise Integration Pack</span></span>

<span data-ttu-id="02eb0-104">Scheman bekräfta att hello XML-dokument som du får är giltiga och har hello förväntade data i ett fördefinierat format.</span><span class="sxs-lookup"><span data-stu-id="02eb0-104">Schemas confirm that hello XML documents you receive are valid and have hello expected data in a predefined format.</span></span> <span data-ttu-id="02eb0-105">Scheman kan också hjälpa Validera meddelanden som utbyts i ett B2B-scenario.</span><span class="sxs-lookup"><span data-stu-id="02eb0-105">Schemas also help validate messages that are exchanged in a B2B scenario.</span></span>

## <a name="add-a-schema"></a><span data-ttu-id="02eb0-106">Lägger till ett schema</span><span class="sxs-lookup"><span data-stu-id="02eb0-106">Add a schema</span></span>

1. <span data-ttu-id="02eb0-107">Välj i hello Azure-portalen, **fler tjänster**.</span><span class="sxs-lookup"><span data-stu-id="02eb0-107">In hello Azure portal, select **More services**.</span></span>

    ![Azure-portalen ”fler tjänster”](media/logic-apps-enterprise-integration-schemas/overview-11.png)

2. <span data-ttu-id="02eb0-109">Skriv i sökrutan för hello filter **integrering**, och välj **Integrationskonton** från hello resultatlistan.</span><span class="sxs-lookup"><span data-stu-id="02eb0-109">In hello filter search box, enter **integration**, and select **Integration Accounts** from hello results list.</span></span>

    ![Filtrera sökrutan](media/logic-apps-enterprise-integration-schemas/overview-21.png)

3. <span data-ttu-id="02eb0-111">Välj hello **integrering konto** där du vill att tooadd hello schemat.</span><span class="sxs-lookup"><span data-stu-id="02eb0-111">Select hello **integration account** where you want tooadd hello schema.</span></span>

    ![Lista över integrationskonton](media/logic-apps-enterprise-integration-schemas/overview-31.png)

4. <span data-ttu-id="02eb0-113">Välj hello **scheman** panelen.</span><span class="sxs-lookup"><span data-stu-id="02eb0-113">Choose hello **Schemas** tile.</span></span>

    ![Exempel integration kontot, ”scheman”](media/logic-apps-enterprise-integration-schemas/schema-11.png)

### <a name="add-a-schema-file-smaller-than-2-mb"></a><span data-ttu-id="02eb0-115">Lägga till en schemafil som är mindre än 2 MB</span><span class="sxs-lookup"><span data-stu-id="02eb0-115">Add a schema file smaller than 2 MB</span></span>

1. <span data-ttu-id="02eb0-116">I hello **scheman** bladet som öppnas (från hello som föregående steg IT-avdelning), väljer **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="02eb0-116">In hello **Schemas** blade that opens (from hello preceding steps), choose **Add**.</span></span>

    ![Scheman bladet ”Lägg till”](media/logic-apps-enterprise-integration-schemas/schema-21.png)

2. <span data-ttu-id="02eb0-118">Ange ett namn för schemat.</span><span class="sxs-lookup"><span data-stu-id="02eb0-118">Enter a name for your schema.</span></span> <span data-ttu-id="02eb0-119">Överför hello schemafilen genom att välja hello mappen ikonen nästa toohello **schemat** rutan.</span><span class="sxs-lookup"><span data-stu-id="02eb0-119">Upload hello schema file by selecting hello folder icon next toohello **Schema** box.</span></span> <span data-ttu-id="02eb0-120">När hello överföringen är klar väljer du **OK**.</span><span class="sxs-lookup"><span data-stu-id="02eb0-120">After hello upload process completes, select **OK**.</span></span>

    ![Skärmbild av ”Lägg till schemat” med ”liten fil” markerat](media/logic-apps-enterprise-integration-schemas/schema-31.png)

### <a name="add-a-schema-file-larger-than-2-mb-up-too8-mb-maximum"></a><span data-ttu-id="02eb0-122">Lägga till en schemafil som är större än 2 MB (upp too8 MB maximalt)</span><span class="sxs-lookup"><span data-stu-id="02eb0-122">Add a schema file larger than 2 MB (up too8 MB maximum)</span></span>

<span data-ttu-id="02eb0-123">De här stegen variera beroende på åtkomstnivå för hello blob-behållare: **offentliga** eller **ingen anonym åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="02eb0-123">These steps differ based on hello blob container access level: **Public** or **No anonymous access**.</span></span>

<span data-ttu-id="02eb0-124">**toodetermine på den här åtkomstnivån**</span><span class="sxs-lookup"><span data-stu-id="02eb0-124">**toodetermine this access level**</span></span>

1.  <span data-ttu-id="02eb0-125">Öppna **Azure Lagringsutforskaren**.</span><span class="sxs-lookup"><span data-stu-id="02eb0-125">Open **Azure Storage Explorer**.</span></span> 

2.  <span data-ttu-id="02eb0-126">Under **Blobbbehållare**väljer hello blob-behållare som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="02eb0-126">Under **Blob Containers**, select hello blob container you want.</span></span> 

3.  <span data-ttu-id="02eb0-127">Välj **säkerhet**, **åtkomstnivå**.</span><span class="sxs-lookup"><span data-stu-id="02eb0-127">Select **Security**, **Access Level**.</span></span>

<span data-ttu-id="02eb0-128">Om hello blob säkerhetsåtkomstnivå **offentliga**, Följ dessa steg.</span><span class="sxs-lookup"><span data-stu-id="02eb0-128">If hello blob security access level is **Public**, follow these steps.</span></span>

![Azure Lagringsutforskaren med ”Blob-behållare”, ”säkerhet” och ”offentliga” markerat](media/logic-apps-enterprise-integration-schemas/blob-public.png)

1. <span data-ttu-id="02eb0-130">Överför hello schemat tooyour storage-konto och kopiera hello URI.</span><span class="sxs-lookup"><span data-stu-id="02eb0-130">Upload hello schema tooyour storage account, and copy hello URI.</span></span>

    ![Storage-konto med URI markerat](media/logic-apps-enterprise-integration-schemas/schema-blob.png)

2. <span data-ttu-id="02eb0-132">I **Lägg till Schema**väljer **stora filer**, och ange hello URI i hello **innehåll URI** textruta.</span><span class="sxs-lookup"><span data-stu-id="02eb0-132">In **Add Schema**, select **Large file**, and provide hello URI in hello **Content URI** text box.</span></span>

    ![Scheman med knappen ”Lägg till” och ”stora filer” markerat](media/logic-apps-enterprise-integration-schemas/schema-largefile.png)

<span data-ttu-id="02eb0-134">Om hello blob säkerhetsåtkomstnivå **ingen anonym åtkomst**, Följ dessa steg.</span><span class="sxs-lookup"><span data-stu-id="02eb0-134">If hello blob security access level is **No anonymous access**, follow these steps.</span></span>

![Azure Lagringsutforskaren med ”Blob-behållare”, ”säkerhet” och ”ingen anonym åtkomst” markerat](media/logic-apps-enterprise-integration-schemas/blob-1.png)

1. <span data-ttu-id="02eb0-136">Överför hello schemat tooyour storage-konto.</span><span class="sxs-lookup"><span data-stu-id="02eb0-136">Upload hello schema tooyour storage account.</span></span>

    ![Lagringskonto](media/logic-apps-enterprise-integration-schemas/blob-3.png)

2. <span data-ttu-id="02eb0-138">Generera en signatur för delad åtkomst för hello-schemat.</span><span class="sxs-lookup"><span data-stu-id="02eb0-138">Generate a shared access signature for hello schema.</span></span>

    ![Storage-konto med delad åtkomst signaturer fliken markerat](media/logic-apps-enterprise-integration-schemas/blob-2.png)

3. <span data-ttu-id="02eb0-140">I **Lägg till Schema**väljer **stora filer**, och ange hello delad åtkomstsignatur URI i hello **innehåll URI** textruta.</span><span class="sxs-lookup"><span data-stu-id="02eb0-140">In **Add Schema**, select **Large file**, and provide hello shared access signature URI in hello **Content URI** text box.</span></span>

    ![Scheman med knappen ”Lägg till” och ”stora filer” markerat](media/logic-apps-enterprise-integration-schemas/schema-largefile.png)

4. <span data-ttu-id="02eb0-142">I hello **scheman** bladet för ditt konto integration nytillagda schemat ska visas.</span><span class="sxs-lookup"><span data-stu-id="02eb0-142">In hello **Schemas** blade of your integration account, your newly added schema should appear.</span></span>

    ![Kontot integrering med ”scheman” och hello nya schemat markerat](media/logic-apps-enterprise-integration-schemas/schema-41.png)

## <a name="edit-schemas"></a><span data-ttu-id="02eb0-144">Redigera scheman</span><span class="sxs-lookup"><span data-stu-id="02eb0-144">Edit schemas</span></span>

1. <span data-ttu-id="02eb0-145">Välj hello **scheman** panelen.</span><span class="sxs-lookup"><span data-stu-id="02eb0-145">Choose hello **Schemas** tile.</span></span>

2. <span data-ttu-id="02eb0-146">Efter hello **scheman** blad öppnas, Välj hello schema som du vill tooedit.</span><span class="sxs-lookup"><span data-stu-id="02eb0-146">After hello **Schemas** blade opens, select hello schema that you want tooedit.</span></span>

3. <span data-ttu-id="02eb0-147">På hello **scheman** bladet välj **redigera**.</span><span class="sxs-lookup"><span data-stu-id="02eb0-147">On hello **Schemas** blade, choose **Edit**.</span></span>

    ![Scheman bladet](media/logic-apps-enterprise-integration-schemas/edit-12.png)

4. <span data-ttu-id="02eb0-149">Välj hello schemafilen som du vill tooedit och sedan välja **öppna**.</span><span class="sxs-lookup"><span data-stu-id="02eb0-149">Select hello schema file that you want tooedit, then select **Open**.</span></span>

    ![Öppna schemat filen tooedit](media/logic-apps-enterprise-integration-schemas/edit-31.png)

<span data-ttu-id="02eb0-151">Azure visas ett meddelande som hello schema har överförts.</span><span class="sxs-lookup"><span data-stu-id="02eb0-151">Azure shows a message that hello schema uploaded successfully.</span></span>

## <a name="delete-schemas"></a><span data-ttu-id="02eb0-152">Ta bort scheman</span><span class="sxs-lookup"><span data-stu-id="02eb0-152">Delete schemas</span></span>

1. <span data-ttu-id="02eb0-153">Välj hello **scheman** panelen.</span><span class="sxs-lookup"><span data-stu-id="02eb0-153">Choose hello **Schemas** tile.</span></span>

2. <span data-ttu-id="02eb0-154">Efter hello **scheman** blad öppnas väljer hello schema som du vill använda toodelete.</span><span class="sxs-lookup"><span data-stu-id="02eb0-154">After hello **Schemas** blade opens, select hello schema you want toodelete.</span></span>

3. <span data-ttu-id="02eb0-155">På hello **scheman** bladet välj **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="02eb0-155">On hello **Schemas** blade, choose **Delete**.</span></span>

    ![Scheman bladet](media/logic-apps-enterprise-integration-schemas/delete-12.png)

4. <span data-ttu-id="02eb0-157">tooconfirm som du vill toodelete hello valts schemat, Välj **Ja**.</span><span class="sxs-lookup"><span data-stu-id="02eb0-157">tooconfirm that you want toodelete hello selected schema, choose **Yes**.</span></span>

    ![Schemat bekräftelse-meddelande](media/logic-apps-enterprise-integration-schemas/delete-21.png)

    <span data-ttu-id="02eb0-159">I hello **scheman** bladet hello schemat listan uppdateras och innehåller inte längre hello-schema som du har tagit bort.</span><span class="sxs-lookup"><span data-stu-id="02eb0-159">In hello **Schemas** blade, hello schema list refreshes  and no longer includes hello schema that you deleted.</span></span>

    ![Din integrering konto med ”scheman” markerat](media/logic-apps-enterprise-integration-schemas/delete-31.png)

## <a name="next-steps"></a><span data-ttu-id="02eb0-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="02eb0-161">Next steps</span></span>
* <span data-ttu-id="02eb0-162">[Mer information om hello Enterprise-Integrationspaket](logic-apps-enterprise-integration-overview.md "Lär dig mer om hello enterprise-integrationspaket").</span><span class="sxs-lookup"><span data-stu-id="02eb0-162">[Learn more about hello Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "Learn about hello enterprise integration pack").</span></span>  

