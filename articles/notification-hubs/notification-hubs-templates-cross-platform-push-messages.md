---
title: aaaTemplates
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
ms.openlocfilehash: 0149f0c7473e5a4b952905bc8217582b58db2a0d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="templates"></a><span data-ttu-id="262f9-103">Mallar</span><span class="sxs-lookup"><span data-stu-id="262f9-103">Templates</span></span>
## <a name="overview"></a><span data-ttu-id="262f9-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="262f9-104">Overview</span></span>
<span data-ttu-id="262f9-105">Mallar kan klienten programmet toospecify hello exakt format hello meddelanden läggs tooreceive.</span><span class="sxs-lookup"><span data-stu-id="262f9-105">Templates enable a client application toospecify hello exact format of hello notifications it wants tooreceive.</span></span> <span data-ttu-id="262f9-106">Med hjälp av mallar, Tänk en app flera olika fördelar, inklusive hello följande:</span><span class="sxs-lookup"><span data-stu-id="262f9-106">Using templates, an app can realize several different benefits, including hello following :</span></span>

* <span data-ttu-id="262f9-107">En plattformsoberoende serverdel</span><span class="sxs-lookup"><span data-stu-id="262f9-107">A platform-agnostic backend</span></span>
* <span data-ttu-id="262f9-108">Anpassade meddelanden</span><span class="sxs-lookup"><span data-stu-id="262f9-108">Personalized notifications</span></span>
* <span data-ttu-id="262f9-109">Klientversionen oberoende</span><span class="sxs-lookup"><span data-stu-id="262f9-109">Client-version independence</span></span>
* <span data-ttu-id="262f9-110">Enkelt lokalisering</span><span class="sxs-lookup"><span data-stu-id="262f9-110">Easy localization</span></span>

<span data-ttu-id="262f9-111">Det här avsnittet innehåller två ingående exempel på hur toouse mallar toosend-plattformsoberoende meddelanden målobjekt för alla enheter på plattformar och toopersonalize broadcast-meddelanden tooeach enhet.</span><span class="sxs-lookup"><span data-stu-id="262f9-111">This section provides two in-depth examples of how toouse templates toosend platform-agnostic notifications targeting all your devices across platforms, and toopersonalize broadcast notification tooeach device.</span></span>

## <a name="using-templates-cross-platform"></a><span data-ttu-id="262f9-112">Med hjälp av mallar plattformar</span><span class="sxs-lookup"><span data-stu-id="262f9-112">Using templates cross-platform</span></span>
<span data-ttu-id="262f9-113">hello standardmetoden toosend push-meddelanden är toosend för varje meddelande som är toobe skickas, en specifik nyttolast tooplatform notification services (WNS, APN).</span><span class="sxs-lookup"><span data-stu-id="262f9-113">hello standard way toosend push notifications is toosend, for each notification that is toobe sent, a specific payload tooplatform notification services (WNS, APNS).</span></span> <span data-ttu-id="262f9-114">Till exempel toosend en avisering tooAPNS är hello nyttolasten ett Json-objekt för hello följande form:</span><span class="sxs-lookup"><span data-stu-id="262f9-114">For example, toosend an alert tooAPNS, hello payload is a Json object of hello following form:</span></span>

    {"aps": {"alert" : "Hello!" }}

<span data-ttu-id="262f9-115">toosend ett liknande popup-meddelande på en Windows Store-programmet hello XML-nyttolasten är följande:</span><span class="sxs-lookup"><span data-stu-id="262f9-115">toosend a similar toast message on a Windows Store application, hello XML payload is as follows:</span></span>

    <toast>
      <visual>
        <binding template=\"ToastText01\">
          <text id=\"1\">Hello!</text>
        </binding>
      </visual>
    </toast>

<span data-ttu-id="262f9-116">Du kan skapa liknande nyttolaster för MPNS (Windows Phone) och (Android) GCM-plattformar.</span><span class="sxs-lookup"><span data-stu-id="262f9-116">You can create similar payloads for MPNS (Windows Phone) and GCM (Android) platforms.</span></span>

