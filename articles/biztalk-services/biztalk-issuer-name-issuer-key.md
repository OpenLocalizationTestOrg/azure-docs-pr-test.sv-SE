---
title: "aaaIssuer namn och nyckel utfärdaren i BizTalk-tjänst | Microsoft Docs"
description: "Lär dig hur tooretrieve Utfärdares namn och utfärdaren nyckeln för Service Bus eller Access Control (ACS) i BizTalk-tjänst. MABS WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 067fe356-d1aa-420f-b2f2-1a418686470a
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: cc84c2820724ae3e7fc7c40ddbcd83a169add911
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="biztalk-services-issuer-name-and-issuer-key"></a><span data-ttu-id="acf99-104">BizTalk Services: Utfärdarens namn och nyckel</span><span class="sxs-lookup"><span data-stu-id="acf99-104">BizTalk Services: Issuer Name and Issuer Key</span></span>

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

<span data-ttu-id="acf99-105">Azure BizTalk-tjänster använder hello Service Bus Utfärdarens namn och utfärdare, och hello Access Control Utfärdarens namn och utfärdaren nyckeln.</span><span class="sxs-lookup"><span data-stu-id="acf99-105">Azure BizTalk Services uses hello Service Bus Issuer Name and Issuer Key, and hello Access Control Issuer Name and Issuer Key.</span></span> <span data-ttu-id="acf99-106">Närmare bestämt:</span><span class="sxs-lookup"><span data-stu-id="acf99-106">Specifically:</span></span>

| <span data-ttu-id="acf99-107">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="acf99-107">Task</span></span> | <span data-ttu-id="acf99-108">Vilka Utfärdarens namn och en utfärdare nyckel</span><span class="sxs-lookup"><span data-stu-id="acf99-108">Which Issuer Name and Issuer Key</span></span> |
| --- | --- |
| <span data-ttu-id="acf99-109">Distribuera programmet från Visual Studio</span><span class="sxs-lookup"><span data-stu-id="acf99-109">Deploying your application from Visual Studio</span></span> |<span data-ttu-id="acf99-110">Access Control Utfärdarens namn och en utfärdare nyckel</span><span class="sxs-lookup"><span data-stu-id="acf99-110">Access Control Issuer Name and Issuer Key</span></span> |
| <span data-ttu-id="acf99-111">Konfigurera hello Azure BizTalk-Services-portalen</span><span class="sxs-lookup"><span data-stu-id="acf99-111">Configuring hello Azure BizTalk Services Portal</span></span> |<span data-ttu-id="acf99-112">Access Control Utfärdarens namn och en utfärdare nyckel</span><span class="sxs-lookup"><span data-stu-id="acf99-112">Access Control Issuer Name and Issuer Key</span></span> |
| <span data-ttu-id="acf99-113">Skapa LOB-reläer med hello BizTalk Adapter tjänster i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="acf99-113">Creating LOB Relays with hello BizTalk Adapter Services in Visual Studio</span></span> |<span data-ttu-id="acf99-114">Service Bus Utfärdarens namn och en utfärdare nyckel</span><span class="sxs-lookup"><span data-stu-id="acf99-114">Service Bus Issuer Name and Issuer Key</span></span> |

<span data-ttu-id="acf99-115">Det här avsnittet visar hello steg tooretrieve hello Utfärdarens namn och nyckeln för utfärdaren.</span><span class="sxs-lookup"><span data-stu-id="acf99-115">This topic lists hello steps tooretrieve hello Issuer Name and Issuer Key.</span></span> 

## <a name="access-control-issuer-name-and-issuer-key"></a><span data-ttu-id="acf99-116">Access Control Utfärdarens namn och en utfärdare nyckel</span><span class="sxs-lookup"><span data-stu-id="acf99-116">Access Control Issuer Name and Issuer Key</span></span>
<span data-ttu-id="acf99-117">hello Access Control Utfärdarens namn och utfärdaren nyckeln används av hello följande:</span><span class="sxs-lookup"><span data-stu-id="acf99-117">hello Access Control Issuer Name and Issuer Key are used by hello following:</span></span>

