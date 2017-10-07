---
title: "Ställ in hello källmiljön (fysiska servrar tooAzure) | Microsoft Docs"
description: "Den här artikeln beskriver hur tooset upp din lokala miljö toostart replikera fysiska servrar som kör Windows eller Linux till Azure."
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
ms.openlocfilehash: d4702265bf36910015685d2bba99d6e577531bd0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-hello-source-environment-physical-server-tooazure"></a>Ställ in hello källmiljön (fysisk server tooAzure)
> [!div class="op_single_selector"]
> * [VMware tooAzure](./site-recovery-set-up-vmware-to-azure.md)
> * [Fysisk tooAzure](./site-recovery-set-up-physical-to-azure.md)

Den här artikeln beskriver hur tooset upp din lokala miljö toostart replikera fysiska servrar som kör Windows eller Linux till Azure.

## <a name="prerequisites"></a>Krav

hello artikeln förutsätter att du redan har:
1. Recovery Services-valvet i hello [Azure-portalen](http://portal.azure.com "Azure-portalen").
3. En fysisk dator på vilken tooinstall hello konfigurationsservern.

### <a name="configuration-server-minimum-requirements"></a>Minimikrav för konfiguration av servern
hello i den följande tabellen listas hello minimikraven för maskinvara, programvara och nätverkskraven för en konfigurationsserver.
[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

> [!NOTE]
> HTTPS-baserade proxyservrar stöds inte av hello konfigurationsservern.

## <a name="choose-your-protection-goals"></a>Välja skyddsmål

1. I hello Azure-portalen, går toohello **återställningstjänster** valv bladet och välj ditt valv.
2. I hello **resurs** menyn hello valvet, klickar du på **komma igång** > **Site Recovery** > **steg 1: Förbered Infrastruktur** > **skyddsmål**.

    ![Välja mål](./media/site-recovery-set-up-physical-to-azure/choose-goals.png)
3. I **skyddsmål**väljer **tooAzure** och **inte virtualiserade/andra**, och klicka sedan på **OK**.

    ![Välja mål](./media/site-recovery-set-up-physical-to-azure/physical-protection-goal.PNG)

## <a name="set-up-hello-source-environment"></a>Ställ in hello källmiljön

1. I **Förbered källa**, om du inte har en konfigurationsservern och klicka på **+ konfigurationsservern** tooadd en.

  ![Konfigurera källan](./media/site-recovery-set-up-physical-to-azure/plus-config-srv.png)
2. I hello **Lägg till Server** bladet, kontrollera att **konfigurationsservern** visas i **servertyp**.
4. Hämta installationsfilen för hello Unified installationsprogram för Site Recovery.
5. Hämta hello valvregistreringsnyckel. Du behöver hello registreringsnyckel när du kör installationsprogrammet för enhetlig. hello nyckeln är giltig i fem dagar efter genereringen.

    ![Konfigurera källan](./media/site-recovery-set-up-physical-to-azure/set-source2.png)
6. Kör på hello-dator som du använder som hello konfigurationsservern **Unified installationsprogram för Azure Site Recovery** tooinstall hello konfigurationsservern, hello processervern och hello master mål-servern.

#### <a name="run-azure-site-recovery-unified-setup"></a>Kör Azure Site Recovery enhetlig installation

> [!TIP]
> Registrera för konfiguration av servern misslyckas om hello tiden på datorns systemklockan är mer än fem minuter från lokal tid. Synkronisera systemklockan med en [tidsserver](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) innan du påbörjar installationen hello.

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> hello konfigurationsservern kan installeras via kommandoraden. Mer information finns i [installera konfigurationsservern med hjälp av kommandoradsverktyg](http://aka.ms/installconfigsrv).


## <a name="common-issues"></a>Vanliga problem

[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a>Nästa steg

Nästa steg omfattar [konfigurera målmiljön](./site-recovery-prepare-target-physical-to-azure.md) i Azure.
