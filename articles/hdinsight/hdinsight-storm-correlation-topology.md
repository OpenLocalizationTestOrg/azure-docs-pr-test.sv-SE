---
title: "aaaCorrelate händelser över tid med Storm och HBase i HDInsight"
description: "Lär dig hur toocorrelate händelser som visas vid olika tidpunkter med Storm och HBase på HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 17d11479-2d02-4790-8d29-05fb38351479
ms.service: hdinsight
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/07/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: f915eef77bbda5b02bfd02ad0b5a4923ff2f4f26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="correlate-events-that-arrive-at-different-times-using-storm-and-hbase"></a>Samordna händelser anländer vid olika tidpunkter med Storm och HBase

Du kan jämföra poster som visas vid olika tidpunkter med hjälp av en beständig datalager med Apache Storm. Till exempel varade länka logga in och logga ut händelser för en användare session toocalculate hur länge hello session.

I detta dokument kan du lära dig hur toocreate en grundläggande C# Storm-topologi som följer logga in och logga ut händelser för användarsessioner och beräknar hello varaktighet hello-sessionen. hello topologin använder HBase som en beständig datalager. HBase kan du också tooperform batch frågor på hello historiska data tooproduce ytterligare insikter. Hur många användarsessioner har startats eller avslutats under en viss tidsperiod.

## <a name="prerequisites"></a>Krav

* Visual Studio och hello HDInsight tools för Visual Studio. Mer information finns i [komma igång med hello HDInsight tools för Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).

* Apache Storm på HDInsight-kluster (Windows-baserade).

  > [!WARNING]
  > Medan SCP.NET topologier stöds på Linux-baserade Storm-kluster som skapas efter 2016/10/28 fungerar hello HBase SDK för .NET-paket som är tillgängliga från och med 10/28/2016 inte på Linux-baserat HDInsight

* Apache HBase på HDInsight-kluster (Linux och Windows-baserade).

  > [!IMPORTANT]
  > Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* [Java](https://java.com) 1.7 eller mer på din utvecklingsmiljö. Java är används toopackage hello topologi när det inskickade toohello HDInsight-kluster.

  * Hej **JAVA_HOME** miljö variabeln måste punkt toohello katalog som innehåller Java.
  * Hej **%JAVA_HOME%/bin** katalogen måste vara i hello sökväg

## <a name="architecture"></a>Arkitektur

![Diagram över hello dataflöde via hello-topologi](./media/hdinsight-storm-correlation-topology/correlationtopology.png)

Korrelerar händelser kräver ett gemensamt ID för hello händelsekälla. Till exempel ett användar-ID, sessions-ID eller annan typ av data som är en) unikt och b) ingår i alla data som skickas tooStorm. Det här exemplet används en GUID-värde toorepresent ett sessions-ID.

Det här exemplet består av två HDInsight-kluster:

* HBase: beständiga datalager för historiska data
* Storm: används tooingest inkommande data

hello data genereras slumpmässigt av hello Storm-topologi och består av hello följande objekt:

* Sessions-ID: ett GUID som unikt identifierar varje session
* Händelse: en START- eller händelse. I det här exemplet inträffar START alltid före slut
* Tid: hello tidpunkt hello händelsen.

Dessa data bearbetas och lagras i HBase.

### <a name="storm-topology"></a>Storm-topologi

När en session startar en **starta** händelse tas emot av hello topologi och loggas tooHBase. När en **END** händelsen har tagits emot, hello topologi hämtar hello **starta** händelse och beräknar hello tiden mellan hello två händelser. Detta **varaktighet** värdet lagras sedan i HBase tillsammans med hello **END** händelseinformation.

> [!IMPORTANT]
> När den här topologin visar hello grundläggande mönster, måste en lösning för produktion tootake design för hello följande scenarier:
>
> * Händelser anländer utanför ordning
> * Duplicera händelser
> * Ignorerade händelser

Hej exempeltopologi består av hello följande komponenter:

* Session.CS: simulerar en användarsession genom att skapa ett slumpmässigt sessions-ID, start tiden och hur länge hello session varar.

