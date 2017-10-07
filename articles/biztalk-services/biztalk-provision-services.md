---
title: "aaaCreate Azure BizTalk-tjänst i hello Azure-portalen | Microsoft Docs"
description: "Lär dig hur tooprovision eller skapa Azure BizTalk-tjänst i hello Azure-portalen; MABS WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 3ad18876-a649-40d6-9aa0-1509c1d62c43
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 6781cadada8ac9c84e1fe045d2b0f995811f75b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-biztalk-services-using-hello-azure-portal"></a><span data-ttu-id="908ee-103">Skapa BizTalk-tjänster med hjälp av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="908ee-103">Create BizTalk Services using hello Azure portal</span></span>

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]


> [!TIP]
> <span data-ttu-id="908ee-104">toosign i toohello Azure-portalen som du behöver ett Azure-konto och Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="908ee-104">toosign in toohello Azure portal, you need an Azure account and Azure subscription.</span></span> <span data-ttu-id="908ee-105">Om du inte har något konto kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="908ee-105">If you don't have an account, you can create a free trial account within a few minutes.</span></span> <span data-ttu-id="908ee-106">Se [Kostnadsfri utvärderingsversion av Azure](http://go.microsoft.com/fwlink/p/?LinkID=239738).</span><span class="sxs-lookup"><span data-stu-id="908ee-106">See [Azure Free Trial](http://go.microsoft.com/fwlink/p/?LinkID=239738).</span></span>


## <span data-ttu-id="908ee-107"><a name="CreateService"></a>Skapa en BizTalk-tjänst</span><span class="sxs-lookup"><span data-stu-id="908ee-107"><a name="CreateService"></a>Create a BizTalk Service</span></span>
<span data-ttu-id="908ee-108">Beroende på hello Edition som du väljer, kanske inte alla BizTalk Service-inställningar är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="908ee-108">Depending on hello Edition you choose, not all BizTalk Service settings may be available.</span></span>

1. <span data-ttu-id="908ee-109">Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=213885).</span><span class="sxs-lookup"><span data-stu-id="908ee-109">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).</span></span>
2. <span data-ttu-id="908ee-110">Hello nedre navigeringsfönstret och välj **ny**:</span><span class="sxs-lookup"><span data-stu-id="908ee-110">In hello bottom navigation pane, select **NEW**:</span></span>  
   <span data-ttu-id="908ee-111">![Välj ny hello-knapp][NEWButton]</span><span class="sxs-lookup"><span data-stu-id="908ee-111">![Select hello New button][NEWButton]</span></span>
3. <span data-ttu-id="908ee-112">Välj **APPTJÄNSTER** > **BIZTALK-TJÄNST** > **SKAPA ANPASSAD**:</span><span class="sxs-lookup"><span data-stu-id="908ee-112">Select **APP SERVICES** > **BIZTALK SERVICE** > **CUSTOM CREATE**:</span></span>  
   <span data-ttu-id="908ee-113">![Välj BizTalk-tjänst och välj Skapa anpassad][NewBizTalkService]</span><span class="sxs-lookup"><span data-stu-id="908ee-113">![Select BizTalk Service and select Custom Create][NewBizTalkService]</span></span>
4. <span data-ttu-id="908ee-114">Ange inställningar för hello BizTalk-tjänst:</span><span class="sxs-lookup"><span data-stu-id="908ee-114">Enter hello BizTalk Service settings:</span></span>
   
    <table border="1">
    <tr>
    <td><span data-ttu-id="908ee-115"><strong>BizTalk-tjänstens namn</strong></span><span class="sxs-lookup"><span data-stu-id="908ee-115"><strong>BizTalk service name</strong></span></span></td>
    <td><span data-ttu-id="908ee-116">Du kan ange ett namn, men var specifik.</span><span class="sxs-lookup"><span data-stu-id="908ee-116">You can enter any name but be specific.</span></span> <span data-ttu-id="908ee-117">Några exempel är:</span><span class="sxs-lookup"><span data-stu-id="908ee-117">Some examples include:</span></span><br/><br/><span data-ttu-id="908ee-118">
    <em>mycompany</em>.biztalk.windows.net</span><span class="sxs-lookup"><span data-stu-id="908ee-118">
    <em>mycompany</em>.biztalk.windows.net</span></span><br/><span data-ttu-id="908ee-119">
    <em>mycompanymyapplication</em>.biztalk.windows.net</span><span class="sxs-lookup"><span data-stu-id="908ee-119">
    <em>mycompanymyapplication</em>.biztalk.windows.net</span></span><br/><span data-ttu-id="908ee-120">
    <em>myapplication</em>.biztalk.windows.net</span><span class="sxs-lookup"><span data-stu-id="908ee-120">
    <em>myapplication</em>.biztalk.windows.net</span></span><br/><br/><span data-ttu-id="908ee-121">”. biztalk.windows.net” är automatiskt tillagda toohello namn som du anger.</span><span class="sxs-lookup"><span data-stu-id="908ee-121">".biztalk.windows.net" is automatically added toohello name you enter.</span></span> <span data-ttu-id="908ee-122">Detta skapar en URL-adress används tooaccess din BizTalk-tjänst, som <strong>https://<em>myapplication</em>. biztalk.windows.net</strong>.</span><span class="sxs-lookup"><span data-stu-id="908ee-122">This creates a URL that is used tooaccess your BizTalk Service, like <strong>https://<em>myapplication</em>.biztalk.windows.net</strong>.</span></span>
    </td>
    </tr>
    <tr>
    <td><span data-ttu-id="908ee-123"><strong>Utgåva</strong></span><span class="sxs-lookup"><span data-stu-id="908ee-123"><strong>Edition</strong></span></span></td>
    <td><span data-ttu-id="908ee-124">Om du är i hello testning/development fasen Välj <strong>Developer</strong>.</span><span class="sxs-lookup"><span data-stu-id="908ee-124">If you are in hello testing/development phase, choose <strong>Developer</strong>.</span></span> <span data-ttu-id="908ee-125">Om du i hello produktionsfasen, använder hello <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302279">BizTalk-tjänst: utgåvor diagram</a> toodetermine om <strong>Premium</strong>, <strong>Standard</strong>, eller <strong>Basic</strong>är hello rätt val för ditt affärsscenario.</span><span class="sxs-lookup"><span data-stu-id="908ee-125">If you are in hello production phase, use hello <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302279">BizTalk Services: Editions Chart</a> toodetermine if <strong>Premium</strong>, <strong>Standard</strong>, or <strong>Basic</strong> is hello correct choice for your business scenario.</span></span>
    </td>
    </tr>
    <tr>
    <td><span data-ttu-id="908ee-126"><strong>Region</strong></span><span class="sxs-lookup"><span data-stu-id="908ee-126"><strong>Region</strong></span></span></td>
    <td><span data-ttu-id="908ee-127">Välj hello geografiska region toohost BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="908ee-127">Select hello geographic region toohost your BizTalk Service.</span></span></td>
    </tr>
    <tr>
    <td><span data-ttu-id="908ee-128"><strong>Domänens URL</strong></span><span class="sxs-lookup"><span data-stu-id="908ee-128"><strong>Domain URL</strong></span></span></td>
    <td><span data-ttu-id="908ee-129"><strong>Valfritt</strong>.</span><span class="sxs-lookup"><span data-stu-id="908ee-129"><strong>Optional</strong>.</span></span> <span data-ttu-id="908ee-130">Hello domän-URL är som standard <em>YourBizTalkServiceName</em>. biztalk.windows.net.</span><span class="sxs-lookup"><span data-stu-id="908ee-130">By default, hello domain URL is <em>YourBizTalkServiceName</em>.biztalk.windows.net.</span></span> <span data-ttu-id="908ee-131">Du kan även ange en anpassad domän.</span><span class="sxs-lookup"><span data-stu-id="908ee-131">A custom domain can also be entered.</span></span> <span data-ttu-id="908ee-132">Om din domän till exempel är <em>contoso</em> kan du ange:</span><span class="sxs-lookup"><span data-stu-id="908ee-132">For example, if your domain is <em>contoso</em>, you can enter:</span></span> <br/><br/><span data-ttu-id="908ee-133">
    <em>MyCompany</em>.contoso.com</span><span class="sxs-lookup"><span data-stu-id="908ee-133">
    <em>MyCompany</em>.contoso.com</span></span><br/><span data-ttu-id="908ee-134">
    <em>MyCompanyMyApplication</em>.contoso.com</span><span class="sxs-lookup"><span data-stu-id="908ee-134">
    <em>MyCompanyMyApplication</em>.contoso.com</span></span><br/><span data-ttu-id="908ee-135">
    <em>MyApplication</em>.contoso.com</span><span class="sxs-lookup"><span data-stu-id="908ee-135">
    <em>MyApplication</em>.contoso.com</span></span><br/><span data-ttu-id="908ee-136">
    <em>YourBizTalkServiceName</em>.contoso.com</span><span class="sxs-lookup"><span data-stu-id="908ee-136">
    <em>YourBizTalkServiceName</em>.contoso.com</span></span><br/>
    </td>
    </tr>
    </table>
