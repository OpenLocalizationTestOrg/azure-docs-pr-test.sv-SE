---
title: aaaInstall lokala datagateway - Azure Logic Apps | Microsoft Docs
description: "Innan du komma åt datakällor lokalt installera hello lokala datagateway för snabb överföring och kryptering mellan datakällor lokalt och logikappar"
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
ms.openlocfilehash: 01386a904d856ff545f2eca8eb1b008dcdc08574
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-hello-on-premises-data-gateway-for-azure-logic-apps"></a><span data-ttu-id="6298e-104">Installera hello lokala datagateway för Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="6298e-104">Install hello on-premises data gateway for Azure Logic Apps</span></span>

<span data-ttu-id="6298e-105">Innan dina logic apps kan komma åt datakällor lokalt, måste du installera och konfigurera hello lokala datagateway.</span><span class="sxs-lookup"><span data-stu-id="6298e-105">Before your logic apps can access data sources on premises, you must install and set up hello on-premises data gateway.</span></span> <span data-ttu-id="6298e-106">hello gateway fungerar som en brygga som ger snabb överföring och kryptering mellan lokala system och dina logic apps.</span><span class="sxs-lookup"><span data-stu-id="6298e-106">hello gateway acts as a bridge that provides quick data transfer and encryption between on-premises systems and your logic apps.</span></span> <span data-ttu-id="6298e-107">hello gateway vidarebefordrar data från lokala datakällor på krypterade kanaler via hello Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="6298e-107">hello gateway relays data from on-premises sources on encrypted channels through hello Azure Service Bus.</span></span> <span data-ttu-id="6298e-108">All trafik som kommer från som säkra utgående trafik från hello gateway agent.</span><span class="sxs-lookup"><span data-stu-id="6298e-108">All traffic originates as secure outbound traffic from hello gateway agent.</span></span> <span data-ttu-id="6298e-109">Lär dig mer om [hur hello datagateway fungerar](#gateway-cloud-service).</span><span class="sxs-lookup"><span data-stu-id="6298e-109">Learn more about [how hello data gateway works](#gateway-cloud-service).</span></span>

<span data-ttu-id="6298e-110">hello gateway har stöd för anslutningar toothese datakällor lokalt:</span><span class="sxs-lookup"><span data-stu-id="6298e-110">hello gateway supports connections toothese data sources on premises:</span></span>

*   <span data-ttu-id="6298e-111">BizTalk Server 2016</span><span class="sxs-lookup"><span data-stu-id="6298e-111">BizTalk Server 2016</span></span>
*   <span data-ttu-id="6298e-112">DB2</span><span class="sxs-lookup"><span data-stu-id="6298e-112">DB2</span></span>  
*   <span data-ttu-id="6298e-113">Filsystem</span><span class="sxs-lookup"><span data-stu-id="6298e-113">File System</span></span>
*   <span data-ttu-id="6298e-114">Informix</span><span class="sxs-lookup"><span data-stu-id="6298e-114">Informix</span></span>
*   <span data-ttu-id="6298e-115">MQ</span><span class="sxs-lookup"><span data-stu-id="6298e-115">MQ</span></span>
*   <span data-ttu-id="6298e-116">MySQL</span><span class="sxs-lookup"><span data-stu-id="6298e-116">MySQL</span></span>
*   <span data-ttu-id="6298e-117">Oracle-databas</span><span class="sxs-lookup"><span data-stu-id="6298e-117">Oracle Database</span></span>
*   <span data-ttu-id="6298e-118">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="6298e-118">PostgreSQL</span></span>
*   <span data-ttu-id="6298e-119">SAP-programserver</span><span class="sxs-lookup"><span data-stu-id="6298e-119">SAP Application Server</span></span> 
*   <span data-ttu-id="6298e-120">SAP Message Server</span><span class="sxs-lookup"><span data-stu-id="6298e-120">SAP Message Server</span></span>
*   <span data-ttu-id="6298e-121">SharePoint</span><span class="sxs-lookup"><span data-stu-id="6298e-121">SharePoint</span></span>
*   <span data-ttu-id="6298e-122">SQL Server</span><span class="sxs-lookup"><span data-stu-id="6298e-122">SQL Server</span></span>
*   <span data-ttu-id="6298e-123">Teradata</span><span class="sxs-lookup"><span data-stu-id="6298e-123">Teradata</span></span>

<span data-ttu-id="6298e-124">Dessa steg visar hur toofirst installera hello lokala datagateway innan du [upprätta en anslutning mellan hello gateway och dina logic apps](./logic-apps-gateway-connection.md).</span><span class="sxs-lookup"><span data-stu-id="6298e-124">These steps show how toofirst install hello on-premises data gateway before you [set up a connection between hello gateway and your logic apps](./logic-apps-gateway-connection.md).</span></span> <span data-ttu-id="6298e-125">Läs mer om kopplingar som stöds, [kopplingar för Azure Logikappar](https://docs.microsoft.com/azure/connectors/apis-list).</span><span class="sxs-lookup"><span data-stu-id="6298e-125">For more information about supported connectors, see [Connectors for Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list).</span></span> 

<span data-ttu-id="6298e-126">Information om hur toouse hello gateway med andra tjänster finns i följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="6298e-126">For information about how toouse hello gateway with other services, see these articles:</span></span>

*   [<span data-ttu-id="6298e-127">Microsoft Power BI lokala datagateway</span><span class="sxs-lookup"><span data-stu-id="6298e-127">Microsoft Power BI on-premises data gateway</span></span>](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem/)
*   [<span data-ttu-id="6298e-128">Azure Analysis Services lokala datagateway</span><span class="sxs-lookup"><span data-stu-id="6298e-128">Azure Analysis Services on-premises data gateway</span></span>](../analysis-services/analysis-services-gateway.md)
*   [<span data-ttu-id="6298e-129">Microsoft Flow lokala datagateway</span><span class="sxs-lookup"><span data-stu-id="6298e-129">Microsoft Flow on-premises data gateway</span></span>](https://flow.microsoft.com/documentation/gateway-manage/)
*   [<span data-ttu-id="6298e-130">Microsoft PowerApps lokala datagateway</span><span class="sxs-lookup"><span data-stu-id="6298e-130">Microsoft PowerApps on-premises data gateway</span></span>](https://powerapps.microsoft.com/tutorials/gateway-management/)

<a name="requirements"></a>
## <a name="requirements"></a><span data-ttu-id="6298e-131">Krav</span><span class="sxs-lookup"><span data-stu-id="6298e-131">Requirements</span></span>

<span data-ttu-id="6298e-132">**Minsta**:</span><span class="sxs-lookup"><span data-stu-id="6298e-132">**Minimum**:</span></span>

* <span data-ttu-id="6298e-133">4.5 för .NET framework</span><span class="sxs-lookup"><span data-stu-id="6298e-133">.NET 4.5 Framework</span></span>
* <span data-ttu-id="6298e-134">64-bitars version av Windows 7 eller Windows Server 2008 R2 (eller senare)</span><span class="sxs-lookup"><span data-stu-id="6298e-134">64-bit version of Windows 7 or Windows Server 2008 R2 (or later)</span></span>

<span data-ttu-id="6298e-135">**Rekommenderade**:</span><span class="sxs-lookup"><span data-stu-id="6298e-135">**Recommended**:</span></span>

* <span data-ttu-id="6298e-136">8 kärnor CPU</span><span class="sxs-lookup"><span data-stu-id="6298e-136">8 Core CPU</span></span>
* <span data-ttu-id="6298e-137">8 GB minne</span><span class="sxs-lookup"><span data-stu-id="6298e-137">8 GB Memory</span></span>
* <span data-ttu-id="6298e-138">64-bitarsversionen av Windows 2012 R2 (eller senare)</span><span class="sxs-lookup"><span data-stu-id="6298e-138">64-bit version of Windows 2012 R2 (or later)</span></span>

<span data-ttu-id="6298e-139">**Tänk på följande**:</span><span class="sxs-lookup"><span data-stu-id="6298e-139">**Important considerations**:</span></span>

* <span data-ttu-id="6298e-140">Installera hello lokala datagateway på en lokal dator.</span><span class="sxs-lookup"><span data-stu-id="6298e-140">Install hello on-premises data gateway only on a local computer.</span></span>
<span data-ttu-id="6298e-141">Du kan inte installera hello gateway på en domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="6298e-141">You can't install hello gateway on a domain controller.</span></span>

   > [!TIP]
   > <span data-ttu-id="6298e-142">Det finns inte tooinstall hello gateway på hello samma dator som datakällan.</span><span class="sxs-lookup"><span data-stu-id="6298e-142">You don't have tooinstall hello gateway on hello same computer as your data source.</span></span> <span data-ttu-id="6298e-143">toominimize latens, kan du installera hello gateway så nära som möjligt tooyour datakälla, eller på hello samma dator, förutsatt att du har behörighet.</span><span class="sxs-lookup"><span data-stu-id="6298e-143">toominimize latency, you can install hello gateway as close as possible tooyour data source, or on hello same computer, assuming that you have permissions.</span></span>

* <span data-ttu-id="6298e-144">Installera inte hello gateway på en dator som stängs av, går toosleep eller inte ansluta toohello Internet eftersom hello gateway inte kan köras under dessa omständigheter.</span><span class="sxs-lookup"><span data-stu-id="6298e-144">Don't install hello gateway on a computer that turns off, goes toosleep, or doesn't connect toohello Internet because hello gateway can't run under those circumstances.</span></span> <span data-ttu-id="6298e-145">Gateway-prestanda kan dessutom lidande över ett nätverk.</span><span class="sxs-lookup"><span data-stu-id="6298e-145">Also, gateway performance might suffer over a wireless network.</span></span>

* <span data-ttu-id="6298e-146">Under installationen måste du logga in med en [arbets- eller skolkonto](https://docs.microsoft.com/azure/active-directory/sign-up-organization) som hanteras av Azure Active Directory (Azure AD), inte ett Microsoft-konto.</span><span class="sxs-lookup"><span data-stu-id="6298e-146">During installation, you must sign in with a [work or school account](https://docs.microsoft.com/azure/active-directory/sign-up-organization) that's managed by Azure Active Directory (Azure AD), not a Microsoft account.</span></span> 

  <span data-ttu-id="6298e-147">Du har toouse hello samma arbets- eller skolkonto konto senare i hello Azure portal när du skapar och associerar en gateway-resurs med gatewayinstallationen.</span><span class="sxs-lookup"><span data-stu-id="6298e-147">You have toouse hello same work or school account later in hello Azure portal when you create and associate a gateway resource with your gateway installation.</span></span> <span data-ttu-id="6298e-148">Du kan sedan välja den här gatewayresursen när du skapar hello anslutning mellan logik appen och hello lokala datakällan.</span><span class="sxs-lookup"><span data-stu-id="6298e-148">You then select this gateway resource when you create hello connection between your logic app and hello on-premises data source.</span></span> [<span data-ttu-id="6298e-149">Varför måste använda en Azure AD-arbets eller skolkonto?</span><span class="sxs-lookup"><span data-stu-id="6298e-149">Why must I use an Azure AD work or school account?</span></span>](#why-azure-work-school-account)

  > [!TIP]
  > <span data-ttu-id="6298e-150">Om du har registrerat dig för ett erbjudande för Office 365 och gick inte att få din faktiska arbetet i e-post, din adress för inloggningen kan se ut som jeff@contoso.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="6298e-150">If you signed up for an Office 365 offering and didn't supply your actual work email, your sign-in address might look like jeff@contoso.onmicrosoft.com.</span></span> 

* <span data-ttu-id="6298e-151">Om du har en befintlig gateway som konfigurerats med ett installationsprogram som är äldre än version 14.16.6317.4 kan ändra du inte plats för din gateway körs hello senaste Installer.</span><span class="sxs-lookup"><span data-stu-id="6298e-151">If you have an existing gateway that you set up with an installer that's earlier than version 14.16.6317.4, you can't change your gateway's location by running hello latest installer.</span></span> <span data-ttu-id="6298e-152">Du kan dock använda hello senaste installer tooset upp en ny gateway med hello plats i stället.</span><span class="sxs-lookup"><span data-stu-id="6298e-152">However, you can use hello latest installer tooset up a new gateway with hello location that you want instead.</span></span>
  
  <span data-ttu-id="6298e-153">Om du har en gateway-installationsprogram som är äldre än version 14.16.6317.4, men du inte har installerat din gateway ännu, kan du hämta och använda hello senaste installer.</span><span class="sxs-lookup"><span data-stu-id="6298e-153">If you have a gateway installer that's earlier than version 14.16.6317.4, but you haven't installed your gateway yet, you can download and use hello latest installer.</span></span>

<a name="install-gateway"></a>

## <a name="install-hello-data-gateway"></a><span data-ttu-id="6298e-154">Installera hello datagateway</span><span class="sxs-lookup"><span data-stu-id="6298e-154">Install hello data gateway</span></span>

1.  <span data-ttu-id="6298e-155">[Hämta och kör installationsprogrammet för hello-gateway på en lokal dator](http://go.microsoft.com/fwlink/?LinkID=820931&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="6298e-155">[Download and run hello gateway installer on a local computer](http://go.microsoft.com/fwlink/?LinkID=820931&clcid=0x409).</span></span>

2. <span data-ttu-id="6298e-156">Granska och Godkänn hello användningsvillkoren och sekretesspolicyn.</span><span class="sxs-lookup"><span data-stu-id="6298e-156">Review and accept hello terms of use and privacy statement.</span></span>

3. <span data-ttu-id="6298e-157">Ange hello sökväg på den lokala datorn där du vill att tooinstall hello gateway.</span><span class="sxs-lookup"><span data-stu-id="6298e-157">Specify hello path on your local computer where you want tooinstall hello gateway.</span></span>

4. <span data-ttu-id="6298e-158">När du uppmanas logga in med ditt Azure arbets- eller skolkonto inte ett Microsoft-konto.</span><span class="sxs-lookup"><span data-stu-id="6298e-158">When prompted, sign in with your Azure work or school account, not a Microsoft account.</span></span>

   ![Logga in med Azure arbets- eller skolkonto](./media/logic-apps-gateway-install/sign-in-gateway-install.png)

5. <span data-ttu-id="6298e-160">Nu registrera din installerade gateway med hello [gateway-Molntjänsten](#gateway-cloud-service).</span><span class="sxs-lookup"><span data-stu-id="6298e-160">Now register your installed gateway with hello [gateway cloud service](#gateway-cloud-service).</span></span> <span data-ttu-id="6298e-161">Välj **registrera en ny gateway på den här datorn**.</span><span class="sxs-lookup"><span data-stu-id="6298e-161">Choose **Register a new gateway on this computer**.</span></span>

   <span data-ttu-id="6298e-162">hello gateway-Molntjänsten krypterar och lagrar dina autentiseringsuppgifter för datakälla och gateway-information.</span><span class="sxs-lookup"><span data-stu-id="6298e-162">hello gateway cloud service encrypts and stores your data source credentials and gateway details.</span></span> 
   <span data-ttu-id="6298e-163">hello service skickar också frågor och deras resultat mellan din logikapp, hello lokala datagateway och datakällan lokalt.</span><span class="sxs-lookup"><span data-stu-id="6298e-163">hello service also routes queries and their results between your logic app, hello on-premises data gateway, and your data source on premises.</span></span>

6. <span data-ttu-id="6298e-164">Ange ett namn för din gateway-installation.</span><span class="sxs-lookup"><span data-stu-id="6298e-164">Provide a name for your gateway installation.</span></span> <span data-ttu-id="6298e-165">Skapa en återställningsnyckel och sedan bekräfta din återställningsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="6298e-165">Create a recovery key, then confirm your recovery key.</span></span> 

   > [!IMPORTANT] 
   > <span data-ttu-id="6298e-166">Återställningsnyckeln måste innehålla minst åtta tecken.</span><span class="sxs-lookup"><span data-stu-id="6298e-166">Your recovery key must contain at least eight characters.</span></span> <span data-ttu-id="6298e-167">Kontrollera att du sparar och hålla hello nyckel på en säker plats.</span><span class="sxs-lookup"><span data-stu-id="6298e-167">Make sure that you save and keep hello key in a safe place.</span></span> <span data-ttu-id="6298e-168">Du måste också den här nyckeln om du vill toomigrate, återställa eller ta över en befintlig gateway.</span><span class="sxs-lookup"><span data-stu-id="6298e-168">You also need this key when you want toomigrate, restore, or take over an existing gateway.</span></span>

   1. <span data-ttu-id="6298e-169">Välj toochange hello standardregion för hello gateway-Molntjänsten och Azure Service Bus som används av din gateway-installation **ändra Region**.</span><span class="sxs-lookup"><span data-stu-id="6298e-169">toochange hello default region for hello gateway cloud service and Azure Service Bus used by your gateway installation, choose **Change Region**.</span></span>

      ![Ändra region](./media/logic-apps-gateway-install/change-region-gateway-install.png)

      <span data-ttu-id="6298e-171">hello standardregion är hello region som är associerade med Azure AD-klienten.</span><span class="sxs-lookup"><span data-stu-id="6298e-171">hello default region is hello region associated with your Azure AD tenant.</span></span>

   2. <span data-ttu-id="6298e-172">Öppna hello på hello nästa ruta, **väljer Region** för välja en annan region.</span><span class="sxs-lookup"><span data-stu-id="6298e-172">On hello next pane, open hello **Select Region** too choose a different region.</span></span>

      ![Välj en annan region](./media/logic-apps-gateway-install/select-region-gateway-install.png)

      <span data-ttu-id="6298e-174">Till exempel du kanske hello Välj samma region som din logikapp eller välj hello region närmaste tooyour lokala datakällan så att du kan minska svarstiden.</span><span class="sxs-lookup"><span data-stu-id="6298e-174">For example, you might select hello same region as your logic app, or select hello region closest tooyour on-premises data source so you can reduce latency.</span></span> <span data-ttu-id="6298e-175">Gateway-resurs och logik appen kan ha olika platser.</span><span class="sxs-lookup"><span data-stu-id="6298e-175">Your gateway resource and logic app can have different locations.</span></span>

      > [!IMPORTANT]
      > <span data-ttu-id="6298e-176">Du kan inte ändra den här regionen efter installationen.</span><span class="sxs-lookup"><span data-stu-id="6298e-176">You can't change this region after installation.</span></span> <span data-ttu-id="6298e-177">Den här regionen avgör också och begränsar hello plats där du kan skapa hello Azure-resurs för din gateway.</span><span class="sxs-lookup"><span data-stu-id="6298e-177">This region also determines and restricts hello location where you can create hello Azure resource for your gateway.</span></span> <span data-ttu-id="6298e-178">Så när du skapar din gateway-resurs i Azure, se till att hello resursplats matchar hello region som du valde under gatewayinstallationen av.</span><span class="sxs-lookup"><span data-stu-id="6298e-178">So when you create your gateway resource in Azure, make sure that hello resource location matches hello region that you selected during gateway installation.</span></span>
      > 
      > <span data-ttu-id="6298e-179">Om du vill toouse en annan region för din gateway senare, måste du ställa in en ny gateway.</span><span class="sxs-lookup"><span data-stu-id="6298e-179">If you want toouse a different region for your gateway later, you must set up a new gateway.</span></span>

   3. <span data-ttu-id="6298e-180">När du är klar kan du välja **klar**.</span><span class="sxs-lookup"><span data-stu-id="6298e-180">When you're ready, choose **Done**.</span></span>

7. <span data-ttu-id="6298e-181">Nu följer du stegen i hello Azure-portalen så att du kan [skapa en Azure-resurs för din gateway](../logic-apps/logic-apps-gateway-connection.md).</span><span class="sxs-lookup"><span data-stu-id="6298e-181">Now follow these steps in hello Azure portal so you can [create an Azure resource for your gateway](../logic-apps/logic-apps-gateway-connection.md).</span></span> 

<span data-ttu-id="6298e-182">Lär dig mer om [hur hello datagateway fungerar](#gateway-cloud-service).</span><span class="sxs-lookup"><span data-stu-id="6298e-182">Learn more about [how hello data gateway works](#gateway-cloud-service).</span></span>

## <a name="migrate-restore-or-take-over-an-existing-gateway"></a><span data-ttu-id="6298e-183">Migrera, Återställ eller ta över en befintlig gateway</span><span class="sxs-lookup"><span data-stu-id="6298e-183">Migrate, restore, or take over an existing gateway</span></span>

<span data-ttu-id="6298e-184">tooperform dessa uppgifter, måste du ha hello återställningsnyckeln som angavs när hello gateway har installerats.</span><span class="sxs-lookup"><span data-stu-id="6298e-184">tooperform these tasks, you must have hello recovery key that was specified when hello gateway was installed.</span></span>

1. <span data-ttu-id="6298e-185">Datorns Start-menyn, Välj **lokala datagateway**.</span><span class="sxs-lookup"><span data-stu-id="6298e-185">From your computer's Start menu, choose **On-premises data gateway**.</span></span>

2. <span data-ttu-id="6298e-186">När hello installer öppnas, logga in med hello samma Azure arbets- eller skolkonto som tidigare var används tooinstall hello gateway.</span><span class="sxs-lookup"><span data-stu-id="6298e-186">After hello installer opens, sign in with hello same Azure work or school account that was previously used tooinstall hello gateway.</span></span>

3. <span data-ttu-id="6298e-187">Välj **migrera, Återställ eller ta över en befintlig gateway**.</span><span class="sxs-lookup"><span data-stu-id="6298e-187">Choose **Migrate, restore, or take over an existing gateway**.</span></span>

4. <span data-ttu-id="6298e-188">Ange hello namn och återställa nyckeln för hello-gateway som du vill ha toomigrate, Återställ eller ta över.</span><span class="sxs-lookup"><span data-stu-id="6298e-188">Provide hello name and recovery key for hello gateway that you want toomigrate, restore, or take over.</span></span>

<a name="restart-gateway"></a>
## <a name="restart-hello-gateway"></a><span data-ttu-id="6298e-189">Starta om hello gateway</span><span class="sxs-lookup"><span data-stu-id="6298e-189">Restart hello gateway</span></span>

<span data-ttu-id="6298e-190">hello gateway körs som en Windows-tjänst.</span><span class="sxs-lookup"><span data-stu-id="6298e-190">hello gateway runs as a Windows service.</span></span> <span data-ttu-id="6298e-191">Precis som alla andra Windows-tjänster, kan du starta och stoppa hello-tjänsten på flera olika sätt.</span><span class="sxs-lookup"><span data-stu-id="6298e-191">Like any other Windows service, you can start and stop hello service in multiple ways.</span></span> <span data-ttu-id="6298e-192">Du kan till exempel öppna en kommandotolk med förhöjd behörighet på hello datorn där hello gatewayen körs och kör antingen dessa kommandon:</span><span class="sxs-lookup"><span data-stu-id="6298e-192">For example, you can open a command prompt with elevated permissions on hello computer where hello gateway is running, and run either these commands:</span></span>

* <span data-ttu-id="6298e-193">toostop hello-tjänst, köra det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="6298e-193">toostop hello service, run this command:</span></span>
  
    `net stop PBIEgwService`

* <span data-ttu-id="6298e-194">toostart hello-tjänst, köra det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="6298e-194">toostart hello service, run this command:</span></span>
  
    `net start PBIEgwService`

### <a name="windows-service-account"></a><span data-ttu-id="6298e-195">Windows-tjänstkontot</span><span class="sxs-lookup"><span data-stu-id="6298e-195">Windows service account</span></span>

<span data-ttu-id="6298e-196">hello lokala datagateway har konfigurerats toouse `NT SERVICE\PBIEgwService` tjänsten inloggningsuppgifter för Windows hello.</span><span class="sxs-lookup"><span data-stu-id="6298e-196">hello on-premises data gateway is set up toouse `NT SERVICE\PBIEgwService` for hello Windows service logon credentials.</span></span> <span data-ttu-id="6298e-197">Som standard har hello gateway hello ”logga in som en tjänst” höger för hello datorn där du installerar hello gateway.</span><span class="sxs-lookup"><span data-stu-id="6298e-197">By default, hello gateway has hello "Log on as a service" right for hello machine where you install hello gateway.</span></span>

> [!NOTE]
> <span data-ttu-id="6298e-198">hello Windows-tjänstkontot skiljer sig från hello-konto som används för att ansluta tooon lokala datakällor och hello Azure arbets eller skolkonto används toosign i toocloud services.</span><span class="sxs-lookup"><span data-stu-id="6298e-198">hello Windows service account differs from hello account used for connecting tooon-premises data sources, and from hello Azure work or school account used toosign in toocloud services.</span></span>

## <a name="configure-a-firewall-or-proxy"></a><span data-ttu-id="6298e-199">Konfigurera en brandvägg eller proxyserver</span><span class="sxs-lookup"><span data-stu-id="6298e-199">Configure a firewall or proxy</span></span>

<span data-ttu-id="6298e-200">hello gateway skapar en utgående anslutning för [Azure Service Bus](https://azure.microsoft.com/services/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="6298e-200">hello gateway creates an outbound connection too [Azure Service Bus](https://azure.microsoft.com/services/service-bus/).</span></span> <span data-ttu-id="6298e-201">information om tooprovide proxy för din gateway finns [konfigurera proxyinställningar](https://powerbi.microsoft.com/documentation/powerbi-gateway-proxy/).</span><span class="sxs-lookup"><span data-stu-id="6298e-201">tooprovide proxy information for your gateway, see [Configure proxy settings](https://powerbi.microsoft.com/documentation/powerbi-gateway-proxy/).</span></span>

<span data-ttu-id="6298e-202">toocheck om din brandvägg eller proxyserver, kan blockera anslutningar, bekräfta om datorn faktiskt kan ansluta toohello internet och hello [Azure Service Bus](https://azure.microsoft.com/services/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="6298e-202">toocheck whether your firewall, or proxy, might block connections, confirm whether your machine can actually connect toohello internet and hello [Azure Service Bus](https://azure.microsoft.com/services/service-bus/).</span></span> <span data-ttu-id="6298e-203">Kör kommandot från en PowerShell-kommandotolk:</span><span class="sxs-lookup"><span data-stu-id="6298e-203">From a PowerShell prompt, run this command:</span></span>

`Test-NetConnection -ComputerName watchdog.servicebus.windows.net -Port 9350`

> [!NOTE]
> <span data-ttu-id="6298e-204">Det här kommandot kan endast test nätverksanslutning och anslutningen toohello Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="6298e-204">This command only tests network connectivity and connectivity toohello Azure Service Bus.</span></span> <span data-ttu-id="6298e-205">Så hello-kommandot har ingenting toodo med hello gateway eller hello gateway-Molntjänsten som krypterar och lagrar dina autentiseringsuppgifter och gateway-information.</span><span class="sxs-lookup"><span data-stu-id="6298e-205">So hello command doesn't have anything toodo with hello gateway or hello gateway cloud service that encrypts and stores your credentials and gateway details.</span></span> 
>
> <span data-ttu-id="6298e-206">Det här kommandot är också bara tillgängligt på Windows Server 2012 R2 eller senare och Windows 8.1 eller senare.</span><span class="sxs-lookup"><span data-stu-id="6298e-206">Also, this command is only available on Windows Server 2012 R2 or later, and Windows 8.1 or later.</span></span> <span data-ttu-id="6298e-207">I tidigare versioner av Operativsystemet, kan du använda Telnet för testa anslutningen.</span><span class="sxs-lookup"><span data-stu-id="6298e-207">On earlier OS versions, you can use Telnet too test connectivity.</span></span> <span data-ttu-id="6298e-208">Lär dig mer om [Azure Service Bus och hybridlösningar](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="6298e-208">Learn more about [Azure Service Bus and hybrid solutions](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).</span></span>

<span data-ttu-id="6298e-209">Resultatet bör se ut ungefär toothis exempel:</span><span class="sxs-lookup"><span data-stu-id="6298e-209">Your results should look similar toothis example:</span></span>

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

<span data-ttu-id="6298e-210">Om **TcpTestSucceeded** har inte angetts för**SANT**, du kan blockeras av en brandvägg.</span><span class="sxs-lookup"><span data-stu-id="6298e-210">If **TcpTestSucceeded** is not set too**True**, you might be blocked by a firewall.</span></span> <span data-ttu-id="6298e-211">Om du vill toobe omfattande ersätta hello **ComputerName** och **Port** värden med hello värden som visas under [konfigurera portar](#configure-ports) i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="6298e-211">If you want toobe comprehensive, substitute hello **ComputerName** and **Port** values with hello values listed under [Configure ports](#configure-ports) in this topic.</span></span>

<span data-ttu-id="6298e-212">hello-brandväggen kan också blockera anslutningar som hello Azure Service Bus gör toohello Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="6298e-212">hello firewall might also block connections that hello Azure Service Bus makes toohello Azure datacenters.</span></span> <span data-ttu-id="6298e-213">Om det här scenariot inträffar kan godkänna (avblockera) alla hello IP-adresser för de datacenter i din region.</span><span class="sxs-lookup"><span data-stu-id="6298e-213">If this scenario happens, approve (unblock) all hello IP addresses for those datacenters in your region.</span></span> <span data-ttu-id="6298e-214">För de IP-adresserna [get hello Azure IP-adresslistan här](https://www.microsoft.com/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="6298e-214">For those IP addresses, [get hello Azure IP addresses list here](https://www.microsoft.com/download/details.aspx?id=41653).</span></span>

## <a name="configure-ports"></a><span data-ttu-id="6298e-215">Konfigurera portar</span><span class="sxs-lookup"><span data-stu-id="6298e-215">Configure ports</span></span>

<span data-ttu-id="6298e-216">hello gateway skapar en utgående anslutning för[Azure Service Bus](https://azure.microsoft.com/services/service-bus/) och kommunicerar på utgående portar: TCP 443 (standard), 5671, 5672, 9350 via 9354.</span><span class="sxs-lookup"><span data-stu-id="6298e-216">hello gateway creates an outbound connection too[Azure Service Bus](https://azure.microsoft.com/services/service-bus/) and communicates on outbound ports: TCP 443 (default), 5671, 5672, 9350 through 9354.</span></span> <span data-ttu-id="6298e-217">hello gateway kräver inte ingående portar.</span><span class="sxs-lookup"><span data-stu-id="6298e-217">hello gateway doesn't require inbound ports.</span></span> <span data-ttu-id="6298e-218">Lär dig mer om [Azure Service Bus och hybridlösningar](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="6298e-218">Learn more about [Azure Service Bus and hybrid solutions](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).</span></span>

| <span data-ttu-id="6298e-219">DOMÄNNAMN</span><span class="sxs-lookup"><span data-stu-id="6298e-219">DOMAIN NAMES</span></span> | <span data-ttu-id="6298e-220">UTGÅENDE PORTAR</span><span class="sxs-lookup"><span data-stu-id="6298e-220">OUTBOUND PORTS</span></span> | <span data-ttu-id="6298e-221">BESKRIVNING</span><span class="sxs-lookup"><span data-stu-id="6298e-221">DESCRIPTION</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6298e-222">*. analysis.windows.net</span><span class="sxs-lookup"><span data-stu-id="6298e-222">*.analysis.windows.net</span></span> | <span data-ttu-id="6298e-223">443</span><span class="sxs-lookup"><span data-stu-id="6298e-223">443</span></span> | <span data-ttu-id="6298e-224">HTTPS</span><span class="sxs-lookup"><span data-stu-id="6298e-224">HTTPS</span></span> | 
| <span data-ttu-id="6298e-225">*. login.windows.net</span><span class="sxs-lookup"><span data-stu-id="6298e-225">*.login.windows.net</span></span> | <span data-ttu-id="6298e-226">443</span><span class="sxs-lookup"><span data-stu-id="6298e-226">443</span></span> | <span data-ttu-id="6298e-227">HTTPS</span><span class="sxs-lookup"><span data-stu-id="6298e-227">HTTPS</span></span> | 
| <span data-ttu-id="6298e-228">*. servicebus.windows.net</span><span class="sxs-lookup"><span data-stu-id="6298e-228">*.servicebus.windows.net</span></span> | <span data-ttu-id="6298e-229">5671-5672</span><span class="sxs-lookup"><span data-stu-id="6298e-229">5671-5672</span></span> | <span data-ttu-id="6298e-230">Avancerade Message Queuing-protokollet (AMQP)</span><span class="sxs-lookup"><span data-stu-id="6298e-230">Advanced Message Queuing Protocol (AMQP)</span></span> | 
| <span data-ttu-id="6298e-231">*. servicebus.windows.net</span><span class="sxs-lookup"><span data-stu-id="6298e-231">*.servicebus.windows.net</span></span> | <span data-ttu-id="6298e-232">443, 9350-9354</span><span class="sxs-lookup"><span data-stu-id="6298e-232">443, 9350-9354</span></span> | <span data-ttu-id="6298e-233">Lyssnare på Service Bus Relay via TCP (kräver 443 för åtkomstkontroll token)</span><span class="sxs-lookup"><span data-stu-id="6298e-233">Listeners on Service Bus Relay over TCP (requires 443 for Access Control token acquisition)</span></span> | 
| <span data-ttu-id="6298e-234">*. frontend.clouddatahub.net</span><span class="sxs-lookup"><span data-stu-id="6298e-234">*.frontend.clouddatahub.net</span></span> | <span data-ttu-id="6298e-235">443</span><span class="sxs-lookup"><span data-stu-id="6298e-235">443</span></span> | <span data-ttu-id="6298e-236">HTTPS</span><span class="sxs-lookup"><span data-stu-id="6298e-236">HTTPS</span></span> | 
| <span data-ttu-id="6298e-237">*. core.windows.net</span><span class="sxs-lookup"><span data-stu-id="6298e-237">*.core.windows.net</span></span> | <span data-ttu-id="6298e-238">443</span><span class="sxs-lookup"><span data-stu-id="6298e-238">443</span></span> | <span data-ttu-id="6298e-239">HTTPS</span><span class="sxs-lookup"><span data-stu-id="6298e-239">HTTPS</span></span> | 
| <span data-ttu-id="6298e-240">login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="6298e-240">login.microsoftonline.com</span></span> | <span data-ttu-id="6298e-241">443</span><span class="sxs-lookup"><span data-stu-id="6298e-241">443</span></span> | <span data-ttu-id="6298e-242">HTTPS</span><span class="sxs-lookup"><span data-stu-id="6298e-242">HTTPS</span></span> | 
| <span data-ttu-id="6298e-243">*. msftncsi.com</span><span class="sxs-lookup"><span data-stu-id="6298e-243">*.msftncsi.com</span></span> | <span data-ttu-id="6298e-244">443</span><span class="sxs-lookup"><span data-stu-id="6298e-244">443</span></span> | <span data-ttu-id="6298e-245">Använda tootest internet-anslutning när hello gateway inte kan nås av hello Power BI-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="6298e-245">Used tootest internet connectivity when hello gateway is unreachable by hello Power BI service.</span></span> | 

<span data-ttu-id="6298e-246">Om du har tooapprove IP-adresser i stället för hello domäner, du kan hämta och använda hello [Microsoft Azure Datacenter IP-intervall lista](https://www.microsoft.com/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="6298e-246">If you have tooapprove IP addresses instead of hello domains, you can download and use hello [Microsoft Azure Datacenter IP ranges list](https://www.microsoft.com/download/details.aspx?id=41653).</span></span> <span data-ttu-id="6298e-247">I vissa fall görs hello Azure Service Bus-anslutningar med IP-adress i stället för fullständigt kvalificerade domännamn.</span><span class="sxs-lookup"><span data-stu-id="6298e-247">In some cases, hello Azure Service Bus connections are made with IP Address rather than fully qualified domain names.</span></span>

<a name="gateway-cloud-service"></a>
## <a name="how-does-hello-data-gateway-work"></a><span data-ttu-id="6298e-248">Hur fungerar hello datagateway?</span><span class="sxs-lookup"><span data-stu-id="6298e-248">How does hello data gateway work?</span></span>

<span data-ttu-id="6298e-249">Hej datagatewayen underlättar snabbt och säkert kommunikationen mellan din logikapp, hello gateway-Molntjänsten och lokala datakällan.</span><span class="sxs-lookup"><span data-stu-id="6298e-249">hello data gateway facilitates quick and secure communication between your logic app, hello gateway cloud service, and your on-premises data source.</span></span> 

![diagram-for-on-premises-data-gateway-Flow](./media/logic-apps-gateway-install/how-on-premises-data-gateway-works-flow-diagram.png)

<span data-ttu-id="6298e-251">Så när hello användaren i hello molnet interagerar med ett element som har anslutit tooyour lokala datakällan:</span><span class="sxs-lookup"><span data-stu-id="6298e-251">So when hello user in hello cloud interacts with an element that's connected tooyour on-premises data source:</span></span>

1. <span data-ttu-id="6298e-252">hello gateway-Molntjänsten skapar en fråga, tillsammans med hello krypterade autentiseringsuppgifter för datakälla för hello, och skickar hello frågan toohello kön för hello gateway tooprocess.</span><span class="sxs-lookup"><span data-stu-id="6298e-252">hello gateway cloud service creates a query, along with hello encrypted credentials for hello data source, and sends hello query toohello queue for hello gateway tooprocess.</span></span>

2. <span data-ttu-id="6298e-253">hello gateway-Molntjänsten analyserar hello frågan och skickar hello begäran toohello Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="6298e-253">hello gateway cloud service analyzes hello query and pushes hello request toohello Azure Service Bus.</span></span>

3. <span data-ttu-id="6298e-254">hello lokala datagateway avsöker hello Azure Service Bus för väntande begäranden.</span><span class="sxs-lookup"><span data-stu-id="6298e-254">hello on-premises data gateway polls hello Azure Service Bus for pending requests.</span></span>

4. <span data-ttu-id="6298e-255">hello gateway hämtar hello frågan dekrypterar hello autentiseringsuppgifter och ansluter toohello datakällan med autentiseringsuppgifterna.</span><span class="sxs-lookup"><span data-stu-id="6298e-255">hello gateway gets hello query, decrypts hello credentials, and connects toohello data source with those credentials.</span></span>

5. <span data-ttu-id="6298e-256">hello gateway skickar hello toohello frågedatakällan för körning.</span><span class="sxs-lookup"><span data-stu-id="6298e-256">hello gateway sends hello query toohello data source for execution.</span></span>

6. <span data-ttu-id="6298e-257">hello resultat skickas från hello datakällan tillbaka toohello gateway och sedan toohello gateway-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="6298e-257">hello results are sent from hello data source, back toohello gateway, and then toohello gateway cloud service.</span></span> <span data-ttu-id="6298e-258">hello använder gateway-Molntjänsten sedan hello resultat.</span><span class="sxs-lookup"><span data-stu-id="6298e-258">hello gateway cloud service then uses hello results.</span></span>

<a name="faq"></a>
## <a name="frequently-asked-questions"></a><span data-ttu-id="6298e-259">Vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="6298e-259">Frequently asked questions</span></span>

### <a name="general"></a><span data-ttu-id="6298e-260">Allmänt</span><span class="sxs-lookup"><span data-stu-id="6298e-260">General</span></span>

<span data-ttu-id="6298e-261">**Q**: behöver jag en gateway för datakällor i hello molnet, till exempel SQL Azure?</span><span class="sxs-lookup"><span data-stu-id="6298e-261">**Q**: Do I need a gateway for data sources in hello cloud, such as SQL Azure?</span></span> <br/><span data-ttu-id="6298e-262">
**En**: Nej</span><span class="sxs-lookup"><span data-stu-id="6298e-262">
**A**: No.</span></span> <span data-ttu-id="6298e-263">En gateway ansluter bara tooon lokala datakällor.</span><span class="sxs-lookup"><span data-stu-id="6298e-263">A gateway connects tooon-premises data sources only.</span></span>

<span data-ttu-id="6298e-264">**Q**: har hello gateway toobe installerad på detsamma datorn som datakälla för hello hello?</span><span class="sxs-lookup"><span data-stu-id="6298e-264">**Q**: Does hello gateway have toobe installed on hello same machine as hello data source?</span></span> <br/><span data-ttu-id="6298e-265">
**En**: Nej</span><span class="sxs-lookup"><span data-stu-id="6298e-265">
**A**: No.</span></span> <span data-ttu-id="6298e-266">hello gateway ansluter toohello datakälla med hjälp av hello anslutningsinformationen som angavs.</span><span class="sxs-lookup"><span data-stu-id="6298e-266">hello gateway connects toohello data source using hello connection information that was provided.</span></span> <span data-ttu-id="6298e-267">Överväg att hello gateway som ett klientprogram på detta sätt.</span><span class="sxs-lookup"><span data-stu-id="6298e-267">Consider hello gateway as a client application in this sense.</span></span> <span data-ttu-id="6298e-268">hello gateway måste bara hello kapaciteten tooconnect toohello namn på servern som har angetts.</span><span class="sxs-lookup"><span data-stu-id="6298e-268">hello gateway just needs hello capability tooconnect toohello server name that was provided.</span></span>

<a name="why-azure-work-school-account"></a>

<span data-ttu-id="6298e-269">**Q**: Varför måste jag använda en Azure arbete eller skola konto toosign i?</span><span class="sxs-lookup"><span data-stu-id="6298e-269">**Q**: Why must I use an Azure work or school account toosign in?</span></span> <br/><span data-ttu-id="6298e-270">
**En**: du kan bara använda en Azure arbets- eller skolkonto när du installerar hello lokala datagateway.</span><span class="sxs-lookup"><span data-stu-id="6298e-270">
**A**: You can only use an Azure work or school account when you install hello on-premises data gateway.</span></span> <span data-ttu-id="6298e-271">Din inloggning konto lagras i en klient som hanteras av Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="6298e-271">Your sign-in account is stored in a tenant that's managed by Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="6298e-272">Azure AD-kontot användarens huvudnamn (UPN) matchar vanligtvis hello e-postadress.</span><span class="sxs-lookup"><span data-stu-id="6298e-272">Usually, your Azure AD account's user principal name (UPN) matches hello email address.</span></span>

<span data-ttu-id="6298e-273">**Q**: var lagras mina autentiseringsuppgifter?</span><span class="sxs-lookup"><span data-stu-id="6298e-273">**Q**: Where are my credentials stored?</span></span> <br/><span data-ttu-id="6298e-274">
**En**: hello-autentiseringsuppgifter som du anger för en datakälla krypteras och lagras i hello gateway-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="6298e-274">
**A**: hello credentials that you enter for a data source are encrypted and stored in hello gateway cloud service.</span></span> <span data-ttu-id="6298e-275">hello autentiseringsuppgifter dekrypteras på hello lokala datagateway.</span><span class="sxs-lookup"><span data-stu-id="6298e-275">hello credentials are decrypted at hello on-premises data gateway.</span></span>

<span data-ttu-id="6298e-276">**Q**: finns det några krav för nätverksbandbredd?</span><span class="sxs-lookup"><span data-stu-id="6298e-276">**Q**: Are there any requirements for network bandwidth?</span></span> <br/><span data-ttu-id="6298e-277">
**En**: Vi rekommenderar att nätverksanslutningen har bra genomströmning.</span><span class="sxs-lookup"><span data-stu-id="6298e-277">
**A**: We recommend that your network connection has good throughput.</span></span> <span data-ttu-id="6298e-278">Alla miljöer skiljer sig, och hello mängden data som skickas påverkar hello resultat.</span><span class="sxs-lookup"><span data-stu-id="6298e-278">Every environment is different, and hello amount of data being sent affects hello results.</span></span> <span data-ttu-id="6298e-279">Med hjälp av ExpressRoute kan hjälpa tooguarantee en nivå av dataflödet mellan lokala och hello Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="6298e-279">Using ExpressRoute could help tooguarantee a level of throughput between on-premises and hello Azure datacenters.</span></span>
<span data-ttu-id="6298e-280">Du kan använda hello verktyg från tredje part hastighet testning i Azure app toohelp mätaren din genomflöde.</span><span class="sxs-lookup"><span data-stu-id="6298e-280">You can use hello third-party tool Azure Speed Test app toohelp gauge your throughput.</span></span>

<span data-ttu-id="6298e-281">**Q**: Vad är hello svarstid för att köra frågor tooa datakällan från hello gateway?</span><span class="sxs-lookup"><span data-stu-id="6298e-281">**Q**: What is hello latency for running queries tooa data source from hello gateway?</span></span> <span data-ttu-id="6298e-282">Vad är bästa hello-arkitektur?</span><span class="sxs-lookup"><span data-stu-id="6298e-282">What is hello best architecture?</span></span> <br/><span data-ttu-id="6298e-283">
**En**: tooreduce Nätverksfördröjningen, installera hello gateway som Stäng toohello datakälla som möjligt.</span><span class="sxs-lookup"><span data-stu-id="6298e-283">
**A**: tooreduce network latency, install hello gateway as close toohello data source as possible.</span></span> <span data-ttu-id="6298e-284">Om du kan installera hello gateway på hello faktiska datakällan, minimerar den här närhet hello latens införs.</span><span class="sxs-lookup"><span data-stu-id="6298e-284">If you can install hello gateway on hello actual data source, this proximity minimizes hello latency introduced.</span></span> <span data-ttu-id="6298e-285">Överväg att hello Datacenter för.</span><span class="sxs-lookup"><span data-stu-id="6298e-285">Consider hello datacenters too.</span></span> <span data-ttu-id="6298e-286">Till exempel om din tjänst använder hello västra USA datacenter, och du har SQL Server finns i en Azure VM, ska Azure VM vara i hello västra USA för.</span><span class="sxs-lookup"><span data-stu-id="6298e-286">For example, if your service uses hello West US datacenter, and you have SQL Server hosted in an Azure VM, your Azure VM should be in hello West US too.</span></span> <span data-ttu-id="6298e-287">Den här närhet minimerar fördröjning och undviker utgång avgifter för hello Azure VM.</span><span class="sxs-lookup"><span data-stu-id="6298e-287">This proximity minimizes latency and avoids egress charges on hello Azure VM.</span></span>

<span data-ttu-id="6298e-288">**Q**: hur är skickade tillbaka toohello molnet?</span><span class="sxs-lookup"><span data-stu-id="6298e-288">**Q**: How are results sent back toohello cloud?</span></span> <br/><span data-ttu-id="6298e-289">
**En**: resultat skickas via hello Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="6298e-289">
**A**: Results are sent through hello Azure Service Bus.</span></span>

<span data-ttu-id="6298e-290">**Q**: finns det några inkommande anslutningar toohello gateway från hello molnet?</span><span class="sxs-lookup"><span data-stu-id="6298e-290">**Q**: Are there any inbound connections toohello gateway from hello cloud?</span></span> <br/><span data-ttu-id="6298e-291">
**En**: Nej</span><span class="sxs-lookup"><span data-stu-id="6298e-291">
**A**: No.</span></span> <span data-ttu-id="6298e-292">hello gateway använder utgående anslutningar tooAzure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="6298e-292">hello gateway uses outbound connections tooAzure Service Bus.</span></span>

<span data-ttu-id="6298e-293">**Q**: Vad händer om jag blockera utgående anslutningar?</span><span class="sxs-lookup"><span data-stu-id="6298e-293">**Q**: What if I block outbound connections?</span></span> <span data-ttu-id="6298e-294">Vad gör jag behöver tooopen?</span><span class="sxs-lookup"><span data-stu-id="6298e-294">What do I need tooopen?</span></span> <br/><span data-ttu-id="6298e-295">
**En**: Se hello portar och värdar som hello gateway används.</span><span class="sxs-lookup"><span data-stu-id="6298e-295">
**A**: See hello ports and hosts that hello gateway uses.</span></span>

<span data-ttu-id="6298e-296">**Q**: det faktiska Windows hello-tjänsten som kallas?</span><span class="sxs-lookup"><span data-stu-id="6298e-296">**Q**: What is hello actual Windows service called?</span></span><br/><span data-ttu-id="6298e-297">
**En**: tjänster, hello gateway kallas i Power BI Enterprise Gateway-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="6298e-297">
**A**: In Services, hello gateway is called Power BI Enterprise Gateway Service.</span></span>

<span data-ttu-id="6298e-298">**Q**: kan hello gateway Windows-tjänsten körs med ett Azure Active Directory-konto?</span><span class="sxs-lookup"><span data-stu-id="6298e-298">**Q**: Can hello gateway Windows service run with an Azure Active Directory account?</span></span> <br/><span data-ttu-id="6298e-299">
**En**: Nej</span><span class="sxs-lookup"><span data-stu-id="6298e-299">
**A**: No.</span></span> <span data-ttu-id="6298e-300">hello Windows-tjänst måste ha ett giltigt Windows-konto.</span><span class="sxs-lookup"><span data-stu-id="6298e-300">hello Windows service must have a valid Windows account.</span></span> <span data-ttu-id="6298e-301">Som standard körs hello-tjänsten med hello tjänst-SID NT SERVICE\PBIEgwService.</span><span class="sxs-lookup"><span data-stu-id="6298e-301">By default, hello service runs with hello Service SID, NT SERVICE\PBIEgwService.</span></span>

### <a name="high-availability-and-disaster-recovery"></a><span data-ttu-id="6298e-302">Hög tillgänglighet och haveriberedskap</span><span class="sxs-lookup"><span data-stu-id="6298e-302">High availability and disaster recovery</span></span>

<span data-ttu-id="6298e-303">**Q**: vilka alternativ som är tillgängliga för katastrofåterställning?</span><span class="sxs-lookup"><span data-stu-id="6298e-303">**Q**: What options are available for disaster recovery?</span></span> <br/><span data-ttu-id="6298e-304">
**En**: du kan använda hello recovery viktiga toorestore eller flytta en gateway.</span><span class="sxs-lookup"><span data-stu-id="6298e-304">
**A**: You can use hello recovery key toorestore or move a gateway.</span></span> <span data-ttu-id="6298e-305">När du installerar hello gateway, ange hello återställningsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="6298e-305">When you install hello gateway, specify hello recovery key.</span></span>

<span data-ttu-id="6298e-306">**Q**: Vad är hello fördelen hello återställningsnyckeln?</span><span class="sxs-lookup"><span data-stu-id="6298e-306">**Q**: What is hello benefit of hello recovery key?</span></span> <br/><span data-ttu-id="6298e-307">
**En**: hello återställningsnyckeln ger ett sätt toomigrate eller återställa gatewayinställningarna efter en katastrof.</span><span class="sxs-lookup"><span data-stu-id="6298e-307">
**A**: hello recovery key provides a way toomigrate or recover your gateway settings after a disaster.</span></span>

<span data-ttu-id="6298e-308">**Q**: finns det några planer för att aktivera scenarier med hög tillgänglighet med hello gateway?</span><span class="sxs-lookup"><span data-stu-id="6298e-308">**Q**: Are there any plans for enabling high availability scenarios with hello gateway?</span></span> <br/><span data-ttu-id="6298e-309">
**En**: dessa scenarier är hello översikt, men vi ännu inte har en tidslinje.</span><span class="sxs-lookup"><span data-stu-id="6298e-309">
**A**: These scenarios are on hello roadmap, but we don't have a timeline yet.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="6298e-310">Felsökning</span><span class="sxs-lookup"><span data-stu-id="6298e-310">Troubleshooting</span></span>

[!INCLUDE [existing-gateway-location-changed](../../includes/logic-apps-existing-gateway-location-changed.md)]

<span data-ttu-id="6298e-311">**Q**: hur kan jag se vilka frågor skickas toohello lokala datakällan?</span><span class="sxs-lookup"><span data-stu-id="6298e-311">**Q**: How can I see what queries are being sent toohello on-premises data source?</span></span> <br/><span data-ttu-id="6298e-312">
**En**: du kan aktivera frågespårning, vilket innefattar hello frågor som skickas.</span><span class="sxs-lookup"><span data-stu-id="6298e-312">
**A**: You can enable query tracing, which includes hello queries that are sent.</span></span> <span data-ttu-id="6298e-313">Kom ihåg toochange frågan spårning tillbaka toohello ursprungliga värdet när du är klar felsökning.</span><span class="sxs-lookup"><span data-stu-id="6298e-313">Remember toochange query tracing back toohello original value when done troubleshooting.</span></span> <span data-ttu-id="6298e-314">Lämna frågespårning aktiverad skapar större loggar.</span><span class="sxs-lookup"><span data-stu-id="6298e-314">Leaving query tracing turned on creates larger logs.</span></span>

<span data-ttu-id="6298e-315">Du kan också titta på Verktyg som datakällan har för spårning frågor.</span><span class="sxs-lookup"><span data-stu-id="6298e-315">You can also look at tools that your data source has for tracing queries.</span></span> <span data-ttu-id="6298e-316">Du kan till exempel använda Extended Events eller SQL Profiler för SQL Server och Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="6298e-316">For example, you can use Extended Events or SQL Profiler for SQL Server and Analysis Services.</span></span>

<span data-ttu-id="6298e-317">**Q**: där är hello gateway-loggarna?</span><span class="sxs-lookup"><span data-stu-id="6298e-317">**Q**: Where are hello gateway logs?</span></span> <br/><span data-ttu-id="6298e-318">
**En**: Se verktyg senare i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="6298e-318">
**A**: See Tools later in this topic.</span></span>

### <a name="update-toohello-latest-version"></a><span data-ttu-id="6298e-319">Uppdatera toohello senaste versionen</span><span class="sxs-lookup"><span data-stu-id="6298e-319">Update toohello latest version</span></span>

<span data-ttu-id="6298e-320">Många problem kan ansluta när hello gatewayversionen blir inaktuella.</span><span class="sxs-lookup"><span data-stu-id="6298e-320">Many issues can surface when hello gateway version becomes outdated.</span></span> <span data-ttu-id="6298e-321">Kontrollera att du använder hello senaste versionen som allmän bra.</span><span class="sxs-lookup"><span data-stu-id="6298e-321">As good general practice, make sure that you use hello latest version.</span></span> <span data-ttu-id="6298e-322">Om du inte har uppdaterat hello gateway för en månad eller längre, kan du Överväg att installera hello senaste versionen av hello gateway och se om du kan återskapa hello problemet.</span><span class="sxs-lookup"><span data-stu-id="6298e-322">If you haven't updated hello gateway for a month or longer, you might consider installing hello latest version of hello gateway, and see if you can reproduce hello issue.</span></span>

### <a name="error-failed-tooadd-user-toogroup--2147463168-pbiegwservice-performance-log-users"></a><span data-ttu-id="6298e-323">Fel: Det gick inte tooadd användaren toogroup.</span><span class="sxs-lookup"><span data-stu-id="6298e-323">Error: Failed tooadd user toogroup.</span></span> <span data-ttu-id="6298e-324">(-2147463168 PBIEgwService användare)</span><span class="sxs-lookup"><span data-stu-id="6298e-324">(-2147463168 PBIEgwService Performance Log Users)</span></span>

<span data-ttu-id="6298e-325">Du kan få detta fel om du försöker tooinstall hello gateway på en domänkontrollant, vilket inte stöds.</span><span class="sxs-lookup"><span data-stu-id="6298e-325">You might get this error if you try tooinstall hello gateway on a domain controller, which isn't supported.</span></span> <span data-ttu-id="6298e-326">Se till att du distribuerar hello gateway på en dator som inte är en domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="6298e-326">Make sure that you deploy hello gateway on a machine that isn't a domain controller.</span></span>

## <a name="tools"></a><span data-ttu-id="6298e-327">Verktyg</span><span class="sxs-lookup"><span data-stu-id="6298e-327">Tools</span></span>

### <a name="collect-logs-from-hello-gateway-configurer"></a><span data-ttu-id="6298e-328">Samla in loggar från hello gateway configurer</span><span class="sxs-lookup"><span data-stu-id="6298e-328">Collect logs from hello gateway configurer</span></span>

<span data-ttu-id="6298e-329">Du kan samla in flera loggar för hello gateway.</span><span class="sxs-lookup"><span data-stu-id="6298e-329">You can collect several logs for hello gateway.</span></span> <span data-ttu-id="6298e-330">Börja alltid med hello loggar!</span><span class="sxs-lookup"><span data-stu-id="6298e-330">Always start with hello logs!</span></span>

#### <a name="installer-logs"></a><span data-ttu-id="6298e-331">Installationsloggarna</span><span class="sxs-lookup"><span data-stu-id="6298e-331">Installer logs</span></span>

`%localappdata%\Temp\Power_BI_Gateway_–Enterprise.log`

#### <a name="configuration-logs"></a><span data-ttu-id="6298e-332">Av konfigurationsloggar</span><span class="sxs-lookup"><span data-stu-id="6298e-332">Configuration logs</span></span>

`%localappdata%\Microsoft\Power BI Enterprise Gateway\GatewayConfigurator.log`

#### <a name="enterprise-gateway-service-logs"></a><span data-ttu-id="6298e-333">Enterprise gateway tjänstloggar</span><span class="sxs-lookup"><span data-stu-id="6298e-333">Enterprise gateway service logs</span></span>

`C:\Users\PBIEgwService\AppData\Local\Microsoft\Power BI Enterprise Gateway\EnterpriseGateway.log`

#### <a name="event-logs"></a><span data-ttu-id="6298e-334">Händelseloggar</span><span class="sxs-lookup"><span data-stu-id="6298e-334">Event logs</span></span>

<span data-ttu-id="6298e-335">Du kan hitta hello loggar Data Management Gateway och PowerBIGateway under **program- och tjänstloggar**.</span><span class="sxs-lookup"><span data-stu-id="6298e-335">You can find hello Data Management Gateway and PowerBIGateway logs under **Application and Services Logs**.</span></span>

### <a name="fiddler-trace"></a><span data-ttu-id="6298e-336">Fiddler spårning</span><span class="sxs-lookup"><span data-stu-id="6298e-336">Fiddler Trace</span></span>

<span data-ttu-id="6298e-337">[Fiddler](http://www.telerik.com/fiddler) är ett kostnadsfritt verktyg från Telerik som övervakar HTTP-trafik.</span><span class="sxs-lookup"><span data-stu-id="6298e-337">[Fiddler](http://www.telerik.com/fiddler) is a free tool from Telerik that monitors HTTP traffic.</span></span> <span data-ttu-id="6298e-338">Du kan se den här trafiken med hello Power BI-tjänsten från hello klientdatorn.</span><span class="sxs-lookup"><span data-stu-id="6298e-338">You can see this traffic with hello Power BI service from hello client machine.</span></span> <span data-ttu-id="6298e-339">Den här tjänsten kan visa fel och annan relaterad information.</span><span class="sxs-lookup"><span data-stu-id="6298e-339">This service might show errors and other related information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6298e-340">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6298e-340">Next steps</span></span>
    
* [<span data-ttu-id="6298e-341">Ansluta tooon lokala data från logikappar</span><span class="sxs-lookup"><span data-stu-id="6298e-341">Connect tooon-premises data from logic apps</span></span>](../logic-apps/logic-apps-gateway-connection.md)
* [<span data-ttu-id="6298e-342">Enterprise integration-funktioner</span><span class="sxs-lookup"><span data-stu-id="6298e-342">Enterprise integration features</span></span>](../logic-apps/logic-apps-enterprise-integration-overview.md)
* [<span data-ttu-id="6298e-343">Kopplingar för Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="6298e-343">Connectors for Azure Logic Apps</span></span>](../connectors/apis-list.md)
