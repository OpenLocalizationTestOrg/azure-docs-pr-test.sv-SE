---
title: "aaaMigrate från Mobile Services tooan Apptjänst Mobile App"
description: "Lär dig hur tooeasily migrera din Mobile Services programmet tooan Apptjänst Mobile App"
services: app-service\mobile
documentationcenter: 
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 07507ea2-690f-4f79-8776-3375e2adeb9e
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile
ms.devlang: na
ms.topic: article
ms.date: 10/03/2016
ms.author: glenga
ms.openlocfilehash: cd2e8d98595703389300b79da9bf51cdcefe7b40
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="article-top"></a>Migrera dina befintliga Azure-Mobiltjänst tooAzure Apptjänst
Med hello [allmän tillgång till Azure App Service], Azure Mobile Services-platser kan vara enkelt migreras på plats tootake utnyttja alla funktioner i hello Azure App Service.  Det här dokumentet beskriver vilka tooexpect när du migrerar din webbplats från Azure Mobile Services tooAzure Apptjänst.

## <a name="what-does-migration-do"></a>Vad gör migrering tooyour platsen
Migrering av din Azure-Mobiltjänst stängs din Mobiltjänst i ett [Azure App Service] app utan att påverka hello kod.  Din Notification Hubs, SQL-anslutningen, autentiseringsinställningar, schemalagda jobb och domännamn ändras inte.  Mobila klienter som använder ditt Azure-Mobiltjänst fortsätta toooperate normalt.  Migreringen startar om tjänsten när den är överförda tooAzure Apptjänst.

