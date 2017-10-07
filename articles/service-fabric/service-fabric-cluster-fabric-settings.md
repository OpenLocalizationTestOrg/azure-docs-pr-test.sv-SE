---
title: "inställningar för aaaChange Azure Service Fabric-kluster | Microsoft Docs"
description: "Den här artikeln beskriver hello infrastrukturinställningarna och hello fabric uppgradera principer som du kan anpassa."
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
ms.openlocfilehash: a8b125f7b4a02547cf4bcf2c936d0c7f1686c1a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="customize-service-fabric-cluster-settings-and-fabric-upgrade-policy"></a>Anpassa inställningar för Service Fabric-kluster och Fabric-uppgradera princip
Det här dokumentet får du reda på hur toocustomize hello olika infrastrukturinställningarna och hello fabric princip för versionsuppgradering för Service Fabric-klustret. Du kan anpassa dem på hello-portalen eller med en Azure Resource Manager-mall.

> [!NOTE]
> Inte alla inställningar kan vara tillgängliga via hello-portalen. Om en inställning nedan inte är tillgängliga via hello portal anpassa den med hjälp av en Azure Resource Manager-mall.
> 

## <a name="customizing-service-fabric-cluster-settings-using-azure-resource-manager-templates"></a>Anpassa inställningar för Service Fabric-kluster med hjälp av Azure Resource Manager-mallar
hello stegen nedan illustrerar hur tooadd en ny inställning *MaxDiskQuotaInMB* toohello *diagnostik* avsnitt.

1. Gå toohttps://resources.azure.com
2. Navigera tooyour prenumeration genom att expandera prenumerationer -> resurs grupper -> Microsoft.ServiceFabric -> klustrets namn
3. I hello övre högra hörnet, väljer du ”full”
4. Välj Redigera och uppdatera hello `fabricSettings` JSON-element och lägga till ett nytt element

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
Här följer hello Fabric-inställningar som du kan anpassa:

### <a name="section-name-diagnostics"></a>Avsnittsnamn: diagnostik
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| ConsumerInstances |Sträng |hello listan över DCA konsumenten instanser. |
| ProducerInstances |Sträng |hello listan över DCA producenten instanser. |
| AppEtwTraceDeletionAgeInDays |Int, standardinställningen är 3 |Antal dagar efter vilken vi ta bort gamla ETL-filer som innehåller programmet ETW-spårning. |
| AppDiagnosticStoreAccessRequiresImpersonation |Bool, standard är SANT |Personifiering krävs oavsett när åtkomst till diagnostiska lagrar uppdrag hello program. |
| MaxDiskQuotaInMB |Int, standard är 65536 |Diskkvot i MB för Windows Fabric-loggfiler. |
| DiskFullSafetySpaceInMB |Int, standard är 1024 |Återstående diskutrymme i MB tooprotect från användning av DCA. |
| ApplicationLogsFormatVersion |Int, standardvärdet är 0 |Versionen för programmet loggar format. Värden som stöds är 0 och 1. Version 1 innehåller flera fält från hello ETW händelsepost än version 0. |
| ClusterId |Sträng |hello unikt id för hello klustret. Det här genereras när hello klustret har skapats. |
| EnableTelemetry |Bool, standard är SANT |Detta händer tooenable eller inaktivera telemetri. |
| EnableCircularTraceSession |Bool, standard är FALSKT |Flagga som anger om cirkulär spårningssessioner ska användas. |

### <a name="section-name-traceetw"></a>Avsnittsnamn: Spåra/Etw
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| Nivå |Int, standardvärdet är 4 |Spårningsnivån för etw kan ha värden 1, 2, 3, 4. toobe som stöds måste du behålla hello spårningsnivån vid 4 |

### <a name="section-name-performancecounterlocalstore"></a>Avsnittsnamn: PerformanceCounterLocalStore
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| IsEnabled |Bool, standard är SANT |Flagga som anger om prestandaräknarsamlingen på lokal nod är aktiverad. |
| SamplingIntervalInSeconds |Int, standardvärdet är 60 |Exempelintervall för prestandaräknare som samlas in. |
| Räknare |Sträng |Kommaavgränsad lista över prestandaräknare toocollect. |
| MaxCounterBinaryFileSizeInMB |Int, standardvärdet är 1 |Maximal storlek (i MB) för varje prestandaräknaren binär fil. |
| NewCounterBinaryFileCreationIntervalInMinutes |Int, standardvärdet är 10 |Maximalt tidsintervall (i sekunder) efter vilken en ny prestandaräknaren binär fil skapas. |

### <a name="section-name-setup"></a>Avsnittsnamn: installationsprogrammet
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| FabricDataRoot |Sträng |Rotkatalogen för Service Fabric-data. Som standard för Azure är d:\svcfab |
| FabricLogRoot |Sträng |Service fabric rot loggkatalogen. Detta är SA loggar och spårningar placering. |
| ServiceRunAsAccountName |Sträng |hello kontonamn under vilka toorun infrastrukturvärdtjänsten. |
| ServiceStartupType |Sträng |hello starttyp för hello infrastrukturvärdtjänsten. |
| SkipFirewallConfiguration |Bool, standard är FALSKT |Anger om brandväggsinställningar måste toobe anges av hello system eller inte. Detta gäller endast om du använder windows-brandväggen. Om du använder brandväggar från tredje part, måste du öppna hello portar för hello system och program toouse |

### <a name="section-name-transactionalreplicator"></a>Avsnittsnamn: TransactionalReplicator
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| MaxCopyQueueSize |Uint, standard är 16384 |Detta är hello maxvärdet definierar hello startstorlek för hello kö som underhåller replikeringsåtgärder. Observera att det måste vara delbart med 2. Om körningsfel hello kön kommer att begränsas toothis storlek åtgärder mellan hello primära och sekundära replikatörer. |
| BatchAcknowledgementInterval | Tid i sekunder är standardvärdet 0.015 | Ange tidsintervall i sekunder. Anger hello tidsperiod som hello replikatorn väntar när du har fått en åtgärd innan en bekräftelse skickas tillbaka. Andra åtgärder som togs emot under den här tiden har sina bekräftelser skickas tillbaka i ett enda meddelande -> minska nätverkstrafiken, men kan vara att minska hello genomflöde hello replikatorn. |
| MaxReplicationMessageSize |Uint, standard är 52428800 | Maximal meddelandestorlek på replikeringsåtgärder. Standardvärdet är 50MB. |
| ReplicatorAddress |strängen, standard är ”localhost:0” | hello-slutpunkt i form av en sträng-'IP:Port' som används av hello Windows Fabric replikatorn tooestablish anslutningar med repliker i ordning toosend och ta emot åtgärder. |
| InitialPrimaryReplicationQueueSize |Uint, standard är 64 | Det här värdet definierar hello startstorlek för hello kö som underhåller hello replikeringsåtgärder på hello primära. Observera att det måste vara delbart med 2.|
| MaxPrimaryReplicationQueueSize |Uint, standard är 8192 |Detta är hello maximalt antal åtgärder som kan finnas i kö för hello primär replikering. Observera att det måste vara delbart med 2. |
| MaxPrimaryReplicationQueueMemorySize |Uint, standardvärdet är 0 |Detta är hello maximivärdet för hello primära Replikeringskön i byte. |
| InitialSecondaryReplicationQueueSize |Uint, standard är 64 |Det här värdet definierar hello startstorlek för hello kö som underhåller hello replikeringsåtgärder på hello sekundär. Observera att det måste vara delbart med 2. |
| MaxSecondaryReplicationQueueSize |Uint, standard är 16384 |Detta är hello maximalt antal åtgärder som kan finnas i hello sekundära Replikeringskön. Observera att det måste vara delbart med 2. |
| MaxSecondaryReplicationQueueMemorySize |Uint, standardvärdet är 0 |Detta är hello maximivärdet för hello sekundära Replikeringskön i byte. |
| SecondaryClearAcknowledgedOperations |Bool, standard är FALSKT |Bool som styr om hello åtgärder på hello sekundära replikatorn är avmarkerad när de bekräftade toohello primära (en toohello disk). Inställningar för den här tooTRUE kan medföra ytterligare Diskläsningar på nya primära hello när fånga repliker efter en redundansväxling. |
| MaxMetadataSizeInKB |Int, standardvärdet är 4 |Maximal storlek för hello log-metadata för dataströmmen. |
| MaxRecordSizeInKB |Uint, standard är 1024 | Maximal storlek för en loggpost för dataströmmen. |
| CheckpointThresholdInMB |Int, standardvärdet är 50 |En kontrollpunkt påbörjas när hello logganvändning överskrider detta värde. |
| MaxAccumulatedBackupLogSizeInMB |Int, är standard 800 |Max ackumulerade storleken (i MB) för säkerhetskopiering loggar i en viss loggkedja. En inkrementell säkerhetskopiering begäranden misslyckas om hello inkrementell säkerhetskopiering skulle Generera en säkerhetskopiering logg som skulle orsaka hello ackumulerade säkerhetskopieringsloggar sedan hello relevanta fullständig säkerhetskopiering toobe större än den här storleken. I sådana fall måste är användaren obligatoriska tootake en fullständig säkerhetskopia. |
| MaxWriteQueueDepthInKB |Int, standardvärdet är 0 | Int för maximal skriva ködjup som hello core loggaren kan använda som anges i kilobyte för hello-logg som är associerad med den här repliken. Det här värdet är hello maximalt antal byte som kan vara utestående under viktiga uppdateringar av meddelandeloggfiler. Det kan vara 0 för hello core loggaren toocompute ett lämpligt värde eller en multipel av 4. |
| SharedLogId |Sträng |Delade identifierare. Detta är ett guid och måste vara unikt för varje delad logg. |
| SharedLogPath |Sträng |Sökvägen toohello delade logg. Om det här värdet är tomt används hello standard delade loggen. |
| SlowApiMonitoringDuration |Tid i sekunder, standard är 300 | Ange varaktighet för api innan varning hälsa händelsen utlöses.|
| MinLogSizeInMB |Int, standardvärdet är 0 |Minimistorlek hello transaktionella loggen. hello loggen tillåts inte tootruncate tooa storlek under den här inställningen. 0 anger att hello replikatorn avgör hello minsta loggfilsstorleken enligt tooother inställningar. Ökar det här värdet ökas hello möjligheten att göra partiella kopior och inkrementella säkerhetskopieringar eftersom risken för relevanta loggposter trunkerades sänks. |

