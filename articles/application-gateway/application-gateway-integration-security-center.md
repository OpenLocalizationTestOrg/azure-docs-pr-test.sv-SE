---
title: aaaApplication Gateway-integration med Azure Security Center | Microsoft Docs
description: "Den här sidan innehåller information om hur Application Gateway är integrerat i Azure Security Center."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: 
ms.assetid: e5ea5cf9-3b41-4b85-a12c-e758bff7f3ec
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: 
ms.workload: infrastructure-services
ms.date: 06/07/2017
ms.author: gwallace
ms.openlocfilehash: 6f6ace105e84c01f525ab02938e81ce040c5c9d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-integration-between-application-gateway-and-azure-security-center"></a><span data-ttu-id="823d4-103">Översikt över integrering mellan Programgateway och Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="823d4-103">Overview of integration between Application Gateway and Azure Security Center</span></span>

<span data-ttu-id="823d4-104">Lär dig hur Programgateway och Security Center skydda resurser i ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="823d4-104">Learn how Application Gateway and Security Center help protect your web application resources.</span></span> <span data-ttu-id="823d4-105">Brandvägg för programmet gateway webbaserade program (Brandvägg) kan integreras med [Security Center](../security-center/security-center-intro.md) tooprovide en sömlös visa tooprevent identifierar och åtgärdar toothreats toounprotected webbprogram i din miljö.</span><span class="sxs-lookup"><span data-stu-id="823d4-105">Application gateway web application firewall (WAF) integrates with [Security Center](../security-center/security-center-intro.md) tooprovide a seamless view tooprevent, detect and respond toothreats toounprotected web applications in your environment.</span></span>

## <a name="overview"></a><span data-ttu-id="823d4-106">Översikt</span><span class="sxs-lookup"><span data-stu-id="823d4-106">Overview</span></span>

<span data-ttu-id="823d4-107">Programmet Gateway Brandvägg är en rekommendation i Security Center för att skydda webbprogram från kryphål och säkerhetsproblem.</span><span class="sxs-lookup"><span data-stu-id="823d4-107">Application Gateway WAF is a recommendation in Security Center for protecting web applications from exploits and vulnerabilities.</span></span> <span data-ttu-id="823d4-108">Aktiverad webbresurser som inte skyddas av Brandvägg visas i hello security center som Hög allvarlighetsgrad rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="823d4-108">Web enabled resources that are not protected by WAF show in hello security center as high severity recommendations.</span></span> <span data-ttu-id="823d4-109">Rekommendationer för web application brandväggar visas på hello **översikt** sidan under **program**.</span><span class="sxs-lookup"><span data-stu-id="823d4-109">Recommendations for web application firewalls are shown on hello **Overview** page, under **Applications**.</span></span>

![integrering med security center][1]

<span data-ttu-id="823d4-111">Om du klickar på några rekommendationer om Brandvägg för webbaserade program öppnas ett nytt blad som visar hello information om hello rekommendation.</span><span class="sxs-lookup"><span data-stu-id="823d4-111">Clicking any recommendations regarding web application firewall opens a new blade showing hello details of hello recommendation.</span></span>

## <a name="add-a-web-application-firewall-tooan-existing-resource"></a><span data-ttu-id="823d4-112">Lägg till ett program brandväggen tooan befintliga webbresurs</span><span class="sxs-lookup"><span data-stu-id="823d4-112">Add a web application firewall tooan existing resource</span></span>

<span data-ttu-id="823d4-113">Navigera för**fler tjänster** > **säkerhet + identitet** > **Security Center** och på hello **Security Center - översikt**  bladet, klickar du på **program**.</span><span class="sxs-lookup"><span data-stu-id="823d4-113">Navigate too**More Services** > **Security + Identity** > **Security Center** and on hello **Security Center - Overview** blade, click **Applications**.</span></span> <span data-ttu-id="823d4-114">På hello **Security Center - program** bladet hello tabellen innehåller en lista över program som Security Center identifieras i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="823d4-114">On hello **Security Center - Applications** blade, hello table contains a list of applications that Security Center detected in your subscription.</span></span>

![webbprogram][3]

<span data-ttu-id="823d4-116">Genom att klicka på ett webbprogram med ett allvarligt problem kan du få hello **programmet säkerhetshälsa** bladet.</span><span class="sxs-lookup"><span data-stu-id="823d4-116">By clicking on a web application with a critical issue, you get hello **Application security health** blade.</span></span> <span data-ttu-id="823d4-117">Hej webbprogram som inte skyddas av en brandvägg för webbaserade program i hello bilden nedan.</span><span class="sxs-lookup"><span data-stu-id="823d4-117">In hello image below, hello web application that is not protected by a web application firewall.</span></span> 

![webbresurser som inte skyddas][2]

