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
# <a name="step-10-install-hello-mobility-service"></a>Steg 10: Installera hello mobilitetstjänsten


Den här artikeln beskriver hur tooconfigure käll- och inställningar när du replikerar lokalt VMware virtuella datorer tooAzure med hello [Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.

Hej mobilitetstjänsten samlar in dataskrivningar på en dator och vidarebefordrar dem toohello processervern. Den bör installeras på varje dator som du vill tooreplicate tooAzure.

Du kan installera hello Mobility tjänsten manuellt med hjälp av en push-installation från hello Site Recovery processervern när replikering har aktiverats eller verktyget System Center Configuration Manager. Om du använder push-installation installeras hello på hello VM när replikeringen är aktiverad.

Publicera kommentarer och frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="install-manually"></a>Installera manuellt

1. Kontrollera hello [krav](site-recovery-vmware-to-azure-install-mob-svc.md#prerequisites) för manuell installation.
2. Följ [instruktionerna](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) för manuell installation med hjälp av hello portal.
3. Om du föredrar tooinstall från kommandoraden hello följer [instruktionerna](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt).

## <a name="install-from-hello-process-server"></a>Installera från hello processervern

Om du vill toopush hello Mobility Tjänstinstallationen från hello processervern när du aktiverar replikering för en virtuell dator, måste ett konto som kan användas av hello processen server tooaccess hello VM. hello kontot används bara för hello push-installation.

1. Du bör ha [skapat ett konto](vmware-walkthrough-prepare-vmware.md) som kan användas för push-installation. Sedan kan du ange hello-konto som du vill toouse när du konfigurerar inställningar för datakälla under distributionen av Site Recovery.
2. Följ [instruktionerna](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) om du vill toopush hello mobilitetstjänsten på virtuella datorer som kör Windows eller Linux.

## <a name="other-methods"></a>Andra metoder

Lär dig mer om [hello mobilitetstjänsten med Configuration Manager installeras](site-recovery-install-mobility-service-using-sccm.md), eller med hjälp av [Azure Automation DSC](site-recovery-automate-mobility-service-install.md).

## <a name="next-steps"></a>Nästa steg

Gå för[steg 11: Aktivera replikering](vmware-walkthrough-enable-replication.md)
