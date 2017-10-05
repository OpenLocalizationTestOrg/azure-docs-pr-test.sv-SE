---
title: "Lägg till en brandvägg för webbaserade program i Azure Security Center | Microsoft Docs"
description: "Det här dokumentet beskrivs hur du implementerar Azure Security Center-rekommendationerna ** lägga till en web application firewall ** och ** slutför programmet skydd **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 8f56139a-4466-48ac-90fb-86d002cf8242
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2017
ms.author: terrylan
ms.openlocfilehash: d04a07237029953d8a9b20704d85e852ce45d867
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="add-a-web-application-firewall-in-azure-security-center"></a><span data-ttu-id="28543-103">Lägg till en brandvägg för webbaserade program i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="28543-103">Add a web application firewall in Azure Security Center</span></span>
<span data-ttu-id="28543-104">Azure Security Center kan rekommenderar att du lägger till en brandvägg för webbaserade program (Brandvägg) från en Microsoft-partner att skydda dina webbprogram.</span><span class="sxs-lookup"><span data-stu-id="28543-104">Azure Security Center may recommend that you add a web application firewall (WAF) from a Microsoft partner to secure your web applications.</span></span> <span data-ttu-id="28543-105">Det här dokumentet vägleder dig genom ett exempel på hur du använder den här rekommendationen.</span><span class="sxs-lookup"><span data-stu-id="28543-105">This document walks you through an example of how to apply this recommendation.</span></span>

<span data-ttu-id="28543-106">En Brandvägg rekommendation visas för alla offentliga Internetriktade IP-adresser (instans nivå IP eller Load belastningsutjämnade IP) som har en nätverkssäkerhetsgrupp med öppna webbplats för inkommande portar (80,443).</span><span class="sxs-lookup"><span data-stu-id="28543-106">A WAF recommendation is shown for any public facing IP (either Instance Level IP or Load Balanced IP) that has an associated network security group with open inbound web ports (80,443).</span></span>

