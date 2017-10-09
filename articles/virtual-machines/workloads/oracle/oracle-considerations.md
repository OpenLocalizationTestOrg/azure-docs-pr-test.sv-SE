---
title: "aaaOracle lösningar på Microsoft Azure | Microsoft Docs"
description: "Läs mer om konfigurationer som stöds och begränsningar för Oracle-lösningar för Microsoft Azure."
services: virtual-machines-linux
documentationcenter: 
manager: timlt
author: rickstercdn
tags: azure-resource-management
ms.assetid: 5d71886b-463a-43ae-b61f-35c6fc9bae25
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 06/15/2017
ms.author: rclaus
ms.openlocfilehash: 54dc62e76129535da7fb6f131af90c83adfec6cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="oracle-solutions-and-their-deployment-on-microsoft-azure"></a>Oracle-lösningar och deras distribution på Microsoft Azure
Den här artikeln innehåller information som krävs toosuccesfully distribuera olika Oracle-lösningar för Microsoft Azure. Dessa lösningar baseras på virtuella avbildningar publicerade av Oracle i hello Azure Marketplace. tooget en lista över tillgängliga avbildningar, kör hello följande kommando:
```azurecli-interactive
az vm image list --publisher oracle -o table --all
```
Från och med 6 – 16-2017 hello listan över bilder är hello följande:
```bash
Offer                   Publisher    Sku                     Urn                                                          Version
----------------------  -----------  ----------------------  -----------------------------------------------------------  -------------
Oracle-Database-Ee      Oracle       12.1.0.2                Oracle:Oracle-Database-Ee:12.1.0.2:12.1.20170202             12.1.20170202
Oracle-Database-Se      Oracle       12.1.0.2                Oracle:Oracle-Database-Se:12.1.0.2:12.1.20170202             12.1.20170202
Oracle-Linux            Oracle       6.4                     Oracle:Oracle-Linux:6.4:6.4.20141206                         6.4.20141206
Oracle-Linux            Oracle       6.7                     Oracle:Oracle-Linux:6.7:6.7.20161007                         6.7.20161007
Oracle-Linux            Oracle       6.8                     Oracle:Oracle-Linux:6.8:6.8.20161020                         6.8.20161020
Oracle-Linux            Oracle       6.9                     Oracle:Oracle-Linux:6.9:6.9.20170406                         6.9.20170406
Oracle-Linux            Oracle       7.0                     Oracle:Oracle-Linux:7.0:7.0.20141217                         7.0.20141217
Oracle-Linux            Oracle       7.2                     Oracle:Oracle-Linux:7.2:7.2.20161020                         7.2.20161020
Oracle-Linux            Oracle       7.3                     Oracle:Oracle-Linux:7.3:7.3.20170320                         7.3.20170320
Oracle-WebLogic-Server  Oracle       Oracle-WebLogic-Server  Oracle:Oracle-WebLogic-Server:Oracle-WebLogic-Server:12.1.2  12.1.2
```

Dessa avbildningar betraktas som ”Bring Your Own License” och som sådan endast debiteras du för bearbetning, lagring och nätverk kostnaderna genom att köra en virtuell dator.  Du är korrekt licensierade toouse Oracle-programvara och att du har en aktuell supportavtal på plats med Oracle antas. Oracle har garanteras licensera mobility från lokala tooAzure. Se hello publicerade [Oracle och Microsoft](http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html) Obs mer information om licensmobilitet. 

Enskilda användare kan också välja toobase sina lösningar på anpassade avbildningar de skapa från grunden i Azure eller ladda upp en anpassad bilder från sina på lokala miljöer.

