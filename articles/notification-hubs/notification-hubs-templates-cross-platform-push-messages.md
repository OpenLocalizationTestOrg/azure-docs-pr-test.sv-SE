---
title: Mallar
description: "Det här avsnittet beskriver mallar för Azure notification hubs."
services: notification-hubs
documentationcenter: .net
author: ysxu
manager: erikre
editor: 
ms.assetid: a41897bb-5b4b-48b2-bfd5-2e3c65edc37e
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: multiple
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 1ca24a4bf08ecdbe1c1e47a931613144309a04a9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="templates"></a><span data-ttu-id="c6aac-103">Mallar</span><span class="sxs-lookup"><span data-stu-id="c6aac-103">Templates</span></span>
## <a name="overview"></a><span data-ttu-id="c6aac-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="c6aac-104">Overview</span></span>
<span data-ttu-id="c6aac-105">Mallar kan ett klientprogram att ange det exakta formatet för den vill ta emot meddelanden.</span><span class="sxs-lookup"><span data-stu-id="c6aac-105">Templates enable a client application to specify the exact format of the notifications it wants to receive.</span></span> <span data-ttu-id="c6aac-106">Med hjälp av mallar, Tänk en app flera olika fördelar, inklusive följande:</span><span class="sxs-lookup"><span data-stu-id="c6aac-106">Using templates, an app can realize several different benefits, including the following :</span></span>

* <span data-ttu-id="c6aac-107">En plattformsoberoende serverdel</span><span class="sxs-lookup"><span data-stu-id="c6aac-107">A platform-agnostic backend</span></span>
* <span data-ttu-id="c6aac-108">Anpassade meddelanden</span><span class="sxs-lookup"><span data-stu-id="c6aac-108">Personalized notifications</span></span>
* <span data-ttu-id="c6aac-109">Klientversionen oberoende</span><span class="sxs-lookup"><span data-stu-id="c6aac-109">Client-version independence</span></span>
* <span data-ttu-id="c6aac-110">Enkelt lokalisering</span><span class="sxs-lookup"><span data-stu-id="c6aac-110">Easy localization</span></span>

<span data-ttu-id="c6aac-111">Det här avsnittet innehåller två ingående exempel på hur du använder mallar att skicka plattformsoberoende meddelanden målobjekt för alla enheter på plattformar och för att anpassa broadcast-meddelanden för varje enhet.</span><span class="sxs-lookup"><span data-stu-id="c6aac-111">This section provides two in-depth examples of how to use templates to send platform-agnostic notifications targeting all your devices across platforms, and to personalize broadcast notification to each device.</span></span>

## <a name="using-templates-cross-platform"></a><span data-ttu-id="c6aac-112">Med hjälp av mallar plattformar</span><span class="sxs-lookup"><span data-stu-id="c6aac-112">Using templates cross-platform</span></span>
<span data-ttu-id="c6aac-113">Standardmetoden för att skicka push-meddelanden är att skicka för varje meddelande som ska skickas, en specifik nyttolast till platform notification services (WNS, APN).</span><span class="sxs-lookup"><span data-stu-id="c6aac-113">The standard way to send push notifications is to send, for each notification that is to be sent, a specific payload to platform notification services (WNS, APNS).</span></span> <span data-ttu-id="c6aac-114">Om du vill skicka en avisering till APN är till exempel nyttolasten ett Json-objekt av följande format:</span><span class="sxs-lookup"><span data-stu-id="c6aac-114">For example, to send an alert to APNS, the payload is a Json object of the following form:</span></span>

    {"aps": {"alert" : "Hello!" }}

<span data-ttu-id="c6aac-115">Om du vill skicka ett liknande popup-meddelande på en Windows Store-programmet är nyttolasten i XML:</span><span class="sxs-lookup"><span data-stu-id="c6aac-115">To send a similar toast message on a Windows Store application, the XML payload is as follows:</span></span>

    <toast>
      <visual>
        <binding template=\"ToastText01\">
          <text id=\"1\">Hello!</text>
        </binding>
      </visual>
    </toast>

<span data-ttu-id="c6aac-116">Du kan skapa liknande nyttolaster för MPNS (Windows Phone) och (Android) GCM-plattformar.</span><span class="sxs-lookup"><span data-stu-id="c6aac-116">You can create similar payloads for MPNS (Windows Phone) and GCM (Android) platforms.</span></span>

