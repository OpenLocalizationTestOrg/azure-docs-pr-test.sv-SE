---
title: "aaaReview hello arkitektur för replikering av virtuella Azure-datorer mellan Azure-regioner | Microsoft Docs"
description: "Den här artikeln innehåller en översikt över komponenter och arkitektur som används när du replikerar virtuella Azure-datorer mellan Azure-regioner hello Azure Site Recovery-tjänsten."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/12/2017
ms.author: raynew
ms.openlocfilehash: 4caab4e7a764040f317201d1345c40c73f836d81
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-1-review-hello-architecture-for-azure-vm-replication-between-azure-regions"></a>Steg 1: Granska hello arkitektur för Azure VM replikering mellan Azure-regioner


När du har granskat hello [översikt över stegen](azure-to-azure-walkthrough-overview.md) läsa den här artikeln toounderstand hello komponenter och processer som används när replikering och återställa virtuella Azure-datorer (VM) från en Azure-region tooanother med för den här distributionen [Azure Site Recovery](site-recovery-overview.md).

- Du bör ha en förståelse av hur Azure VM replikering tooanother region fungerar när du är klar hello artikel.
- Skicka kommentarer längst ned hello i den här artikeln eller ställa frågor i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

>[!NOTE]
>Azure VM-replikering med hello Site Recovery-tjänsten är för närvarande under förhandsgranskning.



## <a name="architectural-components"></a>Arkitekturkomponenter

hello följande diagram ger en övergripande bild av en virtuell dator i Azure-miljö i en viss region (i det här exemplet hello östra USA plats). I en miljö med virtuella Azure-datorn:
- Appar kan köras på virtuella datorer med diskar som är fördelade på storage-konton.
- hello virtuella datorer kan ingå i en eller flera undernät i ett virtuellt nätverk.

![kund-miljö](./media/azure-to-azure-walkthrough-architecture/source-environment.png)

## <a name="replication-process"></a>Replikeringsprocessen

### <a name="step-1"></a>Steg 1

När du aktiverar Azure VM-replikering i hello Azure-portalen hello resurserna som visas i följande hello diagram och tabell skapas automatiskt i hello målregionen. Som standard skapas resurser baserat på inställningarna för datakälla region. Du kan anpassa hello Målinställningar efter behov. [Läs mer](site-recovery-replicate-azure-to-azure.md).

![Aktivera replikeringen steg 1](./media/azure-to-azure-walkthrough-architecture/enable-replication-step-1.png)

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

   ![Aktivera replikeringen steg 2](./media/azure-to-azure-walkthrough-architecture/enable-replication-step-2.png)

  
  Observera att Site Recovery aldrig måste inkommande anslutning toohello VM. Endast utgående anslutning tooSite återställningstjänsten URL: er/IP-adresser, autentisering för Office 365-URL: er/IP-adresser och IP-adresser för cache storage-konto krävs. 

## <a name="continuous-replication-process"></a>Kontinuerlig replikeringsprocess

När kontinuerlig replikering fungerar, överförs disk skrivningar är omedelbart toohello cache storage-konto. Site Recovery bearbetar hello data och skickar den toohello mål-lagringskontot. När hello data bearbetas skapas återställningspunkter i hello mål-lagringskontot med några minuters mellanrum.

## <a name="failover-process"></a>Failover-processen

När du påbörja en växling mål hello virtuella datorer skapas i hello målresursgruppen, virtuella målnätverket, mål-undernät och hello-tillgänglighetsuppsättning. Du kan använda valfri återställningspunkt under en redundansväxling.

![Failover-processen](./media/azure-to-azure-walkthrough-architecture/failover.png)

## <a name="next-steps"></a>Nästa steg

Gå för[steg 2: Verifiera förutsättningar och begränsningar](azure-to-azure-walkthrough-prerequisites.md)
