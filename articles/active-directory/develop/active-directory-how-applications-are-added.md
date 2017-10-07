---
title: "aaaHow program läggs tooAzure Active Directory."
description: "Den här artikeln beskriver hur program läggs tooan instans av Azure Active Directory."
services: active-directory
documentationcenter: 
author: shoatman
manager: kbrint
editor: 
ms.assetid: 3321d130-f2a8-4e38-b35e-0959693f3576
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/09/2016
ms.author: shoatman
ms.custom: aaddev
ms.openlocfilehash: 3ca710c58a403b52e8b728202ad9010f4873bcea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-and-why-applications-are-added-tooazure-ad"></a>Hur och varför program läggs tooAzure AD
En hello puzzling saker först när du visar en lista över program i din instans av Azure Active Directory för att förstå var hello program kommer ifrån och varför de finns.  Den här artikeln ger en översiktlig överblick över hur program representeras i hello directory och förse dig med kontext som hjälper dig att förstå hur ett program kommer toobe i katalogen.

## <a name="what-services-does-azure-ad-provide-tooapplications"></a>Vilka tjänster som Azure AD tillhandahåller tooapplications?
Program läggs tooAzure AD tooleverage en eller flera av hello tjänster.  Tjänsterna omfattar:

* App-autentisering och auktorisering
* Användarautentisering och auktorisering
* Enkel inloggning (SSO) använder federation eller lösenord
* Användaretablering & synkronisering
* Rollbaserad åtkomstkontroll; Använd hello directory toodefine roller tooperform programroller baserat auktoriseringskontroller i en app.
* oAuth-auktoriseringstjänster (används av Office 365 och andra Microsoft-appar tooauthorize åtkomst tooAPIs/resurser).
* Programpublicering & proxy; Publicera en app från ett privat nätverk toohello internet

## <a name="how-are-applications-represented-in-hello-directory"></a>Hur representeras program i hello directory?
Program som representeras i hello Azure AD-objekt 2: programobjekt och ett service principal-objekt.  En program-objekt registrerats i ett ”hem” / ”ägare” eller ”publicera” katalog och en eller flera service principal-objekt som representerar hello programmet i alla kataloger som det fungerar.  

hello programobjektet beskriver hello app tooAzure AD (hello flera innehavare service) och får innehålla något av följande hello: (*Observera*: Detta är inte en fullständig förteckning.)

* Namn, logotyp och utgivare
* Hemligheter (symmetrisk och/eller asymmetriska nycklar används tooauthenticate hello app)
* API-beroenden (oAuth)
* API: er eller resurser/Omfattningarna publicerade (oAuth)
* Appen roller (RBAC)
* Metadata för SSO och konfiguration (SSO)
* Användaretablering metadata och konfiguration
* Metadata för proxy och konfiguration

hello tjänstens huvudnamn är en post för varje katalog där programmet hello fungerar inklusive sin hemkatalog hello program.  hello tjänstens huvudnamn:

* Refererar tillbaka tooan programobjektet via hello app id-egenskap
* Innehåller lokala användare och grupp app-rolltilldelningar
* Innehåller lokala användare och admin behörigheter beviljas toohello app
  * Till exempel: behörigheten för hello app tooaccess en viss användare e-post
* Poster lokala principer, inklusive principen för villkorlig åtkomst
* Poster lokala alternativa lokala inställningar för en app
  * Regler för omvandling av anspråk
  * Attributmappning (användaretablering)
  * Klient specifik app roller (om hello app stöder anpassade roller)
  * Namn/logotyp

### <a name="a-diagram-of-application-objects-and-service-principals-across-directories"></a>Ett diagram över programobjekt och tjänstens huvudnamn i kataloger
![Ett diagram som illustrerar hur programmet objekt och tjänsten säkerhetsobjekt befintliga med Azure AD-instanser.][apps_service_principals_directory]

Som du kan se hello diagrammet ovan.  Microsoft har två kataloger internt (på hello vänster) används toopublish program.

* En för Microsoft Apps (Microsoft services directory)
* En för appar förintegrerade med 3 part (App-galleriet katalog)