<span data-ttu-id="c6aac-117">Det här kravet tvingar appens serverdel för att skapa olika nyttolaster för varje plattform och blir serverdelen som ansvarar för en del av presentation lager av appen.</span><span class="sxs-lookup"><span data-stu-id="c6aac-117">This requirement forces the app backend to produce different payloads for each platform, and effectively makes the backend responsible for part of the presentation layer of the app.</span></span> <span data-ttu-id="c6aac-118">Vissa problem är lokalisering och grafiska layouter (särskilt för Windows Store-appar som innehåller meddelanden för olika typer av paneler).</span><span class="sxs-lookup"><span data-stu-id="c6aac-118">Some concerns include localization and graphical layouts (especially for Windows Store apps that include notifications for various types of tiles).</span></span>

<span data-ttu-id="c6aac-119">Notification Hubs mall-funktionen kan ett klientprogram att skapa särskilda registreringar som kallas mall registreringar, bland annat, utöver uppsättningen taggar, en mall.</span><span class="sxs-lookup"><span data-stu-id="c6aac-119">The Notification Hubs template feature enables a client app to create special registrations, called template registrations, which include, in addition to the set of tags, a template.</span></span> <span data-ttu-id="c6aac-120">Notification Hubs mall-funktionen kan ett klientprogram att associera enheter med mallar om du arbetar med installationer (rekommenderas) eller registreringar.</span><span class="sxs-lookup"><span data-stu-id="c6aac-120">The Notification Hubs template feature enables a client app to associate devices with templates whether you are working with Installations (preferred) or Registrations.</span></span> <span data-ttu-id="c6aac-121">Nyttolasten i föregående exempel är, endast plattformsoberoende information den faktiska varning (Hej!).</span><span class="sxs-lookup"><span data-stu-id="c6aac-121">Given the preceding payload examples, the only platform-independent information is the actual alert message (Hello!).</span></span> <span data-ttu-id="c6aac-122">En mall är en uppsättning instruktioner för Meddelandehubben om hur du formaterar meddelandet plattformsoberoende för registrering av den specifika klient-app.</span><span class="sxs-lookup"><span data-stu-id="c6aac-122">A template is a set of instructions for the Notification Hub on how to format a platform-independent message for the registration of that specific client app.</span></span> <span data-ttu-id="c6aac-123">I föregående exempel plattform oberoende meddelandet är en enskild egenskap: **message = Hej!**.</span><span class="sxs-lookup"><span data-stu-id="c6aac-123">In the preceding example, the platform independent message is a single property: **message = Hello!**.</span></span>

<span data-ttu-id="c6aac-124">Följande bild illustrerar processen för ovan:</span><span class="sxs-lookup"><span data-stu-id="c6aac-124">The following picture illustrates the above process:</span></span>

![](./media/notification-hubs-templates/notification-hubs-hello.png)

<span data-ttu-id="c6aac-125">Mallen för iOS-app klientregistrering är följande:</span><span class="sxs-lookup"><span data-stu-id="c6aac-125">The template for the iOS client app registration is as follows:</span></span>

    {"aps": {"alert": "$(message)"}}

<span data-ttu-id="c6aac-126">Motsvarande mallen för Windows Store-klientappen är:</span><span class="sxs-lookup"><span data-stu-id="c6aac-126">The corresponding template for the Windows Store client app is:</span></span>

    <toast>
        <visual>
            <binding template=\"ToastText01\">
                <text id=\"1\">$(message)</text>
            </binding>
        </visual>
    </toast>

<span data-ttu-id="c6aac-127">Observera att det faktiska meddelandet ersätts av uttrycket $(meddelande).</span><span class="sxs-lookup"><span data-stu-id="c6aac-127">Notice that the actual message is substituted for the expression $(message).</span></span> <span data-ttu-id="c6aac-128">Det här uttrycket instruerar Meddelandehubben, när den skickar ett meddelande till den här viss registreringen att skapa ett meddelande som följer den och växlar i vanliga värdet.</span><span class="sxs-lookup"><span data-stu-id="c6aac-128">This expression instructs the Notification Hub, whenever it sends a message to this particular registration, to build a message that follows it and switches in the common value.</span></span>