<span data-ttu-id="908ee-137">Välj hello nästa-pilen.</span><span class="sxs-lookup"><span data-stu-id="908ee-137">Select hello NEXT arrow.</span></span>
<span data-ttu-id="908ee-138">5.</span><span class="sxs-lookup"><span data-stu-id="908ee-138">5.</span></span> <span data-ttu-id="908ee-139">Ange hello lagring och databasinställningar:</span><span class="sxs-lookup"><span data-stu-id="908ee-139">Enter hello Storage and Database Settings:</span></span>  <table border="1">
    <tr>
    <td><span data-ttu-id="908ee-140"><strong>Övervaka/arkivera lagringskontot</strong></span><span class="sxs-lookup"><span data-stu-id="908ee-140"><strong>Monitoring/Archiving storage account</strong></span></span></td>
    <td><span data-ttu-id="908ee-141">Välj ett befintligt lagringskonto eller skapa ett nytt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="908ee-141">Select an existing storage account or create a new storage account.</span></span> <br/><br/><span data-ttu-id="908ee-142">Om du skapar ett nytt lagringskonto anger hello <strong>Lagringskontonamnet</strong>.</span><span class="sxs-lookup"><span data-stu-id="908ee-142">If you create a new Storage account, enter hello <strong>Storage Account Name</strong>.</span></span></td>
    </tr>
    <tr>
    <td><span data-ttu-id="908ee-143"><strong>Spårning av databasen</strong></span><span class="sxs-lookup"><span data-stu-id="908ee-143"><strong>Tracking database</strong></span></span></td>
    <td><span data-ttu-id="908ee-144">Om du använder en befintlig Azure SQL Database kan den inte användas av en annan BizTalk-tjänst.</span><span class="sxs-lookup"><span data-stu-id="908ee-144">If you use an existing Azure SQL Database, it cannot be used by another BizTalk Service.</span></span> <span data-ttu-id="908ee-145">Du måste hello inloggningsnamnet och lösenordet som angavs när den Azure SQL-databasservern skapades.</span><span class="sxs-lookup"><span data-stu-id="908ee-145">You need hello login name and password entered when that Azure SQL Database Server was created.</span></span><br/><br/><span data-ttu-id="908ee-146"><strong>Tips</strong> skapa hello spårning databas och övervakning/arkivering storage-konto i hello samma region som hello BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="908ee-146"><strong>TIP</strong> Create hello Tracking database and Monitoring/Archiving storage account in hello same region as hello BizTalk Service.</span></span></td>
    </tr>
    </table>
<span data-ttu-id="908ee-147">Välj hello nästa-pilen.</span><span class="sxs-lookup"><span data-stu-id="908ee-147">Select hello NEXT arrow.</span></span>
<span data-ttu-id="908ee-148">6.</span><span class="sxs-lookup"><span data-stu-id="908ee-148">6.</span></span> <span data-ttu-id="908ee-149">Ange inställningar för hello:</span><span class="sxs-lookup"><span data-stu-id="908ee-149">Enter hello Database settings:</span></span>  <table border="1">
    <tr>
    <td><span data-ttu-id="908ee-150"><strong>Namn</strong></span><span class="sxs-lookup"><span data-stu-id="908ee-150"><strong>Name</strong></span></span></td>
    <td><span data-ttu-id="908ee-151">Tillgängligt när <strong>skapa en ny SQL Database-instans</strong> väljs i föregående hello-skärmen.</span><span class="sxs-lookup"><span data-stu-id="908ee-151">Available when <strong>Create a new SQL Database instance</strong> is selected in hello previous screen.</span></span>
    <br/><br/>
