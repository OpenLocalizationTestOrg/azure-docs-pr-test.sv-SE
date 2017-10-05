---
title: "Ändra inställningarna för Azure Service Fabric-kluster | Microsoft Docs"
description: "Den här artikeln beskriver fabric-inställningar och fabric uppgradera principer som du kan anpassa."
services: service-fabric
documentationcenter: .net
author: chackdan
manager: timlt
editor: 
ms.assetid: 7ced36bf-bd3f-474f-a03a-6ebdbc9677e2
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/15/2017
ms.author: chackdan
ms.openlocfilehash: d71fdcf2a054a9b9ac1d74ddd3a6b43d151fa87d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="customize-service-fabric-cluster-settings-and-fabric-upgrade-policy"></a>Anpassa inställningar för Service Fabric-kluster och Fabric-uppgradera princip
Det här dokumentet förklarar hur du anpassar olika infrastrukturinställningarna och infrastrukturen uppgradera princip för Service Fabric-klustret. Du kan anpassa dem på portalen eller med en Azure Resource Manager-mall.

> [!NOTE]
> Inte alla inställningar kan vara tillgängliga via portalen. Anpassa den med hjälp av en Azure Resource Manager-mall om en inställning nedan inte är tillgängliga via portalen.
> 

## <a name="customizing-service-fabric-cluster-settings-using-azure-resource-manager-templates"></a>Anpassa inställningar för Service Fabric-kluster med hjälp av Azure Resource Manager-mallar
Stegen nedan illustrerar hur du lägger till en ny inställning *MaxDiskQuotaInMB* till den *diagnostik* avsnitt.

1. Gå till https://resources.azure.com
2. Navigera till din prenumeration genom att expandera prenumerationer -> resurs grupper -> Microsoft.ServiceFabric -> klustrets namn
3. I det övre högra hörnet väljer du ”full”
4. Välj Redigera och uppdatera den `fabricSettings` JSON-element och lägga till ett nytt element

```
      {
        "name": "Diagnostics",
        "parameters": [
          {
            "name": "MaxDiskQuotaInMB",
            "value": "65536"
          }
        ]
      }
```

## <a name="fabric-settings-that-you-can-customize"></a>Fabric-inställningar som du kan anpassa
Här följer de Fabric-inställningar som du kan anpassa:

### <a name="section-name-diagnostics"></a>Avsnittsnamn: diagnostik
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| ConsumerInstances |Sträng |Lista över DCA konsumenten instanser. |
| ProducerInstances |Sträng |Lista över DCA producenten instanser. |
| AppEtwTraceDeletionAgeInDays |Int, standardinställningen är 3 |Antal dagar efter vilken vi ta bort gamla ETL-filer som innehåller programmet ETW-spårning. |
| AppDiagnosticStoreAccessRequiresImpersonation |Bool, standard är SANT |Om personifiering är obligatorisk när åtkomst till diagnostiska lagrar åt programmet. |
| MaxDiskQuotaInMB |Int, standard är 65536 |Diskkvot i MB för Windows Fabric-loggfiler. |
| DiskFullSafetySpaceInMB |Int, standard är 1024 |Återstående diskutrymme i MB för att skydda från användning av DCA. |
| ApplicationLogsFormatVersion |Int, standardvärdet är 0 |Versionen för programmet loggar format. Värden som stöds är 0 och 1. Version 1 innehåller flera fält i posten ETW-händelse än version 0. |
| ClusterId |Sträng |Unikt id för klustret. Det här genereras när klustret skapas. |
| EnableTelemetry |Bool, standard är SANT |Detta kommer att aktivera eller inaktivera telemetri. |
| EnableCircularTraceSession |Bool, standard är FALSKT |Flagga som anger om cirkulär spårningssessioner ska användas. |

### <a name="section-name-traceetw"></a>Avsnittsnamn: Spåra/Etw
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| Nivå |Int, standardvärdet är 4 |Spårningsnivån för etw kan ha värden 1, 2, 3, 4. Att kunna användas måste du behålla spårningsnivån vid 4 |

### <a name="section-name-performancecounterlocalstore"></a>Avsnittsnamn: PerformanceCounterLocalStore
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| IsEnabled |Bool, standard är SANT |Flagga som anger om prestandaräknarsamlingen på lokal nod är aktiverad. |
| SamplingIntervalInSeconds |Int, standardvärdet är 60 |Exempelintervall för prestandaräknare som samlas in. |
| Räknare |Sträng |Kommaavgränsad lista över prestandaräknare för att samla in. |
| MaxCounterBinaryFileSizeInMB |Int, standardvärdet är 1 |Maximal storlek (i MB) för varje prestandaräknaren binär fil. |
| NewCounterBinaryFileCreationIntervalInMinutes |Int, standardvärdet är 10 |Maximalt tidsintervall (i sekunder) efter vilken en ny prestandaräknaren binär fil skapas. |

### <a name="section-name-setup"></a>Avsnittsnamn: installationsprogrammet
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| FabricDataRoot |Sträng |Rotkatalogen för Service Fabric-data. Som standard för Azure är d:\svcfab |
| FabricLogRoot |Sträng |Service fabric rot loggkatalogen. Detta är SA loggar och spårningar placering. |
| ServiceRunAsAccountName |Sträng |Kontonamn som du vill köra infrastrukturvärdtjänsten under. |
| ServiceStartupType |Sträng |Starttypen för fabrikstjänsten-värden. |
| SkipFirewallConfiguration |Bool, standard är FALSKT |Anger om brandväggsinställningar måste anges i systemet eller inte. Detta gäller endast om du använder windows-brandväggen. Om du använder brandväggar från tredje part, måste du öppna portar för system och program som ska användas |

### <a name="section-name-transactionalreplicator"></a>Avsnittsnamn: TransactionalReplicator
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| MaxCopyQueueSize |Uint, standard är 16384 |Detta är högsta värdet definierar den ursprungliga storleken för kön som underhåller replikeringsåtgärder. Observera att det måste vara delbart med 2. Att kommer begränsas om körningsfel kön till den här storleken-åtgärder mellan de primära och sekundära replikatörer. |
| BatchAcknowledgementInterval | Tid i sekunder är standardvärdet 0.015 | Ange tidsintervall i sekunder. Anger hur lång tid som replikatorn väntar när du har fått en åtgärd innan en bekräftelse skickas tillbaka. Andra åtgärder som togs emot under den här tiden har sina bekräftelser skickas tillbaka i ett enda meddelande -> minska nätverkstrafiken men potentiellt minskar genomflödet av replikatorn. |
| MaxReplicationMessageSize |Uint, standard är 52428800 | Maximal meddelandestorlek på replikeringsåtgärder. Standardvärdet är 50MB. |
| ReplicatorAddress |strängen, standard är ”localhost:0” | Slutpunkt i form av en sträng-'IP:Port' som används av Windows Fabric replikatorn upprätta anslutningar till andra repliker för att kunna skicka och ta emot åtgärder. |
| InitialPrimaryReplicationQueueSize |Uint, standard är 64 | Det här värdet anger den ursprungliga storleken för kön som underhåller replikeringsåtgärder på primärt. Observera att det måste vara delbart med 2.|
| MaxPrimaryReplicationQueueSize |Uint, standard är 8192 |Detta är det maximala antalet åtgärder som kan finnas i kön primär replikering. Observera att det måste vara delbart med 2. |
| MaxPrimaryReplicationQueueMemorySize |Uint, standardvärdet är 0 |Detta är det maximala värdet för den primära Replikeringskön i byte. |
| InitialSecondaryReplicationQueueSize |Uint, standard är 64 |Det här värdet anger den ursprungliga storleken för kön som underhåller replikeringsåtgärder på sekundärt. Observera att det måste vara delbart med 2. |
| MaxSecondaryReplicationQueueSize |Uint, standard är 16384 |Detta är det maximala antalet åtgärder som kan finnas i sekundära Replikeringskön. Observera att det måste vara delbart med 2. |
| MaxSecondaryReplicationQueueMemorySize |Uint, standardvärdet är 0 |Detta är det maximala värdet för den sekundära Replikeringskön i byte. |
| SecondaryClearAcknowledgedOperations |Bool, standard är FALSKT |Bool som styr om åtgärder på sekundär replikatorn är avmarkerad när de bekräftas till den primära servern (replikatorn tömmer till disken). Inställningar för den till TRUE kan medföra ytterligare Diskläsningar på den nya primärt när fånga repliker efter en redundansväxling. |
| MaxMetadataSizeInKB |Int, standardvärdet är 4 |Maximal storlek för log-metadata för dataströmmen. |
| MaxRecordSizeInKB |Uint, standard är 1024 | Maximal storlek för en loggpost för dataströmmen. |
| CheckpointThresholdInMB |Int, standardvärdet är 50 |En kontrollpunkt påbörjas när loggen användningen överskrider detta värde. |
| MaxAccumulatedBackupLogSizeInMB |Int, är standard 800 |Max ackumulerade storleken (i MB) för säkerhetskopiering loggar i en viss loggkedja. En inkrementell säkerhetskopiering begäranden misslyckas om den inkrementella säkerhetskopian skulle Generera en säkerhetskopiering logg som skulle orsaka de ackumulerade antalet säkerhetskopieringsloggar sedan den relevanta fullständiga säkerhetskopian ska vara större än den här storleken. I sådana fall måste användaren gör en fullständig säkerhetskopia. |
| MaxWriteQueueDepthInKB |Int, standardvärdet är 0 | Int för maximal skriva ködjup som core loggaren kan använda som anges i kilobyte för den logg som är associerad med den här repliken. Det här värdet är maximalt antal byte som kan vara utestående under viktiga uppdateringar av meddelandeloggfiler. Det kan vara 0 för kärnor loggaren att beräkna ett lämpligt värde eller en multipel av 4. |
| SharedLogId |Sträng |Delade identifierare. Detta är ett guid och måste vara unikt för varje delad logg. |
| SharedLogPath |Sträng |Sökvägen till delad loggen. Om det här värdet är tomt används standard delade loggen. |
| SlowApiMonitoringDuration |Tid i sekunder, standard är 300 | Ange varaktighet för api innan varning hälsa händelsen utlöses.|
| MinLogSizeInMB |Int, standardvärdet är 0 |Minsta storlek för transaktionell loggen. Loggen kommer inte att kunna trunkera till en storlek under den här inställningen. 0 anger att replikatorn avgör den minsta loggfilsstorleken enligt andra inställningar. Det här värdet ökar risken för att göra partiella kopior och inkrementella säkerhetskopieringar eftersom risken för relevanta loggposter trunkerades sänks. |