<span data-ttu-id="c6aac-129">Om du arbetar med installationsmodell innehåller nyckeln ”mallar” installationen en JSON över flera mallar.</span><span class="sxs-lookup"><span data-stu-id="c6aac-129">If you are working with Installation model, the installation “templates” key holds a JSON of multiple templates.</span></span> <span data-ttu-id="c6aac-130">Om du arbetar med registrering modell kan klientprogrammet skapa flera registreringar för att kunna använda flera mallar. till exempel uppdateringar en mall för aviseringar och en mall för bricka.</span><span class="sxs-lookup"><span data-stu-id="c6aac-130">If you are working with Registration model, the client application can create multiple registrations in order to use multiple templates; for example, a template for alert messages and a template for tile updates.</span></span> <span data-ttu-id="c6aac-131">Klientprogram kan också blanda interna registreringar (registreringar utan någon mall) och mallen registreringar.</span><span class="sxs-lookup"><span data-stu-id="c6aac-131">Client applications can also mix native registrations (registrations with no template) and template registrations.</span></span>

<span data-ttu-id="c6aac-132">Notification Hub skickar en avisering för varje mall utan att överväga om de hör till klientappen med samma.</span><span class="sxs-lookup"><span data-stu-id="c6aac-132">The Notification Hub sends one notification for each template without considering whether they belong to the same client app.</span></span> <span data-ttu-id="c6aac-133">Det här beteendet kan användas för att översätta plattformsoberoende meddelanden till flera meddelanden.</span><span class="sxs-lookup"><span data-stu-id="c6aac-133">This behavior can be used to translate platform-independent notifications into more notifications.</span></span> <span data-ttu-id="c6aac-134">Till exempel kan samma plattform oberoende meddelande Notification Hub sömlöst översättas i ett popup-meddelande och en sida vid sida-uppdatering utan backend ska vara medvetna om det.</span><span class="sxs-lookup"><span data-stu-id="c6aac-134">For example, the same platform independent message to the Notification Hub can be seamlessly translated in a toast alert and a tile update, without requiring the backend to be aware of it.</span></span> <span data-ttu-id="c6aac-135">Observera att vissa plattformar (till exempel iOS) kan komprimera flera meddelanden till samma enhet om de skickas i en kort tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="c6aac-135">Note that some platforms (for example, iOS) might collapse multiple notifications to the same device if they are sent in a short period of time.</span></span>

## <a name="using-templates-for-personalization"></a><span data-ttu-id="c6aac-136">Med hjälp av mallar för anpassning</span><span class="sxs-lookup"><span data-stu-id="c6aac-136">Using templates for personalization</span></span>
<span data-ttu-id="c6aac-137">En annan fördel med att använda mallar är möjligheten att använda Notification Hubs för att utföra per registrering anpassningar av meddelanden.</span><span class="sxs-lookup"><span data-stu-id="c6aac-137">Another advantage to using templates is the ability to use Notification Hubs to perform per-registration personalization of notifications.</span></span> <span data-ttu-id="c6aac-138">Anta till exempel att en väder-app som visar en panel med väder villkor på en viss plats.</span><span class="sxs-lookup"><span data-stu-id="c6aac-138">For example, consider a weather app that displays a tile with the weather conditions at a specific location.</span></span> <span data-ttu-id="c6aac-139">En användare kan välja mellan Celsius eller Fahrenheit grader och en enkel eller fem dagar prognos.</span><span class="sxs-lookup"><span data-stu-id="c6aac-139">A user can choose between Celsius or Fahrenheit degrees, and a single or five-day forecast.</span></span> <span data-ttu-id="c6aac-140">Med hjälp av mallar, kan varje app klientinstallation registreras för det format som krävs (1 dag Celsius, 1 dag Fahrenheit, 5 dagar Celsius 5 dagar Fahrenheit), och skicka ett enda meddelande som innehåller den information som krävs för att fylla mallarna serverdelen (till exempel fem dagar vid en prognos med Celsius och Fahrenheit grader).</span><span class="sxs-lookup"><span data-stu-id="c6aac-140">Using templates, each client app installation can register for the format required (1-day Celsius, 1-day Fahrenheit, 5-days Celsius, 5-days Fahrenheit), and have the backend send a single message that contains all the information required to fill those templates (for example, a five-day forecast with Celsius and Fahrenheit degrees).</span></span>

<span data-ttu-id="c6aac-141">Mall för en dag prognosen med Celsius temperaturer är följande:</span><span class="sxs-lookup"><span data-stu-id="c6aac-141">The template for the one-day forecast with Celsius temperatures is as follows:</span></span>

    <tile>
      <visual>
        <binding template="TileWideSmallImageAndText04">
          <image id="1" src="$(day1_image)" alt="alt text"/>
          <text id="1">Seattle, WA</text>
          <text id="2">$(day1_tempC)</text>
        </binding>  
      </visual>
    </tile>

