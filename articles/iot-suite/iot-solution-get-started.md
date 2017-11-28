---
title: 'MyDriving Azure IoT-exempel: Snabbstart | Microsoft Docs'
description: "Kom igång med en app som är en omfattande demonstration av hur tooarchitect en IoT-system med hjälp av Microsoft Azure, inklusive Stream Analytics, Machine Learning och Händelsehubbar."
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
ms.openlocfilehash: 411b9a992deb22b915f8291d8559e2917d976b2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="mydriving-iot-system-quick-start"></a><span data-ttu-id="fd143-103">MyDriving IoT system: Snabbstart</span><span class="sxs-lookup"><span data-stu-id="fd143-103">MyDriving IoT system: Quick start</span></span>
<span data-ttu-id="fd143-104">MyDriving är ett system som visar hello designen och implementeringen av en typisk [Sakernas Internet](iot-suite-overview.md) (IoT)-lösning som samlar in telemetri från enheter, bearbetar data i molnet hello och tillämpar maskininlärning tooprovide ett anpassningsbar svar.</span><span class="sxs-lookup"><span data-stu-id="fd143-104">MyDriving is a system that demonstrates hello design and implementation of a typical [Internet of Things](iot-suite-overview.md) (IoT) solution that gathers telemetry from devices, processes that data in hello cloud, and applies machine learning tooprovide an adaptive response.</span></span> <span data-ttu-id="fd143-105">hello demonstration loggar data om din bil resor, med hjälp av data från både din mobiltelefon och ett kort som samlar in information från en bil kontrollsystem.</span><span class="sxs-lookup"><span data-stu-id="fd143-105">hello demonstration logs data about your car trips, by using data from both your mobile phone and an adapter that collects information from your car’s control system.</span></span> <span data-ttu-id="fd143-106">Den använder denna data tooprovide feedback på formatmallen intresseväckande jämförelse tooother användare.</span><span class="sxs-lookup"><span data-stu-id="fd143-106">It uses this data tooprovide feedback on your driving style in comparison tooother users.</span></span>

<span data-ttu-id="fd143-107">hello verkliga syftet med MyDriving är tooget som du startade i att skapa en egen IoT-lösning.</span><span class="sxs-lookup"><span data-stu-id="fd143-107">hello real purpose of MyDriving is tooget you started in creating your own IoT solution.</span></span> <span data-ttu-id="fd143-108">Men innan som ska få igång med hello MyDriving själva appen--som medlem i vår test användare team.</span><span class="sxs-lookup"><span data-stu-id="fd143-108">But before that, let’s get you going with hello MyDriving app itself--as a member of our test user team.</span></span> <span data-ttu-id="fd143-109">Detta ger dig en upplevelse av hello app och hello system bakom det som en konsument innan du ger dig i hello-arkitekturen.</span><span class="sxs-lookup"><span data-stu-id="fd143-109">This gives you an experience of hello app and hello system behind it as a consumer, before you delve into hello architecture.</span></span> <span data-ttu-id="fd143-110">Introducerar även du tooHockeyApp kall sätt för att hantera hello alpha och beta distribution av appar tootest användarna.</span><span class="sxs-lookup"><span data-stu-id="fd143-110">It also introduces you tooHockeyApp, a cool way of managing hello alpha and beta distributions of your apps tootest users.</span></span>

## <a name="use-hello-mobile-experience"></a><span data-ttu-id="fd143-111">Använd hello mobila upplevelsen</span><span class="sxs-lookup"><span data-stu-id="fd143-111">Use hello mobile experience</span></span>
<span data-ttu-id="fd143-112">Du kan använda hello MyDriving app om du har en Android, iOS eller Windows 10-enhet.</span><span class="sxs-lookup"><span data-stu-id="fd143-112">You can use hello MyDriving app if you have an Android, iOS, or Windows 10 device.</span></span>

### <a name="android-and-windows-10-mobile-installation"></a><span data-ttu-id="fd143-113">Android och Windows 10 Mobile-installation</span><span class="sxs-lookup"><span data-stu-id="fd143-113">Android and Windows 10 Mobile installation</span></span>
<span data-ttu-id="fd143-114">På din enhet:</span><span class="sxs-lookup"><span data-stu-id="fd143-114">On your device:</span></span>

