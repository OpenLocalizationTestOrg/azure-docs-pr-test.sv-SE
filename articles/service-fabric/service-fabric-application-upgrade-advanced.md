---
title: aaaAdvanced program uppgradera avsnitt | Microsoft Docs
description: "Den här artikeln beskriver vissa avancerade ämnen gällande tooupgrading ett Service Fabric-program."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: e29585ff-e96f-46f4-a07f-6682bbe63281
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar;chackdan
ms.openlocfilehash: bdaf3db6209c574d39f57e0bf9951fad5ad1cbec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-application-upgrade-advanced-topics"></a>Uppgradering av Service Fabric-programmet: avancerade alternativ
## <a name="adding-or-removing-services-during-an-application-upgrade"></a>Att lägga till eller ta bort tjänster under en uppgradering av programmet
Om en ny tjänst läggs tooan program som redan distribuerats och publiceras som en uppgradering, är hello ny tjänst tillagda toohello distribuerade program.  Sådana uppgradering påverkar inte någon av hello-tjänster som redan fanns hello program. Men en instans av hello-tjänsten som har lagts till måste startas för hello nya service toobe active (med hjälp av hello `New-ServiceFabricService` cmdlet).

Tjänster kan också tas bort från ett program som en del av en uppgradering. Men alla aktuella instanser av hello till ska-bort tjänsten måste stoppas innan du fortsätter med uppgraderingen hello (med hjälp av hello `Remove-ServiceFabricService` cmdlet).

## <a name="manual-upgrade-mode"></a>Manuellt Uppgraderingsläge
> [!NOTE]
> hello oövervakade manuellt läge bör övervägas endast för en misslyckad eller pausat uppgradering. hello övervakat läge är hello rekommenderade Uppgraderingsläge för Service Fabric-program.
>
>

Azure Service Fabric innehåller flera uppgradera lägen toosupport kluster för utveckling och produktion. Distributionsalternativ valt kan vara olika för olika miljöer.

programmet hello övervakas uppgraderingen är hello vanligaste uppgradera toouse i hello-produktionsmiljö. När hello uppgraderar princip har angetts, Service Fabric säkerställer att programmet hello är felfri innan hello uppgraderingen fortsätter.

 hello-administratörer kan använda hello manuell rullande program Uppgraderingsläge toohave fullständig kontroll över hello Uppgraderingsförlopp via hello olika uppgraderingsdomäner. Det här läget är användbart när en anpassad eller komplexa utvärdering hälsoprincip krävs, eller en icke-konventionella uppgraderingen sker (till exempel hello programmet finns redan i förlust av data).

Slutligen är hello automatiserat program uppgraderingen användbart för utveckling eller tester miljöer tooprovide en snabb iteration växla under utvecklingen av tjänsten.

## <a name="change-toomanual-upgrade-mode"></a>Ändra toomanual Uppgraderingsläge
**Manuell**--stoppa hello Programuppgradering på hello aktuella UD och ändrar hello uppgradera läge tooUnmonitored manuell. Hej administratör måste toomanually anropet **MoveNextApplicationUpgradeDomainAsync** tooproceed med hello uppgradera eller starta en återställning genom att initiera en ny uppgradering. När hello uppgraderingen går in i hello manuellt läge, förblir den i hello manuellt läge tills en ny uppgradering initieras. Hej **GetApplicationUpgradeProgressAsync** kommando returnerar FABRIC\_programmet\_uppgradera\_tillstånd\_rullande\_framåt\_väntar.

## <a name="upgrade-with-a-diff-package"></a>Uppgradera med en diff-paket
Ett Service Fabric-program kan uppgraderas genom etablering med en fullständig, självständiga programpaket. Ett program kan också uppgraderas med hjälp av en diff-paket som innehåller endast hello uppdateras programfiler, hello uppdateras programmanifestet och hello service manifest-filer.

En fullständig programpaketet innehåller alla hello filer nödvändiga toostart och kör ett Service Fabric-program. En diff-paketet innehåller endast hello-filer som ändrats mellan hello senare bestämmelsen och hello uppgraderingen, plus hello fullständig programmanifestet och hello service manifest filer. Alla referenser i programmanifestet hello eller service manifest som inte kan hittas i hello build layout sökte efter i hello avbildningsarkivet.

Fullständig programpaket krävs för hello första installationen av ett program toohello kluster. Efterföljande uppdateringar kan vara ett fullständigt programpaket eller en diff-paketet.

Tillfällen när du använder en diff-paketet är ett bra alternativ:

* En diff-paketet rekommenderas när du har ett stort programpaket som refererar till flera service manifest-filer och/eller flera kod paket, config paket eller data paket.
* En diff-paketet rekommenderas när du har en distribution som genererar hello build layout direkt från din Byggprocessen. Även om hello koden inte har ändrats hämta nyligen inbyggd sammansättningar i det här fallet en annan kontrollsumma. Med hjälp av en fullständig programpaket skulle kräva du tooupdate hello-versionen på alla kod paket. Använder en diff-paketet ange du bara hello-filer som ändras och hello manifestfiler där hello version har ändrats.

När ett program uppgraderas med Visual Studio, publiceras hello diff-paketet automatiskt. toocreate en diff-paketet hello manuellt programmanifestet, och hello service manifest måste uppdateras, men endast hello ändras paket ska tas med i hello slutliga programpaket.

Till exempel börja med hello följande program (versionsnummer för enkel förstå):

```text
app1           1.0.0
  service1     1.0.0
    code       1.0.0
    config     1.0.0
  service2     1.0.0
    code       1.0.0
    config     1.0.0
```

Nu antar vi att du vill ha tooupdate endast hello kodpaketet av service1 använder en diff-paketet med hjälp av PowerShell. Nu har uppdaterade programmet hello följande mappstrukturen:

```text
app1           2.0.0      <-- new version
  service1     2.0.0      <-- new version
    code       2.0.0      <-- new version
    config     1.0.0
  service2     1.0.0
    code       1.0.0
    config     1.0.0
```

I så fall måste uppdatera du hello application manifest too2.0.0 och hello service manifest för uppdatering av service1 tooreflect hello paketet. hello mapp för ditt programpaket skulle ha hello följande struktur:

```text
app1/
  service1/
    code/
```

## <a name="next-steps"></a>Nästa steg
[Uppgradera ditt program med hjälp av Visual Studio](service-fabric-application-upgrade-tutorial.md) vägleder dig genom en uppgradering av programmet med hjälp av Visual Studio.

[Uppgradera ditt program med hjälp av Powershell](service-fabric-application-upgrade-tutorial-powershell.md) vägleder dig genom en uppgradering av programmet med hjälp av PowerShell.

Styra hur programmet ska uppgraderas med hjälp av [uppgradera parametrar](service-fabric-application-upgrade-parameters.md).

Gör din programuppgraderingar kompatibla genom att lära dig hur toouse [dataserialisering](service-fabric-application-upgrade-data-serialization.md).

Lösa vanliga problem i programuppgraderingar genom att referera toohello stegen i [felsökning programuppgraderingar](service-fabric-application-upgrade-troubleshooting.md).
