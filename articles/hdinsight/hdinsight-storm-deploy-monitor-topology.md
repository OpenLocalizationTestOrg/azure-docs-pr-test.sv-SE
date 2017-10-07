---
title: "aaaDeploy och hantera Apache Storm-topologier på HDInsight | Microsoft Docs"
description: "Lär dig hur toodeploy, övervaka och hantera Apache Storm-topologier med hello Storm-instrumentpanelen på HDInsight. Använda Hadoop-verktyg för Visual Studio."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 5e542072-f014-42aa-82d6-2694a76df520
ms.service: hdinsight
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/01/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 05f05fe8dd519fe99fb771d36bfc3d28168ca85f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-manage-apache-storm-topologies-on-windows-based-hdinsight"></a>Distribuera och hantera Apache Storm-topologier på Windows-baserade HDInsight

hello Storm-instrumentpanelen kan du tooeasily distribuera och köra Apache Storm-topologier tooyour HDInsight-kluster med hjälp av webbläsaren. Du kan också använda hello instrumentpanelen toomonitor och hantera topologier som körs. Om du använder Visual Studio innehåller hello HDInsight Tools för Visual Studio liknande funktioner i Visual Studio.

hello Storm-instrumentpanelen och hello Storm-funktioner i hello HDInsight Tools förlitar sig på hello Storm REST-API som kan vara används toocreate övervakning och hantering av lösningar.

> [!IMPORTANT]
> hello stegen i det här dokumentet kräver ett Storm på HDInsight-kluster som använder Windows hello-operativsystemet. Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).
>
> Mer information om distribuera och hantera Storm-topologier med ett HDInsight-kluster som använder Linux finns [distribuera och hantera Apache Storm-topologier på Linux-baserat HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)

## <a name="prerequisites"></a>Krav

* **Apache Storm på HDInsight** -finns [Kom igång med Apache Storm på HDInsight](hdinsight-apache-storm-tutorial-get-started.md) anvisningar om hur du skapar ett kluster.

* För hello **Storm-instrumentpanelen**: en modern webbläsare som stöder HTML5.

* För **Visual Studio** -Azure SDK 2.5.1 eller senare och hello HDInsight Tools för Visual Studio. Se [komma igång med HDInsight Tools för Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md) tooinstall och konfigurera hello HDInsight tools för Visual Studio.

    En av följande versioner av Visual Studio hello:

  * Visual Studio 2012 med Update 4

  * Visual Studio 2013 med Update 4 eller Visual Studio Community 2013

  * Visual Studio 2015 (någon utgåva)

  * 2017 för Visual Studio (någon utgåva)

## <a name="storm-dashboard"></a>Storm-instrumentpanelen

hello Storm-instrumentpanelen är en webbsida som är tillgängliga på Storm-kluster. hello URL: en är **https://&lt;klusternamn >.azurehdinsight.net/**, där **klusternamn** är hello namnet på ditt Storm på HDInsight-kluster.

Hello överkant hello Storm-instrumentpanelen, Välj **skicka topologi**. Följ hello anvisningar på hello sidan toorun en exempeltopologi eller tooupload och kör en topologi som du skapade.

![hello skicka topologi sida][storm-dashboard-submit]

### <a name="storm-ui"></a>Storm UI

Hello Storm-instrumentpanelen, Välj hello **Storm-Användargränssnittet** länk. Då visas information om hello-klustret i tillägg tooany topologier som körs.

![hello storm-användargränssnittet][storm-dashboard-ui]

> [!NOTE]
> I vissa versioner av Internet Explorer kan du identifiera den hello Storm-Användargränssnittet inte kan uppdatera när du först har besökt. Det kan till exempel inte visa hello nya topologier du skickat, eller den kan visa en topologi som aktiv när du tidigare har inaktiverats. Microsoft är medveten om det här problemet och arbetar på en lösning.

#### <a name="main-page"></a>Huvudsida

