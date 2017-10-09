---
title: aaaAzure Security Center-plattformen migrering | Microsoft Docs
description: "Det här dokumentet beskriver vissa ändringar toohello sätt Azure Security Center-data som samlas in."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 80246b00-bdb8-4bbc-af54-06b7d12acf58
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2017
ms.author: yurid
ms.openlocfilehash: 28cb8d85912a3f62941cf113da51070081b5eda2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-center-platform-migration"></a>Migrering av Azure Security Center-plattformen

Från och med tidig juni 2017 samlar Azure Security Center viktiga ändringar toohello sätt säkerhetsdata som samlas in och lagras.  Ändringarna låsa upp nya funktioner som hello möjlighet tooeasily Sök säkerhetsdata och bättre överensstämmer med andra Azure-hantering och övervakning av tjänster.

> [!NOTE]
> hello plattform migrering bör inte påverka dina produktionsresurser och ingen åtgärd krävs från din sida.


## <a name="whats-happening-during-this-platform-migration"></a>Vad händer under den här plattformsmigreringen?

Tidigare använda Security Center hello Azure Monitoring Agent toocollect säkerhetsdata från dina virtuella datorer. Detta omfattar information om säkerhetskonfigurationer som används tooidentify säkerhetsrisker, och säkerhetshändelser som används toodetect hot. Dessa data lagrades i dina lagringskonton i Azure.

Framöver, Security Center använder hello Microsoft Monitoring Agent – är detta hello samma agent används av hello Operations Management Suite och Log Analytics-tjänsten. Data som samlas in från den här agenten lagras i antingen en befintlig *logganalys* [arbetsytan](../log-analytics/log-analytics-manage-access.md) som är associerade med din Azure-prenumeration eller en ny arbetsytor med hänsyn till kontot hello geolokalisering av hello VM .

## <a name="agent"></a>Agent

Som en del av hello övergång, hello Microsoft Monitoring Agent (för [Windows](../log-analytics/log-analytics-windows-agents.md) eller [Linux](../log-analytics/log-analytics-linux-agents.md)) är installerad på alla virtuella datorer i Azure som data samlas för närvarande.  Om hello VM har redan hello Microsoft Monitoring Agent installerad, Security Center utnyttjar hello aktuella installerad agent.

För en viss tidsperiod (vanligtvis några dagar) körs både agenter sida vid sida tooensure en smidig övergång utan att förlora data. På så sätt kan Microsoft toovalidate som hello nya data pipeline fungerar innan avslutade användning av hello aktuella pipeline. En gång verifierade hello Azure Monitoring Agent tas bort från dina virtuella datorer. Ingen åtgärd krävs från din sida. Du får ett e-postmeddelande när alla kunder har migrerats.
 
Det rekommenderas inte att manuellt avinstallera hello Azure Monitoring Agent under migreringen hello som luckor i säkerhetsdata kan orsaka. Kontakta [Microsofts kundservice och support](https://support.microsoft.com/contactus/) om du behöver mer hjälp. 

hello Microsoft Monitoring Agent för Windows kräver används TCP-port 443, läsa [Azure Security Center Troubleshooting Guide](security-center-troubleshooting-guide.md) för mer information.


> [!NOTE] 
> Eftersom hello Microsoft Monitoring Agent kan användas av andra Azure-hantering och övervakning services hello-agenten avinstalleras inte automatiskt när du stänga av insamling av data i Security Center. Du kan dock manuellt avinstallera hello agenten om det behövs.

## <a name="workspace"></a>Arbetsyta

Som tidigare beskrivits kan data som samlas in från hello Microsoft Monitoring Agent (uppdrag Security Center) lagras i en befintlig logganalys arbetsytor som är associerade med din Azure-prenumeration eller en ny arbetsytor med hänsyn till kontot hello geoplats av hello VM.

Du kan bläddra toosee en lista över dina logganalys arbetsytor, inklusive alla som skapats av Security Center i hello Azure-portalen. En relaterad resursgrupp skapas för nya arbetsytor. Både har följande namnkonvention:

- Arbetsyta: *DefaultWorkspace-[prenumerations-id]-[geo]*
- Resursgrupp: *DefaultResourceGroup-[geo]* 
 
För arbetsytor som skapats av Security Center sparas data i 30 dagar. För befintliga arbetsytor baseras kvarhållning på hello arbetsytan prisnivån.

> [!NOTE]
> Data som tidigare samlats in av Security Center ligger kvar i dina lagringskonton. När hello migreringen är klar, kan du ta bort dessa lagringskonton.

### <a name="oms-security-solution"></a>OMS-säkerhetslösning 

För befintliga kunder som inte har OMS-säkerhetslösningen installerad så installerar Microsoft den på arbetsytan, men endast med virtuella Azure-datorer som mål. Avinstallera inte den här lösningen, eftersom det inte finns något automatiskt sätt att åtgärda detta om du gör det från OMS-hanteringskonsolen.


## <a name="other-updates"></a>Övriga uppdateringar

Tillsammans med hello plattform migrering vi lansera vissa ytterligare mindre uppdateringar:

- Fler OS-versioner stöds. Visa hello lista [här](security-center-faq.md#virtual-machines).
- hello lista över OS säkerhetsproblem ska expanderas. Visa hello lista [här](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).
- [Prissättningen](https://azure.microsoft.com/pricing/details/security-center/) kommer att räknas om per timme (tidigare var det per dag), vilket leder till kostnadsbesparingar för vissa kunder.
- Insamling av data ska krävs och aktiveras automatiskt för kunder på hello Standard prisnivån.
- Azure Security Center kommer att börja identifiera lösningar mot skadlig kod som inte har distribuerats via Azure-tillägg. Identifiering av Symantec Endpoint Protection och Defender för Windows 2016 blir tillgänglig först.
- Principer för att förebygga och meddelanden kan endast konfigureras på hello *prenumeration* nivå, men priser kan fortfarande anges på hello *resursgruppen* nivå

