---
title: "aaaGet igång med Azure Search i Java | Microsoft Docs"
description: "Hur toobuild ett värdbaserat moln Sök program på Azure med hjälp av Java som du programmeringsspråk."
services: search
documentationcenter: 
author: EvanBoyle
manager: pablocas
editor: v-lincan
ms.assetid: 8b4df3c9-3ae5-4e3a-b4bb-74b516a91c8e
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.date: 07/14/2016
ms.author: evboyle
ms.openlocfilehash: 5476a2103f3b60fe6ec78ff3d3fdba9fcff55c37
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-search-in-java"></a>Komma igång med Azure Search i Java
> [!div class="op_single_selector"]
> * [Portal](search-get-started-portal.md)
> * [.NET](search-howto-dotnet-sdk.md)
> 
> 

Lär dig hur toobuild en anpassad Java söka program som använder Azure Search för dess sökinställningar. Den här kursen använder hello [Azure Söktjänsts-REST API](https://msdn.microsoft.com/library/dn798935.aspx) tooconstruct hello objekt och åtgärder som används i den här övningen.

toorun det här exemplet måste du ha en Azure Search-tjänst som du kan registrera dig för i hello [Azure Portal](https://portal.azure.com). Se [skapa en Azure Search-tjänst i hello portal](search-create-service-portal.md) stegvisa instruktioner.

Vi används hello följande programvara toobuild och testa det här exemplet:

* [Eclipse IDE för Java EE-utvecklare](https://eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/lunar). Vara att toodownload hello EE version. En av hello verifieringssteg kräver en funktion som bara finns i den här versionen.
* [JDK 8u40](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Apache Tomcat 8.0](http://tomcat.apache.org/download-80.cgi)

## <a name="about-hello-data"></a>Om hello data
Det här exempelprogrammet använder data från hello [USA geologiska tjänster (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), filtrerad på hello tillstånd Rhode dataö tooreduce hello dataset storlek. Vi använder den här data toobuild ett sökprogram som returnerar Landmärke byggnader, till exempel sjukhus och skolorna samt geologiska funktioner som dataströmmar, sjöar och toppmöten.

I det här programmet hello **SearchServlet.java** programmet skapar och belastningar hello index med hjälp av en [indexeraren](https://msdn.microsoft.com/library/azure/dn798918.aspx) konstruktion, hämtar hello filtrerade USGS datamängd från en offentlig Azure SQL-databas. Fördefinierade autentiseringsuppgifter och anslutningen information toohello online-datakälla finns i hello programkod. Ingen ytterligare konfiguration krävs vad gäller dataåtkomsten.

> [!NOTE]
> Vi har använt ett filter på den här datauppsättningen toostay under hello 10 000 dokumentet gränsen på hello kostnadsfria prisnivån. Den här begränsningen gäller inte om du använder hello standardnivån och du kan ändra den här koden toouse en större datamängd. Mer information om kapaciteten för varje prisnivå finns i [Gränser och begränsningar](search-limits-quotas-capacity.md).
> 
> 

## <a name="about-hello-program-files"></a>Om hello programfiler
hello beskrivs följande lista hello-filer som är relevanta toothis exempel.

* Search.JSP: Innehåller hello användargränssnitt
* SearchServlet.java: Tillhandahåller metoder (liknande tooa styrenhet i MVC)
* SearchServiceClient.java: Hanterar HTTP-begäranden
* SearchServiceHelper.java: En hjälparklass som tillhandahåller statiska metoder
* Document.Java: Ger hello-datamodell
* Config.Properties: Anger hello Sök URL: en och api-nyckel
* Pom.XML: Ett Maven-beroende

<a id="sub-2"></a>

## <a name="find-hello-service-name-and-api-key-of-your-azure-search-service"></a>Hitta hello tjänstnamn och api-nyckel för Azure Search-tjänst
REST API-anrop i Azure Search kräva att du anger hello URL: en och en api-nyckel. 

1. Logga in toohello [Azure Portal](https://portal.azure.com).
2. Klicka på hello hopp-fältet **söktjänsten** toolist alla hello Azure Search-tjänster har etablerats för din prenumeration.
3. Välj hello-tjänster som du vill toouse.
4. På instrumentpanelen för hello-tjänsten ser du paneler för viktig information samt hello nyckelikonen för att komma åt hello admin nycklar.
   
      ![][3]
5. Kopiera hello URL: en och en administrationsnyckel. Du behöver dem senare, när du lägger till dem toohello **config.properties** fil.

## <a name="download-hello-sample-files"></a>Hämta hello exempelfilerna
1. Gå för[AzureSearchJavaDemo](https://github.com/AzureSearch/AzureSearchJavaIndexerDemo) på GitHub.
2. Klicka på **hämta ZIP**, spara hello ZIP-filen toodisk och sedan extrahera alla hello filer den innehåller. Överväg att extrahera hello-filer i dina Java arbetsytan toomake som enklare toofind hello projektet senare.
3. hello exempelfiler är skrivskyddade. Högerklicka på mappegenskaper och avmarkerar hello skrivskyddsattributet.

Alla efterföljande filändringar och körningsinstruktioner görs mot filer i den här mappen.  

## <a name="import-project"></a>Importera projekt
1. I Eclipse väljer du **File** > **Import** > **General** > **Existing Projects into Workspace**.
   
    ![][4]
2. I **väljer rotkatalogen**, bläddra toohello mapp som innehåller exempel på filer. Välj hello-mappen som innehåller hello .project mappen. hello projektet ska visas i hello **projekt** listan som det markerade objektet.
   
    ![][12]
3. Klicka på **Slutför**.
4. Använd **Projektutforskaren** tooview och redigera hello-filer. Om den inte redan är öppen klickar du på **fönstret** > **visa** > **Projektutforskaren** eller använda hello genvägen tooopen den.

## <a name="configure-hello-service-url-and-api-key"></a>Konfigurera hello URL: en och api-nyckel
1. I **Projektutforskaren**, dubbelklicka på **config.properties** tooedit hello konfigurationsinställningar som innehåller hello servernamn och api-nyckel.
2. Se toohello anvisningarna tidigare i den här artikeln, där du hittade hello URL: en och api-nyckel i hello [Azure Portal](https://portal.azure.com), tooget hello värden som du kommer nu att träda i **config.properties**.
3. I **config.properties**, Ersätt ”Api-nyckeln” med hello api-nyckel för din tjänst. Därefter tjänstnamn (hello första delen av hello URL http://servicename.search.windows.net) ersätter ”Tjänstenamn” i hello samma fil.
   
    ![][5]

## <a name="configure-hello-project-build-and-runtime-environments"></a>Konfigurera hello projekt, bygga och runtime-miljöer
1. Högerklicka på hello-projektet i Eclipse i Projektutforskaren > **egenskaper** > **Projektfasetter**.
2. Välj **Dynamic Web Module**, **Java** och **JavaScript**.
   
    ![][6]
3. Klicka på **Apply**.
4. Välj **Window** > **Preferences** > **Server** > **Runtime Environments** > **Add**.
5. Visa Apache och välj hello version av hello Apache Tomcat-server som du tidigare har installerat. I vårt system installerade vi version 8.
   
    ![][7]
6. Ange hello Tomcat-installationskatalogen på hello nästa sida. På en Windows-dator är detta antagligen C:\Program\Apache Software Foundation\Tomcat *version*.
7. Klicka på **Finish**.
8. Välj **Window** > **Preferences** > **Java** > **Installed JREs** > **Add**.
9. Välj **Standard VM**i **Add JRE**.
10. Klicka på **Nästa**.
11. Klicka på **Directory** i JRE Definition på JRE-startsidan.
12. Navigera för**programfiler** > **Java** och välj hello JDK som du tidigare har installerat. Det är viktigt tooselect hello JDK som hello JRE.
13. Välj hello i installerat JREs **JDK**. Inställningarna bör se ut ungefär toohello följande skärmbild.
    
    ![][9]
14. Alternativt, Välj **fönstret** > **webbläsare** > **Internet Explorer** tooopen hello programmet i en extern webbläsarfönster. En extern webbläsare ger dig en bättre webbupplevelse.
    
    ![][8]

Du har nu slutfört hello konfigurationsåtgärder. Därefter du skapa och köra hello-projektet.

## <a name="build-hello-project"></a>Skapa hello-projekt
1. Högerklicka på hello projektnamn i Projektutforskaren, och välj **kör som** > **Maven build...**  tooconfigure hello projektet.
   
    ![][10]
2. I Goals i Edit Configuration skriver du ”clean install” och klickar på **Run**.

Statusmeddelanden är utdata toohello konsolfönstret. Du bör se Skapa lyckade som anger hello-projektet skapats utan fel.

## <a name="run-hello-app"></a>Kör hello-appen
I det sista steget körs hello program i en lokal server-körningsmiljön.

Om du inte har angett en server körningsmiljö ännu i Eclipse, behöver du toodo som först.

1. Expandera **WebContent** i Project Explorer.
2. Högerklicka på **Search.jsp** > **Run As** > **Run on Server**. Välj hello Apache Tomcat server och klicka sedan på **kör**.

> [!TIP]
> Om du använder en icke-förvalt arbetsytan toostore projektet, behöver du toomodify **kör konfiguration** toopoint toohello projekt plats tooavoid ett serverfel uppstart. I Project Explorer högerklickar du på **Search.jsp** > **Run As** > **Run Configurations**. Välj hello Apache Tomcat-server. Klicka på **Arguments**. Klicka på **arbetsytan** eller **filsystemet** tooset hello mapp som innehåller hello-projekt.
> 
> 

När du kör programmet hello bör du se ett webbläsarfönster sökrutan för att ange villkoren.

Vänta ungefär en minut innan du klickar på **Sök** toogive hello service tid toocreate och Läs in hello index. Om du får en HTTP 404-fel, måste du bara toowait lite längre innan du försöker igen.

## <a name="search-on-usgs-data"></a>Söka i USGS-data
Hej USGS datauppsättningen innehåller poster som är relevanta toohello tillstånd Rhode dataö. Om du klickar på **Sök** för en tom sökrutan visas hello upp 50 poster som är hello standard.

Ange en sökterm ger hello sökmotor något toogo på. Prova att skriva namnet på någon från regionen. ”Roger Williams” var hello första resursstyrningen av Rhode dataö. Många parker, byggnader och skolor bär hans namn.

![][11]

Du kan också prova någon av dessa söktermer:

* Pawtucket
* Pembroke
* goose +cape

## <a name="next-steps"></a>Nästa steg
Detta är hello första Azure Search-självstudierna baserat på Java och hello USGS dataset. Över tiden, kommer vi utöka den här självstudiekursen toodemonstrate ytterligare sökfunktioner som du kanske vill toouse i din anpassade lösningar.

Om du redan har vissa bakgrunden i Azure Search kan du kan använda det här exemplet som en springboard för ytterligare undersökningar kanske höja hello [söksidan](search-pagination-page-layout.md), eller implementera [fasetterad navigering](search-faceted-navigation.md). Du kan också förbättra vid hello sökresultatsidan genom att lägga till antal och batchbearbetning dokument så att användare kan bläddra igenom hello resultat.

Nya tooAzure sökningen? Vi rekommenderar att du försöker andra självstudiekurser toodevelop förstå vad du kan skapa. Besök vår [dokumentationssidan](https://azure.microsoft.com/documentation/services/search/) toofind mer resurser. Du kan också visa hello länkar i vår [Video-och kursen](search-video-demo-tutorial-list.md) tooaccess mer information.

<!--Image references-->
[1]: ./media/search-get-started-java/create-search-portal-1.PNG
[2]: ./media/search-get-started-java/create-search-portal-21.PNG
[3]: ./media/search-get-started-java/create-search-portal-31.PNG
[4]: ./media/search-get-started-java/AzSearch-Java-Import1.PNG
[5]: ./media/search-get-started-java/AzSearch-Java-config1.PNG
[6]: ./media/search-get-started-java/AzSearch-Java-ProjectFacets1.PNG
[7]: ./media/search-get-started-java/AzSearch-Java-runtime1.PNG
[8]: ./media/search-get-started-java/AzSearch-Java-Browser1.PNG
[9]: ./media/search-get-started-java/AzSearch-Java-JREPref1.PNG
[10]: ./media/search-get-started-java/AzSearch-Java-BuildProject1.PNG
[11]: ./media/search-get-started-java/rogerwilliamsschool1.PNG
[12]: ./media/search-get-started-java/AzSearch-Java-SelectProject.png