### <a name="section-name-fabricclient"></a>Avsnittsnamn: FabricClient
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| NodeAddresses |sträng, standardvärdet är ”” |En samling av adresser (anslutningssträngar) på olika noder som kan användas för att kommunicera med den Naming Service. Först ansluter klienten att välja en av adresserna slumpmässigt. Om mer än en anslutningssträngen har angetts och en anslutning misslyckas på grund av en kommunikation eller timeout-fel. Klienten växlar att använda nästa adress sekventiellt. Se Naming serviceadressen försök information om återförsök semantik. |
| ConnectionInitializationTimeout |Tid i sekunder, är standardvärdet 2 |Ange tidsintervall i sekunder. Tidsgränsen för anslutning för varje gång klienten försöker öppna en anslutning till gatewayen. |
| PartitionLocationCacheLimit |Int, standard är 100000 |Antalet partitioner som cachelagrats för tjänsten upplösning (Ange 0 för ingen gräns). |
| ServiceChangePollInterval |Tid i sekunder, standardvärdet är 120 |Ange tidsintervall i sekunder. Intervallet mellan på varandra följande avsökningar för tjänst ändras från klienten till gateway för registrerade tjänsten ändra meddelanden återanrop. |
| KeepAliveIntervalInSeconds |Int, standardvärdet är 20 |Tidsintervall som FabricClient transporten skickar keep-alive-meddelanden till gatewayen. För 0; keepAlive är inaktiverad. Måste vara ett positivt värde. |
| HealthOperationTimeout |Tid i sekunder, standardvärdet är 120 |Ange tidsintervall i sekunder. Timeout för en rapportmeddelande som skickas till Health Manager. |
| HealthReportSendInterval |Tid i sekunder, standardvärdet är 30 |Ange tidsintervall i sekunder. Intervallet då reporting komponenten skickar ackumulerade hälsorapporter till Health Manager. |
| HealthReportRetrySendInterval |Tid i sekunder, standardvärdet är 30 |Ange tidsintervall i sekunder. Intervallet då reporting komponenten igen skickar ackumulerade hälsorapporter till Health Manager. |
| RetryBackoffInterval |Tid i sekunder, standardinställningen är 3 |Ange tidsintervall i sekunder. Inte intervallet innan du försöker igen. |
| MaxFileSenderThreads |Uint, standardvärdet är 10 |Max antal filer som överförs parallellt. |

### <a name="section-name-common"></a>Avsnittsnamn: vanliga
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| PerfMonitorInterval |Tid i sekunder är standardvärdet 1 |Ange tidsintervall i sekunder. Prestanda Övervakningsintervall. Inställningen till 0 eller negativt värde inaktiverar övervakning. |

### <a name="section-name-healthmanager"></a>Avsnittsnamn: HealthManager
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| EnableApplicationTypeHealthEvaluation |Bool, standard är FALSKT |Klustret hälsoprincip för utvärdering: aktivera per program typen hälsoutvärderingen. |

### <a name="section-name-fabricnode"></a>Avsnittsnamn: FabricNode
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| StateTraceInterval |Tid i sekunder, standard är 300 |Ange tidsintervall i sekunder. Intervall för att spåra nod status på varje nod och uppåt noder på FM/FMM. |

### <a name="section-name-nodedomainids"></a>Avsnittsnamn: NodeDomainIds
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| UpgradeDomainId |sträng, standardvärdet är ”” |Beskriver uppgraderingsdomänen som tillhör en nod. |
| PropertyGroup |NodeFaultDomainIdCollection |Beskriver feldomäner som tillhör en nod. Feldomänen definieras via en URI som beskriver var noden i datacentret.  Fel domän URI: er är i formatet fd: / fd/följt av ett URI-sökvägssegment.|

### <a name="section-name-nodeproperties"></a>Avsnittsnamn: NodeProperties
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| PropertyGroup |NodePropertyCollectionMap |En samling sträng nyckel / värde-par för egenskaper för en nod. |

### <a name="section-name-nodecapacities"></a>Avsnittsnamn: NodeCapacities
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| PropertyGroup |NodeCapacityCollectionMap |En samling av noden kapacitet för olika mått. |

### <a name="section-name-fabricnode"></a>Avsnittsnamn: FabricNode
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| StartApplicationPortRange |Int, standardvärdet är 0 |Starta programmet portar som hanteras av värd undersystemet. Krävs om EndpointFilteringEnabled är true i värd. |
| EndApplicationPortRange |Int, standardvärdet är 0 |Slut (inte inklusiva) program-portar som hanteras av värd undersystemet. Krävs om EndpointFilteringEnabled är true i värd. |
| ClusterX509StoreName |sträng, standardvärdet är ”Mina” |Namnet på X.509-certifikatarkiv som innehåller klustret certifikat för säker kommunikation inom klustret. |
| ClusterX509FindType |strängen, standard är ”FindByThumbprint” |Anger hur du söker efter klustret certifikat i arkivet som anges av ClusterX509StoreName stöds värdena: ”FindByThumbprint”; ”FindBySubjectName” med ”FindBySubjectName”; När det finns flera matchningar; med längst giltighetstid används. |
| ClusterX509FindValue |sträng, standardvärdet är ”” |Sök filtervärdet som används för att hitta certifikat för klustret. |
| ClusterX509FindValueSecondary |sträng, standardvärdet är ”” |Sök filtervärdet som används för att hitta certifikat för klustret. |
| ServerAuthX509StoreName |sträng, standardvärdet är ”Mina” |Namnet på X.509-certifikatarkiv som innehåller servercertifikat för förrätter service. |
| ServerAuthX509FindType |strängen, standard är ”FindByThumbprint” |Anger hur du söker efter servercertifikat i arkivet som anges av ServerAuthX509StoreName stöds värde: FindByThumbprint; FindBySubjectName. |
| ServerAuthX509FindValue |sträng, standardvärdet är ”” |Sök filtervärdet som används för att hitta servercertifikat. |
| ServerAuthX509FindValueSecondary |sträng, standardvärdet är ”” |Sök filtervärdet som används för att hitta servercertifikat. |
| ClientAuthX509StoreName |sträng, standardvärdet är ”Mina” |Namnet på X.509-certifikatarkiv som innehåller standard administratörsroll FabricClient-certifikat. |
| ClientAuthX509FindType |strängen, standard är ”FindByThumbprint” |Anger hur du söker efter certifikat i arkivet som anges av ClientAuthX509StoreName stöds värde: FindByThumbprint; FindBySubjectName. |
| ClientAuthX509FindValue |sträng, standardvärdet är ”” | Sök filtervärdet som används för att hitta certifikatet för standard administratörsroll FabricClient. |
| ClientAuthX509FindValueSecondary |sträng, standardvärdet är ”” |Sök filtervärdet som används för att hitta certifikatet för standard administratörsroll FabricClient. |
| UserRoleClientX509StoreName |sträng, standardvärdet är ”Mina” |Namnet på X.509-certifikatarkiv som innehåller certifikat för standardanvändarrollen FabricClient. |
| UserRoleClientX509FindType |strängen, standard är ”FindByThumbprint” |Anger hur du söker efter certifikat i arkivet som anges av UserRoleClientX509StoreName stöds värde: FindByThumbprint; FindBySubjectName. |
| UserRoleClientX509FindValue |sträng, standardvärdet är ”” |Sök filtervärdet som används för att hitta certifikatet för standardanvändarrollen FabricClient. |
| UserRoleClientX509FindValueSecondary |sträng, standardvärdet är ”” |Sök filtervärdet som används för att hitta certifikatet för standardanvändarrollen FabricClient. |

### <a name="section-name-paas"></a>Avsnittsnamn: Paas
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| ClusterId |sträng, standardvärdet är ”” |X509 certifikatarkivet användas av fabric för konfiguration av skydd. |

### <a name="section-name-fabrichost"></a>Avsnittsnamn: FabricHost
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| StopTimeout |Tid i sekunder, standard är 300 |Ange tidsintervall i sekunder. Tidsgränsen för aktivering i värdtjänsten. Inaktivering och uppgradering. |
| StartTimeout |Tid i sekunder, standard är 300 |Ange tidsintervall i sekunder. Timeout för fabricactivationmanager start. |
| ActivationRetryBackoffInterval |Tid i sekunder, standardvärdet är 5 |Ange tidsintervall i sekunder. Backoff intervall på varje Aktiveringsfel; på varje kontinuerlig Aktiveringsfel systemet försöker aktivering för upp till MaxActivationFailureCount. Återförsöksintervall på varje försök är en produkt från kontinuerlig Aktiveringsfel och inte aktiveringsintervallet. |
| ActivationMaxRetryInterval |Tid i sekunder, standard är 300 |Ange tidsintervall i sekunder. Max återförsöksintervall för aktivering. På varje kontinuerlig fel beräknas återförsöksintervall som Min (ActivationMaxRetryInterval; Kontinuerlig Felberäkning * ActivationRetryBackoffInterval). |
| ActivationMaxFailureCount |Int, standardvärdet är 10 |Detta är det högsta antalet som systemet ska misslyckade försök aktivera igen innan den ger upp. |
| EnableServiceFabricAutomaticUpdates |Bool, standard är FALSKT |Detta är att aktivera fabric automatiska uppdateringar via Windows Update. |
| EnableServiceFabricBaseUpgrade |Bool, standard är FALSKT |Detta är att aktivera grundläggande uppdatering för servern. |
| EnableRestartManagement |Bool, standard är FALSKT |Detta är att starta om servern. |


