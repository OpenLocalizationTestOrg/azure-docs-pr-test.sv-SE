---
title: aaaUse Apache Storm med Power BI - Azure HDInsight | Microsoft Docs
description: "Skapa en Power BI-rapport med hjälp av data från en C#-topologi som körs på en Apache Storm-kluster i HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 36fe3b9c-5232-4464-8d75-95403b6da7a1
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/31/2017
ms.author: larryfr
ms.openlocfilehash: 194cd8220bd60475af1d64a110e4b23ef92e1bc8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-power-bi-toovisualize-data-from-an-apache-storm-topology"></a>Använd Power BI toovisualize data från en Apache Storm-topologi

Powerbi kan du toovisually visa data som rapporter. Det här dokumentet innehåller ett exempel på hur toouse Apache Storm på HDInsight toogenerate data för Power BI.

> [!NOTE]
> hello stegen i det här dokumentet förlitar sig på en Windows-utvecklingsmiljö med Visual Studio. hello kompilerade projekt kan vara skickade tooa Linux-baserade HDInsight-kluster. Linux-baserade kluster skapas bara efter 10/28/2016 stöd SCP.NET topologier.
>
> toouse en C#-topologi med en Linux-baserade kluster update hello Microsoft.SCP.Net.SDK NuGet-paketet används av ditt projekt tooversion 0.10.0.6 eller högre. hello version av hello paketet måste även matcha hello huvudversion av Storm installerad på HDInsight. Storm på HDInsight versionerna 3.3 och 3.4 använder till exempel Storm version 0.10.x, medan HDInsight 3.5 använder Storm 1.0.x.
>
> C#-topologier på Linux-baserade kluster måste använda .NET 4.5 och Använd monoljud toorun på hello HDInsight-kluster. De flesta saker fungerar. Men du bör kontrollera hello [Mono kompatibilitet](http://www.mono-project.com/docs/about-mono/compatibility/) dokument för potentiella inkompatibiliteter.
>
> En Java-version av det här projektet som fungerar med Linux- eller Windows-baserade HDInsight finns [bearbeta händelser från Azure Event Hubs med Storm på HDInsight (Java)](hdinsight-storm-develop-java-event-hub-topology.md).

## <a name="prerequisites"></a>Krav

