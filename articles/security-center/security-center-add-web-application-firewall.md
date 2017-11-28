---
title: "aaaAdd en brandvägg för webbaserade program i Azure Security Center | Microsoft Docs"
description: "Det här dokumentet beskrivs hur tooimplement hello Azure Security Center rekommendationer ** lägga till en web application firewall ** och ** slutför programmet skydd **."
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
ms.openlocfilehash: bff0aa2a5c6e0dde23396f93de52abe295053581
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-web-application-firewall-in-azure-security-center"></a><span data-ttu-id="ac9e6-103">Lägg till en brandvägg för webbaserade program i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="ac9e6-103">Add a web application firewall in Azure Security Center</span></span>
<span data-ttu-id="ac9e6-104">Azure Security Center kan rekommenderar att du lägger till en brandvägg för webbaserade program (Brandvägg) från en Microsoft-partner toosecure webbaserade program.</span><span class="sxs-lookup"><span data-stu-id="ac9e6-104">Azure Security Center may recommend that you add a web application firewall (WAF) from a Microsoft partner toosecure your web applications.</span></span> <span data-ttu-id="ac9e6-105">Det här dokumentet vägleder dig genom ett exempel på hur tooapply den här rekommendationen.</span><span class="sxs-lookup"><span data-stu-id="ac9e6-105">This document walks you through an example of how tooapply this recommendation.</span></span>

<span data-ttu-id="ac9e6-106">En Brandvägg rekommendation visas för alla offentliga Internetriktade IP-adresser (instans nivå IP eller Load belastningsutjämnade IP) som har en nätverkssäkerhetsgrupp med öppna webbplats för inkommande portar (80,443).</span><span class="sxs-lookup"><span data-stu-id="ac9e6-106">A WAF recommendation is shown for any public facing IP (either Instance Level IP or Load Balanced IP) that has an associated network security group with open inbound web ports (80,443).</span></span>

