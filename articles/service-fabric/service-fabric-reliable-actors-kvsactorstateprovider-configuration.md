---
title: "aaaChange KVSActorStateProvider inställningar i Azure mikrotjänster | Microsoft Docs"
description: "Lär dig mer om hur du konfigurerar Azure Service Fabric tillståndskänslig aktörer av typen KVSActorStateProvider."
services: Service-Fabric
documentationcenter: .net
author: sumukhs
manager: timlt
editor: 
ms.assetid: dbed72f4-dda5-4287-bd56-da492710cd96
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 6/29/2017
ms.author: sumukhs
ms.openlocfilehash: e003512678556e68a8926b1b9c6c28d9ae3979d2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-reliable-actors--kvsactorstateprovider"></a>Konfigurera Reliable Actors--KVSActorStateProvider
Du kan ändra hello standardkonfigurationen av KVSActorStateProvider genom att ändra hello settings.xml fil som har genererats i hello Microsoft Visual Studio paketrot hello Config i mappen för hello angivna aktören.

hello Azure Service Fabric runtime söker efter fördefinierade Avsnittsnamnen i hello settings.xml filen och förbrukar hello konfigurationsvärden när du skapar hello underliggande körtidskomponenter.

> [!NOTE]
> Gör **inte** ta bort eller ändra hello Avsnittsnamnen av hello följande konfigurationer i hello settings.xml-filen som har genererats i hello Visual Studio-lösning.
> 
> 

## <a name="replicator-security-configuration"></a>Replikatorn säkerhetskonfiguration
Replikatorn säkerhetskonfigurationer är används toosecure hello kommunikationskanalen som används vid replikering. Det innebär att tjänster inte kan se varandras replikeringstrafik, säkerställer att hello data som görs högtillgänglig också säker.
Som standard förhindrar en tom säkerhetskonfigurationsavsnittet replikeringssäkerhet.

### <a name="section-name"></a>Avsnittsnamn
&lt;ActorName&gt;ServiceReplicatorSecurityConfig

## <a name="replicator-configuration"></a>Replikatorn konfiguration
Konfigurationer för replikatorn konfigurera hello replikatorn som ansvarar för att göra hello aktören Tillståndsprovidern tillstånd hög tillförlitlig.
hello standardkonfigurationen genereras av hello Visual Studio-mall och bör vara tillräckligt. Det här avsnittet innehåller information om ytterligare konfigurationer som är tillgängliga tootune hello replikatorn.

### <a name="section-name"></a>Avsnittsnamn
&lt;ActorName&gt;ServiceReplicatorConfig

### <a name="configuration-names"></a>Konfigurationsnamn
| Namn | Enhet | Standardvärde | Kommentarer |
| --- | --- | --- | --- |
| BatchAcknowledgementInterval |Sekunder |0.015 |Tidsperiod för vilken hello replikatorn på hello sekundära väntar när du har fått en åtgärd innan du skickar tillbaka en bekräftelse toohello primära. Alla andra bekräftelser toobe skickas för åtgärder som behandlas inom intervallet skickas som ett svar. |
| ReplicatorEndpoint |Saknas |Ingen standard--obligatorisk parameter |IP-adress och port som hello primära och sekundära replikatorn använder toocommunicate med andra replikatörer i hello replikuppsättningen. Detta bör referera en TCP-slutpunkt för resurs i hello service manifest. Se för[Service manifest resurser](service-fabric-service-manifest-resources.md) tooread mer om hur du definierar endpoint resurser i hello service manifest. |
| RetryInterval |Sekunder |5 |Tidsperiod efter vilken hello replikatorn igen skickar ett meddelande om den inte får ett godkännande för en åtgärd. |
| MaxReplicationMessageSize |Byte |50 MB |Maximal storlek för replikeringsdata som kan överföras i ett enda meddelande. |
| MaxPrimaryReplicationQueueSize |Antal åtgärder |1024 |Maximalt antal åtgärder i hello primära kön. En åtgärd frigjorts när hello primära replikatorn tar emot en bekräftelse från alla hello sekundära replikatörer. Det här värdet måste vara större än 64 och delbart med 2. |
| MaxSecondaryReplicationQueueSize |Antal åtgärder |2048 |Maximalt antal åtgärder i hello sekundär kö. När du har gjort tillståndet hög tillgänglighet via beständiga frigjorts en åtgärd. Det här värdet måste vara större än 64 och delbart med 2. |

## <a name="store-configuration"></a>Store-konfiguration
Store konfigurationer är används tooconfigure hello lokalt arkiv som har använt toopersist hello tillstånd som håller på att replikeras.
hello standardkonfigurationen genereras av hello Visual Studio-mall och bör vara tillräckligt. Det här avsnittet innehåller information om ytterligare konfigurationer som är tillgängliga tootune hello lokalt Arkiv.

### <a name="section-name"></a>Avsnittsnamn
&lt;ActorName&gt;ServiceLocalStoreConfig

### <a name="configuration-names"></a>Konfigurationsnamn
| Namn | Enhet | Standardvärde | Kommentarer |
| --- | --- | --- | --- |
| MaxAsyncCommitDelayInMilliseconds |millisekunder |200 |Anger hello maximala batchbearbetning intervall för beständig lokalt Arkiv genomföranden. |
| MaxVerPages |Antal sidor |16384 |hello maximalt antal version sidor i hello lokal lagring databasen. Den avgör hello maximalt antal väntande transaktioner. |

## <a name="sample-configuration-file"></a>Exempel på konfigurationsfil
```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="MyActorServiceReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="MyActorServiceReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
   </Section>
   <Section Name="MyActorServiceLocalStoreConfig">
      <Parameter Name="MaxVerPages" Value="8192" />
   </Section>
   <Section Name="MyActorServiceReplicatorSecurityConfig">
      <Parameter Name="CredentialType" Value="X509" />
      <Parameter Name="FindType" Value="FindByThumbprint" />
      <Parameter Name="FindValue" Value="9d c9 06 b1 69 dc 4f af fd 16 97 ac 78 1e 80 67 90 74 9d 2f" />
      <Parameter Name="StoreLocation" Value="LocalMachine" />
      <Parameter Name="StoreName" Value="My" />
      <Parameter Name="ProtectionLevel" Value="EncryptAndSign" />
      <Parameter Name="AllowedCommonNames" Value="My-Test-SAN1-Alice,My-Test-SAN1-Bob" />
   </Section>
</Settings>
```
## <a name="remarks"></a>Kommentarer
Hej BatchAcknowledgementInterval parameter styr replikeringsfördröjning. Värdet '0' resulterar i hello lägsta möjliga tidsfördröjning, kostnad hello genomströmning (som flera bekräftelsemeddelanden måste skickas och bearbetas, som innehåller färre bekräftelser).
hello större hello-värdet för BatchAcknowledgementInterval, hello högre hello övergripande replikering genomströmning vid hello kostnaden för högre latens för åtgärden. Detta innebär direkt toohello svarstiden för genomförda transaktioner.

