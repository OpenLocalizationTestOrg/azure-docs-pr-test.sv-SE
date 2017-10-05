---
title: "Security Center-plattformen migrering vanliga frågor och svar | Microsoft Docs"
description: "Dessa vanliga frågor svar på frågor om Azure Security Center-plattformen migreringen."
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
ms.openlocfilehash: 2ffbaca614d667db565197f3c13b1658fffc2a7c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="security-center-platform-migration-faq"></a>Security Center-plattformen migrering vanliga frågor och svar
I tidig juni 2017 började Azure Security Center med hjälp av Microsoft Monitoring Agent att samla in och lagra data. Läs mer i [Azure Security Center-plattformen migrering](security-center-platform-migration.md). Dessa vanliga frågor svar på frågor om migreringen plattform.

## <a name="data-collection-agents-and-workspaces"></a>Insamling av data, agenter och arbetsytor

### <a name="how-is-data-collected"></a>Hur data samlas in?
Security Center använder Microsoft Monitoring Agent för att samla in säkerhetsdata från dina virtuella datorer. Säkerhetsdata innehåller information om säkerhetskonfigurationer som används för att identifiera säkerhetsproblem och säkerhetshändelser som används för att identifiera hot. Data som samlas in av agenten lagras i en befintlig logganalys-arbetsyta som är ansluten till den virtuella datorn eller en ny arbetsyta som skapats av Security Center. När Security Center skapar en ny arbetsyta, beaktas geolokalisering av den virtuella datorn.

> [!NOTE]
> Microsoft Monitoring Agent är samma agent används av den Operations Management Suite (OMS), Log Analytics-tjänsten och System Center Operations Manager (SCOM).
>
>

När datainsamling har aktiverats för första gången eller när dina prenumerationer har migrerats, kontrollerar Security Center om Microsoft Monitoring Agent är redan installerat som en Azure-tillägget på var och en av dina virtuella datorer. Om Microsoft Monitoring Agent inte är installerad, kommer Security Center:

- installera Microsoft Monitoring agent på den virtuella datorn
   - Om en arbetsyta som skapats av Security Center redan finns i samma geolokalisering som den virtuella datorn, är den kopplad till den här arbetsytan
   - Om en arbetsyta inte finns, Security Center skapar en ny resursgrupp standard arbetsytan i den geoplats och Anslut agenten till arbetsytan. Namngivningskonventionen för gruppen arbetsytan och resursen är:

       Arbetsytan: DefaultWorkspace-[prenumerations-ID]-[geo]

       Resursgrupp: DefaultResouceGroup-[geo]
- installera en lösning för Security Center på arbetsytan

Platsen för arbetsytan baseras på platsen för den virtuella datorn. Läs mer i [datasäkerhet](security-center-data-security.md).

> [!NOTE]
> Security Center samlas in säkerhetsdata från dina virtuella datorer med hjälp av Azure Monitoring Agent före plattform migreringen och data lagras i ditt lagringskonto. Efter migreringen plattform använder Security Center arbetsytan och Microsoft Monitoring Agent för att samla in och lagra samma data. Lagringskontot kan tas bort efter migreringen.
>
>

### <a name="am-i-billed-for-log-analytics-or-oms-on-the-workspaces-created-by-security-center"></a>Kan jag debiteras för Log Analytics eller OMS på de arbetsytor som skapats av Security Center?
Nej. Arbetsytor som skapats av Security Center, samtidigt som konfigurerats för OMS per nod fakturering inte avgifter OMS. Security Center fakturering är alltid baserat på din säkerhetsprincip Security Center och lösningar som installerats på en arbetsyta:

- **Kostnadsfri nivå** – Security Center installerar 'SecurityCenterFree' lösningen på standardarbetsytan. Du debiteras inte för den kostnadsfria nivån.
- **Standardnivån** – Security Center installerar de 'SecurityCenterFree' och '' säkerhetslösningar på standardarbetsytan.