<span data-ttu-id="c6aac-142">Meddelandet som skickas till Meddelandehubben innehåller följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="c6aac-142">The message sent to the Notification Hub contains all the following properties:</span></span>

<table border="1">

<tr><td><span data-ttu-id="c6aac-143">day1_image</span><span class="sxs-lookup"><span data-stu-id="c6aac-143">day1_image</span></span></td><td><span data-ttu-id="c6aac-144">day2_image</span><span class="sxs-lookup"><span data-stu-id="c6aac-144">day2_image</span></span></td><td><span data-ttu-id="c6aac-145">day3_image</span><span class="sxs-lookup"><span data-stu-id="c6aac-145">day3_image</span></span></td><td><span data-ttu-id="c6aac-146">day4_image</span><span class="sxs-lookup"><span data-stu-id="c6aac-146">day4_image</span></span></td><td><span data-ttu-id="c6aac-147">day5_image</span><span class="sxs-lookup"><span data-stu-id="c6aac-147">day5_image</span></span></td></tr>

<tr><td><span data-ttu-id="c6aac-148">day1_tempC</span><span class="sxs-lookup"><span data-stu-id="c6aac-148">day1_tempC</span></span></td><td><span data-ttu-id="c6aac-149">day2_tempC</span><span class="sxs-lookup"><span data-stu-id="c6aac-149">day2_tempC</span></span></td><td><span data-ttu-id="c6aac-150">day3_tempC</span><span class="sxs-lookup"><span data-stu-id="c6aac-150">day3_tempC</span></span></td><td><span data-ttu-id="c6aac-151">day4_tempC</span><span class="sxs-lookup"><span data-stu-id="c6aac-151">day4_tempC</span></span></td><td><span data-ttu-id="c6aac-152">day5_tempC</span><span class="sxs-lookup"><span data-stu-id="c6aac-152">day5_tempC</span></span></td></tr>

<tr><td><span data-ttu-id="c6aac-153">day1_tempF</span><span class="sxs-lookup"><span data-stu-id="c6aac-153">day1_tempF</span></span></td><td><span data-ttu-id="c6aac-154">day2_tempF</span><span class="sxs-lookup"><span data-stu-id="c6aac-154">day2_tempF</span></span></td><td><span data-ttu-id="c6aac-155">day3_tempF</span><span class="sxs-lookup"><span data-stu-id="c6aac-155">day3_tempF</span></span></td><td><span data-ttu-id="c6aac-156">day4_tempF</span><span class="sxs-lookup"><span data-stu-id="c6aac-156">day4_tempF</span></span></td><td><span data-ttu-id="c6aac-157">day5_tempF</span><span class="sxs-lookup"><span data-stu-id="c6aac-157">day5_tempF</span></span></td></tr>
</table><br/>

<span data-ttu-id="c6aac-158">Med hjälp av det här mönstret skickas serverdelen endast ett meddelande utan att behöva lagra specifika anpassningsalternativ för appanvändare.</span><span class="sxs-lookup"><span data-stu-id="c6aac-158">By using this pattern, the backend only sends a single message without having to store specific personalization options for the app users.</span></span> <span data-ttu-id="c6aac-159">Följande bild visar det här scenariot:</span><span class="sxs-lookup"><span data-stu-id="c6aac-159">The following picture illustrates this scenario:</span></span>

![](./media/notification-hubs-templates/notification-hubs-registration-specific.png)

## <a name="how-to-register-templates"></a><span data-ttu-id="c6aac-160">Så här registrerar du mallar</span><span class="sxs-lookup"><span data-stu-id="c6aac-160">How to register templates</span></span>
<span data-ttu-id="c6aac-161">Om du vill registrera med mallar med hjälp av installationsmodell (rekommenderas) eller registrering av modellen, se [registrering Management](notification-hubs-push-notification-registration-management.md).</span><span class="sxs-lookup"><span data-stu-id="c6aac-161">To register with templates using the Installation model (preferred), or the Registration model, see [Registration Management](notification-hubs-push-notification-registration-management.md).</span></span>

