---
title: kryptering av aaaApply i Azure Security Center | Microsoft Docs
description: "Det här dokumentet beskrivs hur tooimplement hello Azure Security Center rekommendation ** gäller disk encryption **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 6cc7824a-8d6b-4a5f-ab40-e3bbaebc4a91
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: cd803f1120018c5c86da91186eec1e59d425ede7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="apply-disk-encryption-in-azure-security-center"></a><span data-ttu-id="ef2b9-103">Tillämpa diskkryptering i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="ef2b9-103">Apply disk encryption in Azure Security Center</span></span>
<span data-ttu-id="ef2b9-104">Azure Security Center rekommenderar att du tillämpar kryptering om du har Windows eller Linux VM diskar som inte är krypterade med hjälp av Azure Disk Encryption.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-104">Azure Security Center recommends that you apply disk encryption if you have Windows or Linux VM disks that are not encrypted using Azure Disk Encryption.</span></span> <span data-ttu-id="ef2b9-105">Diskkryptering kan du kryptera din Windows- och Linux IaaS-VM-diskar.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-105">Disk Encryption lets you encrypt your Windows and Linux IaaS VM disks.</span></span>  <span data-ttu-id="ef2b9-106">Kryptering rekommenderas för både hello Operativsystemet och datavolymer på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-106">Encryption is recommended for both hello OS and data volumes on your VM.</span></span>

