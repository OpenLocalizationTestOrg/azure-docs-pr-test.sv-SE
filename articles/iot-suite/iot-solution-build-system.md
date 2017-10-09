---
title: 'MyDriving Azure IoT-exempel: skapa den | Microsoft Docs'
description: "Skapa en app som är en omfattande demonstration av hur tooarchitect en IoT-system med hjälp av Microsoft Azure, inklusive Stream Analytics, Machine Learning och Händelsehubbar."
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
ms.openlocfilehash: e78571225697f745fe011c722e57c8600704c392
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="build-and-deploy-hello-mydriving-solution-tooyour-environment"></a>Skapa och distribuera hello MyDriving lösning tooyour miljö
MyDriving är en Sakernas Internet (IoT) som samlar in data från en bil, bearbetar den med hjälp av machine learning och visas på din mobiltelefon. hello serverdel består av en mängd olika tjänster som tillhandahålls av Microsoft Azure. hello-klienter kan vara Android, iOS eller Windows 10-telefoner.

Vi har skapat hello MyDriving lösning toogive du förhandstillgång för att skapa en egen IoT-system. Från hello [MyDriving databasen på GitHub](https://github.com/Azure-Samples/MyDriving), du kan få Azure Resource Manager skript toodeploy hello backend-arkitektur till din egen Azure-konto. Från den tidpunkten kan du konfigurera om hello olika tjänster, ändra hello frågor toosuit dina egna data och så vidare. Du hittar dessa skript--tillsammans med koden för hello mobila appar, hello Azure App Service API-projekt med mera – hello MyDriving på lagringsplatsen.

Om du inte har försökt hello app ännu, titta på hello [Get igång](iot-solution-get-started.md).

Det finns ett detaljerat konto för hello-arkitekturen i hello [MyDriving referenshandboken](http://aka.ms/mydrivingdocs). Sammanfattningsvis: det finns flera uppgifter som vi konfigurerar toocreate ett liknande projekt:

* En **klientappen** körs på Android, iOS och Windows 10-telefoner. Vi använder hello Xamarin plattform tooshare mycket av hello kod, som är lagrad på GitHub under `src/MobileApp`. hello app utför två olika funktioner:
  * Den vidarebefordrar telemetri från hello inbyggd diagnostik (OBD) enhet och från sin egen plats service toohello systemets molnet serverdel.
  * Det är ett användargränssnitt där användare kan fråga om deras inspelade körda mil.
* En **Molntjänsten** en hello väg resa data i realtid och bearbetar dessa. hello huvudsakliga arbetet med att skapa den här tjänsten är toochoose parameterstyra och ansluta dig en mängd olika Azure-tjänster. En del av hello delar kräver skript toofilter och processen hello inkommande data. Vi använder en Azure Resource Manager-mall tooconfigure alla hello delar.
* En **Mobiltjänst app** är hello webbtjänst bakom hello användaren gränssnittet tillhör hello enhetsapp. Det huvudsakliga jobbet är tooquery hello databasen lagras, bearbetade data. Kod som finns på GitHub under `src/MobileAppService`.
* **Visual Studio med Xamarin** är vår utvecklingsmiljö. Xamarin som finns både som en komponent i Visual Studio och som en fristående integrerad utvecklingsmiljö (IDE), används toobuild hello plattformsoberoende enheten kod. toobuild hello iOS kod, är det nödvändigt toohave en instans av Xamarin som körs på en OS X-dator. Om det behövs kan du köra den som en agent hanteras från Visual Studio.
* **Testa enheten** av hello enhet utförs appar i molnet för Xamarin-Test.
* **GitHub** är hello lagringsplats där vi lagrar alla hello kod, skript och mallar.
* **Visual Studio Team Services** är en molnbaserad tjänst som har använt toomanage hello kontinuerlig bygg- och test av hello tjänsten och webbprogram.
* **HockeyApp** är används toodistribute versioner av hello kod för enheten. Den samlar även in krascher och användningsrapporter och användarfeedback.
* **Visual Studio Application Insights** Övervakare hello mobila webbtjänsten.

Så Låt oss se hur vi ställer in alla som. 

> [!NOTE] 
> Många av hello följande steg är valfria.
>
>

## <a name="sign-up-for-accounts"></a>Registrera dig för konton
* [Visual Studio Dev Essentials](https://www.visualstudio.com/products/visual-studio-dev-essentials-vs.aspx). Det här kostnadsfria programmet ger enkel åtkomst toomany utvecklingsverktyg och tjänster, inklusive Visual Studio, Visual Studio Team Services och Azure. Den ger dig en 25 USD per månad kredit på Azure i tolv månader. Den omfattar också prenumerationer tooPluralsight utbildning och Xamarin University. Du kan också registrera dig separat för kostnadsfria nivåer av [Azure](https://azure.com) och [Visual Studio Team Services](https://www.visualstudio.com/products/visual-studio-team-services-vs.aspx), men de ger inte Azure-krediter.
* [HockeyApp](https://rink.hockeyapp.net/) (valfritt), för att hantera test distribution av mobila appar och samla in telemetri.
* [Xamarin](https://xamarin.com/) (obligatoriskt), för att skapa hello mobilappar och körs debug körs och tester [Xamarin Test molnet](https://xamarin.com/test-cloud).
* [GitHub](https://github.com/Azure-Samples/MyDriving/) (valfritt) toocreate ledigt offentliga databaser för egen kod (privat databaser är betalda). Alternativt kan du använda hello grundläggande planen i Visual Studio Team Services för privata databaser.
* [Power BI](https://powerbi.microsoft.com/) (valfritt) toocreate omfattande visualiseringar av data över hello hela systemet.

> [!NOTE]
> Du behöver ett konto tooaccess hello MyDriving koden i GitHub [hello GitHub MyDriving databasen](https://github.com/Azure-Samples/MyDriving).
> 
> 

## <a name="install-development-tools"></a>Installera utvecklingsverktygen
hello följande inställningar är för att utveckla hello fullständig lösning: iOS-, Android- och Windows 10 Mobile plattformsoberoende app med en Azure-serverdel.

Alternativt kan använda du Xamarin Studio på Mac- eller Windows toodevelop hello mobilappar om du inte arbetar med hello Azure tillbaka avslutas.

Det finns en [längre beskrivning av den här installationen](https://msdn.microsoft.com/library/mt613162.aspx).

### <a name="windows-development-machine"></a>Windows-utvecklingsdator
hello centrala verktyget i Windows är Visual Studio för att arbeta med hello MyDriving app för Android och Windows hello App Service API-projekt och mikrotjänster tillägg.

Xamarin, Git, emulatorerna och andra användbara komponenter integreras med Visual Studio.

Installera:

* [Visual Studio med Xamarin](https://www.visualstudio.com/products/visual-studio-community-vs) (en utgåva--Community är ledigt).
* [SQLite för universella Windows-plattformen](https://visualstudiogallery.msdn.microsoft.com/4913e7d5-96c9-4dde-a1a1-69820d615936). Krävs toobuild hello Windows 10 Mobile-kod.
* [Azure SDK för Visual Studio](https://www.visualstudio.com/vs/azure-tools/). Ger du hello SDK för program som körs i Azure, tillsammans med verktyg för att hantera Azure.
* [Azure Service Fabric SDK](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric). Nödvändiga toobuild hello [mikrotjänster](../service-fabric/service-fabric-get-started.md) tillägg.

Var noga med att du har hello rätt Visual Studio-tillägg. Kontrollera att under **verktyg**, visas **Android, iOS, Xamarin...** . Om inte, öppna Visual Studio, söka efter Xamarin och följ hello prompter tooinstall den. Kontrollera också att **Git för Windows** är installerad. Om inte, i Visual Studio, söka efter den och följ hello prompter tooinstall den. 

### <a name="mac-development-machine"></a>Mac-utvecklingsdator
hello Mac (Yosemite eller senare) krävs om du vill toodevelop för iOS. Även om vi använder Visual Studio med Xamarin på Windows toodevelop och hantera alla hello kod Xamarin använder en agent installeras på en Mac i ordning toobuild och logga hello iOS-kod.

![Utveckla på Windows och bygger på Mac](./media/iot-solution-build-system/image1.png)

(Som ett alternativ kan du kan använda Xamarin Studio direkt på hello Mac toodevelop plattformsoberoende appar.)

Du behöver inte hello Mac om du inte vill tooinclude iOS som en målplattform.

Installera:

* [Xamarin Studio för iOS](https://developer.xamarin.com/guides/ios/getting_started/installation/mac/). Du kan också ställa in Visual Studio och Xamarin på en Mac som kör en virtuell Windows-dator. Se [konfiguration, installation och verifieringar för Mac-användare](https://msdn.microsoft.com/library/mt488770.aspx) på MSDN.
* [Azure utvecklingsverktyg](https://azure.microsoft.com/downloads/) (valfritt).

Aktivera fjärrinloggning på hello Mac. Öppna **systeminställningar** > **delning**, och välj sedan **fjärrinloggning**.

När du öppnar en iOS-projekt i Visual Studio i Windows hello Xamarin plugin-programmet kommer att fråga efter hello-ID för hello Mac.

## <a name="fetch-hello-github-repository"></a>Hämta hello GitHub-lagringsplatsen
Hämta en lokal kopia av [hello GitHub MyDriving databasen](https://github.com/Azure-Samples/MyDriving) med hjälp av hello **hämta ZIP** knappen i GitHub, Visual Studio eller en annan Git-klient.

Packa upp hello tooa mapp med en kort sökväg, till exempel C:\\kod.

Alternativt, om du vill tookeep in toodate med eller bidra tooour kod klona hello databasen på följande sätt:

**Git-klon https://github.com/Azure-Samples/MyDriving.git**

## <a name="get-a-bing-maps-api-key"></a>Hämta en Bing maps API-nyckel
[Registrera dig för en Bing Maps API-nyckel](https://msdn.microsoft.com/library/ff428642.aspx).

Du behöver det i rad 22 i tooreplace `src/MobileApps/MyDriving/MyDriving.Utils/Logger.cs`.

## <a name="build-hello-demo-app"></a>Skapa hello demo-appen
Öppna dessa lösningar i Visual Studio:

* src\MobileApps\MyDriving.SLN
* src\MobileAppService\MyDrivingService.SLN
* src\Extensions\ServiceFabric\VINLookUpApplication\VINLookUpApplication.SLN

Får du anvisningarna för att:

* Förtroende för vissa potentiellt opålitliga projekt. Välj tooopen dem om du vill toogo vidare.
* Ange utvecklarläget om du arbetar med en ny Windows 10-dator.
* Ange dina autentiseringsuppgifter för Xamarin.
* Ansluta toohello Xamarin Mac. Om du inte har en Mac, högerklicka på hello iOS-projekt i Visual Studio och välj sedan **ta bort projektet**.

Återskapa hello lösning.

Om du har problem med att skapa försök hello lösningar tooquirks som vi har hittat:

* *VINLookupApplication projekt inte läsa in*: Kontrollera att du installerat hello [Azure SDK för Visual Studio](https://www.visualstudio.com/vs/azure-tools/).
* *Service Fabric-projektet inte skapa*: skapa hello gränssnittet projekt först och kontrollera att du installerat hello Service Fabric-SDK.
* *Android-app inte skapa*:
  
  * Öppna **verktyg** > **Android** > **Android SDK Manager**, och kontrollera att Android 6 (API-23) / SDK-plattformen är installerat.
  * Ta bort den här katalogen och bygg sedan:<br/>
    `%LocalAppData%\Xamarin\zips`

## <a name="get-tooknow-hello-code"></a>Hämta tooknow hello kod
I hello lösning hittar du:

* Azure-tillägg: Service Fabric.
* Azure HDInsight: Skript för att bearbeta resa data i Azure.
* Mobila appar: appar hello enheter.
* MobileAppsService/MyDrivingService: hello web tillbaka avslutas.
* Powerbi: hello rapportdefinitionen.
* Skript:
  
  * Resource Manager: mallar toobuild hello Azure-resurser.
  * PowerShell: Skript toorun hello Resource Manager-mallar.
  * Azure SQL Database: Felsökning databaser.
* SQL Database: CreateTables: schemadefinitioner.
* Azure Stream Analytics: Frågar den transformering hello inkommande dataströmmen.

## <a name="run-hello-apps-in-development-mode"></a>Köra hello appar i utvecklingsläge
Vidta åtgärden toorun hello appar, baserat på hello-enhet som du använder:

* Serverdelen: Ange MyDrivingService som hello Startprojekt och tryck på F5 toorun hello-backend-webbtjänst. En Webbläsarvy över hello API lista öppnas.
* Mobila klienter: hello [mobila appar utvecklas i Xamarin](https://developer.xamarin.com/guides/cross-platform/deployment,_testing,_and_metrics/debugging_with_xamarin/).
  
  * Android: Mer information finns i [felsökning Android i Xamarin](http://developer.xamarin.com/guides/android/deployment,_testing,_and_metrics/debugging_with_xamarin_android/).
  * iOS: Mer information finns i [felsökning i iOS-](http://developer.xamarin.com/guides/ios/deployment,_testing,_and_metrics/debugging_in_xamarin_ios/).
  * Windows Phone: Mer information finns i [Xamarin + Windows Phone](https://developer.xamarin.com/guides/cross-platform/windows/phone/).

## <a name="upload-hello-mobile-app-toohockeyapp"></a>Överför hello mobilappen tooHockeyApp
HockeyApp hanterar hello distribution av Android, iOS eller Windows tootest användare av ditt program, meddela användaren om nya versioner. Den samlar även in användbar kraschrapporter, Användarfeedback med skärmbilder och användningsstatistik.

[Starta genom att överföra](http://support.hockeyapp.net/kb/app-management-2/how-to-create-a-new-app) build-app. Logga sedan in för[HockeyApp](https://rink.hockeyapp.net) från utvecklingsdatorn. På instrumentpanelen för utvecklare av hello klickar du på **ny App**, och dra sedan hello inbyggda filer till hello-fönstret. (Senare, kan du automatisera dina build service toodo detta.)

Du är nu i din app instrumentpanel.

![Översikt översiktsfliken på instrumentpanelen i hello](./media/iot-solution-build-system/image2.png)

Upprepa hello processen för varje plattform som appen körs på. Gör sedan följande hello:

* Använd hello [app-ID](http://support.hockeyapp.net/kb/app-management-2/how-to-find-the-app-id) från hello instrumentpanelen toosend kraschdata och feedback från din app. Uppdatera hello-ID: N i src/MobileApps/MyDriving/MyDriving.Utils/Logger.cs i MyDriving.
* [Bjud in testanvändare](http://support.hockeyapp.net/kb/app-management-2/how-to-invite-beta-testers). Du får en URL-toorecruit Testare användare. De kommer att kunna toosign för din grupp, hämta hello app och skicka feedback.
* Om du föredrar en mer öppna betaversion ange hello distribution toopublic. Klicka på **hantera appen** > **Distribution** > **hämta = offentlig**. Nu kan vem som helst hämta din app och skicka feedback och de ser ett meddelande när du publicerar en ny version. Du kan få vissa kraschrapporter från dem för.
  
   ![Team på hello instrumentpanel](./media/iot-solution-build-system/image3.png)
* [Länken krascher rapporterar tooVisual Studio Team Services](http://support.hockeyapp.net/kb/third-party-bug-trackers-services-and-webhooks/how-to-use-hockeyapp-with-visual-studio-team-services-vsts-or-team-foundation-server-tfs). Klicka på **hantera appen** > **Visual Studio Team Services**. HockeyApp kan skapa arbetsobjekt i Team Services automatiskt när det finns kraschrapporter eller när feedback tas emot.

Läs mer på hello [HockeyApp plats](https://hockeyapp.net).

## <a name="test-hello-mobile-app-on-xamarin-test-cloud"></a>Testa hello mobila app i Xamarin Test moln
[Xamarin Test molnet](https://developer.xamarin.com/guides/testcloud/introduction-to-test-cloud/) automatiserar UI testning på verkliga enheter i hello molnet. Genom att använda hello NUnit framework kan skriva du tester som kör appen via hello användargränssnitt.

toouse Xamarin du införliva hello [Xamarin.UITests](https://developer.xamarin.com/guides/testcloud/uitest/intro-to-uitest/) SDK i din app, som levereras som en NuGet-paketet. Du hittar i hello demo-appen och det har ingår när du skapar nya testprojekt med hello Xamarin mallar.

![Där toofind hello plattformsoberoende SDK på hello-gränssnittet](./media/iot-solution-build-system/image4.png)

Ett exempelprojekt test ingår i hello app i hello-databasen. I [MyDriving](https://github.com/Azure-Samples/MyDriving/tree/master/src/MobileAppService), tittar du under [src](https://github.com/Azure-Samples/MyDriving/tree/master/src)/MobileApps/[MyDriving](https://github.com/Azure-Samples/MyDriving/tree/master/src/MobileApps/MyDriving)/MyDriving.UITests/.

Om du använder en version av Visual Studio Team Services, är det enkelt toowrite Xamarin UI enhet testar och köra dem som en del av din version.

## <a name="deploy-azure-services"></a>Distribuera Azure-tjänster
tooperform en automatisk distribution av Azure-tjänster och Team Services build tjänster finns toohello detaljerade instruktioner i **scripts/README.md**.

Microsoft Azure tillhandahåller en mängd olika tjänster som du kan använda toobuild molnprogram. Även om många kan användas enskilt (till exempel App Service/Web Apps), de är på deras när de är sammankopplade tooform ett integrerat system som de som vi använder i MyDriving.

Det är möjligt toocreate och manuellt kopplas samman Azure-tjänster, men det är mycket snabbare och mer tillförlitligt toouse Azure Resource Manager-mallar. [Hanteraren för filserverresurser](../azure-resource-manager/resource-group-overview.md) automatiserar hello distribution av en lösning resurser och göra hello anslutningarna mellan dem.

Du hittar hello mall för hello MyDriving system i hello GitHub-lagringsplatsen under [skript/ARM](https://github.com/Azure-Samples/MyDriving/tree/master/scripts/ARM). Det ger en omfattande och kortfattad översikt över hur hello olika tjänster i vår arkitektur är sammankopplade. Vi förklarar alla dessa i detalj i hello [MyDriving referenshandboken](http://aka.ms/mydrivingdocs), men du kan lära dig mycket genom att läsa igenom själva hello mallen.

> [!NOTE]
> De flesta Azure-tjänster har en associerad kostnad, beroende på hello prisnivån. Om du är ny tooAzure, kan du [Prova gratis](https://azure.microsoft.com/free/). Om du inte toouse vissa komponenter i hello MyDriving system kan vara att tooremove dem tooavoid ta upp kostnader. Hej ”uppskattning driftskostnaderna” avsnitt innehåller senare i den här artikeln en översikt över vanliga service kostnader.
> 
> 

### <a name="edit-hello-template"></a>Redigera hello mall
toocustomize distributionen kanske tooremove onödiga komponenter eller tooadd andra, först skapa en kopior av scenariot\_complete.params.json och scenario\_complete.json i vilka toomake ändras.

Du kan använda hello scenariot\_complete.params.json filen toooverride olika standardvärdena, till exempel hello service SKU eller hello replikering lagringstyp, enligt beskrivningen i följande tabell hello. hello standardvärden Välj hello billigaste alternativ.

| **Parametern** | **Beskrivning** | **Standardvärde** |
| --- | --- | --- |
| IoT-hubb SKU |Nivå för Azure IoT Hub-tjänsten |F1 |
| Lagringskontotypen |Replikering lagringstyp |Standard-LRS |
| SQL-Tjänstmålet |Concurrency fack förbrukning |DW100 |
| Värd Plan SKU |Service-plan för Apptjänst |F1 |

I scenariot\_complete.json:

* Sök efter ”baseName” och ändra den tooa namn som du föredrar.
* Sök efter ”skapa”. Var och en av dessa avsnitt skapar en resurs.
* Ange sqlServerAdminLogin och sqlServerAdminPassword toosuitable värden.
* Innan du tar bort ett avsnitt som skapar en resurs kan du kontrollera om det har underordnade genom att söka efter namnet på andra ställen i hello-filen. Observera att varje avsnitt som skapar en tjänst innehåller en *dependsOn* avsnitt som visar dess beroenden.

Här följer konfigurerar vilken hello-mall. Mer information finns i hello [referenshandboken](http://aka.ms/mydrivingdocs).

| **Tjänst** | **Beskrivning och information** |
| --- | --- |
| Lagringskonton |hello skapas tre konton: |
| -En SQL-databas som tar emot aggregerade telemetri från Stream Analytics och fungerar som hello stödjande store för Azure App Service-tabeller som visar den här informationen via API-slutpunkter. | |
| -Blob lagring som ackumulerar historiska data från en annan Stream Analytics-jobbet toobe som bearbetas av HDInsight. | |
| -En SQL-databas som tar emot resultat som bearbetas av HDInsight för användning med Power BI. | |
| Azure IoT Hub |Upprättar en dubbelriktad anslutning tooeach ansluten enhet. I hello MyDriving lösning, hello mobilappar som fungerar som en fältet gateway toosend data tooAzure IoT-hubb. Azure IoT-hubb fungerar sedan som en inkommande tooStream Analytics. |
| Azure Event Hubs |Utdata för ett Stream Analytics-jobb som köer hello utdata tooextensions som skapats med Azure Service Fabric. |
| Azure SQL Data Warehouse | |
| Stream Analytics-jobb |Anslut indata och utdata med en fråga som används tooaggregate både i realtid och historiska data för hello App Service API: er, Azure Machine Learning, tillägg och Power BI. |
| Machine Learning-arbetsytan |Innehåller experiment, R-koden och API-tjänsten. |
| Azure Data Factory |Schemalagda Machine Learning via programmering. |
| Plan för Service Fabric-värd |För tillägg. |
| Apptjänst (”Mobilapp”) |Värdar hello Mobile Apps API-projekt som tillhandahåller slutpunkter för hello mobila appar. hello API-koden måste vara distribuerad tooApp tjänst från Visual Studio. |
| Aviseringsregler |Skickar du e-post om hello app svar anger fel. |
| Application Insights |För att övervaka prestanda för hello API: er i Apptjänst. Du har tooconfigure hello anslutning i Visual Studio. |
| Azure Key Vault |För att spara certifikat hello web service-kluster. |

### <a name="run-hello-template"></a>Kör hello mallen
I **scripts/README.md**, det finns detaljerade anvisningar för att köra hello-mallen.

tooprovision dessa tjänster i din egen Azure-konto med hjälp av hello skript gör något av följande hello:

* Använd PowerShell:
  
  ```
  
  cd scripts/PowerShell;
  deploy.ps1 *location* *resourceGroupName*
  ```
  
  * *plats* är hello [Azure-plats](https://azure.microsoft.com/regions/), som `North Europe` eller `West US`. Använd `Get-AzureLocation` toofind en lista över tillgängliga platser.
  * *resourceGroupName* är hello-namn som du vill toogive toohello grupp som alla hello-resurser ska tillhöra. När du är klar med hello resurser du kan ta bort dem samtidigt genom att ta bort den här gruppen.
* Kör DeploymentScripts/Bash/deploy.sh med Bash.
* Öppna och skapa hello Visual Studio-lösning DeploymentScripts/VS/DeployARM.sln.

Observera att varje gång hello mall körs, skapas en ny uppsättning resurser med nya namn. toodelete hello resurser går toohello portal och ta bort hello resursgruppen.

Om hello skriptet misslyckas av någon anledning, kan du köra det igen.

hello skriptet ger du hello möjlighet att konfigurera kontinuerlig integration i Visual Studio Team Services. Om du har skapat ett Team Services-projekt, har du en URL: https://yourAccountName.visualstudio.com. Ange hello fullständiga URL: en när du tillfrågas. Du kan ge det ett nytt eller befintligt namn för ett Team Services-projekt.

## <a name="set-up-build-and-test-definitions-in-visual-studio-team-services"></a>Konfigurera skapa och testa definitioner i Visual Studio Team Services
Vi använder Team Services för det här projektet främst för dess version och testa funktioner. Men den innehåller också utmärkt samarbete stöd, till exempel aktiviteten hantering med kanban-kort, kod granska integrerad med uppgifter och källkontrollen och gated bygger. Integrerar väl med andra verktyg, till exempel GitHub, Xamarin, HockeyApp och naturligtvis Visual Studio. Du kan nå den via webbgränssnittet för hello eller Visual Studio, beroende på vilket som är det mer praktiskt när som helst.

hello stegen i hello version och versionen definitioner använder flera plugin-tjänster som är tillgängliga i hello Team Services [Marketplace](https://marketplace.visualstudio.com/VSTS). Det finns tjänster som kör versioner av Xamarin-, Android- och andra leverantörer och som ansluter tooHockeyApp i tillägg toobasic verktyg toorun kommandorader eller kopiera filer.

![Skapa alternativ i Team Services](./media/iot-solution-build-system/image5.png)

### <a name="build-definitions"></a>Skapa definitioner
Vi har build definitioner för varje hello huvudsakliga mål. Vi har också variationer för funktionen och regression testning. Det ger oss:

* MyDriving.Services (hello backend-webb-appen för mobila hello)
* MyDriving.Xamarin.Android
  
  * MyDriving.Xamarin.Android-funktionen
  * MyDriving.Xamarin.Android Regression
* MyDriving.Xamarin.iOS
  
  * MyDriving.Xamarin.iOS-funktionen
  * MyDriving.Xamarin.iOS Regression
* MyDriving.Xamarin.UWP
  
  * MyDriving.Xamarin.UWP-funktionen
  * MyDriving.Xamarin.UWP Regression

Om du vill toosee hello fullständig information om vår konfiguration, se 4.7 av hello [MyDriving referenshandboken](http://aka.ms/mydrivingdocs), ”bygg och versionen Configuration”. De följer hello samma allmänna mönster. hello skript:

1. Återställer hello NuGet-paketet. Vi Behåll inte kompilerad kod i hello databas så att hello första stegen för varje version är toorestore hello krävs NuGet-paket.
2. Aktiverar hello-licens. hello build utförs i hello moln, där vi måste ha en licens--särskilt för hello Xamarin byggtjänst--har vi tooactivate våra licens hello aktuella build-dator. Sedan vi inaktivera omedelbart efteråt tooallow den toobe som används på en annan dator.
3. Skapar med hjälp av lämplig hello-tjänst. Vi använder Xamarin versioner för hello mobila appar och Visual Studio skapar för hello backend webbtjänst.
4. Skapar tester.
5. Kör tester. Vi kör hello mobilappen test i Xamarin Test molnet.
6. Publicerar hello build resultatet toohello målplatsen.

hello utlösare för hello huvudsakliga versioner anges toocontinuous integrering. Som är körs hello build varje gång kod kontrolleras i toohello mastergrenen.

![Gränssnitt där hello utlösare är uppsättningen toocontinuous integrering](./media/iot-solution-build-system/image6.png)

### <a name="release-definitions"></a>Viktiga definitioner
Versionen definitioner ställs in i mycket hello samma sätt.

För hello-webbtjänsten konfigurera vi distribution som en Azure webbapp:

![Gränssnitt för att konfigurera distribution som en Azure-webbapp](./media/iot-solution-build-system/image7.png)

Och vi hello versionen utlösaren toocontinuous distribution. Det vill säga varje incheckning följt av en lyckad build resulterar i en update toohello webbapp.

![Gränssnitt för att ange hello versionen utlösaren toocontinuous distribution](./media/iot-solution-build-system/image8.png)

För mobila appar distribuera vi tooHockeyApp:

![Gränssnitt för att distribuera en mobilapp tooHockeyApp](./media/iot-solution-build-system/image9.png)

## <a name="explore-telemetry-by-using-application-insights"></a>Utforska telemetri med hjälp av Application Insights
[Application Insights](../application-insights/app-insights-overview.md) samlar in telemetri om hello prestanda och användning av web services. hello Application Insights SDK skickar telemetri från hello service toohello Application Insights-resurs i Azure.

Bläddra toohello Application Insights-resurs hello mallen ställer in. Där kan du utforska diagram av hello prestanda hos din [Mobile App Service-projekt](https://github.com/Azure-Samples/MyDriving/tree/master/src/MobileAppService). De visar serverbegäranden och svarstider, fel och undantag räknas. Det finns också diagram av beroende svarstider – det vill säga anrop toohello databasen och tooREST API: er som Machine Learning. Om det finns några problem med prestanda, kommer du att kunna toosee vilken typ av systemet som orsakar dem.

![Exempeldiagram av prestanda](./media/iot-solution-build-system/image11.png)

Om du har en webbtjänst som du ställer in manuellt enkelt tooget hello samma diagram. Klicka på hello web service bladet **verktyg** > **tillägg** > **Lägg till**. Välj **Programinsikter**.

![Gränssnitt för att välja Application Insights tooget hello diagram](./media/iot-solution-build-system/image12.png)

hello-funktionen fungerar av instrumentering ditt program med hello Application Insights SDK.

Du kan lägga till anpassad telemetri (eller som ett program som körs någonstans utanför Azure) av [lägger till hello Application Insights SDK](../application-insights/app-insights-asp-net.md) utveckling för närvarande. Detta är användbart toolog mått som är beroende av hello program, till exempel användarnas genomsnittlig resa längd eller totala hittills utfört. Högerklicka på hello-projekt i Visual Studio och välj sedan **Lägg till Application Insights**.

![Gränssnitt för att välja Lägg till Application Insights tooadd anpassad telemetri](./media/iot-solution-build-system/image10.png)

Application Insights skickar aviseringar om onormal antal fel svar. Du kan också ställa in aviseringar på olika mätvärden, till exempel svarstider.

Bara toobe att webbtjänsten alltid är igång och körs, kan du ställa in [tillgänglighetstester](../application-insights/app-insights-monitor-web-app-availability.md). Dessa tester pinga webbplatsen från olika platser runt hello world var 15: e minut. Igen, får du ett e-postmeddelande om systemdiskar toobe problem.

## <a name="estimate-operational-costs"></a>Beräkna driftskostnaderna
Det är anmärkningsvärt prisvärda toorun en app som det här i liten skala. Många av hello tjänster har ledigt enklare nivåer, så att utveckling och drift av småskaliga kostar mycket lite. Och naturligtvis egna appar har inte toouse alla hello-funktioner som visas i MyDriving.

Här är en grov uppskattning av våra kostnader i hello development konfiguration för MyDriving. Vi Observera även vissa alternativ som vi gjorde *inte* använder. Den här informationen kan vara användbar när du uppskattar dina egna kostnader.

Vi förutsätter:

* En grupp av fler än fem (plus sett intressenter).
* Kör för om en månad.
* 100 användare med fyra resor per dag.

> [!NOTE]
> Om du är ny tooAzure finns en [kostnadsfritt konto](https://azure.microsoft.com/free/).
> 
> 

| **Komponenten och tjänst** | **Anteckningar** | **Kostnaden per månad** |
| --- | --- | --- |
| [Visual Studio 2015 Community](https://www.visualstudio.com/products/visual-studio-community-vs) med [Xamarin](https://visualstudiogallery.msdn.microsoft.com/dcd5b7bd-48f0-4245-80b6-002d22ea6eee) <br/>Plattformsoberoende utvecklingsmiljö |Visual Studio Community. (Måste [Visual Studio Professional](https://www.visualstudio.com/vs-2015-product-editions) för [Xamarin.Forms](https://xamarin.com/forms), toodesign plattformar från en enda kodbasen.) |$0 |
| [Azure IoT Hub](https://azure.microsoft.com/pricing/details/iot-hub/) <br/>Dubbelriktad data anslutning toodevices |8 000 meddelanden + 0,5 KB/visas ledigt. |$0 |
| [Stream Analytics](https://azure.microsoft.com/pricing/details/stream-analytics/)  <br/>   Omfattande dataströmmen databearbetning |Kostnad för $0.031 per streaming unit per timme, medan aktiverad. Du väljer hello antalet strömmande enheter som du vill. flera tooscale upp. |$23 |
| [Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/)<br/> Anpassningsbar svar |$10/klient/månad. <br/>                                                                                                                                                                                 + 3 timmar experiment \* 1 USD / experimentera timme. <br/>                                                                                                                                                           + 3.5 timmars API CPU \* $2 / produktion CPU timme. <br/>                                                                                                                                                          API-CPU-tid förutsätter 5 minuter per dag omtränings, även om detta kommer att öka med mer indata.                   <br/>                                                                                                                                                                     + 2 min/dag bedömningen tooprocess 400 resor/dag. |$20 |
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
Eftersom vi skapade MyDriving toohelp rivstart IoT systemen vill vi verkligen toohear från dig om hur det fungerar. Berätta för oss om:

* Du stöter på problem eller utmaningar.
* Det finns en plats för tillägg som gör det lämpligare tooyour scenario.
* Du hittar ett mer effektivt sätt tooaccomplish vissa behov.
* Du har några förslag för att förbättra MyDriving eller den här dokumentationen.

toogive feedback filen [problem på GitHub] eller lämna en kommentar nedan (en-us edition).

Vi ser fram emot toohearing från dig!

## <a name="next-steps"></a>Nästa steg
Vi rekommenderar hello [MyDriving referenshandboken](http://aka.ms/mydrivingdocs), vilket är en heltäckande beskrivning av hello designen av hello system och dess komponenter.

