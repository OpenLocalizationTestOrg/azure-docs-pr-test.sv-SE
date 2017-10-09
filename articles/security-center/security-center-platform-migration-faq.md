---
title: "aaaSecurity Center-plattformen migrering vanliga frågor och svar | Microsoft Docs"
description: "Dessa vanliga frågor svar på frågor om hello Azure Security Center-plattformen migrering."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 4d1364cd-7847-425a-bb3a-722cb0779f78
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2017
ms.author: terrylan
ms.openlocfilehash: fcb14ae83167ef79a60371e4fcb625cf99bee6c9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="security-center-platform-migration-faq"></a>Security Center-plattformen migrering vanliga frågor och svar
I tidig juni 2017 började Azure Security Center med hjälp av hello Microsoft Monitoring Agent toocollect och lagra data. Det finns fler toolearn [Azure Security Center-plattformen migrering](security-center-platform-migration.md). Dessa vanliga frågor svar på frågor om hello plattform migrering.

## <a name="data-collection-agents-and-workspaces"></a>Insamling av data, agenter och arbetsytor

### <a name="how-is-data-collected"></a>Hur data samlas in?
Security Center använder Microsoft Monitoring Agent hello toocollect säkerhetsdata från dina virtuella datorer. hello säkerhetsdata innehåller information om säkerhetskonfigurationer som används tooidentify säkerhetsrisker, och säkerhetshändelser som används toodetect hot. Data som samlas in av hello agenten lagras i en befintlig logganalys arbetsytan anslutna toohello VM eller en ny arbetsyta som skapats av Security Center. När Security Center skapar en ny arbetsyta, hello geolokalisering av hello VM beaktas.

> [!NOTE]
> hello Microsoft Monitoring Agent är hello samma agent används av hello Operations Management Suite (OMS), Log Analytics-tjänsten och System Center Operations Manager (SCOM).
>
>

När datainsamling har aktiverats för hello första gången eller när dina prenumerationer har migrerats, kontrollerar Security Center toosee om hello Microsoft Monitoring Agent är redan installerad som en Azure-tillägget på var och en av dina virtuella datorer. Om inte hello Microsoft Monitoring Agent är installerad, kommer Security Center:

- installera hello Microsoft Monitoring agent på hello VM
   - Om en arbetsyta som skapats av Security Center redan finns i samma geolokalisering som hello VM, hello hello är agent anslutna toothis arbetsytan
   - Om en arbetsyta inte finns, Security Center skapar en ny resursgrupp standard arbetsytan i den geoplats och ansluta hello agent toothat arbetsytan. hello namngivningskonvention för hello arbetsytan och resursen är:

       Arbetsytan: DefaultWorkspace-[prenumerations-ID]-[geo]

       Resursgrupp: DefaultResouceGroup-[geo]
- installera en lösning för Security Center i hello-arbetsytan

hello platsen för hello arbetsytan baserat på hello plats hello VM. Det finns fler toolearn [datasäkerhet](security-center-data-security.md).

> [!NOTE]
> Tidigare tooplatform migrering Security Center samlas in säkerhetsdata från dina virtuella datorer med hjälp av hello Azure Monitoring Agent och data lagras i ditt lagringskonto. Efter migreringen av hello plattform Security Center använder hello Microsoft Monitoring Agent och arbetsytan toocollect och lagra hello samma data. Hej lagringskontot kan tas bort efter hello migreringen.
>
>

### <a name="am-i-billed-for-log-analytics-or-oms-on-hello-workspaces-created-by-security-center"></a>Kan jag debiteras för Log Analytics eller OMS på hello arbetsytor som skapats av Security Center?
Nej. Arbetsytor som skapats av Security Center, samtidigt som konfigurerats för OMS per nod fakturering inte avgifter OMS. Security Center fakturering är alltid baserat på din Security Center policy och hello säkerhetslösningar installerad på en arbetsyta:

- **Kostnadsfri nivå** – Security Center installerar hello 'SecurityCenterFree' lösning på hello standardarbetsytan. Du debiteras inte för hello kostnadsfria nivån.
- **Standardnivån** – Security Center installerar hello 'SecurityCenterFree' och ' säkerhetslösningar ' på hello standardarbetsytan.

