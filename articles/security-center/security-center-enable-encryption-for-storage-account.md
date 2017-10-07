---
title: "aaaEnable kryptering för storage-konto i Azure Security Center | Microsoft Docs"
description: "Det här dokumentet beskrivs hur tooimplement hello Azure Security Center rekommendationer ** Aktivera kryptering för Azure Storage konto **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/20/2016
ms.author: terrylan
ms.openlocfilehash: c5cbafbf3a8be86f213dcf1c0c0ddcc0222b3d95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-encryption-for-azure-storage-account-in-azure-security-center"></a><span data-ttu-id="4b489-103">Aktivera kryptering för Azure storage-konto i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="4b489-103">Enable encryption for Azure storage account in Azure Security Center</span></span>
<span data-ttu-id="4b489-104">Azure Security Center kan rekommenderar att du aktiverar Azure Storage Service-kryptering för data i vila.</span><span class="sxs-lookup"><span data-stu-id="4b489-104">Azure Security Center may recommend that you enable Azure Storage Service Encryption for data at rest.</span></span>

<span data-ttu-id="4b489-105">Kryptering för lagring-tjänsten (SSE) fungerar genom att kryptera hello data när de skrivs tooAzure lagring och dekryptera hello data innan hämtning.</span><span class="sxs-lookup"><span data-stu-id="4b489-105">Storage Service Encryption (SSE) works by encrypting hello data when it is written tooAzure storage and decrypting hello data before retrieval.</span></span>  <span data-ttu-id="4b489-106">SSE är för närvarande endast tillgängligt för hello Azure Blob-tjänsten och kan användas för blockblobbar, sidblobbar, och tilläggsblobar.</span><span class="sxs-lookup"><span data-stu-id="4b489-106">SSE is currently available only for hello Azure Blob service and can be used for block blobs, page blobs, and append blobs.</span></span>  <span data-ttu-id="4b489-107">Det finns fler toolearn [Lagringstjänstens kryptering av vilande data](../storage/common/storage-service-encryption.md).</span><span class="sxs-lookup"><span data-stu-id="4b489-107">toolearn more, see [Storage Service Encryption for data at rest](../storage/common/storage-service-encryption.md).</span></span>