### <a name="section-name-fabricclient"></a>Avsnittsnamn: FabricClient
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| NodeAddresses |sträng, standardvärdet är ”” |En samling av adresser (anslutningssträngar) på olika noder som kan använda toocommunicate med hello hello Naming Service. Ursprungligen hello ansluter klienten att välja en av adresserna hello slumpmässigt. Om mer än en anslutningssträngen har angetts och en anslutning misslyckas på grund av en kommunikation eller timeout-fel. hello växlar klienten toouse hello nästa adress sekventiellt. Avsnittet hello Naming serviceadressen försök information på återförsök semantik. |
| ConnectionInitializationTimeout |Tid i sekunder, är standardvärdet 2 |Ange tidsintervall i sekunder. Tidsgränsen för anslutning för varje gång klienten försöker tooopen en anslutning toohello gateway. |
| PartitionLocationCacheLimit |Int, standard är 100000 |Antalet partitioner som cachelagrats för tjänsten upplösning (Ange too0 obegränsad). |
| ServiceChangePollInterval |Tid i sekunder, standardvärdet är 120 |Ange tidsintervall i sekunder. hello intervall mellan på varandra följande tjänst ändras från hello klient toohello gateway för registrerade tjänsten ändra meddelanden återanrop. |
| KeepAliveIntervalInSeconds |Int, standardvärdet är 20 |hello intervall på vilka hello FabricClient transport skickar keep-alive-meddelanden toohello gateway. För 0; keepAlive är inaktiverad. Måste vara ett positivt värde. |
| HealthOperationTimeout |Tid i sekunder, standardvärdet är 120 |Ange tidsintervall i sekunder. hello timeout för ett rapportmeddelande skickas tooHealth Manager. |
| HealthReportSendInterval |Tid i sekunder, standardvärdet är 30 |Ange tidsintervall i sekunder. hello intervall på vilka rapporteringskomponenten skickar ackumulerade hälsa rapporterar tooHealth Manager. |
| HealthReportRetrySendInterval |Tid i sekunder, standardvärdet är 30 |Ange tidsintervall i sekunder. hello intervall på vilka rapporteringskomponenten igen skickar ackumulerade hälsa rapporterar tooHealth Manager. |
| RetryBackoffInterval |Tid i sekunder, standardinställningen är 3 |Ange tidsintervall i sekunder. hello inte intervallet innan du försöker hello igen. |
| MaxFileSenderThreads |Uint, standardvärdet är 10 |Hej max antal filer som överförs parallellt. |

### <a name="section-name-common"></a>Avsnittsnamn: vanliga
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| PerfMonitorInterval |Tid i sekunder är standardvärdet 1 |Ange tidsintervall i sekunder. Prestanda Övervakningsintervall. Inställningen too0 eller negativt inaktiverar övervakning. |

### <a name="section-name-healthmanager"></a>Avsnittsnamn: HealthManager
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| EnableApplicationTypeHealthEvaluation |Bool, standard är FALSKT |Klustret hälsoprincip för utvärdering: aktivera per program typen hälsoutvärderingen. |

### <a name="section-name-fabricnode"></a>Avsnittsnamn: FabricNode
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| StateTraceInterval |Tid i sekunder, standard är 300 |Ange tidsintervall i sekunder. hello-intervall för att spåra nod status på varje nod och uppåt noder på FM/FMM. |

### <a name="section-name-nodedomainids"></a>Avsnittsnamn: NodeDomainIds
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| UpgradeDomainId |sträng, standardvärdet är ”” |Beskriver hello en nod som hör till domänen. |
| PropertyGroup |NodeFaultDomainIdCollection |Beskriver hello feldomäner tillhör en nod. Hej feldomän definieras via en URI som beskriver hello plats hello nod i hello datacenter.  Fel domän URI: er är av hello formatera fd: / fd/följt av ett URI-sökvägssegment.|

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
| StartApplicationPortRange |Int, standardvärdet är 0 |Starta hello programmet portar som hanteras av värd undersystemet. Krävs om EndpointFilteringEnabled är true i värd. |
| EndApplicationPortRange |Int, standardvärdet är 0 |Slut (inte inklusiva) hello programmet portar som hanteras av värd undersystemet. Krävs om EndpointFilteringEnabled är true i värd. |
| ClusterX509StoreName |sträng, standardvärdet är ”Mina” |Namnet på X.509-certifikatarkiv som innehåller klustret certifikat för säker kommunikation inom klustret. |
| ClusterX509FindType |strängen, standard är ”FindByThumbprint” |Anger hur toosearch för kluster-certifikat i arkivet för hello anges av ClusterX509StoreName stöds värden: ”FindByThumbprint”; ”FindBySubjectName” med ”FindBySubjectName”; När det finns flera matchningar; hello en hello längst giltighetstid används. |
| ClusterX509FindValue |sträng, standardvärdet är ”” |Sök filtervärdet används toolocate klustret certifikat. |
| ClusterX509FindValueSecondary |sträng, standardvärdet är ”” |Sök filtervärdet används toolocate klustret certifikat. |
| ServerAuthX509StoreName |sträng, standardvärdet är ”Mina” |Namnet på X.509-certifikatarkiv som innehåller servercertifikat för förrätter service. |
| ServerAuthX509FindType |strängen, standard är ”FindByThumbprint” |Anger hur toosearch för servercertifikat i hello store anges av ServerAuthX509StoreName stöds värde: FindByThumbprint; FindBySubjectName. |
| ServerAuthX509FindValue |sträng, standardvärdet är ”” |Sök filtervärdet används toolocate servercertifikat. |
| ServerAuthX509FindValueSecondary |sträng, standardvärdet är ”” |Sök filtervärdet används toolocate servercertifikat. |
| ClientAuthX509StoreName |sträng, standardvärdet är ”Mina” |Namnet på hello X.509-certifikatarkiv som innehåller standard administratörsroll FabricClient-certifikat. |
| ClientAuthX509FindType |strängen, standard är ”FindByThumbprint” |Anger hur toosearch för certifikatet i arkivet hello anges av ClientAuthX509StoreName stöds värde: FindByThumbprint; FindBySubjectName. |
| ClientAuthX509FindValue |sträng, standardvärdet är ”” | Sök filtervärdet används toolocate certifikat för standard administratörsroll FabricClient. |
| ClientAuthX509FindValueSecondary |sträng, standardvärdet är ”” |Sök filtervärdet används toolocate certifikat för standard administratörsroll FabricClient. |
| UserRoleClientX509StoreName |sträng, standardvärdet är ”Mina” |Namnet på hello X.509-certifikatarkiv som innehåller certifikat för standardanvändarrollen FabricClient. |
| UserRoleClientX509FindType |strängen, standard är ”FindByThumbprint” |Anger hur toosearch för certifikatet i arkivet hello anges av UserRoleClientX509StoreName stöds värde: FindByThumbprint; FindBySubjectName. |
| UserRoleClientX509FindValue |sträng, standardvärdet är ”” |Sök filtervärdet används toolocate certifikat för standardanvändarrollen FabricClient. |
| UserRoleClientX509FindValueSecondary |sträng, standardvärdet är ”” |Sök filtervärdet används toolocate certifikat för standardanvändarrollen FabricClient. |

### <a name="section-name-paas"></a>Avsnittsnamn: Paas
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| ClusterId |sträng, standardvärdet är ”” |X509 certifikatarkivet användas av fabric för konfiguration av skydd. |