### <a name="section-name-failovermanager"></a>Avsnittsnamn: FailoverManager
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| UserReplicaRestartWaitDuration |Tid i sekunder är standard 60,0 * 30 |Ange tidsintervall i sekunder. När en beständiga replik kraschar; Windows Fabric väntar varaktigheten för repliken för att gå tillbaka innan du skapar nya ersättning repliker (som kräver en kopia av tillståndet). |
| QuorumLossWaitDuration |Tid i sekunder är standardvärdet MaxValue |Ange tidsintervall i sekunder. Det här är den maximala längd som tillåter vi en partition är i tillståndet förlorar kvorum. Om partitionen är fortfarande i kvorumförlust efter varaktigheten; partitionen återställs från förlorar kvorum genom att beakta nedåt replikerna som förlorad. Observera att detta potentiellt kan medföra dataförluster. |
| UserStandByReplicaKeepDuration |Tid i sekunder är standardvärdet 3600.0 * 24 * 7 |Ange tidsintervall i sekunder. När en beständiga replik kommer tillbaka från ett nedåt tillstånd. den kan ha redan ersatts. Den här timern anger hur länge FM behåller vänteläge repliken innan du tar bort den. |
| UserMaxStandByReplicaCount |Int, standardvärdet är 1 |Standard max antal StandBy-repliker som systemet håller för användaren tjänster. |

### <a name="section-name-namingservice"></a>Avsnittsnamn: NamingService
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| TargetReplicaSetSize |Int, standard är 7 |Antal replik anger för varje partition för arkivet Naming Service. Öka antalet replikuppsättningar ger en högre tillförlitlighet för informationen i arkivet Naming; minska ändringen att informationen går förlorade på grund av nodfel; kostnad för ökad belastning på Windows Fabric och hur lång tid tar det för att utföra uppdateringar på namngivning data.|
|MinReplicaSetSize | Int, standardinställningen är 3 | Det minsta antalet namngivningstjänst repliker som krävs för att skriva till att slutföra en uppdatering. Om det finns färre repliker än det aktiva i systemet systemets tillförlitlighet nekar uppdateringar till arkivet Naming förrän replikeringarna har återställts. Det här värdet ska aldrig vara mer än TargetReplicaSetSize. |
|ReplicaRestartWaitDuration | Tid i sekunder är standardvärdet (60,0 * 30)| Ange tidsintervall i sekunder. När en replik för namngivningstjänst kraschar; den här timern startas.  När den upphör FM börjar ersätta repliker som är nere (den inte ännu anser tappas bort). |
|QuorumLossWaitDuration | Tid i sekunder är standardvärdet MaxValue | Ange tidsintervall i sekunder. När en Naming Service hämtar i kvorumförlust; den här timern startas.  När den upphör att gälla beaktas FM nedåt repliker som gå förlorad. och försöka att återställa kvorum. Inte som den här kan resultera i förlust av data. |
|StandByReplicaKeepDuration | Tid i sekunder är standard 3600.0 * 2 | Ange tidsintervall i sekunder. När en namngivningstjänst repliker gå tillbaka från ett nedåt tillstånd. den kan ha redan ersatts.  Den här timern anger hur länge FM behåller vänteläge repliken innan du tar bort den. |
|PlacementConstraints | sträng, standardvärdet är ”” | Placering begränsningen för namngivning av tjänsten. |
|ServiceDescriptionCacheLimit | Int, standardvärdet är 0 | Det maximala antalet poster som underhålls i LRU tjänstens beskrivning cache på lagringstjänst Naming (Ange 0 för ingen gräns). |
|RepairInterval | Tid i sekunder, standardvärdet är 5 | Ange tidsintervall i sekunder. Intervall som reparera namngivning inkonsekvens mellan myndigheten ägare och namnägare startar. |
|MaxNamingServiceHealthReports | Int, standardvärdet är 10 | Det maximala antalet långsam åtgärder som lagrar Naming service rapporter feltillstånd i taget. Om 0; alla åtgärder för långsam skickas. |
| MaxMessageSize |Int, standardvärdet är 4*1024*1024 |Maximal storlek för noden klientkommunikation när du använder naming. DOS-attack lindra; Standardvärdet är 4MB. |
| MaxFileOperationTimeout |Tid i sekunder, standardvärdet är 30 |Ange tidsintervall i sekunder. Maximal tidsgräns som tillåts för filåtgärd store-tjänsten. Ange en längre tidsgräns nekas. |
| MaxOperationTimeout |Tid i sekunder, standard är 600 |Ange tidsintervall i sekunder. Maximal tidsgräns som tillåts för Klientåtgärder. Ange en längre tidsgräns nekas. |
| MaxClientConnections |Int, standardvärdet är 1000 |Maximalt tillåtna antalet klientanslutningar per gateway. |
| ServiceNotificationTimeout |Tid i sekunder, standardvärdet är 30 |Ange tidsintervall i sekunder. Den tidsgräns som används när tjänstmeddelanden levereras till klienten. |
| MaxOutstandingNotificationsPerClient |Int, standardvärdet är 1000 |Det maximala antalet väntande meddelanden innan en klientregistrering stängs framtvingar av gateway. |
| MaxIndexedEmptyPartitions |Int, standardvärdet är 1000 |Det maximala antalet tomma partitioner som ska vara indexera i notification-cache för att synkronisera återanslutning klienter. Eventuella tomma partitioner ovanför det här numret tas bort från indexet i stigande ordning för sökning version. Ansluta klienter kan fortfarande synkronisera och ta emot uppdateringar missade tom partition. men synkroniseringsprotokollet blir dyrare. |
| GatewayServiceDescriptionCacheLimit |Int, standardvärdet är 0 |Det maximala antalet poster som underhålls i LRU tjänstens beskrivning cache på Namngivningsgateway (Ange 0 för ingen gräns). |
| PartitionCount |Int, standardinställningen är 3 |Antalet partitioner för tjänsten Naming lagra som ska skapas. Varje partition äger en enda partitionsnyckel som motsvarar dess index. så partitionsnycklar [0; PartitionCount) finns. Öka antalet namngivningstjänst partitioner ökar skalan som Naming Service kan utföra genom att minska den genomsnittliga mängden data från alla stödjande repliker ange; kostnad ökad användning av resurser (eftersom PartitionCount * ReplicaSetSize service repliker måste underhållas).|

### <a name="section-name-runas"></a>Avsnittsnamn: RunAs
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| RunAsAccountName |sträng, standardvärdet är ”” |Anger namnet på RunAs-kontot. Detta krävs endast för ”DomainUser” eller ”ManagedServiceAccount” typen. Giltiga värden är ”domän\användare” eller ”user@domain”. |
|RunAsAccountType|sträng, standardvärdet är ”” |Anger typen för RunAs-konto. Detta krävs för alla RunAs-avsnittet, giltiga värden är ”DomainUser/NetworkService/ManagedServiceAccount/LocalSystem”.|
|Lösenord|sträng, standardvärdet är ”” |Anger lösenordet för RunAs-konto. Detta krävs endast för typen ”DomainUser”. |

### <a name="section-name-runasfabric"></a>Avsnittsnamn: RunAs_Fabric
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| RunAsAccountName |sträng, standardvärdet är ”” |Anger namnet på RunAs-kontot. Detta krävs endast för ”DomainUser” eller ”ManagedServiceAccount” typen. Giltiga värden är ”domän\användare” eller ”user@domain”. |
|RunAsAccountType|sträng, standardvärdet är ”” |Anger typen för RunAs-konto. Detta krävs för alla RunAs-avsnittet, giltiga värden är ”Lokalanvändare/DomainUser/NetworkService/ManagedServiceAccount/LocalSystem”. |
|Lösenord|sträng, standardvärdet är ”” |Anger lösenordet för RunAs-konto. Detta krävs endast för typen ”DomainUser”. |

### <a name="section-name-runashttpgateway"></a>Avsnittsnamn: RunAs_HttpGateway
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| RunAsAccountName |sträng, standardvärdet är ”” |Anger namnet på RunAs-kontot. Detta krävs endast för ”DomainUser” eller ”ManagedServiceAccount” typen. Giltiga värden är ”domän\användare” eller ”user@domain”. |
|RunAsAccountType|sträng, standardvärdet är ”” |Anger typen för RunAs-konto. Detta krävs för alla RunAs-avsnittet, giltiga värden är ”Lokalanvändare/DomainUser/NetworkService/ManagedServiceAccount/LocalSystem”. |
|Lösenord|sträng, standardvärdet är ”” |Anger lösenordet för RunAs-konto. Detta krävs endast för typen ”DomainUser”. |

### <a name="section-name-runasdca"></a>Avsnittsnamn: RunAs_DCA
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| RunAsAccountName |sträng, standardvärdet är ”” |Anger namnet på RunAs-kontot. Detta krävs endast för ”DomainUser” eller ”ManagedServiceAccount” typen. Giltiga värden är ”domän\användare” eller ”user@domain”. |
|RunAsAccountType|sträng, standardvärdet är ”” |Anger typen för RunAs-konto. Detta krävs för alla RunAs-avsnittet, giltiga värden är ”Lokalanvändare/DomainUser/NetworkService/ManagedServiceAccount/LocalSystem”. |
|Lösenord|sträng, standardvärdet är ”” |Anger lösenordet för RunAs-konto. Detta krävs endast för typen ”DomainUser”. |

### <a name="section-name-httpgateway"></a>Avsnittsnamn: HttpGateway
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
|IsEnabled|Bool, standard är FALSKT | Aktiverar eller inaktiverar httpgateway. Httpgateway är inaktiverad som standard och den här konfigurationen måste anges för att aktivera den. |
|ActiveListeners |Uint, standardvärdet är 50 | Antalet läsningar publicera till kön för http-servern. Detta anger antalet samtidiga begäranden som kan betjänas av HttpGateway. |
|MaxEntityBodySize |Uint, standardvärdet är 4194304 |  Ger den maximala storleken för brödtext som kan förväntas från en http-begäran. Standardvärdet är 4MB. Httpgateway misslyckas en begäran om den har en uppsättning storlek > det här värdet. Minsta skrivskyddade segmentstorleken är 4096 byte. Så här måste vara > = 4096. |
|HttpGatewayHealthReportSendInterval |Tid i sekunder, standardvärdet är 30 | Ange tidsintervall i sekunder. Intervallet med vilken Http-Gateway skickar ackumulerade hälsa rapporterar till Health Manager. |

