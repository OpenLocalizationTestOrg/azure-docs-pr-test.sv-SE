---
title: Skydda dina personliga data med Azure Security Center | Microsoft Docs
description: "skydda personliga data med hjälp av Azure security center"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: 3a941389713a4d3dbffbbfe8a717409927d85c6d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="protect-personal-data-from-breaches-and-attacks-azure-security-center"></a><span data-ttu-id="085df-103">Skydda personliga data från överträdelser och attacker: Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="085df-103">Protect personal data from breaches and attacks: Azure Security Center</span></span>

<span data-ttu-id="085df-104">Den här artikeln hjälper dig att förstå hur du använder Azure Security Center för att skydda personliga data från överträdelser och attacker.</span><span class="sxs-lookup"><span data-stu-id="085df-104">This article will help you understand how to use Azure Security Center to protect personal data from breaches and attacks.</span></span>

## <a name="scenario"></a><span data-ttu-id="085df-105">Scenario</span><span class="sxs-lookup"><span data-stu-id="085df-105">Scenario</span></span> 

<span data-ttu-id="085df-106">Ett stort kryssning företag, sitt säte i USA utökar åtgärderna för att erbjuda färdvägar i Medelhavsområdet och baltiska havet samt Förenta staterna.</span><span class="sxs-lookup"><span data-stu-id="085df-106">A large cruise company, headquartered in the United States, is expanding its operations to offer itineraries in the Mediterranean, and Baltic seas, as well as the British Isles.</span></span> <span data-ttu-id="085df-107">Hjälpen i dessa åtgärder har det fått flera mindre kryssning rader i Italien, Tyskland, Danmark och Storbritannien</span><span class="sxs-lookup"><span data-stu-id="085df-107">To help in those efforts, it has acquired several smaller cruise lines based in Italy, Germany, Denmark, and the U.K.</span></span>

<span data-ttu-id="085df-108">Företaget använder Microsoft Azure för att lagra företagets data i molnet.</span><span class="sxs-lookup"><span data-stu-id="085df-108">The company uses Microsoft Azure to store corporate data in the cloud.</span></span> <span data-ttu-id="085df-109">Detta inkluderar personligt identifierbar information, till exempel namn, adresser, telefonnummer och kreditkortsinformation.</span><span class="sxs-lookup"><span data-stu-id="085df-109">This includes personal identifiable information such as names, addresses, phone numbers, and credit card information.</span></span> <span data-ttu-id="085df-110">Den inkluderar också personal information såsom:</span><span class="sxs-lookup"><span data-stu-id="085df-110">It also includes Human Resources information such as:</span></span>

- <span data-ttu-id="085df-111">Adresser</span><span class="sxs-lookup"><span data-stu-id="085df-111">Addresses</span></span>
- <span data-ttu-id="085df-112">telefonnummer</span><span class="sxs-lookup"><span data-stu-id="085df-112">Phone numbers</span></span>
- <span data-ttu-id="085df-113">skatt ID-nummer</span><span class="sxs-lookup"><span data-stu-id="085df-113">Tax identification numbers</span></span>
- <span data-ttu-id="085df-114">medicinska information</span><span class="sxs-lookup"><span data-stu-id="085df-114">Medical information</span></span>

<span data-ttu-id="085df-115">Raden kryssning har också en stor databas med ersättning och förmåner medlemmar.</span><span class="sxs-lookup"><span data-stu-id="085df-115">The cruise line also maintains a large database of reward and loyalty program members.</span></span> <span data-ttu-id="085df-116">Företagets anställda åtkomst till nätverket från företagets fjärranslutna kontor och resa agenter finns runtom i världen har åtkomst till vissa företagsresurser.</span><span class="sxs-lookup"><span data-stu-id="085df-116">Corporate employees access the network from the company’s remote offices and travel agents located around the world have access to some company resources.</span></span>
<span data-ttu-id="085df-117">Personliga data överförs över nätverket mellan dessa platser och Microsoft-datacenter.</span><span class="sxs-lookup"><span data-stu-id="085df-117">Personal data travels across the network between these locations and the Microsoft data center.</span></span>

## <a name="problem-statement"></a><span data-ttu-id="085df-118">Problembeskrivning</span><span class="sxs-lookup"><span data-stu-id="085df-118">Problem statement</span></span>

