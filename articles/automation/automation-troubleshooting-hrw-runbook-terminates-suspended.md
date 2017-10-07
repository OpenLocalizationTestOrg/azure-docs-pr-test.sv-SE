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
ms.openlocfilehash: 513a90d144e7ade9c21cd7f3b718578989702c25
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="hybrid-runbook-worker-a-runbook-job-terminates-with-a-status-of-suspended"></a>Hybrid Runbook Worker: Ett Runbook-jobb avslutas med statusen Pausad
## <a name="summary"></a>Sammanfattning
Din runbook har pausats strax efter försöker tooexecute den tre gånger. Det finns villkor som kan avbryta hello runbook slutförs och relaterade fel hälsningsmeddelande innehåller inte någon ytterligare information som anger varför. Den här artikeln innehåller felsökningssteg för problem relaterade toohello Runbook Worker-Hybrid runbook körning.

Om din Azure problemet inte åtgärdas i den här artikeln, gå hello Azure-forum på [MSDN och hello Stack Overflow](https://azure.microsoft.com/support/forums/). Du kan publicera problemet på dessa forum eller för[ @AzureSupport på Twitter](https://twitter.com/AzureSupport). Du kan också filen en Azure-supportbegäran genom att välja **få support** på hello [Azure-supporten](https://azure.microsoft.com/support/options/) plats.

## <a name="symptom"></a>Symtom
Runbook-körningen misslyckas och hello-felet som returnerades är ”hello Jobbåtgärden 'Aktivera' inte kan köras, eftersom hello processen oväntat stoppades. hello Jobbåtgärden gjordes 3 gånger ”.

## <a name="cause"></a>Orsak
Det finns flera möjliga orsaker till hello-fel: 

1. hello hybrid worker finns bakom en proxyserver eller brandvägg
2. hello datorn hello hybrid worker körs på har mindre än minimikraven för maskinvara hello [krav](automation-hybrid-runbook-worker.md#hybrid-runbook-worker-requirements) 
3. Hej runbooks autentisera inte med lokala resurser

## <a name="cause-1-hybrid-runbook-worker-is-behind-proxy-or-firewall"></a>Orsak 1: Runbook Worker-Hybrid är bakom en proxyserver eller brandvägg
hello datorn hello Hybrid Runbook Worker körs på finns bakom en brandvägg eller proxyserver server och utgående nätverksåtkomst kan inte beviljas eller konfigurerats korrekt.

### <a name="solution"></a>Lösning
Kontrollera hello datorn har utgående åtkomst too*.cloudapp .net på port 443, 9354 och 30000 30199. 

## <a name="cause-2-computer-has-less-than-minimum-hardware-requirements"></a>Orsak 2: Datorn har mindre än minimikraven för maskinvara
Datorer som kör hello Hybrid Runbook Worker måste uppfylla hello maskinvarukrav innan du utser den toohost den här funktionen. Annars kommer att bli överbelastad hello datorn beroende på hello resursutnyttjande andra bakgrundsprocesser och konkurrens på grund av runbooks under körning och orsaka fördröjningar för runbook-jobb eller timeout. 

### <a name="solution"></a>Lösning
Kontrollera först hello dator toorun hello Hybrid Runbook Worker funktionen uppfyller hello maskinvarukrav.  Om den finns, övervaka CPU och minne användning toodetermine alla korrelation mellan hello prestanda för Hybrid Runbook Worker-processer och Windows.  Om det finns minne eller CPU-belastning, detta kan indikera hello måste tooupgrade eller lägga till fler processorer eller öka minne tooaddress hello resurs flaskhals och lösa hello-fel. Du kan också markera en annan beräkningsresurser som stöder hello minimikrav och skala när arbetsbelastning indikera att öka krävs.         

## <a name="cause-3-runbooks-cannot-authenticate-with-local-resources"></a>Orsak 3: Runbooks inte autentisera med lokala resurser
### <a name="solution"></a>Lösning
Kontrollera hello **Microsoft SMA** händelseloggen för motsvarande händelse med beskrivning *Win32 processen avslutades med koden [4294967295]*.  hello orsaken till felet är du inte har konfigurerat autentisering i dina runbooks eller angetts hello kör som-autentiseringsuppgifter för hello Hybrid worker-gruppen.  Granska [Runbook-behörigheter](automation-hybrid-runbook-worker.md#runbook-permissions) tooconfirm som du har konfigurerat autentisering korrekt för dina runbooks.  

