---
title: "Installera mobilitetstjänsten för VMware på Azure replikering | Microsoft Docs"
description: "Den här artikeln beskriver hur du installerar mobilitetstjänstagenten för VMware på Azure replikering med Azure Site Recovery-tjänsten."
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
ms.openlocfilehash: bc520bd2ea54208889861a7a3b275e3008a05d53
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="step-10-install-the-mobility-service"></a><span data-ttu-id="b1e21-103">Steg 10: Installera mobilitetstjänsten</span><span class="sxs-lookup"><span data-stu-id="b1e21-103">Step 10: Install the Mobility service</span></span>


<span data-ttu-id="b1e21-104">Den här artikeln beskriver hur du konfigurerar inställningar för källa och mål när replikera lokala virtuella VMware-datorer till Azure, med hjälp av den [Azure Site Recovery](site-recovery-overview.md) tjänsten i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b1e21-104">This article describes how to configure source and target settings when replicating on-premises VMware virtual machines to Azure, using the [Azure Site Recovery](site-recovery-overview.md) service in the Azure portal.</span></span>

<span data-ttu-id="b1e21-105">Mobilitetstjänsten samlar in dataskrivningar på en dator och vidarebefordrar dem till processervern.</span><span class="sxs-lookup"><span data-stu-id="b1e21-105">The Mobility service captures data writes on a machine, and forwards them to the process server.</span></span> <span data-ttu-id="b1e21-106">Den bör installeras på varje dator som du vill replikera till Azure.</span><span class="sxs-lookup"><span data-stu-id="b1e21-106">It should be installed on each machine that you want to replicate to Azure.</span></span>

<span data-ttu-id="b1e21-107">Du kan installera Mobility service manuell, med hjälp av en push-installation från Site Recovery processervern när replikering har aktiverats eller verktyget System Center Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="b1e21-107">You can install the Mobility service manual, using a push installation from the Site Recovery process server when replication is enabled, or use a tool System Center Configuration Manager.</span></span> <span data-ttu-id="b1e21-108">Om du använder push-installation av är tjänsten installerad på den virtuella datorn när replikeringen är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="b1e21-108">If you use push installation, the service is installed on the VM when replication is enabled.</span></span>

<span data-ttu-id="b1e21-109">Publicera kommentarer och frågor längst ned i den här artikeln eller i den [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="b1e21-109">Post comments and questions at the bottom of this article, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="install-manually"></a><span data-ttu-id="b1e21-110">Installera manuellt</span><span class="sxs-lookup"><span data-stu-id="b1e21-110">Install manually</span></span>

1. <span data-ttu-id="b1e21-111">Kontrollera den [krav](site-recovery-vmware-to-azure-install-mob-svc.md#prerequisites) för manuell installation.</span><span class="sxs-lookup"><span data-stu-id="b1e21-111">Check the [prerequisites](site-recovery-vmware-to-azure-install-mob-svc.md#prerequisites) for manual installation.</span></span>
2. <span data-ttu-id="b1e21-112">Följ [instruktionerna](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) för manuell installation med hjälp av portalen.</span><span class="sxs-lookup"><span data-stu-id="b1e21-112">Follow [these instructions](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) for manual installation using the portal.</span></span>
3. <span data-ttu-id="b1e21-113">Om du vill installera från kommandoraden följer [instruktionerna](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt).</span><span class="sxs-lookup"><span data-stu-id="b1e21-113">If you prefer to install from the command line, follow [these instructions](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt).</span></span>

## <a name="install-from-the-process-server"></a><span data-ttu-id="b1e21-114">Installera från processervern</span><span class="sxs-lookup"><span data-stu-id="b1e21-114">Install from the process server</span></span>

<span data-ttu-id="b1e21-115">Om du vill skicka Mobility Tjänstinstallationen från processervern när du aktiverar replikering för en virtuell dator behöver du ett konto som kan användas av processervern för att få åtkomst till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="b1e21-115">If you want to push the Mobility service installation from the process server when you enable replication for a VM, you need an account that can be used by the process server to access the VM.</span></span> <span data-ttu-id="b1e21-116">Kontot används endast för push-installation.</span><span class="sxs-lookup"><span data-stu-id="b1e21-116">The account is only used for the push installation.</span></span>

1. <span data-ttu-id="b1e21-117">Du bör ha [skapat ett konto](vmware-walkthrough-prepare-vmware.md) som kan användas för push-installation.</span><span class="sxs-lookup"><span data-stu-id="b1e21-117">You should have [created an account](vmware-walkthrough-prepare-vmware.md) that can be used for push installation.</span></span> <span data-ttu-id="b1e21-118">Sedan kan du ange det konto som du vill använda när du konfigurerar inställningar för datakälla under distributionen av Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="b1e21-118">You then specify the account you want to use when you configure source settings during Site Recovery deployment.</span></span>
2. <span data-ttu-id="b1e21-119">Följ [instruktionerna](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) om du vill skicka mobilitetstjänsten på virtuella datorer som kör Windows eller Linux.</span><span class="sxs-lookup"><span data-stu-id="b1e21-119">Then follow [these instructions](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) if you want to push the Mobility service on VMs running Windows or Linux.</span></span>

## <a name="other-methods"></a><span data-ttu-id="b1e21-120">Andra metoder</span><span class="sxs-lookup"><span data-stu-id="b1e21-120">Other methods</span></span>

<span data-ttu-id="b1e21-121">Lär dig mer om [mobilitetstjänsten med Configuration Manager installeras](site-recovery-install-mobility-service-using-sccm.md), eller med hjälp av [Azure Automation DSC](site-recovery-automate-mobility-service-install.md).</span><span class="sxs-lookup"><span data-stu-id="b1e21-121">Learn more about [installing the Mobility service using Configuration Manager](site-recovery-install-mobility-service-using-sccm.md), or using [Azure Automation DSC](site-recovery-automate-mobility-service-install.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b1e21-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b1e21-122">Next steps</span></span>

<span data-ttu-id="b1e21-123">Gå till [steg 11: Aktivera replikering](vmware-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="b1e21-123">Go to [Step 11: Enable replication](vmware-walkthrough-enable-replication.md)</span></span>
