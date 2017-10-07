---
title: "aaaAzure riktlinjer för Data Lake Store Storm-prestandajustering | Microsoft Docs"
description: Azure Data Lake Store Storm prestandajustering riktlinjer
services: data-lake-store
documentationcenter: 
author: stewu
manager: amitkul
editor: stewu
ms.assetid: ebde7b9f-2e51-4d43-b7ab-566417221335
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/19/2016
ms.author: stewu
ms.openlocfilehash: 5412fd46cf2373f5877030913df4fe1fc6f5473a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="performance-tuning-guidance-for-storm-on-hdinsight-and-azure-data-lake-store"></a>Prestandajustering för Storm på HDInsight och Azure Data Lake Store

Förstå hello faktorer som ska beaktas när du finjustera hello prestanda för en Azure Storm-topologi. Exempelvis är det viktigt toounderstand hello egenskaper hello arbete som utförs av hello kanaler och hello bultar (om hello arbete är i/o- eller minnesintensiva). Den här artikeln innehåller en uppsättning prestandajustering riktlinjer, inklusive felsökning av vanliga problem.

## <a name="prerequisites"></a>Krav

* **En Azure-prenumeration**. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Ett Azure Data Lake Store-konto**. Anvisningar för hur toocreate en, se [Kom igång med Azure Data Lake Store](data-lake-store-get-started-portal.md).
* **Ett Azure HDInsight-kluster** med åtkomst tooa Data Lake Store-konto. Se [skapar ett HDInsight-kluster med Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md). Kontrollera att du kan aktivera Fjärrskrivbord för hello kluster.
* **Kör ett Storm-kluster på Data Lake Store**. Mer information finns i [Storm på HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-storm-overview).
* **Prestandajustering riktlinjer för Data Lake Store**.  Allmänna prestanda begrepp finns [Data Lake Store justera Prestandaråd](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-performance-tuning-guidance).  

## <a name="tune-hello-parallelism-of-hello-topology"></a>Finjustera hello parallellitet hello-topologi

Du kanske kan tooimprove prestanda genom att öka hello samtidighet på hello i/o-tooand från Data Lake Store. En Storm-topologi har en uppsättning av konfigurationer som bestämmer hello parallellitet:
* Antalet arbetsprocesser (hello arbetare är jämnt fördelad över hello virtuella datorer).
* Antalet kanal utföraren instanser.
* Antalet instanser av bulten utförare.
* Antal kanal uppgifter.
* Antal bult uppgifter.

Anta till exempel på ett kluster med 4 virtuella datorer och 4 arbetsprocesser, 32 kanal executors och 32 kanal aktiviteter och 256 bult executors och 512 bult uppgifter att hello följande:

Varje Övervakare som är en arbetsnod, har en enda worker Java virtual machine (JVM)-processen. Den här processen JVM hanterar 4 kanal trådar och 64 bult trådar. Inom varje tråd körs aktiviteter sekventiellt. Varje kanal tråd har 1 aktivitet med hello föregående konfiguration, och varje bult tråd har 2 aktiviteter.

Här följer hello olika komponenter som ingår i Storm, och hur de påverkar hello nivå av parallellitet som du har:
* hello huvudnod (kallas Nimbus i Storm) är används toosubmit och hantera jobb. Dessa noder har ingen inverkan på hello grad av parallellitet.
* hello handledarens noder. I HDInsight motsvarar vilket tooa arbetsnoden Azure VM.
* hello worker uppgifter är Storm-processer som körs i hello virtuella datorer. Varje worker aktivitet motsvarar tooa JVM-instans. Storm distribuerar hello många arbetsprocesser anger du toohello arbetarnoder jämnt som möjligt.
* Prata och bultar utföraren instanser. Varje instans utföraren motsvarar tooa tråd körs i hello arbetare (JVMs).
* Storm-aktiviteter. Detta är logiska aktiviteter som var och en av dessa trådar körs. Detta ändrar inte hello nivå av parallellitet, så att du ska utvärdera om du behöver flera uppgifter per utföraren eller inte.

