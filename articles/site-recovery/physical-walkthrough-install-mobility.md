---
title: "aaaInstall hello mobilitetstjänsten för fysisk server tooAzure replikering | Microsoft Docs"
description: "Den här artikeln beskriver hur tooinstall hello mobilitetstjänstagenten på fysiska servrar som replikerar tooAzure med hello Azure Site Recovery-tjänsten."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 3189fbcd-6b5b-4ffb-b5a9-e2080c37f9d9
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 48fd2c0ffe67875ed446c8167c2ae7f90d3f537c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-9-install-hello-mobility-service"></a><span data-ttu-id="ef1ad-103">Steg 9: Installera hello mobilitetstjänsten</span><span class="sxs-lookup"><span data-stu-id="ef1ad-103">Step 9: Install hello Mobility service</span></span>


<span data-ttu-id="ef1ad-104">Den här artikeln beskriver hur tooinstall hello mobilitetstjänsten vid replikering av lokala Windows-/ Linux fysiska servrar tooAzure, med hjälp av hello [Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ef1ad-104">This article describes how tooinstall hello Mobility service component when replicating on-premises Windows/Linux physical servers tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>

<span data-ttu-id="ef1ad-105">Hej mobilitetstjänsten samlar in dataskrivningar på en dator och vidarebefordrar dem toohello processervern.</span><span class="sxs-lookup"><span data-stu-id="ef1ad-105">hello Mobility service captures data writes on a machine, and forwards them toohello process server.</span></span> <span data-ttu-id="ef1ad-106">Den bör installeras på varje server som du vill tooreplicate tooAzure.</span><span class="sxs-lookup"><span data-stu-id="ef1ad-106">It should be installed on each server that you want tooreplicate tooAzure.</span></span>

<span data-ttu-id="ef1ad-107">Du kan installera hello mobilitetstjänsten manuellt eller med hjälp av en push-installation från hello Site Recovery bearbeta servern när replikering har aktiverats eller med ett verktyg som System Center Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="ef1ad-107">You can install hello Mobility service manually, or using a push installation from hello Site Recovery process server when replication is enabled, or using a tool such as System Center Configuration Manager.</span></span> <span data-ttu-id="ef1ad-108">Om du använder push-installation installeras hello på hello servern när du aktiverar replikering.</span><span class="sxs-lookup"><span data-stu-id="ef1ad-108">If you use push installation, hello service is installed on hello server when you enable replication.</span></span>

<span data-ttu-id="ef1ad-109">Publicera kommentarer och frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="ef1ad-109">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="install-manually"></a><span data-ttu-id="ef1ad-110">Installera manuellt</span><span class="sxs-lookup"><span data-stu-id="ef1ad-110">Install manually</span></span>

1. <span data-ttu-id="ef1ad-111">Kontrollera hello [krav](site-recovery-vmware-to-azure-install-mob-svc.md#prerequisites) för manuell installation.</span><span class="sxs-lookup"><span data-stu-id="ef1ad-111">Check hello [prerequisites](site-recovery-vmware-to-azure-install-mob-svc.md#prerequisites) for manual installation.</span></span>
2. <span data-ttu-id="ef1ad-112">Följ [instruktionerna](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) för manuell installation med hjälp av hello portal.</span><span class="sxs-lookup"><span data-stu-id="ef1ad-112">Follow [these instructions](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) for manual installation using hello portal.</span></span>
3. <span data-ttu-id="ef1ad-113">Om du föredrar tooinstall från kommandoraden hello följer [instruktionerna](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt).</span><span class="sxs-lookup"><span data-stu-id="ef1ad-113">If you prefer tooinstall from hello command line, follow [these instructions](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt).</span></span>

## <a name="install-from-hello-process-server"></a><span data-ttu-id="ef1ad-114">Installera från hello processervern</span><span class="sxs-lookup"><span data-stu-id="ef1ad-114">Install from hello process server</span></span>

<span data-ttu-id="ef1ad-115">Om du vill toopush hello Mobility Tjänstinstallationen från hello processervern när du aktiverar replikering för en dator, måste ett konto som kan användas av hello processen tooaccess hello-serverdatorn.</span><span class="sxs-lookup"><span data-stu-id="ef1ad-115">If you want toopush hello Mobility service installation from hello process server when you enable replication for a machine, you need an account that can be used by hello process server tooaccess hello machine.</span></span> <span data-ttu-id="ef1ad-116">hello kontot används bara för hello push-installation.</span><span class="sxs-lookup"><span data-stu-id="ef1ad-116">hello account is only used for hello push installation.</span></span>

1. <span data-ttu-id="ef1ad-117">Om du inte har skapat ett konto kan du göra det med hjälp av dessa riktlinjer:</span><span class="sxs-lookup"><span data-stu-id="ef1ad-117">If you haven't created an account, do so using these guidelines:</span></span>

    - <span data-ttu-id="ef1ad-118">Du kan använda en domän eller lokalt konto</span><span class="sxs-lookup"><span data-stu-id="ef1ad-118">You can use a domain or local account</span></span>
    - <span data-ttu-id="ef1ad-119">För Windows, om du inte använder ett domänkonto måste toodisable fjärranslutna användare åtkomstkontroll på hello lokal dator.</span><span class="sxs-lookup"><span data-stu-id="ef1ad-119">For Windows, if you're not using a domain account, you need toodisable Remote User Access control on hello local machine.</span></span> <span data-ttu-id="ef1ad-120">toodo, hello registrera under **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, lägga till DWORD-posten för hello **LocalAccountTokenFilterPolicy**, med värdet 1.</span><span class="sxs-lookup"><span data-stu-id="ef1ad-120">toodo this, in hello register under **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, add hello DWORD entry **LocalAccountTokenFilterPolicy**, with a value of 1.</span></span>
    - <span data-ttu-id="ef1ad-121">Om du vill tooadd hello registerposten för Windows från en CLI, skriver du:</span><span class="sxs-lookup"><span data-stu-id="ef1ad-121">If you want tooadd hello registry entry for Windows from a CLI, type:</span></span>

        ```
        REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.
        ```

    - <span data-ttu-id="ef1ad-122">För Linux ska hello kontot vara rot på hello Linux källservern.</span><span class="sxs-lookup"><span data-stu-id="ef1ad-122">For Linux, hello account should be root on hello source Linux server.</span></span>

2. <span data-ttu-id="ef1ad-123">Följ [instruktionerna](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) om du vill toopush hello mobilitetstjänsten på virtuella datorer som kör Windows eller Linux.</span><span class="sxs-lookup"><span data-stu-id="ef1ad-123">Then follow [these instructions](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) if you want toopush hello Mobility service on VMs running Windows or Linux.</span></span>

## <a name="other-installation-methods"></a><span data-ttu-id="ef1ad-124">Andra installationsmetoder</span><span class="sxs-lookup"><span data-stu-id="ef1ad-124">Other installation methods</span></span>

- <span data-ttu-id="ef1ad-125">[Lär dig mer om](site-recovery-install-mobility-service-using-sccm.md) hello mobilitetstjänsten med Configuration Manager installeras</span><span class="sxs-lookup"><span data-stu-id="ef1ad-125">[Learn about](site-recovery-install-mobility-service-using-sccm.md) installing hello Mobility service using Configuration Manager</span></span>
- <span data-ttu-id="ef1ad-126">[Lär dig mer om](site-recovery-automate-mobility-service-install.md) installerar med Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="ef1ad-126">[Learn about](site-recovery-automate-mobility-service-install.md) installing with Azure Automation DSC.</span></span>


## <a name="next-steps"></a><span data-ttu-id="ef1ad-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ef1ad-127">Next steps</span></span>

<span data-ttu-id="ef1ad-128">Gå för[steg 10: Aktivera replikering](physical-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="ef1ad-128">Go too[Step 10: Enable replication](physical-walkthrough-enable-replication.md)</span></span>
