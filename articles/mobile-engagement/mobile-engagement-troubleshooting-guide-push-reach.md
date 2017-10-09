---
title: aaaAzure Mobile Engagement Troubleshooting Guide - Push/Reach
description: "Felsökning av problem med användarens interaktion och meddelanden i Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 3f1886b7-1fdd-47f4-b6b0-d79f158d5ef3
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 4ee0b34b9b753a98ccf24863acb28a5dc76bfb95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-guide-for-push-and-reach-issues"></a>Felsökningsguide för problem med push och reach
hello följande är möjliga problem som kan uppstå med hur Azure Mobile Engagement skickar information tooyour användare.

## <a name="push-failures"></a>Push-fel
### <a name="issue"></a>Problem
* Push-meddelanden fungerar inte (i appen, utanför appen eller båda).

### <a name="causes"></a>Orsaker
* Många gånger ett push fel är en indikation på att Azure Mobile Engagement, Reach eller andra avancerade funktioner för Azure Mobile Engagement inte är korrekt integrerad eller att en uppgradering är obligatoriskt i hello SDK toofix ett känt problem med en ny plattform OS eller enhet.
* Testa bara ett push i appen och bara en av App push toodetermine om det är något problem i App eller Out-of-App.
* Testa från både hello UI och hello API som en felsökning steg toosee vilka ytterligare felinformation är tillgänglig båda platserna.
* Utanför appen fungerar inte push-meddelanden om räckvidd och Azure Mobile Engagement är integrerade i hello SDK.
* Push-meddelanden fungerar inte om certifikat inte är giltiga eller använder PROD vs. DEV korrekt (endast iOS). (**Obs:** ”utanför appen” push-meddelanden kan inte levereras tooiOS, om båda hello-utveckling (utveckling) och produktion (PROD) versioner av ditt program är installerade på hello samma enhet eftersom hello säkerhetstoken som är associerade vara kan ogiltig med ditt certifikat från Apple. tooresolve problemet, avinstallerar du både hello DEV och PRODUKTPRENUMERATION versioner av programmet och installera om endast hello en versionen på din enhet.)
* Push-antal hanteras annorlunda på olika plattformar (iOS visar mindre information än Android om native push-meddelanden inaktiveras på en enhet, hello API kan ge mer information än hello Användargränssnittet på push stats) utanför appen.
* Utanför appen blockeras push-meddelanden av kunder på OS-nivå (iOS och Android).
* Utanför appen visas push-meddelanden som inaktiverad i hello Azure Mobile Engagement UI om de inte är integrerad korrekt, men kan misslyckas tyst från hello API.
* Push-meddelanden i appen kommer inte att fungera om räckvidd och Azure Mobile Engagement är integrerade i hello SDK.
* GCM och ADM push-meddelanden fungerar inte om inte Azure Mobile Engagement och hello specifik server som är integrerade i hello SDK (endast Android).
* I appen och Out-of-App push-meddelanden ska testas separat toodetermine om det beror på Push- eller räckvidden.
* Kräv hello appen är öppna toobe togs emot i appen push-meddelanden.
* Push-meddelanden är ofta installationsprogrammet toobe filtrerade med opt i eller gå ur appen info koden i appen.
* Om du använder en anpassad kategori i Reach toodisplay i appen meddelanden, behöver du toofollow hello rätt livscykel hello-meddelande, annars hello-meddelande kan inte tömmas när hello användaren stänga den.
* Om du startar en kampanj med inget slutdatum och en enhet tar emot hello i avisering via app men visas inte det ännu, hello användaren fortfarande får hello notification hello nästa gång de loggar in hello app, även om du avslutar manuellt hello kampanj.
* Bekräfta att du verkligen vill toouse hello Push API i stället för hello nå API (eftersom hello nå API används oftare) och att du inte är förvirrande hello ”nyttolast” och ”meddelaren” parametrar för problem med hello Push-API.
* Testa din push-kampanj med både en enhet är ansluten via Wi-Fi och 3G tooeliminate hello nätverksanslutning som en möjlig problem.

## <a name="push-testing"></a>Push-testning
### <a name="issue"></a>Problem
* Push-meddelanden kan skickas tooa specifik enhet baserat på en enhet-ID.

### <a name="causes"></a>Orsaker
* Testenheter ställs in på olika sätt för varje plattform, men orsakar en händelse i din app på en testenhet och letar efter enhets-ID i hello portal ska fungera toofind enhets-ID för alla plattformar.
* Testenheter fungerar olika med IDFA vs. IDFV (endast iOS).

## <a name="push-customization"></a>Push-anpassning
### <a name="issue"></a>Problem
* Avancerade push innehåll objektet inte fungerar (badge, ring, vibrerar, bild, osv.).
* Länkar från push-meddelanden fungerar inte (i appen, utanför appen, tooa webbplats, tooa plats i appen).
* Push statistik Visa som inte har en push skickas tooas många personer som förväntat (för många eller finns inte tillräckligt med).
* Push dubbletter och mottagna två gånger.
* Det går inte att registrera testenhet för Azure Mobile Engagement push-meddelanden (med din egen produktprenumeration eller DEV app).

