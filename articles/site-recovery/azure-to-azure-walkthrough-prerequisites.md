---
title: "Innan du börjar replikering av virtuella Azure-datorer till en annan region | Microsoft Docs"
description: "Sammanfattas de steg du måste utföra innan du replikerar virtuella Azure-datorer mellan Azure-regioner, med hjälp av Azure Site Recovery-tjänsten"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: dab98aa5-9c41-4475-b7dc-2e07ab1cfd18
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: raynew
ms.openlocfilehash: fc6682edc020796431324005e10ca4ff0c0fd2f0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="step-2-before-you-start"></a>Steg 2: Innan du börjar

När du har granskat den [arkitektur](azure-to-azure-walkthrough-architecture.md) för replikering av virtuella Azure-datorer (VM) mellan Azure-regioner med [Azure Site Recovery](site-recovery-overview.md), Använd den här artikeln för att verifiera krav. 

- Du bör ha en förståelse för vad behövs för att distributionen fungerar och har slutfört nödvändiga steg när du är klar artikeln.
- Skriv dina kommentarer eller frågor längst ned i den här artikeln eller i [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

>[!NOTE]
>
> Azure VM-replikering är för närvarande under förhandsgranskning.



## <a name="support-recommendations"></a>Rekommendationer

Granska i tabellen nedan.

**Komponent** | **Krav**
--- | ---
**Recovery Services-valvet** | Vi rekommenderar att du skapar ett Recovery Services-valv i mål-Azure-region som du vill använda för katastrofåterställning. Om du vill replikera virtuella källdatorer i östra USA till centrala USA kan du till exempel skapa valvet i centrala USA.
**Azure-prenumeration** | Din Azure-prenumeration måste vara aktiverat för att skapa virtuella datorer, i den målplats som du vill använda som disaster recovery-region. Kontakta supporten om du vill aktivera kvoten som krävs.
**Mål region kapacitet** | Prenumerationen ska ha tillräckligt med kapacitet för virtuella datorer, lagringskonton och nätverkskomponenter i målet Azure-region.
**Storage** | Använd den [lagringsanvisningar](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks) för källan virtuella Azure-datorer att undvika prestandaproblem.<br/><br/> Storage-konton måste vara i samma region som valvet.<br/><br/> Du kan inte replikeras till premium-konton i Central och södra Indien.<br/><br/> Om du distribuerar replikering med standardinställningarna skapar Site Recovery krävs storage-konton som baseras på källserverns konfiguration. Om du har ändrat inställningarna följer den [skalbarhetsmål för Virtuella diskar](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks).
**Nätverk** | Du måste tillåta utgående anslutning från virtuella Azure-datorer för specifika URL: er eller den IP-adressintervall.<br/><br/> Nätverkskonton måste vara i samma region som valvet. 
**Virtuell Azure-dator** | Kontrollera att alla de senaste rotcertifikat finns i Windows-/ Linux Azure VM. Om de inte kan du inte registrera den virtuella datorn i Site Recovery på grund av säkerhetsbegränsningar.
**Azure-konto** | Ditt Azure-konto måste ha vissa [behörigheter](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) att aktivera replikering för en virtuell Azure-dator.

Hämta en fullständig lista över kraven för stöd i den [supportmatrisen](site-recovery-support-matrix-azure-to-azure.md)


## <a name="set-permissions-on-the-account"></a>Ange behörigheter för kontot

1. Läs mer om den [behörigheter](site-recovery-role-based-linked-access-control.md) du behöver för replikering.
2. Följ dessa [instruktioner](../active-directory/role-based-access-control-configure.md#add-access) att lägga till behörigheter.


## <a name="verify-target-resources"></a>Kontrollera target-resurser

1. Aktivera din Azure-prenumeration så att du kan skapa virtuella datorer i mål-region som du vill använda för katastrofåterställning som du vill använda som disaster recovery-region. Kontakta supporten om du vill aktivera kvoten som krävs.
2. Kontrollera att din prenumeration har tillräckligt med resurser för att aktivera virtuella datorer med storlekar som matchar dina virtuella källdatorer. Som standard när ställer in replikering, hämtar Site Recovery samma storlek för mål VM eller den närmaste möjliga storleken. Lär dig mer om [felsökning](site-recovery-azure-to-azure-troubleshoot-errors.md#azure-resource-quota-issues-error-code-150097) mål resurser.

## <a name="verify-azure-vm-certificates"></a>Verifiera Azure VM-certifikat

1. Kontrollera att de senaste rotcertifikat finns på Windows- eller Linux virtuella datorer du vill replikera. Om de senaste rotcertifikat inte finns, kan inte den virtuella datorn registreras till Site Recovery på grund av säkerhetsbegränsningar.
2. För virtuella Windows-datorer, gör du följande:

    - Installera de senaste uppdateringarna på den virtuella datorn så att alla betrodda rotcertifikat som finns på datorn.
    - Följ Windows Update process-certifikatet uppdatering standardprocessen för i din organisation att hämta de senaste rotcertifikat i en frånkopplad miljö, och uppdatera CRL på virtuella datorer.
3. För Linux virtuella datorer, följer du de riktlinjer som tillhandahålls av Linux-distributör för att hämta de senaste betrodda rotcertifikat och senaste listan över återkallade certifikat på den virtuella datorn. Lär dig mer om [felsökning](site-recovery-azure-to-azure-troubleshoot-errors.md#trusted-root-certificates-error-code-151066) betrodda rot-problem.


## <a name="next-steps"></a>Nästa steg

Gå till [steg3: Planera nätverk](azure-to-azure-walkthrough-network.md) att ställa in utgående anslutning.