<span data-ttu-id="908ee-152">Ange en SQL-databas namnet toobe som används av BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="908ee-152">Enter a SQL Database name toobe used by your BizTalk Service.</span></span></td>
    </tr>
    <tr>
    <td><span data-ttu-id="908ee-153"><strong>Server</strong></span><span class="sxs-lookup"><span data-stu-id="908ee-153"><strong>Server</strong></span></span></td>
    <td><span data-ttu-id="908ee-154">Tillgängligt när <strong>skapa en ny SQL Database-instans</strong> väljs i föregående hello-skärmen.</span><span class="sxs-lookup"><span data-stu-id="908ee-154">Available when <strong>Create a new SQL Database instance</strong> is selected in hello previous screen.</span></span>
    <br/><br/>
<span data-ttu-id="908ee-155">Välj en befintlig SQL Database-server eller skapa en ny SQL Database-server.</span><span class="sxs-lookup"><span data-stu-id="908ee-155">Select an existing SQL Database Server or create a new SQL Database server.</span></span></td>
    </tr>
    <tr>
    <td><span data-ttu-id="908ee-156"><strong>Serverns inloggningsnamn</strong></span><span class="sxs-lookup"><span data-stu-id="908ee-156"><strong>Server login name</strong></span></span></td>
    <td><span data-ttu-id="908ee-157">Ange hello inloggningsnamn för användaren.</span><span class="sxs-lookup"><span data-stu-id="908ee-157">Enter hello login user name.</span></span></td>
    </tr>
    <tr>
    <td><span data-ttu-id="908ee-158"><strong>Lösenord för serverinloggning</strong></span><span class="sxs-lookup"><span data-stu-id="908ee-158"><strong>Server login password</strong></span></span></td>
    <td><span data-ttu-id="908ee-159">Ange hello inloggningslösenordet.</span><span class="sxs-lookup"><span data-stu-id="908ee-159">Enter hello login password.</span></span></td>
    </tr>
    <tr>
    <td><span data-ttu-id="908ee-160"><strong>Region</strong></span><span class="sxs-lookup"><span data-stu-id="908ee-160"><strong>Region</strong></span></span></td>
    <td><span data-ttu-id="908ee-161">Tillgängligt om <strong>Skapa en ny SQL Database-instans</strong> har valts.</span><span class="sxs-lookup"><span data-stu-id="908ee-161">Available when <strong>Create a new SQL Database instance</strong> is selected.</span></span> <span data-ttu-id="908ee-162">Välj hello geografiska region toohost SQL-databasen.</span><span class="sxs-lookup"><span data-stu-id="908ee-162">Select hello geographic region toohost your SQL Database.</span></span></td>
    </tr>
    </table>

<span data-ttu-id="908ee-163">Välj hello markerat toocomplete hello guiden.</span><span class="sxs-lookup"><span data-stu-id="908ee-163">Select hello check mark toocomplete hello wizard.</span></span> <span data-ttu-id="908ee-164">hello Förloppsikon visas:</span><span class="sxs-lookup"><span data-stu-id="908ee-164">hello progress icon appears:</span></span>  
![Förloppsikonen visar när du är klar][ProgressComplete]

<span data-ttu-id="908ee-166">När du är färdig har hello Azure BizTalk-tjänst skapats och redo för dina program.</span><span class="sxs-lookup"><span data-stu-id="908ee-166">When complete, hello Azure BizTalk Service is created and ready for your applications.</span></span> <span data-ttu-id="908ee-167">hello standardinställningarna är tillräckliga.</span><span class="sxs-lookup"><span data-stu-id="908ee-167">hello default settings are sufficient.</span></span> <span data-ttu-id="908ee-168">Om du vill toochange hello standardinställningarna väljer **BIZTALK-tjänst** i hello vänstra navigeringsfönstret och välj sedan BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="908ee-168">If you want toochange hello default settings, select **BIZTALK SERVICES** in hello left navigation pane, and then select your BizTalk Service.</span></span> <span data-ttu-id="908ee-169">Ytterligare inställningar visas i hello [instrumentpanelen, övervaka och skala flikar](biztalk-dashboard-monitor-scale-tabs.md) hello överst.</span><span class="sxs-lookup"><span data-stu-id="908ee-169">Additional settings are displayed in hello [Dashboard, Monitor, and Scale tabs](biztalk-dashboard-monitor-scale-tabs.md) at hello top.</span></span>

<span data-ttu-id="908ee-170">Beroende på hello tillståndet för hello BizTalk Service finns det vissa åtgärder inte kan slutföras.</span><span class="sxs-lookup"><span data-stu-id="908ee-170">Depending on hello state of hello BizTalk Service, there are some operations that cannot be completed.</span></span> <span data-ttu-id="908ee-171">En lista över de här åtgärderna gå för[BizTalk tjänster tillstånd diagram](biztalk-service-state-chart.md).</span><span class="sxs-lookup"><span data-stu-id="908ee-171">For a list of these operations, go too[BizTalk Services State Chart](biztalk-service-state-chart.md).</span></span>