### <a name="section-name-fabrichost"></a>Avsnittsnamn: FabricHost
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| StopTimeout |Tid i sekunder, standard är 300 |Ange tidsintervall i sekunder. hello tidsgränsen för aktivering i värdtjänsten; Inaktivering och uppgradering. |
| StartTimeout |Tid i sekunder, standard är 300 |Ange tidsintervall i sekunder. Timeout för fabricactivationmanager start. |
| ActivationRetryBackoffInterval |Tid i sekunder, standardvärdet är 5 |Ange tidsintervall i sekunder. Backoff intervall på varje Aktiveringsfel; på varje kontinuerlig aktivering fel hello system kommer att försöka hello aktivering för in toohello MaxActivationFailureCount. hello återförsöksintervall på varje försök är en produkt från kontinuerlig felet och hello aktivering inte aktiveringsintervallet. |
| ActivationMaxRetryInterval |Tid i sekunder, standard är 300 |Ange tidsintervall i sekunder. Max återförsöksintervall för aktivering. På varje kontinuerlig fel hello försök beräknas intervall som Min (ActivationMaxRetryInterval; Kontinuerlig Felberäkning * ActivationRetryBackoffInterval). |
| ActivationMaxFailureCount |Int, standardvärdet är 10 |Detta är hello maximalt antal som systemet ska misslyckade försök aktivera igen innan den ger upp. |
| EnableServiceFabricAutomaticUpdates |Bool, standard är FALSKT |Detta är tooenable fabric automatiska uppdateringar via Windows Update. |
| EnableServiceFabricBaseUpgrade |Bool, standard är FALSKT |Detta är tooenable grundläggande uppdatering för servern. |
| EnableRestartManagement |Bool, standard är FALSKT |Detta är tooenable omstart av servern. |


### <a name="section-name-failovermanager"></a>Avsnittsnamn: FailoverManager
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| UserReplicaRestartWaitDuration |Tid i sekunder är standard 60,0 * 30 |Ange tidsintervall i sekunder. När en beständiga replik kraschar; Windows Fabric väntar varaktigheten för hello replik toocome säkerhetskopiera innan du skapar nya ersättning repliker (som kräver en kopia av hello tillstånd). |
| QuorumLossWaitDuration |Tid i sekunder är standardvärdet MaxValue |Ange tidsintervall i sekunder. Detta är hello maximala längd som tillåter vi en partition toobe av förlorar kvorum. Om hello partitionen är fortfarande i kvorumförlust efter varaktigheten; hello partition återställs från förlorar kvorum med funderar på hello ned repliker som förlorad. Observera att detta potentiellt kan medföra dataförluster. |
| UserStandByReplicaKeepDuration |Tid i sekunder är standardvärdet 3600.0 * 24 * 7 |Ange tidsintervall i sekunder. När en beständiga replik kommer tillbaka från ett nedåt tillstånd. den kan ha redan ersatts. Den här timern anger hur länge hello FM behålls hello vänteläge repliken innan du tar bort den. |
| UserMaxStandByReplicaCount |Int, standardvärdet är 1 |hello standard max antal StandBy-repliker hello systemet håller för användaren tjänster. |

### <a name="section-name-namingservice"></a>Avsnittsnamn: NamingService
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| TargetReplicaSetSize |Int, standard är 7 |hello antal replik anger för varje partition för hello namngivningstjänst store. Öka hello antal replik anger ökar hello nivå av tillförlitlighet hello information i hello Naming Service Store; minska hello ändring som hello information kommer att gå förlorade på grund av nodfel; kostnad för ökad belastning på Windows Fabric och hello lång tid tar det tooperform uppdateringar toohello namngivning data.|
|MinReplicaSetSize | Int, standardinställningen är 3 | hello minsta antalet namngivningstjänst repliker krävs toowrite till toocomplete en uppdatering. Om det finns färre replikeringar än nekar den här aktiv i hello system hello tillförlitlighet System uppdateringar toohello Naming Service Store förrän replikeringarna har återställts. Det här värdet ska aldrig vara mer än hello TargetReplicaSetSize. |
|ReplicaRestartWaitDuration | Tid i sekunder är standardvärdet (60,0 * 30)| Ange tidsintervall i sekunder. När en replik för namngivningstjänst kraschar; den här timern startas.  När den upphör att gälla hello FM börjar tooreplace hello repliker som är nere (den inte ännu anser tappas bort). |
|QuorumLossWaitDuration | Tid i sekunder är standardvärdet MaxValue | Ange tidsintervall i sekunder. När en Naming Service hämtar i kvorumförlust; den här timern startas.  När den upphör att gälla hello FM beaktas hello ned repliker som gå förlorad. och försök toorecover kvorum. Inte som den här kan resultera i förlust av data. |
|StandByReplicaKeepDuration | Tid i sekunder är standard 3600.0 * 2 | Ange tidsintervall i sekunder. När en namngivningstjänst repliker gå tillbaka från ett nedåt tillstånd. den kan ha redan ersatts.  Den här timern anger hur länge hello FM behålls hello vänteläge repliken innan du tar bort den. |
|PlacementConstraints | sträng, standardvärdet är ”” | Placering begränsningen för hello Naming Service. |
|ServiceDescriptionCacheLimit | Int, standardvärdet är 0 | hello maximala antalet poster som underhålls i hello LRU beskrivning cache-tjänst på hello Naming lagringstjänst (Ange too0 obegränsad). |
|RepairInterval | Tid i sekunder, standardvärdet är 5 | Ange tidsintervall i sekunder. Intervall i vilka hello namngivning inkonsekvens reparera mellan hello myndigheten ägare och namnägare startar. |
|MaxNamingServiceHealthReports | Int, standardvärdet är 10 | hello maximalt antal långsam åtgärder som lagrar Naming service rapporter feltillstånd i taget. Om 0; alla åtgärder för långsam skickas. |
| MaxMessageSize |Int, standardvärdet är 4*1024*1024 |hello maximal storlek för noden klientkommunikation när du använder naming. DOS-attack lindra; Standardvärdet är 4MB. |
| MaxFileOperationTimeout |Tid i sekunder, standardvärdet är 30 |Ange tidsintervall i sekunder. hello Maximal timeout för åtgärden för filen store-tjänsten. Ange en längre tidsgräns nekas. |
| MaxOperationTimeout |Tid i sekunder, standard är 600 |Ange tidsintervall i sekunder. hello Maximal tidsgräns för Klientåtgärder. Ange en längre tidsgräns nekas. |
| MaxClientConnections |Int, standardvärdet är 1000 |hello högsta tillåtna antalet klientanslutningar per gateway. |
| ServiceNotificationTimeout |Tid i sekunder, standardvärdet är 30 |Ange tidsintervall i sekunder. hello timeout används för att leverera meddelanden toohello klienten. |
| MaxOutstandingNotificationsPerClient |Int, standardvärdet är 1000 |hello maximalt antal väntande meddelanden innan en klientregistrering stängs framtvingar av hello gateway. |
| MaxIndexedEmptyPartitions |Int, standardvärdet är 1000 |hello maximalt antal tomma partitioner som ska vara indexera i hello notification cache för att synkronisera återanslutning klienter. Eventuella tomma partitioner ovanför det här numret tas bort från hello index i stigande ordning för sökning version. Ansluta klienter kan fortfarande synkronisera och ta emot uppdateringar missade tom partition. men hello synkroniseringsprotokollet blir dyrare. |
| GatewayServiceDescriptionCacheLimit |Int, standardvärdet är 0 |hello maximala antalet poster som underhålls i hello LRU beskrivning cache-tjänst på hello Namngivningsgateway (Ange too0 obegränsad). |
| PartitionCount |Int, standardinställningen är 3 |hello antalet partitioner i hello namngivningstjänst store toobe skapas. Varje partition äger en enda partitionsnyckel som motsvarar tooits index. så partitionsnycklar [0; PartitionCount) finns. Ange ökande hello antalet namngivningstjänst partitioner ökar hello skalan som hello Naming Service kan utföra genom att minska hello genomsnittliga mängden data som finns hos någon stödjande replik; kostnad ökad användning av resurser (eftersom PartitionCount * ReplicaSetSize service repliker måste underhållas).|

### <a name="section-name-runas"></a>Avsnittsnamn: RunAs
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| RunAsAccountName |sträng, standardvärdet är ”” |Anger namnet för hello RunAs-kontot. Detta krävs endast för ”DomainUser” eller ”ManagedServiceAccount” typen. Giltiga värden är ”domän\användare” eller ”user@domain”. |
|RunAsAccountType|sträng, standardvärdet är ”” |Anger typen för hello RunAs-konto. Detta krävs för alla RunAs-avsnittet, giltiga värden är ”DomainUser/NetworkService/ManagedServiceAccount/LocalSystem”.|
|Lösenord|sträng, standardvärdet är ”” |Anger lösenordet för hello RunAs-konto. Detta krävs endast för typen ”DomainUser”. |