<span data-ttu-id="085df-119">Företaget är orolig för angrepp på sina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="085df-119">The company is concerned about the threat of attacks on their Azure resources.</span></span> <span data-ttu-id="085df-120">De vill förhindra exponering av kunders och anställdas personuppgifter till obehöriga.</span><span class="sxs-lookup"><span data-stu-id="085df-120">They want to prevent exposure of customers’ and employees’ personal data to unauthorized persons.</span></span> <span data-ttu-id="085df-121">De vill ha hjälp med både förebyggande och svar/reparation, samt ett effektivt sätt att övervaka pågående säkerheten för deras molnresurser.</span><span class="sxs-lookup"><span data-stu-id="085df-121">They want guidance on both prevention and response/remediation, as well as an effective way to monitor the ongoing security of their cloud resources.</span></span>
<span data-ttu-id="085df-122">Behöver de en stark försvarslinje mot dagens avancerade och organiserade angripare.</span><span class="sxs-lookup"><span data-stu-id="085df-122">They need a strong line of defense against today’s sophisticated and organized attackers.</span></span>

## <a name="company-goal"></a><span data-ttu-id="085df-123">Företagets mål</span><span class="sxs-lookup"><span data-stu-id="085df-123">Company goal</span></span>

<span data-ttu-id="085df-124">En av företagets mål är att säkerställa säkerheten för kunderna och anställdas personliga data genom att skydda mot hot.</span><span class="sxs-lookup"><span data-stu-id="085df-124">One of the company’s goals is to ensure the privacy of customers’ and employees’ personal data by protecting it from threats.</span></span> <span data-ttu-id="085df-125">En av sina mål är svarar omedelbart på tecken på intrång att minimera effekten.</span><span class="sxs-lookup"><span data-stu-id="085df-125">One of their goals is to respond immediately to signs of breach to mitigate the impact.</span></span> <span data-ttu-id="085df-126">Det krävs ett sätt att utvärdera det aktuella tillståndet för säkerhet, identifiera sårbara konfigurationer och åtgärda dem.</span><span class="sxs-lookup"><span data-stu-id="085df-126">It requires a way to assess the current state of security, identify vulnerable configurations, and remediate them.</span></span>

## <a name="solutions"></a><span data-ttu-id="085df-127">Lösningar</span><span class="sxs-lookup"><span data-stu-id="085df-127">Solutions</span></span>

<span data-ttu-id="085df-128">Microsoft Azure Security Center (ASC) ger en integrerad säkerhet säkerhetsövervakning och principhantering hanteringslösning.</span><span class="sxs-lookup"><span data-stu-id="085df-128">Microsoft Azure Security Center (ASC) provides an integrated security monitoring and policy management solution.</span></span> <span data-ttu-id="085df-129">Lätt att använda och effektivt hot ger funktioner för förebyggande, identifiering och svar.</span><span class="sxs-lookup"><span data-stu-id="085df-129">It delivers easy-to-use and effective threat prevention, detection, and response capabilities.</span></span>

### <a name="prevention"></a><span data-ttu-id="085df-130">Prevention (Skydd)</span><span class="sxs-lookup"><span data-stu-id="085df-130">Prevention</span></span>

<span data-ttu-id="085df-131">ASC hjälper dig att förhindra överträdelser genom att du kan ställa in säkerhetsprinciper, ger just-in-time-åtkomst och implementera säkerhetsrekommendationer.</span><span class="sxs-lookup"><span data-stu-id="085df-131">ASC helps you prevent breaches by enabling you to set security policies, provide just-in-time access, and implement security recommendations.</span></span>

<span data-ttu-id="085df-132">En säkerhetsprincip definierar ett antal kontrollfunktioner som rekommenderas för resurser inom den angivna prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="085df-132">A security policy defines the set of controls recommended for resources within the specified subscription.</span></span> <span data-ttu-id="085df-133">Just-in-time-åtkomst kan användas för att låsa inkommande trafik till din virtuella Azure-datorer, vilket minskar risken för attacker.</span><span class="sxs-lookup"><span data-stu-id="085df-133">Just in time access can be used to lock down inbound traffic to your Azure VMs, reducing exposure to attacks.</span></span> <span data-ttu-id="085df-134">Säkerhetsrekommendationer skapas ASC när du har analyserat säkerhetstillståndet hos dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="085df-134">Security recommendations are created by ASC after analyzing the security state of your Azure resources.</span></span>

#### <a name="how-do-i-set-security-policies-in-asc"></a><span data-ttu-id="085df-135">Hur ställer in säkerhetsprinciper i ASC?</span><span class="sxs-lookup"><span data-stu-id="085df-135">How do I set security policies in ASC?</span></span>

