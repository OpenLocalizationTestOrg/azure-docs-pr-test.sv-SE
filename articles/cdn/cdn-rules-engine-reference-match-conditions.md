---
title: aaaAzure CDN regler motorn matchar villkoren | Microsoft Docs
description: "I referensdokumentationen för Azure CDN regler motorn matchar villkoren och funktioner."
services: cdn
documentationcenter: 
author: Lichard
manager: akucer
editor: 
ms.assetid: 669ef140-a6dd-4b62-9b9d-3f375a14215e
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: rli
ms.openlocfilehash: 5e79f8f0c75a646e13bf315c492b9f2a9defc396
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cdn-rules-engine-match-conditions"></a>Azure CDN regelmotor matchar villkoren
Det här avsnittet visar detaljerade beskrivningar av hello tillgängliga matchar villkor för Azure Content Delivery Network (CDN) [regelmotor](cdn-rules-engine.md).

hello andra delen av en regel är hello matchar villkoret. En matchar villkoret identifierar vissa typer av begäranden som en uppsättning funktioner kommer att utföras.

Exempelvis kanske används toofilter förfrågningar om innehåll på en viss plats begäranden som genereras från en viss IP-adress eller ett land eller av huvudinformation.

## <a name="always"></a>Alltid

hello är alltid matchar villkoret utformad tooapply standard uppsättning funktioner tooall begäranden.

## <a name="device"></a>Enhet

