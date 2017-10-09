---
title: "aaaHow toogenerate och överför HSM-skyddade nycklar för Azure Key Vault | Microsoft Docs"
description: "Använd den här artikeln toohelp du planera för generera och överföra din egen HSM-skyddade nycklar toouse med Azure Key Vault. Kallas även BYOK eller egna nycklar."
services: key-vault
documentationcenter: 
author: cabailey
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 51abafa1-812b-460f-a129-d714fdc391da
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2017
ms.author: ambapat
ms.openlocfilehash: 3bb234bd1c4b81770542ccf7110e256385ca3309
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toogenerate-and-transfer-hsm-protected-keys-for-azure-key-vault"></a><span data-ttu-id="7613c-104">Hur toogenerate och överför HSM-skyddade nycklar för Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="7613c-104">How toogenerate and transfer HSM-protected keys for Azure Key Vault</span></span>
## <a name="introduction"></a><span data-ttu-id="7613c-105">Introduktion</span><span class="sxs-lookup"><span data-stu-id="7613c-105">Introduction</span></span>
<span data-ttu-id="7613c-106">För ytterligare säkerhet när du använder Azure Key Vault kan importera eller generera nycklar i maskinvarusäkerhetsmoduler (HSM) som lämnar aldrig hello HSM gräns.</span><span class="sxs-lookup"><span data-stu-id="7613c-106">For added assurance, when you use Azure Key Vault, you can import or generate keys in hardware security modules (HSMs) that never leave hello HSM boundary.</span></span> <span data-ttu-id="7613c-107">Det här scenariot är ofta hänvisas tooas *egna nycklar*, eller BYOK.</span><span class="sxs-lookup"><span data-stu-id="7613c-107">This scenario is often referred tooas *bring your own key*, or BYOK.</span></span> <span data-ttu-id="7613c-108">hello HSM är FIPS 140-2 Level 2-verifierade.</span><span class="sxs-lookup"><span data-stu-id="7613c-108">hello HSMs are FIPS 140-2 Level 2 validated.</span></span> <span data-ttu-id="7613c-109">Azure Key Vault använder Thales nShield-familjen för HSM tooprotect dina nycklar.</span><span class="sxs-lookup"><span data-stu-id="7613c-109">Azure Key Vault uses Thales nShield family of HSMs tooprotect your keys.</span></span>

<span data-ttu-id="7613c-110">Använd hello information i det här avsnittet toohelp du planera för generera och överföra din egen HSM-skyddade nycklar toouse med Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="7613c-110">Use hello information in this topic toohelp you plan for, generate, and then transfer your own HSM-protected keys toouse with Azure Key Vault.</span></span>

<span data-ttu-id="7613c-111">Den här funktionen är inte tillgänglig för Azure Kina.</span><span class="sxs-lookup"><span data-stu-id="7613c-111">This functionality is not available for Azure China.</span></span>