## <a name="template-expression-language"></a><span data-ttu-id="c6aac-162">Språk för malluttryck</span><span class="sxs-lookup"><span data-stu-id="c6aac-162">Template expression language</span></span>
<span data-ttu-id="c6aac-163">Mallar är begränsade till XML- eller JSON-dokumentformat.</span><span class="sxs-lookup"><span data-stu-id="c6aac-163">Templates are limited to XML or JSON document formats.</span></span> <span data-ttu-id="c6aac-164">Du kan också bara placera uttryck på särskilda platser. till exempel noden attribut eller värden för XML, sträng egenskapsvärden för JSON.</span><span class="sxs-lookup"><span data-stu-id="c6aac-164">Also, you can only place expressions in particular places; for example, node attributes or values for XML, string property values for JSON.</span></span>

<span data-ttu-id="c6aac-165">I följande tabell visas det språk som tillåts i mallar:</span><span class="sxs-lookup"><span data-stu-id="c6aac-165">The following table shows the language allowed in templates:</span></span>

| <span data-ttu-id="c6aac-166">uttryck</span><span class="sxs-lookup"><span data-stu-id="c6aac-166">Expression</span></span> | <span data-ttu-id="c6aac-167">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c6aac-167">Description</span></span> |
| --- | --- |
| <span data-ttu-id="c6aac-168">$(prop)</span><span class="sxs-lookup"><span data-stu-id="c6aac-168">$(prop)</span></span> |<span data-ttu-id="c6aac-169">Referens till en händelseegenskap med det angivna namnet.</span><span class="sxs-lookup"><span data-stu-id="c6aac-169">Reference to an event property with the given name.</span></span> <span data-ttu-id="c6aac-170">Egenskapsnamn är inte skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="c6aac-170">Property names are not case-sensitive.</span></span> <span data-ttu-id="c6aac-171">Det här uttrycket löser i egenskapsvärdet text eller en tom sträng om egenskapen inte finns.</span><span class="sxs-lookup"><span data-stu-id="c6aac-171">This expression resolves into the property’s text value or into an empty string if the property is not present.</span></span> |
| <span data-ttu-id="c6aac-172">$(prop, n)</span><span class="sxs-lookup"><span data-stu-id="c6aac-172">$(prop, n)</span></span> |<span data-ttu-id="c6aac-173">Som ovan, men texten är uttryckligen bryts vid n tecken, till exempel $(rubrik 20) klipp innehållet i egenskapen Rubrik på 20 tecken.</span><span class="sxs-lookup"><span data-stu-id="c6aac-173">As above, but the text is explicitly clipped at n characters, for example $(title, 20) clips the contents of the title property at 20 characters.</span></span> |
| <span data-ttu-id="c6aac-174">. (prop, n)</span><span class="sxs-lookup"><span data-stu-id="c6aac-174">.(prop, n)</span></span> |<span data-ttu-id="c6aac-175">Som ovan, men texten suffixet med tre punkter som den klipps.</span><span class="sxs-lookup"><span data-stu-id="c6aac-175">As above, but the text is suffixed with three dots as it is clipped.</span></span> <span data-ttu-id="c6aac-176">Den totala storleken på den förkortade strängen och suffixet överstiger inte n tecken.</span><span class="sxs-lookup"><span data-stu-id="c6aac-176">The total size of the clipped string and the suffix does not exceed n characters.</span></span> <span data-ttu-id="c6aac-177">. (rubrik, 20) med en inkommande egenskap ”är rubriken” resultaten i **detta är titeln...**</span><span class="sxs-lookup"><span data-stu-id="c6aac-177">.(title, 20) with an input property of “This is the title line” results in **This is the title...**</span></span> |
| <span data-ttu-id="c6aac-178">%(prop)</span><span class="sxs-lookup"><span data-stu-id="c6aac-178">%(prop)</span></span> |<span data-ttu-id="c6aac-179">Liknar $(name) förutom att utdata URI-kodad.</span><span class="sxs-lookup"><span data-stu-id="c6aac-179">Similar to $(name) except that the output is URI-encoded.</span></span> |
| <span data-ttu-id="c6aac-180">#(prop)</span><span class="sxs-lookup"><span data-stu-id="c6aac-180">#(prop)</span></span> |<span data-ttu-id="c6aac-181">Används i JSON-mallar (t.ex, för iOS och Android mallar).</span><span class="sxs-lookup"><span data-stu-id="c6aac-181">Used in JSON templates (for example, for iOS and Android templates).</span></span><br><br><span data-ttu-id="c6aac-182">Den här funktionen fungerar exakt samma sätt som $(prop) tidigare angiven förutom när det används i JSON-mallar (till exempel Apple mallar).</span><span class="sxs-lookup"><span data-stu-id="c6aac-182">This function works exactly the same as $(prop) previously specified, except when used in JSON templates (for example, Apple templates).</span></span> <span data-ttu-id="c6aac-183">I det här fallet, om den här funktionen inte är omgiven av ”{” ”,}” (till exempel 'myJsonProperty': '#(namn)'), och utvärderas till ett tal i Javascript-format, till exempel regexp: (0 &#124; (&#91; 1 – 9, #93; & #91, 0-9 & #93 ;*))(\. &#91; 0-9 & #93. +)? ((e &#124; E) (+ &#124;-)? &#91; 0-9 & #93. +)?, och sedan utdata-JSON är ett tal.</span><span class="sxs-lookup"><span data-stu-id="c6aac-183">In this case, if this function is not surrounded by “{‘,’}” (for example, ‘myJsonProperty’ : ‘#(name)’), and it evaluates to a number in Javascript format, for example, regexp: (0&#124;(&#91;1-9&#93;&#91;0-9&#93;*))(\.&#91;0-9&#93;+)?((e&#124;E)(+&#124;-)?&#91;0-9&#93;+)?, then the output JSON is a number.</span></span><br><br><span data-ttu-id="c6aac-184">Till exempel ' Aktivitetsikon: ”#(namn)” blir ”badge': 40 (och inte” 40').</span><span class="sxs-lookup"><span data-stu-id="c6aac-184">For example, ‘badge : ‘#(name)’ becomes ‘badge’ : 40 (and not ‘40‘).</span></span> |
| <span data-ttu-id="c6aac-185">”text” eller ”text”</span><span class="sxs-lookup"><span data-stu-id="c6aac-185">‘text’ or “text”</span></span> |<span data-ttu-id="c6aac-186">En literal.</span><span class="sxs-lookup"><span data-stu-id="c6aac-186">A literal.</span></span> <span data-ttu-id="c6aac-187">Literaler innehålla godtycklig text inom enkla eller dubbla citattecken.</span><span class="sxs-lookup"><span data-stu-id="c6aac-187">Literals contain arbitrary text enclosed in single or double quotes.</span></span> |
| <span data-ttu-id="c6aac-188">Uttr1 + uttr2</span><span class="sxs-lookup"><span data-stu-id="c6aac-188">expr1 + expr2</span></span> |<span data-ttu-id="c6aac-189">Operatorn sammanfogning koppla två uttryck till en sträng.</span><span class="sxs-lookup"><span data-stu-id="c6aac-189">The concatenation operator joining two expressions into a single string.</span></span> |