<span data-ttu-id="262f9-117">Det här kravet tvingar hello app backend tooproduce olika nyttolaster för varje plattform och blir hello backend som ansvarar för en del av hello presentation säkerhetslager hello app.</span><span class="sxs-lookup"><span data-stu-id="262f9-117">This requirement forces hello app backend tooproduce different payloads for each platform, and effectively makes hello backend responsible for part of hello presentation layer of hello app.</span></span> <span data-ttu-id="262f9-118">Vissa problem är lokalisering och grafiska layouter (särskilt för Windows Store-appar som innehåller meddelanden för olika typer av paneler).</span><span class="sxs-lookup"><span data-stu-id="262f9-118">Some concerns include localization and graphical layouts (especially for Windows Store apps that include notifications for various types of tiles).</span></span>

<span data-ttu-id="262f9-119">Hej Meddelandehubbar mall funktionen kan en klient app toocreate särskilda registreringar, kallas mall registreringar, bland annat dessutom toohello uppsättning taggar, en mall.</span><span class="sxs-lookup"><span data-stu-id="262f9-119">hello Notification Hubs template feature enables a client app toocreate special registrations, called template registrations, which include, in addition toohello set of tags, a template.</span></span> <span data-ttu-id="262f9-120">Hej Meddelandehubbar mall funktionen kan en app tooassociate klientenheter med mallar om du arbetar med installationer (rekommenderas) eller registreringar.</span><span class="sxs-lookup"><span data-stu-id="262f9-120">hello Notification Hubs template feature enables a client app tooassociate devices with templates whether you are working with Installations (preferred) or Registrations.</span></span> <span data-ttu-id="262f9-121">Hello föregående exempel nyttolast är, hello endast plattformsoberoende information hello faktiska meddelande (Hej!).</span><span class="sxs-lookup"><span data-stu-id="262f9-121">Given hello preceding payload examples, hello only platform-independent information is hello actual alert message (Hello!).</span></span> <span data-ttu-id="262f9-122">En mall är en uppsättning instruktioner för hello Notification Hub på hur tooformat en plattformsoberoende meddelande för hello registreringen av den specifika klient-app.</span><span class="sxs-lookup"><span data-stu-id="262f9-122">A template is a set of instructions for hello Notification Hub on how tooformat a platform-independent message for hello registration of that specific client app.</span></span> <span data-ttu-id="262f9-123">I föregående exempel hello, oberoende hälsningsmeddelande för plattformen är en enskild egenskap: **message = Hej!**.</span><span class="sxs-lookup"><span data-stu-id="262f9-123">In hello preceding example, hello platform independent message is a single property: **message = Hello!**.</span></span>

<span data-ttu-id="262f9-124">hello följande bild visar hello ovan processen:</span><span class="sxs-lookup"><span data-stu-id="262f9-124">hello following picture illustrates hello above process:</span></span>

![](./media/notification-hubs-templates/notification-hubs-hello.png)

<span data-ttu-id="262f9-125">hello-mall för hello iOS app klientregistrering är följande:</span><span class="sxs-lookup"><span data-stu-id="262f9-125">hello template for hello iOS client app registration is as follows:</span></span>

    {"aps": {"alert": "$(message)"}}

<span data-ttu-id="262f9-126">hello motsvarande mall för hello Windows Store-klientappen är:</span><span class="sxs-lookup"><span data-stu-id="262f9-126">hello corresponding template for hello Windows Store client app is:</span></span>

    <toast>
        <visual>
            <binding template=\"ToastText01\">
                <text id=\"1\">$(message)</text>
            </binding>
        </visual>
    </toast>

<span data-ttu-id="262f9-127">Observera att hello faktiska meddelandet ersätts av hello uttryck $(meddelande).</span><span class="sxs-lookup"><span data-stu-id="262f9-127">Notice that hello actual message is substituted for hello expression $(message).</span></span> <span data-ttu-id="262f9-128">Det här uttrycket instruerar hello Notification Hub när den skickar ett meddelande toothis viss registrering, toobuild ett meddelande som följer den och växlar i hello vanliga värde.</span><span class="sxs-lookup"><span data-stu-id="262f9-128">This expression instructs hello Notification Hub, whenever it sends a message toothis particular registration, toobuild a message that follows it and switches in hello common value.</span></span>

