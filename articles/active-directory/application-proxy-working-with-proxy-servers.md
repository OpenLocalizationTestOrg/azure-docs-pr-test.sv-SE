---
title: aaaWork med befintliga lokala proxyservrar och Azure AD | Microsoft Docs
description: Beskriver hur toowork med befintliga lokala proxyservrar.
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
ms.openlocfilehash: 7f8cec4f676f99bead5211bcbcf23056bd7f211f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="work-with-existing-on-premises-proxy-servers"></a>Arbeta med befintliga lokala proxyservrar

Den här artikeln förklarar hur tooconfigure Azure Active Directory (Azure AD) Application Proxy kopplingar toowork med utgående proxy-servrar. Det är avsett för kunder med nätverksmiljöer som har befintliga proxyservrar.

Vi börjar med att titta på dessa huvudsakliga distributionsscenarier:
* Konfigurera kopplingar toobypass din lokala utgående proxyservrar.
* Konfigurera kopplingar toouse en utgående proxy tooaccess Azure AD Application Proxy.

Mer information om hur kopplingar fungerar finns [förstå Azure AD Application Proxy kopplingar](application-proxy-understand-connectors.md).

## <a name="configure-hello-outbound-proxy"></a>Konfigurera hello utgående proxy

Om du har en utgående proxy i din miljö kan du använda ett konto med behörighet tooconfigure hello utgående proxy. Eftersom hello installationsprogrammet körs i hello kontexten för hello användare som utför installationen hello, kan du kontrollera hello konfigurationen med hjälp av Microsoft Edge eller en annan webbläsare.

tooconfigure hello proxyinställningarna i Microsoft Edge:

1. Gå för**inställningar** > **visa avancerade inställningar** > **öppna proxyinställningar** > **manuell Proxy-inställningar** .
2. Ange **använder en proxyserver** för**på**väljer hello **inte använder hello proxyserver för lokala adresser (intranät)** kryssrutan och sedan ändra hello-adress och port tooreflect en lokal proxyserver.
3. Fyll i hello nödvändiga proxyinställningar.

   ![Dialogrutan för proxy-inställningar](./media/application-proxy-working-with-proxy-servers/proxy-bypass-local-addresses.png)

## <a name="bypass-outbound-proxies"></a>Kringgå utgående proxyservrar

Kopplingar har underliggande OS-komponenter som begär utgående. De här komponenterna försöker automatiskt toolocate en proxyserver i hello nätverk. De kan använda Web Proxy Auto-Discovery (WPAD), om den är aktiverad i hello-miljö.

hello OS komponenter försök toolocate en proxyserver genom att utföra en DNS-sökning för wpad.domainsuffix. Om det löser i DNS, sedan en HTTP-begäran görs toohello IP-adress för wpad.dat. Denna begäran blir hello proxykonfigurationsskript i din miljö. hello-anslutningen använder det här skriptet tooselect en utgående proxyserver. Dock kan connector trafik fortfarande inte går via, på grund av ytterligare konfiguration krävs på hello proxy.

Du kan konfigurera hello connector toobypass din lokala proxy tooensure som används för direkt anslutning toohello Azure services. Vi rekommenderar den här metoden (om nätverksprincipen tillåter det), eftersom det innebär att du har en mindre configuration toomaintain.

toodisable utgående proxy-användning för hello connector redigera hello C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config filen och Lägg till hello *system.net* avsnittet som visas i det här kodexemplet :

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
tooensure hello Connector Updater tjänsten också kringgår hello proxy, se en liknande ändra toohello ApplicationProxyConnectorUpdaterService.exe.config-fil som finns i C:\Program Files\Microsoft AAD App Proxy Connector Updater.

Vara säker på att toomake kopior av hello ursprungliga filer om du behöver toorevert toohello standard .config-filer.

## <a name="use-hello-outbound-proxy-server"></a>Använd hello utgående proxyserver

Vissa miljöer kräver att alla utgående trafik toogo via en utgående proxy utan undantag. Kringgå proxy för hello är därför inte ett alternativ.

Du kan konfigurera hello connector trafik toogo via hello utgående proxy, som visas i följande diagram hello:

 ![Konfigurera anslutningen trafik toogo via en utgående proxy tooAzure AD Application Proxy](./media/application-proxy-working-with-proxy-servers/configure-proxy-settings.png)

Som ett resultat av med endast utgående trafik, det finns inget behov av tooconfigure inkommande åtkomst via dina brandväggar.

