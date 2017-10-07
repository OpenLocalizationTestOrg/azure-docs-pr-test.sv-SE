---
title: aaaUse Tez UI med Windows-baserade HDInsight - Azure | Microsoft Docs
description: "Lär dig hur toouse hello Tez UI toodebug Tez jobb på Windows-baserade HDInsight HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: a55bccb9-7c32-4ff2-b654-213a2354bd5c
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/17/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 7ae21242ee1f8dc34a8501bed1ca995480885540
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-tez-ui-toodebug-tez-jobs-on-windows-based-hdinsight"></a>Använda hello Tez UI toodebug Tez jobb på Windows-baserade HDInsight
Hej Tez UI är en webbsida som kan använda toounderstand och felsöka jobb som använder Tez som motorn för körning av hello på Windows-baserade HDInsight-kluster. Hej Tez UI kan toovisualize hello jobb som ett diagram över anslutna objekt detaljer om varje objekt och hämta statistik och loggningsinformation.

> [!IMPORTANT]
> hello stegen i det här dokumentet kräver ett HDInsight-kluster som använder Windows. Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="prerequisites"></a>Krav
* Ett Windows-baserade HDInsight-kluster. Anvisningar om hur du skapar ett nytt kluster finns [komma igång med Windows-baserade HDInsight](hdinsight-hadoop-tutorial-get-started-windows.md).

  > [!IMPORTANT]
  > Hej Tez UI är bara tillgängligt på Windows-baserade HDInsight-kluster som skapas efter den 8 februari 2016.
  >
  >
* En Windows-baserade fjärrskrivbordsklienten.

## <a name="understanding-tez"></a>Förstå Tez
Tez är ett utökningsbart ramverk för databearbetning i Hadoop och som ger högre hastigheter än traditionella MapReduce-bearbetning. För Windows-baserade HDInsight-kluster är det en valfri motor som du kan aktivera för Hive med hjälp av följande kommando som en del av Hive-fråga hello:

    set hive.execution.engine=tez;

När arbetet är skickade tooTez, skapar en dirigeras acykliska diagram (DAG) som beskriver hello ordningen för körningen av hello-åtgärder som krävs av hello jobb. Enskilda åtgärder kallas formhörnen och köra en typ av hello övergripande jobb. hello faktiska utförande av hello beskrivs av en nod kallas för en aktivitet och kan distribueras över flera noder i klustret hello.

### <a name="understanding-hello-tez-ui"></a>Förstå hello Tez UI
Hej Tez UI är en webbsida ger information om processer som körs eller har kördes tidigare med Tez. Det gör att du tooview hello DAG som genererats av Tez, hur den distribueras till kluster, räknare, till exempel minne som används av uppgifter och formhörnen och information om felet. Den kan erbjuda användbar information i hello följande scenarier:

* Övervaka tidskrävande processer, visa hello förloppet för kartan och minska uppgifter.
* Analysera historiska data för lyckade eller misslyckade processer toolearn hur bearbetning kan förbättras eller orsaken till felet.

## <a name="generate-a-dag"></a>Generera en DAG
Hej Tez UI innehåller bara data om ett jobb som använder hello Tez motorn körs eller har varit kördes i hello tidigare. Enkel Hive-frågor kan vanligtvis lösas utan att använda Tez, men mer komplexa frågor som gör filtrering, gruppering, sortering, kopplingar, etc. kräver vanligtvis Tez.

Använd följande steg toorun en Hive-fråga som ska köras med hjälp av Tez hello.

1. I en webbläsare, navigerar du toohttps://CLUSTERNAME.azurehdinsight.net, där **KLUSTERNAMN** är hello namnet på ditt HDInsight-kluster.
2. Välj hello hello menyn hello överst på sidan hello **Hive-redigeraren**. Då visas en sida med hello följande exempelfråga.

        Select * from hivesampletable

    Radera hello exempelfråga och Ersätt den med hello följande.

        set hive.execution.engine=tez;
        select market, state, country from hivesampletable where deviceplatform='Android' group by market, country, state;
3. Välj hello **skicka** knappen. Hej **jobbet Session** avsnittet längst ned hello hello sidan visar hello status för hello fråga. En gång hello status ändras för**slutförd**väljer hello **visa information** länka tooview hello resultat. Hej **Jobbutdata** ska vara liknande toohello följande:

        en-GB   Hessen      Germany
        en-GB   Kingston    Jamaica
        en-GB   Nairobi Area    Kenya

## <a name="use-hello-tez-ui"></a>Använd hello Tez UI
> [!NOTE]
> Hej Tez UI är endast tillgänglig från hello desktop head hello klusternoder, så du måste använda Fjärrskrivbord tooconnect toohello huvudnoderna.
>
>

