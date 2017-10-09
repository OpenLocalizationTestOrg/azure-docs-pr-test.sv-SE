---
title: "aaaAccess datakällor lokalt för Logikappar i Azure | Microsoft Docs"
description: "Konfigurera hello lokala datagateway så att du kan komma åt datakällor lokalt från logikappar"
keywords: "komma åt data på lokala, dataöverföring, kryptering och datakällor"
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 6cb4449d-e6b8-4c35-9862-15110ae73e6a
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/13/2017
ms.author: LADocs; dimazaid; estfan
ms.openlocfilehash: 1d3deaac5a095316ce78e224dab0c08559bc2ff2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="access-data-sources-on-premises-from-logic-apps-with-hello-on-premises-data-gateway"></a><span data-ttu-id="7c986-104">Komma åt datakällor lokalt från logikappar med hello lokala datagateway</span><span class="sxs-lookup"><span data-stu-id="7c986-104">Access data sources on premises from logic apps with hello on-premises data gateway</span></span>

<span data-ttu-id="7c986-105">tooaccess datakällor lokalt från dina logic apps ställa in en lokal datagateway logikappar kan använda med kopplingar som stöds.</span><span class="sxs-lookup"><span data-stu-id="7c986-105">tooaccess data sources on premises from your logic apps, set up an on-premises data gateway that logic apps can use with supported connectors.</span></span> <span data-ttu-id="7c986-106">hello gateway fungerar som en brygga som ger snabb överföring och kryptering mellan datakällor lokalt och dina logic apps.</span><span class="sxs-lookup"><span data-stu-id="7c986-106">hello gateway acts as a bridge that provides quick data transfer and encryption between data sources on premises and your logic apps.</span></span> <span data-ttu-id="7c986-107">hello gateway vidarebefordrar data från lokala datakällor på krypterade kanaler via hello Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="7c986-107">hello gateway relays data from on-premises sources on encrypted channels through hello Azure Service Bus.</span></span> <span data-ttu-id="7c986-108">All trafik som kommer från som säkra utgående trafik från hello gateway agent.</span><span class="sxs-lookup"><span data-stu-id="7c986-108">All traffic originates as secure outbound traffic from hello gateway agent.</span></span> <span data-ttu-id="7c986-109">Lär dig mer om [hur hello datagateway fungerar](logic-apps-gateway-install.md#gateway-cloud-service).</span><span class="sxs-lookup"><span data-stu-id="7c986-109">Learn more about [how hello data gateway works](logic-apps-gateway-install.md#gateway-cloud-service).</span></span> 

<span data-ttu-id="7c986-110">hello gateway har stöd för anslutningar toothese datakällor lokalt:</span><span class="sxs-lookup"><span data-stu-id="7c986-110">hello gateway supports connections toothese data sources on premises:</span></span>

*   <span data-ttu-id="7c986-111">BizTalk Server 2016</span><span class="sxs-lookup"><span data-stu-id="7c986-111">BizTalk Server 2016</span></span>
*   <span data-ttu-id="7c986-112">DB2</span><span class="sxs-lookup"><span data-stu-id="7c986-112">DB2</span></span>  
*   <span data-ttu-id="7c986-113">Filsystem</span><span class="sxs-lookup"><span data-stu-id="7c986-113">File System</span></span>
*   <span data-ttu-id="7c986-114">Informix</span><span class="sxs-lookup"><span data-stu-id="7c986-114">Informix</span></span>
*   <span data-ttu-id="7c986-115">MQ</span><span class="sxs-lookup"><span data-stu-id="7c986-115">MQ</span></span>
*   <span data-ttu-id="7c986-116">MySQL</span><span class="sxs-lookup"><span data-stu-id="7c986-116">MySQL</span></span>
*   <span data-ttu-id="7c986-117">Oracle-databas</span><span class="sxs-lookup"><span data-stu-id="7c986-117">Oracle Database</span></span>
*   <span data-ttu-id="7c986-118">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="7c986-118">PostgreSQL</span></span>
*   <span data-ttu-id="7c986-119">SAP-programserver</span><span class="sxs-lookup"><span data-stu-id="7c986-119">SAP Application Server</span></span> 
*   <span data-ttu-id="7c986-120">SAP Message Server</span><span class="sxs-lookup"><span data-stu-id="7c986-120">SAP Message Server</span></span>
*   <span data-ttu-id="7c986-121">SharePoint</span><span class="sxs-lookup"><span data-stu-id="7c986-121">SharePoint</span></span>
*   <span data-ttu-id="7c986-122">SQL Server</span><span class="sxs-lookup"><span data-stu-id="7c986-122">SQL Server</span></span>
*   <span data-ttu-id="7c986-123">Teradata</span><span class="sxs-lookup"><span data-stu-id="7c986-123">Teradata</span></span>

<span data-ttu-id="7c986-124">Dessa steg visar hur tooset in hello lokala data gateway toowork med logic apps.</span><span class="sxs-lookup"><span data-stu-id="7c986-124">These steps show how tooset up hello on-premises data gateway toowork with your logic apps.</span></span> <span data-ttu-id="7c986-125">Läs mer om kopplingar som stöds, [kopplingar för Azure Logikappar](../connectors/apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="7c986-125">For more information about supported connectors, see [Connectors for Azure Logic Apps](../connectors/apis-list.md).</span></span> 

<span data-ttu-id="7c986-126">Information om hur toouse hello gateway med andra tjänster finns i följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="7c986-126">For information about how toouse hello gateway with other services, see these articles:</span></span>

*   [<span data-ttu-id="7c986-127">Microsoft Power BI lokala datagateway</span><span class="sxs-lookup"><span data-stu-id="7c986-127">Microsoft Power BI on-premises data gateway</span></span>](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem/)
*   [<span data-ttu-id="7c986-128">Azure Analysis Services lokala datagateway</span><span class="sxs-lookup"><span data-stu-id="7c986-128">Azure Analysis Services on-premises data gateway</span></span>](../analysis-services/analysis-services-gateway.md)
*   [<span data-ttu-id="7c986-129">Microsoft Flow lokala datagateway</span><span class="sxs-lookup"><span data-stu-id="7c986-129">Microsoft Flow on-premises data gateway</span></span>](https://flow.microsoft.com/documentation/gateway-manage/)
*   [<span data-ttu-id="7c986-130">Microsoft PowerApps lokala datagateway</span><span class="sxs-lookup"><span data-stu-id="7c986-130">Microsoft PowerApps on-premises data gateway</span></span>](https://powerapps.microsoft.com/tutorials/gateway-management/)

## <a name="requirements"></a><span data-ttu-id="7c986-131">Krav</span><span class="sxs-lookup"><span data-stu-id="7c986-131">Requirements</span></span>

* <span data-ttu-id="7c986-132">Du måste redan ha [installerat hello datagateway på en lokal dator](logic-apps-gateway-install.md).</span><span class="sxs-lookup"><span data-stu-id="7c986-132">You must have already [installed hello data gateway on a local computer](logic-apps-gateway-install.md).</span></span>

* <span data-ttu-id="7c986-133">När du loggar in toohello Azure-portalen måste du ha toouse hello samma arbets- eller skolkonto som användes för[installera hello lokala datagateway](logic-apps-gateway-install.md#requirements).</span><span class="sxs-lookup"><span data-stu-id="7c986-133">When you sign in toohello Azure portal, you have toouse hello same work or school account that was used too[install hello on-premises data gateway](logic-apps-gateway-install.md#requirements).</span></span> <span data-ttu-id="7c986-134">Din inloggning kontot måste också ha en Azure-prenumeration toouse när du skapar en gateway-resurs i hello Azure-portalen för gatewayinstallationen.</span><span class="sxs-lookup"><span data-stu-id="7c986-134">Your sign-in account must also have an Azure subscription toouse when you create a gateway resource in hello Azure portal for your gateway installation.</span></span>

* <span data-ttu-id="7c986-135">Gatewayinstallationen kan inte redan begärts av en Azure gateway-resurs.</span><span class="sxs-lookup"><span data-stu-id="7c986-135">Your gateway installation can't already be claimed by an Azure gateway resource.</span></span> <span data-ttu-id="7c986-136">Du kan associera din gateway installation tooonly en Azure gateway-resurs.</span><span class="sxs-lookup"><span data-stu-id="7c986-136">You can associate your gateway installation tooonly one Azure gateway resource.</span></span> <span data-ttu-id="7c986-137">Anspråk händer när du skapar hello gateway resurs så att hello installation är inte tillgängligt för andra resurser.</span><span class="sxs-lookup"><span data-stu-id="7c986-137">Claim happens when you create hello gateway resource so that hello installation is unavailable for other resources.</span></span>

## <a name="set-up-hello-data-gateway-connection"></a><span data-ttu-id="7c986-138">Ställ in hello data gateway-anslutningen</span><span class="sxs-lookup"><span data-stu-id="7c986-138">Set up hello data gateway connection</span></span>

### <a name="1-install-hello-on-premises-data-gateway"></a><span data-ttu-id="7c986-139">1. Installera hello lokala datagateway</span><span class="sxs-lookup"><span data-stu-id="7c986-139">1. Install hello on-premises data gateway</span></span>

<span data-ttu-id="7c986-140">Om du inte redan gjort följer hello [steg tooinstall hello lokala datagateway](logic-apps-gateway-install.md).</span><span class="sxs-lookup"><span data-stu-id="7c986-140">If you haven't already, follow hello [steps tooinstall hello on-premises data gateway](logic-apps-gateway-install.md).</span></span> <span data-ttu-id="7c986-141">Innan du fortsätter med hello andra steg, se till att du har installerat hello datagateway på en lokal dator.</span><span class="sxs-lookup"><span data-stu-id="7c986-141">Before you continue with hello other steps, make sure that you installed hello data gateway on a local computer.</span></span>

<a name="create-gateway-resource"></a>
### <a name="2-create-an-azure-resource-for-hello-on-premises-data-gateway"></a><span data-ttu-id="7c986-142">2. Skapa en Azure-resurs för hello lokala datagateway</span><span class="sxs-lookup"><span data-stu-id="7c986-142">2. Create an Azure resource for hello on-premises data gateway</span></span>

<span data-ttu-id="7c986-143">När du har installerat hello gateway på en lokal dator måste du skapa din datagateway som en resurs i Azure.</span><span class="sxs-lookup"><span data-stu-id="7c986-143">After you install hello gateway on a local computer, you must create your data gateway as a resource in Azure.</span></span> <span data-ttu-id="7c986-144">Det här steget associerar även din gateway-resurs med din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7c986-144">This step also associates your gateway resource with your Azure subscription.</span></span>

1. <span data-ttu-id="7c986-145">Logga in toohello [Azure-portalen](https://portal.azure.com "Azure-portalen").</span><span class="sxs-lookup"><span data-stu-id="7c986-145">Sign in toohello [Azure portal](https://portal.azure.com "Azure portal").</span></span> <span data-ttu-id="7c986-146">Kontrollera att toouse hello samma Azure arbete eller skola e-postadress används tooinstall hello gateway.</span><span class="sxs-lookup"><span data-stu-id="7c986-146">Make sure toouse hello same Azure work or school email address used tooinstall hello gateway.</span></span>

2. <span data-ttu-id="7c986-147">Hello vänstra menyn i Azure och välj **ny** > **Enterprise Integration** > **lokala datagateway** som visas här:</span><span class="sxs-lookup"><span data-stu-id="7c986-147">On hello left menu in Azure, choose **New** > **Enterprise Integration** > **On-premises data gateway** as shown here:</span></span>

   ![Sök efter ”lokala datagateway”](./media/logic-apps-gateway-connection/find-on-premises-data-gateway.png)

3. <span data-ttu-id="7c986-149">På hello **skapa anslutningen gateway** bladet och ange följande information toocreate din data gateway-resurs:</span><span class="sxs-lookup"><span data-stu-id="7c986-149">On hello **Create connection gateway** blade, provide these details toocreate your data gateway resource:</span></span>

    * <span data-ttu-id="7c986-150">**Namnet**: Ange ett namn för din gateway-resurs.</span><span class="sxs-lookup"><span data-stu-id="7c986-150">**Name**: Enter a name for your gateway resource.</span></span> 

    * <span data-ttu-id="7c986-151">**Prenumerationen**: Välj hello Azure-prenumeration tooassociate med gateway-resurs.</span><span class="sxs-lookup"><span data-stu-id="7c986-151">**Subscription**: Select hello Azure subscription tooassociate with your gateway resource.</span></span> 
    <span data-ttu-id="7c986-152">Den här prenumerationen ska vara hello samma prenumeration som din logikapp.</span><span class="sxs-lookup"><span data-stu-id="7c986-152">This subscription should be hello same subscription as your logic app.</span></span>
   
      <span data-ttu-id="7c986-153">Hej standardabonnemang baseras på hello Azure-konto som du använde toosign i.</span><span class="sxs-lookup"><span data-stu-id="7c986-153">hello default subscription is based on hello Azure account that you used toosign in.</span></span>

    * <span data-ttu-id="7c986-154">**Resursgruppen**: skapa en resursgrupp eller välj en befintlig resursgrupp för att distribuera din gateway-resurs.</span><span class="sxs-lookup"><span data-stu-id="7c986-154">**Resource group**: Create a resource group or select an existing resource group for deploying your gateway resource.</span></span> 
    <span data-ttu-id="7c986-155">Resursgrupper kan du hantera relaterade Azure-resurser som en samling.</span><span class="sxs-lookup"><span data-stu-id="7c986-155">Resource groups help you manage related Azure assets as a collection.</span></span>

    * <span data-ttu-id="7c986-156">**Plats**: Azure begränsar den här platsen toohello samma region som valts för hello gateway-Molntjänsten under [gatewayinstallationen](logic-apps-gateway-install.md).</span><span class="sxs-lookup"><span data-stu-id="7c986-156">**Location**: Azure restricts this location toohello same region that was selected for hello gateway cloud service during [gateway installation](logic-apps-gateway-install.md).</span></span> 

      > [!NOTE]
      > <span data-ttu-id="7c986-157">Kontrollera att hello gateway resursplats matchar hello gateway cloud service plats.</span><span class="sxs-lookup"><span data-stu-id="7c986-157">Make sure that hello gateway resource location matches hello gateway cloud service location.</span></span> <span data-ttu-id="7c986-158">Annars kan gatewayinstallationen inte visas i hello installerade gateways listan för du tooselect i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="7c986-158">Otherwise, your gateway installation might not appear in hello installed gateways list for you tooselect in hello next step.</span></span>
      > 
      > <span data-ttu-id="7c986-159">Du kan använda olika regioner för din gateway-resurs och för din logikapp.</span><span class="sxs-lookup"><span data-stu-id="7c986-159">You can use different regions for your gateway resource and for your logic app.</span></span>

    * <span data-ttu-id="7c986-160">**Installationen namnet**: Välj hello-gateway som du tidigare har installerat om gatewayinstallationen av inte redan har markerats.</span><span class="sxs-lookup"><span data-stu-id="7c986-160">**Installation Name**: If your gateway installation isn't already selected, select hello gateway that you previously installed.</span></span> 

    <span data-ttu-id="7c986-161">tooadd hello gateway resurs tooyour Azure instrumentpanelen, Välj **PIN-kod toodashboard**.</span><span class="sxs-lookup"><span data-stu-id="7c986-161">tooadd hello gateway resource tooyour Azure dashboard, choose **Pin toodashboard**.</span></span> 
    <span data-ttu-id="7c986-162">När du är klar väljer **skapa**.</span><span class="sxs-lookup"><span data-stu-id="7c986-162">When you're done, choose **Create**.</span></span>

    <span data-ttu-id="7c986-163">Exempel:</span><span class="sxs-lookup"><span data-stu-id="7c986-163">For example:</span></span>

    ![Ange information toocreate lokala datagateway](./media/logic-apps-gateway-connection/createblade.png)

    <span data-ttu-id="7c986-165">toofind eller visa din datagateway när som helst hello Azure vänstra huvudmenyn gå för **fler tjänster** > **Enterprise Integration** > **lokala Data Gateways**.</span><span class="sxs-lookup"><span data-stu-id="7c986-165">toofind or view your data gateway at any time,  from hello main Azure left menu, go too  **More Services** > **Enterprise Integration** > **On-premises Data Gateways**.</span></span>

    ![Gå för ”fler tjänster”, ”Enterprise Integration”, ”lokala Data Gateways”](./media/logic-apps-gateway-connection/find-on-premises-data-gateway-enterprise-integration.png)

<a name="connect-logic-app-gateway"></a>
### <a name="3-connect-your-logic-app-toohello-on-premises-data-gateway"></a><span data-ttu-id="7c986-167">3. Ansluta logik app toohello lokala datagateway</span><span class="sxs-lookup"><span data-stu-id="7c986-167">3. Connect your logic app toohello on-premises data gateway</span></span>

<span data-ttu-id="7c986-168">Nu när du har skapat din data gateway-resurs och tillhörande din Azure-prenumeration med den här resursen, skapa en anslutning mellan logik appen och hello datagateway.</span><span class="sxs-lookup"><span data-stu-id="7c986-168">Now that you've created your data gateway resource and associated your Azure subscription with that resource, create a connection between your logic app and hello data gateway.</span></span>

> [!NOTE]
> <span data-ttu-id="7c986-169">Din plats för gateway-anslutningen måste finnas i hello samma region som din logikapp, men du kan använda en datagateway som finns i en annan region.</span><span class="sxs-lookup"><span data-stu-id="7c986-169">Your gateway connection location must exist in hello same region as your logic app, but you can use a data gateway that exists in a different region.</span></span>

1. <span data-ttu-id="7c986-170">Skapa eller öppna logikappen i logik App Designer i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7c986-170">In hello Azure portal, create or open your logic app in Logic App Designer.</span></span>

2. <span data-ttu-id="7c986-171">Lägg till en koppling som har stöd för lokala anslutningar, t.ex. SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7c986-171">Add a connector that supports on-premises connections, like SQL Server.</span></span>

3. <span data-ttu-id="7c986-172">Följande hello ordning som visas, Välj **Anslut via lokala datagateway**, ange ett unikt anslutningsnamn och hello nödvändig information och välj hello data gateway resurs som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="7c986-172">Following hello order shown, select **Connect via on-premises data gateway**, provide a unique connection name and hello required information, and select hello data gateway resource that you want toouse.</span></span> <span data-ttu-id="7c986-173">När du är klar väljer **skapa**.</span><span class="sxs-lookup"><span data-stu-id="7c986-173">When you're done, choose **Create**.</span></span>

   > [!TIP]
   > <span data-ttu-id="7c986-174">Ett unikt anslutningsnamn hjälper dig att identifiera anslutningen senare, speciellt när du skapar flera anslutningar.</span><span class="sxs-lookup"><span data-stu-id="7c986-174">A unique connection name helps you easily identify that connection later, especially when you create multiple connections.</span></span> <span data-ttu-id="7c986-175">Om tillämpligt, även innehålla hello kvalificerade domännamnet för ditt användarnamn.</span><span class="sxs-lookup"><span data-stu-id="7c986-175">If applicable, also include hello qualified domain for your username.</span></span> 

   ![Skapa anslutning mellan logik appen och data gateway](./media/logic-apps-gateway-connection/blankconnection.png)

<span data-ttu-id="7c986-177">Grattis, gateway-anslutningen är klar för din app toouse för logik.</span><span class="sxs-lookup"><span data-stu-id="7c986-177">Congratulations, your gateway connection is now ready for your logic app toouse.</span></span>

## <a name="edit-your-gateway-connection-settings"></a><span data-ttu-id="7c986-178">Redigera anslutningsinställningarna gateway</span><span class="sxs-lookup"><span data-stu-id="7c986-178">Edit your gateway connection settings</span></span>

<span data-ttu-id="7c986-179">När du har skapat en gateway-anslutningen för din logikapp, kanske du vill toolater update hello-inställningarna för den specifika anslutningen.</span><span class="sxs-lookup"><span data-stu-id="7c986-179">After you create a gateway connection for your logic app, you might want toolater update hello settings for that specific connection.</span></span>

1. <span data-ttu-id="7c986-180">toofind hello gateway-anslutningen:</span><span class="sxs-lookup"><span data-stu-id="7c986-180">toofind hello gateway connection:</span></span>

   * <span data-ttu-id="7c986-181">Hello logik app bladet under **utvecklingsverktyg**väljer **API anslutningar**.</span><span class="sxs-lookup"><span data-stu-id="7c986-181">On hello logic app blade, under **Development Tools**, select **API Connections**.</span></span> 
   
     <span data-ttu-id="7c986-182">Hej **API anslutningar** visar alla API-anslutningar som är associerade med din logikapp, inklusive gateway-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="7c986-182">hello **API Connections** pane shows all API connections associated with your logic app, including gateway connections.</span></span>

     ![Gå tooyour logikapp, Välj ”API-anslutningar](./media/logic-apps-gateway-connection/logic-app-find-api-connections.png)

   * <span data-ttu-id="7c986-184">Hello Azure vänstra huvudmenyn går för **fler tjänster** > **webb- och Mobile Services** > **API anslutningar** för alla API-anslutningar inklusive gateway-anslutningar som är associerade med din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7c986-184">Or, from hello main Azure left menu, go too **More Services** > **Web & Mobile Services** > **API Connections** for all API connections, including gateway connections, that are associated with your Azure subscription.</span></span> 

   * <span data-ttu-id="7c986-185">Hello Azure vänstra huvudmenyn går för**alla resurser** för alla API-anslutningar, inklusive gateway-anslutningar som är associerade med din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7c986-185">Or, on hello main Azure left menu, go too**All resources** for all API connections, including gateway connections, that are associated with your Azure subscription.</span></span>

2. <span data-ttu-id="7c986-186">Välj hello gateway-anslutningen som du vill att tooview eller redigera och välj **redigera API-anslutningen**.</span><span class="sxs-lookup"><span data-stu-id="7c986-186">Select hello gateway connection that you want tooview or edit, and choose **Edit API connection**.</span></span>

   > [!TIP]
   > <span data-ttu-id="7c986-187">Om uppdateringarna inte gälla försök [stoppa och starta om Windows hello gateway-tjänsten](./logic-apps-gateway-install.md#restart-gateway).</span><span class="sxs-lookup"><span data-stu-id="7c986-187">If your updates don't take effect, try [stopping and restarting hello gateway Windows service](./logic-apps-gateway-install.md#restart-gateway).</span></span>

<a name="change-delete-gateway-resource"></a>
## <a name="switch-or-delete-your-on-premises-data-gateway-resource"></a><span data-ttu-id="7c986-188">Växla eller ta bort dina lokala data gateway-resurs</span><span class="sxs-lookup"><span data-stu-id="7c986-188">Switch or delete your on-premises data gateway resource</span></span>

<span data-ttu-id="7c986-189">toocreate en annan gateway-resurs, associera din gateway med en annan resurs eller ta bort hello gateway resurs, kan du ta bort hello gatewayresursen utan att påverka hello gateway-installationen.</span><span class="sxs-lookup"><span data-stu-id="7c986-189">toocreate a different gateway resource, associate your gateway with a different resource, or remove hello gateway resource, you can delete hello gateway resource without affecting hello gateway installation.</span></span> 

1. <span data-ttu-id="7c986-190">Hello Azure vänstra huvudmenyn gå för**alla resurser**.</span><span class="sxs-lookup"><span data-stu-id="7c986-190">From hello main Azure left menu, go too**All resources**.</span></span> 
2. <span data-ttu-id="7c986-191">Hitta och välj din data gateway-resurs.</span><span class="sxs-lookup"><span data-stu-id="7c986-191">Find and select your data gateway resource.</span></span>
3. <span data-ttu-id="7c986-192">Välj **lokala Data Gateway**, och välj hello resurs verktygsfältet **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="7c986-192">Choose **On-premises Data Gateway**, and on hello resource toolbar, choose **Delete**.</span></span>

<a name="faq"></a>
## <a name="frequently-asked-questions"></a><span data-ttu-id="7c986-193">Vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="7c986-193">Frequently asked questions</span></span>

[!INCLUDE [existing-gateway-location-changed](../../includes/logic-apps-existing-gateway-location-changed.md)]

## <a name="next-steps"></a><span data-ttu-id="7c986-194">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7c986-194">Next steps</span></span>

* [<span data-ttu-id="7c986-195">Skydda dina Logic Apps</span><span class="sxs-lookup"><span data-stu-id="7c986-195">Secure your logic apps</span></span>](./logic-apps-securing-a-logic-app.md)
* [<span data-ttu-id="7c986-196">Vanliga exempel och scenarier för logic apps</span><span class="sxs-lookup"><span data-stu-id="7c986-196">Common examples and scenarios for logic apps</span></span>](./logic-apps-examples-and-scenarios.md)