### <a name="section-name-ktllogger"></a>Avsnittsnamn: KtlLogger
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
|AutomaticMemoryConfiguration |Int, standardvärdet är 1 | Flagga som anger om minnesinställningarna ska automatiskt och dynamiskt konfigureras. Om noll sedan konfigurationsinställningarna för minne används direkt och ändra inte baserat på villkor som system. Om en sedan minnesinställningarna konfigureras automatiskt och kan ändras baserat på villkor som system. |
|WriteBufferMemoryPoolMinimumInKB |Int, standard är 8388608 |Antal KB ursprungligen allokera för skrivning buffert minnespoolen. Använd 0 för att ange någon gräns standard ska stämma överens med SharedLogSizeInMB nedan. |
|WriteBufferMemoryPoolMaximumInKB | Int, standardvärdet är 0 |Antal KB att skriva buffert minnespoolen att växa upp till. Använd 0 om du vill ange någon gräns. |
|MaximumDestagingWriteOutstandingInKB | Int, standardvärdet är 0 | Antal KB så att delade loggen att Avancera i dedikerade loggen. Använd 0 om du vill ange någon gräns.
|SharedLogPath |sträng, standardvärdet är ”” | Sökvägen och filnamnet för plats för delade loggen behållare. Använd ”” för att använda standardsökvägen under fabric-dataroten. |
|SharedLogId |sträng, standardvärdet är ”” |Unikt guid för delade loggen behållare. Använd ”” om du använder standardsökvägen under fabric-dataroten. |
|SharedLogSizeInMB |Int, standard är 8192 | Antal MB för att allokera i behållaren delade loggen. |

### <a name="section-name-applicationgatewayhttp"></a>Avsnittsnamn: ApplicationGateway/Http
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
|IsEnabled |Bool, standard är FALSKT | Aktiverar eller inaktiverar HttpApplicationGateway. HttpApplicationGateway är inaktiverad som standard och den här konfigurationen måste anges för att aktivera den. |
|NumberOfParallelOperations | Uint, standard är 5 000 | Antalet läsningar publicera till kön för http-servern. Detta anger antalet samtidiga begäranden som kan betjänas av HttpGateway. |
|DefaultHttpRequestTimeout |Tid i sekunder. Standardvärdet är 120 |Ange tidsintervall i sekunder.  Ger standardtimeout för begäranden för http-begäranden som bearbetas i HTTP-app gateway. |
|ResolveServiceBackoffInterval |Tid i sekunder, standardvärdet är 5 |Ange tidsintervall i sekunder.  Ger inte standardintervallet innan du försöker en misslyckad lösa tjänståtgärden. |
|BodyChunkSize |Uint, standard är 16384 |  Anger storleken på för segmentet i byte som används för att läsa innehållet. |
|GatewayAuthCredentialType |String, standard är ”ingen” | Anger typ av säkerhetsautentiseringsuppgifter som används på http-app gateway endpoint giltiga värden är ”ingen / X 509. |
|GatewayX509CertificateStoreName |sträng, standardvärdet är ”Mina” | Namnet på X.509-certifikatarkiv som innehåller certifikat för HTTP-app gateway. |
|GatewayX509CertificateFindType |strängen, standard är ”FindByThumbprint” | Anger hur du söker efter certifikat i arkivet som anges av GatewayX509CertificateStoreName stöds värde: FindByThumbprint; FindBySubjectName. |
|GatewayX509CertificateFindValue | sträng, standardvärdet är ”” | Sök filtervärdet som används för att hitta gateway-certifikat för HTTP-app. Det här certifikatet är konfigurerad för https-slutpunkt och kan också användas för att kontrollera identiteten på appen om det behövs av tjänsterna. FindValue slås upp första; och om som inte finns. FindValueSecondary slås upp. |
|GatewayX509CertificateFindValueSecondary | sträng, standardvärdet är ”” |Sök filtervärdet som används för att hitta gateway-certifikat för HTTP-app. Det här certifikatet är konfigurerad för https-slutpunkt och kan också användas för att kontrollera identiteten på appen om det behövs av tjänsterna. FindValue slås upp första; och om som inte finns. FindValueSecondary slås upp.|

### <a name="section-name-management"></a>Avsnittsnamn: Management
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| ImageStoreConnectionString |SecureString | Anslutningssträngen till roten för Avbildningsarkiv. |
| ImageStoreMinimumTransferBPS | Int, standard är 1024 |Den minsta överföringshastigheten mellan klustret och Avbildningsarkiv. Det här värdet används för att fastställa tidsgräns vid åtkomst till externa ImageStore. Ändra det här värdet bara om fördröjningen mellan klustret och Avbildningsarkiv är hög att ge mer tid för att klustret ska hämta från externa ImageStore. |
|AzureStorageMaxWorkerThreads | Int, standard är 25 | Maximalt antal arbetstrådar parallellt. |
|AzureStorageMaxConnections | Int, standard är 5 000 | Maximalt antal samtidiga anslutningar till azure-lagring. |
|AzureStorageOperationTimeout | Tid i sekunder är standardvärdet 6000 | Ange tidsintervall i sekunder. Tidsgränsen nåddes för åtgärden xstore skulle slutföras. |
|ImageCachingEnabled | Bool, standard är SANT | Den här konfigurationen gör att vi kan aktivera eller inaktivera cachelagring. |
|DisableChecksumValidation | Bool, standard är FALSKT | Den här konfigurationen gör att vi kan aktivera eller inaktivera validering av kontrollsumma under etableringen av programmet. |
|DisableServerSideCopy | Bool, standard är FALSKT | Den här konfigurationen aktiverar eller inaktiverar serverkopia av programpaket på ImageStore under etableringen av programmet. |

### <a name="section-name-healthmanagerclusterhealthpolicy"></a>Avsnittsnamn: HealthManager/ClusterHealthPolicy
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| ConsiderWarningAsError |Bool, standard är FALSKT |Klustret hälsoprincip för utvärdering: behandlas varningar som fel. |
| MaxPercentUnhealthyNodes | Int, standardvärdet är 0 |Klustret hälsoprincip för utvärdering: Maxprocent felaktiga noder som tillåts för att klustret ska felfri. |
| MaxPercentUnhealthyApplications | Int, standardvärdet är 0 |Klustret hälsoprincip för utvärdering: maximala procentandelen felaktiga program tillåts för att klustret ska felfritt. |
|MaxPercentDeltaUnhealthyNodes | Int, standardvärdet är 10 |Klustret uppgradera utvärdering hälsoprincip: Maxprocent delta felaktiga noder som tillåts för att klustret ska felfritt. |
|MaxPercentUpgradeDomainDeltaUnhealthyNodes | Int, standardvärdet är 15 |Klustret uppgradera utvärdering hälsoprincip: maximala procentandelen delta felaktiga noder i en uppgraderingsdomän tillåts för att klustret ska felfritt.|

### <a name="section-name-faultanalysisservice"></a>Avsnittsnamn: FaultAnalysisService
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| TargetReplicaSetSize |Int, standardvärdet är 0 |NOT_PLATFORM_UNIX_START TargetReplicaSetSize för FaultAnalysisService. |
| MinReplicaSetSize |Int, standardvärdet är 0 |MinReplicaSetSize för FaultAnalysisService. |
| ReplicaRestartWaitDuration |Tid i sekunder, standardvärdet är 60 minuter|Ange tidsintervall i sekunder. ReplicaRestartWaitDuration för FaultAnalysisService. |
| QuorumLossWaitDuration | Tid i sekunder är standardvärdet MaxValue |Ange tidsintervall i sekunder. QuorumLossWaitDuration för FaultAnalysisService. |
| StandByReplicaKeepDuration| Tid i sekunder är standardvärdet (60*24*7) minuter |Ange tidsintervall i sekunder. StandByReplicaKeepDuration för FaultAnalysisService. |
| PlacementConstraints | sträng, standardvärdet är ””| PlacementConstraints för FaultAnalysisService. |
| StoredActionCleanupIntervalInSeconds | Int, standard är 3 600 |Detta är hur ofta arkivet rensas.  Endast åtgärder i ett avslutat tillstånd; och som slutförts minst CompletedActionKeepDurationInSeconds sedan kommer att tas bort. |
| CompletedActionKeepDurationInSeconds | Int, standard är 604800 | Detta är ungefär hur lång tid att behålla åtgärder som är i ett avslutat tillstånd.  Detta beror också på StoredActionCleanupIntervalInSeconds; eftersom arbetet som rensning utförs endast på intervallet. 604800 är 7 dagar. |
| StoredChaosEventCleanupIntervalInSeconds | Int, standard är 3 600 |Detta är hur ofta arkivet kommer att granskas för rensning; Om antalet händelser som är mer än 30000; rensningen kommer startar. |

