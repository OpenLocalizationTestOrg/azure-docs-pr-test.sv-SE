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
# <a name="geo-fenced-push-notifications-with-azure-notification-hubs-and-bing-spatial-data"></a><span data-ttu-id="c4a43-104">Push-meddelanden till geografiskt avgränsade områden med Azure Notification Hubs och Bing Spatial Data</span><span class="sxs-lookup"><span data-stu-id="c4a43-104">Geo-fenced push notifications with Azure Notification Hubs and Bing Spatial Data</span></span>
> [!NOTE]
> <span data-ttu-id="c4a43-105">toocomplete den här självstudiekursen kommer du måste ha ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="c4a43-105">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="c4a43-106">Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="c4a43-106">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="c4a43-107">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02).</span><span class="sxs-lookup"><span data-stu-id="c4a43-107">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02).</span></span>
> 
> 

<span data-ttu-id="c4a43-108">I kursen får du lära dig hur toodeliver platsbaserade push-meddelanden med Azure Notification Hub och Bing Spatial Data utnyttjas från en Uwp-programmet.</span><span class="sxs-lookup"><span data-stu-id="c4a43-108">In this tutorial, you will learn how toodeliver location-based push notifications with Azure Notification Hubs and Bing Spatial Data, leveraged from within a Universal Windows Platform application.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c4a43-109">Krav</span><span class="sxs-lookup"><span data-stu-id="c4a43-109">Prerequisites</span></span>
<span data-ttu-id="c4a43-110">Först och främst, behöver du toomake till att du har alla hello program och förutsättningar:</span><span class="sxs-lookup"><span data-stu-id="c4a43-110">First and foremost, you need toomake sure that you have all hello software and service pre-requisites:</span></span>