hello huvudsidan för hello Storm-Användargränssnittet innehåller hello följande information:

* **Översikt över kluster**: grundläggande information om hello Storm-kluster.

* **Översikt över topologi**: en lista över topologier som körs. Använd hello länkar i det här avsnittet tooview mer information om specifika topologier.

* **Översikt över handledarens**: Information om hello Storm chef.

* **Nimbus configuration**: Nimbus-konfigurationen för hello kluster.

#### <a name="topology-summary"></a>Topologi sammanfattning

Att välja en länk från hello **Topology summary** avsnitt visar följande information om hello topologi hello:

* **Översikt över topologi**: grundläggande information om hello-topologi.

* **Topologi åtgärder**: hanteringsåtgärder som du kan utföra för hello-topologi.

  * **Aktivera**: återupptar bearbetningen av en inaktiverad topologi.

  * **Inaktivera**: pausar en topologi som körs.

  * **Balansera**: justerar hello parallellitet hello-topologi. Du bör balansera om topologier som körs när du har ändrat hello antalet noder i klustret hello. Detta gör hello topologi tooadjust parallellitet toocompensate för hello ökas eller minskas antalet noder i klustret hello.

      Mer information finns i [förstå hello parallellitet i en Storm-topologi (http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html)](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).

  * **Avsluta**: avslutar en Storm-topologi efter hello angivna timeout.

* **Topology stats**: statistik om hello-topologi. Hello länkarna i hello **fönstret** kolumnen tooset hello tidsperioden för hello återstående poster på hello-sidan.

* **Spouts**: hello kanaler som används av hello-topologi. Använd hello länkar i det här avsnittet tooview mer information om specifika kanaler.

* **Bolts**: hello bultar som används av hello-topologi. Använd hello länkar i det här avsnittet tooview mer information om specifika bultar.

* **Topologi configuration**: hello konfigurationen av hello valt topologi.

#### <a name="spout-and-bolt-summary"></a>Kanal och bult sammanfattning

Att välja en kanal hello **Spouts** eller **Bolts** avsnitt visar hello följande information om hello markerat objekt:

* **Sammanfattning**: grundläggande information om hello-kanal eller en bult.

* **Kanal/bult stats**: statistik om hello prata eller bultar. Hello länkarna i hello **fönstret** kolumnen tooset hello tidsperioden för hello återstående poster på hello-sidan.

* **Input stats** (endast bultar): Information om hello indataström som används av hello bulten.

* **Utdata stats**: Information om hello-dataströmmar som orsakat av detta kanal eller bultar.

* **Executors**: Information om hello instanser av hello-kanal eller en bult. Välj hello **Port** post för en specifik utförare tooview genereras en logg över diagnostisk information för den här instansen.

* **Fel**: felinformation för den här kanalen eller bultar.

## <a name="hdinsight-tools-for-visual-studio"></a>HDInsight Tools för Visual Studio

Hej HDInsight-verktyg kan vara används toosubmit eller hybrid C#-topologier tooyour Storm-kluster. hello följande använder ett exempelprogram. Information om hur du skapar egna topologier med hjälp av hello HDInsight Tools finns [utveckla C#-topologier med hello HDInsight Tools för Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

Använd hello följande steg toodeploy exempel-tooyour Storm på HDInsight-kluster och sedan visa och hantera hello-topologi.

1. Om du inte redan har installerat hello senaste versionen av hello HDInsight Tools för Visual Studio finns [komma igång med HDInsight Tools för Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).

2. Öppna Visual Studio, markera **filen** > **ny** > **projekt**.

3. I hello **nytt projekt** dialogrutan Expandera **installerad** > **mallar**, och välj sedan **HDInsight**. Välj hello listan över mallar **Storm exempel**. Ange ett namn för programmet hello hello längst ned i dialogrutan för hello i.

    ![Bild](./media/hdinsight-storm-deploy-monitor-topology/sample.png)

