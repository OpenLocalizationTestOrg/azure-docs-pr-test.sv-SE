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
# <a name="step-9-install-hello-mobility-service"></a>Steg 9: Installera hello mobilitetstjänsten


Den här artikeln beskriver hur tooinstall hello mobilitetstjänsten vid replikering av lokala Windows-/ Linux fysiska servrar tooAzure, med hjälp av hello [Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.

Hej mobilitetstjänsten samlar in dataskrivningar på en dator och vidarebefordrar dem toohello processervern. Den bör installeras på varje server som du vill tooreplicate tooAzure.

Du kan installera hello mobilitetstjänsten manuellt eller med hjälp av en push-installation från hello Site Recovery bearbeta servern när replikering har aktiverats eller med ett verktyg som System Center Configuration Manager. Om du använder push-installation installeras hello på hello servern när du aktiverar replikering.

Publicera kommentarer och frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="install-manually"></a>Installera manuellt

1. Kontrollera hello [krav](site-recovery-vmware-to-azure-install-mob-svc.md#prerequisites) för manuell installation.
2. Följ [instruktionerna](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) för manuell installation med hjälp av hello portal.
3. Om du föredrar tooinstall från kommandoraden hello följer [instruktionerna](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt).

## <a name="install-from-hello-process-server"></a>Installera från hello processervern

Om du vill toopush hello Mobility Tjänstinstallationen från hello processervern när du aktiverar replikering för en dator, måste ett konto som kan användas av hello processen tooaccess hello-serverdatorn. hello kontot används bara för hello push-installation.

1. Om du inte har skapat ett konto kan du göra det med hjälp av dessa riktlinjer:

    - Du kan använda en domän eller lokalt konto
    - För Windows, om du inte använder ett domänkonto måste toodisable fjärranslutna användare åtkomstkontroll på hello lokal dator. toodo, hello registrera under **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, lägga till DWORD-posten för hello **LocalAccountTokenFilterPolicy**, med värdet 1.
    - Om du vill tooadd hello registerposten för Windows från en CLI, skriver du:

        ```
        REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.
        ```

    - För Linux ska hello kontot vara rot på hello Linux källservern.

2. Följ [instruktionerna](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) om du vill toopush hello mobilitetstjänsten på virtuella datorer som kör Windows eller Linux.

## <a name="other-installation-methods"></a>Andra installationsmetoder

- [Lär dig mer om](site-recovery-install-mobility-service-using-sccm.md) hello mobilitetstjänsten med Configuration Manager installeras
- [Lär dig mer om](site-recovery-automate-mobility-service-install.md) installerar med Azure Automation DSC.


## <a name="next-steps"></a>Nästa steg

Gå för[steg 10: Aktivera replikering](physical-walkthrough-enable-replication.md)