* <span data-ttu-id="c4a43-111">[Visual Studio 2015 Update 1](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx) eller senare ([Community Edition](https://go.microsoft.com/fwlink/?LinkId=691978&clcid=0x409) går också bra).</span><span class="sxs-lookup"><span data-stu-id="c4a43-111">[Visual Studio 2015 Update 1](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx) or later ([Community Edition](https://go.microsoft.com/fwlink/?LinkId=691978&clcid=0x409) will do as well).</span></span> 
* <span data-ttu-id="c4a43-112">Senaste versionen av hello [Azure SDK](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="c4a43-112">Latest version of hello [Azure SDK](https://azure.microsoft.com/downloads/).</span></span> 
* <span data-ttu-id="c4a43-113">[Konto på Bing Maps Dev Center](https://www.bingmapsportal.com/) (du kan skapa ett gratis och associera det med ditt Microsoft-konto).</span><span class="sxs-lookup"><span data-stu-id="c4a43-113">[Bing Maps Dev Center account](https://www.bingmapsportal.com/) (you can create one for free and associate it with your Microsoft account).</span></span> 

## <a name="getting-started"></a><span data-ttu-id="c4a43-114">Komma igång</span><span class="sxs-lookup"><span data-stu-id="c4a43-114">Getting Started</span></span>
<span data-ttu-id="c4a43-115">Börja med att skapa hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="c4a43-115">Let’s start by creating hello project.</span></span> <span data-ttu-id="c4a43-116">Starta ett nytt projekt av typen **tom app (Universal Windows)** i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c4a43-116">In Visual Studio, start a new project of type **Blank App (Universal Windows)**.</span></span>

![](./media/notification-hubs-geofence/notification-hubs-create-blank-app.png)

<span data-ttu-id="c4a43-117">Du bör ha hello stomme för själva hello appen när hello projektet har skapats.</span><span class="sxs-lookup"><span data-stu-id="c4a43-117">Once hello project creation is complete, you should have hello harness for hello app itself.</span></span> <span data-ttu-id="c4a43-118">Nu ska vi ställa in allt för hello geografiska avgränsningar infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="c4a43-118">Now let’s set up everything for hello geo-fencing infrastructure.</span></span> <span data-ttu-id="c4a43-119">Eftersom vi kommer toouse Bing services för detta är det en offentlig REST API-slutpunkt som gör att vi tooquery specifika platsramar:</span><span class="sxs-lookup"><span data-stu-id="c4a43-119">Because we are going toouse Bing services for this, there is a public REST API endpoint that allows us tooquery specific location frames:</span></span>

    http://spatial.virtualearth.net/REST/v1/data/

<span data-ttu-id="c4a43-120">Du behöver toospecify hello följande parametrar tooget detta ska fungera:</span><span class="sxs-lookup"><span data-stu-id="c4a43-120">You will need toospecify hello following parameters tooget it working:</span></span>

* <span data-ttu-id="c4a43-121">**Datakällans ID** och **Datakällnamn** – I Bing Maps API innehåller datakällorna flera bucketgrupperade metadata, som platser och öppettider.</span><span class="sxs-lookup"><span data-stu-id="c4a43-121">**Data Source ID** and **Data Source Name** – in Bing Maps API, data sources contain various bucketed metadata, such as locations and business hours of operation.</span></span> <span data-ttu-id="c4a43-122">Du kan läsa mer om dessa här.</span><span class="sxs-lookup"><span data-stu-id="c4a43-122">You can read more about those here.</span></span> 
* <span data-ttu-id="c4a43-123">**Entitetsnamnet** – hello entitet som du vill använda toouse som referenspunkt för hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="c4a43-123">**Entity Name** – hello entity you want toouse as a reference point for hello notification.</span></span> 
* <span data-ttu-id="c4a43-124">**Bing Maps API-nyckel** – det här är hello-nyckel som du fick tidigare när du skapade hello Bing Dev Center-konto.</span><span class="sxs-lookup"><span data-stu-id="c4a43-124">**Bing Maps API Key** – this is hello key that you obtained earlier when you created hello Bing Dev Center account.</span></span>

<span data-ttu-id="c4a43-125">Låt oss göra en djupdykning i hello installationen för varje hello elementen ovan.</span><span class="sxs-lookup"><span data-stu-id="c4a43-125">Let’s do a deep-dive on hello setup for each of hello elements above.</span></span>

## <a name="setting-up-hello-data-source"></a><span data-ttu-id="c4a43-126">Skapa hello-datakälla</span><span class="sxs-lookup"><span data-stu-id="c4a43-126">Setting up hello data source</span></span>
<span data-ttu-id="c4a43-127">Du kan göra det i hello Bing Maps Dev Center.</span><span class="sxs-lookup"><span data-stu-id="c4a43-127">You can do it in hello Bing Maps Dev Center.</span></span> <span data-ttu-id="c4a43-128">Klicka på **datakällor** i hello övre navigeringsfältet och välj **Hantera datakällor**.</span><span class="sxs-lookup"><span data-stu-id="c4a43-128">Simply click on **Data sources** in hello top navigation bar and select **Manage Data Sources**.</span></span>

![](./media/notification-hubs-geofence/bing-maps-manage-data.png)

<span data-ttu-id="c4a43-129">Om du inte har arbetat med Bing Maps API tidigare troligen det inte några datakällor som finns, så du kan bara skapa en ny genom att klicka på ladda upp tooa datakälla.</span><span class="sxs-lookup"><span data-stu-id="c4a43-129">If you have not worked with Bing Maps API before, most likely there won’t be any data sources present, so you can just create a new one by clicking on Upload data tooa data source.</span></span> <span data-ttu-id="c4a43-130">Kontrollera att du fyller i alla fält för hello som krävs:</span><span class="sxs-lookup"><span data-stu-id="c4a43-130">Make sure you fill out all hello required fields:</span></span>

![](./media/notification-hubs-geofence/bing-maps-create-data.png)

<span data-ttu-id="c4a43-131">Du kanske undrar: Vad är hello datafilen och vad ska du ska föra över?</span><span class="sxs-lookup"><span data-stu-id="c4a43-131">You might be wondering – what is hello data file and what should you be uploading?</span></span> <span data-ttu-id="c4a43-132">Vi kan bara använda hello pipe-baserade exempel som omfattar en del av San Franciscos Hamnområde hello hello enligt det här testet:</span><span class="sxs-lookup"><span data-stu-id="c4a43-132">For hello purposes of this test, we can just use hello sample pipe-based that frames an area of hello San Francisco waterfront:</span></span>

    Bing Spatial Data Services, 1.0, TestBoundaries
    EntityID(Edm.String,primaryKey)|Name(Edm.String)|Longitude(Edm.Double)|Latitude(Edm.Double)|Boundary(Edm.Geography)
    1|SanFranciscoPier|||POLYGON ((-122.389825 37.776598,-122.389438 37.773087,-122.381885 37.771849,-122.382186 37.777022,-122.389825 37.776598))

<span data-ttu-id="c4a43-133">hello ovanstående representerar den här entiteten:</span><span class="sxs-lookup"><span data-stu-id="c4a43-133">hello above represents this entity:</span></span>

![](./media/notification-hubs-geofence/bing-maps-geofence.png)

<span data-ttu-id="c4a43-134">Kopiera och klistra in hello ovanstående sträng i en ny fil och sparar den som helt enkelt **NotificationHubsGeofence.pipe**, och överföra den i hello Bing Dev Center.</span><span class="sxs-lookup"><span data-stu-id="c4a43-134">Simply copy and paste hello string above into a new file and save it as **NotificationHubsGeofence.pipe**, and upload it in hello Bing Dev Center.</span></span>

> [!NOTE]
> <span data-ttu-id="c4a43-135">Du kanske ange toospecify en ny nyckel för hello **huvudnyckeln** som skiljer sig från hello **Frågenyckeln**.</span><span class="sxs-lookup"><span data-stu-id="c4a43-135">You might be prompted toospecify a new key for hello **Master Key** that is different from hello **Query Key**.</span></span> <span data-ttu-id="c4a43-136">Skapa en ny nyckel via hello instrumentpanelen och uppdatera hello datakälla ladda upp på sidan.</span><span class="sxs-lookup"><span data-stu-id="c4a43-136">Simply create a new key through hello dashboard and refresh hello data source upload page.</span></span>
> 
> 

<span data-ttu-id="c4a43-137">När du överför hello datafilen måste du publicera datakällan hello toomake.</span><span class="sxs-lookup"><span data-stu-id="c4a43-137">Once you upload hello data file, you will need toomake sure that you publish hello data source.</span></span> 

<span data-ttu-id="c4a43-138">Gå för**Hantera datakällor**, precis som vi gjorde ovan, hitta datakällan i hello lista och klicka på **publicera** i hello **åtgärder** kolumn.</span><span class="sxs-lookup"><span data-stu-id="c4a43-138">Go too**Manage Data Sources**, just like we did above, find your data source in hello list and click on **Publish** in hello **Actions** column.</span></span> <span data-ttu-id="c4a43-139">I en stund bör du se din datakälla på hello **publicerade datakällor** fliken:</span><span class="sxs-lookup"><span data-stu-id="c4a43-139">In a bit, you should see your data source in hello **Published Data Sources** tab:</span></span>

![](./media/notification-hubs-geofence/bing-maps-published-data.png)

<span data-ttu-id="c4a43-140">Om du klickar på **redigera**, du kommer att kunna toosee en överblick över vilka platser vi har fört in i den:</span><span class="sxs-lookup"><span data-stu-id="c4a43-140">If you click **Edit**, you will be able toosee at a glance what locations we introduced in it:</span></span>

![](./media/notification-hubs-geofence/bing-maps-data-details.png)

<span data-ttu-id="c4a43-141">Nu visas inte hello portal du hello gränser för hello geofence-området som vi skapade – allt vi behöver är en bekräftelse som hello-plats som anges i hello rätt område.</span><span class="sxs-lookup"><span data-stu-id="c4a43-141">At this point, hello portal does not show you hello boundaries for hello geofence that we created – all we need is a confirmation that hello location specified is in hello right vicinity.</span></span>

<span data-ttu-id="c4a43-142">Nu har du alla hello krav för hello-datakälla.</span><span class="sxs-lookup"><span data-stu-id="c4a43-142">Now you have all hello requirements for hello data source.</span></span> <span data-ttu-id="c4a43-143">tooget hello information om hello URL-begäran för hello API-anrop i hello Bing Maps Dev Center klickar du på **datakällor** och välj **Datakällinformationen**.</span><span class="sxs-lookup"><span data-stu-id="c4a43-143">tooget hello details on hello request URL for hello API call, in hello Bing Maps Dev Center, click **Data sources** and select **Data Source Information**.</span></span>

![](./media/notification-hubs-geofence/bing-maps-data-info.png)

<span data-ttu-id="c4a43-144">Hej **fråge-URL** är vad vi letar efter här.</span><span class="sxs-lookup"><span data-stu-id="c4a43-144">hello **Query URL** is what we’re after here.</span></span> <span data-ttu-id="c4a43-145">Detta är hello slutpunkt mot vilken vi kan köra frågor toocheck om hello enheten är för närvarande i hello platsgränserna eller inte.</span><span class="sxs-lookup"><span data-stu-id="c4a43-145">This is hello endpoint against which we can execute queries toocheck whether hello device is currently within hello boundaries of a location or not.</span></span> <span data-ttu-id="c4a43-146">den här kontrollen behöver vi bara tooexecute GET anropa mot hello fråge-URL med följande parametrar måste läggas till hello tooperform:</span><span class="sxs-lookup"><span data-stu-id="c4a43-146">tooperform this check, we simply need tooexecute a GET call against hello query URL, with hello following parameters appended:</span></span>

    ?spatialFilter=intersects(%27POINT%20LONGITUDE%20LATITUDE)%27)&$format=json&key=QUERY_KEY

<span data-ttu-id="c4a43-147">På så sätt anger du en målpunkt som vi får från hello enheten och Bing Maps kommer automatiskt att utföra hello beräkningar toosee om det är inom hello geofence-området.</span><span class="sxs-lookup"><span data-stu-id="c4a43-147">That way you're specifying a target point that we obtain from hello device and Bing Maps will automatically perform hello calculations toosee whether it is within hello geofence.</span></span> <span data-ttu-id="c4a43-148">När du kör hello begäran via en webbläsare (eller cURL), får du standard JSON-svar:</span><span class="sxs-lookup"><span data-stu-id="c4a43-148">Once you execute hello request through a browser (or cURL), you will get standard a JSON response:</span></span>

![](./media/notification-hubs-geofence/bing-maps-json.png)

<span data-ttu-id="c4a43-149">Detta svar inträffar bara när hello punkten verkligen ligger inom hello avses gränser.</span><span class="sxs-lookup"><span data-stu-id="c4a43-149">This response only happens when hello point is actually within hello designated boundaries.</span></span> <span data-ttu-id="c4a43-150">Om detta inte är fallet, får du upp en tom **resultat**-bucket:</span><span class="sxs-lookup"><span data-stu-id="c4a43-150">If it is not, you will get an empty **results** bucket:</span></span>

![](./media/notification-hubs-geofence/bing-maps-nores.png)

## <a name="setting-up-hello-uwp-application"></a><span data-ttu-id="c4a43-151">Konfigurera hello UWP-appen</span><span class="sxs-lookup"><span data-stu-id="c4a43-151">Setting up hello UWP application</span></span>
<span data-ttu-id="c4a43-152">Nu när vi har hello datakällan är redo kan börja vi jobba hello UWP-appen som vi startade om tidigare.</span><span class="sxs-lookup"><span data-stu-id="c4a43-152">Now that we have hello data source ready, we can start working on hello UWP application that we bootstrapped earlier.</span></span>

<span data-ttu-id="c4a43-153">Först och främst måste vi aktivera platstjänster för vår app.</span><span class="sxs-lookup"><span data-stu-id="c4a43-153">First and foremost, we must enable location services for our application.</span></span> <span data-ttu-id="c4a43-154">toodo detta, dubbelklickar du på `Package.appxmanifest` filen i **Solution Explorer**.</span><span class="sxs-lookup"><span data-stu-id="c4a43-154">toodo this, double-click on `Package.appxmanifest` file in **Solution Explorer**.</span></span>

![](./media/notification-hubs-geofence/vs-package-manifest.png)

<span data-ttu-id="c4a43-155">I hello flik för Paketegenskaper som just öppnades, klickar du på **funktioner** och se till att du väljer **plats**:</span><span class="sxs-lookup"><span data-stu-id="c4a43-155">In hello package properties tab that just opened, click on **Capabilities** and make sure that you select **Location**:</span></span>

![](./media/notification-hubs-geofence/vs-package-location.png)

<span data-ttu-id="c4a43-156">Eftersom hello Platsegenskapen har angetts, skapa en ny mapp i din lösning som kallas `Core`, och Lägg till en ny fil i den anropade `LocationHelper.cs`:</span><span class="sxs-lookup"><span data-stu-id="c4a43-156">As hello location capability is declared, create a new folder in your solution named `Core`, and add a new file within it, called `LocationHelper.cs`:</span></span>

![](./media/notification-hubs-geofence/vs-location-helper.png)

<span data-ttu-id="c4a43-157">Hej `LocationHelper` klassen är ganska grundläggande nu – allt den gör är att vi tooobtain hello användarens plats via hello system API:</span><span class="sxs-lookup"><span data-stu-id="c4a43-157">hello `LocationHelper` class itself is fairly basic at this point – all it does is allow us tooobtain hello user location through hello system API:</span></span>

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

<span data-ttu-id="c4a43-158">Du kan läsa mer om att få hello användarens plats med UWP-appar i hello officiella [MSDN-dokumentet](https://msdn.microsoft.com/library/windows/apps/mt219698.aspx).</span><span class="sxs-lookup"><span data-stu-id="c4a43-158">You can read more about getting hello user’s location in UWP apps in hello official [MSDN document](https://msdn.microsoft.com/library/windows/apps/mt219698.aspx).</span></span>

<span data-ttu-id="c4a43-159">toocheck hello platsinsamlingen faktiskt fungerar, öppna hello kod sida huvudsakliga (`MainPage.xaml.cs`).</span><span class="sxs-lookup"><span data-stu-id="c4a43-159">toocheck that hello location acquisition is actually working, open hello code side of your main page (`MainPage.xaml.cs`).</span></span> <span data-ttu-id="c4a43-160">Skapa en ny händelsehanterare för hello `Loaded` händelse i hello `MainPage` konstruktorn:</span><span class="sxs-lookup"><span data-stu-id="c4a43-160">Create a new event handler for hello `Loaded` event in hello `MainPage` constructor:</span></span>

    public MainPage()
    {
        this.InitializeComponent();
        this.Loaded += MainPage_Loaded;
    }

<span data-ttu-id="c4a43-161">hello-implementeringen av händelsehanteraren hello är följande:</span><span class="sxs-lookup"><span data-stu-id="c4a43-161">hello implementation of hello event handler is as follows:</span></span>

    private async void MainPage_Loaded(object sender, RoutedEventArgs e)
    {
        var location = await LocationHelper.GetCurrentLocation();

        if (location != null)
        {
            Debug.WriteLine(string.Concat(location.Coordinate.Longitude,
                " ", location.Coordinate.Latitude));
        }
    }

<span data-ttu-id="c4a43-162">Observera att vi har deklarerat hello hanteraren som asynkron eftersom `GetCurrentLocation` är awaitable och därför kräver toobe som körs i en asynkron kontext.</span><span class="sxs-lookup"><span data-stu-id="c4a43-162">Notice that we declared hello handler as async because `GetCurrentLocation` is awaitable, and therefore requires toobe executed in an async context.</span></span> <span data-ttu-id="c4a43-163">Dessutom eftersom under vissa omständigheter vi kan få en null-plats (t.ex. hello platstjänster är inaktiverade eller programmet hello nekades behörigheter tooaccess plats), behöver vi toomake till att den hanteras korrekt med en null-kontroll.</span><span class="sxs-lookup"><span data-stu-id="c4a43-163">Also, because under certain circumstances we might end up with a null location (e.g. hello location services are disabled or hello application was denied permissions tooaccess location), we need toomake sure that it is properly handled with a null check.</span></span>

<span data-ttu-id="c4a43-164">Kör hello program.</span><span class="sxs-lookup"><span data-stu-id="c4a43-164">Run hello application.</span></span> <span data-ttu-id="c4a43-165">Kontrollera att du tillåter platsåtkomst:</span><span class="sxs-lookup"><span data-stu-id="c4a43-165">Make sure you allow location access:</span></span>

![](./media/notification-hubs-geofence/notification-hubs-location-access.png)

<span data-ttu-id="c4a43-166">Hej en gång appen startas, bör du kunna toosee hello koordinater i hello **utdata** fönster:</span><span class="sxs-lookup"><span data-stu-id="c4a43-166">Once hello application launches, you should be able toosee hello coordinates in hello **Output** window:</span></span>

![](./media/notification-hubs-geofence/notification-hubs-location-output.png)

<span data-ttu-id="c4a43-167">Nu vet du att platsförvärvet fungerar anser ledigt tooremove hello test händelsehanteraren för eftersom vi inte använda den längre.</span><span class="sxs-lookup"><span data-stu-id="c4a43-167">Now you know that location acquisition works – feel free tooremove hello test event handler for Loaded because we won’t be using it anymore.</span></span>

<span data-ttu-id="c4a43-168">hello nästa steg är toocapture ändras.</span><span class="sxs-lookup"><span data-stu-id="c4a43-168">hello next step is toocapture location changes.</span></span> <span data-ttu-id="c4a43-169">För att göra detta går vi tillbaka toohello `LocationHelper` klassen och Lägg till hello händelsehanteraren för `PositionChanged`:</span><span class="sxs-lookup"><span data-stu-id="c4a43-169">For that, let’s go back toohello `LocationHelper` class and add hello event handler for `PositionChanged`:</span></span>

    geolocator.PositionChanged += Geolocator_PositionChanged;

<span data-ttu-id="c4a43-170">hello implementeringen visar platskoordinaterna hello i hello **utdata** fönster:</span><span class="sxs-lookup"><span data-stu-id="c4a43-170">hello implementation will show hello location coordinates in hello **Output** window:</span></span>

    private static async void Geolocator_PositionChanged(Geolocator sender, PositionChangedEventArgs args)
    {
        await CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
        {
            Debug.WriteLine(string.Concat(args.Position.Coordinate.Longitude, " ", args.Position.Coordinate.Latitude));
        });
    }

## <a name="setting-up-hello-backend"></a><span data-ttu-id="c4a43-171">Konfigurera hello backend</span><span class="sxs-lookup"><span data-stu-id="c4a43-171">Setting up hello backend</span></span>
<span data-ttu-id="c4a43-172">Hämta hello [exempel för .NET-serverdel från GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers).</span><span class="sxs-lookup"><span data-stu-id="c4a43-172">Download hello [.NET Backend Sample from GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers).</span></span> <span data-ttu-id="c4a43-173">När hello nedladdningen är klar öppnar du hello `NotifyUsers` mappen och därefter hello `NotifyUsers.sln` fil.</span><span class="sxs-lookup"><span data-stu-id="c4a43-173">Once hello download completes, open hello `NotifyUsers` folder, and subsequently – hello `NotifyUsers.sln` file.</span></span>

<span data-ttu-id="c4a43-174">Ange hello `AppBackend` projekt som hello **Startprojekt** och starta den.</span><span class="sxs-lookup"><span data-stu-id="c4a43-174">Set hello `AppBackend` project as hello **StartUp Project** and launch it.</span></span>

![](./media/notification-hubs-geofence/vs-startup-project.png)

<span data-ttu-id="c4a43-175">hello projektet är redan konfigurerade toosend push-meddelanden tootarget enheter, så vi behöver toodo bara två saker – byta hello rätt anslutningssträng för hello meddelandehubben och lägga till gräns identifiering toosend hello meddelande endast när hello användaren är inom hello geofence-området.</span><span class="sxs-lookup"><span data-stu-id="c4a43-175">hello project is already configured toosend push notifications tootarget devices, so we’ll need toodo only two things – swap out hello right connection string for hello notification hub and add boundary identification toosend hello notification only when hello user is within hello geofence.</span></span>

<span data-ttu-id="c4a43-176">tooconfigure hello anslutningssträngen i hello `Models` öppna mappen `Notifications.cs`.</span><span class="sxs-lookup"><span data-stu-id="c4a43-176">tooconfigure hello connection string, in hello `Models` folder open `Notifications.cs`.</span></span> <span data-ttu-id="c4a43-177">Hej `NotificationHubClient.CreateClientFromConnectionString` funktionen ska innehålla hello information om meddelandehubben som du kan hämta i hello [Azure Portal](https://portal.azure.com) (titta på hello **åtkomstprinciper** bladet i  **Inställningar för**).</span><span class="sxs-lookup"><span data-stu-id="c4a43-177">hello `NotificationHubClient.CreateClientFromConnectionString` function should contain hello information about your notification hub that you can get in hello [Azure Portal](https://portal.azure.com) (look inside hello **Access Policies** blade in **Settings**).</span></span> <span data-ttu-id="c4a43-178">Spara hello uppdaterade konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="c4a43-178">Save hello updated configuration file.</span></span>

<span data-ttu-id="c4a43-179">Nu måste vi toocreate en modell för hello Bing Maps API-resultatet.</span><span class="sxs-lookup"><span data-stu-id="c4a43-179">Now we need toocreate a model for hello Bing Maps API result.</span></span> <span data-ttu-id="c4a43-180">Hej enklaste sättet toodo som är Högerklicka på hello `Models` mappen **Lägg till** > **klassen**.</span><span class="sxs-lookup"><span data-stu-id="c4a43-180">hello easiest way toodo that is right-click on hello `Models` folder, **Add** > **Class**.</span></span> <span data-ttu-id="c4a43-181">Ge den namnet `GeofenceBoundary.cs`.</span><span class="sxs-lookup"><span data-stu-id="c4a43-181">Name it `GeofenceBoundary.cs`.</span></span> <span data-ttu-id="c4a43-182">När du har gjort, kopiera hello JSON från hello API-svar som vi pratade hello första avsnittet och i Visual Studio använder **redigera** > **klistra in Special** > **klistra in JSON som klasser**.</span><span class="sxs-lookup"><span data-stu-id="c4a43-182">Once done, copy hello JSON from hello API response that we discussed in hello first section and in Visual Studio use **Edit** > **Paste Special** > **Paste JSON as Classes**.</span></span> 

<span data-ttu-id="c4a43-183">På så sätt vi se till att objektet hello ska avserialiseras exakt som det är tänkt.</span><span class="sxs-lookup"><span data-stu-id="c4a43-183">That way we ensure that hello object will be deserialized exactly as it was intended.</span></span> <span data-ttu-id="c4a43-184">Du bör nu ha en klassuppsättning som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="c4a43-184">Your resulting class set should resemble this:</span></span>

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

<span data-ttu-id="c4a43-185">Därefter öppnar du `Controllers` > `NotificationsController.cs`.</span><span class="sxs-lookup"><span data-stu-id="c4a43-185">Next, open `Controllers` > `NotificationsController.cs`.</span></span> <span data-ttu-id="c4a43-186">Vi behöver tootweak hello efter anropet tooaccount för hello mållongituden och mållatituden i beräkningen.</span><span class="sxs-lookup"><span data-stu-id="c4a43-186">We need tootweak hello Post call tooaccount for hello target longitude and latitude.</span></span> <span data-ttu-id="c4a43-187">För att helt enkelt lägga till två strängar toohello funktionssignatur – `latitude` och `longitude`.</span><span class="sxs-lookup"><span data-stu-id="c4a43-187">For that, simply add two strings toohello function signature – `latitude` and `longitude`.</span></span>

    public async Task<HttpResponseMessage> Post(string pns, [FromBody]string message, string to_tag, string latitude, string longitude)

<span data-ttu-id="c4a43-188">Skapa en ny klass i projektet med namnet hello `ApiHelper.cs` – vi använder den tooconnect tooBing toocheck punkt punktgränser.</span><span class="sxs-lookup"><span data-stu-id="c4a43-188">Create a new class within hello project called `ApiHelper.cs` – we’ll use it tooconnect tooBing toocheck point boundary intersections.</span></span> <span data-ttu-id="c4a43-189">Gör så här för att implementera en `IsPointWithinBounds`-funktion:</span><span class="sxs-lookup"><span data-stu-id="c4a43-189">Implement a `IsPointWithinBounds` function, like this:</span></span>

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
> <span data-ttu-id="c4a43-190">Se till att toosubstitute hello API-slutpunkt med hello fråge-URL som du tidigare hämtade från hello Bing Dev Center (det samma gäller toohello API-nyckel).</span><span class="sxs-lookup"><span data-stu-id="c4a43-190">Make sure toosubstitute hello API endpoint with hello query URL that you obtained earlier from hello Bing Dev Center (same applies toohello API key).</span></span> 
> 
> 

<span data-ttu-id="c4a43-191">Om resultaten toohello frågan som innebär att hello angivna punkten ligger inom hello gränserna för geofence-området hello, därför returnerar vi `true`.</span><span class="sxs-lookup"><span data-stu-id="c4a43-191">If there are results toohello query, that means that hello specified point is within hello boundaries of hello geofence, so we return `true`.</span></span> <span data-ttu-id="c4a43-192">Om det finns inga resultat, Bing tala om för oss att hello punkten ligger utanför hello uppslagsramen, därför returnerar vi `false`.</span><span class="sxs-lookup"><span data-stu-id="c4a43-192">If there are no results, Bing is telling us that hello point is outside hello lookup frame, so we return `false`.</span></span>

<span data-ttu-id="c4a43-193">Tillbaka i `NotificationsController.cs`, skapar du en markering precis före switch-instruktionen hello:</span><span class="sxs-lookup"><span data-stu-id="c4a43-193">Back in `NotificationsController.cs`, create a check right before hello switch statement:</span></span>

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

<span data-ttu-id="c4a43-194">På så sätt kan hello-meddelande skickas endast när hello punkten befinner sig inom hello gränser.</span><span class="sxs-lookup"><span data-stu-id="c4a43-194">That way, hello notification is only sent when hello point is within hello boundaries.</span></span>

## <a name="testing-push-notifications-in-hello-uwp-app"></a><span data-ttu-id="c4a43-195">Testa push-meddelanden i hello UWP-appen</span><span class="sxs-lookup"><span data-stu-id="c4a43-195">Testing push notifications in hello UWP app</span></span>
<span data-ttu-id="c4a43-196">Gå tillbaka toohello UWP-appen, bör vi nu kunna tootest meddelanden.</span><span class="sxs-lookup"><span data-stu-id="c4a43-196">Going back toohello UWP app, we should now be able tootest notifications.</span></span> <span data-ttu-id="c4a43-197">Inom hello `LocationHelper` klassen, skapa en ny funktion – `SendLocationToBackend`:</span><span class="sxs-lookup"><span data-stu-id="c4a43-197">Within hello `LocationHelper` class, create a new function – `SendLocationToBackend`:</span></span>

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
> <span data-ttu-id="c4a43-198">Växla hello `POST_URL` toohello platsen för din distribuerade webbapp som vi skapade i föregående avsnitt i hello.</span><span class="sxs-lookup"><span data-stu-id="c4a43-198">Swap hello `POST_URL` toohello location of your deployed web application that we created in hello previous section.</span></span> <span data-ttu-id="c4a43-199">Nu är det OK toorun det lokalt, men när du arbetar med att distribuera en offentlig version, måste toohost den med en extern leverantör.</span><span class="sxs-lookup"><span data-stu-id="c4a43-199">For now, it’s OK toorun it locally, but as you work on deploying a public version, you will need toohost it with an external provider.</span></span>
> 
> 

<span data-ttu-id="c4a43-200">Vi ska nu se till att vi registrerar hello UWP-appen för push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="c4a43-200">Let’s now make sure that we register hello UWP app for push notifications.</span></span> <span data-ttu-id="c4a43-201">I Visual Studio klickar du på **projekt** > **lagra** > **associera appen med butiken hello**.</span><span class="sxs-lookup"><span data-stu-id="c4a43-201">In Visual Studio, click on **Project** > **Store** > **Associate app with hello store**.</span></span>

![](./media/notification-hubs-geofence/vs-associate-with-store.png)

<span data-ttu-id="c4a43-202">När du loggar in tooyour utvecklarkonto, kontrollera att du väljer en befintlig app eller skapa en ny och associerar hello paketet till den.</span><span class="sxs-lookup"><span data-stu-id="c4a43-202">Once you sign in tooyour developer account, make sure you select an existing app or create a new one and associate hello package with it.</span></span> 

<span data-ttu-id="c4a43-203">Gå toohello Dev Center och öppna hello-app som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="c4a43-203">Go toohello Dev Center and open hello app that you just created.</span></span> <span data-ttu-id="c4a43-204">Klicka på **Tjänster** > **Push-meddelanden** > **Webbplatsen för Live-tjänster** .</span><span class="sxs-lookup"><span data-stu-id="c4a43-204">Click **Services** > **Push Notifications** > **Live Services site**.</span></span>

![](./media/notification-hubs-geofence/ms-live-services.png)

<span data-ttu-id="c4a43-205">På hello platsen anteckna hello **Programhemlighet** och hello **paket-SID**.</span><span class="sxs-lookup"><span data-stu-id="c4a43-205">On hello site, take note of hello **Application Secret** and hello **Package SID**.</span></span> <span data-ttu-id="c4a43-206">Behöver du både i hello Azure-portalen – öppna din meddelandehubb, klicka på **inställningar** > **Notification Services** > **Windows (WNS)**och ange hello information i hello obligatoriskt fält.</span><span class="sxs-lookup"><span data-stu-id="c4a43-206">You will need both in hello Azure Portal – open your notification hub, click on **Settings** > **Notification Services** > **Windows (WNS)** and enter hello information in hello required fields.</span></span>

![](./media/notification-hubs-geofence/notification-hubs-wns.png)

<span data-ttu-id="c4a43-207">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="c4a43-207">Click on **Save**.</span></span>

<span data-ttu-id="c4a43-208">Högerklicka på **Referenser** i **Solution Explorer** och välj **Hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="c4a43-208">Right click on **References** in **Solution Explorer** and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="c4a43-209">Vi behöver tooadd en referens toohello **Microsoft Azure Service Bus hanterade biblioteket** – Sök bara efter `WindowsAzure.Messaging.Managed` och lägga till den tooyour projekt.</span><span class="sxs-lookup"><span data-stu-id="c4a43-209">We will need tooadd a reference toohello **Microsoft Azure Service Bus managed library** – simply search for `WindowsAzure.Messaging.Managed` and add it tooyour project.</span></span>

![](./media/notification-hubs-geofence/vs-nuget.png)

<span data-ttu-id="c4a43-210">I testsyfte kan vi skapa hello `MainPage_Loaded` händelsehanteraren igen och Lägg till den här koden fragment tooit:</span><span class="sxs-lookup"><span data-stu-id="c4a43-210">For testing purposes, we can create hello `MainPage_Loaded` event handler once again, and add this code snippet tooit:</span></span>

    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    var hub = new NotificationHub("HUB_NAME", "HUB_LISTEN_CONNECTION_STRING");
    var result = await hub.RegisterNativeAsync(channel.Uri);

    // Displays hello registration ID so you know it was successful
    if (result.RegistrationId != null)
    {
        Debug.WriteLine("Reg successful.");
    }

<span data-ttu-id="c4a43-211">hello ovan registrerar hello app med hello notification hub.</span><span class="sxs-lookup"><span data-stu-id="c4a43-211">hello above registers hello app with hello notification hub.</span></span> <span data-ttu-id="c4a43-212">Du är klar toogo!</span><span class="sxs-lookup"><span data-stu-id="c4a43-212">You are ready toogo!</span></span> 

<span data-ttu-id="c4a43-213">I `LocationHelper`, inuti hello `Geolocator_PositionChanged` hanteraren, du kan lägga till en del av en testkod som med tvång placerar hello platsen inom geofence-området för hello:</span><span class="sxs-lookup"><span data-stu-id="c4a43-213">In `LocationHelper`, inside hello `Geolocator_PositionChanged` handler, you can add a piece of test code that will forcefully put hello location inside hello geofence:</span></span>

    await LocationHelper.SendLocationToBackend("wns", "TEST_USER", "TEST", "37.7746", "-122.3858");

<span data-ttu-id="c4a43-214">Eftersom vi inte skickar hello faktiska koordinaterna (som inte kanske inom gränserna för hello hello tillfället) och använder fördefinierade testvärden, kommer vi att se ett meddelande som visas i uppdateringen:</span><span class="sxs-lookup"><span data-stu-id="c4a43-214">Because we are not passing hello real coordinates (which might not be within hello boundaries at hello moment) and are using predefined test values, we will see a notification show up on update:</span></span>

![](./media/notification-hubs-geofence/notification-hubs-test-notification.png)

## <a name="whats-next"></a><span data-ttu-id="c4a43-215">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c4a43-215">What’s next?</span></span>
<span data-ttu-id="c4a43-216">Det finns några steg som du kan behöva toofollow i tillägget toohello ovan toomake att hello lösningen är klar för produktion.</span><span class="sxs-lookup"><span data-stu-id="c4a43-216">There are a couple of steps that you might need toofollow in addition toohello above toomake sure that hello solution is production-ready.</span></span>

<span data-ttu-id="c4a43-217">Först och främst kan behöva du tooensure att geofence-områdena är dynamiska.</span><span class="sxs-lookup"><span data-stu-id="c4a43-217">First and foremost, you might need tooensure that geofences are dynamic.</span></span> <span data-ttu-id="c4a43-218">Detta kräver lite extraarbete med hello Bing API i ordning toobe kan tooupload nya gränser inom hello befintlig datakälla.</span><span class="sxs-lookup"><span data-stu-id="c4a43-218">This will require some extra work with hello Bing API in order toobe able tooupload new boundaries within hello existing data source.</span></span> <span data-ttu-id="c4a43-219">Kontakta hello [Bing Spatial Data Services API-dokumentationen](https://msdn.microsoft.com/library/ff701734.aspx) för mer information om hello ämne.</span><span class="sxs-lookup"><span data-stu-id="c4a43-219">Consult hello [Bing Spatial Data Services API documentation](https://msdn.microsoft.com/library/ff701734.aspx) for more details on hello subject.</span></span>

<span data-ttu-id="c4a43-220">Andra, eftersom du arbeta tooensure hello leveransen görs toohello rätt deltagare kanske du vill tootarget dem via [taggning](notification-hubs-tags-segment-push-message.md).</span><span class="sxs-lookup"><span data-stu-id="c4a43-220">Second, as you are working tooensure that hello delivery is done toohello right participants, you might want tootarget them via [tagging](notification-hubs-tags-segment-push-message.md).</span></span>

<span data-ttu-id="c4a43-221">hello lösningen ovan beskriver ett scenario där du kan ha en mängd olika målplattformar, så inte begränsar vi hello geofencing toosystem-specifika funktioner.</span><span class="sxs-lookup"><span data-stu-id="c4a43-221">hello solution shown above describes a scenario in which you might have a wide variety of target platforms, so we did not limit hello geofencing toosystem-specific capabilities.</span></span> <span data-ttu-id="c4a43-222">Namn, hello Uwp har funktioner för[detektera geofence rätt out-of-the-box](https://msdn.microsoft.com/windows/uwp/maps-and-location/set-up-a-geofence).</span><span class="sxs-lookup"><span data-stu-id="c4a43-222">That said, hello Universal Windows Platform offers capabilities too[detect geofences right out-of-the-box](https://msdn.microsoft.com/windows/uwp/maps-and-location/set-up-a-geofence).</span></span>

<span data-ttu-id="c4a43-223">För mer information om funktioner i Notification Hubs, se vår [dokumentationsportal](https://azure.microsoft.com/documentation/services/notification-hubs/).</span><span class="sxs-lookup"><span data-stu-id="c4a43-223">For more details regarding Notification Hubs capabilities, check out our [documentation portal](https://azure.microsoft.com/documentation/services/notification-hubs/).</span></span>