### <a name="causes"></a>Orsaker
* toolink tooa specifik plats i appen kräver ”kategorier” (endast Android).
* Djupgående länka scheman tooredirect användare tooan alternativ plats när du klickar på ett push-meddelande måste toobe skapas i och hanteras av ditt program och hello enhetens operativsystem inte av Mobile Engagement direkt. (**Obs:** utanför appen meddelanden kan inte länka direkt tooin iOS app platser som de kan göra med Android.)
* Externa bilden servrar behöver toobe kan toouse HTTP ”GET” och ”gå” för stor bild skickar toowork (endast Android).
* I din kod du inaktivera hello Azure Mobile Engagement-agenten när hello tangentbord öppnas och har koden återaktiverar hello Azure Mobile Engagement-agenten när hello tangentbord stängs så att hello tangentbord inte påverkar hello utseendet på dina meddelande (endast iOS).
* Vissa objekt fungerar inte i test simulering, men endast verkliga kampanjer (badge, ring, vibrerar, bild, osv.).
* Inga data loggas när du använder hello-knappen på serversidan för ”test” push-meddelanden. Data loggas endast för verkliga push-kampanjer.
* toohelp isolera problemet bör du felsöka med: testa, simulera, och en verklig kampanj eftersom de fungerar lite annorlunda.
* hello kan tid ditt ”i app” och ”helst” kampanjer är schemalagda toorun påverka överföringen siffror eftersom en kampanj levereras endast toousers som ”program” medan hello kampanj körs (och användare som har sina Enhetsinställningar anges tooreceive meddelanden ”utanför appen”).
* hello skillnaderna mellan hur Android och iOS referensen från appen meddelanden gör det svårt toodirectly jämför push statistik mellan hello Android och iOS-versionen av programmet. Android ger OS nivån meddelande mer än iOS. Android rapporterar när ett enhetligt meddelande tas emot, klickar på eller tas bort i hello meddelandecentret, men iOS rapporterar inte den här informationen om hello-meddelande klickas. 
* hello främsta skälet som ”intryckt” siffror skiljer sig annorlunda än ”levererade” nummer för nå kampanjer är att ”i app” och ”utanför appen” meddelanden räknas inte på samma sätt. ”I app” meddelanden hanteras av Mobile Engagement, men ”utanför appen” meddelanden hanteras av hello meddelandecentret i hello OS av enheten.

## <a name="push-targeting"></a>Push-mål
### <a name="issue"></a>Problem
* Inbyggda riktad fungerar inte som förväntat.
* Appinfo taggen riktad fungerar inte som förväntat.
* Målobjekt för geografiska plats fungerar inte som förväntat.
* Språkinställningar fungerar inte som förväntat.

### <a name="causes"></a>Orsaker
* Kontrollera att du har överfört app-info taggar via hello Azure Mobile Engagement Användargränssnittet eller API.
* Bandbreddsbegränsning hello push hastighet eller push kvot på programnivå hello eller begränsande hello målgruppen på hello kampanj nivå kan förhindra att en person som tar emot vissa push även om de uppfyller dina målobjekt kriterier. 
* Ange ”språk” skiljer sig riktad baserat på land eller språk, som också är annat än riktad baserat på geografiska plats på en GPS-plats eller phone plats.
* hälsningsmeddelande i hello ”standardspråk” skickas tooany kund som inte har sina enhetsuppsättning tooone hello alternativa språk som du anger.

## <a name="push-scheduling"></a>Push-planering
### <a name="issue"></a>Problem
* Push schemaläggning fungerar inte som förväntat (har skickats för tidig eller fördröjd).

### <a name="causes"></a>Orsaker
* Tidszoner kan problem med schemaläggning, särskilt när du använder hello slutanvändarens tidszon.
* Avancerade push-funktioner kan fördröja push-meddelanden.
* Målobjekt för baserat på telefon inställningar (i stället för taggar för App-Info) kan fördröja push-meddelanden eftersom Azure Mobile Engagement kan ha toorequest data från hello phone realtid innan du skickar ett push.
* Kampanjer som har skapats utan ett slutdatum lagra hello push lokalt på hello enhet och visar den hello nästa gång hello appen öppnas även om hello kampanj manuellt avslutas.
* Starta mer än en kampanj på hello samtidigt kan ta en längre tid tooscan användarbasen (försök tooonly start en kampanj med högst fyra samtidigt, även fokusera tooyour aktiva användare så att gamla användare inte har toobe genomsöks).
* Om alternativet hello ”Ignorera målgrupp, push skickas toousers via hello API” hello ”kampanj” avsnittet av en Reach-kampanj hello kampanj skickas inte automatiskt, måste toosend den manuellt via hello nå API.
* Om du använder en anpassad kategori i Reach toodisplay i appen meddelanden, behöver du toofollow hello rätt livscykeln för ett meddelande, annars hello-meddelande kan inte tömmas när hello användaren stänga den.