* Spout.CS: skapar 100 sessioner, genererar en Starthändelse, väntar hello slumpmässig tidsgräns för varje session och skickar en händelse från SLUTPUNKT. Återanvänder avslutats sedan sessioner toogenerate nya.

* HBaseLookupBolt.cs: använder hello sessions-ID toolook upp sessionsinformation från HBase. När en händelse från SLUTPUNKT bearbetas hittar hello motsvarande Starthändelse och beräknar hello varaktighet hello-sessionen.

* HBaseBolt.cs: Lagrar informationen i HBase.

* TypeHelper.cs: Hjälper dig med typkonvertering när från läsning / skrivning tooHBase.

### <a name="hbase-schema"></a>HBase-schema

I HBase lagras hello data i en tabell med följande inställningar för schema/hello:

* Radnyckel: hello sessions-ID används som hello nyckel för rader i den här tabellen.

* Kolumnfamilj: hello namn är 'cf'. Kolumner som lagras i den här serien är:

  * händelse: början eller slutet.

  * tid: hello tid i millisekunder som hello händelsen inträffade.

  * varaktighet: hello är längden mellan START- och SLUTDATUM händelse.

* VERSIONER: hello 'cf-familjen anges tooretain 5 versioner av varje rad.

  > [!NOTE]
  > Versioner är en logg över tidigare värden som lagras för en specifik rad-nyckel. Som standard returnerar HBase endast hello värdet för hello den senaste versionen av en rad. I det här fallet används hello samma rad för alla händelser (START, avslutas.) varje version av en rad har identifierats av hello tidsstämpelvärde. Versioner ger en historisk bild av händelser som loggats för ett visst-ID.

## <a name="download-hello-project"></a>Ladda ned hello-projekt