## <a name="support-for-jd-edwards"></a>Stöd för JD Edwards
Enligt tooOracle stöd Obs [Doc-ID 2178595.1](https://support.oracle.com/epmos/faces/DocumentDisplay?_afrLoop=573435677515785&id=2178595.1&_afrWindowMode=0&_adf.ctrl-state=o852dw7d_4) , JD Edwards EnterpriseOne versioner 9.2 och senare stöds i **alla offentliga molntjänster erbjudande** som uppfyller specifikt `Minimum Technical Requirements` (MTR).  Du behöver toocreate anpassade avbildningar som uppfyller deras MTR specifikationerna för operativsystem och programvara kompatibilitet för program. 

## <a name="oracle-database-virtual-machine-images"></a>Oracle Database avbildningar av virtuella datorer
Oracle stöder körs Oracle DB 12.1 Standard och Enterprise-versioner i Azure på avbildningar av virtuella datorer baserat på Oracle Linux.  Hello bästa prestanda för produktionsarbetsbelastningar av Oracle-databas på Azure vara säker på att tooproperly storlek hello VM-avbildning och använda hanterade diskar som backas upp av Premium-lagring. Anvisningar om hur tooquickly få en Oracle-databas körs i Azure med hjälp av hello Oracle publicerade VM-avbildning [försök hello Oracle DB Quickstart genomgången](oracle-database-quick-create.md).

### <a name="attached-disk-configuration-options"></a>Ansluten disk konfigurationsalternativ

Anslutna diskar förlitar sig på hello Azure Blob storage-tjänst. Varje disk som standard kan teoretiskt maximalt cirka 500 i/o-åtgärder per sekund (IOPS). Vår premium disk erbjudande är att föredra för högpresterande arbetsbelastningar och kan få upp too5000 IOps per disk. Du kan använda en enda disk om som uppfyller dina prestanda måste - du kan förbättra hello effektiva IOPS prestanda om du använder flera anslutna diskar, fördelade databasdata på dem och sedan använda Oracle automatisk Storage Management (ASM). Se [automatiska Oracle-lagring – översikt](http://www.oracle.com/technetwork/database/index-100339.html) mer Oracle ASM specifik information. Ett exempel på hur tooinstall och konfigurera Oracle ASM på en Linux Azure VM - du kan försöka hello [installera och konfigurera Oracle Automated lagringshantering](configure-oracle-asm.md) kursen.

### <a name="oracle-realtime-application-cluster-rac"></a>Oracle realtid programmet kluster (RAC)
Oracle RAC är utformad toomitigate hello fel på en enskild nod i ett kluster med flera noder lokala-konfiguration.  Den förlitar sig på två lokala tekniker som inte är inbyggd toohyper skala offentliga molnmiljöer: multicast-nätverket och delad disk. Det finns tredjeparts-lösningar som skapats av andra företag [, till exempel FlashGrid](https://www.flashgrid.io/oracle-rac-in-azure/) som emulerar dessa tekniker om du behöver toodeploy Oracle RAC i Azure. 

### <a name="high-availability-and-disaster-recovery-considerations"></a>Hög tillgänglighet och katastrofåterställning överväganden
När du använder Oracle-databaser i Azure måste ansvarar du för att implementera en hög tillgänglighet och disaster recovery lösning tooavoid driftavbrott. 

Hög tillgänglighet och katastrofåterställning återställning för Oracle Database Enterprise Edition (utan RAC) i Azure kan uppnås med hjälp av [Data Guard, aktiva Data Guard](http://www.oracle.com/technetwork/articles/oem/dataguardoverview-083155.html), eller [Oracle guld Gate](http://www.oracle.com/technetwork/middleware/goldengate), med två databaser i två separata virtuella datorer. Både virtuella datorer ska vara i hello samma [virtuellt nätverk](https://azure.microsoft.com/documentation/services/virtual-network/) tooensure som de har åtkomst till varandra via hello privat beständiga IP-adress.  Vi rekommenderar dessutom att placera hello virtuella datorer i hello samma tillgänglighetsuppsättning tooallow Azure tooplace dem i separata fault-domäner och uppgradera domäner.  Om du vill toohave geo-redundans - har du två databaserna replikera mellan två olika regioner och ansluta hello två instanser med en VPN-Gateway.

Vi har en självstudiekurs ”[implementera Oracle DataGuard på Azure](configure-oracle-dataguard.md)” som vägleder dig genom hello grundinställning proceduren tootrial detta i Azure.  

Med Oracle Data Guard hög tillgänglighet kan uppnås med en primär databas i en virtuell dator, en sekundär (standby) databas i en annan virtuell dator och konfigurera mellan dem replikering. hello resultatet är läsbehörighet toohello kopia av hello-databasen. Du kan konfigurera dubbelriktad replikering mellan två hello-databaser med Oracle GoldenGate. toolearn hur tooset in en lösning för hög tillgänglighet för dina databaser med hjälp av dessa verktyg finns [aktiva Data Guard](http://www.oracle.com/technetwork/database/features/availability/data-guard-documentation-152848.html) och [GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html) dokumentationen på hello Oracle-webbplatsen. Om du behöver skrivskyddad åtkomst toohello kopia av hello-databasen, kan du använda [Active Oracle Data Guard](http://www.oracle.com/uk/products/database/options/active-data-guard/overview/index.html).

Vi har en självstudiekurs ”[implementera Oracle GoldenGate på Azure](configure-oracle-golden-gate.md)” som vägleder dig genom hello grundläggande seup proceduren tootrial detta i Azure.

Trots att ha en lösning för hög tillgänglighet och Katastrofåterställning konstruerad i Azure, vill du tooensure som du har en strategi för säkerhetskopiering i toorestore plats för din databas.  Vi har en självstudiekurs [säkerhetskopiering och Återställ en Oracle-databas](oracle-backup-recovery.md) som vägleder dig genom hello grundläggande proceduren för att upprätta en consistant-säkerhetskopia.

## <a name="oracle-weblogic-server-virtual-machine-images"></a>Oracle WebLogic Server avbildningar av virtuella datorer
* **Kluster stöds på Enterprise Edition endast.** Du är licensierad toouse WebLogic kluster när du använder hello WebLogic Server Enterprise Edition. Använd inte kluster med WebLogic Server Standard Edition.
* **UDP-multicast stöds inte.** Azure stöder UDP unicast, men inte multicasting eller broadcasting. WebLogic Server är kan toorely Azure UDP unicast-funktioner. För bästa resultat förlita dig på UDP unicast, rekommenderar vi hello WebLogic klusterstorleken hållas statisk, eller hållas med fler än 10 hanterade servrar som ingår i hello kluster.
* **WebLogic Server förväntar offentliga och privata portar toobe hello samma för T3 åtkomst till (till exempel när du använder Enterprise JavaBeans).** Överväg ett scenario med flera nivåer där ett tjänstprogram layer (EJB) körs på ett serverkluster med WebLogic som består av två eller flera virtuella datorer i ett vNet med namnet **SLWLS**. hello klienten nivå är i ett annat undernät i hello samma virtuella nätverk som kör ett enkelt Java-program som försöker toocall EJB i hello-tjänstnivå. Eftersom det är nödvändigt tooload saldo hello-tjänstnivå, måste en offentlig slutpunkt för Utjämning av nätverksbelastning toobe som skapats för hello virtuella datorer i hello WebLogic Server-kluster. Om hello privat port som du anger skiljer sig från hello offentlig port (till exempel 7006:7008), inträffar ett fel, till exempel hello följande:

       [java] javax.naming.CommunicationException [Root exception is java.net.ConnectException: t3://example.cloudapp.net:7006:

       Bootstrap to: example.cloudapp.net/138.91.142.178:7006' over: 't3' got an error or timed out]

   Detta beror på att för fjärråtkomst T3, WebLogic servern förväntar sig hello belastningen belastningsutjämnaren port och hello WebLogic hanterad server port toobe hello samma. I hello senare fallet hello-klient har åtkomst till port 7006 (hello belastningen belastningsutjämnaren port) och hello hanterade servern lyssnar på 7008 (hello privat port). Den här begränsningen gäller endast för T3 åtkomst, inte HTTP.

   tooavoid problemet genom att använda ett av följande lösningar hello:

  * Använd hello samma privata och offentliga portnummer för den belastningsutjämnade slutpunkter dedikerade tooT3 åtkomst.
  * Inkludera hello efter JVM parameter när du startar WebLogic Server:

         -Dweblogic.rjvm.enableprotocolswitch=true

Mer information finns i KB-artikel **860340.1** på <http://support.oracle.com>.

* **Dynamisk kluster och begränsningar för belastningsutjämning.** Du kanske vill toouse ett dynamiskt WebLogic Server-kluster och exponera den via en enskild, offentlig belastningsutjämnade slutpunkt i Azure. Detta kan göras så länge som du använder en fast portnummer för varje hello hanterade servrar (inte dynamiskt tilldelade från ett intervall) och starta inte mer hanterade servrar än det finns datorer Hej administratör för att spåra (som är mer än en hanterad server per Radnr i virtuell loggning av användaråtkomst datorn). Om din konfiguration resulterar i fler WebLogic servrar startas än det finns virtuella datorer (det vill säga där flera WebLogic instanser serverresurs hello samma virtuella dator), är inte möjligt för mer än en av de instanserna av WebLogic Server servrar toobind tooa angivna portnummer – hello andra på den virtuella datorn misslyckas.

   På hello däremot om du konfigurerar hello admin server tooautomatically tilldela unika portnummer tooits hanterade servrar, belastningsutjämning är inte möjligt eftersom Azure inte stöder mappning från en enskild offentlig port toomultiple privata portar, vilket är krävs för den här konfigurationen.
* **Flera instanser av Weblogic Server på en virtuell dator.** Beroende på distributionskraven kan du överväga att hello alternativet för flera instanser av WebLogic Server som körs på hello samma virtuella dator, om hello virtuella datorn är tillräckligt stor. Till exempel på en normal storlek virtuell dator som innehåller två kärnor, kan du välja toorun två instanser av WebLogic Server. Observera dock att fortfarande rekommenderar vi att du inte införa enskilda felpunkter i din arkitektur som skulle vara fallet hello om du använder en virtuell dator som kör flera instanser av WebLogic Server. Med hjälp av minst två virtuella datorer kan vara en bättre metod och var och en av de virtuella datorerna kan sedan köra flera instanser av WebLogic Server. Var och en av dessa instanser av WebLogic Server kan fortfarande vara en del av hello samma kluster. Obs!, men det går för närvarande inte toouse Azure tooload saldo slutpunkter som exponeras av sådana WebLogic Server-distributioner inom hello samma virtuella dator eftersom Azure belastningsutjämnare kräver hello belastningsutjämnade servrar toobe fördelas mellan Unik virtuella datorer.

## <a name="oracle-jdk-virtual-machine-images"></a>Oracle JDK avbildningar av virtuella datorer
* **JDK 6 och 7 senaste uppdateringarna.** Medan vi rekommenderar att du använder hello senaste offentliga, stöds version av Java (för närvarande Java 8), tillgängliggör Azure också JDK 6 och 7 bilder. Detta är avsett för äldre program som inte är redo toobe uppgraderas tooJDK 8. När uppdateringar tooprevious JDK avbildningar kan inte längre vara tillgängliga toohello allmänheten, angivna hello avsedd Microsoft tillsammans med Oracle, hello JDK 6 och 7 bilder som tillhandahålls av Azure är toocontain en senare uppdatering för icke-offentliga som normalt Oracle tooonly en viss grupp med Oracle stöds kunder. Nya versioner av hello JDK bilder ska göras tillgänglig över tid med uppdaterade versioner av JDK 6 och 7.

   Hej JDK som är tillgängliga i den här JDK 6 och 7 avbildningar och hello virtuella datorer och bilder som har härletts från dem, kan endast användas i Azure.
* **64-bitars JDK.** hello Oracle WebLogic Server avbildningar av virtuella datorer och hello Oracle JDK avbildningar av virtuella datorer som tillhandahålls av Azure innehåller hello 64-bitars versioner av Windows Server- och hello JDK.

## <a name="next-steps"></a>Nästa steg
Nu har du en översikt över aktuella Oracle-lösningar för Microsoft Azure. Nästa steg är toogo och distribuera din första Oracle-databas på Azure.
- Försök hello [skapa en Oracle-databas på Azure](oracle-database-quick-create.md) självstudiekursen tooget igång.