### <a name="section-name-runasfabric"></a>Avsnittsnamn: RunAs_Fabric
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| RunAsAccountName |sträng, standardvärdet är ”” |Anger namnet för hello RunAs-kontot. Detta krävs endast för ”DomainUser” eller ”ManagedServiceAccount” typen. Giltiga värden är ”domän\användare” eller ”user@domain”. |
|RunAsAccountType|sträng, standardvärdet är ”” |Anger typen för hello RunAs-konto. Detta krävs för alla RunAs-avsnittet, giltiga värden är ”Lokalanvändare/DomainUser/NetworkService/ManagedServiceAccount/LocalSystem”. |
|Lösenord|sträng, standardvärdet är ”” |Anger lösenordet för hello RunAs-konto. Detta krävs endast för typen ”DomainUser”. |

### <a name="section-name-runashttpgateway"></a>Avsnittsnamn: RunAs_HttpGateway
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| RunAsAccountName |sträng, standardvärdet är ”” |Anger namnet för hello RunAs-kontot. Detta krävs endast för ”DomainUser” eller ”ManagedServiceAccount” typen. Giltiga värden är ”domän\användare” eller ”user@domain”. |
|RunAsAccountType|sträng, standardvärdet är ”” |Anger typen för hello RunAs-konto. Detta krävs för alla RunAs-avsnittet, giltiga värden är ”Lokalanvändare/DomainUser/NetworkService/ManagedServiceAccount/LocalSystem”. |
|Lösenord|sträng, standardvärdet är ”” |Anger lösenordet för hello RunAs-konto. Detta krävs endast för typen ”DomainUser”. |

### <a name="section-name-runasdca"></a>Avsnittsnamn: RunAs_DCA
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| RunAsAccountName |sträng, standardvärdet är ”” |Anger namnet för hello RunAs-kontot. Detta krävs endast för ”DomainUser” eller ”ManagedServiceAccount” typen. Giltiga värden är ”domän\användare” eller ”user@domain”. |
|RunAsAccountType|sträng, standardvärdet är ”” |Anger typen för hello RunAs-konto. Detta krävs för alla RunAs-avsnittet, giltiga värden är ”Lokalanvändare/DomainUser/NetworkService/ManagedServiceAccount/LocalSystem”. |
|Lösenord|sträng, standardvärdet är ”” |Anger lösenordet för hello RunAs-konto. Detta krävs endast för typen ”DomainUser”. |

### <a name="section-name-httpgateway"></a>Avsnittsnamn: HttpGateway
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
|IsEnabled|Bool, standard är FALSKT | Aktiverar eller inaktiverar hello httpgateway. Httpgateway är inaktiverad som standard och den här konfigurationen måste toobe set tooenable den. |
|ActiveListeners |Uint, standardvärdet är 50 | Antalet läsningar toopost toohello HTTP-server-kön. Detta styr hello antalet samtidiga begäranden som kan betjänas av hello HttpGateway. |
|MaxEntityBodySize |Uint, standardvärdet är 4194304 |  Ger hello maxstorleken för hello brödtext som kan förväntas från en http-begäran. Standardvärdet är 4MB. Httpgateway misslyckas en begäran om den har en uppsättning storlek > det här värdet. Minsta skrivskyddade segmentstorleken är 4096 byte. Så här har toobe > = 4096. |
|HttpGatewayHealthReportSendInterval |Tid i sekunder, standardvärdet är 30 | Ange tidsintervall i sekunder. hello intervall på vilka hello Http Gateway skickar ackumulerade hälsa rapporter tooHealth Manager. |

### <a name="section-name-ktllogger"></a>Avsnittsnamn: KtlLogger
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
|AutomaticMemoryConfiguration |Int, standardvärdet är 1 | Flagga som anger om hello minne dynamiskt och automatiskt konfigureras. Om noll sedan konfigurationsinställningarna för hello-minne som används direkt och ändra inte baserat på villkor som system. Om en baserat hello minne inställningar konfigureras automatiskt och kan ändras på system-villkor. |
|WriteBufferMemoryPoolMinimumInKB |Int, standard är 8388608 |hello antal KB tooinitially allokera för hello skrivåtgärder buffert minnespoolen. Använd 0 tooindicate ingen gräns standard ska vara konsekvent med SharedLogSizeInMB nedan. |
|WriteBufferMemoryPoolMaximumInKB | Int, standardvärdet är 0 |hello antal KB tooallow hello skriva buffert minne poolen toogrow upp till. Använd 0 tooindicate någon gräns. |
|MaximumDestagingWriteOutstandingInKB | Int, standardvärdet är 0 | hello antal KB tooallow hello delade loggen tooadvance i hello dedikerade loggen. Använd 0 tooindicate någon gräns.
|SharedLogPath |sträng, standardvärdet är ”” | Sökvägen och namnet toolocation tooplace delade loggen behållare. Använd ”” för att använda standardsökvägen under fabric-dataroten. |
|SharedLogId |sträng, standardvärdet är ”” |Unikt guid för delade loggen behållare. Använd ”” om du använder standardsökvägen under fabric-dataroten. |
|SharedLogSizeInMB |Int, standard är 8192 | hello antal MB tooallocate i hello delade loggen behållaren. |

### <a name="section-name-applicationgatewayhttp"></a>Avsnittsnamn: ApplicationGateway/Http
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
|IsEnabled |Bool, standard är FALSKT | Aktiverar eller inaktiverar hello HttpApplicationGateway. HttpApplicationGateway är inaktiverad som standard och den här konfigurationen måste toobe set tooenable den. |
|NumberOfParallelOperations | Uint, standard är 5 000 | Antalet läsningar toopost toohello HTTP-server-kön. Detta styr hello antalet samtidiga begäranden som kan betjänas av hello HttpGateway. |
|DefaultHttpRequestTimeout |Tid i sekunder. Standardvärdet är 120 |Ange tidsintervall i sekunder.  Ger hello standard begäran-timeout för hello http-begäranden som bearbetas i hello http app gateway. |
|ResolveServiceBackoffInterval |Tid i sekunder, standardvärdet är 5 |Ange tidsintervall i sekunder.  Ger hello inte standardintervallet innan du försöker en misslyckad Lös tjänståtgärd. |
|BodyChunkSize |Uint, standard är 16384 |  Ger hello storlek för hello segmentet i byte används tooread hello brödtext. |
|GatewayAuthCredentialType |String, standard är ”ingen” | Anger hello säkerhet autentiseringsuppgifter toouse på hello http app gateway endpoint giltiga värden är ”ingen / X 509. |
|GatewayX509CertificateStoreName |sträng, standardvärdet är ”Mina” | Namnet på X.509-certifikatarkiv som innehåller certifikat för HTTP-app gateway. |
|GatewayX509CertificateFindType |strängen, standard är ”FindByThumbprint” | Anger hur toosearch för certifikatet i arkivet hello anges av GatewayX509CertificateStoreName stöds värde: FindByThumbprint; FindBySubjectName. |
|GatewayX509CertificateFindValue | sträng, standardvärdet är ”” | Sök filtervärdet används toolocate hello http app gateway-certifikat. Det här certifikatet har konfigurerats på hello https-slutpunkt och kan också vara används tooverify hello identitet hello app om det behövs hello-tjänster. FindValue slås upp första; och om som inte finns. FindValueSecondary slås upp. |
|GatewayX509CertificateFindValueSecondary | sträng, standardvärdet är ”” |Sök filtervärdet används toolocate hello http app gateway-certifikat. Det här certifikatet har konfigurerats på hello https-slutpunkt och kan också vara används tooverify hello identitet hello app om det behövs hello-tjänster. FindValue slås upp första; och om som inte finns. FindValueSecondary slås upp.|

### <a name="section-name-management"></a>Avsnittsnamn: Management
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| ImageStoreConnectionString |SecureString | Anslutningen sträng toohello rot för Avbildningsarkiv. |
| ImageStoreMinimumTransferBPS | Int, standard är 1024 |hello minsta överföringshastighet mellan hello klustret och Avbildningsarkiv. Det här värdet är används toodetermine hello timeout när åtkomst till hello externa Avbildningsarkiv. Ändra det här värdet bara om hello fördröjning mellan hello klustret och Avbildningsarkiv är hög tooallow mer tid för hello klustret toodownload från hello externa Avbildningsarkiv. |
|AzureStorageMaxWorkerThreads | Int, standard är 25 | hello maximalt antal arbetstrådar parallellt. |
|AzureStorageMaxConnections | Int, standard är 5 000 | hello maximalt antal samtidiga anslutningar tooazure lagring. |
|AzureStorageOperationTimeout | Tid i sekunder är standardvärdet 6000 | Ange tidsintervall i sekunder. Tidsgräns för xstore åtgärden toocomplete. |
|ImageCachingEnabled | Bool, standard är SANT | Den här konfigurationen kan vi tooenable eller inaktivera cachelagring. |
|DisableChecksumValidation | Bool, standard är FALSKT | Den här konfigurationen gör att vi tooenable eller inaktivera validering av kontrollsumma under etableringen av programmet. |
|DisableServerSideCopy | Bool, standard är FALSKT | Den här konfigurationen aktiverar eller inaktiverar serverkopia av programpaket på hello Avbildningsarkiv under etableringen av programmet. |

