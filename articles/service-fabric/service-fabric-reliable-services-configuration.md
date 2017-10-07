---
title: "aaaConfigure tillförlitliga Azure mikrotjänster | Microsoft Docs"
description: "Lär dig mer om hur du konfigurerar tillståndskänsliga Reliable Services i Azure Service Fabric."
services: Service-Fabric
documentationcenter: .net
author: sumukhs
manager: timlt
editor: vturecek
ms.assetid: 9f72373d-31dd-41e3-8504-6e0320a11f0e
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 6/29/2017
ms.author: sumukhs
ms.openlocfilehash: 1e9c2890b62890a777561f25c04dc0fd11db9f1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-stateful-reliable-services"></a>Konfigurera tillståndskänsliga reliable services
Det finns två uppsättningar av konfigurationsinställningar för tillförlitlig tjänster. En uppsättning är globala för alla tillförlitliga tjänster i hello klustret medan hello andra är särskilda tooa viss tillförlitlig tjänst.

## <a name="global-configuration"></a>Global konfiguration
hello tjänsten för global tillförlitlig konfiguration har angetts i hello klustermanifestet för hello kluster under hello KtlLogger avsnitt. Det gör att konfigurationen av hello delade loggen platsen och storleken plus hello globalt minnesgränser som används av hello loggaren. Hej klustermanifestet är en XML-fil som innehåller inställningar och konfigurationer som gäller tooall noder och tjänster i hello klustret. ClusterManifest.xml kallas vanligtvis hello-filen. Du kan se hello klustermanifestet för klustret med hjälp av hello Get-ServiceFabricClusterManifest powershell-kommando.

### <a name="configuration-names"></a>Konfigurationsnamn
| Namn | Enhet | Standardvärde | Kommentarer |
| --- | --- | --- | --- |
| WriteBufferMemoryPoolMinimumInKB |Kilobyte |8388608 |Minsta antal KB tooallocate i kernel-läge för hello loggaren skriva buffert minnespoolen. Den här minnespoolen används för cachelagring statusinformation innan toodisk. |
| WriteBufferMemoryPoolMaximumInKB |Kilobyte |Obegränsat |Maxstorlek toowhich hello loggaren skrivåtgärder minne buffertpool kan växa. |
| SharedLogId |GUID |"" |Anger en unik GUID-toouse för att identifiera hello delade Standardloggfilen används av alla tillförlitliga tjänster på alla noder i klustret hello som inte anger hello SharedLogId i deras specifika tjänstekonfigurationen. Om SharedLogId anges måste även SharedLogPath anges. |
| SharedLogPath |Fullständig sökväg |"" |Anger hello fullständigt kvalificerade sökvägen där hello delade loggfil som används av alla tillförlitliga tjänster på alla noder i klustret hello som inte anger hello SharedLogPath i deras specifika tjänstekonfigurationen. Men om SharedLogPath anges måste sedan SharedLogId också anges. |
| SharedLogSizeInMB |Megabyte |8192 |Anger numret på hello MB diskutrymme toostatically allokera för delade hello-loggen. hello-värdet måste vara 2048 eller större. |

I Azure ARM eller lokala JSON-mall visar hello exemplet nedan hur skapade toochange hello hello delade transaktionsloggen som hämtar tooback några tillförlitliga samlingar för tillståndskänsliga tjänster.

    "fabricSettings": [{
        "name": "KtlLogger",
        "parameters": [{
            "name": "SharedLogSizeInMB",
            "value": "4096"
        }]
    }]

### <a name="sample-local-developer-cluster-manifest-section"></a>Exempel lokala developer klustret manifest-delen
Om du vill toochange detta på din lokala utvecklingsmiljö måste ha du tooedit hello lokala clustermanifest.xml filen.

```xml
   <Section Name="KtlLogger">
     <Parameter Name="SharedLogSizeInMB" Value="4096"/>
     <Parameter Name="WriteBufferMemoryPoolMinimumInKB" Value="8192" />
     <Parameter Name="WriteBufferMemoryPoolMaximumInKB" Value="8192" />
     <Parameter Name="SharedLogId" Value="{7668BB54-FE9C-48ed-81AC-FF89E60ED2EF}"/>
     <Parameter Name="SharedLogPath" Value="f:\SharedLog.Log"/>
   </Section>
```

