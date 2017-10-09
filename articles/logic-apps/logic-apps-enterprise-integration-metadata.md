---
title: aaaManage integrering konto artefakt metadata - Azure Logic Apps | Microsoft Docs
description: "Lägg till eller hämta artefakt metadata från integrationskonton för Logikappar i Azure"
author: padmavc
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 11/21/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 8de71bffa9f9975d5409716b2208fa6c3a9545d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-artifact-metadata-in-integration-accounts-for-logic-apps"></a><span data-ttu-id="5f96d-103">Hantera artefakt metadata i integrationskonton för logic apps</span><span class="sxs-lookup"><span data-stu-id="5f96d-103">Manage artifact metadata in integration accounts for logic apps</span></span>

<span data-ttu-id="5f96d-104">Du kan ange anpassade metadata för artefakter i integrationskonton och hämta den metadata under körningen för din logikapp.</span><span class="sxs-lookup"><span data-stu-id="5f96d-104">You can define custom metadata for artifacts in integration accounts and retrieve that metadata during runtime for your logic app.</span></span> <span data-ttu-id="5f96d-105">Exempelvis kan du ange metadata för artefakter som partners, avtal, scheman och maps - alla lagrar metadata med nyckel-värdepar.</span><span class="sxs-lookup"><span data-stu-id="5f96d-105">For example, you can specify metadata for artifacts like partners, agreements, schemas, and maps - all store metadata using key-value pairs.</span></span> <span data-ttu-id="5f96d-106">För närvarande artefakter kan inte skapa metadata via Användargränssnittet, men du kan använda REST API: er toocreate metadata.</span><span class="sxs-lookup"><span data-stu-id="5f96d-106">Currently, artifacts can't create metadata through UI, but you can use REST APIs toocreate metadata.</span></span> <span data-ttu-id="5f96d-107">Välj tooadd metadata när du skapar eller välj en partner, avtal eller schemat i hello Azure-portalen **redigera som JSON**.</span><span class="sxs-lookup"><span data-stu-id="5f96d-107">tooadd metadata when you create or select a partner, agreement, or schema in hello Azure portal, choose **Edit as JSON**.</span></span> <span data-ttu-id="5f96d-108">Du kan använda hello Integration artefakt Kontouppslag funktionen tooretrieve artefakt metadata i logikappar.</span><span class="sxs-lookup"><span data-stu-id="5f96d-108">tooretrieve artifact metadata in logic apps, you can use hello Integration Account Artifact Lookup feature.</span></span>

## <a name="add-metadata-tooartifacts-in-integration-accounts"></a><span data-ttu-id="5f96d-109">Lägg till metadata tooartifacts i integrationskonton</span><span class="sxs-lookup"><span data-stu-id="5f96d-109">Add metadata tooartifacts in integration accounts</span></span>

1. <span data-ttu-id="5f96d-110">Skapa en [integrering konto](logic-apps-enterprise-integration-create-integration-account.md).</span><span class="sxs-lookup"><span data-stu-id="5f96d-110">Create an [integration account](logic-apps-enterprise-integration-create-integration-account.md).</span></span>

