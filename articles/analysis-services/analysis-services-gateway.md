---
title: aaaOn lokala datagateway | Microsoft Docs
description: "En lokal gateway är nödvändigt om Analysis Services-server i Azure ansluter tooon lokala datakällor."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: cd596155-b608-4a34-935e-e45c95d884a9
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/21/2017
ms.author: owend
ms.openlocfilehash: fc7b9c69e6f81b41deb7a5d6d963225593845d84
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connecting-tooon-premises-data-sources-with-azure-on-premises-data-gateway"></a>Ansluta tooon lokala datakällor med Azure-lokala Data Gateway
hello lokala datagateway fungerar som en brygga som tillhandahåller säker dataöverföring mellan lokala datakällor och dina Azure Analysis Services-servrar i hello molnet. I tillägg tooworking med flera Azure Analysis Services-servrar i hello fungerar även samma region, hello senaste versionen av hello gateway med Azure Logikappar, Power BI, Power appar och Microsoft Flow. Du kan associera flera tjänster i hello samma region med en enda gateway. 

 Azure Analysis Services kräver en gateway-resurs i hello samma region. Om du har Azure Analysis Services-servrar i hello östra USA 2 region måste till exempel en gateway-resurs i hello östra USA 2 region. Flera servrar i östra USA 2 kan använda hello samma gateway.

Hämtar installationsprogrammet med hello gateway hello första gången är en process i fyra delar:

- **Hämta och kör installationsprogrammet** -det här steget installerar gateway-tjänsten på en dator i din organisation.

- **Registrera din gateway** – i det här steget kan du ange ett namn och återställning för din gateway och välj en region som registrerar din gateway med hello Gateway-Molntjänsten.

- **Skapa en gateway-resurs i Azure** – i det här steget skapar du en gateway-resurs i din Azure-prenumeration.

- **Anslut dina servrar tooyour gatewayresursen** -när du har en gateway-resurs i din prenumeration kan du börja ansluta dina servrar tooit.

När du har en gateway-resurs som har konfigurerats för din prenumeration kan ansluta du flera servrar och andra tjänster tooit. Du bara behöver tooinstall en annan gateway och skapa resurser för ytterligare en gateway om det finns servrar eller andra tjänster i en annan region.

tooget igång nu direkt se [installera och konfigurera lokala datagateway](analysis-services-gateway-install.md).

## <a name="how-it-works"></a>Hur det fungerar
hello-gateway som du installerar på en dator i din organisation körs som en windowstjänst **lokala datagateway**. Den här lokal tjänst har registrerats med hello Gateway Cloud Service via Azure Service Bus. Du kan sedan skapa en gateway-resurs Gateway Cloud Service för din Azure-prenumeration. Din Azure Analysis Services servrar är sedan ansluten tooyour gatewayresursen. När modeller på din server behöver tooconnect tooyour lokala datakällor för frågor eller bearbetning, hello lokala lokala data gateway-tjänsten och dina datakällor i en fråga och data flödet går hello gateway resurs, Azure Service Bus. 

![Hur det fungerar](./media/analysis-services-gateway/aas-gateway-how-it-works.png)

Frågor och dataflöde:

1. En fråga skapas av hello molntjänst med hello krypterade autentiseringsuppgifter för datakälla för hello lokalt. Har skickas sedan tooa kön för hello gateway tooprocess.
2. hello gateway-Molntjänsten analyserar hello frågan och skickar hello begäran toohello [Azure Service Bus](https://azure.microsoft.com/documentation/services/service-bus/).
3. hello lokala datagateway avsöker hello Azure Service Bus för väntande begäranden.
4. hello gateway hämtar hello frågan dekrypterar hello autentiseringsuppgifter och ansluter toohello datakällor med inloggningsinformationen.
5. hello gateway skickar hello toohello frågedatakällan för körning.
6. hello resultat skickas från hello datakälla, bakre toohello gateway, och sedan på hello Molntjänsten och servern.

## <a name="windows-service-account"></a>För Windows-tjänstkontot
hello lokala datagateway är konfigurerade toouse *NT SERVICE\PBIEgwService* för hello Windows service inloggning autentiseringsuppgifter. Som standard har hello till höger i inloggning som en tjänst. hello gäller hello-dator som du installerar hello gateway på. Den här autentiseringsuppgiften hello är inte samma konto som används för tooconnect tooon lokala datakällor eller ditt Azure-konto.  

Om du får problem med proxyservern på grund av tooauthentication, vill du kanske toochange hello Windows tjänst konto tooa domänanvändare eller hanteras tjänstkonto.

## <a name="ports"></a>Portar
hello gateway skapar en utgående anslutning tooAzure Service Bus. Den kommunicerar på utgående portar: TCP 443 (standard), 5671, 5672, 9350 via 9354.  hello gateway kräver inte ingående portar.

Vi rekommenderar att du godkända hello IP-adresser för dina dataområdet i brandväggen. Du kan hämta hello [Microsoft Azure-Datacenter IP-lista](https://www.microsoft.com/download/details.aspx?id=41653). Den här listan uppdateras varje vecka.

> [!NOTE]
> hello IP-adresser som anges i hello Azure Datacenter IP-adresser finns i CIDR-notering. Till exempel innebär inte 10.0.0.0/24 10.0.0.0 via 10.0.0.24. Mer information om hello [CIDR-notering](http://whatismyipaddress.com/cidr).
>
>

hello följande är hello fullständigt kvalificerade domännamn som används av hello gateway.

| Domännamn | Utgående portar | Beskrivning |
| --- | --- | --- |
| *. powerbi.com |80 |HTTP används toodownload hello installer. |
| *. powerbi.com |443 |HTTPS |
| *. analysis.windows.net |443 |HTTPS |
| *. login.windows.net |443 |HTTPS |
| *. servicebus.windows.net |5671-5672 |Avancerade Message Queuing-protokollet (AMQP) |
| *. servicebus.windows.net |443, 9350-9354 |Lyssnare på Service Bus Relay via TCP (kräver 443 för åtkomstkontroll token) |
| *. frontend.clouddatahub.net |443 |HTTPS |
| *. core.windows.net |443 |HTTPS |
| login.microsoftonline.com |443 |HTTPS |
| *. msftncsi.com |443 |Använda tootest Internetanslutning om hello gateway inte kan nås av hello Power BI-tjänsten. |
| *.microsoftonline p.com |443 |Används för autentisering beroende på konfiguration. |

### <a name="force-https"></a>Tvinga HTTPS-kommunikation med Azure Service Bus
Du kan tvinga hello gateway toocommunicate med Azure Service Bus med hjälp av HTTPS i stället för direkt TCP; men kan detta avsevärt minska prestanda. Du kan ändra hello *Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config* filen genom att ändra hello-värde från `AutoDetect` för`Https`. Den här filen finns vanligtvis i *C:\Program Files\On lokala datagateway*.

```
<setting name="ServiceBusSystemConnectivityModeString" serializeAs="String">
    <value>Https</value>
</setting>
```

## <a name="faq"></a>Vanliga frågor och svar

### <a name="general"></a>Allmänt

**Q**: behöver jag en gateway för datakällor i hello molnet, till exempel Azure SQL Database? <br/>
**En**: Nej En gateway ansluter bara tooon lokala datakällor.

**Q**: har hello gateway toobe installerad på detsamma datorn som datakälla för hello hello? <br/>
**En**: Nej hello gateway ansluter toohello datakälla med hjälp av hello anslutningsinformationen som angavs. Överväg att hello gateway som ett klientprogram på detta sätt. hello gateway behövs bara hello kapaciteten tooconnect toohello namn på servern som har angetts, vanligtvis på hello samma nätverk.

<a name="why-azure-work-school-account"></a>

**Q**: Varför jag behöver toouse ett arbets eller skolkonto konto toosign i? <br/>
**En**: du kan bara använda en Azure arbets- eller skolkonto när du installerar hello lokala datagateway. Din inloggning konto lagras i en klient som hanteras av Azure Active Directory (AD Azure). Azure AD-kontot användarens huvudnamn (UPN) matchar vanligtvis hello e-postadress.

**Q**: var lagras mina autentiseringsuppgifter? <br/>
**En**: hello-autentiseringsuppgifter som du anger för en datakälla krypteras och lagras i hello Gateway-Molntjänsten. hello autentiseringsuppgifter dekrypteras på hello lokala datagateway.

**Q**: finns det några krav för nätverksbandbredd? <br/>
**En**: det har rekommenderar nätverket anslutningen har bra genomströmning. Alla miljöer skiljer sig, och hello mängden data som skickas påverkar hello resultat. Med hjälp av ExpressRoute kan hjälpa tooguarantee en nivå av dataflödet mellan lokala och hello Azure-datacenter.
Du kan använda hello verktyg från tredje part hastighet testning i Azure app toohelp mätaren din genomflöde.

**Q**: Vad är hello svarstid för att köra frågor tooa datakällan från hello gateway? Vad är bästa hello-arkitektur? <br/>
**En**: tooreduce Nätverksfördröjningen, installera hello gateway som Stäng toohello datakälla som möjligt. Om du kan installera hello gateway på hello faktiska datakällan, minimerar den här närhet hello latens införs. Överväg att hello Datacenter för. Till exempel om din tjänst använder hello västra USA datacenter, och du har SQL Server finns i en Azure VM, ska Azure VM vara i hello västra USA för. Den här närhet minimerar fördröjning och undviker utgång avgifter för hello Azure VM.

**Q**: hur är skickade tillbaka toohello molnet? <br/>
**En**: resultat skickas via hello Azure Service Bus.

**Q**: finns det några inkommande anslutningar toohello gateway från hello molnet? <br/>
**En**: Nej hello gateway använder utgående anslutningar tooAzure Service Bus.

**Q**: Vad händer om jag blockera utgående anslutningar? Vad gör jag behöver tooopen? <br/>
**En**: Se hello portar och värdar som hello gateway används.

**Q**: det faktiska Windows hello-tjänsten som kallas?<br/>
**En**: I tjänster, hello gateway kallas lokala data gateway-tjänsten.

**Q**: kan hello gateway Windows-tjänsten körs med ett Azure Active Directory-konto? <br/>
**En**: Nej hello Windows-tjänst måste ha ett giltigt Windows-konto. Som standard körs hello-tjänsten med hello tjänst-SID NT SERVICE\PBIEgwService.

### <a name="high-availability"></a>Hög tillgänglighet och disaster recovery

**Q**: vilka alternativ som är tillgängliga för katastrofåterställning? <br/>
**En**: du kan använda hello recovery viktiga toorestore eller flytta en gateway. När du installerar hello gateway, ange hello återställningsnyckeln.

**Q**: Vad är hello fördelen hello återställningsnyckeln? <br/>
**En**: hello återställningsnyckeln ger ett sätt toomigrate eller återställa gatewayinställningarna efter en katastrof.

## <a name="troubleshooting"></a>Felsökning

**Q**: hur kan jag se vilka frågor skickas toohello lokala datakällan? <br/>
**En**: du kan aktivera frågespårning, vilket innefattar hello frågor som skickas. Kom ihåg toochange frågan spårning tillbaka toohello ursprungliga värdet när du är klar felsökning. Lämna frågespårning aktiverad skapar större loggar.

Du kan också titta på Verktyg som datakällan har för spårning frågor. Du kan till exempel använda Extended Events eller SQL Profiler för SQL Server och Analysis Services.

**Q**: där är hello gateway-loggarna? <br/>
**En**: Se loggarna senare i det här avsnittet.

### <a name="update"></a>Uppdatera toohello senaste versionen

Många problem kan ansluta när hello gatewayversionen blir inaktuella. Kontrollera att du använder hello senaste versionen som allmän bra. Om du inte har uppdaterat hello gateway för en månad eller längre, kan du Överväg att installera hello senaste versionen av hello gateway och se om du kan återskapa hello problemet.

### <a name="error-failed-tooadd-user-toogroup--2147463168-pbiegwservice-performance-log-users"></a>Fel: Det gick inte tooadd användaren toogroup. (-2147463168 PBIEgwService användare)

Du kan få detta fel om du försöker tooinstall hello gateway på en domänkontrollant, vilket inte stöds. Se till att du distribuerar hello gateway på en dator som inte är en domänkontrollant.

## <a name="logs"></a>Loggar

Loggfiler är en viktig resurs när du felsöker.

#### <a name="enterprise-gateway-service-logs"></a>Enterprise gateway tjänstloggar

`C:\Users\PBIEgwService\AppData\Local\Microsoft\On-premises data gateway\<yyyyymmdd>.<Number>.log`

#### <a name="configuration-logs"></a>Av konfigurationsloggar

`C:\Users\<username>\AppData\Local\Microsoft\On-premises data gateway\GatewayConfigurator.log`




#### <a name="event-logs"></a>Händelseloggar

Du kan hitta hello loggar Data Management Gateway och PowerBIGateway under **program- och tjänstloggar**.


## <a name="telemetry"></a>Telemetri
Telemetri kan användas för övervakning och felsökning. Som standard

**tooturn telemetri**

1.  Kontrollera hello lokala gateway-klienten datakatalog på hello dator. Vanligtvis är det **%systemdrive%\Program Files\On lokala datagateway**. Du kan öppna tjänstekonsolen och kontrollera hello sökvägen tooexecutable: en egenskap för hello lokala data gateway-tjänsten.
2.  I hello Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config filen från katalogen för klienten. Ändra hello SendTelemetry inställningen tootrue.
        
    ```
        <setting name="SendTelemetry" serializeAs="String">
                    <value>true</value>
        </setting>
    ```

3.  Spara dina ändringar och starta om tjänsten för Windows hello: lokala data gateway-tjänsten.




## <a name="next-steps"></a>Nästa steg
* [Hantera Analysis Services](analysis-services-manage.md)
* [Hämta data från Azure Analysis Services](analysis-services-connect.md)
