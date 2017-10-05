---
title: Installera lokala datagateway - Azure Logic Apps | Microsoft Docs
description: "Innan du komma åt datakällor lokalt installera lokala datagateway för snabb överföring och kryptering mellan datakällor lokalt och logikappar"
keywords: "komma åt data på lokala, dataöverföring, kryptering och datakällor"
services: logic-apps
documentationcenter: 
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 47e3024e-88a0-4017-8484-8f392faec89d
ms.service: logic-apps
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/13/2017
ms.author: LADocs; dimazaid; estfan
ms.openlocfilehash: 34e68ae7d35019848b35c785a2715ec458dc6e73
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="install-the-on-premises-data-gateway-for-azure-logic-apps"></a><span data-ttu-id="1fa04-104">Installera den lokala datagatewayen för Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="1fa04-104">Install the on-premises data gateway for Azure Logic Apps</span></span>

<span data-ttu-id="1fa04-105">Innan dina logic apps kan komma åt datakällor lokalt, måste du installera och konfigurera den lokala datagatewayen.</span><span class="sxs-lookup"><span data-stu-id="1fa04-105">Before your logic apps can access data sources on premises, you must install and set up the on-premises data gateway.</span></span> <span data-ttu-id="1fa04-106">Gatewayen fungerar som en brygga som ger snabb överföring och kryptering mellan lokala system och dina logic apps.</span><span class="sxs-lookup"><span data-stu-id="1fa04-106">The gateway acts as a bridge that provides quick data transfer and encryption between on-premises systems and your logic apps.</span></span> <span data-ttu-id="1fa04-107">Gatewayen som vidarebefordrar data från lokala datakällor på krypterade kanaler via Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="1fa04-107">The gateway relays data from on-premises sources on encrypted channels through the Azure Service Bus.</span></span> <span data-ttu-id="1fa04-108">Det kommer all trafik som säkra utgående trafik från gateway-agenten.</span><span class="sxs-lookup"><span data-stu-id="1fa04-108">All traffic originates as secure outbound traffic from the gateway agent.</span></span> <span data-ttu-id="1fa04-109">Lär dig mer om [hur datagateway fungerar](#gateway-cloud-service).</span><span class="sxs-lookup"><span data-stu-id="1fa04-109">Learn more about [how the data gateway works](#gateway-cloud-service).</span></span>

<span data-ttu-id="1fa04-110">Gatewayen stöder anslutningar till dessa datakällor lokalt:</span><span class="sxs-lookup"><span data-stu-id="1fa04-110">The gateway supports connections to these data sources on premises:</span></span>

*   <span data-ttu-id="1fa04-111">BizTalk Server 2016</span><span class="sxs-lookup"><span data-stu-id="1fa04-111">BizTalk Server 2016</span></span>
*   <span data-ttu-id="1fa04-112">DB2</span><span class="sxs-lookup"><span data-stu-id="1fa04-112">DB2</span></span>  
*   <span data-ttu-id="1fa04-113">Filsystem</span><span class="sxs-lookup"><span data-stu-id="1fa04-113">File System</span></span>
*   <span data-ttu-id="1fa04-114">Informix</span><span class="sxs-lookup"><span data-stu-id="1fa04-114">Informix</span></span>
*   <span data-ttu-id="1fa04-115">MQ</span><span class="sxs-lookup"><span data-stu-id="1fa04-115">MQ</span></span>
*   <span data-ttu-id="1fa04-116">MySQL</span><span class="sxs-lookup"><span data-stu-id="1fa04-116">MySQL</span></span>
*   <span data-ttu-id="1fa04-117">Oracle-databas</span><span class="sxs-lookup"><span data-stu-id="1fa04-117">Oracle Database</span></span>
*   <span data-ttu-id="1fa04-118">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="1fa04-118">PostgreSQL</span></span>
*   <span data-ttu-id="1fa04-119">SAP-programserver</span><span class="sxs-lookup"><span data-stu-id="1fa04-119">SAP Application Server</span></span> 
*   <span data-ttu-id="1fa04-120">SAP Message Server</span><span class="sxs-lookup"><span data-stu-id="1fa04-120">SAP Message Server</span></span>
*   <span data-ttu-id="1fa04-121">SharePoint</span><span class="sxs-lookup"><span data-stu-id="1fa04-121">SharePoint</span></span>
*   <span data-ttu-id="1fa04-122">SQL Server</span><span class="sxs-lookup"><span data-stu-id="1fa04-122">SQL Server</span></span>
*   <span data-ttu-id="1fa04-123">Teradata</span><span class="sxs-lookup"><span data-stu-id="1fa04-123">Teradata</span></span>

<span data-ttu-id="1fa04-124">Dessa steg visar hur du först installera den lokala datagatewayen innan du [upprätta en anslutning mellan gateway och dina logic apps](./logic-apps-gateway-connection.md).</span><span class="sxs-lookup"><span data-stu-id="1fa04-124">These steps show how to first install the on-premises data gateway before you [set up a connection between the gateway and your logic apps](./logic-apps-gateway-connection.md).</span></span> <span data-ttu-id="1fa04-125">Läs mer om kopplingar som stöds, [kopplingar för Azure Logikappar](https://docs.microsoft.com/azure/connectors/apis-list).</span><span class="sxs-lookup"><span data-stu-id="1fa04-125">For more information about supported connectors, see [Connectors for Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list).</span></span> 

<span data-ttu-id="1fa04-126">Information om hur du använder en gateway med andra tjänster finns i följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="1fa04-126">For information about how to use the gateway with other services, see these articles:</span></span>

*   [<span data-ttu-id="1fa04-127">Microsoft Power BI lokala datagateway</span><span class="sxs-lookup"><span data-stu-id="1fa04-127">Microsoft Power BI on-premises data gateway</span></span>](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem/)
*   [<span data-ttu-id="1fa04-128">Azure Analysis Services lokala datagateway</span><span class="sxs-lookup"><span data-stu-id="1fa04-128">Azure Analysis Services on-premises data gateway</span></span>](../analysis-services/analysis-services-gateway.md)
*   [<span data-ttu-id="1fa04-129">Microsoft Flow lokala datagateway</span><span class="sxs-lookup"><span data-stu-id="1fa04-129">Microsoft Flow on-premises data gateway</span></span>](https://flow.microsoft.com/documentation/gateway-manage/)
*   [<span data-ttu-id="1fa04-130">Microsoft PowerApps lokala datagateway</span><span class="sxs-lookup"><span data-stu-id="1fa04-130">Microsoft PowerApps on-premises data gateway</span></span>](https://powerapps.microsoft.com/tutorials/gateway-management/)

<a name="requirements"></a>
## <a name="requirements"></a><span data-ttu-id="1fa04-131">Krav</span><span class="sxs-lookup"><span data-stu-id="1fa04-131">Requirements</span></span>

<span data-ttu-id="1fa04-132">**Minsta**:</span><span class="sxs-lookup"><span data-stu-id="1fa04-132">**Minimum**:</span></span>

* <span data-ttu-id="1fa04-133">4.5 för .NET framework</span><span class="sxs-lookup"><span data-stu-id="1fa04-133">.NET 4.5 Framework</span></span>
* <span data-ttu-id="1fa04-134">64-bitars version av Windows 7 eller Windows Server 2008 R2 (eller senare)</span><span class="sxs-lookup"><span data-stu-id="1fa04-134">64-bit version of Windows 7 or Windows Server 2008 R2 (or later)</span></span>

<span data-ttu-id="1fa04-135">**Rekommenderade**:</span><span class="sxs-lookup"><span data-stu-id="1fa04-135">**Recommended**:</span></span>

* <span data-ttu-id="1fa04-136">8 kärnor CPU</span><span class="sxs-lookup"><span data-stu-id="1fa04-136">8 Core CPU</span></span>
* <span data-ttu-id="1fa04-137">8 GB minne</span><span class="sxs-lookup"><span data-stu-id="1fa04-137">8 GB Memory</span></span>
* <span data-ttu-id="1fa04-138">64-bitarsversionen av Windows 2012 R2 (eller senare)</span><span class="sxs-lookup"><span data-stu-id="1fa04-138">64-bit version of Windows 2012 R2 (or later)</span></span>

<span data-ttu-id="1fa04-139">**Tänk på följande**:</span><span class="sxs-lookup"><span data-stu-id="1fa04-139">**Important considerations**:</span></span>

* <span data-ttu-id="1fa04-140">Installera datagateway lokalt på en lokal dator.</span><span class="sxs-lookup"><span data-stu-id="1fa04-140">Install the on-premises data gateway only on a local computer.</span></span>
<span data-ttu-id="1fa04-141">Du kan inte installera gatewayen på en domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="1fa04-141">You can't install the gateway on a domain controller.</span></span>

   > [!TIP]
   > <span data-ttu-id="1fa04-142">Du behöver installera gatewayen på samma dator som datakällan.</span><span class="sxs-lookup"><span data-stu-id="1fa04-142">You don't have to install the gateway on the same computer as your data source.</span></span> <span data-ttu-id="1fa04-143">Du kan installera gatewayen så nära som möjligt till datakällan eller på samma dator, förutsatt att du har behörighet för att minimera svarstider.</span><span class="sxs-lookup"><span data-stu-id="1fa04-143">To minimize latency, you can install the gateway as close as possible to your data source, or on the same computer, assuming that you have permissions.</span></span>

* <span data-ttu-id="1fa04-144">Installera inte gatewayen på en dator som stängs av, försätts i strömsparläge, eller inte ansluta till Internet eftersom gatewayen inte kan köras under dessa omständigheter.</span><span class="sxs-lookup"><span data-stu-id="1fa04-144">Don't install the gateway on a computer that turns off, goes to sleep, or doesn't connect to the Internet because the gateway can't run under those circumstances.</span></span> <span data-ttu-id="1fa04-145">Gateway-prestanda kan dessutom lidande över ett nätverk.</span><span class="sxs-lookup"><span data-stu-id="1fa04-145">Also, gateway performance might suffer over a wireless network.</span></span>

* <span data-ttu-id="1fa04-146">Under installationen måste du logga in med en [arbets- eller skolkonto](https://docs.microsoft.com/azure/active-directory/sign-up-organization) som hanteras av Azure Active Directory (Azure AD), inte ett Microsoft-konto.</span><span class="sxs-lookup"><span data-stu-id="1fa04-146">During installation, you must sign in with a [work or school account](https://docs.microsoft.com/azure/active-directory/sign-up-organization) that's managed by Azure Active Directory (Azure AD), not a Microsoft account.</span></span> 

  <span data-ttu-id="1fa04-147">Du måste använda samma arbets- eller skolkonto senare i Azure-portalen när du skapar och associerar en gateway-resurs med gatewayinstallationen.</span><span class="sxs-lookup"><span data-stu-id="1fa04-147">You have to use the same work or school account later in the Azure portal when you create and associate a gateway resource with your gateway installation.</span></span> <span data-ttu-id="1fa04-148">Du kan sedan välja den här gatewayresursen när du skapar anslutningen mellan din logikapp och lokala datakällan.</span><span class="sxs-lookup"><span data-stu-id="1fa04-148">You then select this gateway resource when you create the connection between your logic app and the on-premises data source.</span></span> [<span data-ttu-id="1fa04-149">Varför måste använda en Azure AD-arbets eller skolkonto?</span><span class="sxs-lookup"><span data-stu-id="1fa04-149">Why must I use an Azure AD work or school account?</span></span>](#why-azure-work-school-account)

  > [!TIP]
  > <span data-ttu-id="1fa04-150">Om du har registrerat dig för ett erbjudande för Office 365 och gick inte att få din faktiska arbetet i e-post, din adress för inloggningen kan se ut som jeff@contoso.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="1fa04-150">If you signed up for an Office 365 offering and didn't supply your actual work email, your sign-in address might look like jeff@contoso.onmicrosoft.com.</span></span> 

* <span data-ttu-id="1fa04-151">Om du har en befintlig gateway som konfigurerats med ett installationsprogram som är äldre än version 14.16.6317.4 kan ändra du inte plats för din gateway genom att köra den senaste versionen installatören.</span><span class="sxs-lookup"><span data-stu-id="1fa04-151">If you have an existing gateway that you set up with an installer that's earlier than version 14.16.6317.4, you can't change your gateway's location by running the latest installer.</span></span> <span data-ttu-id="1fa04-152">Du kan dock använda senaste installationsprogrammet för att ställa in en ny gateway med den plats som du vill använda i stället.</span><span class="sxs-lookup"><span data-stu-id="1fa04-152">However, you can use the latest installer to set up a new gateway with the location that you want instead.</span></span>
  
  <span data-ttu-id="1fa04-153">Om du har en gateway-installationsprogram som är äldre än version 14.16.6317.4, men du inte har installerat din gateway ännu, kan du hämtar och använder den senaste versionen installatören.</span><span class="sxs-lookup"><span data-stu-id="1fa04-153">If you have a gateway installer that's earlier than version 14.16.6317.4, but you haven't installed your gateway yet, you can download and use the latest installer.</span></span>

<a name="install-gateway"></a>

## <a name="install-the-data-gateway"></a><span data-ttu-id="1fa04-154">Installera datagateway</span><span class="sxs-lookup"><span data-stu-id="1fa04-154">Install the data gateway</span></span>

1.  <span data-ttu-id="1fa04-155">[Hämta och kör installationsprogrammet för gateway på en lokal dator](http://go.microsoft.com/fwlink/?LinkID=820931&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="1fa04-155">[Download and run the gateway installer on a local computer](http://go.microsoft.com/fwlink/?LinkID=820931&clcid=0x409).</span></span>

2. <span data-ttu-id="1fa04-156">Granska och Godkänn användningsvillkoren och sekretesspolicyn.</span><span class="sxs-lookup"><span data-stu-id="1fa04-156">Review and accept the terms of use and privacy statement.</span></span>

3. <span data-ttu-id="1fa04-157">Ange sökvägen på den lokala datorn där du vill installera gatewayen.</span><span class="sxs-lookup"><span data-stu-id="1fa04-157">Specify the path on your local computer where you want to install the gateway.</span></span>

4. <span data-ttu-id="1fa04-158">När du uppmanas logga in med ditt Azure arbets- eller skolkonto inte ett Microsoft-konto.</span><span class="sxs-lookup"><span data-stu-id="1fa04-158">When prompted, sign in with your Azure work or school account, not a Microsoft account.</span></span>

   ![Logga in med Azure arbets- eller skolkonto](./media/logic-apps-gateway-install/sign-in-gateway-install.png)

5. <span data-ttu-id="1fa04-160">Nu registrera din installerade gateway med den [gateway-Molntjänsten](#gateway-cloud-service).</span><span class="sxs-lookup"><span data-stu-id="1fa04-160">Now register your installed gateway with the [gateway cloud service](#gateway-cloud-service).</span></span> <span data-ttu-id="1fa04-161">Välj **registrera en ny gateway på den här datorn**.</span><span class="sxs-lookup"><span data-stu-id="1fa04-161">Choose **Register a new gateway on this computer**.</span></span>

   <span data-ttu-id="1fa04-162">Gateway-Molntjänsten krypterar och lagrar dina autentiseringsuppgifter för datakälla och gateway-information.</span><span class="sxs-lookup"><span data-stu-id="1fa04-162">The gateway cloud service encrypts and stores your data source credentials and gateway details.</span></span> 
   <span data-ttu-id="1fa04-163">Tjänsten skickar också frågor och deras resultat mellan din logikapp, lokala datagateway och datakällan lokalt.</span><span class="sxs-lookup"><span data-stu-id="1fa04-163">The service also routes queries and their results between your logic app, the on-premises data gateway, and your data source on premises.</span></span>

6. <span data-ttu-id="1fa04-164">Ange ett namn för din gateway-installation.</span><span class="sxs-lookup"><span data-stu-id="1fa04-164">Provide a name for your gateway installation.</span></span> <span data-ttu-id="1fa04-165">Skapa en återställningsnyckel och sedan bekräfta din återställningsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="1fa04-165">Create a recovery key, then confirm your recovery key.</span></span> 

   > [!IMPORTANT] 
   > <span data-ttu-id="1fa04-166">Återställningsnyckeln måste innehålla minst åtta tecken.</span><span class="sxs-lookup"><span data-stu-id="1fa04-166">Your recovery key must contain at least eight characters.</span></span> <span data-ttu-id="1fa04-167">Kontrollera att du sparar och hålla nyckeln på en säker plats.</span><span class="sxs-lookup"><span data-stu-id="1fa04-167">Make sure that you save and keep the key in a safe place.</span></span> <span data-ttu-id="1fa04-168">Du måste också den här nyckeln om du vill migrera, Återställ eller ta över en befintlig gateway.</span><span class="sxs-lookup"><span data-stu-id="1fa04-168">You also need this key when you want to migrate, restore, or take over an existing gateway.</span></span>

   1. <span data-ttu-id="1fa04-169">Om du vill ändra standardregion för gateway-Molntjänsten och Azure Service Bus som används av gatewayinstallationen, Välj **ändra Region**.</span><span class="sxs-lookup"><span data-stu-id="1fa04-169">To change the default region for the gateway cloud service and Azure Service Bus used by your gateway installation, choose **Change Region**.</span></span>

      ![Ändra region](./media/logic-apps-gateway-install/change-region-gateway-install.png)

      <span data-ttu-id="1fa04-171">Standardregionen är den region som är associerade med Azure AD-klienten.</span><span class="sxs-lookup"><span data-stu-id="1fa04-171">The default region is the region associated with your Azure AD tenant.</span></span>

   2. <span data-ttu-id="1fa04-172">I nästa steg, öppna den **väljer Region** att välja en annan region.</span><span class="sxs-lookup"><span data-stu-id="1fa04-172">On the next pane, open the **Select Region** to choose a different region.</span></span>

      ![Välj en annan region](./media/logic-apps-gateway-install/select-region-gateway-install.png)

      <span data-ttu-id="1fa04-174">Du kan till exempel välja samma region som din logikapp eller väljer du regionen som är närmast datakällan lokalt så att du kan minska svarstiden.</span><span class="sxs-lookup"><span data-stu-id="1fa04-174">For example, you might select the same region as your logic app, or select the region closest to your on-premises data source so you can reduce latency.</span></span> <span data-ttu-id="1fa04-175">Gateway-resurs och logik appen kan ha olika platser.</span><span class="sxs-lookup"><span data-stu-id="1fa04-175">Your gateway resource and logic app can have different locations.</span></span>

      > [!IMPORTANT]
      > <span data-ttu-id="1fa04-176">Du kan inte ändra den här regionen efter installationen.</span><span class="sxs-lookup"><span data-stu-id="1fa04-176">You can't change this region after installation.</span></span> <span data-ttu-id="1fa04-177">Den här regionen avgör också och begränsar den plats där du kan skapa Azure-resurs för din gateway.</span><span class="sxs-lookup"><span data-stu-id="1fa04-177">This region also determines and restricts the location where you can create the Azure resource for your gateway.</span></span> <span data-ttu-id="1fa04-178">Så när du skapar din gateway-resurs i Azure, se till att platsen för resursen matchar den region som du valde under gatewayinstallationen.</span><span class="sxs-lookup"><span data-stu-id="1fa04-178">So when you create your gateway resource in Azure, make sure that the resource location matches the region that you selected during gateway installation.</span></span>
      > 
      > <span data-ttu-id="1fa04-179">Om du vill använda en annan region för din gateway senare måste du ställa in en ny gateway.</span><span class="sxs-lookup"><span data-stu-id="1fa04-179">If you want to use a different region for your gateway later, you must set up a new gateway.</span></span>

   3. <span data-ttu-id="1fa04-180">När du är klar kan du välja **klar**.</span><span class="sxs-lookup"><span data-stu-id="1fa04-180">When you're ready, choose **Done**.</span></span>

7. <span data-ttu-id="1fa04-181">Nu följer du stegen i Azure-portalen så att du kan [skapa en Azure-resurs för din gateway](../logic-apps/logic-apps-gateway-connection.md).</span><span class="sxs-lookup"><span data-stu-id="1fa04-181">Now follow these steps in the Azure portal so you can [create an Azure resource for your gateway](../logic-apps/logic-apps-gateway-connection.md).</span></span> 

<span data-ttu-id="1fa04-182">Lär dig mer om [hur datagateway fungerar](#gateway-cloud-service).</span><span class="sxs-lookup"><span data-stu-id="1fa04-182">Learn more about [how the data gateway works](#gateway-cloud-service).</span></span>

## <a name="migrate-restore-or-take-over-an-existing-gateway"></a><span data-ttu-id="1fa04-183">Migrera, Återställ eller ta över en befintlig gateway</span><span class="sxs-lookup"><span data-stu-id="1fa04-183">Migrate, restore, or take over an existing gateway</span></span>

<span data-ttu-id="1fa04-184">Om du vill utföra dessa uppgifter måste du ha återställningsnyckeln som angavs när gatewayen har installerats.</span><span class="sxs-lookup"><span data-stu-id="1fa04-184">To perform these tasks, you must have the recovery key that was specified when the gateway was installed.</span></span>

1. <span data-ttu-id="1fa04-185">Datorns Start-menyn, Välj **lokala datagateway**.</span><span class="sxs-lookup"><span data-stu-id="1fa04-185">From your computer's Start menu, choose **On-premises data gateway**.</span></span>

2. <span data-ttu-id="1fa04-186">När installationsprogrammet har öppnats, logga in med samma Azure arbets- eller skolkonto som tidigare användes för att installera gatewayen.</span><span class="sxs-lookup"><span data-stu-id="1fa04-186">After the installer opens, sign in with the same Azure work or school account that was previously used to install the gateway.</span></span>

3. <span data-ttu-id="1fa04-187">Välj **migrera, Återställ eller ta över en befintlig gateway**.</span><span class="sxs-lookup"><span data-stu-id="1fa04-187">Choose **Migrate, restore, or take over an existing gateway**.</span></span>

4. <span data-ttu-id="1fa04-188">Ange namn och återställa nyckeln för den gateway som du vill migrera, Återställ eller ta över.</span><span class="sxs-lookup"><span data-stu-id="1fa04-188">Provide the name and recovery key for the gateway that you want to migrate, restore, or take over.</span></span>

<a name="restart-gateway"></a>
## <a name="restart-the-gateway"></a><span data-ttu-id="1fa04-189">Startar du om gatewayen</span><span class="sxs-lookup"><span data-stu-id="1fa04-189">Restart the gateway</span></span>

<span data-ttu-id="1fa04-190">Gatewayen körs som en Windows-tjänst.</span><span class="sxs-lookup"><span data-stu-id="1fa04-190">The gateway runs as a Windows service.</span></span> <span data-ttu-id="1fa04-191">Precis som alla andra Windows-tjänster, kan du starta och stoppa tjänsten på flera olika sätt.</span><span class="sxs-lookup"><span data-stu-id="1fa04-191">Like any other Windows service, you can start and stop the service in multiple ways.</span></span> <span data-ttu-id="1fa04-192">Du kan till exempel öppna en kommandotolk med förhöjd behörighet på den dator där gatewayen körs och kör antingen dessa kommandon:</span><span class="sxs-lookup"><span data-stu-id="1fa04-192">For example, you can open a command prompt with elevated permissions on the computer where the gateway is running, and run either these commands:</span></span>

* <span data-ttu-id="1fa04-193">Kör det här kommandot för att stoppa tjänsten:</span><span class="sxs-lookup"><span data-stu-id="1fa04-193">To stop the service, run this command:</span></span>
  
    `net stop PBIEgwService`

* <span data-ttu-id="1fa04-194">Om du vill starta tjänsten, kör du kommandot:</span><span class="sxs-lookup"><span data-stu-id="1fa04-194">To start the service, run this command:</span></span>
  
    `net start PBIEgwService`

### <a name="windows-service-account"></a><span data-ttu-id="1fa04-195">Windows-tjänstkontot</span><span class="sxs-lookup"><span data-stu-id="1fa04-195">Windows service account</span></span>

<span data-ttu-id="1fa04-196">Lokala datagateway har konfigurerats att använda `NT SERVICE\PBIEgwService` tjänsten inloggningsuppgifter för Windows.</span><span class="sxs-lookup"><span data-stu-id="1fa04-196">The on-premises data gateway is set up to use `NT SERVICE\PBIEgwService` for the Windows service logon credentials.</span></span> <span data-ttu-id="1fa04-197">Gatewayen har som standard rättigheten ”logga in som en tjänst” på datorn där du installerar en gateway.</span><span class="sxs-lookup"><span data-stu-id="1fa04-197">By default, the gateway has the "Log on as a service" right for the machine where you install the gateway.</span></span>

> [!NOTE]
> <span data-ttu-id="1fa04-198">Windows-tjänstkontot skiljer sig från det konto som används för att ansluta till lokala data källor, och från Azure arbets- eller skolkonto som används för att logga in till molntjänster.</span><span class="sxs-lookup"><span data-stu-id="1fa04-198">The Windows service account differs from the account used for connecting to on-premises data sources, and from the Azure work or school account used to sign in to cloud services.</span></span>

## <a name="configure-a-firewall-or-proxy"></a><span data-ttu-id="1fa04-199">Konfigurera en brandvägg eller proxyserver</span><span class="sxs-lookup"><span data-stu-id="1fa04-199">Configure a firewall or proxy</span></span>

<span data-ttu-id="1fa04-200">Gatewayen skapar en utgående anslutning till [Azure Service Bus](https://azure.microsoft.com/services/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="1fa04-200">The gateway creates an outbound connection to [Azure Service Bus](https://azure.microsoft.com/services/service-bus/).</span></span> <span data-ttu-id="1fa04-201">För att ge information om proxy för din gateway, se [konfigurera proxyinställningar](https://powerbi.microsoft.com/documentation/powerbi-gateway-proxy/).</span><span class="sxs-lookup"><span data-stu-id="1fa04-201">To provide proxy information for your gateway, see [Configure proxy settings](https://powerbi.microsoft.com/documentation/powerbi-gateway-proxy/).</span></span>

<span data-ttu-id="1fa04-202">Om du vill kontrollera om din brandvägg eller proxyserver, kan blockera anslutningar, bekräfta om datorn faktiskt kan ansluta till internet och [Azure Service Bus](https://azure.microsoft.com/services/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="1fa04-202">To check whether your firewall, or proxy, might block connections, confirm whether your machine can actually connect to the internet and the [Azure Service Bus](https://azure.microsoft.com/services/service-bus/).</span></span> <span data-ttu-id="1fa04-203">Kör kommandot från en PowerShell-kommandotolk:</span><span class="sxs-lookup"><span data-stu-id="1fa04-203">From a PowerShell prompt, run this command:</span></span>

`Test-NetConnection -ComputerName watchdog.servicebus.windows.net -Port 9350`

> [!NOTE]
> <span data-ttu-id="1fa04-204">Det här kommandot kan endast test nätverksanslutning och anslutningen till Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="1fa04-204">This command only tests network connectivity and connectivity to the Azure Service Bus.</span></span> <span data-ttu-id="1fa04-205">Så har kommandot ingenting att göra med en gateway eller gateway-Molntjänsten som krypterar och lagrar dina autentiseringsuppgifter och gateway-information.</span><span class="sxs-lookup"><span data-stu-id="1fa04-205">So the command doesn't have anything to do with the gateway or the gateway cloud service that encrypts and stores your credentials and gateway details.</span></span> 
>
> <span data-ttu-id="1fa04-206">Det här kommandot är också bara tillgängligt på Windows Server 2012 R2 eller senare och Windows 8.1 eller senare.</span><span class="sxs-lookup"><span data-stu-id="1fa04-206">Also, this command is only available on Windows Server 2012 R2 or later, and Windows 8.1 or later.</span></span> <span data-ttu-id="1fa04-207">Du kan använda Telnet tidigare OS-version för att testa anslutningen.</span><span class="sxs-lookup"><span data-stu-id="1fa04-207">On earlier OS versions, you can use Telnet to test connectivity.</span></span> <span data-ttu-id="1fa04-208">Lär dig mer om [Azure Service Bus och hybridlösningar](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="1fa04-208">Learn more about [Azure Service Bus and hybrid solutions](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).</span></span>

<span data-ttu-id="1fa04-209">Resultatet bör se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="1fa04-209">Your results should look similar to this example:</span></span>

```text
ComputerName           : watchdog.servicebus.windows.net
RemoteAddress          : 70.37.104.240
RemotePort             : 5672
InterfaceAlias         : vEthernet (Broadcom NetXtreme Gigabit Ethernet - Virtual Switch)
SourceAddress          : 10.120.60.105
PingSucceeded          : False
PingReplyDetails (RTT) : 0 ms
TcpTestSucceeded       : True
```

<span data-ttu-id="1fa04-210">Om **TcpTestSucceeded** inte har angetts till **SANT**, du kan blockeras av en brandvägg.</span><span class="sxs-lookup"><span data-stu-id="1fa04-210">If **TcpTestSucceeded** is not set to **True**, you might be blocked by a firewall.</span></span> <span data-ttu-id="1fa04-211">Om du vill ska vara omfattande ersätta den **ComputerName** och **Port** värden med de värden som anges [konfigurera portar](#configure-ports) i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="1fa04-211">If you want to be comprehensive, substitute the **ComputerName** and **Port** values with the values listed under [Configure ports](#configure-ports) in this topic.</span></span>

<span data-ttu-id="1fa04-212">Brandväggen kan också blockera anslutningar som Azure Service Bus gör att Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="1fa04-212">The firewall might also block connections that the Azure Service Bus makes to the Azure datacenters.</span></span> <span data-ttu-id="1fa04-213">Om det här scenariot inträffar kan godkänna (avblockera) alla IP-adresser för de datacenter i din region.</span><span class="sxs-lookup"><span data-stu-id="1fa04-213">If this scenario happens, approve (unblock) all the IP addresses for those datacenters in your region.</span></span> <span data-ttu-id="1fa04-214">För de IP-adresserna [hämta Azure IP-adresslistan här](https://www.microsoft.com/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="1fa04-214">For those IP addresses, [get the Azure IP addresses list here](https://www.microsoft.com/download/details.aspx?id=41653).</span></span>

## <a name="configure-ports"></a><span data-ttu-id="1fa04-215">Konfigurera portar</span><span class="sxs-lookup"><span data-stu-id="1fa04-215">Configure ports</span></span>

<span data-ttu-id="1fa04-216">Gatewayen skapar en utgående anslutning till [Azure Service Bus](https://azure.microsoft.com/services/service-bus/) och kommunicerar på utgående portar: TCP 443 (standard), 5671, 5672, 9350 via 9354.</span><span class="sxs-lookup"><span data-stu-id="1fa04-216">The gateway creates an outbound connection to [Azure Service Bus](https://azure.microsoft.com/services/service-bus/) and communicates on outbound ports: TCP 443 (default), 5671, 5672, 9350 through 9354.</span></span> <span data-ttu-id="1fa04-217">Gatewayen kräver inte ingående portar.</span><span class="sxs-lookup"><span data-stu-id="1fa04-217">The gateway doesn't require inbound ports.</span></span> <span data-ttu-id="1fa04-218">Lär dig mer om [Azure Service Bus och hybridlösningar](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="1fa04-218">Learn more about [Azure Service Bus and hybrid solutions](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).</span></span>

| <span data-ttu-id="1fa04-219">DOMÄNNAMN</span><span class="sxs-lookup"><span data-stu-id="1fa04-219">DOMAIN NAMES</span></span> | <span data-ttu-id="1fa04-220">UTGÅENDE PORTAR</span><span class="sxs-lookup"><span data-stu-id="1fa04-220">OUTBOUND PORTS</span></span> | <span data-ttu-id="1fa04-221">BESKRIVNING</span><span class="sxs-lookup"><span data-stu-id="1fa04-221">DESCRIPTION</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1fa04-222">*. analysis.windows.net</span><span class="sxs-lookup"><span data-stu-id="1fa04-222">*.analysis.windows.net</span></span> | <span data-ttu-id="1fa04-223">443</span><span class="sxs-lookup"><span data-stu-id="1fa04-223">443</span></span> | <span data-ttu-id="1fa04-224">HTTPS</span><span class="sxs-lookup"><span data-stu-id="1fa04-224">HTTPS</span></span> | 
| <span data-ttu-id="1fa04-225">*. login.windows.net</span><span class="sxs-lookup"><span data-stu-id="1fa04-225">*.login.windows.net</span></span> | <span data-ttu-id="1fa04-226">443</span><span class="sxs-lookup"><span data-stu-id="1fa04-226">443</span></span> | <span data-ttu-id="1fa04-227">HTTPS</span><span class="sxs-lookup"><span data-stu-id="1fa04-227">HTTPS</span></span> | 
| <span data-ttu-id="1fa04-228">*. servicebus.windows.net</span><span class="sxs-lookup"><span data-stu-id="1fa04-228">*.servicebus.windows.net</span></span> | <span data-ttu-id="1fa04-229">5671-5672</span><span class="sxs-lookup"><span data-stu-id="1fa04-229">5671-5672</span></span> | <span data-ttu-id="1fa04-230">Avancerade Message Queuing-protokollet (AMQP)</span><span class="sxs-lookup"><span data-stu-id="1fa04-230">Advanced Message Queuing Protocol (AMQP)</span></span> | 
| <span data-ttu-id="1fa04-231">*. servicebus.windows.net</span><span class="sxs-lookup"><span data-stu-id="1fa04-231">*.servicebus.windows.net</span></span> | <span data-ttu-id="1fa04-232">443, 9350-9354</span><span class="sxs-lookup"><span data-stu-id="1fa04-232">443, 9350-9354</span></span> | <span data-ttu-id="1fa04-233">Lyssnare på Service Bus Relay via TCP (kräver 443 för åtkomstkontroll token)</span><span class="sxs-lookup"><span data-stu-id="1fa04-233">Listeners on Service Bus Relay over TCP (requires 443 for Access Control token acquisition)</span></span> | 
| <span data-ttu-id="1fa04-234">*. frontend.clouddatahub.net</span><span class="sxs-lookup"><span data-stu-id="1fa04-234">*.frontend.clouddatahub.net</span></span> | <span data-ttu-id="1fa04-235">443</span><span class="sxs-lookup"><span data-stu-id="1fa04-235">443</span></span> | <span data-ttu-id="1fa04-236">HTTPS</span><span class="sxs-lookup"><span data-stu-id="1fa04-236">HTTPS</span></span> | 
| <span data-ttu-id="1fa04-237">*. core.windows.net</span><span class="sxs-lookup"><span data-stu-id="1fa04-237">*.core.windows.net</span></span> | <span data-ttu-id="1fa04-238">443</span><span class="sxs-lookup"><span data-stu-id="1fa04-238">443</span></span> | <span data-ttu-id="1fa04-239">HTTPS</span><span class="sxs-lookup"><span data-stu-id="1fa04-239">HTTPS</span></span> | 
| <span data-ttu-id="1fa04-240">login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="1fa04-240">login.microsoftonline.com</span></span> | <span data-ttu-id="1fa04-241">443</span><span class="sxs-lookup"><span data-stu-id="1fa04-241">443</span></span> | <span data-ttu-id="1fa04-242">HTTPS</span><span class="sxs-lookup"><span data-stu-id="1fa04-242">HTTPS</span></span> | 
| <span data-ttu-id="1fa04-243">*. msftncsi.com</span><span class="sxs-lookup"><span data-stu-id="1fa04-243">*.msftncsi.com</span></span> | <span data-ttu-id="1fa04-244">443</span><span class="sxs-lookup"><span data-stu-id="1fa04-244">443</span></span> | <span data-ttu-id="1fa04-245">Används för att testa internet-anslutning när gatewayen inte kan nås av Power BI-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="1fa04-245">Used to test internet connectivity when the gateway is unreachable by the Power BI service.</span></span> | 

<span data-ttu-id="1fa04-246">Om du behöver godkänna IP-adresser i stället för domänerna som du kan hämta och använda den [Microsoft Azure Datacenter IP-intervall lista](https://www.microsoft.com/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="1fa04-246">If you have to approve IP addresses instead of the domains, you can download and use the [Microsoft Azure Datacenter IP ranges list](https://www.microsoft.com/download/details.aspx?id=41653).</span></span> <span data-ttu-id="1fa04-247">I vissa fall kan görs Azure Service Bus-anslutningar med IP-adress i stället för fullständigt kvalificerade domännamn.</span><span class="sxs-lookup"><span data-stu-id="1fa04-247">In some cases, the Azure Service Bus connections are made with IP Address rather than fully qualified domain names.</span></span>

<a name="gateway-cloud-service"></a>
## <a name="how-does-the-data-gateway-work"></a><span data-ttu-id="1fa04-248">Hur fungerar datagateway?</span><span class="sxs-lookup"><span data-stu-id="1fa04-248">How does the data gateway work?</span></span>

<span data-ttu-id="1fa04-249">Datagatewayen underlättar snabb och säker kommunikation mellan din logikapp, gateway-Molntjänsten och lokala datakällan.</span><span class="sxs-lookup"><span data-stu-id="1fa04-249">The data gateway facilitates quick and secure communication between your logic app, the gateway cloud service, and your on-premises data source.</span></span> 

![diagram-for-on-premises-data-gateway-Flow](./media/logic-apps-gateway-install/how-on-premises-data-gateway-works-flow-diagram.png)

<span data-ttu-id="1fa04-251">När användaren i molnet som interagerar med ett element som är ansluten till din lokala datakälla:</span><span class="sxs-lookup"><span data-stu-id="1fa04-251">So when the user in the cloud interacts with an element that's connected to your on-premises data source:</span></span>

1. <span data-ttu-id="1fa04-252">Gateway-Molntjänsten skapar en fråga, tillsammans med krypterade autentiseringsuppgifterna för datakällan, och skickar frågan till kön för gatewayen att bearbeta.</span><span class="sxs-lookup"><span data-stu-id="1fa04-252">The gateway cloud service creates a query, along with the encrypted credentials for the data source, and sends the query to the queue for the gateway to process.</span></span>

2. <span data-ttu-id="1fa04-253">Gateway-Molntjänsten analyserar frågan och skickar begäran till Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="1fa04-253">The gateway cloud service analyzes the query and pushes the request to the Azure Service Bus.</span></span>

3. <span data-ttu-id="1fa04-254">Lokala datagateway avsöker Azure Service Bus för väntande begäranden.</span><span class="sxs-lookup"><span data-stu-id="1fa04-254">The on-premises data gateway polls the Azure Service Bus for pending requests.</span></span>

4. <span data-ttu-id="1fa04-255">Gatewayen hämtar frågan dekrypterar autentiseringsuppgifterna och ansluter till datakällan med autentiseringsuppgifterna.</span><span class="sxs-lookup"><span data-stu-id="1fa04-255">The gateway gets the query, decrypts the credentials, and connects to the data source with those credentials.</span></span>

5. <span data-ttu-id="1fa04-256">Gatewayen skickar frågan till datakällan för körning.</span><span class="sxs-lookup"><span data-stu-id="1fa04-256">The gateway sends the query to the data source for execution.</span></span>

6. <span data-ttu-id="1fa04-257">Resultatet skickas från datakällan, tillbaka till gatewayen och sedan till gateway-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="1fa04-257">The results are sent from the data source, back to the gateway, and then to the gateway cloud service.</span></span> <span data-ttu-id="1fa04-258">Gateway-Molntjänsten använder sedan resultaten.</span><span class="sxs-lookup"><span data-stu-id="1fa04-258">The gateway cloud service then uses the results.</span></span>

<a name="faq"></a>
## <a name="frequently-asked-questions"></a><span data-ttu-id="1fa04-259">Vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="1fa04-259">Frequently asked questions</span></span>

### <a name="general"></a><span data-ttu-id="1fa04-260">Allmänt</span><span class="sxs-lookup"><span data-stu-id="1fa04-260">General</span></span>

<span data-ttu-id="1fa04-261">**Q**: behöver jag en gateway för datakällor i molnet, till exempel SQL Azure?</span><span class="sxs-lookup"><span data-stu-id="1fa04-261">**Q**: Do I need a gateway for data sources in the cloud, such as SQL Azure?</span></span> <br/><span data-ttu-id="1fa04-262">
**En**: Nej</span><span class="sxs-lookup"><span data-stu-id="1fa04-262">
**A**: No.</span></span> <span data-ttu-id="1fa04-263">En gateway ansluter till endast lokala datakällor.</span><span class="sxs-lookup"><span data-stu-id="1fa04-263">A gateway connects to on-premises data sources only.</span></span>

<span data-ttu-id="1fa04-264">**Q**: har en gateway installeras på samma dator som datakällan?</span><span class="sxs-lookup"><span data-stu-id="1fa04-264">**Q**: Does the gateway have to be installed on the same machine as the data source?</span></span> <br/><span data-ttu-id="1fa04-265">
**En**: Nej</span><span class="sxs-lookup"><span data-stu-id="1fa04-265">
**A**: No.</span></span> <span data-ttu-id="1fa04-266">Gatewayen ansluter till datakällan med hjälp av informationen som angavs.</span><span class="sxs-lookup"><span data-stu-id="1fa04-266">The gateway connects to the data source using the connection information that was provided.</span></span> <span data-ttu-id="1fa04-267">Överväg att gatewayen som ett klientprogram på detta sätt.</span><span class="sxs-lookup"><span data-stu-id="1fa04-267">Consider the gateway as a client application in this sense.</span></span> <span data-ttu-id="1fa04-268">Gatewayen måste bara möjligheten att ansluta till namnet på servern som har angetts.</span><span class="sxs-lookup"><span data-stu-id="1fa04-268">The gateway just needs the capability to connect to the server name that was provided.</span></span>

<a name="why-azure-work-school-account"></a>

<span data-ttu-id="1fa04-269">**Q**: Varför måste jag använda en Azure arbets eller skolkonto för att logga in?</span><span class="sxs-lookup"><span data-stu-id="1fa04-269">**Q**: Why must I use an Azure work or school account to sign in?</span></span> <br/><span data-ttu-id="1fa04-270">
**En**: du kan bara använda en Azure arbets eller skolkonto när du installerar den lokala datagatewayen.</span><span class="sxs-lookup"><span data-stu-id="1fa04-270">
**A**: You can only use an Azure work or school account when you install the on-premises data gateway.</span></span> <span data-ttu-id="1fa04-271">Din inloggning konto lagras i en klient som hanteras av Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="1fa04-271">Your sign-in account is stored in a tenant that's managed by Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="1fa04-272">Azure AD-kontot användarens huvudnamn (UPN) matchar vanligtvis e-postadress.</span><span class="sxs-lookup"><span data-stu-id="1fa04-272">Usually, your Azure AD account's user principal name (UPN) matches the email address.</span></span>

<span data-ttu-id="1fa04-273">**Q**: var lagras mina autentiseringsuppgifter?</span><span class="sxs-lookup"><span data-stu-id="1fa04-273">**Q**: Where are my credentials stored?</span></span> <br/><span data-ttu-id="1fa04-274">
**En**: de autentiseringsuppgifter som du anger för en datakälla krypteras och lagras i gateway-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="1fa04-274">
**A**: The credentials that you enter for a data source are encrypted and stored in the gateway cloud service.</span></span> <span data-ttu-id="1fa04-275">Autentiseringsuppgifterna dekrypteras på lokala datagateway.</span><span class="sxs-lookup"><span data-stu-id="1fa04-275">The credentials are decrypted at the on-premises data gateway.</span></span>

<span data-ttu-id="1fa04-276">**Q**: finns det några krav för nätverksbandbredd?</span><span class="sxs-lookup"><span data-stu-id="1fa04-276">**Q**: Are there any requirements for network bandwidth?</span></span> <br/><span data-ttu-id="1fa04-277">
**En**: Vi rekommenderar att nätverksanslutningen har bra genomströmning.</span><span class="sxs-lookup"><span data-stu-id="1fa04-277">
**A**: We recommend that your network connection has good throughput.</span></span> <span data-ttu-id="1fa04-278">Alla miljöer skiljer sig, och mängden data som skickas påverkar resultaten.</span><span class="sxs-lookup"><span data-stu-id="1fa04-278">Every environment is different, and the amount of data being sent affects the results.</span></span> <span data-ttu-id="1fa04-279">Med hjälp av ExpressRoute kan hjälpa till att garantera en nivå av dataflödet mellan lokala och Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="1fa04-279">Using ExpressRoute could help to guarantee a level of throughput between on-premises and the Azure datacenters.</span></span>
<span data-ttu-id="1fa04-280">Du kan använda verktyg från tredje part Azure hastighet testa appen för att mäta din genomflöde.</span><span class="sxs-lookup"><span data-stu-id="1fa04-280">You can use the third-party tool Azure Speed Test app to help gauge your throughput.</span></span>

<span data-ttu-id="1fa04-281">**Q**: Vad är svarstiden för att köra frågor till en datakälla från gateway?</span><span class="sxs-lookup"><span data-stu-id="1fa04-281">**Q**: What is the latency for running queries to a data source from the gateway?</span></span> <span data-ttu-id="1fa04-282">Vad är den bästa arkitekturen?</span><span class="sxs-lookup"><span data-stu-id="1fa04-282">What is the best architecture?</span></span> <br/><span data-ttu-id="1fa04-283">
**En**: Installera gatewayen för att minska Nätverksfördröjningen som nära datakällan som möjligt.</span><span class="sxs-lookup"><span data-stu-id="1fa04-283">
**A**: To reduce network latency, install the gateway as close to the data source as possible.</span></span> <span data-ttu-id="1fa04-284">Om du kan installera gatewayen på den faktiska datakällan, minimerar den här närhet svarstiden införs.</span><span class="sxs-lookup"><span data-stu-id="1fa04-284">If you can install the gateway on the actual data source, this proximity minimizes the latency introduced.</span></span> <span data-ttu-id="1fa04-285">Överväg att Datacenter för.</span><span class="sxs-lookup"><span data-stu-id="1fa04-285">Consider the datacenters too.</span></span> <span data-ttu-id="1fa04-286">Till exempel om din tjänst använder västra USA datacenter, och du har SQL Server finns i en Azure VM, ska Azure VM vara i västra USA för.</span><span class="sxs-lookup"><span data-stu-id="1fa04-286">For example, if your service uses the West US datacenter, and you have SQL Server hosted in an Azure VM, your Azure VM should be in the West US too.</span></span> <span data-ttu-id="1fa04-287">Den här närhet minimerar fördröjning och undviker utgång avgifter på Azure VM.</span><span class="sxs-lookup"><span data-stu-id="1fa04-287">This proximity minimizes latency and avoids egress charges on the Azure VM.</span></span>

<span data-ttu-id="1fa04-288">**Q**: hur resultatet skickas tillbaka till molnet?</span><span class="sxs-lookup"><span data-stu-id="1fa04-288">**Q**: How are results sent back to the cloud?</span></span> <br/><span data-ttu-id="1fa04-289">
**En**: resultat skickas via Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="1fa04-289">
**A**: Results are sent through the Azure Service Bus.</span></span>

<span data-ttu-id="1fa04-290">**Q**: finns det några inkommande anslutningar till gatewayen från molnet?</span><span class="sxs-lookup"><span data-stu-id="1fa04-290">**Q**: Are there any inbound connections to the gateway from the cloud?</span></span> <br/><span data-ttu-id="1fa04-291">
**En**: Nej</span><span class="sxs-lookup"><span data-stu-id="1fa04-291">
**A**: No.</span></span> <span data-ttu-id="1fa04-292">Gatewayen använder utgående anslutningar till Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="1fa04-292">The gateway uses outbound connections to Azure Service Bus.</span></span>

<span data-ttu-id="1fa04-293">**Q**: Vad händer om jag blockera utgående anslutningar?</span><span class="sxs-lookup"><span data-stu-id="1fa04-293">**Q**: What if I block outbound connections?</span></span> <span data-ttu-id="1fa04-294">Vad behöver jag att öppna?</span><span class="sxs-lookup"><span data-stu-id="1fa04-294">What do I need to open?</span></span> <br/><span data-ttu-id="1fa04-295">
**En**: Se portar och värdar som används av gateway-servern.</span><span class="sxs-lookup"><span data-stu-id="1fa04-295">
**A**: See the ports and hosts that the gateway uses.</span></span>

<span data-ttu-id="1fa04-296">**Q**: det faktiska Windows-tjänsten som kallas?</span><span class="sxs-lookup"><span data-stu-id="1fa04-296">**Q**: What is the actual Windows service called?</span></span><br/><span data-ttu-id="1fa04-297">
**En**: I tjänster gatewayen kallas Power BI Enterprise Gateway-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="1fa04-297">
**A**: In Services, the gateway is called Power BI Enterprise Gateway Service.</span></span>

<span data-ttu-id="1fa04-298">**Q**: kan gateway Windows-tjänsten körs med ett Azure Active Directory-konto?</span><span class="sxs-lookup"><span data-stu-id="1fa04-298">**Q**: Can the gateway Windows service run with an Azure Active Directory account?</span></span> <br/><span data-ttu-id="1fa04-299">
**En**: Nej</span><span class="sxs-lookup"><span data-stu-id="1fa04-299">
**A**: No.</span></span> <span data-ttu-id="1fa04-300">Windows-tjänsten måste ha ett giltigt Windows-konto.</span><span class="sxs-lookup"><span data-stu-id="1fa04-300">The Windows service must have a valid Windows account.</span></span> <span data-ttu-id="1fa04-301">Som standard körs tjänsten med tjänst-SID NT SERVICE\PBIEgwService.</span><span class="sxs-lookup"><span data-stu-id="1fa04-301">By default, the service runs with the Service SID, NT SERVICE\PBIEgwService.</span></span>

### <a name="high-availability-and-disaster-recovery"></a><span data-ttu-id="1fa04-302">Hög tillgänglighet och haveriberedskap</span><span class="sxs-lookup"><span data-stu-id="1fa04-302">High availability and disaster recovery</span></span>

<span data-ttu-id="1fa04-303">**Q**: vilka alternativ som är tillgängliga för katastrofåterställning?</span><span class="sxs-lookup"><span data-stu-id="1fa04-303">**Q**: What options are available for disaster recovery?</span></span> <br/><span data-ttu-id="1fa04-304">
**En**: du kan använda återställningsnyckeln för att återställa eller flytta en gateway.</span><span class="sxs-lookup"><span data-stu-id="1fa04-304">
**A**: You can use the recovery key to restore or move a gateway.</span></span> <span data-ttu-id="1fa04-305">När du installerar en gateway måste du ange återställningsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="1fa04-305">When you install the gateway, specify the recovery key.</span></span>

<span data-ttu-id="1fa04-306">**Q**: Vad är fördelen med återställningsnyckeln?</span><span class="sxs-lookup"><span data-stu-id="1fa04-306">**Q**: What is the benefit of the recovery key?</span></span> <br/><span data-ttu-id="1fa04-307">
**En**: återställningsnyckeln är ett sätt att migrera eller återställa gatewayinställningarna efter en katastrof.</span><span class="sxs-lookup"><span data-stu-id="1fa04-307">
**A**: The recovery key provides a way to migrate or recover your gateway settings after a disaster.</span></span>

<span data-ttu-id="1fa04-308">**Q**: finns det några planer för att aktivera scenarier med hög tillgänglighet med gatewayen?</span><span class="sxs-lookup"><span data-stu-id="1fa04-308">**Q**: Are there any plans for enabling high availability scenarios with the gateway?</span></span> <br/><span data-ttu-id="1fa04-309">
**En**: dessa scenarier är översikt, men vi ännu inte har en tidslinje.</span><span class="sxs-lookup"><span data-stu-id="1fa04-309">
**A**: These scenarios are on the roadmap, but we don't have a timeline yet.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="1fa04-310">Felsökning</span><span class="sxs-lookup"><span data-stu-id="1fa04-310">Troubleshooting</span></span>

[!INCLUDE [existing-gateway-location-changed](../../includes/logic-apps-existing-gateway-location-changed.md)]

<span data-ttu-id="1fa04-311">**Q**: hur kan jag se vilka frågor som ska skickas till den lokala datakällan?</span><span class="sxs-lookup"><span data-stu-id="1fa04-311">**Q**: How can I see what queries are being sent to the on-premises data source?</span></span> <br/><span data-ttu-id="1fa04-312">
**En**: du kan aktivera frågespårning som innehåller de förfrågningar som skickas.</span><span class="sxs-lookup"><span data-stu-id="1fa04-312">
**A**: You can enable query tracing, which includes the queries that are sent.</span></span> <span data-ttu-id="1fa04-313">Kom ihåg att ändra frågan spåra tillbaka till det ursprungliga värdet när du är klar felsökning.</span><span class="sxs-lookup"><span data-stu-id="1fa04-313">Remember to change query tracing back to the original value when done troubleshooting.</span></span> <span data-ttu-id="1fa04-314">Lämna frågespårning aktiverad skapar större loggar.</span><span class="sxs-lookup"><span data-stu-id="1fa04-314">Leaving query tracing turned on creates larger logs.</span></span>

<span data-ttu-id="1fa04-315">Du kan också titta på Verktyg som datakällan har för spårning frågor.</span><span class="sxs-lookup"><span data-stu-id="1fa04-315">You can also look at tools that your data source has for tracing queries.</span></span> <span data-ttu-id="1fa04-316">Du kan till exempel använda Extended Events eller SQL Profiler för SQL Server och Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="1fa04-316">For example, you can use Extended Events or SQL Profiler for SQL Server and Analysis Services.</span></span>

<span data-ttu-id="1fa04-317">**Q**: där är gateway-loggarna?</span><span class="sxs-lookup"><span data-stu-id="1fa04-317">**Q**: Where are the gateway logs?</span></span> <br/><span data-ttu-id="1fa04-318">
**En**: Se verktyg senare i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="1fa04-318">
**A**: See Tools later in this topic.</span></span>

### <a name="update-to-the-latest-version"></a><span data-ttu-id="1fa04-319">Uppdatera till den senaste versionen</span><span class="sxs-lookup"><span data-stu-id="1fa04-319">Update to the latest version</span></span>

<span data-ttu-id="1fa04-320">Många problem kan ansluta när gateway-versionen blir inaktuella.</span><span class="sxs-lookup"><span data-stu-id="1fa04-320">Many issues can surface when the gateway version becomes outdated.</span></span> <span data-ttu-id="1fa04-321">Kontrollera att du använder den senaste versionen som allmän bra.</span><span class="sxs-lookup"><span data-stu-id="1fa04-321">As good general practice, make sure that you use the latest version.</span></span> <span data-ttu-id="1fa04-322">Om du inte har uppdaterat en gateway för en månad eller längre, kan du du överväga att installera den senaste versionen av gatewayen och se om du kan återskapa problemet.</span><span class="sxs-lookup"><span data-stu-id="1fa04-322">If you haven't updated the gateway for a month or longer, you might consider installing the latest version of the gateway, and see if you can reproduce the issue.</span></span>

### <a name="error-failed-to-add-user-to-group--2147463168-pbiegwservice-performance-log-users"></a><span data-ttu-id="1fa04-323">Fel: Det gick inte att lägga till användaren i gruppen.</span><span class="sxs-lookup"><span data-stu-id="1fa04-323">Error: Failed to add user to group.</span></span> <span data-ttu-id="1fa04-324">(-2147463168 PBIEgwService användare)</span><span class="sxs-lookup"><span data-stu-id="1fa04-324">(-2147463168 PBIEgwService Performance Log Users)</span></span>

<span data-ttu-id="1fa04-325">Du kan få detta fel om du försöker installera gatewayen på en domänkontrollant, vilket inte stöds.</span><span class="sxs-lookup"><span data-stu-id="1fa04-325">You might get this error if you try to install the gateway on a domain controller, which isn't supported.</span></span> <span data-ttu-id="1fa04-326">Se till att du distribuerar gatewayen på en dator som inte är en domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="1fa04-326">Make sure that you deploy the gateway on a machine that isn't a domain controller.</span></span>

## <a name="tools"></a><span data-ttu-id="1fa04-327">Verktyg</span><span class="sxs-lookup"><span data-stu-id="1fa04-327">Tools</span></span>

### <a name="collect-logs-from-the-gateway-configurer"></a><span data-ttu-id="1fa04-328">Samla in loggar från gatewayen configurer</span><span class="sxs-lookup"><span data-stu-id="1fa04-328">Collect logs from the gateway configurer</span></span>

<span data-ttu-id="1fa04-329">Du kan samla in flera loggar för gateway.</span><span class="sxs-lookup"><span data-stu-id="1fa04-329">You can collect several logs for the gateway.</span></span> <span data-ttu-id="1fa04-330">Börja alltid med loggar!</span><span class="sxs-lookup"><span data-stu-id="1fa04-330">Always start with the logs!</span></span>

#### <a name="installer-logs"></a><span data-ttu-id="1fa04-331">Installationsloggarna</span><span class="sxs-lookup"><span data-stu-id="1fa04-331">Installer logs</span></span>

`%localappdata%\Temp\Power_BI_Gateway_–Enterprise.log`

#### <a name="configuration-logs"></a><span data-ttu-id="1fa04-332">Av konfigurationsloggar</span><span class="sxs-lookup"><span data-stu-id="1fa04-332">Configuration logs</span></span>

`%localappdata%\Microsoft\Power BI Enterprise Gateway\GatewayConfigurator.log`

#### <a name="enterprise-gateway-service-logs"></a><span data-ttu-id="1fa04-333">Enterprise gateway tjänstloggar</span><span class="sxs-lookup"><span data-stu-id="1fa04-333">Enterprise gateway service logs</span></span>

`C:\Users\PBIEgwService\AppData\Local\Microsoft\Power BI Enterprise Gateway\EnterpriseGateway.log`

#### <a name="event-logs"></a><span data-ttu-id="1fa04-334">Händelseloggar</span><span class="sxs-lookup"><span data-stu-id="1fa04-334">Event logs</span></span>

<span data-ttu-id="1fa04-335">Du kan hitta loggar Data Management Gateway och PowerBIGateway under **program- och tjänstloggar**.</span><span class="sxs-lookup"><span data-stu-id="1fa04-335">You can find the Data Management Gateway and PowerBIGateway logs under **Application and Services Logs**.</span></span>

### <a name="fiddler-trace"></a><span data-ttu-id="1fa04-336">Fiddler spårning</span><span class="sxs-lookup"><span data-stu-id="1fa04-336">Fiddler Trace</span></span>

<span data-ttu-id="1fa04-337">[Fiddler](http://www.telerik.com/fiddler) är ett kostnadsfritt verktyg från Telerik som övervakar HTTP-trafik.</span><span class="sxs-lookup"><span data-stu-id="1fa04-337">[Fiddler](http://www.telerik.com/fiddler) is a free tool from Telerik that monitors HTTP traffic.</span></span> <span data-ttu-id="1fa04-338">Du kan se den här trafiken med Power BI-tjänsten från klientdatorn.</span><span class="sxs-lookup"><span data-stu-id="1fa04-338">You can see this traffic with the Power BI service from the client machine.</span></span> <span data-ttu-id="1fa04-339">Den här tjänsten kan visa fel och annan relaterad information.</span><span class="sxs-lookup"><span data-stu-id="1fa04-339">This service might show errors and other related information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1fa04-340">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1fa04-340">Next steps</span></span>
    
* [<span data-ttu-id="1fa04-341">Ansluta till lokala data från logikappar</span><span class="sxs-lookup"><span data-stu-id="1fa04-341">Connect to on-premises data from logic apps</span></span>](../logic-apps/logic-apps-gateway-connection.md)
* [<span data-ttu-id="1fa04-342">Enterprise integration-funktioner</span><span class="sxs-lookup"><span data-stu-id="1fa04-342">Enterprise integration features</span></span>](../logic-apps/logic-apps-enterprise-integration-overview.md)
* [<span data-ttu-id="1fa04-343">Kopplingar för Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="1fa04-343">Connectors for Azure Logic Apps</span></span>](../connectors/apis-list.md)
