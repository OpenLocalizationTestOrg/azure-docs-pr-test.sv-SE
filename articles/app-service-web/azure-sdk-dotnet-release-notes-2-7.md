---
title: "aaaAzure SDK för .NET 2.7 och .NET 2.7.1 viktig information"
description: "Azure SDK för .NET 2.7 och .NET 2.7.1 viktig information"
services: app-service\web
documentationcenter: .net
author: chrissfanos
editor: 
ms.assetid: 877d070a-9bd5-49b3-8fac-6bb5f65c3554
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 02/24/2017
ms.author: juliako
ms.openlocfilehash: 8ec72b0f18702e6d811f0cbe4790685be7881d96
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sdk-for-net-27-and-net-271-release-notes"></a>Azure SDK för .NET 2.7 och .NET 2.7.1 viktig information
## <a name="overview"></a>Översikt
Det här dokumentet innehåller hello viktig information för hello Azure SDK för .NET 2.7 versionen. 

hello-dokumentet innehåller också hello viktig information för hello Azure SDK för .NET 2.7.1 versionen.

Azure SDK 2.7 stöds bara i Visual Studio 2015 och Visual Studio 2013. [Azure SDK 2.6](https://azure.microsoft.com/downloads/) är hello stöds senast SDK för Visual Studio 2012.

Detaljerad information om den här versionen finns [Azure SDK 2.7 meddelande post](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/) och [Azure SDK 2.7.1 meddelande efter](http://go.microsoft.com/fwlink/?LinkId=623850).

## <a name="azure-sdk-for-net-27"></a>Azure SDK för .NET 2.7
### <a name="sign-in-improvements-for-visual-studio-2015"></a>Logga in förbättringar för Visual Studio 2015
Azure SDK 2.7 för Visual Studio 2015 stöder hello nya identity-hanteringsfunktioner i Visual Studio 2015.  Detta inkluderar stöd för konton som har åtkomst till Azure via rollbaserad åtkomstkontroll, Cloud Solution Providers, DreamSpark och andra typer av konto och prenumeration.

hello inloggning förbättringar som ingår i Azure SDK 2.7 är bara tillgängliga i Visual Studio 2015. Stöd för Visual Studio 2013 ingår i Azure SDK 2.7.1.

### <a name="mobile-sdk"></a>Mobila SDK
Uppdatera **Mobilappar** mallar tooreflect senaste [NuGet-paketet](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) och installationsprocessen.

### <a name="service-bus"></a>Service Bus
Allmän felkorrigeringar och förbättringar. Information om uppdateringarna och funktionerna finns toohello viktig information för hello senaste [Service Bus-NuGet](http://www.nuget.org/packages/WindowsAzure.ServiceBus/).

### <a name="hdinsight-tools"></a>HDInsight-verktyg
Följande uppdateringar har gjorts i den här versionen hello. De här uppdateringarna finns i förhandsgranskningen. Mer information finns i [bloggen](http://go.microsoft.com/fwlink/?LinkId=619108).

* Hive diagram för Hive jobb på Tez
* Fullständig Hive DML IntelliSense-stöd
* Stöd för Pig-mall
* Storm-mallar för Azure-tjänster

#### <a name="breaking-changes"></a>Gör ändringar
* Gamla **Storm** projektet måste uppgraderas när du använder den här versionen av hello verktyg. Mer information finns i [bloggen](http://go.microsoft.com/fwlink/?LinkId=619108).
* Visual Studio Web Express stöds inte längre. Mer information finns i [bloggen](http://go.microsoft.com/fwlink/?LinkId=619108).

### <a name="azure-app-service-tools"></a>Azure Apptjänst-verktyg
I den här versionen hello har följande uppdateringar gjorts tooWeb verktyg tillägg. Mer information finns i [detta](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/) blogg. 

* Stöd för DreamSpark-konton som lagts till
* Fullständig ändra tooAzure verktyg som gjorts toosupport hello nya Azure Resource Manager API: er
* Stöd för Azure App Service lagts till för[Cloud Explorer](#cloud_explorer)

#### <a name="known-issues"></a>Kända problem
Web App distribution fack noder visas inte under hello fack noden i Server Explorer och Web App distribution fack underordnade noder laddas inte under Cloud Explorer. Det här problemet har lösts och förberedda för hello nästa SDK-version. 

### <a name="cloud_explorer"></a>Cloud Explorer för Visual Studio 2015
Azure SDK 2.7 omfattar Cloud Explorer för Visual Studio 2015, vilket gör att du tooview dina Azure-resurser, granska deras egenskaper och utföra viktiga developer åtgärder från Visual Studio. 

Cloud explorer stöder hello följande:

* Resursgruppen och resurstypen vyer för dina Azure-resurser 
* Söka efter resurser efter namn (tillgängligt i resurstyp vy)
* Stöd för prenumerationer och resurser som har rollen åtkomstkontroll (RBAC) används 
* Integrerad åtgärdspanel som visar specifika tooselected resurser för utvecklare åtgärder. Till exempel: Koppla remote felsökare för virtuella datorer som skapats med hjälp av hello Resource Manager-stacken, visa diagnostik-data för virtuella datorer osv.
* Integrerad egenskapsvyn som visar utvecklare egenskaper som ofta behövs under utveckling och testning 
* Snabb växling av hello konto toouse vid uppräkning av resurser (kommandot inställningar på verktygsfältet) 
* Filtrering av prenumerationer toouse vid uppräkning av resurser (kommandot inställningar på verktygsfältet) 
* Djuplänkar toohello Azure-portalen för hantering av resurser och resursgrupper 

### <a name="azure-resource-manager-tools"></a>Azure Resource Manager-verktyg
hello Azure Resource Manager-verktyg har uppdaterats toowork med rollbaserad åtkomstkontroll (RBAC) och den nya prenumerationstyper.  Ingår i dessa ändringar är hello möjlighet toouse nya lagringskonton, dessutom tooclassic lagring, toostore artefakter under distributionen.  

Om du använder ett Azure-resursgrupp projekt från en tidigare version av hello SDK med hello SDK 2.7 är en ny distribution med skriptet nödvändiga toodeploy med ett nytt lagringskonto i stället för klassiska lagring.  Du uppmanas innan ändringar tooyour projekt tooadd hello nytt skript.  kommer att byta namn på hello gamla skript och du måste toomanually gör eventuella ändringar i toohello nytt skript.

### <a name="storage-explorer-tools"></a>Verktyg för lagring Explorer
* Stöd för att visa Tilläggsblobbar. Mer information i [i det här blogginlägget](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/04/13/introducing-azure-storage-append-blob.aspx). 
* Stöd för att visa Premium Storage-konton via Server Explorer. Server Explorer visas endast sidblobbar för premium storage-konton som de är hello stöds endast typ för premium-lagringskonton.

### <a name="azure-data-factory-tools-for-visual-studio"></a>Azure Data Factory-verktyg för Visual Studio
Introduktion till **Azure Data Factory-verktyg** för Visual Studio. Nedan finns hello aktiverat funktioner. Se [bloggen](http://go.microsoft.com/fwlink/?LinkId=617530) för mer information.

* **Mallen baseras redigering**: Välj alltid i användning baserat mallar, mallar för flytt av data eller databearbetning mallar toodeploy en lösning för integrering av data för slutpunkt till slutpunkt och kom igång praktiska snabbt med Data Factory. 
* **Integrering med Solution Explorer för redigering och distribuera Data Factory entiteter**: skapa och distribuera pipelines och relaterade entiteter som Visual Studio-projekt. 
* **Integrering med diagramvyn för visual interaktion när du redigerar**: redigera visuellt pipelines och datauppsättningar med stöd från hello diagramvyn. 
* **Integrering med Server explorer för bläddring och interaktion med redan distribuerade entiteter**: utnyttjar hello Server Explorer toobrowse redan har distribuerats datafabriker och motsvarande enheter. Importera en distribuerad datafabrik eller entiteter (Pipeline, länkade tjänsten datauppsättningar) i projektet. 
* **JSON redigering med schemat validering och omfattande intellisense**: konfigurera och redigera JSON-dokument av Data Factory entiteter med omfattande intellisense och schema-validering 
* **Miljöer publicering**: publicera skapade pipelines toodev, test eller Prod miljö genom att skapa separata konfigurationsfiler för varje miljö.
* **Pig, Hive och .net-baserat stöd för databearbetning**: stöd för Pig och Hive-skript i Data Factory-projekt. Stöd för refererar till C#-projekt för att hantera .net aktivitet.

## <a name="azure-sdk-for-net-271"></a>Azure SDK för .NET 2.7.1
hello följande avsnitt innehåller uppdateringar som introducerades med hello Azure SDK för .NET 2.7.1 versionen.

### <a name="hdinsight-tools"></a>HDInsight-verktyg
Mer detaljerad förklaring av uppdateringar för HDInsight finns [bloggen](http://go.microsoft.com/fwlink/?LinkId=623831).

* Hive-jobbet Operator-vyn (en ny funktion)
  
    toohelp som du känner till ditt Hive-frågan bättre, hello Hive Operator-vyn funktionen har lagts till. toosee alla hello operatorer innanför en brytpunkt Dubbelklicka på formhörnen hello hello jobbdiagram. tooview mer information om en viss operator hovra över den operatorn.
* Hive fel markör (en ny funktion)
  
    tooenable du tooview hello grammatikfel direkt hello Hive fel markör funktionen har lagts till. Dessutom felmeddelanden har förbättrats och du kan nu se detaljerad grammatikfel omedelbart (fram den här versionen, var du tvungen toosubmit en Hive-skript toohello klustret och vänta en stund innan du hämtar information om ditt felmeddelande).  
* Storm-topologi diagram (en ny funktion)
  
    Det är mycket viktigt att visualisera när du vill toosee om din topologi fungerar som förväntat. Vi lagt till visualiseringen för Storm diagram i den här versionen. Du kan visualisera hello viktiga mått för topologin (till exempel en färg anger väder en vissa bult är ”upptagen” eller inte). Du kan också dubbelklicka på hello bult/kanal tooview mer information.
* Stöd för HDInsight-kluster som har skapats i hello Azure-portalen (buggfix)
  
    Du kan nu använda Visual Studio tooview och skicka jobb tooall ditt HDInsight-kluster oavsett där hello klustret har skapats.
* Fler IntelliSense-stöd & snabbare Hive-Metadata lästes in (en förbättring)
  
    Vi har förbättrat hello IntelliSense genom att lägga till mer användarvänlig förslag. Till exempel kan tabellalias nu också att föreslås i IntelliSense så att du lättare kan skriva en fråga. Vi har också förbättrats hello Hive-metadata läser in så att det tar bara några sekunder toolist hello databaser, tabeller och kolumner i dina Hive metastore.

Mer detaljerad förklaring av uppdateringar för HDInsight finns [bloggen](http://go.microsoft.com/fwlink/?LinkId=623831).

### <a name="improvements-in-visual-studio-2013"></a>Förbättringar i Visual Studio 2013
* Azure SDK 2.7.1 låter Visual Studio 2013 tooaccess Azure-konton och -prenumerationer via rollbaserad åtkomstkontroll, Cloud Solution Providers och Dreamspark.
* Med Azure SDK 2.7.1 är hello nytt Cloud Explorer verktygsfönster nu också tillgänglig i Visual Studio 2013.

### <a name="known-issues"></a>Kända problem
Installera hello Azure SDK 2.6 eller 2.7.1 för Visual Studio Community 2013 på en icke - engelska OS visas en varning om att hello engelska och icke-engelska resurser av Visual Studio är eventuellt felaktigt matchade. Den här varningen att avvisas på ett säkert sätt. Det görs bara om hello datorn innehåller inte en tidigare installation av Visual Studio Community 2013 och du installerar hello SDK på en icke - engelska OS. hello varning visas när hello språkpaket gäller hello RTM resurser tooVisual Studio, men innan det gäller uppdatering 4. Ignorera hello varning kan hello language pack toocontinue och fullständig använder hello uppdatering 4-versionen av hello språkpaket innehåll.

LightSwitch-projekt är inte kompatibelt med den här versionen. Det här problemet löses med hello nästa SDK-versionen.

## <a name="also-see"></a>Se även
[Azure SDK 2.7.1 meddelande post](http://go.microsoft.com/fwlink/?LinkId=623850)

[Azure SDK 2.7 meddelande post](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/)

[Support och tillbakadragning Information för hello Azure SDK för .NET och API: er](https://msdn.microsoft.com/library/azure/dn479282.aspx/)

