---
title: Programmet Gateway-integration med Azure Security Center | Microsoft Docs
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
ms.openlocfilehash: 737cdff3140be68cf9d6d396b470dd09c65c52f2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="overview-of-integration-between-application-gateway-and-azure-security-center"></a><span data-ttu-id="54471-103">Översikt över integrering mellan Programgateway och Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="54471-103">Overview of integration between Application Gateway and Azure Security Center</span></span>

<span data-ttu-id="54471-104">Lär dig hur Programgateway och Security Center skydda resurser i ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="54471-104">Learn how Application Gateway and Security Center help protect your web application resources.</span></span> <span data-ttu-id="54471-105">Brandvägg för programmet gateway webbaserade program (Brandvägg) kan integreras med [Security Center](../security-center/security-center-intro.md) för att ge en smidig vy för att förhindra, identifiera och åtgärda hot till oskyddade webbprogram i din miljö.</span><span class="sxs-lookup"><span data-stu-id="54471-105">Application gateway web application firewall (WAF) integrates with [Security Center](../security-center/security-center-intro.md) to provide a seamless view to prevent, detect and respond to threats to unprotected web applications in your environment.</span></span>

## <a name="overview"></a><span data-ttu-id="54471-106">Översikt</span><span class="sxs-lookup"><span data-stu-id="54471-106">Overview</span></span>

<span data-ttu-id="54471-107">Programmet Gateway Brandvägg är en rekommendation i Security Center för att skydda webbprogram från kryphål och säkerhetsproblem.</span><span class="sxs-lookup"><span data-stu-id="54471-107">Application Gateway WAF is a recommendation in Security Center for protecting web applications from exploits and vulnerabilities.</span></span> <span data-ttu-id="54471-108">Aktiverad webbresurser som inte skyddas av Brandvägg visas i security center som Hög allvarlighetsgrad rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="54471-108">Web enabled resources that are not protected by WAF show in the security center as high severity recommendations.</span></span> <span data-ttu-id="54471-109">Rekommendationer för web application brandväggar visas på den **översikt** sidan under **program**.</span><span class="sxs-lookup"><span data-stu-id="54471-109">Recommendations for web application firewalls are shown on the **Overview** page, under **Applications**.</span></span>

![integrering med security center][1]

<span data-ttu-id="54471-111">Klicka på några rekommendationer om Brandvägg för webbaserade program öppnas ett nytt blad som visar information om rekommendationen.</span><span class="sxs-lookup"><span data-stu-id="54471-111">Clicking any recommendations regarding web application firewall opens a new blade showing the details of the recommendation.</span></span>

## <a name="add-a-web-application-firewall-to-an-existing-resource"></a><span data-ttu-id="54471-112">Lägg till en brandvägg för webbaserade program i en befintlig resurs</span><span class="sxs-lookup"><span data-stu-id="54471-112">Add a web application firewall to an existing resource</span></span>

<span data-ttu-id="54471-113">Gå till **fler tjänster** > **säkerhet + identitet** > **Security Center** och på den **Security Center - översikt**  bladet, klickar du på **program**.</span><span class="sxs-lookup"><span data-stu-id="54471-113">Navigate to **More Services** > **Security + Identity** > **Security Center** and on the **Security Center - Overview** blade, click **Applications**.</span></span> <span data-ttu-id="54471-114">På den **Security Center - program** bladet tabellen innehåller en lista över program som Security Center identifieras i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="54471-114">On the **Security Center - Applications** blade, the table contains a list of applications that Security Center detected in your subscription.</span></span>

![webbprogram][3]

<span data-ttu-id="54471-116">Genom att klicka på ett webbprogram med ett allvarligt problem kan du hämta den **programmet säkerhetshälsa** bladet.</span><span class="sxs-lookup"><span data-stu-id="54471-116">By clicking on a web application with a critical issue, you get the **Application security health** blade.</span></span> <span data-ttu-id="54471-117">I bilden nedan, webbprogram som inte skyddas av en brandvägg för webbaserade program.</span><span class="sxs-lookup"><span data-stu-id="54471-117">In the image below, the web application that is not protected by a web application firewall.</span></span> 

![webbresurser som inte skyddas][2]

<span data-ttu-id="54471-119">Klicka på **lägga till en brandvägg för webbaserade program** under **rekommendationer** att öppna den **lägga till en brandvägg för webbaserade program** bladet.</span><span class="sxs-lookup"><span data-stu-id="54471-119">Click **Add a web application firewall** under **Recommendations** to open the **Add a Web Application Firewall** blade.</span></span>