Utgivare/programleverantörer som integreras med Azure AD är obligatoriska toohave en publishing katalog.  (Vissa SAAS Directory).

Program som du lägger till dig själv innehåller:

* Appar som du har utvecklat (integrerat med AAD)
* Appar som du har anslutit för enkel inloggning
* Appar som du har publicerat med hello Azure AD-programproxy.

### <a name="a-couple-of-notes-and-exceptions"></a>Några anteckningar och undantag
* Inte alla tjänstens huvudnamn peka tillbaka tooapplication objekt.  Huh? När Azure AD har ursprungligen skapats tillgängliga hello services tooapplications var mycket mer begränsade och hello tjänstens huvudnamn var tillräckligt för att upprätta en app-identitet.  hello ursprungliga tjänstens huvudnamn var närmare i formen toohello Windows Server Active Directory-tjänstkontot.  Därför är det fortfarande möjligt toocreate hello tjänstens huvudnamn med hjälp av Azure AD PowerShell utan att först skapa ett programobjekt.  hello Graph API kräver ett app-objekt innan du skapar en tjänst huvudnamn.
* Inte alla hello uppgifter som beskrivs ovan är för närvarande exponerad programmässigt.  hello följande är bara tillgängliga i hello UI:
  * Regler för omvandling av anspråk
  * Attributmappning (användaretablering)
