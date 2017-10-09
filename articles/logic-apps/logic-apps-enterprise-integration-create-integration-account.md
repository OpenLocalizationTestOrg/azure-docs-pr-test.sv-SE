---
title: "aaaCreate, länkar, ta bort eller flytta integration konton i Azure logikappar | Microsoft Docs"
description: "Hur toocreate en integration konto och länka det tooyour logikappar"
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: d3ad9e99-a9ee-477b-81bf-0881e11e632f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/23/2017
ms.author: LADocs; mandia
ms.openlocfilehash: fda6c91723b3e3624ee176df112ba8b6c9800273
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-an-integration-account"></a><span data-ttu-id="47dd0-103">Vad är en integrering konto?</span><span class="sxs-lookup"><span data-stu-id="47dd0-103">What is an integration account?</span></span>

<span data-ttu-id="47dd0-104">Ett konto för integrering tillåter enterprise integration apparna toomanage artefakter, inklusive scheman, kartor, certifikat, partners och avtal.</span><span class="sxs-lookup"><span data-stu-id="47dd0-104">An integration account allows enterprise integration apps toomanage artifacts, including schemas, maps, certificates, partners and agreements.</span></span> <span data-ttu-id="47dd0-105">Alla integration appar som du skapar använder en integration konto tooaccess dessa scheman, kartor, certifikat och så vidare.</span><span class="sxs-lookup"><span data-stu-id="47dd0-105">Any integration app you create uses an integration account tooaccess these schemas, maps, certificates, and so on.</span></span>

## <a name="create-an-integration-account"></a><span data-ttu-id="47dd0-106">Skapa ett konto för integrering</span><span class="sxs-lookup"><span data-stu-id="47dd0-106">Create an integration account</span></span>

