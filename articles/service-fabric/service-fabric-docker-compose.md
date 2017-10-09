---
title: "aaaAzure Service Fabric Docker Compose förhandsgranskning"
description: "Azure Service Fabric accepterar Docker Compose format toomake den enklare tooorchestrate-befintlig behållare med hjälp av Service Fabric. Detta stöd är för närvarande under förhandsgranskning."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: a60d1321fd6ef07b241a98c5ab2b8dfe5d441b53
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="docker-compose-application-support-in-azure-service-fabric-preview"></a>Docker Compose programstöd i Azure Service Fabric (förhandsgranskning)

Docker använder hello [docker-compose.yml](https://docs.docker.com/compose) filen för att definiera flera behållare program. toomake det enkelt för kunder som är bekant med Docker tooorchestrate befintliga behållarprogram på Azure Service Fabric, vi har tagit preview stöd för Docker Compose internt i hello-plattformen. Service Fabric kan acceptera version 3 och senare av `docker-compose.yml` filer. 

Eftersom detta stöd i förhandsgranskningen stöds bara en del av Skriv direktiven. Till exempel stöds programuppgraderingar inte. Du kan alltid ta bort och distribuera program i stället för att uppgradera dem.

toouse detta Förhandsgranska, skapa klustret med version 5.7 eller en större hello Service Fabric Runtime via hello Azure-portalen tillsammans med hello motsvarande SDK. 

> [!NOTE]
> Den här funktionen är i förhandsvisning och stöds inte i produktion.

## <a name="deploy-a-docker-compose-file-on-service-fabric"></a>Distribuera en Docker Compose fil på Service Fabric

hello följande kommandon skapar ett Service Fabric-program (med namnet `fabric:/TestContainerApp` i hello föregående exempel), som du kan övervaka och hantera som alla andra Service Fabric-program. Du kan använda hello angivna programnamnet för hälsoförfrågningar.

### <a name="use-powershell"></a>Använd PowerShell

Skapa ett Service Fabric skriva program från en docker-compose.yml-fil genom att köra följande kommando i PowerShell hello:

```powershell
New-ServiceFabricComposeApplication -ApplicationName fabric:/TestContainerApp -Compose docker-compose.yml [-RegistryUserName <>] [-RegistryPassword <>] [-PasswordEncrypted]
```

`RegistryUserName`och `RegistryPassword` finns toohello behållare registret användarnamn och lösenord. När du har slutfört hello programmet, kan du kontrollera dess status med hjälp av hello följande kommando:

```powershell
Get-ServiceFabricComposeApplicationStatus -ApplicationName fabric:/TestContainerApp -GetAllPages
```

toodelete hello skriva program via PowerShell, Använd hello följande kommando:

```powershell
Remove-ServiceFabricComposeApplication  -ApplicationName fabric:/TestContainerApp
```

### <a name="use-azure-service-fabric-cli-sfctl"></a>Använd Azure Service Fabric CLI (sfctl)

Alternativt kan du använda följande kommando för Service Fabric CLI hello:

```azurecli
sfctl compose create --application-id fabric:/TestContainerApp --compose-file docker-compose.yml [ [ --repo-user --repo-pass --encrypted ] | [ --repo-user ] ] [ --timeout ]
```

När du har skapat programmet hello, kan du kontrollera statusen genom att använda hello följande kommando:

```azurecli
sfctl compose status --application-id TestContainerApp [ --timeout ]
```

toodelete hello skriva program, Använd hello följande kommando:

```azurecli
sfctl compose remove  --application-id TestContainerApp [ --timeout ]
```

## <a name="supported-compose-directives"></a>Skriv-direktiv som stöds

Den här förhandsversionen stöder en delmängd av hello konfigurationsalternativ från hello Skriv version 3-format, inklusive hello följande primitiver:

* Tjänster > Distribuera > repliker
* Tjänster > Distribuera > placering > begränsningar
* Tjänster > Distribuera > resurser > gränser
    * cpu - resurser
    * -minne
    * -minne-byte
* Tjänster > kommandon
* Tjänster > miljö
* Tjänster > portar
* Tjänster > bild
* Tjänster > isolering (endast för Windows)
* Tjänster > loggning > drivrutin
* Tjänster > loggning > drivrutinen > Alternativ
* Volymen & Distribuera > volym

Konfigurera hello kluster för att genomdriva gränserna, enligt beskrivningen i [Service Fabric resurs styrning](service-fabric-resource-governance.md). Alla andra Docker Compose direktiv stöds inte för den här förhandsversionen.

## <a name="servicednsname-computation"></a>ServiceDnsName beräkning

Om hello tjänstnamnet som du anger i en Skriv-fil är ett fullständigt kvalificerat domännamn (dvs, den innehåller en punkt [.]), hello DNS-namn som registrerats av Service Fabric är `<ServiceName>` (inklusive hello punkt). Om inte, varje sökvägssegmentet i hello programnamn blir en domänetikett i hello tjänsten DNS-namn med hello första sökvägssegment blir hello toppnivådomänen etikett.

Till exempel om hello anges programnamnet är `fabric:/SampleApp/MyComposeApp`, `<ServiceName>.MyComposeApp.SampleApp` skulle vara hello registrerade DNS-namn.

## <a name="differences-between-compose-instance-definition-and-service-fabric-application-model-type-definition"></a>Skillnader mellan Skriv (definition av instans) och Service Fabric-programmodell (typdefinition)

En filen docker-compose.yml beskrivs distribuerbara behållare, inklusive deras egenskaper och konfigurationer.
Till exempel kan hello-filen innehålla miljövariabler och portar. Du kan också ange parametrar för distribution, till exempel placeringen och gränserna för DNS-namn, i filen för hello docker-compose.yml.

Hej [Service Fabric programmodell](service-fabric-application-model.md) använder tjänsttyper och programtyper, där du kan ha många programinstanser av hello samma typ. Du kan till exempel ha en programinstansen per kund. Den här typen baserat modellen har stöd för flera versioner av hello samma typ av program som har registrerats med hello körning.

Till exempel kund A kan ha ett program som instansieras med AppTypeA 1.0 och kund B kan ha ett annat program instansierades med hello samma typ och version. Du definierar hello programtyper i hello applikationsmanifest, och du kan ange hello namn och distribution programparametrar när du skapar hello program.

Även om den här modellen ger flexibilitet, planerar vi också toosupport en enklare, instans-baserade distributionsmodell där typer är implicit från hello manifestfilen. I den här modellen får varje program egna oberoende manifest. Vi förhandsgranskar detta arbete genom att lägga till stöd för docker-compose.yml, vilket är ett instans-baserad distribution-format.

## <a name="next-steps"></a>Nästa steg

* Håll dig uppdaterad om hello [programmodell för Service Fabric](service-fabric-application-model.md)
* [Kom igång med Service Fabric CLI](service-fabric-cli.md)
