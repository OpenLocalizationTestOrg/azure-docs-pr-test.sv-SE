---
title: "aaaInstall Mobilitetstjänsten (VMware eller fysiska tooAzure) | Microsoft Docs"
description: "Lär dig hur hello tooinstall Mobilitetstjänsten agent tooprotect lokala datorer."
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
ms.date: 06/29/2017
ms.author: anoopkv
ms.openlocfilehash: f7836e6b35d3838bae1eff927838ce4b245b9f56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-mobility-service-vmware-or-physical-tooazure"></a>Installera Mobilitetstjänsten (VMware eller fysiska tooAzure)
Azure Site Recovery-Mobilitetstjänsten samlar in dataskrivningar på en dator och vidarebefordrar dem toohello processervern. Distribuera Mobilitetstjänsten tooevery dator (VMware VM eller fysiska servrar som du vill tooreplicate tooAzure). Du kan distribuera Mobilitetstjänsten toohello servrar som du vill tooprotect med hjälp av hello följande metoder:


* [Installera Mobilitetstjänsten med hjälp av verktyg för programvarudistribution som System Center Configuration Manager](site-recovery-install-mobility-service-using-sccm.md)
* [Installera Mobilitetstjänsten med hjälp av Azure Automation och Desired State Configuration Automation DSC)](site-recovery-automate-mobility-service-install.md)
* [Installera Mobilitetstjänsten manuellt med hjälp av hello grafiskt användargränssnitt (GUI)](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui)
* [Installera Mobilitetstjänsten manuellt i en kommandotolk](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt)
* [Installera Mobilitetstjänsten genom push-installation från Azure Site Recovery](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery)


>[!IMPORTANT]
> Från och med version 9.7.0.0, på Windows-datorer (VM), hello Mobilitetstjänsten installerar installationsprogrammet också hello senaste tillgängliga [Virtuella Azure-datoragenten](../virtual-machines/windows/extensions-features.md#azure-vm-agent). När en dator växlar tooAzure, uppfyller hello datorn hello agentinstallation som är nödvändiga för att använda alla VM-tillägg.

## <a name="prerequisites"></a>Krav
Gör följande förutsättningar innan du installerar Mobilitetstjänsten manuellt på servern:
1. Logga in tooyour konfigurationsservern och öppna ett kommandotolksfönster som administratör.
2. Ändra hello directory toohello bin-mappen och sedan skapa en lösenfras fil:

    ```
    cd %ProgramData%\ASR\home\svsystems\bin
    genpassphrase.exe -v > MobSvc.passphrase
    ```
3. Lagra hello lösenfras på en säker plats. Du kan använda hello fil under hello installation av Mobilitetstjänsten.
4. Mobility Service installationsprogram för alla operativsystem som stöds finns i hello %ProgramData%\ASR\home\svsystems\pushinstallsvc\repository mappen.

### <a name="mobility-service-installer-to-operating-system-mapping"></a>Mobility-installationsprogram för operativsystem systemmappning

| Installer filnamn för mallen| Operativsystem |
|---|--|
|Microsoft ASR\_UA\*Windows\*release.exe | Windows Server 2008 R2 SP1 (64-bitars) </br> Windows Server 2012 (64-bitars) </br> Windows Server 2012 R2 (64-bitars) |
|Microsoft ASR\_UA\*RHEL6 64*release.tar.gz| Red Hat Enterprise Linux (RHEL) 6.4, 6.5, 6.6, 6.7, 6.8 (endast 64-bitars) </br> CentOS 6.4, 6.5, 6.6, 6.7, 6.8 (endast 64-bitars) |
|Microsoft ASR\_UA\*RHEL7 64\*release.tar.gz | Red Hat Enterprise Linux (RHEL) 7.1, 7.2 (endast 64-bitars) </br> CentOS 7.0, 7.1, 7.2 (endast 64-bitars)</br> CentOs 7.3 (migrering) |
|Microsoft ASR\_UA\*SLES11-SP3-64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP3 (endast 64-bitars)|
|Microsoft ASR\_UA\*SLES11-SP4-64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP4 (endast 64-bitars)|
|Microsoft ASR\_UA\*OL6 64\*release.tar.gz | Oracle Enterprise Linux 6.4, 6.5 (endast 64-bitars)|
|Microsoft ASR\_UA\*UBUNTU-14.04-64\*release.tar.gz | Ubuntu Linux 14.04 (endast 64-bitars)|


## <a name="install-mobility-service-manually-by-using-hello-gui"></a>Installera Mobilitetstjänsten manuellt med hjälp av hello GUI

>[!IMPORTANT]
> Om du använder en **konfigurationsservern** tooreplicate **Azure IaaS-virtuella datorer** från en Azure-prenumeration/Region tooanother sedan **använda hello baserat kommandoradsinstallation**  metod

[!INCLUDE [site-recovery-install-mob-svc-gui](../../includes/site-recovery-install-mob-svc-gui.md)]

## <a name="install-mobility-service-manually-at-a-command-prompt"></a>Installera Mobilitetstjänsten manuellt i en kommandotolk

### <a name="command-line-installation-on-a-windows-computer"></a>Kommandoradsparametrarna för installation på en Windows-dator
[!INCLUDE [site-recovery-install-mob-svc-win-cmd](../../includes/site-recovery-install-mob-svc-win-cmd.md)]

### <a name="command-line-installation-on-a-linux-computer"></a>Kommandoradsparametrarna för installation på en Linux-dator
[!INCLUDE [site-recovery-install-mob-svc-lin-cmd](../../includes/site-recovery-install-mob-svc-lin-cmd.md)]


## <a name="install-mobility-service-by-push-installation-from-azure-site-recovery"></a>Installera Mobilitetstjänsten genom push-installation från Azure Site Recovery
toodo en push-installation av Mobilitetstjänsten genom att använda Site Recovery alla måldatorer måste uppfylla följande förutsättningar hello.

[!INCLUDE [site-recovery-prepare-push-install-mob-svc-win](../../includes/site-recovery-prepare-push-install-mob-svc-win.md)]

[!INCLUDE [site-recovery-prepare-push-install-mob-svc-lin](../../includes/site-recovery-prepare-push-install-mob-svc-lin.md)]


> [!NOTE]
När Mobilitetstjänsten är installerad i hello Azure-portalen markerar hello **replikera** knappen toostart skyddar dessa virtuella datorer.

## <a name="uninstall-mobility-service-on-a-windows-server-computer"></a>Avinstallera Mobilitetstjänsten på en dator med Windows Server
Använd någon av hello följande metoder toouninstall Mobilitetstjänsten på en Windows Server-dator.

### <a name="uninstall-by-using-hello-gui"></a>Avinstallera med hello GUI
1. På Kontrollpanelen, Välj **program**.
2. Välj **Microsoft Azure Site Recovery Mobility tjänsten eller Huvudtjänsten målservern**, och välj sedan **avinstallera**.

### <a name="uninstall-at-a-command-prompt"></a>Avinstallera i en kommandotolk
1. Öppna ett kommandotolksfönster som administratör.
2. toouninstall Mobilitetstjänsten, kör hello följande kommando:

```
MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
```

## <a name="uninstall-mobility-service-on-a-linux-computer"></a>Avinstallera Mobilitetstjänsten på en Linux-dator
1. Logga in på Linux-servern som en **rot** användare.
2. Gå för/användare/lokal/ASR i en terminal.
3. toouninstall Mobilitetstjänsten, kör hello följande kommando:

```
uninstall.sh -Y
```
