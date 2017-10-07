---
title: "frågor för Azure Programgateway och aaaFrequently | Microsoft Docs"
description: "Den här sidan innehåller svar toofrequently frågor och svar om Azure Programgateway"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: d54ee7ec-4d6b-4db7-8a17-6513fda7e392
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/19/2017
ms.author: gwallace
ms.openlocfilehash: b2df3a82a71a3264d3d34d317d08e4b4f72c6e3e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions-for-application-gateway"></a>Vanliga frågor för Programgateway

## <a name="general"></a>Allmänt

**FRÅGOR. Vad är Application Gateway?**

Azure Application Gateway är programmet leverans domänkontrollant (ADC) som en tjänst med olika layer 7 belastningsutjämning för dina program. Det ger hög tillgänglighet och skalbarhet tjänsten, som är fullständigt hanterade av Azure.

**FRÅGOR. Vilka funktioner stöder Programgateway?**

Programgateway stöder SSL genom att avlasta- och slutdatum tooend SSL, Brandvägg för webbaserade program, cookie-baserad session tillhörighet, url sökväg-baserade routning, värd för flera plats och andra. En fullständig lista över funktioner som stöds finns [introduktion tooApplication Gateway](application-gateway-introduction.md)

**FRÅGOR. Vad är hello skillnaden mellan Programgateway och Azure belastningsutjämnare?**

Programgateway är en lager 7 belastningsutjämnare, vilket innebär att den fungerar med webbtrafik endast (WebSocket-HTTP/HTTPS). Det stöder funktioner som SSL-avslutning, cookie-baserad session tillhörighet och resursallokering för trafik för belastningsutjämning. Belastningsutjämnare, Läs in saldon trafik på layer 4 (TCP/UDP).

**FRÅGOR. Vilka protokoll stöder Programgateway?**

Application Gateway har stöd för HTTP, HTTPS och WebSocket.

**FRÅGOR. Vilka resurser som stöds i dag som en del av serverdelspool?**

Serverdelspooler kan bestå av nätverkskort, skalningsuppsättningar i virtuella datorer, offentliga IP-adresser, trots att interna IP-adresser, fullständigt kvalificerade domännamnet (FQDN) och flera innehavare-servrar som Azure Web Apps. Programgateway backend inte poolmedlemmar knutna tooan tillgänglighetsuppsättning. Medlemmar i serverdelspooler kan vara över kluster, Datacenter eller utanför Azure så länge som de har IP-anslutning.

**FRÅGOR. Vilka regioner är hello tjänsten?**