<span data-ttu-id="085df-136">Det går att ställa in särskilda säkerhetsprinciper för varje prenumeration.</span><span class="sxs-lookup"><span data-stu-id="085df-136">You can configure security policies for each subscription.</span></span> <span data-ttu-id="085df-137">För att kunna ändra en säkerhetsprincip måste du vara ägare eller deltagare i den aktuella prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="085df-137">To modify a security policy, you must be an owner or contributor of that subscription.</span></span> <span data-ttu-id="085df-138">Gör följande i Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="085df-138">In the Azure portal, do the following:</span></span>

1. <span data-ttu-id="085df-139">Välj **princip** i ASC instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="085df-139">Select **Policy** in the ASC dashboard.</span></span>

2. <span data-ttu-id="085df-140">Välj den prenumeration som du vill aktivera principen.</span><span class="sxs-lookup"><span data-stu-id="085df-140">Select the subscription on which you want to enable the policy.</span></span>

3. <span data-ttu-id="085df-141">Välj **skyddsprincip** att konfigurera principer per prenumeration.</span><span class="sxs-lookup"><span data-stu-id="085df-141">Choose **Prevention policy** to configure policies per subscription.</span></span> <span data-ttu-id="085df-142">**Samla in data från virtuella datorer** ska anges till **på.**</span><span class="sxs-lookup"><span data-stu-id="085df-142">**Collect data from virtual machines** should be set to **On.**</span></span>

4. <span data-ttu-id="085df-143">I den **skyddsprincip** alternativ, Välj **på** att aktivera säkerhetsrekommendationer som är relevanta för prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="085df-143">In the **Prevention policy** options, select **On** to enable the security recommendations that are relevant for the subscription.</span></span>

![](media/protect-personal-data-azure-security-center/prevention-policy.png)

