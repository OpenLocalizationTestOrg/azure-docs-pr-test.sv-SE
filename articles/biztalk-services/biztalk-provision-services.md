---
title: "aaaCreate Azure BizTalk-tjänst i hello Azure-portalen | Microsoft Docs"
description: "Lär dig hur tooprovision eller skapa Azure BizTalk-tjänst i hello Azure-portalen; MABS WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 3ad18876-a649-40d6-9aa0-1509c1d62c43
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 6781cadada8ac9c84e1fe045d2b0f995811f75b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-biztalk-services-using-hello-azure-portal"></a>Skapa BizTalk-tjänster med hjälp av hello Azure-portalen

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]


> [!TIP]
> toosign i toohello Azure-portalen som du behöver ett Azure-konto och Azure-prenumeration. Om du inte har något konto kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter. Se [Kostnadsfri utvärderingsversion av Azure](http://go.microsoft.com/fwlink/p/?LinkID=239738).


## <a name="CreateService"></a>Skapa en BizTalk-tjänst
Beroende på hello Edition som du väljer, kanske inte alla BizTalk Service-inställningar är tillgängliga.

1. Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Hello nedre navigeringsfönstret och välj **ny**:  
   ![Välj ny hello-knapp][NEWButton]
3. Välj **APPTJÄNSTER** > **BIZTALK-TJÄNST** > **SKAPA ANPASSAD**:  
   ![Välj BizTalk-tjänst och välj Skapa anpassad][NewBizTalkService]
4. Ange inställningar för hello BizTalk-tjänst:
   
    <table border="1">
    <tr>
    <td><strong>BizTalk-tjänstens namn</strong></td>
    <td>Du kan ange ett namn, men var specifik. Några exempel är:<br/><br/>
    <em>mycompany</em>.biztalk.windows.net<br/>
    <em>mycompanymyapplication</em>.biztalk.windows.net<br/>
    <em>myapplication</em>.biztalk.windows.net<br/><br/>”. biztalk.windows.net” är automatiskt tillagda toohello namn som du anger. Detta skapar en URL-adress används tooaccess din BizTalk-tjänst, som <strong>https://<em>myapplication</em>. biztalk.windows.net</strong>.
    </td>
    </tr>
    <tr>
    <td><strong>Utgåva</strong></td>
    <td>Om du är i hello testning/development fasen Välj <strong>Developer</strong>. Om du i hello produktionsfasen, använder hello <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302279">BizTalk-tjänst: utgåvor diagram</a> toodetermine om <strong>Premium</strong>, <strong>Standard</strong>, eller <strong>Basic</strong>är hello rätt val för ditt affärsscenario.
    </td>
    </tr>
    <tr>
    <td><strong>Region</strong></td>
    <td>Välj hello geografiska region toohost BizTalk Service.</td>
    </tr>
    <tr>
    <td><strong>Domänens URL</strong></td>
    <td><strong>Valfritt</strong>. Hello domän-URL är som standard <em>YourBizTalkServiceName</em>. biztalk.windows.net. Du kan även ange en anpassad domän. Om din domän till exempel är <em>contoso</em> kan du ange: <br/><br/>
    <em>MyCompany</em>.contoso.com<br/>
    <em>MyCompanyMyApplication</em>.contoso.com<br/>
    <em>MyApplication</em>.contoso.com<br/>
    <em>YourBizTalkServiceName</em>.contoso.com<br/>
    </td>
    </tr>
    </table>
Välj hello nästa-pilen.
5. Ange hello lagring och databasinställningar:  <table border="1">
    <tr>
    <td><strong>Övervaka/arkivera lagringskontot</strong></td>
    <td>Välj ett befintligt lagringskonto eller skapa ett nytt lagringskonto. <br/><br/>Om du skapar ett nytt lagringskonto anger hello <strong>Lagringskontonamnet</strong>.</td>
    </tr>
    <tr>
    <td><strong>Spårning av databasen</strong></td>
    <td>Om du använder en befintlig Azure SQL Database kan den inte användas av en annan BizTalk-tjänst. Du måste hello inloggningsnamnet och lösenordet som angavs när den Azure SQL-databasservern skapades.<br/><br/><strong>Tips</strong> skapa hello spårning databas och övervakning/arkivering storage-konto i hello samma region som hello BizTalk Service.</td>
    </tr>
    </table>
Välj hello nästa-pilen.
6. Ange inställningar för hello:  <table border="1">
    <tr>
    <td><strong>Namn</strong></td>
    <td>Tillgängligt när <strong>skapa en ny SQL Database-instans</strong> väljs i föregående hello-skärmen.
    <br/><br/>
Ange en SQL-databas namnet toobe som används av BizTalk Service.</td>
    </tr>
    <tr>
    <td><strong>Server</strong></td>
    <td>Tillgängligt när <strong>skapa en ny SQL Database-instans</strong> väljs i föregående hello-skärmen.
    <br/><br/>
Välj en befintlig SQL Database-server eller skapa en ny SQL Database-server.</td>
    </tr>
    <tr>
    <td><strong>Serverns inloggningsnamn</strong></td>
    <td>Ange hello inloggningsnamn för användaren.</td>
    </tr>
    <tr>
    <td><strong>Lösenord för serverinloggning</strong></td>
    <td>Ange hello inloggningslösenordet.</td>
    </tr>
    <tr>
    <td><strong>Region</strong></td>
    <td>Tillgängligt om <strong>Skapa en ny SQL Database-instans</strong> har valts. Välj hello geografiska region toohost SQL-databasen.</td>
    </tr>
    </table>

Välj hello markerat toocomplete hello guiden. hello Förloppsikon visas:  
![Förloppsikonen visar när du är klar][ProgressComplete]

När du är färdig har hello Azure BizTalk-tjänst skapats och redo för dina program. hello standardinställningarna är tillräckliga. Om du vill toochange hello standardinställningarna väljer **BIZTALK-tjänst** i hello vänstra navigeringsfönstret och välj sedan BizTalk Service. Ytterligare inställningar visas i hello [instrumentpanelen, övervaka och skala flikar](biztalk-dashboard-monitor-scale-tabs.md) hello överst.

Beroende på hello tillståndet för hello BizTalk Service finns det vissa åtgärder inte kan slutföras. En lista över de här åtgärderna gå för[BizTalk tjänster tillstånd diagram](biztalk-service-state-chart.md).

## <a name="post-provisioning-steps"></a>Steg efter etableringen
* [Installera hello certifikat på en lokal dator](#InstallCert)
* [Lägg till ett produktionsklart certifikat](#AddCert)
* [Hämta hello åtkomstkontroll namnområde](#ACS)

#### <a name="InstallCert"></a>Installera hello certifikat på en lokal dator
Som en del av etableringen av BizTalk-tjänsten skapas ett självsignerat certifikat som associeras med prenumerationen på BizTalk-tjänsten. Du måste hämta det här certifikatet och installera den på datorer där du distribuerar BizTalk Service program eller skicka meddelanden tooa BizTalk Service slutpunkt.

1. Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Välj **BIZTALK-tjänst** i hello vänstra navigeringsfönstret och välj sedan din BizTalk Service-prenumeration.
3. Välj hello **instrumentpanelen** fliken.
4. Välj **Hämta SSL-certifikat**:  
   ![Ändra SSL-certifikat][QuickGlance]
5. Dubbelklicka på hello certifikatet och kör genom hello guiden tooinstall hello certifikat. Kontrollera att du installerar hello certifikatet under hello **betrodda rotcertifikatutfärdare** lagras.

#### <a name="AddCert"></a>Lägg till ett produktionsklart certifikat
hello självsignerat certifikat som skapas automatiskt när du skapar BizTalk-tjänst är avsedd att användas i utvecklingsmiljöer endast. I produktionsscenarier ersätter du det med ett produktionsklart certifikat.

1. På hello **instrumentpanelen** väljer **uppdatering SSL-certifikat**.
2. Bläddra tooyour privata SSL-certifikat (*CertificateName*.pfx) som innehåller namnet på din BizTalk Service ange hello lösenord och klicka sedan på hello är markerat.

#### <a name="ACS"></a>Hämta hello åtkomstkontroll namnområde
1. Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Välj **BIZTALK-tjänst** i hello vänstra navigeringsfönstret och välj sedan BizTalk Service.
3. Markera i Aktivitetsfältet hello **anslutningsinformationen**:  
   ![Välj Anslutningsinformation][ACSConnectInfo]
4. Kopiera hello åtkomstkontroll värden.

När du distribuerar ett BizTalk-tjänstprojekt från Visual Studio anger du detta Access Control-namnområde. hello åtkomstkontroll namnområdet skapas automatiskt för BizTalk Service.

hello åtkomstkontroll värden kan användas med alla program. När Azure BizTalk-tjänst skapas styr åtkomstkontroll namnområdet hello autentisering med BizTalk Service-distributionen. Om du vill toochange hello prenumerationen eller hantera hello namnområde, väljer **ACTIVE DIRECTORY** i hello vänstra navigeringsfönstret och välj sedan namnområdet. hello Aktivitetsfältet listar alternativen.

Klicka på **hantera** öppnas hello Access Control-hanteringsportalen. Hej BizTalk Service använder i hello Access Control-hanteringsportalen, **tjänsten identiteter**:  
![ACS-tjänstidentiteter i hello Access Control Management Portal][ACSServiceIdentities]

hello tjänstidentiteten för åtkomstkontroll är en uppsättning autentiseringsuppgifter som gör att program eller klienter tooauthenticate direkt med åtkomstkontroll och ta emot en token.

> [!IMPORTANT]
> hello BizTalk Service använder **ägare** för hello standardidentiteten för tjänsten och hello **lösenord** värde. Om du använder hello symmetrisk nyckel värde i stället för hello lösenordsvärdet kan hello följande fel uppstå.<br/><br/>*Kunde inte ansluta toohello Access Control Management Service-kontot med hello angivna autentiseringsuppgifter*
> 
> 

I [Hantera ditt ACS-namnområde](https://msdn.microsoft.com/library/azure/hh674478.aspx) visas några riktlinjer och rekommendationer.

## <a name="requirements-explained"></a>Kraven beskrivs
Dessa krav gäller inte toohello Free Edition.

<table border="1">
<tr bgcolor="FAF9F9">
        <td><strong>Vad du behöver</strong></td>
        <td><strong>Varför du behöver den</strong></td>
</tr>
<tr>
<td>Azure-prenumeration</td>
<td>hello prenumerationen anger vem som kan logga in toohello Azure-portalen. Hej kontoinnehavarens medgivande skapar hello prenumeration på <a HREF="https://account.windowsazure.com/Subscriptions"> Azure-prenumerationer</a>.
<br/><br/>
hello Azure-konto kan ha flera prenumerationer och kan hanteras av vem som helst som är tillåtet. Till exempel Azure-konto-innehavaren skapas en prenumeration med namnet <em>BizTalkServiceSubscription</em> och ger hello BizTalk-administratörer inom företaget (till exempel ContosoBTSAdmins@live.com) åtkomst till toothis prenumeration. I det här scenariot hello BizTalk-administratörer för att logga in toohello Azure-portalen och har fullständig administratör rättigheter tooall hello värdbaserade tjänster i hello-prenumeration, inklusive Azure BizTalk-tjänst. hello BizTalk-administratörer inte hello Azure-konto innehavare och har därför inte åtkomst tooany faktureringsinformation.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=267577">Hantera prenumerationer och Storage-konton i hello Azure-portalen</a> innehåller mer information.
</td>
</tr>
<tr>
<td>Azure SQL Database</td>
<td>Lagrar hello tabeller, vyer och lagrade procedurer som används av hello BizTalk Service, inklusive hello spårningsdata.
<br/><br/>
Du kan använda en befintlig Azure SQL Server, Azure SQL Database eller automatiskt skapa en ny server eller databas när du skapar en BizTalk-tjänst.
<br/><br/>
hello SQL Database skala konfigureras automatiskt. Normalt är hello standard skala tillräcklig för en BizTalk Service. Ändra hello skala påverkar priser. Se <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=234930"> Konton och fakturering i Azure SQL Database</a>
<br/><br/>
<strong>Anteckningar</strong>
<br/>
<ul>
<li> När du skapar en ny Azure SQL Server och databas, aktiveras automatiskt Azure-tjänsterna. hello BizTalk Service kräver Azure Services aktiveras.</li>
<li>Om du skapar en ny Azure SQL-databas på en befintlig Azure SQL Server hello brandväggsregler i hello Server inte ändras. Det är därför möjligt andra Azure-tjänster inte är tillåtna åtkomst toohello Server-databaser.</li>
</ul>
</td>
</tr>
<tr>
<td>Namnområdet Azure Access Control</td>
<td>Autentiserar med Azure BizTalk Services. När du distribuerar ett BizTalk-tjänstprojekt från Visual Studio anger du detta Access Control-namnområde. När du skapar en BizTalk Service skapas automatiskt hello åtkomstkontroll namnområde.</td>
</tr>

<tr>
<td>Azure-lagringskonto</td>
<td>Ger åtkomst till tootables, blobbar och köer som används av din BizTalk Service toosave hello följande:

<ul>
<li>Loggfiler för att övervaka hello BizTalk Service. hello övervakning utdata visas även i hello **övervakning** fliken i hello Azure-portalen.</li>
<li>När du skapar en X12 eller AS2-avtal mellan partner kan aktivera du hello arkivering funktionen toostore meddelandeegenskaper. Informationen sparas i hello Storage-konto.</li>
</ul>
<br/>
När du skapar en BizTalk-tjänst kan du använda ett befintligt lagringskonto eller automatiskt skapa ett nytt lagringskonto.
<br/><br/>
hello standardinställningarna för lagring är tillräckliga för en BizTalk Service.
<br/><br/>
När du skapar ett lagringskonto skapas automatiskt en primär nyckel och en sekundär nyckel. De här nycklarna styra åtkomst tooyour Storage-konto. hello BizTalk Service används automatiskt hello primärnyckel.
<br/><br/>
Se <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285671"> Lagring</a> för mer information.
</td>
</tr>

<tr>
<td>Privata SSL-certifikat</td>
<td>
När en Azure BizTalk-tjänst skapas, skapas också en HTTPS-URL som innehåller namnet på din BizTalk-tjänst. Den här URL: en är automatiskt konfigurerade toouse ett självsignerat certifikat endast för utveckling. För produktion behöver du ett privat SSL-certifikat.
<br/><br/>
<strong>Viktig SSL-certifikatsinformation</strong>

<ul>
<li>hello certifikatets förfallodatum måste vara mindre än 5 år.</li>
<li>Alla privata certifikat kräver ett lösenord. Kom ihåg lösenordet och dela det med dina administratörer.</li>
<li>Självsignerade certifikat används i test-/utvecklingsmiljöer. När du använder självsignerade certifikat, importera hello certifikatarkivet tooyour personliga certifikat och hello certifikatarkivet för betrodda rotcertifikatutfärdare.</li>
</ul>
<br/>När du skickar hello produktion certifikat begäran tooyour certifikatutfärdare kan ge hello följande egenskaper för certifikat:
<br/>

<ul>
<li><strong>Förbättrad nyckelanvändning</strong>: Azure BizTalk Services kräver minst serverautentisering.</li>
<li><strong>Eget namn</strong>: Ange hello fullständigt kvalificerade domännamnet (FQDN) för Azure BizTalk-tjänst-URL. Se <a HREF="#CreateService">Skapa en BizTalk-tjänst</a> i den här artikeln.</li>
</ul>
<br/>
Ett nytt eller annat certifikat kan läggas till efter hello BizTalk Service skapas.
</td>
</tr>
</table>
<!---Loc Comment: Please, check link [Create a BizTalk Service] since it is not redirecting tooany location.--->



## <a name="hybrid-connections"></a>Hybridanslutningar
När du skapar ett Azure BizTalk-tjänst hello **Hybridanslutningar** är tillgänglig:

![Fliken Hybridanslutningar][HybridConnectionTab]

Hybridanslutningar har använt tooconnect en Azure-webbplats eller Azure-mobiltjänsten tooany lokala resursen som använder en statisk TCP-port, till exempel SQL Server, MySQL, http-webb-API: er, Mobile Services och de flesta anpassade webbtjänster.  Hybridanslutningar och hello BizTalk-tjänst för nätverkskort är olika. hello BizTalk-tjänst för nätverkskort är används tooconnect Azure BizTalk-tjänst tooan lokalt rad Affärsappar (LOB).

 Se [Hybridanslutningar](integration-hybrid-connection-overview.md) toolearn mer, inklusive att skapa och hantera Hybridanslutningar.

## <a name="next-steps"></a>Nästa steg
En BizTalk Service är skapat kan du bekanta dig med olika hello [BizTalk-tjänst: instrumentpanelen, övervaka och skala flikar](biztalk-dashboard-monitor-scale-tabs.md). Din BizTalk-tjänst är redo för dina program. för att skapa program, gå toostart[Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197).

## <a name="see-also"></a>Se även
* [BizTalk Services: Diagram över utgåvor](biztalk-editions-feature-chart.md)<br/>
* [BizTalk Services: Statusdiagram](biztalk-service-state-chart.md)<br/>
* [BizTalk Services: Säkerhetskopiering och återställning](biztalk-backup-restore.md)<br/>
* [BizTalk Services: Begränsning](biztalk-throttling-thresholds.md)<br/>
* [BizTalk Services: Utfärdarens namn och nyckel](biztalk-issuer-name-issuer-key.md)<br/>
* [Hur jag börja använda hello Azure BizTalk Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
* [Hybridanslutningar](integration-hybrid-connection-overview.md)

[NewBizTalkService]: ./media/biztalk-provision-services/WABS_NewBizTalkService.png
[NEWButton]: ./media/biztalk-provision-services/WABS_New.png
[ProgressComplete]: ./media/biztalk-provision-services/WABS_ProgressComplete.png
[ACSConnectInfo]: ./media/biztalk-provision-services/WABS_ACSConnectInformation.png
[QuickGlance]: ./media/biztalk-provision-services/WABS_QuickGlance.png
[ACSServiceIdentities]: ./media/biztalk-provision-services/WABS_ACSServiceIdentities.png
[HybridConnectionTab]: ./media/biztalk-provision-services/WABS_HybridConnectionTab.png
