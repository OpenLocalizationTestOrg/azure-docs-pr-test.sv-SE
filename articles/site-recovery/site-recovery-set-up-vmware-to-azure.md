---
title: "Ställ in hello källmiljön (VMware tooAzure) | Microsoft Docs"
description: "Den här artikeln beskriver hur tooset upp din lokala miljö toostart replikera VMware virtuella datorer tooAzure."
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
ms.workload: storage-backup-recovery
ms.date: 06/29/2017
ms.author: anoopkv
ms.openlocfilehash: cbeeffc1061b11223e12ff3e237fa7e55b999aca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-hello-source-environment-vmware-tooazure"></a>Ställ in hello källmiljön (VMware tooAzure)
> [!div class="op_single_selector"]
> * [VMware tooAzure](./site-recovery-set-up-vmware-to-azure.md)
> * [Fysisk tooAzure](./site-recovery-set-up-physical-to-azure.md)

Den här artikeln beskriver hur tooset upp din lokala miljö toostart replikera virtuella datorer som körs på VMware tooAzure.

## <a name="prerequisites"></a>Krav

hello artikeln förutsätter att du redan har skapat:
- Recovery Services-ventilen i hello [Azure-portalen](http://portal.azure.com "Azure-portalen").
- Ett särskilt konto i din VMware vCenter som kan användas för [automatisk identifiering](./site-recovery-vmware-to-azure.md).
- En virtuell dator på vilken tooinstall hello konfigurationsservern.

## <a name="configuration-server-minimum-requirements"></a>Minimikrav för konfiguration av servern
hello configuration server-program ska distribueras på en virtuell dator för hög tillgänglighet VMware. hello i den följande tabellen listas hello minimikraven för maskinvara, programvara och nätverkskraven för en konfigurationsserver.
[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

> [!NOTE]
> HTTPS-baserade proxyservrar stöds inte av hello konfigurationsservern.

## <a name="choose-your-protection-goals"></a>Välja skyddsmål

1. I hello Azure-portalen, går toohello **återställningstjänster** valvet bladet och välj ditt valv.
2. Hello resurs menyn hello valvet, gå för**komma igång** > **Site Recovery** > **steg 1: Förbered infrastrukturen**  >  **Skyddsmål**.

    ![Välja mål](./media/site-recovery-set-up-vmware-to-azure/choose-goals.png)
3. I **skyddsmål**väljer **tooAzure**, och välj **Ja, med VMware vSphere-Hypervisor**. Klicka sedan på **OK**.

    ![Välja mål](./media/site-recovery-set-up-vmware-to-azure/choose-goals2.png)

## <a name="set-up-hello-source-environment"></a>Ställ in hello källmiljön
Konfigurera hello källmiljön omfattar två huvudsakliga aktiviteter:

- Installera och registrera en konfigurationsserver med Site Recovery.
- Identifiera dina lokala virtuella datorer genom att ansluta Site Recovery tooyour lokal VMware vCenter eller vSphere EXSi värdar.

### <a name="step-1-install-and-register-a-configuration-server"></a>Steg 1: Installera och registrera en konfigurationsserver

1. Klicka på **steg 1: förbereda infrastrukturen** > **källa**. I **Förbered källa**, om du inte har en konfigurationsservern och klicka på **+ konfigurationsservern** tooadd en.

    ![Konfigurera källan](./media/site-recovery-set-up-vmware-to-azure/set-source1.png)
2. På hello **Lägg till Server** bladet, kontrollera att **konfigurationsservern** visas i **servertyp**.
4. Hämta installationsfilen för hello Unified installationsprogram för Site Recovery.
5. Hämta hello valvregistreringsnyckel. Du behöver hello registreringsnyckel när du kör installationsprogrammet för enhetlig. hello nyckeln är giltig i fem dagar efter genereringen.

    ![Konfigurera källan](./media/site-recovery-set-up-vmware-to-azure/set-source2.png)
6. Kör på hello-dator som du använder som hello konfigurationsservern **Unified installationsprogram för Azure Site Recovery** tooinstall hello konfigurationsservern, hello processervern och hello master mål-servern.

#### <a name="run-azure-site-recovery-unified-setup"></a>Kör Azure Site Recovery enhetlig installation

> [!TIP]
> Registrera för konfiguration av servern misslyckas om hello tiden på datorns systemklocka skiljer sig från lokal tid med fler än fem minuter. Synkronisera systemklockan med en [tidsserver](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) innan du påbörjar installationen hello.

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> hello konfigurationsservern kan installeras via kommandoraden. Mer information finns i [installerar hello konfigurationsservern med hjälp av kommandoradsverktyg](http://aka.ms/installconfigsrv).

#### <a name="add-hello-vmware-account-for-automatic-discovery"></a>Lägg till hello VMware-konto för automatisk identifiering

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

### <a name="step-2-add-a-vcenter"></a>Steg 2: Lägg till en vCenter
tooallow Azure Site Recovery toodiscover virtuella datorer som körs i din lokala miljö, behöver du tooconnect din VMware vCenter-servern eller vSphere ESXi-värdar med Site Recovery.

Välj **+ vCenter** toostart ansluter en VMware vCenter-server eller en VMware vSphere ESXi-värd.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]


## <a name="common-issues"></a>Vanliga problem
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a>Nästa steg
[Konfigurera målmiljön](./site-recovery-prepare-target-vmware-to-azure.md) i Azure.