## <a name="post-provisioning-steps"></a><span data-ttu-id="908ee-172">Steg efter etableringen</span><span class="sxs-lookup"><span data-stu-id="908ee-172">Post-provisioning steps</span></span>
* [<span data-ttu-id="908ee-173">Installera hello certifikat på en lokal dator</span><span class="sxs-lookup"><span data-stu-id="908ee-173">Install hello certificate on a local computer</span></span>](#InstallCert)
* [<span data-ttu-id="908ee-174">Lägg till ett produktionsklart certifikat</span><span class="sxs-lookup"><span data-stu-id="908ee-174">Add a production-ready certificate</span></span>](#AddCert)
* [<span data-ttu-id="908ee-175">Hämta hello åtkomstkontroll namnområde</span><span class="sxs-lookup"><span data-stu-id="908ee-175">Get hello Access Control namespace</span></span>](#ACS)

#### <span data-ttu-id="908ee-176"><a name="InstallCert"></a>Installera hello certifikat på en lokal dator</span><span class="sxs-lookup"><span data-stu-id="908ee-176"><a name="InstallCert"></a>Install hello certificate on a local computer</span></span>
<span data-ttu-id="908ee-177">Som en del av etableringen av BizTalk-tjänsten skapas ett självsignerat certifikat som associeras med prenumerationen på BizTalk-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="908ee-177">As part of BizTalk Service provisioning, a self-signed certificate is created and associated with your BizTalk Service subscription.</span></span> <span data-ttu-id="908ee-178">Du måste hämta det här certifikatet och installera den på datorer där du distribuerar BizTalk Service program eller skicka meddelanden tooa BizTalk Service slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="908ee-178">You must download this certificate and install it on computers from where you either deploy BizTalk Service applications or send messages tooa BizTalk Service endpoint.</span></span>

1. <span data-ttu-id="908ee-179">Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=213885).</span><span class="sxs-lookup"><span data-stu-id="908ee-179">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).</span></span>
2. <span data-ttu-id="908ee-180">Välj **BIZTALK-tjänst** i hello vänstra navigeringsfönstret och välj sedan din BizTalk Service-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="908ee-180">Select **BIZTALK SERVICES** in hello left navigation pane, and then select your BizTalk Service subscription.</span></span>
3. <span data-ttu-id="908ee-181">Välj hello **instrumentpanelen** fliken.</span><span class="sxs-lookup"><span data-stu-id="908ee-181">Select hello **Dashboard** tab.</span></span>
4. <span data-ttu-id="908ee-182">Välj **Hämta SSL-certifikat**:</span><span class="sxs-lookup"><span data-stu-id="908ee-182">Select **Download SSL Certificate**:</span></span>  
   <span data-ttu-id="908ee-183">![Ändra SSL-certifikat][QuickGlance]</span><span class="sxs-lookup"><span data-stu-id="908ee-183">![Modify SSL Certificate][QuickGlance]</span></span>
5. <span data-ttu-id="908ee-184">Dubbelklicka på hello certifikatet och kör genom hello guiden tooinstall hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="908ee-184">Double-click hello certificate and run through hello wizard tooinstall hello certificate.</span></span> <span data-ttu-id="908ee-185">Kontrollera att du installerar hello certifikatet under hello **betrodda rotcertifikatutfärdare** lagras.</span><span class="sxs-lookup"><span data-stu-id="908ee-185">Make sure you install hello certificate under hello **Trusted Root Certificate Authorities** store.</span></span>

#### <span data-ttu-id="908ee-186"><a name="AddCert"></a>Lägg till ett produktionsklart certifikat</span><span class="sxs-lookup"><span data-stu-id="908ee-186"><a name="AddCert"></a>Add a production-ready certificate</span></span>
<span data-ttu-id="908ee-187">hello självsignerat certifikat som skapas automatiskt när du skapar BizTalk-tjänst är avsedd att användas i utvecklingsmiljöer endast.</span><span class="sxs-lookup"><span data-stu-id="908ee-187">hello self-signed certificate that is automatically created when creating BizTalk Services is intended for use in development environments only.</span></span> <span data-ttu-id="908ee-188">I produktionsscenarier ersätter du det med ett produktionsklart certifikat.</span><span class="sxs-lookup"><span data-stu-id="908ee-188">For production scenarios, replace it with a production-ready certificate.</span></span>

1. <span data-ttu-id="908ee-189">På hello **instrumentpanelen** väljer **uppdatering SSL-certifikat**.</span><span class="sxs-lookup"><span data-stu-id="908ee-189">On hello **Dashboard** tab, select **Update SSL Certificate**.</span></span>
2. <span data-ttu-id="908ee-190">Bläddra tooyour privata SSL-certifikat (*CertificateName*.pfx) som innehåller namnet på din BizTalk Service ange hello lösenord och klicka sedan på hello är markerat.</span><span class="sxs-lookup"><span data-stu-id="908ee-190">Browse tooyour private SSL certificate (*CertificateName*.pfx) that includes your BizTalk Service name, enter hello password, and then click hello check mark.</span></span>

#### <span data-ttu-id="908ee-191"><a name="ACS"></a>Hämta hello åtkomstkontroll namnområde</span><span class="sxs-lookup"><span data-stu-id="908ee-191"><a name="ACS"></a>Get hello Access Control namespace</span></span>
1. <span data-ttu-id="908ee-192">Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=213885).</span><span class="sxs-lookup"><span data-stu-id="908ee-192">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).</span></span>
2. <span data-ttu-id="908ee-193">Välj **BIZTALK-tjänst** i hello vänstra navigeringsfönstret och välj sedan BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="908ee-193">Select **BIZTALK SERVICES** in hello left navigation pane, and then select your BizTalk Service.</span></span>
3. <span data-ttu-id="908ee-194">Markera i Aktivitetsfältet hello **anslutningsinformationen**:</span><span class="sxs-lookup"><span data-stu-id="908ee-194">In hello task bar, select **Connection Information**:</span></span>  
   <span data-ttu-id="908ee-195">![Välj Anslutningsinformation][ACSConnectInfo]</span><span class="sxs-lookup"><span data-stu-id="908ee-195">![Select Connection Information][ACSConnectInfo]</span></span>
4. <span data-ttu-id="908ee-196">Kopiera hello åtkomstkontroll värden.</span><span class="sxs-lookup"><span data-stu-id="908ee-196">Copy hello Access Control values.</span></span>

<span data-ttu-id="908ee-197">När du distribuerar ett BizTalk-tjänstprojekt från Visual Studio anger du detta Access Control-namnområde.</span><span class="sxs-lookup"><span data-stu-id="908ee-197">When you deploy a BizTalk Service project from Visual Studio, you enter this Access Control namespace.</span></span> <span data-ttu-id="908ee-198">hello åtkomstkontroll namnområdet skapas automatiskt för BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="908ee-198">hello Access Control namespace is automatically created for your BizTalk Service.</span></span>