### <a name="section-name-healthmanagerclusterhealthpolicy"></a>Avsnittsnamn: HealthManager/ClusterHealthPolicy
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| ConsiderWarningAsError |Bool, standard är FALSKT |Klustret hälsoprincip för utvärdering: behandlas varningar som fel. |
| MaxPercentUnhealthyNodes | Int, standardvärdet är 0 |Klustret hälsoprincip för utvärdering: Maxprocent felaktiga noder tillåts för hello klustret toobe felfritt. |
| MaxPercentUnhealthyApplications | Int, standardvärdet är 0 |Klustret hälsoprincip för utvärdering: maximala procentandelen felaktiga program tillåts för hello klustret toobe felfritt. |
|MaxPercentDeltaUnhealthyNodes | Int, standardvärdet är 10 |Klustret uppgradera utvärdering hälsoprincip: Maxprocent delta felaktiga noder tillåts för hello klustret toobe felfritt. |
|MaxPercentUpgradeDomainDeltaUnhealthyNodes | Int, standardvärdet är 15 |Klustret uppgradera utvärdering hälsoprincip: maximala procentandelen delta felaktiga noder i en uppgraderingsdomän tillåts för hello klustret toobe felfritt.|

### <a name="section-name-faultanalysisservice"></a>Avsnittsnamn: FaultAnalysisService
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| TargetReplicaSetSize |Int, standardvärdet är 0 |NOT_PLATFORM_UNIX_START hello TargetReplicaSetSize för FaultAnalysisService. |
| MinReplicaSetSize |Int, standardvärdet är 0 |Hej MinReplicaSetSize för FaultAnalysisService. |
| ReplicaRestartWaitDuration |Tid i sekunder, standardvärdet är 60 minuter|Ange tidsintervall i sekunder. Hej ReplicaRestartWaitDuration för FaultAnalysisService. |
| QuorumLossWaitDuration | Tid i sekunder är standardvärdet MaxValue |Ange tidsintervall i sekunder. Hej QuorumLossWaitDuration för FaultAnalysisService. |
| StandByReplicaKeepDuration| Tid i sekunder är standardvärdet (60*24*7) minuter |Ange tidsintervall i sekunder. Hej StandByReplicaKeepDuration för FaultAnalysisService. |
| PlacementConstraints | sträng, standardvärdet är ””| Hej PlacementConstraints för FaultAnalysisService. |
| StoredActionCleanupIntervalInSeconds | Int, standard är 3 600 |Detta är hur ofta hello store rensas.  Endast åtgärder i ett avslutat tillstånd; och som slutförts minst CompletedActionKeepDurationInSeconds sedan kommer att tas bort. |
| CompletedActionKeepDurationInSeconds | Int, standard är 604800 | Det här är ungefär hur länge tookeep åtgärder som är i ett avslutat tillstånd.  Detta beror också på StoredActionCleanupIntervalInSeconds; eftersom hello arbete toocleanup utförs endast på intervallet. 604800 är 7 dagar. |
| StoredChaosEventCleanupIntervalInSeconds | Int, standard är 3 600 |Detta är hur ofta hello store kommer att granskas för rensning; Om hello antalet händelser som är mer än 30000; rensning av hello kommer startar. |

### <a name="section-name-filestoreservice"></a>Avsnittsnamn: FileStoreService
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| NamingOperationTimeout |Tid i sekunder, standardvärdet är 60 |Ange tidsintervall i sekunder. hello tidsgräns för namngivning åtgärd utfördes. |
| QueryOperationTimeout | Tid i sekunder, standardvärdet är 60 |Ange tidsintervall i sekunder. hello timeout för att utföra frågeåtgärden igen. |
| MaxCopyOperationThreads | Uint, standardvärdet är 0 | hello maximalt antal parallella filer som sekundär kan kopiera från primära. '0' == antal kärnor. |
| MaxFileOperationThreads | Uint, standardvärdet är 100 | hello maximalt antal tillåtna tooperform FileOperations (kopiera/flytta)-parallell trådar i hello primära. '0' == antal kärnor. |
| MaxStoreOperations | Uint, standardvärdet är 4096 |hello maximalt antal tillåtna på den primära parallella store transcation åtgärder. '0' == antal kärnor. |
| MaxRequestProcessingThreads | Uint, standardinställningen är 200 |hello maximalt antal parallella trådar tillåtna tooprocess begäranden i hello primära. '0' == antal kärnor. |
| MaxSecondaryFileCopyFailureThreshold | Uint, standard är 25| hello maximalt antal filen kopia på hello sekundära innan den ger upp. |
| AnonymousAccessEnabled | Bool, standard är SANT |Aktivera/inaktivera anonym åtkomst toohello FileStoreService delar. |
| PrimaryAccountType | sträng, standardvärdet är ”” |hello primära AccountType av hello huvudnamn tooACL hello FileStoreService delar. |
| PrimaryAccountUserName | sträng, standardvärdet är ”” |hello primära konto användarnamnet för hello huvudnamn tooACL hello FileStoreService resurser. |
| PrimaryAccountUserPassword | SecureString, standard är tom |hello primära kontolösenord för hello huvudnamn tooACL hello FileStoreService delar. |
| FileStoreService | PrimaryAccountNTLMPasswordSecret | SecureString, standard är tom | hello lösenord hemlighet som används som startvärde toogenerated samma lösenord när du använder NTLM-autentisering. |
| PrimaryAccountNTLMX509StoreLocation | strängen, standard är ”LocalMachine”| hello lagringsplatsen för hello X509 certifikat som används för toogenerate HMAC på hello PrimaryAccountNTLMPasswordSecret när du använder NTLM-autentisering. |
| PrimaryAccountNTLMX509StoreName | strängen, standard är ”MY”| hello store namnet på hello X509 certifikat används toogenerate HMAC på hello PrimaryAccountNTLMPasswordSecret när du använder NTLM-autentisering. |
| PrimaryAccountNTLMX509Thumbprint | sträng, standardvärdet är ””|hello hello X509 certifikatets tumavtryck används toogenerate HMAC på hello PrimaryAccountNTLMPasswordSecret när du använder NTLM-autentisering. |
| SecondaryAccountType | sträng, standardvärdet är ””| hello sekundär AccountType av hello huvudnamn tooACL hello FileStoreService delar. |
| SecondaryAccountUserName | sträng, standardvärdet är ””| hello sekundära konto användarnamnet för hello huvudnamn tooACL hello FileStoreService resurser. |
| SecondaryAccountUserPassword | SecureString, standard är tom |hello sekundära kontolösenord för hello huvudnamn tooACL hello FileStoreService delar.  |
| SecondaryAccountNTLMPasswordSecret | SecureString, standard är tom | hello lösenord hemlighet som används som startvärde toogenerated samma lösenord när du använder NTLM-autentisering. |
| SecondaryAccountNTLMX509StoreLocation | strängen, standard är ”LocalMachine” |hello lagringsplatsen för hello X509 certifikat som används för toogenerate HMAC på hello SecondaryAccountNTLMPasswordSecret när du använder NTLM-autentisering. |
| SecondaryAccountNTLMX509StoreName | strängen, standard är ”MY” |hello store namnet på hello X509 certifikat används toogenerate HMAC på hello SecondaryAccountNTLMPasswordSecret när du använder NTLM-autentisering. |
| SecondaryAccountNTLMX509Thumbprint | sträng, standardvärdet är ””| hello hello X509 certifikatets tumavtryck används toogenerate HMAC på hello SecondaryAccountNTLMPasswordSecret när du använder NTLM-autentisering. |

### <a name="section-name-imagestoreservice"></a>Avsnittsnamn: ImageStoreService
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| Enabled |Bool, standard är FALSKT |hello aktiverad flaggan för ImageStoreService. |
| TargetReplicaSetSize | Int, standard är 7 |Hej TargetReplicaSetSize för ImageStoreService. |
| MinReplicaSetSize | Int, standardinställningen är 3 |Hej MinReplicaSetSize för ImageStoreService. |
| ReplicaRestartWaitDuration | Tid i sekunder är standard 60,0 * 30 | Ange tidsintervall i sekunder. Hej ReplicaRestartWaitDuration för ImageStoreService. |
| QuorumLossWaitDuration | Tid i sekunder är standardvärdet MaxValue | Ange tidsintervall i sekunder. Hej QuorumLossWaitDuration för ImageStoreService. |
| StandByReplicaKeepDuration | Tid i sekunder är standard 3600.0 * 2 | Ange tidsintervall i sekunder. Hej StandByReplicaKeepDuration för ImageStoreService. |
| PlacementConstraints | sträng, standardvärdet är ”” | Hej PlacementConstraints för ImageStoreService. |
| ClientUploadTimeout | Tid i sekunder är standardvärdet 1 800 |Ange tidsintervall i sekunder. Timeout-värdet för högsta nivån överför begäran tooImage Store-tjänsten. |
| ClientCopyTimeout | Tid i sekunder är standardvärdet 1 800 | Ange tidsintervall i sekunder. Timeout-värdet för högsta nivån kopiera begära tooImage Store-tjänsten. |
| ClientDownloadTimeout | Tid i sekunder är standardvärdet 1 800 | Ange tidsintervall i sekunder. Timeout-värdet för högsta nivån download begära tooImage Store-tjänsten |
| ClientListTimeout | Tid i sekunder, standard är 600 | Ange tidsintervall i sekunder. Timeout-värdet för översta listan begära tooImage Store-tjänsten. |
| ClientDefaultTimeout | Tid i sekunder, standard är 180 | Ange tidsintervall i sekunder. Timeout-värdet för alla icke-överför/icke-hämtningsbegäranden (t.ex. finns och ta bort) tooImage Store-tjänsten. |