4. I **Solution Explorer**, högerklicka på hello-projektet och välj **skicka tooStorm på HDInsight**.

   > [!NOTE]
   > Om du uppmanas ange hello inloggningsuppgifterna för din Azure-prenumeration. Om du har mer än en prenumeration kan du logga in toohello en som innehåller ditt Storm på HDInsight-kluster.

5. Välj ditt Storm på HDInsight-kluster från hello **Storm-kluster** listrutan och välj sedan **skicka**. Du kan övervaka om hello ansökan har genomförts med hello **utdata** fönster.

6. När hello-topologi har skickats, hello **Storm-topologier** för hello klustret ska visas. Välj hello topologi hello listan tooview information om hello kör topologi.

    ![Övervakare för Visual studio](./media/hdinsight-storm-deploy-monitor-topology/vsmonitor.png)

   > [!NOTE]
   > Du kan också visa **Storm-topologier** från **Server Explorer** genom att expandera **Azure** > **HDInsight**, och sedan Högerklicka på ett Storm på HDInsight-kluster och välja **visa Storm-topologier**.

    Markera hello form för hello spouts eller bolts tooview information om dessa komponenter. För varje vald öppnas ett nytt fönster.

   > [!NOTE]
   > hello hello topologi heter hello klassnamnet för hello-topologi (i det här fallet `HelloWord`,) med en tidstämpel läggas till.

7. Från hello **Topology Summary** väljer **Kill** toostop hello-topologi.

   > [!NOTE]
   > Storm-topologier fortsätta köras förrän de har stoppats eller hello klustret tas bort.


## <a name="rest-api"></a>REST API

hello Storm-Användargränssnittet är byggt på hello REST-API, så att du kan göra liknande hantering och övervakning av funktioner med hjälp av hello REST API. Du kan använda hello REST API toocreate anpassade verktyg för att hantera och övervaka Storm-topologier.

Mer information finns i [Storm UI REST API](https://github.com/apache/storm/blob/0.9.3-branch/STORM-UI-REST-API.md). hello är följande uppgifter specifika toousing hello REST-API med Apache Storm på HDInsight.

### <a name="base-uri"></a>Bas-URI

hello bas-URI för hello REST API på HDInsight-kluster är **https://&lt;klusternamn >.azurehdinsight.net/stormui/api/v1/**, där **klusternamn** är hello namnet på ditt Storm på HDInsight-kluster.

### <a name="authentication"></a>Autentisering

Begäranden toohello REST API måste använda **grundläggande autentisering**, så du använda hello HDInsight-kluster administratörens namn och lösenord.

> [!NOTE]
> Eftersom grundläggande autentisering skickas i klartext, bör du **alltid** använder toosecure HTTPS-kommunikation med hello-kluster.

### <a name="return-values"></a>Returnera värden

Information som returneras från hello REST-API kan endast användas från inom hello kluster eller virtuella datorer på hello samma virtuella Azure-nätverk som hello kluster. Hello fullständigt kvalificerade domännamnet (FQDN) returneras för Zookeeper-servrar är till exempel inte att komma åt från hello Internet.

## <a name="next-steps"></a>Nästa steg

Nu när du har lärt dig hur toodeploy och övervaka topologier med hjälp av hello Storm-instrumentpanelen, lär du dig hur du:

* [Utveckla C#-topologier med hello HDInsight Tools för Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md)

* [Utveckla Java-baserad topologier med Maven](hdinsight-storm-develop-java-topology.md)

En lista över flera exempeltopologier finns [exempeltopologier för Storm på HDInsight](hdinsight-storm-example-topology.md).

[hdinsight-dashboard]: ./media/hdinsight-storm-deploy-monitor-topology/dashboard-link.png
[storm-dashboard-submit]: ./media/hdinsight-storm-deploy-monitor-topology/submit.png
[storm-dashboard-ui]: ./media/hdinsight-storm-deploy-monitor-topology/storm-ui-summary.png
