---
title: Lokala datagateway | Microsoft Docs
description: "En lokal gateway är nödvändigt om Analysis Services-server i Azure ansluter till lokala datakällor."
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
ms.openlocfilehash: 514b5404e8cbfa0baa657eb41736e20cad502638
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="connecting-to-on-premises-data-sources-with-azure-on-premises-data-gateway"></a>Ansluter till lokala datakällor med Azure-lokala Data Gateway
Lokala datagateway fungerar som en brygga som tillhandahåller säker dataöverföring mellan lokala datakällor och dina Azure Analysis Services-servrar i molnet. Förutom att arbeta med flera Azure Analysis Services-servrar i samma region fungerar även den senaste versionen av gatewayen med Azure Logikappar, Power BI, Power appar och Microsoft Flow. Du kan associera flera tjänster i samma region med en enda gateway. 

 Azure Analysis Services kräver en gateway-resurs i samma region. Du måste till exempel en gateway-resurs i östra USA 2 om du har Azure Analysis Services-servrar i östra USA 2. Flera servrar i östra USA 2 kan använda samma gateway.

Hämtar installationsprogrammet gatewayen första gången är en process i fyra delar:

- **Hämta och kör installationsprogrammet** -det här steget installerar gateway-tjänsten på en dator i din organisation.

- **Registrera din gateway** – i det här steget kan du ange ett namn och återställning för din gateway och välj en region som registrerar din gateway med Gateway-Molntjänsten.

- **Skapa en gateway-resurs i Azure** – i det här steget skapar du en gateway-resurs i din Azure-prenumeration.

- **Anslut dina servrar till din gateway-resurs** -när du har en gateway-resurs i din prenumeration kan du börja ansluter dina servrar.

När du har en gateway-resurs som har konfigurerats för din prenumeration kan ansluta du flera servrar och andra tjänster till den. Du behöver bara installera en annan gateway och skapa resurser för ytterligare en gateway om det finns servrar eller andra tjänster i en annan region.

Om du vill komma igång nu direkt, se [installera och konfigurera lokala datagateway](analysis-services-gateway-install.md).

## <a name="how-it-works"></a>Hur det fungerar
Den gateway som du installerar på en dator i din organisation körs som en windowstjänst **lokala datagateway**. Den här lokal tjänst har registrerats med Gateway-Molntjänsten via Azure Service Bus. Du kan sedan skapa en gateway-resurs Gateway Cloud Service för din Azure-prenumeration. Azure Analysis Services-servrar är nu ansluten till din gateway-resurs. När modeller på servern behöver ansluta till dina lokala data källor för frågor eller bearbetning, passerar fråga och dataflöde gatewayresursen, Azure Service Bus, lokala lokala data gateway-tjänsten och dina datakällor. 

![Hur det fungerar](./media/analysis-services-gateway/aas-gateway-how-it-works.png)

Frågor och dataflöde:

1. En fråga skapas av Molntjänsten med krypterade autentiseringsuppgifter för den lokala datakällan. Sedan skickas till en kö att bearbeta-gateway.
2. Gateway-Molntjänsten analyserar frågan och skickar en begäran om att den [Azure Service Bus](https://azure.microsoft.com/documentation/services/service-bus/).
3. Lokala datagateway avsöker Azure Service Bus för väntande begäranden.
4. Gatewayen hämtar frågan dekrypterar autentiseringsuppgifterna och ansluter till datakällor med inloggningsinformationen.
5. Gatewayen skickar frågan till datakällan för körning.
6. Resultatet skickas från datakällan, tillbaka till gateway och sedan på Molntjänsten och servern.

## <a name="windows-service-account"></a>För Windows-tjänstkontot
Lokala datagateway är konfigurerad för att använda *NT SERVICE\PBIEgwService* för Windows-tjänsten inloggning autentiseringsuppgifter. Som standard har rätt att logga in som en tjänst. i samband med den dator som du installerar en gateway på. Den här autentiseringsuppgiften är inte samma konto som används för att ansluta till lokala datakällor eller ditt Azure-konto.  

Om du får problem med proxyservern på grund av autentisering du kanske vill ändra Windows-tjänstkontot till en domänanvändare eller Hanterat tjänstkonto.

## <a name="ports"></a>Portar
Gatewayen skapar en utgående anslutning till Azure Service Bus. Den kommunicerar på utgående portar: TCP 443 (standard), 5671, 5672, 9350 via 9354.  Gatewayen kräver inte ingående portar.

Vi rekommenderar att du listan över godkända IP-adresser för dina dataområdet i brandväggen. Du kan hämta den [Microsoft Azure-Datacenter IP-lista](https://www.microsoft.com/download/details.aspx?id=41653). Den här listan uppdateras varje vecka.

> [!NOTE]
> IP-adresser i listan Azure Datacenter IP finns i CIDR-notering. Till exempel innebär inte 10.0.0.0/24 10.0.0.0 via 10.0.0.24. Lär dig mer om den [CIDR-notering](http://whatismyipaddress.com/cidr).
>
>

Följande är fullständigt kvalificerat domännamn som används av gateway.

| Domännamn | Utgående portar | Beskrivning |
| --- | --- | --- |
| *. powerbi.com |80 |HTTP används för att hämta installationsprogrammet. |
| *. powerbi.com |443 |HTTPS |
| *. analysis.windows.net |443 |HTTPS |
| *. login.windows.net |443 |HTTPS |
| *. servicebus.windows.net |5671-5672 |Avancerade Message Queuing-protokollet (AMQP) |
| *. servicebus.windows.net |443, 9350-9354 |Lyssnare på Service Bus Relay via TCP (kräver 443 för åtkomstkontroll token) |
| *. frontend.clouddatahub.net |443 |HTTPS |
| *. core.windows.net |443 |HTTPS |
| login.microsoftonline.com |443 |HTTPS |
| *. msftncsi.com |443 |Används för att testa internet-anslutningen om denna gateway inte kan nås av Power BI-tjänsten. |
| *.microsoftonline p.com |443 |Används för autentisering beroende på konfiguration. |

### <a name="force-https"></a>Tvinga HTTPS-kommunikation med Azure Service Bus
Du kan tvinga gateway att kommunicera med Azure Service Bus med hjälp av HTTPS i stället för direkt TCP; men kan detta avsevärt minska prestanda. Du kan ändra den *Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config* filen genom att ändra värdet från `AutoDetect` till `Https`. Den här filen finns vanligtvis i *C:\Program Files\On lokala datagateway*.

```
<setting name="ServiceBusSystemConnectivityModeString" serializeAs="String">
    <value>Https</value>
</setting>
```

## <a name="faq"></a>Vanliga frågor och svar

### <a name="general"></a>Allmänt

**Q**: behöver jag en gateway för datakällor i molnet, till exempel Azure SQL Database? <br/>
**En**: Nej En gateway ansluter till endast lokala datakällor.

**Q**: har en gateway installeras på samma dator som datakällan? <br/>
**En**: Nej Gatewayen ansluter till datakällan med hjälp av informationen som angavs. Överväg att gatewayen som ett klientprogram på detta sätt. Gatewayen måste bara möjligheten att ansluta till namnet på servern som angavs, vanligtvis i samma nätverk.

<a name="why-azure-work-school-account"></a>

**Q**: Varför måste jag använda ett arbets- eller skolkonto konto för att logga in? <br/>
**En**: du kan bara använda en Azure arbets eller skolkonto när du installerar den lokala datagatewayen. Din inloggning konto lagras i en klient som hanteras av Azure Active Directory (AD Azure). Azure AD-kontot användarens huvudnamn (UPN) matchar vanligtvis e-postadress.

**Q**: var lagras mina autentiseringsuppgifter? <br/>
**En**: de autentiseringsuppgifter som du anger för en datakälla krypteras och lagras i Gateway-Molntjänsten. Autentiseringsuppgifterna dekrypteras på lokala datagateway.

**Q**: finns det några krav för nätverksbandbredd? <br/>
**En**: det har rekommenderar nätverket anslutningen har bra genomströmning. Alla miljöer skiljer sig, och mängden data som skickas påverkar resultaten. Med hjälp av ExpressRoute kan hjälpa till att garantera en nivå av dataflödet mellan lokala och Azure-datacenter.
Du kan använda verktyg från tredje part Azure hastighet testa appen för att mäta din genomflöde.

**Q**: Vad är svarstiden för att köra frågor till en datakälla från gateway? Vad är den bästa arkitekturen? <br/>
**En**: Installera gatewayen för att minska Nätverksfördröjningen som nära datakällan som möjligt. Om du kan installera gatewayen på den faktiska datakällan, minimerar den här närhet svarstiden införs. Överväg att Datacenter för. Till exempel om din tjänst använder västra USA datacenter, och du har SQL Server finns i en Azure VM, ska Azure VM vara i västra USA för. Den här närhet minimerar fördröjning och undviker utgång avgifter på Azure VM.

**Q**: hur resultatet skickas tillbaka till molnet? <br/>
**En**: resultat skickas via Azure Service Bus.

**Q**: finns det några inkommande anslutningar till gatewayen från molnet? <br/>
**En**: Nej Gatewayen använder utgående anslutningar till Azure Service Bus.

**Q**: Vad händer om jag blockera utgående anslutningar? Vad behöver jag att öppna? <br/>
**En**: Se portar och värdar som används av gateway-servern.

**Q**: det faktiska Windows-tjänsten som kallas?<br/>
**En**: I tjänster gatewayen som kallas lokala data gateway-tjänsten.

**Q**: kan gateway Windows-tjänsten körs med ett Azure Active Directory-konto? <br/>
**En**: Nej Windows-tjänsten måste ha ett giltigt Windows-konto. Som standard körs tjänsten med tjänst-SID NT SERVICE\PBIEgwService.

### <a name="high-availability"></a>Hög tillgänglighet och disaster recovery

**Q**: vilka alternativ som är tillgängliga för katastrofåterställning? <br/>
**En**: du kan använda återställningsnyckeln för att återställa eller flytta en gateway. När du installerar en gateway måste du ange återställningsnyckeln.

**Q**: Vad är fördelen med återställningsnyckeln? <br/>
**En**: återställningsnyckeln är ett sätt att migrera eller återställa gatewayinställningarna efter en katastrof.

## <a name="troubleshooting"></a>Felsökning

**Q**: hur kan jag se vilka frågor som ska skickas till den lokala datakällan? <br/>
**En**: du kan aktivera frågespårning som innehåller de förfrågningar som skickas. Kom ihåg att ändra frågan spåra tillbaka till det ursprungliga värdet när du är klar felsökning. Lämna frågespårning aktiverad skapar större loggar.

Du kan också titta på Verktyg som datakällan har för spårning frågor. Du kan till exempel använda Extended Events eller SQL Profiler för SQL Server och Analysis Services.

**Q**: där är gateway-loggarna? <br/>
**En**: Se loggarna senare i det här avsnittet.

### <a name="update"></a>Uppdatera till den senaste versionen

Många problem kan ansluta när gateway-versionen blir inaktuella. Kontrollera att du använder den senaste versionen som allmän bra. Om du inte har uppdaterat en gateway för en månad eller längre, kan du du överväga att installera den senaste versionen av gatewayen och se om du kan återskapa problemet.

### <a name="error-failed-to-add-user-to-group--2147463168-pbiegwservice-performance-log-users"></a>Fel: Det gick inte att lägga till användaren i gruppen. (-2147463168 PBIEgwService användare)

Du kan få detta fel om du försöker installera gatewayen på en domänkontrollant, vilket inte stöds. Se till att du distribuerar gatewayen på en dator som inte är en domänkontrollant.

## <a name="logs"></a>Loggar

Loggfiler är en viktig resurs när du felsöker.

#### <a name="enterprise-gateway-service-logs"></a>Enterprise gateway tjänstloggar

`C:\Users\PBIEgwService\AppData\Local\Microsoft\On-premises data gateway\<yyyyymmdd>.<Number>.log`

#### <a name="configuration-logs"></a>Av konfigurationsloggar

`C:\Users\<username>\AppData\Local\Microsoft\On-premises data gateway\GatewayConfigurator.log`




#### <a name="event-logs"></a>Händelseloggar

Du kan hitta loggar Data Management Gateway och PowerBIGateway under **program- och tjänstloggar**.


## <a name="telemetry"></a>Telemetri
Telemetri kan användas för övervakning och felsökning. Som standard

**Aktivera telemetri**

1.  Kontrollera katalogen lokala data gateway-klienten på datorn. Vanligtvis är det **%systemdrive%\Program Files\On lokala datagateway**. Du kan öppna tjänstekonsolen och kontrollera sökvägen till körbar fil: en egenskap för lokala data gateway-tjänsten.
2.  I filen Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config från klienten directory. Ändra SendTelemetry-inställningen till true.
        
    ```
        <setting name="SendTelemetry" serializeAs="String">
                    <value>true</value>
        </setting>
    ```

3.  Spara dina ändringar och starta om tjänsten Windows: lokala data gateway-tjänsten.




## <a name="next-steps"></a>Nästa steg
* [Hantera Analysis Services](analysis-services-manage.md)
* [Hämta data från Azure Analysis Services](analysis-services-connect.md)