1. Från hello [Azure-portalen](https://portal.azure.com), Välj ditt HDInsight-kluster. Hello överkant hello HDInsight bladet välj hello **fjärrskrivbord** ikon. Detta visar hello remote desktop-bladet

    ![Ikon för Remote desktop](./media/hdinsight-debug-tez-ui/remotedesktopicon.png)
2. Hello fjärrskrivbord bladet välj **Anslut** tooconnect toohello klustrets huvudnod. Vid uppmaning används hello klustret användarens namn och lösenord tooauthenticate hello fjärrskrivbordsanslutning.

    ![Anslutningen till fjärrskrivbord ikon](./media/hdinsight-debug-tez-ui/remotedesktopconnect.png)

   > [!NOTE]
   > Om du inte har aktiverat Fjärrskrivbord-anslutningen, ange ett användarnamn, lösenord och upphör att gälla och välj sedan **aktivera** tooenable fjärrskrivbord. Använd hello föregående steg tooconnect när den har aktiverats.
   >
   >
3. När du är ansluten, öppna Internet Explorer på hello fjärrskrivbordet väljer hello växeln ikonen i hello övre högra hörnet på hello webbläsaren och välj sedan **Kompatibilitetsvyinställningarna**.
4. Hello längst ned i **Kompatibilitetsvyinställningarna**avmarkerar hello kryssrutan för **visa intranätplatser i Kompatibilitetsvy** och **Använd Microsoft kompatibilitetslista**, och välj sedan **Stäng**.
5. Bläddra i Internet Explorer, toohttp://headnodehost:8188/tezui / #/. Detta visar hello Tez UI

    ![Tez-Gränssnittet](./media/hdinsight-debug-tez-ui/tezui.png)

    När hello Tez UI läses in, visas en lista över dag som för närvarande körs eller har körts på hello klustret. hello standardvyn innehåller hello Dag Name, -Id, runtimenamn, Skickat, Status, starttid, sluttid, varaktighet, program-ID och kön. Fler kolumner läggas med hello växeln-ikonen på hello höger i hello sidan.

    Om du har en enda post blir för hello-fråga som du körde hello föregående avsnitt. Om du har flera poster kan du söka genom att ange sökvillkor i hello fälten ovanför hello dag och sedan klicka på **RETUR**.
6. Välj hello **Dag Name** för hello senaste DAG posten. Information om hello DAG, samt hello alternativet toodownload en zip JSON-filer som innehåller information om hello DAG visas.

    ![DAG-information](./media/hdinsight-debug-tez-ui/dagdetails.png)
7. Ovan hello **DAG information** är flera länkar som kan vara används toodisplay information om hello DAG.

   * **DAG räknare** visar informationen från prestandaräknarna för denna DAG.
   * **Grafisk vy** visar en grafisk representation av den här DAG.
   * **Alla formhörnen** visar en lista över hello formhörnen i denna DAG.
   * **Alla aktiviteter** visar en lista över hello aktiviteter för alla hörn i denna DAG.
   * **Alla TaskAttempts** visar information om hello försöker toorun uppgifter för denna DAG.

     > [!NOTE]
     > Om du bläddrar hello kolumnen visas för formhörnen, uppgifter och TaskAttempts Observera att det finns länkar tooview **räknare** och **visa eller hämta loggar** för varje rad.
     >
     >

     Om det uppstod ett fel med hello jobb, visas hello DAG information statusen misslyckades, tillsammans med länkar tooinformation om hello om aktiviteten. Diagnostikinformationen visas under hello DAG information.
8. Välj **grafisk vy**. Visar en grafisk representation av hello DAG. Du kan placera hello muspekaren över varje nod i hello visa toodisplay information om den.

    ![Grafisk vy](./media/hdinsight-debug-tez-ui/dagdiagram.png)
9. Klicka på en brytpunkt läses hello **Vertex information** för objektet. Klicka på hello **kartan 1** vertex toodisplay information för den här artikeln. Välj **Bekräfta** tooconfirm hello navigering.

    ![Vertex information](./media/hdinsight-debug-tez-ui/vertexdetails.png)
10. Observera att du nu har länkar hello överst på hello sidan som är relaterade toovertices och uppgifter.

    > [!NOTE]
    > Du kan också komma fram till den här sidan genom att gå tillbaka för**DAG information**, välja **Vertex information**, och sedan välja hello **kartan 1** hörn.
    >
    >

    * **Vertex räknare** visar räknarinformation för den här hörn.
    * **Uppgifter** uppgifter för den här vertex visas.
    * **Uppgift försök** visar information om försök toorun uppgifter för den här hörn.
    * **Datakällor & egenskaperna** visar datakällor och egenskaperna för den här hörn.

      > [!NOTE]
      > Som med tidigare hello-menyn kan du bläddrar hello kolumnen visas för aktiviteter, länkar aktivitet försök och källor & Sinks__ toodisplay toomore information för varje artikel.
      >
      >
11. Välj **uppgifter**, och sedan väljer hello-objekt med namnet **00_000000**. Detta visar **aktivitetsinformation** för den här uppgiften. Du kan visa från den här skärmen **aktivitet räknare** och **aktivitet försök**.

    ![Uppgiftsinformation](./media/hdinsight-debug-tez-ui/taskdetails.png)

## <a name="next-steps"></a>Nästa steg
Nu när du har lärt dig hur toouse hello Tez visa, lär du dig mer om [med hjälp av Hive i HDInsight](hdinsight-use-hive.md).

Mer teknisk information om Tez finns hello [Tez sidan vid Hortonworks](http://hortonworks.com/hadoop/tez/).