1.  <span data-ttu-id="47dd0-107">Logga in toohello [Azure-portalen](http://portal.azure.com "Azure-portalen").</span><span class="sxs-lookup"><span data-stu-id="47dd0-107">Sign in toohello [Azure portal](http://portal.azure.com "Azure portal").</span></span> <span data-ttu-id="47dd0-108">Välj hello vänstra menyn **fler tjänster**.</span><span class="sxs-lookup"><span data-stu-id="47dd0-108">From hello left menu, select **More services**.</span></span>

    ![Välj ”fler tjänster”](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. <span data-ttu-id="47dd0-110">I sökrutan hello skriver du ”integration” för filtret.</span><span class="sxs-lookup"><span data-stu-id="47dd0-110">In hello search box, type "integration" for your filter.</span></span> <span data-ttu-id="47dd0-111">Markera i hello resultat **Integrationskonton**.</span><span class="sxs-lookup"><span data-stu-id="47dd0-111">In hello results list, select **Integration Accounts**.</span></span>

    ![Filtrera efter ”integration”, Välj ”Integrationskonton”](./media/logic-apps-enterprise-integration-accounts/account-2.png)  

3. <span data-ttu-id="47dd0-113">Överst hello på hello sidan, Välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="47dd0-113">At hello top of hello page, choose **Add**.</span></span>

    ![Välj Lägg till](./media/logic-apps-enterprise-integration-accounts/account-3.png)

4. <span data-ttu-id="47dd0-115">Namnge ditt konto för integrering och välj hello Azure-prenumeration som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="47dd0-115">Name your integration account and select hello Azure subscription that you want toouse.</span></span> <span data-ttu-id="47dd0-116">Du kan antingen skapa en ny **resursgruppen** eller välj en befintlig resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="47dd0-116">You can either create a new **Resource group** or select an existing resource group.</span></span> <span data-ttu-id="47dd0-117">Välj en **plats** som värd för ditt konto för integrering och ett **prisnivån**.</span><span class="sxs-lookup"><span data-stu-id="47dd0-117">Then select a **Location** for hosting your integration account and a **Pricing Tier**.</span></span> 

    <span data-ttu-id="47dd0-118">När du är klar kan du välja **skapa**.</span><span class="sxs-lookup"><span data-stu-id="47dd0-118">When you're ready, choose **Create**.</span></span>

    ![Ange information för ditt konto för integrering](./media/logic-apps-enterprise-integration-accounts/account-4.png)

    <span data-ttu-id="47dd0-120">Azure tillhandahåller kontot integration hello valt plats, vilket bör vara klart inom 1 minut.</span><span class="sxs-lookup"><span data-stu-id="47dd0-120">Azure provisions your integration account  in hello selected location, which should complete within 1 minute.</span></span>

5. <span data-ttu-id="47dd0-121">Uppdatera hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="47dd0-121">Refresh hello page.</span></span> <span data-ttu-id="47dd0-122">Du ser ditt nya konto för integrering i listan.</span><span class="sxs-lookup"><span data-stu-id="47dd0-122">You see your new integration account listed.</span></span>

    ![Det nya kontot på integrering visas](./media/logic-apps-enterprise-integration-accounts/account-5.png) 

<span data-ttu-id="47dd0-124">Därefter länka hello integration konto som du skapade tooyour logikapp.</span><span class="sxs-lookup"><span data-stu-id="47dd0-124">Next, link hello integration account that you created tooyour logic app.</span></span> 

## <a name="link-an-integration-account-tooa-logic-app"></a><span data-ttu-id="47dd0-125">Länka en integration konto tooa logikapp</span><span class="sxs-lookup"><span data-stu-id="47dd0-125">Link an integration account tooa logic app</span></span>

<span data-ttu-id="47dd0-126">toogive dina logic apps komma åt toomaps, scheman, avtal och andra artefakter i kontot integration länken hello integration konto tooyour logikapp.</span><span class="sxs-lookup"><span data-stu-id="47dd0-126">toogive your logic apps access toomaps, schemas, agreements, and other artifacts in your integration account, link hello integration account tooyour logic app.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="47dd0-127">Krav</span><span class="sxs-lookup"><span data-stu-id="47dd0-127">Prerequisites</span></span>

* <span data-ttu-id="47dd0-128">Ett konto för integrering</span><span class="sxs-lookup"><span data-stu-id="47dd0-128">An integration account</span></span>
* <span data-ttu-id="47dd0-129">En logikapp</span><span class="sxs-lookup"><span data-stu-id="47dd0-129">A logic app</span></span>

> [!NOTE] 
> <span data-ttu-id="47dd0-130">Kontrollera appen integration konto och logik i hello *samma Azure-plats* innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="47dd0-130">Make sure your integration account and logic app are in hello *same Azure location* before you begin.</span></span>


1. <span data-ttu-id="47dd0-131">Välj din logikapp i hello Azure-portalen och kontrollera platsen för din logikapp.</span><span class="sxs-lookup"><span data-stu-id="47dd0-131">In hello Azure portal, select your logic app, and check your logic app's location.</span></span>

    ![Välj din logikapp, Kontrollera platsen](./media/logic-apps-enterprise-integration-accounts/linkaccount-1.png)

2. <span data-ttu-id="47dd0-133">Under **inställningar**väljer **integrering konto**.</span><span class="sxs-lookup"><span data-stu-id="47dd0-133">Under **Settings**, select **Integration Account**.</span></span>

    ![Välj ”Integration konto”](./media/logic-apps-enterprise-integration-accounts/linkaccount-2.png)

3. <span data-ttu-id="47dd0-135">Från hello **välja ett konto för integrering** listan, Välj hello integration konto som du vill toolink tooyour logikapp.</span><span class="sxs-lookup"><span data-stu-id="47dd0-135">From hello **Select an Integration account** list, select hello integration account you want toolink tooyour logic app.</span></span> <span data-ttu-id="47dd0-136">toofinish länkar, Välj **spara**.</span><span class="sxs-lookup"><span data-stu-id="47dd0-136">toofinish linking, choose **Save**.</span></span>

    ![Välj kontot integrering](./media/logic-apps-enterprise-integration-accounts/linkaccount-3.png)

    <span data-ttu-id="47dd0-138">Du får ett meddelande som visar din integrering kontot är länkade tooyour logikapp och att alla artefakter i ditt konto för integrering är nu tillgängliga tooyour logikapp.</span><span class="sxs-lookup"><span data-stu-id="47dd0-138">You get a notification that shows your integration account is linked tooyour logic app,  and that all artifacts in your integration account are now available tooyour logic app.</span></span>

    ![Din logikapp länkas tooyour integrering konto](./media/logic-apps-enterprise-integration-accounts/linkaccount-5.png)

<span data-ttu-id="47dd0-140">Nu när ditt konto integration är länkade tooyour logikapp, kan du använda hello B2B-kopplingar i dina logic apps.</span><span class="sxs-lookup"><span data-stu-id="47dd0-140">Now that your integration account is linked tooyour logic app, you can use hello B2B connectors in your logic apps.</span></span> <span data-ttu-id="47dd0-141">Några vanliga B2B-kopplingar innehåller XML-verifiering och koda/avkoda flat-fil.</span><span class="sxs-lookup"><span data-stu-id="47dd0-141">Some common B2B connectors include XML validation and flat file encode/decode.</span></span>  

## <a name="delete-your-integration-account"></a><span data-ttu-id="47dd0-142">Ta bort ditt konto för integrering</span><span class="sxs-lookup"><span data-stu-id="47dd0-142">Delete your integration account</span></span>

1. <span data-ttu-id="47dd0-143">Välj **fler tjänster**.</span><span class="sxs-lookup"><span data-stu-id="47dd0-143">Select **More services**.</span></span>

    ![Välj ”fler tjänster”](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. <span data-ttu-id="47dd0-145">I sökrutan hello skriver du ”integration” för filtret.</span><span class="sxs-lookup"><span data-stu-id="47dd0-145">In hello search box, type "integration" for your filter.</span></span> <span data-ttu-id="47dd0-146">Markera i hello resultat **Integrationskonton**.</span><span class="sxs-lookup"><span data-stu-id="47dd0-146">In hello results list, select **Integration Accounts**.</span></span>

    ![Filtrera efter ”integration”, Välj ”Integrationskonton”](./media/logic-apps-enterprise-integration-accounts/account-2.png)  

3. <span data-ttu-id="47dd0-148">Välj hello integration konto som du vill toodelete.</span><span class="sxs-lookup"><span data-stu-id="47dd0-148">Select hello integration account that you want toodelete.</span></span>

    ![Välj integration konto toodelete](./media/logic-apps-enterprise-integration-accounts/account-5.png)

4. <span data-ttu-id="47dd0-150">Välj på menyn hello **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="47dd0-150">On hello menu, choose **Delete**.</span></span>

    ![Välj ”Ta bort”](./media/logic-apps-enterprise-integration-accounts/delete.png)

5. <span data-ttu-id="47dd0-152">Bekräfta ditt val toodelete hello integration konto.</span><span class="sxs-lookup"><span data-stu-id="47dd0-152">Confirm your choice toodelete hello integration account.</span></span>

## <a name="move-your-integration-account"></a><span data-ttu-id="47dd0-153">Flytta integration-kontot</span><span class="sxs-lookup"><span data-stu-id="47dd0-153">Move your integration account</span></span>

<span data-ttu-id="47dd0-154">toomove en integration konto tooanother Azure-prenumerationen eller resursen grupp, Följ dessa steg.</span><span class="sxs-lookup"><span data-stu-id="47dd0-154">toomove an integration account tooanother Azure subscription or resource group, follow these steps.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="47dd0-155">När du flyttar ett integration konto måste du uppdatera alla skript toouse hello ny resurs-ID.</span><span class="sxs-lookup"><span data-stu-id="47dd0-155">You must update all scripts toouse hello new resource IDs after you move an integration account.</span></span>

1. <span data-ttu-id="47dd0-156">Välj **fler tjänster**.</span><span class="sxs-lookup"><span data-stu-id="47dd0-156">Select **More services**.</span></span>

    ![Välj ”fler tjänster”](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. <span data-ttu-id="47dd0-158">I sökrutan hello skriver du ”integration” för filtret.</span><span class="sxs-lookup"><span data-stu-id="47dd0-158">In hello search box, type "integration" for your filter.</span></span> <span data-ttu-id="47dd0-159">Markera i hello resultat **Integrationskonton**.</span><span class="sxs-lookup"><span data-stu-id="47dd0-159">In hello results list, select **Integration Accounts**.</span></span>

    ![Filtrera efter ”integration”, Välj ”Integrationskonton”](./media/logic-apps-enterprise-integration-accounts/account-2.png)

3. <span data-ttu-id="47dd0-161">Välj hello integration konto som du vill toomove.</span><span class="sxs-lookup"><span data-stu-id="47dd0-161">Select hello integration account that you want toomove.</span></span> <span data-ttu-id="47dd0-162">Under **inställningar**, Välj **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="47dd0-162">Under **Settings**, choose **Properties**.</span></span>

    ![Välj integration konto toomove.](./media/logic-apps-enterprise-integration-accounts/move.png)

5. <span data-ttu-id="47dd0-165">Ändra hello resursgrupp eller Azure-prenumeration som är kopplad till ditt konto för integrering.</span><span class="sxs-lookup"><span data-stu-id="47dd0-165">Change hello resource group or Azure subscription that's associated with your integration account.</span></span>

    ![Välj Ändra resursgrupp eller ändra prenumerationen](./media/logic-apps-enterprise-integration-accounts/move-2.png)

## <a name="next-steps"></a><span data-ttu-id="47dd0-167">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="47dd0-167">Next Steps</span></span>
* [<span data-ttu-id="47dd0-168">Mer information om avtalen</span><span class="sxs-lookup"><span data-stu-id="47dd0-168">Learn more about agreements</span></span>](../logic-apps/logic-apps-enterprise-integration-agreements.md "Lär dig mer om enterprise integration-avtal")  