<span data-ttu-id="823d4-119">Klicka på **lägga till en brandvägg för webbaserade program** under **rekommendationer** tooopen hello **lägga till en brandvägg för webbaserade program** bladet.</span><span class="sxs-lookup"><span data-stu-id="823d4-119">Click **Add a web application firewall** under **Recommendations** tooopen hello **Add a Web Application Firewall** blade.</span></span>

<span data-ttu-id="823d4-120">Om du inte har en befintlig Programgateway eller vill toocreate en ny, klickar du på **Skapa nytt** och på hello **skapa en ny Brandvägg för webbaserade program** bladet och klickar på **Microsoft - Programgateway**.</span><span class="sxs-lookup"><span data-stu-id="823d4-120">If you do not have an existing Application Gateway, or want toocreate a new one, click **Create New** and on hello **Create a new Web Application Firewall** blade, and click **Microsoft - Application Gateway**.</span></span> <span data-ttu-id="823d4-121">Detta tar dig igenom hello steg toocreate en Programgateway.</span><span class="sxs-lookup"><span data-stu-id="823d4-121">This takes you through hello steps toocreate an application gateway.</span></span> <span data-ttu-id="823d4-122">Webbprogrammet har nu lagts till som en skyddad resurs, Security Center nu spårar att den här resursen skyddas av en brandvägg för webbaserade program.</span><span class="sxs-lookup"><span data-stu-id="823d4-122">At this point, your web application is added as a protected resource, Security Center now tracks that this resource is protected by a web application firewall.</span></span> <span data-ttu-id="823d4-123">Detta lägger inte till den som en medlem för backend-poolen.</span><span class="sxs-lookup"><span data-stu-id="823d4-123">This does not add it as a backend pool member.</span></span>

<span data-ttu-id="823d4-124">Om du har en befintlig Programgateway, kan du välja den under **med befintliga lösning**</span><span class="sxs-lookup"><span data-stu-id="823d4-124">If you have an existing application gateway, you can choose it under **Use existing solution**</span></span>

![Brandvägg för webbaserade program lägger du till bladet][4]

<span data-ttu-id="823d4-126">När du lägger till en web application tooan Programgateway via Security Center inte hello resurs som medlem backend-adresspool, måste du göra det på hello programresursen gateway direkt.</span><span class="sxs-lookup"><span data-stu-id="823d4-126">Adding a web application tooan application gateway through Security Center does not add hello resource as a backend pool member, this must be done on hello application gateway resource directly.</span></span>

## <a name="add-a-resource-tooan-existing-web-application-firewall"></a><span data-ttu-id="823d4-127">Lägg till en resurs tooan befintliga Brandvägg för webbaserade program</span><span class="sxs-lookup"><span data-stu-id="823d4-127">Add a resource tooan existing web application firewall</span></span>

<span data-ttu-id="823d4-128">Navigera för**fler tjänster** > **säkerhet + identitet** > **Security Center** och på hello **Security Center - översikt**  bladet, klickar du på **partnerlösningar**.</span><span class="sxs-lookup"><span data-stu-id="823d4-128">Navigate too**More Services** > **Security + Identity** > **Security Center** and on hello **Security Center - Overview** blade, click **Partner solutions**.</span></span> <span data-ttu-id="823d4-129">Befintliga gateways för Security Center-medvetna programmet visas i hello **partnerlösningar** bladet.</span><span class="sxs-lookup"><span data-stu-id="823d4-129">Existing Security Center aware application gateways show in hello **Partner Solutions** blade.</span></span>

![partnerlösningar][7]

<span data-ttu-id="823d4-131">Klicka på **Link app** tooopen hello **länken program** bladet här du ges hello alternativ tooselect befintliga program.</span><span class="sxs-lookup"><span data-stu-id="823d4-131">Click **Link app** tooopen hello **Link Applications** blade, here you are given hello options tooselect existing applications.</span></span> <span data-ttu-id="823d4-132">Välj hello program tooprotect och på **OK**.</span><span class="sxs-lookup"><span data-stu-id="823d4-132">Choose hello applications tooprotect and click **OK**.</span></span> <span data-ttu-id="823d4-133">Detta lägger inte till hello web toohello backend programpool för hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="823d4-133">This does not add hello web application toohello backend pool of hello application gateway.</span></span> <span data-ttu-id="823d4-134">Detta anger hello resurser som en skyddad resurs så kan spåra av Security Center.</span><span class="sxs-lookup"><span data-stu-id="823d4-134">This sets hello resources as a protected resource so Security Center can track it.</span></span> <span data-ttu-id="823d4-135">tooadd hello resurs som medlem backend-adresspool, måste du göra det på hello Programgateway, från hello aktuella bladet kan du klicka på **lösning konsolen** toobe tas toohello programresursen gateway där du kan lägga till hello web toohello backend programpoolen.</span><span class="sxs-lookup"><span data-stu-id="823d4-135">tooadd hello resource as a backend pool member, this must be done on hello application gateway, from hello current blade you can click **Solution console** toobe taken toohello application gateway resource where you can add hello web application toohello backend pool.</span></span>

