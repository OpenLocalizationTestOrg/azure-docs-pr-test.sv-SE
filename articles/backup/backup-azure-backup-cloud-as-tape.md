---
title: aaaUse Azure Backup tooreplace infrastrukturen band | Microsoft Docs
description: "Lär dig hur ger Azure Backup band-liknande semantik som gör att du toobackup och återställa data i Azure"
services: backup
documentationcenter: 
author: trinadhk
manager: vijayts
editor: 
ms.assetid: 2e1bb67d-986c-4437-8056-3a63169b4214
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 1/10/2017
ms.author: saurse;trinadhk;markgal
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4c5b095d95d39267c54b1eed9427bda09658bb94
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-your-long-term-storage-from-tape-toohello-azure-cloud"></a>Flytta dina långsiktig lagring från band toohello Azure-molnet
Azure Backup och System Center Data Protection Manager-kunder kan:

* Säkerhetskopiera data i scheman som bäst passar hello organisationens behov.
* Behåll hello säkerhetskopierade informationen under längre perioder
* Se Azure som en del av deras långsiktig kvarhållning måste (istället för band).

Den här artikeln förklarar hur kunder kan aktivera principer för säkerhetskopiering och kvarhållning. Kunder som använder band tooaddress lång-sikt-samtidigt måste nu har ett kraftfullt och genomförbart alternativ med hello tillgänglighet för den här funktionen. hello funktionen är aktiverad i hello senaste versionen av hello Azure Backup (som är tillgänglig [här](http://aka.ms/azurebackup_agent)). System Center DPM-kunder måste uppdatera till, åtminstone, DPM 2012 R2 UR5 innan du använder DPM med hello Azure Backup-tjänsten.

## <a name="what-is-hello-backup-schedule"></a>Vad är hello schema för säkerhetskopiering?
schemat för säkerhetskopiering av hello anger hello frekvens av hello säkerhetskopieringsåtgärd. Till exempel ange hello inställningarna i följande skärmbild hello säkerhetskopieringar tas dagligen vid 18: 00 och vid midnatt.

![Dagsschema](./media/backup-azure-backup-cloud-as-tape/dailybackupschedule.png)

Kunder kan även schemalägga en säkerhetskopiering varje vecka. Till exempel indikera hello inställningarna i följande skärmbild hello att säkerhetskopieringar vidtas för varje alternativ söndag & onsdag på 9:30 och 1:00:00.

![Veckoschema](./media/backup-azure-backup-cloud-as-tape/weeklybackupschedule.png)

## <a name="what-is-hello-retention-policy"></a>Vad är hello bevarandeprincip?
hello bevarandeprincip anger hello varaktighet som hello säkerhetskopiering måste lagras. Kunder kan i stället för att bara ange en ”platt policy” för alla återställningspunkter för säkerhetskopiering, ange olika bevarandeprinciper baserat på när hello säkerhetskopia görs. Till exempel bevaras hello säkerhetskopieringspunkt tas dagligen, som fungerar som en operativa återställningspunkt i 90 dagar. Hej säkerhetskopieringspunkt vidtas hello slutet av varje kvartal granskningssyfte bevaras under en längre period.

![Bevarandeprincip](./media/backup-azure-backup-cloud-as-tape/retentionpolicy.png)

hello Totalt antal ”bevarandepunkter” anges i den här principen är 90 (dagliga poäng) + 40 (en varje kvartal för 10 år) = 130.

## <a name="example--putting-both-together"></a>Exempel – kombinera båda
![Exempel skärmen](./media/backup-azure-backup-cloud-as-tape/samplescreen.png)

1. **Dagliga bevarandeprincip**: säkerhetskopieringar tas dagligen sparas i sju dagar.
2. **Varje vecka bevarandeprincip**: säkerhetskopior som gjorts varje dag vid midnatt och 18: 00 lördag bevaras i fyra veckor
3. **Månatliga bevarandeprincip**: bevaras säkerhetskopior som har gjorts vid midnatt och 18: 00 på hello senaste lördag i varje månad i 12 månader
4. **Årlig bevarandeprincip**: säkerhetskopior som har gjorts vid midnatt på hello senaste lördag varje mars bevaras för 10 år

Totalt antal ”bevarandepunkter” hello (poäng som en kund kan återställa data) i hello föregående diagram beräknas enligt följande:

* två hanteringsplatser per dag för sju dagar = 14 återställningspunkter
* två hanteringsplatser per vecka för fyra veckor = 8 återställningspunkter
* två hanteringsplatser per månad i 12 månader = 24 återställningspunkter
* en punkt per år och 10 år = 10 recovery pekar

hello Totalt antal återställningspunkter är 56.

> [!NOTE]
> Azure-säkerhetskopiering har inte en begränsning på antalet återställningspunkter.
>
>

## <a name="advanced-configuration"></a>Avancerad konfiguration
Genom att klicka på **ändra** i hello föregående skärm, kunder har större flexibilitet att ange scheman.

![Ändra](./media/backup-azure-backup-cloud-as-tape/modify.png)

## <a name="next-steps"></a>Nästa steg
Mer information om Azure Backup finns:

* [Introduktion tooAzure säkerhetskopiering](backup-introduction-to-azure-backup.md)
* [Prova Azure-säkerhetskopiering](backup-try-azure-backup-in-10-mins.md)