* <span data-ttu-id="acf99-118">Ditt program för Azure BizTalk-tjänst som skapats i Visual Studio: toosuccessfully distribuera BizTalk Service programmet i Visual Studio tooAzure, anger du hello Access Control Utfärdarens namn och nyckeln för utfärdaren.</span><span class="sxs-lookup"><span data-stu-id="acf99-118">Your Azure BizTalk Service application created in Visual Studio: toosuccessfully deploy your BizTalk Service application in Visual Studio tooAzure, you enter hello Access Control Issuer Name and Issuer Key.</span></span> 
* <span data-ttu-id="acf99-119">hello Azure BizTalk-Services-portalen: när du skapar en BizTalk Service och öppna hello BizTalk-Services-portalen, din Access Control Utfärdarens namn och nyckeln för utfärdaren registreras automatiskt för dina distributioner med hello samma värden för åtkomstkontroll.</span><span class="sxs-lookup"><span data-stu-id="acf99-119">hello Azure BizTalk Services  Portal: When you create a BizTalk Service and open hello BizTalk Services Portal, your Access Control Issuer Name and Issuer Key are automatically registered for your deployments with hello same Access Control values.</span></span>

### <a name="get-hello-access-control-issuer-name-and-issuer-key"></a><span data-ttu-id="acf99-120">Hämta hello Access Control Utfärdarens namn och en utfärdare nyckel</span><span class="sxs-lookup"><span data-stu-id="acf99-120">Get hello Access Control Issuer Name and Issuer Key</span></span>

<span data-ttu-id="acf99-121">toouse ACS för autentisering och get Hej Utfärdarens namn och nyckel för utfärdaren, hello övergripande stegen innefattar:</span><span class="sxs-lookup"><span data-stu-id="acf99-121">toouse ACS for authentication, and get hello Issuer Name and Issuer Key values, hello overall steps include:</span></span>

1. <span data-ttu-id="acf99-122">Installera hello [Azure Powershell-cmdlets](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).</span><span class="sxs-lookup"><span data-stu-id="acf99-122">Install hello [Azure Powershell cmdlets](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).</span></span>
2. <span data-ttu-id="acf99-123">Lägg till ditt Azure-konto:`Add-AzureAccount`</span><span class="sxs-lookup"><span data-stu-id="acf99-123">Add your Azure account: `Add-AzureAccount`</span></span>
3. <span data-ttu-id="acf99-124">Returnera namnet på din prenumeration:`get-azuresubscription`</span><span class="sxs-lookup"><span data-stu-id="acf99-124">Return your subscription name: `get-azuresubscription`</span></span>
4. <span data-ttu-id="acf99-125">Välj din prenumeration:`select-azuresubscription <name of your subscription>`</span><span class="sxs-lookup"><span data-stu-id="acf99-125">Select your subscription: `select-azuresubscription <name of your subscription>`</span></span> 
5. <span data-ttu-id="acf99-126">Skapa ett nytt namnområde:`new-azuresbnamespace <name for hello service bus> "Location" -CreateACSNamespace $true -NamespaceType Messaging`</span><span class="sxs-lookup"><span data-stu-id="acf99-126">Create a new namespace: `new-azuresbnamespace <name for hello service bus> "Location" -CreateACSNamespace $true -NamespaceType Messaging`</span></span>

    <span data-ttu-id="acf99-127">Exempel:`new-azuresbnamespace biztalksbnamespace "South Central US" -CreateACSNamespace $true -NamespaceType Messaging`</span><span class="sxs-lookup"><span data-stu-id="acf99-127">Example:    `new-azuresbnamespace biztalksbnamespace "South Central US" -CreateACSNamespace $true -NamespaceType Messaging`</span></span>
      
5. <span data-ttu-id="acf99-128">När hello nya ACS-namnområdet har skapats (vilket kan ta flera minuter), anges hello Utfärdarens namn och nyckeln för utfärdaren värdena i anslutningssträngen för hello:</span><span class="sxs-lookup"><span data-stu-id="acf99-128">When hello new ACS namespace is created (which can take several minutes), hello Issuer Name and Issuer Key values are listed in hello connection string:</span></span> 

    ```
    Name                  : biztalksbnamespace
    Region                : South Central US
    DefaultKey            : abcdefghijklmnopqrstuvwxyz
    Status                : Active
    CreatedAt             : 10/18/2016 9:36:30 PM
    AcsManagementEndpoint : https://biztalksbnamespace-sb.accesscontrol.windows.net/
    ServiceBusEndpoint    : https://biztalksbnamespace.servicebus.windows.net/
    ConnectionString      : Endpoint=sb://biztalksbnamespace.servicebus.windows.net/;SharedSecretIssuer=owner;SharedSecretValue=abcdefghijklmnopqrstuvwxyz
    NamespaceType         : Messaging
    ```

