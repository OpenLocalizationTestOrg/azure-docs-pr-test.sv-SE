---
title: aaaProtect personliga data med Azure Security Center | Microsoft Docs
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
ms.openlocfilehash: 8e2c893d13318392f47fa912089d52618f9e7b45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="protect-personal-data-from-breaches-and-attacks-azure-security-center"></a><span data-ttu-id="cb3b4-103">Skydda personliga data från överträdelser och attacker: Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="cb3b4-103">Protect personal data from breaches and attacks: Azure Security Center</span></span>

<span data-ttu-id="cb3b4-104">Den här artikeln hjälper dig att förstå hur toouse Azure Security Center tooprotect personliga data från överträdelser och attacker.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-104">This article will help you understand how toouse Azure Security Center tooprotect personal data from breaches and attacks.</span></span>

## <a name="scenario"></a><span data-ttu-id="cb3b4-105">Scenario</span><span class="sxs-lookup"><span data-stu-id="cb3b4-105">Scenario</span></span> 

<span data-ttu-id="cb3b4-106">Ett stort kryssning företag, sitt säte i hello USA utökar dess operations toooffer färdvägar i hello Medelhavsområdet och baltiska havet samt hello brittiska staterna.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-106">A large cruise company, headquartered in hello United States, is expanding its operations toooffer itineraries in hello Mediterranean, and Baltic seas, as well as hello British Isles.</span></span> <span data-ttu-id="cb3b4-107">toohelp i dessa åtgärder har det fått flera mindre kryssning rader i Italien, Tyskland, Danmark och hello Storbritannien</span><span class="sxs-lookup"><span data-stu-id="cb3b4-107">toohelp in those efforts, it has acquired several smaller cruise lines based in Italy, Germany, Denmark, and hello U.K.</span></span>

<span data-ttu-id="cb3b4-108">hello företag använder Microsoft Azure toostore företagsdata i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-108">hello company uses Microsoft Azure toostore corporate data in hello cloud.</span></span> <span data-ttu-id="cb3b4-109">Detta inkluderar personligt identifierbar information, till exempel namn, adresser, telefonnummer och kreditkortsinformation.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-109">This includes personal identifiable information such as names, addresses, phone numbers, and credit card information.</span></span> <span data-ttu-id="cb3b4-110">Den inkluderar också personal information såsom:</span><span class="sxs-lookup"><span data-stu-id="cb3b4-110">It also includes Human Resources information such as:</span></span>

- <span data-ttu-id="cb3b4-111">Adresser</span><span class="sxs-lookup"><span data-stu-id="cb3b4-111">Addresses</span></span>
- <span data-ttu-id="cb3b4-112">telefonnummer</span><span class="sxs-lookup"><span data-stu-id="cb3b4-112">Phone numbers</span></span>
- <span data-ttu-id="cb3b4-113">skatt ID-nummer</span><span class="sxs-lookup"><span data-stu-id="cb3b4-113">Tax identification numbers</span></span>
- <span data-ttu-id="cb3b4-114">medicinska information</span><span class="sxs-lookup"><span data-stu-id="cb3b4-114">Medical information</span></span>

<span data-ttu-id="cb3b4-115">hello kryssning rad har också en stor databas med ersättning och förmåner medlemmar.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-115">hello cruise line also maintains a large database of reward and loyalty program members.</span></span> <span data-ttu-id="cb3b4-116">Företagets anställda åtkomst hello nätverket från hello företagets fjärranslutna kontor och resa agenter finns runt hello world har åtkomst till företagsresurser för toosome.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-116">Corporate employees access hello network from hello company’s remote offices and travel agents located around hello world have access toosome company resources.</span></span>
<span data-ttu-id="cb3b4-117">Personliga data överförs över hello nätverk mellan dessa platser och hello Microsoft-datacenter.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-117">Personal data travels across hello network between these locations and hello Microsoft data center.</span></span>

## <a name="problem-statement"></a><span data-ttu-id="cb3b4-118">Problembeskrivning</span><span class="sxs-lookup"><span data-stu-id="cb3b4-118">Problem statement</span></span>