<span data-ttu-id="28543-107">Security Center rekommenderar att du etablerar en Brandvägg för att skydda mot attacker målobjekt för webbaserade program på virtuella datorer samt på Apptjänst-miljö.</span><span class="sxs-lookup"><span data-stu-id="28543-107">Security Center recommends that you provision a WAF to help defend against attacks targeting your web applications on virtual machines and on App Service Environment.</span></span> <span data-ttu-id="28543-108">En App Service miljö (ASE) är en [Premium](https://azure.microsoft.com/pricing/details/app-service/) service plan alternativet för Azure App Service som tillhandahåller en helt isolerad och dedikerad miljö för Azure App Service-program som körs på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="28543-108">An App Service Environment (ASE) is a [Premium](https://azure.microsoft.com/pricing/details/app-service/) service plan option of Azure App Service that provides a fully isolated and dedicated environment for securely running Azure App Service apps.</span></span> <span data-ttu-id="28543-109">Mer information om ASE finns i [dokumentationen till App Service-miljö](../app-service/app-service-app-service-environments-readme.md).</span><span class="sxs-lookup"><span data-stu-id="28543-109">To learn more about ASE, see the [App Service Environment Documentation](../app-service/app-service-app-service-environments-readme.md).</span></span>

> [!NOTE]
> <span data-ttu-id="28543-110">I det här dokumentet beskrivs tjänsten genom en exempeldistribution.</span><span class="sxs-lookup"><span data-stu-id="28543-110">This document introduces the service by using an example deployment.</span></span>  <span data-ttu-id="28543-111">Det här dokumentet är inte en stegvis guide.</span><span class="sxs-lookup"><span data-stu-id="28543-111">This document is not a step-by-step guide.</span></span>
>
>

## <a name="implement-the-recommendation"></a><span data-ttu-id="28543-112">Implementera rekommendationen</span><span class="sxs-lookup"><span data-stu-id="28543-112">Implement the recommendation</span></span>
1. <span data-ttu-id="28543-113">I den **rekommendationer** bladet väljer **säkra webbprogram med hjälp av Brandvägg för webbaserade program**.</span><span class="sxs-lookup"><span data-stu-id="28543-113">In the **Recommendations** blade, select **Secure web application using web application firewall**.</span></span>
   <span data-ttu-id="28543-114">![Skydda webb-program][1]</span><span class="sxs-lookup"><span data-stu-id="28543-114">![Secure web Application][1]</span></span>
2. <span data-ttu-id="28543-115">I den **skydda ditt webbprogram med hjälp av Brandvägg för webbaserade program** bladet Välj ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="28543-115">In the **Secure your web applications using web application firewall** blade, select a web application.</span></span> <span data-ttu-id="28543-116">Den **lägga till en brandvägg för webbaserade program** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="28543-116">The **Add a Web Application Firewall** blade opens.</span></span>
   <span data-ttu-id="28543-117">![Lägga till en brandvägg för webbappar][2]</span><span class="sxs-lookup"><span data-stu-id="28543-117">![Add a web application firewall][2]</span></span>
3. <span data-ttu-id="28543-118">Du kan välja att använda en befintlig Brandvägg för webbaserade program om det är tillgängligt eller du kan skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="28543-118">You can choose to use an existing web application firewall if available or you can create a new one.</span></span> <span data-ttu-id="28543-119">I det här exemplet finns det inga befintliga WAFs så skapar vi en Brandvägg.</span><span class="sxs-lookup"><span data-stu-id="28543-119">In this example, there are no existing WAFs available so we create a WAF.</span></span>
4. <span data-ttu-id="28543-120">Välj en lösning för att skapa en Brandvägg, i listan med integrerade partnerleverantörer.</span><span class="sxs-lookup"><span data-stu-id="28543-120">To create a WAF, select a solution from the list of integrated partners.</span></span> <span data-ttu-id="28543-121">I det här exemplet väljer vi **Barracuda Brandvägg för webbaserade program**.</span><span class="sxs-lookup"><span data-stu-id="28543-121">In this example, we select **Barracuda Web Application Firewall**.</span></span>
5. <span data-ttu-id="28543-122">Den **Barracuda Brandvägg för webbaserade program** öppnas ett blad med information om Partnerlösningen.</span><span class="sxs-lookup"><span data-stu-id="28543-122">The **Barracuda Web Application Firewall** blade opens providing you information about the partner solution.</span></span> <span data-ttu-id="28543-123">Välj **skapa** i bladet information.</span><span class="sxs-lookup"><span data-stu-id="28543-123">Select **Create** in the information blade.</span></span>

   ![Brandväggen information bladet][3]

6. <span data-ttu-id="28543-125">Den **ny Brandvägg för webbaserade program** blad öppnas, där du kan utföra **VM-konfiguration** steg och ange **Brandvägg Information**.</span><span class="sxs-lookup"><span data-stu-id="28543-125">The **New Web Application Firewall** blade opens, where you can perform **VM Configuration** steps and provide **WAF Information**.</span></span> <span data-ttu-id="28543-126">Välj **VM-konfiguration**.</span><span class="sxs-lookup"><span data-stu-id="28543-126">Select **VM Configuration**.</span></span>
7. <span data-ttu-id="28543-127">I den **VM-konfiguration** bladet anger du information som krävs för att få igång den virtuella datorn som kör Brandvägg.</span><span class="sxs-lookup"><span data-stu-id="28543-127">In the **VM Configuration** blade, you enter information required to spin up the virtual machine that runs the WAF.</span></span>
   <span data-ttu-id="28543-128">![VM-konfiguration][4]</span><span class="sxs-lookup"><span data-stu-id="28543-128">![VM configuration][4]</span></span>
8. <span data-ttu-id="28543-129">Gå tillbaka till den **ny Brandvägg för webbaserade program** och välj **Brandvägg Information**.</span><span class="sxs-lookup"><span data-stu-id="28543-129">Return to the **New Web Application Firewall** blade and select **WAF Information**.</span></span> <span data-ttu-id="28543-130">I den **Brandvägg Information** bladet du konfigurera Brandvägg sig själv.</span><span class="sxs-lookup"><span data-stu-id="28543-130">In the **WAF Information** blade, you configure the WAF itself.</span></span> <span data-ttu-id="28543-131">Steg 7 kan du konfigurera den virtuella datorn som Brandvägg körs på och steg 8 kan du etablera Brandvägg sig själv.</span><span class="sxs-lookup"><span data-stu-id="28543-131">Step 7 allows you to configure the virtual machine on which the WAF runs and step 8 enables you to provision the WAF itself.</span></span>

## <a name="finalize-application-protection"></a><span data-ttu-id="28543-132">Slutför programskydd</span><span class="sxs-lookup"><span data-stu-id="28543-132">Finalize application protection</span></span>
1. <span data-ttu-id="28543-133">Gå tillbaka till den **rekommendationer** bladet.</span><span class="sxs-lookup"><span data-stu-id="28543-133">Return to the **Recommendations** blade.</span></span> <span data-ttu-id="28543-134">En ny post skapades när du har skapat Brandvägg, kallas **Slutför programskydd**.</span><span class="sxs-lookup"><span data-stu-id="28543-134">A new entry was generated after you created the WAF, called **Finalize application protection**.</span></span> <span data-ttu-id="28543-135">Den här posten kan du vet att du måste slutföra processen för att koppla samman Brandvägg i det virtuella Azure-nätverket så att den kan skydda programmet.</span><span class="sxs-lookup"><span data-stu-id="28543-135">This entry lets you know that you need to complete the process of actually wiring up the WAF within the Azure Virtual Network so that it can protect the application.</span></span>

   ![Slutför programskydd][5]

2. <span data-ttu-id="28543-137">Välj **Slutför programskydd**.</span><span class="sxs-lookup"><span data-stu-id="28543-137">Select **Finalize application protection**.</span></span> <span data-ttu-id="28543-138">Ett nytt blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="28543-138">A new blade opens.</span></span> <span data-ttu-id="28543-139">Du kan se att det finns ett program som måste ha sin trafik dirigeras om.</span><span class="sxs-lookup"><span data-stu-id="28543-139">You can see that there is a web application that needs to have its traffic rerouted.</span></span>
3. <span data-ttu-id="28543-140">Välj webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="28543-140">Select the web application.</span></span> <span data-ttu-id="28543-141">Då öppnas ett blad som ger steg för att slutföra inställningarna för webbprogrammet brandväggen.</span><span class="sxs-lookup"><span data-stu-id="28543-141">A blade opens that gives you steps for finalizing the web application firewall setup.</span></span> <span data-ttu-id="28543-142">Utför stegen och välj sedan **begränsa trafik**.</span><span class="sxs-lookup"><span data-stu-id="28543-142">Complete the steps, and then select **Restrict traffic**.</span></span> <span data-ttu-id="28543-143">Security Center sedan gör kablar upp.</span><span class="sxs-lookup"><span data-stu-id="28543-143">Security Center then does the wiring-up for you.</span></span>

   ![Begränsa trafik][6]

> [!NOTE]
> <span data-ttu-id="28543-145">Du kan skydda flera webbprogram i Security Center genom att lägga till dessa program till din befintliga Brandvägg-distributioner.</span><span class="sxs-lookup"><span data-stu-id="28543-145">You can protect multiple web applications in Security Center by adding these applications to your existing WAF deployments.</span></span>
>
>

<span data-ttu-id="28543-146">Loggar från den Brandvägg är nu fullständigt integrerat.</span><span class="sxs-lookup"><span data-stu-id="28543-146">The logs from that WAF are now fully integrated.</span></span> <span data-ttu-id="28543-147">Security Center kan börja samla in och analysera loggarna så att den kan ansluta viktiga säkerhetsaviseringar du automatiskt.</span><span class="sxs-lookup"><span data-stu-id="28543-147">Security Center can start automatically gathering and analyzing the logs so that it can surface important security alerts to you.</span></span>

## <a name="next-steps"></a><span data-ttu-id="28543-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="28543-148">Next steps</span></span>
<span data-ttu-id="28543-149">Det här dokumentet visar dig hur du implementerar Security Center-rekommendationen ”Lägg till ett webbprogram”.</span><span class="sxs-lookup"><span data-stu-id="28543-149">This document showed you how to implement the Security Center recommendation "Add a web application."</span></span> <span data-ttu-id="28543-150">Mer information om hur du konfigurerar en brandvägg för webbaserade program finns i följande avsnitt:</span><span class="sxs-lookup"><span data-stu-id="28543-150">To learn more about configuring a web application firewall, see the following:</span></span>

* [<span data-ttu-id="28543-151">Konfigurera en brandvägg för webbaserade program (Brandvägg) för Apptjänst-miljö</span><span class="sxs-lookup"><span data-stu-id="28543-151">Configuring a Web Application Firewall (WAF) for App Service Environment</span></span>](../app-service-web/app-service-app-service-environment-web-application-firewall.md)

<span data-ttu-id="28543-152">I följande avsnitt kan du lära dig mer om Security Center:</span><span class="sxs-lookup"><span data-stu-id="28543-152">To learn more about Security Center, see the following:</span></span>

* <span data-ttu-id="28543-153">[Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Här får du lära dig hur du ställer in säkerhetsprinciper för prenumerationer och resursgrupper i Azure.</span><span class="sxs-lookup"><span data-stu-id="28543-153">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how to configure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="28543-154">[Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md) – Lär dig att övervaka hälsotillståndet hos dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="28543-154">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how to monitor the health of your Azure resources.</span></span>
* <span data-ttu-id="28543-155">[Hantera och åtgärda säkerhetsaviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Här får du lära dig hur du hanterar och åtgärdar säkerhetsaviseringar.</span><span class="sxs-lookup"><span data-stu-id="28543-155">[Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how to manage and respond to security alerts.</span></span>
* <span data-ttu-id="28543-156">[Hantera säkerhetsrekommendationer i Azure Security Center](security-center-recommendations.md) – Lär dig rekommendationer för att skydda dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="28543-156">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="28543-157">[Vanliga frågor och svar om Azure Security Center](security-center-faq.md) – Här hittar du vanliga frågor och svar om tjänsten.</span><span class="sxs-lookup"><span data-stu-id="28543-157">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using the service.</span></span>
* <span data-ttu-id="28543-158">[Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) --hittar du blogginlägg om säkerhet och Azure kompatibilitet.</span><span class="sxs-lookup"><span data-stu-id="28543-158">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Find blog posts about Azure security and compliance.</span></span>

<!--Image references-->
[1]: ./media/security-center-add-web-application-firewall/secure-web-application.png
[2]:./media/security-center-add-web-application-firewall/add-a-waf.png
[3]: ./media/security-center-add-web-application-firewall/info-blade.png
[4]: ./media/security-center-add-web-application-firewall/select-vm-config.png
[5]: ./media/security-center-add-web-application-firewall/finalize-waf.png
[6]: ./media/security-center-add-web-application-firewall/restrict-traffic.png