### <a name="remarks"></a>Kommentarer
hello loggaren har en global pool av minne som allokerats från icke växlingsbart kernelminne tillgängliga tooall reliable Services på en nod för att cachelagra tillståndsdata innan de skrivs toohello dedikerade loggen som är associerade med hello tillförlitlig tjänst replik. hello poolstorlek styrs av hello WriteBufferMemoryPoolMinimumInKB och WriteBufferMemoryPoolMaximumInKB inställningar. WriteBufferMemoryPoolMinimumInKB anger både hello storleken på den här minnespoolen och hello lägsta storlek toowhich hello minnespoolen kan förminskas. WriteBufferMemoryPoolMaximumInKB är hello högsta storlek toowhich hello minnespoolen kan växa. Varje tillförlitlig tjänst replik som har öppnats kan öka hello storlek hello minnespoolen med ett system som är fastställt belopp upp tooWriteBufferMemoryPoolMaximumInKB. Om det finns flera behovet av minne från hello minnespoolen än vad som är tillgängliga, begäranden för minne kommer att skjutas upp tills minne är tillgänglig. Därför om hello skrivåtgärder buffertpool minne är för litet för en specifik konfiguration påverkas sedan prestanda negativt.

Hej SharedLogId och SharedLogPath är alltid användas tillsammans toodefine hello GUID och plats för hello standard delade loggen för alla noder i klustret hello. hello standard delade loggen används för alla tillförlitliga tjänster som inte anger hello inställningar i hello settings.xml för hello specifik tjänst. För bästa prestanda bör delade loggfiler placeras på diskar som används enbart för hello delade log file tooreduce konkurrens.

SharedLogSizeInMB anger hello disk space toopreallocate för hello standard delade loggen på alla noder.  SharedLogId och SharedLogPath behöver inte toobe som angetts för SharedLogSizeInMB toobe anges.

## <a name="service-specific-configuration"></a>Tjänstspecifik konfiguration
Du kan ändra standardkonfigurationer för tillståndskänsliga Reliable Services genom att använda hello konfigurationspaket (Config) eller hello implementering (kod).

* **Config** -konfigurationen via hello konfigurationspaketet kan åstadkommas genom att ändra hello Settings.xml fil som har genererats i hello Microsoft Visual Studio paketrot under hello Config mapp för varje tjänst i hello program.
* **Koden** -konfigurationen via kod kan åstadkommas genom att skapa en ReliableStateManager med ett ReliableStateManagerConfiguration-objekt med hello lämpliga alternativ.

Som standard hello Azure Service Fabric runtime söker efter fördefinierade Avsnittsnamnen i hello Settings.xml filen och förbrukar hello konfigurationsvärden när du skapar hello underliggande körtidskomponenter.

> [!NOTE]
> Gör **inte** ta bort hello Avsnittsnamnen av hello följande konfigurationer i hello Settings.xml-filen som har genererats i hello Visual Studio-lösning om du planerar tooconfigure tjänsten via kod.
> Byta namn på hello config paket eller avsnittet namn kommer att kräva en kodändring när du konfigurerar hello ReliableStateManager.
> 
> 

### <a name="replicator-security-configuration"></a>Replikatorn säkerhetskonfiguration
Replikatorn säkerhetskonfigurationer är används toosecure hello kommunikationskanalen som används vid replikering. Det innebär att tjänster inte kan toosee varandras replikeringstrafik, säkerställer att hello data som görs högtillgänglig även skydda. Som standard förhindrar en tom säkerhetskonfigurationsavsnittet replikeringssäkerhet.

### <a name="default-section-name"></a>Avsnittet för standardnamnet
ReplicatorSecurityConfig

> [!NOTE]
> toochange Avsnittsnamnet, Åsidosätt hello replicatorSecuritySectionName parametern toohello ReliableStateManagerConfiguration konstruktor när du skapar hello ReliableStateManager för den här tjänsten.
> 
> 

### <a name="replicator-configuration"></a>Replikatorn konfiguration
Konfigurationer för replikatorn konfigurera hello replikatorn som ansvarar för att göra hello tillståndskänsliga Reliable tjänstens tillstånd mycket pålitlig genom att replikera och spara hello tillstånd lokalt.
hello standardkonfigurationen genereras av hello Visual Studio-mall och bör vara tillräckligt. Det här avsnittet innehåller information om ytterligare konfigurationer som är tillgängliga tootune hello replikatorn.