Mer information om priser finns [Security Center priser](https://azure.microsoft.com/pricing/details/security-center/). Prissättningssidan adresser ändringar av säkerhet för datalagring och proportionellt fördelad fakturering med början i juni 2017.

> [!NOTE]
> OMS prisnivån arbetsytor som skapats av Security Center påverkar inte Security Center fakturering.
>
>

### <a name="can-i-delete-the-default-workspaces-created-by-security-center"></a>Kan jag ta bort Standardarbetsytor som skapats av Security Center?
**Det rekommenderas inte att ta bort standardarbetsytan.** Security Center använder Standardarbetsytor för att lagra säkerhetsdata från dina virtuella datorer.  Om du tar bort en arbetsyta Security Center kan inte samla in dessa data och vissa säkerhetsrekommendationer och aviseringar är inte tillgänglig

Ansluten till arbetsytan tagits bort för att återställa eller ta bort Microsoft Monitoring Agent på virtuella datorer. Security Center installerar om agenten och skapar nya Standardarbetsytor.

### <a name="what-if-the-microsoft-monitoring-agent-was-already-installed-as-an-extension-on-the-vm"></a>Vad händer om Microsoft Monitoring Agent har redan installerats som ett tillägg på den virtuella datorn?
Security Center åsidosätts inte befintliga anslutningar till användaren arbetsytor. Security Center lagrar säkerhetsdata från den virtuella datorn i arbetsytan redan ansluten.

### <a name="what-if-i-had-a-microsoft-monitoring-agent-installed-on-the-machine-but-not-as-an-extension"></a>Vad händer om jag hade en Microsoft Monitoring Agent installerad på datorn men inte som ett tillägg?
Om Microsoft Monitoring Agent är installerad direkt på VM (inte som en Azure-tillägget), Security Center kommer inte att installera Microsoft Monitoring Agent och säkerhetsövervakning begränsas.

### <a name="what-is-the-impact-of-removing-these-extensions"></a>Vad är effekt vid borttagning av dessa tillägg?
Om du tar bort tillägget för övervakning av Microsoft Security Center är inte kunna samla in säkerhetsdata från den virtuella datorn och vissa säkerhetsrekommendationer och aviseringar är inte tillgänglig. Security Center anger att den virtuella datorn saknar tillägget och installerar om tillägget inom 24 timmar.

### <a name="how-do-i-stop-the-automatic-agent-installation-and-workspace-creation"></a>Hur förhindrar att agenten för automatisk installation och arbetsytan skapa?
Du kan inaktivera datainsamling för dina prenumerationer i säkerhetsprincipen men detta rekommenderas inte. Stänga av databegränsningar samling Security Center-rekommendationerna och aviseringar. Insamling av data krävs för prenumerationer på standarden prisnivån. Inaktivera datainsamling:

1. Om din prenumeration har konfigurerats för standardnivån, öppna säkerhetsprincip för den prenumerationen och väljer den **lediga** nivå.

   ![Prisnivå][1]

2. Sedan stänga av datainsamlingen genom att välja **av** på den **säkerhetsprincip – datainsamling** bladet.

   ![Datainsamling][2]

### <a name="how-do-i-remove-oms-extensions-installed-by-security-center"></a>Hur tar jag bort OMS-tillägg installeras av Security Center?
Du kan manuellt ta bort Microsoft Monitoring Agent. Detta rekommenderas inte eftersom det begränsar Security Center rekommendationer och aviseringar.

> [!NOTE]
> Om datainsamling har aktiverats Security Center kommer installera om agenten när du tar bort den.  Du måste inaktivera datainsamling innan du tar bort agenten manuellt. Se [hur stoppa agenten för automatisk installation och arbetsyta skapas?](#how-do-i-stop-the-automatic-agent-installation-and-workspace-creation?) anvisningar om hur du inaktiverar datainsamling.
>
>

Ta bort agenten manuellt:

1.  Öppna i portalen, **logganalys**.
2.  Välj en arbetsyta i bladet logganalys:
3.  Markera varje virtuell dator som du inte vill övervaka och välj **frånkoppling**.

   ![Ta bort agenten][3]

> [!NOTE]
> Om en Linux VM redan har en icke-tillägget OMS-agent, tar bort tillägget tas bort agenten samt och kunden har installera det på nytt.
>
>

## <a name="existing-oms-customers"></a>Befintliga OMS-kunder

### <a name="does-security-center-override-any-existing-connections-between-vms-and-workspaces"></a>Security Center åsidosätta alla befintliga anslutningar mellan virtuella datorer och arbetsytor?
Om en virtuell dator har redan Microsoft Monitoring Agent installeras som en Azure-tillägget, åsidosätter inte den befintliga anslutningen i arbetsytan i Security Center. Security Center används i stället befintlig arbetsyta.

En lösning för Security Center är installerat på arbetsytan om det inte finns redan och lösningen tillämpas endast på de relevanta virtuella datorerna. När du lägger till en lösning distribueras den automatiskt som standard till alla Windows- och Linux-agenter ansluten till logganalys-arbetsytan. [Lösning målinriktning](../operations-management-suite/operations-management-suite-solution-targeting.md), vilket är en OMS-funktion kan du använda ett scope till dina lösningar.

Om Microsoft Monitoring Agent installeras direkt på den virtuella datorn (inte som en Azure-tillägget), Security Center kommer inte att installera Microsoft Monitoring Agent och säkerhetsövervakning är begränsad.

### <a name="what-should-i-do-if-i-suspect-that-the-data-platform-migration-broke-the-connection-between-one-of-my-vms-and-my-workspace"></a>Vad gör jag om jag misstänker att migrering av data platform bröt mot anslutningen mellan en av Mina virtuella datorer och Min arbetsyta?
Det borde inte ske. Om det sker, sedan [skapa en supportförfrågan för Azure](../azure-supportability/how-to-create-azure-support-request.md) och innehåller följande information:

- Azure-resurs-ID för påverkas VM
- Azure-resurs-ID för arbetsytan som konfigurerats på tillägget innan anslutningen har brutits
- Agenten och version som tidigare har installerats

### <a name="does-security-center-install-solutions-on-my-existing-oms-workspaces-what-are-the-billing-implications"></a>Installeras Security Center lösningar på Mina befintliga OMS-arbetsytor? Vad är fakturering effekterna?
När Security Center identifierar att en virtuell dator är redan ansluten till en arbetsyta som du skapade, kan Security Center lösningar på den här arbetsytan enligt prisnivå. Lösningarna som endast används för den relevanta virtuella Azure-datorer [lösning riktad](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-solution-targeting), så faktureringen är densamma.

- **Kostnadsfri nivå** – Security Center installerar 'SecurityCenterFree' lösningen på arbetsytan. Du debiteras inte för den kostnadsfria nivån.
- **Standardnivån** – Security Center installerar de 'SecurityCenterFree' och '' säkerhetslösningar på arbetsytan.

   ![Lösningar i standardarbetsytan][4]

> [!NOTE]
> ' Säkerhetslösning ' i logganalys är säkerhets- och granska lösningen i OMS.
>
>

### <a name="i-already-have-workspaces-in-my-environment-can-i-use-them-to-collect-security-data"></a>Jag har redan arbetsytor i Min miljö, kan jag använda dem för att samla in säkerhetsdata om?
Om en virtuell dator har redan Microsoft Monitoring Agent installeras som en Azure-tillägget, använder befintliga anslutna arbetsytan i Security Center. En lösning för Security Center är installerat på arbetsytan om det inte finns redan och lösningen tillämpas endast på de relevanta virtuella datorerna via [lösning riktad](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-solution-targeting).

När Security Center installerar Microsoft Monitoring Agent på virtuella datorer, använder standard-arbetsytor som skapats av Security Center. Kunder kommer snart att kunna konfigurera vilka arbetsytor används.

### <a name="i-already-have-security-solution-on-my-workspaces-what-are-the-billing-implications"></a>Jag har redan säkerhetslösning på Mina arbetsytor. Vad är fakturering effekterna?
Säkerhets- och granska lösningen används för att aktivera Security Center Standard nivå funktioner för virtuella Azure-datorer. Om säkerhets- och granska lösningen är redan installerad på en arbetsyta, använder den befintliga lösningen Security Center. Det finns ingen ändring i fakturering.

## <a name="next-steps"></a>Nästa steg
Mer information om Security Center-plattformen migrering finns

- [Azure Security Center-plattformen migrering](security-center-platform-migration.md)
- [Azure Security Center Troubleshooting Guide](security-center-troubleshooting-guide.md)

<!--Image references-->
[1]: ./media/security-center-platform-migration-faq/pricing-tier.png
[2]: ./media/security-center-platform-migration-faq/data-collection.png
[3]: ./media/security-center-platform-migration-faq/remove-the-agent.png
[4]: ./media/security-center-platform-migration-faq/solutions.png