### <a name="step-1-configure-hello-connector-and-related-services-toogo-through-hello-outbound-proxy"></a>Steg 1: Konfigurera hello-anslutningen och relaterade tjänster toogo via hello utgående proxy

Som omfattas tidigare, om WPAD är aktiverat i hello miljö och korrekt konfigurerad, hello connector automatiskt identifiera hello utgående proxy-servern och försök toouse den. Du kan uttryckligen konfigurera hello connector toogo via en utgående proxy.

toodo så redigera hello C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config filen och Lägg till hello *system.net* avsnittet som visas i det här kodexemplet. Ändra *proxyserver:8080* tooreflect din lokala Proxyservernamn eller IP-adress och hello porten den lyssnar på.

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

Konfigurera hello Connector Updater toouse hello proxyservern genom att göra en liknande ändra toohello-fil som finns i C:\Program Files\Microsoft AAD App Proxy Connector Updater\ApplicationProxyConnectorUpdaterService.exe.config.

### <a name="step-2-configure-hello-proxy-tooallow-traffic-from-hello-connector-and-related-services-tooflow-through"></a>Steg 2: Konfigurera hello proxy tooallow trafik från hello koppling och relaterade tjänster tooflow via

Det finns fyra aspekter tooconsider på hello utgående proxy:
* Proxy utgående regler
* Proxyautentisering
* Proxy-portar
* SSL-kontroll

#### <a name="proxy-outbound-rules"></a>Proxy utgående regler
Tillåt åtkomst toohello följande slutpunkter för åtkomst till anslutningen:

* *. msappproxy.net
* *. servicebus.windows.net

Tillåt åtkomst toohello följande slutpunkter för inledande registrering:

* login.windows.net
* login.microsoftonline.com

Om du inte tillåter anslutningar av FQDN och behöver toospecify IP-adressintervall i stället kan du använda följande alternativ:

* Tillåt utgående åtkomst för hello connector tooall mål.
* Tillåt utgående åtkomst för hello connector för[IP-adressintervall för Azure-datacenter](https://www.microsoft.com/en-gb/download/details.aspx?id=41653). hello utmaningen med hello lista med IP-adressintervall för Azure-datacenter är uppdateras varje vecka. Du måste tooput en process i plats tooensure att reglerna för access uppdateras.

#### <a name="proxy-authentication"></a>Proxyautentisering

Proxy-autentisering stöds inte för närvarande. Vår aktuell rekommendation är tooallow hello connector anonym åtkomst toohello Internet mål.

#### <a name="proxy-ports"></a>Proxy-portar

hello connector gör utgående SSL-baserad anslutningar med hjälp av metoden för hello CONNECT. Den här metoden anger i princip en tunnel genom hello utgående proxy. Konfigurera hello proxy server tooallow tunneling tooports 443 och 80.

>[!NOTE]
>När Service Bus körs via HTTPS, använder port 443. Dock försöker TCP-direktanslutningar Service Bus som standard och faller tillbaka tooHTTPS endast om direkt anslutning misslyckades.

tooensure som Hej Service Bus trafik som också skickas via hello utgående proxyserver, kontrollera hello kopplingen inte kan ansluta direkt toohello Azure-tjänster för portar 9350 och 9352 5671.

#### <a name="ssl-inspection"></a>SSL-kontroll
Använd inte SSL-kontroll för hello connector trafik, eftersom den orsakar problem för hello connector-trafik.

## <a name="troubleshoot-connector-proxy-problems-and-service-connectivity-issues"></a>Felsöka problem med anslutningen proxy och anslutningsproblem för tjänsten
Du bör nu se all trafik som passerar genom hello proxy. Om du har problem bör hjälpa hello efter felsökningsinformation.

Hej bästa sätt tooidentify och felsöka-anslutning problem är tootake ett nätverk avbilda på hello kopplingstjänsten när hello kopplingstjänsten. Detta kan vara problematiskt, vi ska titta på snabba tips om att samla in och filtrera nätverksspår.

Du kan använda hello övervakning verktyg som helst. Vi använde Microsoft Network Monitor 3.4 hello enligt den här artikeln. Du kan [ladda ned det från Microsoft](https://www.microsoft.com/download/details.aspx?id=4865).

hello exempel och filter som vi använder i följande avsnitt hello är särskilda tooNetwork övervakaren men hello principer kan vara tillämpade tooany verktyget.

### <a name="take-a-capture-by-using-network-monitor"></a>Ta en avbildning med hjälp av Network Monitor

toostart en avbildning:

1. Öppna Network Monitor och klicka på **nya avbilda**.
2. Klicka på hello **starta** knappen.

   ![Fönstret i Network Monitor](./media/application-proxy-working-with-proxy-servers/network-capture.png)

När du har slutfört en avbildning, klickar du på hello **stoppa** knappen tooend den.

### <a name="take-a-capture-of-connector-traffic"></a>Ta en avbildning av connector-trafik

Utför följande hello inledande felsökningsinformation:

1. Stoppa hello Azure AD Application Proxy Connector tjänsten från services.msc.
2. Starta hello nätverksbild.
3. Starta hello Azure AD Application Proxy Connector-tjänsten.
4. Stoppa hello nätverksbild.

   ![Azure AD Application Proxy Connector-tjänsten i services.msc](./media/application-proxy-working-with-proxy-servers/services-local.png)

### <a name="look-at-hello-requests-from-hello-connector-toohello-proxy-server"></a>Titta på hello begäranden från hello connector toohello proxyserver

Nu när du har en nätverksbild du är klar toofilter den. hello viktiga toolooking på hello trace är att förstå hur toofilter hello avbilda.

Ett filter är (där 8080 är hello Proxyport service):

**(http. Begäran eller http. Svar) och tcp.port==8080**

Om du anger det här filtret i hello **visningsfilter** och välj **tillämpa**, filtrerar hello avbildas trafik utifrån hello filter.

hello föregående filtret visas bara hello HTTP-begäranden och -svar från hello Proxyport. För en koppling Start där hello connector är konfigurerade toouse en proxyserver, visas hello filter ungefär så här:

 ![Exempellistan över filtrerade HTTP-begäranden och -svar](./media/application-proxy-working-with-proxy-servers/http-requests.png)

Du nu söker specifikt hello Anslut begäranden som visar kommunikation med hello proxyserver. Vid lyckas, kan du få ett OK (200) för HTTP-svar.

Om du ser andra svarskoder, till exempel 407 eller 502, är hello proxy kräver autentisering eller inte tillåter trafik för hello av någon anledning. Nu kan du kontaktar supportteamet proxy-server.

### <a name="identify-failed-tcp-connection-attempts"></a>Identifiera misslyckade TCP-anslutningsförsök

hello andra vanligt scenario som du vill ha är när hello koppling försöker tooconnect direkt, men den inte.

Ett annat Network Monitor-filter som hjälper tooeasily identifiera problemet är:

**Egenskapen. TCPSynRetransmit**

SYN-paket är hello första paketet skickas tooestablish en TCP-anslutning. Om det här paketet inte returnerar ett svar, reattempted hello SYN. Du kan använda hello föregående filter toosee alla att SYN-förfrågningar. Du kan sedan kontrollera om dessa SYN-förfrågningar motsvarar tooany connector-relaterad trafik.

hello följande exempel visas ett misslyckade anslutningsförsök tooService Bus port 9352:

 ![Exempelsvar misslyckade anslutningsförsök](./media/application-proxy-working-with-proxy-servers/failed-connection-attempt.png)

Om du ser något som liknar föregående svar hello försöker hello koppling toocommunicate direkt med hello Azure Service Bus-tjänst. Om du förväntar dig hello connector toomake direkta anslutningar toohello Azure-tjänster, svaret är en tydlig indikation på att du har ett problem med nätverket eller brandväggen.

>[!NOTE]
>Om du konfigurerade toouse en proxyserver, kan svaret innebära att Service Bus försöker en direkt TCP-anslutning innan du byter tooattempting en anslutning via HTTPS.
>

Nätverket trace analys är inte för alla. Men det kan vara ett ovärderligt verktyg tooget snabbt information om vad som händer med nätverket.

Om du fortsätter toostruggle med connector anslutningsproblem kan du skapa en biljett med vårt supportteam. hello-teamet kan hjälpa dig med felsökningen.

Information om hur du löser problem med Application Proxy Connector finns i [felsöka Application Proxy](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-troubleshoot).

## <a name="next-steps"></a>Nästa steg

[Förstå Azure AD Application Proxy-kopplingar](application-proxy-understand-connectors.md)<br>
[Hur toosilently installerar hello Azure AD Application Proxy Connector](active-directory-application-proxy-silent-installation.md)