![partner solutions program][6]

## <a name="finalize-configuration"></a><span data-ttu-id="823d4-137">Slutför konfiguration</span><span class="sxs-lookup"><span data-stu-id="823d4-137">Finalize configuration</span></span>

<span data-ttu-id="823d4-138">Security Center spårar program till tooan Programgateway som en skyddad resurs.</span><span class="sxs-lookup"><span data-stu-id="823d4-138">Security Center tracks applications added tooan application gateway as a protected resource.</span></span>  <span data-ttu-id="823d4-139">Den övervakar hello hälsotillståndet för den här resursen och garanterar att den skyddas av en Programgateway.</span><span class="sxs-lookup"><span data-stu-id="823d4-139">It monitors hello health of this resource and ensures that it is protected by an application gateway.</span></span> <span data-ttu-id="823d4-140">hello nästa steg är tooadd hello privata IP-, offentlig IP-adress eller nätverkskort på din virtuella toohello serverdelspool för hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="823d4-140">hello next step is tooadd hello private IP, public IP, or NIC of your virtual machine toohello backend pool of hello application gateway.</span></span> <span data-ttu-id="823d4-141">Tills detta görs en ytterligare rekommendation av **Slutför programskydd** visas förrän hello resursen har lagts till.</span><span class="sxs-lookup"><span data-stu-id="823d4-141">Until this is done an additional recommendation of **Finalize application protection** is shown until hello resource is added.</span></span>

![Brandvägg för webbaserade program lägger du till bladet][5]

## <a name="security-alerts"></a><span data-ttu-id="823d4-143">Säkerhetsaviseringar</span><span class="sxs-lookup"><span data-stu-id="823d4-143">Security Alerts</span></span>

<span data-ttu-id="823d4-144">Navigera för i Security Center**identifiering** > **säkerhetsaviseringar**.</span><span class="sxs-lookup"><span data-stu-id="823d4-144">Within Security Center navigate too**DETECTION** > **Security Alerts**.</span></span>  <span data-ttu-id="823d4-145">Här hittar du Brandvägg aviseringar för din programgatewayer.</span><span class="sxs-lookup"><span data-stu-id="823d4-145">Here you find WAF alerts for your application gateways.</span></span> <span data-ttu-id="823d4-146">Aviseringar är fördelade på Brandvägg regeln.</span><span class="sxs-lookup"><span data-stu-id="823d4-146">Alerts are broken down by WAF rule.</span></span>

![säkerhetsaviseringar][8]

<span data-ttu-id="823d4-148">Klicka på en regel ger en lista över aviseringar för den specifika regeln Brandvägg.</span><span class="sxs-lookup"><span data-stu-id="823d4-148">Clicking an rule will provide a list of alerts for that specific WAF rule.</span></span> <span data-ttu-id="823d4-149">Varje avisering visas ytterligare information på hello söka efter.</span><span class="sxs-lookup"><span data-stu-id="823d4-149">Each alert shows additional details on hello finding.</span></span> <span data-ttu-id="823d4-150">hello information innehåller en länk toohello application gateway.</span><span class="sxs-lookup"><span data-stu-id="823d4-150">hello details provide a link toohello application gateway.</span></span>
 
![aviseringsinformation][9]

## <a name="next-steps"></a><span data-ttu-id="823d4-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="823d4-152">Next steps</span></span>

<span data-ttu-id="823d4-153">toolearn hur Brandvägg för tooenable webbaserade program på en befintlig Programgateway finns [skapa eller uppdatera en Azure Programgateway med Brandvägg för webbaserade program](application-gateway-web-application-firewall-portal.md#add-web-application-firewall-to-an-existing-application-gateway)</span><span class="sxs-lookup"><span data-stu-id="823d4-153">toolearn how tooenable web application firewall on an existing application gateway, visit [Create or update an Azure Application Gateway with web application firewall](application-gateway-web-application-firewall-portal.md#add-web-application-firewall-to-an-existing-application-gateway)</span></span>

[1]: ./media/application-gateway-integration-security-center/figure1.png
[2]: ./media/application-gateway-integration-security-center/figure2.png
[3]: ./media/application-gateway-integration-security-center/figure3.png
[4]: ./media/application-gateway-integration-security-center/figure4.png
[5]: ./media/application-gateway-integration-security-center/figure5.png
[6]: ./media/application-gateway-integration-security-center/figure6.png
[7]: ./media/application-gateway-integration-security-center/figure7.png
[8]: ./media/application-gateway-integration-security-center/securitycenter.png
[9]: ./media/application-gateway-integration-security-center/figure9.png