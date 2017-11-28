---
title: "Komma åt datakällor lokalt för Logikappar i Azure | Microsoft Docs"
description: "Konfigurera lokala datagateway så att du kan komma åt datakällor lokalt från logikappar"
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
ms.openlocfilehash: 24793b83ca284fe9510fe21bc2d13b0589209d36
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="access-data-sources-on-premises-from-logic-apps-with-the-on-premises-data-gateway"></a><span data-ttu-id="74fc8-104">Komma åt datakällor lokalt från logikappar med lokala datagateway</span><span class="sxs-lookup"><span data-stu-id="74fc8-104">Access data sources on premises from logic apps with the on-premises data gateway</span></span>

<span data-ttu-id="74fc8-105">Konfigurera en lokal datagateway logikappar kan använda med kopplingar som stöds för att komma åt datakällor lokalt från dina logic apps.</span><span class="sxs-lookup"><span data-stu-id="74fc8-105">To access data sources on premises from your logic apps, set up an on-premises data gateway that logic apps can use with supported connectors.</span></span> <span data-ttu-id="74fc8-106">Gatewayen fungerar som en brygga som ger snabb överföring och kryptering mellan datakällor lokalt och dina logic apps.</span><span class="sxs-lookup"><span data-stu-id="74fc8-106">The gateway acts as a bridge that provides quick data transfer and encryption between data sources on premises and your logic apps.</span></span> <span data-ttu-id="74fc8-107">Gatewayen som vidarebefordrar data från lokala datakällor på krypterade kanaler via Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="74fc8-107">The gateway relays data from on-premises sources on encrypted channels through the Azure Service Bus.</span></span> <span data-ttu-id="74fc8-108">Det kommer all trafik som säkra utgående trafik från gateway-agenten.</span><span class="sxs-lookup"><span data-stu-id="74fc8-108">All traffic originates as secure outbound traffic from the gateway agent.</span></span> <span data-ttu-id="74fc8-109">Lär dig mer om [hur datagateway fungerar](logic-apps-gateway-install.md#gateway-cloud-service).</span><span class="sxs-lookup"><span data-stu-id="74fc8-109">Learn more about [how the data gateway works](logic-apps-gateway-install.md#gateway-cloud-service).</span></span> 

<span data-ttu-id="74fc8-110">Gatewayen stöder anslutningar till dessa datakällor lokalt:</span><span class="sxs-lookup"><span data-stu-id="74fc8-110">The gateway supports connections to these data sources on premises:</span></span>

*   <span data-ttu-id="74fc8-111">BizTalk Server 2016</span><span class="sxs-lookup"><span data-stu-id="74fc8-111">BizTalk Server 2016</span></span>
*   <span data-ttu-id="74fc8-112">DB2</span><span class="sxs-lookup"><span data-stu-id="74fc8-112">DB2</span></span>  
*   <span data-ttu-id="74fc8-113">Filsystem</span><span class="sxs-lookup"><span data-stu-id="74fc8-113">File System</span></span>
*   <span data-ttu-id="74fc8-114">Informix</span><span class="sxs-lookup"><span data-stu-id="74fc8-114">Informix</span></span>
*   <span data-ttu-id="74fc8-115">MQ</span><span class="sxs-lookup"><span data-stu-id="74fc8-115">MQ</span></span>
*   <span data-ttu-id="74fc8-116">MySQL</span><span class="sxs-lookup"><span data-stu-id="74fc8-116">MySQL</span></span>
*   <span data-ttu-id="74fc8-117">Oracle-databas</span><span class="sxs-lookup"><span data-stu-id="74fc8-117">Oracle Database</span></span>
*   <span data-ttu-id="74fc8-118">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="74fc8-118">PostgreSQL</span></span>
*   <span data-ttu-id="74fc8-119">SAP-programserver</span><span class="sxs-lookup"><span data-stu-id="74fc8-119">SAP Application Server</span></span> 
*   <span data-ttu-id="74fc8-120">SAP Message Server</span><span class="sxs-lookup"><span data-stu-id="74fc8-120">SAP Message Server</span></span>
*   <span data-ttu-id="74fc8-121">SharePoint</span><span class="sxs-lookup"><span data-stu-id="74fc8-121">SharePoint</span></span>
*   <span data-ttu-id="74fc8-122">SQL Server</span><span class="sxs-lookup"><span data-stu-id="74fc8-122">SQL Server</span></span>
*   <span data-ttu-id="74fc8-123">Teradata</span><span class="sxs-lookup"><span data-stu-id="74fc8-123">Teradata</span></span>

<span data-ttu-id="74fc8-124">Dessa steg visar hur du ställer in lokala datagateway att fungera med dina logic apps.</span><span class="sxs-lookup"><span data-stu-id="74fc8-124">These steps show how to set up the on-premises data gateway to work with your logic apps.</span></span> <span data-ttu-id="74fc8-125">Läs mer om kopplingar som stöds, [kopplingar för Azure Logikappar](../connectors/apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="74fc8-125">For more information about supported connectors, see [Connectors for Azure Logic Apps](../connectors/apis-list.md).</span></span> 

<span data-ttu-id="74fc8-126">Information om hur du använder en gateway med andra tjänster finns i följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="74fc8-126">For information about how to use the gateway with other services, see these articles:</span></span>

*   [<span data-ttu-id="74fc8-127">Microsoft Power BI lokala datagateway</span><span class="sxs-lookup"><span data-stu-id="74fc8-127">Microsoft Power BI on-premises data gateway</span></span>](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem/)
*   [<span data-ttu-id="74fc8-128">Azure Analysis Services lokala datagateway</span><span class="sxs-lookup"><span data-stu-id="74fc8-128">Azure Analysis Services on-premises data gateway</span></span>](../analysis-services/analysis-services-gateway.md)
*   [<span data-ttu-id="74fc8-129">Microsoft Flow lokala datagateway</span><span class="sxs-lookup"><span data-stu-id="74fc8-129">Microsoft Flow on-premises data gateway</span></span>](https://flow.microsoft.com/documentation/gateway-manage/)
*   [<span data-ttu-id="74fc8-130">Microsoft PowerApps lokala datagateway</span><span class="sxs-lookup"><span data-stu-id="74fc8-130">Microsoft PowerApps on-premises data gateway</span></span>](https://powerapps.microsoft.com/tutorials/gateway-management/)

## <a name="requirements"></a><span data-ttu-id="74fc8-131">Krav</span><span class="sxs-lookup"><span data-stu-id="74fc8-131">Requirements</span></span>

* <span data-ttu-id="74fc8-132">Du måste redan ha [installerat datagateway på en lokal dator](logic-apps-gateway-install.md).</span><span class="sxs-lookup"><span data-stu-id="74fc8-132">You must have already [installed the data gateway on a local computer](logic-apps-gateway-install.md).</span></span>

* <span data-ttu-id="74fc8-133">När du loggar in på Azure-portalen måste du använda samma arbets- eller skolkonto som används för att [installera lokala datagateway](logic-apps-gateway-install.md#requirements).</span><span class="sxs-lookup"><span data-stu-id="74fc8-133">When you sign in to the Azure portal, you have to use the same work or school account that was used to [install the on-premises data gateway](logic-apps-gateway-install.md#requirements).</span></span> <span data-ttu-id="74fc8-134">Logga in kontot måste också ha en Azure-prenumeration som används när du skapar en gateway-resurs i Azure-portalen för gatewayinstallationen.</span><span class="sxs-lookup"><span data-stu-id="74fc8-134">Your sign-in account must also have an Azure subscription to use when you create a gateway resource in the Azure portal for your gateway installation.</span></span>

* <span data-ttu-id="74fc8-135">Gatewayinstallationen kan inte redan begärts av en Azure gateway-resurs.</span><span class="sxs-lookup"><span data-stu-id="74fc8-135">Your gateway installation can't already be claimed by an Azure gateway resource.</span></span> <span data-ttu-id="74fc8-136">Du kan associera din gateway-installation till en resurs i Azure gateway.</span><span class="sxs-lookup"><span data-stu-id="74fc8-136">You can associate your gateway installation to only one Azure gateway resource.</span></span> <span data-ttu-id="74fc8-137">Anspråk händer när du skapar gatewayresursen så att installationen är inte tillgängligt för andra resurser.</span><span class="sxs-lookup"><span data-stu-id="74fc8-137">Claim happens when you create the gateway resource so that the installation is unavailable for other resources.</span></span>

## <a name="set-up-the-data-gateway-connection"></a><span data-ttu-id="74fc8-138">Ställ in data gateway-anslutningen</span><span class="sxs-lookup"><span data-stu-id="74fc8-138">Set up the data gateway connection</span></span>

### <a name="1-install-the-on-premises-data-gateway"></a><span data-ttu-id="74fc8-139">1. Installera den lokala datagatewayen</span><span class="sxs-lookup"><span data-stu-id="74fc8-139">1. Install the on-premises data gateway</span></span>

<span data-ttu-id="74fc8-140">Så om du inte redan gjort det [steg för att installera den lokala datagatewayen](logic-apps-gateway-install.md).</span><span class="sxs-lookup"><span data-stu-id="74fc8-140">If you haven't already, follow the [steps to install the on-premises data gateway](logic-apps-gateway-install.md).</span></span> <span data-ttu-id="74fc8-141">Innan du fortsätter med andra steg, se till att du har installerat datagateway på en lokal dator.</span><span class="sxs-lookup"><span data-stu-id="74fc8-141">Before you continue with the other steps, make sure that you installed the data gateway on a local computer.</span></span>

<a name="create-gateway-resource"></a>
### <a name="2-create-an-azure-resource-for-the-on-premises-data-gateway"></a><span data-ttu-id="74fc8-142">2. Skapa en Azure-resurs för lokala datagateway</span><span class="sxs-lookup"><span data-stu-id="74fc8-142">2. Create an Azure resource for the on-premises data gateway</span></span>

<span data-ttu-id="74fc8-143">När du har installerat gatewayen på en lokal dator måste du skapa din datagateway som en resurs i Azure.</span><span class="sxs-lookup"><span data-stu-id="74fc8-143">After you install the gateway on a local computer, you must create your data gateway as a resource in Azure.</span></span> <span data-ttu-id="74fc8-144">Det här steget associerar även din gateway-resurs med din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="74fc8-144">This step also associates your gateway resource with your Azure subscription.</span></span>

1. <span data-ttu-id="74fc8-145">Logga in på [Azure Portal](https://portal.azure.com "Azure Portal").</span><span class="sxs-lookup"><span data-stu-id="74fc8-145">Sign in to the [Azure portal](https://portal.azure.com "Azure portal").</span></span> <span data-ttu-id="74fc8-146">Se till att använda samma Azure arbetet eller skolan e-postadress används för att installera gatewayen.</span><span class="sxs-lookup"><span data-stu-id="74fc8-146">Make sure to use the same Azure work or school email address used to install the gateway.</span></span>

2. <span data-ttu-id="74fc8-147">Välj på den vänstra menyn i Azure **ny** > **Enterprise Integration** > **lokala datagateway** som visas här:</span><span class="sxs-lookup"><span data-stu-id="74fc8-147">On the left menu in Azure, choose **New** > **Enterprise Integration** > **On-premises data gateway** as shown here:</span></span>

   ![Sök efter ”lokala datagateway”](./media/logic-apps-gateway-connection/find-on-premises-data-gateway.png)

3. <span data-ttu-id="74fc8-149">På den **skapa anslutningen gateway** bladet Tillhandahåll följande information för att skapa din data gateway-resurs:</span><span class="sxs-lookup"><span data-stu-id="74fc8-149">On the **Create connection gateway** blade, provide these details to create your data gateway resource:</span></span>

    * <span data-ttu-id="74fc8-150">**Namnet**: Ange ett namn för din gateway-resurs.</span><span class="sxs-lookup"><span data-stu-id="74fc8-150">**Name**: Enter a name for your gateway resource.</span></span> 

    * <span data-ttu-id="74fc8-151">**Prenumerationen**: Välj den Azure-prenumerationen ska associeras med din gateway-resurs.</span><span class="sxs-lookup"><span data-stu-id="74fc8-151">**Subscription**: Select the Azure subscription to associate with your gateway resource.</span></span> 
    <span data-ttu-id="74fc8-152">Den här prenumerationen måste vara samma prenumeration som din logikapp.</span><span class="sxs-lookup"><span data-stu-id="74fc8-152">This subscription should be the same subscription as your logic app.</span></span>
   
      <span data-ttu-id="74fc8-153">Standard-prenumerationen är baserad på det Azure-konto som du använde för att logga in.</span><span class="sxs-lookup"><span data-stu-id="74fc8-153">The default subscription is based on the Azure account that you used to sign in.</span></span>

    * <span data-ttu-id="74fc8-154">**Resursgruppen**: skapa en resursgrupp eller välj en befintlig resursgrupp för att distribuera din gateway-resurs.</span><span class="sxs-lookup"><span data-stu-id="74fc8-154">**Resource group**: Create a resource group or select an existing resource group for deploying your gateway resource.</span></span> 
    <span data-ttu-id="74fc8-155">Resursgrupper kan du hantera relaterade Azure-resurser som en samling.</span><span class="sxs-lookup"><span data-stu-id="74fc8-155">Resource groups help you manage related Azure assets as a collection.</span></span>

    * <span data-ttu-id="74fc8-156">**Plats**: Azure begränsar den här platsen till samma region som valts för gateway-Molntjänsten under [gatewayinstallationen](logic-apps-gateway-install.md).</span><span class="sxs-lookup"><span data-stu-id="74fc8-156">**Location**: Azure restricts this location to the same region that was selected for the gateway cloud service during [gateway installation](logic-apps-gateway-install.md).</span></span> 

      > [!NOTE]
      > <span data-ttu-id="74fc8-157">Kontrollera att gateway-resursplats matchar gateway cloud service-plats.</span><span class="sxs-lookup"><span data-stu-id="74fc8-157">Make sure that the gateway resource location matches the gateway cloud service location.</span></span> <span data-ttu-id="74fc8-158">Annars visas inte din gateway-installation i listan över installerade gateways som du kan välja i nästa steg.</span><span class="sxs-lookup"><span data-stu-id="74fc8-158">Otherwise, your gateway installation might not appear in the installed gateways list for you to select in the next step.</span></span>
      > 
      > <span data-ttu-id="74fc8-159">Du kan använda olika regioner för din gateway-resurs och för din logikapp.</span><span class="sxs-lookup"><span data-stu-id="74fc8-159">You can use different regions for your gateway resource and for your logic app.</span></span>

    * <span data-ttu-id="74fc8-160">**Installationen namnet**: Välj den gateway som du tidigare har installerat om gatewayinstallationen av inte redan har markerats.</span><span class="sxs-lookup"><span data-stu-id="74fc8-160">**Installation Name**: If your gateway installation isn't already selected, select the gateway that you previously installed.</span></span> 

    <span data-ttu-id="74fc8-161">Lägg till gateway-resurs i instrumentpanelen i Azure, Välj **fäst på instrumentpanelen**.</span><span class="sxs-lookup"><span data-stu-id="74fc8-161">To add the gateway resource to your Azure dashboard, choose **Pin to dashboard**.</span></span> 
    <span data-ttu-id="74fc8-162">När du är klar väljer **skapa**.</span><span class="sxs-lookup"><span data-stu-id="74fc8-162">When you're done, choose **Create**.</span></span>

    <span data-ttu-id="74fc8-163">Exempel:</span><span class="sxs-lookup"><span data-stu-id="74fc8-163">For example:</span></span>

    ![Ange information för att skapa din lokala datagateway](./media/logic-apps-gateway-connection/createblade.png)

    <span data-ttu-id="74fc8-165">Om du vill hitta eller visa din datagateway när som helst från Azure vänstra huvudmenyn, gå till **fler tjänster** > **Enterprise Integration** > **lokala Data Gateways**.</span><span class="sxs-lookup"><span data-stu-id="74fc8-165">To find or view your data gateway at any time,  from the main Azure left menu, go to  **More Services** > **Enterprise Integration** > **On-premises Data Gateways**.</span></span>

    ![Gå till ”fler tjänster”, ”Enterprise Integration”, ”lokala Data Gateways”](./media/logic-apps-gateway-connection/find-on-premises-data-gateway-enterprise-integration.png)

<a name="connect-logic-app-gateway"></a>
### <a name="3-connect-your-logic-app-to-the-on-premises-data-gateway"></a><span data-ttu-id="74fc8-167">3. Ansluta din logikapp till lokala datagateway</span><span class="sxs-lookup"><span data-stu-id="74fc8-167">3. Connect your logic app to the on-premises data gateway</span></span>

<span data-ttu-id="74fc8-168">Nu när du har skapat din data gateway-resurs och tillhörande din Azure-prenumeration med den här resursen, skapa en anslutning mellan din logikapp och datagateway.</span><span class="sxs-lookup"><span data-stu-id="74fc8-168">Now that you've created your data gateway resource and associated your Azure subscription with that resource, create a connection between your logic app and the data gateway.</span></span>

> [!NOTE]
> <span data-ttu-id="74fc8-169">Din plats för gateway-anslutningen måste finnas i samma region som din logikapp, men du kan använda en datagateway som finns i en annan region.</span><span class="sxs-lookup"><span data-stu-id="74fc8-169">Your gateway connection location must exist in the same region as your logic app, but you can use a data gateway that exists in a different region.</span></span>

1. <span data-ttu-id="74fc8-170">Skapa eller öppna logikappen i logik App Designer i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="74fc8-170">In the Azure portal, create or open your logic app in Logic App Designer.</span></span>

2. <span data-ttu-id="74fc8-171">Lägg till en koppling som har stöd för lokala anslutningar, t.ex. SQL Server.</span><span class="sxs-lookup"><span data-stu-id="74fc8-171">Add a connector that supports on-premises connections, like SQL Server.</span></span>

3. <span data-ttu-id="74fc8-172">Efter den ordning som visas, Välj **Anslut via lokala datagateway**, ange ett unikt anslutningsnamn och informationen som krävs och välj den data gateway-resurs som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="74fc8-172">Following the order shown, select **Connect via on-premises data gateway**, provide a unique connection name and the required information, and select the data gateway resource that you want to use.</span></span> <span data-ttu-id="74fc8-173">När du är klar väljer **skapa**.</span><span class="sxs-lookup"><span data-stu-id="74fc8-173">When you're done, choose **Create**.</span></span>

   > [!TIP]
   > <span data-ttu-id="74fc8-174">Ett unikt anslutningsnamn hjälper dig att identifiera anslutningen senare, speciellt när du skapar flera anslutningar.</span><span class="sxs-lookup"><span data-stu-id="74fc8-174">A unique connection name helps you easily identify that connection later, especially when you create multiple connections.</span></span> <span data-ttu-id="74fc8-175">Om tillämpligt, ange domännamn för ditt användarnamn.</span><span class="sxs-lookup"><span data-stu-id="74fc8-175">If applicable, also include the qualified domain for your username.</span></span> 

   ![Skapa anslutning mellan logik appen och data gateway](./media/logic-apps-gateway-connection/blankconnection.png)

<span data-ttu-id="74fc8-177">Grattis, gateway-anslutningen är klar för din logikapp ska användas.</span><span class="sxs-lookup"><span data-stu-id="74fc8-177">Congratulations, your gateway connection is now ready for your logic app to use.</span></span>

## <a name="edit-your-gateway-connection-settings"></a><span data-ttu-id="74fc8-178">Redigera anslutningsinställningarna gateway</span><span class="sxs-lookup"><span data-stu-id="74fc8-178">Edit your gateway connection settings</span></span>

<span data-ttu-id="74fc8-179">När du har skapat en gateway-anslutningen för din logikapp kanske du vill uppdatera inställningarna för den specifika anslutningen.</span><span class="sxs-lookup"><span data-stu-id="74fc8-179">After you create a gateway connection for your logic app, you might want to later update the settings for that specific connection.</span></span>

1. <span data-ttu-id="74fc8-180">Hitta gateway-anslutningen:</span><span class="sxs-lookup"><span data-stu-id="74fc8-180">To find the gateway connection:</span></span>

   * <span data-ttu-id="74fc8-181">På bladet logik under **utvecklingsverktyg**väljer **API anslutningar**.</span><span class="sxs-lookup"><span data-stu-id="74fc8-181">On the logic app blade, under **Development Tools**, select **API Connections**.</span></span> 
   
     <span data-ttu-id="74fc8-182">Den **API anslutningar** visar alla API-anslutningar som är associerade med din logikapp, inklusive gateway-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="74fc8-182">The **API Connections** pane shows all API connections associated with your logic app, including gateway connections.</span></span>

     ![Gå till din logikapp, Välj ”API-anslutningar](./media/logic-apps-gateway-connection/logic-app-find-api-connections.png)

   * <span data-ttu-id="74fc8-184">Från Azure vänstra huvudmenyn, går du till **fler tjänster** > **webb- och Mobile Services** > **API anslutningar** för alla API-anslutningar inklusive gateway-anslutningar som är associerade med din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="74fc8-184">Or, from the main Azure left menu, go to **More Services** > **Web & Mobile Services** > **API Connections** for all API connections, including gateway connections, that are associated with your Azure subscription.</span></span> 

   * <span data-ttu-id="74fc8-185">På Azure vänstra huvudmenyn, går du till **alla resurser** för alla API-anslutningar, inklusive gateway-anslutningar som är associerade med din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="74fc8-185">Or, on the main Azure left menu, go to **All resources** for all API connections, including gateway connections, that are associated with your Azure subscription.</span></span>

2. <span data-ttu-id="74fc8-186">Välj gateway-anslutningen som du vill visa eller redigera och välj **redigera API-anslutningen**.</span><span class="sxs-lookup"><span data-stu-id="74fc8-186">Select the gateway connection that you want to view or edit, and choose **Edit API connection**.</span></span>

   > [!TIP]
   > <span data-ttu-id="74fc8-187">Om uppdateringarna inte gälla försök [stoppa och starta om Windows-tjänst för gateway](./logic-apps-gateway-install.md#restart-gateway).</span><span class="sxs-lookup"><span data-stu-id="74fc8-187">If your updates don't take effect, try [stopping and restarting the gateway Windows service](./logic-apps-gateway-install.md#restart-gateway).</span></span>

<a name="change-delete-gateway-resource"></a>
## <a name="switch-or-delete-your-on-premises-data-gateway-resource"></a><span data-ttu-id="74fc8-188">Växla eller ta bort dina lokala data gateway-resurs</span><span class="sxs-lookup"><span data-stu-id="74fc8-188">Switch or delete your on-premises data gateway resource</span></span>

<span data-ttu-id="74fc8-189">För att skapa en annan gateway-resurs, associera din gateway med en annan resurs eller ta bort gateway-resurs, kan du ta bort gatewayresursen utan att påverka gateway-installationen.</span><span class="sxs-lookup"><span data-stu-id="74fc8-189">To create a different gateway resource, associate your gateway with a different resource, or remove the gateway resource, you can delete the gateway resource without affecting the gateway installation.</span></span> 

1. <span data-ttu-id="74fc8-190">Gå till den Azure vänstra huvudmenyn **alla resurser**.</span><span class="sxs-lookup"><span data-stu-id="74fc8-190">From the main Azure left menu, go to **All resources**.</span></span> 
2. <span data-ttu-id="74fc8-191">Hitta och välj din data gateway-resurs.</span><span class="sxs-lookup"><span data-stu-id="74fc8-191">Find and select your data gateway resource.</span></span>
3. <span data-ttu-id="74fc8-192">Välj **lokala Data Gateway**, och välj verktygsfältet resurs **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="74fc8-192">Choose **On-premises Data Gateway**, and on the resource toolbar, choose **Delete**.</span></span>

<a name="faq"></a>
## <a name="frequently-asked-questions"></a><span data-ttu-id="74fc8-193">Vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="74fc8-193">Frequently asked questions</span></span>

[!INCLUDE [existing-gateway-location-changed](../../includes/logic-apps-existing-gateway-location-changed.md)]

## <a name="next-steps"></a><span data-ttu-id="74fc8-194">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="74fc8-194">Next steps</span></span>

* [<span data-ttu-id="74fc8-195">Skydda dina Logic Apps</span><span class="sxs-lookup"><span data-stu-id="74fc8-195">Secure your logic apps</span></span>](./logic-apps-securing-a-logic-app.md)
* [<span data-ttu-id="74fc8-196">Vanliga exempel och scenarier för logic apps</span><span class="sxs-lookup"><span data-stu-id="74fc8-196">Common examples and scenarios for logic apps</span></span>](./logic-apps-examples-and-scenarios.md)