### <a name="get-hello-best-performance-from-data-lake-store"></a>Hämta hello bästa möjliga prestanda från Data Lake Store

När du arbetar med Data Lake Store kan få du hello bästa prestanda om du hello följande:
* Slå samman ett litet lägger till större storlek (helst 4 MB).
* Göra så många samtidiga förfrågningar som möjligt. Eftersom varje bult tråd gör blockerande läser du toohave någonstans i hello intervallet 12 8 trådar per kärna. Detta håller hello NIC och hello CPU som också används. En större VM gör det möjligt för flera samtidiga begäranden.  

### <a name="example-topology"></a>Exempeltopologi

Anta att du har en 8 worker nodskluster med en D13v2 Azure VM. Den här virtuella datorn har 8 kärnor, så hello 8 arbetarnoder bland har 64 Totalt antal kärnor.

Anta att vi gör 8 bult trådar per kärna. Den angivna 64 kärnor innebär som vi vill 512 totala bult utföraren instanser (det vill säga trådar). I det här fallet anta att vi börjar med en JVM per virtuell dator och använder främst hello tråd samtidighet inom hello JVM tooachieve samtidighet. Det innebär att vi behöver 8 worker uppgifter (en per virtuell Azure-dator) och 512 bult executors. Med den här konfigurationen Storm försöker toodistribute hello arbetare jämnt över arbetsnoder (även kallat handledarens noder), ger varje arbetsnod 1 JVM. Nu inom hello ansvariga försöker Storm toodistribute hello executors jämnt mellan ansvariga, trådar ger varje handledarens (det vill säga JVM) 8 varje.

## <a name="tune-additional-parameters"></a>Finjustera ytterligare parametrar
När du har grundläggande hello-topologi kan du om du vill tootweak någon av parametrarna hello:
* **Antal JVMs per arbetsnoden.** Om du har en stor datastruktur (till exempel en uppslagstabell) kräver en separat kopia av du värden i minnet, varje JVM. Du kan också använda hello-datastrukturen över många trådar om du har färre JVMs. För hello bult i/o gör hello antalet JVMs inte så stor del av en skillnad som hello antal trådar som har lagts till i dessa JVMs. För enkelhetens skull, är det en bra idé toohave en JVM per worker. Beroende på vad din bult gör eller vilka program som bearbetar du kräva, även om du kanske måste toochange numret.
* **Antal kanal executors.** Eftersom hello föregående exempel använder bultar för att skriva tooData Datasjölager, hello antal kanaler är inte direkt relevanta toohello bult prestanda. Hello mycket bearbetning eller i/o som händer i hello kanal är det en bra idé spouts dock tootune hello för bästa prestanda. Se till att du har tillräckligt med kanaler toobe kan tookeep hello bultar upptagen. hello utdata frekvens av hello kanaler ska matcha hello genomflödet i hello bultar. hello faktiska konfigurationen beror på hello-kanal.
* **Antal uppgifter.** Varje bult körs som en enskild tråd. Ytterligare aktiviteter per bult Ange inte några ytterligare samtidighet. hello enda gång de är till nytta är om processen för bekräftades hello tuppel tar en stor del av din bult körningstid. Det är en bra idé toogroup många tupplar i en större läggas till innan du skickar en bekräftelse från hello bulten. Så i de flesta fall ger inga ytterligare fördelar i flera uppgifter.
* **Lokal eller blanda gruppering.** När den här inställningen är aktiverad tupplar skickas toobolts inom hello samma arbetsprocesser. Detta minskar mellan processer anrop för kommunikation och nätverk. Detta rekommenderas för de flesta topologier.

Det här grundläggande scenariot är en bra utgångspunkt. Testa med dina egna data tootweak hello föregående parametrar tooachieve optimala prestanda.

