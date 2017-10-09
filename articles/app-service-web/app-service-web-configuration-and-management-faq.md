---
title: "aaaConfiguration vanliga frågor och svar för Azure-webbappar | Microsoft Docs"
description: "Få svar toofrequently frågor och svar om konfiguration och hantering av problem för hello Web Apps-funktionen i Azure App Service."
services: app-service\web
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: genli
ms.openlocfilehash: 5aa405582813291a4aaf144d2f821ecb20a5edcb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configuration-and-management-faqs-for-web-apps-in-azure"></a>Konfiguration och hantering av vanliga frågor och svar för Web Apps i Azure

Den här artikeln innehåller svar toofrequently frågor (FAQ) och om konfiguration och hantering av problem för hello [funktionen Web Apps i Azure App Service](https://azure.microsoft.com/services/app-service/web/).

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="are-there-limitations-i-should-be-aware-of-if-i-want-toomove-app-service-resources"></a>Finns det några begränsningar som jag bör känna till om jag vill toomove Apptjänst resurser?

Om du planerar toomove Apptjänst resurser tooa ny resursgrupp eller prenumeration finns några begränsningar toobe medveten om. Mer information finns i [Apptjänst begränsningar](../azure-resource-manager/resource-group-move-resources.md#app-service-limitations).

## <a name="how-do-i-use-a-custom-domain-name-for-my-web-app"></a>Hur använder ett anpassat domännamn för webbappen?

Svaren toocommon frågor om hur du använder ett anpassat domännamn med ditt Azure webbapp finns i vår sju minutersvideo [lägga till ett anpassat domännamn](https://channel9.msdn.com/blogs/Azure-App-Service-Self-Help/Add-a-Custom-Domain-Name). hello video erbjuder en genomgång av hur tooadd ett anpassat domännamn. Beskriver hur toouse din egen URL i stället för hello *. azurewebsites.net URL med din App Service webbapp. Du kan också se en detaljerad genomgång av [hur toomap ett anpassat domännamn](web-sites-custom-domain-name.md).


## <a name="how-do-i-purchase-a-new-custom-domain-for-my-web-app"></a>Hur köpa en ny anpassad domän för webbappen?

hur toopurchase och skapa en anpassad domän för din App Service webbapp, se toolearn [köpa och konfigurera ett anpassat domännamn i App Service](custom-dns-web-site-buydomains-web-app.md).


## <a name="how-do-i-upload-and-configure-an-existing-ssl-certificate-for-my-web-app"></a>Hur gör överför och konfigurera ett SSL-certifikat för webbappen?

hur tooupload och skapa ett anpassat SSL-certifikat, se toolearn [binda en befintlig anpassad SSL-certifikat tooan Azure webbapp](app-service-web-tutorial-custom-ssl.md#upload).


## <a name="how-do-i-purchase-and-configure-a-new-ssl-certificate-in-azure-for-my-web-app"></a>Hur gör köp och konfigurera ett nytt SSL-certifikat i Azure för webbappen?

hur toopurchase och konfigurera ett SSL-certifikat för webbappen App Service finns toolearn [lägga till en SSL-certifikat tooyour Apptjänst-app](web-sites-purchase-ssl-web-site.md).


## <a name="how-do-i-move-application-insights-resources"></a>Hur jag för att flytta Application Insights-resurser?

Azure Application Insights stöder för närvarande inte hello move-åtgärd. Om din ursprungliga resursgrupp innehåller en Application Insights-resurs, kan du inte flytta den här resursen. Om du inkluderar hello Application Insights-resursen när du försöker toomove en Apptjänst-app, flytta hello hela åtgärden misslyckas. Dock hello Application Insights och hello App Service-plan inte behöver toobe i samma resursgrupp som hello app för hello app toofunction korrekt.

Mer information finns i [Apptjänst begränsningar](../azure-resource-manager/resource-group-move-resources.md#app-service-limitations).

## <a name="where-can-i-find-a-guidance-checklist-and-learn-more-about-resource-move-operations"></a>Var kan jag hitta en checklista för vägledning och lära dig mer om resursen flytta åtgärder?

[Apptjänst begränsningar](../azure-resource-manager/resource-group-move-resources.md#app-service-limitations) visar hur toomove resurser tooeither en ny prenumeration eller tooa ny resurs gruppera i hello samma prenumeration. Du kan få information om hello resurs flytta checklista, Läs mer om vilka tjänster stöder hello flyttningsåtgärd och lär dig mer om Apptjänst begränsningar och annan information.

## <a name="how-do-i-set-hello-server-time-zone-for-my-web-app"></a>Hur ställer jag in hello serverns tidszon för webbappen

tooset hello serverns tidszon för ditt webbprogram:

1. I hello Azure-portalen i din App Service-prenumeration går toohello **programinställningar** menyn.
2. Under **appinställningar**, lägga till den här inställningen:
    * Nyckel = WEBSITE_TIME_ZONE
    * Värde = *hello tidszon*
3. Välj **Spara**.

## <a name="why-do-my-continuous-webjobs-sometimes-fail"></a>Varför min kontinuerliga Webbjobb Ibland misslyckas?

Som standard inaktiveras webbappar om de är inaktiv under en angiven tidsperiod. På så sätt kan hello system spara resurser. I Basic och Standard planer, kan du aktivera hello **alltid på** inställningen tookeep hello webbprogrammet laddas alla hello tid. Om ditt webbprogram kör kontinuerliga Webbjobb, bör du aktivera **alltid på**, eller hello WebJobs kanske inte körs på ett tillförlitligt sätt. Mer information finns i [skapa ett Webbjobb som körs kontinuerligt](web-sites-create-web-jobs.md#CreateContinuous).

## <a name="how-do-i-get-hello-outbound-ip-address-for-my-web-app"></a>Hur skaffar jag hello utgående IP-adress för webbappen

tooget hello lista över utgående IP-adresser för ditt webbprogram:

1. I hello Azure-portalen på web app-bladet går toohello **egenskaper** menyn.
2. Sök efter **utgående ip-adresser**.

hello visas utgående IP-adresser.

Om din webbplats finns i Apptjänst-miljö för PowerApps toolearn hur tooget din utgående IP-adress finns [utgående nätverksadresser](app-service-app-service-environment-network-architecture-overview.md#outbound-network-addresses).

## <a name="how-do-i-get-a-reserved-or-dedicated-inbound-ip-address-for-my-web-app"></a>Hur skaffar jag en reserverad eller dedikerade inkommande IP-adress för webbappen

tooset upp en dedikerad eller reserverade IP-adress för inkommande anrop tooyour webbplats för Azure-program, installera och konfigurera ett IP-baserade SSL-certifikat.

Observera att ett dedikerat toouse eller reserverade IP-adressen för inkommande samtal App Service-plan måste vara i en grundläggande eller senare serviceplan.

## <a name="can-i-export-my-app-service-certificate-toouse-outside-azure-such-as-for-a-website-hosted-elsewhere"></a>Kan jag exportera min App Service certifikat toouse utanför Azure som värd för en webbplats på en annan plats? 

Apptjänstcertifikat betraktas som Azure-resurser. De är inte avsedda toouse utanför Azure-tjänster. Du kan inte exportera dem toouse utanför Azure. Mer information finns i [vanliga frågor och svar för apptjänstcertifikat och anpassade domäner](https://social.msdn.microsoft.com/Forums/azure/f3e6faeb-5ed4-435a-adaa-987d5db43b80/faq-on-app-service-certificates-and-custom-domains?forum=windowsazurewebsitespreview).

## <a name="can-i-export-my-app-service-certificate-toouse-with-other-azure-cloud-services"></a>Kan jag exportera toouse min App Service-certifikat med andra Azure-molntjänster?

hello-portalen innehåller en förstklassig miljö för att distribuera ett certifikat för App Service via Azure Key Vault tooApp-appar. Men har vi fått förfrågningar från kunder toouse certifikaten utanför hello Apptjänst-plattformen, till exempel med virtuella datorer i Azure. toolearn hur toocreate en lokal PFX-kopia av din Apptjänst certifikat så att du kan använda hello certifikat med andra Azure-resurser, se [skapa en lokal PFX-kopia av ett certifikat för App Service](https://blogs.msdn.microsoft.com/appserviceteam/2017/02/24/creating-a-local-pfx-copy-of-app-service-certificate/).

Mer information finns i [vanliga frågor och svar för apptjänstcertifikat och anpassade domäner](https://social.msdn.microsoft.com/Forums/azure/f3e6faeb-5ed4-435a-adaa-987d5db43b80/faq-on-app-service-certificates-and-custom-domains?forum=windowsazurewebsitespreview).


## <a name="why-do-i-see-hello-message-partially-succeeded-when-i-try-tooback-up-my-web-app"></a>Varför ser jag hello-meddelande som ”delvis lyckades” när jag försöker tooback konfigurera min webbprogram?

En vanlig orsak till Säkerhetskopieringsfel är att vissa filer som används av programmet hello. Filer som används är låsta när du utför hello säkerhetskopiering. Detta förhindrar att de här filerna säkerhetskopieras och kan resultera i statusen ”delvis har slutförts”. Du kan eventuellt förhindra detta genom att utesluta filer från hello säkerhetskopieringen. Du kan välja tooback upp som krävs. Mer information finns i [säkerhetskopiera bara hello viktiga delar av din webbplats med Azure webbappar](http://www.zainrizvi.io/2015/06/05/creating-partial-backups-of-your-site-with-azure-web-apps/).

## <a name="how-do-i-remove-a-header-from-hello-http-response"></a>Hur tar jag bort ett sidhuvud från hello HTTP-svar

tooremove hello meddelandehuvuden från hello HTTP-svar uppdatera din webbplats web.config-filen. Mer information finns i [ta bort standard server rubriker på Azure-webbplatser](https://azure.microsoft.com/blog/removing-standard-server-headers-on-windows-azure-web-sites/).

## <a name="is-app-service-compliant-with-pci-standard-30-and-31"></a>Är Apptjänst som är kompatibel med PCI Standard 3.0 och 3.1?

För närvarande hello Web Apps funktion i Azure App Service är kompatibla med PCI Data Security Standard (DSS) version 3.0 nivå 1. PCI DSS version 3.1 finns på vår översikt. Planera pågår redan för hur tillämpning av hello senaste standard kommer att fortsätta.

PCI DSS version 3.1 certifikatutfärdare kräver inaktiverar Transport Layer Security (TLS) 1.0. Inaktivera TLS 1.0 är för närvarande inte ett alternativ för de flesta App Service-planer. Om du använder Apptjänstmiljö eller är villigt toomigrate din arbetsbelastning tooApp-miljö kan du få större kontroll över din miljö. Detta innebär att inaktivera TLS 1.0 genom att kontakta Azure-supporten. I hello nära framtiden kommer toomake dessa inställningar tillgängliga toousers.

Mer information finns i [Microsoft Azure App Service web appkompatibilitet med PCI Standard 3.0 och 3.1](https://support.microsoft.com/help/3124528).

## <a name="how-do-i-use-hello-staging-environment-and-deployment-slots"></a>Hur använder hello mellanlagring miljö och distributionsplatser?

I Standard- och Premium App Service-planer, när du distribuerar din web app tooApp tjänsten, kan du distribuera tooa separat distributionsplats i stället för toohello standard produktionsplatsen. Distributionsplatser är live-webb-appar som har sina egna värdnamn. Web app innehåll och konfiguration element kan växlas mellan två distributionsplatser, inklusive hello produktionsplatsen.

Mer information om hur du använder distributionsplatser finns [skapa mellanlagringsmiljön i App Service](web-sites-staged-publishing.md).

## <a name="how-do-i-access-and-review-webjob-logs"></a>Hur gör åtkomst och granska loggarna för Webbjobb?

tooreview Webbjobb loggar:

1. Logga in tooyour [Kudu webbplats](https://*yourwebsitename*.scm.azurewebsites.net).
2. Välj hello Webbjobb.
3. Välj hello **växla utdata** knappen.
4. toodownload hello utdatafilen, Välj hello **hämta** länk.
5. Enskilda körs Välj **enskilda anropa**.
6. Välj hello **växla utdata** knappen.
7. Välj hello länken.

## <a name="im-trying-toouse-hybrid-connections-with-sql-server-why-do-i-see-hello-message-systemoverflowexception-arithmetic-operation-resulted-in-an-overflow"></a>Du försöker toouse Hybridanslutningar med SQL Server. Varför ser jag hello-meddelande ”System.OverflowException: aritmetiska operationen orsakade spill”?

Om du använder Hybridanslutningar tooaccess SQL Server kan orsaka en Microsoft .NET-uppdatering på 10 maj 2016 anslutningar toofail. Du kan se det här meddelandet:

```
Exception: System.Data.Entity.Core.EntityException: hello underlying provider failed on Open. —> System.OverflowException: Arithmetic operation resulted in an overflow. or (64 bit Web app) System.OverflowException: Array dimensions exceeded supported range, at System.Data.SqlClient.TdsParser.ConsumePreLoginHandshake
```

### <a name="resolution"></a>Lösning

Vi arbetar tooupdate Hybridanslutningshanteraren toofix problemet. Lösningar, se [Hybridanslutningar fel med SQL Server: System.OverflowException: aritmetiska operationen orsakade spill](https://blogs.msdn.microsoft.com/waws/2016/05/17/hybrid-connection-error-with-sql-server-system-overflowexception-arithmetic-operation-resulted-in-an-overflow/).

## <a name="how-do-i-add-or-edit-a-url-rewrite-rule"></a>Hur jag för att lägga till eller redigera en URL-omskrivning regel?

tooadd eller redigera en URL-omskrivning regel:

1. Konfigurera Internet Information Services (IIS) Manager så att den ansluter tooyour App Service webbapp. hur tooconnect IIS-hanteraren tooApp tjänsten, se toolearn [fjärradministration av Azure webbplatser med hjälp av IIS-hanteraren](https://azure.microsoft.com/blog/remote-administration-of-windows-azure-websites-using-iis-manager/).
2. I IIS-hanteraren, Lägg till eller redigera en URL-omskrivning regel. toolearn hur tooadd eller redigera en URL-omskrivning om regeln, se [skapa omarbetning regler för URL: en för hello omarbetning modulen](https://www.iis.net/learn/extensions/url-rewrite-module/creating-rewrite-rules-for-the-url-rewrite-module).

## <a name="how-do-i-control-inbound-traffic-tooapp-service"></a>Hur kan jag styra inkommande trafik tooApp tjänsten?

På hello platsnivå har du två alternativ för att styra inkommande trafik tooApp tjänsten:

* Aktivera dynamisk IP-begränsningar. hur tooturn på dynamisk IP-adressbegränsningar Se toolearn [IP- och begränsningar för Azure websites](https://azure.microsoft.com/blog/ip-and-domain-restrictions-for-windows-azure-web-sites/).
* Aktivera modulen säkerhet. hur tooturn i modulen, se toolearn [Brandvägg för ModSecurity webbaserade program på Azure websites](https://azure.microsoft.com/blog/modsecurity-for-azure-websites/).

Om du använder Apptjänst-miljö kan du använda [Barracuda brandväggen](https://azure.microsoft.com/blog/configuring-barracuda-web-application-firewall-for-azure-app-service-environment/).

## <a name="how-do-i-block-ports-in-an-app-service-web-app"></a>Hur blockerar portar i en App Service webbapp?

Hej Apptjänst delade klient miljö kan inte möjligt tooblock specifika portar på grund av hello uppbyggnad hello infrastruktur. TCP-portar 4016 och 4018 4020 också vara öppen för Visual Studio fjärrfelsökning.

I Apptjänst-miljön har du full kontroll över inkommande och utgående trafik. Du kan använda Nätverkssäkerhetsgrupper toorestrict eller blockera specifika portar. Mer information om Apptjänst-miljö finns [introduktion till Apptjänstmiljö](https://azure.microsoft.com/blog/introducing-app-service-environment/).

## <a name="how-do-i-capture-an-f12-trace"></a>Hur jag för att avbilda en F12 trace?

Har du två alternativ för att samla in en F12-spårning:

* F12 http-spårning
* F12 konsolens utdata

### <a name="f12-http-trace"></a>F12 http-spårning

1. Gå tooyour webbplats i Internet Explorer. Det är viktigt toosign innan du hello nästa steg. Annars avbildar hello F12 trace känsliga inloggning data.
2. Tryck på F12.
3. Kontrollera att hello **nätverk** är markerad och välj sedan hello grön **spela upp** knappen.
4. Hello steg som återskapa hello utfärdar.
5. Välj hello red **stoppa** knappen.
6. Välj hello **spara** knappen (diskikonen) och spara hello HAR (i Internet Explorer och Edge) *eller* högerklickar du på hello HAR och välj sedan **Spara som HAR med innehåll** () i Chrome).

### <a name="f12-console-output"></a>F12 konsolens utdata

1. Välj hello **konsolen** fliken.
2. För varje flik som innehåller fler än noll objekt, väljer du fliken hello (**fel**, **varning**, eller **Information**). Om hello fliken inte är markerat är hello tab-ikon grå eller svart när du flyttar markören hello bort från den.
3. Högerklicka i hello meddelandeområdet i hello fönstret och välj sedan **kopiera alla**.
4. Klistra in hello kopierade texten i en fil och sparar hello-filen.

tooview HAR-fil som du kan använda hello [HAR viewer](http://www.softwareishard.com/har/viewer/).

## <a name="why-do-i-get-an-error-when-i-try-tooconnect-an-app-service-web-app-tooa-virtual-network-that-is-connected-tooexpressroute"></a>Varför visas ett fel när jag försöker tooconnect en App Service web app tooa virtuellt nätverk som är anslutna tooExpressRoute?

Om du försöker tooconnect ett Azure web app tooa virtuella nätverk som har anslutit tooAzure ExpressRoute misslyckas. hello visas följande meddelande: ”Gateway är inte en VPN-gateway”.

Du kan för närvarande inte punkt-till-plats VPN-anslutningar tooa virtuellt nätverk som är anslutna tooExpressRoute. En punkt-till-plats VPN- och ExpressRoute kan inte samexistera för hello samma virtuella nätverk. Mer information finns i [ExpressRoute- och plats-till-plats VPN-anslutningar gränser och begränsningar](../expressroute/expressroute-howto-coexist-classic.md#limits-and-limitations).

## <a name="how-do-i-connect-an-app-service-web-app-tooa-virtual-network-that-has-a-static-routing-policy-based-gateway"></a>Hur ansluter en App Service web app tooa virtuellt nätverk som har en statisk routning (principbaserad) gateway?

För närvarande kan stöds ansluta en App Service web app tooa virtuellt nätverk som har en statisk routning (principbaserad) gateway inte. Om det virtuella nätverket målet redan finns måste den ha punkt-till-plats VPN är aktiverat innan den kan vara anslutna tooan app med en dynamisk routning gateway. Om din gateway är toostatic routning, kan du aktivera en punkt-till-plats-VPN. 

Mer information finns i [integrera en app med Azure-nätverk](web-sites-integrate-with-vnet.md#getting-started).

## <a name="in-my-app-service-environment-why-can-i-create-only-one-app-service-plan-even-though-i-have-two-workers-available"></a>I min Apptjänst-miljö, varför kan jag skapa endast en App Service-plan, trots att jag har två personer som är tillgängliga?

tooprovide feltolerans Apptjänst-miljön kräver att varje arbetspool måste minst en ytterligare beräkningsresurser. hello ytterligare beräkningsresurser kan inte tilldelas en arbetsbelastning.

Mer information finns i [hur toocreate en Apptjänstmiljö](app-service-web-how-to-create-an-app-service-environment.md).

## <a name="why-do-i-see-timeouts-when-i-try-toocreate-an-app-service-environment"></a>Varför ser jag timeout när jag försöker toocreate en Apptjänst-miljö?

Ibland kan misslyckas skapa en Apptjänst-miljö. I så fall kan du se följande fel i aktiviteten loggar hello hello:
```
ResourceID: /subscriptions/{SubscriptionID}/resourceGroups/Default-Networking/providers/Microsoft.Web/hostingEnvironments/{ASEname}
Error:{"error":{"code":"ResourceDeploymentFailure","message":"hello resource provision operation did not complete within hello allowed timeout period.”}}
```

tooresolve, se till att ingen av hello följande villkor är uppfyllda:
* hello undernät är för liten.
* hello är inte tomt.
* ExpressRoute förhindrar hello anslutningskrav av en Apptjänst-miljö.
* En felaktig Nätverkssäkerhetsgrupp förhindrar hello anslutningskrav av en Apptjänst-miljö.
* Tvingad tunneling är aktiverat.

Mer information finns i [frekventa problem när du distribuerar (skapa) en ny Azure Apptjänst-miljön](https://blogs.msdn.microsoft.com/waws/2016/05/13/most-frequent-issues-when-deploying-creating-a-new-azure-app-service-environment-ase/).

## <a name="why-cant-i-delete-my-app-service-plan"></a>Varför kan jag ta bort App Service-plan

Du kan inte ta bort en apptjänstplan om Apptjänst-appar som är associerade med hello App Service-plan. Ta bort alla associerade Apptjänst-appar från hello programtjänstplanen innan du tar bort en programtjänstplan.

## <a name="how-do-i-schedule-a-webjob"></a>Hur jag för att schemalägga ett Webbjobb?

Du kan skapa ett schemalagda Webbjobb med hjälp av Cron-uttryck:

1. Skapa en settings.job-fil.
2. I JSON-filen är en schema-egenskap med ett Cron-uttryck: 
    ```
    { "schedule": "{second}
    {minute} {hour} {day}
    {month} {day of hello week}" }
    ```

Läs mer om schemalagda WebJobs [skapa schemalagda Webbjobb med hjälp av ett Cron-uttryck](web-sites-create-web-jobs.md#CreateScheduledCRON).

## <a name="how-do-i-perform-penetration-testing-for-my-app-service-app"></a>Hur gör intrång testa för Apptjänst-app?

tooperform intrång testning, [skicka en begäran om](https://security-forms.azure.com/penetration-testing/terms).

## <a name="how-do-i-configure-a-custom-domain-name-for-an-app-service-web-app-that-uses-traffic-manager"></a>Konfigurera ett anpassat domännamn för en Apptjänst-webbapp som använder Traffic Manager?

hur toouse ett anpassat domännamn med en Apptjänst-app som använder Azure Traffic Manager för belastningsutjämning, se toolearn [och konfigurera ett anpassat domännamn för en Azure-webbapp med Traffic Manager](web-sites-traffic-manager-custom-domain-name.md).

## <a name="my-app-service-certificate-is-flagged-for-fraud-how-do-i-resolve-this"></a>Min App Tjänstcertifikatet har flaggats för bedrägeri. Hur lösa problemet?

Under hello domänverifiering från inköpsdatumet en Apptjänst-certifikat, kan du se hello följande meddelande:

”Ditt certifikat har flaggats för eventuellt bedrägeriförsök. hello-begäran är för närvarande granskas. Om hello certifikatet inte blir kan användas inom 24 timmar, kontakta Azure Support ”.

Som visar hello-meddelande, kan bedrägeri verifieringsprocessen ta upp too24 timmar toocomplete. Under denna tid fortsätter du toosee hello-meddelande.

Om din App tjänstcertifikat fortsätter tooshow det här meddelandet efter 24 timmar, kör du hello följande PowerShell-skript. hello skript kontakter hello [certifikatutfärdaren](https://www.godaddy.com/) direkt tooresolve hello problemet.

```
Login-AzureRmAccount
Set-AzureRmContext -SubscriptionId <subId>
$actionProperties = @{
    "Name"= "<Customer Email Address>"
    };
Invoke-AzureRmResourceAction -ResourceGroupName "<App Service Certificate Resource Group Name>" -ResourceType Microsoft.CertificateRegistration/certificateOrders -ResourceName "<App Service Certificate Resource Name>" -Action resendRequestEmails -Parameters $actionProperties -ApiVersion 2015-08-01 -Force   
```

## <a name="how-do-authentication-and-authorization-work-in-app-service"></a>Hur autentisering och auktorisering fungerar i App Service?

Mer detaljerad dokumentation för autentisering och auktorisering i Apptjänst finns [Apptjänst säkerhet](../app-service/app-service-security-readme.md). hello dokumentationen innehåller även information om hur du konfigurerar Apptjänst toouse olika identifiera providern inloggningar:
* [Azure Active Directory](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md)
* [Facebook](../app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication.md)
* [Google](../app-service-mobile/app-service-mobile-how-to-configure-google-authentication.md)
* [Microsoft-konto](../app-service-mobile/app-service-mobile-how-to-configure-microsoft-authentication.md)
* [Twitter](../app-service-mobile/app-service-mobile-how-to-configure-twitter-authentication.md)

## <a name="how-do-i-redirect-hello-default-azurewebsitesnet-domain-toomy-azure-web-apps-custom-domain"></a>Hur omdirigera hello standard *. azurewebsites.net domän toomy Azure webbapp domänen?

När du skapar en ny webbplats med hjälp av Web Apps i Azure, standard *sitename*. azurewebsites.net tilldelas tooyour plats. Om du lägger till en anpassad värd namn tooyour plats och inte vill att användarna toobe kan tooaccess standard *. azurewebsites.net domän, kan du omdirigera hello standard-URL. toolearn hur tooredirect all trafik från din webbplats standard domän tooyour anpassad domän, finns i [omdirigering hello domän tooyour anpassade standarddomän i Azure web apps](http://www.zainrizvi.io/2016/04/07/block-default-azure-websites-domain/).

## <a name="how-do-i-determine-which-version-of-net-version-is-installed-in-app-service"></a>Hur tar jag reda på vilken version av .NET version är installerad i App Service?

hello snabbaste sättet toofind hello versionen av Microsoft .NET som är installerad i App Service är via hello Kudu-konsolen. Du kan komma åt hello Kudu-konsolen från hello-portalen eller genom att använda hello URL för din Apptjänst-app. Detaljerade instruktioner finns [avgör hello installerat .NET version i App Service](https://blogs.msdn.microsoft.com/waws/2016/11/02/how-to-determine-the-installed-net-version-in-azure-app-services/).

## <a name="why-isnt-autoscale-working-as-expected"></a>Varför inte Autoskala fungerar som förväntat?

Om Azure Autoskalningsfunktionen har inte skalas i eller utskalad hello web app-instansen som du väntade dig kanske körs i ett scenario där vi avsiktligt välja inte tooscale tooavoid en oändlig loop på grund av för ”växlar”. Detta inträffar vanligtvis när det inte finns en tillräcklig marginal mellan hello skalbar och skala i tröskelvärden. hur tooavoid ”växlar” och tooread om andra Autoskala bästa praxis, se toolearn [Autoskala metodtips](../monitoring-and-diagnostics/insights-autoscale-best-practices.md#autoscale-best-practices).

## <a name="why-does-autoscale-sometimes-scale-only-partially"></a>Varför Autoskala ibland skalas endast delvis?

Autoskala utlöses när mått överstiger förkonfigurerade gränser. Ibland kanske du märker att hello är bara delvis fyllda jämfört med toowhat som du förväntade dig. Detta kan inträffa när hello antalet instanser som du vill använda inte är tillgängliga. I det scenariot fyller Autoskala delvis in med hello tillgängligt antal instanser. Autoskala körs hello balansera logik tooget mer kapacitet. Den allokerar hello återstående instanser. Observera att det kan ta några minuter.

Om du inte ser hello förväntat antal instanser efter några minuter, kan det bero på att hello delvis påfyllning har tillräckligt med toobring hello mått inom hello gränser. Eller så kanske har skalats Autoskala eftersom den nått hello nedre gräns för mått.

Om ingen av dessa villkor gäller och hello problemet kvarstår kan du skicka en supportbegäran.

## <a name="how-do-i-turn-on-http-compression-for-my-content"></a>Hur aktiverar jag HTTP-komprimering för min innehåll?

tooturn på komprimering både för statiska och dynamiska typer av innehåll, Lägg till hello följande kod toohello på programnivå web.config-filen:

```
<system.webServer>
<urlCompression doStaticCompression="true" doDynamicCompression="true" />
< /system.webServer>
```

Du kan också ange hello specifika dynamiska och statiska MIME-typer som du vill toocompress. Mer information finns i vårt forum svar tooa frågan i [httpCompression inställningarna på en enkel Azure-webbplatsen](https://social.msdn.microsoft.com/Forums/azure/890b6d25-f7dd-4272-8970-da7798bcf25d/httpcompression-settings-on-a-simple-azure-website?forum=windowsazurewebsitespreview).

## <a name="how-do-i-migrate-from-an-on-premises-environment-tooapp-service"></a>Hur migrera från en lokal miljö tooApp tjänsten?

toomigrate platser från Windows- och Linux web servrar tooApp tjänsten, kan du använda Azure App Service Migration Assistant. hello Migreringsverktyget skapar webbappar och databaser i Azure efter behov och sedan publicerar hello innehåll. Mer information finns i [Azure App Service Migration Assistant](https://www.movemetothecloud.net/).