<span data-ttu-id="cb3b4-119">hello företaget är orolig hello risken för angrepp på sina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-119">hello company is concerned about hello threat of attacks on their Azure resources.</span></span> <span data-ttu-id="cb3b4-120">De vill tooprevent exponering av kunders och anställdas personliga data toounauthorized personer.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-120">They want tooprevent exposure of customers’ and employees’ personal data toounauthorized persons.</span></span> <span data-ttu-id="cb3b4-121">De vill ha hjälp med både förebyggande och svar/reparation, samt ett effektivt sätt toomonitor hello pågående säkerheten för deras molnresurser.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-121">They want guidance on both prevention and response/remediation, as well as an effective way toomonitor hello ongoing security of their cloud resources.</span></span>
<span data-ttu-id="cb3b4-122">Behöver de en stark försvarslinje mot dagens avancerade och organiserade angripare.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-122">They need a strong line of defense against today’s sophisticated and organized attackers.</span></span>

## <a name="company-goal"></a><span data-ttu-id="cb3b4-123">Företagets mål</span><span class="sxs-lookup"><span data-stu-id="cb3b4-123">Company goal</span></span>

<span data-ttu-id="cb3b4-124">En av hello företagets mål är tooensure hello sekretess kundernas och anställdas personliga data genom att skydda mot hot.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-124">One of hello company’s goals is tooensure hello privacy of customers’ and employees’ personal data by protecting it from threats.</span></span> <span data-ttu-id="cb3b4-125">En av sina mål är toorespond omedelbart toosigns för intrång toomitigate hello påverkan.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-125">One of their goals is toorespond immediately toosigns of breach toomitigate hello impact.</span></span> <span data-ttu-id="cb3b4-126">Det krävs ett sätt som tooassess hello aktuell status för säkerhet, identifiera sårbara konfigurationer och åtgärda dem.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-126">It requires a way tooassess hello current state of security, identify vulnerable configurations, and remediate them.</span></span>

## <a name="solutions"></a><span data-ttu-id="cb3b4-127">Lösningar</span><span class="sxs-lookup"><span data-stu-id="cb3b4-127">Solutions</span></span>

<span data-ttu-id="cb3b4-128">Microsoft Azure Security Center (ASC) ger en integrerad säkerhet säkerhetsövervakning och principhantering hanteringslösning.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-128">Microsoft Azure Security Center (ASC) provides an integrated security monitoring and policy management solution.</span></span> <span data-ttu-id="cb3b4-129">Lätt att använda och effektivt hot ger funktioner för förebyggande, identifiering och svar.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-129">It delivers easy-to-use and effective threat prevention, detection, and response capabilities.</span></span>

### <a name="prevention"></a><span data-ttu-id="cb3b4-130">Prevention (Skydd)</span><span class="sxs-lookup"><span data-stu-id="cb3b4-130">Prevention</span></span>

<span data-ttu-id="cb3b4-131">ASC hjälper dig att förhindra överträdelser genom att aktivera tooset säkerhetsprinciper, ger just-in-time-åtkomst och implementera säkerhetsrekommendationer.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-131">ASC helps you prevent breaches by enabling you tooset security policies, provide just-in-time access, and implement security recommendations.</span></span>

<span data-ttu-id="cb3b4-132">En säkerhetsprincip definierar hello antal kontrollfunktioner som rekommenderas för resurser inom hello angivna prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-132">A security policy defines hello set of controls recommended for resources within hello specified subscription.</span></span> <span data-ttu-id="cb3b4-133">Åtkomst kan vara används toolock ned inkommande trafik tooyour virtuella Azure-datorer att minska exponeringen tooattacks precis i tid.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-133">Just in time access can be used toolock down inbound traffic tooyour Azure VMs, reducing exposure tooattacks.</span></span> <span data-ttu-id="cb3b4-134">Säkerhetsrekommendationer skapas ASC när du har analyserat hello säkerhetstillståndet hos dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-134">Security recommendations are created by ASC after analyzing hello security state of your Azure resources.</span></span>

#### <a name="how-do-i-set-security-policies-in-asc"></a><span data-ttu-id="cb3b4-135">Hur ställer in säkerhetsprinciper i ASC?</span><span class="sxs-lookup"><span data-stu-id="cb3b4-135">How do I set security policies in ASC?</span></span>

