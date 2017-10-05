---
title: "Push-meddelanden till geografiskt avgränsade områden med Azure Notification Hubs och Bing Spatial Data | Microsoft Docs"
description: "I den här självstudiekurskursen får lära du dig att leverera platsbaserade push-meddelanden med Azure Notification Hub och Bing Spatial Data."
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
ms.openlocfilehash: b2a84e0479aac9ded08bb64e1ea20ddee6636cce
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="geo-fenced-push-notifications-with-azure-notification-hubs-and-bing-spatial-data"></a><span data-ttu-id="bba16-104">Push-meddelanden till geografiskt avgränsade områden med Azure Notification Hubs och Bing Spatial Data</span><span class="sxs-lookup"><span data-stu-id="bba16-104">Geo-fenced push notifications with Azure Notification Hubs and Bing Spatial Data</span></span>
> [!NOTE]
> <span data-ttu-id="bba16-105">Du måste ha ett aktivt Azure-konto för att slutföra den här kursen.</span><span class="sxs-lookup"><span data-stu-id="bba16-105">To complete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="bba16-106">Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="bba16-106">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="bba16-107">Mer information om den kostnadsfria utvärderingsversionen av Azure finns [här](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02).</span><span class="sxs-lookup"><span data-stu-id="bba16-107">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02).</span></span>
> 
> 

<span data-ttu-id="bba16-108">I den här självstudiekursen får lära du dig att leverera platsbaserade push-meddelanden med Azure Notification Hub och Bing Spatial Data. Användningen underlättas av att allt genomförs från en UWP-app (Universal Windows Platform).</span><span class="sxs-lookup"><span data-stu-id="bba16-108">In this tutorial, you will learn how to deliver location-based push notifications with Azure Notification Hubs and Bing Spatial Data, leveraged from within a Universal Windows Platform application.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bba16-109">Krav</span><span class="sxs-lookup"><span data-stu-id="bba16-109">Prerequisites</span></span>
<span data-ttu-id="bba16-110">Först och främst måste du se till att du har alla program och att alla grundläggande krav uppfylls: </span><span class="sxs-lookup"><span data-stu-id="bba16-110">First and foremost, you need to make sure that you have all the software and service pre-requisites:</span></span>

