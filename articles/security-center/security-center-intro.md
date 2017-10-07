---
title: aaaIntroduction tooAzure Security Center | Microsoft Docs
description: "Här får du lära dig om Azure Security Center, de viktigaste funktionerna och hur Security Center fungerar."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 45b9756b-6449-49ec-950b-5ed1e7c56daa
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: terrylan
ms.openlocfilehash: 287dbaaa7e2004c522f103595bc316261daf05b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-security-center"></a><span data-ttu-id="f05fe-103">Introduktion tooAzure Security Center</span><span class="sxs-lookup"><span data-stu-id="f05fe-103">Introduction tooAzure Security Center</span></span>
<span data-ttu-id="f05fe-104">Här får du lära dig om Azure Security Center, de viktigaste funktionerna och hur Security Center fungerar.</span><span class="sxs-lookup"><span data-stu-id="f05fe-104">Learn about Azure Security Center, its key capabilities, and how it works.</span></span>

> [!NOTE]
> <span data-ttu-id="f05fe-105">Från och med tidig juni 2017 Security Center använder hello Microsoft Monitoring Agent toocollect och lagra data.</span><span class="sxs-lookup"><span data-stu-id="f05fe-105">Beginning in early June 2017, Security Center will use hello Microsoft Monitoring Agent toocollect and store data.</span></span> <span data-ttu-id="f05fe-106">Se [Azure Security Center-plattformen migrering](security-center-platform-migration.md) toolearn mer.</span><span class="sxs-lookup"><span data-stu-id="f05fe-106">See [Azure Security Center Platform Migration](security-center-platform-migration.md) toolearn more.</span></span> <span data-ttu-id="f05fe-107">hello informationen i den här artikeln representerar Security Center-funktionalitet efter övergången toohello Microsoft Monitoring Agent.</span><span class="sxs-lookup"><span data-stu-id="f05fe-107">hello information in this article represents Security Center functionality after transition toohello Microsoft Monitoring Agent.</span></span>
>
>

## <a name="what-is-azure-security-center"></a><span data-ttu-id="f05fe-108">Vad är Azure Security Center?</span><span class="sxs-lookup"><span data-stu-id="f05fe-108">What is Azure Security Center?</span></span>
 <span data-ttu-id="f05fe-109">Security Center hjälper dig att förebygga, upptäcka och åtgärda toothreats med bättre överblick och kontroll över hello säkerheten för dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="f05fe-109">Security Center helps you prevent, detect, and respond toothreats with increased visibility into and control over hello security of your Azure resources.</span></span> <span data-ttu-id="f05fe-110">Härifrån kan du övervaka och hantera principer för alla Azureprenumerationer på en gång och upptäcka hot som annars kanske skulle förbli oupptäckta. Azure Security Center fungerar tillsammans med ett vittomfattande ekosystem med säkerhetslösningar.</span><span class="sxs-lookup"><span data-stu-id="f05fe-110">It provides integrated security monitoring and policy management across your Azure subscriptions, helps detect threats that might otherwise go unnoticed, and works with a broad ecosystem of security solutions.</span></span>

## <a name="key-capabilities"></a><span data-ttu-id="f05fe-111">De viktigaste funktionerna</span><span class="sxs-lookup"><span data-stu-id="f05fe-111">Key capabilities</span></span>
 <span data-ttu-id="f05fe-112">Security Center har lätt att använda och effektivt hot förhindra, upptäcka och svar-funktioner som är inbyggda i tooAzure.</span><span class="sxs-lookup"><span data-stu-id="f05fe-112">Security Center delivers easy-to-use and effective threat prevention, detection, and response capabilities that are built in tooAzure.</span></span> <span data-ttu-id="f05fe-113">Här är de viktigaste funktionerna:</span><span class="sxs-lookup"><span data-stu-id="f05fe-113">Key capabilities are:</span></span>