<span data-ttu-id="262f9-129">Om du arbetar med installationsmodell innehåller hello installationen ”mallar” nyckeln en JSON över flera mallar.</span><span class="sxs-lookup"><span data-stu-id="262f9-129">If you are working with Installation model, hello installation “templates” key holds a JSON of multiple templates.</span></span> <span data-ttu-id="262f9-130">Om du arbetar med registrering modell kan hello klientprogrammet skapa flera registreringar i ordning toouse flera mallar. till exempel uppdateringar en mall för aviseringar och en mall för bricka.</span><span class="sxs-lookup"><span data-stu-id="262f9-130">If you are working with Registration model, hello client application can create multiple registrations in order toouse multiple templates; for example, a template for alert messages and a template for tile updates.</span></span> <span data-ttu-id="262f9-131">Klientprogram kan också blanda interna registreringar (registreringar utan någon mall) och mallen registreringar.</span><span class="sxs-lookup"><span data-stu-id="262f9-131">Client applications can also mix native registrations (registrations with no template) and template registrations.</span></span>

<span data-ttu-id="262f9-132">hello Notification Hub skickar en avisering för varje mall utan att överväga om de tillhör toohello samma klientapp.</span><span class="sxs-lookup"><span data-stu-id="262f9-132">hello Notification Hub sends one notification for each template without considering whether they belong toohello same client app.</span></span> <span data-ttu-id="262f9-133">Det här problemet kan vara används tootranslate plattformsoberoende meddelanden till flera meddelanden.</span><span class="sxs-lookup"><span data-stu-id="262f9-133">This behavior can be used tootranslate platform-independent notifications into more notifications.</span></span> <span data-ttu-id="262f9-134">Till exempel hello samma plattform oberoende meddelandet toohello Notification Hub kan översättas sömlöst i en popup-avisering och en sida vid sida-uppdatering utan hello backend toobe medveten om den.</span><span class="sxs-lookup"><span data-stu-id="262f9-134">For example, hello same platform independent message toohello Notification Hub can be seamlessly translated in a toast alert and a tile update, without requiring hello backend toobe aware of it.</span></span> <span data-ttu-id="262f9-135">Observera att vissa plattformar (till exempel iOS) kan komprimera flera meddelanden toohello samma enhet om de skickas i en kort tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="262f9-135">Note that some platforms (for example, iOS) might collapse multiple notifications toohello same device if they are sent in a short period of time.</span></span>

## <a name="using-templates-for-personalization"></a><span data-ttu-id="262f9-136">Med hjälp av mallar för anpassning</span><span class="sxs-lookup"><span data-stu-id="262f9-136">Using templates for personalization</span></span>
<span data-ttu-id="262f9-137">En annan fördel toousing mallar är hello möjlighet toouse Meddelandehubbar tooperform per registrering anpassningar av meddelanden.</span><span class="sxs-lookup"><span data-stu-id="262f9-137">Another advantage toousing templates is hello ability toouse Notification Hubs tooperform per-registration personalization of notifications.</span></span> <span data-ttu-id="262f9-138">Anta till exempel att en väder-app som visar en panel med hello väder på en viss plats.</span><span class="sxs-lookup"><span data-stu-id="262f9-138">For example, consider a weather app that displays a tile with hello weather conditions at a specific location.</span></span> <span data-ttu-id="262f9-139">En användare kan välja mellan Celsius eller Fahrenheit grader och en enkel eller fem dagar prognos.</span><span class="sxs-lookup"><span data-stu-id="262f9-139">A user can choose between Celsius or Fahrenheit degrees, and a single or five-day forecast.</span></span> <span data-ttu-id="262f9-140">Med hjälp av mallar, kan varje app klientinstallation registreras för hello-format som krävs (1 dag Celsius, 1 dag Fahrenheit, 5 dagar Celsius 5 dagar Fahrenheit), och har hello backend skicka ett enda meddelande som innehåller alla hello information krävs toofill de mallar (till exempel fem dagar vid en prognos med Celsius och Fahrenheit grader).</span><span class="sxs-lookup"><span data-stu-id="262f9-140">Using templates, each client app installation can register for hello format required (1-day Celsius, 1-day Fahrenheit, 5-days Celsius, 5-days Fahrenheit), and have hello backend send a single message that contains all hello information required toofill those templates (for example, a five-day forecast with Celsius and Fahrenheit degrees).</span></span>

<span data-ttu-id="262f9-141">hello mall för hello en dag prognos med Celsius temperaturer är följande:</span><span class="sxs-lookup"><span data-stu-id="262f9-141">hello template for hello one-day forecast with Celsius temperatures is as follows:</span></span>

    <tile>
      <visual>
        <binding template="TileWideSmallImageAndText04">
          <image id="1" src="$(day1_image)" alt="alt text"/>
          <text id="1">Seattle, WA</text>
          <text id="2">$(day1_tempC)</text>
        </binding>  
      </visual>
    </tile>