> [!Note]
> <span data-ttu-id="4b489-108">När du har aktiverat kryptering krypteras endast nya data.</span><span class="sxs-lookup"><span data-stu-id="4b489-108">After enabling encryption, only new data is encrypted.</span></span> <span data-ttu-id="4b489-109">Alla befintliga blobbar i ditt lagringskonto vara okrypterad.</span><span class="sxs-lookup"><span data-stu-id="4b489-109">Any existing blobs in your storage account remain unencrypted.</span></span> <span data-ttu-id="4b489-110">tooencrypt befintliga blobbar, se hello [vanliga frågor och svar Storage Service-kryptering](../storage/common/storage-service-encryption.md#frequently-asked-questions-about-storage-service-encryption-for-data-at-rest).</span><span class="sxs-lookup"><span data-stu-id="4b489-110">tooencrypt existing blobs, see hello [Storage Service Encryption FAQ](../storage/common/storage-service-encryption.md#frequently-asked-questions-about-storage-service-encryption-for-data-at-rest).</span></span>
>
>

<span data-ttu-id="4b489-111">Lagringstjänstens kryptering stöds endast i hanteraren för filserverresurser storage-konton.</span><span class="sxs-lookup"><span data-stu-id="4b489-111">Storage Service Encryption is only supported on Resource Manager storage accounts.</span></span> <span data-ttu-id="4b489-112">Klassiska lagringskonton stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="4b489-112">Classic storage accounts are not currently supported.</span></span> <span data-ttu-id="4b489-113">toounderstand hello klassiska och Resource Manager distributionsmodellerna finns [Azure distributionsmodeller](../azure-classic-rm.md).</span><span class="sxs-lookup"><span data-stu-id="4b489-113">toounderstand hello classic and Resource Manager deployment models, see [Azure deployment models](../azure-classic-rm.md).</span></span>

> [!NOTE]
> <span data-ttu-id="4b489-114">Det här dokumentet introducerar hello-tjänsten med hjälp av ett exempel på distribution.</span><span class="sxs-lookup"><span data-stu-id="4b489-114">This document introduces hello service by using an example deployment.</span></span>  <span data-ttu-id="4b489-115">Det här dokumentet är inte en stegvis guide.</span><span class="sxs-lookup"><span data-stu-id="4b489-115">This document is not a step-by-step guide.</span></span>
>
>

## <a name="implement-hello-recommendation"></a><span data-ttu-id="4b489-116">Implementera hello rekommendation</span><span class="sxs-lookup"><span data-stu-id="4b489-116">Implement hello recommendation</span></span>
1. <span data-ttu-id="4b489-117">I hello **rekommendationer** bladet väljer **Aktivera kryptering för Azure Storage-konto**.</span><span class="sxs-lookup"><span data-stu-id="4b489-117">In hello **Recommendations** blade, select **Enable encryption for Azure Storage Account**.</span></span>
   <span data-ttu-id="4b489-118">![Aktivera kryptering för lagringskonto][1]</span><span class="sxs-lookup"><span data-stu-id="4b489-118">![Enable encryption for storage account][1]</span></span>
2. <span data-ttu-id="4b489-119">Hej **aktivera lagringskryptering** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="4b489-119">hello **Enable storage encryption** blade opens.</span></span> <span data-ttu-id="4b489-120">Det här bladet visar hello Azure storage-konton där storage kryptering har inaktiverats.</span><span class="sxs-lookup"><span data-stu-id="4b489-120">This blade lists hello Azure storage accounts where storage encryption is disabled.</span></span> <span data-ttu-id="4b489-121">I det här exemplet ska vi väljer **storageacct1**.</span><span class="sxs-lookup"><span data-stu-id="4b489-121">In this example, let's select **storageacct1**.</span></span>
   <span data-ttu-id="4b489-122">![Aktivera lagringskryptering][2]</span><span class="sxs-lookup"><span data-stu-id="4b489-122">![Enable storage encryption][2]</span></span>
3. <span data-ttu-id="4b489-123">Hej **kryptering** bladet för **storageacct1** öppnas.</span><span class="sxs-lookup"><span data-stu-id="4b489-123">hello **Encryption** blade for **storageacct1** opens.</span></span> <span data-ttu-id="4b489-124">Välj **aktiverat**.</span><span class="sxs-lookup"><span data-stu-id="4b489-124">Select **Enabled**.</span></span>
   <span data-ttu-id="4b489-125">![Kryptering bladet][3]</span><span class="sxs-lookup"><span data-stu-id="4b489-125">![Encryption blade][3]</span></span>
4. <span data-ttu-id="4b489-126">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="4b489-126">Select **Save**.</span></span>

<span data-ttu-id="4b489-127">Du har nu aktiverats lagringskryptering för **storageacct1**.</span><span class="sxs-lookup"><span data-stu-id="4b489-127">You have now enabled storage encryption for **storageacct1**.</span></span>


## <a name="see-also"></a><span data-ttu-id="4b489-128">Se även</span><span class="sxs-lookup"><span data-stu-id="4b489-128">See also</span></span>
<span data-ttu-id="4b489-129">Det här dokumentet visar dig hur hello tooimplement Security Center rekommendation ”Aktivera kryptering för Azure Storage-konto”.</span><span class="sxs-lookup"><span data-stu-id="4b489-129">This document showed you how tooimplement hello Security Center recommendation "Enable encryption for Azure Storage Account."</span></span> <span data-ttu-id="4b489-130">toolearn mer om Azure Storage Service-kryptering, finns följande hello:</span><span class="sxs-lookup"><span data-stu-id="4b489-130">toolearn more about Azure Storage Service Encryption, see hello following:</span></span>

* [<span data-ttu-id="4b489-131">Azure Storage Service-kryptering av vilande Data</span><span class="sxs-lookup"><span data-stu-id="4b489-131">Azure Storage Service Encryption for Data at Rest</span></span>](../storage/common/storage-service-encryption.md)

<span data-ttu-id="4b489-132">toolearn mer om Security Center finns hello följande:</span><span class="sxs-lookup"><span data-stu-id="4b489-132">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="4b489-133">[Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Lär dig hur tooconfigure säkerhetsprinciper för dina Azure-prenumerationer och resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="4b489-133">[Setting security policies in Azure Security Center](security-center-policies.md) - Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="4b489-134">[Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md) – Lär dig hur toomonitor hello Azure-resursers hälsa.</span><span class="sxs-lookup"><span data-stu-id="4b489-134">[Security health monitoring in Azure Security Center](security-center-monitoring.md) - Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="4b489-135">[Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar.</span><span class="sxs-lookup"><span data-stu-id="4b489-135">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) - Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="4b489-136">[Hantera säkerhetsrekommendationer i Azure Security Center](security-center-recommendations.md) – Lär dig rekommendationer för att skydda dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="4b489-136">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) - Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="4b489-137">[Vanliga frågor om Azure Security Center](security-center-faq.md) -finns vanliga frågor om hur du använder hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="4b489-137">[Azure Security Center FAQ](security-center-faq.md) - Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="4b489-138">[Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) -hittar du blogginlägg om säkerhet och Azure kompatibilitet.</span><span class="sxs-lookup"><span data-stu-id="4b489-138">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) - Find blog posts about Azure security and compliance.</span></span>

<!--Image references-->
[1]: ./media/security-center-enable-encryption-for-storage-account/enable-encryption-for-storage-account.png
[2]: ./media/security-center-enable-encryption-for-storage-account/enable-storage-encryption.png
[3]: ./media/security-center-enable-encryption-for-storage-account/encryption-blade.png
