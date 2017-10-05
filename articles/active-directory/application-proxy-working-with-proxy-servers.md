---
title: Arbeta med befintliga lokala proxyservrar och Azure AD | Microsoft Docs
description: Beskriver hur du arbetar med befintliga lokala proxyservrar.
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: kgremban
ms.openlocfilehash: bdca442755507c4ffe8d43692c5b7f2aa3a746f3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="work-with-existing-on-premises-proxy-servers"></a>Arbeta med befintliga lokala proxyservrar

Den här artikeln beskriver hur du konfigurerar Azure Active Directory (Azure AD) Application Proxy kopplingar att arbeta med utgående proxy-servrar. Det är avsett för kunder med nätverksmiljöer som har befintliga proxyservrar.

Vi börjar med att titta på dessa huvudsakliga distributionsscenarier:
* Konfigurera att kopplingar för att kringgå din lokala utgående proxyservrar.
* Konfigurera att kopplingar ska använda en utgående proxy för att komma åt Azure AD Application Proxy.

Mer information om hur kopplingar fungerar finns [förstå Azure AD Application Proxy kopplingar](application-proxy-understand-connectors.md).

## <a name="configure-the-outbound-proxy"></a>Konfigurera utgående proxy

Om du har en utgående proxy i din miljö måste du använda ett konto med behörighet för att konfigurera utgående proxy. Eftersom installationsprogrammet körs i kontexten för den användare som utför installationen, kan du kontrollera konfigurationen med hjälp av Microsoft Edge eller en annan webbläsare.

Konfigurera proxyinställningar i Microsoft Edge:

1. Gå till **inställningar** > **visa avancerade inställningar** > **öppna proxyinställningar** > **manuell Proxy installationsprogrammet**.
2. Ange **använder en proxyserver** till **på**, Välj den **Använd inte proxyservern för lokala adresser (intranät)** kryssrutan och sedan ändra den adress och port för att avspegla din lokal proxyserver.
3. Fyll i de nödvändiga proxyinställningarna.

   ![Dialogrutan för proxy-inställningar](./media/application-proxy-working-with-proxy-servers/proxy-bypass-local-addresses.png)

## <a name="bypass-outbound-proxies"></a>Kringgå utgående proxyservrar

Kopplingar har underliggande OS-komponenter som begär utgående. De här komponenterna försöker automatiskt hitta en proxyserver i nätverket. De kan använda Web Proxy Auto-Discovery (WPAD), om den är aktiverad i miljön.

OS-komponenter kommer att försöka hitta en proxyserver genom att utföra en DNS-sökning för wpad.domainsuffix. Om det löser i DNS, görs sedan en HTTP-begäran till IP-adressen för wpad.dat. Denna begäran blir proxykonfigurationsskript i din miljö. Anslutningen används det här skriptet för att välja en utgående proxyserver. Dock kan connector trafik fortfarande inte går via, på grund av ytterligare konfigurationsinställningar som behövs på proxyservern.

Du kan konfigurera anslutningen för att kringgå proxyservern lokalt för att säkerställa att den använder direkt anslutning till Azure-tjänster. Vi rekommenderar den här metoden (om nätverksprincipen tillåter det), eftersom det innebär att du har en mindre konfiguration att underhålla.

Om du vill inaktivera utgående proxy-användning för anslutningen, redigerar du filen C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config och lägga till den *system.net* avsnittet som visas i det här kodexemplet:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.net>
    <defaultProxy enabled="false"></defaultProxy>
  </system.net>
  <runtime>
    <gcServer enabled="true"/>
  </runtime>
  <appSettings>
    <add key="TraceFilename" value="AadAppProxyConnector.log" />
  </appSettings>