<span data-ttu-id="ac9e6-107">Security Center rekommenderar att etablera en Brandvägg toohelp skydd mot angrepp målobjekt för webbaserade program på virtuella datorer samt på Apptjänst-miljö.</span><span class="sxs-lookup"><span data-stu-id="ac9e6-107">Security Center recommends that you provision a WAF toohelp defend against attacks targeting your web applications on virtual machines and on App Service Environment.</span></span> <span data-ttu-id="ac9e6-108">En App Service miljö (ASE) är en [Premium](https://azure.microsoft.com/pricing/details/app-service/) service plan alternativet för Azure App Service som tillhandahåller en helt isolerad och dedikerad miljö för Azure App Service-program som körs på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="ac9e6-108">An App Service Environment (ASE) is a [Premium](https://azure.microsoft.com/pricing/details/app-service/) service plan option of Azure App Service that provides a fully isolated and dedicated environment for securely running Azure App Service apps.</span></span> <span data-ttu-id="ac9e6-109">toolearn mer om ASE, se hello [dokumentationen till App Service-miljö](../app-service/app-service-app-service-environments-readme.md).</span><span class="sxs-lookup"><span data-stu-id="ac9e6-109">toolearn more about ASE, see hello [App Service Environment Documentation](../app-service/app-service-app-service-environments-readme.md).</span></span>

> [!NOTE]
> <span data-ttu-id="ac9e6-110">Det här dokumentet introducerar hello-tjänsten med hjälp av ett exempel på distribution.</span><span class="sxs-lookup"><span data-stu-id="ac9e6-110">This document introduces hello service by using an example deployment.</span></span>  <span data-ttu-id="ac9e6-111">Det här dokumentet är inte en stegvis guide.</span><span class="sxs-lookup"><span data-stu-id="ac9e6-111">This document is not a step-by-step guide.</span></span>
>
>

## <a name="implement-hello-recommendation"></a><span data-ttu-id="ac9e6-112">Implementera hello rekommendation</span><span class="sxs-lookup"><span data-stu-id="ac9e6-112">Implement hello recommendation</span></span>
1. <span data-ttu-id="ac9e6-113">I hello **rekommendationer** bladet väljer **säkra webbprogram med hjälp av Brandvägg för webbaserade program**.</span><span class="sxs-lookup"><span data-stu-id="ac9e6-113">In hello **Recommendations** blade, select **Secure web application using web application firewall**.</span></span>
   <span data-ttu-id="ac9e6-114">![Skydda webb-program][1]</span><span class="sxs-lookup"><span data-stu-id="ac9e6-114">![Secure web Application][1]</span></span>
2. <span data-ttu-id="ac9e6-115">I hello **skydda ditt webbprogram med hjälp av Brandvägg för webbaserade program** bladet Välj ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="ac9e6-115">In hello **Secure your web applications using web application firewall** blade, select a web application.</span></span> <span data-ttu-id="ac9e6-116">Hej **lägga till en brandvägg för webbaserade program** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="ac9e6-116">hello **Add a Web Application Firewall** blade opens.</span></span>
   <span data-ttu-id="ac9e6-117">![Lägga till en brandvägg för webbappar][2]</span><span class="sxs-lookup"><span data-stu-id="ac9e6-117">![Add a web application firewall][2]</span></span>
3. <span data-ttu-id="ac9e6-118">Du kan välja toouse en befintlig Brandvägg för webbaserade program om det är tillgängligt eller du kan skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="ac9e6-118">You can choose toouse an existing web application firewall if available or you can create a new one.</span></span> <span data-ttu-id="ac9e6-119">I det här exemplet finns det inga befintliga WAFs så skapar vi en Brandvägg.</span><span class="sxs-lookup"><span data-stu-id="ac9e6-119">In this example, there are no existing WAFs available so we create a WAF.</span></span>
4. <span data-ttu-id="ac9e6-120">toocreate en Brandvägg, markera en lösning hello listan med integrerade partnerleverantörer.</span><span class="sxs-lookup"><span data-stu-id="ac9e6-120">toocreate a WAF, select a solution from hello list of integrated partners.</span></span> <span data-ttu-id="ac9e6-121">I det här exemplet väljer vi **Barracuda Brandvägg för webbaserade program**.</span><span class="sxs-lookup"><span data-stu-id="ac9e6-121">In this example, we select **Barracuda Web Application Firewall**.</span></span>
5. <span data-ttu-id="ac9e6-122">Hej **Barracuda Brandvägg för webbaserade program** öppnas ett blad med information om hello partnerlösning.</span><span class="sxs-lookup"><span data-stu-id="ac9e6-122">hello **Barracuda Web Application Firewall** blade opens providing you information about hello partner solution.</span></span> <span data-ttu-id="ac9e6-123">Välj **skapa** hello information-bladet.</span><span class="sxs-lookup"><span data-stu-id="ac9e6-123">Select **Create** in hello information blade.</span></span>

   ![Brandväggen information bladet][3]

6. <span data-ttu-id="ac9e6-125">Hej **ny Brandvägg för webbaserade program** blad öppnas, där du kan utföra **VM-konfiguration** steg och ange **Brandvägg Information**.</span><span class="sxs-lookup"><span data-stu-id="ac9e6-125">hello **New Web Application Firewall** blade opens, where you can perform **VM Configuration** steps and provide **WAF Information**.</span></span> <span data-ttu-id="ac9e6-126">Välj **VM-konfiguration**.</span><span class="sxs-lookup"><span data-stu-id="ac9e6-126">Select **VM Configuration**.</span></span>
7. <span data-ttu-id="ac9e6-127">I hello **VM-konfiguration** bladet anger du information som krävs toospin in hello virtuell dator som kör hello Brandvägg.</span><span class="sxs-lookup"><span data-stu-id="ac9e6-127">In hello **VM Configuration** blade, you enter information required toospin up hello virtual machine that runs hello WAF.</span></span>
   <span data-ttu-id="ac9e6-128">![VM-konfiguration][4]</span><span class="sxs-lookup"><span data-stu-id="ac9e6-128">![VM configuration][4]</span></span>
8. <span data-ttu-id="ac9e6-129">Returnera toohello **ny Brandvägg för webbaserade program** och välj **Brandvägg Information**.</span><span class="sxs-lookup"><span data-stu-id="ac9e6-129">Return toohello **New Web Application Firewall** blade and select **WAF Information**.</span></span> <span data-ttu-id="ac9e6-130">I hello **Brandvägg Information** bladet du konfigurerar hello Brandvägg sig själv.</span><span class="sxs-lookup"><span data-stu-id="ac9e6-130">In hello **WAF Information** blade, you configure hello WAF itself.</span></span> <span data-ttu-id="ac9e6-131">Steg 7 kan tooconfigure hello virtuell dator på vilken hello Brandvägg körs och steg 8 kan du tooprovision hello Brandvägg sig själv.</span><span class="sxs-lookup"><span data-stu-id="ac9e6-131">Step 7 allows you tooconfigure hello virtual machine on which hello WAF runs and step 8 enables you tooprovision hello WAF itself.</span></span>

## <a name="finalize-application-protection"></a><span data-ttu-id="ac9e6-132">Slutför programskydd</span><span class="sxs-lookup"><span data-stu-id="ac9e6-132">Finalize application protection</span></span>
1. <span data-ttu-id="ac9e6-133">Returnera toohello **rekommendationer** bladet.</span><span class="sxs-lookup"><span data-stu-id="ac9e6-133">Return toohello **Recommendations** blade.</span></span> <span data-ttu-id="ac9e6-134">En ny post skapades när du har skapat hello Brandvägg, kallas **Slutför programskydd**.</span><span class="sxs-lookup"><span data-stu-id="ac9e6-134">A new entry was generated after you created hello WAF, called **Finalize application protection**.</span></span> <span data-ttu-id="ac9e6-135">Den här posten kan du vet att du måste toocomplete hello processen för att koppla samman hello Brandvägg inom hello Azure Virtual Network så att den kan skydda hello program.</span><span class="sxs-lookup"><span data-stu-id="ac9e6-135">This entry lets you know that you need toocomplete hello process of actually wiring up hello WAF within hello Azure Virtual Network so that it can protect hello application.</span></span>

   ![Slutför programskydd][5]

2. <span data-ttu-id="ac9e6-137">Välj **Slutför programskydd**.</span><span class="sxs-lookup"><span data-stu-id="ac9e6-137">Select **Finalize application protection**.</span></span> <span data-ttu-id="ac9e6-138">Ett nytt blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="ac9e6-138">A new blade opens.</span></span> <span data-ttu-id="ac9e6-139">Du kan se att det finns ett program som behöver toohave dess trafik dirigeras om.</span><span class="sxs-lookup"><span data-stu-id="ac9e6-139">You can see that there is a web application that needs toohave its traffic rerouted.</span></span>
3. <span data-ttu-id="ac9e6-140">Välj hello webbprogram.</span><span class="sxs-lookup"><span data-stu-id="ac9e6-140">Select hello web application.</span></span> <span data-ttu-id="ac9e6-141">Då öppnas ett blad som ger steg för att slutföra installationen av brandväggen hello.</span><span class="sxs-lookup"><span data-stu-id="ac9e6-141">A blade opens that gives you steps for finalizing hello web application firewall setup.</span></span> <span data-ttu-id="ac9e6-142">Utför hello steg och välj sedan **begränsa trafik**.</span><span class="sxs-lookup"><span data-stu-id="ac9e6-142">Complete hello steps, and then select **Restrict traffic**.</span></span> <span data-ttu-id="ac9e6-143">Security Center sedan hello-kablar upp för dig.</span><span class="sxs-lookup"><span data-stu-id="ac9e6-143">Security Center then does hello wiring-up for you.</span></span>

   ![Begränsa trafik][6]

> [!NOTE]
> <span data-ttu-id="ac9e6-145">Du kan skydda flera webbprogram i Security Center genom att lägga till dessa program tooyour befintliga Brandvägg-distributioner.</span><span class="sxs-lookup"><span data-stu-id="ac9e6-145">You can protect multiple web applications in Security Center by adding these applications tooyour existing WAF deployments.</span></span>
>
>

<span data-ttu-id="ac9e6-146">hello loggar från den Brandvägg är nu fullständigt integrerat.</span><span class="sxs-lookup"><span data-stu-id="ac9e6-146">hello logs from that WAF are now fully integrated.</span></span> <span data-ttu-id="ac9e6-147">Security Center kan starta automatiskt samla in och analysera hello-loggarna så att den kan ansluta viktig säkerhetsuppgift aviseringar tooyou.</span><span class="sxs-lookup"><span data-stu-id="ac9e6-147">Security Center can start automatically gathering and analyzing hello logs so that it can surface important security alerts tooyou.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ac9e6-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ac9e6-148">Next steps</span></span>
<span data-ttu-id="ac9e6-149">Det här dokumentet visar dig hur hello tooimplement Security Center rekommendation ”Lägg till ett webbprogram”.</span><span class="sxs-lookup"><span data-stu-id="ac9e6-149">This document showed you how tooimplement hello Security Center recommendation "Add a web application."</span></span> <span data-ttu-id="ac9e6-150">toolearn mer om hur du konfigurerar en brandvägg för webbaserade program, finns följande hello:</span><span class="sxs-lookup"><span data-stu-id="ac9e6-150">toolearn more about configuring a web application firewall, see hello following:</span></span>

* [<span data-ttu-id="ac9e6-151">Konfigurera en brandvägg för webbaserade program (Brandvägg) för Apptjänst-miljö</span><span class="sxs-lookup"><span data-stu-id="ac9e6-151">Configuring a Web Application Firewall (WAF) for App Service Environment</span></span>](../app-service-web/app-service-app-service-environment-web-application-firewall.md)

<span data-ttu-id="ac9e6-152">toolearn mer om Security Center finns hello följande:</span><span class="sxs-lookup"><span data-stu-id="ac9e6-152">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="ac9e6-153">[Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Lär dig hur tooconfigure säkerhetsprinciper för dina Azure-prenumerationer och resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="ac9e6-153">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="ac9e6-154">[Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md) – Lär dig hur toomonitor hello Azure-resursers hälsa.</span><span class="sxs-lookup"><span data-stu-id="ac9e6-154">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="ac9e6-155">[Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar.</span><span class="sxs-lookup"><span data-stu-id="ac9e6-155">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="ac9e6-156">[Hantera säkerhetsrekommendationer i Azure Security Center](security-center-recommendations.md) – Lär dig rekommendationer för att skydda dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="ac9e6-156">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="ac9e6-157">[Vanliga frågor om Azure Security Center](security-center-faq.md) --finns vanliga frågor om hur du använder hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ac9e6-157">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="ac9e6-158">[Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) --hittar du blogginlägg om säkerhet och Azure kompatibilitet.</span><span class="sxs-lookup"><span data-stu-id="ac9e6-158">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Find blog posts about Azure security and compliance.</span></span>

<!--Image references-->
[1]: ./media/security-center-add-web-application-firewall/secure-web-application.png
[2]:./media/security-center-add-web-application-firewall/add-a-waf.png
[3]: ./media/security-center-add-web-application-firewall/info-blade.png
[4]: ./media/security-center-add-web-application-firewall/select-vm-config.png
[5]: ./media/security-center-add-web-application-firewall/finalize-waf.png
[6]: ./media/security-center-add-web-application-firewall/restrict-traffic.png