<span data-ttu-id="cb3b4-136">Det går att ställa in särskilda säkerhetsprinciper för varje prenumeration.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-136">You can configure security policies for each subscription.</span></span> <span data-ttu-id="cb3b4-137">toomodify säkerhetspolicyn, bör du vara ägare eller deltagare i den aktuella prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-137">toomodify a security policy, you must be an owner or contributor of that subscription.</span></span> <span data-ttu-id="cb3b4-138">I hello Azure-portalen, hello följande:</span><span class="sxs-lookup"><span data-stu-id="cb3b4-138">In hello Azure portal, do hello following:</span></span>

1. <span data-ttu-id="cb3b4-139">Välj **princip** hello ASC instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-139">Select **Policy** in hello ASC dashboard.</span></span>

2. <span data-ttu-id="cb3b4-140">Välj hello prenumeration där du vill tooenable hello principen.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-140">Select hello subscription on which you want tooenable hello policy.</span></span>

3. <span data-ttu-id="cb3b4-141">Välj **skyddsprincip** tooconfigure principer per prenumeration.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-141">Choose **Prevention policy** tooconfigure policies per subscription.</span></span> <span data-ttu-id="cb3b4-142">**Samla in data från virtuella datorer** ska anges för**på.**</span><span class="sxs-lookup"><span data-stu-id="cb3b4-142">**Collect data from virtual machines** should be set too**On.**</span></span>

4. <span data-ttu-id="cb3b4-143">I hello **skyddsprincip** alternativ, Välj **på** tooenable hello säkerhetsrekommendationer som är relevanta för hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-143">In hello **Prevention policy** options, select **On** tooenable hello security recommendations that are relevant for hello subscription.</span></span>

![](media/protect-personal-data-azure-security-center/prevention-policy.png)

