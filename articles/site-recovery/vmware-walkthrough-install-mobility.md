---
title: "aaaInstall hello mobilitetstjänsten för VMware tooAzure replikering | Microsoft Docs"
description: "Den här artikeln beskriver hur tooinstall hello mobilitetstjänstagenten för VMware tooAzure replikering med hello Azure Site Recovery-tjänsten."
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
ms.openlocfilehash: d3b7bc9c4d84d13317e0b0b47adcf38e8c41d0bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-10-install-hello-mobility-service"></a><span data-ttu-id="5bc8b-103">Steg 10: Installera hello mobilitetstjänsten</span><span class="sxs-lookup"><span data-stu-id="5bc8b-103">Step 10: Install hello Mobility service</span></span>


<span data-ttu-id="5bc8b-104">Den här artikeln beskriver hur tooconfigure käll- och inställningar när du replikerar lokalt VMware virtuella datorer tooAzure med hello [Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="5bc8b-104">This article describes how tooconfigure source and target settings when replicating on-premises VMware virtual machines tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>

<span data-ttu-id="5bc8b-105">Hej mobilitetstjänsten samlar in dataskrivningar på en dator och vidarebefordrar dem toohello processervern.</span><span class="sxs-lookup"><span data-stu-id="5bc8b-105">hello Mobility service captures data writes on a machine, and forwards them toohello process server.</span></span> <span data-ttu-id="5bc8b-106">Den bör installeras på varje dator som du vill tooreplicate tooAzure.</span><span class="sxs-lookup"><span data-stu-id="5bc8b-106">It should be installed on each machine that you want tooreplicate tooAzure.</span></span>

<span data-ttu-id="5bc8b-107">Du kan installera hello Mobility tjänsten manuellt med hjälp av en push-installation från hello Site Recovery processervern när replikering har aktiverats eller verktyget System Center Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="5bc8b-107">You can install hello Mobility service manual, using a push installation from hello Site Recovery process server when replication is enabled, or use a tool System Center Configuration Manager.</span></span> <span data-ttu-id="5bc8b-108">Om du använder push-installation installeras hello på hello VM när replikeringen är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="5bc8b-108">If you use push installation, hello service is installed on hello VM when replication is enabled.</span></span>

<span data-ttu-id="5bc8b-109">Publicera kommentarer och frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="5bc8b-109">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="install-manually"></a><span data-ttu-id="5bc8b-110">Installera manuellt</span><span class="sxs-lookup"><span data-stu-id="5bc8b-110">Install manually</span></span>

1. <span data-ttu-id="5bc8b-111">Kontrollera hello [krav](site-recovery-vmware-to-azure-install-mob-svc.md#prerequisites) för manuell installation.</span><span class="sxs-lookup"><span data-stu-id="5bc8b-111">Check hello [prerequisites](site-recovery-vmware-to-azure-install-mob-svc.md#prerequisites) for manual installation.</span></span>
2. <span data-ttu-id="5bc8b-112">Följ [instruktionerna](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) för manuell installation med hjälp av hello portal.</span><span class="sxs-lookup"><span data-stu-id="5bc8b-112">Follow [these instructions](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) for manual installation using hello portal.</span></span>
3. <span data-ttu-id="5bc8b-113">Om du föredrar tooinstall från kommandoraden hello följer [instruktionerna](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt).</span><span class="sxs-lookup"><span data-stu-id="5bc8b-113">If you prefer tooinstall from hello command line, follow [these instructions](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt).</span></span>

## <a name="install-from-hello-process-server"></a><span data-ttu-id="5bc8b-114">Installera från hello processervern</span><span class="sxs-lookup"><span data-stu-id="5bc8b-114">Install from hello process server</span></span>

<span data-ttu-id="5bc8b-115">Om du vill toopush hello Mobility Tjänstinstallationen från hello processervern när du aktiverar replikering för en virtuell dator, måste ett konto som kan användas av hello processen server tooaccess hello VM.</span><span class="sxs-lookup"><span data-stu-id="5bc8b-115">If you want toopush hello Mobility service installation from hello process server when you enable replication for a VM, you need an account that can be used by hello process server tooaccess hello VM.</span></span> <span data-ttu-id="5bc8b-116">hello kontot används bara för hello push-installation.</span><span class="sxs-lookup"><span data-stu-id="5bc8b-116">hello account is only used for hello push installation.</span></span>

1. <span data-ttu-id="5bc8b-117">Du bör ha [skapat ett konto](vmware-walkthrough-prepare-vmware.md) som kan användas för push-installation.</span><span class="sxs-lookup"><span data-stu-id="5bc8b-117">You should have [created an account](vmware-walkthrough-prepare-vmware.md) that can be used for push installation.</span></span> <span data-ttu-id="5bc8b-118">Sedan kan du ange hello-konto som du vill toouse när du konfigurerar inställningar för datakälla under distributionen av Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="5bc8b-118">You then specify hello account you want toouse when you configure source settings during Site Recovery deployment.</span></span>
2. <span data-ttu-id="5bc8b-119">Följ [instruktionerna](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) om du vill toopush hello mobilitetstjänsten på virtuella datorer som kör Windows eller Linux.</span><span class="sxs-lookup"><span data-stu-id="5bc8b-119">Then follow [these instructions](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) if you want toopush hello Mobility service on VMs running Windows or Linux.</span></span>

## <a name="other-methods"></a><span data-ttu-id="5bc8b-120">Andra metoder</span><span class="sxs-lookup"><span data-stu-id="5bc8b-120">Other methods</span></span>

<span data-ttu-id="5bc8b-121">Lär dig mer om [hello mobilitetstjänsten med Configuration Manager installeras](site-recovery-install-mobility-service-using-sccm.md), eller med hjälp av [Azure Automation DSC](site-recovery-automate-mobility-service-install.md).</span><span class="sxs-lookup"><span data-stu-id="5bc8b-121">Learn more about [installing hello Mobility service using Configuration Manager](site-recovery-install-mobility-service-using-sccm.md), or using [Azure Automation DSC](site-recovery-automate-mobility-service-install.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="5bc8b-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5bc8b-122">Next steps</span></span>

<span data-ttu-id="5bc8b-123">Gå för[steg 11: Aktivera replikering](vmware-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="5bc8b-123">Go too[Step 11: Enable replication](vmware-walkthrough-enable-replication.md)</span></span>