> [!NOTE]
> <span data-ttu-id="7613c-112">Mer information om Azure Key Vault finns [vad är Azure Key Vault?](key-vault-whatis.md)</span><span class="sxs-lookup"><span data-stu-id="7613c-112">For more information about Azure Key Vault, see [What is Azure Key Vault?](key-vault-whatis.md)</span></span>  
>
> <span data-ttu-id="7613c-113">En komma igång-kursen, vilket innefattar att skapa nyckelvalvet för HSM-skyddade nycklar, se [Kom igång med Azure Key Vault](key-vault-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="7613c-113">For a getting started tutorial, which includes creating a key vault for HSM-protected keys, see [Get started with Azure Key Vault](key-vault-get-started.md).</span></span>
>
>

<span data-ttu-id="7613c-114">Mer information om generera och överför en HSM-skyddad nyckel över hello Internet:</span><span class="sxs-lookup"><span data-stu-id="7613c-114">More information about generating and transferring an HSM-protected key over hello Internet:</span></span>

* <span data-ttu-id="7613c-115">Du kan generera hello nyckel från en arbetsstation som offline, vilket minskar risken för angrepp hello.</span><span class="sxs-lookup"><span data-stu-id="7613c-115">You generate hello key from an offline workstation, which reduces hello attack surface.</span></span>
* <span data-ttu-id="7613c-116">hello nyckeln krypteras med en KeyExchange-nyckel (KEK), som är krypterad tills den är överförda toohello Azure Key Vault HSM.</span><span class="sxs-lookup"><span data-stu-id="7613c-116">hello key is encrypted with a Key Exchange Key (KEK), which stays encrypted until it is transferred toohello Azure Key Vault HSMs.</span></span> <span data-ttu-id="7613c-117">Endast hello krypterad version av din nyckel lämnar hello ursprungliga arbetsstationen.</span><span class="sxs-lookup"><span data-stu-id="7613c-117">Only hello encrypted version of your key leaves hello original workstation.</span></span>
* <span data-ttu-id="7613c-118">hello-verktyg anger egenskaperna för din klientnyckel som binder din nyckel toohello Azure Key Vault-säkerhetsvärlden.</span><span class="sxs-lookup"><span data-stu-id="7613c-118">hello toolset sets properties on your tenant key that binds your key toohello Azure Key Vault security world.</span></span> <span data-ttu-id="7613c-119">Så när hello Azure Key Vault HSM tagit emot och dekrypterat din nyckel, kan bara dessa HSM: er använda den.</span><span class="sxs-lookup"><span data-stu-id="7613c-119">So after hello Azure Key Vault HSMs receive and decrypt your key, only these HSMs can use it.</span></span> <span data-ttu-id="7613c-120">Din nyckel kan inte exporteras.</span><span class="sxs-lookup"><span data-stu-id="7613c-120">Your key cannot be exported.</span></span> <span data-ttu-id="7613c-121">Den här bindningen är påtvingad av hello Thales HSM: er.</span><span class="sxs-lookup"><span data-stu-id="7613c-121">This binding is enforced by hello Thales HSMs.</span></span>
* <span data-ttu-id="7613c-122">Hej KeyExchange-nyckel (KEK) som används tooencrypt din nyckel genereras inuti hello Azure Key Vault HSM och kan inte exporteras.</span><span class="sxs-lookup"><span data-stu-id="7613c-122">hello Key Exchange Key (KEK) that is used tooencrypt your key is generated inside hello Azure Key Vault HSMs and is not exportable.</span></span> <span data-ttu-id="7613c-123">hello HSM: erna genomdriver att det kan finnas någon klartextversion av hello KEK utanför hello HSM.</span><span class="sxs-lookup"><span data-stu-id="7613c-123">hello HSMs enforce that there can be no clear version of hello KEK outside hello HSMs.</span></span> <span data-ttu-id="7613c-124">Dessutom innehåller hello verktygsuppsättningen intyg från Thales om att hello KEK inte kan exporteras och genereras inuti en äkta HSM som har tillverkats av Thales.</span><span class="sxs-lookup"><span data-stu-id="7613c-124">In addition, hello toolset includes attestation from Thales that hello KEK is not exportable and was generated inside a genuine HSM that was manufactured by Thales.</span></span>
* <span data-ttu-id="7613c-125">hello verktygsuppsättningen innehåller intyg från Thales om att hello Azure Key Vault säkerhetsvärld genererades i en äkta HSM tillverkad av Thales.</span><span class="sxs-lookup"><span data-stu-id="7613c-125">hello toolset includes attestation from Thales that hello Azure Key Vault security world was also generated on a genuine HSM manufactured by Thales.</span></span> <span data-ttu-id="7613c-126">Detta bevisar tooyou att Microsoft använder äkta maskinvara.</span><span class="sxs-lookup"><span data-stu-id="7613c-126">This attestation proves tooyou that Microsoft is using genuine hardware.</span></span>
* <span data-ttu-id="7613c-127">Microsoft använder separata KeyExchange-nycklar och separata Säkerhetsvärldar i varje geografisk region.</span><span class="sxs-lookup"><span data-stu-id="7613c-127">Microsoft uses separate KEKs and separate Security Worlds in each geographical region.</span></span> <span data-ttu-id="7613c-128">Den här separationen säkerställer att din nyckel kan användas i datacenter i hello region där den krypterades.</span><span class="sxs-lookup"><span data-stu-id="7613c-128">This separation ensures that your key can be used only in data centers in hello region in which you encrypted it.</span></span> <span data-ttu-id="7613c-129">Exempelvis kan en nyckel från en europeisk kund inte användas i datacenter i Nordamerika eller Asien.</span><span class="sxs-lookup"><span data-stu-id="7613c-129">For example, a key from a European customer cannot be used in data centers in North American or Asia.</span></span>

## <a name="more-information-about-thales-hsms-and-microsoft-services"></a><span data-ttu-id="7613c-130">Mer information om Thales HSM: er och Microsoft-tjänster</span><span class="sxs-lookup"><span data-stu-id="7613c-130">More information about Thales HSMs and Microsoft services</span></span>
<span data-ttu-id="7613c-131">Thales e-Security är en ledande global leverantör av kryptering av data och cyber security lösningar toohello finansiella tjänster, avancerad teknik, tillverkning, myndigheter och informationsteknik.</span><span class="sxs-lookup"><span data-stu-id="7613c-131">Thales e-Security is a leading global provider of data encryption and cyber security solutions toohello financial services, high technology, manufacturing, government, and technology sectors.</span></span> <span data-ttu-id="7613c-132">Thales lösningar som används av fyra hello fem största energi- och aerospace företag med en 40 år spåra post för att skydda företagets och myndigheter information.</span><span class="sxs-lookup"><span data-stu-id="7613c-132">With a 40-year track record of protecting corporate and government information, Thales solutions are used by four of hello five largest energy and aerospace companies.</span></span> <span data-ttu-id="7613c-133">Sina lösningar används också av 22 NATO-länder och skyddar mer än 80 procent av transaktioner över hela världen.</span><span class="sxs-lookup"><span data-stu-id="7613c-133">Their solutions are also used by 22 NATO countries, and secure more than 80 per cent of worldwide payment transactions.</span></span>

<span data-ttu-id="7613c-134">Microsoft har samarbetat med Thales tooenhance hello tillstånd bilder för HSM.</span><span class="sxs-lookup"><span data-stu-id="7613c-134">Microsoft has collaborated with Thales tooenhance hello state of art for HSMs.</span></span> <span data-ttu-id="7613c-135">Dessa förbättringar kan du tooget hello vanliga fördelarna med värdtjänster utan att behöva lämna ifrån dig kontrollen över dina nycklar.</span><span class="sxs-lookup"><span data-stu-id="7613c-135">These enhancements enable you tooget hello typical benefits of hosted services without relinquishing control over your keys.</span></span> <span data-ttu-id="7613c-136">Mer specifikt att förbättringarna Microsoft hanterar hello HSM: er så att du inte behöver.</span><span class="sxs-lookup"><span data-stu-id="7613c-136">Specifically, these enhancements let Microsoft manage hello HSMs so that you do not have to.</span></span> <span data-ttu-id="7613c-137">Som ett moln som tjänsten, Azure Key Vault därmed utökas med kort varsel toomeet organisationens användning ger spikar i diagrammet.</span><span class="sxs-lookup"><span data-stu-id="7613c-137">As a cloud service, Azure Key Vault scales up at short notice toomeet your organization’s usage spikes.</span></span> <span data-ttu-id="7613c-138">AT hello samma time, skyddas din nyckel i Microsofts HSM: du behålla kontrollen över hello livscykel eftersom du generera hello nyckel och överför den Toomicrosoft's HSM.</span><span class="sxs-lookup"><span data-stu-id="7613c-138">At hello same time, your key is protected inside Microsoft’s HSMs: You retain control over hello key lifecycle because you generate hello key and transfer it tooMicrosoft’s HSMs.</span></span>

## <a name="implementing-bring-your-own-key-byok-for-azure-key-vault"></a><span data-ttu-id="7613c-139">Implementera byok (BYOK) för Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="7613c-139">Implementing bring your own key (BYOK) for Azure Key Vault</span></span>
<span data-ttu-id="7613c-140">Använd hello följande information och procedurer om du ska skapa egna HSM-skyddad nyckel och sedan överföra tooAzure Key Vault – hello ta med din egen nyckel (BYOK)-scenario.</span><span class="sxs-lookup"><span data-stu-id="7613c-140">Use hello following information and procedures if you will generate your own HSM-protected key and then transfer it tooAzure Key Vault—hello bring your own key (BYOK) scenario.</span></span>

## <a name="prerequisites-for-byok"></a><span data-ttu-id="7613c-141">Krav för BYOK</span><span class="sxs-lookup"><span data-stu-id="7613c-141">Prerequisites for BYOK</span></span>
<span data-ttu-id="7613c-142">Se hello i den följande tabellen för en lista över förutsättningarna för att överföra din egen nyckel (BYOK) för Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="7613c-142">See hello following table for a list of prerequisites for bring your own key (BYOK) for Azure Key Vault.</span></span>

| <span data-ttu-id="7613c-143">Krav</span><span class="sxs-lookup"><span data-stu-id="7613c-143">Requirement</span></span> | <span data-ttu-id="7613c-144">Mer information</span><span class="sxs-lookup"><span data-stu-id="7613c-144">More information</span></span> |
| --- | --- |
| <span data-ttu-id="7613c-145">En prenumeration tooAzure</span><span class="sxs-lookup"><span data-stu-id="7613c-145">A subscription tooAzure</span></span> |<span data-ttu-id="7613c-146">toocreate ett Azure Key Vault du behöver en Azure-prenumeration: [registrera dig för en kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/)</span><span class="sxs-lookup"><span data-stu-id="7613c-146">toocreate an Azure Key Vault, you need an Azure subscription: [Sign up for free trial](https://azure.microsoft.com/pricing/free-trial/)</span></span> |
| <span data-ttu-id="7613c-147">hello Azure Key Vault Premium-nivån toosupport HSM-skyddad för tjänsten</span><span class="sxs-lookup"><span data-stu-id="7613c-147">hello Azure Key Vault Premium service tier toosupport HSM-protected keys</span></span> |<span data-ttu-id="7613c-148">Mer information om hello tjänstnivåer och funktioner för Azure Key Vault finns hello [priser för Azure Key Vault](https://azure.microsoft.com/pricing/details/key-vault/) webbplats.</span><span class="sxs-lookup"><span data-stu-id="7613c-148">For more information about hello service tiers and capabilities for Azure Key Vault, see hello [Azure Key Vault Pricing](https://azure.microsoft.com/pricing/details/key-vault/) website.</span></span> |
| <span data-ttu-id="7613c-149">Thales HSM, smartkort och hjälpprogram</span><span class="sxs-lookup"><span data-stu-id="7613c-149">Thales HSM, smartcards, and support software</span></span> |<span data-ttu-id="7613c-150">Du måste ha åtkomst till tooa maskinvarusäkerhetsmodul och grundläggande operativa kunskaper om Thales HSM: er.</span><span class="sxs-lookup"><span data-stu-id="7613c-150">You must have access tooa Thales Hardware Security Module and basic operational knowledge of Thales HSMs.</span></span> <span data-ttu-id="7613c-151">Se [maskinvarusäkerhetsmodul](https://www.thales-esecurity.com/msrms/buy) hello lista över kompatibla modeller eller toopurchase en HSM om du inte har någon.</span><span class="sxs-lookup"><span data-stu-id="7613c-151">See [Thales Hardware Security Module](https://www.thales-esecurity.com/msrms/buy) for hello list of compatible models, or toopurchase an HSM if you do not have one.</span></span> |
| <span data-ttu-id="7613c-152">Hej följande maskinvara och programvara:</span><span class="sxs-lookup"><span data-stu-id="7613c-152">hello following hardware and software:</span></span><ol><li><span data-ttu-id="7613c-153">Ett offline x64 arbetsstation med minst Windows-operativsystemet Windows 7 och Thales nShield-programvara som är minst version 11.50.</span><span class="sxs-lookup"><span data-stu-id="7613c-153">An offline x64 workstation with a minimum Windows operation system of Windows 7 and Thales nShield software that is at least version 11.50.</span></span><br/><br/><span data-ttu-id="7613c-154">Om den här arbetsstationen kör Windows 7, måste du [installera Microsoft .NET Framework 4.5](http://download.microsoft.com/download/b/a/4/ba4a7e71-2906-4b2d-a0e1-80cf16844f5f/dotnetfx45_full_x86_x64.exe).</span><span class="sxs-lookup"><span data-stu-id="7613c-154">If this workstation runs Windows 7, you must [install Microsoft .NET Framework 4.5](http://download.microsoft.com/download/b/a/4/ba4a7e71-2906-4b2d-a0e1-80cf16844f5f/dotnetfx45_full_x86_x64.exe).</span></span></li><li><span data-ttu-id="7613c-155">En arbetsstation som är anslutna toohello Internet och har en minsta Windows-operativsystemet Windows 7 och [Azure PowerShell](/powershell/azure/overview) **minimiversionen 1.1.0** installerad.</span><span class="sxs-lookup"><span data-stu-id="7613c-155">A workstation that is connected toohello Internet and has a minimum Windows operating system of Windows 7 and [Azure PowerShell](/powershell/azure/overview) **minimum version 1.1.0** installed.</span></span></li><li><span data-ttu-id="7613c-156">En USB-enhet eller annan bärbar lagringsenhet som har minst 16 MB ledigt utrymme.</span><span class="sxs-lookup"><span data-stu-id="7613c-156">A USB drive or other portable storage device that has at least 16 MB free space.</span></span></li></ol> |<span data-ttu-id="7613c-157">Av säkerhetsskäl rekommenderar vi att hello första arbetsstationen inte är anslutna tooa nätverk.</span><span class="sxs-lookup"><span data-stu-id="7613c-157">For security reasons, we recommend that hello first workstation is not connected tooa network.</span></span> <span data-ttu-id="7613c-158">Men är den här rekommendationen inte programmässigt tvingande.</span><span class="sxs-lookup"><span data-stu-id="7613c-158">However, this recommendation is not programmatically enforced.</span></span><br/><br/><span data-ttu-id="7613c-159">Observera att den här arbetsstationen i hello instruktionerna som följer är refererad tooas hello frånkopplade arbetsstationen.</span><span class="sxs-lookup"><span data-stu-id="7613c-159">Note that in hello instructions that follow, this workstation is referred tooas hello disconnected workstation.</span></span></p></blockquote><br/><span data-ttu-id="7613c-160">Om din klientnyckel är avsedd för ett produktionsnätverk, rekommenderar vi dessutom att du använder en andra, separat arbetsstation toodownload hello verktygen och överföra hello klientnyckel.</span><span class="sxs-lookup"><span data-stu-id="7613c-160">In addition, if your tenant key is for a production network, we recommend that you use a second, separate workstation toodownload hello toolset and upload hello tenant key.</span></span> <span data-ttu-id="7613c-161">Men i testsyfte kan du använda hello samma arbetsstation som hello första.</span><span class="sxs-lookup"><span data-stu-id="7613c-161">But for testing purposes, you can use hello same workstation as hello first one.</span></span><br/><br/><span data-ttu-id="7613c-162">Observera att den här andra arbetsstationen i hello instruktionerna som följer är refererad tooas hello Internetanslutna arbetsstationen.</span><span class="sxs-lookup"><span data-stu-id="7613c-162">Note that in hello instructions that follow, this second workstation is referred tooas hello Internet-connected workstation.</span></span></p></blockquote><br/> |

## <a name="generate-and-transfer-your-key-tooazure-key-vault-hsm"></a><span data-ttu-id="7613c-163">Generera och överför din nyckel tooAzure Key Vault HSM</span><span class="sxs-lookup"><span data-stu-id="7613c-163">Generate and transfer your key tooAzure Key Vault HSM</span></span>
<span data-ttu-id="7613c-164">Du vill använda hello följande fem steg toogenerate och överför din nyckel tooan Azure Key Vault HSM:</span><span class="sxs-lookup"><span data-stu-id="7613c-164">You will use hello following five steps toogenerate and transfer your key tooan Azure Key Vault HSM:</span></span>

* [<span data-ttu-id="7613c-165">Steg 1: Förbered din Internetanslutna arbetsstation</span><span class="sxs-lookup"><span data-stu-id="7613c-165">Step 1: Prepare your Internet-connected workstation</span></span>](#step-1-prepare-your-internet-connected-workstation)
* [<span data-ttu-id="7613c-166">Steg 2: Förbered din frånkopplade arbetsstation</span><span class="sxs-lookup"><span data-stu-id="7613c-166">Step 2: Prepare your disconnected workstation</span></span>](#step-2-prepare-your-disconnected-workstation)
* [<span data-ttu-id="7613c-167">Steg 3: Skapa din nyckel</span><span class="sxs-lookup"><span data-stu-id="7613c-167">Step 3: Generate your key</span></span>](#step-3-generate-your-key)
* [<span data-ttu-id="7613c-168">Steg 4: Förbereda din nyckel för överföring</span><span class="sxs-lookup"><span data-stu-id="7613c-168">Step 4: Prepare your key for transfer</span></span>](#step-4-prepare-your-key-for-transfer)
* [<span data-ttu-id="7613c-169">Steg 5: Överför din nyckel tooAzure Key Vault</span><span class="sxs-lookup"><span data-stu-id="7613c-169">Step 5: Transfer your key tooAzure Key Vault</span></span>](#step-5-transfer-your-key-to-azure-key-vault)

## <a name="step-1-prepare-your-internet-connected-workstation"></a><span data-ttu-id="7613c-170">Steg 1: Förbered din Internetanslutna arbetsstation</span><span class="sxs-lookup"><span data-stu-id="7613c-170">Step 1: Prepare your Internet-connected workstation</span></span>
<span data-ttu-id="7613c-171">I den här första steget hello följande procedurer på en arbetsstation som är anslutna toohello Internet.</span><span class="sxs-lookup"><span data-stu-id="7613c-171">For this first step, do hello following procedures on your workstation that is connected toohello Internet.</span></span>

### <a name="step-11-install-azure-powershell"></a><span data-ttu-id="7613c-172">Steg 1.1: Installera Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="7613c-172">Step 1.1: Install Azure PowerShell</span></span>
<span data-ttu-id="7613c-173">Hämta och installera hello Azure PowerShell-modulen som innehåller hello cmdlets toomanage Azure Key Vault från hello Internetanslutna arbetsstationen.</span><span class="sxs-lookup"><span data-stu-id="7613c-173">From hello Internet-connected workstation, download and install hello Azure PowerShell module that includes hello cmdlets toomanage Azure Key Vault.</span></span> <span data-ttu-id="7613c-174">Detta kräver en lägsta version av 0.8.13.</span><span class="sxs-lookup"><span data-stu-id="7613c-174">This requires a minimum version of 0.8.13.</span></span>

<span data-ttu-id="7613c-175">Installationsanvisningar finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="7613c-175">For installation instructions, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

### <a name="step-12-get-your-azure-subscription-id"></a><span data-ttu-id="7613c-176">Steg 1.2: Hämta ditt Azure prenumerations-ID</span><span class="sxs-lookup"><span data-stu-id="7613c-176">Step 1.2: Get your Azure subscription ID</span></span>
<span data-ttu-id="7613c-177">Starta en Azure PowerShell-session och logga in tooyour Azure-konto med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="7613c-177">Start an Azure PowerShell session and sign in tooyour Azure account by using hello following command:</span></span>

        Add-AzureAccount
<span data-ttu-id="7613c-178">Ange ditt användarnamn för Azure-konto och lösenord i hello popup-webbläsarfönstret.</span><span class="sxs-lookup"><span data-stu-id="7613c-178">In hello pop-up browser window, enter your Azure account user name and password.</span></span> <span data-ttu-id="7613c-179">Använd sedan hello [Get-AzureSubscription](/powershell/module/azure/get-azuresubscription?view=azuresmps-3.7.0) kommando:</span><span class="sxs-lookup"><span data-stu-id="7613c-179">Then, use hello [Get-AzureSubscription](/powershell/module/azure/get-azuresubscription?view=azuresmps-3.7.0) command:</span></span>

        Get-AzureSubscription
<span data-ttu-id="7613c-180">Hitta hello-ID för hello-prenumeration som du ska använda för Azure Key Vault från hello-utdata.</span><span class="sxs-lookup"><span data-stu-id="7613c-180">From hello output, locate hello ID for hello subscription you will use for Azure Key Vault.</span></span> <span data-ttu-id="7613c-181">Du behöver den här prenumerations-ID senare.</span><span class="sxs-lookup"><span data-stu-id="7613c-181">You will need this subscription ID later.</span></span>

<span data-ttu-id="7613c-182">Stäng inte hello Azure PowerShell-fönster.</span><span class="sxs-lookup"><span data-stu-id="7613c-182">Do not close hello Azure PowerShell window.</span></span>

### <a name="step-13-download-hello-byok-toolset-for-azure-key-vault"></a><span data-ttu-id="7613c-183">Steg 1.3: Hämta hello BYOK-verktyg för Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="7613c-183">Step 1.3: Download hello BYOK toolset for Azure Key Vault</span></span>
<span data-ttu-id="7613c-184">Gå toohello Microsoft Download Center och [hämta hello Azure Key Vault BYOK-verktygen](http://www.microsoft.com/download/details.aspx?id=45345) för ett geografiskt område eller instans av Azure.</span><span class="sxs-lookup"><span data-stu-id="7613c-184">Go toohello Microsoft Download Center and [download hello Azure Key Vault BYOK toolset](http://www.microsoft.com/download/details.aspx?id=45345) for your geographic region or instance of Azure.</span></span> <span data-ttu-id="7613c-185">Använder hello följande information tooidentify hello paketet namnet toodownload och dess motsvarande SHA-256 paketet hash:</span><span class="sxs-lookup"><span data-stu-id="7613c-185">Use hello following information tooidentify hello package name toodownload and its corresponding SHA-256 package hash:</span></span>

- - -
<span data-ttu-id="7613c-186">**USA:**</span><span class="sxs-lookup"><span data-stu-id="7613c-186">**United States:**</span></span>

<span data-ttu-id="7613c-187">KeyVault-BYOK-verktyg-Förenade States.zip</span><span class="sxs-lookup"><span data-stu-id="7613c-187">KeyVault-BYOK-Tools-UnitedStates.zip</span></span>

<span data-ttu-id="7613c-188">760EE9BD6445C87CFF0E8B032577118704B3BEAA045AA55977C10EF68BC67E2B</span><span class="sxs-lookup"><span data-stu-id="7613c-188">760EE9BD6445C87CFF0E8B032577118704B3BEAA045AA55977C10EF68BC67E2B</span></span>

- - -
<span data-ttu-id="7613c-189">**Europa:**</span><span class="sxs-lookup"><span data-stu-id="7613c-189">**Europe:**</span></span>

<span data-ttu-id="7613c-190">KeyVault-BYOK-verktyg-Europe.zip</span><span class="sxs-lookup"><span data-stu-id="7613c-190">KeyVault-BYOK-Tools-Europe.zip</span></span>

<span data-ttu-id="7613c-191">7A64B94225F59B847C5C27C2200BAD7D16C901E1687767EDBBB8B09BB285011D</span><span class="sxs-lookup"><span data-stu-id="7613c-191">7A64B94225F59B847C5C27C2200BAD7D16C901E1687767EDBBB8B09BB285011D</span></span>

- - -
<span data-ttu-id="7613c-192">**Asien:**</span><span class="sxs-lookup"><span data-stu-id="7613c-192">**Asia:**</span></span>

<span data-ttu-id="7613c-193">KeyVault-BYOK-verktyg-AsiaPacific.zip</span><span class="sxs-lookup"><span data-stu-id="7613c-193">KeyVault-BYOK-Tools-AsiaPacific.zip</span></span>

<span data-ttu-id="7613c-194">813DC94B23079CF7A5CEA71D5B444E86B292F463C53EE47AED25D4F7CD58E7D8</span><span class="sxs-lookup"><span data-stu-id="7613c-194">813DC94B23079CF7A5CEA71D5B444E86B292F463C53EE47AED25D4F7CD58E7D8</span></span>

- - -
<span data-ttu-id="7613c-195">**Latinamerika:**</span><span class="sxs-lookup"><span data-stu-id="7613c-195">**Latin America:**</span></span>

<span data-ttu-id="7613c-196">KeyVault-BYOK-verktyg-LatinAmerica.zip</span><span class="sxs-lookup"><span data-stu-id="7613c-196">KeyVault-BYOK-Tools-LatinAmerica.zip</span></span>

<span data-ttu-id="7613c-197">3F29069E3500F95C0E156F4B8914E1DC60C20FB64B464306A299EA5145D755C0</span><span class="sxs-lookup"><span data-stu-id="7613c-197">3F29069E3500F95C0E156F4B8914E1DC60C20FB64B464306A299EA5145D755C0</span></span>

- - -
<span data-ttu-id="7613c-198">**Japan:**</span><span class="sxs-lookup"><span data-stu-id="7613c-198">**Japan:**</span></span>

<span data-ttu-id="7613c-199">KeyVault-BYOK-verktyg-Japan.zip</span><span class="sxs-lookup"><span data-stu-id="7613c-199">KeyVault-BYOK-Tools-Japan.zip</span></span>

<span data-ttu-id="7613c-200">453FFEA2F8F410720B68B8BAC4CF79135A7F37F4E491FF840BE9E69E88A98C90</span><span class="sxs-lookup"><span data-stu-id="7613c-200">453FFEA2F8F410720B68B8BAC4CF79135A7F37F4E491FF840BE9E69E88A98C90</span></span>

- - -
<span data-ttu-id="7613c-201">**Korea:**</span><span class="sxs-lookup"><span data-stu-id="7613c-201">**Korea:**</span></span>

<span data-ttu-id="7613c-202">KeyVault-BYOK-verktyg-Korea.zip</span><span class="sxs-lookup"><span data-stu-id="7613c-202">KeyVault-BYOK-Tools-Korea.zip</span></span>

<span data-ttu-id="7613c-203">C17B7E93224DA80F5668E09CF7DAE2F92527E8226179995BBE2E43DA4323595A</span><span class="sxs-lookup"><span data-stu-id="7613c-203">C17B7E93224DA80F5668E09CF7DAE2F92527E8226179995BBE2E43DA4323595A</span></span>

- - -
<span data-ttu-id="7613c-204">**Australien:**</span><span class="sxs-lookup"><span data-stu-id="7613c-204">**Australia:**</span></span>

<span data-ttu-id="7613c-205">KeyVault-BYOK-verktyg-Australia.zip</span><span class="sxs-lookup"><span data-stu-id="7613c-205">KeyVault-BYOK-Tools-Australia.zip</span></span>

<span data-ttu-id="7613c-206">4AD893396E86F2D2A71682876A6A8EA59E3C7895BEAD2F7E7C8516682582C34B</span><span class="sxs-lookup"><span data-stu-id="7613c-206">4AD893396E86F2D2A71682876A6A8EA59E3C7895BEAD2F7E7C8516682582C34B</span></span>

- - -
[<span data-ttu-id="7613c-207">**Azure Government:**</span><span class="sxs-lookup"><span data-stu-id="7613c-207">**Azure Government:**</span></span>](https://azure.microsoft.com/features/gov/)

<span data-ttu-id="7613c-208">KeyVault-BYOK-verktyg-USGovCloud.zip</span><span class="sxs-lookup"><span data-stu-id="7613c-208">KeyVault-BYOK-Tools-USGovCloud.zip</span></span>

<span data-ttu-id="7613c-209">3AAE1A96B9D15B899B8126CFC0380719EB54FDF2EA94489B43FAD21ECC745F64</span><span class="sxs-lookup"><span data-stu-id="7613c-209">3AAE1A96B9D15B899B8126CFC0380719EB54FDF2EA94489B43FAD21ECC745F64</span></span>

- - -
<span data-ttu-id="7613c-210">**USA: S regering DOD:**</span><span class="sxs-lookup"><span data-stu-id="7613c-210">**US Government DOD:**</span></span>

<span data-ttu-id="7613c-211">KeyVault-BYOK-verktyg-USGovernmentDoD.zip</span><span class="sxs-lookup"><span data-stu-id="7613c-211">KeyVault-BYOK-Tools-USGovernmentDoD.zip</span></span>

<span data-ttu-id="7613c-212">A61E78297B0732DF2682FDE63D7B572CE4D23B0BC27CC48AFF620BD060BB9E9D</span><span class="sxs-lookup"><span data-stu-id="7613c-212">A61E78297B0732DF2682FDE63D7B572CE4D23B0BC27CC48AFF620BD060BB9E9D</span></span>

- - -
<span data-ttu-id="7613c-213">**Kanada:**</span><span class="sxs-lookup"><span data-stu-id="7613c-213">**Canada:**</span></span>

<span data-ttu-id="7613c-214">KeyVault-BYOK-verktyg-Canada.zip</span><span class="sxs-lookup"><span data-stu-id="7613c-214">KeyVault-BYOK-Tools-Canada.zip</span></span>

<span data-ttu-id="7613c-215">30B87A0BA8208F6B7241C30C794FED1C370D7445ACA179685816E4E156CD2AF7</span><span class="sxs-lookup"><span data-stu-id="7613c-215">30B87A0BA8208F6B7241C30C794FED1C370D7445ACA179685816E4E156CD2AF7</span></span>

- - -
<span data-ttu-id="7613c-216">**Tyskland:**</span><span class="sxs-lookup"><span data-stu-id="7613c-216">**Germany:**</span></span>

<span data-ttu-id="7613c-217">KeyVault-BYOK-verktyg-Germany.zip</span><span class="sxs-lookup"><span data-stu-id="7613c-217">KeyVault-BYOK-Tools-Germany.zip</span></span>

<span data-ttu-id="7613c-218">5E3E4AA54715E4F93C3C145035B18275B7C6815A06D7ABB212E7FADBF2929261</span><span class="sxs-lookup"><span data-stu-id="7613c-218">5E3E4AA54715E4F93C3C145035B18275B7C6815A06D7ABB212E7FADBF2929261</span></span>

- - -
<span data-ttu-id="7613c-219">**Indien:**</span><span class="sxs-lookup"><span data-stu-id="7613c-219">**India:**</span></span>

<span data-ttu-id="7613c-220">KeyVault-BYOK-verktyg-India.zip</span><span class="sxs-lookup"><span data-stu-id="7613c-220">KeyVault-BYOK-Tools-India.zip</span></span>

<span data-ttu-id="7613c-221">136733A6C6A71D75571BB80819B3D55A9B83CCAD5C996C686BC5682A3F369BF7</span><span class="sxs-lookup"><span data-stu-id="7613c-221">136733A6C6A71D75571BB80819B3D55A9B83CCAD5C996C686BC5682A3F369BF7</span></span>

- - -
<span data-ttu-id="7613c-222">**Storbritannien:**</span><span class="sxs-lookup"><span data-stu-id="7613c-222">**United Kingdom:**</span></span>

<span data-ttu-id="7613c-223">KeyVault-BYOK-verktyg-UnitedKingdom.zip</span><span class="sxs-lookup"><span data-stu-id="7613c-223">KeyVault-BYOK-Tools-UnitedKingdom.zip</span></span>

<span data-ttu-id="7613c-224">ED331A6F1D34A402317D3F27D5396046AF0E5C2D44B5D10CCCE293472942D268</span><span class="sxs-lookup"><span data-stu-id="7613c-224">ED331A6F1D34A402317D3F27D5396046AF0E5C2D44B5D10CCCE293472942D268</span></span>

- - -

<span data-ttu-id="7613c-225">toovalidate hello integriteten hos dina hämtade BYOK-verktyg från Azure PowerShell-sessionen, Använd hello [Get-FileHash](https://technet.microsoft.com/library/dn520872.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="7613c-225">toovalidate hello integrity of your downloaded BYOK toolset, from your Azure PowerShell session, use hello [Get-FileHash](https://technet.microsoft.com/library/dn520872.aspx) cmdlet.</span></span>

    Get-FileHash KeyVault-BYOK-Tools-*.zip

<span data-ttu-id="7613c-226">hello verktygsuppsättningen innehåller hello följande:</span><span class="sxs-lookup"><span data-stu-id="7613c-226">hello toolset includes hello following:</span></span>

* <span data-ttu-id="7613c-227">Ett paket för KeyExchange-nyckel (KEK) som har ett namn som börjar med **BYOK-KEK - pkg-.**</span><span class="sxs-lookup"><span data-stu-id="7613c-227">A Key Exchange Key (KEK) package that has a name beginning with **BYOK-KEK-pkg-.**</span></span>
* <span data-ttu-id="7613c-228">Ett säkerhetsvärldspaket som har ett namn som börjar med **BYOK-SecurityWorld - pkg-.**</span><span class="sxs-lookup"><span data-stu-id="7613c-228">A Security World package that has a name beginning with **BYOK-SecurityWorld-pkg-.**</span></span>
* <span data-ttu-id="7613c-229">Python-skriptet **verifykeypackage.py.**</span><span class="sxs-lookup"><span data-stu-id="7613c-229">A python script named **verifykeypackage.py.**</span></span>
* <span data-ttu-id="7613c-230">En kommandorad körbara filen **KeyTransferRemote.exe** och tillhörande DLL-filer.</span><span class="sxs-lookup"><span data-stu-id="7613c-230">A command-line executable file named **KeyTransferRemote.exe** and associated DLLs.</span></span>
* <span data-ttu-id="7613c-231">Ett Visual C++ Redistributable Package med namnet **vcredist_x64.exe.**</span><span class="sxs-lookup"><span data-stu-id="7613c-231">A Visual C++ Redistributable Package, named **vcredist_x64.exe.**</span></span>

<span data-ttu-id="7613c-232">Kopiera hello paketet tooa USB-enhet eller annan bärbar lagringsenhet.</span><span class="sxs-lookup"><span data-stu-id="7613c-232">Copy hello package tooa USB drive or other portable storage.</span></span>

## <a name="step-2-prepare-your-disconnected-workstation"></a><span data-ttu-id="7613c-233">Steg 2: Förbered din frånkopplade arbetsstation</span><span class="sxs-lookup"><span data-stu-id="7613c-233">Step 2: Prepare your disconnected workstation</span></span>
<span data-ttu-id="7613c-234">För den här andra steget hello följande procedurer hello arbetsstation som inte är anslutna tooa nätverk (hello Internet eller ditt interna nätverk).</span><span class="sxs-lookup"><span data-stu-id="7613c-234">For this second step, do hello following procedures on hello workstation that is not connected tooa network (either hello Internet or your internal network).</span></span>

### <a name="step-21-prepare-hello-disconnected-workstation-with-thales-hsm"></a><span data-ttu-id="7613c-235">Steg 2.1: Förbered hello frånkopplade arbetsstationen med Thales HSM</span><span class="sxs-lookup"><span data-stu-id="7613c-235">Step 2.1: Prepare hello disconnected workstation with Thales HSM</span></span>
<span data-ttu-id="7613c-236">Installera programvara hello nCipher (Thales) på en Windows-dator och sedan koppla en Thales HSM toothat dator.</span><span class="sxs-lookup"><span data-stu-id="7613c-236">Install hello nCipher (Thales) support software on a Windows computer, and then attach a Thales HSM toothat computer.</span></span>

<span data-ttu-id="7613c-237">Se till att hello Thales-verktygen finns i din sökväg (**%nfast_home%\bin**).</span><span class="sxs-lookup"><span data-stu-id="7613c-237">Ensure that hello Thales tools are in your path (**%nfast_home%\bin**).</span></span> <span data-ttu-id="7613c-238">Skriv till exempel hello följande:</span><span class="sxs-lookup"><span data-stu-id="7613c-238">For example, type hello following:</span></span>

        set PATH=%PATH%;"%nfast_home%\bin"

<span data-ttu-id="7613c-239">Mer information finns i hello användarhandboken som medföljer hello Thales HSM.</span><span class="sxs-lookup"><span data-stu-id="7613c-239">For more information, see hello user guide included with hello Thales HSM.</span></span>

### <a name="step-22-install-hello-byok-toolset-on-hello-disconnected-workstation"></a><span data-ttu-id="7613c-240">Steg 2.2: Installera hello BYOK-verktygen på hello frånkopplade arbetsstationen</span><span class="sxs-lookup"><span data-stu-id="7613c-240">Step 2.2: Install hello BYOK toolset on hello disconnected workstation</span></span>
<span data-ttu-id="7613c-241">Kopiera hello BYOK-verktygspaketet från hello USB-enhet eller annan bärbar lagringsenhet och sedan hello följande:</span><span class="sxs-lookup"><span data-stu-id="7613c-241">Copy hello BYOK toolset package from hello USB drive or other portable storage, and then do hello following:</span></span>

1. <span data-ttu-id="7613c-242">Extrahera hello filer från hello hämtade paketet till en annan mapp.</span><span class="sxs-lookup"><span data-stu-id="7613c-242">Extract hello files from hello downloaded package into any folder.</span></span>
2. <span data-ttu-id="7613c-243">Kör vcredist_x64.exe från denna mapp.</span><span class="sxs-lookup"><span data-stu-id="7613c-243">From that folder, run vcredist_x64.exe.</span></span>
3. <span data-ttu-id="7613c-244">Följ instruktionerna för hello toohello installerar hello Visual C++-körtidskomponenter för Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="7613c-244">Follow hello instructions toohello install hello Visual C++ runtime components for Visual Studio 2013.</span></span>

## <a name="step-3-generate-your-key"></a><span data-ttu-id="7613c-245">Steg 3: Skapa din nyckel</span><span class="sxs-lookup"><span data-stu-id="7613c-245">Step 3: Generate your key</span></span>
<span data-ttu-id="7613c-246">För det här tredje steget hello följande procedurer på hello frånkopplade arbetsstationen.</span><span class="sxs-lookup"><span data-stu-id="7613c-246">For this third step, do hello following procedures on hello disconnected workstation.</span></span> <span data-ttu-id="7613c-247">toocomplete det här steget din HSM måste vara i läget för initiering.</span><span class="sxs-lookup"><span data-stu-id="7613c-247">toocomplete this step your HSM must be in initialization mode.</span></span> 


### <a name="step-31-change-hello-hsm-mode-tooi"></a><span data-ttu-id="7613c-248">Steg 3.1: Ändra hello HSM läge too'I'</span><span class="sxs-lookup"><span data-stu-id="7613c-248">Step 3.1: Change hello HSM mode too'I'</span></span>
<span data-ttu-id="7613c-249">Om du använder Thales nShield gränsen, toochange hello läge: 1.</span><span class="sxs-lookup"><span data-stu-id="7613c-249">If you are using Thales nShield Edge, toochange hello mode: 1.</span></span> <span data-ttu-id="7613c-250">Använd hello läge knappen toohighlight hello krävs läge.</span><span class="sxs-lookup"><span data-stu-id="7613c-250">Use hello Mode button toohighlight hello required mode.</span></span> <span data-ttu-id="7613c-251">2.</span><span class="sxs-lookup"><span data-stu-id="7613c-251">2.</span></span> <span data-ttu-id="7613c-252">Tryck och håll hello Rensa-knapp för några sekunder inom några sekunder.</span><span class="sxs-lookup"><span data-stu-id="7613c-252">Within a few seconds, press and hold hello Clear button for a couple of seconds.</span></span> <span data-ttu-id="7613c-253">Om hello läge ändras hello nya läget Indikator slutar blinka och fortsätter att lysa.</span><span class="sxs-lookup"><span data-stu-id="7613c-253">If hello mode changes, hello new mode’s LED stops flashing and remains lit.</span></span> <span data-ttu-id="7613c-254">hello Status Indikator kan flash oregelbundet under några sekunder och sedan blinkar regelbundet när hello enheten är klar.</span><span class="sxs-lookup"><span data-stu-id="7613c-254">hello Status LED might flash irregularly for a few seconds and then flashes regularly when hello device is ready.</span></span> <span data-ttu-id="7613c-255">Annars hello enheten fortfarande hello aktuellt läge med hello lämpliga läge Indikator upplysta.</span><span class="sxs-lookup"><span data-stu-id="7613c-255">Otherwise, hello device remains in hello current mode, with hello appropriate mode LED lit.</span></span>

### <a name="step-32-create-a-security-world"></a><span data-ttu-id="7613c-256">Steg 3.2: Skapa en säkerhetsvärld</span><span class="sxs-lookup"><span data-stu-id="7613c-256">Step 3.2: Create a security world</span></span>
<span data-ttu-id="7613c-257">Starta en kommandotolk och kör hello Thales nya världen program.</span><span class="sxs-lookup"><span data-stu-id="7613c-257">Start a command prompt and run hello Thales new-world program.</span></span>

    new-world.exe --initialize --cipher-suite=DLf1024s160mRijndael --module=1 --acs-quorum=2/3

<span data-ttu-id="7613c-258">Det här programmet skapar en **Säkerhetsvärld** filen i % NFAST_KMDATA%\local\world, vilket motsvarar toohello C:\ProgramData\nCipher\Key Management Data\local mapp.</span><span class="sxs-lookup"><span data-stu-id="7613c-258">This program creates a **Security World** file at %NFAST_KMDATA%\local\world, which corresponds toohello C:\ProgramData\nCipher\Key Management Data\local folder.</span></span> <span data-ttu-id="7613c-259">Du kan använda olika värden för hello kvorum men i vårt exempel är tooenter ange tre tomma kort och PIN-koder för varje kriterium.</span><span class="sxs-lookup"><span data-stu-id="7613c-259">You can use different values for hello quorum but in our example, you’re prompted tooenter three blank cards and pins for each one.</span></span> <span data-ttu-id="7613c-260">Sedan ger två valfria kort fullständig åtkomst toohello säkerhetsvärlden.</span><span class="sxs-lookup"><span data-stu-id="7613c-260">Then, any two cards give full access toohello security world.</span></span> <span data-ttu-id="7613c-261">Dessa kort blir hello **Administratörskortsuppsättningen** för hello nya säkerhetsvärlden.</span><span class="sxs-lookup"><span data-stu-id="7613c-261">These cards become hello **Administrator Card Set** for hello new security world.</span></span>

<span data-ttu-id="7613c-262">Sedan hello följande:</span><span class="sxs-lookup"><span data-stu-id="7613c-262">Then do hello following:</span></span>

* <span data-ttu-id="7613c-263">Säkerhetskopiera hello world-filen.</span><span class="sxs-lookup"><span data-stu-id="7613c-263">Back up hello world file.</span></span> <span data-ttu-id="7613c-264">Säkra och skydda hello world-fil, hello Administratörskort och sina PIN-koder och se till att ingen enskild person får åtkomst toomore än ett kort.</span><span class="sxs-lookup"><span data-stu-id="7613c-264">Secure and protect hello world file, hello Administrator Cards, and their pins, and make sure that no single person has access toomore than one card.</span></span>

### <a name="step-33-change-hello-hsm-mode-tooo"></a><span data-ttu-id="7613c-265">Steg 3.3: Ändra hello HSM läge too'O'</span><span class="sxs-lookup"><span data-stu-id="7613c-265">Step 3.3: Change hello HSM mode too'O'</span></span>
<span data-ttu-id="7613c-266">Om du använder Thales nShield gränsen, toochange hello läge: 1.</span><span class="sxs-lookup"><span data-stu-id="7613c-266">If you are using Thales nShield Edge, toochange hello mode: 1.</span></span> <span data-ttu-id="7613c-267">Använd hello läge knappen toohighlight hello krävs läge.</span><span class="sxs-lookup"><span data-stu-id="7613c-267">Use hello Mode button toohighlight hello required mode.</span></span> <span data-ttu-id="7613c-268">2.</span><span class="sxs-lookup"><span data-stu-id="7613c-268">2.</span></span> <span data-ttu-id="7613c-269">Tryck och håll hello Rensa-knapp för några sekunder inom några sekunder.</span><span class="sxs-lookup"><span data-stu-id="7613c-269">Within a few seconds, press and hold hello Clear button for a couple of seconds.</span></span> <span data-ttu-id="7613c-270">Om hello läge ändras hello nya läget Indikator slutar blinka och fortsätter att lysa.</span><span class="sxs-lookup"><span data-stu-id="7613c-270">If hello mode changes, hello new mode’s LED stops flashing and remains lit.</span></span> <span data-ttu-id="7613c-271">hello Status Indikator kan flash oregelbundet under några sekunder och sedan blinkar regelbundet när hello enheten är klar.</span><span class="sxs-lookup"><span data-stu-id="7613c-271">hello Status LED might flash irregularly for a few seconds and then flashes regularly when hello device is ready.</span></span> <span data-ttu-id="7613c-272">Annars hello enheten fortfarande hello aktuellt läge med hello lämpliga läge Indikator upplysta.</span><span class="sxs-lookup"><span data-stu-id="7613c-272">Otherwise, hello device remains in hello current mode, with hello appropriate mode LED lit.</span></span>


### <a name="step-34-validate-hello-downloaded-package"></a><span data-ttu-id="7613c-273">Steg 3.4: Verifiera hello hämtade paketet</span><span class="sxs-lookup"><span data-stu-id="7613c-273">Step 3.4: Validate hello downloaded package</span></span>
<span data-ttu-id="7613c-274">Det här steget är valfritt men rekommenderas så att du kan validera hello följande:</span><span class="sxs-lookup"><span data-stu-id="7613c-274">This step is optional but recommended so that you can validate hello following:</span></span>

* <span data-ttu-id="7613c-275">Hej KeyExchange-nyckeln som ingår i hello verktygsuppsättningen har genererats från en äkta Thales HSM.</span><span class="sxs-lookup"><span data-stu-id="7613c-275">hello Key Exchange Key that is included in hello toolset has been generated from a genuine Thales HSM.</span></span>
* <span data-ttu-id="7613c-276">hello-hash för hello Säkerhetsvärld som ingår i hello verktygsuppsättningen har genererats från en äkta Thales HSM.</span><span class="sxs-lookup"><span data-stu-id="7613c-276">hello hash of hello Security World that is included in hello toolset has been generated in a genuine Thales HSM.</span></span>
* <span data-ttu-id="7613c-277">Hej KeyExchange-nyckel är inte kan exporteras.</span><span class="sxs-lookup"><span data-stu-id="7613c-277">hello Key Exchange Key is non-exportable.</span></span>

> [!NOTE]
> <span data-ttu-id="7613c-278">toovalidate hello hämtade paketet, hello HSM måste vara ansluten, påslagen, och måste ha en säkerhetsvärld (till exempel hello något du nyss skapat).</span><span class="sxs-lookup"><span data-stu-id="7613c-278">toovalidate hello downloaded package, hello HSM must be connected, powered on, and must have a security world on it (such as hello one you’ve just created).</span></span>
>
>

<span data-ttu-id="7613c-279">toovalidate hello hämtade paketet:</span><span class="sxs-lookup"><span data-stu-id="7613c-279">toovalidate hello downloaded package:</span></span>

1. <span data-ttu-id="7613c-280">Kör skript för hello verifykeypackage.py genom att skriva en hello följande, beroende på din geografiskt område eller en instans av Azure:</span><span class="sxs-lookup"><span data-stu-id="7613c-280">Run hello verifykeypackage.py script by typing one of hello following, depending on your geographic region or instance of Azure:</span></span>

   * <span data-ttu-id="7613c-281">För Nordamerika:</span><span class="sxs-lookup"><span data-stu-id="7613c-281">For North America:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-NA-1 -w BYOK-SecurityWorld-pkg-NA-1
   * <span data-ttu-id="7613c-282">För Europa:</span><span class="sxs-lookup"><span data-stu-id="7613c-282">For Europe:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-EU-1 -w BYOK-SecurityWorld-pkg-EU-1
   * <span data-ttu-id="7613c-283">För Asien:</span><span class="sxs-lookup"><span data-stu-id="7613c-283">For Asia:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-AP-1 -w BYOK-SecurityWorld-pkg-AP-1
   * <span data-ttu-id="7613c-284">För Latinamerika:</span><span class="sxs-lookup"><span data-stu-id="7613c-284">For Latin America:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-LATAM-1 -w BYOK-SecurityWorld-pkg-LATAM-1
   * <span data-ttu-id="7613c-285">För Japan:</span><span class="sxs-lookup"><span data-stu-id="7613c-285">For Japan:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-JPN-1 -w BYOK-SecurityWorld-pkg-JPN-1
   * <span data-ttu-id="7613c-286">För Korea:</span><span class="sxs-lookup"><span data-stu-id="7613c-286">For Korea:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-KOREA-1 -w BYOK-SecurityWorld-pkg-KOREA-1
   * <span data-ttu-id="7613c-287">För Australien:</span><span class="sxs-lookup"><span data-stu-id="7613c-287">For Australia:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-AUS-1 -w BYOK-SecurityWorld-pkg-AUS-1
   * <span data-ttu-id="7613c-288">För [Azure Government](https://azure.microsoft.com/features/gov/), som använder hello US government instans av Azure:</span><span class="sxs-lookup"><span data-stu-id="7613c-288">For [Azure Government](https://azure.microsoft.com/features/gov/), which uses hello US government instance of Azure:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-USGOV-1 -w BYOK-SecurityWorld-pkg-USGOV-1
   * <span data-ttu-id="7613c-289">För USA: S regering DOD:</span><span class="sxs-lookup"><span data-stu-id="7613c-289">For US Government DOD:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-USDOD-1 -w BYOK-SecurityWorld-pkg-USDOD-1
   * <span data-ttu-id="7613c-290">För Kanada:</span><span class="sxs-lookup"><span data-stu-id="7613c-290">For Canada:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-CANADA-1 -w BYOK-SecurityWorld-pkg-CANADA-1
   * <span data-ttu-id="7613c-291">Tyskland:</span><span class="sxs-lookup"><span data-stu-id="7613c-291">For Germany:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-GERMANY-1 -w BYOK-SecurityWorld-pkg-GERMANY-1
   * <span data-ttu-id="7613c-292">För Indien:</span><span class="sxs-lookup"><span data-stu-id="7613c-292">For India:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-INDIA-1 -w BYOK-SecurityWorld-pkg-INDIA-1
     > [!TIP]
     > <span data-ttu-id="7613c-293">Hej Thales-programvaran innehåller python på %NFAST_HOME%\python\bin</span><span class="sxs-lookup"><span data-stu-id="7613c-293">hello Thales software includes python at %NFAST_HOME%\python\bin</span></span>
     >
     >
2. <span data-ttu-id="7613c-294">Bekräfta att du ser hello följande, vilket betyder att valideringen har lyckats: **resultat: lyckades**</span><span class="sxs-lookup"><span data-stu-id="7613c-294">Confirm that you see hello following, which indicates successful validation: **Result: SUCCESS**</span></span>

<span data-ttu-id="7613c-295">Det här skriptet validerar hello undertecknarkedjan upp toohello Thales rotnyckel.</span><span class="sxs-lookup"><span data-stu-id="7613c-295">This script validates hello signer chain up toohello Thales root key.</span></span> <span data-ttu-id="7613c-296">hello-hash för den här rotnyckeln är inbäddad i hello skript och dess värde bör vara **59178a47 de508c3f 291277ee 184f46c4 f1d9c639**.</span><span class="sxs-lookup"><span data-stu-id="7613c-296">hello hash of this root key is embedded in hello script and its value should be **59178a47 de508c3f 291277ee 184f46c4 f1d9c639**.</span></span> <span data-ttu-id="7613c-297">Du kan också bekräfta det här värdet separat genom att besöka hello [Thales webbplats](http://www.thalesesec.com/).</span><span class="sxs-lookup"><span data-stu-id="7613c-297">You can also confirm this value separately by visiting hello [Thales website](http://www.thalesesec.com/).</span></span>

<span data-ttu-id="7613c-298">Nu är du redo toocreate en ny nyckel.</span><span class="sxs-lookup"><span data-stu-id="7613c-298">You’re now ready toocreate a new key.</span></span>

### <a name="step-35-create-a-new-key"></a><span data-ttu-id="7613c-299">Steg 3.5: Skapa en ny nyckel</span><span class="sxs-lookup"><span data-stu-id="7613c-299">Step 3.5: Create a new key</span></span>
<span data-ttu-id="7613c-300">Generera en nyckel med hjälp av hello Thales **generatekey** program.</span><span class="sxs-lookup"><span data-stu-id="7613c-300">Generate a key by using hello Thales **generatekey** program.</span></span>

<span data-ttu-id="7613c-301">Kör följande toogenerate hello tangent hello:</span><span class="sxs-lookup"><span data-stu-id="7613c-301">Run hello following command toogenerate hello key:</span></span>

    generatekey --generate simple type=RSA size=2048 protect=module ident=contosokey plainname=contosokey nvram=no pubexp=

<span data-ttu-id="7613c-302">Följ dessa instruktioner när du kör det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="7613c-302">When you run this command, use these instructions:</span></span>

* <span data-ttu-id="7613c-303">hello parametern *skydda* måste anges värdet toohello **modulen**, som visas.</span><span class="sxs-lookup"><span data-stu-id="7613c-303">hello parameter *protect* must be set toohello value **module**, as shown.</span></span> <span data-ttu-id="7613c-304">Detta skapar en modulskyddad nyckel.</span><span class="sxs-lookup"><span data-stu-id="7613c-304">This creates a module-protected key.</span></span> <span data-ttu-id="7613c-305">Hej BYOK-verktygen har inte stöd för OCS-skyddade nycklar.</span><span class="sxs-lookup"><span data-stu-id="7613c-305">hello BYOK toolset does not support OCS-protected keys.</span></span>
* <span data-ttu-id="7613c-306">Ersätt hello värdet för *contosokey* för hello **ident** och **plainname** med valfritt strängvärde.</span><span class="sxs-lookup"><span data-stu-id="7613c-306">Replace hello value of *contosokey* for hello **ident** and **plainname** with any string value.</span></span> <span data-ttu-id="7613c-307">toominimize administrativa kostnaderna och minska hello risken för fel, rekommenderar vi att du använder hello samma värde för båda.</span><span class="sxs-lookup"><span data-stu-id="7613c-307">toominimize administrative overheads and reduce hello risk of errors, we recommend that you use hello same value for both.</span></span> <span data-ttu-id="7613c-308">Hej **ident** värdet måste innehålla endast siffror, bindestreck och gemener.</span><span class="sxs-lookup"><span data-stu-id="7613c-308">hello **ident** value must contain only numbers, dashes, and lower case letters.</span></span>
* <span data-ttu-id="7613c-309">Hej pubexp lämnas tomt (standard) i det här exemplet, men du kan ange specifika värden.</span><span class="sxs-lookup"><span data-stu-id="7613c-309">hello pubexp is left blank (default) in this example, but you can specify specific values.</span></span> <span data-ttu-id="7613c-310">Mer information finns i hello Thales-dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="7613c-310">For more information, see hello Thales documentation.</span></span>

<span data-ttu-id="7613c-311">Det här kommandot skapar en fil för Tokeniserad nyckel i mappen %NFAST_KMDATA%\local med ett namn som börjar med **key_simple_**, följt av hello **ident** som angavs i hello-kommandot.</span><span class="sxs-lookup"><span data-stu-id="7613c-311">This command creates a Tokenized Key file in your %NFAST_KMDATA%\local folder with a name starting with **key_simple_**, followed by hello **ident** that was specified in hello command.</span></span> <span data-ttu-id="7613c-312">Till exempel: **key_simple_contosokey**.</span><span class="sxs-lookup"><span data-stu-id="7613c-312">For example: **key_simple_contosokey**.</span></span> <span data-ttu-id="7613c-313">Den här filen innehåller en krypterad nyckel.</span><span class="sxs-lookup"><span data-stu-id="7613c-313">This file contains an encrypted key.</span></span>

<span data-ttu-id="7613c-314">Säkerhetskopiera filen för Tokeniserad nyckel på en säker plats.</span><span class="sxs-lookup"><span data-stu-id="7613c-314">Back up this Tokenized Key File in a safe location.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7613c-315">När du senare överför din nyckel tooAzure Key Vault kan inte Microsoft exportera den här nyckeln tillbaka tooyou så att det är mycket viktigt att säkerhetskopiera din nyckel och säkerhetsvärld på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="7613c-315">When you later transfer your key tooAzure Key Vault, Microsoft cannot export this key back tooyou so it becomes extremely important that you back up your key and security world safely.</span></span> <span data-ttu-id="7613c-316">Kontakta Thales för vägledning och bästa praxis för säkerhetskopiering av din nyckel.</span><span class="sxs-lookup"><span data-stu-id="7613c-316">Contact Thales for guidance and best practices for backing up your key.</span></span>
>
>

<span data-ttu-id="7613c-317">Du är nu redo tootransfer din nyckel tooAzure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="7613c-317">You are now ready tootransfer your key tooAzure Key Vault.</span></span>

## <a name="step-4-prepare-your-key-for-transfer"></a><span data-ttu-id="7613c-318">Steg 4: Förbereda din nyckel för överföring</span><span class="sxs-lookup"><span data-stu-id="7613c-318">Step 4: Prepare your key for transfer</span></span>
<span data-ttu-id="7613c-319">För det här fjärde steget hello följande procedurer på hello frånkopplade arbetsstationen.</span><span class="sxs-lookup"><span data-stu-id="7613c-319">For this fourth step, do hello following procedures on hello disconnected workstation.</span></span>

### <a name="step-41-create-a-copy-of-your-key-with-reduced-permissions"></a><span data-ttu-id="7613c-320">Steg 4.1: Skapa en kopia av din nyckel med minskade behörigheter</span><span class="sxs-lookup"><span data-stu-id="7613c-320">Step 4.1: Create a copy of your key with reduced permissions</span></span>

<span data-ttu-id="7613c-321">Öppna en ny kommandotolk och ändra hello aktuella toohello katalogplatsen där du uppackade hello BYOK zip-filen.</span><span class="sxs-lookup"><span data-stu-id="7613c-321">Open a new command prompt and change hello current directory toohello location where you unzipped hello BYOK zip file.</span></span> <span data-ttu-id="7613c-322">tooreduce hello behörigheter på din nyckel från en kommandotolk kör något av hello följande, beroende på din geografiskt område eller en instans av Azure:</span><span class="sxs-lookup"><span data-stu-id="7613c-322">tooreduce hello permissions on your key, from a command prompt, run one of hello following, depending on your geographic region or instance of Azure:</span></span>

* <span data-ttu-id="7613c-323">För Nordamerika:</span><span class="sxs-lookup"><span data-stu-id="7613c-323">For North America:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1
* <span data-ttu-id="7613c-324">För Europa:</span><span class="sxs-lookup"><span data-stu-id="7613c-324">For Europe:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1
* <span data-ttu-id="7613c-325">För Asien:</span><span class="sxs-lookup"><span data-stu-id="7613c-325">For Asia:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1
* <span data-ttu-id="7613c-326">För Latinamerika:</span><span class="sxs-lookup"><span data-stu-id="7613c-326">For Latin America:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1
* <span data-ttu-id="7613c-327">För Japan:</span><span class="sxs-lookup"><span data-stu-id="7613c-327">For Japan:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1
* <span data-ttu-id="7613c-328">För Korea:</span><span class="sxs-lookup"><span data-stu-id="7613c-328">For Korea:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-KOREA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-KOREA-1
* <span data-ttu-id="7613c-329">För Australien:</span><span class="sxs-lookup"><span data-stu-id="7613c-329">For Australia:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1
* <span data-ttu-id="7613c-330">För [Azure Government](https://azure.microsoft.com/features/gov/), som använder hello US government instans av Azure:</span><span class="sxs-lookup"><span data-stu-id="7613c-330">For [Azure Government](https://azure.microsoft.com/features/gov/), which uses hello US government instance of Azure:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1
* <span data-ttu-id="7613c-331">För USA: S regering DOD:</span><span class="sxs-lookup"><span data-stu-id="7613c-331">For US Government DOD:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USDOD-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USDOD-1
* <span data-ttu-id="7613c-332">För Kanada:</span><span class="sxs-lookup"><span data-stu-id="7613c-332">For Canada:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1
* <span data-ttu-id="7613c-333">Tyskland:</span><span class="sxs-lookup"><span data-stu-id="7613c-333">For Germany:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1
* <span data-ttu-id="7613c-334">För Indien:</span><span class="sxs-lookup"><span data-stu-id="7613c-334">For India:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1

<span data-ttu-id="7613c-335">När du kör det här kommandot ersätter *contosokey* med hello samma värde som du angav i **steg 3.5: skapa en ny nyckel** från hello [generera din nyckel](#step-3-generate-your-key) steg.</span><span class="sxs-lookup"><span data-stu-id="7613c-335">When you run this command, replace *contosokey* with hello same value you specified in **Step 3.5: Create a new key** from hello [Generate your key](#step-3-generate-your-key) step.</span></span>

<span data-ttu-id="7613c-336">Du uppmanas tooplug i dina säkerhet world admin-kort.</span><span class="sxs-lookup"><span data-stu-id="7613c-336">You are asked tooplug in your security world admin cards.</span></span>

<span data-ttu-id="7613c-337">När hello-kommandot har slutförts visas **resultat: lyckades** och hello kopia av din nyckel med minskade behörigheter finns i hello filen key_xferacid_<contosokey>.</span><span class="sxs-lookup"><span data-stu-id="7613c-337">When hello command completes, you see **Result: SUCCESS** and hello copy of your key with reduced permissions are in hello file named key_xferacId_<contosokey>.</span></span>

<span data-ttu-id="7613c-338">Du kan kontrolleras hello ACL: er med hjälp av följande kommandon med hjälp av hello Thales-verktygen:</span><span class="sxs-lookup"><span data-stu-id="7613c-338">You may inspects hello ACLS using following commands using hello Thales utilities:</span></span>

* <span data-ttu-id="7613c-339">aclprint.PY:</span><span class="sxs-lookup"><span data-stu-id="7613c-339">aclprint.py:</span></span>

        "%nfast_home%\bin\preload.exe" -m 1 -A xferacld -K contosokey "%nfast_home%\python\bin\python" "%nfast_home%\python\examples\aclprint.py"
* <span data-ttu-id="7613c-340">kmfile-dump.exe:</span><span class="sxs-lookup"><span data-stu-id="7613c-340">kmfile-dump.exe:</span></span>

        "%nfast_home%\bin\kmfile-dump.exe" "%NFAST_KMDATA%\local\key_xferacld_contosokey"
  <span data-ttu-id="7613c-341">När du kör kommandona, ersätter contosokey med samma värde som du angav i hello **steg 3.5: skapa en ny nyckel** från hello [generera din nyckel](#step-3-generate-your-key) steg.</span><span class="sxs-lookup"><span data-stu-id="7613c-341">When you run these commands, replace contosokey with hello same value you specified in **Step 3.5: Create a new key** from hello [Generate your key](#step-3-generate-your-key) step.</span></span>

### <a name="step-42-encrypt-your-key-by-using-microsofts-key-exchange-key"></a><span data-ttu-id="7613c-342">Steg 4.2: Kryptera nyckeln med hjälp av Microsofts KeyExchange-nyckel</span><span class="sxs-lookup"><span data-stu-id="7613c-342">Step 4.2: Encrypt your key by using Microsoft’s Key Exchange Key</span></span>
<span data-ttu-id="7613c-343">Kör något av följande kommandon, beroende på din geografiskt område eller en instans av Azure hello:</span><span class="sxs-lookup"><span data-stu-id="7613c-343">Run one of hello following commands, depending on your geographic region or instance of Azure:</span></span>

* <span data-ttu-id="7613c-344">För Nordamerika:</span><span class="sxs-lookup"><span data-stu-id="7613c-344">For North America:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="7613c-345">För Europa:</span><span class="sxs-lookup"><span data-stu-id="7613c-345">For Europe:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="7613c-346">För Asien:</span><span class="sxs-lookup"><span data-stu-id="7613c-346">For Asia:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="7613c-347">För Latinamerika:</span><span class="sxs-lookup"><span data-stu-id="7613c-347">For Latin America:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="7613c-348">För Japan:</span><span class="sxs-lookup"><span data-stu-id="7613c-348">For Japan:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="7613c-349">För Korea:</span><span class="sxs-lookup"><span data-stu-id="7613c-349">For Korea:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-KOREA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-KOREA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="7613c-350">För Australien:</span><span class="sxs-lookup"><span data-stu-id="7613c-350">For Australia:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="7613c-351">För [Azure Government](https://azure.microsoft.com/features/gov/), som använder hello US government instans av Azure:</span><span class="sxs-lookup"><span data-stu-id="7613c-351">For [Azure Government](https://azure.microsoft.com/features/gov/), which uses hello US government instance of Azure:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="7613c-352">För USA: S regering DOD:</span><span class="sxs-lookup"><span data-stu-id="7613c-352">For US Government DOD:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USDOD-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USDOD-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="7613c-353">För Kanada:</span><span class="sxs-lookup"><span data-stu-id="7613c-353">For Canada:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="7613c-354">Tyskland:</span><span class="sxs-lookup"><span data-stu-id="7613c-354">For Germany:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="7613c-355">För Indien:</span><span class="sxs-lookup"><span data-stu-id="7613c-355">For India:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey

<span data-ttu-id="7613c-356">Följ dessa instruktioner när du kör det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="7613c-356">When you run this command, use these instructions:</span></span>

* <span data-ttu-id="7613c-357">Ersätt *contosokey* med hello-identifierare som du använde toogenerate hello nyckeln i **steg 3.5: skapa en ny nyckel** från hello [generera din nyckel](#step-3-generate-your-key) steg.</span><span class="sxs-lookup"><span data-stu-id="7613c-357">Replace *contosokey* with hello identifier that you used toogenerate hello key in **Step 3.5: Create a new key** from hello [Generate your key](#step-3-generate-your-key) step.</span></span>
* <span data-ttu-id="7613c-358">Ersätt *SubscriptionID* med hello ID hello Azure-prenumeration som innehåller nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="7613c-358">Replace *SubscriptionID* with hello ID of hello Azure subscription that contains your key vault.</span></span> <span data-ttu-id="7613c-359">Du har hämtat värdet tidigare i **steg 1.2: hämta ditt Azure-prenumeration ID** från hello [Förbered din Internetanslutna arbetsstation](#step-1-prepare-your-internet-connected-workstation) steg.</span><span class="sxs-lookup"><span data-stu-id="7613c-359">You retrieved this value previously, in **Step 1.2: Get your Azure subscription ID** from hello [Prepare your Internet-connected workstation](#step-1-prepare-your-internet-connected-workstation) step.</span></span>
* <span data-ttu-id="7613c-360">Ersätt *ContosoFirstHSMKey* med en etikett som används för utdata-filnamn.</span><span class="sxs-lookup"><span data-stu-id="7613c-360">Replace *ContosoFirstHSMKey* with a label that is used for your output file name.</span></span>

<span data-ttu-id="7613c-361">När detta är klar visas **resultat: lyckades** och det finns en ny fil i hello aktuella mappen hello efter namnet: keytransferpackage*ContosoFirstHSMkey*.byok</span><span class="sxs-lookup"><span data-stu-id="7613c-361">When this completes successfully, it displays **Result: SUCCESS** and there is a new file in hello current folder that has hello following name: KeyTransferPackage-*ContosoFirstHSMkey*.byok</span></span>

### <a name="step-43-copy-your-key-transfer-package-toohello-internet-connected-workstation"></a><span data-ttu-id="7613c-362">Steg 4.3: Kopiera överföringen nyckeln paketet toohello Internetanslutna arbetsstationen</span><span class="sxs-lookup"><span data-stu-id="7613c-362">Step 4.3: Copy your key transfer package toohello Internet-connected workstation</span></span>
<span data-ttu-id="7613c-363">Använd en USB-enhet eller annan bärbar lagringsenhet toocopy hello utdatafilen från hello föregående steg (KeyTransferPackage ContosoFirstHSMkey.byok) tooyour Internetanslutna arbetsstationen.</span><span class="sxs-lookup"><span data-stu-id="7613c-363">Use a USB drive or other portable storage toocopy hello output file from hello previous step (KeyTransferPackage-ContosoFirstHSMkey.byok) tooyour Internet-connected workstation.</span></span>

## <a name="step-5-transfer-your-key-tooazure-key-vault"></a><span data-ttu-id="7613c-364">Steg 5: Överför din nyckel tooAzure Key Vault</span><span class="sxs-lookup"><span data-stu-id="7613c-364">Step 5: Transfer your key tooAzure Key Vault</span></span>
<span data-ttu-id="7613c-365">Använd hello på hello Internetanslutna arbetsstationen för det här sista steget, [Lägg till AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurermkeyvaultkey) cmdlet tooupload hello nyckelöverföringspaketet som du kopierade från hello frånkopplade arbetsstationen toohello Azure Key Vault HSM:</span><span class="sxs-lookup"><span data-stu-id="7613c-365">For this final step, on hello Internet-connected workstation, use hello [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurermkeyvaultkey) cmdlet tooupload hello key transfer package that you copied from hello disconnected workstation toohello Azure Key Vault HSM:</span></span>

    Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMkey' -KeyFilePath 'c:\KeyTransferPackage-ContosoFirstHSMkey.byok' -Destination 'HSM'

<span data-ttu-id="7613c-366">Om hello uppladdningen lyckas finns visas hello egenskaper hello-nyckel som du just lagt till.</span><span class="sxs-lookup"><span data-stu-id="7613c-366">If hello upload is successful, you see displayed hello properties of hello key that you just added.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7613c-367">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7613c-367">Next steps</span></span>
<span data-ttu-id="7613c-368">Du kan nu använda den här HSM-skyddad nyckel i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="7613c-368">You can now use this HSM-protected key in your key vault.</span></span> <span data-ttu-id="7613c-369">Mer information finns i hello **om du vill toouse en maskinvarusäkerhetsmodul (HSM)** avsnitt i hello [komma igång med Azure Key Vault](key-vault-get-started.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="7613c-369">For more information, see hello **If you want toouse a hardware security module (HSM)** section in hello [Getting started with Azure Key Vault](key-vault-get-started.md) tutorial.</span></span>
