---
title: aaaTroubleshoot Azure Automation Hybrid Runbook Worker | Microsoft Docs
description: "Beskriv hello symptom, orsaker och lösningar för hello vanligaste Hybrid Runbook Worker problem i Azure Automation."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 02c6606e-8924-4328-a196-45630c2255e9
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/25/2017
ms.author: magoedte
ms.openlocfilehash: 292af517c88d1ffd21262a286ccc079e7270bfad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-tips-for-hybrid-runbook-worker"></a>Felsökningstips för Hybrid Runbook Worker

Den här artikeln innehåller hjälp med att felsöka fel som du kan uppleva med Automation Hybrid Runbook Worker och ger förslag på lösningar tooresolve dem.

## <a name="a-runbook-job-terminates-with-a-status-of-suspended"></a>En runbook-jobbet avslutas med statusen Pausad

Din runbook har pausats strax efter försöker tooexecute den tre gånger. Det finns villkor som kan avbryta hello runbook slutförs och relaterade fel hälsningsmeddelande innehåller inte någon ytterligare information som anger varför. Den här artikeln innehåller felsökningssteg för problem relaterade toohello Runbook Worker-Hybrid runbook körning.

Om din Azure problemet inte åtgärdas i den här artikeln, gå hello Azure-forum på [MSDN och hello Stack Overflow](https://azure.microsoft.com/support/forums/). Du kan publicera problemet på dessa forum eller för[ @AzureSupport på Twitter](https://twitter.com/AzureSupport). Du kan också filen en Azure-supportbegäran genom att välja **få support** på hello [Azure-supporten](https://azure.microsoft.com/support/options/) plats.

### <a name="symptom"></a>Symtom
Runbook-körningen misslyckas och hello-felet som returnerades är ”hello Jobbåtgärden 'Aktivera' inte kan köras, eftersom hello processen oväntat stoppades. hello Jobbåtgärden gjordes 3 gånger ”.

Det finns flera möjliga orsaker till hello-fel: 

1. hello hybrid worker finns bakom en proxyserver eller brandvägg
2. hello datorn hello hybrid worker körs på har mindre än minsta hello [maskinvarukrav](automation-offering-get-started.md#hybrid-runbook-worker)  
3. Hej runbooks autentisera inte med lokala resurser

#### <a name="cause-1-hybrid-runbook-worker-is-behind-proxy-or-firewall"></a>Orsak 1: Runbook Worker-Hybrid är bakom en proxyserver eller brandvägg
hello datorn hello Hybrid Runbook Worker körs på finns bakom en brandvägg eller proxyserver server och utgående nätverksåtkomst kan inte beviljas eller konfigurerats korrekt.

#### <a name="solution"></a>Lösning
Kontrollera hello datorn utgående åtkomst too*.azure-automation.net på port 443. 

#### <a name="cause-2-computer-has-less-than-minimum-hardware-requirements"></a>Orsak 2: Datorn har mindre än minimikraven för maskinvara
Datorer som kör hello Hybrid Runbook Worker måste uppfylla hello maskinvarukrav innan du utser den toohost den här funktionen. Annars kommer att bli överbelastad hello datorn beroende på hello resursutnyttjande andra bakgrundsprocesser och konkurrens på grund av runbooks under körning och orsaka fördröjningar för runbook-jobb eller timeout. 

#### <a name="solution"></a>Lösning
Kontrollera först hello dator toorun hello Hybrid Runbook Worker funktionen uppfyller hello maskinvarukrav.  Om den finns, övervaka CPU och minne användning toodetermine alla korrelation mellan hello prestanda för Hybrid Runbook Worker-processer och Windows.  Om det finns minne eller CPU-belastning, detta kan indikera hello måste tooupgrade eller lägga till fler processorer eller öka minne tooaddress hello resurs flaskhals och lösa hello-fel. Du kan också markera en annan beräkningsresurser som stöder hello minimikrav och skala när arbetsbelastning indikera att öka krävs.         

#### <a name="cause-3-runbooks-cannot-authenticate-with-local-resources"></a>Orsak 3: Runbooks inte autentisera med lokala resurser

#### <a name="solution"></a>Lösning
Kontrollera hello **Microsoft SMA** händelseloggen för motsvarande händelse med beskrivning *Win32 processen avslutades med koden [4294967295]*.  hello orsaken till felet är du inte har konfigurerat autentisering i dina runbooks eller angetts hello kör som-autentiseringsuppgifter för hello Hybrid worker-gruppen.  Granska [Runbook-behörigheter](automation-hrw-run-runbooks.md#runbook-permissions) tooconfirm som du har konfigurerat autentisering korrekt för dina runbooks.  

## <a name="next-steps"></a>Nästa steg

Hjälp med felsökning av andra problem i Automation finns [felsökning av vanliga problem med Azure Automation](automation-troubleshooting-automation-errors.md) 