hello exempelprojektet kan hämtas från [https://github.com/Azure-Samples/hdinsight-storm-dotnet-event-correlation](https://github.com/Azure-Samples/hdinsight-storm-dotnet-event-correlation).

Den här filen innehåller hello följande C#-projekt:

* CorrelationTopology: C# Storm-topologi som slumpmässigt genererar händelser som start- och slutdatum för användarsessioner. Varje session varar mellan 1 och 5 minuter.

* SessionInfo: C#-konsolprogram som skapar hello HBase-tabell och innehåller exempelfrågor tooreturn information om lagrade sessionsdata.

## <a name="create-hello-table"></a>Skapa hello tabell

1. Öppna hello **SessionInfo** projekt i Visual Studio.

2. I **Solution Explorer**, högerklicka på hello **SessionInfo** projektet och välj **egenskaper**.

    ![Skärmbild av menyn med egenskaper valt](./media/hdinsight-storm-correlation-topology/selectproperties.png)

3. Välj **inställningar**, och sedan ange hello följande värden:

   * HBaseClusterURL: hello URL tooyour HBase-kluster. Till exempel https://myhbasecluster.azurehdinsight.net.

   * HBaseClusterUserName: Hej admin/http-användarkonto för klustret

   * HBaseClusterPassword: hello lösenordet för användarkontot för hello admin/HTTP

   * HBaseTableName: hello namnet på hello tabellen toouse med det här exemplet

   * HBaseTableColumnFamily: hello kolumnnamn

     ![Bild av dialogrutan Inställningar](./media/hdinsight-storm-correlation-topology/settings.png)

4. Kör hello lösning. När du uppmanas, Välj hello ”c” viktiga toocreate hello tabell på din HBase-kluster.

## <a name="build-and-deploy-hello-storm-topology"></a>Skapa och distribuera hello Storm-topologi

1. Öppna hello **CorrelationTopology** lösningen i Visual Studio.

2. I **Solution Explorer**, högerklicka på hello **CorrelationTopology** projektet och välj Egenskaper.

3. I egenskapsfönstret för hello Välj **inställningar** och ange hello konfigurationsvärden för det här projektet. hello första 5 är hello samma värden som används av hello **SessionInfo** projektet:

   * HBaseClusterURL: hello URL tooyour HBase-kluster. Till exempel https://myhbasecluster.azurehdinsight.net.

   * HBaseClusterUserName: hello admin/http-användarkonto för klustret.

   * HBaseClusterPassword: hello lösenordet för användarkontot för hello admin/HTTP.

   * HBaseTableName: hello namnet på hello tabellen toouse med det här exemplet. Det här värdet är hello samma tabellnamn som används i hello SessionInfo projekt.

   * HBaseTableColumnFamily: hello familj kolumnnamnet. Det här värdet är hello samma familj kolumnnamn som används i hello SessionInfo projekt.

   > [!IMPORTANT]
   > Ändra inte hello HBaseTableColumnNames, som hello standardvärdena är hello-namn som används av **SessionInfo** tooretrieve data.

4. Spara hello egenskaper och sedan skapa hello-projekt.

5. I **Solution Explorer**, högerklicka på hello-projektet och välj **skicka tooStorm på HDInsight**. Om du uppmanas du ange hello autentiseringsuppgifter för din Azure-prenumeration.

   ![Bild av hello skicka toostorm menyobjekt](./media/hdinsight-storm-correlation-topology/submittostorm.png)

6. I hello **skicka topologi** dialogrutan, Välj hello Storm-kluster som du vill toodeploy den här topologin till.

   > [!NOTE]
   > hello kan första gången som du skickar en topologi, det ta några sekunder tooretrieve hello namnet på ditt HDInsight-kluster.

7. När hello-topologi har överföras och skickade toohello kluster, hello **Storm topologiska vyn** öppnas och visar hello kör topologi. toorefresh hello data, Välj hello **CorrelationTopology** och använder hello uppdateringsknappen på hello uppifrån hello-sidan.

   ![Bild av hello topologiska vyn](./media/hdinsight-storm-correlation-topology/topologyview.png)

   När hello topologi börjar generera data hello värdet i hello **avgivna** kolumnen steg.

   > [!NOTE]
   > Om hello **Storm topologiska vyn** inte öppnas automatiskt, använder du följande steg tooopen hello den:
   >
   > 1. I **Solution Explorer**, expandera **Azure**, och expandera sedan **HDInsight**.
   > 2. Högerklicka på hello Storm-kluster som hello topologi körs på och välj sedan **visa Storm-topologier**

## <a name="query-hello-data"></a>Fråga hello data

Använd hello följande steg tooquery hello data när data har släppts.

1. Returnera toohello **SessionInfo** projekt. Om inte körs startar du en ny instans av den.

2. När du uppmanas, Välj **s** toosearch för START-händelse. Du tillfrågas tooenter start och avsluta tid toodefine tidsintervall - returneras endast händelser mellan dessa två gånger.

    Använd hello följande format när du lägger till hello start- och sluttider: hh: mm och ''M' eller 'pm'. Till exempel 23:20:00.

    tooreturn loggas händelser, använda en starttid innan hello Storm-topologi har distribuerats och en sluttid för nu. hello returnera data innehåller poster liknande toohello följande text:

        Session e6992b3e-79be-4991-afcf-5cb47dd1c81c started at 6/5/2015 6:10:15 PM. Timestamp = 1433527820737

Söker efter slutet händelser fungerar hello samma som START-händelser. Dock genereras Sluthändelser slumpmässigt mellan 1 och 5 minuter efter hello START händelsen. Du kan ha tootry några tid adressintervall toofind hello Sluthändelser. END-händelser innehåller också hello varaktighet hello session - hello skillnaden mellan hello händelse starttid och sluttid för händelsen. Här är ett exempel på data för END händelser:

    Session fc9fa8e6-6892-4073-93b3-a587040d892e lasted 2 minutes, and ended at 6/5/2015 6:12:15 PM

> [!NOTE]
> Hello tid-värden som du anger är i lokal tid, är hello tid som har returnerats från hello fråga i UTC.

## <a name="stop-hello-topology"></a>Stoppa hello-topologi

När du är klar toostop hello topologi, returnera toohello **CorrelationTopology** projekt i Visual Studio. I hello **Storm topologiska vyn**hello topologi och välj sedan använda hello **Kill** knappen hello överst i hello topologiska vyn.

## <a name="delete-your-cluster"></a>Ta bort klustret

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Nästa steg

Fler Storm-exempel finns [exempeltopologier för Storm på HDInsight](hdinsight-storm-example-topology.md).