<span data-ttu-id="54471-120">Om du inte har en befintlig Gateway för programmet, eller om du vill skapa en ny, klickar du på **Skapa nytt** och på den **skapa en ny Brandvägg för webbaserade program** bladet och klickar på **Microsoft - program Gateway**.</span><span class="sxs-lookup"><span data-stu-id="54471-120">If you do not have an existing Application Gateway, or want to create a new one, click **Create New** and on the **Create a new Web Application Firewall** blade, and click **Microsoft - Application Gateway**.</span></span> <span data-ttu-id="54471-121">Detta tar dig igenom stegen för att skapa en Programgateway.</span><span class="sxs-lookup"><span data-stu-id="54471-121">This takes you through the steps to create an application gateway.</span></span> <span data-ttu-id="54471-122">Webbprogrammet har nu lagts till som en skyddad resurs, Security Center nu spårar att den här resursen skyddas av en brandvägg för webbaserade program.</span><span class="sxs-lookup"><span data-stu-id="54471-122">At this point, your web application is added as a protected resource, Security Center now tracks that this resource is protected by a web application firewall.</span></span> <span data-ttu-id="54471-123">Detta lägger inte till den som en medlem för backend-poolen.</span><span class="sxs-lookup"><span data-stu-id="54471-123">This does not add it as a backend pool member.</span></span>

<span data-ttu-id="54471-124">Om du har en befintlig Programgateway, kan du välja den under **med befintliga lösning**</span><span class="sxs-lookup"><span data-stu-id="54471-124">If you have an existing application gateway, you can choose it under **Use existing solution**</span></span>

![Brandvägg för webbaserade program lägger du till bladet][4]

<span data-ttu-id="54471-126">Lägger till ett webbprogram till en Programgateway via Security Center inte lägger till resursen som medlem backend-adresspool, måste du göra det på gateway-programresurs direkt.</span><span class="sxs-lookup"><span data-stu-id="54471-126">Adding a web application to an application gateway through Security Center does not add the resource as a backend pool member, this must be done on the application gateway resource directly.</span></span>

## <a name="add-a-resource-to-an-existing-web-application-firewall"></a><span data-ttu-id="54471-127">Lägg till en resurs i en befintlig Brandvägg för webbaserade program</span><span class="sxs-lookup"><span data-stu-id="54471-127">Add a resource to an existing web application firewall</span></span>

<span data-ttu-id="54471-128">Gå till **fler tjänster** > **säkerhet + identitet** > **Security Center** och på den **Security Center - översikt**  bladet, klickar du på **partnerlösningar**.</span><span class="sxs-lookup"><span data-stu-id="54471-128">Navigate to **More Services** > **Security + Identity** > **Security Center** and on the **Security Center - Overview** blade, click **Partner solutions**.</span></span> <span data-ttu-id="54471-129">Befintliga gateways för Security Center-medvetna programmet visas i den **partnerlösningar** bladet.</span><span class="sxs-lookup"><span data-stu-id="54471-129">Existing Security Center aware application gateways show in the **Partner Solutions** blade.</span></span>

![partnerlösningar][7]

<span data-ttu-id="54471-131">Klicka på **Link app** att öppna den **länken program** bladet här du ges alternativ för befintliga program.</span><span class="sxs-lookup"><span data-stu-id="54471-131">Click **Link app** to open the **Link Applications** blade, here you are given the options to select existing applications.</span></span> <span data-ttu-id="54471-132">Välj program att skydda och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="54471-132">Choose the applications to protect and click **OK**.</span></span> <span data-ttu-id="54471-133">Detta lägger inte till webbprogrammet till serverdelspoolen för programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="54471-133">This does not add the web application to the backend pool of the application gateway.</span></span> <span data-ttu-id="54471-134">Anger resurserna som en skyddad resurs så kan spåra av Security Center.</span><span class="sxs-lookup"><span data-stu-id="54471-134">This sets the resources as a protected resource so Security Center can track it.</span></span> <span data-ttu-id="54471-135">Om du vill lägga till resursen som medlem i serverdelen poolen detta måste göras på Programgateway från det aktuella bladet kan du klicka på **lösning konsolen** för att programmet gatewayresursen där du kan lägga till webbprogrammet till den serverdelspool.</span><span class="sxs-lookup"><span data-stu-id="54471-135">To add the resource as a backend pool member, this must be done on the application gateway, from the current blade you can click **Solution console** to be taken to the application gateway resource where you can add the web application to the backend pool.</span></span>