[!INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

## <a name="why-migrate"></a>Varför du bör migrera din plats
Microsoft rekommenderar att du migrerar din Azure-Mobiltjänst tootake fördel av hello funktionerna i Azure App Service, inklusive:

* Nya värdfunktioner såsom [WebJobs] och [anpassade domännamn].
* Anslutningen tooyour lokala resurser med hjälp av [VNet] dessutom för[Hybridanslutningar].
* Övervakning och felsökning med New Relic eller [Programinsikter].
* Inbyggda DevOps-tooling, inklusive [mellanlagring fack], återställning och i produktion testning.
* [Autoskala], belastningsutjämning, och [prestandaövervakning].

Mer information om hello fördelar i Azure App Service finns hello [jämfört med Mobile Services. Apptjänst] avsnittet.

## <a name="before-you-begin"></a>Innan du börjar
Innan du påbörjar något större arbete på din webbplats, bör du säkerhetskopiera din Mobiltjänst skript och SQL-databas.

## <a name="migrating-site"></a>Migrera dina platser
hello migreringsprocessen migrerar alla platser inom en enda Azure-Region.

toomigrate webbplatsen:

1. Logga in toohello [klassiska Azure-portalen].
2. Välj en Mobiltjänst i hello region som du vill toomigrate.
3. Klicka på hello **migrera tooApp Service** knappen.

   ![hello migrera knappen][0]
4. Läsa hello migrera tooApp Service dialogrutan.
5. Ange hello namnet på din Mobiltjänst i hello rutan.  Till exempel om ditt domännamn är contoso.azure mobile.net, ange *contoso* i hello rutan.
6. Klicka hello skalstreck.

Övervaka hello migrering i hello aktivitetsövervakning hello status. Webbplatsen visas som *migrera* i hello klassiska Azure-portalen.

  ![Aktivitetsövervakning för migrering][1]

Varje migrering kan ta allt från 3 too15 minuter per mobil tjänst som migreras.  Webbplatsen finns kvar under hello migreringen.
Webbplatsen har startats om hello slutet av hello migreringsprocessen.  hello-webbplatsen inte är tillgänglig under hello omstartsprocessen, vilket kan senaste några sekunder.

## <a name="finalizing-migration"></a>Slutför hello migrering
Planera tootest webbplatsen från en mobil klient på hello slutsats om hello migreringsprocessen.  Se till att du kan utföra alla vanliga Klientåtgärder utan ändringar toohello mobila klienten.  

### <a name="update-app-service-tier"></a>Välj en lämplig Apptjänst prisnivån
Har du mer flexibilitet i priser när du har migrerat tooAzure Apptjänst.

1. Logga in toohello [Azure-portalen].
2. Välj **alla resurser** eller **Apptjänster** Klicka hello namnet på din migrerade mobiltjänst.
3. hello inställningsbladet öppnas som standard.
4. Klicka på **Apptjänstplan** i hello inställningsmenyn.
5. Klicka på hello **prisnivån** panelen.
6. Hello panelen lämpliga tooyour krav Klicka **Välj**.  Du kan behöva tooClick **visa alla** toosee hello tillgängliga prisnivåer.

Som en startpunkt rekommenderar vi hello följande nivåer:

| Mobiltjänst prisnivån | Prisnivån för Apptjänst |
|:--- |:--- |
| Kostnadsfri |F1 Kostnadsfri |
| Basic |B1 Basic |
| Standard |S1 Standard |

Det finns stor flexibilitet att välja hello höger prisnivån för ditt program.  Se för[priser för Apptjänst] fullständig information om hello priser av din nya App Service.

> [!TIP]
> hello App Service standardnivån innehåller åtkomst toomany funktioner som du kan ha toouse, inklusive [mellanlagring fack], automatiska säkerhetskopieringar och automatisk skalning.  Ta en titt hello nya funktioner när du är det!
>
>

### <a name="review-migration-scheduler-jobs"></a>Granska hello migreras schemaläggare
Schemaläggare visas inte förrän cirka 30 minuter efter migreringen.  Schemalagda jobb fortsätta toorun i hello bakgrund.
tooview schemalagda jobb när de är synliga igen:

1. Logga in toohello [Azure-portalen].
2. Välj **Bläddra >**, ange **schema** i hello *Filter* och sedan markera **Scheduler samlingar**.

Det finns ett begränsat antal kostnadsfria scheduler jobb tillgängliga efter migreringen.  Granska din användning och hello [Azure Scheduler planer].

### <a name="configure-cors"></a>Konfigurera CORS vid behov
Resursdelning för korsande ursprung är en teknik tooallow webbplats-tooaccess webb-API i en annan domän.  Om du använder Azure Mobile Services med en associerad webbplats måste tooconfigure CORS som en del av hello migrering.  Om du använder Azure Mobile Services uteslutande på mobila enheter, behöver toobe konfigurerats utom i sällsynta fall inte i CORS.

Inställningarna för migrerade CORS är tillgängliga som hello **MS_CrossDomainWhitelist** Appinställningen.  toomigrate din webbplats toohello Apptjänst-CORS-anläggningen:

1. Logga in toohello [Azure-portalen].
2. Välj **alla resurser** eller **Apptjänster** Klicka hello namnet på din migrerade mobiltjänst.
3. hello inställningsbladet öppnas som standard.
4. Klicka på **CORS** hello API-menyn.
5. Ange några tillåtna ursprung i hello rutan, tryck på RETUR efter varje kriterium.
6. När din lista över tillåtna ursprung är korrekt Klicka hello spara.

> [!TIP]
> En av hello fördelarna med att använda en Azure App Service är att du kan köra din webbplats och Mobiltjänst på hello samma plats.  Mer information finns i hello [nästa steg](#next-steps) avsnitt.
>
>

### <a name="download-publish-profile"></a>Hämta en ny publiceringsprofilen
Hej publiceringsprofilen webbplatsens ändras när du migrerar tooAzure Apptjänst.  Om du planerar toopublish webbplatsen från Visual Studio, måste en ny publiceringsprofilen.  toodownload hello nya publiceringsprofilen:

1. Logga in toohello [Azure-portalen].
2. Välj **alla resurser** eller **Apptjänster** Klicka hello namnet på din migrerade mobiltjänst.
3. Klicka på **Get publiceringsprofil**.

Hej PublishSettings-filen är hämtade tooyour dator.  Det kallas vanligtvis *sitename*. PublishSettings.  Importera hello Publiceringsinställningar i projektet:

1. Öppna Visual Studio och Azure Mobile Service-projekt.
2. Högerklicka på projektet i hello **Solution Explorer** och välj **publicera...**
3. Klicka på **Import**
4. Klicka på **Bläddra** och välj din hämtade inställningsfilen för publicering.  Klicka på **OK**
5. Klicka på **Validera anslutningen** tooensure hello publicera inställningarna fungerar.
6. Klicka på **publicera** toopublish din plats.

## <a name="working-with-your-site"></a>Arbeta med din webbplats efter migrering
Börjar arbeta med din nya App Service i hello [Azure-portalen] efter migreringen.  hello följande är några anteckningar på specifika åtgärder som du använt tooperform i hello [klassiska Azure-portalen], tillsammans med motsvarande Apptjänst.

### <a name="publishing-your-site"></a>Ladda ned och publicera webbplatsen migrerade
Webbplatsen är tillgänglig via git och FTP- och kan publiceras med olika olika sätt, inklusive WebDeploy, TFS, ett, GitHub och FTP.  autentiseringsuppgifter för distribution av hello migreras med hello resten av platsen.  Om du inte har angett dina autentiseringsuppgifter för distribution eller om du inte kommer ihåg dem, kan du återställa dem:

1. Logga in toohello [Azure-portalen].
2. Välj **alla resurser** eller **Apptjänster** Klicka hello namnet på din migrerade mobiltjänst.
3. hello inställningsbladet öppnas som standard.
4. Klicka på **distributionsbehörigheterna** i hello publicering menyn.
5. Ange hello nya autentiseringsuppgifter för distribution i rutorna hello och sedan på knappen Spara för hello.

Du kan använda dessa autentiseringsuppgifter tooclone hello plats med git eller konfigurera automatiserad distribution från GitHub, TFS eller ett.  Mer information finns i hello [dokumentationen för Azure App Service].

### <a name="appsettings"></a>Programinställningar
De flesta inställningar för en migrerad Mobiltjänst finns tillgängliga via App-inställningar.  Du kan hämta en lista över hello app-inställningar från hello [Azure-portalen].
tooview eller ändra appinställningar för din:

1. Logga in toohello [Azure-portalen].
2. Välj **alla resurser** eller **Apptjänster** Klicka hello namnet på din migrerade mobiltjänst.
3. hello inställningsbladet öppnas som standard.
4. Klicka på **programinställningar** allmänna hello-menyn.
5. Rulla toohello App-inställningar och hitta appinställning.
6. Klicka på hello värdet för hello app tooedit hello värdet.  Klicka på **spara** toosave hello värde.

Du kan uppdatera flera appinställningar på hello samtidigt.

> [!TIP]
> Det finns två programinställningar med hello samma värde.  Du kan till exempel se *ApplicationKey* och *MS\_ApplicationKey*.  Uppdatera båda programinställningar på hello samtidigt.
>
>

### <a name="authentication"></a>Autentisering
Alla inställningar för autentisering är tillgängliga som App-inställningar på din migrerade plats.  tooupdate autentiseringsinställningarna måste du ändra lämpliga app-inställningar.  hello visar följande tabell hello lämplig app-inställningar för din autentiseringsprovider:

| Leverantör | Klient-ID | Klienthemlighet | Andra inställningar |
|:--- |:--- |:--- |:--- |
| Microsoft-konto |**MS\_MicrosoftClientID** |**MS\_MicrosoftClientSecret** |**MS\_MicrosoftPackageSID** |
| Facebook |**MS\_FacebookAppID** |**MS\_FacebookAppSecret** | |
| Twitter |**MS\_TwitterConsumerKey** |**MS\_TwitterConsumerSecret** | |
| Google |**MS\_GoogleClientID** |**MS\_GoogleClientSecret** | |
| Azure AD |**MS\_AadClientID** | |**MS\_AadTenants** |

Obs: **MS\_AadTenants** lagras som en kommaavgränsad lista över klient domäner (hello ”tillåtna klienter”-fält i hello Mobile Services-portalen).

> [!WARNING]
> **Använd inte hello autentiseringsmekanismer i hello inställningsmenyn**
>
> Azure App Service tillhandahåller ett separat ”ingen kod” autentisering och auktorisering system under hello *autentisering / auktorisering* menyn Inställningar och hello (föråldrad) *Mobile autentisering* alternativet hello inställningarna menyn.  Dessa alternativ är inte kompatibel med en migrerade Azure-mobiltjänst.  Du kan [uppgraderar platsen](app-service-mobile-net-upgrading-from-mobile-services.md) tootake nytta av hello Azure App Service-autentisering.
>
>

### <a name="easytables"></a>Data
Hej *Data* fliken i Mobile Services har ersatts av *enkelt tabeller* inom hello Azure-portalen.  tooaccess enkelt tabeller:

1. Logga in toohello [Azure-portalen].
2. Välj **alla resurser** eller **Apptjänster** Klicka hello namnet på din migrerade mobiltjänst.
3. hello inställningsbladet öppnas som standard.
4. Klicka på **enkelt tabeller** mobila hello-menyn.

Du kan lägga till en tabell genom att klicka på hello **Lägg till** knappen eller få åtkomst till dina befintliga tabeller genom att klicka på ett tabellnamn.  Det finns olika åtgärder som du kan göra från det här bladet, inklusive:

* Ändra tabellbehörigheter
* Redigera hello operativa skript
* Hantera hello tabellens schema
* Om du tar bort hello tabell
* Hello innehållet i en tabell
* Ta bort specifika rader av hello tabell

### <a name="easyapis"></a>API
Hej *API* fliken i Mobile Services har ersatts av *enkelt API: er* inom hello Azure-portalen.  tooaccess enkelt API: er:

1. Logga in toohello [Azure-portalen].
2. Välj **alla resurser** eller **Apptjänster** Klicka hello namnet på din migrerade mobiltjänst.
3. hello inställningsbladet öppnas som standard.
4. Klicka på **enkelt API: er** mobila hello-menyn.

Dina migrerade API: er finns redan i hello-bladet.  Du kan också lägga till en API från det här bladet.  toomanage specifika API, klicka på hello API.
Du kan justera hello behörigheter och redigera hello skript för hello API från hello nya bladet.

### <a name="on-demand-jobs"></a>Jobb för Schemaläggaren
Alla jobb som Schemaläggaren är tillgängliga via hello Scheduler-Jobbsamlingar avsnitt.  tooaccess jobb för Schemaläggaren:

1. Logga in toohello [Azure-portalen].
2. Välj **Bläddra >**, ange **schema** i hello *Filter* och sedan markera **Scheduler samlingar**.
3. Välj hello Jobbsamlingen för webbplatsen.  Det heter *sitename*-jobb.
4. Klicka på **inställningar**.
5. Klicka på **schemaläggare** under hantera.

Schemalagda jobb visas med hello frekvens som du angett innan migreringen.  På begäran-jobben är inaktiverade.  toorun ett jobb på begäran:

1. Välj hello jobb som du vill toorun.
2. Om det behövs klickar du på **aktivera** tooenable hello jobb.
3. Klicka på **inställningar**, sedan **schema**.
4. Välj en upprepning av **när**, klicka på **spara**

Jobb på begäran finns i `App_Data/config/scripts/scheduler post-migration`.  Vi rekommenderar att du konverterar alla jobb som på begäran för[WebJobs] eller [funktioner].  Skriva nya schemaläggare som [WebJobs] eller [funktioner].

### <a name="notification-hubs"></a>Notification Hubs
Mobile Services använder Notification Hubs för push-meddelanden.  hello följande Appinställningar är används toolink hello Notification Hub tooyour Mobiltjänst efter migreringen:

| Tillämpningsinställning | Beskrivning |
|:--- |:--- |
| **MS\_PushEntityNamespace** |hello Namespace för Notification Hub |
| **MS\_NotificationHubName** |hello namn på Meddelandehubb |
| **MS\_NotificationHubConnectionString** |hello anslutningssträngen för Notification Hub |
| **MS\_NamespaceName** |Ett alias för MS_PushEntityNamespace |

Din Meddelandehubb hanteras via hello [Azure-portalen].  Observera hello Notification Hub-namn (du hittar detta med hjälp av hello Appinställningar):

1. Logga in toohello [Azure-portalen].
2. Välj **Bläddra**> och välj **Notification Hubs**
3. Klicka på hello Notification Hub-namn som associeras med hello mobiltjänst.

> [!NOTE]
> Om din Meddelandehubb är av typen ”Mixed”, är den inte synlig.  ”Blandade” Skriv notification hubs använder både Meddelandehubbar och äldre Service Bus-funktioner.  [Omvandla dina blandat namnområden] innan du fortsätter.  När hello konverteringen har slutförts din meddelandehubb som visas i hello [Azure-portalen].
>
>

Mer information finns i hello [Meddelandehubbar] dokumentation.

> [!TIP]
> Notification Hubs-hanteringsfunktioner i hello [Azure-portalen] är fortfarande under förhandsgranskning.  Hej [klassiska Azure-portalen] finns kvar för att hantera alla Meddelandehubbar.
>
>

### <a name="legacy-push"></a>Äldre Push-inställningar
Om du har konfigurerat Push på mobiltjänsten innan hello introduktion på Notification Hubs använder du *äldre push*.  Om du använder Push och du inte ser en Meddelandehubb som visas i konfigurationen och sedan är det troligt att du använder *äldre push*.  Den här funktionen har migrerats med andra funktioner.  Vi rekommenderar dock att du uppgraderar tooNotification hubbar strax efter hello migreringen är klar.

I hello tillfälligt är alla hello äldre push-inställningar (med hello viktiga undantag av hello APN-certifikatet) tillgängliga i App-inställningar.  Uppdatera hello APN-certifikatet genom att ersätta hello lämplig fil på hello filsystem.

### <a name="app-settings"></a>Andra Appinställningar
hello följande ytterligare app-inställningar har migrerats från din Mobiltjänst och är tillgängliga under *inställningar* > *Appinställningar*:

| Tillämpningsinställning | Beskrivning |
|:--- |:--- |
| **MS\_MobileServiceName** |hello namnet på din app |
| **MS\_MobileServiceDomainSuffix** |Hej domänprefixet. engångsfaktorautentisering Azure-mobile.net |
| **MS\_ApplicationKey** |Din nyckel för programmet |
| **MS\_MasterKey** |Huvudnyckeln för din app |

hello tangent och huvudnyckeln är identiska toohello programmet nycklar från din ursprungliga mobiltjänst.  I synnerhet skickas hello tangent av mobila klienter toovalidate användningen av hello mobila API: et.

### <a name="cliequivalents"></a>Kommandoradsverktyget motsvarigheter
Du kan använda hello längre *azure mobila* kommandot toomanage Azure Mobile Services-platsen.  I stället många funktioner har ersatts av hello *azure plats* kommando.  Använd följande tabell toofind motsvarigheter för vanliga kommandon hello:

| *Azure Mobile* kommando | Motsvarande *Azure Site* kommando |
|:--- |:--- |
| mobila platser |plats för webbplatslista |
| mobila lista |platslista |
| mobila visa *namn* |platsen visa *namn* |
| mobila omstart *namn* |platsen omstart *namn* |
| mobila Omdistributionen *namn* |Distribuera om distributionen av site *commitId* *namn* |
| mobila nyckeluppsättning *namn* *typen* *värde* |ta bort platsen appsetting *nyckeln* *namn* <br/> Lägg till webbplats appsetting *nyckeln*=*värdet* *namn* |
| mobila config listan *namn* |appsetting platslista *namn* |
| Hämta mobila config *namn* *nyckel* |platsen appsetting visa *nyckeln* *namn* |
| mobila konfigurationsuppsättning *namn* *nyckel* |ta bort platsen appsetting *nyckeln* *namn* <br/> Lägg till webbplats appsetting *nyckeln*=*värdet* *namn* |
| mobila domänlistan *namn* |domän platslista *namn* |
| lägga till mobila domän *namn* *domän* |Lägg till webbplats domän *domän* *namn* |
| ta bort mobila domän *namn* |ta bort plats-domänen *domän* *namn* |
| Visa mobila skala *namn* |platsen visa *namn* |
| mobila skala ändra *namn* |läget för webbplatsen skala *läge* *namn* <br /> platsen skala instanser *instanser* *namn* |
| mobila appsetting listan *namn* |appsetting platslista *namn* |
| Lägg till mobila appsetting *namn* *nyckeln* *värde* |Lägg till webbplats appsetting *nyckeln*=*värdet* *namn* |
| ta bort mobila appsetting *namn* *nyckel* |ta bort platsen appsetting *nyckeln* *namn* |
| mobila appsetting visa *namn* *nyckel* |ta bort platsen appsetting *nyckeln* *namn* |

Uppdatera autentisering eller push notification inställningar genom att uppdatera hello lämplig inställning för programmet.
Redigera filerna och publicera webbplatsen via FTP- eller git.

### <a name="diagnostics"></a>Diagnostik- och loggning
Diagnostikloggning är normalt inaktiverat i en Azure App Service.  tooenable diagnostikloggning:

1. Logga in toohello [Azure-portalen].
2. Välj **alla resurser** eller **Apptjänster** Klicka hello namnet på din migrerade mobiltjänst.
3. hello inställningsbladet öppnas som standard.
4. Välj **diagnostikloggar** hello funktioner-menyn.
5. Klicka på **ON** för hello följande loggar: **programloggning (filsystem)**, **detaljerade felmeddelanden**, och **spårning av misslyckade begäranden**
6. Klicka på **filsystemet** för Web server-loggning
7. Klicka på **spara**

tooview hello loggar:

1. Logga in toohello [Azure-portalen].
2. Välj **alla resurser** eller **Apptjänster** Klicka hello namnet på din migrerade mobiltjänst.
3. Klicka på hello **verktyg** knappen
4. Välj **loggström** hello Följ-menyn.

Loggarna visas i fönstret hello när de skapas.  Du kan också hämta hello loggar för senare analys med dina autentiseringsuppgifter för distribution. Mer information finns i hello [loggning] dokumentation.

## <a name="known-issues"></a>Kända problem
### <a name="deleting-a-migrated-mobile-app-clone-causes-a-site-outage"></a>Om du tar bort en migreras Mobile App klon orsakar ett avbrott för platsen
Om du klonar migrerade mobiltjänsten med hjälp av Azure PowerShell och sedan ta bort hello klona tas hello DNS-posten för produktionstjänsten bort.  Platsen är inte längre att vara tillgänglig från hello Internet.  

Lösning: Om du vill tooclone din webbplats kan göra det via hello-portalen.

### <a name="changing-webconfig-does-not-work"></a>Om du ändrar Web.config fungerar inte
Om du har en ASP.NET-webbplats kan ändras toohello `Web.config` filen inte tillämpas.  hello Azure App Service skapar ett lämpligt `Web.config` filen körningsfel Start toosupport hello Mobile Services.  Du kan åsidosätta vissa inställningar (t.ex anpassade huvuden) med hjälp av en XML-transformation.  Skapa en fil i kallas `applicationHost.xdt` -filen måste hamna i hello `D:\home\site` på hello Azure-tjänsten.  Överför den `applicationHost.xdt` filen via ett anpassat distributionsskriptet eller direkt via Kudu.  hello nedan visar ett exempel dokument:

```
<?xml version="1.0" encoding="utf-8"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <system.webServer>
    <httpProtocol>
      <customHeaders>
        <add name="X-Frame-Options" value="DENY" xdt:Transform="Replace" />
        <remove name="X-Powered-By" xdt:Transform="Insert" />
      </customHeaders>
    </httpProtocol>
    <security>
      <requestFiltering removeServerHeader="true" xdt:Transform="SetAttributes(removeServerHeader)" />
    </security>
  </system.webServer>
</configuration>
```

Mer information finns i hello [XDT transformera prover] dokumentation på GitHub.

### <a name="migrated-mobile-services-cannot-be-added-tootraffic-manager"></a>Migrerade Mobile Services kan inte läggas till tooTraffic Manager
Du kan inte direkt välja en profil för migrerat Mobiltjänst toohello när du skapar en Traffic Manager-profil.  Använda en ”extern slutpunkt”.  Den externa slutpunkten kan endast läggas till via PowerShell.  Mer information finns i [Traffic Manager kursen](https://azure.microsoft.com/blog/azure-traffic-manager-external-endpoints-and-weighted-round-robin-via-powershell/).

## <a name="next-steps"></a>Nästa steg
Nu när ditt program är migrerade tooApp Service, finns det ytterligare funktioner som du kan använda:

* Distribution [mellanlagring fack] Tillåt toostage ändringar tooyour plats och utföra A / B-testning.
* [WebJobs] ersätta för på-begäran schemalagda jobb.
* Du kan [kontinuerligt distribuera] webbplatsen genom att länka din webbplats tooGitHub TFS eller ett.
* Du kan använda [Programinsikter] toomonitor din plats.
* Fungera som en webbplats och ett Mobile-API från hello samma kod.

### <a name="upgrading-your-site"></a>Uppgradera din Mobile Services-platsen tooAzure Mobile Apps-SDK
* För Node.js-baserad server projekt hello nya [Mobile Apps Node.js SDK] innehåller flera nya funktioner. Exempelvis kan du nu göra lokal utveckling och felsökning, använda en Node.js-version ovan 0.10 och anpassa med någon Express.js mellanprogram.
* För. NET-baserad server-projekt hello nya [Mobile Apps SDK NuGet-paket](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) har du mer flexibilitet i NuGet-beroenden.  Dessa paket stöder hello nya App Service-autentisering och skapa med ASP.NET-projekt. Mer information om hur du uppgraderar finns [uppgradera din befintliga mobiltjänst .NET tooApp tjänsten](app-service-mobile-net-upgrading-from-mobile-services.md).

<!-- Images -->
[0]: ./media/app-service-mobile-migrating-from-mobile-services/migrate-to-app-service-button.PNG
[1]: ./media/app-service-mobile-migrating-from-mobile-services/migration-activity-monitor.png
[2]: ./media/app-service-mobile-migrating-from-mobile-services/triggering-job-with-postman.png

<!-- Links -->
[Priser för Apptjänst]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[Programinsikter]: ../application-insights/app-insights-overview.md
[Autoskala]: ../app-service-web/web-sites-scale.md
[Azure App Service]: ../app-service/app-service-value-prop-what-is.md
[dokumentationen för Azure App Service]: ../app-service-web/web-sites-deploy.md
[klassiska Azure-portalen]: https://manage.windowsazure.com
[Azure-portalen]: https://portal.azure.com
[Azure Region]: https://azure.microsoft.com/en-us/regions/
[Azure Scheduler planer]: ../scheduler/scheduler-plans-billing.md
[kontinuerligt distribuera]: ../app-service-web/app-service-continuous-deployment.md
[Omvandla dina blandat namnområden]: https://azure.microsoft.com/en-us/blog/updates-from-notification-hubs-independent-nuget-installation-model-pmt-and-more/
[curl]: http://curl.haxx.se/
[anpassade domännamn]: ../app-service-web/web-sites-custom-domain-name.md
[Fiddler]: http://www.telerik.com/fiddler
[allmän tillgång till Azure App Service]: https://azure.microsoft.com/blog/announcing-general-availability-of-app-service-mobile-apps/
[Hybridanslutningar]: ../app-service-web/web-sites-hybrid-connection-get-started.md
[loggning]: ../app-service-web/web-sites-enable-diagnostic-log.md
[Mobile Apps Node.js SDK]: https://github.com/azure/azure-mobile-apps-node
[jämfört med Mobile Services. Apptjänst]: app-service-mobile-value-prop-migration-from-mobile-services.md
[Meddelandehubbar]: ../notification-hubs/notification-hubs-push-notification-overview.md
[prestandaövervakning]: ../app-service-web/web-sites-monitor.md
[Postman]: http://www.getpostman.com/
[mellanlagring fack]: ../app-service-web/web-sites-staged-publishing.md
[VNet]: ../app-service-web/web-sites-integrate-with-vnet.md
[WebJobs]: ../app-service-web/websites-webjobs-resources.md
[XDT transformera prover]: https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples
[funktioner]: ../azure-functions/functions-overview.md
