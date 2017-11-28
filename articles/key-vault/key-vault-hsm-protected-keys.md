---
title: "Generera och överför HSM-skyddade nycklar för Azure Key Vault | Microsoft Docs"
description: "Använd den här artikeln för att planera för, generera och överföra din egen HSM-skyddade nycklar som ska användas med Azure Key Vault. Kallas även BYOK eller egna nycklar."
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
ms.openlocfilehash: 5dbee1221f64045c64fecb344de1e03b2183dfb1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-generate-and-transfer-hsm-protected-keys-for-azure-key-vault"></a><span data-ttu-id="45626-104">Generera och överför HSM-skyddade nycklar för Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="45626-104">How to generate and transfer HSM-protected keys for Azure Key Vault</span></span>
## <a name="introduction"></a><span data-ttu-id="45626-105">Introduktion</span><span class="sxs-lookup"><span data-stu-id="45626-105">Introduction</span></span>
<span data-ttu-id="45626-106">För ytterligare säkerhet när du använder Azure Key Vault kan importera eller generera nycklar i maskinvarusäkerhetsmoduler (HSM) som lämnar aldrig HSM-gräns.</span><span class="sxs-lookup"><span data-stu-id="45626-106">For added assurance, when you use Azure Key Vault, you can import or generate keys in hardware security modules (HSMs) that never leave the HSM boundary.</span></span> <span data-ttu-id="45626-107">Det här scenariot kallas ofta till *egna nycklar*, eller BYOK.</span><span class="sxs-lookup"><span data-stu-id="45626-107">This scenario is often referred to as *bring your own key*, or BYOK.</span></span> <span data-ttu-id="45626-108">HSM-modulerna är FIPS 140-2 Level 2-verifierade.</span><span class="sxs-lookup"><span data-stu-id="45626-108">The HSMs are FIPS 140-2 Level 2 validated.</span></span> <span data-ttu-id="45626-109">Azure Key Vault använder Thales nShield familj av HSM för att skydda dina nycklar.</span><span class="sxs-lookup"><span data-stu-id="45626-109">Azure Key Vault uses Thales nShield family of HSMs to protect your keys.</span></span>

<span data-ttu-id="45626-110">Använd informationen i det här avsnittet för att hjälpa dig att planera för, generera och överföra din egen HSM-skyddade nycklar som ska användas med Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="45626-110">Use the information in this topic to help you plan for, generate, and then transfer your own HSM-protected keys to use with Azure Key Vault.</span></span>

<span data-ttu-id="45626-111">Den här funktionen är inte tillgänglig för Azure Kina.</span><span class="sxs-lookup"><span data-stu-id="45626-111">This functionality is not available for Azure China.</span></span>