hello enheten matchar villkoret identifierar begäranden från en mobil enhet baserat på dess egenskaper.  Identifiering av mobila enheter uppnås genom [WURFL](http://wurfl.sourceforge.net/).  WURFL funktioner och deras CDN regelmotor variabler visas nedan.

> [!NOTE] 
> hello variabler nedan stöds i hello **ändra klienten begär huvud** och **ändra klienten svarshuvud** funktioner.

Funktion | Variabel | Beskrivning | Exempel värden
-----------|----------|-------------|----------------
Varumärke | % {wurfl_cap_brand_name} | En sträng som anger hello varumärke av hello enhet. | Samsung
Enhetens OS | % {wurfl_cap_device_os} | En sträng som anger hello operativsystemet installeras på hello enhet. | IOS
Enhetens operativsystemversion | % {wurfl_cap_device_os_version} | En sträng som anger hello versionsnumret för hello OS är installerat på hello enhet. | 1.0.1
Dubbla orientering | % {wurfl_cap_dual_orientation} | Ett booleskt värde som anger om hello enhet stöder dubbla orientering. | SANT
HTML önskad DTD | % {wurfl_cap_html_preferred_dtd} | En sträng som anger hello enhetens prioriterade dokumenttypsdefinition (DTD) för HTML-innehåll. | Ingen<br/>xhtml_basic<br/>HTML5
Bild Inlining | % {wurfl_cap_image_inlining} | Ett booleskt värde som anger om hello enhet stöder Base64-kodade bilder. | FALSKT
Är Android | % {wurfl_vcap_is_android} | Ett booleskt värde som anger om hello enhet använder hello i Android OS. | SANT
Är IOS | % {wurfl_vcap_is_ios} | Ett booleskt värde som anger om hello enhet använder iOS. | FALSKT
Är Smart TV | % {wurfl_cap_is_smarttv} | Ett booleskt värde som anger om hello enheten är en smart TV. | FALSKT
Är Smartphone | % {wurfl_vcap_is_smartphone} | Ett booleskt värde som anger om hello enheten är en smartphone. | SANT
Är Tablet | % {wurfl_cap_is_tablet} | Ett booleskt värde som anger om hello enheten är en surfplatta. Det här är en oberoende av OS-beskrivning. | SANT
Är trådlösa enheter | % {wurfl_cap_is_wireless_device} | Ett booleskt värde som anger om hello anses vara en trådlös enhet. | SANT
Marknadsföring namn | % {wurfl_cap_marketing_name} | En sträng som anger hello enhetens marknadsföring namn. | BlackBerry 8100 Pearl
Mobila webbläsare | % {wurfl_cap_mobile_browser} | En sträng som anger hello webbläsare toorequest innehållet från hello enhet har använts. | Chrome
Mobila webbläsarversion | % {wurfl_cap_mobile_browser_version} | En sträng som anger hello version av webbläsaren hello toorequest innehållet från hello enhet har använts. | 31
Modellnamnet | % {wurfl_cap_model_name} | En sträng som anger hello enhetens namn. | S3
Progressiv hämtning | % {wurfl_cap_progressive_download} | Ett booleskt värde som anger om hello enhet stöder hello uppspelning av ljud och video medan den hämtas fortfarande. | SANT
Utgivningsdatum | % {wurfl_cap_release_date} | En sträng som anger hello år och månad på vilka hello enhet har lagts till toohello WURFL databas.<br/><br/>Format:`yyyy_mm` | 2013_december
Lösning höjd | % {wurfl_cap_resolution_height} | Ett heltal som visar hello enhetens höjd i bildpunkter. | 768
Lösning bredd | % {wurfl_cap_resolution_width} | Ett heltal som anger hello enhetens bredd i bildpunkter. | 1024


## <a name="location"></a>Plats

De matchar de villkor som är utformad tooidentify förfrågningar baserat på hello beställaren plats.

Namn | Syfte
-----|--------
SOM tal | Identifierar förfrågningar som kommer från ett visst nätverk.
Land/region | Identifierar förfrågningar som kommer från hello angivna länder.


## <a name="origin"></a>Ursprung

De matchar de villkor som är utformad tooidentify begäranden som punkt tooCDN lagring eller en kund ursprungsservern.

Namn | Syfte
-----|--------
CDN ursprung | Identifierar förfrågningar om innehåll som lagras på CDN lagring.
Kunden ursprung | Identifierar förfrågningar om innehåll som lagras på en viss kund ursprungsservern.


## <a name="request"></a>Förfrågan

De matchar de villkor som är utformad tooidentify förfrågningar baserat på deras egenskaper.

Namn | Syfte
-----|--------
Klientens IP-adress | Identifierar förfrågningar som kommer från en viss IP-adress.
Cookie-Parameter | Kontrollerar hello cookies som är associerade med varje begäran om hello anges värdet.
Cookie-parametern Regex | Kontrollerar hello cookies som är associerade med varje begäran om hello angivna reguljära uttrycket.
Edge Cname | Identifierar begäranden som pekar tooa specifika edge CNAME-post.
Refererande domän | Identifierar begäranden som har hänvisats från hello angivna hostname(s).
Begäran sidhuvud Literal | Identifierar begäranden som innehåller hello angivna huvudet set tooa angivna värden.
Begäran sidhuvud Regex | Identifierar begäranden som innehåller hello angetts set tooa huvudvärde som matchar hello angivna reguljära uttrycket.
Begäran huvud med jokertecken | Identifierar begäranden som innehåller hello angivna huvudet värdet tooa som matchar angivna hello-mönster.
Metod för begäran | Identifierar begäranden via sina HTTP-metoden.
Schemat för begäran | Identifierar begäranden via sina HTTP-protokollet.

## <a name="url"></a>URL: EN

De matchar de villkor som är utformad tooidentify förfrågningar baserat på deras URL: er.

Namn | Syfte
-----|--------
URL-sökväg-katalog | Identifierar begäranden via deras relativa sökvägen.
URL-sökväg-tillägget | Identifierar begäranden efter deras filnamnstillägg.
URL-sökväg filnamn | Identifierar begäranden av filnamnet.
URL-sökväg Literal | Jämför en begäran har relativ sökväg toohello angivet värde.
URL-sökväg Regex | Jämför en begäran har angetts relativ sökväg toohello reguljärt uttryck.
URL-sökväg med jokertecken | Jämför en begäran relativ sökväg toohello angivna mönstret.
URL-frågan Literal | Jämför en begäran har fråga sträng toohello angivet värde.
Frågeparametern för URL | Identifierar begäranden som innehåller hello angivna frågan strängparameter värdet tooa som matchar ett specifikt mönster.
URL-frågan Regex | Identifierar begäranden som innehåller hello angivna frågan strängparameter värdet tooa som matchar angivna reguljära uttrycket.
URL: en fråga med jokertecken | Jämför hello angetts värden mot hello begäran frågesträngen.


## <a name="next-steps"></a>Nästa steg
* [Azure CDN-översikt](cdn-overview.md)
* [Regler modulreferens](cdn-rules-engine-reference.md)
* [Regler motorn villkorsuttryck](cdn-rules-engine-reference-conditional-expressions.md)
* [Regler motorn funktioner](cdn-rules-engine-reference-features.md)
* [Åsidosätta HTTP standardinställningar med hjälp av hello regelmotor](cdn-rules-engine.md)