Programgateway är tillgänglig i alla regioner för globala Azure. Det är också tillgängliga i [Azure Kina](https://www.azure.cn/) och [Azure Government](https://azure.microsoft.com/en-us/overview/clouds/government/)

**FRÅGOR. Är detta en dedikerad distribution för min prenumeration eller delas mellan kunder?**

Programgateway är en särskild distribution i ditt virtuella nätverk.

**FRÅGOR. Är HTTP -> HTTPS omdirigering stöds?**

Omdirigering stöds. Besök [Programgateway omdirigering översikt](application-gateway-redirect-overview.md) toolearn mer.

**FRÅGOR. I vilken ordning bearbetas lyssnare?**

Lyssnare bearbetas i hello ordning de visas. Därför om en grundläggande lyssnare matchar en inkommande begäran bearbetas först.  Lyssnare för flera platser bör konfigureras innan en grundläggande lyssnare tooensure trafik dirigeras toohello rätt backend.

**FRÅGOR. Var hittar jag Application Gateway IP- och DNS?**

När du använder en offentlig IP-adress som en slutpunkt, kan den här informationen hittas på hello offentliga IP-adressresurs eller hello översiktssidan för hello Programgateway hello-portalen. För interna IP-adresser, kan detta hittas på översiktssidan för hello.

**FRÅGOR. Hello IP- eller DNS-ändras under hello livslängd för hello Programgateway?**

Ändra hello VIP om hello gateway stoppas och startas av hello kunden. hello DNS som är associerade med Programgateway ändras inte över hello livscykeln för hello gateway. Därför är det rekommenderade toouse en CNAME-alias och peka på den toohello DNS-serveradress för hello Application Gateway.

**FRÅGOR. Application Gateway har stöd för statisk IP-adress?**

Nej, Application Gateway har inte stöd för statiska offentliga IP-adresser, men det stöder statiska interna IP-adresser.

**FRÅGOR. Stöder Programgateway flera offentliga IP-adresser på hello gateway?**

Endast en offentlig IP-adress stöds på en Programgateway.

**FRÅGOR. Stöder Programgateway x vidarebefordras för huvuden?**

Ja, Programgateway infogar x vidarebefordras för, x vidarebefordras proto och x vidarebefordras port huvuden i hello begäran vidarebefordras toohello backend. hello-formatet för x vidarebefordras för huvudet är en kommaavgränsad lista över IP:Port. hello giltiga värden för x vidarebefordras proto är http eller https. X vidarebefordras port Anger hello port på vilken hello-begäran på hello Application Gateway.

**FRÅGOR. Hur lång tid tar det toodeploy en Programgateway? Min Programgateway fortfarande fungerar när uppdateras?**

Nya Programgateway distributioner kan ta upp too20 minuter tooprovision. Ändringar tooinstance storlek/antal är inte störande och hello gateway förblir aktiv under denna tid.

## <a name="configuration"></a>Konfiguration

**FRÅGOR. Programgateway alltid distribueras i ett virtuellt nätverk?**

Ja, distribueras alltid Application Gateway i ett undernät för virtuellt nätverk. Det här undernätet kan endast innehålla Programgatewayer.

**FRÅGOR. Kontakta Programgateway tooinstances utanför dess virtuella nätverket?**

Programgateway kontakta tooinstances utanför hello virtuella nätverk som det är så länge det finns en IP-anslutning. Om du planerar toouse interna IP-adresser som backend poolmedlemmar, då kräver [VNET-Peering](../virtual-network/virtual-network-peering-overview.md) eller [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).

**FRÅGOR. Kan jag distribuera något annat i hello Programgateway undernät?**

Nej, men du kan distribuera andra programgatewayer i hello undernät.

**FRÅGOR. Stöds Nätverkssäkerhetsgrupper på hello Programgateway undernät?**

Nätverkssäkerhetsgrupper stöds i hello Programgateway undernät med hello följande begränsningar:

* Undantag ställas på inkommande trafik på portarna 65503 65534 för serverdelen hälsa toowork korrekt.

* Utgående internet-anslutningen blockeras inte.

* Trafik från hello taggen AzureLoadBalancer måste tillåtas.

**FRÅGOR. Vad är hello gränserna för Programgateway? Kan jag öka dessa gränser?**

Besök [programmet Gateway gränser](../azure-subscription-service-limits.md#application-gateway-limits) tooview hello gränser.

**FRÅGOR. Kan jag använda Programgateway för både externa och interna trafik samtidigt?**

Ja, stöder Application Gateway har en intern IP-adress och en extern IP-adress per Application Gateway.

**FRÅGOR. VNet-peering stöds?**

Ja, VNet-peering stöds och är bra för trafik i andra virtuella nätverk för belastningsutjämning.

**FRÅGOR. Kan jag kontakta tooon lokala servrar när de är anslutna via ExpressRoute eller VPN-tunnlar?**

Ja, förutsatt att trafik tillåts.

**FRÅGOR. Kan jag har en serverdelspool som betjänar många program på olika portar?**

Arkitektur för Micro stöds. Du behöver flera HTTP-inställningarna tooprobe på olika portar.

**FRÅGOR. Stöder anpassade avsökningar jokertecken/regex på svarsdata?**

Anpassade avsökningar stöder inte jokertecken eller regex på svarsdata. 

**FRÅGOR. Hur bearbetas regler?**

Regler bearbetas i hello ordning som de är konfigurerade. Du rekommenderas att regler för flera platser konfigureras innan grundläggande regler tooreduce hello chans att trafiken dirigeras toohello olämpliga backend som grundläggande hello-regeln matchar baserat på tidigare toohello flera platser portregel som utvärderas.

**FRÅGOR. Hur bearbetas regler?**

Regler bearbetas i hello ordning de skapades. Du rekommenderas att regler för flera platser konfigureras före grundläggande regler. Den här konfigurationen minskar hello chansen att trafik är routade toohello olämpliga backend genom att konfigurera flera platser lyssnare först. Routning problemet kan inträffa eftersom grundläggande hello-regeln matchar baserat på tidigare toohello flera platser portregel som utvärderas.

**FRÅGOR. Vad hello värdfältet för anpassade avsökningar obestämd?**

Värd anger hello namn toosend hello avsökningen till. Gäller endast när flera platser har konfigurerats på Application Gateway, Använd annars ”127.0.0.1”. Det här värdet skiljer sig från den virtuella datorns värdnamn och har formatet \<protokollet\>://\<värden\>:\<port\>\<sökvägen\>.

**FRÅGOR. Kan jag godkända Programgateway åtkomst tooa få datakällan IP-adresser?**

Det här scenariot kan göras med hjälp av NSG: er i Application Gateway-undernät. hello följande begränsningar ska placeras i hello undernät i hello visas prioritetsordning:

* Tillåt inkommande trafik från käll-IP-/ IP-intervall.

* Tillåt inkommande begäranden från alla källor tooports 65503 65534 för [backend hälsa kommunikation](application-gateway-diagnostics.md).

* Tillåt inkommande Azure belastningsutjämnare avsökningar (taggen AzureLoadBalancer) och inkommande trafik i virtuella nätverk (taggen VirtualNetwork) på hello [NSG](../virtual-network/virtual-networks-nsg.md).

* Blockera alla andra inkommande trafik med en neka alla regeln.

* Tillåt utgående trafik toohello internet för alla mål.

## <a name="performance"></a>Prestanda

**FRÅGOR. Hur stöder Programgateway hög tillgänglighet och skalbarhet?**

Application Gateway har stöd för scenarier med hög tillgänglighet när du har två eller fler instanser som har distribuerats. Azure distribuerar dessa instanser i uppdateringen och feltolerans domäner tooensure som alla instanser inte växlar vid hello samtidigt. Programgateway stöder skalbarhet genom att lägga till flera instanser av hello samma gateway tooshare hello belastningen.

**FRÅGOR. Hur jag uppnå katastrofåterställning över datacenter med Application Gateway?**

Kunder kan använda Traffic Manager toodistribute trafik över flera Programgatewayer i olika datacenter.

**FRÅGOR. Automatisk skalning stöds?**

Nej, men Application Gateway har ett mått på genomflödet som kan använda tooalert när ett tröskelvärde uppnås. Manuellt lägga till instanser eller ändrar storlek startar inte om hello gateway och påverkar inte befintliga trafiken.

**FRÅGOR. Stöder manuell skala upp eller ned orsak driftavbrott?**

Det finns inget driftstopp, instanser är fördelade på uppgraderingsdomäner och feldomäner.

**FRÅGOR. Kan jag ändra instansstorleken från medelhög toolarge utan avbrott?**

Ja, Azure distribuerar instanser i uppdateringen och feltolerans domäner tooensure som alla instanser inte växlar vid hello samtidigt. Programmet Gateway har stöd för skalning genom att lägga till flera instanser av hello samma gateway tooshare hello belastningen.

## <a name="ssl-configuration"></a>SSL-konfiguration

**FRÅGOR. Vilka certifikat som stöds på Programgateway?**

Automatiskt signerat certifikat, CA-certifikat och certifikat jokertecken stöds. EV certifikat stöds inte.

**FRÅGOR. Vad är hello aktuella krypteringssviter som stöds av Application Gateway?**

hello följande är hello aktuella krypteringssviter som stöds av Programgateway. Besök: [Konfigurera SSL princip versioner och krypteringssviter på Programgateway](application-gateway-configure-ssl-policy-powershell.md) toolearn hur toocustomize SSL-alternativ.

- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA
- TLS_DHE_RSA_WITH_AES_256_GCM_SHA384
- TLS_DHE_RSA_WITH_AES_128_GCM_SHA256
- TLS_DHE_RSA_WITH_AES_256_CBC_SHA
- TLS_DHE_RSA_WITH_AES_128_CBC_SHA
- TLS_RSA_WITH_AES_256_GCM_SHA384
- TLS_RSA_WITH_AES_128_GCM_SHA256
- TLS_RSA_WITH_AES_256_CBC_SHA256
- TLS_RSA_WITH_AES_128_CBC_SHA256
- TLS_RSA_WITH_AES_256_CBC_SHA
- TLS_RSA_WITH_AES_128_CBC_SHA
- TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA256
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA256
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA
- TLS_RSA_WITH_3DES_EDE_CBC_SHA
- TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA

**FRÅGOR. Stöder Programgateway också omkryptering av trafik toohello backend?**

Ja, Application Gateway har stöd för SSL-avlastning och end tooend SSL, som krypterar hello trafik toohello backend.

**FRÅGOR. Kan jag konfigurera SSL princip toocontrol SSL-protokoll versioner?**

Ja, du kan konfigurera Programgateway toodeny TLS1.0, TLS1.1 och TLS1.2. SSL 2.0 och 3.0 är redan inaktiverad som standard och kan inte konfigureras.

**FRÅGOR. Kan jag konfigurera chiffersviter och principen ordning?**

Ja, [konfiguration av krypteringssviter](application-gateway-ssl-policy-overview.md) stöds. När du definierar en anpassad princip måste minst en av följande krypteringssviter hello aktiveras. Programgateway använder SHA256 toofor backend management.

* TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 
* TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
* TLS_DHE_RSA_WITH_AES_128_GCM_SHA256
* TLS_RSA_WITH_AES_128_GCM_SHA256
* TLS_RSA_WITH_AES_256_CBC_SHA256
* TLS_RSA_WITH_AES_128_CBC_SHA256

**FRÅGOR. Hur många SSL-certifikat som stöds?**

In too20 SSL som certifikat stöds.

**FRÅGOR. Hur många certifikat för klientautentisering för backend-omkryptering stöds?**

In too10 stöds certifikat för klientautentisering med en standard på 5.

**FRÅGOR. Kan Programgateway integreras med Azure Key Vault internt?**

Nej, det inte är integrerat med Azure Key Vault.

## <a name="web-application-firewall-waf-configuration"></a>Konfiguration av webbprogram Firewall (Brandvägg)

**FRÅGOR. Erbjuder hello Brandvägg SKU alla hello funktioner som är tillgängliga med hello Standard SKU?**

Ja, Brandvägg stöder alla hello-funktioner i hello Standard-SKU.

**FRÅGOR. Vad är hello CR version Application Gateway har stöd för?**

Programgateway stöder CR [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) och CR [3.0](application-gateway-crs-rulegroups-rules.md#owasp30).

**FRÅGOR. Hur övervakar Brandvägg?**

Brandvägg övervakas via diagnostikloggning, mer information om diagnostikloggning kan hittas på [diagnostik loggning och mått för Programgateway](application-gateway-diagnostics.md)

**FRÅGOR. Blockerar identifieringsläget trafik?**

Nej, loggar identifieringsläget endast trafik som utlöses av en Brandvägg regel.

**FRÅGOR. Hur anpassar Brandvägg regler?**

Ja, Brandvägg regler kan anpassas för mer information om hur toocustomize dem finns [anpassa Brandvägg regelgrupper och regler](application-gateway-customize-waf-rules-portal.md)

**FRÅGOR. Vilka regler som är tillgängliga?**

Brandvägg stöder för närvarande CR [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) och [3.0](application-gateway-crs-rulegroups-rules.md#owasp30), som ger grundläggande säkerhet mot de flesta av hello topp 10 sårbarheter som identifieras av hello öppna Web Application säkerhet projekt (OWASP) finns här [OWASP topp 10 säkerhetsrisker](https://www.owasp.org/index.php/Top10#OWASP_Top_10_for_2013)

* Skydd mot SQL-inmatning

* Skydd mot skriptkörning över flera webbplatser

* Skydd mot vanliga webbattacker, som kommandoinmatning, dold HTTP-begäran, delning av HTTP-svar och attack genom införande av fjärrfil

* Skydd mot åtgärder som inte följer HTTP-protokollet

* Skydd mot avvikelser i HTTP-protokollet som att användaragent för värden och accept-huvud saknas

* Skydd mot robotar, crawlers och skannrar

* Identifiering av program vanliga felinställningar (det vill säga Apache, IIS osv.)

**FRÅGOR. Stöder Brandvägg också DDoS förebyggande?**

Nej, Brandvägg ger inte några DDoS förebyggande.

## <a name="diagnostics-and-logging"></a>Diagnostik- och loggning

**FRÅGOR. Vilka typer av loggar är tillgängliga med Application Gateway?**

Det finns tre loggarna för Programgateway. Mer information om dessa loggar och andra diagnostiska funktioner finns [Backend hälsa och diagnostik loggar mått för Programgateway](application-gateway-diagnostics.md).

- **ApplicationGatewayAccessLog** -hello åtkomst loggen innehåller varje förfrågan har skickats toohello Programgateway klientdel. hello data omfattar hello anroparen IP begärd, URL svar svarstid, returkod, byte in och ut. Åtkomstlogg samlas in varje 300 sekunder. Den här loggfilen innehåller en post per instans av Application Gateway.
- **ApplicationGatewayPerformanceLog** -hello prestanda loggen innehåller prestandainformation om för per instanser inklusive totala begäran skickades, genomflöde i byte, totalt antal begäranden som betjänats misslyckade begäranden antal felfria och feltillstånd backend-instanser.
- **ApplicationGatewayFirewallLog** -hello brandväggsloggen innehåller begäranden som loggas via identifiering eller förhindra läget för en Programgateway som är konfigurerad med Brandvägg för webbaserade program.

**FRÅGOR. Hur vet jag om min backend poolmedlemmar felfri?**

Du kan använda PowerShell-cmdlet för hello `Get-AzureRmApplicationGatewayBackendHealth` eller verifiera hälsotillstånd hello-portalen genom att besöka [Gateway Programdiagnostik](application-gateway-diagnostics.md)

**FRÅGOR. Vad är hello bevarandeprincip på hello diagnostik loggar?**

Diagnostikloggar flödet toohello kunder storage-konto och kunder kan ange hello bevarandeprincip baserat på deras inställningar. Diagnostikloggar kan även skickas tooan Event Hub eller logganalys. Besök [Gateway Programdiagnostik](application-gateway-diagnostics.md) för mer information.

**FRÅGOR. Hur skaffar jag granskningsloggarna för Programgateway**

Granskningsloggar är tillgängliga för Application Gateway. I hello-portalen klickar du på **aktivitetsloggen** i hello menyn bladet för en Programgateway tooaccess hello granskningslogg. 

**FRÅGOR. Kan jag ställa in aviseringar med Application Gateway?**

Ja, Application Gateway har stöd för varningar, aviseringar konfigureras av mått.  Application Gateway har för närvarande ”dataflöde”, vilket kan vara konfigurerade tooalert för mått. toolearn mer om varningar, besök [få aviseringar](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

**FRÅGOR. Backend-hälsa returnerar okänd status, vilka kan vara orsaken till denna status?**

hello beror oftast åtkomst toohello backend blockeras av en NSG eller anpassade DNS. Besök [Backend hälsa, diagnostikloggning och mått för Programgateway](application-gateway-diagnostics.md) toolearn mer.

## <a name="next-steps"></a>Nästa steg

Mer om Programgateway finns toolearn [introduktion tooApplication Gateway](application-gateway-introduction.md).
