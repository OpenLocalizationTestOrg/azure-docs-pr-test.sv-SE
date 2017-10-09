---
title: "aaaAzure IoT Suite anslutna factory vanliga frågor och svar | Microsoft Docs"
description: "Vanliga frågor och svar för IoT Suite anslutna factory"
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/15/2017
ms.author: corywink
ms.openlocfilehash: 4ae9beb0daf1b0578850cd652eaca7635b0d039d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions-for-iot-suite-connected-factory-preconfigured-solution"></a>Vanliga frågor och svar för IoT Suite anslutna factory förkonfigurerade lösningen

Se även, hello allmänna [vanliga frågor och svar](iot-suite-faq.md) för IoT Suite.

### <a name="where-can-i-find-hello-source-code-for-hello-preconfigured-solution"></a>Var hittar jag hello källkoden för hello förkonfigurerade lösningen?

hello källkoden lagras i hello följande GitHub-lagringsplatsen:

* [Anslutna factory förkonfigurerade lösningen](https://github.com/Azure/azure-iot-connected-factory)

### <a name="what-is-opc-ua"></a>Vad är OPC UA?

OPC enhetlig arkitektur (UA), utgivet 2008, är en plattformsoberoende, tjänstorienterad samverkan som standard. OPC UA används av olika industriella datorer och enheter, till exempel branschen datorer, sensorer och PLCs. OPC UA integrerar hello funktioner hello OPC klassiska specifikationer i ett utökningsbart ramverk med inbyggd säkerhet. Det är en standard som styrs av hello OPC Foundation. Hej [OPC Foundation](http://opcfoundation.org/) är en icke-kommersiell organisation med mer än 440 medlemmar. hello målet med hello organisation är toouse OPC specifikationer toofacilitate flera leverantörer, flera plattformar, säkert och tillförlitligt interoperabilitet via:

* Infrastruktur
* Specifikationer
* Teknologi
* Processer

### <a name="why-did-microsoft-choose-opc-ua-for-hello-connected-factory-preconfigured-solution"></a>Varför Microsoft att välja OPC UA för hello anslutna factory förkonfigurerade lösningen?

Microsoft har valt OPC UA eftersom det är en öppen plattform för icke-generiska, oberoende branschen identifieras och beprövade standard. Det är ett krav för att säkerställa samverkan mellan en bred uppsättning tillverkningsprocesser och utrustning Industrie 4.0 (RAMI4.0) referens arkitektur lösningar. Microsoft ser begäran från våra kunder toobuild Industrie 4.0-lösningar. Stöd för OPC UA hjälper lägre hello barriären för kunder tooachieve sina mål och ger omedelbar business värdet toothem.

### <a name="how-do-i-add-a-public-ip-address-toohello-simulation-vm"></a>Hur lägger jag till en offentlig IP-adress toohello simulering VM?

Du har två alternativ tooadd hello IP-adress:

* Använd PowerShell-skript för hello `Simulation/Factory/Add-SimulationPublicIp.ps1` i hello [databasen](https://github.com/Azure/azure-iot-connected-factory). Ange distributionsnamnet på din som en parameter. En lokal distribution använda `<your username>ConnFactoryLocal`. hello skript skriver ut hello hello Virtuella IP-adress.

* Leta upp hello resursgruppen för distributionen i hello Azure-portalen. Förutom en lokal distribution har hello resursgruppen hello namn du angav som lösning eller distribution. För en lokal distribution med hello build skript, hello hello resursgruppen heter `<your username>ConnFactoryLocal`. Lägg nu till en ny **offentliga IP-adressen** resurs toohello resursgrupp.

> [!NOTE]
> I båda fallen kan se till att du installerar hello senaste korrigeringarna genom att följa hello anvisningar på hello [Ubuntu webbplats](https://wiki.ubuntu.com/Security/Upgrades). Behåll hello installationen in toodate för så länge som den virtuella datorn kan nås via en offentlig IP-adress.

### <a name="how-do-i-remove-hello-public-ip-address-toohello-simulation-vm"></a>Hur tar jag bort hello offentliga IP-adress toohello simuleringen VM?

Du har två alternativ tooremove hello IP-adress:

* Använda hello PowerShell-skript Simulation/Factory/Remove-SimulationPublicIp.ps1 av hello [databasen](https://github.com/Azure/azure-iot-connected-factory). Ange distributionsnamnet på din som en parameter. En lokal distribution använda `<your username>ConnFactoryLocal`. hello skript skriver ut hello hello Virtuella IP-adress.

* Leta upp hello resursgruppen för distributionen i hello Azure-portalen. Förutom en lokal distribution har hello resursgruppen hello namn du angav som lösning eller distribution. För en lokal distribution med hello build skript, hello hello resursgruppen heter `<your username>ConnFactoryLocal`. Ta bort hello nu **offentliga IP-adressen** resurs från hello resursgruppen.

### <a name="how-do-i-sign-in-toohello-simulation-vm"></a>Hur loggar jag in toohello simuleringen VM?

Logga in toohello simuleringen VM stöds endast om du har distribuerat din lösning med hjälp av PowerShell-skript för hello `build.ps1` i hello [databasen](https://github.com/Azure/azure-iot-connected-factory).

Om du har distribuerat hello-lösning från www.azureiotsuite.com kan du inte logga in toohello VM. Du kan inte logga in, eftersom hello lösenord genereras slumpmässigt och du kan inte återställa den.

1. Lägg till en offentlig IP-adress toohello VM. Se [hur lägger jag till en offentlig IP-adress toohello simulering VM?](#how-do-i-remove-the-public-ip-address-to-the-simulation-vm)
1. Skapa en SSH-session tooyour VM med hjälp av hello VM hello IP-adress.
1. hello användarnamn toouse är: `docker`.
1. hello lösenord toouse beror på hello-version som du använde toodeploy:
    * Lösningar som distribueras med hello build.ps1 skript före den 1 juni 2017 hello lösenordet är: `Passw0rd`.
    * För lösningar som distribueras med hjälp av hello build.ps1 skript efter den 1 juni 2017, hittar du hello lösenord i hello `<name of your deployment>.config.user` fil. hello lösenord lagras i hello **VmAdminPassword** inställningen. hello lösenord genereras slumpmässigt vid tidpunkten för distribution om du inte anger den med hjälp av hello `build.ps1` skript parametern`-VmAdminPassword`

### <a name="how-do-i-stop-and-start-all-docker-processes-in-hello-simulation-vm"></a>Hur jag för att stoppa och starta alla docker processer i hello simuleringen VM?

1. Logga in toohello simuleringen VM. Se [hur loggar jag in toohello simuleringen VM?](#how-do-i-sign-in-to-the-simulation-vm)
1. toocheck vilka behållare är aktiv, kör: `docker ps`.
1. toostop alla simuleringen behållare, kör: `./stopsimulation`.
1. toostart alla simuleringen behållare:
    * Exportera en shell variabel med hello namn **IOTHUB_CONNECTIONSTRING**. Använda hello värdet hello **IotHubOwnerConnectionString** i hello `<name of your deployment>.config.user` fil. Exempel:

        ```
        export IOTHUB_CONNECTIONSTRING="HostName={yourdeployment}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={your key}"
        ```

    * Kör `./startsimulation`.

### <a name="how-do-i-update-hello-simulation-in-hello-vm"></a>Hur uppdaterar hello simuleringen i hello VM?

Om du har gjort ändringar toohello simulering, kan du använda hello PowerShell-skript `build.ps1` i hello [databasen](https://github.com/Azure/azure-iot-connected-factory) med hello `updatedimulation` kommando. Det här skriptet bygger alla hello simuleringen komponenter, stoppar hello simuleringen i hello VM, överför, installerar och startar dem..

### <a name="how-do-i-find-out-hello-connection-string-of-hello-iot-hub-used-by-my-solution"></a>Hur tar jag reda hello anslutningssträngen för hello IoT-hubb som används av min-lösning?

Om du har distribuerat en lösning med hello `build.ps1` skriptet i hello [databasen](https://github.com/Azure/azure-iot-connected-factory), hello anslutningssträngen är hello värdet för **IotHubOwnerConnectionString** i hello `<name of your deployment>.config.user` fil.

Du kan också hitta hello anslutningssträngen med hello Azure-portalen. Leta upp inställningarna för hello-anslutningssträngar i hello IoT-hubb resurs i hello resursgruppen för din distribution.

### <a name="which-iot-hub-devices-does-hello-connected-factory-simulation-use"></a>Vilka IoT Hub-enheter hello anslutna factory simuleringen används?

Hej simuleringen self registrerar hello följande enheter:

* proxy.Beijing.corp.contoso
* proxy.capetown.corp.contoso
* proxy.Mumbai.corp.contoso
* proxy.munich0.corp.contoso
* proxy.Rio.corp.contoso
* proxy.Seattle.corp.contoso
* Publisher.Beijing.corp.contoso
* Publisher.capetown.corp.contoso
* Publisher.Mumbai.corp.contoso
* Publisher.munich0.corp.contoso
* Publisher.Rio.corp.contoso
* Publisher.Seattle.corp.contoso

Med hjälp av hello [DeviceExplorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) eller [iothub explorer](https://github.com/azure/iothub-explorer) verktyg, men du kan kontrollera vilka enheter som registreras med hello IoT-hubb med hjälp av din lösning. toouse dessa verktyg, behöver du hello anslutningssträngen för hello IoT-hubb i distributionen.

### <a name="how-can-i-get-log-data-from-hello-simulation-components"></a>Hur kan jag loggdata från hello simuleringen komponenter?

Alla komponenter i hello simuleringen logginformation i toolog filer. Dessa filer kan hittas i hello VM i hello mappen `home/docker/Logs`. tooretrieve hello händelseloggarna, använder du PowerShell-skript för hello `Simulation/Factory/Get-SimulationLogs.ps1` i hello [databasen](https://github.com/Azure/azure-iot-connected-factory).

Det här skriptet måste toosign i toohello VM. Du kan behöva tooprovide autentiseringsuppgifter för inloggning hello. Se [hur loggar jag in toohello simuleringen VM?](#how-do-i-sign-in-to-the-simulation-vm) toofind hello autentiseringsuppgifter.

hello skriptet lägger till/tar bort en offentlig IP-adress toohello VM, om det ännu inte har något och tar bort den. hello skript placerar alla loggfiler i ett Arkiv och hämtar hello Arkiv tooyour development arbetsstation.

Du kan också logga in toohello VM via SSH och inspektera hello loggfiler vid körning.

### <a name="how-can-i-check-if-hello-simulation-is-sending-data-toohello-cloud"></a>Hur kan jag kontrollera om hello simuleringen skickar data toohello molnet?

Med hello [DeviceExplorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) eller hello [iothub explorer](https://github.com/azure/iothub-explorer) verktyg men, du kan inspektera hello data som skickas tooIoT hubb från vissa enheter. toouse dessa verktyg, behöver du tooknow hello anslutningssträngen för hello IoT-hubb i distributionen. Se [hur tar jag reda hello anslutningssträngen för hello IoT-hubb som används av min-lösning?](#how-do-i-find-out-the-connection-string-of-the-iot-hub-used-by-my-solution)

Inspektera hello data som skickas av en av hello publisher enheter:

* Publisher.Beijing.corp.contoso
* Publisher.capetown.corp.contoso
* Publisher.Mumbai.corp.contoso
* Publisher.munich0.corp.contoso
* Publisher.Rio.corp.contoso
* Publisher.Seattle.corp.contoso

Om du ser inga data skickas tooIoT navet är ett problem med hello simuleringen. Som ett första steg analys bör du analysera hello loggfiler hello simuleringen komponenter. Se [hur kan jag loggdata från hello simuleringen komponenter?](#how-can-i-get-log-data-from-the-simulation-components) Därefter försök toostop och starta hello simulering och om det finns fortfarande inga data skickas, uppdatera hello simuleringen helt. Se [hur uppdaterar jag hello simuleringen i hello VM?](#how-do-i-update-the-simulation-in-the-vm)

### <a name="next-steps"></a>Nästa steg

Du kan även utforska några av hello andra funktioner och förmågor i hello IoT Suite förkonfigurerade lösningar:

* [Förutsägande Underhåll förkonfigurerade lösning: översikt](iot-suite-predictive-overview.md)
* [Anslutna factory förkonfigurerade lösning: översikt](iot-suite-connected-factory-overview.md)
* [IoT-säkerhet från hello bakgrund](securing-iot-ground-up.md)
