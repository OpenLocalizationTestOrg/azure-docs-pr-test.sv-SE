---
title: "aaaSet upp en replikeringsprincip för fysisk server replication tooAzure med Azure Site Recovery | Microsoft Docs"
description: "Sammanfattar hello stegen tooset upp en replikeringsprincip när replikera lokala fysiska servrar tooAzure lagring med hjälp av hello Azure Site Recovery-tjänsten"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d7874bd8-6626-4668-9ec9-dbd2d26f8f81
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: d6bdeeffc24ffc8eaba24311f7c76452edb65648
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-8-set-up-a-replication-policy-for-physical-server-replication-tooazure"></a>Steg 8: Skapa en replikeringsprincip för fysisk server replication tooAzure


Den här artikeln beskriver hur tooset upp en replikeringsprincip när du replikerar Windows-/ Linux fysiska servrar tooAzure med hello [Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.


Publicera kommentarer och frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="configure-a-policy"></a>Konfigurera en princip

1. Klicka på **Site Recovery-infrastruktur** > **replikeringsprinciper** > **+ replikeringsprincip**.
2. I **skapa replikeringsprincip**, ange ett principnamn.
3. I **Återställningspunktmål tröskelvärdet**, ange hello Återställningspunktmål gränsen. Det här värdet anger hur ofta data återställningspunkter skapas. En avisering genereras om kontinuerlig replikering överskrider den gränsen.
4. I **kvarhållningstid för återställningspunkten**, ange hur länge (i timmar) hello kvarhållningsperiod är för varje återställningspunkt. Replikerade virtuella datorer kan återställas tooany punkt i ett fönster. Replikerade toopremium lagring och 72 timmar för standardlagring in too24 timmar kvarhållning har stöd för datorer.
5. I **frekvens av programkonsekventa ögonblicksbilder**, ange hur ofta (i minuter) återställningspunkter som innehåller programkonsekventa ögonblicksbilder ska skapas. Klicka på **OK** toocreate hello princip.

    ![Replikeringsprincip](./media/physical-walkthrough-replication/gs-replication2.png)
8. När du skapar en ny princip associeras den automatiskt med hello konfigurationsservern. Som standard skapas automatiskt en matchande princip för återställning efter fel. Till exempel om hello replikeringsprincip är **rep princip** hello principer ska vara **rep-princip-återställning**. Den här principen används inte förrän du har initierat en återställning från Azure.

## <a name="next-steps"></a>Nästa steg

Gå för[steg 9: Installera hello mobilitetstjänsten](physical-walkthrough-install-mobility.md)