### <a name="section-name-filestoreservice"></a>Avsnittsnamn: FileStoreService
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| NamingOperationTimeout |Tid i sekunder, standardvärdet är 60 |Ange tidsintervall i sekunder. Tidsgräns för namngivning åtgärd utfördes. |
| QueryOperationTimeout | Tid i sekunder, standardvärdet är 60 |Ange tidsintervall i sekunder. Tidsgränsen för att utföra frågeåtgärden igen. |
| MaxCopyOperationThreads | Uint, standardvärdet är 0 | Det maximala antalet parallella filer som andra kan kopiera från primära. '0' == antal kärnor. |
| MaxFileOperationThreads | Uint, standardvärdet är 100 | Maximalt antal parallella trådar som får utföra FileOperations (kopiera/flytta) i den primära servern. '0' == antal kärnor. |
| MaxStoreOperations | Uint, standardvärdet är 4096 |Maximalt antal tillåtna på den primära parallella store transcation åtgärder. '0' == antal kärnor. |
| MaxRequestProcessingThreads | Uint, standardinställningen är 200 |Maximalt antal parallella trådar som tillåts att behandla begäranden i den primära servern. '0' == antal kärnor. |
| MaxSecondaryFileCopyFailureThreshold | Uint, standard är 25| Det maximala antalet filen kopia på sekundärt innan den ger upp. |
| AnonymousAccessEnabled | Bool, standard är SANT |Aktivera/inaktivera anonym åtkomst till FileStoreService resurser. |
| PrimaryAccountType | sträng, standardvärdet är ”” |Primärt AccountType huvudkontots åtkomstkontrollistan i FileStoreService delar. |
| PrimaryAccountUserName | sträng, standardvärdet är ”” |Det primära kontot användarnamnet för huvudnamn för ACL i FileStoreService delar. |
| PrimaryAccountUserPassword | SecureString, standard är tom |Primär kontolösenordet huvudkontots åtkomstkontrollistan i FileStoreService delar. |
| FileStoreService | PrimaryAccountNTLMPasswordSecret | SecureString, standard är tom | Den hemlighet som lösenord som används som startvärde för samma lösenord när du använder NTLM-autentisering. |
| PrimaryAccountNTLMX509StoreLocation | strängen, standard är ”LocalMachine”| Lagringsplatsen för X509 certifikat som används för att generera HMAC på PrimaryAccountNTLMPasswordSecret när du använder NTLM-autentisering. |
| PrimaryAccountNTLMX509StoreName | strängen, standard är ”MY”| Store-namnet på X509 certifikat som används för att generera HMAC på PrimaryAccountNTLMPasswordSecret när du använder NTLM-autentisering. |
| PrimaryAccountNTLMX509Thumbprint | sträng, standardvärdet är ””|Tumavtrycket för X509 certifikat som används för att generera HMAC på PrimaryAccountNTLMPasswordSecret när du använder NTLM-autentisering. |
| SecondaryAccountType | sträng, standardvärdet är ””| Sekundärt AccountType huvudkontots åtkomstkontrollistan i FileStoreService delar. |
| SecondaryAccountUserName | sträng, standardvärdet är ””| Det sekundära kontot användarnamn huvudkontots åtkomstkontrollistan i FileStoreService delar. |
| SecondaryAccountUserPassword | SecureString, standard är tom |Sekundär kontolösenordet huvudkontots åtkomstkontrollistan i FileStoreService delar.  |
| SecondaryAccountNTLMPasswordSecret | SecureString, standard är tom | Den hemlighet som lösenord som används som startvärde för samma lösenord när du använder NTLM-autentisering. |
| SecondaryAccountNTLMX509StoreLocation | strängen, standard är ”LocalMachine” |Lagringsplatsen för X509 certifikat som används för att generera HMAC på SecondaryAccountNTLMPasswordSecret när du använder NTLM-autentisering. |
| SecondaryAccountNTLMX509StoreName | strängen, standard är ”MY” |Store-namnet på X509 certifikat som används för att generera HMAC på SecondaryAccountNTLMPasswordSecret när du använder NTLM-autentisering. |
| SecondaryAccountNTLMX509Thumbprint | sträng, standardvärdet är ””| Tumavtrycket för X509 certifikat som används för att generera HMAC på SecondaryAccountNTLMPasswordSecret när du använder NTLM-autentisering. |

### <a name="section-name-imagestoreservice"></a>Avsnittsnamn: ImageStoreService
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| Enabled |Bool, standard är FALSKT |Flaggan aktiverad för ImageStoreService. |
| TargetReplicaSetSize | Int, standard är 7 |TargetReplicaSetSize för ImageStoreService. |
| MinReplicaSetSize | Int, standardinställningen är 3 |MinReplicaSetSize för ImageStoreService. |
| ReplicaRestartWaitDuration | Tid i sekunder är standard 60,0 * 30 | Ange tidsintervall i sekunder. ReplicaRestartWaitDuration för ImageStoreService. |
| QuorumLossWaitDuration | Tid i sekunder är standardvärdet MaxValue | Ange tidsintervall i sekunder. QuorumLossWaitDuration för ImageStoreService. |
| StandByReplicaKeepDuration | Tid i sekunder är standard 3600.0 * 2 | Ange tidsintervall i sekunder. StandByReplicaKeepDuration för ImageStoreService. |
| PlacementConstraints | sträng, standardvärdet är ”” | PlacementConstraints för ImageStoreService. |
| ClientUploadTimeout | Tid i sekunder är standardvärdet 1 800 |Ange tidsintervall i sekunder. Värdet för tidsgränsen för begäran om översta överför att Image Store-tjänsten. |
| ClientCopyTimeout | Tid i sekunder är standardvärdet 1 800 | Ange tidsintervall i sekunder. Värdet för tidsgränsen för begäran om översta kopia att Image Store-tjänsten. |
| ClientDownloadTimeout | Tid i sekunder är standardvärdet 1 800 | Ange tidsintervall i sekunder. Timeout-värdet för högsta nivån hämtningsbegäran till Image Store-tjänsten |
| ClientListTimeout | Tid i sekunder, standard är 600 | Ange tidsintervall i sekunder. Värdet för tidsgränsen för begäran om översta listan att Image Store-tjänsten. |
| ClientDefaultTimeout | Tid i sekunder, standard är 180 | Ange tidsintervall i sekunder. Timeout-värdet för alla icke-överför/icke-hämtningsbegäranden (t.ex. finns och ta bort) att Image Store-tjänsten. |

### <a name="section-name-imagestoreclient"></a>Avsnittsnamn: ImageStoreClient
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| ClientUploadTimeout |Tid i sekunder är standardvärdet 1 800 | Ange tidsintervall i sekunder. Värdet för tidsgränsen för begäran om översta överför att Image Store-tjänsten. |
| ClientCopyTimeout | Tid i sekunder är standardvärdet 1 800 | Ange tidsintervall i sekunder. Värdet för tidsgränsen för begäran om översta kopia att Image Store-tjänsten. |
|ClientDownloadTimeout | Tid i sekunder är standardvärdet 1 800 | Ange tidsintervall i sekunder. Timeout-värdet för högsta nivån hämtningsbegäran till Image Store-tjänsten. |
|ClientListTimeout | Tid i sekunder, standard är 600 |Ange tidsintervall i sekunder. Värdet för tidsgränsen för begäran om översta listan att Image Store-tjänsten. |
|ClientDefaultTimeout | Tid i sekunder, standard är 180 | Ange tidsintervall i sekunder. Timeout-värdet för alla icke-överför/icke-hämtningsbegäranden (t.ex. finns och ta bort) att Image Store-tjänsten. |

### <a name="section-name-tokenvalidationservice"></a>Avsnittsnamn: TokenValidationService
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| Leverantörer |strängen, standard är ”DSTS” |Kommaavgränsad lista över token validering leverantörer att aktivera (giltigt providers är: DSTS; AAD). För närvarande bara en enskild provider kan aktiveras när som helst. |

### <a name="section-name-upgradeorchestrationservice"></a>Avsnittsnamn: UpgradeOrchestrationService
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| TargetReplicaSetSize |Int, standardvärdet är 0 |TargetReplicaSetSize för UpgradeOrchestrationService. |
| MinReplicaSetSize |Int, standardvärdet är 0 | MinReplicaSetSize för UpgradeOrchestrationService.
| ReplicaRestartWaitDuration | Tid i sekunder, standardvärdet är 60 minuter| Ange tidsintervall i sekunder. ReplicaRestartWaitDuration för UpgradeOrchestrationService. |
| QuorumLossWaitDuration | Tid i sekunder är standardvärdet MaxValue | Ange tidsintervall i sekunder. QuorumLossWaitDuration för UpgradeOrchestrationService. |
| StandByReplicaKeepDuration | Tid i sekunder, standardvärdet är 60*24*7 minuter | Ange tidsintervall i sekunder. StandByReplicaKeepDuration för UpgradeOrchestrationService. |
| PlacementConstraints | sträng, standardvärdet är ”” | PlacementConstraints för UpgradeOrchestrationService. |
| AutoupgradeEnabled | Bool, standard är SANT | Automatisk avsökning och uppgraderingsåtgärden baserat på mål-tillståndsfil. |
| UpgradeApprovalRequired | Bool, standard är FALSKT | Ställa in att koden uppgradering kräver administratörsgodkännande innan du fortsätter. |

### <a name="section-name-upgradeservice"></a>Avsnittsnamn: UpgradeService
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| PlacementConstraints |sträng, standardvärdet är ”” |PlacementConstraints för uppgradering. |
| TargetReplicaSetSize | Int, standardinställningen är 3 | TargetReplicaSetSize för UpgradeService. |
| MinReplicaSetSize | Int, standardvärdet är 2 | MinReplicaSetSize för UpgradeService. |
| CoordinatorType | strängen, standard är ”WUTest”| CoordinatorType för UpgradeService. |
| BaseUrl | sträng, standardvärdet är ”” |BaseUrl för UpgradeService. |
| ClusterId | sträng, standardvärdet är ”” | ClusterId för UpgradeService. |
| X509StoreName | sträng, standardvärdet är ”Mina”| X509StoreName för UpgradeService. |
| X509StoreLocation | sträng, standardvärdet är ”” | X509StoreLocation för UpgradeService. |
| X509FindType | sträng, standardvärdet är ””| X509FindType för UpgradeService. |
| X509FindValue | sträng, standardvärdet är ”” | X509FindValue för UpgradeService. |
| X509SecondaryFindValue | sträng, standardvärdet är ”” | X509SecondaryFindValue för UpgradeService. |
| OnlyBaseUpgrade | Bool, standard är FALSKT | OnlyBaseUpgrade för UpgradeService. |
| TestCabFolder | sträng, standardvärdet är ”” | TestCabFolder för UpgradeService. |