<span data-ttu-id="acf99-129">toosummarize:</span><span class="sxs-lookup"><span data-stu-id="acf99-129">toosummarize:</span></span>  
<span data-ttu-id="acf99-130">Utfärdarens namn = SharedSecretIssuer</span><span class="sxs-lookup"><span data-stu-id="acf99-130">Issuer Name = SharedSecretIssuer</span></span>  
<span data-ttu-id="acf99-131">Utfärdaren nyckel = SharedSecretKey</span><span class="sxs-lookup"><span data-stu-id="acf99-131">Issuer Key = SharedSecretKey</span></span>

<span data-ttu-id="acf99-132">Mer om hello [ny AzureSBNamespace](https://msdn.microsoft.com/library/dn495165.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="acf99-132">More on hello [New-AzureSBNamespace](https://msdn.microsoft.com/library/dn495165.aspx) cmdlet.</span></span> 

## <a name="service-bus-issuer-name-and-issuer-key"></a><span data-ttu-id="acf99-133">Service Bus Utfärdarens namn och en utfärdare nyckel</span><span class="sxs-lookup"><span data-stu-id="acf99-133">Service Bus Issuer Name and Issuer Key</span></span>
<span data-ttu-id="acf99-134">Service Bus Utfärdarens namn och nyckeln för utfärdaren som används av BizTalk-tjänst för nätverkskortet.</span><span class="sxs-lookup"><span data-stu-id="acf99-134">Service Bus Issuer Name and Issuer Key are used by BizTalk Adapter Services.</span></span> <span data-ttu-id="acf99-135">I projektet BizTalk-tjänst i Visual Studio kan du använda hello BizTalk Adapter Services tooconnect tooan lokalt branschspecifika (LOB) system.</span><span class="sxs-lookup"><span data-stu-id="acf99-135">In your BizTalk Services project in Visual Studio, you use hello BizTalk Adapter Services tooconnect tooan on-premises Line-of-Business (LOB) system.</span></span> <span data-ttu-id="acf99-136">tooconnect, du skapar hello LOB Relay och ange din information för LOB-system.</span><span class="sxs-lookup"><span data-stu-id="acf99-136">tooconnect, you create hello LOB Relay and enter your LOB system details.</span></span> <span data-ttu-id="acf99-137">När du gör detta måste ange du också hello Service Bus Utfärdarens namn och nyckeln för utfärdaren.</span><span class="sxs-lookup"><span data-stu-id="acf99-137">When doing this, you also enter hello Service Bus Issuer Name and Issuer Key.</span></span>

### <a name="tooretrieve-hello-service-bus-issuer-name-and-issuer-key"></a><span data-ttu-id="acf99-138">tooretrieve hello Service Bus Utfärdarens namn och en utfärdare nyckel</span><span class="sxs-lookup"><span data-stu-id="acf99-138">tooretrieve hello Service Bus Issuer Name and Issuer Key</span></span>
1. <span data-ttu-id="acf99-139">Logga in toohello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=213885).</span><span class="sxs-lookup"><span data-stu-id="acf99-139">Sign in toohello [Azure classic portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).</span></span>
2. <span data-ttu-id="acf99-140">Hello vänstra navigeringsfönstret och välj **Service Bus**.</span><span class="sxs-lookup"><span data-stu-id="acf99-140">In hello left navigation pane, select **Service Bus**.</span></span>
3. <span data-ttu-id="acf99-141">Välj namnområdet.</span><span class="sxs-lookup"><span data-stu-id="acf99-141">Select your namespace.</span></span> <span data-ttu-id="acf99-142">Markera i Aktivitetsfältet hello **anslutningsinformationen**.</span><span class="sxs-lookup"><span data-stu-id="acf99-142">In hello task bar, select **Connection Information**.</span></span> <span data-ttu-id="acf99-143">Detta visar hello **standard utfärdaren** (Utfärdarens namn) och **standard nyckeln** (utfärdaren nyckel).</span><span class="sxs-lookup"><span data-stu-id="acf99-143">This displays hello **Default Issuer** (Issuer Name) and **Default Key** (Issuer Key).</span></span> <span data-ttu-id="acf99-144">Deras värden kan kopieras.</span><span class="sxs-lookup"><span data-stu-id="acf99-144">Their values can be copied.</span></span>  