> [!NOTE]
> <span data-ttu-id="45626-112">Mer information om Azure Key Vault finns [vad är Azure Key Vault?](key-vault-whatis.md)</span><span class="sxs-lookup"><span data-stu-id="45626-112">For more information about Azure Key Vault, see [What is Azure Key Vault?](key-vault-whatis.md)</span></span>  
>
> <span data-ttu-id="45626-113">En komma igång-kursen, vilket innefattar att skapa nyckelvalvet för HSM-skyddade nycklar, se [Kom igång med Azure Key Vault](key-vault-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="45626-113">For a getting started tutorial, which includes creating a key vault for HSM-protected keys, see [Get started with Azure Key Vault](key-vault-get-started.md).</span></span>
>
>

<span data-ttu-id="45626-114">Mer information om generera och överför en HSM-skyddad nyckel via Internet:</span><span class="sxs-lookup"><span data-stu-id="45626-114">More information about generating and transferring an HSM-protected key over the Internet:</span></span>

* <span data-ttu-id="45626-115">Du kan generera nyckeln från en arbetsstation som offline, vilket minskar risken för angrepp.</span><span class="sxs-lookup"><span data-stu-id="45626-115">You generate the key from an offline workstation, which reduces the attack surface.</span></span>
* <span data-ttu-id="45626-116">Nyckeln är krypterad med en KeyExchange-nyckel (KEK), som är krypterad tills den överförs till Azure Key Vault HSM.</span><span class="sxs-lookup"><span data-stu-id="45626-116">The key is encrypted with a Key Exchange Key (KEK), which stays encrypted until it is transferred to the Azure Key Vault HSMs.</span></span> <span data-ttu-id="45626-117">Den krypterade versionen av din nyckel lämnar den ursprungliga arbetsstationen.</span><span class="sxs-lookup"><span data-stu-id="45626-117">Only the encrypted version of your key leaves the original workstation.</span></span>
* <span data-ttu-id="45626-118">Verktygsuppsättningen anger egenskaperna för din klientnyckel som binder din nyckel till Azure Key Vault-säkerhetsvärlden.</span><span class="sxs-lookup"><span data-stu-id="45626-118">The toolset sets properties on your tenant key that binds your key to the Azure Key Vault security world.</span></span> <span data-ttu-id="45626-119">Så när Azure Key Vault HSM tagit emot och dekrypterat din nyckel, kan bara dessa HSM: er använda den.</span><span class="sxs-lookup"><span data-stu-id="45626-119">So after the Azure Key Vault HSMs receive and decrypt your key, only these HSMs can use it.</span></span> <span data-ttu-id="45626-120">Din nyckel kan inte exporteras.</span><span class="sxs-lookup"><span data-stu-id="45626-120">Your key cannot be exported.</span></span> <span data-ttu-id="45626-121">Den här bindningen är påtvingad av Thales HSM: er.</span><span class="sxs-lookup"><span data-stu-id="45626-121">This binding is enforced by the Thales HSMs.</span></span>
* <span data-ttu-id="45626-122">Den KeyExchange-nyckel (KEK) som används för att kryptera nyckeln genereras i Azure Key Vault HSM och kan inte exporteras.</span><span class="sxs-lookup"><span data-stu-id="45626-122">The Key Exchange Key (KEK) that is used to encrypt your key is generated inside the Azure Key Vault HSMs and is not exportable.</span></span> <span data-ttu-id="45626-123">De HSM: erna genomdriver att det kan finnas någon klartextversion av KEK utanför HSM: er.</span><span class="sxs-lookup"><span data-stu-id="45626-123">The HSMs enforce that there can be no clear version of the KEK outside the HSMs.</span></span> <span data-ttu-id="45626-124">Dessutom innehåller verktygsuppsättningen intyg från Thales om att KEK inte kan exporteras och genereras inuti en äkta HSM som har tillverkats av Thales.</span><span class="sxs-lookup"><span data-stu-id="45626-124">In addition, the toolset includes attestation from Thales that the KEK is not exportable and was generated inside a genuine HSM that was manufactured by Thales.</span></span>
* <span data-ttu-id="45626-125">Verktygsuppsättningen innehåller intyg från Thales om att Azure Key Vault-säkerhetsvärld genererades i en äkta HSM tillverkad av Thales.</span><span class="sxs-lookup"><span data-stu-id="45626-125">The toolset includes attestation from Thales that the Azure Key Vault security world was also generated on a genuine HSM manufactured by Thales.</span></span> <span data-ttu-id="45626-126">Detta bevisar för dig att Microsoft använder äkta maskinvara.</span><span class="sxs-lookup"><span data-stu-id="45626-126">This attestation proves to you that Microsoft is using genuine hardware.</span></span>
* <span data-ttu-id="45626-127">Microsoft använder separata KeyExchange-nycklar och separata Säkerhetsvärldar i varje geografisk region.</span><span class="sxs-lookup"><span data-stu-id="45626-127">Microsoft uses separate KEKs and separate Security Worlds in each geographical region.</span></span> <span data-ttu-id="45626-128">Den här separationen säkerställer att din nyckel kan användas i datacenter i den region där den krypterades.</span><span class="sxs-lookup"><span data-stu-id="45626-128">This separation ensures that your key can be used only in data centers in the region in which you encrypted it.</span></span> <span data-ttu-id="45626-129">Exempelvis kan en nyckel från en europeisk kund inte användas i datacenter i Nordamerika eller Asien.</span><span class="sxs-lookup"><span data-stu-id="45626-129">For example, a key from a European customer cannot be used in data centers in North American or Asia.</span></span>

## <a name="more-information-about-thales-hsms-and-microsoft-services"></a><span data-ttu-id="45626-130">Mer information om Thales HSM: er och Microsoft-tjänster</span><span class="sxs-lookup"><span data-stu-id="45626-130">More information about Thales HSMs and Microsoft services</span></span>
<span data-ttu-id="45626-131">Thales e-Security är en ledande global leverantör krypteringen och cybersäkerhetslösningar till finansiella tjänster, avancerad teknik, tillverkning, myndigheter och informationsteknik.</span><span class="sxs-lookup"><span data-stu-id="45626-131">Thales e-Security is a leading global provider of data encryption and cyber security solutions to the financial services, high technology, manufacturing, government, and technology sectors.</span></span> <span data-ttu-id="45626-132">Thales lösningar som används av fyra av fem största energi- och aerospace företag med en 40 år spåra post för att skydda företagets och myndigheter information.</span><span class="sxs-lookup"><span data-stu-id="45626-132">With a 40-year track record of protecting corporate and government information, Thales solutions are used by four of the five largest energy and aerospace companies.</span></span> <span data-ttu-id="45626-133">Sina lösningar används också av 22 NATO-länder och skyddar mer än 80 procent av transaktioner över hela världen.</span><span class="sxs-lookup"><span data-stu-id="45626-133">Their solutions are also used by 22 NATO countries, and secure more than 80 per cent of worldwide payment transactions.</span></span>

<span data-ttu-id="45626-134">Microsoft har samarbetat med Thales för att göra-objekt för HSM.</span><span class="sxs-lookup"><span data-stu-id="45626-134">Microsoft has collaborated with Thales to enhance the state of art for HSMs.</span></span> <span data-ttu-id="45626-135">Dessa förbättringar kan du få de vanliga fördelarna med värdtjänster utan att behöva lämna ifrån dig kontrollen över dina nycklar.</span><span class="sxs-lookup"><span data-stu-id="45626-135">These enhancements enable you to get the typical benefits of hosted services without relinquishing control over your keys.</span></span> <span data-ttu-id="45626-136">Mer specifikt låta dessa förbättringar Microsoft hantera HSM: erna så att du inte behöver.</span><span class="sxs-lookup"><span data-stu-id="45626-136">Specifically, these enhancements let Microsoft manage the HSMs so that you do not have to.</span></span> <span data-ttu-id="45626-137">Azure Key Vault skala med kort varsel för att uppfylla din organisations användningstoppar som en molntjänst.</span><span class="sxs-lookup"><span data-stu-id="45626-137">As a cloud service, Azure Key Vault scales up at short notice to meet your organization’s usage spikes.</span></span> <span data-ttu-id="45626-138">På samma gång, skyddas din nyckel i Microsofts HSM: du behålla kontrollen över nyckelns livscykel eftersom du genererar nyckeln och överför den till Microsofts HSM: er.</span><span class="sxs-lookup"><span data-stu-id="45626-138">At the same time, your key is protected inside Microsoft’s HSMs: You retain control over the key lifecycle because you generate the key and transfer it to Microsoft’s HSMs.</span></span>

## <a name="implementing-bring-your-own-key-byok-for-azure-key-vault"></a><span data-ttu-id="45626-139">Implementera byok (BYOK) för Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="45626-139">Implementing bring your own key (BYOK) for Azure Key Vault</span></span>
<span data-ttu-id="45626-140">Använd följande information och procedurer om du ska generera en egen HSM-skyddad nyckel och sedan överföra det till Azure Key Vault – ta din egen nyckel (BYOK)-scenario.</span><span class="sxs-lookup"><span data-stu-id="45626-140">Use the following information and procedures if you will generate your own HSM-protected key and then transfer it to Azure Key Vault—the bring your own key (BYOK) scenario.</span></span>

## <a name="prerequisites-for-byok"></a><span data-ttu-id="45626-141">Krav för BYOK</span><span class="sxs-lookup"><span data-stu-id="45626-141">Prerequisites for BYOK</span></span>
<span data-ttu-id="45626-142">I tabellen nedan finns en lista över förutsättningarna för att överföra din egen nyckel (BYOK) för Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="45626-142">See the following table for a list of prerequisites for bring your own key (BYOK) for Azure Key Vault.</span></span>

| <span data-ttu-id="45626-143">Krav</span><span class="sxs-lookup"><span data-stu-id="45626-143">Requirement</span></span> | <span data-ttu-id="45626-144">Mer information</span><span class="sxs-lookup"><span data-stu-id="45626-144">More information</span></span> |
| --- | --- |
| <span data-ttu-id="45626-145">En prenumeration på Azure</span><span class="sxs-lookup"><span data-stu-id="45626-145">A subscription to Azure</span></span> |<span data-ttu-id="45626-146">Du behöver en Azure-prenumeration för att skapa ett Azure Key Vault: [registrera dig för en kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/)</span><span class="sxs-lookup"><span data-stu-id="45626-146">To create an Azure Key Vault, you need an Azure subscription: [Sign up for free trial](https://azure.microsoft.com/pricing/free-trial/)</span></span> |
| <span data-ttu-id="45626-147">Azure Key Vault Premium tjänstnivån att stödja HSM-skyddade nycklar</span><span class="sxs-lookup"><span data-stu-id="45626-147">The Azure Key Vault Premium service tier to support HSM-protected keys</span></span> |<span data-ttu-id="45626-148">Mer information om tjänstnivåer och funktioner för Azure Key Vault finns i [priser för Azure Key Vault](https://azure.microsoft.com/pricing/details/key-vault/) webbplats.</span><span class="sxs-lookup"><span data-stu-id="45626-148">For more information about the service tiers and capabilities for Azure Key Vault, see the [Azure Key Vault Pricing](https://azure.microsoft.com/pricing/details/key-vault/) website.</span></span> |
| <span data-ttu-id="45626-149">Thales HSM, smartkort och hjälpprogram</span><span class="sxs-lookup"><span data-stu-id="45626-149">Thales HSM, smartcards, and support software</span></span> |<span data-ttu-id="45626-150">Du måste ha åtkomst till en maskinvarusäkerhetsmodul och grundläggande operativa kunskaper om Thales HSM: er.</span><span class="sxs-lookup"><span data-stu-id="45626-150">You must have access to a Thales Hardware Security Module and basic operational knowledge of Thales HSMs.</span></span> <span data-ttu-id="45626-151">Se [maskinvarusäkerhetsmodul](https://www.thales-esecurity.com/msrms/buy) för listan över kompatibla modeller eller för att köpa en HSM om du inte har någon.</span><span class="sxs-lookup"><span data-stu-id="45626-151">See [Thales Hardware Security Module](https://www.thales-esecurity.com/msrms/buy) for the list of compatible models, or to purchase an HSM if you do not have one.</span></span> |
| <span data-ttu-id="45626-152">Följande maskinvara och programvara:</span><span class="sxs-lookup"><span data-stu-id="45626-152">The following hardware and software:</span></span><ol><li><span data-ttu-id="45626-153">Ett offline x64 arbetsstation med minst Windows-operativsystemet Windows 7 och Thales nShield-programvara som är minst version 11.50.</span><span class="sxs-lookup"><span data-stu-id="45626-153">An offline x64 workstation with a minimum Windows operation system of Windows 7 and Thales nShield software that is at least version 11.50.</span></span><br/><br/><span data-ttu-id="45626-154">Om den här arbetsstationen kör Windows 7, måste du [installera Microsoft .NET Framework 4.5](http://download.microsoft.com/download/b/a/4/ba4a7e71-2906-4b2d-a0e1-80cf16844f5f/dotnetfx45_full_x86_x64.exe).</span><span class="sxs-lookup"><span data-stu-id="45626-154">If this workstation runs Windows 7, you must [install Microsoft .NET Framework 4.5](http://download.microsoft.com/download/b/a/4/ba4a7e71-2906-4b2d-a0e1-80cf16844f5f/dotnetfx45_full_x86_x64.exe).</span></span></li><li><span data-ttu-id="45626-155">En arbetsstation som är ansluten till Internet och har en minsta Windows-operativsystemet Windows 7 och [Azure PowerShell](/powershell/azure/overview) **minimiversionen 1.1.0** installerad.</span><span class="sxs-lookup"><span data-stu-id="45626-155">A workstation that is connected to the Internet and has a minimum Windows operating system of Windows 7 and [Azure PowerShell](/powershell/azure/overview) **minimum version 1.1.0** installed.</span></span></li><li><span data-ttu-id="45626-156">En USB-enhet eller annan bärbar lagringsenhet som har minst 16 MB ledigt utrymme.</span><span class="sxs-lookup"><span data-stu-id="45626-156">A USB drive or other portable storage device that has at least 16 MB free space.</span></span></li></ol> |<span data-ttu-id="45626-157">Av säkerhetsskäl rekommenderar vi att den första arbetsstationen inte är ansluten till ett nätverk.</span><span class="sxs-lookup"><span data-stu-id="45626-157">For security reasons, we recommend that the first workstation is not connected to a network.</span></span> <span data-ttu-id="45626-158">Men är den här rekommendationen inte programmässigt tvingande.</span><span class="sxs-lookup"><span data-stu-id="45626-158">However, this recommendation is not programmatically enforced.</span></span><br/><br/><span data-ttu-id="45626-159">Observera att i instruktionerna som följer den här arbetsstationen kallas den frånkopplade arbetsstationen.</span><span class="sxs-lookup"><span data-stu-id="45626-159">Note that in the instructions that follow, this workstation is referred to as the disconnected workstation.</span></span></p></blockquote><br/><span data-ttu-id="45626-160">Om din klientnyckel är avsedd för ett produktionsnätverk, rekommenderar vi dessutom att du använder andra, separat arbetsstation för att hämta verktygen och överföra klientnyckeln.</span><span class="sxs-lookup"><span data-stu-id="45626-160">In addition, if your tenant key is for a production network, we recommend that you use a second, separate workstation to download the toolset and upload the tenant key.</span></span> <span data-ttu-id="45626-161">Men i testsyfte kan du använda samma arbetsstation som den första.</span><span class="sxs-lookup"><span data-stu-id="45626-161">But for testing purposes, you can use the same workstation as the first one.</span></span><br/><br/><span data-ttu-id="45626-162">Observera att i instruktionerna som följer den här andra arbetsstationen kallas den Internetanslutna arbetsstationen.</span><span class="sxs-lookup"><span data-stu-id="45626-162">Note that in the instructions that follow, this second workstation is referred to as the Internet-connected workstation.</span></span></p></blockquote><br/> |

## <a name="generate-and-transfer-your-key-to-azure-key-vault-hsm"></a><span data-ttu-id="45626-163">Generera och överför din nyckel till Azure Key Vault HSM</span><span class="sxs-lookup"><span data-stu-id="45626-163">Generate and transfer your key to Azure Key Vault HSM</span></span>
<span data-ttu-id="45626-164">Du använder följande fem steg för att generera och överför din nyckel till en Azure Key Vault HSM:</span><span class="sxs-lookup"><span data-stu-id="45626-164">You will use the following five steps to generate and transfer your key to an Azure Key Vault HSM:</span></span>

* [<span data-ttu-id="45626-165">Steg 1: Förbered din Internetanslutna arbetsstation</span><span class="sxs-lookup"><span data-stu-id="45626-165">Step 1: Prepare your Internet-connected workstation</span></span>](#step-1-prepare-your-internet-connected-workstation)
* [<span data-ttu-id="45626-166">Steg 2: Förbered din frånkopplade arbetsstation</span><span class="sxs-lookup"><span data-stu-id="45626-166">Step 2: Prepare your disconnected workstation</span></span>](#step-2-prepare-your-disconnected-workstation)
* [<span data-ttu-id="45626-167">Steg 3: Skapa din nyckel</span><span class="sxs-lookup"><span data-stu-id="45626-167">Step 3: Generate your key</span></span>](#step-3-generate-your-key)
* [<span data-ttu-id="45626-168">Steg 4: Förbereda din nyckel för överföring</span><span class="sxs-lookup"><span data-stu-id="45626-168">Step 4: Prepare your key for transfer</span></span>](#step-4-prepare-your-key-for-transfer)
* [<span data-ttu-id="45626-169">Steg 5: Överför din nyckel till Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="45626-169">Step 5: Transfer your key to Azure Key Vault</span></span>](#step-5-transfer-your-key-to-azure-key-vault)

## <a name="step-1-prepare-your-internet-connected-workstation"></a><span data-ttu-id="45626-170">Steg 1: Förbered din Internetanslutna arbetsstation</span><span class="sxs-lookup"><span data-stu-id="45626-170">Step 1: Prepare your Internet-connected workstation</span></span>
<span data-ttu-id="45626-171">Gör följande på en arbetsstation som är ansluten till Internet för det här första steget.</span><span class="sxs-lookup"><span data-stu-id="45626-171">For this first step, do the following procedures on your workstation that is connected to the Internet.</span></span>

### <a name="step-11-install-azure-powershell"></a><span data-ttu-id="45626-172">Steg 1.1: Installera Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="45626-172">Step 1.1: Install Azure PowerShell</span></span>
<span data-ttu-id="45626-173">Hämta och installera Azure PowerShell-modulen som innehåller cmdlets för att hantera Azure Key Vault från den Internetanslutna arbetsstationen.</span><span class="sxs-lookup"><span data-stu-id="45626-173">From the Internet-connected workstation, download and install the Azure PowerShell module that includes the cmdlets to manage Azure Key Vault.</span></span> <span data-ttu-id="45626-174">Detta kräver en lägsta version av 0.8.13.</span><span class="sxs-lookup"><span data-stu-id="45626-174">This requires a minimum version of 0.8.13.</span></span>

<span data-ttu-id="45626-175">Installationsanvisningar finns i [hur du installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="45626-175">For installation instructions, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

### <a name="step-12-get-your-azure-subscription-id"></a><span data-ttu-id="45626-176">Steg 1.2: Hämta ditt Azure prenumerations-ID</span><span class="sxs-lookup"><span data-stu-id="45626-176">Step 1.2: Get your Azure subscription ID</span></span>
<span data-ttu-id="45626-177">Starta en Azure PowerShell-session och logga in på ditt Azure-konto med hjälp av följande kommando:</span><span class="sxs-lookup"><span data-stu-id="45626-177">Start an Azure PowerShell session and sign in to your Azure account by using the following command:</span></span>

        Add-AzureAccount
<span data-ttu-id="45626-178">Ange användarnamnet och lösenordet för ditt Azure-konto i popup-fönstret i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="45626-178">In the pop-up browser window, enter your Azure account user name and password.</span></span> <span data-ttu-id="45626-179">Använd sedan den [Get-AzureSubscription](/powershell/module/azure/get-azuresubscription?view=azuresmps-3.7.0) kommando:</span><span class="sxs-lookup"><span data-stu-id="45626-179">Then, use the [Get-AzureSubscription](/powershell/module/azure/get-azuresubscription?view=azuresmps-3.7.0) command:</span></span>

        Get-AzureSubscription
<span data-ttu-id="45626-180">Leta upp ID för den prenumeration som du ska använda för Azure Key Vault utdata.</span><span class="sxs-lookup"><span data-stu-id="45626-180">From the output, locate the ID for the subscription you will use for Azure Key Vault.</span></span> <span data-ttu-id="45626-181">Du behöver den här prenumerations-ID senare.</span><span class="sxs-lookup"><span data-stu-id="45626-181">You will need this subscription ID later.</span></span>

<span data-ttu-id="45626-182">Stäng inte Azure PowerShell-fönster.</span><span class="sxs-lookup"><span data-stu-id="45626-182">Do not close the Azure PowerShell window.</span></span>

### <a name="step-13-download-the-byok-toolset-for-azure-key-vault"></a><span data-ttu-id="45626-183">Steg 1.3: Hämta BYOK-verktygen för Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="45626-183">Step 1.3: Download the BYOK toolset for Azure Key Vault</span></span>
<span data-ttu-id="45626-184">Gå till Microsoft Download Center och [hämta Azure Key Vault BYOK-verktygen](http://www.microsoft.com/download/details.aspx?id=45345) för ett geografiskt område eller instans av Azure.</span><span class="sxs-lookup"><span data-stu-id="45626-184">Go to the Microsoft Download Center and [download the Azure Key Vault BYOK toolset](http://www.microsoft.com/download/details.aspx?id=45345) for your geographic region or instance of Azure.</span></span> <span data-ttu-id="45626-185">Använd följande information för att identifiera paketnamnet att ladda ned och dess motsvarande hash för SHA-256:</span><span class="sxs-lookup"><span data-stu-id="45626-185">Use the following information to identify the package name to download and its corresponding SHA-256 package hash:</span></span>

- - -
<span data-ttu-id="45626-186">**USA:**</span><span class="sxs-lookup"><span data-stu-id="45626-186">**United States:**</span></span>

<span data-ttu-id="45626-187">KeyVault-BYOK-verktyg-Förenade States.zip</span><span class="sxs-lookup"><span data-stu-id="45626-187">KeyVault-BYOK-Tools-UnitedStates.zip</span></span>

<span data-ttu-id="45626-188">760EE9BD6445C87CFF0E8B032577118704B3BEAA045AA55977C10EF68BC67E2B</span><span class="sxs-lookup"><span data-stu-id="45626-188">760EE9BD6445C87CFF0E8B032577118704B3BEAA045AA55977C10EF68BC67E2B</span></span>

- - -
<span data-ttu-id="45626-189">**Europa:**</span><span class="sxs-lookup"><span data-stu-id="45626-189">**Europe:**</span></span>

<span data-ttu-id="45626-190">KeyVault-BYOK-verktyg-Europe.zip</span><span class="sxs-lookup"><span data-stu-id="45626-190">KeyVault-BYOK-Tools-Europe.zip</span></span>

<span data-ttu-id="45626-191">7A64B94225F59B847C5C27C2200BAD7D16C901E1687767EDBBB8B09BB285011D</span><span class="sxs-lookup"><span data-stu-id="45626-191">7A64B94225F59B847C5C27C2200BAD7D16C901E1687767EDBBB8B09BB285011D</span></span>

- - -
<span data-ttu-id="45626-192">**Asien:**</span><span class="sxs-lookup"><span data-stu-id="45626-192">**Asia:**</span></span>

<span data-ttu-id="45626-193">KeyVault-BYOK-verktyg-AsiaPacific.zip</span><span class="sxs-lookup"><span data-stu-id="45626-193">KeyVault-BYOK-Tools-AsiaPacific.zip</span></span>

<span data-ttu-id="45626-194">813DC94B23079CF7A5CEA71D5B444E86B292F463C53EE47AED25D4F7CD58E7D8</span><span class="sxs-lookup"><span data-stu-id="45626-194">813DC94B23079CF7A5CEA71D5B444E86B292F463C53EE47AED25D4F7CD58E7D8</span></span>

- - -
<span data-ttu-id="45626-195">**Latinamerika:**</span><span class="sxs-lookup"><span data-stu-id="45626-195">**Latin America:**</span></span>

<span data-ttu-id="45626-196">KeyVault-BYOK-verktyg-LatinAmerica.zip</span><span class="sxs-lookup"><span data-stu-id="45626-196">KeyVault-BYOK-Tools-LatinAmerica.zip</span></span>

<span data-ttu-id="45626-197">3F29069E3500F95C0E156F4B8914E1DC60C20FB64B464306A299EA5145D755C0</span><span class="sxs-lookup"><span data-stu-id="45626-197">3F29069E3500F95C0E156F4B8914E1DC60C20FB64B464306A299EA5145D755C0</span></span>

- - -
<span data-ttu-id="45626-198">**Japan:**</span><span class="sxs-lookup"><span data-stu-id="45626-198">**Japan:**</span></span>

<span data-ttu-id="45626-199">KeyVault-BYOK-verktyg-Japan.zip</span><span class="sxs-lookup"><span data-stu-id="45626-199">KeyVault-BYOK-Tools-Japan.zip</span></span>

<span data-ttu-id="45626-200">453FFEA2F8F410720B68B8BAC4CF79135A7F37F4E491FF840BE9E69E88A98C90</span><span class="sxs-lookup"><span data-stu-id="45626-200">453FFEA2F8F410720B68B8BAC4CF79135A7F37F4E491FF840BE9E69E88A98C90</span></span>

- - -
<span data-ttu-id="45626-201">**Korea:**</span><span class="sxs-lookup"><span data-stu-id="45626-201">**Korea:**</span></span>

<span data-ttu-id="45626-202">KeyVault-BYOK-verktyg-Korea.zip</span><span class="sxs-lookup"><span data-stu-id="45626-202">KeyVault-BYOK-Tools-Korea.zip</span></span>

<span data-ttu-id="45626-203">C17B7E93224DA80F5668E09CF7DAE2F92527E8226179995BBE2E43DA4323595A</span><span class="sxs-lookup"><span data-stu-id="45626-203">C17B7E93224DA80F5668E09CF7DAE2F92527E8226179995BBE2E43DA4323595A</span></span>

- - -
<span data-ttu-id="45626-204">**Australien:**</span><span class="sxs-lookup"><span data-stu-id="45626-204">**Australia:**</span></span>

<span data-ttu-id="45626-205">KeyVault-BYOK-verktyg-Australia.zip</span><span class="sxs-lookup"><span data-stu-id="45626-205">KeyVault-BYOK-Tools-Australia.zip</span></span>

<span data-ttu-id="45626-206">4AD893396E86F2D2A71682876A6A8EA59E3C7895BEAD2F7E7C8516682582C34B</span><span class="sxs-lookup"><span data-stu-id="45626-206">4AD893396E86F2D2A71682876A6A8EA59E3C7895BEAD2F7E7C8516682582C34B</span></span>

- - -
[<span data-ttu-id="45626-207">**Azure Government:**</span><span class="sxs-lookup"><span data-stu-id="45626-207">**Azure Government:**</span></span>](https://azure.microsoft.com/features/gov/)

<span data-ttu-id="45626-208">KeyVault-BYOK-verktyg-USGovCloud.zip</span><span class="sxs-lookup"><span data-stu-id="45626-208">KeyVault-BYOK-Tools-USGovCloud.zip</span></span>

<span data-ttu-id="45626-209">3AAE1A96B9D15B899B8126CFC0380719EB54FDF2EA94489B43FAD21ECC745F64</span><span class="sxs-lookup"><span data-stu-id="45626-209">3AAE1A96B9D15B899B8126CFC0380719EB54FDF2EA94489B43FAD21ECC745F64</span></span>

- - -
<span data-ttu-id="45626-210">**USA: S regering DOD:**</span><span class="sxs-lookup"><span data-stu-id="45626-210">**US Government DOD:**</span></span>

<span data-ttu-id="45626-211">KeyVault-BYOK-verktyg-USGovernmentDoD.zip</span><span class="sxs-lookup"><span data-stu-id="45626-211">KeyVault-BYOK-Tools-USGovernmentDoD.zip</span></span>

<span data-ttu-id="45626-212">A61E78297B0732DF2682FDE63D7B572CE4D23B0BC27CC48AFF620BD060BB9E9D</span><span class="sxs-lookup"><span data-stu-id="45626-212">A61E78297B0732DF2682FDE63D7B572CE4D23B0BC27CC48AFF620BD060BB9E9D</span></span>

- - -
<span data-ttu-id="45626-213">**Kanada:**</span><span class="sxs-lookup"><span data-stu-id="45626-213">**Canada:**</span></span>

<span data-ttu-id="45626-214">KeyVault-BYOK-verktyg-Canada.zip</span><span class="sxs-lookup"><span data-stu-id="45626-214">KeyVault-BYOK-Tools-Canada.zip</span></span>

<span data-ttu-id="45626-215">30B87A0BA8208F6B7241C30C794FED1C370D7445ACA179685816E4E156CD2AF7</span><span class="sxs-lookup"><span data-stu-id="45626-215">30B87A0BA8208F6B7241C30C794FED1C370D7445ACA179685816E4E156CD2AF7</span></span>

- - -
<span data-ttu-id="45626-216">**Tyskland:**</span><span class="sxs-lookup"><span data-stu-id="45626-216">**Germany:**</span></span>

<span data-ttu-id="45626-217">KeyVault-BYOK-verktyg-Germany.zip</span><span class="sxs-lookup"><span data-stu-id="45626-217">KeyVault-BYOK-Tools-Germany.zip</span></span>

<span data-ttu-id="45626-218">5E3E4AA54715E4F93C3C145035B18275B7C6815A06D7ABB212E7FADBF2929261</span><span class="sxs-lookup"><span data-stu-id="45626-218">5E3E4AA54715E4F93C3C145035B18275B7C6815A06D7ABB212E7FADBF2929261</span></span>

- - -
<span data-ttu-id="45626-219">**Indien:**</span><span class="sxs-lookup"><span data-stu-id="45626-219">**India:**</span></span>

<span data-ttu-id="45626-220">KeyVault-BYOK-verktyg-India.zip</span><span class="sxs-lookup"><span data-stu-id="45626-220">KeyVault-BYOK-Tools-India.zip</span></span>

<span data-ttu-id="45626-221">136733A6C6A71D75571BB80819B3D55A9B83CCAD5C996C686BC5682A3F369BF7</span><span class="sxs-lookup"><span data-stu-id="45626-221">136733A6C6A71D75571BB80819B3D55A9B83CCAD5C996C686BC5682A3F369BF7</span></span>

- - -
<span data-ttu-id="45626-222">**Storbritannien:**</span><span class="sxs-lookup"><span data-stu-id="45626-222">**United Kingdom:**</span></span>

<span data-ttu-id="45626-223">KeyVault-BYOK-verktyg-UnitedKingdom.zip</span><span class="sxs-lookup"><span data-stu-id="45626-223">KeyVault-BYOK-Tools-UnitedKingdom.zip</span></span>

<span data-ttu-id="45626-224">ED331A6F1D34A402317D3F27D5396046AF0E5C2D44B5D10CCCE293472942D268</span><span class="sxs-lookup"><span data-stu-id="45626-224">ED331A6F1D34A402317D3F27D5396046AF0E5C2D44B5D10CCCE293472942D268</span></span>

- - -

<span data-ttu-id="45626-225">Om du vill validera integriteten för din hämtade BYOK-verktyg från Azure PowerShell-sessionen använder den [Get-FileHash](https://technet.microsoft.com/library/dn520872.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="45626-225">To validate the integrity of your downloaded BYOK toolset, from your Azure PowerShell session, use the [Get-FileHash](https://technet.microsoft.com/library/dn520872.aspx) cmdlet.</span></span>

    Get-FileHash KeyVault-BYOK-Tools-*.zip

<span data-ttu-id="45626-226">Denna verktygsuppsättning omfattar följande:</span><span class="sxs-lookup"><span data-stu-id="45626-226">The toolset includes the following:</span></span>

* <span data-ttu-id="45626-227">Ett paket för KeyExchange-nyckel (KEK) som har ett namn som börjar med **BYOK-KEK - pkg-.**</span><span class="sxs-lookup"><span data-stu-id="45626-227">A Key Exchange Key (KEK) package that has a name beginning with **BYOK-KEK-pkg-.**</span></span>
* <span data-ttu-id="45626-228">Ett säkerhetsvärldspaket som har ett namn som börjar med **BYOK-SecurityWorld - pkg-.**</span><span class="sxs-lookup"><span data-stu-id="45626-228">A Security World package that has a name beginning with **BYOK-SecurityWorld-pkg-.**</span></span>
* <span data-ttu-id="45626-229">Python-skriptet **verifykeypackage.py.**</span><span class="sxs-lookup"><span data-stu-id="45626-229">A python script named **verifykeypackage.py.**</span></span>
* <span data-ttu-id="45626-230">En kommandorad körbara filen **KeyTransferRemote.exe** och tillhörande DLL-filer.</span><span class="sxs-lookup"><span data-stu-id="45626-230">A command-line executable file named **KeyTransferRemote.exe** and associated DLLs.</span></span>
* <span data-ttu-id="45626-231">Ett Visual C++ Redistributable Package med namnet **vcredist_x64.exe.**</span><span class="sxs-lookup"><span data-stu-id="45626-231">A Visual C++ Redistributable Package, named **vcredist_x64.exe.**</span></span>

<span data-ttu-id="45626-232">Kopiera paketet till en USB-enhet eller annan bärbar lagringsenhet.</span><span class="sxs-lookup"><span data-stu-id="45626-232">Copy the package to a USB drive or other portable storage.</span></span>

## <a name="step-2-prepare-your-disconnected-workstation"></a><span data-ttu-id="45626-233">Steg 2: Förbered din frånkopplade arbetsstation</span><span class="sxs-lookup"><span data-stu-id="45626-233">Step 2: Prepare your disconnected workstation</span></span>
<span data-ttu-id="45626-234">Gör följande på den arbetsstation som inte är ansluten till ett nätverk (Internet eller ditt interna nätverk) för den här andra steget.</span><span class="sxs-lookup"><span data-stu-id="45626-234">For this second step, do the following procedures on the workstation that is not connected to a network (either the Internet or your internal network).</span></span>

### <a name="step-21-prepare-the-disconnected-workstation-with-thales-hsm"></a><span data-ttu-id="45626-235">Steg 2.1: Förbered den frånkopplade arbetsstationen med Thales HSM</span><span class="sxs-lookup"><span data-stu-id="45626-235">Step 2.1: Prepare the disconnected workstation with Thales HSM</span></span>
<span data-ttu-id="45626-236">Installera hjälpprogrammet nCipher (Thales) på en Windows-dator och sedan kopplar en Thales HSM till datorn.</span><span class="sxs-lookup"><span data-stu-id="45626-236">Install the nCipher (Thales) support software on a Windows computer, and then attach a Thales HSM to that computer.</span></span>

<span data-ttu-id="45626-237">Se till att Thales-verktygen finns i din sökväg (**%nfast_home%\bin**).</span><span class="sxs-lookup"><span data-stu-id="45626-237">Ensure that the Thales tools are in your path (**%nfast_home%\bin**).</span></span> <span data-ttu-id="45626-238">Skriv till exempel följande:</span><span class="sxs-lookup"><span data-stu-id="45626-238">For example, type the following:</span></span>

        set PATH=%PATH%;"%nfast_home%\bin"

<span data-ttu-id="45626-239">Mer information finns i användarhandboken som medföljer Thales HSM.</span><span class="sxs-lookup"><span data-stu-id="45626-239">For more information, see the user guide included with the Thales HSM.</span></span>

### <a name="step-22-install-the-byok-toolset-on-the-disconnected-workstation"></a><span data-ttu-id="45626-240">Steg 2.2: Installera BYOK-verktygen på den frånkopplade arbetsstationen</span><span class="sxs-lookup"><span data-stu-id="45626-240">Step 2.2: Install the BYOK toolset on the disconnected workstation</span></span>
<span data-ttu-id="45626-241">Kopiera BYOK-verktygspaketet från USB-enhet eller annan bärbar lagringsenhet och gör sedan följande:</span><span class="sxs-lookup"><span data-stu-id="45626-241">Copy the BYOK toolset package from the USB drive or other portable storage, and then do the following:</span></span>

1. <span data-ttu-id="45626-242">Extrahera filerna från det Hämta paketet till en annan mapp.</span><span class="sxs-lookup"><span data-stu-id="45626-242">Extract the files from the downloaded package into any folder.</span></span>
2. <span data-ttu-id="45626-243">Kör vcredist_x64.exe från denna mapp.</span><span class="sxs-lookup"><span data-stu-id="45626-243">From that folder, run vcredist_x64.exe.</span></span>
3. <span data-ttu-id="45626-244">Följ instruktionerna för att installera Visual C++-körtidskomponenter för Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="45626-244">Follow the instructions to the install the Visual C++ runtime components for Visual Studio 2013.</span></span>

## <a name="step-3-generate-your-key"></a><span data-ttu-id="45626-245">Steg 3: Skapa din nyckel</span><span class="sxs-lookup"><span data-stu-id="45626-245">Step 3: Generate your key</span></span>
<span data-ttu-id="45626-246">Gör följande på den frånkopplade arbetsstationen för den här tredje steget.</span><span class="sxs-lookup"><span data-stu-id="45626-246">For this third step, do the following procedures on the disconnected workstation.</span></span> <span data-ttu-id="45626-247">För att slutföra det här steget måste din HSM vara i läget för initiering.</span><span class="sxs-lookup"><span data-stu-id="45626-247">To complete this step your HSM must be in initialization mode.</span></span> 


### <a name="step-31-change-the-hsm-mode-to-i"></a><span data-ttu-id="45626-248">Steg 3.1: Ändra HSM-läge till 'I'</span><span class="sxs-lookup"><span data-stu-id="45626-248">Step 3.1: Change the HSM mode to 'I'</span></span>
<span data-ttu-id="45626-249">Om du använder Thales nShield gränsen, för att ändra läget: 1.</span><span class="sxs-lookup"><span data-stu-id="45626-249">If you are using Thales nShield Edge, to change the mode: 1.</span></span> <span data-ttu-id="45626-250">Använd knappen läge för att markera det nödvändiga läget.</span><span class="sxs-lookup"><span data-stu-id="45626-250">Use the Mode button to highlight the required mode.</span></span> <span data-ttu-id="45626-251">2.</span><span class="sxs-lookup"><span data-stu-id="45626-251">2.</span></span> <span data-ttu-id="45626-252">Tryck och håll Rensa-knapp för några sekunder inom några sekunder.</span><span class="sxs-lookup"><span data-stu-id="45626-252">Within a few seconds, press and hold the Clear button for a couple of seconds.</span></span> <span data-ttu-id="45626-253">Om läget ändras, slutar blinka Indikator för det nya läget och fortsätter att lysa.</span><span class="sxs-lookup"><span data-stu-id="45626-253">If the mode changes, the new mode’s LED stops flashing and remains lit.</span></span> <span data-ttu-id="45626-254">Indikator för Status kan flash oregelbundet under några sekunder och sedan blinkar regelbundet när enheten är klar.</span><span class="sxs-lookup"><span data-stu-id="45626-254">The Status LED might flash irregularly for a few seconds and then flashes regularly when the device is ready.</span></span> <span data-ttu-id="45626-255">Annars kan enheten fortfarande i det aktuella läget med det aktuella läget Indikator upplysta.</span><span class="sxs-lookup"><span data-stu-id="45626-255">Otherwise, the device remains in the current mode, with the appropriate mode LED lit.</span></span>

### <a name="step-32-create-a-security-world"></a><span data-ttu-id="45626-256">Steg 3.2: Skapa en säkerhetsvärld</span><span class="sxs-lookup"><span data-stu-id="45626-256">Step 3.2: Create a security world</span></span>
<span data-ttu-id="45626-257">Starta en kommandotolk och kör Thales nya världen programmet.</span><span class="sxs-lookup"><span data-stu-id="45626-257">Start a command prompt and run the Thales new-world program.</span></span>

    new-world.exe --initialize --cipher-suite=DLf1024s160mRijndael --module=1 --acs-quorum=2/3

<span data-ttu-id="45626-258">Det här programmet skapar en **Säkerhetsvärld** filen i % NFAST_KMDATA%\local\world, vilket motsvarar mappen C:\ProgramData\nCipher\Key Management Data\local.</span><span class="sxs-lookup"><span data-stu-id="45626-258">This program creates a **Security World** file at %NFAST_KMDATA%\local\world, which corresponds to the C:\ProgramData\nCipher\Key Management Data\local folder.</span></span> <span data-ttu-id="45626-259">Du kan använda olika värden för kvorumet men i vårt exempel uppmanas du att ange tre tomma kort och PIN-koder för vart och ett.</span><span class="sxs-lookup"><span data-stu-id="45626-259">You can use different values for the quorum but in our example, you’re prompted to enter three blank cards and pins for each one.</span></span> <span data-ttu-id="45626-260">Därefter ger två valfria kort fullständig åtkomst till säkerhetsvärlden.</span><span class="sxs-lookup"><span data-stu-id="45626-260">Then, any two cards give full access to the security world.</span></span> <span data-ttu-id="45626-261">Dessa kort blir den **Administratörskortsuppsättningen** för den nya säkerhetsvärlden.</span><span class="sxs-lookup"><span data-stu-id="45626-261">These cards become the **Administrator Card Set** for the new security world.</span></span>

<span data-ttu-id="45626-262">Gör något av följande:</span><span class="sxs-lookup"><span data-stu-id="45626-262">Then do the following:</span></span>

* <span data-ttu-id="45626-263">Säkerhetskopiera världfilen.</span><span class="sxs-lookup"><span data-stu-id="45626-263">Back up the world file.</span></span> <span data-ttu-id="45626-264">Säkra och skydda världfilen, Administratörskorten och sina PIN-koder och se till att ingen enskild person har tillgång till mer än ett kort.</span><span class="sxs-lookup"><span data-stu-id="45626-264">Secure and protect the world file, the Administrator Cards, and their pins, and make sure that no single person has access to more than one card.</span></span>

### <a name="step-33-change-the-hsm-mode-to-o"></a><span data-ttu-id="45626-265">Steg 3.3: Ändra HSM läget ' o-</span><span class="sxs-lookup"><span data-stu-id="45626-265">Step 3.3: Change the HSM mode to 'O'</span></span>
<span data-ttu-id="45626-266">Om du använder Thales nShield gränsen, för att ändra läget: 1.</span><span class="sxs-lookup"><span data-stu-id="45626-266">If you are using Thales nShield Edge, to change the mode: 1.</span></span> <span data-ttu-id="45626-267">Använd knappen läge för att markera det nödvändiga läget.</span><span class="sxs-lookup"><span data-stu-id="45626-267">Use the Mode button to highlight the required mode.</span></span> <span data-ttu-id="45626-268">2.</span><span class="sxs-lookup"><span data-stu-id="45626-268">2.</span></span> <span data-ttu-id="45626-269">Tryck och håll Rensa-knapp för några sekunder inom några sekunder.</span><span class="sxs-lookup"><span data-stu-id="45626-269">Within a few seconds, press and hold the Clear button for a couple of seconds.</span></span> <span data-ttu-id="45626-270">Om läget ändras, slutar blinka Indikator för det nya läget och fortsätter att lysa.</span><span class="sxs-lookup"><span data-stu-id="45626-270">If the mode changes, the new mode’s LED stops flashing and remains lit.</span></span> <span data-ttu-id="45626-271">Indikator för Status kan flash oregelbundet under några sekunder och sedan blinkar regelbundet när enheten är klar.</span><span class="sxs-lookup"><span data-stu-id="45626-271">The Status LED might flash irregularly for a few seconds and then flashes regularly when the device is ready.</span></span> <span data-ttu-id="45626-272">Annars kan enheten fortfarande i det aktuella läget med det aktuella läget Indikator upplysta.</span><span class="sxs-lookup"><span data-stu-id="45626-272">Otherwise, the device remains in the current mode, with the appropriate mode LED lit.</span></span>


### <a name="step-34-validate-the-downloaded-package"></a><span data-ttu-id="45626-273">Steg 3.4: Validera det Hämta paketet</span><span class="sxs-lookup"><span data-stu-id="45626-273">Step 3.4: Validate the downloaded package</span></span>
<span data-ttu-id="45626-274">Det här steget är valfritt men rekommenderas så att du kan validera följande:</span><span class="sxs-lookup"><span data-stu-id="45626-274">This step is optional but recommended so that you can validate the following:</span></span>

* <span data-ttu-id="45626-275">KeyExchange-nyckeln som ingår i verktygsuppsättningen har genererats från en äkta Thales HSM.</span><span class="sxs-lookup"><span data-stu-id="45626-275">The Key Exchange Key that is included in the toolset has been generated from a genuine Thales HSM.</span></span>
* <span data-ttu-id="45626-276">Hash för Säkerhetsvärld som ingår i verktygsuppsättningen har genererats från en äkta Thales HSM.</span><span class="sxs-lookup"><span data-stu-id="45626-276">The hash of the Security World that is included in the toolset has been generated in a genuine Thales HSM.</span></span>
* <span data-ttu-id="45626-277">KeyExchange-nyckeln kan inte exporteras.</span><span class="sxs-lookup"><span data-stu-id="45626-277">The Key Exchange Key is non-exportable.</span></span>

> [!NOTE]
> <span data-ttu-id="45626-278">Om du vill validera det Hämta paketet HSM måste vara ansluten, påslagen, och måste ha en säkerhetsvärld (som den du nyss skapat).</span><span class="sxs-lookup"><span data-stu-id="45626-278">To validate the downloaded package, the HSM must be connected, powered on, and must have a security world on it (such as the one you’ve just created).</span></span>
>
>

<span data-ttu-id="45626-279">Att validera det Hämta paketet:</span><span class="sxs-lookup"><span data-stu-id="45626-279">To validate the downloaded package:</span></span>

1. <span data-ttu-id="45626-280">Kör skriptet verifykeypackage.PY genom att skriva ett av följande, beroende på din geografiskt område eller en instans av Azure:</span><span class="sxs-lookup"><span data-stu-id="45626-280">Run the verifykeypackage.py script by typing one of the following, depending on your geographic region or instance of Azure:</span></span>

   * <span data-ttu-id="45626-281">För Nordamerika:</span><span class="sxs-lookup"><span data-stu-id="45626-281">For North America:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-NA-1 -w BYOK-SecurityWorld-pkg-NA-1
   * <span data-ttu-id="45626-282">För Europa:</span><span class="sxs-lookup"><span data-stu-id="45626-282">For Europe:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-EU-1 -w BYOK-SecurityWorld-pkg-EU-1
   * <span data-ttu-id="45626-283">För Asien:</span><span class="sxs-lookup"><span data-stu-id="45626-283">For Asia:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-AP-1 -w BYOK-SecurityWorld-pkg-AP-1
   * <span data-ttu-id="45626-284">För Latinamerika:</span><span class="sxs-lookup"><span data-stu-id="45626-284">For Latin America:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-LATAM-1 -w BYOK-SecurityWorld-pkg-LATAM-1
   * <span data-ttu-id="45626-285">För Japan:</span><span class="sxs-lookup"><span data-stu-id="45626-285">For Japan:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-JPN-1 -w BYOK-SecurityWorld-pkg-JPN-1
   * <span data-ttu-id="45626-286">För Korea:</span><span class="sxs-lookup"><span data-stu-id="45626-286">For Korea:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-KOREA-1 -w BYOK-SecurityWorld-pkg-KOREA-1
   * <span data-ttu-id="45626-287">För Australien:</span><span class="sxs-lookup"><span data-stu-id="45626-287">For Australia:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-AUS-1 -w BYOK-SecurityWorld-pkg-AUS-1
   * <span data-ttu-id="45626-288">För [Azure Government](https://azure.microsoft.com/features/gov/), som använder US government instans av Azure:</span><span class="sxs-lookup"><span data-stu-id="45626-288">For [Azure Government](https://azure.microsoft.com/features/gov/), which uses the US government instance of Azure:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-USGOV-1 -w BYOK-SecurityWorld-pkg-USGOV-1
   * <span data-ttu-id="45626-289">För USA: S regering DOD:</span><span class="sxs-lookup"><span data-stu-id="45626-289">For US Government DOD:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-USDOD-1 -w BYOK-SecurityWorld-pkg-USDOD-1
   * <span data-ttu-id="45626-290">För Kanada:</span><span class="sxs-lookup"><span data-stu-id="45626-290">For Canada:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-CANADA-1 -w BYOK-SecurityWorld-pkg-CANADA-1
   * <span data-ttu-id="45626-291">Tyskland:</span><span class="sxs-lookup"><span data-stu-id="45626-291">For Germany:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-GERMANY-1 -w BYOK-SecurityWorld-pkg-GERMANY-1
   * <span data-ttu-id="45626-292">För Indien:</span><span class="sxs-lookup"><span data-stu-id="45626-292">For India:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-INDIA-1 -w BYOK-SecurityWorld-pkg-INDIA-1
     > [!TIP]
     > <span data-ttu-id="45626-293">Thales-programvaran innehåller python på %NFAST_HOME%\python\bin</span><span class="sxs-lookup"><span data-stu-id="45626-293">The Thales software includes python at %NFAST_HOME%\python\bin</span></span>
     >
     >
2. <span data-ttu-id="45626-294">Bekräfta att du ser följande, vilket betyder att valideringen har lyckats: **resultat: lyckades**</span><span class="sxs-lookup"><span data-stu-id="45626-294">Confirm that you see the following, which indicates successful validation: **Result: SUCCESS**</span></span>

<span data-ttu-id="45626-295">Det här skriptet validerar undertecknarkedjan upp till Thales rotnyckel.</span><span class="sxs-lookup"><span data-stu-id="45626-295">This script validates the signer chain up to the Thales root key.</span></span> <span data-ttu-id="45626-296">Hashen för den här rotnyckeln är inbäddad i skriptet och dess värde bör vara **59178a47 de508c3f 291277ee 184f46c4 f1d9c639**.</span><span class="sxs-lookup"><span data-stu-id="45626-296">The hash of this root key is embedded in the script and its value should be **59178a47 de508c3f 291277ee 184f46c4 f1d9c639**.</span></span> <span data-ttu-id="45626-297">Du kan också bekräfta det här värdet separat genom att besöka den [Thales webbplats](http://www.thalesesec.com/).</span><span class="sxs-lookup"><span data-stu-id="45626-297">You can also confirm this value separately by visiting the [Thales website](http://www.thalesesec.com/).</span></span>

<span data-ttu-id="45626-298">Nu är du redo att skapa en ny nyckel.</span><span class="sxs-lookup"><span data-stu-id="45626-298">You’re now ready to create a new key.</span></span>

### <a name="step-35-create-a-new-key"></a><span data-ttu-id="45626-299">Steg 3.5: Skapa en ny nyckel</span><span class="sxs-lookup"><span data-stu-id="45626-299">Step 3.5: Create a new key</span></span>
<span data-ttu-id="45626-300">Generera en nyckel med hjälp av Thales **generatekey** program.</span><span class="sxs-lookup"><span data-stu-id="45626-300">Generate a key by using the Thales **generatekey** program.</span></span>

<span data-ttu-id="45626-301">Kör följande kommando för att generera nyckeln:</span><span class="sxs-lookup"><span data-stu-id="45626-301">Run the following command to generate the key:</span></span>

    generatekey --generate simple type=RSA size=2048 protect=module ident=contosokey plainname=contosokey nvram=no pubexp=

<span data-ttu-id="45626-302">Följ dessa instruktioner när du kör det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="45626-302">When you run this command, use these instructions:</span></span>

* <span data-ttu-id="45626-303">Parametern *skydda* måste anges till värdet **modulen**, som visas.</span><span class="sxs-lookup"><span data-stu-id="45626-303">The parameter *protect* must be set to the value **module**, as shown.</span></span> <span data-ttu-id="45626-304">Detta skapar en modulskyddad nyckel.</span><span class="sxs-lookup"><span data-stu-id="45626-304">This creates a module-protected key.</span></span> <span data-ttu-id="45626-305">BYOK-verktygen har inte stöd för OCS-skyddade nycklar.</span><span class="sxs-lookup"><span data-stu-id="45626-305">The BYOK toolset does not support OCS-protected keys.</span></span>
* <span data-ttu-id="45626-306">Ersätt värdet för *contosokey* för den **ident** och **plainname** med valfritt strängvärde.</span><span class="sxs-lookup"><span data-stu-id="45626-306">Replace the value of *contosokey* for the **ident** and **plainname** with any string value.</span></span> <span data-ttu-id="45626-307">Om du vill minimera de administrativa kostnaderna och minska risken för fel, rekommenderar vi att du använder samma värde för båda.</span><span class="sxs-lookup"><span data-stu-id="45626-307">To minimize administrative overheads and reduce the risk of errors, we recommend that you use the same value for both.</span></span> <span data-ttu-id="45626-308">Den **ident** värdet måste innehålla endast siffror, bindestreck och gemener.</span><span class="sxs-lookup"><span data-stu-id="45626-308">The **ident** value must contain only numbers, dashes, and lower case letters.</span></span>
* <span data-ttu-id="45626-309">Pubexp lämnas tomt (standard) i det här exemplet, men du kan ange specifika värden.</span><span class="sxs-lookup"><span data-stu-id="45626-309">The pubexp is left blank (default) in this example, but you can specify specific values.</span></span> <span data-ttu-id="45626-310">Mer information finns i Thales-dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="45626-310">For more information, see the Thales documentation.</span></span>

<span data-ttu-id="45626-311">Det här kommandot skapar en fil för Tokeniserad nyckel i mappen %NFAST_KMDATA%\local med ett namn som börjar med **key_simple_**, följt av den **ident** som angavs i kommandot.</span><span class="sxs-lookup"><span data-stu-id="45626-311">This command creates a Tokenized Key file in your %NFAST_KMDATA%\local folder with a name starting with **key_simple_**, followed by the **ident** that was specified in the command.</span></span> <span data-ttu-id="45626-312">Till exempel: **key_simple_contosokey**.</span><span class="sxs-lookup"><span data-stu-id="45626-312">For example: **key_simple_contosokey**.</span></span> <span data-ttu-id="45626-313">Den här filen innehåller en krypterad nyckel.</span><span class="sxs-lookup"><span data-stu-id="45626-313">This file contains an encrypted key.</span></span>

<span data-ttu-id="45626-314">Säkerhetskopiera filen för Tokeniserad nyckel på en säker plats.</span><span class="sxs-lookup"><span data-stu-id="45626-314">Back up this Tokenized Key File in a safe location.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="45626-315">När du senare överför din nyckel till Azure Key Vault kan inte Microsoft exportera den här nyckeln till dig så att det är mycket viktigt att säkerhetskopiera din nyckel och säkerhetsvärld på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="45626-315">When you later transfer your key to Azure Key Vault, Microsoft cannot export this key back to you so it becomes extremely important that you back up your key and security world safely.</span></span> <span data-ttu-id="45626-316">Kontakta Thales för vägledning och bästa praxis för säkerhetskopiering av din nyckel.</span><span class="sxs-lookup"><span data-stu-id="45626-316">Contact Thales for guidance and best practices for backing up your key.</span></span>
>
>

<span data-ttu-id="45626-317">Du är nu redo att överföra din nyckel till Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="45626-317">You are now ready to transfer your key to Azure Key Vault.</span></span>

## <a name="step-4-prepare-your-key-for-transfer"></a><span data-ttu-id="45626-318">Steg 4: Förbereda din nyckel för överföring</span><span class="sxs-lookup"><span data-stu-id="45626-318">Step 4: Prepare your key for transfer</span></span>
<span data-ttu-id="45626-319">Gör följande på den frånkopplade arbetsstationen för det här fjärde steget.</span><span class="sxs-lookup"><span data-stu-id="45626-319">For this fourth step, do the following procedures on the disconnected workstation.</span></span>

### <a name="step-41-create-a-copy-of-your-key-with-reduced-permissions"></a><span data-ttu-id="45626-320">Steg 4.1: Skapa en kopia av din nyckel med minskade behörigheter</span><span class="sxs-lookup"><span data-stu-id="45626-320">Step 4.1: Create a copy of your key with reduced permissions</span></span>

<span data-ttu-id="45626-321">Öppna en ny kommandotolk och ändra katalogen till den plats där du unzipped BYOK zip-filen.</span><span class="sxs-lookup"><span data-stu-id="45626-321">Open a new command prompt and change the current directory to the location where you unzipped the BYOK zip file.</span></span> <span data-ttu-id="45626-322">Kör något av följande, beroende på din geografiskt område eller en instans av Azure för att minska behörigheterna för din nyckel från en kommandotolk:</span><span class="sxs-lookup"><span data-stu-id="45626-322">To reduce the permissions on your key, from a command prompt, run one of the following, depending on your geographic region or instance of Azure:</span></span>

* <span data-ttu-id="45626-323">För Nordamerika:</span><span class="sxs-lookup"><span data-stu-id="45626-323">For North America:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1
* <span data-ttu-id="45626-324">För Europa:</span><span class="sxs-lookup"><span data-stu-id="45626-324">For Europe:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1
* <span data-ttu-id="45626-325">För Asien:</span><span class="sxs-lookup"><span data-stu-id="45626-325">For Asia:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1
* <span data-ttu-id="45626-326">För Latinamerika:</span><span class="sxs-lookup"><span data-stu-id="45626-326">For Latin America:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1
* <span data-ttu-id="45626-327">För Japan:</span><span class="sxs-lookup"><span data-stu-id="45626-327">For Japan:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1
* <span data-ttu-id="45626-328">För Korea:</span><span class="sxs-lookup"><span data-stu-id="45626-328">For Korea:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-KOREA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-KOREA-1
* <span data-ttu-id="45626-329">För Australien:</span><span class="sxs-lookup"><span data-stu-id="45626-329">For Australia:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1
* <span data-ttu-id="45626-330">För [Azure Government](https://azure.microsoft.com/features/gov/), som använder US government instans av Azure:</span><span class="sxs-lookup"><span data-stu-id="45626-330">For [Azure Government](https://azure.microsoft.com/features/gov/), which uses the US government instance of Azure:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1
* <span data-ttu-id="45626-331">För USA: S regering DOD:</span><span class="sxs-lookup"><span data-stu-id="45626-331">For US Government DOD:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USDOD-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USDOD-1
* <span data-ttu-id="45626-332">För Kanada:</span><span class="sxs-lookup"><span data-stu-id="45626-332">For Canada:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1
* <span data-ttu-id="45626-333">Tyskland:</span><span class="sxs-lookup"><span data-stu-id="45626-333">For Germany:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1
* <span data-ttu-id="45626-334">För Indien:</span><span class="sxs-lookup"><span data-stu-id="45626-334">For India:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1

<span data-ttu-id="45626-335">När du kör det här kommandot ersätter *contosokey* med samma värde som du angav i **steg 3.5: skapa en ny nyckel** från den [generera din nyckel](#step-3-generate-your-key) steg.</span><span class="sxs-lookup"><span data-stu-id="45626-335">When you run this command, replace *contosokey* with the same value you specified in **Step 3.5: Create a new key** from the [Generate your key](#step-3-generate-your-key) step.</span></span>

<span data-ttu-id="45626-336">Du uppmanas att ansluta dina säkerhet world admin-kort.</span><span class="sxs-lookup"><span data-stu-id="45626-336">You are asked to plug in your security world admin cards.</span></span>

<span data-ttu-id="45626-337">När kommandot har slutförts visas **resultat: lyckades** och kopia av din nyckel med minskade behörigheter finns i filen key_xferacid_<contosokey>.</span><span class="sxs-lookup"><span data-stu-id="45626-337">When the command completes, you see **Result: SUCCESS** and the copy of your key with reduced permissions are in the file named key_xferacId_<contosokey>.</span></span>

<span data-ttu-id="45626-338">Du kan undersöker en ACL: er med hjälp av följande kommandon med hjälp av Thales-verktygen:</span><span class="sxs-lookup"><span data-stu-id="45626-338">You may inspects the ACLS using following commands using the Thales utilities:</span></span>

* <span data-ttu-id="45626-339">aclprint.PY:</span><span class="sxs-lookup"><span data-stu-id="45626-339">aclprint.py:</span></span>

        "%nfast_home%\bin\preload.exe" -m 1 -A xferacld -K contosokey "%nfast_home%\python\bin\python" "%nfast_home%\python\examples\aclprint.py"
* <span data-ttu-id="45626-340">kmfile-dump.exe:</span><span class="sxs-lookup"><span data-stu-id="45626-340">kmfile-dump.exe:</span></span>

        "%nfast_home%\bin\kmfile-dump.exe" "%NFAST_KMDATA%\local\key_xferacld_contosokey"
  <span data-ttu-id="45626-341">När du kör kommandona, ersätter contosokey med samma värde som du angav i **steg 3.5: skapa en ny nyckel** från den [generera din nyckel](#step-3-generate-your-key) steg.</span><span class="sxs-lookup"><span data-stu-id="45626-341">When you run these commands, replace contosokey with the same value you specified in **Step 3.5: Create a new key** from the [Generate your key](#step-3-generate-your-key) step.</span></span>

### <a name="step-42-encrypt-your-key-by-using-microsofts-key-exchange-key"></a><span data-ttu-id="45626-342">Steg 4.2: Kryptera nyckeln med hjälp av Microsofts KeyExchange-nyckel</span><span class="sxs-lookup"><span data-stu-id="45626-342">Step 4.2: Encrypt your key by using Microsoft’s Key Exchange Key</span></span>
<span data-ttu-id="45626-343">Kör något av följande kommandon, beroende på din geografiskt område eller en instans av Azure:</span><span class="sxs-lookup"><span data-stu-id="45626-343">Run one of the following commands, depending on your geographic region or instance of Azure:</span></span>

* <span data-ttu-id="45626-344">För Nordamerika:</span><span class="sxs-lookup"><span data-stu-id="45626-344">For North America:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="45626-345">För Europa:</span><span class="sxs-lookup"><span data-stu-id="45626-345">For Europe:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="45626-346">För Asien:</span><span class="sxs-lookup"><span data-stu-id="45626-346">For Asia:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="45626-347">För Latinamerika:</span><span class="sxs-lookup"><span data-stu-id="45626-347">For Latin America:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="45626-348">För Japan:</span><span class="sxs-lookup"><span data-stu-id="45626-348">For Japan:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="45626-349">För Korea:</span><span class="sxs-lookup"><span data-stu-id="45626-349">For Korea:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-KOREA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-KOREA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="45626-350">För Australien:</span><span class="sxs-lookup"><span data-stu-id="45626-350">For Australia:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="45626-351">För [Azure Government](https://azure.microsoft.com/features/gov/), som använder US government instans av Azure:</span><span class="sxs-lookup"><span data-stu-id="45626-351">For [Azure Government](https://azure.microsoft.com/features/gov/), which uses the US government instance of Azure:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="45626-352">För USA: S regering DOD:</span><span class="sxs-lookup"><span data-stu-id="45626-352">For US Government DOD:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USDOD-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USDOD-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="45626-353">För Kanada:</span><span class="sxs-lookup"><span data-stu-id="45626-353">For Canada:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="45626-354">Tyskland:</span><span class="sxs-lookup"><span data-stu-id="45626-354">For Germany:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="45626-355">För Indien:</span><span class="sxs-lookup"><span data-stu-id="45626-355">For India:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey

<span data-ttu-id="45626-356">Följ dessa instruktioner när du kör det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="45626-356">When you run this command, use these instructions:</span></span>

* <span data-ttu-id="45626-357">Ersätt *contosokey* med den identifierare som du använde för att generera nyckeln i **steg 3.5: skapa en ny nyckel** från den [generera din nyckel](#step-3-generate-your-key) steg.</span><span class="sxs-lookup"><span data-stu-id="45626-357">Replace *contosokey* with the identifier that you used to generate the key in **Step 3.5: Create a new key** from the [Generate your key](#step-3-generate-your-key) step.</span></span>
* <span data-ttu-id="45626-358">Ersätt *SubscriptionID* med ID: T för Azure-prenumeration som innehåller nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="45626-358">Replace *SubscriptionID* with the ID of the Azure subscription that contains your key vault.</span></span> <span data-ttu-id="45626-359">Detta värde hämtades tidigare i **steg 1.2: hämta ditt Azure-prenumeration ID** från den [Förbered din Internetanslutna arbetsstation](#step-1-prepare-your-internet-connected-workstation) steg.</span><span class="sxs-lookup"><span data-stu-id="45626-359">You retrieved this value previously, in **Step 1.2: Get your Azure subscription ID** from the [Prepare your Internet-connected workstation](#step-1-prepare-your-internet-connected-workstation) step.</span></span>
* <span data-ttu-id="45626-360">Ersätt *ContosoFirstHSMKey* med en etikett som används för utdata-filnamn.</span><span class="sxs-lookup"><span data-stu-id="45626-360">Replace *ContosoFirstHSMKey* with a label that is used for your output file name.</span></span>

<span data-ttu-id="45626-361">När detta är klar visas **resultat: lyckades** och det finns en ny fil i den aktuella mappen med följande namn: keytransferpackage*ContosoFirstHSMkey*.byok</span><span class="sxs-lookup"><span data-stu-id="45626-361">When this completes successfully, it displays **Result: SUCCESS** and there is a new file in the current folder that has the following name: KeyTransferPackage-*ContosoFirstHSMkey*.byok</span></span>

### <a name="step-43-copy-your-key-transfer-package-to-the-internet-connected-workstation"></a><span data-ttu-id="45626-362">Steg 4.3: Kopiera ditt nyckelöverföringspaket till den Internetanslutna arbetsstationen</span><span class="sxs-lookup"><span data-stu-id="45626-362">Step 4.3: Copy your key transfer package to the Internet-connected workstation</span></span>
<span data-ttu-id="45626-363">Använd en USB-enhet eller annan bärbar lagringsenhet för att kopiera utdatafilen från föregående steg (KeyTransferPackage ContosoFirstHSMkey.byok) till din Internetanslutna arbetsstation.</span><span class="sxs-lookup"><span data-stu-id="45626-363">Use a USB drive or other portable storage to copy the output file from the previous step (KeyTransferPackage-ContosoFirstHSMkey.byok) to your Internet-connected workstation.</span></span>

## <a name="step-5-transfer-your-key-to-azure-key-vault"></a><span data-ttu-id="45626-364">Steg 5: Överför din nyckel till Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="45626-364">Step 5: Transfer your key to Azure Key Vault</span></span>
<span data-ttu-id="45626-365">För det här sista steget, på den Internetanslutna arbetsstationen använder den [Lägg till AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurermkeyvaultkey) för att ladda upp nyckelöverföringspaketet som du kopierade från den frånkopplade arbetsstationen till Azure Key Vault HSM:</span><span class="sxs-lookup"><span data-stu-id="45626-365">For this final step, on the Internet-connected workstation, use the [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurermkeyvaultkey) cmdlet to upload the key transfer package that you copied from the disconnected workstation to the Azure Key Vault HSM:</span></span>

    Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMkey' -KeyFilePath 'c:\KeyTransferPackage-ContosoFirstHSMkey.byok' -Destination 'HSM'

<span data-ttu-id="45626-366">Om överföringen lyckas du se egenskaperna för den nyckel som du just lagt till.</span><span class="sxs-lookup"><span data-stu-id="45626-366">If the upload is successful, you see displayed the properties of the key that you just added.</span></span>

## <a name="next-steps"></a><span data-ttu-id="45626-367">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="45626-367">Next steps</span></span>
<span data-ttu-id="45626-368">Du kan nu använda den här HSM-skyddad nyckel i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="45626-368">You can now use this HSM-protected key in your key vault.</span></span> <span data-ttu-id="45626-369">Mer information finns i **om du vill använda en maskinvarusäkerhetsmodul (HSM)** under den [komma igång med Azure Key Vault](key-vault-get-started.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="45626-369">For more information, see the **If you want to use a hardware security module (HSM)** section in the [Getting started with Azure Key Vault](key-vault-get-started.md) tutorial.</span></span>