| <span data-ttu-id="f05fe-114">Fas</span><span class="sxs-lookup"><span data-stu-id="f05fe-114">Stage</span></span> | <span data-ttu-id="f05fe-115">Funktion</span><span class="sxs-lookup"><span data-stu-id="f05fe-115">Capability</span></span> |
| --- | --- |
| <span data-ttu-id="f05fe-116">Förebygga</span><span class="sxs-lookup"><span data-stu-id="f05fe-116">Prevent</span></span> |<span data-ttu-id="f05fe-117">Övervakare hello säkerhetstillståndet hos dina Azure-resurser</span><span class="sxs-lookup"><span data-stu-id="f05fe-117">Monitors hello security state of your Azure resources</span></span> |
| <span data-ttu-id="f05fe-118">Förebygga</span><span class="sxs-lookup"><span data-stu-id="f05fe-118">Prevent</span></span> | <span data-ttu-id="f05fe-119">Definierar principer för dina Azure-prenumerationer utifrån företagets säkerhetskrav, hello typer av program som du använder och hello känslighet för dina data</span><span class="sxs-lookup"><span data-stu-id="f05fe-119">Defines policies for your Azure subscriptions based on your company’s security requirements, hello types of applications that you use, and hello sensitivity of your data</span></span> |
| <span data-ttu-id="f05fe-120">Förebygga</span><span class="sxs-lookup"><span data-stu-id="f05fe-120">Prevent</span></span> | <span data-ttu-id="f05fe-121">Utifrån principerna ges säkerhet rekommendationer tooguide tjänstens ägare via hello process för att implementera som behövs.</span><span class="sxs-lookup"><span data-stu-id="f05fe-121">Uses policy-driven security recommendations tooguide service owners through hello process of implementing needed controls</span></span> |
| <span data-ttu-id="f05fe-122">Förebygga</span><span class="sxs-lookup"><span data-stu-id="f05fe-122">Prevent</span></span> | <span data-ttu-id="f05fe-123">Tjänster och redskap från Microsoft och partnerföretag kan snabbt distribueras.</span><span class="sxs-lookup"><span data-stu-id="f05fe-123">Rapidly deploys security services and appliances from Microsoft and partners</span></span> |
| <span data-ttu-id="f05fe-124">Upptäcka</span><span class="sxs-lookup"><span data-stu-id="f05fe-124">Detect</span></span> |<span data-ttu-id="f05fe-125">Automatiskt samlas in och analyseras säkerhetsdata från Azure-resurser, hello nätverket och partnerlösningarna, till exempel program mot skadlig kod och brandväggar</span><span class="sxs-lookup"><span data-stu-id="f05fe-125">Automatically collects and analyzes security data from your Azure resources, hello network, and partner solutions like antimalware programs and firewalls</span></span> |
| <span data-ttu-id="f05fe-126">Upptäcka</span><span class="sxs-lookup"><span data-stu-id="f05fe-126">Detect</span></span> | <span data-ttu-id="f05fe-127">Använder globala hot intelligence från Microsoft-produkter och tjänster, hello Microsoft Digital Crimes Unit (DCU), hello Microsoft Security Response Center (MSRC) och externa flöden används.</span><span class="sxs-lookup"><span data-stu-id="f05fe-127">Uses global threat intelligence from Microsoft products and services, hello Microsoft Digital Crimes Unit (DCU), hello Microsoft Security Response Center (MSRC), and external feeds</span></span> |
| <span data-ttu-id="f05fe-128">Upptäcka</span><span class="sxs-lookup"><span data-stu-id="f05fe-128">Detect</span></span> | <span data-ttu-id="f05fe-129">Avancerad analysteknik, till exempel machine learning och beteendeanalys används.</span><span class="sxs-lookup"><span data-stu-id="f05fe-129">Applies advanced analytics, including machine learning and behavioral analysis</span></span> |
| <span data-ttu-id="f05fe-130">Åtgärda</span><span class="sxs-lookup"><span data-stu-id="f05fe-130">Respond</span></span> |<span data-ttu-id="f05fe-131">Säkerhetsrelaterade incidenter/aviseringar prioritetsklassas och rapporteras.</span><span class="sxs-lookup"><span data-stu-id="f05fe-131">Provides prioritized security incidents/alerts</span></span> |
| <span data-ttu-id="f05fe-132">Åtgärda</span><span class="sxs-lookup"><span data-stu-id="f05fe-132">Respond</span></span> | <span data-ttu-id="f05fe-133">Ger insikter om hello källan hello angrepp och vilka resurser</span><span class="sxs-lookup"><span data-stu-id="f05fe-133">Offers insights into hello source of hello attack and impacted resources</span></span> |
| <span data-ttu-id="f05fe-134">Åtgärda</span><span class="sxs-lookup"><span data-stu-id="f05fe-134">Respond</span></span> | <span data-ttu-id="f05fe-135">Förslag om hur toostop hello pågående angrepp och förebygga framtida angrepp.</span><span class="sxs-lookup"><span data-stu-id="f05fe-135">Suggests ways toostop hello current attack and help prevent future attacks</span></span> |

