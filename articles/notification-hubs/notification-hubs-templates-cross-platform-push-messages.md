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
# <a name="templates"></a>Mallar
## <a name="overview"></a>Översikt
Mallar kan klienten programmet toospecify hello exakt format hello meddelanden läggs tooreceive. Med hjälp av mallar, Tänk en app flera olika fördelar, inklusive hello följande:

* En plattformsoberoende serverdel
* Anpassade meddelanden
* Klientversionen oberoende
* Enkelt lokalisering

Det här avsnittet innehåller två ingående exempel på hur toouse mallar toosend-plattformsoberoende meddelanden målobjekt för alla enheter på plattformar och toopersonalize broadcast-meddelanden tooeach enhet.

## <a name="using-templates-cross-platform"></a>Med hjälp av mallar plattformar
hello standardmetoden toosend push-meddelanden är toosend för varje meddelande som är toobe skickas, en specifik nyttolast tooplatform notification services (WNS, APN). Till exempel toosend en avisering tooAPNS är hello nyttolasten ett Json-objekt för hello följande form:

    {"aps": {"alert" : "Hello!" }}

toosend ett liknande popup-meddelande på en Windows Store-programmet hello XML-nyttolasten är följande:

    <toast>
      <visual>
        <binding template=\"ToastText01\">
          <text id=\"1\">Hello!</text>
        </binding>
      </visual>
    </toast>

Du kan skapa liknande nyttolaster för MPNS (Windows Phone) och (Android) GCM-plattformar.

Det här kravet tvingar hello app backend tooproduce olika nyttolaster för varje plattform och blir hello backend som ansvarar för en del av hello presentation säkerhetslager hello app. Vissa problem är lokalisering och grafiska layouter (särskilt för Windows Store-appar som innehåller meddelanden för olika typer av paneler).

Hej Meddelandehubbar mall funktionen kan en klient app toocreate särskilda registreringar, kallas mall registreringar, bland annat dessutom toohello uppsättning taggar, en mall. Hej Meddelandehubbar mall funktionen kan en app tooassociate klientenheter med mallar om du arbetar med installationer (rekommenderas) eller registreringar. Hello föregående exempel nyttolast är, hello endast plattformsoberoende information hello faktiska meddelande (Hej!). En mall är en uppsättning instruktioner för hello Notification Hub på hur tooformat en plattformsoberoende meddelande för hello registreringen av den specifika klient-app. I föregående exempel hello, oberoende hälsningsmeddelande för plattformen är en enskild egenskap: **message = Hej!**.

hello följande bild visar hello ovan processen:

![](./media/notification-hubs-templates/notification-hubs-hello.png)

hello-mall för hello iOS app klientregistrering är följande:

    {"aps": {"alert": "$(message)"}}

hello motsvarande mall för hello Windows Store-klientappen är:

    <toast>
        <visual>
            <binding template=\"ToastText01\">
                <text id=\"1\">$(message)</text>
            </binding>
        </visual>
    </toast>

Observera att hello faktiska meddelandet ersätts av hello uttryck $(meddelande). Det här uttrycket instruerar hello Notification Hub när den skickar ett meddelande toothis viss registrering, toobuild ett meddelande som följer den och växlar i hello vanliga värde.

Om du arbetar med installationsmodell innehåller hello installationen ”mallar” nyckeln en JSON över flera mallar. Om du arbetar med registrering modell kan hello klientprogrammet skapa flera registreringar i ordning toouse flera mallar. till exempel uppdateringar en mall för aviseringar och en mall för bricka. Klientprogram kan också blanda interna registreringar (registreringar utan någon mall) och mallen registreringar.

hello Notification Hub skickar en avisering för varje mall utan att överväga om de tillhör toohello samma klientapp. Det här problemet kan vara används tootranslate plattformsoberoende meddelanden till flera meddelanden. Till exempel hello samma plattform oberoende meddelandet toohello Notification Hub kan översättas sömlöst i en popup-avisering och en sida vid sida-uppdatering utan hello backend toobe medveten om den. Observera att vissa plattformar (till exempel iOS) kan komprimera flera meddelanden toohello samma enhet om de skickas i en kort tidsperiod.

## <a name="using-templates-for-personalization"></a>Med hjälp av mallar för anpassning
En annan fördel toousing mallar är hello möjlighet toouse Meddelandehubbar tooperform per registrering anpassningar av meddelanden. Anta till exempel att en väder-app som visar en panel med hello väder på en viss plats. En användare kan välja mellan Celsius eller Fahrenheit grader och en enkel eller fem dagar prognos. Med hjälp av mallar, kan varje app klientinstallation registreras för hello-format som krävs (1 dag Celsius, 1 dag Fahrenheit, 5 dagar Celsius 5 dagar Fahrenheit), och har hello backend skicka ett enda meddelande som innehåller alla hello information krävs toofill de mallar (till exempel fem dagar vid en prognos med Celsius och Fahrenheit grader).