### <a name="section-name-imagestoreclient"></a>Avsnittsnamn: ImageStoreClient
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| ClientUploadTimeout |Tid i sekunder är standardvärdet 1 800 | Ange tidsintervall i sekunder. Timeout-värdet för högsta nivån överför begäran tooImage Store-tjänsten. |
| ClientCopyTimeout | Tid i sekunder är standardvärdet 1 800 | Ange tidsintervall i sekunder. Timeout-värdet för högsta nivån kopiera begära tooImage Store-tjänsten. |
|ClientDownloadTimeout | Tid i sekunder är standardvärdet 1 800 | Ange tidsintervall i sekunder. Timeout-värdet för högsta nivån download begära tooImage Store-tjänsten. |
|ClientListTimeout | Tid i sekunder, standard är 600 |Ange tidsintervall i sekunder. Timeout-värdet för översta listan begära tooImage Store-tjänsten. |
|ClientDefaultTimeout | Tid i sekunder, standard är 180 | Ange tidsintervall i sekunder. Timeout-värdet för alla icke-överför/icke-hämtningsbegäranden (t.ex. finns och ta bort) tooImage Store-tjänsten. |

### <a name="section-name-tokenvalidationservice"></a>Avsnittsnamn: TokenValidationService
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| Leverantörer |strängen, standard är ”DSTS” |Kommaavgränsad lista över token validering providers tooenable (giltigt providers är: DSTS; AAD). För närvarande bara en enskild provider kan aktiveras när som helst. |

### <a name="section-name-upgradeorchestrationservice"></a>Avsnittsnamn: UpgradeOrchestrationService
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| TargetReplicaSetSize |Int, standardvärdet är 0 |Hej TargetReplicaSetSize för UpgradeOrchestrationService. |
| MinReplicaSetSize |Int, standardvärdet är 0 | Hej MinReplicaSetSize för UpgradeOrchestrationService.
| ReplicaRestartWaitDuration | Tid i sekunder, standardvärdet är 60 minuter| Ange tidsintervall i sekunder. Hej ReplicaRestartWaitDuration för UpgradeOrchestrationService. |
| QuorumLossWaitDuration | Tid i sekunder är standardvärdet MaxValue | Ange tidsintervall i sekunder. Hej QuorumLossWaitDuration för UpgradeOrchestrationService. |
| StandByReplicaKeepDuration | Tid i sekunder, standardvärdet är 60*24*7 minuter | Ange tidsintervall i sekunder. Hej StandByReplicaKeepDuration för UpgradeOrchestrationService. |
| PlacementConstraints | sträng, standardvärdet är ”” | Hej PlacementConstraints för UpgradeOrchestrationService. |
| AutoupgradeEnabled | Bool, standard är SANT | Automatisk avsökning och uppgraderingsåtgärden baserat på mål-tillståndsfil. |
| UpgradeApprovalRequired | Bool, standard är FALSKT | Inställningen toomake koduppgradering kräver administratörsgodkännande innan du fortsätter. |

### <a name="section-name-upgradeservice"></a>Avsnittsnamn: UpgradeService
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| PlacementConstraints |sträng, standardvärdet är ”” |Hej PlacementConstraints för uppgradering. |
| TargetReplicaSetSize | Int, standardinställningen är 3 | Hej TargetReplicaSetSize för UpgradeService. |
| MinReplicaSetSize | Int, standardvärdet är 2 | Hej MinReplicaSetSize för UpgradeService. |
| CoordinatorType | strängen, standard är ”WUTest”| Hej CoordinatorType för UpgradeService. |
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
| ReportUpgradeHealth |String, standard är ”Admin” | Säkerhetskonfiguration för återupptar programuppgraderingar med hello aktuella uppgraderingsprocessen. |
| ReportHealth |String, standard är ”Admin” | Säkerhetskonfiguration för rapportering hälsa. |
| ProvisionFabric |String, standard är ”Admin” | Säkerhetskonfiguration för MSI och/eller klusternamnresursen manifestet etablering. |
| UpgradeFabric |String, standard är ”Admin” | För att starta klusteruppgradering säkerhetskonfiguration. |
| RollbackFabricUpgrade |String, standard är ”Admin” | För att återställa klusteruppgradering säkerhetskonfiguration. |
| UnprovisionFabric |String, standard är ”Admin” | Säkerhetskonfiguration för MSI och/eller klusternamnresursen manifestet avetablering. |
| MoveNextFabricUpgradeDomain |String, standard är ”Admin” | Säkerhetskonfiguration för återupptar klusteruppgradering med en explicit uppgraderingen domän. |
| ReportFabricUpgradeHealth |String, standard är ”Admin” | Säkerhetskonfiguration för återupptar klusteruppgradering med hello aktuella uppgraderingsprocessen. |
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
| FileContent |String, standard är ”Admin” | Säkerhetskonfiguration för avbildningen lagra klienten filöverföring (extern toocluster). |
| FileDownload |String, standard är ”Admin” | Säkerhetskonfiguration för image store klienten filen download inleda (extern toocluster). |
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
| StartPartitionRestart |String, standard är ”Admin” | Startar samtidigt om vissa eller alla hello repliker för en partition. |
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
| GetPartitionDataLossProgress | strängen, standard är ”Admin\|\|Användaren ” | Hämtar hello pågår för en api-anrop invoke data går förlorade. |
| GetPartitionQuorumLossProgress | strängen, standard är ”Admin\|\|Användaren ” | Hämtar hello pågår för en invoke kvorum förlust api-anrop. |
| GetPartitionRestartProgress | strängen, standard är ”Admin\|\|Användaren ” | Hämtar hello pågår för en omstart partition api-anrop. |
| GetChaosReport | strängen, standard är ”Admin\|\|Användaren ” | Hämtar hello status kaotisk inom ett angivet tidsintervall. |
| GetNodeTransitionProgress | strängen, standard är ”Admin\|\|Användaren ” | Säkerhetskonfiguration för att hämta pågår på en nod övergång. |
| GetClusterConfigurationUpgradeStatus | strängen, standard är ”Admin\|\|Användaren ” | Startar GetClusterConfigurationUpgradeStatus på en partition. |
| GetClusterConfiguration | strängen, standard är ”Admin\|\|Användaren ” | Startar GetClusterConfiguration på en partition. |

### <a name="section-name-reconfigurationagent"></a>Avsnittsnamn: ReconfigurationAgent
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| ApplicationUpgradeMaxReplicaCloseDuration | Tid i sekunder är standardvärdet 900 |Ange tidsintervall i sekunder. hello varaktighet för vilka hello systemet väntar innan försöket avbryts värdar som har repliker som fastnat i slutet. |
| ServiceApiHealthDuration | Tid i sekunder, standardinställningen är 30 minuter | Ange tidsintervall i sekunder. ServiceApiHealthDuration definierar hur länge vi vänta på en service API toorun innan vi rapporterar den feltillstånd. |
| ServiceReconfigurationApiHealthDuration | Tid i sekunder, standardvärdet är 30 | Ange tidsintervall i sekunder. ServiceReconfigurationApiHealthDuration definierar hur länge hello innan en tjänst i omkonfiguration rapporteras som ohälsosam. |
| PeriodicApiSlowTraceInterval | Tid i sekunder, standardvärdet är 5 minuter | Ange tidsintervall i sekunder. PeriodicApiSlowTraceInterval definierar hello intervallet över vilket långsam API-anrop ska fångas av hello API-Övervakare. |
| NodeDeactivationMaxReplicaCloseDuration | Tid i sekunder är standardvärdet 900 | Ange tidsintervall i sekunder. hello maximal tid toowait innan tjänstvärden som blockerar nodinaktiveringen. |
| FabricUpgradeMaxReplicaCloseDuration | Tid i sekunder är standardvärdet 900 | Ange tidsintervall i sekunder. hello maximal varaktighet RA ska vänta innan tjänstvärden replik som inte avslutas. |
| IsDeactivationInfoEnabled | Bool, standard är SANT | Avgör om RA använda inaktiveringen information för att utföra primära ny val för nya kluster med den här konfigurationen ska anges tootrue för befintliga kluster som ska uppgraderas finns hello viktig information om hur tooenable detta. |

