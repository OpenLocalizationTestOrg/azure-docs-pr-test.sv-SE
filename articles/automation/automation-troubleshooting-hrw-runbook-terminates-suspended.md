---
title: 'Runbook Worker-hybrid: Avslutar ett runbook-jobb med statusen Pausad | Microsoft Docs'
description: "Symptom orsaker och lösningar för Hybrid Runbook Worker jobbfel avslutning."
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
ms.date: 08/17/2016
ms.author: magoedte
ms.openlocfilehash: 7c6365b729d73f1c5b9bc57952b1723255d9e9f0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="hybrid-runbook-worker-a-runbook-job-terminates-with-a-status-of-suspended"></a>Hybrid Runbook Worker: Ett Runbook-jobb avslutas med statusen Pausad
## <a name="summary"></a>Sammanfattning
Din runbook har pausats strax efter att försök att köra den tre gånger. Det finns villkor som kan avbryta runbook slutförs och relaterade felmeddelandet innehåller inte någon ytterligare information som anger varför. Den här artikeln innehåller felsökningssteg för problem relaterade till Runbook Worker-Hybrid runbook körning av fel.

Om din Azure problemet inte åtgärdas i den här artikeln, besöker du Azure-forum på [MSDN och Stack Overflow](https://azure.microsoft.com/support/forums/). Du kan publicera problemet på dessa forum eller [ @AzureSupport på Twitter](https://twitter.com/AzureSupport). Du kan också filen en Azure-supportbegäran genom att välja **få support** på den [Azure-supporten](https://azure.microsoft.com/support/options/) plats.

## <a name="symptom"></a>Symtom
Runbook-körningen misslyckas och felet som returnerades är ”Jobbåtgärden 'Aktivera' inte kan köras eftersom processen oväntat stoppades. Jobbåtgärden gjordes 3 gånger ”.

## <a name="cause"></a>Orsak
Det finns flera möjliga orsaker till felet: 

1. Worker-hybriden finns bakom en proxyserver eller brandvägg
2. Worker-hybriden körs på datorn har mindre än minimikraven på maskinvara [krav](automation-hybrid-runbook-worker.md#hybrid-runbook-worker-requirements) 
3. Runbooks kan inte autentisera med lokala resurser

## <a name="cause-1-hybrid-runbook-worker-is-behind-proxy-or-firewall"></a>Orsak 1: Runbook Worker-Hybrid är bakom en proxyserver eller brandvägg
Runbook Worker-hybriden körs på datorn finns bakom en brandvägg eller proxyserver server och utgående nätverksåtkomst kan inte beviljas eller konfigurerats korrekt.

### <a name="solution"></a>Lösning
Kontrollera att datorn är utgående åtkomst till *. cloudapp.net på port 443, 9354 och 30000 30199. 

## <a name="cause-2-computer-has-less-than-minimum-hardware-requirements"></a>Orsak 2: Datorn har mindre än minimikraven för maskinvara
Datorer som kör Runbook Worker-hybriden ska uppfylla minimikraven för maskinvara innan du utser den som värd för den här funktionen. Annars kommer att bli överbelastad datorn beroende på resursutnyttjande andra bakgrundsprocesser och konkurrens på grund av runbooks under körning och orsaka fördröjningar för runbook-jobb eller timeout. 

### <a name="solution"></a>Lösning
Kontrollera först tilldelad att köra funktionen Hybrid Runbook Worker datorn uppfyller minimikraven på maskinvara.  Om den finns, kan du övervaka användningen av CPU och minne för att fastställa alla korrelation mellan prestanda för Hybrid Runbook Worker-processer och Windows.  Om det finns minne eller CPU-belastning, kan detta tyda på att behöva uppgradera eller lägga till fler processorer eller öka minne för att adressera resurs flaskhalsar och åtgärda felet. Du kan också markera en annan beräkningsresurser som stöder de minimikrav och skala när arbetsbelastning indikera att öka krävs.         

## <a name="cause-3-runbooks-cannot-authenticate-with-local-resources"></a>Orsak 3: Runbooks inte autentisera med lokala resurser
### <a name="solution"></a>Lösning
Kontrollera den **Microsoft SMA** händelseloggen för motsvarande händelse med beskrivning *Win32 processen avslutades med koden [4294967295]*.  Orsaken till felet är att du inte har konfigurerat autentisering i dina runbooks eller angivna kör som-autentiseringsuppgifter för Hybrid worker-gruppen.  Granska [Runbook-behörigheter](automation-hybrid-runbook-worker.md#runbook-permissions) att bekräfta att du har konfigurerat autentisering korrekt för dina runbooks.  