### <a name="default-section-name"></a>Avsnittet för standardnamnet
ReplicatorConfig

> [!NOTE]
> toochange Avsnittsnamnet, Åsidosätt hello replicatorSettingsSectionName parametern toohello ReliableStateManagerConfiguration konstruktor när du skapar hello ReliableStateManager för den här tjänsten.
> 
> 

### <a name="configuration-names"></a>Konfigurationsnamn
| Namn | Enhet | Standardvärde | Kommentarer |
| --- | --- | --- | --- |
| BatchAcknowledgementInterval |Sekunder |0.015 |Tidsperiod för vilken hello replikatorn på hello sekundära väntar när du har fått en åtgärd innan du skickar tillbaka en bekräftelse toohello primära. Alla andra bekräftelser toobe skickas för åtgärder som behandlas inom intervallet skickas som ett svar. |
| ReplicatorEndpoint |Saknas |Ingen standard--obligatorisk parameter |IP-adress och port som hello primära och sekundära replikatorn använder toocommunicate med andra replikatörer i hello replikuppsättningen. Detta bör referera en TCP-slutpunkt för resurs i hello service manifest. Se för[Service manifest resurser](service-fabric-service-manifest-resources.md) tooread mer om hur du definierar endpoint resurser i en service manifest. |
| MaxPrimaryReplicationQueueSize |Antal åtgärder |8192 |Maximalt antal åtgärder i hello primära kön. En åtgärd frigjorts när hello primära replikatorn tar emot en bekräftelse från alla hello sekundära replikatörer. Det här värdet måste vara större än 64 och delbart med 2. |
| MaxSecondaryReplicationQueueSize |Antal åtgärder |16384 |Maximalt antal åtgärder i hello sekundär kö. När du har gjort tillståndet hög tillgänglighet via beständiga frigjorts en åtgärd. Det här värdet måste vara större än 64 och delbart med 2. |
| CheckpointThresholdInMB |MB |50 |Mängden utrymme i loggfilen efter vilken hello tillstånd är kontrollpunkt. |
| MaxRecordSizeInKB |kB |1024 |Största storlek som poster som hello replikatorn kan skriva i hello-loggen. Det här värdet måste vara en multipel av 4 och större än 16. |
| MinLogSizeInMB |MB |0 (system fastställa) |Minimistorlek hello transaktionella loggen. hello loggen tillåts inte tootruncate tooa storlek under den här inställningen. 0 anger att hello replikatorn avgör hello minsta loggfilens storlek. Ökar det här värdet ökas hello möjligheten att göra partiella kopior och inkrementella säkerhetskopieringar eftersom risken för relevanta loggposter trunkerades sänks. |
| TruncationThresholdFactor |Faktor |2 |Anger i vilken storlek hello loggen trunkering ska utlösas. Trunkering tröskelvärdet bestäms av MinLogSizeInMB multiplicerat med TruncationThresholdFactor. TruncationThresholdFactor måste vara större än 1. MinLogSizeInMB * TruncationThresholdFactor måste vara mindre än MaxStreamSizeInMB. |
| ThrottlingThresholdFactor |Faktor |4 |Anger hur stor hello loggen hello replik påbörjas begränsas. Bandbreddsbegränsning tröskelvärdet (i MB) bestäms av Max ((MinLogSizeInMB * ThrottlingThresholdFactor),(CheckpointThresholdInMB * ThrottlingThresholdFactor)). Bandbreddsbegränsning tröskelvärdet (i MB) måste vara större än trunkering tröskelvärdet (i MB). Trunkering tröskelvärdet (i MB) måste vara mindre än MaxStreamSizeInMB. |
| MaxAccumulatedBackupLogSizeInMB |MB |800 |Max ackumulerade storleken (i MB) för säkerhetskopiering loggar i en viss loggkedja. En inkrementell säkerhetskopiering begäranden misslyckas om hello inkrementell säkerhetskopiering skulle Generera en säkerhetskopiering logg som skulle orsaka hello ackumulerade säkerhetskopieringsloggar sedan hello relevanta fullständig säkerhetskopiering toobe större än den här storleken. I sådana fall måste är användaren obligatoriska tootake en fullständig säkerhetskopia. |
| SharedLogId |GUID |"" |Anger en unik GUID-toouse för att identifiera hello delade loggfilen används med den här repliken. Tjänster ska normalt inte använda den här inställningen. Men om SharedLogId anges måste sedan SharedLogPath också anges. |
| SharedLogPath |Fullständig sökväg |"" |Anger hello fullständigt kvalificerade sökvägen där hello delade loggfilen för den här replikeringen kommer att skapas. Tjänster ska normalt inte använda den här inställningen. Men om SharedLogPath anges måste sedan SharedLogId också anges. |
| SlowApiMonitoringDuration |Sekunder |300 |Anger hello Övervakningsintervall för hanterade API-anrop. Exempel: säkerhetskopiering Återanropsfunktionen angavs av användaren. När hello intervall har överskridits, skickas en varning hälsorapport toohello Health Manager. |

