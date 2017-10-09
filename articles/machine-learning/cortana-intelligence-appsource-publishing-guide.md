---
title: aaaCortana Intelligence AppSource publiceringsguide | Microsoft Docs
description: "Här är alla hello stegen toofollow toopublish tooAppSource din Cortana Intelligence-lösning som en Microsoft-Partner."
services: machine-learning
documentationcenter: 
author: AnupamMicrosoft
manager: jhubbard
editor: cgronlun
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2017
ms.author: anupams;v-bruham;garye
ms.openlocfilehash: 66f26016b5eb709c567e9829aa787ca926e13d9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="cortana-intelligence-appsource-publishing-guide"></a>Cortana Intelligence AppSource publiceringsguide

## <a name="overview"></a>Översikt
AppSource hello enda mål för beslutsfattare (BDMs) toodiscover och försök sömlöst lösningar/företagsappar skapat av partners och utvärderas av Microsoft. Titta på [den här videon](https://youtu.be/hpq_Y9LuIB8) toolearn hur AppSource fungerar. 

Som en Microsoft-Partner kan du verkligen utnyttja publicera på AppSource om du har:
- En intelligent lösning-app med hjälp av [Cortana Intelligence Suite](https://azure.microsoft.com/en-us/suites/cortana-intelligence-suite/?cdn=disable).
- Din lösning eller app adresser specifika affärsproblem.
- Du har skapat moduler eller immateriella som dina kunder kan återanvända relativt snabbt förutsägbart.

Ta en titt på [Cortana Intelligence-lösningar](https://appsource.microsoft.com/en-us/marketplace/apps?product=cortana-intelligence&page=1) som redan har publicerats på AppSource. 

Den här artikeln kommer att gå igenom hello steg tooget en partner s Cortana Intelligence lösning publicerade tooAppSource

## <a name="getting-started"></a>Komma igång
1. I hello [Partner Community fördelar guiden](https://www.microsoftpartnerserverandcloud.com/_layouts/download.aspx?SourceUrl=Hosted%20Documents/Partner%20Community%20Benefits%20Guide%20-%20Cloud%20and%20Enterprise.pdf) (PDF), se sidan 9 tooget listats som en Advanced Analytics-partner.
1. Fullständig hello [skicka din app](https://appsource.microsoft.com/en-us/partners/list-an-app) formuläret.

    För hello fråga *”Välj hello produkter som din app är utformat för*”, kontrollera hello **andra** kryssrutan och listan ”Cortana Intelligence” i hello redigera kontrollen.
1. Innan en Cortana Intelligence-app kan hämta publicerats så tooAppSource, måste hämta godkänd av Cortana Intelligence Partner lösning för teknik-teamet. tooget som påbörjades Se dela information om din app genom att fylla i formuläret på en undersökning [Cortana Intelligence lösning utvärdering för AppSource publicering](https://aka.ms/cisappsrceval). Den här platsen är lösenordsskyddat och hello platsen har anvisningar om hur tooget hello lösenord.

## <a name="solution-evaluation-criteria"></a>Kriterier för utvärdering av lösning
Här är hello lista över villkor hello app måste toomeet
1. Appen måste tooaddress specifika Använd case affärsproblem inom ett angivet funktionsområde på ett repeterbara sätt med minimala konfigurationer för fördefinierade värderingsförslag implementable inom en kort tidsperiod.
1. Lösningen ska använda minst en av hello följande komponenter:

    - HDInsight
    - Machine Learning
    - Data Lake Analytics
    - Stream Analytics
    - Cognitive Services
    - Bot Framework
    - Analysis Services
    - Fristående Microsoft R Server
    - R-tjänster i SQL 2016 eller HDInsight Premium
1. Lösningen ska generera minst 1000 $ i månad per kunden med DPOR/CSP.
1. Lösningen använder hello Azure Active Directory federerad enkel inloggning (AAD federerad enkel inloggning) med medgivande som aktiverats för autentisering och åtkomstkontroll för resursen. Du behöver tooshow toohello utvärdering team att din lösning är AAD federerad enkel inloggning aktiverad innan appen kan publicerats så tooAppSource.

     toosee vad det innebär att toobe AAD federerad enkel inloggning aktiverad, söka tooposition 02:35 i [AppSource utvärderingsversionen igenom](https://aka.ms/trialexperienceforwebapps) video. Om din app inte är aktiverad med AAD federerad enkel inloggning ännu, här är några dokumentationen om den
   1. [Ett klick inloggning](https://identity.microsoft.com/Landing?ru=https://identity.microsoft.com/).
   1. [Integrera program med Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#adding-an-application).
     
1. Använd Power BI i din lösning: valfritt men rekommenderas eftersom det bevisas toogenerate högre antal leads.

## <a name="devcenter-account-setup"></a>Konfiguration av DevCenter
Detta är hello processen för att registrera ditt företag toobecome en utgivare med Microsoft. Om du redan är en registrerad utgivare med ett befintligt konto DevCenter dela hello e-post-ID som är associerade med kontot DevCenter. När din ansökan har godkänts för publicering, du får åtkomst toofull dokumentation [moln Portal hantera publisher profiler](https://cloudpartner.azure.com/#documentation/manage-publisher-profile)

Om du inte har någon är nedan hello viktiga steg toosetup DevCenter-konto.
1. Skapa ett Microsoft-konto [här](https://signup.live.com/signup.aspx).
1. Gå toohello Microsoft DevCenter [registreringssidan](http://go.microsoft.com/fwlink/?LinkId=615100) och klicka på ”Logga”.
1. När du uppmanas att göra betalningen använda hello koden som vi angav tooyou. Om du inte har en kod, kontakta [ appsourcecissupport@microsoft.com ](mailto:appsourcecissupport@microsoft.com?subject=Request%20for%20promotion-code%20for%20DevCenter%20account%20setup).
1. Välj hello [land/region](https://docs.microsoft.com/windows/uwp/publish/account-types-locations-and-fees) där du bor, eller där din verksamhet finns. **Du kommer inte att kunna toochange detta senare.**
1. Välj din [developer kontotyp](https://docs.microsoft.com/windows/uwp/publish/account-types-locations-and-fees) för AppSource Välj **företagets**. För ett företag vara säker på att tooreview dessa [riktlinjer](https://docs.microsoft.com/windows/uwp/publish/opening-a-developer-account).
1. Ange hello kontaktinformation som du vill använda toouse för ditt utvecklarkonto.
Detaljerade steg-för-steg-instruktioner för hur toosetup DevCenter konto, se hello instruktioner [här](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-accounts-creation-registration).

## <a name="provide-info-for-microsoft-sellers"></a>Ange information för Microsoft säljare
En av hello viktiga värderingsförslag av AppSource för partner är toobe kan toocollaborate med Microsoft Sellers placera partnerappar framför potentiella kunder.

Fylls [Partner lösning information för Microsoft Sellers](https://aka.ms/aapartnerappinfo) och skicka det för[appsourcecissupport@microsoft.com](mailto:appsourcecissupport@microsoft.com?subject=Request%20publisher%20account%20creation%20for%20%3cPartner%20Name%3e%20and%20whitelist%20owner/contributer%20AAD/MSA%20email%20IDs). Detta är nödvändiga steg för en Cortana Intelligence app tooget som godkänts för publicering.

## <a name="build-a-compelling-customer-walkthrough-on-appsource"></a>Skapa en tvingande kunden genomgång på AppSource
Först, ta en titt på [Neal Analytics inventering optimering](https://appsource.microsoft.com/en-us/product/web-apps/neal_analytics.8066ad01-1e61-40cd-bd33-9b86c65fa73a?tab=Overview&tag=CISHome) på AppSource. Varje app posten i AppSource har rubrik, Sammanfattning (maximalt 100 tecken), beskrivning (1300 tecken max), bilder, videor (valfritt), PDF-dokument förutom posten pekar för en utvärderingsversionen. Partners bör utnyttja dessa toobuild en tvingande kundupplevelse.

Partners bör tänka på hello innehåll som de lägger på AppSource som en end tooend försäljning orchestration. Kunder läsa hello rubrik och beskrivning toounderstand hello prisvärt alternativ och sedan gå igenom bilder & videor toounderstand vilka hello-lösningen är om. Fallstudier kunderna potentiella se hur befintliga kunder får värdet. 

Alla detta bör se hello kunden du intresserad av och förskjutning tooknow mer. Tänk på dessa som breddsteg däck baserat presentation en bra teknisk säljare skulle igenom hello nya kunder. hello förslag har formatet beskrivning toobreak hello text i underordnade avsnitt baserat på värderingsförslag, var och en med markerade med en underrubrik. 

I korthet vanligtvis besökare över hello ”erbjuder sammanfattning” fält och underordnade rubriker tooget gist av vilka hello app-adresser och varför bör de hello app i bara en snabb överblick över. Därför är det viktigt tooget hello användarens uppmärksamhet ge dem en orsak tooread tooget hello detaljerad information.

Se vad dessa tillverkare har gjort.
- [Neal Analytics inventering optimering](https://appsource.microsoft.com/en-us/product/web-apps/neal_analytics.8066ad01-1e61-40cd-bd33-9b86c65fa73a?tab=Overview)
- [Plexure Retail-anpassning](https://appsource.microsoft.com/en-us/product/web-apps/plexure.c82dc2fc-817b-487e-ae83-1658c1bc8ff2?tab=Overview)
- [AvePoint men tjänster](https://appsource.microsoft.com/en-us/product/web-apps/avepoint.7738ac97-fd40-4ed3-aaab-327c3e0fe0b3?tab=Overview)

hello är slutliga försäljning tooshow en demonstration av hello app/lösningen visar hur hello värdeförslag levereras. Det är exakt en utvärderingsversion kundupplevelse på AppSource är avsett för. Gör en bra demonstration hello följande:
- Lista över ut hello personer inom hello kunden företag som vill interagera med hello lösning och nytt sammanfatta hello molntjänsters hello app i korthet
- Berätta för en kontext som hello artikeln och anger om en exempel-kund behandlar specifika problem
- Förklara hur hello situationen kan vanligtvis förvärvats och påverka hello kunden (innan) jämfört med vad som skulle hända om hello lösningen används. Detta kan visas med PowerBI instrumentpaneler osv.
- Sammanfatta hur hello lösningen gör det ske med hjälp av en specifik maskininlärningsalgoritmer osv.

hello innehåll som läggs till i AppSource ska vara av hög kvalitet och stitched in tillräckligt med tooenable hello följande:
- En partner tekniska försäljning avdelningen bör använda den för sina försäljning orchestration. Din försäljning grupper med hjälp av det är ett bra tecken som du förväntar dig MS försäljning avdelningen som också kan toodo hello samma. Detta aktiverar kunden kontaktpunkt toobe kan tooconsistently relay hello samma artikel tootheir team förarbiträden och högre ups tooget budget och godkännanden innan kan du göra ett inköp erbjudande.
- En kund som webbplatsen hello ekologiskt kan gå igenom hello innehåll sig själva och anser glad över toorespond tillbaka toopartner kommunikation toomove vidare med nästa steg.

Som är orsaken partner bör tänka på hello innehåll de spärra AppSource som en end tooend försäljning orchestration. Baserat på hello artikeln rad och funktioner toobe visas i utvärderingsversionen, kan typ av erbjudandet fastställas.

## <a name="publish-your-app-on-hello-publishing-portal"></a>Publicera appen i hello publishing portal
När vi har utvärderat hello ovanstående steg för programmet, du får åtkomst toohello publishing portal och kan se [hur toopublish en Cortana Intelligence erbjuder via molnet partnerportalen](https://cloudpartner.azure.com/#documentation/cloud-partner-portal-publish-cortana-intelligence-app) detaljerad information om nästa steg.

Om du behöver information om något av fälten hello e- < appsourcecissupport@microsoft.com >.
## <a name="my-app-is-published-on-appsource---now-what"></a>Min app har publicerats på AppSource - vad?
Det första publicerade Gratulerar komma din app.
hello andelen returnerar dig från att publicera en app på AppSource kraftigt beror på hur du påverka hello målgrupp. Se [hackningsförsök Cortana Intelligence appen på AppSource tillväxt](http://aka.ms/aagrowthhackguide) för mer information om hur du kan maximera hello returnerar.

Om du har några frågor eller förslag, se nå ut toous på < appsourcecissupport@microsoft.com >.