Mer information om priser finns [Security Center priser](https://azure.microsoft.com/pricing/details/security-center/). hello priser sidan adresser ändras toosecurity datalagring och proportionellt fördelad fakturering med början i juni 2017.

> [!NOTE]
> hello OMS prisnivån arbetsytor som skapats av Security Center påverkar inte Security Center fakturering.
>
>

### <a name="can-i-delete-hello-default-workspaces-created-by-security-center"></a>Kan jag ta bort hello Standardarbetsytor som skapats av Security Center?
**Det rekommenderas inte att ta bort hello standardarbetsytan.** Security Center använder hello standard arbetsytor toostore säkerhetsdata från dina virtuella datorer.  Om du tar bort en arbetsyta Security Center är toocollect dessa data och vissa säkerhetsrekommendationer och aviseringar är inte tillgänglig

toorecover, ta bort hello Microsoft Monitoring Agent på hello VMs anslutna toohello bort arbetsytan. Security Center återinstalleras hello agent och skapar nya Standardarbetsytor.

### <a name="what-if-hello-microsoft-monitoring-agent-was-already-installed-as-an-extension-on-hello-vm"></a>Vad händer om hello Microsoft Monitoring Agent har installerats som ett tillägg på hello VM?
Security Center åsidosätts inte befintliga anslutningar toouser arbetsytor. Security Center lagrar säkerhetsdata från hello VM hello arbetsytan är redan ansluten.

### <a name="what-if-i-had-a-microsoft-monitoring-agent-installed-on-hello-machine-but-not-as-an-extension"></a>Vad händer om jag hade en Microsoft Monitoring Agent installerad på datorn hello men inte som ett tillägg?
Om hello Microsoft Monitoring Agent är installerad direkt på hello VM (inte som en Azure-tillägget), Security Center kan inte installeras hello Microsoft Monitoring Agent och säkerhetsövervakning begränsas.

### <a name="what-is-hello-impact-of-removing-these-extensions"></a>Vad är hello effekt vid borttagning av dessa tillägg?
Om du tar bort hello tillägg för övervakning av Microsoft Security Center är inte kan toocollect säkerhetsdata från hello VM och vissa säkerhetsrekommendationer och säkerhetsaviseringar är inte tillgängliga. Inom 24 timmar fastställer Security Center att hello VM saknas hello-tillägget och installerar om hello tillägg.

### <a name="how-do-i-stop-hello-automatic-agent-installation-and-workspace-creation"></a>Hur jag så att hello agenten för automatisk installation och skapa arbetsyta?
Du kan inaktivera datainsamling för dina prenumerationer i hello säkerhetsprincipen men detta rekommenderas inte. Stänga av databegränsningar samling Security Center-rekommendationerna och aviseringar. Insamling av data krävs för prenumerationer på hello Standard prisnivå. toodisable datainsamling:

1. Om din prenumeration har konfigurerats för hello standardnivån, öppna hello säkerhetsprinciper för den prenumerationen och välj hello **lediga** nivå.

   ![Prisnivå][1]

2. Sedan stänga av datainsamlingen genom att välja **av** på hello **säkerhetsprincip – datainsamling** bladet.

   ![Datainsamling][2]

### <a name="how-do-i-remove-oms-extensions-installed-by-security-center"></a>Hur tar jag bort OMS-tillägg installeras av Security Center?
Du kan manuellt ta bort hello Microsoft Monitoring Agent. Detta rekommenderas inte eftersom det begränsar Security Center rekommendationer och aviseringar.

> [!NOTE]
> Om datainsamling har aktiverats installeras hello agent Security Center när du tar bort den.  Du måste toodisable datainsamling innan du tar bort hello-agenten manuellt. Se [hur stoppa hello agenten för automatisk installation skapande av arbetsyta?](#how-do-i-stop-the-automatic-agent-installation-and-workspace-creation?) anvisningar om hur du inaktiverar datainsamling.
>
>

toomanually tar du bort hello agent:

1.  Öppna i hello portal **logganalys**.
2.  Välj en arbetsyta på hello logganalys blad:
3.  Markera varje virtuell dator som du inte vill använda toomonitor och välj **frånkoppling**.

   ![Ta bort hello-agent][3]

> [!NOTE]
> Om en Linux VM redan har en icke-tillägget OMS-agent, tar bort hello tillägget tas bort hello-agenten och hello kunden har tooreinstall den.
>
>

## <a name="existing-oms-customers"></a>Befintliga OMS-kunder

### <a name="does-security-center-override-any-existing-connections-between-vms-and-workspaces"></a>Security Center åsidosätta alla befintliga anslutningar mellan virtuella datorer och arbetsytor?
Om en virtuell dator har redan hello Microsoft Monitoring Agent som installeras som en Azure-tillägget, åsidosätter inte hello befintlig arbetsyta anslutning i Security Center. Security Center används i stället hello befintlig arbetsyta.

En lösning för Security Center är installerat på hello arbetsytan om det inte finns redan och hello lösningen är tillämpade endast toohello relevanta virtuella datorer. När du lägger till en lösning distribueras den automatiskt med standard tooall Windows och Linux-agenter anslutna tooyour logganalys-arbetsytan. [Lösning målinriktning](../operations-management-suite/operations-management-suite-solution-targeting.md), vilket är en OMS-funktion kan du tooapply ett scope tooyour lösningar.

Om hello Microsoft Monitoring Agent är installerad direkt på hello VM (inte som en Azure-tillägget), Security Center kan inte installeras hello Microsoft Monitoring Agent och säkerhetsövervakning är begränsad.

### <a name="what-should-i-do-if-i-suspect-that-hello-data-platform-migration-broke-hello-connection-between-one-of-my-vms-and-my-workspace"></a>Vad gör jag om jag misstänker att hello datamigrering plattform bröt mot hello anslutning mellan en av Mina virtuella datorer och Min arbetsyta?
Det borde inte ske. Om det sker, sedan [skapa en supportförfrågan för Azure](../azure-supportability/how-to-create-azure-support-request.md) och inkludera hello följande information:

- hello Azure resurs-ID för hello påverkas VM
- hello Azure resurs-ID för hello arbetsytan som konfigurerats på hello tillägget innan hello anslutningen bröts
- hello agent och version som tidigare har installerats

### <a name="does-security-center-install-solutions-on-my-existing-oms-workspaces-what-are-hello-billing-implications"></a>Installeras Security Center lösningar på Mina befintliga OMS-arbetsytor? Vad är hello fakturering?
När Security Center identifierar att en virtuell dator är redan anslutet tooa arbetsytans, Security Center kan lösningar på den här arbetsytan enligt tooyour prisnivån. Hej lösningar är tillämpade endast toohello relevanta virtuella Azure-datorer, via [lösning riktad](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-solution-targeting), så hello fakturering förblir hello samma.

- **Kostnadsfri nivå** – Security Center installerar hello 'SecurityCenterFree' lösning på hello arbetsytan. Du debiteras inte för hello kostnadsfria nivån.
- **Standardnivån** – Security Center installerar hello 'SecurityCenterFree' och ' säkerhetslösningar ' på hello arbetsytan.

   ![Lösningar i standardarbetsytan][4]

> [!NOTE]
> hello-säkerhetslösning ' i logganalys är hello säkerhet & Audit lösning i OMS.
>
>

### <a name="i-already-have-workspaces-in-my-environment-can-i-use-them-toocollect-security-data"></a>Jag har redan arbetsytor i Min miljö kan jag använda dem toocollect säkerhetsdata?
Om en virtuell dator har redan hello Microsoft Monitoring Agent som installeras som en Azure-tillägget, använder hello befintlig anslutna arbetsyta i Security Center. En lösning för Security Center är installerat på hello arbetsytan om det inte finns redan och hello lösningen är tillämpade endast toohello relevanta virtuella datorer via [lösning riktad](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-solution-targeting).

När Security Center installeras hello Microsoft Monitoring Agent på virtuella datorer använder hello standard arbetsytor som skapats av Security Center. Kunder kommer snart att kunna tooconfigure vilka arbetsytor används.

### <a name="i-already-have-security-solution-on-my-workspaces-what-are-hello-billing-implications"></a>Jag har redan säkerhetslösning på Mina arbetsytor. Vad är hello fakturering?
hello säkerhets- och granska lösningen är används tooenable Security Center Standard nivå funktioner för virtuella Azure-datorer. Om hello säkerhets- och granska lösningen är redan installerad på en arbetsyta, använder Security Center hello befintlig lösning. Det finns ingen ändring i fakturering.

## <a name="next-steps"></a>Nästa steg
toolearn mer om hello Security Center-plattformen migrering, se

- [Azure Security Center-plattformen migrering](security-center-platform-migration.md)
- [Azure Security Center Troubleshooting Guide](security-center-troubleshooting-guide.md)

<!--Image references-->
[1]: ./media/security-center-platform-migration-faq/pricing-tier.png
[2]: ./media/security-center-platform-migration-faq/data-collection.png
[3]: ./media/security-center-platform-migration-faq/remove-the-agent.png
[4]: ./media/security-center-platform-migration-faq/solutions.png
