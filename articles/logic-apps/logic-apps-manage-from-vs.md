---
title: aaaManage logikappar i Visual Studio - Azure Logic Apps | Microsoft Docs
description: "Hantera logikappar och andra Azure tillgångar med Visual Studio Cloud Explorer"
author: klam
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 12/19/2016
ms.author: LADocs; klam
ms.openlocfilehash: 419f83eb062b56e4ac2642dea4de1a025f747521
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-your-logic-apps-with-visual-studio-cloud-explorer"></a><span data-ttu-id="1e060-103">Hantera dina logic apps med Visual Studio Cloud Explorer</span><span class="sxs-lookup"><span data-stu-id="1e060-103">Manage your logic apps with Visual Studio Cloud Explorer</span></span>

<span data-ttu-id="1e060-104">Även om hello [Azure-portalen](https://portal.azure.com/) erbjuder ett bra sätt du toodesign och hantera Azure Logikappar kan du använda Visual Studio Cloud Explorer för att hantera många Azure tillgångar, inklusive logikappar.</span><span class="sxs-lookup"><span data-stu-id="1e060-104">Although hello [Azure portal](https://portal.azure.com/) offers a great way for you toodesign and manage Azure Logic Apps, you can use Visual Studio Cloud Explorer for managing many Azure assets, including logic apps.</span></span> <span data-ttu-id="1e060-105">Visual Studio Cloud Explorer kan du bläddra, hantera, redigera och hämta publicerade logikappar.</span><span class="sxs-lookup"><span data-stu-id="1e060-105">Visual Studio Cloud Explorer lets you browse, manage, edit, and download published logic apps.</span></span> <span data-ttu-id="1e060-106">Hanteringsuppgifter omfattar aktivera, inaktivera och visa kör historik.</span><span class="sxs-lookup"><span data-stu-id="1e060-106">Management tasks include enable, disable, and view run history.</span></span> 

<span data-ttu-id="1e060-107">Innan du kan komma åt och hantera dina logic apps i Visual Studio, installera och konfigurera de här Visual Studio-verktygen för Logikappar i Azure.</span><span class="sxs-lookup"><span data-stu-id="1e060-107">Before you can access and manage your logic apps in Visual Studio, install and configure these Visual Studio tools for Azure Logic Apps.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="1e060-108">Krav</span><span class="sxs-lookup"><span data-stu-id="1e060-108">Prerequisites</span></span>

* [<span data-ttu-id="1e060-109">Visual Studio 2015 eller Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="1e060-109">Visual Studio 2015 or Visual Studio 2017</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
* <span data-ttu-id="1e060-110">[Den senaste Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 eller senare)</span><span class="sxs-lookup"><span data-stu-id="1e060-110">[Latest Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 or greater)</span></span>
* [<span data-ttu-id="1e060-111">Visual Studio Cloud Explorer</span><span class="sxs-lookup"><span data-stu-id="1e060-111">Visual Studio Cloud Explorer</span></span>](https://marketplace.visualstudio.com/items?itemName=MicrosoftCloudExplorer.CloudExplorerforVisualStudio2015)
* <span data-ttu-id="1e060-112">Åtkomst toohello web när du använder hello inbäddade designer</span><span class="sxs-lookup"><span data-stu-id="1e060-112">Access toohello web when using hello embedded designer</span></span>

## <a name="install-visual-studio-tools-for-logic-apps"></a><span data-ttu-id="1e060-113">Installera Visual Studio tools för Logic Apps</span><span class="sxs-lookup"><span data-stu-id="1e060-113">Install Visual Studio tools for Logic Apps</span></span>

<span data-ttu-id="1e060-114">När du har installerat hello krav, hämta och installera hello Azure Logic Apps verktyg för Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1e060-114">After you install hello prerequisites, download and install hello Azure Logic Apps Tools for Visual Studio.</span></span>

1. <span data-ttu-id="1e060-115">Öppna Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1e060-115">Open Visual Studio.</span></span> <span data-ttu-id="1e060-116">På hello **verktyg** väljer du **tillägg och uppdateringar**.</span><span class="sxs-lookup"><span data-stu-id="1e060-116">On hello **Tools** menu, select **Extensions and Updates**.</span></span>
2. <span data-ttu-id="1e060-117">Expandera hello **Online** kategori så att du kan söka online i hello Visual Studio-galleriet.</span><span class="sxs-lookup"><span data-stu-id="1e060-117">Expand hello **Online** category so you can search online in hello Visual Studio Gallery.</span></span>
3. <span data-ttu-id="1e060-118">Bläddra eller Sök efter **Logikappar** tills du hittar **Azure Logic Apps Tools för Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="1e060-118">Browse or search for **Logic Apps** until you find **Azure Logic Apps Tools for Visual Studio**.</span></span>
4. <span data-ttu-id="1e060-119">toodownload och installera hello-tillägg, klickar du på **hämta**.</span><span class="sxs-lookup"><span data-stu-id="1e060-119">toodownload and install hello extension, click **Download**.</span></span>
5. <span data-ttu-id="1e060-120">Starta om Visual Studio efter installationen.</span><span class="sxs-lookup"><span data-stu-id="1e060-120">Restart Visual Studio after installation.</span></span>

> [!NOTE]
> <span data-ttu-id="1e060-121">toodownload hello Azure Logic Apps Tools för Visual Studio gå direkt, toohello [Visual Studio Marketplace](https://visualstudiogallery.msdn.microsoft.com/e25ad307-46cf-412e-8ba5-5b555d53d2d9).</span><span class="sxs-lookup"><span data-stu-id="1e060-121">toodownload hello Azure Logic Apps Tools for Visual Studio directly, go toohello [Visual Studio Marketplace](https://visualstudiogallery.msdn.microsoft.com/e25ad307-46cf-412e-8ba5-5b555d53d2d9).</span></span>

## <a name="browse-for-logic-apps-in-cloud-explorer"></a><span data-ttu-id="1e060-122">Bläddra efter logikappar i Cloud Explorer</span><span class="sxs-lookup"><span data-stu-id="1e060-122">Browse for logic apps in Cloud Explorer</span></span>

1.  <span data-ttu-id="1e060-123">tooopen Cloud Explorer på hello **visa** -menyn, Välj **Cloud Explorer**.</span><span class="sxs-lookup"><span data-stu-id="1e060-123">tooopen Cloud Explorer, on hello **View** menu, choose **Cloud Explorer**.</span></span>
2.  <span data-ttu-id="1e060-124">Leta upp din logikapp genom resursgruppen eller resursen.</span><span class="sxs-lookup"><span data-stu-id="1e060-124">Browse for your logic app, either by resource group or by resource type.</span></span> 

    * <span data-ttu-id="1e060-125">Om du bläddrar efter resurstyp, Välj din Azure-prenumeration, expandera hello **Logic Apps** avsnittet och väljer din logikapp.</span><span class="sxs-lookup"><span data-stu-id="1e060-125">If you browse by resource type, select your Azure subscription, expand hello **Logic Apps** section, and select your logic app.</span></span> 
    * <span data-ttu-id="1e060-126">Om du bläddrar med resursgrupp, expandera hello resursgrupp som har logikappen och välj din logikapp.</span><span class="sxs-lookup"><span data-stu-id="1e060-126">If you browse by resource group, expand hello resource group that has your logic app, and select your logic app.</span></span>

    <span data-ttu-id="1e060-127">tooview kommandon för din logikapp antingen högerklicka på din logikapp eller längst ned hello i Cloud Explorer, väljer hello **åtgärder** menyn.</span><span class="sxs-lookup"><span data-stu-id="1e060-127">tooview commands for your logic app, either right-click your logic app, or at hello bottom of Cloud Explorer, choose from hello **Actions** menu.</span></span>

    ![Bläddra efter din logikapp](./media/logic-apps-manage-from-vs/browse.png)

## <a name="edit-your-logic-app-with-logic-apps-designer"></a><span data-ttu-id="1e060-129">Redigera din logikapp med Logic Apps Designer</span><span class="sxs-lookup"><span data-stu-id="1e060-129">Edit your logic app with Logic Apps Designer</span></span>

<span data-ttu-id="1e060-130">Från Cloud Explorer kan du öppna en för tillfället distribuerade logikapp i hello samma designer som du använder i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1e060-130">From Cloud Explorer, you can open a currently deployed logic app in hello same designer that you use in hello Azure portal.</span></span> 

* <span data-ttu-id="1e060-131">tooedit logikappen i Cloud Explorer, högerklicka på din logikapp och välj **öppna med logik App Editor**.</span><span class="sxs-lookup"><span data-stu-id="1e060-131">tooedit your logic app, in Cloud Explorer, right-click your logic app, and select **Open with Logic App Editor**.</span></span> 

* <span data-ttu-id="1e060-132">toopublish uppdateringar-toohello molnet, Välj **publicera**.</span><span class="sxs-lookup"><span data-stu-id="1e060-132">toopublish your updates toohello cloud, choose **Publish**.</span></span> 

* <span data-ttu-id="1e060-133">Välj toostart ett nytt kör **köra utlösaren**.</span><span class="sxs-lookup"><span data-stu-id="1e060-133">toostart a new run, choose **Run Trigger**.</span></span>

![Logic Apps Designer](./media/logic-apps-manage-from-vs/designer.png)

<span data-ttu-id="1e060-135">Från hello designer kan du också **hämta** en logikapp.</span><span class="sxs-lookup"><span data-stu-id="1e060-135">From hello designer, you can also **Download** a logic app.</span></span> <span data-ttu-id="1e060-136">Den här åtgärden automatiskt parameterizes hello logik app definition och sparar hello definition som en Distributionsmall av Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="1e060-136">This action automatically parameterizes hello logic app definition, and saves hello definition as an Azure Resource Manager deployment template.</span></span> <span data-ttu-id="1e060-137">Du kan lägga till den här mallen tooyour Azure-resursgrupp distributionsprojekt.</span><span class="sxs-lookup"><span data-stu-id="1e060-137">You can add this deployment template tooyour Azure Resource Group project.</span></span>

## <a name="browse-your-logic-app-run-history"></a><span data-ttu-id="1e060-138">Bläddra logikappen kör historik</span><span class="sxs-lookup"><span data-stu-id="1e060-138">Browse your logic app run history</span></span>

<span data-ttu-id="1e060-139">tooview hello kör historik för din logikapp, högerklicka på din logikapp och välj **öppna kör historik**.</span><span class="sxs-lookup"><span data-stu-id="1e060-139">tooview hello run history for your logic app, right-click your logic app, and select **Open run history**.</span></span> <span data-ttu-id="1e060-140">tooreorder historiken kör baserat på något av hello egenskaper visas, väljer hello kolumnrubriken.</span><span class="sxs-lookup"><span data-stu-id="1e060-140">tooreorder your run history based on any of hello properties shown, select hello column header.</span></span>

![Kör historik](media/logic-apps-manage-from-vs/runs.png)

<span data-ttu-id="1e060-142">tooshow hello kör historik för en instans så att du kan granska hello köra resultatet, inklusive hello indata och utdata från varje steg dubbelklickar du på något av hello kör instanser.</span><span class="sxs-lookup"><span data-stu-id="1e060-142">tooshow hello run history for an instance so you can review hello run results, including hello inputs and outputs from each step, double-click one of hello run instances.</span></span>

![Kör historikresultat, indata och utdata från steg](./media/logic-apps-manage-from-vs/history.png)

## <a name="next-steps"></a><span data-ttu-id="1e060-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1e060-144">Next steps</span></span>

* [<span data-ttu-id="1e060-145">Skapa din första logiska app</span><span class="sxs-lookup"><span data-stu-id="1e060-145">Create your first logic app</span></span>](logic-apps-create-a-logic-app.md)
* [<span data-ttu-id="1e060-146">Utforma, skapa och distribuera logikappar i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1e060-146">Design, build, and deploy logic apps in Visual Studio</span></span>](logic-apps-deploy-from-vs.md)
* [<span data-ttu-id="1e060-147">Visa vanliga exempel och scenarier</span><span class="sxs-lookup"><span data-stu-id="1e060-147">View common examples and scenarios</span></span>](logic-apps-examples-and-scenarios.md)
* [<span data-ttu-id="1e060-148">Video: Automatisera affärsprocesser med Azure Logikappar</span><span class="sxs-lookup"><span data-stu-id="1e060-148">Video: Automate business processes with Azure Logic Apps</span></span>](http://channel9.msdn.com/Events/Build/2016/T694)
* [<span data-ttu-id="1e060-149">Video: Integrera dina system med Azure Logikappar</span><span class="sxs-lookup"><span data-stu-id="1e060-149">Video: Integrate your systems with Azure Logic Apps</span></span>](http://channel9.msdn.com/Events/Build/2016/P462)