1. <span data-ttu-id="fd143-115">Tillåt att utveckla appar:</span><span class="sxs-lookup"><span data-stu-id="fd143-115">Allow development apps:</span></span>
   
   * <span data-ttu-id="fd143-116">Android: I **inställningar** > **säkerhet**, Tillåt appar från **okända källor**.</span><span class="sxs-lookup"><span data-stu-id="fd143-116">Android: In **Settings** > **Security**, allow apps from **Unknown sources**.</span></span>
   * <span data-ttu-id="fd143-117">Windows 10: I **inställningar** > **uppdateringar** > **för utvecklare**, ange **utvecklarläge**.</span><span class="sxs-lookup"><span data-stu-id="fd143-117">Windows 10: In **Settings** > **Updates** > **For Developers**, set **Developer mode**.</span></span>
2. <span data-ttu-id="fd143-118">Ansluta till vår beta testgruppen genom att registrera sig med eller logga in på, [HockeyApp](https://rink.hockeyapp.net).</span><span class="sxs-lookup"><span data-stu-id="fd143-118">Join our beta test team by signing up with, or signing in to, [HockeyApp](https://rink.hockeyapp.net).</span></span> <span data-ttu-id="fd143-119">HockeyApp gör det enkelt toodistribute tidig versioner av app tootest användarna.</span><span class="sxs-lookup"><span data-stu-id="fd143-119">HockeyApp makes it easy toodistribute early releases of your app tootest users.</span></span>
   
   <span data-ttu-id="fd143-120">Om du använder Windows 10, använda hello Edge-webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="fd143-120">If you’re using Windows 10, use hello Edge browser.</span></span>
   
   <span data-ttu-id="fd143-121">Om du har en Build 2016 deltagare, logga in med hello samma Microsoft-konto e-post som har registrerats för hello konferens med någon av hello Microsoft knappar.</span><span class="sxs-lookup"><span data-stu-id="fd143-121">If you were a Build 2016 attendee, sign in with hello same Microsoft account email that you registered for hello conference, by using one of hello Microsoft buttons.</span></span> <span data-ttu-id="fd143-122">Du har redan loggat med HockeyApp.</span><span class="sxs-lookup"><span data-stu-id="fd143-122">You’re already signed up with HockeyApp.</span></span>
   
   ![HockeyApp inloggningssida](./media/iot-solution-get-started/image1.png)
3. <span data-ttu-id="fd143-124">Hämta och installera appen hello härifrån:</span><span class="sxs-lookup"><span data-stu-id="fd143-124">Download and install hello app from here:</span></span>
   
   * [<span data-ttu-id="fd143-125">Android</span><span class="sxs-lookup"><span data-stu-id="fd143-125">Android</span></span>](http://rink.io/spMyDrivingAndroid)
   * [<span data-ttu-id="fd143-126">Windows 10</span><span class="sxs-lookup"><span data-stu-id="fd143-126">Windows 10</span></span>](http://rink.io/spMyDrivingUWP)
   
   <span data-ttu-id="fd143-127">Det finns två objekt.</span><span class="sxs-lookup"><span data-stu-id="fd143-127">There are two items.</span></span> <span data-ttu-id="fd143-128">Installera hello certifikat i **betrodda personer**.</span><span class="sxs-lookup"><span data-stu-id="fd143-128">Install hello certificate in **Trusted People**.</span></span> <span data-ttu-id="fd143-129">Installera sedan hello app.</span><span class="sxs-lookup"><span data-stu-id="fd143-129">Then install hello app.</span></span>

<span data-ttu-id="fd143-130">*Eventuella problem som startar hello app på Windows 10 Mobile?*</span><span class="sxs-lookup"><span data-stu-id="fd143-130">*Any issues starting hello app on Windows 10 Mobile?*</span></span> <span data-ttu-id="fd143-131">Din telefon kan vara en uppdatering eller två bakom.</span><span class="sxs-lookup"><span data-stu-id="fd143-131">Your phone might be an update or two behind.</span></span> <span data-ttu-id="fd143-132">Kontrollera att du har fått hello senaste uppdateringar eller installera:</span><span class="sxs-lookup"><span data-stu-id="fd143-132">Make sure you've got hello latest updates, or install:</span></span>

* [<span data-ttu-id="fd143-133">Microsoft.NET.Native.Framework.1.2.appx</span><span class="sxs-lookup"><span data-stu-id="fd143-133">Microsoft.NET.Native.Framework.1.2.appx</span></span>](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Framework.1.2.appx) 
* [<span data-ttu-id="fd143-134">Microsoft.NET.Native.Runtime.1.1.appx</span><span class="sxs-lookup"><span data-stu-id="fd143-134">Microsoft.NET.Native.Runtime.1.1.appx</span></span>](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Runtime.1.1.appx) 
* [<span data-ttu-id="fd143-135">Microsoft.VCLibs.ARM.14.00.appx</span><span class="sxs-lookup"><span data-stu-id="fd143-135">Microsoft.VCLibs.ARM.14.00.appx</span></span>](https://download.hockeyapp.net/packages/win10/Microsoft.VCLibs.ARM.14.00.appx)

### <a name="ios-installation"></a><span data-ttu-id="fd143-136">iOS-installation</span><span class="sxs-lookup"><span data-stu-id="fd143-136">iOS installation</span></span>
<span data-ttu-id="fd143-137">Om du var Build 2016 hämta hello appen som en medlem i vår test-teamet på HockeyApp:</span><span class="sxs-lookup"><span data-stu-id="fd143-137">If you attended Build 2016, download hello app as a member of our test team on HockeyApp:</span></span>

1. <span data-ttu-id="fd143-138">På iOS-enheten, logga in för[HockeyApp](https://rink.hockeyapp.net).</span><span class="sxs-lookup"><span data-stu-id="fd143-138">On your iOS device, sign in too[HockeyApp](https://rink.hockeyapp.net).</span></span>
   <span data-ttu-id="fd143-139">Använd en av hello Microsoft inloggning knappar och loggar in med hello samma Microsoft-konto e-post som du har registrerat med hello konferens.</span><span class="sxs-lookup"><span data-stu-id="fd143-139">Use one of hello Microsoft sign-in buttons, and sign in with hello same Microsoft account email that you registered with hello conference.</span></span> <span data-ttu-id="fd143-140">(Använd inte hello fälten för e-post och lösenord.)</span><span class="sxs-lookup"><span data-stu-id="fd143-140">(Don’t use hello email and password fields.)</span></span>
   
   ![HockeyApp inloggningssida](./media/iot-solution-get-started/image1.png)
2. <span data-ttu-id="fd143-142">Välj MyDriving hello HockeyApp instrumentpanelen och ladda ned den.</span><span class="sxs-lookup"><span data-stu-id="fd143-142">In hello HockeyApp dashboard, select MyDriving and download it.</span></span>
3. <span data-ttu-id="fd143-143">Auktorisera hello betaversion från HockeyApp:</span><span class="sxs-lookup"><span data-stu-id="fd143-143">Authorize hello beta release from HockeyApp:</span></span>
   
   <span data-ttu-id="fd143-144">a.</span><span class="sxs-lookup"><span data-stu-id="fd143-144">a.</span></span> <span data-ttu-id="fd143-145">Gå för**inställningar** > **allmänna** > **profiler och hantering av enheter.**</span><span class="sxs-lookup"><span data-stu-id="fd143-145">Go too**Settings** > **General** > **Profiles and Device Management.**</span></span>
   
   <span data-ttu-id="fd143-146">b.</span><span class="sxs-lookup"><span data-stu-id="fd143-146">b.</span></span> <span data-ttu-id="fd143-147">Förtroende hello **bitars Stadium GmbH** certifikat.</span><span class="sxs-lookup"><span data-stu-id="fd143-147">Trust hello **Bit Stadium GmbH** certificate.</span></span>

<span data-ttu-id="fd143-148">Om du inte tar tag Build 2016, kan du skapa och distribuera hello app själv:</span><span class="sxs-lookup"><span data-stu-id="fd143-148">If you didn’t attend Build 2016, you can build and deploy hello app yourself:</span></span>

1. <span data-ttu-id="fd143-149">Hämta hello koden [från GitHub].</span><span class="sxs-lookup"><span data-stu-id="fd143-149">Download hello code [from GitHub].</span></span>
2. <span data-ttu-id="fd143-150">Skapa och distribuera med [med Xamarin].</span><span class="sxs-lookup"><span data-stu-id="fd143-150">Build and deploy by [using Xamarin].</span></span>

<span data-ttu-id="fd143-151">Mer information finns i hello [MyDriving referenshandboken](http://aka.ms/mydrivingdocs).</span><span class="sxs-lookup"><span data-stu-id="fd143-151">Find more details in hello [MyDriving Reference Guide](http://aka.ms/mydrivingdocs).</span></span>

## <a name="get-an-obd-adapter-optional"></a><span data-ttu-id="fd143-152">Hämta ett OBD-kort (valfritt)</span><span class="sxs-lookup"><span data-stu-id="fd143-152">Get an OBD adapter (optional)</span></span>
<span data-ttu-id="fd143-153">Detta är en del av hello som gör det en verklig Sakernas Internet system!</span><span class="sxs-lookup"><span data-stu-id="fd143-153">This is hello part that makes this a real Internet of Things system!</span></span> <span data-ttu-id="fd143-154">Du kan använda hello app utan ett, men det är roligt med hello verkligt och de är inte dyrt.</span><span class="sxs-lookup"><span data-stu-id="fd143-154">You can use hello app without one, but it’s more fun with hello real thing, and they aren’t expensive.</span></span>

<span data-ttu-id="fd143-155">Inbyggd diagnostik (OBD) är hello funktion i bilen som hello garage använder tootune in bilen och diagnostisera udda brus och varning ljus.</span><span class="sxs-lookup"><span data-stu-id="fd143-155">On-board diagnostics (OBD) is hello feature of your car that hello garage uses tootune up your car and diagnose odd noises and warning lamps.</span></span> <span data-ttu-id="fd143-156">Om inte bilen är för stor antiquity, hittar du en socket någonstans i hello utrustning vanligtvis bakom en flik under hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="fd143-156">Unless your car is of great antiquity, you’ll find a socket somewhere in hello cabin, typically behind a flap under hello dashboard.</span></span> <span data-ttu-id="fd143-157">Med rätt hello-anslutningen kan du hämta mätvärden för hello motorns prestanda och gör vissa ändringar.</span><span class="sxs-lookup"><span data-stu-id="fd143-157">With hello right connector, you can get metrics of hello engine’s performance and make certain adjustments.</span></span> <span data-ttu-id="fd143-158">En OBD-koppling kan köpas billigt från hello vanliga platser.</span><span class="sxs-lookup"><span data-stu-id="fd143-158">An OBD connector can be purchased cheaply from hello usual places.</span></span> <span data-ttu-id="fd143-159">Ansluter med hjälp av Bluetooth- eller Wi-Fi tooan app på din telefon.</span><span class="sxs-lookup"><span data-stu-id="fd143-159">It connects by using Bluetooth or Wi-Fi tooan app on your phone.</span></span>

<span data-ttu-id="fd143-160">I det här fallet vi tooconnect bilen toohello molnet.</span><span class="sxs-lookup"><span data-stu-id="fd143-160">In this case, we’re going tooconnect your car toohello cloud.</span></span> <span data-ttu-id="fd143-161">hello direkt anslutning från hello OBD är tooyour telefon, men vår app fungerar som ett relä.</span><span class="sxs-lookup"><span data-stu-id="fd143-161">hello direct connection from hello OBD is tooyour phone, but our app works as a relay.</span></span> <span data-ttu-id="fd143-162">En bil telemetri skickas raka toohello MyDriving IoT-hubb, där det är bearbetas toolog körda mil och utvärdera din intresseväckande stil.</span><span class="sxs-lookup"><span data-stu-id="fd143-162">Your car's telemetry is sent straight toohello MyDriving IoT hub, where it's processed toolog your road trips and assess your driving style.</span></span>

<span data-ttu-id="fd143-163">tooconnect en OBD-enhet:</span><span class="sxs-lookup"><span data-stu-id="fd143-163">tooconnect an OBD device:</span></span>

1. <span data-ttu-id="fd143-164">Kontrollera att bilen har en OBD-socket.</span><span class="sxs-lookup"><span data-stu-id="fd143-164">Check that your car has an OBD socket.</span></span>
2. <span data-ttu-id="fd143-165">Hämta ett OBD-nätverkskort:</span><span class="sxs-lookup"><span data-stu-id="fd143-165">Obtain an OBD adapter:</span></span>
   
   * <span data-ttu-id="fd143-166">Om du använder en Android eller Windows phone måste ett Bluetooth-aktiverade OBD II-kort.</span><span class="sxs-lookup"><span data-stu-id="fd143-166">If you're using an Android or Windows phone, you need a Bluetooth-enabled OBD II adapter.</span></span> <span data-ttu-id="fd143-167">Vi använde [BAFX produkter 34t5 Bluetooth OBDII Scan Tool].</span><span class="sxs-lookup"><span data-stu-id="fd143-167">We used [BAFX Products 34t5 Bluetooth OBDII Scan Tool].</span></span>
   * <span data-ttu-id="fd143-168">Om du använder en iOS-telefoner, måste ett Wi-Fi-aktiverade OBD-kort.</span><span class="sxs-lookup"><span data-stu-id="fd143-168">If you're using an iOS phone, you need a Wi-Fi-enabled OBD adapter.</span></span> <span data-ttu-id="fd143-169">Vi använde [Sökningsverktyget OBDLink MX Wi-Fi: OBD-kort/diagnostik skanner].</span><span class="sxs-lookup"><span data-stu-id="fd143-169">We used [ScanTool OBDLink MX Wi-Fi: OBD Adapter/Diagnostic Scanner].</span></span>
3. <span data-ttu-id="fd143-170">Följ instruktionerna för hello som medföljer din OBD-kort tooconnect den tooyour phone.</span><span class="sxs-lookup"><span data-stu-id="fd143-170">Follow hello instructions that come with your OBD adapter tooconnect it tooyour phone.</span></span> <span data-ttu-id="fd143-171">Tänk på följande hello:</span><span class="sxs-lookup"><span data-stu-id="fd143-171">Keep hello following in mind:</span></span>
   
   * <span data-ttu-id="fd143-172">En Bluetooth-adapter måste kombineras med hello telefon på hello **inställningar** sidan.</span><span class="sxs-lookup"><span data-stu-id="fd143-172">A Bluetooth adapter must be paired with hello phone, on hello **Settings** page.</span></span>
   * <span data-ttu-id="fd143-173">Wi-Fi-kort måste ha en adress i hello intervallet 192.168.xxx.xxx.</span><span class="sxs-lookup"><span data-stu-id="fd143-173">A Wi-Fi adapter must have an address in hello range 192.168.xxx.xxx.</span></span>
4. <span data-ttu-id="fd143-174">Om du har flera bilar kan hämta du ett separat nätverkskort för varje (högst tre).</span><span class="sxs-lookup"><span data-stu-id="fd143-174">If you have several cars, you can get a separate adapter for each (maximum of three).</span></span>

<span data-ttu-id="fd143-175">Om du inte har ett OBD-kort, hello appen fortfarande skickar plats och hastighet data från hello phone GPS-mottagare toohello tillbaka avslutas och ber om du vill toosimulate en OBD.</span><span class="sxs-lookup"><span data-stu-id="fd143-175">If you don’t have an OBD adapter, hello app will still send location and speed data from hello phone's GPS receiver toohello back end and will ask if you want toosimulate an OBD.</span></span>

<span data-ttu-id="fd143-176">Du hittar mer information om hur hello appen använder data från hello OBD-kort och alternativ för att skapa OBD-enheten i 2.1, ”IoT-enheter” i hello [MyDriving referenshandboken](http://aka.ms/mydrivingdocs).</span><span class="sxs-lookup"><span data-stu-id="fd143-176">You can find out more about how hello app uses data from hello OBD adapter and about options for creating your own OBD device in section 2.1, "IoT Devices," in hello [MyDriving Reference Guide](http://aka.ms/mydrivingdocs).</span></span>

## <a name="use-hello-app"></a><span data-ttu-id="fd143-177">Använda hello app</span><span class="sxs-lookup"><span data-stu-id="fd143-177">Use hello app</span></span>
<span data-ttu-id="fd143-178">Starta hello app.</span><span class="sxs-lookup"><span data-stu-id="fd143-178">Start hello app.</span></span> <span data-ttu-id="fd143-179">Det finns en inledande Quickstart toowalk igenom hur det fungerar.</span><span class="sxs-lookup"><span data-stu-id="fd143-179">There’s an initial Quickstart toowalk you through how it works.</span></span>

### <a name="track-your-trips"></a><span data-ttu-id="fd143-180">Spåra dina resor</span><span class="sxs-lookup"><span data-stu-id="fd143-180">Track your trips</span></span>
<span data-ttu-id="fd143-181">Knacka hello poster knappen (stort röd cirkel längst hello hello-skärmen) toostart resa och igen på tooend.</span><span class="sxs-lookup"><span data-stu-id="fd143-181">Tap hello record button (big red circle at hello bottom of hello screen) toostart a trip, and tap again tooend.</span></span>

![Bild av hello post knapp för resa spårning](./media/iot-solution-get-started/image2.png)

<span data-ttu-id="fd143-183">Varje gång du startar en resa, om det finns ingen OBD-enhet, får du en fråga om du vill toouse hello simulatorn.</span><span class="sxs-lookup"><span data-stu-id="fd143-183">Each time you start a trip, if there’s no OBD device, you’ll be asked if you want toouse hello simulator.</span></span>

<span data-ttu-id="fd143-184">Hello slutet av en resa får tryck hello stopp-knappen, och du en sammanfattning.</span><span class="sxs-lookup"><span data-stu-id="fd143-184">At hello end of a trip, tap hello stop button, and you get a summary.</span></span>

![Exempel på en sammanfattande resa](./media/iot-solution-get-started/image3.png)

### <a name="review-your-trips"></a><span data-ttu-id="fd143-186">Granska dina resor</span><span class="sxs-lookup"><span data-stu-id="fd143-186">Review your trips</span></span>
![Exempel på en tidigare resa](./media/iot-solution-get-started/image4.png)

### <a name="review-your-profile"></a><span data-ttu-id="fd143-188">Granska din profil</span><span class="sxs-lookup"><span data-stu-id="fd143-188">Review your profile</span></span>
![Exempel på en profil för körning-format](./media/iot-solution-get-started/image5.png)

## <a name="send-us-your-test-feedback"></a><span data-ttu-id="fd143-190">Skicka test-feedback</span><span class="sxs-lookup"><span data-stu-id="fd143-190">Send us your test feedback</span></span>
<span data-ttu-id="fd143-191">Eftersom vi skapade MyDriving toohelp rivstart IoT systemen vill vi verkligen toohear från dig om hur det fungerar.</span><span class="sxs-lookup"><span data-stu-id="fd143-191">Because we created MyDriving toohelp jumpstart your own IoT systems, we certainly want toohear from you about how well it works.</span></span> <span data-ttu-id="fd143-192">Berätta för oss om:</span><span class="sxs-lookup"><span data-stu-id="fd143-192">Let us know if:</span></span>

* <span data-ttu-id="fd143-193">Du stöter på problem eller utmaningar.</span><span class="sxs-lookup"><span data-stu-id="fd143-193">You run into difficulties or challenges.</span></span>
* <span data-ttu-id="fd143-194">Det finns en plats för tillägg som gör det lämpligare tooyour scenario.</span><span class="sxs-lookup"><span data-stu-id="fd143-194">There is an extension point that would make it more suitable tooyour scenario.</span></span>
* <span data-ttu-id="fd143-195">Du hittar ett mer effektivt sätt tooaccomplish vissa behov.</span><span class="sxs-lookup"><span data-stu-id="fd143-195">You find a more efficient way tooaccomplish certain needs.</span></span>
* <span data-ttu-id="fd143-196">Du har några förslag för att förbättra MyDriving eller den här dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="fd143-196">You have any other suggestions for improving MyDriving or this documentation.</span></span>

<span data-ttu-id="fd143-197">Du kan använda hello inbyggd HockeyApp feedback mekanism i hello MyDriving app sig själv: på iOS och Android bara ge din telefon en shake eller använda hello **Feedback** kommando.</span><span class="sxs-lookup"><span data-stu-id="fd143-197">Within hello MyDriving app itself, you can use hello built-in HockeyApp feedback mechanism: on iOS and Android, just give your phone a shake, or use hello **Feedback** menu command.</span></span> <span data-ttu-id="fd143-198">Detta ska koppla en skärmbild automatiskt så att vi vet vad du pratar om.</span><span class="sxs-lookup"><span data-stu-id="fd143-198">This will automatically attach a screenshot, so that we’ll know what you’re talking about.</span></span> <span data-ttu-id="fd143-199">Och om det finns några olycklig krascher, HockeyApp samlar in hello krascher loggar tootell oss om dem.</span><span class="sxs-lookup"><span data-stu-id="fd143-199">And if there are any unfortunate crashes, HockeyApp collects hello crash logs tootell us about them.</span></span> <span data-ttu-id="fd143-200">Du kan också ge feedback via hello [HockeyApp-portalen].</span><span class="sxs-lookup"><span data-stu-id="fd143-200">You can also give feedback through hello [HockeyApp portal].</span></span>

<span data-ttu-id="fd143-201">Du kan också lagra en [problemet på GitHub], eller lämna en kommentar nedan (SV-oss edition).</span><span class="sxs-lookup"><span data-stu-id="fd143-201">You can also file an [issue on GitHub], or leave a comment below (en-us edition).</span></span>

<span data-ttu-id="fd143-202">Vi ser fram emot toohearing från dig!</span><span class="sxs-lookup"><span data-stu-id="fd143-202">We look forward toohearing from you!</span></span>

## <a name="next-steps"></a><span data-ttu-id="fd143-203">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fd143-203">Next steps</span></span>
* <span data-ttu-id="fd143-204">Utforska hello [MyDriving referenshandboken](http://aka.ms/mydrivingdocs) toounderstand hur vi har utformat och skapat hello hela MyDriving system.</span><span class="sxs-lookup"><span data-stu-id="fd143-204">Explore hello [MyDriving Reference Guide](http://aka.ms/mydrivingdocs) toounderstand how we’ve designed and built hello entire MyDriving system.</span></span>
* <span data-ttu-id="fd143-205">[Skapa och distribuera ett system med din egen](iot-solution-build-system.md) med hjälp av vår Azure Resource Manager-skript.</span><span class="sxs-lookup"><span data-stu-id="fd143-205">[Create and deploy a system of your own](iot-solution-build-system.md) by using our Azure Resource Manager scripts.</span></span> <span data-ttu-id="fd143-206">Hej [MyDriving referenshandboken](http://aka.ms/mydrivingdocs) hjälper dig också att områden där du ska göra hello de flesta anpassningar.</span><span class="sxs-lookup"><span data-stu-id="fd143-206">hello [MyDriving Reference Guide](http://aka.ms/mydrivingdocs) also guides you through areas where you’ll make hello most customizations.</span></span>

[från GitHub]: https://github.com/Azure-Samples/MyDriving
[med Xamarin]: https://developer.xamarin.com/guides/ios/getting_started/installation/
[BAFX produkter 34t5 Bluetooth OBDII Scan Tool]: http://www.amazon.com/gp/product/B005NLQAHS
[Sökningsverktyget OBDLink MX Wi-Fi: OBD-kort/diagnostik skanner]: http://www.amazon.com/gp/product/B00OCYXTYY/ref=s9_simh_gw_g263_i1_r?pf_rd_m=ATVPDKIKX0DER&pf_rd_s=desktop-2&pf_rd_r=1MWRMKXK4KK9VYMJ44MP
[HockeyApp-portalen]: https://rink.hockeyapp.org
[problemet på GitHub]: https://github.com/Azure-Samples/MyDriving/issues