### <a name="section-name-securityclientaccess"></a>Avsnittsnamn: Säkerhet/ClientAccess
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| CreateName |String, standard är ”Admin” |Konfiguration för skapande av namngivnings-URI. |
| DeleteName |String, standard är ”Admin” |Säkerhetskonfiguration för borttagning av namngivnings-URI. |
| PropertyWriteBatch |String, standard är ”Admin” |Säkerhetskonfiguration för Naming egenskapen skrivåtgärder. |
| CreateService |String, standard är ”Admin” | Säkerhetskonfiguration för att skapa tjänster. |
| CreateServiceFromTemplate |String, standard är ”Admin” |Säkerhetskonfiguration för att skapa från mall. |
| UpdateService |String, standard är ”Admin” |Säkerhetskonfiguration för uppdateringar av tjänsten. |
| DeleteService  |String, standard är ”Admin” |Säkerhetskonfiguration för borttagning av tjänsten. |
| ProvisionApplicationType |String, standard är ”Admin” | Säkerhetskonfiguration för programmet typen etablering. |
| CreateApplication |String, standard är ”Admin” | Konfiguration för skapande av programmet. |
| DeleteApplication |String, standard är ”Admin” | Säkerhetskonfiguration för borttagning av programmet. |
| UpgradeApplication |String, standard är ”Admin” | Under start eller att avbryta programuppgraderingar säkerhetskonfiguration. |
| RollbackApplicationUpgrade |String, standard är ”Admin” | För att återställa programuppgraderingar säkerhetskonfiguration. |
| UnprovisionApplicationType |String, standard är ”Admin” | Säkerhetskonfiguration för programmet typen avetablering. |
| MoveNextUpgradeDomain |String, standard är ”Admin” | Säkerhetskonfiguration för återupptar programuppgraderingar med en explicit uppgradera domän. |
| ReportUpgradeHealth |String, standard är ”Admin” | Säkerhetskonfiguration för återupptar programuppgraderingar med den aktuella uppgraderingen pågår. |
| ReportHealth |String, standard är ”Admin” | Säkerhetskonfiguration för rapportering hälsa. |
| ProvisionFabric |String, standard är ”Admin” | Säkerhetskonfiguration för MSI och/eller klusternamnresursen manifestet etablering. |
| UpgradeFabric |String, standard är ”Admin” | För att starta klusteruppgradering säkerhetskonfiguration. |
| RollbackFabricUpgrade |String, standard är ”Admin” | För att återställa klusteruppgradering säkerhetskonfiguration. |
| UnprovisionFabric |String, standard är ”Admin” | Säkerhetskonfiguration för MSI och/eller klusternamnresursen manifestet avetablering. |
| MoveNextFabricUpgradeDomain |String, standard är ”Admin” | Säkerhetskonfiguration för återupptar klusteruppgradering med en explicit uppgraderingen domän. |
| ReportFabricUpgradeHealth |String, standard är ”Admin” | Säkerhetskonfiguration för återupptar klusteruppgradering med den aktuella uppgraderingen pågår. |
| StartInfrastructureTask |String, standard är ”Admin” | Säkerhetskonfiguration för start av infrastrukturen. |
| FinishInfrastructureTask |String, standard är ”Admin” | Säkerhetskonfiguration för färdigställa infrastruktur uppgifter. |
| ActivateNode |String, standard är ”Admin” | Säkerhetskonfiguration för aktivering med en nod. |
| DeactivateNode |String, standard är ”Admin” | Säkerhetskonfiguration för en nod. |
| DeactivateNodesBatch |String, standard är ”Admin” | Konfiguration för flera noder. |
| RemoveNodeDeactivations |String, standard är ”Admin” | Säkerhetskonfiguration för Återför inaktivering på flera noder. |
| GetNodeDeactivationStatus |String, standard är ”Admin” | För att kontrollera status för inaktivering av säkerhetskonfiguration. |
| NodeStateRemoved |String, standard är ”Admin” | Säkerhetskonfiguration för rapportering nodens tillstånd tas bort. |
| RecoverPartition |String, standard är ”Admin” | Säkerhetskonfiguration för återställning av en partition. |
| RecoverPartitions |String, standard är ”Admin” | Säkerhetskonfiguration för återställning av partitioner. |
| RecoverServicePartitions |String, standard är ”Admin” | Säkerhetskonfiguration för att återskapa partitioner för tjänsten. |
| RecoverSystemPartitions |String, standard är ”Admin” | För att återställa tjänsten systempartitionerna säkerhetskonfiguration. |
| ReportFault |String, standard är ”Admin” | Säkerhetskonfiguration som rapporterade fel. |
| InvokeInfrastructureCommand |String, standard är ”Admin” | Säkerhetskonfiguration för kommandon för hantering av infrastruktur-aktivitet. |
| FileContent |String, standard är ”Admin” | Säkerhetskonfiguration för avbildningen lagra klienten filöverföring (extern till klustret). |
| FileDownload |String, standard är ”Admin” | Säkerhetskonfiguration för image store klienten filen download inleda (extern till klustret). |
| InternalList |String, standard är ”Admin” | Säkerhetskonfiguration för avbildningen lagra filen listan klientåtgärden (intern). |
| Ta bort |String, standard är ”Admin” | Säkerhetskonfiguration för avbildningen lagra ta bort klientåtgärden. |
| Ladda upp |String, standard är ”Admin” | Säkerhetskonfiguration för avbildningen lagra Överföringsåtgärden för klienten. |
| GetStagingLocation |String, standard är ”Admin” | Säkerhetskonfiguration för avbildningen lagra klienten mellanlagring plats hämtning. |
| GetStoreLocation |String, standard är ”Admin” | Säkerhetskonfiguration för avbildningen lagra klienten store plats hämtning. |
| NodeControl |String, standard är ”Admin” | Konfiguration för att starta; Stoppa; och starta om noder. |
| CodePackageControl |String, standard är ”Admin” | Konfiguration för att starta om koden paket. |
| UnreliableTransportControl |String, standard är ”Admin” | Instabilt Transport för att lägga till och ta bort beteenden. |
| MoveReplicaControl |String, standard är ”Admin” | Flytta replik. |
| PredeployPackageToNode |String, standard är ”Admin” | Hjälp av noggrann api. |
| StartPartitionDataLoss |String, standard är ”Admin” | Startar förlust av data på en partition. |
| StartPartitionQuorumLoss |String, standard är ”Admin” | Startar förlorar kvorum på en partition. |
| StartPartitionRestart |String, standard är ”Admin” | Startar samtidigt om vissa eller alla repliker för en partition. |
| CancelTestCommand |String, standard är ”Admin” | Avbryter en specifik TestCommand - om det är aktiverade. |
| StartChaos |String, standard är ”Admin” | Startar Chaos - om det inte redan har startats. |
| StopChaos |String, standard är ”Admin” | Stoppar Chaos - om den har startats. |
| StartNodeTransition |String, standard är ”Admin” | Konfiguration för att starta en nod övergång. |
| StartClusterConfigurationUpgrade |String, standard är ”Admin” | Startar StartClusterConfigurationUpgrade på en partition. |
| GetUpgradesPendingApproval |String, standard är ”Admin” | Startar GetUpgradesPendingApproval på en partition. |
| StartApprovedUpgrades |String, standard är ”Admin” | Startar StartApprovedUpgrades på en partition. |
| Pinga |strängen, standard är ”Admin\|\|Användaren ” | Säkerhetskonfiguration för klienten ping. |
| Fråga |strängen, standard är ”Admin\|\|Användaren ” | Säkerhetskonfiguration för frågor. |
| NameExists |strängen, standard är ”Admin\|\|Användaren ” | Säkerhetskonfiguration namngivnings-URI befintliga kontrollerar. |
| EnumerateSubnames |strängen, standard är ”Admin\|\|Användaren ” | Säkerhetskonfiguration för namngivnings-URI-uppräkningen. |
| EnumerateProperties |strängen, standard är ”Admin\|\|Användaren ” | Säkerhetskonfiguration för namngivning av egenskapen uppräkningen. |
| PropertyReadBatch |strängen, standard är ”Admin\|\|Användaren ” | Säkerhetskonfiguration för Naming egenskapen läsåtgärder. |
| GetServiceDescription |strängen, standard är ”Admin\|\|Användaren ” | Säkerhetskonfiguration för lång avsökning tjänstmeddelanden och läsa beskrivningar. |
| ResolveService |strängen, standard är ”Admin\|\|Användaren ” | Säkerhetskonfiguration för matchning av klagomål-baserad tjänst. |
| ResolveNameOwner |strängen, standard är ”Admin\|\|Användaren ” | Säkerhetskonfiguration för att lösa namngivnings-URI-ägare. |
| ResolvePartition |strängen, standard är ”Admin\|\|Användaren ” | Säkerhetskonfiguration för att lösa systemtjänster. |
| ServiceNotifications |strängen, standard är ”Admin\|\|Användaren ” | Säkerhetskonfiguration för händelsebaserat tjänstmeddelanden. |
| PrefixResolveService |strängen, standard är ”Admin\|\|Användaren ” | Säkerhetskonfiguration för matchning av klagomål-baserad tjänst prefix. |
| GetUpgradeStatus |strängen, standard är ”Admin\|\|Användaren ” | Säkerhetskonfiguration för avsökning av uppgraderingsstatus för programmet. |
| GetFabricUpgradeStatus |strängen, standard är ”Admin\|\|Användaren ” | Säkerhetskonfiguration för avsökning av uppgraderingsstatus för klustret. |
| InvokeInfrastructureQuery |strängen, standard är ”Admin\|\|Användaren ” | Säkerhetskonfiguration för frågor infrastrukturen. |
| Visa lista |strängen, standard är ”Admin\|\|Användaren ” | Säkerhetskonfiguration för avbildningen lagra klientåtgärden filen lista. |
| ResetPartitionLoad |strängen, standard är ”Admin\|\|Användaren ” | Säkerhetskonfiguration för återställning för en failoverUnit. |
| ToggleVerboseServicePlacementHealthReporting | strängen, standard är ”Admin\|\|Användaren ” | Säkerhetskonfiguration för växla utförlig ServicePlacement HealthReporting. |
| GetPartitionDataLossProgress | strängen, standard är ”Admin\|\|Användaren ” | Hämtar status för en api-anrop invoke data går förlorade. |
| GetPartitionQuorumLossProgress | strängen, standard är ”Admin\|\|Användaren ” | Hämtar status för ett invoke kvorum förlust api-anrop. |
| GetPartitionRestartProgress | strängen, standard är ”Admin\|\|Användaren ” | Hämtar status för en omstart partition api-anrop. |
| GetChaosReport | strängen, standard är ”Admin\|\|Användaren ” | Hämtar status för Chaos inom ett angivet tidsintervall. |
| GetNodeTransitionProgress | strängen, standard är ”Admin\|\|Användaren ” | Säkerhetskonfiguration för att hämta pågår på en nod övergång. |
| GetClusterConfigurationUpgradeStatus | strängen, standard är ”Admin\|\|Användaren ” | Startar GetClusterConfigurationUpgradeStatus på en partition. |
| GetClusterConfiguration | strängen, standard är ”Admin\|\|Användaren ” | Startar GetClusterConfiguration på en partition. |