<span data-ttu-id="262f9-142">hello-meddelande skickat toohello Notification Hub innehåller alla hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="262f9-142">hello message sent toohello Notification Hub contains all hello following properties:</span></span>

<table border="1">

<tr><td><span data-ttu-id="262f9-143">day1_image</span><span class="sxs-lookup"><span data-stu-id="262f9-143">day1_image</span></span></td><td><span data-ttu-id="262f9-144">day2_image</span><span class="sxs-lookup"><span data-stu-id="262f9-144">day2_image</span></span></td><td><span data-ttu-id="262f9-145">day3_image</span><span class="sxs-lookup"><span data-stu-id="262f9-145">day3_image</span></span></td><td><span data-ttu-id="262f9-146">day4_image</span><span class="sxs-lookup"><span data-stu-id="262f9-146">day4_image</span></span></td><td><span data-ttu-id="262f9-147">day5_image</span><span class="sxs-lookup"><span data-stu-id="262f9-147">day5_image</span></span></td></tr>

<tr><td><span data-ttu-id="262f9-148">day1_tempC</span><span class="sxs-lookup"><span data-stu-id="262f9-148">day1_tempC</span></span></td><td><span data-ttu-id="262f9-149">day2_tempC</span><span class="sxs-lookup"><span data-stu-id="262f9-149">day2_tempC</span></span></td><td><span data-ttu-id="262f9-150">day3_tempC</span><span class="sxs-lookup"><span data-stu-id="262f9-150">day3_tempC</span></span></td><td><span data-ttu-id="262f9-151">day4_tempC</span><span class="sxs-lookup"><span data-stu-id="262f9-151">day4_tempC</span></span></td><td><span data-ttu-id="262f9-152">day5_tempC</span><span class="sxs-lookup"><span data-stu-id="262f9-152">day5_tempC</span></span></td></tr>

<tr><td><span data-ttu-id="262f9-153">day1_tempF</span><span class="sxs-lookup"><span data-stu-id="262f9-153">day1_tempF</span></span></td><td><span data-ttu-id="262f9-154">day2_tempF</span><span class="sxs-lookup"><span data-stu-id="262f9-154">day2_tempF</span></span></td><td><span data-ttu-id="262f9-155">day3_tempF</span><span class="sxs-lookup"><span data-stu-id="262f9-155">day3_tempF</span></span></td><td><span data-ttu-id="262f9-156">day4_tempF</span><span class="sxs-lookup"><span data-stu-id="262f9-156">day4_tempF</span></span></td><td><span data-ttu-id="262f9-157">day5_tempF</span><span class="sxs-lookup"><span data-stu-id="262f9-157">day5_tempF</span></span></td></tr>
</table><br/>

<span data-ttu-id="262f9-158">Med hjälp av det här mönstret skickas hello backend endast ett meddelande utan att behöva toostore anpassningsalternativ för hello appanvändare.</span><span class="sxs-lookup"><span data-stu-id="262f9-158">By using this pattern, hello backend only sends a single message without having toostore specific personalization options for hello app users.</span></span> <span data-ttu-id="262f9-159">hello följande bild visar det här scenariot:</span><span class="sxs-lookup"><span data-stu-id="262f9-159">hello following picture illustrates this scenario:</span></span>

![](./media/notification-hubs-templates/notification-hubs-registration-specific.png)

## <a name="how-tooregister-templates"></a><span data-ttu-id="262f9-160">Hur tooregister mallar</span><span class="sxs-lookup"><span data-stu-id="262f9-160">How tooregister templates</span></span>
<span data-ttu-id="262f9-161">tooregister med mallar med hjälp av hello installationsmodell (rekommenderas) eller hello registrering modellen finns [registrering Management](notification-hubs-push-notification-registration-management.md).</span><span class="sxs-lookup"><span data-stu-id="262f9-161">tooregister with templates using hello Installation model (preferred), or hello Registration model, see [Registration Management](notification-hubs-push-notification-registration-management.md).</span></span>