</configuration>
```
För att säkerställa att tjänsten Connector Updater också kringgår proxy, ändra liknande filen ApplicationProxyConnectorUpdaterService.exe.config i C:\Program Files\Microsoft AAD App Proxy Connector Updater.

Se till att göra kopior av de ursprungliga filerna om du behöver återgå till standard .config-filer.

## <a name="use-the-outbound-proxy-server"></a>Utgående proxyserver används

Vissa miljöer kräver all utgående trafik att gå via en utgående proxy utan undantag. Kringgå proxy är därför inte ett alternativ.

Du kan konfigurera connector trafiken gå igenom utgående proxy, som visas i följande diagram:

 ![Konfigurera anslutningen trafik att gå via en utgående proxy till Azure AD Application Proxy](./media/application-proxy-working-with-proxy-servers/configure-proxy-settings.png)

Det finns behöver du inte konfigurera inkommande åtkomst via dina brandväggar på grund av att endast utgående trafik.

### <a name="step-1-configure-the-connector-and-related-services-to-go-through-the-outbound-proxy"></a>Steg 1: Konfigurera anslutningen och tillhörande tjänster gå igenom utgående proxy

Som omfattas tidigare, om WPAD är aktiverad i miljön och korrekt konfigurerad, kommer anslutningen automatiskt identifiera utgående proxyservern och försöka använda den. Du kan uttryckligen konfigurera kopplingen för att gå igenom en utgående proxy.

Gör du genom att redigera filen C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config och lägga till den *system.net* avsnittet som visas i det här kodexemplet. Ändra *proxyserver:8080* återspeglar din lokala Proxyservernamn eller IP-adress och port som lyssnar på.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.net>  
    <defaultProxy>   
      <proxy proxyaddress="http://proxyserver:8080" bypassonlocal="True" usesystemdefault="True"/>   
    </defaultProxy>  
  </system.net>
  <runtime>
    <gcServer enabled="true"/>
  </runtime>
  <appSettings>
    <add key="TraceFilename" value="AadAppProxyConnector.log" />
  </appSettings>
</configuration>
```

Konfigurera Connector Updater-tjänsten om du vill använda proxyservern genom att göra en liknande ändring till filen i C:\Program Files\Microsoft AAD App Proxy Connector Updater\ApplicationProxyConnectorUpdaterService.exe.config.

### <a name="step-2-configure-the-proxy-to-allow-traffic-from-the-connector-and-related-services-to-flow-through"></a>Steg 2: Konfigurera proxyn för att tillåta trafik från koppling och relaterade tjänster kan passera

Det finns fyra olika aspekter att tänka på utgående proxy:
* Proxy utgående regler
* Proxyautentisering
* Proxy-portar
* SSL-kontroll

#### <a name="proxy-outbound-rules"></a>Proxy utgående regler
Tillåt åtkomst till följande slutpunkter för connector service åtkomst:

* *. msappproxy.net
* *. servicebus.windows.net

Tillåt åtkomst till följande slutpunkter för inledande registrering:

* login.windows.net
* login.microsoftonline.com

Om du inte tillåter anslutningar av FQDN och måste du ange IP-adressintervall i stället kan du använda följande alternativ:

* Tillåt anslutningen utgående åtkomst till alla måldatorer.
* Tillåt anslutningen utgående åtkomst till [IP-adressintervall för Azure-datacenter](https://www.microsoft.com/en-gb/download/details.aspx?id=41653). Utmaningen med hjälp av listan över IP-adressintervall för Azure-datacenter är uppdateras varje vecka. Du måste placera en process för att säkerställa att din åtkomstregler uppdateras.

#### <a name="proxy-authentication"></a>Proxyautentisering

Proxy-autentisering stöds inte för närvarande. Vår aktuell rekommendation är att kopplingen anonym åtkomst till Internet-mål.

#### <a name="proxy-ports"></a>Proxy-portar

Kopplingen gör utgående SSL-baserad anslutningar med hjälp av metoden CONNECT. Den här metoden anger i princip en tunnel genom den utgående proxyn. Konfigurera proxyservern så att portarna 443 och 80-tunneltrafik.

>[!NOTE]
>När Service Bus körs via HTTPS, använder port 443. Dock försöker TCP-direktanslutningar Service Bus som standard och faller tillbaka till HTTPS endast om direkt anslutning misslyckades.

Se till att anslutningen inte kan ansluta direkt till Azure-tjänster för portar 9350, 9352 och 5671 för att säkerställa att Service Bus-trafik som också skickas via utgående proxyserver.

#### <a name="ssl-inspection"></a>SSL-kontroll
Använd inte SSL-kontroll för connector-trafik, eftersom den orsakar problem för anslutningen.

## <a name="troubleshoot-connector-proxy-problems-and-service-connectivity-issues"></a>Felsöka problem med anslutningen proxy och anslutningsproblem för tjänsten
Du bör nu se all trafik som passerar genom proxyn. Om du har problem med bör följande felsökningsinformation hjälpa.

Det bästa sättet att identifiera och felsöka problem med anslutningen nätverksanslutningen är att ta en nätverksbild på kopplingstjänsten när kopplingstjänsten. Detta kan vara problematiskt, vi ska titta på snabba tips om att samla in och filtrera nätverksspår.

Du kan använda övervakningsverktyg önskat. Vi använde Microsoft Network Monitor 3.4 vid tillämpningen av den här artikeln. Du kan [ladda ned det från Microsoft](https://www.microsoft.com/download/details.aspx?id=4865).

Exempel och filter som vi använder i följande avsnitt är specifika för Network Monitor, men principerna som kan tillämpas på alla verktyget.

### <a name="take-a-capture-by-using-network-monitor"></a>Ta en avbildning med hjälp av Network Monitor

Starta en avbildning:

1. Öppna Network Monitor och klicka på **nya avbilda**.
2. Klicka på den **starta** knappen.

   ![Fönstret i Network Monitor](./media/application-proxy-working-with-proxy-servers/network-capture.png)

När du har slutfört en avbildning, klickar du på den **stoppa** för att avsluta den.

### <a name="take-a-capture-of-connector-traffic"></a>Ta en avbildning av connector-trafik

Utför följande steg för inledande Felsökning:

1. Stoppa tjänsten Azure AD Application Proxy Connector från services.msc.
2. Starta network-avbildningen.
3. Starta tjänsten Azure AD Application Proxy Connector.
4. Stoppa insamlingen nätverk.

   ![Azure AD Application Proxy Connector-tjänsten i services.msc](./media/application-proxy-working-with-proxy-servers/services-local.png)

### <a name="look-at-the-requests-from-the-connector-to-the-proxy-server"></a>Titta på begäranden från anslutningen till proxyservern

Nu när du har en nätverksbild är du redo att filtrera den. För att titta på spårningen förstå så här filtrerar du avbildningen.

Ett filter är (där 8080 är proxyporten som tjänst):

**(http. Begäran eller http. Svar) och tcp.port==8080**

Om du anger det här filtret i den **visningsfilter** och välj **tillämpa**, filtrerar avbildade trafiken baserat på filtret.

Föregående filtret visas endast de HTTP-begäranden och svar till eller från proxyporten. För en koppling Start där anslutningen är konfigurerad för att använda en proxyserver, visas filtret ungefär så här:

 ![Exempellistan över filtrerade HTTP-begäranden och -svar](./media/application-proxy-working-with-proxy-servers/http-requests.png)

Du söker nu specifikt för CONNECT-begäranden som visar kommunikation med proxyservern. Vid lyckas, kan du få ett OK (200) för HTTP-svar.

Om du ser andra svarskoder, till exempel 407 eller 502, är proxyservern kräver autentisering eller inte tillåter trafiken av någon anledning. Nu kan du kontaktar supportteamet proxy-server.

### <a name="identify-failed-tcp-connection-attempts"></a>Identifiera misslyckade TCP-anslutningsförsök

Den vanligt scenario som du vill ha är när anslutningen försöker ansluta direkt, men den inte.

Ett annat Network Monitor-filter som hjälper dig att enkelt identifiera problemet är:

**Egenskapen. TCPSynRetransmit**

SYN-paket är det första paketet skickas för att upprätta en TCP-anslutning. Om det här paketet inte returnerar ett svar, reattempted SYN. Du kan använda föregående filtret för att se alla att SYN-förfrågningar. Du kan sedan kontrollera om dessa SYN-förfrågningar motsvarar connector-relaterade trafik.

I följande exempel visas ett misslyckade anslutningsförsök till Service Bus-port 9352:

 ![Exempelsvar misslyckade anslutningsförsök](./media/application-proxy-working-with-proxy-servers/failed-connection-attempt.png)

Om du ser något som liknar föregående svaret försöker kopplingen kommunicera direkt med tjänsten Azure Service Bus. Om du räknar kopplingen för att ansluta direkt till Azure-tjänster är en tydlig indikation på att du har ett problem med nätverket eller brandväggen i svaret.

>[!NOTE]
>Om du har konfigurerats för att använda en proxyserver, kan svaret innebära att Service Bus försöker en direkt TCP-anslutning innan du växlar till försöker ansluta via HTTPS.
>

Nätverket trace analys är inte för alla. Men det kan vara ett ovärderligt verktyg för att få snabb information om vad som händer med nätverket.

Om du fortsätter att det blir problem med anslutningen anslutningsproblem, kan du skapa en biljett med vårt supportteam. Gruppen kan hjälpa dig med felsökningen.

Information om hur du löser problem med Application Proxy Connector finns i [felsöka Application Proxy](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-troubleshoot).

## <a name="next-steps"></a>Nästa steg

[Förstå Azure AD Application Proxy-kopplingar](application-proxy-understand-connectors.md)<br>
[Tyst installation av Azure AD Application Proxy Connector](active-directory-application-proxy-silent-installation.md)