<span data-ttu-id="908ee-199">hello åtkomstkontroll värden kan användas med alla program.</span><span class="sxs-lookup"><span data-stu-id="908ee-199">hello Access Control values can be used with any application.</span></span> <span data-ttu-id="908ee-200">När Azure BizTalk-tjänst skapas styr åtkomstkontroll namnområdet hello autentisering med BizTalk Service-distributionen.</span><span class="sxs-lookup"><span data-stu-id="908ee-200">When Azure BizTalk Services is created, this Access Control namespace controls hello authentication with your BizTalk Service deployment.</span></span> <span data-ttu-id="908ee-201">Om du vill toochange hello prenumerationen eller hantera hello namnområde, väljer **ACTIVE DIRECTORY** i hello vänstra navigeringsfönstret och välj sedan namnområdet.</span><span class="sxs-lookup"><span data-stu-id="908ee-201">If you want toochange hello subscription or manage hello namespace, select **ACTIVE DIRECTORY** in hello left navigation pane and then select your namespace.</span></span> <span data-ttu-id="908ee-202">hello Aktivitetsfältet listar alternativen.</span><span class="sxs-lookup"><span data-stu-id="908ee-202">hello task bar lists your options.</span></span>

<span data-ttu-id="908ee-203">Klicka på **hantera** öppnas hello Access Control-hanteringsportalen.</span><span class="sxs-lookup"><span data-stu-id="908ee-203">Clicking **Manage** opens hello Access Control Management Portal.</span></span> <span data-ttu-id="908ee-204">Hej BizTalk Service använder i hello Access Control-hanteringsportalen, **tjänsten identiteter**:</span><span class="sxs-lookup"><span data-stu-id="908ee-204">In hello Access Control Management Portal, hello BizTalk Service uses **Service identities**:</span></span>  
<span data-ttu-id="908ee-205">![ACS-tjänstidentiteter i hello Access Control Management Portal][ACSServiceIdentities]</span><span class="sxs-lookup"><span data-stu-id="908ee-205">![ACS Service Identities in hello Access Control Management Portal][ACSServiceIdentities]</span></span>

<span data-ttu-id="908ee-206">hello tjänstidentiteten för åtkomstkontroll är en uppsättning autentiseringsuppgifter som gör att program eller klienter tooauthenticate direkt med åtkomstkontroll och ta emot en token.</span><span class="sxs-lookup"><span data-stu-id="908ee-206">hello Access Control service identity is a set of credentials that allow applications or clients tooauthenticate directly with Access Control and receive a token.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="908ee-207">hello BizTalk Service använder **ägare** för hello standardidentiteten för tjänsten och hello **lösenord** värde.</span><span class="sxs-lookup"><span data-stu-id="908ee-207">hello BizTalk Service uses **Owner** for hello default service identity and hello **Password** value.</span></span> <span data-ttu-id="908ee-208">Om du använder hello symmetrisk nyckel värde i stället för hello lösenordsvärdet kan hello följande fel uppstå.</span><span class="sxs-lookup"><span data-stu-id="908ee-208">If you use hello Symmetric Key value instead of hello Password value, hello following error may occur.</span></span><br/><br/><span data-ttu-id="908ee-209">*Kunde inte ansluta toohello Access Control Management Service-kontot med hello angivna autentiseringsuppgifter*</span><span class="sxs-lookup"><span data-stu-id="908ee-209">*Could not connect toohello Access Control Management Service account with hello specified credentials*</span></span>
> 
> 

