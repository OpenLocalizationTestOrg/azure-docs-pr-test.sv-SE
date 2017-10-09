---
title: "aaaMonitor och hantera Azure HDInsight med Ambari-Webbgränssnittet | Microsoft Docs"
description: "Lär dig hur toouse Ambari toomonitor och hantera Linux-baserade HDInsight-kluster. I det här dokumentet beskrivs hur toouse hello Ambari-Webbgränssnittet som ingår i HDInsight-kluster."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 4787f3cc-a650-4dc3-9d96-a19a67aad046
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: d422c40e63345d7054839a625e115c50dad040f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-hdinsight-clusters-by-using-hello-ambari-web-ui"></a>Hantera HDInsight-kluster med hjälp av hello Ambari-Webbgränssnittet

[!INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

Apache Ambari förenklar hello hantering och övervakning av ett Hadoop-kluster genom att tillhandahålla en enkel toouse web användargränssnitt och REST API. Ambari ingår i Linux-baserade HDInsight-kluster och har använt toomonitor hello klustret och gör ändringar i konfigurationen.

I det här dokumentet beskrivs hur toouse hello Ambari-Webbgränssnittet med ett HDInsight-kluster.

## <a id="whatis"></a>Vad är Ambari?

[Apache Ambari](http://ambari.apache.org) förenklar Hadoop-hanteringen genom att tillhandahålla en enkel att använda webbgränssnittet. Du kan använda Ambari skapa, hantera och övervaka Hadoop-kluster. Utvecklare kan integrera dessa funktioner i sina program med hjälp av hello [Ambari REST API: er](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

Hej Ambari-Webbgränssnittet är som standard med HDInsight-kluster som använder hello Linux-operativsystem.

> [!IMPORTANT]
> Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement). 

## <a name="connectivity"></a>Anslutning

Hej Ambari-Webbgränssnittet är tillgängligt på ditt HDInsight-kluster på HTTPS://CLUSTERNAME.azurehdidnsight.net, där **KLUSTERNAMN** är hello namnet på klustret.

> [!IMPORTANT]
> Ansluta tooAmbari på HDInsight kräver HTTPS. När du uppmanas att autentisera använda hello admin kontonamn och lösenord som du angav när hello klustret har skapats.

## <a name="ssh-tunnel-proxy"></a>SSH-tunnel (proxy)

När Ambari för klustret kan komma åt direkt via hello Internet, hello vissa länkar från hello Ambari-Webbgränssnittet (till exempel toohello JobTracker) inte är tillgängliga på internet. tooaccess dessa tjänster, måste du skapa en SSH-tunnel. Mer information finns i [använda SSH-tunnlar med HDInsight](hdinsight-linux-ambari-ssh-tunnel.md).

## <a name="ambari-web-ui"></a>Ambari-webbgränssnittet

När du ansluter toohello Ambari-Webbgränssnittet är ange tooauthenticate toohello sidan. Använd hello klustret administratörsanvändare (standard Admin) och lösenord som du använde när klustret skapas.

Observera hello fältet hello överst när hello sidan öppnas. Det här fältet innehåller hello följande information och kontroller:

![ambari-nav](./media/hdinsight-hadoop-manage-ambari/ambari-nav.png)

* **Ambari logotypen** -öppnas hello instrumentpanelen, som kan vara används toomonitor hello klustret.

* **Klustrets namn # ops** -visar hello Antal pågående Ambari-åtgärder. Du väljer hello klusternamnet eller **# ops** visar en lista över bakgrundsåtgärder.

* **# aviseringar** -visar varningar eller kritiska varningar för hello klustret.

* **Instrumentpanelen** -visar hello instrumentpanelen.

* **Tjänster** - och konfigurationsinformation inställningar för hello tjänster i hello kluster.

* **Värdar** - och konfigurationsinformation inställningar för hello noder i klustret hello.

* **Aviseringar** -en logg över information, varningar och kritiska aviseringar.

* **Admin** -stacken/programtjänster som är installerade på hello kluster, tjänstens kontoinformation och Kerberos-säkerhet.

* **Admin-knappen** -Ambari hantering, användarinställningar och logga ut.

## <a name="monitoring"></a>Övervakning

### <a name="alerts"></a>Aviseringar

hello innehåller följande lista hello vanliga avisering statusar används av Ambari:

* **OKEJ**
* **Varning**
* **KRITISKA**
* **OKÄND**

Aviseringar än **OK** orsaka hello **# aviseringar** post hello överst i hello sidan toodisplay hello antalet aviseringar. Om du markerar den här posten visas hello aviseringar och deras status.

Aviseringar är uppdelade i flera standardgrupper som kan visas från hello **aviseringar** sidan.

![Sidan varningar](./media/hdinsight-hadoop-manage-ambari/alerts.png)

Du kan hantera hello grupper med hjälp av hello **åtgärder** menyn och välja **hantera avisering grupper**.

![Hantera aviseringar grupper dialogrutan](./media/hdinsight-hadoop-manage-ambari/manage-alerts.png)

Du kan också hantera aviseringar metoder och skapa aviseringar från hello **åtgärder** menyn genom att välja __hantera avisering meddelanden__. De aktuella meddelanden visas. Du kan också skapa aviseringar här. Meddelanden kan skickas **e-post** eller **SNMP** när specifika/allvarlighetsgraden för aviseringen kombinationer inträffar. Du kan exempelvis skicka ett e-postmeddelande när något av hello aviseringar i hello **YARN standard** grupp har angetts för**kritisk**.

![Skapa varningsruta](./media/hdinsight-hadoop-manage-ambari/create-alert-notification.png)

Slutligen väljer __Hantera aviseringsinställningar__ från hello __åtgärder__ menyn kan du tooset hello antalet gånger som en avisering måste inträffa innan ett meddelande har skickats. Den här inställningen kan vara används tooprevent meddelanden för tillfälliga fel.

### <a name="cluster"></a>Kluster

Hej **mått** fliken hello instrumentpanelen innehåller ett antal widgetar som gör det enkelt toomonitor hello status på klustret i korthet. Flera widgetar som **CPU-användning**, innehåller ytterligare information när du klickar på.

![instrumentpanel med mått](./media/hdinsight-hadoop-manage-ambari/metrics.png)

Hej **Heatmaps** fliken visar mått som färgade heatmaps, från grön toored.

![instrumentpanel med heatmaps](./media/hdinsight-hadoop-manage-ambari/heatmap.png)

Mer information om hello noder i hello kluster, Välj **värdar**. Välj sedan hello viss nod som du är intresserad av.

![information om värden](./media/hdinsight-hadoop-manage-ambari/host-details.png)

### <a name="services"></a>Tjänster

Hej **Services** sidopanelen på hello instrumentpanelen ger snabb inblick i hello status för hello-tjänster som körs på hello klustret. Olika ikoner är används tooindicate status eller åtgärder som ska vidtas. Till exempel visas en gul återvinning symbol om en tjänst behöver toobe återanvänds.

![tjänster-sidorutan](./media/hdinsight-hadoop-manage-ambari/service-bar.png)

> [!NOTE]
> hello-tjänster som visas skiljer sig åt mellan HDInsight-klustertyper och versioner. hello-tjänster som visas här är annorlunda än hello-tjänster som visas för klustret.

Att välja en tjänst visar mer detaljerad information på hello-tjänsten.

![sammanfattningsinformation för tjänsten](./media/hdinsight-hadoop-manage-ambari/service-details.png)

#### <a name="quick-links"></a>Snabblänkar

Vissa tjänster visas en **snabblänkar** länk hello överst på hello sidan. Detta kan vara används tooaccess tjänstspecifika web användargränssnitt, exempelvis:

* **Jobbhistorik** -MapReduce historiken för datalagerjobb.
* **Hanteraren för filserverresurser** -YARN ResourceManager-Användargränssnittet.
* **NameNode** -Hadoop Distributed File System (HDFS) NameNode Användargränssnittet.
* **Oozie webbgränssnittet** -Oozie-Användargränssnittet.

När du väljer ett av dessa länkar öppnas en ny flik i webbläsaren, som visar hello valda sidan.

> [!NOTE]
> Du väljer hello **snabblänkar** post för en tjänst kan returnera ett ”det gick inte att hitta” serverfel. Om felet uppstår, måste du använda en SSH-tunnel när du använder hello **snabblänkar** post för den här tjänsten. Mer information finns i [använda SSH-tunnlar med HDInsight](hdinsight-linux-ambari-ssh-tunnel.md)

## <a name="management"></a>Hantering

### <a name="ambari-users-groups-and-permissions"></a>Ambari användare, grupper och behörigheter

Arbeta med användare, grupper och behörigheter stöds när du använder en [domänanslutna](hdinsight-domain-joined-introduction.md) HDInsight-kluster. Information om hur du använder hello Ambari Management Användargränssnittet på en domänansluten klustret finns [hantera domänanslutna HDInsight-kluster](hdinsight-domain-joined-introduction.md).

> [!WARNING]
> Ändra inte hello lösenordet för hello Ambari watchdog (hdinsightwatchdog) på Linux-baserade HDInsight-kluster. Ändra hello lösenord radbrytningar hello möjlighet toouse skriptåtgärder eller utföra skalning åtgärder med ditt kluster.

### <a name="hosts"></a>Värdar

Hej **värdar** sidan listar alla värdar i klustret hello. toomanage värdar, Följ dessa steg.

![sidan för värdar](./media/hdinsight-hadoop-manage-ambari/hosts.png)

> [!NOTE]
> Lägga till, inaktivering och recommissioning en värd får inte användas med HDInsight-kluster.

1. Välj hello-värd som du vill toomanage.

2. Använd hello **åtgärder** menyn tooselect hello åtgärd tooperform gärna:

   * **Starta alla komponenter** -starta alla komponenter på hello värden.

   * **Stoppa alla komponenter** -stoppa alla komponenter på hello värden.

   * **Starta om alla komponenter** - stoppa och starta alla komponenter på hello värden.

   * **Aktivera underhållsläget** -Undertrycker aviseringar för hello värden. Det här läget måste vara aktiverat om du utför åtgärder som genererar aviseringar. Exempelvis stoppa och starta en tjänst.

   * **Inaktivera underhållsläge** -returnerar hello värden toonormal aviseringar.

   * **Stoppa** -stoppar DataNode eller NodeManagers på hello värden.

   * **Starta** -startar DataNode eller NodeManagers på hello värden.

   * **Starta om** -stoppar och startar DataNode eller NodeManagers på hello värden.

   * **Inaktivera** -tar bort en värd från hello klustret.

     > [!NOTE]
     > Använd inte den här åtgärden på HDInsight-kluster.

   * **Recommission** -lägger till ett tidigare inaktiverade toohello värdkluster.

     > [!NOTE]
     > Använd inte den här åtgärden på HDInsight-kluster.

### <a id="service"></a>Tjänster

Från hello **instrumentpanelen** eller **Services** använder hello **åtgärder** knappen längst ned hello hello lista över tjänster toostop och starta alla tjänster.

![tjänståtgärder](./media/hdinsight-hadoop-manage-ambari/service-actions.png)

> [!WARNING]
> Medan **lägga till tjänsten** visas i den här menyn ska inte använda tooadd tjänster toohello HDInsight-kluster. Nya tjänster ska läggas till med en skriptåtgärd under klusteretablering. Mer information om hur du använder skriptåtgärder finns [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md).

När hello **åtgärder** knappen kan starta om alla tjänster, ofta du vill toostart, stoppa eller starta om en specifik tjänst. Använd följande steg tooperform åtgärder på en enskild tjänst hello:

1. Från hello **instrumentpanelen** eller **Services** väljer du en tjänst.

2. Från hello överkant hello **sammanfattning** använder hello **tjänståtgärder** knappen och markera hello åtgärden tootake. Detta startar om hello-tjänsten på alla noder.

    ![åtgärden på tjänsten](./media/hdinsight-hadoop-manage-ambari/individual-service-actions.png)

   > [!NOTE]
   > Starta om vissa tjänster medan hello klustret körs kan generera aviseringar. tooavoid aviseringar, du kan använda hello **tjänståtgärder** knappen tooenable **underhållsläge** för hello-tjänsten innan du utför hello omstart.

3. När en åtgärd har valts, hello **# op** post hello överst i hello sidan steg tooshow som en bakgrundsåtgärden pågår. Om konfigurerat toodisplay visas hello listan över bakgrundsåtgärder.

   > [!NOTE]
   > Om du har aktiverat **underhållsläge** hello tjänsten kommer ihåg toodisable den med hjälp av hello **tjänståtgärder** knappen när hello-åtgärden har slutförts.

tooconfigure en tjänst använda hello följande steg:

1. Från hello **instrumentpanelen** eller **Services** väljer du en tjänst.

2. Välj hello **konfigurationerna** fliken hello aktuell konfiguration visas. En lista över tidigare konfigurationer visas också.

    ![Konfigurationer](./media/hdinsight-hadoop-manage-ambari/service-configs.png)

3. Använd hello fält visas toomodify hello konfiguration och välj sedan **spara**. Eller välj en tidigare konfiguration och välj sedan **se aktuella** tooroll tillbaka toohello tidigare inställningar.

## <a name="ambari-views"></a>Ambari-vyer

Ambari-vyer kan utvecklare tooplug UI-element i hello Ambari-Webbgränssnittet med hello [Ambari vyer Framework](https://cwiki.apache.org/confluence/display/AMBARI/Views). HDInsight tillhandahåller följande vyer med Hadoop klustertyper hello:

* Yarn Queue Manager: hello köhanteraren ger ett enkelt gränssnitt för att visa och ändra YARN köer.

* Hive vy: hello Hive-vy kan du toorun Hive-frågor direkt från din webbläsare. Du kan spara frågor, visa resultat, och spara resultatet toohello klusterlagringen eller hämta resultaten tooyour lokalt system. Mer information om hur du använder Hive vyer finns [Använd Hive-vyer med HDInsight](hdinsight-hadoop-use-hive-ambari-view.md).

* Tez vy: hello Tez vyn kan du toobetter förstå och optimera jobb. Du kan visa information om hur Tez-jobb körs och vilka resurser som används.