## <a name="introductory-walkthrough"></a><span data-ttu-id="f05fe-136">Introduktionsgenomgång</span><span class="sxs-lookup"><span data-stu-id="f05fe-136">Introductory walkthrough</span></span>

> [!NOTE]
> <span data-ttu-id="f05fe-137">Det här dokumentet introducerar hello-tjänsten med hjälp av ett exempel på distribution.</span><span class="sxs-lookup"><span data-stu-id="f05fe-137">This document introduces hello service by using an example deployment.</span></span> <span data-ttu-id="f05fe-138">Det här dokumentet är inte en stegvis guide.</span><span class="sxs-lookup"><span data-stu-id="f05fe-138">This document is not a step-by-step guide.</span></span>
>
>

 <span data-ttu-id="f05fe-139">Security Center öppnas från hello [Azure-portalen](https://azure.microsoft.com/features/azure-portal/).</span><span class="sxs-lookup"><span data-stu-id="f05fe-139">You access Security Center from hello [Azure portal](https://azure.microsoft.com/features/azure-portal/).</span></span> <span data-ttu-id="f05fe-140">[Logga in toohello portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f05fe-140">[Sign in toohello portal](https://portal.azure.com).</span></span> <span data-ttu-id="f05fe-141">Under hello portal huvudmenyn rulla toohello **Security Center** alternativ eller välj hello **Security Center** panelen som du tidigare har fäst toohello portalens instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="f05fe-141">Under hello main portal menu, scroll toohello **Security Center** option or select hello **Security Center** tile that you previously pinned toohello portal dashboard.</span></span>

![Säkerhetsrutan i Azure Portal][1]

<span data-ttu-id="f05fe-143">I Security Center kan du ställa in säkerhetsprinciper, övervaka säkerhetskonfigurationer och se säkerhetsaviseringar.</span><span class="sxs-lookup"><span data-stu-id="f05fe-143">From Security Center, you can set security policies, monitor security configurations, and view security alerts.</span></span>

### <a name="security-policies"></a><span data-ttu-id="f05fe-144">Säkerhetsprinciper</span><span class="sxs-lookup"><span data-stu-id="f05fe-144">Security policies</span></span>
<span data-ttu-id="f05fe-145">Du kan definiera principer för Azure-prenumerationer enligt tooyour företagets säkerhetskrav.</span><span class="sxs-lookup"><span data-stu-id="f05fe-145">You can define policies for your Azure subscriptions according tooyour company's security requirements.</span></span> <span data-ttu-id="f05fe-146">Du kan också anpassa dem toohello typer av program som du använder eller toohello känslighet hello data i varje prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f05fe-146">You can also tailor them toohello types of applications you're using or toohello sensitivity of hello data in each subscription.</span></span> <span data-ttu-id="f05fe-147">De resurser som används för utveckling eller tester behöver kanske en annan typ av säkerhetsinställningar än de som används för program i produktion.</span><span class="sxs-lookup"><span data-stu-id="f05fe-147">For example, resources used for development or testing may have different security requirements than those used for production applications.</span></span> <span data-ttu-id="f05fe-148">På samma sätt behövs det kanske en högre säkerhetsnivå för sådant som det finns lagar om, till exempel personuppgifter.</span><span class="sxs-lookup"><span data-stu-id="f05fe-148">Likewise, applications with regulated data like PII may require a higher level of security.</span></span>

> [!NOTE]
> <span data-ttu-id="f05fe-149">toomodify säkerhetspolicyn, bör du vara en säkerhetsadministratör eller hello Prenumerationens ägare eller deltagare.</span><span class="sxs-lookup"><span data-stu-id="f05fe-149">toomodify a security policy, you must be a Security Administrator or hello subscription's Owner or Contributor.</span></span> <span data-ttu-id="f05fe-150">toolearn mer information om roller och tillåtna åtgärder i Security Center finns [behörigheter i Azure Security Center](security-center-permissions.md).</span><span class="sxs-lookup"><span data-stu-id="f05fe-150">toolearn more about roles and allowed actions in Security Center, see [Permissions in Azure Security Center](security-center-permissions.md).</span></span>
>
>

<span data-ttu-id="f05fe-151">På hello **Security Center** bladet, Välj hello **princip** panelen för en lista över dina prenumerationer och resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="f05fe-151">On hello **Security Center** blade, select hello **Policy** tile for a list of your subscriptions and resource groups.</span></span>   

![Security Center-bladet][2]

<span data-ttu-id="f05fe-153">På hello **säkerhetsprincip** bladet Välj en tooview hello princip prenumerationsinformation.</span><span class="sxs-lookup"><span data-stu-id="f05fe-153">On hello **Security policy** blade, select a subscription tooview hello policy details.</span></span>

<span data-ttu-id="f05fe-154">**Datainsamling** samlas data för en säkerhetsprincip.</span><span class="sxs-lookup"><span data-stu-id="f05fe-154">**Data collection** enables data collection for a security policy.</span></span> <span data-ttu-id="f05fe-155">Om du aktiverar den här funktionen händer följande:</span><span class="sxs-lookup"><span data-stu-id="f05fe-155">Enabling provides:</span></span>

* <span data-ttu-id="f05fe-156">Genomsöks alla kompatibla virtuella datorer (VM) för säkerhetsövervakning och rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="f05fe-156">Daily scanning of all supported virtual machines (VMs) for security monitoring and recommendations.</span></span>
* <span data-ttu-id="f05fe-157">Säkerhetshändelser samlas in för analys och hotidentifiering.</span><span class="sxs-lookup"><span data-stu-id="f05fe-157">Collection of security events for analysis and threat detection.</span></span>

> [!NOTE]
> <span data-ttu-id="f05fe-158">Datainsamling konfigureras på hello prenumerationsnivån.</span><span class="sxs-lookup"><span data-stu-id="f05fe-158">Data collection is configured at hello subscription level.</span></span>
>
>

<span data-ttu-id="f05fe-159">Välj **skyddsprincip** tooopen hello **skyddsprincip** bladet.</span><span class="sxs-lookup"><span data-stu-id="f05fe-159">Select **Prevention policy** tooopen hello **Prevention policy** blade.</span></span> <span data-ttu-id="f05fe-160">**Visa rekommendationer för** kan du välja hello säkerhetsåtgärder som du vill toomonitor och hello rekommendationer som du vill toosee baserat på hello säkerhetsbehov hello resurser inom hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f05fe-160">**Show recommendations for** lets you choose hello security controls that you want toomonitor and hello recommendations that you want toosee based on hello security needs of hello resources within hello subscription.</span></span>

### <a name="security-recommendations"></a><span data-ttu-id="f05fe-161">Säkerhetsrekommendationer</span><span class="sxs-lookup"><span data-stu-id="f05fe-161">Security recommendations</span></span>
 <span data-ttu-id="f05fe-162">Security Center analyserar hello säkerhetstillståndet hos dina Azure-resurser tooidentify potentiella säkerhetsproblem.</span><span class="sxs-lookup"><span data-stu-id="f05fe-162">Security Center analyzes hello security state of your Azure resources tooidentify potential security vulnerabilities.</span></span> <span data-ttu-id="f05fe-163">En lista över rekommendationer hjälper dig att hello konfigureringen av nödvändiga kontroller.</span><span class="sxs-lookup"><span data-stu-id="f05fe-163">A list of recommendations guides you through hello process of configuring needed controls.</span></span> <span data-ttu-id="f05fe-164">Exempel:</span><span class="sxs-lookup"><span data-stu-id="f05fe-164">Examples include:</span></span>

* <span data-ttu-id="f05fe-165">Etablera program mot skadlig kod toohelp identifiera och ta bort skadlig programvara</span><span class="sxs-lookup"><span data-stu-id="f05fe-165">Provisioning antimalware toohelp identify and remove malicious software</span></span>
* <span data-ttu-id="f05fe-166">Konfigurera network security grupper och regler toocontrol trafik tooVMs</span><span class="sxs-lookup"><span data-stu-id="f05fe-166">Configuring network security groups and rules toocontrol traffic tooVMs</span></span>
* <span data-ttu-id="f05fe-167">Etablering av web application brandväggar toohelp skydd mot angrepp riktade webbprogram</span><span class="sxs-lookup"><span data-stu-id="f05fe-167">Provisioning of web application firewalls toohelp defend against attacks that target your web applications</span></span>
* <span data-ttu-id="f05fe-168">genomföra alla systemuppdateringar som fattas</span><span class="sxs-lookup"><span data-stu-id="f05fe-168">Deploying missing system updates</span></span>
* <span data-ttu-id="f05fe-169">OS-konfigurationer som inte matchar hello-adressering rekommenderade baslinjer</span><span class="sxs-lookup"><span data-stu-id="f05fe-169">Addressing OS configurations that do not match hello recommended baselines</span></span>

<span data-ttu-id="f05fe-170">Klicka på hello **rekommendationer** panelen för en lista över rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="f05fe-170">Click hello **Recommendations** tile for a list of recommendations.</span></span> <span data-ttu-id="f05fe-171">Klicka på varje rekommendation tooview ytterligare information eller tootake åtgärd tooresolve hello problemet.</span><span class="sxs-lookup"><span data-stu-id="f05fe-171">Click each recommendation tooview additional information or tootake action tooresolve hello issue.</span></span>

![Säkerhetsrekommendationer i Azure Security Center][5]

### <a name="security-state-of-azure-resources"></a><span data-ttu-id="f05fe-173">Säkerhetstillståndet hos Azure-resurser</span><span class="sxs-lookup"><span data-stu-id="f05fe-173">Security state of Azure resources</span></span>
<span data-ttu-id="f05fe-174">Hej **förebyggande** avsnittet hello instrumentpanelen visar hello övergripande säkerhetstillståndet hello miljön uppdelat efter resurstyp, inklusive virtuella datorer, webbprogram och andra resurser.</span><span class="sxs-lookup"><span data-stu-id="f05fe-174">hello **Prevention** section of hello dashboard shows hello overall security posture of hello environment by resource type, including VMs, web applications, and other resources.</span></span>   

<span data-ttu-id="f05fe-175">Välj en resurstyp under **förebyggande** tooview mer information, inklusive en lista över alla potentiella säkerhetsproblem har identifierats.</span><span class="sxs-lookup"><span data-stu-id="f05fe-175">Select a resource type under **Prevention** tooview more information, including a list of any potential security vulnerabilities that have been identified.</span></span> <span data-ttu-id="f05fe-176">(**Compute** väljs i hello exemplet nedan.)</span><span class="sxs-lookup"><span data-stu-id="f05fe-176">(**Compute** is selected in hello example below.)</span></span>

![Rutan med resurshälsa][6]

### <a name="security-alerts"></a><span data-ttu-id="f05fe-178">Säkerhetsaviseringar</span><span class="sxs-lookup"><span data-stu-id="f05fe-178">Security alerts</span></span>
 <span data-ttu-id="f05fe-179">Security Center automatiskt samlar in, analyseras och integreras loggdata från Azure-resurser, hello nätverket och partnerlösningarna, till exempel program mot skadlig kod och brandväggar.</span><span class="sxs-lookup"><span data-stu-id="f05fe-179">Security Center automatically collects, analyzes, and integrates log data from your Azure resources, hello network, and partner solutions like antimalware programs and firewalls.</span></span> <span data-ttu-id="f05fe-180">Om hot upptäcks skapas en säkerhetsavisering.</span><span class="sxs-lookup"><span data-stu-id="f05fe-180">When threats are detected, a security alert is created.</span></span> <span data-ttu-id="f05fe-181">Exempel på hot:</span><span class="sxs-lookup"><span data-stu-id="f05fe-181">Examples include detection of:</span></span>

* <span data-ttu-id="f05fe-182">Angripna virtuella datorer som kommunicerar med kända skadliga IP-adresser</span><span class="sxs-lookup"><span data-stu-id="f05fe-182">Compromised VMs communicating with known malicious IP addresses</span></span>
* <span data-ttu-id="f05fe-183">avancerad skadlig kod upptäckt genom felrapporteringen i Windows</span><span class="sxs-lookup"><span data-stu-id="f05fe-183">Advanced malware detected by using Windows error reporting</span></span>
* <span data-ttu-id="f05fe-184">Brute force-attacker mot virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="f05fe-184">Brute force attacks against VMs</span></span>
* <span data-ttu-id="f05fe-185">säkerhetsaviseringar från integrerade brandväggar och program mot skadlig kod</span><span class="sxs-lookup"><span data-stu-id="f05fe-185">Security alerts from integrated antimalware programs and firewalls</span></span>

<span data-ttu-id="f05fe-186">Klicka på hello **säkerhetsaviseringar** visas en lista med prioritetsklassade aviseringar.</span><span class="sxs-lookup"><span data-stu-id="f05fe-186">Clicking hello **Security alerts** tile displays a list of prioritized alerts.</span></span>

![Säkerhetsaviseringar][7]

<span data-ttu-id="f05fe-188">Om du väljer en avisering visas mer information om hello och förslag på hur tooremediate den.</span><span class="sxs-lookup"><span data-stu-id="f05fe-188">Selecting an alert shows more information about hello attack and suggestions for how tooremediate it.</span></span>

![Utförlig information om säkerhetsaviseringar][8]

### <a name="partner-solutions"></a><span data-ttu-id="f05fe-190">Partnerlösningar</span><span class="sxs-lookup"><span data-stu-id="f05fe-190">Partner solutions</span></span>
<span data-ttu-id="f05fe-191">Hej **partnerlösningar** sida vid sida kan du snabbt en överblick över hello säkerhetstillstånd på partnerlösningarna som är integrerad med din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f05fe-191">hello **Partner solutions** tile lets you monitor at a glance hello security state of your partner solutions integrated with your Azure subscription.</span></span> <span data-ttu-id="f05fe-192">Security Center visar aviseringar från hello lösningar.</span><span class="sxs-lookup"><span data-stu-id="f05fe-192">Security Center displays alerts coming from hello solutions.</span></span>

<span data-ttu-id="f05fe-193">Välj hello **partnerlösningar** panelen.</span><span class="sxs-lookup"><span data-stu-id="f05fe-193">Select hello **Partner solutions** tile.</span></span> <span data-ttu-id="f05fe-194">Då öppnas ett blad med en lista över alla anslutna partnerlösningar.</span><span class="sxs-lookup"><span data-stu-id="f05fe-194">A blade opens displaying a list of all connected partner solutions.</span></span>

![Partnerlösningar][9]

## <a name="get-started"></a><span data-ttu-id="f05fe-196">Kom igång</span><span class="sxs-lookup"><span data-stu-id="f05fe-196">Get started</span></span>
<span data-ttu-id="f05fe-197">tooget igång med Security Center, behöver du en prenumeration tooMicrosoft Azure.</span><span class="sxs-lookup"><span data-stu-id="f05fe-197">tooget started with Security Center, you need a subscription tooMicrosoft Azure.</span></span> <span data-ttu-id="f05fe-198">Security Center aktiveras genom Azure-prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="f05fe-198">Security Center is enabled with your Azure subscription.</span></span> <span data-ttu-id="f05fe-199">Om du inte har någon prenumeration kan du registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f05fe-199">If you do not have a subscription, you can sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

 <span data-ttu-id="f05fe-200">Security Center öppnas från hello [Azure-portalen](https://azure.microsoft.com/features/azure-portal/).</span><span class="sxs-lookup"><span data-stu-id="f05fe-200">You access Security Center from hello [Azure portal](https://azure.microsoft.com/features/azure-portal/).</span></span> <span data-ttu-id="f05fe-201">Se hello [portaldokumentationen](https://azure.microsoft.com/documentation/services/azure-portal/) toolearn mer.</span><span class="sxs-lookup"><span data-stu-id="f05fe-201">See hello [portal documentation](https://azure.microsoft.com/documentation/services/azure-portal/) toolearn more.</span></span>

<span data-ttu-id="f05fe-202">[Komma igång med Azure Security Center](security-center-get-started.md) snabbt hjälper dig att hello-säkerhetsövervakning och principhantering komponenter i Security Center.</span><span class="sxs-lookup"><span data-stu-id="f05fe-202">[Getting started with Azure Security Center](security-center-get-started.md) quickly guides you through hello security-monitoring and policy-management components of Security Center.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f05fe-203">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f05fe-203">Next steps</span></span>
<span data-ttu-id="f05fe-204">I det här dokumentet har introducerades tooSecurity Center, de viktigaste funktionerna och hur tooget startas.</span><span class="sxs-lookup"><span data-stu-id="f05fe-204">In this document, you were introduced tooSecurity Center, its key capabilities, and how tooget started.</span></span> <span data-ttu-id="f05fe-205">toolearn se fler hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="f05fe-205">toolearn more, see hello following resources:</span></span>

* <span data-ttu-id="f05fe-206">[Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Lär dig hur tooconfigure säkerhetsprinciper för dina Azure-prenumerationer och resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="f05fe-206">[Setting security policies in Azure Security Center](security-center-policies.md) — Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="f05fe-207">[Hantera säkerhetsrekommendationer i Azure Security Center](security-center-recommendations.md) – Lär dig rekommendationer för att skydda dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="f05fe-207">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) — Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="f05fe-208">[Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md) – Lär dig hur toomonitor hello Azure-resursers hälsa.</span><span class="sxs-lookup"><span data-stu-id="f05fe-208">[Security health monitoring in Azure Security Center](security-center-monitoring.md) — Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="f05fe-209">[Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar.</span><span class="sxs-lookup"><span data-stu-id="f05fe-209">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) — Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="f05fe-210">[Övervaka partnerlösningar med Azure Security Center](security-center-partner-solutions.md) – Lär dig hur toomonitor hello dina partnerlösningars hälsostatus.</span><span class="sxs-lookup"><span data-stu-id="f05fe-210">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) — Learn how toomonitor hello health status of your partner solutions.</span></span>
- <span data-ttu-id="f05fe-211">[Datasäkerhet i Azure Security Center](security-center-data-security.md) – Lär dig hur data hanteras och garanteras i Security Center.</span><span class="sxs-lookup"><span data-stu-id="f05fe-211">[Azure Security Center data security](security-center-data-security.md) - Learn how data is managed and safeguarded in Security Center.</span></span>
* <span data-ttu-id="f05fe-212">[Vanliga frågor om Azure Security Center](security-center-faq.md) – finns vanliga frågor om hur du använder hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f05fe-212">[Azure Security Center FAQ](security-center-faq.md) — Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="f05fe-213">[Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) – hello Azure security nyheter och information.</span><span class="sxs-lookup"><span data-stu-id="f05fe-213">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) — Get hello latest Azure security news and information.</span></span>

<!--Image references-->
[1]: ./media/security-center-intro/security-tile.png
[2]: ./media/security-center-intro/security-center.png
[3]: ./media/security-center-intro/security-policy.png
[4]: ./media/security-center-intro/security-policy-blade.png
[5]: ./media/security-center-intro/recommendations.png
[6]: ./media/security-center-intro/resources-health.png
[7]: ./media/security-center-intro/security-alert.png
[8]: ./media/security-center-intro/security-alert-detail.png
[9]: ./media/security-center-intro/partner-solutions.png