<span data-ttu-id="c6aac-190">Uttrycken kan vara något av de föregående formulär.</span><span class="sxs-lookup"><span data-stu-id="c6aac-190">The expressions can be any of the preceding forms.</span></span>

<span data-ttu-id="c6aac-191">När du använder sammanfogning måste helt uttryck omges av {}.</span><span class="sxs-lookup"><span data-stu-id="c6aac-191">When using concatenation, the entire expression must be surrounded with {}.</span></span> <span data-ttu-id="c6aac-192">Till exempel {$(prop) + '-' + $(prop2)}.</span><span class="sxs-lookup"><span data-stu-id="c6aac-192">For example, {$(prop) + ‘ - ’ + $(prop2)}.</span></span> |

<span data-ttu-id="c6aac-193">Till exempel är följande inte en giltig XML-mall:</span><span class="sxs-lookup"><span data-stu-id="c6aac-193">For example, the following is not a valid XML template:</span></span>

    <tile>
      <visual>
        <binding $(property)>
          <text id="1">Seattle, WA</text>
        </binding>  
      </visual>
    </tile>


<span data-ttu-id="c6aac-194">Som beskrivs ovan, när du använder sammanfogning, måste uttryck omges av klammerparenteser.</span><span class="sxs-lookup"><span data-stu-id="c6aac-194">As explained above, when using concatenation, expressions must be wrapped in curly brackets.</span></span> <span data-ttu-id="c6aac-195">Exempel:</span><span class="sxs-lookup"><span data-stu-id="c6aac-195">For example:</span></span>

    <tile>
      <visual>
        <binding template="ToastText01">
          <text id="1">{'Hi, ' + $(name)}</text>
        </binding>  
      </visual>
    </tile>