<span data-ttu-id="ef2b9-107">Diskkryptering använder hello branschstandard [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) funktion i Windows och hello [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) funktion i Linux.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-107">Disk Encryption uses hello industry standard [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) feature of Windows and hello [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) feature of Linux.</span></span> <span data-ttu-id="ef2b9-108">De här funktionerna ger OS och data kryptering toohelp skydda och skydda dina data och uppfylla din organisations säkerhet och efterlevnad åtaganden.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-108">These features provide OS and data encryption toohelp protect and safeguard your data and meet your organizational security and compliance commitments.</span></span> <span data-ttu-id="ef2b9-109">Kryptering är integrerad med [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) toohelp du styra och hantera hello disk krypteringsnycklar och hemligheter i prenumerationen Key Vault samtidigt som du säkerställer att krypteras alla data i hello Virtuella diskar i vila i din [ Azure Storage](https://azure.microsoft.com/documentation/services/storage/).</span><span class="sxs-lookup"><span data-stu-id="ef2b9-109">Disk Encryption is integrated with [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) toohelp you control and manage hello disk encryption keys and secrets in your Key Vault subscription, while ensuring that all data in hello VM disks are encrypted at rest in your [Azure Storage](https://azure.microsoft.com/documentation/services/storage/).</span></span>

> [!NOTE]
> <span data-ttu-id="ef2b9-110">Azure Disk Encryption stöds på hello efter Windows server-operativsystem - Windows Server 2008 R2, Windows Server 2012 och Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-110">Azure Disk Encryption is supported on hello following Windows server operating systems - Windows Server 2008 R2, Windows Server 2012, and Windows Server 2012 R2.</span></span> <span data-ttu-id="ef2b9-111">Diskkryptering stöds på hello följande Linux serveroperativsystem - Ubuntu och CentOS, SUSE SUSE Linux Enterprise Server (SLES).</span><span class="sxs-lookup"><span data-stu-id="ef2b9-111">Disk encryption is supported on hello following Linux server operating systems - Ubuntu, CentOS, SUSE, and SUSE Linux Enterprise Server (SLES).</span></span>
>
>

## <a name="implement-hello-recommendation"></a><span data-ttu-id="ef2b9-112">Implementera hello rekommendation</span><span class="sxs-lookup"><span data-stu-id="ef2b9-112">Implement hello recommendation</span></span>
1. <span data-ttu-id="ef2b9-113">I hello **rekommendationer** bladet väljer **gäller kryptering av**.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-113">In hello **Recommendations** blade, select **Apply disk encryption**.</span></span>
2. <span data-ttu-id="ef2b9-114">I hello **gäller diskkryptering** bladet visas en lista över virtuella datorer som diskkryptering rekommenderas.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-114">In hello **Apply disk encryption** blade, you see a list of VMs for which Disk Encryption is recommended.</span></span>
3. <span data-ttu-id="ef2b9-115">Följ hello instruktioner tooapply kryptering toothese virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-115">Follow hello instructions tooapply encryption toothese VMs.</span></span>

![][1]

<span data-ttu-id="ef2b9-116">tooencrypt Azure virtuella datorer som identifierats av Security Center behov av kryptering, rekommenderar vi hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="ef2b9-116">tooencrypt Azure Virtual Machines that have been identified by Security Center as needing encryption, we recommend hello following steps:</span></span>

* <span data-ttu-id="ef2b9-117">Installera och konfigurera Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-117">Install and configure Azure PowerShell.</span></span> <span data-ttu-id="ef2b9-118">Detta gör att du toorun hello PowerShell-kommandon krävs tooset in hello krav krävs tooencrypt Azure virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-118">This enables you toorun hello PowerShell commands required tooset up hello prerequisites required tooencrypt Azure Virtual Machines.</span></span>
* <span data-ttu-id="ef2b9-119">Hämta och köra hello Azure Disk Encryption krav Azure PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-119">Obtain and run hello Azure Disk Encryption Prerequisites Azure PowerShell script.</span></span>
* <span data-ttu-id="ef2b9-120">Kryptera dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-120">Encrypt your virtual machines.</span></span>

<span data-ttu-id="ef2b9-121">[Kryptera en virtuell dator i Azure](security-center-disk-encryption.md) vägleder dig genom stegen.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-121">[Encrypt an Azure Virtual Machine](security-center-disk-encryption.md) walks you through these steps.</span></span>  <span data-ttu-id="ef2b9-122">Det här avsnittet förutsätter att du använder Windows 10 som hello klientdatorn där du konfigurera diskkryptering.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-122">This topic assumes you are using Windows 10 as hello client machine from which you configure disk encryption.</span></span>

<span data-ttu-id="ef2b9-123">Det finns flera metoder som kan användas för virtuella datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-123">There are many approaches that can be used for Azure Virtual Machines.</span></span> <span data-ttu-id="ef2b9-124">Om du redan känner till väl Azure PowerShell eller Azure CLI, kanske du hellre toouse alternativa metoder.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-124">If you are already well-versed in Azure PowerShell or Azure CLI, then you may prefer toouse alternate approaches.</span></span> <span data-ttu-id="ef2b9-125">toolearn om dessa metoder finns [Azure disk encryption](../security/azure-security-disk-encryption.md).</span><span class="sxs-lookup"><span data-stu-id="ef2b9-125">toolearn about these other approaches, see [Azure disk encryption](../security/azure-security-disk-encryption.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="ef2b9-126">Se även</span><span class="sxs-lookup"><span data-stu-id="ef2b9-126">See also</span></span>
<span data-ttu-id="ef2b9-127">Det här dokumentet visar dig hur hello tooimplement Security Center rekommendation ”Använd disk encryption”.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-127">This document showed you how tooimplement hello Security Center recommendation "Apply disk encryption."</span></span> <span data-ttu-id="ef2b9-128">toolearn mer om diskkryptering, se hello följande:</span><span class="sxs-lookup"><span data-stu-id="ef2b9-128">toolearn more about disk encryption, see hello following:</span></span>

* <span data-ttu-id="ef2b9-129">[Hantering av kryptering och nyckeln med Azure Key Vault](https://azure.microsoft.com/documentation/videos/azurecon-2015-encryption-and-key-management-with-azure-key-vault/) (video-36 min 39 sekund) – Lär dig hur toouse Diskhantering kryptering för virtuella IaaS-datorer och Azure Key Vault toohelp skydda och skydda dina data.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-129">[Encryption and key management with Azure Key Vault](https://azure.microsoft.com/documentation/videos/azurecon-2015-encryption-and-key-management-with-azure-key-vault/) (video, 36 min 39 sec) -- Learn how toouse disk encryption management for IaaS VMs and Azure Key Vault toohelp protect and safeguard your data.</span></span>
* <span data-ttu-id="ef2b9-130">[Kryptera en virtuell dator i Azure](security-center-disk-encryption.md) (dokument) – Lär dig hur tooencrypt Azure virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-130">[Encrypt an Azure Virtual Machine](security-center-disk-encryption.md) (document) -- Learn how tooencrypt Azure Virtual Machines.</span></span>
* <span data-ttu-id="ef2b9-131">[Azure disk encryption](../security/azure-security-disk-encryption.md) (dokument) – Lär dig hur tooenable disk encryption för Windows och Linux virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-131">[Azure disk encryption](../security/azure-security-disk-encryption.md) (document) -- Learn how tooenable disk encryption for Windows and Linux VMs.</span></span>

<span data-ttu-id="ef2b9-132">toolearn mer om Security Center finns hello följande:</span><span class="sxs-lookup"><span data-stu-id="ef2b9-132">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="ef2b9-133">[Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Lär dig hur tooconfigure säkerhetsprinciper.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-133">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how tooconfigure security policies.</span></span>
* <span data-ttu-id="ef2b9-134">[Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md) – Lär dig hur toomonitor hello Azure-resursers hälsa.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-134">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="ef2b9-135">[Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-135">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="ef2b9-136">[Hantera säkerhetsrekommendationer i Azure Security Center](security-center-recommendations.md) – Lär dig rekommendationer för att skydda dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-136">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="ef2b9-137">[Vanliga frågor om Azure Security Center](security-center-faq.md) --finns vanliga frågor om hur du använder hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-137">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="ef2b9-138">[Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) --hittar du blogginlägg om säkerhet och Azure kompatibilitet.</span><span class="sxs-lookup"><span data-stu-id="ef2b9-138">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Find blog posts about Azure security and compliance.</span></span>

<!--Image references-->
[1]: ./media/security-center-apply-disk-encryption/apply-disk-encryption.png
