---
title: "aaaAzure SDK för .NET 2.5.1 viktig information"
description: "Azure SDK för .NET 2.5.1 viktig information"
services: app-service
documentationcenter: .net,nodejs,java
author: Juliako
manager: erikre
editor: 
ms.assetid: 8d3d815f-bb58-447e-8ff0-f9b9603c7b00
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/10/2016
ms.author: juliako
ms.openlocfilehash: 5ee7688617c966baa409045881c172bbbc55ff63
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sdk-for-net-251-release-notes"></a>Azure SDK för .NET 2.5.1 viktig information
Det här dokumentet innehåller hello viktig information för hello Azure SDK för .NET 2.5.1 versionen. 

## <a name="azure-sdk-for-net-251-release-notes"></a>Azure SDK för .NET 2.5.1 viktig information
hello följande är nya funktioner och uppdateringar i hello Azure SDK för .NET 2.5.1.

* Nya features\scenarios relaterade för**Web verktyg Extensions**. 
  
  * Azure Websites har bytt namn till tooAzure Apptjänst. Mer information finns i [Azure App Service och befintliga Azure-tjänster](../app-service-web/app-service-changes-existing-services.md).
  * Azure API Apps (förhandsgranskning) funktioner så att kunder kan publicera ASP.NET-projekt som API Apps och sedan använda hello Lägg till > Azure API-App klient gest i C#-projekt toogenerate kod baserat på hello strukturen för hello distribuerade API-App. 
  * hello webbplatser nod i Server Explorer är föråldrad i stället för hello Azure App Service-noden, som innehåller stöd för resursen gruppbaserade gruppering av Azure API Apps och Mobilappar Web Apps.
  * Stöd för Azure Mobile Apps (förhandsversion) har lagts till så att kunder kan skapa nya Mobile Apps-projekt, lägga till Mobile Apps domänkontrollanter, publicera hello projekt och felsöka program.
  * Lägg till > Azure API-App klient gest stöder nu lokala Swagger JSON-filer, så att Web API-utvecklare kan använda NuGets från tredje part som Swashbuckle toogenerate Swagger eller redigera den manuellt. På så sätt kan klienten utvecklare kan använda hello kodgenerering funktioner tooconsume valfri Swagger-slutpunkt i C#-projekt. 
  * Webbappen och API-App publishing dialogrutor har förbättrat toosupport hello Azure Portal begreppet resurs gruppering och val/skapa Azure-resursgrupper och Apptjänstplaner representeras i hello nya Webbappen och API-App etablering dialogrutan. 
  * Azure API App Server Explorer noder ger länkar toohello API Apps djup länken i hello Azure-portalen, samt andra funktioner som loggen strömning och fjärrfelsökning.
    
    Kända problem och aktuella begränsningar i Azure SDK .NET 2.5.1 [detta](app-service-release-notes.md#known_issues_2_5_1) nedan.
* Nya features\scenarios relaterade för**HDInsight Tools** i Visual Studio har aktiverats i den här versionen. 
  
  * Lokala validering av hive-skript. Klicka hello Validera skriptet i hello verktygsfältet toosee om det finns några fel i skriptet. 
  * Bättre felsökning av Hive-jobb. Nu kan du felsöka Hive-jobb genom att öppna Yarn-loggar i Visual Studio. Om programmet har prestandaproblem, ger undersöka YARN-loggar användbar information...
  * (Förhandsversion) Nyckelordet automatisk komplettering och IntelliSense stöd för Hive. toohelp som du skapar Hive-skript, HDInsight Tools för Visual Studio till nyckelordet automatisk komplettering och IntelliSense-stöd för Hive.
  * Storm-stöd. Du kan nu använda HDInsight Tools för Visual Studio toodevelop Storm-topologier/kanaler/bultar i C#. Du kan sedan skicka hello utvecklas topologi tooa Storm-kluster och se hello kanal-topologi/bult status. Du kan använda systemloggar och kunden loggar tootroubleshoot ditt Storm-topologier/bultar/kanaler. Du kan också använda befintliga JAVA tillgångar i Storm på HDInsight.
    
    Mer information finns i [komma igång med HDInsight Hadoop-verktyg för Visual Studio](../hdinsight/hdinsight-hadoop-visual-studio-tools-get-started.md).

## <a id="known_issues_2_5_1"></a>Azure SDK för .NET 2.5.1 kända problem och begränsningar
* Azure API Apps är synlig som mål för Mobile Apps distribution. Web Apps ska hello endast mål för Mobile Apps förrän en senare version. 
* Azure API-App-etablering kan resultera i lyckades men ibland misslyckas tooupdate hello förlopp i hello aktivitet för Azure App Service-fönstret. Lösningen är toocheck statusen hello nya Azure API App i hello Azure-portalen. 
* Arkiv > Nytt projekt > API-App > F5 upplevelse resulterar i ett HTTP-fel eftersom det inte finns några default/index.html. Lösningen är toomanually Bläddra toohello/api/värden URL. 
* Server Explorer ikoner visas jämna mellanrum, sammanslagna. Starta om VS löser problemet. 
* Om ett undantag vid Web App eller API-App-etablering (till exempel överskred kvoten fel eller Azure API Apps gateway dubblettnamn) visar hello fel vissa rådata JSON-texten. 
* Skapa återkommande projekt problem när Application Insights kontrolleras vid tidpunkten för skapandet av projektet.
* Ibland kan genereras hello Azure API App klientkod saknas namnområden, de behöver toobe ingår manuellt (eller importeras automatiskt via Visual Studio ledtrådar) för koden toocompile. 
* Mobilapp projekt ska vara publicerade tooweb appar, men du måste välja en plats som du har skapat som en Mobilapp i hello Azure-portalen eftersom Mobilapp projekt kräver en databas. 
* hello startsidan för Mobile Apps använder hello termen ”Mobiltjänst” i stället för ”mobila appar” 
* Mobilapp för att skapa projekt kan ta tooa minut toocreate. 
* Under API-App kommer etablering (i vissa fall) ett fel tillbaka från hello Azure API reflektion att hello behörigheter inte gick att ange korrekt när hello API-App har etablerats korrekt och är redo för användning. Du kan ange behörigheter med hjälp av hello Azure-portalen manuellt.
* Application Insights stöds inte på API-App mallar och mallar för Mobilapp.
* API-App-projekt kan inte användas tillsammans med Cloud Service-projekt.
* API-App projektmallar är bara tillgängliga i C#.
* API-App förbrukning via hello ”Lägg till Azure API App klient” snabbmenyn stöds endast i C#.