* En Azure Active Directory-användare med [Power BI](https://powerbi.com) åtkomst.
* Ett HDInsight-kluster. Mer information finns i [Kom igång med Storm på HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md).

  > [!IMPORTANT]
  > Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Visual Studio (ett av följande versioner hello)

  * Visual Studio 2012 med [update 4](http://www.microsoft.com/download/details.aspx?id=39305)
  * Visual Studio 2013 med [uppdatering 4](http://www.microsoft.com/download/details.aspx?id=44921) eller [Visual Studio Community 2013](http://go.microsoft.com/fwlink/?linkid=517284&clcid=0x409)
  * [Visual Studio 2015](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
  * 2017 för Visual Studio (någon utgåva)

* Hej HDInsight Tools för Visual Studio: se [komma igång med hello HDInsight Tools för Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md) information om installationsinformation.

## <a name="how-it-works"></a>Hur det fungerar

Det här exemplet innehåller en C# Storm-topologi som genererar slumpmässigt loggdata för Internet Information Services (IIS). Dessa data skrivs sedan tooa SQL-databas, och där det är används toogenerate rapporter i Power BI.

hello följande filer implementera hello huvudsakliga funktionerna i det här exemplet:

* **SqlAzureBolt.cs**: skriver information i hello Storm-topologi tooSQL databas.
* **IISLogsTable.sql**: hello Transact-SQL-instruktioner används toogenerate hello databas som hello data lagras i.

> [!WARNING]
> Skapa hello tabell i SQL-databasen innan du startar hello-topologi på ditt HDInsight-kluster.

## <a name="download-hello-example"></a>Hämta hello-exempel

Hämta hello [HDInsight C# Storm Power BI exempel](https://github.com/Azure-Samples/hdinsight-dotnet-storm-powerbi). toodownload, antingen förgrening/klona den med hjälp av [git](http://git-scm.com/), eller Använd hello **hämta** länk toodownload en .zip hello-arkivet.

## <a name="create-a-database"></a>Skapa en databas

1. toocreate en databas, Använd hello steg i hello [SQL Database-självstudier](../sql-database/sql-database-get-started.md) dokumentet.

2. Anslut toohello databasen med följande hello stegen i hello [ansluta tooa SQL Database med Visual Studio](../sql-database/sql-database-connect-query.md) dokumentet.

3. I Object Explorer, högerklicka på hello-databasen och välj **ny fråga**. Klistra in hello innehållet i hello **IISLogsTable.sql** -fil som ingår i hello hämtade projektet till hello frågefönstret och sedan använda Ctrl + Skift + E tooexecute hello frågan. Du bör få ett meddelande som hello kommandon har slutförts.

## <a name="configure-hello-sample"></a>Konfigurera hello-exempel

1. Från hello [Azure-portalen](https://portal.azure.com), Välj din SQL-databas. Från hello **Essentials** avsnitt i hello SQL-databasbladet väljer **visa databasanslutningssträngar**. Kopiera hello hello listan som visas, **ADO.NET (SQL-autentisering)** information.

2. Öppna hello exempel i Visual Studio. Från **Solution Explorer**öppnar hello **App.config** filen och leta upp följande post hello:

        <add key="SqlAzureConnectionString" value="##TOBEFILLED##" />

    Ersätt hello **# TOBEFILLED ##** värdet med hello databasanslutningssträng kopierade i föregående steg i hello. Ersätt **{din\_username}** och **{din\_lösenord}** med hello användarnamn och lösenord för hello-databasen.

3. Spara och Stäng hello-filer.

## <a name="deploy-hello-sample"></a>Distribuera hello-exempel

1. Från **Solution Explorer**, högerklicka på hello **StormToSQL** projektet och välj **skicka tooStorm på HDInsight**. Välj hello HDInsight-kluster från hello **Storm-kluster** listrutan dialogrutan.

   > [!NOTE]
   > Det kan ta några sekunder efter Hej **Storm-kluster** listrutan toopopulate med servernamn.
   >
   > Om du uppmanas ange hello inloggningsuppgifterna för din Azure-prenumeration. Om du har mer än en prenumeration kan du logga in toohello en som innehåller ditt Storm på HDInsight-kluster.

2. När hello-topologi har skickats, hello __topologi Viewer__ visas. tooview den här topologin, Välj hello SqlAzureWriterTopology tas hello-listan.

    ![hello topologier med hello-topologi som valts](./media/hdinsight-storm-power-bi-topology/topologyview.png)

    Du kan använda den här vyn toosee informationen hello-topologi eller dubbelklicka på en post (till exempel hello SqlAzureBolt) toosee specifika tooa komponenten i hello-topologi.

3. Efter hello har topologi för några minuter kördes returnerade toohello SQL frågefönstret som du använde toocreate hello-databasen. Ersätt befintliga hello-instruktioner med hello följande fråga:

        select * from iislogs;

    Använda Ctrl + Skift + E tooexecute hello fråga och du får resultat liknande toohello följande data:

        1    2016-05-27 17:57:14.797    255.255.255.255    /bar    GET    200
        2    2016-05-27 17:57:14.843    127.0.0.1    /spam/eggs    POST    500
        3    2016-05-27 17:57:14.850    123.123.123.123    /eggs    DELETE    200
        4    2016-05-27 17:57:14.853    127.0.0.1    /foo    POST    404
        5    2016-05-27 17:57:14.853    10.9.8.7    /bar    GET    200
        6    2016-05-27 17:57:14.857    192.168.1.1    /spam    DELETE    200

    Dessa data har skrivits från hello Storm-topologi.

## <a name="create-a-report"></a>Skapa en rapport

1. Ansluta toohello [Azure SQL Database connector](https://app.powerbi.com/getdata/bigdata/azure-sql-database-with-live-connect) för Power BI. 

2. Inom **databaser**väljer **hämta**.

3. Välj **Azure SQL Database**, och välj sedan **Anslut**.

    > [!NOTE]
    > Du kan bli ombedd toodownload hello Power BI Desktop toocontinue. I så fall använder du följande steg tooconnect hello:
    >
    > 1. Öppna Power BI Desktop och välj __hämta Data__.
    > 2 Markera __Azure__, och sedan __Azure SQL database__.

4. Ange hello information tooconnect tooyour Azure SQL Database. Du hittar den här informationen genom att besöka hello [Azure-portalen](https://portal.azure.com) och välja din SQL-databas.

   > [!NOTE]
   > Du kan också ange hello uppdateringsintervall och anpassade filter med hjälp av **aktivera avancerade alternativ** hello ansluta dialogrutan.

5. När du har anslutit visas en ny datamängd med samma namn som hello databas du är ansluten till hello. Välj hello dataset toobegin skapar rapporter.

6. Från **fält**, expandera hello **IISLOGS** transaktionen. toocreate en rapport som visar hello URI beror, markera kryssrutan för hello för **URISTEM**.

    ![Skapa en rapport](./media/hdinsight-storm-power-bi-topology/createreport.png)

7. Dra därefter **metoden** toohello rapporten. hello rapporten uppdateringar toolist hello beror och hello motsvarande HTTP-metoden för hello HTTP-begäran.

    ![lägga till hello Metoddata](./media/hdinsight-storm-power-bi-topology/uristemandmethod.png)

8. Från hello **visualiseringar** kolumn, Välj hello **fält** ikon och väljer sedan hello nedpilen bredvid för**metoden** i hello **värden**avsnitt. toodisplay antalet hur många gånger en URI har använts, Välj **antal**.

    ![Ändra tooa antal metoder](./media/hdinsight-storm-power-bi-topology/count.png)

9. Välj därefter hello **staplat liggande diagram** toochange hur hello information visas.

    ![Ändra tooa staplade diagram](./media/hdinsight-storm-power-bi-topology/stackedcolumn.png)

10. toosave hello rapport, Välj **spara** och ange ett namn för hello rapporten.

## <a name="stop-hello-topology"></a>Stoppa hello-topologi

hello topologi fortsätter toorun tills du stoppa den eller ta bort hello Storm på HDInsight-kluster. toostop Hej topologi, utföra hello följande steg:

1. I Visual Studio tillbaka toohello topologi viewer och väljer hello-topologi.

2. Välj hello **Kill** knappen toostop hello-topologi.

    ![Avsluta hello topologi sammanfattning-knappen](./media/hdinsight-storm-power-bi-topology/killtopology.png)

## <a name="delete-your-cluster"></a>Ta bort klustret

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Nästa steg

I det här dokumentet du lärt dig hur toosend data från en-databas med Storm-topologi tooSQL visualisera hello data med Power BI. Mer information om hur toowork med andra Azure tekniker med Storm på HDInsight, se hello följande dokument:

* [Exempeltopologier för Storm på HDInsight](hdinsight-storm-example-topology.md)
