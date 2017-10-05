---
title: 'MyDriving Azure IoT-exempel: skapa den | Microsoft Docs'
description: "Skapa en app som är en omfattande demonstration av hur du kan skapa en IoT-system med hjälp av Microsoft Azure, inklusive Stream Analytics, Machine Learning och Händelsehubbar."
services: 
documentationcenter: .net
suite: 
author: harikmenon
manager: douge
ms.assetid: c2fcd6ee-3bbe-43d1-a066-dce52cc3a53d
ms.service: multiple
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: dotnet
ms.topic: article
ms.date: 06/30/2017
ms.author: harikm
ms.openlocfilehash: c4b19cc76ca11f606ca8af6b0f3277b5aa46ac5a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="build-and-deploy-the-mydriving-solution-to-your-environment"></a>Skapa och distribuera MyDriving lösningen till din miljö
MyDriving är en Sakernas Internet (IoT) som samlar in data från en bil, bearbetar den med hjälp av machine learning och visas på din mobiltelefon. Serverdelen består av en mängd olika tjänster som tillhandahålls av Microsoft Azure. Klienterna kan vara Android, iOS eller Windows 10-telefoner.

Vi har skapat MyDriving lösningen för att ge dig en rivstart att skapa en egen IoT-system. Från den [MyDriving databasen på GitHub](https://github.com/Azure-Samples/MyDriving), du kan hämta Azure Resource Manager-skript för att distribuera backend-arkitekturen i din egen Azure-konto. Du kan konfigurera olika tjänster, ändra frågor så att den passar dina egna data och så vidare från den tidpunkten. Du hittar dessa skript--tillsammans med koden för mobilappen, Azure App Service API-projekt med mera – MyDriving på lagringsplatsen.

Om du inte har testat appen ännu, granskar du den [Get igång](iot-solution-get-started.md).

Det finns ett detaljerat konto för arkitekturen i den [MyDriving referenshandboken](http://aka.ms/mydrivingdocs). Sammanfattningsvis: det finns flera uppgifter som vi för att skapa ett liknande projekt:

* En **klientappen** körs på Android, iOS och Windows 10-telefoner. Vi använder Xamarin-plattformen för att dela en stor del av koden, som är lagrad på GitHub under `src/MobileApp`. Appen utför två olika funktioner:
  * Den vidarebefordrar telemetri från enheten inbyggd diagnostik (OBD) och från sin egen platstjänsten till systemets molnet serverdel.
  * Det är ett användargränssnitt där användare kan fråga om deras inspelade körda mil.
* En **Molntjänsten** en väg resa data i realtid och bearbetar dessa. Det huvudsakliga arbetet med att skapa den här tjänsten är att välja parameterstyra och ansluta dig en mängd olika Azure-tjänster. Några av komponenterna kräver skript för att filtrera och processen inkommande data. Vi använder en Azure Resource Manager-mall för att konfigurera alla delar.
* En **Mobiltjänst app** är webbtjänst bakom användaren gränssnittet en del av enheten appen. Dess huvudsakliga uppgift är att söka i databasen lagras, bearbetade data. Kod som finns på GitHub under `src/MobileAppService`.
* **Visual Studio med Xamarin** är vår utvecklingsmiljö. Xamarin som finns både som en komponent i Visual Studio och som en fristående integrerad utvecklingsmiljö (IDE) används för att skapa koden plattformsoberoende enhet. För att skapa iOS-koden är det nödvändigt att ha en instans av Xamarin som körs på en OS X-dator. Om det behövs kan du köra den som en agent hanteras från Visual Studio.
* **Testa enheten** enhetens appar utförs i Xamarin Test molnet.
* **GitHub** är databasen där vi lagrar alla kod, skript och mallar.
* **Visual Studio Team Services** är en molnbaserad tjänst som används för att hantera kontinuerlig bygg- och test av webbprogram för tjänster och enheter.
* **HockeyApp** används för att distribuera versioner av kod för enheten. Den samlar även in krascher och användningsrapporter och användarfeedback.
* **Visual Studio Application Insights** övervakar mobila webbtjänsten.

Så Låt oss se hur vi ställer in alla som. 

> [!NOTE] 
> Många av de följande steg är valfria.
>
>

## <a name="sign-up-for-accounts"></a>Registrera dig för konton
* [Visual Studio Dev Essentials](https://www.visualstudio.com/products/visual-studio-dev-essentials-vs.aspx). Den här kostnadsfria programmet ger enkel åtkomst till många utvecklingsverktyg och tjänster, inklusive Visual Studio, Visual Studio Team Services och Azure. Den ger dig en 25 USD per månad kredit på Azure i tolv månader. Den omfattar också prenumerationer Pluralsight utbildning och Xamarin University. Du kan också registrera dig separat för kostnadsfria nivåer av [Azure](https://azure.com) och [Visual Studio Team Services](https://www.visualstudio.com/products/visual-studio-team-services-vs.aspx), men de ger inte Azure-krediter.
* [HockeyApp](https://rink.hockeyapp.net/) (valfritt), för att hantera test distribution av mobila appar och samla in telemetri.
* [Xamarin](https://xamarin.com/) (obligatoriskt), för att skapa mobilappen och kör debug körs och tester på [Xamarin Test molnet](https://xamarin.com/test-cloud).
* [GitHub](https://github.com/Azure-Samples/MyDriving/) (valfritt) skapa ledigt offentliga databaser för egen kod (privat databaser är betalda). Du kan också använda den grundläggande planen i Visual Studio Team Services för privata databaser.
* [Power BI](https://powerbi.microsoft.com/) (valfritt) skapa omfattande visualiseringar av data i hela systemet.

> [!NOTE]
> Du behöver inte en GitHub-konto för åtkomst till MyDriving koden i [GitHub MyDriving databasen](https://github.com/Azure-Samples/MyDriving).
> 
> 

## <a name="install-development-tools"></a>Installera utvecklingsverktygen
Följande inställningar är för utveckling av den fullständiga lösningen: iOS-, Android- och Windows 10 Mobile plattformsoberoende app med en Azure-serverdel.

Alternativt kan du använda Xamarin Studio i Mac eller Windows för att utveckla mobila appar om du inte arbetar på Azure-serverdel.

Det finns en [längre beskrivning av den här installationen](https://msdn.microsoft.com/library/mt613162.aspx).

### <a name="windows-development-machine"></a>Windows-utvecklingsdator
Verktyget central i Windows är Visual Studio för att arbeta med MyDriving-app för Android och Windows, App Service API-projekt och mikrotjänster tillägg.

Xamarin, Git, emulatorerna och andra användbara komponenter integreras med Visual Studio.

Installera:

* [Visual Studio med Xamarin](https://www.visualstudio.com/products/visual-studio-community-vs) (en utgåva--Community är ledigt).
* [SQLite för universella Windows-plattformen](https://visualstudiogallery.msdn.microsoft.com/4913e7d5-96c9-4dde-a1a1-69820d615936). Krävs för att skapa Windows 10 Mobile-koden.
* [Azure SDK för Visual Studio](https://www.visualstudio.com/vs/azure-tools/). Ger SDK för program som körs i Azure, tillsammans med verktyg för att hantera Azure.
* [Azure Service Fabric SDK](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric). Krävs för att skapa den [mikrotjänster](../service-fabric/service-fabric-get-started.md) tillägg.

Var noga med att du har rätt Visual Studio-tillägg. Kontrollera att under **verktyg**, visas **Android, iOS, Xamarin...** . Om inte, öppna Visual Studio, söka efter Xamarin och följ anvisningarna för att installera den. Kontrollera också att **Git för Windows** är installerad. Om inte, söka efter den i Visual Studio och följ anvisningarna för att installera den. 

### <a name="mac-development-machine"></a>Mac-utvecklingsdator
Mac (Yosemite eller senare) krävs om du vill utveckla för iOS. Även om vi använder Visual Studio med Xamarin i Windows för att utveckla och hantera all kod använder Xamarin en agent installeras på en Mac för att skapa och registrera iOS-koden.

![Utveckla på Windows och bygger på Mac](./media/iot-solution-build-system/image1.png)

(Som ett alternativ kan du kan använda Xamarin Studio direkt på Mac utveckla plattformsoberoende appar.)

Du behöver inte Mac om du inte vill att inkludera iOS som en målplattform.

Installera:

* [Xamarin Studio för iOS](https://developer.xamarin.com/guides/ios/getting_started/installation/mac/). Du kan också ställa in Visual Studio och Xamarin på en Mac som kör en virtuell Windows-dator. Se [konfiguration, installation och verifieringar för Mac-användare](https://msdn.microsoft.com/library/mt488770.aspx) på MSDN.
* [Azure utvecklingsverktyg](https://azure.microsoft.com/downloads/) (valfritt).

Aktivera fjärrinloggning på Mac. Öppna **systeminställningar** > **delning**, och välj sedan **fjärrinloggning**.

När du öppnar en iOS-projekt i Visual Studio i Windows Xamarin plugin-programmet kommer att fråga efter ID för Mac.

## <a name="fetch-the-github-repository"></a>Hämta GitHub-lagringsplatsen
Hämta en lokal kopia av [GitHub MyDriving databasen](https://github.com/Azure-Samples/MyDriving) med hjälp av den **hämta ZIP** knappen i GitHub, Visual Studio eller en annan Git-klient.

Packa upp filen till en mapp med en kort sökväg, till exempel C:\\kod.

Alternativt, om du vill hålla dig uppdaterad med eller bidrar till vår kod klona databasen på följande sätt:

**Git-klon https://github.com/Azure-Samples/MyDriving.git**

## <a name="get-a-bing-maps-api-key"></a>Hämta en Bing maps API-nyckel
[Registrera dig för en Bing Maps API-nyckel](https://msdn.microsoft.com/library/ff428642.aspx).

Du måste ersätta detta i rad 22 i `src/MobileApps/MyDriving/MyDriving.Utils/Logger.cs`.

## <a name="build-the-demo-app"></a>Skapa sedan demo-appen
Öppna dessa lösningar i Visual Studio:

* src\MobileApps\MyDriving.SLN
* src\MobileAppService\MyDrivingService.SLN
* src\Extensions\ServiceFabric\VINLookUpApplication\VINLookUpApplication.SLN

Får du anvisningarna för att:

* Förtroende för vissa potentiellt opålitliga projekt. Välja att öppna dem. Om du vill gå vidare.
* Ange utvecklarläget om du arbetar med en ny Windows 10-dator.
* Ange dina autentiseringsuppgifter för Xamarin.
* Ansluta till Xamarin Mac. Om du inte har en Mac, högerklicka på iOS-projekt i Visual Studio och välj sedan **ta bort projektet**.

Bygg lösningen.

Om du har problem med att skapa försök lösningar på egenskaper som vi har hittat:

* *VINLookupApplication projekt inte läsa in*: Kontrollera att du har installerat den [Azure SDK för Visual Studio](https://www.visualstudio.com/vs/azure-tools/).
* *Service Fabric-projektet inte skapa*: skapa gränssnittet projekt först och kontrollera att du installerat Service Fabric-SDK.
* *Android-app inte skapa*:
  
  * Öppna **verktyg** > **Android** > **Android SDK Manager**, och kontrollera att Android 6 (API-23) / SDK-plattformen är installerat.
  * Ta bort den här katalogen och bygg sedan:<br/>
    `%LocalAppData%\Xamarin\zips`

## <a name="get-to-know-the-code"></a>Bekanta dig med koden
I lösningen hittar du:

* Azure-tillägg: Service Fabric.
* Azure HDInsight: Skript för att bearbeta resa data i Azure.
* Mobile Apps: Enheten apparna.
* MobileAppsService/MyDrivingService: Web tillbaka avslutas.
* Powerbi: Rapportdefinitionen.
* Skript:
  
  * Resource Manager: mallar för att skapa Azure-resurser.
  * PowerShell: Skript körs Resource Manager-mallar.
  * Azure SQL Database: Felsökning databaser.
* SQL Database: CreateTables: schemadefinitioner.
* Azure Stream Analytics: Frågor som transformera inkommande dataström.

## <a name="run-the-apps-in-development-mode"></a>Kör apparna i utvecklingsläge
Vidta åtgärder för att köra appar, baserat på den enhet som du använder:

* Serverdelen: Ange MyDrivingService som Startprojekt och tryck på F5 för att köra backend webbtjänst. Det att öppna en Webbläsarvy av API-lista.
* Mobila klienter: den [mobila appar utvecklas i Xamarin](https://developer.xamarin.com/guides/cross-platform/deployment,_testing,_and_metrics/debugging_with_xamarin/).
  
  * Android: Mer information finns i [felsökning Android i Xamarin](http://developer.xamarin.com/guides/android/deployment,_testing,_and_metrics/debugging_with_xamarin_android/).
  * iOS: Mer information finns i [felsökning i iOS-](http://developer.xamarin.com/guides/ios/deployment,_testing,_and_metrics/debugging_in_xamarin_ios/).
  * Windows Phone: Mer information finns i [Xamarin + Windows Phone](https://developer.xamarin.com/guides/cross-platform/windows/phone/).

## <a name="upload-the-mobile-app-to-hockeyapp"></a>Överföra mobila appen till HockeyApp
HockeyApp hanterar distributionen av appen Android, iOS eller Windows för att testa användare, meddela användaren om nya versioner. Den samlar även in användbar kraschrapporter, Användarfeedback med skärmbilder och användningsstatistik.

[Starta genom att överföra](http://support.hockeyapp.net/kb/app-management-2/how-to-create-a-new-app) build-app. Logga sedan in på [HockeyApp](https://rink.hockeyapp.net) från utvecklingsdatorn. Klicka på instrumentpanelen för utvecklare **ny App**, och sedan dra inbyggd filer till fönstret. (Senare, kan du automatisera build-tjänsten för att göra detta.)

Du är nu i din app instrumentpanel.

![Översikt översiktsfliken på appinstrumentpanelen för](./media/iot-solution-build-system/image2.png)

Upprepa proceduren för varje plattform som appen körs på. Sedan kan du göra följande:

* Använd den [app-ID](http://support.hockeyapp.net/kb/app-management-2/how-to-find-the-app-id) från instrumentpanelen för att skicka kraschdata och feedback från din app. Uppdatera ID: N i src/MobileApps/MyDriving/MyDriving.Utils/Logger.cs i MyDriving.
* [Bjud in testanvändare](http://support.hockeyapp.net/kb/app-management-2/how-to-invite-beta-testers). Du får en URL till rekrytera Testare användare. De kommer att kunna registrera dig för ditt team, ladda ned appen och skicka feedback.
* Om du föredrar en mer öppna betaversion inställt distributionen på public. Klicka på **hantera appen** > **Distribution** > **hämta = offentlig**. Nu kan vem som helst hämta din app och skicka feedback och de ser ett meddelande när du publicerar en ny version. Du kan få vissa kraschrapporter från dem för.
  
   ![Team på instrumentpanelen](./media/iot-solution-build-system/image3.png)
* [Länka kraschrapporter till Visual Studio Team Services](http://support.hockeyapp.net/kb/third-party-bug-trackers-services-and-webhooks/how-to-use-hockeyapp-with-visual-studio-team-services-vsts-or-team-foundation-server-tfs). Klicka på **hantera appen** > **Visual Studio Team Services**. HockeyApp kan skapa arbetsobjekt i Team Services automatiskt när det finns kraschrapporter eller när feedback tas emot.

Läs mer på den [HockeyApp plats](https://hockeyapp.net).

## <a name="test-the-mobile-app-on-xamarin-test-cloud"></a>Testa mobilappen på Xamarin Test moln
[Xamarin Test molnet](https://developer.xamarin.com/guides/testcloud/introduction-to-test-cloud/) automatiserar UI testning på verkliga enheter i molnet. Med hjälp av ramverket NUnit kan skriva du tester som kör appen via användargränssnittet.

Om du vill använda Xamarin måste du lägga till den [Xamarin.UITests](https://developer.xamarin.com/guides/testcloud/uitest/intro-to-uitest/) SDK i din app, som levereras som en NuGet-paketet. Du hittar i demoappen och det har ingår när du skapar nya testprojekt med Xamarin-mallar.

![Var du hittar plattformsoberoende SDK på gränssnittet](./media/iot-solution-build-system/image4.png)

Ett exempelprojekt test ingår i appen i databasen. I [MyDriving](https://github.com/Azure-Samples/MyDriving/tree/master/src/MobileAppService), tittar du under [src](https://github.com/Azure-Samples/MyDriving/tree/master/src)/MobileApps/[MyDriving](https://github.com/Azure-Samples/MyDriving/tree/master/src/MobileApps/MyDriving)/MyDriving.UITests/.

Om du använder en version av Visual Studio Team Services, är det enkelt att skriva Xamarin UI-kontroller och köra dem som en del av din version.

## <a name="deploy-azure-services"></a>Distribuera Azure-tjänster
Om du vill utföra en automatisk distribution av Azure-tjänster och Team Services build tjänster finns i de detaljerade anvisningarna i **scripts/README.md**.

Microsoft Azure tillhandahåller en mängd olika tjänster som du kan använda för att skapa molnprogram. Även om många kan användas enskilt (till exempel App Service/Web Apps), har de sitt bästa när de är sammankopplade formuläret ett integrerat system som vi använder i MyDriving.

Det är möjligt att skapa och manuellt kopplas samman Azure-tjänster, men det är mycket snabbare och mer tillförlitligt att använda Azure Resource Manager-mallar. [Hanteraren för filserverresurser](../azure-resource-manager/resource-group-overview.md) automatiserar distributionen av en lösning resurser och göra detta anslutningarna mellan dem.

Du hittar mallen för MyDriving system i GitHub-lagringsplatsen under [skript/ARM](https://github.com/Azure-Samples/MyDriving/tree/master/scripts/ARM). Det ger en omfattande och kortfattad översikt över hur olika tjänster i vår arkitektur är sammankopplade. Vi förklarar alla dessa i detalj i den [MyDriving referenshandboken](http://aka.ms/mydrivingdocs), men du kan lära dig mycket genom att läsa igenom själva mallen.

> [!NOTE]
> De flesta Azure-tjänster har en associerad kostnad, beroende på prisnivå. Om du har använt Azure kan du [Prova gratis](https://azure.microsoft.com/free/). Men om du inte planerar att använda vissa komponenter i systemet MyDriving, måste du ta bort dem för att undvika kostnader. Avsnittet ”beräkna driftskostnaderna” senare i den här artikeln innehåller en översikt över vanliga service kostnader.
> 
> 

### <a name="edit-the-template"></a>Redigera mallen
Om du vill anpassa distributionen kanske för att ta bort onödiga komponenter eller lägga till andra, först skapa en kopior av scenariot\_complete.params.json och scenario\_complete.json där du vill göra ändringar.

Du kan använda scenariot\_complete.params.json fil att åsidosätta olika standardvärden, till exempel tjänsten SKU eller replikeringstyp lagring enligt beskrivningen i följande tabell. Standardvärden väljer lägst kostnad.

| **Parametern** | **Beskrivning** | **Standardvärde** |
| --- | --- | --- |
| IoT-hubb SKU |Nivå för Azure IoT Hub-tjänsten |F1 |
| Lagringskontotypen |Replikering lagringstyp |Standard-LRS |
| SQL-Tjänstmålet |Concurrency fack förbrukning |DW100 |
| Värd Plan SKU |Service-plan för Apptjänst |F1 |

I scenariot\_complete.json:

* Sök efter ”baseName” och ändra till ett namn som du föredrar.
* Sök efter ”skapa”. Var och en av dessa avsnitt skapar en resurs.
* Ange sqlServerAdminLogin och sqlServerAdminPassword till lämpliga värden.
* Innan du tar bort ett avsnitt som skapar en resurs kan du kontrollera om det har underordnade genom att söka efter namnet på en annan plats i filen. Observera att varje avsnitt som skapar en tjänst innehåller en *dependsOn* avsnitt som visar dess beroenden.

Här är mallen konfigurerar. Mer information finns i den [referenshandboken](http://aka.ms/mydrivingdocs).

| **Tjänst** | **Beskrivning och information** |
| --- | --- |
| Lagringskonton |Tre konton skapas i mallen: |
| -En SQL-databas som tar emot aggregerade telemetri från Stream Analytics och fungerar som den stödjande lagringen för Azure App Service-tabeller som visar den här informationen via API-slutpunkter. | |
| -Blob lagring som ackumulerar historiska data från en annan Stream Analytics-jobb, som kan bearbetas i HDInsight. | |
| -En SQL-databas som tar emot resultat som bearbetas av HDInsight för användning med Power BI. | |
| Azure IoT Hub |Upprättar en dubbelriktad anslutning till varje ansluten enhet. I lösningen MyDriving fungerar mobilappen som en gateway för fältet Skicka data till Azure IoT Hub. Azure IoT-hubb fungerar då som indata till Stream Analytics. |
| Azure Event Hubs |Utdata för ett Stream Analytics-jobb som köer utdata till tillägg som skapats med Azure Service Fabric. |
| Azure SQL Data Warehouse | |
| Stream Analytics-jobb |Anslut indata och utdata med en fråga som används för att sammanställa både i realtid och historiska data för App Service API: er, Azure Machine Learning, tillägg och Power BI. |
| Machine Learning-arbetsytan |Innehåller experiment, R-koden och API-tjänsten. |
| Azure Data Factory |Schemalagda Machine Learning via programmering. |
| Plan för Service Fabric-värd |För tillägg. |
| Apptjänst (”Mobilapp”) |Värd för Mobile Apps API-projekt som tillhandahåller slutpunkter för mobila appar. API-koden måste distribueras till App Service från Visual Studio. |
| Aviseringsregler |Skickar du e-post om app-svar anger fel. |
| Application Insights |För att övervaka prestanda för API: er i Apptjänst. Du måste konfigurera anslutningen i Visual Studio. |
| Azure Key Vault |För att spara service-kluster-certifikat för webbprogram. |

### <a name="run-the-template"></a>Kör sedan mallen
I **scripts/README.md**, det finns detaljerade anvisningar för att köra mallen.

Om du vill distribuera dessa tjänster i din egen Azure-konto med hjälp av skript, gör du något av följande:

* Använd PowerShell:
  
  ```
  
  cd scripts/PowerShell;
  deploy.ps1 *location* *resourceGroupName*
  ```
  
  * *plats* är den [Azure-plats](https://azure.microsoft.com/regions/), som `North Europe` eller `West US`. Använd `Get-AzureLocation` att hitta en lista över tillgängliga platser.
  * *resourceGroupName* är det namn som du vill ge alla resurser som hör till gruppen. När du är klar med resurserna som du kan ta bort dem samtidigt genom att ta bort den här gruppen.
* Kör DeploymentScripts/Bash/deploy.sh med Bash.
* Öppna och skapa DeploymentScripts/VS/DeployARM.sln för Visual Studio-lösning.

Observera att varje gång mallen körs, skapas en ny uppsättning resurser med nya namn. Gå till portalen för att ta bort resurser och ta bort resursgruppen.

Om skriptet misslyckas av någon anledning, kan du köra det igen.

Skriptet ger dig möjlighet att konfigurera kontinuerlig integration i Visual Studio Team Services. Om du har skapat ett Team Services-projekt, har du en URL: https://yourAccountName.visualstudio.com. Ange den fullständiga URL: en när du tillfrågas. Du kan ge det ett nytt eller befintligt namn för ett Team Services-projekt.

## <a name="set-up-build-and-test-definitions-in-visual-studio-team-services"></a>Konfigurera skapa och testa definitioner i Visual Studio Team Services
Vi använder Team Services för det här projektet främst för dess version och testa funktioner. Men den innehåller också utmärkt samarbete stöd, till exempel aktiviteten hantering med kanban-kort, kod granska integrerad med uppgifter och källkontrollen och gated bygger. Integrerar väl med andra verktyg, till exempel GitHub, Xamarin, HockeyApp och naturligtvis Visual Studio. Du kan nå den via webbgränssnittet eller Visual Studio, beroende på vilket som är det mer praktiskt när som helst.

Stegen i definitionerna bygg- och versionen använder en mängd olika plugin-tjänster som är tillgängliga i Team Services [Marketplace](https://marketplace.visualstudio.com/VSTS). Det finns tjänster som kör versioner av Xamarin-, Android- och andra leverantörer och som ansluter till HockeyApp förutom grundläggande verktyg att köra kommandorader eller kopiera filerna.

![Skapa alternativ i Team Services](./media/iot-solution-build-system/image5.png)

### <a name="build-definitions"></a>Skapa definitioner
Vi har build definitioner för var och en av de huvudsakliga målen. Vi har också variationer för funktionen och regression testning. Det ger oss:

* MyDriving.Services (backend-webb-app för mobilappen)
* MyDriving.Xamarin.Android
  
  * MyDriving.Xamarin.Android-funktionen
  * MyDriving.Xamarin.Android Regression
* MyDriving.Xamarin.iOS
  
  * MyDriving.Xamarin.iOS-funktionen
  * MyDriving.Xamarin.iOS Regression
* MyDriving.Xamarin.UWP
  
  * MyDriving.Xamarin.UWP-funktionen
  * MyDriving.Xamarin.UWP Regression

Om du vill visa fullständig information om vår konfiguration avsnittet 4.7 i den [MyDriving referenshandboken](http://aka.ms/mydrivingdocs), ”bygg och versionen Configuration”. De följer samma allmänna mönster. Skript:

1. Återställer NuGet-paketet. Vi Behåll inte kompilerad kod i databasen, så att de första stegen för varje version är att återställa nödvändiga NuGet-paketen.
2. Aktiverar licensen. Bygga utförs i molnet, där vi måste ha en licens--särskilt för build-tjänsten Xamarin har vi aktivera våra licens på den aktuella build-datorn. Sedan inaktivera vi omedelbart efteråt så att den kan användas på en annan dator.
3. Skapar med hjälp av lämplig tjänst. Vi använder Xamarin versioner för mobila appar och Visual Studio skapar för backend-webbtjänsten.
4. Skapar tester.
5. Kör tester. Vi kör tester för mobila appar i molnet för Xamarin-Test.
6. Publicerar build-resultatet till målplatsen.

Utlösare för de huvudsakliga versionerna har angetts till kontinuerlig integration. Som är körs bygga varje gång koden har checkats in till mastergrenen.

![Gränssnitt där utlösare är inställd på kontinuerlig integration](./media/iot-solution-build-system/image6.png)

### <a name="release-definitions"></a>Viktiga definitioner
Versionen definitioner ställs in på ungefär samma sätt.

För webbtjänsten konfigurera vi distribution som en Azure webbapp:

![Gränssnitt för att konfigurera distribution som en Azure-webbapp](./media/iot-solution-build-system/image7.png)

Och vi versionen utlösaren kontinuerlig distribution. Det vill säga varje incheckning följt av en lyckad build resulterar i en uppdatering till webbappen.

![Gränssnitt för att ställa in versionen utlösaren kontinuerlig distribution](./media/iot-solution-build-system/image8.png)

För mobila appar distribuera till HockeyApp:

![Gränssnitt för att distribuera en mobil app till HockeyApp](./media/iot-solution-build-system/image9.png)

## <a name="explore-telemetry-by-using-application-insights"></a>Utforska telemetri med hjälp av Application Insights
[Application Insights](../application-insights/app-insights-overview.md) samlar in telemetri om prestanda och användning av web services. Application Insights SDK skickar telemetri från tjänsten till Application Insights-resurs i Azure.

Bläddra till Application Insights-resurs som mallen. Där kan du utforska diagram för prestandan hos din [Mobile App Service-projekt](https://github.com/Azure-Samples/MyDriving/tree/master/src/MobileAppService). De visar serverbegäranden och svarstider, fel och undantag räknas. Det finns också diagram av beroende svarstider – det vill säga anrop till databasen och till REST API: er, till exempel Machine Learning. Om det finns några problem med prestanda, ska du kunna vilken typ av systemet som orsakar dem.

![Exempeldiagram av prestanda](./media/iot-solution-build-system/image11.png)

Om du har en webbtjänst som du ställer in manuellt är det enkelt att få samma diagram. Klicka på bladet web service **verktyg** > **tillägg** > **Lägg till**. Välj **Programinsikter**.

![Gränssnitt för att välja Application Insights för att hämta diagram](./media/iot-solution-build-system/image12.png)

Funktionen fungerar genom instrumentering ditt program med Application Insights SDK.

Du kan lägga till anpassad telemetri (eller som ett program som körs någonstans utanför Azure) av [att lägga till Application Insights SDK](../application-insights/app-insights-asp-net.md) utveckling för närvarande. Detta är användbart för log mått som är beroende av programmet, till exempel användarnas genomsnittlig resa längd eller totala hittills utfört. Högerklicka på projektet i Visual Studio och välj sedan **Lägg till Application Insights**.

![Gränssnitt för att välja Lägg till Application Insights för att lägga till anpassad telemetri](./media/iot-solution-build-system/image10.png)

Application Insights skickar aviseringar om onormal antal fel svar. Du kan också ställa in aviseringar på olika mätvärden, till exempel svarstider.

Utan bara för att kontrollera att webbtjänsten alltid är igång och körs, kan du ställa in [tillgänglighetstester](../application-insights/app-insights-monitor-web-app-availability.md). Dessa tester pinga webbplatsen från olika platser över hela världen var 15: e minut. Igen, får du ett e-postmeddelande om det verkar vara ett problem.

## <a name="estimate-operational-costs"></a>Beräkna driftskostnaderna
Det är anmärkningsvärt billigt kör en app som detta i liten skala. Många av tjänsterna har ledigt enklare nivåer, så att utveckling och drift av småskaliga kostar mycket lite. Och naturligtvis egna appar behöver inte använda alla funktioner som visas i MyDriving.

Här är en grov uppskattning av våra kostnader i utvecklings-konfiguration för MyDriving. Vi Observera även vissa alternativ som vi gjorde *inte* använder. Den här informationen kan vara användbar när du uppskattar dina egna kostnader.

Vi förutsätter:

* En grupp av fler än fem (plus sett intressenter).
* Kör för om en månad.
* 100 användare med fyra resor per dag.

> [!NOTE]
> Om du har använt Azure, det finns en [kostnadsfritt konto](https://azure.microsoft.com/free/).
> 
> 

| **Komponenten och tjänst** | **Anteckningar** | **Kostnaden per månad** |
| --- | --- | --- |
| [Visual Studio 2015 Community](https://www.visualstudio.com/products/visual-studio-community-vs) med [Xamarin](https://visualstudiogallery.msdn.microsoft.com/dcd5b7bd-48f0-4245-80b6-002d22ea6eee) <br/>Plattformsoberoende utvecklingsmiljö |Visual Studio Community. (Måste [Visual Studio Professional](https://www.visualstudio.com/vs-2015-product-editions) för [Xamarin.Forms](https://xamarin.com/forms), för att utforma plattformsoberoende från en enda kodbasen.) |$0 |
| [Azure IoT Hub](https://azure.microsoft.com/pricing/details/iot-hub/) <br/>Dubbelriktad dataanslutning till enheter |8 000 meddelanden + 0,5 KB/visas ledigt. |$0 |
| [Stream Analytics](https://azure.microsoft.com/pricing/details/stream-analytics/)  <br/>   Omfattande dataströmmen databearbetning |Kostnad för $0.031 per streaming unit per timme, medan aktiverad. Du väljer antal enheter för strömning som du vill. Mer att skala upp. |$23 |
| [Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/)<br/> Anpassningsbar svar |$10/klient/månad. <br/>                                                                                                                                                                                 + 3 timmar experiment \* 1 USD / experimentera timme. <br/>                                                                                                                                                           + 3.5 timmars API CPU \* $2 / produktion CPU timme. <br/>                                                                                                                                                          API-CPU-tid förutsätter 5 minuter per dag omtränings, även om detta kommer att öka med mer indata.                   <br/>                                                                                                                                                                     + 2 min/dag bedömningen för att bearbeta 400 resor/dag. |$20 |
| [App Service](https://azure.microsoft.com/pricing/details/app-service/)  <br/> Värd för mobila serverdel |Nivå B1--webbprogram för produktion. |$56 |
| [Visual Studio Team Services](https://azure.microsoft.com/pricing/details/visual-studio-team-services/)  <br/> Skapa, testa enhet och versionshantering; hantering av aktiviteter |Privata agenter, fem användare. |$0 |
| [Application Insights](https://azure.microsoft.com/pricing/details/application-insights/) <br/>Övervakning av prestanda och användning av webbtjänster och platser |Kostnadsfri nivå. |$0 |
| [HockeyApp](http://hockeyapp.net/pricing/) <br/> Distribution av appar i beta plus samling feedback, användning och kraschdata |Två kostnadsfria appar för nya användare.<br/> $30/ månaden därefter. |$0 |
| [Xamarin](https://store.xamarin.com/)<br/> Kod på en enhetlig plattform för flera enheter |Kostnadsfri utvärderingsversion. <br/>25 USD per månad därefter. |$0 |
| [SQL-databas](https://azure.microsoft.com/pricing/details/sql-database/) för Azure App Service |Grundnivån; enskild databasmodell. |$5 |
| [Service Fabric](https://azure.microsoft.com/pricing/details/service-fabric/) (valfritt) |Kör en lokala klustret. |$0 |
| [Power BI](https://powerbi.microsoft.com/pricing/)<br/> Flexibel visar och undersökning av direktuppspelat och statiska data |Kostnadsfri nivå: 1 GB, 10 000 rader per timme, dag uppdatering. <br/> $10/användare/månad för [högre gränser](https://powerbi.microsoft.com/documentation/powerbi-power-bi-pro-content-what-is-it/), mer anslutningsalternativ, samarbete. |$0 |
| [Storage](https://azure.microsoft.com/pricing/details/storage/) |L (lokalt redundant) &lt; 100 G $0.024/GB. |$3 |
| [Data Factory](https://azure.microsoft.com/pricing/details/data-factory/) |$0,60 per aktivitet \* (8 – 5 FOC). |$2 |
| [HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/) <br/>  Kluster på begäran för dagliga omtränings |Tre A3 noderna på $0,32 per timme en timme varje dag * 31 dagar. |$30 |
| [Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/) |Grundläggande med $11/ månad genomflödesenhet + $0,028 ingång. |$11 |
| OBD-dongel | |$12 |
| **Totalt** | |**$157** |

Mer information finns i:

* Sammanfattning av [Azure service kvoter och gränser](../azure-subscription-service-limits.md#iot-hub-limits)
* Azure [prisnivå Kalkylatorn](https://azure.microsoft.com/pricing/calculator/)

## <a name="send-us-your-feedback"></a>Skicka feedback
Eftersom vi skapade MyDriving att rivstart IoT systemen vill vi verkligen veta hur det fungerar. Berätta för oss om:

* Du stöter på problem eller utmaningar.
* Det finns en plats för tillägg som gör det passar bättre för ditt scenario.
* Du hittar ett mer effektivt sätt att utföra vissa behov.
* Du har några förslag för att förbättra MyDriving eller den här dokumentationen.

Filen [problem på GitHub] om du vill ge feedback eller lämna en kommentar nedan (en-us edition).

Vi hoppas att få höra från dig!

## <a name="next-steps"></a>Nästa steg
Vi rekommenderar den [MyDriving referenshandboken](http://aka.ms/mydrivingdocs), vilket är en heltäckande beskrivning av systemet och dess komponenter.