<span data-ttu-id="085df-144">Mer detaljerade instruktioner och en beskrivning av principrekommendationer som kan aktiveras finns [ställa in säkerhetsprinciper i Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-policies#set-security-policies).</span><span class="sxs-lookup"><span data-stu-id="085df-144">For more detailed instructions and an explanation of each of the policy recommendations that can be enabled, see [Set security policies in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-policies#set-security-policies).</span></span>

#### <a name="how-do-i-configure-just-in-time-access-jit"></a><span data-ttu-id="085df-145">Hur konfigurerar jag precis i tid åtkomst (JIT)?</span><span class="sxs-lookup"><span data-stu-id="085df-145">How do I configure Just in Time Access (JIT)?</span></span>

<span data-ttu-id="085df-146">När JIT aktiveras låser Security Center ned inkommande trafik till din virtuella Azure-datorer genom att skapa en regel för NSG.</span><span class="sxs-lookup"><span data-stu-id="085df-146">When JIT is enabled, Security Center locks down inbound traffic to your Azure VMs by creating an NSG rule.</span></span> <span data-ttu-id="085df-147">Du väljer portar på den virtuella datorn som inkommande trafik ska låsas.</span><span class="sxs-lookup"><span data-stu-id="085df-147">You select the ports on the VM to which inbound traffic will be locked down.</span></span> <span data-ttu-id="085df-148">Om du vill använda JIT åtkomst gör du följande:</span><span class="sxs-lookup"><span data-stu-id="085df-148">To use JIT access, do the following:</span></span>

1. <span data-ttu-id="085df-149">Välj den **precis i tid VM åtkomst panelen** på bladet ASC.</span><span class="sxs-lookup"><span data-stu-id="085df-149">Select the **Just in time VM access tile** on the ASC blade.</span></span>

2. <span data-ttu-id="085df-150">Välj den **rekommenderas** fliken.</span><span class="sxs-lookup"><span data-stu-id="085df-150">Select the **Recommended** tab.</span></span>

3. <span data-ttu-id="085df-151">Under **VMs**, Välj de virtuella datorer som du vill aktivera.</span><span class="sxs-lookup"><span data-stu-id="085df-151">Under **VMs**, select the VMs that you want to enable.</span></span> <span data-ttu-id="085df-152">Detta placerar en bock bredvid en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="085df-152">This puts a checkmark next to a VM.</span></span> 
4. <span data-ttu-id="085df-153">Välj **aktivera JIT** på virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="085df-153">Select **Enable JIT** on VMs.</span></span>
5. <span data-ttu-id="085df-154">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="085df-154">Select **Save**.</span></span>

<span data-ttu-id="085df-155">Du kan se standardportarna som ASC rekommenderar aktiverats för JIT.</span><span class="sxs-lookup"><span data-stu-id="085df-155">Then you can see the default ports that ASC recommends being enabled for JIT.</span></span> <span data-ttu-id="085df-156">Du kan också lägga till och konfigurera en ny port som du vill aktivera den bara i Tidslösning.</span><span class="sxs-lookup"><span data-stu-id="085df-156">You can also add and configure a new port on which you want to enable the just in time solution.</span></span> <span data-ttu-id="085df-157">Den **precis i tid VM access** panelen i Security Center visar hur många virtuella datorer som är konfigurerad för JIT-åtkomst.</span><span class="sxs-lookup"><span data-stu-id="085df-157">The **Just in time VM access** tile in the Security Center shows the number of VMs configured for JIT access.</span></span> <span data-ttu-id="085df-158">Den visar även antal godkända åtkomstbegäranden som görs i den senaste veckan.</span><span class="sxs-lookup"><span data-stu-id="085df-158">It also shows the number of approved access requests made in the last week.</span></span>

![](media/protect-personal-data-azure-security-center/jit-vm.png)

<span data-ttu-id="085df-159">Instruktioner om hur du gör detta, och ytterligare information om precis i Time-åtkomst finns i [hantera virtuella åtkomst med hjälp av precis i tid.](https://docs.microsoft.com/azure/security-center/security-center-just-in-time)</span><span class="sxs-lookup"><span data-stu-id="085df-159">For instructions on how to do this, and additional information about Just in Time access, see [Manage virtual machine access using just in time.](https://docs.microsoft.com/azure/security-center/security-center-just-in-time)</span></span>

#### <a name="how-do-i-implement-asc-security-recommendations"></a><span data-ttu-id="085df-160">Hur jag implementera ASC säkerhetsrekommendationer?</span><span class="sxs-lookup"><span data-stu-id="085df-160">How do I implement ASC security recommendations?</span></span>

<span data-ttu-id="085df-161">När Security Center identifierar potentiella säkerhetsproblem skapas rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="085df-161">When Security Center identifies potential security vulnerabilities, it creates recommendations.</span></span> <span data-ttu-id="085df-162">Via rekommendationerna får du hjälp att ställa in de kontrollfunktioner som behövs.</span><span class="sxs-lookup"><span data-stu-id="085df-162">The recommendations guide you through the process of configuring the needed controls.</span></span> 
1. <span data-ttu-id="085df-163">Välj den **rekommendationer** panelen på instrumentpanelen ASC.</span><span class="sxs-lookup"><span data-stu-id="085df-163">Select the **Recommendations** tile on the ASC dashboard.</span></span>

2. <span data-ttu-id="085df-164">Visa rekommendationer som visas i tabellformat där varje rad utgör en rekommendation.</span><span class="sxs-lookup"><span data-stu-id="085df-164">View the recommendations, which are shown in a table format where each line represents one recommendation.</span></span>

3. <span data-ttu-id="085df-165">Filtrera rekommendationer, Välj **Filter** och välj allvarlighetsgrad och status värdena som du vill se.</span><span class="sxs-lookup"><span data-stu-id="085df-165">To filter recommendations, select **Filter** and select the severity and state values you wish to see.</span></span>

4. <span data-ttu-id="085df-166">För att stänga en rekommendation inte är tillämplig du högerklicka och välj **Ignorera.**</span><span class="sxs-lookup"><span data-stu-id="085df-166">To dismiss a recommendation that is not applicable, you can right click and select **Dismiss.**</span></span>

5. <span data-ttu-id="085df-167">Utvärdera vilka rekommendation tillämpas först.</span><span class="sxs-lookup"><span data-stu-id="085df-167">Evaluate which recommendation should be applied first.</span></span>

6. <span data-ttu-id="085df-168">Tillämpa rekommendationerna i prioritetsordning.</span><span class="sxs-lookup"><span data-stu-id="085df-168">Apply the recommendations in order of priority.</span></span>

<span data-ttu-id="085df-169">En lista över möjliga rekommendationer och praktiska på hur du använder varje finns [hantera säkerhetsrekommendationer i Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-recommendations)</span><span class="sxs-lookup"><span data-stu-id="085df-169">For a list of possible recommendations and walk-throughs on how to apply each, see [Managing security recommendations in Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-recommendations)</span></span>

### <a name="detection-and-response"></a><span data-ttu-id="085df-170">Identifiering och svar</span><span class="sxs-lookup"><span data-stu-id="085df-170">Detection and Response</span></span>

<span data-ttu-id="085df-171">Identifiering och svaret hör ihop, som du vill att svara så snabbt som möjligt efter ett hot har identifierats.</span><span class="sxs-lookup"><span data-stu-id="085df-171">Detection and response go together, as you want to respond as quickly as possible after a threat is detected.</span></span>
<span data-ttu-id="085df-172">Hotidentifiering ASC fungerar genom att automatiskt samla in säkerhetsinformation från resurserna i Azure, nätverket och anslutna partnerlösningar.</span><span class="sxs-lookup"><span data-stu-id="085df-172">ASC threat detection works by automatically collecting security information from your Azure resources, the network, and connected partner solutions.</span></span> <span data-ttu-id="085df-173">ASC kan snabbt uppdateras som angripare släpper nya och allt mer avancerade kryphål.</span><span class="sxs-lookup"><span data-stu-id="085df-173">ASC can rapidly update its detection algorithms as attackers release new and increasingly sophisticated exploits.</span></span> <span data-ttu-id="085df-174">Mer detaljerad information om hur ASCS hotidentifiering fungerar finns [identifieringsfunktionerna i Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-detection-capabilities).</span><span class="sxs-lookup"><span data-stu-id="085df-174">For more detailed information on how ASC’s threat detection works, see [Azure Security Center detection capabilities](https://docs.microsoft.com/azure/security-center/security-center-detection-capabilities).</span></span>

#### <a name="how-do-i-manage-and-respond-to-security-alerts"></a><span data-ttu-id="085df-175">Hur hanterar jag och åtgärdar säkerhetsaviseringar?</span><span class="sxs-lookup"><span data-stu-id="085df-175">How do I manage and respond to security alerts?</span></span>

<span data-ttu-id="085df-176">En lista med prioritetssorterade säkerhetsaviseringar visas i Security Center tillsammans med den information du behöver att snabbt undersöka problemet.</span><span class="sxs-lookup"><span data-stu-id="085df-176">A list of prioritized security alerts is shown in Security Center along with the information you need to quickly investigate the problem.</span></span> <span data-ttu-id="085df-177">Security Center innehåller också rekommendationer för hur du kan avhjälpa angrepp.</span><span class="sxs-lookup"><span data-stu-id="085df-177">Security Center also includes recommendations for how to remediate an attack.</span></span> <span data-ttu-id="085df-178">Om du vill hantera din säkerhetsaviseringar, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="085df-178">To manage your security alerts, do the following:</span></span>

1. <span data-ttu-id="085df-179">Välj den **säkerhetsaviseringar** panelen i ASC instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="085df-179">Select the **Security alerts** tile in the ASC dashboard.</span></span> <span data-ttu-id="085df-180">Detta visar information för varje avisering.</span><span class="sxs-lookup"><span data-stu-id="085df-180">This shows details for each alert.</span></span>

2. <span data-ttu-id="085df-181">Om du vill filtrera aviseringar baserat på datum, status och allvarlighetsgrad, Välj **Filter** och sedan välja de värden som du vill se.</span><span class="sxs-lookup"><span data-stu-id="085df-181">To filter alerts based on date, state, and severity, select **Filter** and then select the values you want to see.</span></span>

3. <span data-ttu-id="085df-182">För att svara på en avisering, markerar du den och granska informationen, och välj sedan den angripna resursen.</span><span class="sxs-lookup"><span data-stu-id="085df-182">To respond to an alert, select it and review the information, then select the resource that was attacked.</span></span>

4. <span data-ttu-id="085df-183">I den **beskrivning** fältet visas information, inklusive rekommenderade åtgärderna.</span><span class="sxs-lookup"><span data-stu-id="085df-183">In the **Description** field, you’ll see details, including recommended remediation.</span></span>

![](media/protect-personal-data-azure-security-center/security-alerts.png)

<span data-ttu-id="085df-184">Mer detaljerad information om åtgärda säkerhetsaviseringar finns [hantera och åtgärda säkerhetsaviseringar i Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts)</span><span class="sxs-lookup"><span data-stu-id="085df-184">For more detailed instructions on responding to security alerts, see [Managing and responding to security alerts in Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts)</span></span>

<span data-ttu-id="085df-185">För ytterligare hjälp i undersöker säkerhetsaviseringar företaget kan integrera ASC aviseringar med sin egen SIEM-lösning med hjälp av [Azure Log-integrering](https://aka.ms/AzLog).</span><span class="sxs-lookup"><span data-stu-id="085df-185">For further help in investigating security alerts, the company can integrate ASC alerts with its own SIEM solution, using [Azure Log Integration](https://aka.ms/AzLog).</span></span>

#### <a name="how-do-i-manage-security-incidents"></a><span data-ttu-id="085df-186">Hur hanterar säkerhetsincidenter?</span><span class="sxs-lookup"><span data-stu-id="085df-186">How do I manage security incidents?</span></span>

<span data-ttu-id="085df-187">Är en sammanställning av alla aviseringar för en resurs som överensstämmer med kill kedjan mönster i ASC, en säkerhetsincident.</span><span class="sxs-lookup"><span data-stu-id="085df-187">In ASC, a security incident is an aggregation of all alerts for a resource that align with kill chain patterns.</span></span> <span data-ttu-id="085df-188">En incident visar en lista över relaterade aviseringar så att du kan få mer information om varje förekomst.</span><span class="sxs-lookup"><span data-stu-id="085df-188">An Incident will reveal the list of related alerts, which enables you to obtain more information about each occurrence.</span></span> <span data-ttu-id="085df-189">Incidenter visas i rutan med säkerhetsaviseringar och bladet.</span><span class="sxs-lookup"><span data-stu-id="085df-189">Incidents appear in the Security Alerts tile and blade.</span></span>

<span data-ttu-id="085df-190">Granska och hantera säkerhetsincidenter genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="085df-190">To review and manage security incidents, do the following:</span></span>

1. <span data-ttu-id="085df-191">Välj den **säkerhetsaviseringar** panelen.</span><span class="sxs-lookup"><span data-stu-id="085df-191">Select the **Security alerts** tile.</span></span> <span data-ttu-id="085df-192">Om en säkerhetsincident har upptäckts, visas den under säkerhet aviseringar diagrammet.</span><span class="sxs-lookup"><span data-stu-id="085df-192">if a security incident is detected, it will appear under the security alerts graph.</span></span> <span data-ttu-id="085df-193">Har en ikon som skiljer sig från andra aviseringar.</span><span class="sxs-lookup"><span data-stu-id="085df-193">It will have an icon that’s different from other alerts.</span></span>

2. <span data-ttu-id="085df-194">Välj objekt att visa mer information om den här säkerhetsincident.</span><span class="sxs-lookup"><span data-stu-id="085df-194">Select the incident to see more details about this security incident.</span></span> <span data-ttu-id="085df-195">Ytterligare information innehåller en fullständig beskrivning, dess allvarlighetsgrad, det aktuella tillståndet, angripna resursen, reparationssteg för incidenten och aviseringar som ingår i den här incidenten.</span><span class="sxs-lookup"><span data-stu-id="085df-195">Additional details include its full description, its severity, its current state, the attacked resource, the remediation steps for the incident, and the alerts that were included in this incident.</span></span>

<span data-ttu-id="085df-196">Du kan filtrera om du vill se **incidenter endast**, **aviseringar endast**, eller **båda**.</span><span class="sxs-lookup"><span data-stu-id="085df-196">You can filter to see **incidents only**, **alerts only**, or **both**.</span></span>

#### <a name="how-do-i-access-the-threat-intelligence-report"></a><span data-ttu-id="085df-197">Hur kommer jag åt hot Intelligence rapporten?</span><span class="sxs-lookup"><span data-stu-id="085df-197">How do I access the Threat Intelligence Report?</span></span>

<span data-ttu-id="085df-198">ASC analyserar information från flera källor för att identifiera hot.</span><span class="sxs-lookup"><span data-stu-id="085df-198">ASC analyzes information from multiple sources to identify threats.</span></span> <span data-ttu-id="085df-199">För att underlätta incidenter team undersöka och åtgärda hot innehåller Security Center en hot intelligence-rapport som innehåller information om hot som har identifierats.</span><span class="sxs-lookup"><span data-stu-id="085df-199">To assist incident response teams investigate and remediate threats, Security Center includes a threat intelligence report that contains information about the threat that was detected.</span></span>

<span data-ttu-id="085df-200">Security Center har tre typer av hot rapporter som kan variera efter attack.</span><span class="sxs-lookup"><span data-stu-id="085df-200">Security Center has three types of threat reports, which can vary per attack.</span></span>
<span data-ttu-id="085df-201">De tillgängliga rapporterna är:</span><span class="sxs-lookup"><span data-stu-id="085df-201">The reports available are:</span></span>

- <span data-ttu-id="085df-202">Aktivitetsgrupp rapport: erbjuder djup dyker ned i angripare, mål och medvetande.</span><span class="sxs-lookup"><span data-stu-id="085df-202">Activity Group Report: provides deep dives into attackers, their objectives and tactics.</span></span>

- <span data-ttu-id="085df-203">Kampanj rapport: fokuserar på information om specifika attack kampanjer.</span><span class="sxs-lookup"><span data-stu-id="085df-203">Campaign Report: focuses on details of specific attack campaigns.</span></span>

- <span data-ttu-id="085df-204">Översiktsrapport för hot: omfattar alla objekt i de föregående två rapporterna.</span><span class="sxs-lookup"><span data-stu-id="085df-204">Threat Summary Report: covers all items in the previous two reports.</span></span>

<span data-ttu-id="085df-205">Den här typen av information är mycket användbar när incidenter blir där det finns en pågående undersökning för att förstå källan för angrepp angriparens motiveringen och vad du gör om du vill undvika det här problemet vidarebefordras.</span><span class="sxs-lookup"><span data-stu-id="085df-205">This type of information is very useful during the incident response process, where there is an ongoing investigation to understand the source of the attack, the attacker’s motivations, and what to do to mitigate this issue moving forward.</span></span>

1. <span data-ttu-id="085df-206">Om du vill komma åt den hot intelligence gör du följande:</span><span class="sxs-lookup"><span data-stu-id="085df-206">To access the threat intelligence report, do the following:</span></span>

2. <span data-ttu-id="085df-207">Välj den **säkerhetsaviseringar** panelen på instrumentpanelen ASC.</span><span class="sxs-lookup"><span data-stu-id="085df-207">Select the **Security alerts** tile on the ASC dashboard.</span></span>

3. <span data-ttu-id="085df-208">Välj säkerhetsaviseringen som du vill få mer information.</span><span class="sxs-lookup"><span data-stu-id="085df-208">Select the security alert for which you want to obtain more information.</span></span>

4. <span data-ttu-id="085df-209">I den **rapporter** fältet, klicka på länken till hot intelligence-rapporten.</span><span class="sxs-lookup"><span data-stu-id="085df-209">In the **Reports** field, click the link to the threat intelligence report.</span></span>

5. <span data-ttu-id="085df-210">PDF-fil som du kan hämta öppnas.</span><span class="sxs-lookup"><span data-stu-id="085df-210">This will open the PDF file, which you can download.</span></span>

![](media/protect-personal-data-azure-security-center/security-alerts-suspicious-process.png)

<span data-ttu-id="085df-211">Mer information om ASC hot intelligence rapporten finns [Azure Security Center hot Intelligence-rapporten.](https://docs.microsoft.com/azure/security-center/security-center-threat-report)</span><span class="sxs-lookup"><span data-stu-id="085df-211">For additional information about the ASC threat intelligence report, see [Azure Security Center Threat Intelligence Report.](https://docs.microsoft.com/azure/security-center/security-center-threat-report)</span></span>

### <a name="assessment"></a><span data-ttu-id="085df-212">Bedömning</span><span class="sxs-lookup"><span data-stu-id="085df-212">Assessment</span></span>

<span data-ttu-id="085df-213">Om du vill hjälpa till med testning, bedömning och utvärdering av din säkerhetstillståndet, ger ASC för inbyggt säkerhetsproblem med Qualys molnet agenter som en del av den virtuella datorn rekommendationer komponenten.</span><span class="sxs-lookup"><span data-stu-id="085df-213">To help with testing, assessment and evaluation of your security posture, ASC provides for integrated vulnerability assessment with Qualys cloud agents, as a part of its virtual machine recommendations component.</span></span>

<span data-ttu-id="085df-214">Qualys agenten rapporterar säkerhetsproblem data till hanteringsplattform Qualys som sedan skickar säkerhetsproblem och hälsoövervakning data tillbaka till ASC.</span><span class="sxs-lookup"><span data-stu-id="085df-214">The Qualys agent reports vulnerability data to the Qualys management platform, which then sends vulnerability and health monitoring data back to ASC.</span></span> <span data-ttu-id="085df-215">Rekommendationen att lägga till en vulnerability assessment lösning visas i den **rekommendationer** bladet på instrumentpanelen ASC.</span><span class="sxs-lookup"><span data-stu-id="085df-215">The recommendation to add a vulnerability assessment solution is displayed in the **Recommendations** blade on the ASC dashboard.</span></span>

<span data-ttu-id="085df-216">När du har installerat lösningen för sårbarhetsbedömning på den virtuella måldatorn, genomsöker Security Center VM:en för att identifiera sårbarheter med systemet och med program.</span><span class="sxs-lookup"><span data-stu-id="085df-216">After the vulnerability assessment solution is installed on the target VM, Security Center scans the VM to detect and identify system and application vulnerabilities.</span></span> <span data-ttu-id="085df-217">Identifierade problem visas under alternativet **rekommendationer för virtuella datorer**.</span><span class="sxs-lookup"><span data-stu-id="085df-217">Detected issues are shown under the **Virtual Machines Recommendations** option.</span></span>

#### <a name="how-do-i-implement-a-vulnerability-assessment-solution"></a><span data-ttu-id="085df-218">Hur jag för att implementera en lösning för vulnerability assessment?</span><span class="sxs-lookup"><span data-stu-id="085df-218">How do I implement a vulnerability assessment solution?</span></span> 

<span data-ttu-id="085df-219">Om en virtuell dator inte har ett inbyggt säkerhetsproblem assessment lösningen har redan distribuerats rekommenderar Security Center att installeras.</span><span class="sxs-lookup"><span data-stu-id="085df-219">If a Virtual Machine does not have an integrated vulnerability assessment solution already deployed, Security Center recommends that it be installed.</span></span>

1. <span data-ttu-id="085df-220">I instrumentpanelen för ASC på den **rekommendationer** bladet väljer **lägger till en vulnerability assessment lösning.**</span><span class="sxs-lookup"><span data-stu-id="085df-220">In the ASC dashboard, on the **Recommendations** blade, select **Add a vulnerability assessment solution.**</span></span>

2. <span data-ttu-id="085df-221">Välj de virtuella datorerna där du vill installera vulnerability assessment lösning.</span><span class="sxs-lookup"><span data-stu-id="085df-221">Select the VMs where you want to install the vulnerability assessment solution.</span></span>

3. <span data-ttu-id="085df-222">Klicka på **installera på virtuella datorer [antal].**</span><span class="sxs-lookup"><span data-stu-id="085df-222">Click on **Install on [number of] VMs.**</span></span>

4. <span data-ttu-id="085df-223">Markera en partnerlösning i Azure Marketplace eller under **använda befintlig lösning,** Välj **Qualys.**</span><span class="sxs-lookup"><span data-stu-id="085df-223">Select a partner solution in the Azure Marketplace, or under **Use existing solution,** select **Qualys.**</span></span>

5. <span data-ttu-id="085df-224">Du kan aktivera inställningar för automatisk uppdatering eller inaktivera den **partnerlösningar** bladet.</span><span class="sxs-lookup"><span data-stu-id="085df-224">You can turn the auto update settings on or off in the **Partner Solutions** blade.</span></span>

<span data-ttu-id="085df-225">Ytterligare instruktioner för hur du implementerar en lösning för vulnerability assessment finns [utvärdering av säkerhetsrisker i Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations)</span><span class="sxs-lookup"><span data-stu-id="085df-225">For further instructions on how to implement a vulnerability assessment solution, see [Vulnerability Assessment in Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations)</span></span>

## <a name="next-steps"></a><span data-ttu-id="085df-226">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="085df-226">Next steps</span></span>

- [<span data-ttu-id="085df-227">Snabbstartsguide för Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="085df-227">Azure Security Center quick start guide</span></span>](https://docs.microsoft.com/azure/security-center/security-center-get-started)

- [<span data-ttu-id="085df-228">Introduktion till Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="085df-228">Introduction to Azure Security Center</span></span>](https://docs.microsoft.com/azure/security-center/security-center-intro)

- [<span data-ttu-id="085df-229">Integrera Azure Security Center-aviseringar med Azure logganalys-integration</span><span class="sxs-lookup"><span data-stu-id="085df-229">Integrating Azure Security Center alerts with Azure log integration</span></span>](https://docs.microsoft.com/azure/security-center/security-center-integrating-alerts-with-log-integration)

- [<span data-ttu-id="085df-230">Öka Azure Security Center med integrerad Vulnerability Assessment</span><span class="sxs-lookup"><span data-stu-id="085df-230">Boost Azure Security Center with Integrated Vulnerability Assessment</span></span>](https://blogs.msdn.microsoft.com/azuresecurity/2016/12/16/boost-azure-security-center-with-integrated-vulnerability-assessment/)