<span data-ttu-id="cb3b4-144">Mer detaljerade instruktioner och en förklaring av varje hello principrekommendationer som kan aktiveras finns [ställa in säkerhetsprinciper i Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-policies#set-security-policies).</span><span class="sxs-lookup"><span data-stu-id="cb3b4-144">For more detailed instructions and an explanation of each of hello policy recommendations that can be enabled, see [Set security policies in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-policies#set-security-policies).</span></span>

#### <a name="how-do-i-configure-just-in-time-access-jit"></a><span data-ttu-id="cb3b4-145">Hur konfigurerar jag precis i tid åtkomst (JIT)?</span><span class="sxs-lookup"><span data-stu-id="cb3b4-145">How do I configure Just in Time Access (JIT)?</span></span>

<span data-ttu-id="cb3b4-146">När JIT aktiveras låser Security Center ned inkommande trafik tooyour virtuella Azure-datorer genom att skapa en regel för NSG.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-146">When JIT is enabled, Security Center locks down inbound traffic tooyour Azure VMs by creating an NSG rule.</span></span> <span data-ttu-id="cb3b4-147">Du väljer hello portar på hello VM toowhich inkommande trafik ska låsas.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-147">You select hello ports on hello VM toowhich inbound traffic will be locked down.</span></span> <span data-ttu-id="cb3b4-148">toouse JIT kan hello följande:</span><span class="sxs-lookup"><span data-stu-id="cb3b4-148">toouse JIT access, do hello following:</span></span>

1. <span data-ttu-id="cb3b4-149">Välj hello **precis i tid VM åtkomst panelen** hello ASC bladet.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-149">Select hello **Just in time VM access tile** on hello ASC blade.</span></span>

2. <span data-ttu-id="cb3b4-150">Välj hello **rekommenderas** fliken.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-150">Select hello **Recommended** tab.</span></span>

3. <span data-ttu-id="cb3b4-151">Under **VMs**, Välj hello virtuella datorer som du vill tooenable.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-151">Under **VMs**, select hello VMs that you want tooenable.</span></span> <span data-ttu-id="cb3b4-152">Detta placerar en markering nästa tooa VM.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-152">This puts a checkmark next tooa VM.</span></span> 
4. <span data-ttu-id="cb3b4-153">Välj **aktivera JIT** på virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-153">Select **Enable JIT** on VMs.</span></span>
5. <span data-ttu-id="cb3b4-154">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-154">Select **Save**.</span></span>

<span data-ttu-id="cb3b4-155">Du kan se hello standardportarna som ASC rekommenderar aktiverats för JIT.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-155">Then you can see hello default ports that ASC recommends being enabled for JIT.</span></span> <span data-ttu-id="cb3b4-156">Du kan också lägga till och konfigurera en ny port som du vill använda tooenable hello precis i Tidslösning.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-156">You can also add and configure a new port on which you want tooenable hello just in time solution.</span></span> <span data-ttu-id="cb3b4-157">Hej **precis i tid VM access** panelen i hello Security Center visar hello antalet virtuella datorer som är konfigurerad för JIT-åtkomst.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-157">hello **Just in time VM access** tile in hello Security Center shows hello number of VMs configured for JIT access.</span></span> <span data-ttu-id="cb3b4-158">Här visas också hello antal godkända åtkomstbegäranden som görs i hello föregående vecka.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-158">It also shows hello number of approved access requests made in hello last week.</span></span>

![](media/protect-personal-data-azure-security-center/jit-vm.png)

<span data-ttu-id="cb3b4-159">Mer information om hur toodo, och ytterligare information om precis i Time-åtkomst finns i [hantera virtuella åtkomst med hjälp av precis i tid.](https://docs.microsoft.com/azure/security-center/security-center-just-in-time)</span><span class="sxs-lookup"><span data-stu-id="cb3b4-159">For instructions on how toodo this, and additional information about Just in Time access, see [Manage virtual machine access using just in time.](https://docs.microsoft.com/azure/security-center/security-center-just-in-time)</span></span>

#### <a name="how-do-i-implement-asc-security-recommendations"></a><span data-ttu-id="cb3b4-160">Hur jag implementera ASC säkerhetsrekommendationer?</span><span class="sxs-lookup"><span data-stu-id="cb3b4-160">How do I implement ASC security recommendations?</span></span>

<span data-ttu-id="cb3b4-161">När Security Center identifierar potentiella säkerhetsproblem skapas rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-161">When Security Center identifies potential security vulnerabilities, it creates recommendations.</span></span> <span data-ttu-id="cb3b4-162">hello rekommendationer leder dig igenom hello konfigureringen av hello behövs kontroller.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-162">hello recommendations guide you through hello process of configuring hello needed controls.</span></span> 
1. <span data-ttu-id="cb3b4-163">Välj hello **rekommendationer** rutan på hello ASC instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-163">Select hello **Recommendations** tile on hello ASC dashboard.</span></span>

2. <span data-ttu-id="cb3b4-164">Visa hello rekommendationer som visas i tabellformat där varje rad utgör en rekommendation.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-164">View hello recommendations, which are shown in a table format where each line represents one recommendation.</span></span>

3. <span data-ttu-id="cb3b4-165">Välj toofilter rekommendationer **Filter** och markera hello allvarlighetsgrad och tillstånd för värden som du vill toosee.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-165">toofilter recommendations, select **Filter** and select hello severity and state values you wish toosee.</span></span>

4. <span data-ttu-id="cb3b4-166">toodismiss en rekommendation som inte är tillämplig, kan du högerklicka och välja **Ignorera.**</span><span class="sxs-lookup"><span data-stu-id="cb3b4-166">toodismiss a recommendation that is not applicable, you can right click and select **Dismiss.**</span></span>

5. <span data-ttu-id="cb3b4-167">Utvärdera vilka rekommendation tillämpas först.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-167">Evaluate which recommendation should be applied first.</span></span>

6. <span data-ttu-id="cb3b4-168">Tillämpa hello rekommendationer i prioritetsordning.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-168">Apply hello recommendations in order of priority.</span></span>

<span data-ttu-id="cb3b4-169">En lista över möjliga rekommendationer och praktiska om hur tooapply, se [hantera säkerhetsrekommendationer i Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-recommendations)</span><span class="sxs-lookup"><span data-stu-id="cb3b4-169">For a list of possible recommendations and walk-throughs on how tooapply each, see [Managing security recommendations in Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-recommendations)</span></span>

### <a name="detection-and-response"></a><span data-ttu-id="cb3b4-170">Identifiering och svar</span><span class="sxs-lookup"><span data-stu-id="cb3b4-170">Detection and Response</span></span>

<span data-ttu-id="cb3b4-171">Identifiering och svar går tillsammans som du vill toorespond så snabbt som möjligt när ett hot har identifierats.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-171">Detection and response go together, as you want toorespond as quickly as possible after a threat is detected.</span></span>
<span data-ttu-id="cb3b4-172">Hotidentifiering ASC fungerar genom att automatiskt samla in säkerhetsinformation från Azure-resurser, hello nätverket och anslutna partnerlösningar.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-172">ASC threat detection works by automatically collecting security information from your Azure resources, hello network, and connected partner solutions.</span></span> <span data-ttu-id="cb3b4-173">ASC kan snabbt uppdateras som angripare släpper nya och allt mer avancerade kryphål.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-173">ASC can rapidly update its detection algorithms as attackers release new and increasingly sophisticated exploits.</span></span> <span data-ttu-id="cb3b4-174">Mer detaljerad information om hur ASCS hotidentifiering fungerar finns [identifieringsfunktionerna i Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-detection-capabilities).</span><span class="sxs-lookup"><span data-stu-id="cb3b4-174">For more detailed information on how ASC’s threat detection works, see [Azure Security Center detection capabilities](https://docs.microsoft.com/azure/security-center/security-center-detection-capabilities).</span></span>

#### <a name="how-do-i-manage-and-respond-toosecurity-alerts"></a><span data-ttu-id="cb3b4-175">Hur jag hanterar och åtgärdar toosecurity aviseringar?</span><span class="sxs-lookup"><span data-stu-id="cb3b4-175">How do I manage and respond toosecurity alerts?</span></span>

<span data-ttu-id="cb3b4-176">En lista med prioritetssorterade säkerhetsaviseringar visas i Security Center tillsammans med hello information du behöver tooquickly undersöka hello problem.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-176">A list of prioritized security alerts is shown in Security Center along with hello information you need tooquickly investigate hello problem.</span></span> <span data-ttu-id="cb3b4-177">Security Center innehåller också rekommendationer för hur tooremediate angreppet.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-177">Security Center also includes recommendations for how tooremediate an attack.</span></span> <span data-ttu-id="cb3b4-178">toomanage säkerheten aviseringar, göra hello följande:</span><span class="sxs-lookup"><span data-stu-id="cb3b4-178">toomanage your security alerts, do hello following:</span></span>

1. <span data-ttu-id="cb3b4-179">Välj hello **säkerhetsaviseringar** panelen i hello ASC instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-179">Select hello **Security alerts** tile in hello ASC dashboard.</span></span> <span data-ttu-id="cb3b4-180">Detta visar information för varje avisering.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-180">This shows details for each alert.</span></span>

2. <span data-ttu-id="cb3b4-181">toofilter varningar efter datum, status och allvarlighetsgrad, Välj **Filter** och välj sedan hello-värden som du vill ha toosee.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-181">toofilter alerts based on date, state, and severity, select **Filter** and then select hello values you want toosee.</span></span>

3. <span data-ttu-id="cb3b4-182">toorespond tooan avisering markerar du den och granska hello information, och välj sedan hello angripna resursen.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-182">toorespond tooan alert, select it and review hello information, then select hello resource that was attacked.</span></span>

4. <span data-ttu-id="cb3b4-183">I hello **beskrivning** fältet visas information, inklusive rekommenderade åtgärderna.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-183">In hello **Description** field, you’ll see details, including recommended remediation.</span></span>

![](media/protect-personal-data-azure-security-center/security-alerts.png)

<span data-ttu-id="cb3b4-184">Mer detaljerad information om svarar toosecurity aviseringar finns [hantering och svarar toosecurity aviseringar i Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts)</span><span class="sxs-lookup"><span data-stu-id="cb3b4-184">For more detailed instructions on responding toosecurity alerts, see [Managing and responding toosecurity alerts in Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts)</span></span>

<span data-ttu-id="cb3b4-185">För ytterligare hjälp i undersöker säkerhetsaviseringar hello företag kan integrera ASC aviseringar med sin egen SIEM-lösning med hjälp av [Azure Log-integrering](https://aka.ms/AzLog).</span><span class="sxs-lookup"><span data-stu-id="cb3b4-185">For further help in investigating security alerts, hello company can integrate ASC alerts with its own SIEM solution, using [Azure Log Integration](https://aka.ms/AzLog).</span></span>

#### <a name="how-do-i-manage-security-incidents"></a><span data-ttu-id="cb3b4-186">Hur hanterar säkerhetsincidenter?</span><span class="sxs-lookup"><span data-stu-id="cb3b4-186">How do I manage security incidents?</span></span>

<span data-ttu-id="cb3b4-187">Är en sammanställning av alla aviseringar för en resurs som överensstämmer med kill kedjan mönster i ASC, en säkerhetsincident.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-187">In ASC, a security incident is an aggregation of all alerts for a resource that align with kill chain patterns.</span></span> <span data-ttu-id="cb3b4-188">En Incident avslöja hello lista över relaterade aviseringar, vilket gör att du tooobtain mer information om varje förekomst.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-188">An Incident will reveal hello list of related alerts, which enables you tooobtain more information about each occurrence.</span></span> <span data-ttu-id="cb3b4-189">Incidenter visas i hello säkerhetsaviseringar och bladet.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-189">Incidents appear in hello Security Alerts tile and blade.</span></span>

<span data-ttu-id="cb3b4-190">tooreview och hantera säkerhetsincidenter, hello följande:</span><span class="sxs-lookup"><span data-stu-id="cb3b4-190">tooreview and manage security incidents, do hello following:</span></span>

1. <span data-ttu-id="cb3b4-191">Välj hello **säkerhetsaviseringar** panelen.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-191">Select hello **Security alerts** tile.</span></span> <span data-ttu-id="cb3b4-192">Om en säkerhetsincident har upptäckts, visas den under hello aviseringar för organisationen.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-192">if a security incident is detected, it will appear under hello security alerts graph.</span></span> <span data-ttu-id="cb3b4-193">Har en ikon som skiljer sig från andra aviseringar.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-193">It will have an icon that’s different from other alerts.</span></span>

2. <span data-ttu-id="cb3b4-194">Välj hello incident toosee mer information om den här säkerhetsincident.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-194">Select hello incident toosee more details about this security incident.</span></span> <span data-ttu-id="cb3b4-195">Ytterligare information innehåller en fullständig beskrivning, dess allvarlighetsgrad, dess aktuella tillstånd, hello angripna resursen, hello steg för hello incidenten och hello aviseringar som ingår i den här incidenten.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-195">Additional details include its full description, its severity, its current state, hello attacked resource, hello remediation steps for hello incident, and hello alerts that were included in this incident.</span></span>

<span data-ttu-id="cb3b4-196">Du kan filtrera toosee **incidenter endast**, **aviseringar endast**, eller **båda**.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-196">You can filter toosee **incidents only**, **alerts only**, or **both**.</span></span>

#### <a name="how-do-i-access-hello-threat-intelligence-report"></a><span data-ttu-id="cb3b4-197">Hur kommer jag åt hello hot Intelligence-rapporten?</span><span class="sxs-lookup"><span data-stu-id="cb3b4-197">How do I access hello Threat Intelligence Report?</span></span>

<span data-ttu-id="cb3b4-198">ASC analyserar information från flera källor tooidentify hot.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-198">ASC analyzes information from multiple sources tooidentify threats.</span></span> <span data-ttu-id="cb3b4-199">tooassist incidenter team undersöka och åtgärda hot, Security Center innehåller en hot intelligence-rapport som innehåller information om hello hot som har identifierats.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-199">tooassist incident response teams investigate and remediate threats, Security Center includes a threat intelligence report that contains information about hello threat that was detected.</span></span>

<span data-ttu-id="cb3b4-200">Security Center har tre typer av hot rapporter som kan variera efter attack.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-200">Security Center has three types of threat reports, which can vary per attack.</span></span>
<span data-ttu-id="cb3b4-201">hello-rapporter som är tillgängliga är:</span><span class="sxs-lookup"><span data-stu-id="cb3b4-201">hello reports available are:</span></span>

- <span data-ttu-id="cb3b4-202">Aktivitetsgrupp rapport: erbjuder djup dyker ned i angripare, mål och medvetande.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-202">Activity Group Report: provides deep dives into attackers, their objectives and tactics.</span></span>

- <span data-ttu-id="cb3b4-203">Kampanj rapport: fokuserar på information om specifika attack kampanjer.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-203">Campaign Report: focuses on details of specific attack campaigns.</span></span>

- <span data-ttu-id="cb3b4-204">Översiktsrapport för hot: omfattar alla objekt i hello föregående två rapporter.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-204">Threat Summary Report: covers all items in hello previous two reports.</span></span>

<span data-ttu-id="cb3b4-205">Den här typen av information är mycket användbar när hello incidenter, där det finns en pågående undersökning toounderstand hello källa hello attacker, hello angriparens motiveringen och vilka toodo toomitigate i detta fall glidande framåt.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-205">This type of information is very useful during hello incident response process, where there is an ongoing investigation toounderstand hello source of hello attack, hello attacker’s motivations, and what toodo toomitigate this issue moving forward.</span></span>

1. <span data-ttu-id="cb3b4-206">tooaccess hello hotinformation rapport, hello följande:</span><span class="sxs-lookup"><span data-stu-id="cb3b4-206">tooaccess hello threat intelligence report, do hello following:</span></span>

2. <span data-ttu-id="cb3b4-207">Välj hello **säkerhetsaviseringar** rutan på hello ASC instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-207">Select hello **Security alerts** tile on hello ASC dashboard.</span></span>

3. <span data-ttu-id="cb3b4-208">Välj hello Säkerhetsvarning som du vill tooobtain mer information.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-208">Select hello security alert for which you want tooobtain more information.</span></span>

4. <span data-ttu-id="cb3b4-209">I hello **rapporter** klickar hello länken toohello hot intelligence-rapporten.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-209">In hello **Reports** field, click hello link toohello threat intelligence report.</span></span>

5. <span data-ttu-id="cb3b4-210">Hello PDF-fil som du kan hämta öppnas.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-210">This will open hello PDF file, which you can download.</span></span>

![](media/protect-personal-data-azure-security-center/security-alerts-suspicious-process.png)

<span data-ttu-id="cb3b4-211">Mer information om hello ASC hot intelligence-rapporten finns [Azure Security Center hot Intelligence-rapporten.](https://docs.microsoft.com/azure/security-center/security-center-threat-report)</span><span class="sxs-lookup"><span data-stu-id="cb3b4-211">For additional information about hello ASC threat intelligence report, see [Azure Security Center Threat Intelligence Report.](https://docs.microsoft.com/azure/security-center/security-center-threat-report)</span></span>

### <a name="assessment"></a><span data-ttu-id="cb3b4-212">Bedömning</span><span class="sxs-lookup"><span data-stu-id="cb3b4-212">Assessment</span></span>

<span data-ttu-id="cb3b4-213">toohelp med testning, bedömning och utvärdering av din säkerhetstillståndet ASC ger för inbyggt säkerhetsproblem med Qualys molnet agenter som en del av den virtuella datorn rekommendationer komponenten.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-213">toohelp with testing, assessment and evaluation of your security posture, ASC provides for integrated vulnerability assessment with Qualys cloud agents, as a part of its virtual machine recommendations component.</span></span>

<span data-ttu-id="cb3b4-214">hello Qualys agenten rapporterar säkerhetsproblem toohello Qualys management dataplattform, som sedan skickar tillbaka tooASC säkerhetsproblem och hälsoövervakning data.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-214">hello Qualys agent reports vulnerability data toohello Qualys management platform, which then sends vulnerability and health monitoring data back tooASC.</span></span> <span data-ttu-id="cb3b4-215">Hej rekommendation tooadd en vulnerability assessment lösning visas i hello **rekommendationer** bladet på instrumentpanelen för hello ASC.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-215">hello recommendation tooadd a vulnerability assessment solution is displayed in hello **Recommendations** blade on hello ASC dashboard.</span></span>

<span data-ttu-id="cb3b4-216">När hello säkerhetsproblem lösning har installerats på hello målet VM Security Center genomsökningar hello VM toodetect och identifiera system- och säkerhetsproblem.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-216">After hello vulnerability assessment solution is installed on hello target VM, Security Center scans hello VM toodetect and identify system and application vulnerabilities.</span></span> <span data-ttu-id="cb3b4-217">Identifierat problem visas under hello **rekommendationer för virtuella datorer** alternativet.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-217">Detected issues are shown under hello **Virtual Machines Recommendations** option.</span></span>

#### <a name="how-do-i-implement-a-vulnerability-assessment-solution"></a><span data-ttu-id="cb3b4-218">Hur jag för att implementera en lösning för vulnerability assessment?</span><span class="sxs-lookup"><span data-stu-id="cb3b4-218">How do I implement a vulnerability assessment solution?</span></span> 

<span data-ttu-id="cb3b4-219">Om en virtuell dator inte har ett inbyggt säkerhetsproblem assessment lösningen har redan distribuerats rekommenderar Security Center att installeras.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-219">If a Virtual Machine does not have an integrated vulnerability assessment solution already deployed, Security Center recommends that it be installed.</span></span>

1. <span data-ttu-id="cb3b4-220">Hello ASC instrumentpanelen på hello **rekommendationer** bladet väljer **lägger till en vulnerability assessment lösning.**</span><span class="sxs-lookup"><span data-stu-id="cb3b4-220">In hello ASC dashboard, on hello **Recommendations** blade, select **Add a vulnerability assessment solution.**</span></span>

2. <span data-ttu-id="cb3b4-221">Välj hello virtuella datorer där du vill att tooinstall hello säkerhetsproblem lösning.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-221">Select hello VMs where you want tooinstall hello vulnerability assessment solution.</span></span>

3. <span data-ttu-id="cb3b4-222">Klicka på **installera på virtuella datorer [antal].**</span><span class="sxs-lookup"><span data-stu-id="cb3b4-222">Click on **Install on [number of] VMs.**</span></span>

4. <span data-ttu-id="cb3b4-223">Markera en partnerlösning i hello Azure Marketplace, eller under **använda befintlig lösning,** Välj **Qualys.**</span><span class="sxs-lookup"><span data-stu-id="cb3b4-223">Select a partner solution in hello Azure Marketplace, or under **Use existing solution,** select **Qualys.**</span></span>

5. <span data-ttu-id="cb3b4-224">Du kan aktivera hello uppdateringsinställningar för automatisk eller inaktivera i hello **partnerlösningar** bladet.</span><span class="sxs-lookup"><span data-stu-id="cb3b4-224">You can turn hello auto update settings on or off in hello **Partner Solutions** blade.</span></span>

<span data-ttu-id="cb3b4-225">Ytterligare instruktioner för hur tooimplement en lösning för vulnerability assessment, se [utvärdering av säkerhetsrisker i Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations)</span><span class="sxs-lookup"><span data-stu-id="cb3b4-225">For further instructions on how tooimplement a vulnerability assessment solution, see [Vulnerability Assessment in Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations)</span></span>

## <a name="next-steps"></a><span data-ttu-id="cb3b4-226">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cb3b4-226">Next steps</span></span>

- [<span data-ttu-id="cb3b4-227">Snabbstartsguide för Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="cb3b4-227">Azure Security Center quick start guide</span></span>](https://docs.microsoft.com/azure/security-center/security-center-get-started)

- [<span data-ttu-id="cb3b4-228">Introduktion tooAzure Security Center</span><span class="sxs-lookup"><span data-stu-id="cb3b4-228">Introduction tooAzure Security Center</span></span>](https://docs.microsoft.com/azure/security-center/security-center-intro)

- [<span data-ttu-id="cb3b4-229">Integrera Azure Security Center-aviseringar med Azure logganalys-integration</span><span class="sxs-lookup"><span data-stu-id="cb3b4-229">Integrating Azure Security Center alerts with Azure log integration</span></span>](https://docs.microsoft.com/azure/security-center/security-center-integrating-alerts-with-log-integration)

- [<span data-ttu-id="cb3b4-230">Öka Azure Security Center med integrerad Vulnerability Assessment</span><span class="sxs-lookup"><span data-stu-id="cb3b4-230">Boost Azure Security Center with Integrated Vulnerability Assessment</span></span>](https://blogs.msdn.microsoft.com/azuresecurity/2016/12/16/boost-azure-security-center-with-integrated-vulnerability-assessment/)
