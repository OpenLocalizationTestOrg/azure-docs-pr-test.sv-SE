---
title: "aaaTroubleshoot långsam säkerhetskopiering av filer och mappar i Azure Backup | Microsoft Docs"
description: "Innehåller felsökning vägledning toohelp du diagnostisera hello orsaken till Azure Backup prestandaproblem"
services: backup
documentationcenter: 
author: genlin
manager: cshepard
editor: 
ms.assetid: e379180a-db13-4e0c-90e4-28e5dd6f5b14
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: genli
ms.openlocfilehash: 21d79bbd03c2706bc43fcc7c14020cffd6b919c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-slow-backup-of-files-and-folders-in-azure-backup"></a>Felsökning av långsam säkerhetskopiering av filer och mappar i Azure Backup
Den här artikeln innehåller felsökningsinformation toohelp du diagnosticera hello orsaken till långsam prestanda vid säkerhetskopiering av filer och mappar när du använder Azure Backup. När du använder hello Azure Backup agent tooback filer kan hello säkerhetskopieringen ta längre tid än förväntat. Den här fördröjningen kan bero på en eller flera av följande hello:

* [Det finns prestandaflaskhalsar på hello-dator som säkerhetskopieras.](#cause1)
* [En annan process eller antivirusprogram kan störa hello Azure Backup-processen.](#cause2)
* [hello Backup-agenten körs på en Azure-dator (VM).](#cause3)  
* [Du säkerhetskopierar ett stort antal (miljoner) filer.](#cause4)

Innan du startar felsökning av problem, rekommenderar vi att du hämtar och installerar hello [senaste Azure Backup-agenten](http://aka.ms/azurebackup_agent). Vi göra uppdateras ofta toohello Backup-agenten toofix olika problem, lägga till funktioner och förbättra prestanda.

Vi också rekommenderar att du läser igenom hello [Azure Backup service FAQ](backup-azure-backup-faq.md) toomake säker på att du inte har någon hello vanliga problem med konfigurationen.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

<a id="cause1"></a>

## <a name="cause-performance-bottlenecks-on-hello-computer"></a>Orsak: Prestandaflaskhalsar på hello-dator
Flaskhalsar på hello-dator som säkerhetskopieras kan orsaka fördröjningar. Till exempel hello datorns möjlighet tooread eller skriva toodisk eller bandbredd toosend data hello nätverket, kan orsaka flaskhalsar.

Windows tillhandahåller ett inbyggda verktyg som kallas [Prestandaövervakaren](https://technet.microsoft.com/magazine/2008.08.pulse.aspx) (Perfmon) toodetect dessa flaskhalsar.

Här följer några prestandaräknare och intervall som kan vara användbar vid diagnos av flaskhalsar för optimala säkerhetskopieringar.

| Räknaren | Status |
| --- | --- |
| Logisk Disk (fysisk Disk)--% inaktiv |• 100% inaktiv too50% inaktiv = felfri</br>• 49% inaktiv too20% inaktiv = varnings- eller Övervakare</br>• 19% inaktiv too0% inaktiv = kritiskt eller Out-of-specifikationen |
| Logisk Disk (fysisk Disk)--% medel Disk sek läsning eller skrivning |• 0,001 ms too0.015 ms = felfri</br>• 0.015 ms too0.025 ms = varnings- eller Övervakare</br>• 0.026 ms eller längre = kritiskt eller Out-of-specifikationen |
| Logisk Disk (fysisk Disk)--Aktuell diskkölängd (för alla instanser) |80 begäranden i mer än 6 minuter |
| Minne – icke växlingsbart systemminne-byte |• Mindre än 60% av poolen förbrukas = felfri<br>• 61% too80% av poolen förbrukas = varnings- eller Övervakare</br>• Större än 80% poolen förbrukas = kritiskt eller Out-of-specifikationen |
| Minne – växlingsbart systemminne-byte |• Mindre än 60% av poolen förbrukas = felfri</br>• 61% too80% av poolen förbrukas = varnings- eller Övervakare</br>• Större än 80% poolen förbrukas = kritiskt eller Out-of-specifikationen |
| Minne – tillgängliga megabyte |• 50% av ledigt minne eller fler = felfri</br>• 25% av ledigt minne = Övervakare</br>• 10% av ledigt minne = varning</br>• Mindre än 100 MB eller 5% av ledigt minne = kritiskt eller Out-of-specifikationen |
| Processor –\%processortid (alla instanser) |• Mindre än 60% förbrukas = felfri</br>• 61% too90% förbrukas = övervakaren eller varning</br>• 91% too100% förbrukas = kritisk |

> [!NOTE]
> Om du finner att hello-infrastrukturen är hello sällan, rekommenderar vi att defragmenterar du hello diskar regelbundet för bättre prestanda.
>
>

<a id="cause2"></a>

## <a name="cause-another-process-or-antivirus-software-interfering-with-azure-backup"></a>Orsak: En annan process eller antivirusprogram stör Azure Backup
Vi har sett flera instanser där andra processer i hello Windows-system har försämrat prestanda för hello Azure Backup agent-processen. Om du använder både hello Azure Backup-agenten och ett annat program tooback data eller om antivirusprogrammet körs och har ett lås på filer toobe säkerhetskopieras hello kan flera lås på filer orsaka konkurrens. I den här situationen hello säkerhetskopiering misslyckas eller hello jobb kan ta längre tid än förväntat.

Hej bästa rekommendation i det här scenariot är tooturn av hello andra säkerhetskopieringsprogrammet toosee om hello säkerhetskopieringstiden för hello Azure Backup agent ändras. Vanligtvis att se till att flera säkerhetskopieringsjobb inte körs på hello samtidigt är tillräckligt tooprevent dem påverkar varandra.

För antivirusprogram, rekommenderar vi att du utesluter hello följande filer och platser:

* C:\Program Files\Microsoft Azure Recovery Services Agent\bin\cbengine.exe som en process
* C:\Program Files\Microsoft Azure Recovery Services Agent\ mappar
* Scratch platsen (om du inte använder standard hello-plats)

<a id="cause3"></a>

## <a name="cause-backup-agent-running-on-an-azure-virtual-machine"></a>Orsak: Backup-agenten körs på en virtuell Azure-dator
Om du kör hello Backup-agenten på en virtuell dator, blir prestanda långsammare än när du kör på en fysisk dator. Detta är förväntat på grund av tooIOPS begränsningar.  Du kan dock optimera hello prestanda genom att växla hello dataenheter som ska säkerhetskopieras tooAzure Premium-lagring. Vi arbetar på att åtgärda problemet och hello korrigering kommer att finnas i en framtida version.

<a id="cause4"></a>

## <a name="cause-backing-up-a-large-number-millions-of-files"></a>Orsak: Säkerhetskopiera ett stort antal (miljoner) filer
Flytta stora mängder data tar längre tid än att flytta en mindre mängd data. I vissa fall säkerhetskopieringstiden relaterade toonot hello endast storleken på hello data men hello antal filer eller mappar. Detta är särskilt viktigt om miljontals små filer (några byte tooa få kilobyte) ska säkerhetskopieras.

Detta beror på att när du arbetar säkerhetskopiera hello data och flytta den tooAzure kan Azure samtidigt katalogiserar dina filer. I vissa sällsynta fall kan hello katalog åtgärden ta längre tid än förväntat.

hello efter indikatorer kan hjälpa dig att förstå hello flaskhals och därefter fungerar på hello nästa steg:

* **Användargränssnittet visar förloppet för hello dataöverföring**. hello data överförs fortfarande. hello nätverksbandbredd eller hello storleken på data som orsakar fördröjningar.
* **Användargränssnittet visar inte förloppet för hello dataöverföring**. Öppna hello loggar finns i C:\Microsoft Azure Recovery Services Agent\Temp och sedan söka efter hello FileProvider::EndData post i hello loggar. Den här posten betyder att hello dataöverföring klar och hello katalog igen som händer. Inte avbryta hello säkerhetskopieringsjobb. I stället vänta en liten stund hello katalog åtgärden toofinish. Om hello problemet kvarstår kontaktar du [Azure-supporten](https://portal.azure.com/#create/Microsoft.Support).