## <a name="tune-hello-spout"></a>Finjustera hello kanal

Du kan ändra följande inställningar tootune hello kanal hello.

- **Tuppel timeout: topology.message.timeout.secs**. Den här inställningen avgör hello lång tid ett meddelande tar toocomplete och få godkännande, innan den anses misslyckades.

- **Maximalt minne per arbetsprocessen: worker.childopts**. Den här inställningen kan du ange ytterligare kommandoradsparametrar toohello Java arbetare. hello vanligaste inställningen här är XmX som bestämmer hello maximalt minne som allokerats tooa JVM heap.

- **Max kanal väntande: topology.max.spout.pending**. Den här inställningen avgör hello antalet tupplar som kan i svarta (ännu inte bekräftas på alla noder i hello topologi) per kanal tråd när som helst.

 En bra beräkningen toodo är tooestimate hello storlek på din tupplar. Sedan ta reda på hur mycket minne en kanal tråd. hello totala mängden minne som allokerats tooa tråd, dividerat med det här värdet bör ge dig hello övre gränsen för Hej max kanal väntande parametern.

## <a name="tune-hello-bolt"></a>Finjustera hello bult
När du skriver tooData Datasjölager, ange en princip för synkronisering storlek (buffert på klientsidan för hello) too4 MB. Lokaliseraren eller hsync() sedan utförs endast när hello buffertstorleken är hello på det här värdet. hello Data Lake Store-drivrutinen på hello worker VM använder automatiskt den här buffring såvida inte du uttryckligen utföra en hsync().

hello standard Data Lake Store Storm bult har en storlek sync princip parameter (fileBufferSize) som kan använda tootune den här parametern.

I I/O-intensiva topologier är det en bra idé toohave varje bult tråd skriva tooits egen fil- och tooset en rotation filprincip (fileRotationSize). När hello-filen når en viss storlek, hello dataströmmen har rensats automatiskt och en ny fil ska skrivas till. hello rekommenderas filstorleken för rotation är 1 GB.

### <a name="handle-tuple-data"></a>Hantera tuppel data

I Storm innehåller en kanal på tooa tuppel tills tillämpas explicit av hello bulten. Om en tuppel har lästs av hello bult men har inte bekräftats, kan inte hello kanal beständiga i Data Lake Store-serverdel. När en tuppel bekräftas hello kanal kan garanteras beständiga av hello bult och kan sedan att ta bort hello källdata oavsett läser från källan.  

För bästa prestanda på Data Lake Store har hello bult bufferten 4 MB tuppel data. Sedan skriva toohello Datasjölager tillbaka avslutas som en 4 MB skrivåtgärder. Efter hello data har har skrivits toohello store (genom att anropa hflush()) hello bult kan bekräfta hello data tillbaka toohello kanal. Det här är vad hello exempel bultar angivna här finns. Det är också godkända toohold ett större antal tupplar innan hello hflush() anrop och hello tupplar bekräftas. Detta ökar dock hello antalet tupplar som rör sig att hello kanal måste toohold, och därför hello ökar mängden minne som behövs per JVM.

> [!NOTE]
Program kan ha en krav tooacknowledge tupplar oftare (på datamängder mindre än 4 MB) för andra prestandaskäl. Som kan dock påverka hello i/o genomströmning toohello lagring serverdel. Noggrant väga det här förhållandet mot hello bult i/o-prestanda.

Om hello mottagningshastighet av tupplar är inte hög så hello 4 MB buffert tar en lång tid toofill, bör du åtgärdar detta genom att:
* Minska hello antalet bultar, så det finns färre buffertar toofill.
* Med en tid- eller count-baserad princip, där en hflush() utlöses varje x rensningar eller varje y millisekunder och hello tupplar ackumulerade hittills är bekräftade tillbaka.