### <a name="section-name-placementandloadbalancing"></a>Avsnittsnamn: PlacementAndLoadBalancing
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| TraceCRMReasons |Bool, standard är SANT |Anger om tootrace orsaker till CRM utfärdats förflyttningar toohello Funktionshändelser kanal. |
| ValidatePlacementConstraint | Bool, standard är SANT | Anger huruvida hello PlacementConstraint uttrycket för en tjänst verifieras när en tjänst ServiceDescription uppdateras. |
| PlacementConstraintValidationCacheSize | Int, standard är 10 000 | Gränser hello storleken på hello tabell används för snabb validering och cachelagring av placering av begränsningsuttryck. |
|VerboseHealthReportLimit | Int, standardvärdet är 20 | Definierar hello antalet gånger som en replik har toogo Ej placerade innan en varning om hälsotillstånd rapporteras för det (om utförlig hälsa reporting är aktiverad). |
|ConstraintViolationHealthReportLimit | Int, standardvärdet är 50 | Definierar hello antalet gånger som villkoret brott mot replik har toobe beständigt olöst innan diagnostik utförs och hälsorapporter släpps. |
|DetailedConstraintViolationHealthReportLimit | Int, standardinställningen är 200 | Definierar hello antalet gånger som villkoret brott mot replik har toobe beständigt olöst innan diagnostik utförs och detaljerad hälsorapporter släpps. |
|DetailedVerboseHealthReportLimit | Int, standardinställningen är 200 | Definierar hello antalet gånger som en replik av en Ej placerade har toobe beständigt Ej placerade innan detaljerad hälsa rapporter släpps. |
|ConsecutiveDroppedMovementsHealthReportLimit | Int, standardvärdet är 20 | Definierar hello många gånger i rad som utfärdats ResourceBalancer förflyttningar ignoreras innan diagnostik utförs och hälsovarningar släpps. Negativt: Inga varningar orsakat under det här villkoret. |
|DetailedNodeListLimit | Int, standardvärdet är 15 | Definierar hello antalet noder per begränsningen tooinclude innan trunkering i hello Ej placerade replik rapporter. |
|DetailedPartitionListLimit | Int, standardvärdet är 15 | Definierar hello antalet partitioner per diagnostiska post för en begränsning tooinclude innan trunkering i diagnostik. |
|DetailedDiagnosticsInfoListLimit | Int, standardvärdet är 15 | Definierar hello antal diagnostikposter (med detaljerad information) per begränsningen tooinclude innan trunkering i diagnostik.|
|PLBRefreshGap | Tid i sekunder är standardvärdet 1 | Ange tidsintervall i sekunder. Definierar hello kortaste tidsperiod som måste passera innan PLB uppdaterar tillstånd igen. |
|MinPlacementInterval | Tid i sekunder är standardvärdet 1 | Ange tidsintervall i sekunder. Definierar hello kortaste tidsperiod som ska förflyta innan två på varandra följande placering Avrundar. |
|MinConstraintCheckInterval | Tid i sekunder är standardvärdet 1 | Ange tidsintervall i sekunder. Definierar hello kortaste tidsperiod som måste passera innan två på varandra följande begränsningen Kontrollera Avrundar. |
|MinLoadBalancingInterval | Tid i sekunder, standardvärdet är 5 | Ange tidsintervall i sekunder. Definierar hello kortaste tidsperiod som ska förflyta innan två på varandra följande belastningsutjämning Avrundar. |
|BalancingDelayAfterNodeDown | Tid i sekunder, standardvärdet är 120 |Ange tidsintervall i sekunder. Starta inte NLB aktiviteter inom denna period efter en nod av händelsen. |
|BalancingDelayAfterNewNode | Tid i sekunder, standardvärdet är 120 |Ange tidsintervall i sekunder. Starta inte NLB aktiviteter inom den tiden när du lägger till en ny nod. |
|ConstraintFixPartialDelayAfterNodeDown | Tid i sekunder, standardvärdet är 120 | Ange tidsintervall i sekunder. Gör inte åtgärda FaultDomain och UpgradeDomain begränsningen överträdelser inom denna period efter en nod av händelsen. |
|ConstraintFixPartialDelayAfterNewNode | Tid i sekunder, standardvärdet är 120 | Ange tidsintervall i sekunder. DDo inte åtgärda FaultDomain och UpgradeDomain begränsningen överträdelser inom den tiden när du lägger till en ny nod. |
|GlobalMovementThrottleThreshold | Uint, standardvärdet är 1000 | Maximalt antal förflyttningar tillåts i hello NLB fas i hello tidigare intervall som anges av GlobalMovementThrottleCountingInterval. |
|GlobalMovementThrottleThresholdForPlacement | Uint, standardvärdet är 0 | Maximalt antal förflyttningar tillåten i placering fas i hello tidigare intervall som anges av GlobalMovementThrottleCountingInterval.0 anger att ingen gräns.|
|GlobalMovementThrottleThresholdForBalancing | Uint, standardvärdet är 0 | Maximalt antal förflyttningar tillåten i NLB fas i hello tidigare intervall som anges av GlobalMovementThrottleCountingInterval. 0 anger att ingen gräns. |
|GlobalMovementThrottleCountingInterval | Tid i sekunder, standard är 600 | Ange tidsintervall i sekunder. Ange hello lång hello tidigare intervall för vilka tootrack per domän replik förflyttningar (används tillsammans med GlobalMovementThrottleThreshold). Du kan ange too0 tooignore global begränsning helt och hållet. |
|MovementPerPartitionThrottleThreshold | Uint, standardvärdet är 50 | Ingen belastningsutjämning relaterade förflyttning utförs för en partition om hello antalet NLB relaterade förflyttningar för repliker av den aktuella partitionen har nått eller överskridit MovementPerFailoverUnitThrottleThreshold i hello tidigare intervall som anges av MovementPerPartitionThrottleCountingInterval. |
|MovementPerPartitionThrottleCountingInterval | Tid i sekunder, standard är 600 | Ange tidsintervall i sekunder. Ange hello lång hello tidigare intervall för vilka tootrack replik förflyttningar för varje partition (används tillsammans med MovementPerPartitionThrottleThreshold). |
|PlacementSearchTimeout | Tid i sekunder, standardvärdet är 0,5 | Ange tidsintervall i sekunder. När du monterar tjänster. Sök efter högst i långt innan ett resultat returneras. |
|UseMoveCostReports | Bool, standard är FALSKT | Instruerar hello LB tooignore hello kostnaden element av hello bedömningen funktion. resulterande potentiellt stora antal flyttar för bättre belastningsutjämnade placering. |
|PreventTransientOvercommit | Bool, standard är FALSKT | Anger bör PLB omedelbart förlita dig på resurser som kan frigöras genom hello initierade flyttar. Som standard. PLB kan initiera flytta ut och flytta i på samma nod som kan skapa tillfälliga överanstränga hello. Inställningen som förhindrar att den här parametern tootrue de typer av overcommits och på begäran defragmentering (aka placementWithMove) kommer att inaktiveras. |
|InBuildThrottlingEnabled | Bool, standard är FALSKT | Bestämma om hello i build-begränsning är aktiverad. |
|InBuildThrottlingAssociatedMetric | sträng, standardvärdet är ”” | hello associerade Måttnamn för den här begränsningen. |
|InBuildThrottlingGlobalMaxValue | Int, standardvärdet är 0 |hello maxantalet i skapa-repliker tillåts globalt. |
|SwapPrimaryThrottlingEnabled | Bool, standard är FALSKT| Bestämma om hello swap-primära begränsning är aktiverad. |
|SwapPrimaryThrottlingAssociatedMetric | sträng, standardvärdet är ””| hello associerade Måttnamn för den här begränsningen. |
|SwapPrimaryThrottlingGlobalMaxValue | Int, standardvärdet är 0 | hello maxantalet swap-primära repliker tillåts globalt. |
|PlacementConstraintPriority | Int, standardvärdet är 0 | Bestämmer hello prioritet för placering begränsningen: 0: hårda; 1: mjuk; negativt: ignorera. |
|PreferredLocationConstraintPriority | Int, standardvärdet är 2| Anger hello prioriteten för önskade platsbegränsningen: 0: hårda; 1: mjuk; 2: optimering; negativt: ignorera |
|CapacityConstraintPriority | Int, standardvärdet är 0 | Bestämmer hello prioritet för kapacitet begränsningen: 0: hårda; 1: mjuk; negativt: ignorera. |
|AffinityConstraintPriority | Int, standardvärdet är 0 | Bestämmer hello prioritet för mappning mellan begränsningen: 0: hårda; 1: mjuk; negativt: ignorera. |
|FaultDomainConstraintPriority | Int, standardvärdet är 0 | Anger hello prioriteten för domänbegränsningar fel: 0: hårda; 1: mjuk; negativt: ignorera. |
|UpgradeDomainConstraintPriority | Int, standardvärdet är 1| Bestämmer hello prioritet på uppgraderingsdomänen begränsning: 0: hårda; 1: mjuk; negativt: ignorera. |
|ScaleoutCountConstraintPriority | Int, standardvärdet är 0 | Anger hello prioriteten för scaleout antal begränsning: 0: hårda; 1: mjuk; negativt: ignorera. |
|ApplicationCapacityConstraintPriority | Int, standardvärdet är 0 | Bestämmer hello prioritet för kapacitet begränsningen: 0: hårda; 1: mjuk; negativt: ignorera. |
|MoveParentToFixAffinityViolation | Bool, standard är FALSKT | Inställningen som bestämmer om överordnade repliker kan flytta toofix tillhörighet begränsningar.|
|MoveExistingReplicaForPlacement | Bool, standard är SANT |Inställning som anger om toomove befintliga repliken under placeringen. |
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
| ServiceTypeRegistrationTimeout |Tid i sekunder, standard är 300 |Högsta tillåtna tiden för hello ServiceType toobe registrerats med fabric |
| ServiceTypeDisableFailureThreshold |Heltal, standard är 1 |Detta är hello tröskelvärdet för hello Felberäkning efter vilken FailoverManager (FM) är ett meddelande toodisable hello tjänsttypen på den noden och prova en annan nod för placering. |
| ActivationRetryBackoffInterval |Tid i sekunder, standardvärdet är 5 |Backoff intervall på varje Aktiveringsfel; På varje kontinuerlig Aktiveringsfel hello hello system återförsök aktivering för in toohello MaxActivationFailureCount. hello återförsöksintervall på varje försök är en produkt från kontinuerlig felet och hello aktivering inte aktiveringsintervallet. |
| ActivationMaxRetryInterval |Tid i sekunder, standard är 300 |På varje kontinuerlig Aktiveringsfel hello hello system återförsök aktivering för in tooActivationMaxFailureCount. ActivationMaxRetryInterval Anger tidsintervall för vänta innan ett återförsök görs efter varje Aktiveringsfel |
| ActivationMaxFailureCount |Heltal, standard är 10 |Antal gånger som system för nya försök misslyckats aktivering innan den ger upp |

