---
title: "aaaHow stöder virtuella Azure-datorreplikering mellan Azure-regioner arbete i Azure Site Recovery?  | Microsoft Docs"
description: "Den här artikeln innehåller en översikt över komponenter och arkitektur som används när du replikerar virtuella Azure-datorer mellan Azure-regioner med hjälp av hello Azure Site Recovery-tjänsten."
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/29/2017
ms.author: sujayt
ms.openlocfilehash: 01eda83e490821f8afc8a2973c18a19453a2e84a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-does-azure-vm-replication-work-in-site-recovery"></a>Hur fungerar Azure VM-replikering i Site Recovery?


Den här artikeln beskriver hello komponenter och processer som är involverad i replikera och återställa virtuella Azure-datorer (VM) från en region tooanother med hjälp av hello [Azure Site Recovery](site-recovery-overview.md) service.

>[!NOTE]
>Azure VM-replikering med hello Site Recovery-tjänsten är för närvarande under förhandsgranskning.

Skicka kommentarer längst ned hello i den här artikeln eller ställa frågor i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="architectural-components"></a>Arkitekturkomponenter

hello följande diagram ger en övergripande bild av en virtuell dator i Azure-miljö i en viss region (i det här exemplet hello östra USA plats). I en miljö med virtuella Azure-datorn:
- Appar kan köras på virtuella datorer med diskar som är fördelade på storage-konton.
- hello virtuella datorer kan ingå i en eller flera undernät i ett virtuellt nätverk.

![kund-miljö](./media/site-recovery-azure-to-azure-architecture/source-environment.png)

Lär dig mer om kraven för distribution av hello och kraven i hello [supportmatrisen](site-recovery-support-matrix-azure-to-azure.md).

## <a name="replication-process"></a>Replikeringsprocessen

### <a name="step-1"></a>Steg 1

När du aktiverar Azure VM-replikering i hello Azure-portalen hello resurserna som visas i följande hello diagram och tabell skapas automatiskt i hello målregionen. Som standard skapas resurser baserat på inställningarna för datakälla region. Du kan anpassa hello Målinställningar efter behov. [Läs mer](site-recovery-replicate-azure-to-azure.md).

![Aktivera replikeringen steg 1](./media/site-recovery-azure-to-azure-architecture/enable-replication-step-1.png)

**Resurs** | **Detaljer**
--- | ---
**Målresursgruppen** | Hej resurs grupp toowhich replikerade virtuella datorer som tillhör efter växling vid fel.
**Mål virtuellt nätverk** | hello virtuellt nätverk där replikerade virtuella datorer finns efter växling vid fel. En nätverksmappning skapas mellan käll- och virtuella nätverk och vice versa.
**Cache-lagringskonton** | Innan ändringarna på källan är replikerats toohello mål-lagringskontot, de spåras och skickas toohello cache storage-konto i hello målplats. Detta säkerställer minimal inverkan på produktion appar som körs på hello VM.
**Mål-lagringskonton**  | Storage-konton i hello mål plats toowhich hello data replikeras.
**Mål-tillgänglighetsuppsättningar**  | Tillgänglighetsuppsättningar i vilka hello replikerade virtuella datorer är placerade efter växling vid fel.

### <a name="step-2"></a>Steg 2

Eftersom replikering är aktiverat, installeras automatiskt hello Site Recovery-tillägget mobilitetstjänsten på hello VM. hello följande inträffar:

1. hello VM har registrerats med Site Recovery.

2. Kontinuerlig replikering har konfigurerats för hello VM. Data skrivs på hello Virtuella diskar som är kontinuerligt överförs toohello cache storage-konto i hello källplats.

   ![Aktivera replikeringen steg 2](./media/site-recovery-azure-to-azure-architecture/enable-replication-step-2.png)

   >[!IMPORTANT]
   > Site Recovery behöver aldrig inkommande anslutning toohello VM. hello VM behöver endast utgående anslutning tooSite återställning URL: er/IP-adresser, Office 365-autentisering URL: er/IP-adresser och IP-adresser för cache storage-konto. Mer information finns i hello [nätverk vägledning för att replikera virtuella datorer i Azure](site-recovery-azure-to-azure-networking-guidance.md) artikel.

## <a name="continuous-replication-process"></a>Kontinuerlig replikeringsprocess

När kontinuerlig replikering fungerar, överförs disk skrivningar är omedelbart toohello cache storage-konto. Site Recovery bearbetar hello data och skickar den toohello mål-lagringskontot. När hello data bearbetas skapas återställningspunkter i hello mål-lagringskontot med några minuters mellanrum.

## <a name="failover-process"></a>Failover-processen

När du påbörja en växling mål hello virtuella datorer skapas i hello målresursgruppen, virtuella målnätverket, mål-undernät och hello-tillgänglighetsuppsättning. Du kan använda valfri återställningspunkt under en redundansväxling.

![Failover-processen](./media/site-recovery-azure-to-azure-architecture/failover.png)

## <a name="next-steps"></a>Nästa steg

- Lär dig mer om [nätverk](site-recovery-azure-to-azure-networking-guidance.md) för Azure VM-replikering.
- Följ en genomgång för[replikera virtuella datorer i Azure.](site-recovery-azure-to-azure.md)