### <a name="section-name-reconfigurationagent"></a>Avsnittsnamn: ReconfigurationAgent
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| ApplicationUpgradeMaxReplicaCloseDuration | Tid i sekunder är standardvärdet 900 |Ange tidsintervall i sekunder. Den varaktighet som systemet ska vänta innan försöket avbryts värdar som har repliker som fastnat i Stäng. |
| ServiceApiHealthDuration | Tid i sekunder, standardinställningen är 30 minuter | Ange tidsintervall i sekunder. ServiceApiHealthDuration definierar hur länge vi vänta på en API för tjänsten ska köras innan vi rapporterar den feltillstånd. |
| ServiceReconfigurationApiHealthDuration | Tid i sekunder, standardvärdet är 30 | Ange tidsintervall i sekunder. ServiceReconfigurationApiHealthDuration definierar hur länge den innan en tjänst i omkonfiguration rapporteras som ohälsosam. |
| PeriodicApiSlowTraceInterval | Tid i sekunder, standardvärdet är 5 minuter | Ange tidsintervall i sekunder. PeriodicApiSlowTraceInterval definierar intervallet över vilket långsam API-anrop ska fångas av API-Övervakare. |
| NodeDeactivationMaxReplicaCloseDuration | Tid i sekunder är standardvärdet 900 | Ange tidsintervall i sekunder. Den maximala tiden att vänta innan en tjänstevärd som blockerar nodinaktiveringen. |
| FabricUpgradeMaxReplicaCloseDuration | Tid i sekunder är standardvärdet 900 | Ange tidsintervall i sekunder. Maximal varaktighet RA ska vänta innan tjänstvärden replik som inte stängs. |
| IsDeactivationInfoEnabled | Bool, standard är SANT | Anger om RA ska använda inaktiveringen info för att utföra primära ny val för nya kluster med den här konfigurationen ska anges till true för befintliga kluster som har uppgraderats ta se viktig information om hur du aktiverar detta. |

### <a name="section-name-placementandloadbalancing"></a>Avsnittsnamn: PlacementAndLoadBalancing
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| TraceCRMReasons |Bool, standard är SANT |Anger om du vill spåra orsakerna till CRM utfärdat förflyttningar operativa händelser-kanalen. |
| ValidatePlacementConstraint | Bool, standard är SANT | Anger huruvida PlacementConstraint-uttrycket för en tjänst verifieras när en tjänst ServiceDescription uppdateras. |
| PlacementConstraintValidationCacheSize | Int, standard är 10 000 | Begränsar storleken på den tabell som används för snabb validering och cachelagring av placering av begränsningsuttryck. |
|VerboseHealthReportLimit | Int, standardvärdet är 20 | Definierar hur många gånger som en replik har gå Ej placerade innan en varning om hälsotillstånd rapporteras för det (om utförlig hälsa reporting är aktiverad). |
|ConstraintViolationHealthReportLimit | Int, standardvärdet är 50 | Definierar hur många gånger som villkoret brott mot replik måste vara beständigt olöst innan diagnostik utförs och hälsorapporter släpps. |
|DetailedConstraintViolationHealthReportLimit | Int, standardinställningen är 200 | Definierar hur många gånger som villkoret brott mot replik måste beständigt olöst innan diagnostik utförs och detaljerade rapporter släpps hälsa. |
|DetailedVerboseHealthReportLimit | Int, standardinställningen är 200 | Definierar hur många gånger som en replik av en Ej placerade måste beständigt Ej placerade innan detaljerad hälsorapporter släpps. |
|ConsecutiveDroppedMovementsHealthReportLimit | Int, standardvärdet är 20 | Definierar hur många gånger i följd som utfärdats ResourceBalancer förflyttningar ignoreras innan diagnostik utförs och hälsovarningar släpps. Negativt: Inga varningar orsakat under det här villkoret. |
|DetailedNodeListLimit | Int, standardvärdet är 15 | Definierar antalet noder per begränsning för att inkludera innan trunkering i rapporter om Ej placerade replik. |
|DetailedPartitionListLimit | Int, standardvärdet är 15 | Definierar antalet partitioner per diagnostiska post för en begränsning att inkludera innan trunkering i diagnostik. |
|DetailedDiagnosticsInfoListLimit | Int, standardvärdet är 15 | Anger antal diagnostikposter (med detaljerad information) per begränsning för att inkludera innan trunkering i diagnostik.|
|PLBRefreshGap | Tid i sekunder är standardvärdet 1 | Ange tidsintervall i sekunder. Definierar den minsta mängden som måste passera innan PLB uppdaterar tillstånd igen. |
|MinPlacementInterval | Tid i sekunder är standardvärdet 1 | Ange tidsintervall i sekunder. Definierar den minsta mängden som måste passera innan två på varandra följande placering Avrundar. |
|MinConstraintCheckInterval | Tid i sekunder är standardvärdet 1 | Ange tidsintervall i sekunder. Definierar den minsta mängden som måste passera innan två på varandra följande begränsningen Kontrollera Avrundar. |
|MinLoadBalancingInterval | Tid i sekunder, standardvärdet är 5 | Ange tidsintervall i sekunder. Definierar den minsta mängden som måste passera innan två på varandra följande belastningsutjämning Avrundar. |
|BalancingDelayAfterNodeDown | Tid i sekunder, standardvärdet är 120 |Ange tidsintervall i sekunder. Starta inte NLB aktiviteter inom denna period efter en nod av händelsen. |
|BalancingDelayAfterNewNode | Tid i sekunder, standardvärdet är 120 |Ange tidsintervall i sekunder. Starta inte NLB aktiviteter inom den tiden när du lägger till en ny nod. |
|ConstraintFixPartialDelayAfterNodeDown | Tid i sekunder, standardvärdet är 120 | Ange tidsintervall i sekunder. Gör inte åtgärda FaultDomain och UpgradeDomain begränsningen överträdelser inom denna period efter en nod av händelsen. |
|ConstraintFixPartialDelayAfterNewNode | Tid i sekunder, standardvärdet är 120 | Ange tidsintervall i sekunder. DDo inte åtgärda FaultDomain och UpgradeDomain begränsningen överträdelser inom den tiden när du lägger till en ny nod. |
|GlobalMovementThrottleThreshold | Uint, standardvärdet är 1000 | Maximalt antal förflyttningar tillåten i fasen belastningsutjämning i det senaste intervall som anges av GlobalMovementThrottleCountingInterval. |
|GlobalMovementThrottleThresholdForPlacement | Uint, standardvärdet är 0 | Maximalt antal förflyttningar tillåten i placering fas i det senaste intervall som anges av GlobalMovementThrottleCountingInterval.0 anger att ingen gräns.|
|GlobalMovementThrottleThresholdForBalancing | Uint, standardvärdet är 0 | Maximalt antal förflyttningar tillåten i NLB fas i det senaste intervall som anges av GlobalMovementThrottleCountingInterval. 0 anger att ingen gräns. |
|GlobalMovementThrottleCountingInterval | Tid i sekunder, standard är 600 | Ange tidsintervall i sekunder. Ange längden på den senaste intervall som du vill spåra per domän replik förflyttningar (används tillsammans med GlobalMovementThrottleThreshold). Kan anges till 0 för att ignorera global begränsning helt och hållet. |
|MovementPerPartitionThrottleThreshold | Uint, standardvärdet är 50 | Ingen belastningsutjämning relaterade förflyttning utförs för en partition om antalet NLB relaterade förflyttningar för repliker av den aktuella partitionen har nått eller överskridit MovementPerFailoverUnitThrottleThreshold i det senaste intervall som anges av MovementPerPartitionThrottleCountingInterval. |
|MovementPerPartitionThrottleCountingInterval | Tid i sekunder, standard är 600 | Ange tidsintervall i sekunder. Ange längden på den senaste intervall som du vill spåra replik förflyttningar för varje partition (används tillsammans med MovementPerPartitionThrottleThreshold). |
|PlacementSearchTimeout | Tid i sekunder, standardvärdet är 0,5 | Ange tidsintervall i sekunder. När du monterar tjänster. Sök efter högst i långt innan ett resultat returneras. |
|UseMoveCostReports | Bool, standard är FALSKT | Instruerar LB att ignorera elementet kostnaden för funktionen bedömningsprofil; resulterande potentiellt stora antal flyttar för bättre belastningsutjämnade placering. |
|PreventTransientOvercommit | Bool, standard är FALSKT | Anger bör PLB omedelbart förlita dig på resurser som kan frigöras genom initierade flyttar. Som standard. PLB initiera flytta ut och flytta i på samma nod som kan skapa tillfälliga överanstränga. Om den här parametern anges till SANT förhindrar att de typer av overcommits och på begäran defragmentering (aka placementWithMove) kommer att inaktiveras. |
|InBuildThrottlingEnabled | Bool, standard är FALSKT | Bestämma om i build-begränsning är aktiverad. |
|InBuildThrottlingAssociatedMetric | sträng, standardvärdet är ”” | Det associerade måttnamnet för denna begränsning. |
|InBuildThrottlingGlobalMaxValue | Int, standardvärdet är 0 |Maxantalet i skapa-repliker tillåts globalt. |
|SwapPrimaryThrottlingEnabled | Bool, standard är FALSKT| Bestämma om byte-primära begränsningen är aktiverad. |
|SwapPrimaryThrottlingAssociatedMetric | sträng, standardvärdet är ””| Det associerade måttnamnet för denna begränsning. |
|SwapPrimaryThrottlingGlobalMaxValue | Int, standardvärdet är 0 | Maxantalet swap-primära repliker tillåts globalt. |
|PlacementConstraintPriority | Int, standardvärdet är 0 | Anger prioriteten för placering begränsning: 0: hårda; 1: mjuk; negativt: ignorera. |
|PreferredLocationConstraintPriority | Int, standardvärdet är 2| Anger prioriteten för önskade platsbegränsningen: 0: hårda; 1: mjuk; 2: optimering; negativt: ignorera |
|CapacityConstraintPriority | Int, standardvärdet är 0 | Anger prioriteten för kapacitet begränsning: 0: hårda; 1: mjuk; negativt: ignorera. |
|AffinityConstraintPriority | Int, standardvärdet är 0 | Anger prioriteten för mappning mellan begränsning: 0: hårda; 1: mjuk; negativt: ignorera. |
|FaultDomainConstraintPriority | Int, standardvärdet är 0 | Anger prioriteten för domänbegränsningar fel: 0: hårda; 1: mjuk; negativt: ignorera. |
|UpgradeDomainConstraintPriority | Int, standardvärdet är 1| Anger prioriteten för uppgraderingsdomänen begränsning: 0: hårda; 1: mjuk; negativt: ignorera. |
|ScaleoutCountConstraintPriority | Int, standardvärdet är 0 | Anger prioriteten för scaleout antal begränsning: 0: hårda; 1: mjuk; negativt: ignorera. |
|ApplicationCapacityConstraintPriority | Int, standardvärdet är 0 | Anger prioriteten för kapacitet begränsning: 0: hårda; 1: mjuk; negativt: ignorera. |
|MoveParentToFixAffinityViolation | Bool, standard är FALSKT | Inställning som avgör om överordnade repliker kan flyttas till åtgärda tillhörighet begränsningar.|
|MoveExistingReplicaForPlacement | Bool, standard är SANT |Inställningen som bestämmer om att flytta befintliga repliken under placeringen. |
|UseSeparateSecondaryLoad | Bool, standard är SANT | Inställningen som bestämmer om Använd olika sekundära belastningen. |
|PlaceChildWithoutParent | Bool, standard är SANT | Inställning som anger om underordnade tjänsten replik kan placeras om ingen överordnad replik är igång. |
|PartiallyPlaceServices | Bool, standard är SANT | Anger om alla repliker för tjänsten i klustret kommer att placeras ”allt eller inget” angivna begränsad lämplig noder för för dessa.|
|InterruptBalancingForAllFailoverUnitUpdates | Bool, standard är FALSKT | Anger om någon typ av failover unit update ska avbryta snabb eller långsam NLB kör. Med angivna ”false” NLB kör avbryts om FailoverUnit: är skapas eller tas bort; saknar repliker; ändra platsen för primära repliken eller ändrade antal repliker. NLB kör avbryts inte i andra fall - om FailoverUnit: har extra repliker; Ändra alla replik flagga; Ändra bara partition version eller annan. |