![partner solutions program][6]

## <a name="finalize-configuration"></a><span data-ttu-id="54471-137">Slutför konfiguration</span><span class="sxs-lookup"><span data-stu-id="54471-137">Finalize configuration</span></span>

<span data-ttu-id="54471-138">Security Center spårar program som har lagts till i en Programgateway som en skyddad resurs.</span><span class="sxs-lookup"><span data-stu-id="54471-138">Security Center tracks applications added to an application gateway as a protected resource.</span></span>  <span data-ttu-id="54471-139">Den övervakar tillståndet för den här resursen och garanterar att den skyddas av en Programgateway.</span><span class="sxs-lookup"><span data-stu-id="54471-139">It monitors the health of this resource and ensures that it is protected by an application gateway.</span></span> <span data-ttu-id="54471-140">Nästa steg är att lägga till privata IP-, offentlig IP-adress eller nätverkskort på den virtuella datorn till serverdelspoolen för programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="54471-140">The next step is to add the private IP, public IP, or NIC of your virtual machine to the backend pool of the application gateway.</span></span> <span data-ttu-id="54471-141">Tills detta görs en ytterligare rekommendation av **Slutför programskydd** visas förrän resursen har lagts till.</span><span class="sxs-lookup"><span data-stu-id="54471-141">Until this is done an additional recommendation of **Finalize application protection** is shown until the resource is added.</span></span>

![Brandvägg för webbaserade program lägger du till bladet][5]

## <a name="security-alerts"></a><span data-ttu-id="54471-143">Säkerhetsaviseringar</span><span class="sxs-lookup"><span data-stu-id="54471-143">Security Alerts</span></span>

<span data-ttu-id="54471-144">Navigera till i Security Center **identifiering** > **säkerhetsaviseringar**.</span><span class="sxs-lookup"><span data-stu-id="54471-144">Within Security Center navigate to **DETECTION** > **Security Alerts**.</span></span>  <span data-ttu-id="54471-145">Här hittar du Brandvägg aviseringar för din programgatewayer.</span><span class="sxs-lookup"><span data-stu-id="54471-145">Here you find WAF alerts for your application gateways.</span></span> <span data-ttu-id="54471-146">Aviseringar är fördelade på Brandvägg regeln.</span><span class="sxs-lookup"><span data-stu-id="54471-146">Alerts are broken down by WAF rule.</span></span>

![säkerhetsaviseringar][8]

<span data-ttu-id="54471-148">Klicka på en regel ger en lista över aviseringar för den specifika regeln Brandvägg.</span><span class="sxs-lookup"><span data-stu-id="54471-148">Clicking an rule will provide a list of alerts for that specific WAF rule.</span></span> <span data-ttu-id="54471-149">Varje avisering visar ytterligare information om den söka efter.</span><span class="sxs-lookup"><span data-stu-id="54471-149">Each alert shows additional details on the finding.</span></span> <span data-ttu-id="54471-150">Informationen innehåller en länk till programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="54471-150">The details provide a link to the application gateway.</span></span>
 
![aviseringsinformation][9]

## <a name="next-steps"></a><span data-ttu-id="54471-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="54471-152">Next steps</span></span>

<span data-ttu-id="54471-153">Ta reda på hur du aktiverar Brandvägg för webbaserade program på en befintlig Programgateway [skapa eller uppdatera en Azure Programgateway med Brandvägg för webbaserade program](application-gateway-web-application-firewall-portal.md#add-web-application-firewall-to-an-existing-application-gateway)</span><span class="sxs-lookup"><span data-stu-id="54471-153">To learn how to enable web application firewall on an existing application gateway, visit [Create or update an Azure Application Gateway with web application firewall](application-gateway-web-application-firewall-portal.md#add-web-application-firewall-to-an-existing-application-gateway)</span></span>

[1]: ./media/application-gateway-integration-security-center/figure1.png
[2]: ./media/application-gateway-integration-security-center/figure2.png
[3]: ./media/application-gateway-integration-security-center/figure3.png
[4]: ./media/application-gateway-integration-security-center/figure4.png
[5]: ./media/application-gateway-integration-security-center/figure5.png
[6]: ./media/application-gateway-integration-security-center/figure6.png
[7]: ./media/application-gateway-integration-security-center/figure7.png
[8]: ./media/application-gateway-integration-security-center/securitycenter.png
[9]: ./media/application-gateway-integration-security-center/figure9.png