Observera att hello genomflöde i det här fallet är lägre men med låg hastighet av händelser, maximalt dataflöde kan inte hello största mål ändå. Dessa åtgärder hjälper dig att minska hello totala tid det tar för en tuppel tooflow via toohello store. Detta kan roll om du vill använda en realtid pipeline även med en låg händelsehastighet. Även Observera att din inkommande tuppel frekvensen är låg, bör justera hello topology.message.timeout_secs parameter, så hello tupplar inte timeout när de får en buffert eller bearbetas.

## <a name="monitor-your-topology-in-storm"></a>Övervaka topologin i Storm  
När din topologi körs, kan du övervaka den i hello Storm-användargränssnittet. Här följer hello huvudsakliga parametrar toolook på:

* **Totalt antal processen körning svarstid.** Detta är hello genomsnittstiden en tuppel tar toobe sänds av hello kanal bearbetas av hello bult och bekräftas.

* **Totalt antal bult processen svarstid.** Detta är hello Genomsnittlig tid av hello tuppel på hello bult tills det mottar en bekräftelse.

* **Totalt antal bult köra svarstid.** Detta är hello Genomsnittlig tid som användes av hello bult i hello execute-metoden.

* **Antal fel.** Detta refererar toohello antalet tupplar som inte kunde bearbetas fullständigt innan de tidsgränsen toobe.

* **Kapacitet.** Detta är ett mått på hur upptagen systemet är. Om värdet är 1, fungerar lika snabbt som de kan din bultar. Om det är mindre än 1, öka hello parallellitet. Om den är större än 1, minska hello parallellitet.

## <a name="troubleshoot-common-problems"></a>Felsöka vanliga problem
Här följer några vanliga scenarier för felsökning.
* **Många tupplar uppnådd tidsgräns.** Titta på varje nod i hello topologi toodetermine där hello flaskhalsen är. hello vanligaste anledningen är att hello bultar inte är kan tookeep upp med hello kanaler. Detta leder tootuples belastning hello interna buffertar när väntar toobe bearbetas. Överväg att öka hello timeout-värde eller minska Hej max kanal väntande.

* **Det finns en hög totala processen körning svarstid, men en låg bult processen fördröjning.** I det här fallet är det möjligt att hello tupplar inte är som ska bekräftas tillräckligt snabbt. Kontrollera att det finns ett tillräckligt antal acknowledgers. En annan möjlighet är att de väntar i hello kö för länge innan hello bolts start behandla dem. Minska Hej max kanal väntande.

* **Det finns en hög bult köra svarstid.** Det innebär att hello execute() metod för din bult tar för lång. Optimera hello-koden eller titta på skrivning storlekar och rensa beteende.

### <a name="data-lake-store-throttling"></a>Begränsning av data Lake Store
Om du träffar hello gränserna för bandbredd som tillhandahålls av Data Lake Store, kan du se misslyckade aktiviteter. Uppgiften loggarna för begränsning av fel. Du kan minska hello parallellitet genom att öka storleken på behållaren.    

toocheck om du har komma begränsats aktivera hello felsökningsloggning på klientsidan för hello:

1. I **Ambari** > **Storm** > **Config** > **avancerade storm-worker-log4j**, ändra  **&lt;rot nivå = ”info”&gt;**  för**&lt;rot nivå = ”debug”&gt;**. Starta om alla hello noder/för hello configuration tootake effekt.
2. Övervakaren hello Storm-topologi som loggar in arbetarnoder (under /var/log/storm/worker-artifacts /&lt;TopologyName&gt;/&lt;port&gt;/worker.log) för Data Lake Store begränsning undantag.

## <a name="next-steps"></a>Nästa steg
Ytterligare prestandajustering för Storm kan refereras i [bloggen](https://blogs.msdn.microsoft.com/shanyu/2015/05/14/performance-tuning-for-hdinsight-storm-and-microsoft-azure-eventhubs/).

Ett ytterligare exempel toorun finns [denna som finns på GitHub](https://github.com/hdinsight/storm-performance-automation).