### <a name="sample-configuration-via-code"></a>Exempel på konfiguration via kod
```csharp
class Program
{
    /// <summary>
    /// This is hello entry point of hello service host process.
    /// </summary>
    static void Main()
    {
        ServiceRuntime.RegisterServiceAsync("HelloWorldStatefulType",
            context => new HelloWorldStateful(context, 
                new ReliableStateManager(context, 
        new ReliableStateManagerConfiguration(
                        new ReliableStateManagerReplicatorSettings()
            {
                RetryInterval = TimeSpan.FromSeconds(3)
                        }
            )))).GetAwaiter().GetResult();
    }
}    
```
```csharp
class MyStatefulService : StatefulService
{
    public MyStatefulService(StatefulServiceContext context, IReliableStateManagerReplica stateManager)
        : base(context, stateManager)
    { }
    ...
}
```


### <a name="sample-configuration-file"></a>Exempel på konfigurationsfil
```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="ReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="ReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
      <Parameter Name="CheckpointThresholdInMB" Value="512" />
   </Section>
   <Section Name="ReplicatorSecurityConfig">
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


### <a name="remarks"></a>Kommentarer
BatchAcknowledgementInterval styr replikeringsfördröjning. Värdet '0' resulterar i hello lägsta möjliga tidsfördröjning, kostnad hello genomströmning (som flera bekräftelsemeddelanden måste skickas och bearbetas, som innehåller färre bekräftelser).
hello större hello-värdet för BatchAcknowledgementInterval, hello högre hello övergripande replikering genomströmning vid hello kostnaden för högre latens för åtgärden. Detta innebär direkt toohello svarstiden för genomförda transaktioner.

hello-värdet för CheckpointThresholdInMB kontroller hello mängden diskutrymme som hello replikatorn kan använda toostore statusinformation i hello replik dedikerade loggfilen. Öka det här tooa högre värde än standard hello kan resultera i omkonfiguration snabbare när en ny replik läggs toohello set. Detta är på grund av toohello partiella tillstånd överföring som äger rum på grund av toohello tillgängligheten för tidigare åtgärder i hello-loggen. Detta kan potentiellt öka hello tiden för återställning av en replik efter en krasch.

Hej MaxRecordSizeInKB inställningen definierar hello maximal storlek för en post som kan skrivas av hello replikatorn till hello loggfil. I de flesta fall är hello standard 1024 KB poststorleken optimalt. Om hello-tjänst som orsakar större data objekt toobe tillhör hello statusinformation, kan sedan det här värdet dock behöva toobe ökat. Det finns lite fördelen med att göra MaxRecordSizeInKB mindre än 1024, mindre poster Använd endast hello utrymmet som krävs för mindre hello-post. Vi räknar med att det här värdet måste toobe ändras i endast sällsynta fall.

Hej SharedLogId och SharedLogPath är alltid användas tillsammans toomake service-Använd en separat delad logg från hello standard delade loggen för hello nod. Mest effektivt så många tjänster som möjligt bör ange hello samma delade loggen. Delade loggfiler ska placeras på diskar som används enbart för hello delade loggen filen tooreduce head flytt konkurrens. Vi räknar med att det här värdet måste toobe ändras i endast sällsynta fall.

## <a name="next-steps"></a>Nästa steg
* [Felsöka Service Fabric-program i Visual Studio](service-fabric-debugging-your-application.md)
* [För utvecklare för Reliable Services](https://msdn.microsoft.com/library/azure/dn706529.aspx)