2. <span data-ttu-id="5f96d-111">Lägga till en artefakt tooyour integrering konto, till exempel en [partner](logic-apps-enterprise-integration-partners.md#how-to-create-a-partner), [avtal](logic-apps-enterprise-integration-agreements.md#how-to-create-agreements), eller [schemat](logic-apps-enterprise-integration-schemas.md).</span><span class="sxs-lookup"><span data-stu-id="5f96d-111">Add an artifact tooyour integration account, for example, a [partner](logic-apps-enterprise-integration-partners.md#how-to-create-a-partner), [agreement](logic-apps-enterprise-integration-agreements.md#how-to-create-agreements), or [schema](logic-apps-enterprise-integration-schemas.md).</span></span>

3.  <span data-ttu-id="5f96d-112">Markera hello artefakt, Välj **redigera som JSON**, och ange information om metadata.</span><span class="sxs-lookup"><span data-stu-id="5f96d-112">Select hello artifact, choose **Edit as JSON**, and enter metadata details.</span></span>

    ![Ange metadata](media/logic-apps-enterprise-integration-metadata/image1.png)

## <a name="retrieve-metadata-from-artifacts-for-logic-apps"></a><span data-ttu-id="5f96d-114">Hämta metadata från artefakter för logikappar</span><span class="sxs-lookup"><span data-stu-id="5f96d-114">Retrieve metadata from artifacts for logic apps</span></span>

1. <span data-ttu-id="5f96d-115">Skapa en [logikapp](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="5f96d-115">Create a [logic app](logic-apps-create-a-logic-app.md).</span></span>

2. <span data-ttu-id="5f96d-116">Skapa en [länk från kontot logik app tooyour integrering](logic-apps-enterprise-integration-create-integration-account.md#link-an-integration-account-to-a-logic-app).</span><span class="sxs-lookup"><span data-stu-id="5f96d-116">Create a [link from your logic app tooyour integration account](logic-apps-enterprise-integration-create-integration-account.md#link-an-integration-account-to-a-logic-app).</span></span> 

3. <span data-ttu-id="5f96d-117">Lägga till en utlösare som logik App Designer *begära* eller *HTTP* tooyour logikapp.</span><span class="sxs-lookup"><span data-stu-id="5f96d-117">In Logic App Designer, add a trigger like *Request* or *HTTP* tooyour logic app.</span></span>

4.  <span data-ttu-id="5f96d-118">Välj **nästa steg** > **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="5f96d-118">Choose **Next Step** > **Add an action**.</span></span> <span data-ttu-id="5f96d-119">Sök efter *integrering* så att du kan söka efter och välj sedan **integrering konto - Integration artefakt Kontouppslag**.</span><span class="sxs-lookup"><span data-stu-id="5f96d-119">Search for *integration* so you can find and then select **Integration Account - Integration Account Artifact Lookup**.</span></span>

    ![Välj Integration artefakt Kontouppslag](media/logic-apps-enterprise-integration-metadata/image2.png)

5. <span data-ttu-id="5f96d-121">Välj hello **typ av artefakt**, och ange hello **artefakt namnet**.</span><span class="sxs-lookup"><span data-stu-id="5f96d-121">Select hello **Artifact Type**, and provide hello **Artifact Name**.</span></span>

    ![Välj typ av artefakt och ange artefakt namn](media/logic-apps-enterprise-integration-metadata/image3.png)

## <a name="example-retrieve-partner-metadata"></a><span data-ttu-id="5f96d-123">Exempel: Hämta partner metadata</span><span class="sxs-lookup"><span data-stu-id="5f96d-123">Example: Retrieve partner metadata</span></span>

<span data-ttu-id="5f96d-124">Partner metadata har dessa `routingUrl` information:</span><span class="sxs-lookup"><span data-stu-id="5f96d-124">Partner metadata has these `routingUrl` details:</span></span>

![Hitta partner ”routingURL” metadata](media/logic-apps-enterprise-integration-metadata/image6.png)

1. <span data-ttu-id="5f96d-126">Lägg till din utlösare i din logikapp, en **integrering konto - Integration artefakt Kontouppslag** åtgärd för din partner och en **HTTP**.</span><span class="sxs-lookup"><span data-stu-id="5f96d-126">In your logic app, add your trigger, an **Integration Account - Integration Account Artifact Lookup** action for your partner, and an **HTTP**.</span></span>

    ![Lägga till utlösare och artefakt sökning ”HTTP” tooyour logikapp](media/logic-apps-enterprise-integration-metadata/image4.png)

2. <span data-ttu-id="5f96d-128">tooretrieve hello URI, gå tooCode vyn för din logikapp.</span><span class="sxs-lookup"><span data-stu-id="5f96d-128">tooretrieve hello URI, go tooCode View for your logic app.</span></span> <span data-ttu-id="5f96d-129">Din logik app definition bör se ut som i det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="5f96d-129">Your logic app definition should look like this example:</span></span>

    ![Sök-sökning](media/logic-apps-enterprise-integration-metadata/image5.png)


## <a name="next-steps"></a><span data-ttu-id="5f96d-131">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5f96d-131">Next steps</span></span>
* [<span data-ttu-id="5f96d-132">Mer information om avtalen</span><span class="sxs-lookup"><span data-stu-id="5f96d-132">Learn more about agreements</span></span>](logic-apps-enterprise-integration-agreements.md "Lär dig mer om enterprise integration-avtal")  