<span data-ttu-id="acf99-145">toosummarize:</span><span class="sxs-lookup"><span data-stu-id="acf99-145">toosummarize:</span></span>  
<span data-ttu-id="acf99-146">Utfärdarens namn = standard utfärdare</span><span class="sxs-lookup"><span data-stu-id="acf99-146">Issuer Name = Default Issuer</span></span>  
<span data-ttu-id="acf99-147">Utfärdaren nyckel = Standardnyckeln</span><span class="sxs-lookup"><span data-stu-id="acf99-147">Issuer Key = Default Key</span></span>

## <a name="next"></a><span data-ttu-id="acf99-148">Nästa</span><span class="sxs-lookup"><span data-stu-id="acf99-148">Next</span></span>
<span data-ttu-id="acf99-149">Avsnitt om ytterligare Azure BizTalk-tjänst:</span><span class="sxs-lookup"><span data-stu-id="acf99-149">Additional Azure BizTalk Services topics:</span></span>

* [<span data-ttu-id="acf99-150">Installera hello Azure BizTalk Services SDK</span><span class="sxs-lookup"><span data-stu-id="acf99-150">Installing hello Azure BizTalk Services SDK</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
* [<span data-ttu-id="acf99-151">Självstudier: Azure BizTalk-tjänst</span><span class="sxs-lookup"><span data-stu-id="acf99-151">Tutorials: Azure BizTalk Services</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
* [<span data-ttu-id="acf99-152">Hur jag börja använda hello Azure BizTalk Services SDK</span><span class="sxs-lookup"><span data-stu-id="acf99-152">How do I Start Using hello Azure BizTalk Services SDK</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
* [<span data-ttu-id="acf99-153">Azure BizTalk-tjänst</span><span class="sxs-lookup"><span data-stu-id="acf99-153">Azure BizTalk Services</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>

## <a name="see-also"></a><span data-ttu-id="acf99-154">Se även</span><span class="sxs-lookup"><span data-stu-id="acf99-154">See Also</span></span>
* [<span data-ttu-id="acf99-155">Så här: använda ACS hanteringstjänsten tooConfigure Tjänsteidentiteter</span><span class="sxs-lookup"><span data-stu-id="acf99-155">How to: Use ACS Management Service tooConfigure Service Identities</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=303942)<br/>
* [<span data-ttu-id="acf99-156">BizTalk-tjänst: Utvecklare, Basic, Standard och Premium-utgåvor diagram</span><span class="sxs-lookup"><span data-stu-id="acf99-156">BizTalk Services: Developer, Basic, Standard and Premium Editions Chart</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
* [<span data-ttu-id="acf99-157">BizTalk-tjänst: Etablering med hjälp av Azure klassiska portal</span><span class="sxs-lookup"><span data-stu-id="acf99-157">BizTalk Services: Provisioning Using Azure classic portal</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
* [<span data-ttu-id="acf99-158">BizTalk Services: Etablering av statusdiagram</span><span class="sxs-lookup"><span data-stu-id="acf99-158">BizTalk Services: Provisioning Status Chart</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
* [<span data-ttu-id="acf99-159">BizTalk Services: Flikarna Instrumentpanel, Övervakare och Skalning</span><span class="sxs-lookup"><span data-stu-id="acf99-159">BizTalk Services: Dashboard, Monitor and Scale tabs</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
* [<span data-ttu-id="acf99-160">BizTalk Services: Säkerhetskopiering och återställning</span><span class="sxs-lookup"><span data-stu-id="acf99-160">BizTalk Services: Backup and Restore</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
* [<span data-ttu-id="acf99-161">BizTalk Services: Begränsning</span><span class="sxs-lookup"><span data-stu-id="acf99-161">BizTalk Services: Throttling</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>

