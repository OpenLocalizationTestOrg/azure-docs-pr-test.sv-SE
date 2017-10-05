---
title: 'MyDriving Azure IoT-exempel: Snabbstart | Microsoft Docs'
description: "Kom igång med en app som är en omfattande demonstration av hur du kan skapa en IoT-system med hjälp av Microsoft Azure, inklusive Stream Analytics, Machine Learning och Händelsehubbar."
services: 
documentationcenter: .net
suite: 
author: harikmenon
manager: douge
ms.assetid: f40ea71b-5721-4a6b-a886-53c2e9dffe8f
ms.service: multiple
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: dotnet
ms.topic: article
ms.date: 03/25/2016
ms.author: harikm
ms.openlocfilehash: 031b492df1f186087e7b91102cbb44f552999293
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="mydriving-iot-system-quick-start"></a><span data-ttu-id="8de5f-103">MyDriving IoT system: Snabbstart</span><span class="sxs-lookup"><span data-stu-id="8de5f-103">MyDriving IoT system: Quick start</span></span>
<span data-ttu-id="8de5f-104">MyDriving är ett system som visar designen och implementeringen av en typisk [Sakernas Internet](iot-suite-overview.md) (IoT)-lösning som samlar in telemetri från enheter, bearbetar data i molnet och använder maskininlärning att ange en anpassningsbar svar.</span><span class="sxs-lookup"><span data-stu-id="8de5f-104">MyDriving is a system that demonstrates the design and implementation of a typical [Internet of Things](iot-suite-overview.md) (IoT) solution that gathers telemetry from devices, processes that data in the cloud, and applies machine learning to provide an adaptive response.</span></span> <span data-ttu-id="8de5f-105">Demonstrationen av loggar data om din bil resor, med hjälp av data från både din mobiltelefon och ett kort som samlar in information från en bil kontrollsystem.</span><span class="sxs-lookup"><span data-stu-id="8de5f-105">The demonstration logs data about your car trips, by using data from both your mobile phone and an adapter that collects information from your car’s control system.</span></span> <span data-ttu-id="8de5f-106">Dessa data används för att ge feedback om formatmallen intresseväckande jämförelse med andra användare.</span><span class="sxs-lookup"><span data-stu-id="8de5f-106">It uses this data to provide feedback on your driving style in comparison to other users.</span></span>

<span data-ttu-id="8de5f-107">Det verkliga syftet med MyDriving är att komma igång med att skapa en egen IoT-lösning.</span><span class="sxs-lookup"><span data-stu-id="8de5f-107">The real purpose of MyDriving is to get you started in creating your own IoT solution.</span></span> <span data-ttu-id="8de5f-108">Men innan som ska få igång med MyDriving själva appen--som medlem i vår test användare team.</span><span class="sxs-lookup"><span data-stu-id="8de5f-108">But before that, let’s get you going with the MyDriving app itself--as a member of our test user team.</span></span> <span data-ttu-id="8de5f-109">Detta ger dig en upplevelse av appen och systemet bakom det som en konsument innan du detaljerad information om arkitekturen.</span><span class="sxs-lookup"><span data-stu-id="8de5f-109">This gives you an experience of the app and the system behind it as a consumer, before you delve into the architecture.</span></span> <span data-ttu-id="8de5f-110">Den också ger en introduktion till HockeyApp, ett kall sätt att hantera dina appar alpha och beta-distributioner att testa användare.</span><span class="sxs-lookup"><span data-stu-id="8de5f-110">It also introduces you to HockeyApp, a cool way of managing the alpha and beta distributions of your apps to test users.</span></span>

## <a name="use-the-mobile-experience"></a><span data-ttu-id="8de5f-111">Använd mobila upplevelsen</span><span class="sxs-lookup"><span data-stu-id="8de5f-111">Use the mobile experience</span></span>
<span data-ttu-id="8de5f-112">Du kan använda appen MyDriving om du har en Android, iOS eller Windows 10-enhet.</span><span class="sxs-lookup"><span data-stu-id="8de5f-112">You can use the MyDriving app if you have an Android, iOS, or Windows 10 device.</span></span>

### <a name="android-and-windows-10-mobile-installation"></a><span data-ttu-id="8de5f-113">Android och Windows 10 Mobile-installation</span><span class="sxs-lookup"><span data-stu-id="8de5f-113">Android and Windows 10 Mobile installation</span></span>
<span data-ttu-id="8de5f-114">På din enhet:</span><span class="sxs-lookup"><span data-stu-id="8de5f-114">On your device:</span></span>