hello mall för hello en dag prognos med Celsius temperaturer är följande:

    <tile>
      <visual>
        <binding template="TileWideSmallImageAndText04">
          <image id="1" src="$(day1_image)" alt="alt text"/>
          <text id="1">Seattle, WA</text>
          <text id="2">$(day1_tempC)</text>
        </binding>  
      </visual>
    </tile>

hello-meddelande skickat toohello Notification Hub innehåller alla hello följande egenskaper:

<table border="1">

<tr><td>day1_image</td><td>day2_image</td><td>day3_image</td><td>day4_image</td><td>day5_image</td></tr>

<tr><td>day1_tempC</td><td>day2_tempC</td><td>day3_tempC</td><td>day4_tempC</td><td>day5_tempC</td></tr>

<tr><td>day1_tempF</td><td>day2_tempF</td><td>day3_tempF</td><td>day4_tempF</td><td>day5_tempF</td></tr>
</table><br/>

Med hjälp av det här mönstret skickas hello backend endast ett meddelande utan att behöva toostore anpassningsalternativ för hello appanvändare. hello följande bild visar det här scenariot:

![](./media/notification-hubs-templates/notification-hubs-registration-specific.png)

## <a name="how-tooregister-templates"></a>Hur tooregister mallar
tooregister med mallar med hjälp av hello installationsmodell (rekommenderas) eller hello registrering modellen finns [registrering Management](notification-hubs-push-notification-registration-management.md).

## <a name="template-expression-language"></a>Språk för malluttryck
Mallar är begränsad tooXML eller JSON-dokumentformat. Du kan också bara placera uttryck på särskilda platser. till exempel noden attribut eller värden för XML, sträng egenskapsvärden för JSON.

hello visar följande tabell hello språk som tillåts i mallar:

| uttryck | Beskrivning |
| --- | --- |
| $(prop) |Referens tooan händelseegenskap med namnet hello. Egenskapsnamn är inte skiftlägeskänsliga. Det här uttrycket löser i textvärdet hello egenskap eller en tom sträng om hello egenskapen inte finns. |
| $(prop, n) |Som ovan, men hello text är uttryckligen bryts vid n tecken, till exempel $(rubrik 20) klipp hello innehållet i hello objektets egenskap title på 20 tecken. |
| . (prop, n) |Som ovan, men hello suffixet text med tre punkter som den klipps. hello total storlek på hello klipps sträng och hello suffix överskrida inte n tecken. . (rubrik, 20) med en inkommande egenskap ”är hello rubriken” resultaten i **detta är hello rubrik...** |
| %(prop) |Liknande too$(name) förutom att hello-utdata är URI-kodad. |
| #(prop) |Används i JSON-mallar (t.ex, för iOS och Android mallar).<br><br>Den här funktionen fungerar hello exakt samma som $(prop) tidigare angiven förutom när det används i JSON-mallar (till exempel Apple mallar). I det här fallet, om den här funktionen inte är omgiven av ”{” ”,}” (till exempel 'myJsonProperty': '#(namn)'), och det utvärderar tooa nummer i Javascript-format, till exempel regexp: (0 &#124; (&#91; 1 – 9, #93; & #91, 0-9 & #93 ;*))(\. &#91; 0-9 & #93. +)? ((e &#124; E) (+ &#124;-)? &#91; 0-9 & #93. +)?, och sedan hello utdata-JSON är ett tal.<br><br>Till exempel ' Aktivitetsikon: ”#(namn)” blir ”badge': 40 (och inte” 40'). |
| ”text” eller ”text” |En literal. Literaler innehålla godtycklig text inom enkla eller dubbla citattecken. |
| Uttr1 + uttr2 |hello sammanfogning operator koppla två uttryck till en sträng. |

hello uttryck kan vara någon av hello föregående formulär.

När du använder sammanfogning måste hello helt uttryck omges av {}. Till exempel {$(prop) + '-' + $(prop2)}. |

Till exempel är hello följande inte en giltig XML-mall:

    <tile>
      <visual>
        <binding $(property)>
          <text id="1">Seattle, WA</text>
        </binding>  
      </visual>
    </tile>


Som beskrivs ovan, när du använder sammanfogning, måste uttryck omges av klammerparenteser. Exempel:

    <tile>
      <visual>
        <binding template="ToastText01">
          <text id="1">{'Hi, ' + $(name)}</text>
        </binding>  
      </visual>
    </tile>

