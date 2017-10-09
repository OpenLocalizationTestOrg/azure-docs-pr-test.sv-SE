---
title: "aaaIntroduction tooweb programmet firewall (Brandvägg) för Azure Programgateway | Microsoft Docs"
description: "Den här sidan innehåller en översikt över brandväggen för webbaserade program (WAF) för Application Gateway"
documentationcenter: na
services: application-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: 04b362bc-6653-4765-86f6-55ee8ec2a0ff
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2017
ms.author: amsriva
ms.openlocfilehash: 5a42ce0fb2bd12a391844099e2de8fa2571195e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="web-application-firewall-waf"></a>Brandvägg för webbaserade program (WAF)

Brandvägg för webbaserade program (WAF) är en funktion i Application Gateway som ger ett centraliserat skydd för dina webbappar mot vanliga kryphål och säkerhetsproblem. 

Brandvägg för webbaserade program baserat på regler från hello [OWASP core uppsättningar](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) 3.0 eller 2.2.9. Webbprogram blir i allt större utsträckning föremål för attacker där kända svagheter i programmen utnyttjas. Vanliga bland dessa kryphål SQL injection attacker, över flera webbplatser attacker tooname några. Förhindrar sådana attacker i programkod kan vara svårt och kan kräva rigorösa underhåll, uppdatering och övervakning av flera lager av hello programmet topologi. En brandvägg för centraliserad webbaserade program gör säkerhetshantering mycket enklare och ger bättre säkerhet tooapplication administratörer mot hot och intrång. En Brandvägg-lösning kan också reagera tooa säkerhetshot snabbare med uppdatering av ett känt problem på en central plats jämfört med att skydda alla enskilda webbprogram. Befintliga programgatewayer kan enkelt vara Programgateway för konverterade tooa web application-brandväggen är aktiverad.

![imageURLroute](./media/application-gateway-web-application-firewall-overview/WAF1.png)

Programgateway fungerar som ett program leverans domänkontrollant och erbjudanden SSL-avslutning, cookie-baserad session tillhörighet, resursallokering belastningsdistribution, innehållsbaserad routning, möjlighet toohost flera förbättringar av webbplatser och säkerhet. Säkerhetsförbättringar som erbjuds av Programgateway inkluderar SSL hantering av slutet tooend stöd för SSL. Programsäkerhet är nu ökas genom att integreras direkt till hello ADC erbjudande Brandvägg (Brandvägg för webbaserade program). Detta ger ett enkelt tooconfigure central plats toomanage och skydda dina webbapplikationer mot vanliga web säkerhetsproblem.

## <a name="benefits"></a>Fördelar

hello här är hello core fördelarna med Brandvägg för Programgateway och webb-program:

### <a name="protection"></a>Skydd

* Skydda ditt webbprogram från webben säkerhetsproblem och attacker utan modifiering toobackend kod.

* Skydda flera web applications vid hello samma tid bakom en Programgateway. Programgateway stöder värd upp too20 webbplatser bakom en enda gateway som kan alla vara skyddad mot webbattacker med Brandvägg.

### <a name="monitoring"></a>Övervakning

* Övervaka om ditt webbprogram utsätts för angrepp med hjälp av en realtidslogg för brandväggen för webbaserade program. Den här loggfilen är integrerad med [Azure-Monitor](../monitoring-and-diagnostics/monitoring-overview.md) tootrack Brandvägg aviseringar och loggar och enkelt kan övervaka trender.

* Brandväggen för webbaserade program kommer snart att integreras med Azure Security Center. Azure Security Center ger en central vy över hello säkerhetsläget för alla dina Azure-resurser.

### <a name="customization"></a>Anpassning

* hello möjlighet toocustomize Brandvägg regler och regeln grupper toosuit kraven för application och undvika falska positiva identifieringar.

## <a name="features"></a>Funktioner

Brandvägg för webbaserade program har förinställda med CR 3.0 som standard men du kan välja toouse 2.2.9. I CRS 3.0 finns bättre möjligheter att eliminera falska positiva identifieringar än i 2.2.9. Hej möjligheten för[anpassa regler toosuit dina behov](application-gateway-customize-waf-rules-portal.md) har angetts. Vissa av hello vanliga web säkerhetsrisker som Brandvägg för webbaserade program som skyddar mot innehåller:

* Skydd mot SQL-inmatning
* Skydd mot skriptkörning över flera webbplatser
* Skydd mot vanliga webbattacker, som kommandoinmatning, dold HTTP-begäran, delning av HTTP-svar och attack genom införande av fjärrfil
* Skydd mot åtgärder som inte följer HTTP-protokollet
* Skydd mot avvikelser i HTTP-protokollet som att användaragent för värden och accept-huvud saknas
* Skydd mot robotar, crawlers och skannrar
* Identifiering av vanliga felkonfigureringar i program (t.ex. Apache, IIS osv.)

En detaljerad lista över regler och deras skydd finns hello följande [Core uppsättningar](#core-rule-sets).

### <a name="core-rule-sets"></a>Core Rule Sets

Application Gateway stöder två regeluppsättningar: CRS 3.0 och CRS 2.2.9. Dessa Core Rule Sets är regelsamlingar som skyddar dina webbprogram mot skadlig aktivitet.

#### <a name="owasp30"></a>OWASP_3.0

regeluppsättning för hello 3.0 core tillhandahålls har 13 regelgrupper som visas i följande tabell hello. Var och en av dessa regelgrupper innehåller flera regler. Dessa kan inaktiveras.

|RuleGroup|Beskrivning|
|---|---|
|**[REQUEST-910-IP-REPUTATION](application-gateway-crs-rulegroups-rules.md#crs910)**|Innehåller regler tooprotect mot kända skickar skräppost eller skadlig aktivitet.|
|**[REQUEST-911-METHOD-ENFORCEMENT](application-gateway-crs-rulegroups-rules.md#crs911)**|Innehåller regler toolock metoder (PUT, korrigering <..)|
|**[REQUEST-912-DOS-PROTECTION](application-gateway-crs-rulegroups-rules.md#crs912)**| Innehåller regler tooprotect mot attacker Denial of Service (DoS).|
|**[REQUEST-913-SCANNER-DETECTION](application-gateway-crs-rulegroups-rules.md#crs913)**| Innehåller regler tooprotect mot port och miljö skannrar.|
|**[REQUEST-920-PROTOCOL-ENFORCEMENT](application-gateway-crs-rulegroups-rules.md#crs920)**|Innehåller regler tooprotect mot kodning problem och protokoll.|
|**[REQUEST-921-PROTOCOL-ATTACK](application-gateway-crs-rulegroups-rules.md#crs921)**|Innehåller regler tooprotect mot huvudet injection, begäransmuggling och dela svar|
|**[REQUEST-930-APPLICATION-ATTACK-LFI](application-gateway-crs-rulegroups-rules.md#crs930)**|Innehåller regler tooprotect mot attacker och sökvägen.|
|**[REQUEST-931-APPLICATION-ATTACK-RFI](application-gateway-crs-rulegroups-rules.md#crs931)**|Innehåller regler tooprotect mot Remote filen inkludering (RFI)|
|**[REQUEST-932-APPLICATION-ATTACK-RCE](application-gateway-crs-rulegroups-rules.md#crs932)**|Innehåller regler tooprotect igen fjärrkörning av kod.|
|**[REQUEST-933-APPLICATION-ATTACK-PHP](application-gateway-crs-rulegroups-rules.md#crs933)**|Innehåller regler tooprotect mot inmatningsangrepp PHP.|
|**[REQUEST-941-APPLICATION-ATTACK-XSS](application-gateway-crs-rulegroups-rules.md#crs941)**|Innehåller regler för att skydda mot skriptkörning över flera webbplatser.|
|**[REQUEST-942-APPLICATION-ATTACK-SQLI](application-gateway-crs-rulegroups-rules.md#crs942)**|Innehåller regler för att skydda mot SQL-inmatningsattacker.|
|**[REQUEST-943-APPLICATION-ATTACK-SESSION-FIXATION](application-gateway-crs-rulegroups-rules.md#crs943)**|Innehåller regler tooprotect mot Session upptagningen attacker.|

#### <a name="owasp229"></a>OWASP_2.2.9

hello 2.2.9 core regeluppsättning som innehåller 10 regelgrupper som visas i hello i den följande tabellen. Var och en av dessa regelgrupper innehåller flera regler. Dessa kan inaktiveras.

|RuleGroup|Beskrivning|
|---|---|
|**[crs_20_protocol_violations](application-gateway-crs-rulegroups-rules.md#crs20)**|Innehåller regler tooprotect mot protokollet överträdelser (ogiltiga tecken, GET med en begärantext osv.)|
|**[crs_21_protocol_anomalies](application-gateway-crs-rulegroups-rules.md#crs21)**|Innehåller regler tooprotect mot felaktig huvudinformation.|
|**[crs_23_request_limits](application-gateway-crs-rulegroups-rules.md#crs23)**|Innehåller regler tooprotect mot argument eller filer som överstiger begränsningar.|
|**[crs_30_http_policy](application-gateway-crs-rulegroups-rules.md#crs30)**|Innehåller regler tooprotect mot begränsade metoder, rubriker och filtyper. |
|**[crs_35_bad_robots](application-gateway-crs-rulegroups-rules.md#crs35)**|Innehåller regler tooprotect mot web robotar och -skannrar.|
|**[crs_40_generic_attacks](application-gateway-crs-rulegroups-rules.md#crs40)**|Innehåller regler tooprotect mot generiska angrepp (session upptagningen, fjärrfil inkludering, PHP injection, etc.)|
|**[crs_41_sql_injection_attacks](application-gateway-crs-rulegroups-rules.md#crs41sql)**|Innehåller regler tooprotect mot SQL injection attacker|
|**[crs_41_xss_attacks](application-gateway-crs-rulegroups-rules.md#crs41xss)**|Innehåller regler tooprotect mot skriptkörning över flera webbplatser.|
|**[crs_42_tight_security](application-gateway-crs-rulegroups-rules.md#crs42)**|Innehåller en regel tooprotect mot sökvägen traversal attacker|
|**[crs_45_trojans](application-gateway-crs-rulegroups-rules.md#crs45)**|Innehåller regler tooprotect mot bakdörrstrojaner.|

### <a name="waf-modes"></a>WAF-lägen

Programmet Gateway Brandvägg kan vara konfigurerade toorun i hello följande två lägen:

* **Identifieringsläget** – när du konfigurerade toorun i identifieringsläget programmet Gateway Brandvägg övervakar och loggar alla hot aviseringar i tooa loggfilen. Loggning diagnostik för Programgateway bör vara aktiverat på hello **diagnostik** avsnitt. Du måste också tooensure som hello Brandvägg loggen har valts och aktiverat. Vid körning i identifieringsläge blockerar brandväggen för webbaserade program inte inkommande begäranden.
* **Förebyggande läge** – när du konfigurerade toorun i förebyggande läge Programgateway blockerar aktivt intrång och attacker som identifieras av dess regler. hello angripare tar emot ett 403 undantag för obehörig åtkomst och hello avbryts. Förebyggande läge fortsätter toolog sådana attacker i hello Brandvägg loggar.

### <a name="application-gateway-waf-reports"></a>WAF-övervakning

Det är viktigt att övervaka hello hälsotillståndet för din Programgateway. Hello hälsotillståndet för dina program brandvägg och hello webbprogram som den skyddar sker via loggning och integrering med Azure-Monitor Azure Security Center (kommer snart) och logganalys.

![diagnostik](./media/application-gateway-web-application-firewall-overview/diagnostics.png)

#### <a name="azure-monitor"></a>Azure Monitor

Alla Application Gateway-loggar är integrerade med [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview.md).  Detta ger dig tootrack diagnostisk information, inklusive Brandvägg varningar och loggar.  Den här funktionen tillhandahålls i hello Programgateway resurs i hello portal under hello **diagnostik** fliken eller via hello Azure-Monitor tjänsten direkt. Mer om att aktivera diagnostikloggar för Programgateway finns toolearn [Programgateway diagnostik](application-gateway-diagnostics.md)

#### <a name="azure-security-center"></a>Azure Security Center

[Azure Security Center](../security-center/security-center-intro.md) kan du förebygga, upptäcka och svara toothreats med bättre överblick och kontroll över hello säkerheten för dina Azure-resurser. Funktionen Programgateway [ingår nu i Azure Security Center](application-gateway-integration-security-center.md). Azure Security Center söker igenom miljön toodetect oskyddade webbprogram. Det kan nu rekommenderar programmet gateway Brandvägg tooprotect resurserna sårbar. Du kan skapa program gateway Brandvägg direkt från hello Azure Security Center.  Dessa Brandvägg instanser är integrerade med Azure Security Center och skickar aviseringar och hälsoinformation tillbaka tooAzure Security Center för rapportering.

![bild 1](./media/application-gateway-web-application-firewall-overview/figure1.png)

#### <a name="logging"></a>Loggning

Application Gateway WAF erbjuder detaljerad rapportering för varje hot som upptäcks. Loggningen är integrerad med loggar och varningar för Azure Diagnostics och registreras i JSON-format. Dessa loggar kan integreras med [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md).

![imageURLroute](./media/application-gateway-web-application-firewall-overview/waf2.png)

```json
{
  "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupId}/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/{appGatewayName}",
  "operationName": "ApplicationGatewayFirewall",
  "time": "2017-03-20T15:52:09.1494499Z",
  "category": "ApplicationGatewayFirewallLog",
  "properties": {
    "instanceId": "ApplicationGatewayRole_IN_0",
    "clientIp": "104.210.252.3",
    "clientPort": "4835",
    "requestUri": "/?a=%3Cscript%3Ealert(%22Hello%22);%3C/script%3E",
    "ruleSetType": "OWASP",
    "ruleSetVersion": "3.0",
    "ruleId": "941320",
    "message": "Possible XSS Attack Detected - HTML Tag Handler",
    "action": "Blocked",
    "site": "Global",
    "details": {
      "message": "Warning. Pattern match \"<(a|abbr|acronym|address|applet|area|audioscope|b|base|basefront|bdo|bgsound|big|blackface|blink|blockquote|body|bq|br|button|caption|center|cite|code|col|colgroup|comment|dd|del|dfn|dir|div|dl|dt|em|embed|fieldset|fn|font|form|frame|frameset|h1|head|h ...\" at ARGS:a.",
      "data": "Matched Data: <script> found within ARGS:a: <script>alert(\\x22hello\\x22);</script>",
      "file": "rules/REQUEST-941-APPLICATION-ATTACK-XSS.conf",
      "line": "865"
    }
  }
} 

```

## <a name="application-gateway-waf-sku-pricing"></a>Priser för WAF SKU för programgatewayen

Brandväggen för webbaserade program är tillgänglig i en ny WAF SKU. Den här SKU är tillgänglig endast i Azure Resource Manager etablering modellen och inte under hello klassiska distributionsmodellen. WAF SKU finns dessutom endast i Application Gateway-instansstorlekarna medel och stor. Alla hello gränser för Programgateway gäller även toohello Brandvägg SKU. Priserna är baserade på avgifter per gatewayinstans per timme och databearbetningsavgifter. Gatewaypriset per timme för WAF SKU skiljer sig från avgifterna för Standard-SKU (se [Priser för Application Gateway](https://azure.microsoft.com/pricing/details/application-gateway/)). Databearbetning kostnader förblir hello samma. Avgifter per regel eller per regelgrupp finns inte. Du kan skydda flera webbprogram bakom hello samma web application firewall och det finns inga ytterligare avgifter för att stödja flera program. 

Fakturering för Brandvägg startar effektivt 2017-5/5 tills sedan hello Brandvägg SKU gateways fortsätter toobe enligt standardpriser.

## <a name="next-steps"></a>Nästa steg

När du lär dig mer om hello funktionerna för Brandvägg, gå [hur tooconfigure web application brandväggen på Programgateway](application-gateway-web-application-firewall-portal.md).