1. <span data-ttu-id="8de5f-115">Tillåt att utveckla appar:</span><span class="sxs-lookup"><span data-stu-id="8de5f-115">Allow development apps:</span></span>
   
   * <span data-ttu-id="8de5f-116">Android: I **inställningar** > **säkerhet**, Tillåt appar från **okända källor**.</span><span class="sxs-lookup"><span data-stu-id="8de5f-116">Android: In **Settings** > **Security**, allow apps from **Unknown sources**.</span></span>
   * <span data-ttu-id="8de5f-117">Windows 10: I **inställningar** > **uppdateringar** > **för utvecklare**, ange **utvecklarläge**.</span><span class="sxs-lookup"><span data-stu-id="8de5f-117">Windows 10: In **Settings** > **Updates** > **For Developers**, set **Developer mode**.</span></span>
2. <span data-ttu-id="8de5f-118">Ansluta till vår beta testgruppen genom att registrera sig med eller logga in på, [HockeyApp](https://rink.hockeyapp.net).</span><span class="sxs-lookup"><span data-stu-id="8de5f-118">Join our beta test team by signing up with, or signing in to, [HockeyApp](https://rink.hockeyapp.net).</span></span> <span data-ttu-id="8de5f-119">HockeyApp gör det enkelt att distribuera tidigare versioner av appen för att testa användare.</span><span class="sxs-lookup"><span data-stu-id="8de5f-119">HockeyApp makes it easy to distribute early releases of your app to test users.</span></span>
   
   <span data-ttu-id="8de5f-120">Om du använder Windows 10 kan du använda Edge-webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="8de5f-120">If you’re using Windows 10, use the Edge browser.</span></span>
   
   <span data-ttu-id="8de5f-121">Om du har en Build 2016 deltagare, logga in med samma Microsoft-konto e-postmeddelandet du registrerat för konferens, genom att använda ett Microsoft-knappar.</span><span class="sxs-lookup"><span data-stu-id="8de5f-121">If you were a Build 2016 attendee, sign in with the same Microsoft account email that you registered for the conference, by using one of the Microsoft buttons.</span></span> <span data-ttu-id="8de5f-122">Du har redan loggat med HockeyApp.</span><span class="sxs-lookup"><span data-stu-id="8de5f-122">You’re already signed up with HockeyApp.</span></span>
   
   ![HockeyApp inloggningssida](./media/iot-solution-get-started/image1.png)
3. <span data-ttu-id="8de5f-124">Hämta och installera appen från den här:</span><span class="sxs-lookup"><span data-stu-id="8de5f-124">Download and install the app from here:</span></span>
   
   * [<span data-ttu-id="8de5f-125">Android</span><span class="sxs-lookup"><span data-stu-id="8de5f-125">Android</span></span>](http://rink.io/spMyDrivingAndroid)
   * [<span data-ttu-id="8de5f-126">Windows 10</span><span class="sxs-lookup"><span data-stu-id="8de5f-126">Windows 10</span></span>](http://rink.io/spMyDrivingUWP)
   
   <span data-ttu-id="8de5f-127">Det finns två objekt.</span><span class="sxs-lookup"><span data-stu-id="8de5f-127">There are two items.</span></span> <span data-ttu-id="8de5f-128">Installera certifikatet i **betrodda personer**.</span><span class="sxs-lookup"><span data-stu-id="8de5f-128">Install the certificate in **Trusted People**.</span></span> <span data-ttu-id="8de5f-129">Installera appen.</span><span class="sxs-lookup"><span data-stu-id="8de5f-129">Then install the app.</span></span>

<span data-ttu-id="8de5f-130">*Eventuella problem som startar appen på Windows 10 Mobile?*</span><span class="sxs-lookup"><span data-stu-id="8de5f-130">*Any issues starting the app on Windows 10 Mobile?*</span></span> <span data-ttu-id="8de5f-131">Din telefon kan vara en uppdatering eller två bakom.</span><span class="sxs-lookup"><span data-stu-id="8de5f-131">Your phone might be an update or two behind.</span></span> <span data-ttu-id="8de5f-132">Kontrollera att du har de senaste uppdateringarna eller installera:</span><span class="sxs-lookup"><span data-stu-id="8de5f-132">Make sure you've got the latest updates, or install:</span></span>

* [<span data-ttu-id="8de5f-133">Microsoft.NET.Native.Framework.1.2.appx</span><span class="sxs-lookup"><span data-stu-id="8de5f-133">Microsoft.NET.Native.Framework.1.2.appx</span></span>](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Framework.1.2.appx) 
* [<span data-ttu-id="8de5f-134">Microsoft.NET.Native.Runtime.1.1.appx</span><span class="sxs-lookup"><span data-stu-id="8de5f-134">Microsoft.NET.Native.Runtime.1.1.appx</span></span>](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Runtime.1.1.appx) 
* [<span data-ttu-id="8de5f-135">Microsoft.VCLibs.ARM.14.00.appx</span><span class="sxs-lookup"><span data-stu-id="8de5f-135">Microsoft.VCLibs.ARM.14.00.appx</span></span>](https://download.hockeyapp.net/packages/win10/Microsoft.VCLibs.ARM.14.00.appx)

### <a name="ios-installation"></a><span data-ttu-id="8de5f-136">iOS-installation</span><span class="sxs-lookup"><span data-stu-id="8de5f-136">iOS installation</span></span>
<span data-ttu-id="8de5f-137">Om du var Build 2016 hämta appen som en medlem i vår test-teamet på HockeyApp:</span><span class="sxs-lookup"><span data-stu-id="8de5f-137">If you attended Build 2016, download the app as a member of our test team on HockeyApp:</span></span>

1. <span data-ttu-id="8de5f-138">Logga in på din iOS-enhet till [HockeyApp](https://rink.hockeyapp.net).</span><span class="sxs-lookup"><span data-stu-id="8de5f-138">On your iOS device, sign in to [HockeyApp](https://rink.hockeyapp.net).</span></span>
   <span data-ttu-id="8de5f-139">Använd någon av Microsoft-inloggning knapparna och logga in med samma Microsoft-konto e-postmeddelandet som du har registrerat med konferens.</span><span class="sxs-lookup"><span data-stu-id="8de5f-139">Use one of the Microsoft sign-in buttons, and sign in with the same Microsoft account email that you registered with the conference.</span></span> <span data-ttu-id="8de5f-140">(Använd inte fälten e-post och lösenord.)</span><span class="sxs-lookup"><span data-stu-id="8de5f-140">(Don’t use the email and password fields.)</span></span>
   
   ![HockeyApp inloggningssida](./media/iot-solution-get-started/image1.png)
2. <span data-ttu-id="8de5f-142">Välj MyDriving på HockeyApp-instrumentpanel och ladda ned den.</span><span class="sxs-lookup"><span data-stu-id="8de5f-142">In the HockeyApp dashboard, select MyDriving and download it.</span></span>
3. <span data-ttu-id="8de5f-143">Auktorisera betaversion från HockeyApp:</span><span class="sxs-lookup"><span data-stu-id="8de5f-143">Authorize the beta release from HockeyApp:</span></span>
   
   <span data-ttu-id="8de5f-144">a.</span><span class="sxs-lookup"><span data-stu-id="8de5f-144">a.</span></span> <span data-ttu-id="8de5f-145">Gå till **inställningar** > **allmänna** > **profiler och hantering av enheter.**</span><span class="sxs-lookup"><span data-stu-id="8de5f-145">Go to **Settings** > **General** > **Profiles and Device Management.**</span></span>
   
   <span data-ttu-id="8de5f-146">b.</span><span class="sxs-lookup"><span data-stu-id="8de5f-146">b.</span></span> <span data-ttu-id="8de5f-147">Förtroende för den **bitars Stadium GmbH** certifikat.</span><span class="sxs-lookup"><span data-stu-id="8de5f-147">Trust the **Bit Stadium GmbH** certificate.</span></span>

<span data-ttu-id="8de5f-148">Om du inte tar tag Build 2016, kan du skapa och distribuera appen själv:</span><span class="sxs-lookup"><span data-stu-id="8de5f-148">If you didn’t attend Build 2016, you can build and deploy the app yourself:</span></span>

1. <span data-ttu-id="8de5f-149">Hämta koden [från GitHub].</span><span class="sxs-lookup"><span data-stu-id="8de5f-149">Download the code [from GitHub].</span></span>
2. <span data-ttu-id="8de5f-150">Skapa och distribuera med [med Xamarin].</span><span class="sxs-lookup"><span data-stu-id="8de5f-150">Build and deploy by [using Xamarin].</span></span>

<span data-ttu-id="8de5f-151">Mer information i den [MyDriving referenshandboken](http://aka.ms/mydrivingdocs).</span><span class="sxs-lookup"><span data-stu-id="8de5f-151">Find more details in the [MyDriving Reference Guide](http://aka.ms/mydrivingdocs).</span></span>

## <a name="get-an-obd-adapter-optional"></a><span data-ttu-id="8de5f-152">Hämta ett OBD-kort (valfritt)</span><span class="sxs-lookup"><span data-stu-id="8de5f-152">Get an OBD adapter (optional)</span></span>
<span data-ttu-id="8de5f-153">Detta är den del som är detta en verklig Sakernas Internet system!</span><span class="sxs-lookup"><span data-stu-id="8de5f-153">This is the part that makes this a real Internet of Things system!</span></span> <span data-ttu-id="8de5f-154">Du kan använda appen utan en, men det är roligt med verkligt och de är inte dyrt.</span><span class="sxs-lookup"><span data-stu-id="8de5f-154">You can use the app without one, but it’s more fun with the real thing, and they aren’t expensive.</span></span>

<span data-ttu-id="8de5f-155">Inbyggd diagnostik (OBD) är en funktion av bilen garaget använder för att finjustera bilen och diagnostisera udda brus och varning ljus.</span><span class="sxs-lookup"><span data-stu-id="8de5f-155">On-board diagnostics (OBD) is the feature of your car that the garage uses to tune up your car and diagnose odd noises and warning lamps.</span></span> <span data-ttu-id="8de5f-156">Om inte bilen är för stor antiquity, hittar du en socket någonstans i utrustning vanligtvis bakom en flik under instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="8de5f-156">Unless your car is of great antiquity, you’ll find a socket somewhere in the cabin, typically behind a flap under the dashboard.</span></span> <span data-ttu-id="8de5f-157">Med rätt-anslutningen kan du hämta mätvärden för motorns prestanda och gör vissa ändringar.</span><span class="sxs-lookup"><span data-stu-id="8de5f-157">With the right connector, you can get metrics of the engine’s performance and make certain adjustments.</span></span> <span data-ttu-id="8de5f-158">En OBD-koppling kan köpas billigt på vanligt sätt.</span><span class="sxs-lookup"><span data-stu-id="8de5f-158">An OBD connector can be purchased cheaply from the usual places.</span></span> <span data-ttu-id="8de5f-159">Ansluter med hjälp av Bluetooth- eller Wi-Fi till en app på din telefon.</span><span class="sxs-lookup"><span data-stu-id="8de5f-159">It connects by using Bluetooth or Wi-Fi to an app on your phone.</span></span>

<span data-ttu-id="8de5f-160">I det här fallet är det dags att ansluta bilen till molnet.</span><span class="sxs-lookup"><span data-stu-id="8de5f-160">In this case, we’re going to connect your car to the cloud.</span></span> <span data-ttu-id="8de5f-161">Direktanslutning från OBD är att din telefon, men vår app fungerar som ett relä.</span><span class="sxs-lookup"><span data-stu-id="8de5f-161">The direct connection from the OBD is to your phone, but our app works as a relay.</span></span> <span data-ttu-id="8de5f-162">En bil telemetri skickas direkt till MyDriving IoT-hubb där den bearbetas logga körda mil och utvärdera din intresseväckande stil.</span><span class="sxs-lookup"><span data-stu-id="8de5f-162">Your car's telemetry is sent straight to the MyDriving IoT hub, where it's processed to log your road trips and assess your driving style.</span></span>

<span data-ttu-id="8de5f-163">Att ansluta en OBD-enhet:</span><span class="sxs-lookup"><span data-stu-id="8de5f-163">To connect an OBD device:</span></span>

1. <span data-ttu-id="8de5f-164">Kontrollera att bilen har en OBD-socket.</span><span class="sxs-lookup"><span data-stu-id="8de5f-164">Check that your car has an OBD socket.</span></span>
2. <span data-ttu-id="8de5f-165">Hämta ett OBD-nätverkskort:</span><span class="sxs-lookup"><span data-stu-id="8de5f-165">Obtain an OBD adapter:</span></span>
   
   * <span data-ttu-id="8de5f-166">Om du använder en Android eller Windows phone måste ett Bluetooth-aktiverade OBD II-kort.</span><span class="sxs-lookup"><span data-stu-id="8de5f-166">If you're using an Android or Windows phone, you need a Bluetooth-enabled OBD II adapter.</span></span> <span data-ttu-id="8de5f-167">Vi använde [BAFX produkter 34t5 Bluetooth OBDII Scan Tool].</span><span class="sxs-lookup"><span data-stu-id="8de5f-167">We used [BAFX Products 34t5 Bluetooth OBDII Scan Tool].</span></span>
   * <span data-ttu-id="8de5f-168">Om du använder en iOS-telefoner, måste ett Wi-Fi-aktiverade OBD-kort.</span><span class="sxs-lookup"><span data-stu-id="8de5f-168">If you're using an iOS phone, you need a Wi-Fi-enabled OBD adapter.</span></span> <span data-ttu-id="8de5f-169">Vi använde [Sökningsverktyget OBDLink MX Wi-Fi: OBD-kort/diagnostik skanner].</span><span class="sxs-lookup"><span data-stu-id="8de5f-169">We used [ScanTool OBDLink MX Wi-Fi: OBD Adapter/Diagnostic Scanner].</span></span>
3. <span data-ttu-id="8de5f-170">Följ instruktionerna som medföljer OBD kortet att ansluta till din telefon.</span><span class="sxs-lookup"><span data-stu-id="8de5f-170">Follow the instructions that come with your OBD adapter to connect it to your phone.</span></span> <span data-ttu-id="8de5f-171">Tänk på följande:</span><span class="sxs-lookup"><span data-stu-id="8de5f-171">Keep the following in mind:</span></span>
   
   * <span data-ttu-id="8de5f-172">En Bluetooth-adapter måste kombineras med telefonen och på den **inställningar** sidan.</span><span class="sxs-lookup"><span data-stu-id="8de5f-172">A Bluetooth adapter must be paired with the phone, on the **Settings** page.</span></span>
   * <span data-ttu-id="8de5f-173">En Wi-Fi-nätverkskortet ha en adress i intervallet 192.168.xxx.xxx.</span><span class="sxs-lookup"><span data-stu-id="8de5f-173">A Wi-Fi adapter must have an address in the range 192.168.xxx.xxx.</span></span>
4. <span data-ttu-id="8de5f-174">Om du har flera bilar kan hämta du ett separat nätverkskort för varje (högst tre).</span><span class="sxs-lookup"><span data-stu-id="8de5f-174">If you have several cars, you can get a separate adapter for each (maximum of three).</span></span>

<span data-ttu-id="8de5f-175">Om du inte har ett kort OBD-appen kommer fortfarande att skicka data för platsen och hastighet från telefonens GPS-mottagare till serverdelen och ber om du vill simulera en OBD.</span><span class="sxs-lookup"><span data-stu-id="8de5f-175">If you don’t have an OBD adapter, the app will still send location and speed data from the phone's GPS receiver to the back end and will ask if you want to simulate an OBD.</span></span>

<span data-ttu-id="8de5f-176">Du hittar mer information om hur appen använder data från OBD-kort och alternativ för att skapa OBD-enheten i 2.1, ”IoT-enheter” i den [MyDriving referenshandboken](http://aka.ms/mydrivingdocs).</span><span class="sxs-lookup"><span data-stu-id="8de5f-176">You can find out more about how the app uses data from the OBD adapter and about options for creating your own OBD device in section 2.1, "IoT Devices," in the [MyDriving Reference Guide](http://aka.ms/mydrivingdocs).</span></span>

## <a name="use-the-app"></a><span data-ttu-id="8de5f-177">Använd appen</span><span class="sxs-lookup"><span data-stu-id="8de5f-177">Use the app</span></span>
<span data-ttu-id="8de5f-178">Starta appen.</span><span class="sxs-lookup"><span data-stu-id="8de5f-178">Start the app.</span></span> <span data-ttu-id="8de5f-179">Det finns en inledande Snabbstart som leder dig igenom hur det fungerar.</span><span class="sxs-lookup"><span data-stu-id="8de5f-179">There’s an initial Quickstart to walk you through how it works.</span></span>

### <a name="track-your-trips"></a><span data-ttu-id="8de5f-180">Spåra dina resor</span><span class="sxs-lookup"><span data-stu-id="8de5f-180">Track your trips</span></span>
<span data-ttu-id="8de5f-181">Knacka på posten (stort röd cirkel längst ned på skärmen) för att starta en resa och tryck på igen om du vill avsluta.</span><span class="sxs-lookup"><span data-stu-id="8de5f-181">Tap the record button (big red circle at the bottom of the screen) to start a trip, and tap again to end.</span></span>

![Bild av knappen post för resa spårning](./media/iot-solution-get-started/image2.png)

<span data-ttu-id="8de5f-183">Varje gång du startar en resa, om det finns ingen OBD-enhet, får du en fråga om du vill använda simulatorn.</span><span class="sxs-lookup"><span data-stu-id="8de5f-183">Each time you start a trip, if there’s no OBD device, you’ll be asked if you want to use the simulator.</span></span>

<span data-ttu-id="8de5f-184">Klicka på Stopp i slutet av en resa och du får en översikt.</span><span class="sxs-lookup"><span data-stu-id="8de5f-184">At the end of a trip, tap the stop button, and you get a summary.</span></span>

![Exempel på en sammanfattande resa](./media/iot-solution-get-started/image3.png)

### <a name="review-your-trips"></a><span data-ttu-id="8de5f-186">Granska dina resor</span><span class="sxs-lookup"><span data-stu-id="8de5f-186">Review your trips</span></span>
![Exempel på en tidigare resa](./media/iot-solution-get-started/image4.png)

### <a name="review-your-profile"></a><span data-ttu-id="8de5f-188">Granska din profil</span><span class="sxs-lookup"><span data-stu-id="8de5f-188">Review your profile</span></span>
![Exempel på en profil för körning-format](./media/iot-solution-get-started/image5.png)

## <a name="send-us-your-test-feedback"></a><span data-ttu-id="8de5f-190">Skicka test-feedback</span><span class="sxs-lookup"><span data-stu-id="8de5f-190">Send us your test feedback</span></span>
<span data-ttu-id="8de5f-191">Eftersom vi skapade MyDriving att rivstart IoT systemen vill vi verkligen veta hur det fungerar.</span><span class="sxs-lookup"><span data-stu-id="8de5f-191">Because we created MyDriving to help jumpstart your own IoT systems, we certainly want to hear from you about how well it works.</span></span> <span data-ttu-id="8de5f-192">Berätta för oss om:</span><span class="sxs-lookup"><span data-stu-id="8de5f-192">Let us know if:</span></span>

* <span data-ttu-id="8de5f-193">Du stöter på problem eller utmaningar.</span><span class="sxs-lookup"><span data-stu-id="8de5f-193">You run into difficulties or challenges.</span></span>
* <span data-ttu-id="8de5f-194">Det finns en plats för tillägg som gör det passar bättre för ditt scenario.</span><span class="sxs-lookup"><span data-stu-id="8de5f-194">There is an extension point that would make it more suitable to your scenario.</span></span>
* <span data-ttu-id="8de5f-195">Du hittar ett mer effektivt sätt att utföra vissa behov.</span><span class="sxs-lookup"><span data-stu-id="8de5f-195">You find a more efficient way to accomplish certain needs.</span></span>
* <span data-ttu-id="8de5f-196">Du har några förslag för att förbättra MyDriving eller den här dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="8de5f-196">You have any other suggestions for improving MyDriving or this documentation.</span></span>

<span data-ttu-id="8de5f-197">Inom MyDriving själva appen, du kan använda den inbyggda HockeyApp feedback mekanismen: på iOS och Android bara ge din telefon en shake eller använda den **Feedback** kommando.</span><span class="sxs-lookup"><span data-stu-id="8de5f-197">Within the MyDriving app itself, you can use the built-in HockeyApp feedback mechanism: on iOS and Android, just give your phone a shake, or use the **Feedback** menu command.</span></span> <span data-ttu-id="8de5f-198">Detta ska koppla en skärmbild automatiskt så att vi vet vad du pratar om.</span><span class="sxs-lookup"><span data-stu-id="8de5f-198">This will automatically attach a screenshot, so that we’ll know what you’re talking about.</span></span> <span data-ttu-id="8de5f-199">Och om det finns några olycklig krascher, HockeyApp samlar in krascher loggarna om du vill berätta om dem.</span><span class="sxs-lookup"><span data-stu-id="8de5f-199">And if there are any unfortunate crashes, HockeyApp collects the crash logs to tell us about them.</span></span> <span data-ttu-id="8de5f-200">Du kan också ge feedback via den [HockeyApp-portalen].</span><span class="sxs-lookup"><span data-stu-id="8de5f-200">You can also give feedback through the [HockeyApp portal].</span></span>

<span data-ttu-id="8de5f-201">Du kan också lagra en [problemet på GitHub], eller lämna en kommentar nedan (SV-oss edition).</span><span class="sxs-lookup"><span data-stu-id="8de5f-201">You can also file an [issue on GitHub], or leave a comment below (en-us edition).</span></span>

<span data-ttu-id="8de5f-202">Vi hoppas att få höra från dig!</span><span class="sxs-lookup"><span data-stu-id="8de5f-202">We look forward to hearing from you!</span></span>

## <a name="next-steps"></a><span data-ttu-id="8de5f-203">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8de5f-203">Next steps</span></span>
* <span data-ttu-id="8de5f-204">Utforska den [MyDriving referenshandboken](http://aka.ms/mydrivingdocs) att förstå hur vi har utformat och skapat MyDriving hela systemet.</span><span class="sxs-lookup"><span data-stu-id="8de5f-204">Explore the [MyDriving Reference Guide](http://aka.ms/mydrivingdocs) to understand how we’ve designed and built the entire MyDriving system.</span></span>
* <span data-ttu-id="8de5f-205">[Skapa och distribuera ett system med din egen](iot-solution-build-system.md) med hjälp av vår Azure Resource Manager-skript.</span><span class="sxs-lookup"><span data-stu-id="8de5f-205">[Create and deploy a system of your own](iot-solution-build-system.md) by using our Azure Resource Manager scripts.</span></span> <span data-ttu-id="8de5f-206">Den [MyDriving referenshandboken](http://aka.ms/mydrivingdocs) hjälper dig också att områden där du ska göra de anpassningar.</span><span class="sxs-lookup"><span data-stu-id="8de5f-206">The [MyDriving Reference Guide](http://aka.ms/mydrivingdocs) also guides you through areas where you’ll make the most customizations.</span></span>

<span data-ttu-id="8de5f-207">[från GitHub]: https://github.com/Azure-Samples/MyDriving</span><span class="sxs-lookup"><span data-stu-id="8de5f-207">[from GitHub]: https://github.com/Azure-Samples/MyDriving</span></span>
<span data-ttu-id="8de5f-208">[med Xamarin]: https://developer.xamarin.com/guides/ios/getting_started/installation/</span><span class="sxs-lookup"><span data-stu-id="8de5f-208">[using Xamarin]: https://developer.xamarin.com/guides/ios/getting_started/installation/</span></span>
<span data-ttu-id="8de5f-209">[BAFX produkter 34t5 Bluetooth OBDII Scan Tool]: http://www.amazon.com/gp/product/B005NLQAHS</span><span class="sxs-lookup"><span data-stu-id="8de5f-209">[BAFX Products 34t5 Bluetooth OBDII Scan Tool]: http://www.amazon.com/gp/product/B005NLQAHS</span></span>
<span data-ttu-id="8de5f-210">[Sökningsverktyget OBDLink MX Wi-Fi: OBD-kort/diagnostik skanner]: http://www.amazon.com/gp/product/B00OCYXTYY/ref=s9_simh_gw_g263_i1_r?pf_rd_m=ATVPDKIKX0DER&pf_rd_s=desktop-2&pf_rd_r=1MWRMKXK4KK9VYMJ44MP</span><span class="sxs-lookup"><span data-stu-id="8de5f-210">[ScanTool OBDLink MX Wi-Fi: OBD Adapter/Diagnostic Scanner]: http://www.amazon.com/gp/product/B00OCYXTYY/ref=s9_simh_gw_g263_i1_r?pf_rd_m=ATVPDKIKX0DER&pf_rd_s=desktop-2&pf_rd_r=1MWRMKXK4KK9VYMJ44MP</span></span>
<span data-ttu-id="8de5f-211">[HockeyApp-portalen]: https://rink.hockeyapp.org</span><span class="sxs-lookup"><span data-stu-id="8de5f-211">[HockeyApp portal]: https://rink.hockeyapp.org</span></span>
<span data-ttu-id="8de5f-212">[problemet på GitHub]: https://github.com/Azure-Samples/MyDriving/issues</span><span class="sxs-lookup"><span data-stu-id="8de5f-212">[issue on GitHub]: https://github.com/Azure-Samples/MyDriving/issues</span></span>
