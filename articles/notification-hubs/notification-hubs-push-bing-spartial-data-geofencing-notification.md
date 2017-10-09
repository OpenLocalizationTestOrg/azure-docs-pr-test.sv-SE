---
title: "aaaGeo avgränsade områden push-meddelanden med Azure Notification Hub och Bing Spatial Data | Microsoft Docs"
description: "I kursen får du lära dig hur toodeliver platsbaserade push-meddelanden med Azure Notification Hub och Bing Spatial Data."
services: notification-hubs
documentationcenter: windows
keywords: push-meddelande, pushmeddelande
author: dend
manager: yuaxu
editor: dend
ms.assetid: f41beea1-0d62-4418-9ffc-c9d70607a1b7
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 05/31/2016
ms.author: dendeli
ms.openlocfilehash: d6efe473537f2159240c53e780741f12619c045d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="geo-fenced-push-notifications-with-azure-notification-hubs-and-bing-spatial-data"></a>Push-meddelanden till geografiskt avgränsade områden med Azure Notification Hubs och Bing Spatial Data
> [!NOTE]
> toocomplete den här självstudiekursen kommer du måste ha ett aktivt Azure-konto. Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02).
> 
> 

I kursen får du lära dig hur toodeliver platsbaserade push-meddelanden med Azure Notification Hub och Bing Spatial Data utnyttjas från en Uwp-programmet.

## <a name="prerequisites"></a>Krav
Först och främst, behöver du toomake till att du har alla hello program och förutsättningar:

* [Visual Studio 2015 Update 1](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx) eller senare ([Community Edition](https://go.microsoft.com/fwlink/?LinkId=691978&clcid=0x409) går också bra). 
* Senaste versionen av hello [Azure SDK](https://azure.microsoft.com/downloads/). 
* [Konto på Bing Maps Dev Center](https://www.bingmapsportal.com/) (du kan skapa ett gratis och associera det med ditt Microsoft-konto). 

## <a name="getting-started"></a>Komma igång
Börja med att skapa hello-projekt. Starta ett nytt projekt av typen **tom app (Universal Windows)** i Visual Studio.

![](./media/notification-hubs-geofence/notification-hubs-create-blank-app.png)

Du bör ha hello stomme för själva hello appen när hello projektet har skapats. Nu ska vi ställa in allt för hello geografiska avgränsningar infrastruktur. Eftersom vi kommer toouse Bing services för detta är det en offentlig REST API-slutpunkt som gör att vi tooquery specifika platsramar:

    http://spatial.virtualearth.net/REST/v1/data/

Du behöver toospecify hello följande parametrar tooget detta ska fungera:

* **Datakällans ID** och **Datakällnamn** – I Bing Maps API innehåller datakällorna flera bucketgrupperade metadata, som platser och öppettider. Du kan läsa mer om dessa här. 
* **Entitetsnamnet** – hello entitet som du vill använda toouse som referenspunkt för hello-meddelande. 
* **Bing Maps API-nyckel** – det här är hello-nyckel som du fick tidigare när du skapade hello Bing Dev Center-konto.

Låt oss göra en djupdykning i hello installationen för varje hello elementen ovan.

## <a name="setting-up-hello-data-source"></a>Skapa hello-datakälla
Du kan göra det i hello Bing Maps Dev Center. Klicka på **datakällor** i hello övre navigeringsfältet och välj **Hantera datakällor**.

![](./media/notification-hubs-geofence/bing-maps-manage-data.png)

Om du inte har arbetat med Bing Maps API tidigare troligen det inte några datakällor som finns, så du kan bara skapa en ny genom att klicka på ladda upp tooa datakälla. Kontrollera att du fyller i alla fält för hello som krävs:

![](./media/notification-hubs-geofence/bing-maps-create-data.png)

Du kanske undrar: Vad är hello datafilen och vad ska du ska föra över? Vi kan bara använda hello pipe-baserade exempel som omfattar en del av San Franciscos Hamnområde hello hello enligt det här testet:

    Bing Spatial Data Services, 1.0, TestBoundaries
    EntityID(Edm.String,primaryKey)|Name(Edm.String)|Longitude(Edm.Double)|Latitude(Edm.Double)|Boundary(Edm.Geography)
    1|SanFranciscoPier|||POLYGON ((-122.389825 37.776598,-122.389438 37.773087,-122.381885 37.771849,-122.382186 37.777022,-122.389825 37.776598))

hello ovanstående representerar den här entiteten:

![](./media/notification-hubs-geofence/bing-maps-geofence.png)

Kopiera och klistra in hello ovanstående sträng i en ny fil och sparar den som helt enkelt **NotificationHubsGeofence.pipe**, och överföra den i hello Bing Dev Center.

> [!NOTE]
> Du kanske ange toospecify en ny nyckel för hello **huvudnyckeln** som skiljer sig från hello **Frågenyckeln**. Skapa en ny nyckel via hello instrumentpanelen och uppdatera hello datakälla ladda upp på sidan.
> 
> 

När du överför hello datafilen måste du publicera datakällan hello toomake. 

Gå för**Hantera datakällor**, precis som vi gjorde ovan, hitta datakällan i hello lista och klicka på **publicera** i hello **åtgärder** kolumn. I en stund bör du se din datakälla på hello **publicerade datakällor** fliken:

![](./media/notification-hubs-geofence/bing-maps-published-data.png)

Om du klickar på **redigera**, du kommer att kunna toosee en överblick över vilka platser vi har fört in i den:

![](./media/notification-hubs-geofence/bing-maps-data-details.png)

Nu visas inte hello portal du hello gränser för hello geofence-området som vi skapade – allt vi behöver är en bekräftelse som hello-plats som anges i hello rätt område.

Nu har du alla hello krav för hello-datakälla. tooget hello information om hello URL-begäran för hello API-anrop i hello Bing Maps Dev Center klickar du på **datakällor** och välj **Datakällinformationen**.

![](./media/notification-hubs-geofence/bing-maps-data-info.png)

Hej **fråge-URL** är vad vi letar efter här. Detta är hello slutpunkt mot vilken vi kan köra frågor toocheck om hello enheten är för närvarande i hello platsgränserna eller inte. den här kontrollen behöver vi bara tooexecute GET anropa mot hello fråge-URL med följande parametrar måste läggas till hello tooperform:

    ?spatialFilter=intersects(%27POINT%20LONGITUDE%20LATITUDE)%27)&$format=json&key=QUERY_KEY

På så sätt anger du en målpunkt som vi får från hello enheten och Bing Maps kommer automatiskt att utföra hello beräkningar toosee om det är inom hello geofence-området. När du kör hello begäran via en webbläsare (eller cURL), får du standard JSON-svar:

![](./media/notification-hubs-geofence/bing-maps-json.png)

Detta svar inträffar bara när hello punkten verkligen ligger inom hello avses gränser. Om detta inte är fallet, får du upp en tom **resultat**-bucket:

![](./media/notification-hubs-geofence/bing-maps-nores.png)

## <a name="setting-up-hello-uwp-application"></a>Konfigurera hello UWP-appen
Nu när vi har hello datakällan är redo kan börja vi jobba hello UWP-appen som vi startade om tidigare.

Först och främst måste vi aktivera platstjänster för vår app. toodo detta, dubbelklickar du på `Package.appxmanifest` filen i **Solution Explorer**.

![](./media/notification-hubs-geofence/vs-package-manifest.png)

I hello flik för Paketegenskaper som just öppnades, klickar du på **funktioner** och se till att du väljer **plats**:

![](./media/notification-hubs-geofence/vs-package-location.png)

Eftersom hello Platsegenskapen har angetts, skapa en ny mapp i din lösning som kallas `Core`, och Lägg till en ny fil i den anropade `LocationHelper.cs`:

![](./media/notification-hubs-geofence/vs-location-helper.png)

Hej `LocationHelper` klassen är ganska grundläggande nu – allt den gör är att vi tooobtain hello användarens plats via hello system API:

    using System;
    using System.Threading.Tasks;
    using Windows.Devices.Geolocation;

    namespace NotificationHubs.Geofence.Core
    {
        public class LocationHelper
        {
            private static readonly uint AppDesiredAccuracyInMeters = 10;

            public async static Task<Geoposition> GetCurrentLocation()
            {
                var accessStatus = await Geolocator.RequestAccessAsync();
                switch (accessStatus)
                {
                    case GeolocationAccessStatus.Allowed:
                        {
                            Geolocator geolocator = new Geolocator { DesiredAccuracyInMeters = AppDesiredAccuracyInMeters };

                            return await geolocator.GetGeopositionAsync();
                        }
                    default:
                        {
                            return null;
                        }
                }
            }

        }
    }

Du kan läsa mer om att få hello användarens plats med UWP-appar i hello officiella [MSDN-dokumentet](https://msdn.microsoft.com/library/windows/apps/mt219698.aspx).

toocheck hello platsinsamlingen faktiskt fungerar, öppna hello kod sida huvudsakliga (`MainPage.xaml.cs`). Skapa en ny händelsehanterare för hello `Loaded` händelse i hello `MainPage` konstruktorn:

    public MainPage()
    {
        this.InitializeComponent();
        this.Loaded += MainPage_Loaded;
    }

hello-implementeringen av händelsehanteraren hello är följande:

    private async void MainPage_Loaded(object sender, RoutedEventArgs e)
    {
        var location = await LocationHelper.GetCurrentLocation();

        if (location != null)
        {
            Debug.WriteLine(string.Concat(location.Coordinate.Longitude,
                " ", location.Coordinate.Latitude));
        }
    }

Observera att vi har deklarerat hello hanteraren som asynkron eftersom `GetCurrentLocation` är awaitable och därför kräver toobe som körs i en asynkron kontext. Dessutom eftersom under vissa omständigheter vi kan få en null-plats (t.ex. hello platstjänster är inaktiverade eller programmet hello nekades behörigheter tooaccess plats), behöver vi toomake till att den hanteras korrekt med en null-kontroll.

Kör hello program. Kontrollera att du tillåter platsåtkomst:

![](./media/notification-hubs-geofence/notification-hubs-location-access.png)

Hej en gång appen startas, bör du kunna toosee hello koordinater i hello **utdata** fönster:

![](./media/notification-hubs-geofence/notification-hubs-location-output.png)

Nu vet du att platsförvärvet fungerar anser ledigt tooremove hello test händelsehanteraren för eftersom vi inte använda den längre.

hello nästa steg är toocapture ändras. För att göra detta går vi tillbaka toohello `LocationHelper` klassen och Lägg till hello händelsehanteraren för `PositionChanged`:

    geolocator.PositionChanged += Geolocator_PositionChanged;

hello implementeringen visar platskoordinaterna hello i hello **utdata** fönster:

    private static async void Geolocator_PositionChanged(Geolocator sender, PositionChangedEventArgs args)
    {
        await CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
        {
            Debug.WriteLine(string.Concat(args.Position.Coordinate.Longitude, " ", args.Position.Coordinate.Latitude));
        });
    }

## <a name="setting-up-hello-backend"></a>Konfigurera hello backend
Hämta hello [exempel för .NET-serverdel från GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers). När hello nedladdningen är klar öppnar du hello `NotifyUsers` mappen och därefter hello `NotifyUsers.sln` fil.

Ange hello `AppBackend` projekt som hello **Startprojekt** och starta den.

![](./media/notification-hubs-geofence/vs-startup-project.png)

hello projektet är redan konfigurerade toosend push-meddelanden tootarget enheter, så vi behöver toodo bara två saker – byta hello rätt anslutningssträng för hello meddelandehubben och lägga till gräns identifiering toosend hello meddelande endast när hello användaren är inom hello geofence-området.

tooconfigure hello anslutningssträngen i hello `Models` öppna mappen `Notifications.cs`. Hej `NotificationHubClient.CreateClientFromConnectionString` funktionen ska innehålla hello information om meddelandehubben som du kan hämta i hello [Azure Portal](https://portal.azure.com) (titta på hello **åtkomstprinciper** bladet i  **Inställningar för**). Spara hello uppdaterade konfigurationsfilen.

Nu måste vi toocreate en modell för hello Bing Maps API-resultatet. Hej enklaste sättet toodo som är Högerklicka på hello `Models` mappen **Lägg till** > **klassen**. Ge den namnet `GeofenceBoundary.cs`. När du har gjort, kopiera hello JSON från hello API-svar som vi pratade hello första avsnittet och i Visual Studio använder **redigera** > **klistra in Special** > **klistra in JSON som klasser**. 

På så sätt vi se till att objektet hello ska avserialiseras exakt som det är tänkt. Du bör nu ha en klassuppsättning som liknar följande:

    namespace AppBackend.Models
    {
        public class Rootobject
        {
            public D d { get; set; }
        }

        public class D
        {
            public string __copyright { get; set; }
            public Result[] results { get; set; }
        }

        public class Result
        {
            public __Metadata __metadata { get; set; }
            public string EntityID { get; set; }
            public string Name { get; set; }
            public float Longitude { get; set; }
            public float Latitude { get; set; }
            public string Boundary { get; set; }
            public string Confidence { get; set; }
            public string Locality { get; set; }
            public string AddressLine { get; set; }
            public string AdminDistrict { get; set; }
            public string CountryRegion { get; set; }
            public string PostalCode { get; set; }
        }

        public class __Metadata
        {
            public string uri { get; set; }
        }
    }

Därefter öppnar du `Controllers` > `NotificationsController.cs`. Vi behöver tootweak hello efter anropet tooaccount för hello mållongituden och mållatituden i beräkningen. För att helt enkelt lägga till två strängar toohello funktionssignatur – `latitude` och `longitude`.

    public async Task<HttpResponseMessage> Post(string pns, [FromBody]string message, string to_tag, string latitude, string longitude)

Skapa en ny klass i projektet med namnet hello `ApiHelper.cs` – vi använder den tooconnect tooBing toocheck punkt punktgränser. Gör så här för att implementera en `IsPointWithinBounds`-funktion:

    public class ApiHelper
    {
        public static readonly string ApiEndpoint = "{YOUR_QUERY_ENDPOINT}?spatialFilter=intersects(%27POINT%20({0}%20{1})%27)&$format=json&key={2}";
        public static readonly string ApiKey = "{YOUR_API_KEY}";

        public static bool IsPointWithinBounds(string longitude,string latitude)
        {
            var json = new WebClient().DownloadString(string.Format(ApiEndpoint, longitude, latitude, ApiKey));
            var result = JsonConvert.DeserializeObject<Rootobject>(json);
            if (result.d.results != null && result.d.results.Count() > 0)
            {
                return true;
            }
            else
            {
                return false;
            }
        }
    }

> [!NOTE]
> Se till att toosubstitute hello API-slutpunkt med hello fråge-URL som du tidigare hämtade från hello Bing Dev Center (det samma gäller toohello API-nyckel). 
> 
> 

Om resultaten toohello frågan som innebär att hello angivna punkten ligger inom hello gränserna för geofence-området hello, därför returnerar vi `true`. Om det finns inga resultat, Bing tala om för oss att hello punkten ligger utanför hello uppslagsramen, därför returnerar vi `false`.

Tillbaka i `NotificationsController.cs`, skapar du en markering precis före switch-instruktionen hello:

    if (ApiHelper.IsPointWithinBounds(longitude, latitude))
    {
        switch (pns.ToLower())
        {
            case "wns":
                //// Windows 8.1 / Windows Phone 8.1
                var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
                            "From " + user + ": " + message + "</text></binding></visual></toast>";
                outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

                // Windows 10 specific Action Center support
                toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
                            "From " + user + ": " + message + "</text></binding></visual></toast>";
                outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

                break;
        }
    }

På så sätt kan hello-meddelande skickas endast när hello punkten befinner sig inom hello gränser.

## <a name="testing-push-notifications-in-hello-uwp-app"></a>Testa push-meddelanden i hello UWP-appen
Gå tillbaka toohello UWP-appen, bör vi nu kunna tootest meddelanden. Inom hello `LocationHelper` klassen, skapa en ny funktion – `SendLocationToBackend`:

    public static async Task SendLocationToBackend(string pns, string userTag, string message, string latitude, string longitude)
    {
        var POST_URL = "http://localhost:8741/api/notifications?pns=" +
            pns + "&to_tag=" + userTag + "&latitude=" + latitude + "&longitude=" + longitude;

        using (var httpClient = new HttpClient())
        {
            try
            {
                await httpClient.PostAsync(POST_URL, new StringContent("\"" + message + "\"",
                    System.Text.Encoding.UTF8, "application/json"));
            }
            catch (Exception ex)
            {
                Debug.WriteLine(ex.Message);
            }
        }
    }

> [!NOTE]
> Växla hello `POST_URL` toohello platsen för din distribuerade webbapp som vi skapade i föregående avsnitt i hello. Nu är det OK toorun det lokalt, men när du arbetar med att distribuera en offentlig version, måste toohost den med en extern leverantör.
> 
> 

Vi ska nu se till att vi registrerar hello UWP-appen för push-meddelanden. I Visual Studio klickar du på **projekt** > **lagra** > **associera appen med butiken hello**.

![](./media/notification-hubs-geofence/vs-associate-with-store.png)

När du loggar in tooyour utvecklarkonto, kontrollera att du väljer en befintlig app eller skapa en ny och associerar hello paketet till den. 

Gå toohello Dev Center och öppna hello-app som du nyss skapade. Klicka på **Tjänster** > **Push-meddelanden** > **Webbplatsen för Live-tjänster** .

![](./media/notification-hubs-geofence/ms-live-services.png)

På hello platsen anteckna hello **Programhemlighet** och hello **paket-SID**. Behöver du både i hello Azure-portalen – öppna din meddelandehubb, klicka på **inställningar** > **Notification Services** > **Windows (WNS)**och ange hello information i hello obligatoriskt fält.

![](./media/notification-hubs-geofence/notification-hubs-wns.png)

Klicka på **Spara**.

Högerklicka på **Referenser** i **Solution Explorer** och välj **Hantera NuGet-paket**. Vi behöver tooadd en referens toohello **Microsoft Azure Service Bus hanterade biblioteket** – Sök bara efter `WindowsAzure.Messaging.Managed` och lägga till den tooyour projekt.

![](./media/notification-hubs-geofence/vs-nuget.png)

I testsyfte kan vi skapa hello `MainPage_Loaded` händelsehanteraren igen och Lägg till den här koden fragment tooit:

    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    var hub = new NotificationHub("HUB_NAME", "HUB_LISTEN_CONNECTION_STRING");
    var result = await hub.RegisterNativeAsync(channel.Uri);

    // Displays hello registration ID so you know it was successful
    if (result.RegistrationId != null)
    {
        Debug.WriteLine("Reg successful.");
    }

hello ovan registrerar hello app med hello notification hub. Du är klar toogo! 

I `LocationHelper`, inuti hello `Geolocator_PositionChanged` hanteraren, du kan lägga till en del av en testkod som med tvång placerar hello platsen inom geofence-området för hello:

    await LocationHelper.SendLocationToBackend("wns", "TEST_USER", "TEST", "37.7746", "-122.3858");

Eftersom vi inte skickar hello faktiska koordinaterna (som inte kanske inom gränserna för hello hello tillfället) och använder fördefinierade testvärden, kommer vi att se ett meddelande som visas i uppdateringen:

![](./media/notification-hubs-geofence/notification-hubs-test-notification.png)

## <a name="whats-next"></a>Nästa steg
Det finns några steg som du kan behöva toofollow i tillägget toohello ovan toomake att hello lösningen är klar för produktion.

Först och främst kan behöva du tooensure att geofence-områdena är dynamiska. Detta kräver lite extraarbete med hello Bing API i ordning toobe kan tooupload nya gränser inom hello befintlig datakälla. Kontakta hello [Bing Spatial Data Services API-dokumentationen](https://msdn.microsoft.com/library/ff701734.aspx) för mer information om hello ämne.

Andra, eftersom du arbeta tooensure hello leveransen görs toohello rätt deltagare kanske du vill tootarget dem via [taggning](notification-hubs-tags-segment-push-message.md).

hello lösningen ovan beskriver ett scenario där du kan ha en mängd olika målplattformar, så inte begränsar vi hello geofencing toosystem-specifika funktioner. Namn, hello Uwp har funktioner för[detektera geofence rätt out-of-the-box](https://msdn.microsoft.com/windows/uwp/maps-and-location/set-up-a-geofence).

För mer information om funktioner i Notification Hubs, se vår [dokumentationsportal](https://azure.microsoft.com/documentation/services/notification-hubs/).