### <a name="section-name-failovermanager"></a>Avsnittsnamn: FailoverManager
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| PeriodicLoadPersistInterval |Tid i sekunder är standard 10 |Detta avgör hur ofta hello FM Sök efter nya belastningen rapporter |

### <a name="section-name-federation"></a>Avsnittsnamn: Federation
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| LeaseDuration |Tid i sekunder, standardvärdet är 30 |Varaktighet som ett lån pågår mellan en nod och sina grannar. |
| LeaseDurationAcrossFaultDomain |Tid i sekunder, standardvärdet är 30 |Varaktighet som ett lån pågår mellan en nod och sina grannar över feldomäner. |

### <a name="section-name-clustermanager"></a>Avsnittsnamn: ClusterManager
| **Parametern** | **Tillåtna värden** | **Vägledning eller en kort beskrivning** |
| --- | --- | --- |
| UpgradeStatusPollInterval |Tid i sekunder, standardvärdet är 60 |hello frekvens för avsökning för uppgraderingsstatus för programmet. Det här värdet fastställer hello frekvensen för uppdatering för alla GetApplicationUpgradeProgress-anrop |
| UpgradeHealthCheckInterval |Tid i sekunder, standardvärdet är 60 |hello frekvens hälsostatus kontrollerar under en uppgradering för övervakade program |
| FabricUpgradeStatusPollInterval |Tid i sekunder, standardvärdet är 60 |hello frekvens för avsökning för Fabric uppgraderingsstatus. Det här värdet fastställer hello frekvensen för uppdatering för alla GetFabricUpgradeProgress-anrop |
| FabricUpgradeHealthCheckInterval |Tid i sekunder, standardvärdet är 60 |hello frekvensen för kontroll av hälsostatusen under en övervakad Fabric-uppgradering |
|InfrastructureTaskProcessingInterval | Tid i sekunder är standard 10 |Ange tidsintervall i sekunder. hello bearbetning intervall som används av hello infrastruktur uppgiften bearbetning tillståndsdatorn. |
|TargetReplicaSetSize |Int, standard är 7 |Hej TargetReplicaSetSize för ClusterManager. |
|MinReplicaSetSize |Int, standardinställningen är 3 |Hej MinReplicaSetSize för ClusterManager. |
|ReplicaRestartWaitDuration |Tid i sekunder är standardvärdet (60,0 * 30)|Ange tidsintervall i sekunder. Hej ReplicaRestartWaitDuration för ClusterManager. |
|QuorumLossWaitDuration |Tid i sekunder är standardvärdet MaxValue | Ange tidsintervall i sekunder. Hej QuorumLossWaitDuration för ClusterManager. |
|StandByReplicaKeepDuration | Tid i sekunder är standardvärdet (3600.0 * 2)|Ange tidsintervall i sekunder. Hej StandByReplicaKeepDuration för ClusterManager. |
|PlacementConstraints | sträng, standardvärdet är ”” |Hej PlacementConstraints för ClusterManager. |
|SkipRollbackUpdateDefaultService | Bool, standard är FALSKT |hello CM hoppar över Återför uppdaterade standardtjänster under uppgraderingen återställningen av programmet. |
|EnableDefaultServicesUpgrade | Bool, standard är FALSKT |Aktivera uppgradering standardtjänster under uppgradering av programmet. Standardtjänsterna skulle skrivas efter uppgraderingen. |
|InfrastructureTaskHealthCheckWaitDuration |Tid i sekunder, standardvärdet är 0| Ange tidsintervall i sekunder. hello mängden tid toowait innan du startar hälsokontroller när aktiviteten en infrastruktur för efterbearbetning. |
|InfrastructureTaskHealthCheckStableDuration | Tid i sekunder, standardvärdet är 0| Ange tidsintervall i sekunder. hello mängden tid tooobserve på varandra följande skickade hälsa kontrollerar innan efter bearbetningen av en infrastruktur-aktivitet har slutförts. Sett misslyckade hälsokontrollen återställs den här timern. |
|InfrastructureTaskHealthCheckRetryTimeout | Tid i sekunder, standardvärdet är 60 |Ange tidsintervall i sekunder. Det gick inte att hälsokontroller när aktiviteten en infrastruktur för efterbearbetning hello mängden tid toospend försöker igen. Sett skickade hälsokontrollen återställs den här timern. |
|ImageBuilderTimeoutBuffer |Tid i sekunder, standardinställningen är 3 |Ange tidsintervall i sekunder. hello mängden tid tooallow för Image Builder specifika timeout-fel tooreturn toohello klienten. Om den här bufferten är för liten. sedan hello klienten timeout innan hello-servern och hämtar en allmän timeout-fel. |
|MinOperationTimeout | Tid i sekunder, standardvärdet är 60 |Ange tidsintervall i sekunder. hello minsta globala tidsgränsen för internt bearbetning vid ClusterManager. |
|MaxOperationTimeout |Tid i sekunder är standardvärdet MaxValue | Ange tidsintervall i sekunder. hello maximala globala tidsgränsen för internt bearbetning vid ClusterManager. |
|MaxTimeoutRetryBuffer | Tid i sekunder, standard är 600 |Ange tidsintervall i sekunder. Hej tidsgräns för maximal åtgärd när internt försöker du igen på grund av tootimeouts är <Original Timeout>  +  <MaxTimeoutRetryBuffer>. Ytterligare timeout har lagts till i steg om MinOperationTimeout. |
|MaxCommunicationTimeout |Tid i sekunder, standard är 600 |Ange tidsintervall i sekunder. Hej Maximal tidsgräns för intern kommunikation mellan ClusterManager och andra systemtjänster (d.v.s.; Namntjänst; Failover Manager och osv). Det här bör vara mindre än globala MaxOperationTimeout (eftersom det kan finnas flera kommunikation mellan komponenter för varje klientåtgärd). |
|MaxDataMigrationTimeout |Tid i sekunder, standard är 600 |Ange tidsintervall i sekunder. Hej Maximal tidsgräns för återställning av data migreringsåtgärder när en Fabric-uppgraderingen har ägt rum. |
|MaxOperationRetryDelay |Tid i sekunder, standardvärdet är 5| Ange tidsintervall i sekunder. hello Maximal fördröjning för interna återförsök när fel uppstår. |
|ReplicaSetCheckTimeoutRollbackOverride |Tid i sekunder är standardvärdet 1200 | Ange tidsintervall i sekunder. Om ReplicaSetCheckTimeout anges toohello maximala värdet för DWORD; sedan åsidosätts den med hello värdet för den här konfigurationen för hello tillämpningen av återställningen. hello-värde som används för loggsäkerhetskopian åsidosätts aldrig. |
|ImageBuilderJobQueueThrottle |Int, standardvärdet är 10 |Tråden antal begränsning för Image Builder proxy jobbkön på programförfrågningar. |

## <a name="next-steps"></a>Nästa steg
Läs artiklarna mer information om hantering av kluster:

[Lägga till, rulla över, ta bort certifikat från ditt Azure-kluster](service-fabric-cluster-security-update-certs-azure.md) 