## <a name="template-expression-language"></a><span data-ttu-id="262f9-162">Språk för malluttryck</span><span class="sxs-lookup"><span data-stu-id="262f9-162">Template expression language</span></span>
<span data-ttu-id="262f9-163">Mallar är begränsad tooXML eller JSON-dokumentformat.</span><span class="sxs-lookup"><span data-stu-id="262f9-163">Templates are limited tooXML or JSON document formats.</span></span> <span data-ttu-id="262f9-164">Du kan också bara placera uttryck på särskilda platser. till exempel noden attribut eller värden för XML, sträng egenskapsvärden för JSON.</span><span class="sxs-lookup"><span data-stu-id="262f9-164">Also, you can only place expressions in particular places; for example, node attributes or values for XML, string property values for JSON.</span></span>

<span data-ttu-id="262f9-165">hello visar följande tabell hello språk som tillåts i mallar:</span><span class="sxs-lookup"><span data-stu-id="262f9-165">hello following table shows hello language allowed in templates:</span></span>

| <span data-ttu-id="262f9-166">uttryck</span><span class="sxs-lookup"><span data-stu-id="262f9-166">Expression</span></span> | <span data-ttu-id="262f9-167">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="262f9-167">Description</span></span> |
| --- | --- |
| <span data-ttu-id="262f9-168">$(prop)</span><span class="sxs-lookup"><span data-stu-id="262f9-168">$(prop)</span></span> |<span data-ttu-id="262f9-169">Referens tooan händelseegenskap med namnet hello.</span><span class="sxs-lookup"><span data-stu-id="262f9-169">Reference tooan event property with hello given name.</span></span> <span data-ttu-id="262f9-170">Egenskapsnamn är inte skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="262f9-170">Property names are not case-sensitive.</span></span> <span data-ttu-id="262f9-171">Det här uttrycket löser i textvärdet hello egenskap eller en tom sträng om hello egenskapen inte finns.</span><span class="sxs-lookup"><span data-stu-id="262f9-171">This expression resolves into hello property’s text value or into an empty string if hello property is not present.</span></span> |
| <span data-ttu-id="262f9-172">$(prop, n)</span><span class="sxs-lookup"><span data-stu-id="262f9-172">$(prop, n)</span></span> |<span data-ttu-id="262f9-173">Som ovan, men hello text är uttryckligen bryts vid n tecken, till exempel $(rubrik 20) klipp hello innehållet i hello objektets egenskap title på 20 tecken.</span><span class="sxs-lookup"><span data-stu-id="262f9-173">As above, but hello text is explicitly clipped at n characters, for example $(title, 20) clips hello contents of hello title property at 20 characters.</span></span> |
| <span data-ttu-id="262f9-174">. (prop, n)</span><span class="sxs-lookup"><span data-stu-id="262f9-174">.(prop, n)</span></span> |<span data-ttu-id="262f9-175">Som ovan, men hello suffixet text med tre punkter som den klipps.</span><span class="sxs-lookup"><span data-stu-id="262f9-175">As above, but hello text is suffixed with three dots as it is clipped.</span></span> <span data-ttu-id="262f9-176">hello total storlek på hello klipps sträng och hello suffix överskrida inte n tecken.</span><span class="sxs-lookup"><span data-stu-id="262f9-176">hello total size of hello clipped string and hello suffix does not exceed n characters.</span></span> <span data-ttu-id="262f9-177">. (rubrik, 20) med en inkommande egenskap ”är hello rubriken” resultaten i **detta är hello rubrik...**</span><span class="sxs-lookup"><span data-stu-id="262f9-177">.(title, 20) with an input property of “This is hello title line” results in **This is hello title...**</span></span> |
| <span data-ttu-id="262f9-178">%(prop)</span><span class="sxs-lookup"><span data-stu-id="262f9-178">%(prop)</span></span> |<span data-ttu-id="262f9-179">Liknande too$(name) förutom att hello-utdata är URI-kodad.</span><span class="sxs-lookup"><span data-stu-id="262f9-179">Similar too$(name) except that hello output is URI-encoded.</span></span> |
| <span data-ttu-id="262f9-180">#(prop)</span><span class="sxs-lookup"><span data-stu-id="262f9-180">#(prop)</span></span> |<span data-ttu-id="262f9-181">Används i JSON-mallar (t.ex, för iOS och Android mallar).</span><span class="sxs-lookup"><span data-stu-id="262f9-181">Used in JSON templates (for example, for iOS and Android templates).</span></span><br><br><span data-ttu-id="262f9-182">Den här funktionen fungerar hello exakt samma som $(prop) tidigare angiven förutom när det används i JSON-mallar (till exempel Apple mallar).</span><span class="sxs-lookup"><span data-stu-id="262f9-182">This function works exactly hello same as $(prop) previously specified, except when used in JSON templates (for example, Apple templates).</span></span> <span data-ttu-id="262f9-183">I det här fallet, om den här funktionen inte är omgiven av ”{” ”,}” (till exempel 'myJsonProperty': '#(namn)'), och det utvärderar tooa nummer i Javascript-format, till exempel regexp: (0 &#124; (&#91; 1 – 9, #93; & #91, 0-9 & #93 ;*))(\. &#91; 0-9 & #93. +)? ((e &#124; E) (+ &#124;-)? &#91; 0-9 & #93. +)?, och sedan hello utdata-JSON är ett tal.</span><span class="sxs-lookup"><span data-stu-id="262f9-183">In this case, if this function is not surrounded by “{‘,’}” (for example, ‘myJsonProperty’ : ‘#(name)’), and it evaluates tooa number in Javascript format, for example, regexp: (0&#124;(&#91;1-9&#93;&#91;0-9&#93;*))(\.&#91;0-9&#93;+)?((e&#124;E)(+&#124;-)?&#91;0-9&#93;+)?, then hello output JSON is a number.</span></span><br><br><span data-ttu-id="262f9-184">Till exempel ' Aktivitetsikon: ”#(namn)” blir ”badge': 40 (och inte” 40').</span><span class="sxs-lookup"><span data-stu-id="262f9-184">For example, ‘badge : ‘#(name)’ becomes ‘badge’ : 40 (and not ‘40‘).</span></span> |
| <span data-ttu-id="262f9-185">”text” eller ”text”</span><span class="sxs-lookup"><span data-stu-id="262f9-185">‘text’ or “text”</span></span> |<span data-ttu-id="262f9-186">En literal.</span><span class="sxs-lookup"><span data-stu-id="262f9-186">A literal.</span></span> <span data-ttu-id="262f9-187">Literaler innehålla godtycklig text inom enkla eller dubbla citattecken.</span><span class="sxs-lookup"><span data-stu-id="262f9-187">Literals contain arbitrary text enclosed in single or double quotes.</span></span> |
| <span data-ttu-id="262f9-188">Uttr1 + uttr2</span><span class="sxs-lookup"><span data-stu-id="262f9-188">expr1 + expr2</span></span> |<span data-ttu-id="262f9-189">hello sammanfogning operator koppla två uttryck till en sträng.</span><span class="sxs-lookup"><span data-stu-id="262f9-189">hello concatenation operator joining two expressions into a single string.</span></span> |