* Mer detaljerad information om hello tjänstens huvudnamn och programobjekt finns toohello Azure AD Graph REST API-referensdokumentation.  *Tipset*: hello Azure AD Graph API-dokumentationen är hello närmaste sak tooa Schemareferens för Azure AD som är tillgänglig.  
  * [Programmet](https://msdn.microsoft.com/library/azure/dn151677.aspx)
  * [Tjänstens huvudnamn](https://msdn.microsoft.com/library/azure/dn194452.aspx)

## <a name="how-are-apps-added-toomy-azure-ad-instance"></a>Hur läggs appar toomy Azure AD-instans?
Det finns många sätt en app kan läggas till tooAzure AD:

* Lägg till en app från hello [Azure Active Directory App-galleriet](https://azure.microsoft.com/updates/azure-active-directory-over-1000-apps/)
* Logga in/till en 3 part App integrerat med Azure Active Directory (till exempel: [Smartsheet](https://app.smartsheet.com/b/home) eller [DocuSign](https://www.docusign.net/member/MemberLogin.aspx))
  * Vid inloggning upp eller användare är och toogive behörighet toohello app tooaccess deras profil och andra behörigheter.  hello första person toogive medgivande orsaker ett huvudnamn för tjänsten som representerar hello programkatalogen toobe tillagda toohello.
* Logga in/till Microsofts onlinetjänster som [Office 365](http://products.office.com/)
  * När du prenumererar tooOffice 365 eller starta en utvärderingsversion eller mer tjänstens huvudnamn skapas i hello directory som representerar hello olika tjänster som används toodeliver hello funktioner som är associerade med Office 365.
  * Vissa Office 365-tjänster som SharePoint skapa tjänstens huvudnamn på ett pågående bas tooallow säker kommunikation mellan komponenter, inklusive arbetsflöden.
* Lägg till en app som du utvecklar i hello finns i Azure-hanteringsportalen: https://msdn.microsoft.com/library/azure/dn132599.aspx
* Lägg till en app som du utvecklar med Visual Studio finns:
  * [ASP.Net-autentiseringsmetoder](http://www.asp.net/visual-studio/overview/2013/creating-web-projects-in-visual-studio#orgauthoptions)
  * [Anslutna tjänster](http://blogs.msdn.com/b/visualstudio/archive/2014/11/19/connecting-to-cloud-services.aspx)
* Lägg till en app toouse toouse hello [Azure AD Application Proxy](https://msdn.microsoft.com/library/azure/dn768219.aspx)
* Anslut en app för enkel inloggning med SAML eller lösenord för inloggning
* Många andra inklusive olika developer upplevelser i Azure och/i API explorer upplevelser i developer Center

## <a name="who-has-permission-tooadd-applications-toomy-azure-ad-instance"></a>Vem har behörighet tooadd program toomy Azure AD-instans?
Endast globala administratörer kan:

* Lägg till appar från galleriet hello Azure AD-app (förintegrerade 3 Partsappar)
* Publicera en app med hello Azure AD Application Proxy

Alla användare i din katalog har rättigheter tooadd program som de utvecklar och gottfinnande över vilka program som de resursen/ge åtkomst tootheir organisationens data.  *Kom ihåg användarinloggning upp/i tooan app och beviljar behörighet kan resultera i en tjänst som håller på att skapas.*

Det här kan ursprungligen ljud om, men behålla hello följande i åtanke:

* Appar har kan tooleverage Windows Server Active Directory för autentisering av användare för många år utan hello programmet toobe registrerade/registrerats i hello directory.  Nu hello organisation har förbättrat synlighet tooexactly hur många appar använder hello katalog och what för.
* Inget behov av administratören drivs appen publicering/registreringsprocessen.  Det var sannolikt att en administratör tooadd en app som har en förlitande part för utvecklare med Active Directory Federation Services.  Utvecklare kan nu självbetjäning.
* Användare som loggar in/upp tooapps med deras organisation konton för affärsändamål är bra.  Om de senare lämnar organisationen hello förlorar de tootheir åtkomstkonto i hello-program som de använder.
* Med en post på vilka data delades som programmet är bra.  Data är transportera än någonsin och ha en tydlig post på som delas vilka data med vilka program som kan användas.
* Appar som använder Azure AD för oAuth bestämma exakt vilka behörigheter som användare kan toogrant tooapplications och vilka behörigheter måste en administratör tooagree till.  Det ska gå utan säger att endast administratörer kan godkänna toolarger scope och större behörighet.
* Användare lägger till, så att appar tooaccess sina data är granskade händelser så att du kan visa hello granskningsrapporter inom hello Azure Management portal toodetermine hur en app har lagts till toohello directory.

**Obs:** *Microsoft själva har fungerat med hello standardkonfigurationen för många månader nu.*

Med alla rapporterade att det är möjligt tooprevent användare i din katalog från att lägga till program och utöva gottfinnande över vilken information som delas med program genom att ändra katalogkonfiguration i hello Azure Management portal.  hello kan följande konfiguration användas inom hello Azure Management portal på fliken för din katalog ”konfigurera”.

![En skärmbild av hello Användargränssnittet för att konfigurera integrerad app-inställningar][app_settings]

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Nästa steg
Läs mer om hur tooadd program tooAzure AD och hur tooconfigure tjänster för appar.

* Utvecklare: [Lär dig hur toointegrate ett program med AAD](https://msdn.microsoft.com/library/azure/dn151122.aspx)
* Utvecklare: [granska exempelkod för appar som är integrerad med Azure Active Directory på GitHub](https://github.com/AzureADSamples)
* Utvecklare och IT-proffs: [granska hello REST API-dokumentation för hello Azure Active Directory Graph API](https://msdn.microsoft.com/library/azure/hh974478.aspx)
* IT-proffs: [Lär dig hur toouse Azure Active Directory redan integrerade program från hello App-galleriet](https://msdn.microsoft.com/library/azure/dn308590.aspx)
* IT-proffs: [hittar självstudier för att konfigurera specifika förintegrerade appar](https://msdn.microsoft.com/library/azure/dn893637.aspx)
* IT-proffs: [Lär dig hur toopublish en app med hjälp av hello Azure Active Directory Application Proxy](https://msdn.microsoft.com/library/azure/dn768219.aspx)

## <a name="see-also"></a>Se även
* [Artikelindex för programhantering i Azure Active Directory](../active-directory-apps-index.md)

<!--Image references-->
[apps_service_principals_directory]:../media/active-directory-how-applications-are-added/HowAppsAreAddedToAAD.jpg
[app_settings]:../media/active-directory-how-applications-are-added/IntegratedAppSettings.jpg