* <span data-ttu-id="bba16-111">[Visual Studio 2015 Update 1](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx) eller senare ([Community Edition](https://go.microsoft.com/fwlink/?LinkId=691978&clcid=0x409) går också bra).</span><span class="sxs-lookup"><span data-stu-id="bba16-111">[Visual Studio 2015 Update 1](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx) or later ([Community Edition](https://go.microsoft.com/fwlink/?LinkId=691978&clcid=0x409) will do as well).</span></span> 
* <span data-ttu-id="bba16-112">Senaste versionen av [Azure SDK](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="bba16-112">Latest version of the [Azure SDK](https://azure.microsoft.com/downloads/).</span></span> 
* <span data-ttu-id="bba16-113">[Konto på Bing Maps Dev Center](https://www.bingmapsportal.com/) (du kan skapa ett gratis och associera det med ditt Microsoft-konto).</span><span class="sxs-lookup"><span data-stu-id="bba16-113">[Bing Maps Dev Center account](https://www.bingmapsportal.com/) (you can create one for free and associate it with your Microsoft account).</span></span> 

## <a name="getting-started"></a><span data-ttu-id="bba16-114">Komma igång</span><span class="sxs-lookup"><span data-stu-id="bba16-114">Getting Started</span></span>
<span data-ttu-id="bba16-115">Låt oss börja med att skapa projektet.</span><span class="sxs-lookup"><span data-stu-id="bba16-115">Let’s start by creating the project.</span></span> <span data-ttu-id="bba16-116">Starta ett nytt projekt av typen **tom app (Universal Windows)** i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bba16-116">In Visual Studio, start a new project of type **Blank App (Universal Windows)**.</span></span>

![](./media/notification-hubs-geofence/notification-hubs-create-blank-app.png)

<span data-ttu-id="bba16-117">När projektet har skapats har du en stomme för själva appen.</span><span class="sxs-lookup"><span data-stu-id="bba16-117">Once the project creation is complete, you should have the harness for the app itself.</span></span> <span data-ttu-id="bba16-118">Nu är det dags att göra alla justeringar för infrastrukturen för geografisk avgränsning (geo-fencing).</span><span class="sxs-lookup"><span data-stu-id="bba16-118">Now let’s set up everything for the geo-fencing infrastructure.</span></span> <span data-ttu-id="bba16-119">Eftersom vi ska använda Bing-tjänster för att göra detta, finns det en offentlig REST-API-slutpunkt med vilken vi kan skicka förfrågningar till specifika platsramar: </span><span class="sxs-lookup"><span data-stu-id="bba16-119">Because we are going to use Bing services for this, there is a public REST API endpoint that allows us to query specific location frames:</span></span>

    http://spatial.virtualearth.net/REST/v1/data/

<span data-ttu-id="bba16-120">Du måste ange följande parametrar för att detta ska fungera:</span><span class="sxs-lookup"><span data-stu-id="bba16-120">You will need to specify the following parameters to get it working:</span></span>

* <span data-ttu-id="bba16-121">**Datakällans ID** och **Datakällnamn** – I Bing Maps API innehåller datakällorna flera bucketgrupperade metadata, som platser och öppettider.</span><span class="sxs-lookup"><span data-stu-id="bba16-121">**Data Source ID** and **Data Source Name** – in Bing Maps API, data sources contain various bucketed metadata, such as locations and business hours of operation.</span></span> <span data-ttu-id="bba16-122">Du kan läsa mer om dessa här.</span><span class="sxs-lookup"><span data-stu-id="bba16-122">You can read more about those here.</span></span> 
* <span data-ttu-id="bba16-123">**Entitetsnamn** – Den entitet som du vill använda som referenspunkt för meddelandet.</span><span class="sxs-lookup"><span data-stu-id="bba16-123">**Entity Name** – the entity you want to use as a reference point for the notification.</span></span> 
* <span data-ttu-id="bba16-124">**Bing Maps API-nyckel** – Det här är den nyckel som du fick tidigare, när du skapade Bing Dev Center-kontot.</span><span class="sxs-lookup"><span data-stu-id="bba16-124">**Bing Maps API Key** – this is the key that you obtained earlier when you created the Bing Dev Center account.</span></span>

<span data-ttu-id="bba16-125">Låt oss göra en djupdykning i inställningarna för vart och ett av elementen ovan.</span><span class="sxs-lookup"><span data-stu-id="bba16-125">Let’s do a deep-dive on the setup for each of the elements above.</span></span>

## <a name="setting-up-the-data-source"></a><span data-ttu-id="bba16-126">Ställa in datakällan</span><span class="sxs-lookup"><span data-stu-id="bba16-126">Setting up the data source</span></span>
<span data-ttu-id="bba16-127">Du kan göra detta i Bing Maps Dev Center.</span><span class="sxs-lookup"><span data-stu-id="bba16-127">You can do it in the Bing Maps Dev Center.</span></span> <span data-ttu-id="bba16-128">Klicka på **Datakällor** i det övre navigeringsfältet och välj **Hantera datakällor**.</span><span class="sxs-lookup"><span data-stu-id="bba16-128">Simply click on **Data sources** in the top navigation bar and select **Manage Data Sources**.</span></span>

![](./media/notification-hubs-geofence/bing-maps-manage-data.png)

<span data-ttu-id="bba16-129">Om du inte har arbetat med Bing Maps API tidigare så kommer det troligen inte att finnas några datakällor där. Då kan du helt enkelt skapa en ny genom att klicka på Föra över data till en datakälla.</span><span class="sxs-lookup"><span data-stu-id="bba16-129">If you have not worked with Bing Maps API before, most likely there won’t be any data sources present, so you can just create a new one by clicking on Upload data to a data source.</span></span> <span data-ttu-id="bba16-130">Kontrollera att du fyller i alla obligatoriska fält:</span><span class="sxs-lookup"><span data-stu-id="bba16-130">Make sure you fill out all the required fields:</span></span>

![](./media/notification-hubs-geofence/bing-maps-create-data.png)

<span data-ttu-id="bba16-131">Du kanske undrar: Vad är datafilen och vad är det du ska föra över?</span><span class="sxs-lookup"><span data-stu-id="bba16-131">You might be wondering – what is the data file and what should you be uploading?</span></span> <span data-ttu-id="bba16-132">I syfte att genomföra det här testet kan vi helt enkelt använda det pipe-baserade exempel som omfattar en del av San Franciscos hamnområde. </span><span class="sxs-lookup"><span data-stu-id="bba16-132">For the purposes of this test, we can just use the sample pipe-based that frames an area of the San Francisco waterfront:</span></span>

    Bing Spatial Data Services, 1.0, TestBoundaries
    EntityID(Edm.String,primaryKey)|Name(Edm.String)|Longitude(Edm.Double)|Latitude(Edm.Double)|Boundary(Edm.Geography)
    1|SanFranciscoPier|||POLYGON ((-122.389825 37.776598,-122.389438 37.773087,-122.381885 37.771849,-122.382186 37.777022,-122.389825 37.776598))

<span data-ttu-id="bba16-133">Ovanstående representerar den här entiteten:</span><span class="sxs-lookup"><span data-stu-id="bba16-133">The above represents this entity:</span></span>

![](./media/notification-hubs-geofence/bing-maps-geofence.png)

<span data-ttu-id="bba16-134">Du behöver bara kopiera och klistra in ovanstående sträng i en ny fil och spara den som **NotificationHubsGeofence.pipe**. Sedan för du över den till Bing Dev Center.</span><span class="sxs-lookup"><span data-stu-id="bba16-134">Simply copy and paste the string above into a new file and save it as **NotificationHubsGeofence.pipe**, and upload it in the Bing Dev Center.</span></span>

> [!NOTE]
> <span data-ttu-id="bba16-135">Du kan uppmanas att ange en ny nyckel för **huvudnyckel**n. Denna skiljer sig från **frågenyckeln**.</span><span class="sxs-lookup"><span data-stu-id="bba16-135">You might be prompted to specify a new key for the **Master Key** that is different from the **Query Key**.</span></span> <span data-ttu-id="bba16-136">Du behöver bara skapa en ny nyckel via instrumentpanelen och uppdatera överföringssidan för datakällan.</span><span class="sxs-lookup"><span data-stu-id="bba16-136">Simply create a new key through the dashboard and refresh the data source upload page.</span></span>
> 
> 

<span data-ttu-id="bba16-137">När du har laddat upp datafilen, måste du publicera datakällan.</span><span class="sxs-lookup"><span data-stu-id="bba16-137">Once you upload the data file, you will need to make sure that you publish the data source.</span></span> 

<span data-ttu-id="bba16-138">Gå till **Hantera datakällor**, precis som du gjorde ovan, leta rätt på datakällan i listan och klicka sedan på **Publicera** i kolumnen **Åtgärder**.</span><span class="sxs-lookup"><span data-stu-id="bba16-138">Go to **Manage Data Sources**, just like we did above, find your data source in the list and click on **Publish** in the **Actions** column.</span></span> <span data-ttu-id="bba16-139">Efter en stund bör du se din datakälla på fliken **Publicerade datakällor**:</span><span class="sxs-lookup"><span data-stu-id="bba16-139">In a bit, you should see your data source in the **Published Data Sources** tab:</span></span>

![](./media/notification-hubs-geofence/bing-maps-published-data.png)

<span data-ttu-id="bba16-140">Om du klickar på **Redigera**, kommer du att få en snabb överblick över vilka platser vi har fört in i den:</span><span class="sxs-lookup"><span data-stu-id="bba16-140">If you click **Edit**, you will be able to see at a glance what locations we introduced in it:</span></span>

![](./media/notification-hubs-geofence/bing-maps-data-details.png)

<span data-ttu-id="bba16-141">I det här läget visar inte portalen gränserna för det geofence som vi skapat. Det enda vi behöver är en bekräftelse på att den angivna platsen ligger ungefär i rätt område.</span><span class="sxs-lookup"><span data-stu-id="bba16-141">At this point, the portal does not show you the boundaries for the geofence that we created – all we need is a confirmation that the location specified is in the right vicinity.</span></span>

<span data-ttu-id="bba16-142">Nu är alla krav uppfyllda för datakällan.</span><span class="sxs-lookup"><span data-stu-id="bba16-142">Now you have all the requirements for the data source.</span></span> <span data-ttu-id="bba16-143">Om du vill ha information om API-anropets URL-adress för begäran i Bing Maps Dev Center, klickar du på **Datakällor** och väljer **Information om datakällan**.</span><span class="sxs-lookup"><span data-stu-id="bba16-143">To get the details on the request URL for the API call, in the Bing Maps Dev Center, click **Data sources** and select **Data Source Information**.</span></span>

![](./media/notification-hubs-geofence/bing-maps-data-info.png)

<span data-ttu-id="bba16-144">Det vi letar efter här är **Fråge-URL**.</span><span class="sxs-lookup"><span data-stu-id="bba16-144">The **Query URL** is what we’re after here.</span></span> <span data-ttu-id="bba16-145">Det här är den slutpunkt mot vilken vi kan köra frågor för att kontrollera om enheten för närvarande befinner sig inom platsgränserna eller inte.</span><span class="sxs-lookup"><span data-stu-id="bba16-145">This is the endpoint against which we can execute queries to check whether the device is currently within the boundaries of a location or not.</span></span> <span data-ttu-id="bba16-146">För att utföra den här kontrollen behöver vi bara köra ett GET-anrop mot fråge-URL:et. Följande parametrar måste läggas till:</span><span class="sxs-lookup"><span data-stu-id="bba16-146">To perform this check, we simply need to execute a GET call against the query URL, with the following parameters appended:</span></span>

    ?spatialFilter=intersects(%27POINT%20LONGITUDE%20LATITUDE)%27)&$format=json&key=QUERY_KEY

<span data-ttu-id="bba16-147">På så sätt anger du en målpunkt som vi får från enheten och Bing Maps kommer automatiskt att utföra beräkningar för att kontrollera om den befinner sig inom vårt geofence eller inte.</span><span class="sxs-lookup"><span data-stu-id="bba16-147">That way you're specifying a target point that we obtain from the device and Bing Maps will automatically perform the calculations to see whether it is within the geofence.</span></span> <span data-ttu-id="bba16-148">När du kör begäran via en webbläsare (eller cURL), kommer du att få ett standard- JSON-svar:</span><span class="sxs-lookup"><span data-stu-id="bba16-148">Once you execute the request through a browser (or cURL), you will get standard a JSON response:</span></span>

![](./media/notification-hubs-geofence/bing-maps-json.png)

<span data-ttu-id="bba16-149">Du får bara tillbaka detta svar när punkten verkligen ligger inom de angivna gränserna.</span><span class="sxs-lookup"><span data-stu-id="bba16-149">This response only happens when the point is actually within the designated boundaries.</span></span> <span data-ttu-id="bba16-150">Om detta inte är fallet, får du upp en tom **resultat**-bucket:</span><span class="sxs-lookup"><span data-stu-id="bba16-150">If it is not, you will get an empty **results** bucket:</span></span>

![](./media/notification-hubs-geofence/bing-maps-nores.png)

## <a name="setting-up-the-uwp-application"></a><span data-ttu-id="bba16-151">Konfigurera UWP-appen</span><span class="sxs-lookup"><span data-stu-id="bba16-151">Setting up the UWP application</span></span>
<span data-ttu-id="bba16-152">Nu när datakällan är redo kan vi börja jobba med UWP-appen som vi startade om tidigare.</span><span class="sxs-lookup"><span data-stu-id="bba16-152">Now that we have the data source ready, we can start working on the UWP application that we bootstrapped earlier.</span></span>

<span data-ttu-id="bba16-153">Först och främst måste vi aktivera platstjänster för vår app.</span><span class="sxs-lookup"><span data-stu-id="bba16-153">First and foremost, we must enable location services for our application.</span></span> <span data-ttu-id="bba16-154">För att göra detta dubbelklickar du på filen `Package.appxmanifest` i **Solution Explorer**.</span><span class="sxs-lookup"><span data-stu-id="bba16-154">To do this, double-click on `Package.appxmanifest` file in **Solution Explorer**.</span></span>

![](./media/notification-hubs-geofence/vs-package-manifest.png)

<span data-ttu-id="bba16-155">I den flik för paketegenskaper som just öppnades klickar du på **Funktioner** och väljer sedan **plats**:</span><span class="sxs-lookup"><span data-stu-id="bba16-155">In the package properties tab that just opened, click on **Capabilities** and make sure that you select **Location**:</span></span>

![](./media/notification-hubs-geofence/vs-package-location.png)

<span data-ttu-id="bba16-156">Eftersom platsegenskapen har angetts ska du skapa en ny mapp i din lösning som kallas för `Core`. Sedan lägger du till en ny fil i denna med namnet `LocationHelper.cs`:</span><span class="sxs-lookup"><span data-stu-id="bba16-156">As the location capability is declared, create a new folder in your solution named `Core`, and add a new file within it, called `LocationHelper.cs`:</span></span>

![](./media/notification-hubs-geofence/vs-location-helper.png)

<span data-ttu-id="bba16-157">Själva `LocationHelper`-klassen är ganska grundläggande just nu: allt den gör är att låta oss hämta användarens plats via system-API:et:</span><span class="sxs-lookup"><span data-stu-id="bba16-157">The `LocationHelper` class itself is fairly basic at this point – all it does is allow us to obtain the user location through the system API:</span></span>

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

<span data-ttu-id="bba16-158">Du kan läsa mer om att hämta användarens plats med UWP-appar i det officiella [MSDN-dokumentet](https://msdn.microsoft.com/library/windows/apps/mt219698.aspx).</span><span class="sxs-lookup"><span data-stu-id="bba16-158">You can read more about getting the user’s location in UWP apps in the official [MSDN document](https://msdn.microsoft.com/library/windows/apps/mt219698.aspx).</span></span>

<span data-ttu-id="bba16-159">Om du vill kontrollera att platsinsamlingen faktiskt fungerar, kan du öppna din huvudsidas kodsida (`MainPage.xaml.cs`).</span><span class="sxs-lookup"><span data-stu-id="bba16-159">To check that the location acquisition is actually working, open the code side of your main page (`MainPage.xaml.cs`).</span></span> <span data-ttu-id="bba16-160">Skapa en ny händelsehanterare för `Loaded`-händelsen i `MainPage`-konstruktorn:</span><span class="sxs-lookup"><span data-stu-id="bba16-160">Create a new event handler for the `Loaded` event in the `MainPage` constructor:</span></span>

    public MainPage()
    {
        this.InitializeComponent();
        this.Loaded += MainPage_Loaded;
    }

<span data-ttu-id="bba16-161">Implementeringen av händelsehanteraren fungerar så här:</span><span class="sxs-lookup"><span data-stu-id="bba16-161">The implementation of the event handler is as follows:</span></span>

    private async void MainPage_Loaded(object sender, RoutedEventArgs e)
    {
        var location = await LocationHelper.GetCurrentLocation();

        if (location != null)
        {
            Debug.WriteLine(string.Concat(location.Coordinate.Longitude,
                " ", location.Coordinate.Latitude));
        }
    }

<span data-ttu-id="bba16-162">Observera att vi har deklarerat hanteraren som asynkron eftersom `GetCurrentLocation` är awaitable och därför måste köras i en asynkron kontext.</span><span class="sxs-lookup"><span data-stu-id="bba16-162">Notice that we declared the handler as async because `GetCurrentLocation` is awaitable, and therefore requires to be executed in an async context.</span></span> <span data-ttu-id="bba16-163">Och eftersom vi under vissa omständigheter kan få en null-plats (t.ex. att platstjänsterna är inaktiverade eller att appen har nekats åtkomst till platsen på grund av fel behörighet), måste vi försäkra oss om att den hanteras korrekt med en null-kontroll.</span><span class="sxs-lookup"><span data-stu-id="bba16-163">Also, because under certain circumstances we might end up with a null location (e.g. the location services are disabled or the application was denied permissions to access location), we need to make sure that it is properly handled with a null check.</span></span>

<span data-ttu-id="bba16-164">Kör appen.</span><span class="sxs-lookup"><span data-stu-id="bba16-164">Run the application.</span></span> <span data-ttu-id="bba16-165">Kontrollera att du tillåter platsåtkomst:</span><span class="sxs-lookup"><span data-stu-id="bba16-165">Make sure you allow location access:</span></span>

![](./media/notification-hubs-geofence/notification-hubs-location-access.png)

<span data-ttu-id="bba16-166">När appen startas ska du kunna se koordinaterna i fönstret **Utmatning**:</span><span class="sxs-lookup"><span data-stu-id="bba16-166">Once the application launches, you should be able to see the coordinates in the **Output** window:</span></span>

![](./media/notification-hubs-geofence/notification-hubs-location-output.png)

<span data-ttu-id="bba16-167">Nu vet du att platsförvärvet fungerar som det ska. Nu kan du även passa på att ta bort händelsehanteraren för test eftersom vi inte behöver använda den längre.</span><span class="sxs-lookup"><span data-stu-id="bba16-167">Now you know that location acquisition works – feel free to remove the test event handler for Loaded because we won’t be using it anymore.</span></span>

<span data-ttu-id="bba16-168">Nästa steg är att samla in platsförändringar.</span><span class="sxs-lookup"><span data-stu-id="bba16-168">The next step is to capture location changes.</span></span> <span data-ttu-id="bba16-169">För att göra detta går vi tillbaka till klassen `LocationHelper` och lägger till händelsehanteraren för `PositionChanged`:</span><span class="sxs-lookup"><span data-stu-id="bba16-169">For that, let’s go back to the `LocationHelper` class and add the event handler for `PositionChanged`:</span></span>

    geolocator.PositionChanged += Geolocator_PositionChanged;

<span data-ttu-id="bba16-170">Implementeringen visar platskoordinaterna i fönstret **Utmatning**:</span><span class="sxs-lookup"><span data-stu-id="bba16-170">The implementation will show the location coordinates in the **Output** window:</span></span>

    private static async void Geolocator_PositionChanged(Geolocator sender, PositionChangedEventArgs args)
    {
        await CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
        {
            Debug.WriteLine(string.Concat(args.Position.Coordinate.Longitude, " ", args.Position.Coordinate.Latitude));
        });
    }

## <a name="setting-up-the-backend"></a><span data-ttu-id="bba16-171">Ställa in serverdelen</span><span class="sxs-lookup"><span data-stu-id="bba16-171">Setting up the backend</span></span>
<span data-ttu-id="bba16-172">Hämta [Exempel för .NET-serverdel från GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers).</span><span class="sxs-lookup"><span data-stu-id="bba16-172">Download the [.NET Backend Sample from GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers).</span></span> <span data-ttu-id="bba16-173">När hämtningen är klar, öppnar du mappen `NotifyUsers` och därefter filen `NotifyUsers.sln`.</span><span class="sxs-lookup"><span data-stu-id="bba16-173">Once the download completes, open the `NotifyUsers` folder, and subsequently – the `NotifyUsers.sln` file.</span></span>

<span data-ttu-id="bba16-174">Ange projektet `AppBackend` som **startprojektet** och starta det.</span><span class="sxs-lookup"><span data-stu-id="bba16-174">Set the `AppBackend` project as the **StartUp Project** and launch it.</span></span>

![](./media/notification-hubs-geofence/vs-startup-project.png)

<span data-ttu-id="bba16-175">Projektet har redan konfigurerats för att skicka push-meddelanden till målenheterna så du behöver bara göra två saker: byta ut rätt anslutningssträng för meddelandehubben och lägga till gränsidentifieringar för att endast skicka meddelanden när användaren befinner sig inom geofence-området.</span><span class="sxs-lookup"><span data-stu-id="bba16-175">The project is already configured to send push notifications to target devices, so we’ll need to do only two things – swap out the right connection string for the notification hub and add boundary identification to send the notification only when the user is within the geofence.</span></span>

<span data-ttu-id="bba16-176">Öppna `Notifications.cs` i mappen `Models` för att konfigurera anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="bba16-176">To configure the connection string, in the `Models` folder open `Notifications.cs`.</span></span> <span data-ttu-id="bba16-177">Funktionen `NotificationHubClient.CreateClientFromConnectionString` bör innehålla den information om meddelandehubben som du kan hämta i [Azure-portalen](https://portal.azure.com) (titta på bladet **Åtkomstprinciper** under **Inställningar**).</span><span class="sxs-lookup"><span data-stu-id="bba16-177">The `NotificationHubClient.CreateClientFromConnectionString` function should contain the information about your notification hub that you can get in the [Azure Portal](https://portal.azure.com) (look inside the **Access Policies** blade in **Settings**).</span></span> <span data-ttu-id="bba16-178">Spara den uppdaterade konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="bba16-178">Save the updated configuration file.</span></span>

<span data-ttu-id="bba16-179">Nu måste vi skapa en modell för Bing Maps API-resultatet.</span><span class="sxs-lookup"><span data-stu-id="bba16-179">Now we need to create a model for the Bing Maps API result.</span></span> <span data-ttu-id="bba16-180">Det enklaste sättet att göra detta är högerklicka på mappen `Models` och sedan på **Lägg till** > **Klass**.</span><span class="sxs-lookup"><span data-stu-id="bba16-180">The easiest way to do that is right-click on the `Models` folder, **Add** > **Class**.</span></span> <span data-ttu-id="bba16-181">Ge den namnet `GeofenceBoundary.cs`.</span><span class="sxs-lookup"><span data-stu-id="bba16-181">Name it `GeofenceBoundary.cs`.</span></span> <span data-ttu-id="bba16-182">När du har gjort detta ska du kopiera JSON:et från det API-svar som vi pratade om i det första avsnittet. Och i Visual Studio använder du sedan **Redigera** > **Klistra in special** > **Klistra in JSON som klass**.</span><span class="sxs-lookup"><span data-stu-id="bba16-182">Once done, copy the JSON from the API response that we discussed in the first section and in Visual Studio use **Edit** > **Paste Special** > **Paste JSON as Classes**.</span></span> 

<span data-ttu-id="bba16-183">På så sätt ser vi till att objektet avserialiseras exakt på det sätt som vi vill att det ska.</span><span class="sxs-lookup"><span data-stu-id="bba16-183">That way we ensure that the object will be deserialized exactly as it was intended.</span></span> <span data-ttu-id="bba16-184">Du bör nu ha en klassuppsättning som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="bba16-184">Your resulting class set should resemble this:</span></span>

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

<span data-ttu-id="bba16-185">Därefter öppnar du `Controllers` > `NotificationsController.cs`.</span><span class="sxs-lookup"><span data-stu-id="bba16-185">Next, open `Controllers` > `NotificationsController.cs`.</span></span> <span data-ttu-id="bba16-186">Vi behöver finjustera efteranropet till kontot för att ta med mållongituden och mållatituden i beräkningen.</span><span class="sxs-lookup"><span data-stu-id="bba16-186">We need to tweak the Post call to account for the target longitude and latitude.</span></span> <span data-ttu-id="bba16-187">För att göra detta behöver vi bara lägga till två strängar i funktionssignaturen: `latitude` och `longitude`.</span><span class="sxs-lookup"><span data-stu-id="bba16-187">For that, simply add two strings to the function signature – `latitude` and `longitude`.</span></span>

    public async Task<HttpResponseMessage> Post(string pns, [FromBody]string message, string to_tag, string latitude, string longitude)

<span data-ttu-id="bba16-188">Skapa en ny klass i projektet med namnet `ApiHelper.cs`. Vi kommer att använda den för att ansluta till Bing och kontrollera skärningspunkterna för punktgränser.</span><span class="sxs-lookup"><span data-stu-id="bba16-188">Create a new class within the project called `ApiHelper.cs` – we’ll use it to connect to Bing to check point boundary intersections.</span></span> <span data-ttu-id="bba16-189">Gör så här för att implementera en `IsPointWithinBounds`-funktion:</span><span class="sxs-lookup"><span data-stu-id="bba16-189">Implement a `IsPointWithinBounds` function, like this:</span></span>

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
> <span data-ttu-id="bba16-190">Ersätt API-slutpunkten med fråge-URL:en som du tidigare hämtade från Bing Dev Center (det samma gäller för API-nyckeln).</span><span class="sxs-lookup"><span data-stu-id="bba16-190">Make sure to substitute the API endpoint with the query URL that you obtained earlier from the Bing Dev Center (same applies to the API key).</span></span> 
> 
> 

<span data-ttu-id="bba16-191">Om du får resultat från begäran så innebär det att den angivna punkten befinner sig inom gränserna för geofence-området. Därför returnerar vi `true`.</span><span class="sxs-lookup"><span data-stu-id="bba16-191">If there are results to the query, that means that the specified point is within the boundaries of the geofence, so we return `true`.</span></span> <span data-ttu-id="bba16-192">Om du inte får några resultat, kommer Bing att tala om för oss att punkten ligger utanför uppslagsramen. Därför returnerar vi `false`.</span><span class="sxs-lookup"><span data-stu-id="bba16-192">If there are no results, Bing is telling us that the point is outside the lookup frame, so we return `false`.</span></span>

<span data-ttu-id="bba16-193">När du har återgått till `NotificationsController.cs` skapar du en markering precis före switch-uttrycket:</span><span class="sxs-lookup"><span data-stu-id="bba16-193">Back in `NotificationsController.cs`, create a check right before the switch statement:</span></span>

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

<span data-ttu-id="bba16-194">Om du gör detta kommer meddelandet endast att skickas när punkten befinner sig inom gränserna.</span><span class="sxs-lookup"><span data-stu-id="bba16-194">That way, the notification is only sent when the point is within the boundaries.</span></span>

## <a name="testing-push-notifications-in-the-uwp-app"></a><span data-ttu-id="bba16-195">Testa push-meddelanden i UWP-appen</span><span class="sxs-lookup"><span data-stu-id="bba16-195">Testing push notifications in the UWP app</span></span>
<span data-ttu-id="bba16-196">När vi går tillbaka till UWP-appen, bör vi nu kunna testa meddelandefunktionen.</span><span class="sxs-lookup"><span data-stu-id="bba16-196">Going back to the UWP app, we should now be able to test notifications.</span></span> <span data-ttu-id="bba16-197">Skapa en ny funktion – `SendLocationToBackend` –  i klassen `LocationHelper`.</span><span class="sxs-lookup"><span data-stu-id="bba16-197">Within the `LocationHelper` class, create a new function – `SendLocationToBackend`:</span></span>

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
> <span data-ttu-id="bba16-198">Byt ut `POST_URL` till platsen för din distribuerade webbapp, den som vi skapade i det förra avsnittet.</span><span class="sxs-lookup"><span data-stu-id="bba16-198">Swap the `POST_URL` to the location of your deployed web application that we created in the previous section.</span></span> <span data-ttu-id="bba16-199">Just nu är det ok att köra appen lokalt men när du arbetar med att distribuera en offentlig version, måste du lägga upp den hos en extern leverantör.</span><span class="sxs-lookup"><span data-stu-id="bba16-199">For now, it’s OK to run it locally, but as you work on deploying a public version, you will need to host it with an external provider.</span></span>
> 
> 

<span data-ttu-id="bba16-200">Nu ska vi registrera UWP-appen för push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="bba16-200">Let’s now make sure that we register the UWP app for push notifications.</span></span> <span data-ttu-id="bba16-201">I Visual Studio klickar du på **Projekt** > **Butik** > **Associera appen med butiken**.</span><span class="sxs-lookup"><span data-stu-id="bba16-201">In Visual Studio, click on **Project** > **Store** > **Associate app with the store**.</span></span>

![](./media/notification-hubs-geofence/vs-associate-with-store.png)

<span data-ttu-id="bba16-202">Kontrollera att du väljer en befintlig app, eller skapa en ny, och associera paketet med den när du loggar in på ditt utvecklarkonto.</span><span class="sxs-lookup"><span data-stu-id="bba16-202">Once you sign in to your developer account, make sure you select an existing app or create a new one and associate the package with it.</span></span> 

<span data-ttu-id="bba16-203">Gå till Dev Center och öppna den app som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="bba16-203">Go to the Dev Center and open the app that you just created.</span></span> <span data-ttu-id="bba16-204">Klicka på **Tjänster** > **Push-meddelanden** > **Webbplatsen för Live-tjänster** .</span><span class="sxs-lookup"><span data-stu-id="bba16-204">Click **Services** > **Push Notifications** > **Live Services site**.</span></span>

![](./media/notification-hubs-geofence/ms-live-services.png)

<span data-ttu-id="bba16-205">När du är på webbplatsen skriver du ned **apphemligheten** och **paket-SID:et**.</span><span class="sxs-lookup"><span data-stu-id="bba16-205">On the site, take note of the **Application Secret** and the **Package SID**.</span></span> <span data-ttu-id="bba16-206">Du kommer att behöva båda i Azure-portalen. Öppna din meddelandehubb, klicka på **Inställningar** > **Notification Services** > **Windows (WNS)** och ange uppgifterna i de obligatoriska fälten.</span><span class="sxs-lookup"><span data-stu-id="bba16-206">You will need both in the Azure Portal – open your notification hub, click on **Settings** > **Notification Services** > **Windows (WNS)** and enter the information in the required fields.</span></span>

![](./media/notification-hubs-geofence/notification-hubs-wns.png)

<span data-ttu-id="bba16-207">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="bba16-207">Click on **Save**.</span></span>

<span data-ttu-id="bba16-208">Högerklicka på **Referenser** i **Solution Explorer** och välj **Hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="bba16-208">Right click on **References** in **Solution Explorer** and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="bba16-209">Vi måste lägga till en referens till **hanteringsbiblioteket för Microsoft Azure Service Bus**. Du behöver bara söka efter `WindowsAzure.Messaging.Managed` och lägga till den i projektet.</span><span class="sxs-lookup"><span data-stu-id="bba16-209">We will need to add a reference to the **Microsoft Azure Service Bus managed library** – simply search for `WindowsAzure.Messaging.Managed` and add it to your project.</span></span>

![](./media/notification-hubs-geofence/vs-nuget.png)

<span data-ttu-id="bba16-210">I testsyfte kan vi skapa händelsehanteraren `MainPage_Loaded` igen och lägga till det här kodstycket i den:</span><span class="sxs-lookup"><span data-stu-id="bba16-210">For testing purposes, we can create the `MainPage_Loaded` event handler once again, and add this code snippet to it:</span></span>

    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    var hub = new NotificationHub("HUB_NAME", "HUB_LISTEN_CONNECTION_STRING");
    var result = await hub.RegisterNativeAsync(channel.Uri);

    // Displays the registration ID so you know it was successful
    if (result.RegistrationId != null)
    {
        Debug.WriteLine("Reg successful.");
    }

<span data-ttu-id="bba16-211">Ovanstående innebär att appen registreras på meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="bba16-211">The above registers the app with the notification hub.</span></span> <span data-ttu-id="bba16-212">Du är nu redo att sätta igång!</span><span class="sxs-lookup"><span data-stu-id="bba16-212">You are ready to go!</span></span> 

<span data-ttu-id="bba16-213">I `LocationHelper`, inuti hanteraren `Geolocator_PositionChanged`, kan du lägga till en del av en testkod som med tvång placerar platsen inom geofence-området:</span><span class="sxs-lookup"><span data-stu-id="bba16-213">In `LocationHelper`, inside the `Geolocator_PositionChanged` handler, you can add a piece of test code that will forcefully put the location inside the geofence:</span></span>

    await LocationHelper.SendLocationToBackend("wns", "TEST_USER", "TEST", "37.7746", "-122.3858");

<span data-ttu-id="bba16-214">Eftersom vi inte skickar de faktiska koordinaterna (som kanske inte ligger inom gränserna för tillfället) och använder fördefinierade testvärden, kommer vi att se en avisering som visas i uppdateringen:</span><span class="sxs-lookup"><span data-stu-id="bba16-214">Because we are not passing the real coordinates (which might not be within the boundaries at the moment) and are using predefined test values, we will see a notification show up on update:</span></span>

![](./media/notification-hubs-geofence/notification-hubs-test-notification.png)

## <a name="whats-next"></a><span data-ttu-id="bba16-215">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bba16-215">What’s next?</span></span>
<span data-ttu-id="bba16-216">Det finns några steg som du kan behöva följa, förutom det som nämns ovan, för att försäkra dig om att lösningen är klar för produktion.</span><span class="sxs-lookup"><span data-stu-id="bba16-216">There are a couple of steps that you might need to follow in addition to the above to make sure that the solution is production-ready.</span></span>

<span data-ttu-id="bba16-217">Först och främst kan du behöva se till att geofence-områdena är dynamiska.</span><span class="sxs-lookup"><span data-stu-id="bba16-217">First and foremost, you might need to ensure that geofences are dynamic.</span></span> <span data-ttu-id="bba16-218">Detta kräver lite extraarbete med Bing-API:iet för att kunna ladda upp nya gränser inom den befintliga datakällan.</span><span class="sxs-lookup"><span data-stu-id="bba16-218">This will require some extra work with the Bing API in order to be able to upload new boundaries within the existing data source.</span></span> <span data-ttu-id="bba16-219">Läs [API-dokumentationen för Bing Spatial Data Services](https://msdn.microsoft.com/library/ff701734.aspx) för mer information om ämnet.</span><span class="sxs-lookup"><span data-stu-id="bba16-219">Consult the [Bing Spatial Data Services API documentation](https://msdn.microsoft.com/library/ff701734.aspx) for more details on the subject.</span></span>

<span data-ttu-id="bba16-220">Och för det andra: När du jobbar med att säkerställa att leveransen görs till rätt deltagare kanske du vill rikta in dig på dem med hjälp av [taggning](notification-hubs-tags-segment-push-message.md).</span><span class="sxs-lookup"><span data-stu-id="bba16-220">Second, as you are working to ensure that the delivery is done to the right participants, you might want to target them via [tagging](notification-hubs-tags-segment-push-message.md).</span></span>

<span data-ttu-id="bba16-221">Lösningen ovan beskriver ett scenario där du kan ha en stor mängd olika målplattformar. Därför begränsar vi inte denna geofencing till systemspecifika funktioner.</span><span class="sxs-lookup"><span data-stu-id="bba16-221">The solution shown above describes a scenario in which you might have a wide variety of target platforms, so we did not limit the geofencing to system-specific capabilities.</span></span> <span data-ttu-id="bba16-222">Men det bör understrykas att UWP har inbyggda, [kraftfulla funktioner för att detektera geofence-områden](https://msdn.microsoft.com/windows/uwp/maps-and-location/set-up-a-geofence).</span><span class="sxs-lookup"><span data-stu-id="bba16-222">That said, the Universal Windows Platform offers capabilities to [detect geofences right out-of-the-box](https://msdn.microsoft.com/windows/uwp/maps-and-location/set-up-a-geofence).</span></span>

<span data-ttu-id="bba16-223">För mer information om funktioner i Notification Hubs, se vår [dokumentationsportal](https://azure.microsoft.com/documentation/services/notification-hubs/).</span><span class="sxs-lookup"><span data-stu-id="bba16-223">For more details regarding Notification Hubs capabilities, check out our [documentation portal](https://azure.microsoft.com/documentation/services/notification-hubs/).</span></span>