<span data-ttu-id="908ee-210">I [Hantera ditt ACS-namnområde](https://msdn.microsoft.com/library/azure/hh674478.aspx) visas några riktlinjer och rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="908ee-210">[Managing Your ACS Namespace](https://msdn.microsoft.com/library/azure/hh674478.aspx) lists some guidelines and recommendations.</span></span>

## <a name="requirements-explained"></a><span data-ttu-id="908ee-211">Kraven beskrivs</span><span class="sxs-lookup"><span data-stu-id="908ee-211">Requirements explained</span></span>
<span data-ttu-id="908ee-212">Dessa krav gäller inte toohello Free Edition.</span><span class="sxs-lookup"><span data-stu-id="908ee-212">These requirements do not apply toohello Free Edition.</span></span>

<table border="1">
<tr bgcolor="FAF9F9">
        <td><span data-ttu-id="908ee-213"><strong>Vad du behöver</strong></span><span class="sxs-lookup"><span data-stu-id="908ee-213"><strong>What you need</strong></span></span></td>
        <td><span data-ttu-id="908ee-214"><strong>Varför du behöver den</strong></span><span class="sxs-lookup"><span data-stu-id="908ee-214"><strong>Why you need it</strong></span></span></td>
</tr>
<tr>
<td><span data-ttu-id="908ee-215">Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="908ee-215">Azure subscription</span></span></td>
<td><span data-ttu-id="908ee-216">hello prenumerationen anger vem som kan logga in toohello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="908ee-216">hello subscription determines who can sign in toohello Azure portal.</span></span> <span data-ttu-id="908ee-217">Hej kontoinnehavarens medgivande skapar hello prenumeration på <a HREF="https://account.windowsazure.com/Subscriptions"> Azure-prenumerationer</a>.</span><span class="sxs-lookup"><span data-stu-id="908ee-217">hello Account holder creates hello subscription at <a HREF="https://account.windowsazure.com/Subscriptions"> Azure Subscriptions</a>.</span></span>
<br/><br/>
<span data-ttu-id="908ee-218">hello Azure-konto kan ha flera prenumerationer och kan hanteras av vem som helst som är tillåtet.</span><span class="sxs-lookup"><span data-stu-id="908ee-218">hello Azure account can have multiple subscriptions and can be managed by anyone who is permitted.</span></span> <span data-ttu-id="908ee-219">Till exempel Azure-konto-innehavaren skapas en prenumeration med namnet <em>BizTalkServiceSubscription</em> och ger hello BizTalk-administratörer inom företaget (till exempel ContosoBTSAdmins@live.com) åtkomst till toothis prenumeration.</span><span class="sxs-lookup"><span data-stu-id="908ee-219">For example, your Azure account holder creates a subscription named <em>BizTalkServiceSubscription</em> and gives hello BizTalk Administrators within your company (for example, ContosoBTSAdmins@live.com) access toothis subscription.</span></span> <span data-ttu-id="908ee-220">I det här scenariot hello BizTalk-administratörer för att logga in toohello Azure-portalen och har fullständig administratör rättigheter tooall hello värdbaserade tjänster i hello-prenumeration, inklusive Azure BizTalk-tjänst.</span><span class="sxs-lookup"><span data-stu-id="908ee-220">In this scenario, hello BizTalk Administrators sign in toohello Azure portal and have full Administrator rights tooall hello hosted services in hello subscription, including Azure BizTalk Services.</span></span> <span data-ttu-id="908ee-221">hello BizTalk-administratörer inte hello Azure-konto innehavare och har därför inte åtkomst tooany faktureringsinformation.</span><span class="sxs-lookup"><span data-stu-id="908ee-221">hello BizTalk Administrators are not hello Azure account holders and therefore don't have access tooany billing information.</span></span>
<br/><br/><span data-ttu-id="908ee-222">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=267577">Hantera prenumerationer och Storage-konton i hello Azure-portalen</a> innehåller mer information.</span><span class="sxs-lookup"><span data-stu-id="908ee-222">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=267577"> Manage Subscriptions and Storage Accounts in hello Azure portal</a> provides more information.</span></span>
</td>
</tr>
<tr>
<td><span data-ttu-id="908ee-223">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="908ee-223">Azure SQL Database</span></span></td>
<td><span data-ttu-id="908ee-224">Lagrar hello tabeller, vyer och lagrade procedurer som används av hello BizTalk Service, inklusive hello spårningsdata.</span><span class="sxs-lookup"><span data-stu-id="908ee-224">Stores hello tables, views, and stored procedures used by hello BizTalk Service, including hello Tracking data.</span></span>
<br/><br/>
<span data-ttu-id="908ee-225">Du kan använda en befintlig Azure SQL Server, Azure SQL Database eller automatiskt skapa en ny server eller databas när du skapar en BizTalk-tjänst.</span><span class="sxs-lookup"><span data-stu-id="908ee-225">When you create a BizTalk Service, you can use an existing Azure SQL Server, Azure SQL Database, or automatically create a new Server or Database.</span></span>
<br/><br/>
<span data-ttu-id="908ee-226">hello SQL Database skala konfigureras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="908ee-226">hello SQL Database scale is automatically configured.</span></span> <span data-ttu-id="908ee-227">Normalt är hello standard skala tillräcklig för en BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="908ee-227">Typically, hello default scale is sufficient for a BizTalk Service.</span></span> <span data-ttu-id="908ee-228">Ändra hello skala påverkar priser.</span><span class="sxs-lookup"><span data-stu-id="908ee-228">Changing hello scale impacts pricing.</span></span> <span data-ttu-id="908ee-229">Se <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=234930"> Konton och fakturering i Azure SQL Database</a>
</span><span class="sxs-lookup"><span data-stu-id="908ee-229">See <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=234930"> Accounts and Billing in Azure SQL Database</a>
</span></span><br/><br/><span data-ttu-id="908ee-230">
<strong>Anteckningar</strong>
</span><span class="sxs-lookup"><span data-stu-id="908ee-230">
<strong>Notes</strong>
</span></span><br/>
<ul>
<li> <span data-ttu-id="908ee-231">När du skapar en ny Azure SQL Server och databas, aktiveras automatiskt Azure-tjänsterna.</span><span class="sxs-lookup"><span data-stu-id="908ee-231">When you create a new Azure SQL Server and Database, Azure Services is automatically enabled.</span></span> <span data-ttu-id="908ee-232">hello BizTalk Service kräver Azure Services aktiveras.</span><span class="sxs-lookup"><span data-stu-id="908ee-232">hello BizTalk Service requires Azure Services be enabled.</span></span></li>
<li><span data-ttu-id="908ee-233">Om du skapar en ny Azure SQL-databas på en befintlig Azure SQL Server hello brandväggsregler i hello Server inte ändras.</span><span class="sxs-lookup"><span data-stu-id="908ee-233">If you create a new Azure SQL Database on an existing Azure SQL Server, hello firewall rules of hello Server are not changed.</span></span> <span data-ttu-id="908ee-234">Det är därför möjligt andra Azure-tjänster inte är tillåtna åtkomst toohello Server-databaser.</span><span class="sxs-lookup"><span data-stu-id="908ee-234">As a result, it's possible other Azure Services are not allowed access toohello Server's databases.</span></span></li>
</ul>
</td>
</tr>
<tr>
<td><span data-ttu-id="908ee-235">Namnområdet Azure Access Control</span><span class="sxs-lookup"><span data-stu-id="908ee-235">Azure Access Control namespace</span></span></td>
<td><span data-ttu-id="908ee-236">Autentiserar med Azure BizTalk Services.</span><span class="sxs-lookup"><span data-stu-id="908ee-236">Authenticates with Azure BizTalk Services.</span></span> <span data-ttu-id="908ee-237">När du distribuerar ett BizTalk-tjänstprojekt från Visual Studio anger du detta Access Control-namnområde.</span><span class="sxs-lookup"><span data-stu-id="908ee-237">When you deploy a BizTalk Service project from Visual Studio, you enter this Access Control namespace.</span></span> <span data-ttu-id="908ee-238">När du skapar en BizTalk Service skapas automatiskt hello åtkomstkontroll namnområde.</span><span class="sxs-lookup"><span data-stu-id="908ee-238">When you create a BizTalk Service, hello Access Control namespace is automatically created.</span></span></td>
</tr>

<tr>
<td><span data-ttu-id="908ee-239">Azure-lagringskonto</span><span class="sxs-lookup"><span data-stu-id="908ee-239">Azure Storage account</span></span></td>
<td><span data-ttu-id="908ee-240">Ger åtkomst till tootables, blobbar och köer som används av din BizTalk Service toosave hello följande:</span><span class="sxs-lookup"><span data-stu-id="908ee-240">Gives access tootables, blobs, and queues used by your BizTalk Service toosave hello following:</span></span>

<ul>
<li><span data-ttu-id="908ee-241">Loggfiler för att övervaka hello BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="908ee-241">Log files that monitor hello BizTalk Service.</span></span> <span data-ttu-id="908ee-242">hello övervakning utdata visas även i hello **övervakning** fliken i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="908ee-242">hello monitoring output is also displayed in hello **Monitoring** tab in hello Azure portal.</span></span></li>
<li><span data-ttu-id="908ee-243">När du skapar en X12 eller AS2-avtal mellan partner kan aktivera du hello arkivering funktionen toostore meddelandeegenskaper.</span><span class="sxs-lookup"><span data-stu-id="908ee-243">When creating an X12 or AS2 agreement between partners, you can enable hello Archiving feature toostore message properties.</span></span> <span data-ttu-id="908ee-244">Informationen sparas i hello Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="908ee-244">This data is saved in hello Storage account.</span></span></li>
</ul>
<br/>
<span data-ttu-id="908ee-245">När du skapar en BizTalk-tjänst kan du använda ett befintligt lagringskonto eller automatiskt skapa ett nytt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="908ee-245">When you create a BizTalk Service, you can use an existing Storage account or automatically create a new Storage account.</span></span>
<br/><br/>
<span data-ttu-id="908ee-246">hello standardinställningarna för lagring är tillräckliga för en BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="908ee-246">hello default Storage settings are sufficient for a BizTalk Service.</span></span>
<br/><br/>
<span data-ttu-id="908ee-247">När du skapar ett lagringskonto skapas automatiskt en primär nyckel och en sekundär nyckel.</span><span class="sxs-lookup"><span data-stu-id="908ee-247">When you create a Storage account, a Primary Key and Secondary Key are automatically created.</span></span> <span data-ttu-id="908ee-248">De här nycklarna styra åtkomst tooyour Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="908ee-248">These Keys control access tooyour Storage account.</span></span> <span data-ttu-id="908ee-249">hello BizTalk Service används automatiskt hello primärnyckel.</span><span class="sxs-lookup"><span data-stu-id="908ee-249">hello BizTalk Service automatically uses hello Primary Key.</span></span>
<br/><br/>
<span data-ttu-id="908ee-250">Se <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285671"> Lagring</a> för mer information.</span><span class="sxs-lookup"><span data-stu-id="908ee-250">See <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285671"> Storage</a> for more information.</span></span>
</td>
</tr>

<tr>
<td><span data-ttu-id="908ee-251">Privata SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="908ee-251">SSL private certificate</span></span></td>
<td>
<span data-ttu-id="908ee-252">När en Azure BizTalk-tjänst skapas, skapas också en HTTPS-URL som innehåller namnet på din BizTalk-tjänst.</span><span class="sxs-lookup"><span data-stu-id="908ee-252">When an Azure BizTalk Service is created, an HTTPS URL that includes your BizTalk Service name is also created.</span></span> <span data-ttu-id="908ee-253">Den här URL: en är automatiskt konfigurerade toouse ett självsignerat certifikat endast för utveckling.</span><span class="sxs-lookup"><span data-stu-id="908ee-253">This URL is automatically configured toouse a self-signed development-only certificate.</span></span> <span data-ttu-id="908ee-254">För produktion behöver du ett privat SSL-certifikat.</span><span class="sxs-lookup"><span data-stu-id="908ee-254">For production, you need a private SSL certificate.</span></span>
<br/><br/><span data-ttu-id="908ee-255">
<strong>Viktig SSL-certifikatsinformation</strong>

</span><span class="sxs-lookup"><span data-stu-id="908ee-255">
<strong>Important SSL Certificate Information</strong>

</span></span><ul>
<li><span data-ttu-id="908ee-256">hello certifikatets förfallodatum måste vara mindre än 5 år.</span><span class="sxs-lookup"><span data-stu-id="908ee-256">hello certificate expiration date must be less than 5 years.</span></span></li>
<li><span data-ttu-id="908ee-257">Alla privata certifikat kräver ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="908ee-257">All private certificates require a password.</span></span> <span data-ttu-id="908ee-258">Kom ihåg lösenordet och dela det med dina administratörer.</span><span class="sxs-lookup"><span data-stu-id="908ee-258">Know this password and as a best practice, share this password with your administrators.</span></span></li>
<li><span data-ttu-id="908ee-259">Självsignerade certifikat används i test-/utvecklingsmiljöer.</span><span class="sxs-lookup"><span data-stu-id="908ee-259">Self-signed certificates are used in a test/development environment.</span></span> <span data-ttu-id="908ee-260">När du använder självsignerade certifikat, importera hello certifikatarkivet tooyour personliga certifikat och hello certifikatarkivet för betrodda rotcertifikatutfärdare.</span><span class="sxs-lookup"><span data-stu-id="908ee-260">When using self-signed certificates, import hello certificate tooyour Personal certificate store and hello Trusted Root Certification Authorities certificate store.</span></span></li>
</ul>
<br/><span data-ttu-id="908ee-261">När du skickar hello produktion certifikat begäran tooyour certifikatutfärdare kan ge hello följande egenskaper för certifikat:</span><span class="sxs-lookup"><span data-stu-id="908ee-261">When sending hello production certificate request tooyour certification authority, give hello following certificate properties:</span></span>
<br/>

<ul>
<li><span data-ttu-id="908ee-262"><strong>Förbättrad nyckelanvändning</strong>: Azure BizTalk Services kräver minst serverautentisering.</span><span class="sxs-lookup"><span data-stu-id="908ee-262"><strong>Enhanced Key Usage</strong>: At a minimum, Azure BizTalk Services requires Server Authentication.</span></span></li>
<li><span data-ttu-id="908ee-263"><strong>Eget namn</strong>: Ange hello fullständigt kvalificerade domännamnet (FQDN) för Azure BizTalk-tjänst-URL.</span><span class="sxs-lookup"><span data-stu-id="908ee-263"><strong>Common Name</strong>: Enter hello fully qualified domain name (FQDN) of your Azure BizTalk Service URL.</span></span> <span data-ttu-id="908ee-264">Se <a HREF="#CreateService">Skapa en BizTalk-tjänst</a> i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="908ee-264">See <a HREF="#CreateService">Create a BizTalk Service</a> in this article.</span></span></li>
</ul>
<br/>
<span data-ttu-id="908ee-265">Ett nytt eller annat certifikat kan läggas till efter hello BizTalk Service skapas.</span><span class="sxs-lookup"><span data-stu-id="908ee-265">A new or different certificate can be added after hello BizTalk Service is created.</span></span>
</td>
</tr>
</table>
<!---Loc Comment: Please, check link [Create a BizTalk Service] since it is not redirecting tooany location.--->



## <a name="hybrid-connections"></a><span data-ttu-id="908ee-266">Hybridanslutningar</span><span class="sxs-lookup"><span data-stu-id="908ee-266">Hybrid Connections</span></span>
<span data-ttu-id="908ee-267">När du skapar ett Azure BizTalk-tjänst hello **Hybridanslutningar** är tillgänglig:</span><span class="sxs-lookup"><span data-stu-id="908ee-267">When you create an Azure BizTalk Service, hello **Hybrid Connections** tab is available:</span></span>

![Fliken Hybridanslutningar][HybridConnectionTab]

<span data-ttu-id="908ee-269">Hybridanslutningar har använt tooconnect en Azure-webbplats eller Azure-mobiltjänsten tooany lokala resursen som använder en statisk TCP-port, till exempel SQL Server, MySQL, http-webb-API: er, Mobile Services och de flesta anpassade webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="908ee-269">Hybrid Connections are used tooconnect an Azure website or Azure mobile service tooany on-premises resource that uses a static TCP port, such as SQL Server, MySQL, HTTP Web APIs, Mobile Services, and most custom Web Services.</span></span>  <span data-ttu-id="908ee-270">Hybridanslutningar och hello BizTalk-tjänst för nätverkskort är olika.</span><span class="sxs-lookup"><span data-stu-id="908ee-270">Hybrid Connections and hello BizTalk Adapter Service are different.</span></span> <span data-ttu-id="908ee-271">hello BizTalk-tjänst för nätverkskort är används tooconnect Azure BizTalk-tjänst tooan lokalt rad Affärsappar (LOB).</span><span class="sxs-lookup"><span data-stu-id="908ee-271">hello BizTalk Adapter Service is used tooconnect Azure BizTalk Services tooan on-premises Line of Business (LOB) system.</span></span>

 <span data-ttu-id="908ee-272">Se [Hybridanslutningar](integration-hybrid-connection-overview.md) toolearn mer, inklusive att skapa och hantera Hybridanslutningar.</span><span class="sxs-lookup"><span data-stu-id="908ee-272">See [Hybrid Connections](integration-hybrid-connection-overview.md) toolearn more, including creating and managing Hybrid Connections.</span></span>

## <a name="next-steps"></a><span data-ttu-id="908ee-273">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="908ee-273">Next steps</span></span>
<span data-ttu-id="908ee-274">En BizTalk Service är skapat kan du bekanta dig med olika hello [BizTalk-tjänst: instrumentpanelen, övervaka och skala flikar](biztalk-dashboard-monitor-scale-tabs.md).</span><span class="sxs-lookup"><span data-stu-id="908ee-274">Now that a BizTalk Service is created, familiarize yourself with hello different [BizTalk Services: Dashboard, Monitor and Scale tabs](biztalk-dashboard-monitor-scale-tabs.md).</span></span> <span data-ttu-id="908ee-275">Din BizTalk-tjänst är redo för dina program.</span><span class="sxs-lookup"><span data-stu-id="908ee-275">Your BizTalk Service is ready for your applications.</span></span> <span data-ttu-id="908ee-276">för att skapa program, gå toostart[Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197).</span><span class="sxs-lookup"><span data-stu-id="908ee-276">toostart creating applications, go too[Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197).</span></span>

## <a name="see-also"></a><span data-ttu-id="908ee-277">Se även</span><span class="sxs-lookup"><span data-stu-id="908ee-277">See also</span></span>
* [<span data-ttu-id="908ee-278">BizTalk Services: Diagram över utgåvor</span><span class="sxs-lookup"><span data-stu-id="908ee-278">BizTalk Services: Editions Chart</span></span>](biztalk-editions-feature-chart.md)<br/>
* [<span data-ttu-id="908ee-279">BizTalk Services: Statusdiagram</span><span class="sxs-lookup"><span data-stu-id="908ee-279">BizTalk Services: State Chart</span></span>](biztalk-service-state-chart.md)<br/>
* [<span data-ttu-id="908ee-280">BizTalk Services: Säkerhetskopiering och återställning</span><span class="sxs-lookup"><span data-stu-id="908ee-280">BizTalk Services: Backup and Restore</span></span>](biztalk-backup-restore.md)<br/>
* [<span data-ttu-id="908ee-281">BizTalk Services: Begränsning</span><span class="sxs-lookup"><span data-stu-id="908ee-281">BizTalk Services: Throttling</span></span>](biztalk-throttling-thresholds.md)<br/>
* [<span data-ttu-id="908ee-282">BizTalk Services: Utfärdarens namn och nyckel</span><span class="sxs-lookup"><span data-stu-id="908ee-282">BizTalk Services: Issuer Name and Issuer Key</span></span>](biztalk-issuer-name-issuer-key.md)<br/>
* [<span data-ttu-id="908ee-283">Hur jag börja använda hello Azure BizTalk Services SDK</span><span class="sxs-lookup"><span data-stu-id="908ee-283">How do I Start Using hello Azure BizTalk Services SDK</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
* [<span data-ttu-id="908ee-284">Hybridanslutningar</span><span class="sxs-lookup"><span data-stu-id="908ee-284">Hybrid Connections</span></span>](integration-hybrid-connection-overview.md)

[NewBizTalkService]: ./media/biztalk-provision-services/WABS_NewBizTalkService.png
[NEWButton]: ./media/biztalk-provision-services/WABS_New.png
[ProgressComplete]: ./media/biztalk-provision-services/WABS_ProgressComplete.png
[ACSConnectInfo]: ./media/biztalk-provision-services/WABS_ACSConnectInformation.png
[QuickGlance]: ./media/biztalk-provision-services/WABS_QuickGlance.png
[ACSServiceIdentities]: ./media/biztalk-provision-services/WABS_ACSServiceIdentities.png
[HybridConnectionTab]: ./media/biztalk-provision-services/WABS_HybridConnectionTab.png