### <a name="section-name-security"></a>Avsnittsnamn: säkerhet
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| ClusterProtectionLevel |Ingen eller EncryptAndSign |Ingen (standard) för oskyddade kluster, EncryptAndSign för säker kluster. |

### <a name="section-name-hosting"></a>Avsnittsnamn: värd
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| ServiceTypeRegistrationTimeout |Tid i sekunder, standard är 300 |Högsta tillåtna tiden för ServiceType registreras med fabric |
| ServiceTypeDisableFailureThreshold |Heltal, standard är 1 |Detta är tröskelvärdet för antal fel som efter vilket meddelas FailoverManager (FM) att inaktivera tjänsttypen på noden och prova en annan nod för placering. |
| ActivationRetryBackoffInterval |Tid i sekunder, standardvärdet är 5 |Backoff intervall på varje Aktiveringsfel; På varje kontinuerlig Aktiveringsfel försöker systemet aktivering för upp till MaxActivationFailureCount. Återförsöksintervall på varje försök är en produkt från kontinuerlig Aktiveringsfel och inte aktiveringsintervallet. |
| ActivationMaxRetryInterval |Tid i sekunder, standard är 300 |På varje kontinuerlig Aktiveringsfel försöker systemet aktivering för upp till ActivationMaxFailureCount. ActivationMaxRetryInterval Anger tidsintervall för vänta innan ett återförsök görs efter varje Aktiveringsfel |
| ActivationMaxFailureCount |Heltal, standard är 10 |Antal gånger som system för nya försök misslyckats aktivering innan den ger upp |

### <a name="section-name-failovermanager"></a>Avsnittsnamn: FailoverManager
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| PeriodicLoadPersistInterval |Tid i sekunder är standard 10 |Detta avgör hur ofta FM-kontroll för nya rapporter |

### <a name="section-name-federation"></a>Avsnittsnamn: Federation
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| LeaseDuration |Tid i sekunder, standardvärdet är 30 |Varaktighet som ett lån pågår mellan en nod och sina grannar. |
| LeaseDurationAcrossFaultDomain |Tid i sekunder, standardvärdet är 30 |Varaktighet som ett lån pågår mellan en nod och sina grannar över feldomäner. |

### <a name="section-name-clustermanager"></a>Avsnittsnamn: ClusterManager
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| UpgradeStatusPollInterval |Tid i sekunder, standardvärdet är 60 |Frekvens för avsökning för uppgraderingsstatus för programmet. Det här värdet fastställer mängden uppdatering för alla GetApplicationUpgradeProgress-anrop |
| UpgradeHealthCheckInterval |Tid i sekunder, standardvärdet är 60 |Frekvensen av hälsostatus kontrollerar under en uppgradering för övervakade program |
| FabricUpgradeStatusPollInterval |Tid i sekunder, standardvärdet är 60 |Frekvens för avsökning för Fabric uppgraderingsstatus. Det här värdet fastställer mängden uppdatering för alla GetFabricUpgradeProgress-anrop |
| FabricUpgradeHealthCheckInterval |Tid i sekunder, standardvärdet är 60 |Frekvensen av hälsostatus Kontrollera under en övervakad Fabric-uppgradering |
|InfrastructureTaskProcessingInterval | Tid i sekunder är standard 10 |Ange tidsintervall i sekunder. Det intervall som används av aktiviteten infrastruktur för bearbetning av datorn. |
|TargetReplicaSetSize |Int, standard är 7 |TargetReplicaSetSize för ClusterManager. |
|MinReplicaSetSize |Int, standardinställningen är 3 |MinReplicaSetSize för ClusterManager. |
|ReplicaRestartWaitDuration |Tid i sekunder är standardvärdet (60,0 * 30)|Ange tidsintervall i sekunder. ReplicaRestartWaitDuration för ClusterManager. |
|QuorumLossWaitDuration |Tid i sekunder är standardvärdet MaxValue | Ange tidsintervall i sekunder. QuorumLossWaitDuration för ClusterManager. |
|StandByReplicaKeepDuration | Tid i sekunder är standardvärdet (3600.0 * 2)|Ange tidsintervall i sekunder. StandByReplicaKeepDuration för ClusterManager. |
|PlacementConstraints | sträng, standardvärdet är ”” |PlacementConstraints för ClusterManager. |
|SkipRollbackUpdateDefaultService | Bool, standard är FALSKT |CM hoppar över Återför uppdaterade standardtjänster under uppgraderingen återställningen av programmet. |
|EnableDefaultServicesUpgrade | Bool, standard är FALSKT |Aktivera uppgradering standardtjänster under uppgradering av programmet. Standardtjänsterna skulle skrivas efter uppgraderingen. |
|InfrastructureTaskHealthCheckWaitDuration |Tid i sekunder, standardvärdet är 0| Ange tidsintervall i sekunder. Mängden tid som ska förflyta innan du startar hälsokontroller när aktiviteten en infrastruktur för efterbearbetning. |
|InfrastructureTaskHealthCheckStableDuration | Tid i sekunder, standardvärdet är 0| Ange tidsintervall i sekunder. Hur lång tid att se på varandra följande skickade hälsokontroller innan efter bearbetning av en infrastruktur-aktivitet har slutförts. Sett misslyckade hälsokontrollen återställs den här timern. |
|InfrastructureTaskHealthCheckRetryTimeout | Tid i sekunder, standardvärdet är 60 |Ange tidsintervall i sekunder. Det gick inte att hälsokontroller när aktiviteten en infrastruktur för efterbearbetning mängden tid för försöker igen. Sett skickade hälsokontrollen återställs den här timern. |
|ImageBuilderTimeoutBuffer |Tid i sekunder, standardinställningen är 3 |Ange tidsintervall i sekunder. Hur lång tid för Image Builder specifika timeout-fel att återgå till klienten. Om den här bufferten är för liten. sedan klienten timeout innan servern och hämtar en allmän timeout-fel. |
|MinOperationTimeout | Tid i sekunder, standardvärdet är 60 |Ange tidsintervall i sekunder. Den minsta globala tidsgränsen för internt bearbetning vid ClusterManager. |
|MaxOperationTimeout |Tid i sekunder är standardvärdet MaxValue | Ange tidsintervall i sekunder. Den maximala globala tidsgränsen för internt bearbetning vid ClusterManager. |
|MaxTimeoutRetryBuffer | Tid i sekunder, standard är 600 |Ange tidsintervall i sekunder. Tidsgräns för maximal åtgärd när du startar om internt på grund av timeout är <Original Timeout>  +  <MaxTimeoutRetryBuffer>. Ytterligare timeout har lagts till i steg om MinOperationTimeout. |
|MaxCommunicationTimeout |Tid i sekunder, standard är 600 |Ange tidsintervall i sekunder. Maximal tidsgräns för intern kommunikation mellan andra system och ClusterManager tjänster (d.v.s.; Namntjänst; Failover Manager och osv). Det här bör vara mindre än globala MaxOperationTimeout (eftersom det kan finnas flera kommunikation mellan komponenter för varje klientåtgärd). |
|MaxDataMigrationTimeout |Tid i sekunder, standard är 600 |Ange tidsintervall i sekunder. Maximal tidsgräns för data migrering återställningsåtgärder när en Fabric-uppgraderingen har ägt rum. |
|MaxOperationRetryDelay |Tid i sekunder, standardvärdet är 5| Ange tidsintervall i sekunder. Maximal fördröjning för interna återförsök när fel uppstår. |
|ReplicaSetCheckTimeoutRollbackOverride |Tid i sekunder är standardvärdet 1200 | Ange tidsintervall i sekunder. Om ReplicaSetCheckTimeout är det maximala värdet för DWORD; sedan är den åsidosättas med värdet för den här konfigurationen för återställning. Det värde som används för loggsäkerhetskopian åsidosätts aldrig. |
|ImageBuilderJobQueueThrottle |Int, standardvärdet är 10 |Tråden antal begränsning för Image Builder proxy jobbkön på programförfrågningar. |

## <a name="next-steps"></a>Nästa steg
Läs artiklarna mer information om hantering av kluster:

[Lägga till, rulla över, ta bort certifikat från ditt Azure-kluster](service-fabric-cluster-security-update-certs-azure.md) 