<span data-ttu-id="262f9-190">hello uttryck kan vara någon av hello föregående formulär.</span><span class="sxs-lookup"><span data-stu-id="262f9-190">hello expressions can be any of hello preceding forms.</span></span>

<span data-ttu-id="262f9-191">När du använder sammanfogning måste hello helt uttryck omges av {}.</span><span class="sxs-lookup"><span data-stu-id="262f9-191">When using concatenation, hello entire expression must be surrounded with {}.</span></span> <span data-ttu-id="262f9-192">Till exempel {$(prop) + '-' + $(prop2)}.</span><span class="sxs-lookup"><span data-stu-id="262f9-192">For example, {$(prop) + ‘ - ’ + $(prop2)}.</span></span> |

<span data-ttu-id="262f9-193">Till exempel är hello följande inte en giltig XML-mall:</span><span class="sxs-lookup"><span data-stu-id="262f9-193">For example, hello following is not a valid XML template:</span></span>

    <tile>
      <visual>
        <binding $(property)>
          <text id="1">Seattle, WA</text>
        </binding>  
      </visual>
    </tile>


<span data-ttu-id="262f9-194">Som beskrivs ovan, när du använder sammanfogning, måste uttryck omges av klammerparenteser.</span><span class="sxs-lookup"><span data-stu-id="262f9-194">As explained above, when using concatenation, expressions must be wrapped in curly brackets.</span></span> <span data-ttu-id="262f9-195">Exempel:</span><span class="sxs-lookup"><span data-stu-id="262f9-195">For example:</span></span>

    <tile>
      <visual>
        <binding template="ToastText01">
          <text id="1">{'Hi, ' + $(name)}</text>
        </binding>  
      </visual>
    </tile>

