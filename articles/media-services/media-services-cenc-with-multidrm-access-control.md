---
title: "CENC med Multi-DRM och Access Control: en referens Design och Implementeringslösning på Azure och Azure Media Services | Microsoft Docs"
description: "Läs mer om hur toolicensing hello Microsoft® Smooth Streaming klienten portera Kit."
services: media-services
documentationcenter: 
author: willzhan
manager: cfowler
editor: 
ms.assetid: 7814739b-cea9-4b9b-8370-538702e5c615
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: willzhan;kilroyh;yanmf;juliako
ms.openlocfilehash: 033fb618650c4fbe9069d467159a8734da759bba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="cenc-with-multi-drm-and-access-control-a-reference-design-and-implementation-on-azure-and-azure-media-services"></a>CENC med Multi-DRM och åtkomstkontroll: En referensdesign och implementering i Azure och Azure Media Services
 
## <a name="introduction"></a>Introduktion
Det är känt att den är en komplicerad uppgift toodesign och skapa ett DRM-undersystem för en OTT eller online strömning lösning. Och det är en vanlig rutin för ansvariga för online video providers toooutsource denna del toospecialized DRM-leverantörer. hello syftet med det här dokumentet är toopresent en referens för design och implementering av DRM-undersystemet för slutpunkt till slutpunkt i OTT eller strömmande onlinelösning.

hello riktade läsare av det här dokumentet är tekniker som arbetar i DRM-undersystemet OTT strömning/flera-screen lösningar online eller alla läsare som är intresserade av DRM-undersystemet. hello antas att läsaren känner till minst en av hello DRM tekniker på hello marknad, till exempel PlayReady, Widevine, FairPlay eller Adobe åtkomst.

Av DRM inkludera vi också CENC (Common Encryption) med multi-DRM. En större trend i online-strömning och OTT branschen är toouse CENC med flera-native-DRM på olika klientplattformar som är en övergång från hello tidigare trend med en enda DRM och dess klient-SDK för olika klientplattformar. När du använder CENC med flera native DRM både PlayReady och Widevine krypteras enligt hello [Common Encryption (ISO/IEC 23001 7 CENC)](http://www.iso.org/iso/home/store/catalogue_ics/catalogue_detail_ics.htm?csnumber=65271/) specifikation.

hello fördelarna med CENC med multi-DRM är följande:

1. Minskar kryptering kostnad eftersom en enda kryptering bearbetning används inriktning på olika plattformar med dess ursprungliga DRMs;
2. Minskar hello kostnaden för att hantera krypterade tillgångar eftersom bara en enda kopia av krypterade tillgångar krävs;
3. Eliminerar DRM klientlicensiering kostnaden eftersom hello-DRM-klienten är vanligtvis ledigt på dess ursprungliga plattform.

Microsoft har en aktiv förslagsställare DASH och CENC tillsammans med vissa viktiga branschen spelare. Microsoft Azure Media Services tillhandahåller i stöd för DASH och CENC. Senaste meddelanden finns Mingfeis bloggar: [meddelar Google Widevines licensleveranstjänster i Azure Media Services](https://azure.microsoft.com/blog/announcing-general-availability-of-google-widevine-license-services/), och [Azure Media Services lägger till Google Widevine-paketering för att leverera Multi-DRM dataströmmen](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/).  

### <a name="overview-of-this-article"></a>Översikt över den här artikeln
hello syftet med den här artikeln innehåller hello följande:

1. Innehåller en referens design av DRM-undersystemet med CENC med multi-DRM;
2. Innehåller en referensimplementering för Microsoft Azure-/ Azure Media Services-plattform.
3. Beskriver vissa design och implementeringslösning avsnitt.

I hello artikeln täcker ”multi-DRM” hello följande:

1. Microsoft PlayReady
2. Google Widevines
3. Apple FairPlay 

hello följande tabell sammanfattas hello interna plattform/egen app och webbläsare som stöds av varje DRM.

| **Klientplattform** | **Intern DRM-Support** | **Browser-appen** | **Strömningsformat** |
| --- | --- | --- | --- |
| **TV-apparater, operatorn digitalboxar, OTT digitalboxar för smartkort** |PlayReady främst och/eller Widevine och/eller andra |Linux, Opera, WebKit, andra |Olika format |
| **Windows 10-enheter (Windows-dator, Windows-surfplattor, Windows Phone, Xbox)** |PlayReady |MS Edge/IE11/EME<br/><br/><br/>UWP |STRECK (för HLS, PlayReady stöds inte)<br/><br/>DASH, Smooth Streaming (HLS för, PlayReady stöds inte) |
| **Android-enheter (telefon, surfplatta, TV)** |Widevine |Chrome/EME |DASH, HLS |
| **iOS (iPhone, iPad), OS X-klienter och Apple TV** |FairPlay |Safari 8 +/ EME |HLS |


Med tanke på hello aktuell status för distributionen för varje DRM, en tjänst vill förmodligen tooimplement 2 eller 3 DRMs toomake att du lösa alla hello typer av slutpunkter i hello bästa sätt.

Det finns en kompromiss mellan hello komplexitet hello service logik och hello komplexitet för hello klienten sida tooreach en viss användare har inloggningen på hello olika klienter.

toomake valet Kom något dessa uppgifter:

* PlayReady är internt implementerad i alla Windows-enhet på en Android-enheter och tillgänglig via programvara SDK: er på nästan alla plattformar
* Widevine implementeras internt i alla Android-enhet, Chrome och vissa andra enheter
* FairPlay finns bara på iOS och Mac OS x-klienter eller via iTunes.

Detta är en typisk multi-DRM:

* Alternativ 1: PlayReady och Widevine
* Alternativ 2: PlayReady, Widevine och FairPlay

## <a name="a-reference-design"></a>En referens-design
I det här avsnittet presenterar vi en referens-design som är oberoende tootechnologies används tooimplement den.

DRM-undersystemet kan innehålla hello följande komponenter:

1. Hantering av nycklar
2. DRM-paketering
3. DRM-licensleverans
4. Rätt kontroll
5. Autentisering/auktorisering
6. Player
7. Ursprung/CDN

hello illustrerar följande diagram hello hög nivå interaktion mellan hello komponenterna i ett DRM-undersystem.

![DRM-undersystemet med CENC](./media/media-services-cenc-with-multidrm-access-control/media-services-generic-drm-subsystem-with-cenc.png)

Det finns tre grundläggande ”lager” i hello design:

1. Säkerhetskopiera office lager (i svart) som inte exponeras externt.
2. ”DMZ” lager (blå) som innehåller alla hello slutpunkter mot offentlig.
3. Offentliga Internet lager (lätta blå) som innehåller CDN och spelare med trafik över offentligt Internet.

Det bör finnas ett innehållshantering verktyg för att styra DRM-skydd, oavsett är statisk eller dynamisk kryptering. hello indata för DRM kryptering bör innehålla:

1. MBR videoinnehåll.
2. Innehållsnyckeln;
3. Licens förvärv URL: er.

Under uppspelningen tiden är hello hög nivå flödet:

1. Användaren autentiseras;
2. Autentiseringstoken har skapats för hello användaren.
3. DRM-skyddat innehåll (manifestet) är hämtade tooplayer;
4. Player skickar förvärv begäran toolicense licensservrar tillsammans med nyckel-ID och auktorisering token.

Innan du flyttar toohello nästa avsnitt, några ord om hello design med nyckelhantering.

| **Tillgång till ContentKey –** | **Scenario** |
| --- | --- |
| 1-till-1 |hello enklaste fallet. Det ger hello finest kontroll. Men det vanligtvis resulterar i hello högsta licens leverans kostnad. Begäran måste anges för varje skyddad tillgång på minst en licens. |
| 1-till-många |Du kan använda hello samma innehåll nyckel för flera tillgångar. Till exempel för alla hello tillgångar i en logisk grupp, till exempel en kategori eller en delmängd av kategori (eller film Gen) använda en enda innehållsnyckel. |
| Många-till-1 |Nycklar för multiinnehåll krävs för varje tillgång. <br/><br/>Du måste till exempel två separata innehåll nycklar, var och en med sin egen ContentKeyType om du behöver skydd för tooapply för dynamiska CENC med multi-DRM för MPEG DASH och dynamisk AES-128-kryptering för HLS. (För hello innehållsnyckeln används för dynamiska CENC skydd, ContentKeyType.CommonEncryption ska användas, medan för hello innehåll nyckel som används för dynamiska AES-128-kryptering, ContentKeyType.EnvelopeEncryption ska användas.)<br/><br/>Ett annat exempel är CENC DASH att skydda innehåll, i teorin, en innehållsnyckel kan använda tooprotect video-ström och ett annat innehåll viktiga tooprotect ljudström. |
| Många-för-många |Kombinationen av hello ovan två scenarier: en uppsättning av innehåll som nycklar används för varje hello flera resurser i hello samma ”grupp”. |

En annan viktig faktor tooconsider är hello användningen av beständiga och icke-beständig licenser.

Varför är detta viktigt?

De har direkt inverkan toolicense leverans om du använder offentliga moln för licensleverans av. Nu ska vi titta efter två olika design fall tooillustrate hello:

1. Månatlig prenumeration: använda beständiga licens och 1-till-många innehåll nyckeln till tillgången mappning. T.ex. för alla hello barnen filmer använder vi en enda innehållsnyckel för kryptering. I det här fallet:

    Totalt antal # licenser som krävs för alla barnen filmer enhet = 1
2. Månatlig prenumeration: använda icke-beständig licens och 1-till-1-mappning mellan innehållsnyckeln och tillgångshantering. I det här fallet:

    Totalt antal # licenser som krävs för alla barnen filmer enhet = [# filmer bevakade] x [# sessioner]

Som du kan enkelt se, begära hello två olika konstruktionerna resultera i mycket olika licens mönster därför licensleverans om licensleveranstjänst tillhandahålls av ett offentligt moln, till exempel Azure Media Services.

## <a name="mapping-design-tootechnology-for-implementation"></a>Mappning design tootechnology för implementering
Därefter mappa vi vårt allmänna design tootechnologies på Microsoft Azure-/ Azure Media Services-plattformen genom att ange vilken teknik toouse för varje byggblock.

hello visar följande tabell hello mappning:

| **Byggblock** | **Teknik** |
| --- | --- |
| **Player** |[Azure Media Player](https://azure.microsoft.com/services/media-services/media-player/) |
| **Identitetsprovider (IDP)** |Azure Active Directory |
| **Secure Token Service (STS)** |Azure Active Directory |
| **Arbetsflöde för DRM-skydd** |Azure Media Services dynamisk skydd |
| **DRM-licensleverans** |1. Azure Media Services licensleverans (PlayReady Widevine, FairPlay), eller <br/>2. Axinom licensserver <br/>3. Anpassade PlayReady licensserver |
| **Ursprung** |Azure Media Services strömmande slutpunkt |
| **Hantering av nycklar** |Behövs inte för referensimplementering |
| **Innehållshantering** |Ett C#-konsolprogram |

Med andra ord kommer både identitet Provider (IDP) och Secure säkerhetstokentjänst (STS) att Azure AD. Vi använder för spelare, [Azure Media Player API](http://amp.azure.net/libs/amp/latest/docs/). Stöder både Azure Media Services och Azure Media Player DASH och CENC med multi-DRM.

följande diagram visar hello hello övergripande struktur och flöde med hello ovan teknik mappning.

![CENC på AMS](./media/media-services-cenc-with-multidrm-access-control/media-services-cenc-subsystem-on-AMS-platform.png)

I ordning tooset in dynamisk CENC kryptering använder hello innehållshantering verktyget hello följande indata:

1. Öppna innehåll.
2. Innehållsnyckeln från nyckeln generation och hantering
3. Licens förvärv webbadresserna.
4. En lista med information från Azure AD.

hello utdata hello innehållshantering verktyget blir:

1. ContentKeyAuthorizationPolicy som innehåller hello-specifikationen på hur licensleverans verifierar en JWT-token och DRM-licens specifikationerna.
2. AssetDeliveryPolicy som innehåller specifikationer direktuppspelning format, DRM-skydd och licens förvärv URL: er.

Under körning är hello flödet som nedan:

1. Vid autentisering av användare genereras en JWT-token;
2. En av hello anspråk som ingår i hello JWT-token är ”grupper” anspråk som innehåller hello objekt-ID för ”EntitledUserGroup”. Denna begäran används för att skicka ”Kontrollera rätt”.
3. Player hämtningar klienten manifestet för en CENC skyddat innehåll och ”ser” hello följande:

   1. nyckel-ID
   2. hello innehållet är skyddade, CENC
   3. Licens förvärv URL: er.
4. Player begär licens förvärv baserat på hello webbläsare/DRM stöds. I hello licens förvärv begäran, nyckel-ID och hello JWT skickas också token. Licensleveranstjänst kontrollerar hello JWT-token och hello anspråk finns innan utfärdande hello behövs licens.

## <a name="implementation"></a>Implementering
### <a name="implementation-procedures"></a>Procedurer för implementering
hello implementering innehåller hello följande steg:

1. Förbereda test tillgångarna: koda/package en test video toomulti bithastighet fragmenterad MP4 i Azure Media Services. Den här tillgången är inte DRM-skyddat. DRM skydd kommer att göras av dynamisk protection senare.
2. Skapa nyckel-ID och innehåll nyckel (eventuellt från viktiga startvärde). Exemplet nyckelhanteringssystem inte behövs eftersom vi arbetar med en uppsättning med ID: T och nyckeln till ett par test tillgångar.
3. Använd AMS API tooconfigure multi-DRM-licensleveranstjänster för hello test tillgång. Om du använder anpassade licensservrar av ditt företag eller företagets leverantörer i stället för licens-tjänster i Azure Media Services kan du hoppa över detta steg och ange licens förvärv URL: er i hello steg för att konfigurera licens. AMS API är nödvändiga toospecify vissa detaljerad konfigurationer, till exempel auktorisering principbegränsning licens svar mallar för olika tjänster för DRM-licens, osv. För tillfället hello Azure-portalen som inte ännu ange hello behövs Användargränssnittet för den här konfigurationen. Du kan hitta API-nivå info och exempelkod i Julia Kornich dokument: [med PlayReady och/eller Widevine Dynamic Common Encryption](media-services-protect-with-drm.md).
4. Använd AMS API tooconfigure tillgångsleveransprincip för hello test tillgång. Du kan hitta API-nivå info och exempelkod i Julia Kornich dokument: [med PlayReady och/eller Widevine Dynamic Common Encryption](media-services-protect-with-drm.md).
5. Skapa och konfigurera en Azure Active Directory-klient i Azure
6. Skapa några användarkonton och grupper i Azure Active Directory-klient: du bör skapa minst ”EntitledUser” och lägga till en användargrupp för toothis. Användare i den här gruppen ska skicka rätt kontroll i licenser och användare inte i den här gruppen misslyckas toopass autentiseringskontroll och kommer inte att kunna tooacquire någon licens. Vara medlem i gruppen ”EntitledUser” är ett obligatoriskt ”grupper” anspråk i hello JWT-token som utfärdas av Azure AD. Detta anspråk krav anges när du konfigurerar multi-DRM-licens leverans services steg.
7. Skapa en ASP.NET MVC-app som ska vara värd för din videospelare. ASP.NET-app skyddas med användarautentisering mot hello Azure Active Directory-klient. Rätt anspråk ska inkluderas i hello åtkomsttoken som erhålls efter autentisering av användare. OpenID Connect API rekommenderas för det här steget. Du behöver tooinstall hello följande NuGet-paket:
   * Install-Package Microsoft.Azure.ActiveDirectory.GraphClient
   * Install-Package Microsoft.Owin.Security.OpenIdConnect
   * Install-Package Microsoft.Owin.Security.Cookies
   * Install-Package Microsoft.Owin.Host.SystemWeb
   * Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
8. Skapa en Windows Media player [Azure Media Player API](http://amp.azure.net/libs/amp/latest/docs/). [Azure Media Player ProtectionInfo API](http://amp.azure.net/libs/amp/latest/docs/) kan du toospecify vilka DRM-teknik toouse på olika DRM-plattformen.
9. Testningsmatris:

| **DRM** | **Webbläsare** | **Resultat för rätt användare** | **Resultat för icke rätt användare** |
| --- | --- | --- | --- |
| **PlayReady** |MS kant eller IE11 på Windows 10 |Lyckas |Misslyckas |
| **Widevine** |Chrome på Windows 10 |Lyckas |Misslyckas |
| **FairPlay** |TBD | | |

George Trifonov för Azure Media Services-teamet har skrivit en blogg som ger detaljerade anvisningar för att konfigurera Azure Active Directory för en ASP.NET MVC player app: [integrera Azure Media Services OWIN MVC baserat app med Azure Active Directory och begränsa innehåll viktiga leverans baserat på JWT anspråk](http://gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

George har skapat en blogg på [JWT-token autentisering i Azure Media Services och dynamisk kryptering](http://gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/). Och här är operativsystemets [på Azure AD-integrering med Azure Media Services viktiga leverans](https://github.com/AzureMediaServicesSamples/Key-delivery-with-AAD-integration/).

Information om Azure Active Directory:

* Du kan hitta information för utvecklare i [Utvecklarhandbok för Azure Active Directory](../active-directory/active-directory-developers-guide.md).
* Du kan hitta information för administratörer i [administrera din Azure AD-katalog](../active-directory/active-directory-administer.md).

### <a name="some-gotchas-in-implementation"></a>Vissa saker i implementering
Det finns vissa ”saker” i hello implementering. Förhoppningsvis hjälper hello följande lista över ”saker” dig felsöka om du stöter på problem.

1. **Utfärdaren** URL bör avslutas med **”/”**.  

    **Målgruppen** ska vara hello player programmet klient-ID och bör du också lägga till **”/”** hello slutet av hello utfärdar-URL.

        <add key="ida:audience" value="[Application Client ID GUID]" />
        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/" />

    I [JWT avkodarens](http://jwt.calebb.net/), bör du se **eller** och **iss** som nedan i hello JWT-token:

    ![1 gotcha](./media/media-services-cenc-with-multidrm-access-control/media-services-1st-gotcha.png)
2. Lägga till behörigheter toohello program i AAD (på fliken Konfigurera av programmet hello). Detta krävs för varje program (lokala och distribuerade versioner).

    ![2 gotcha](./media/media-services-cenc-with-multidrm-access-control/media-services-perms-to-other-apps.png)
3. Använd hello rätt utfärdaren för inställning av dynamisk CENC skydd:

        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/"/>

    Det fungerar inte hello följande:

        <add key="ida:issuer" value="https://willzhanad.onmicrosoft.com/" />

    hello GUID är hello AAD-klient-ID. hello GUID kan hittas i slutpunkter popup-fönster i Azure-portalen.
4. Bevilja gruppmedlemskap anspråk privilegier. Kontrollera att i AAD programmanifestfilen har vi hello följande

    ”groupMembershipClaims”: ”alla” (standardvärdet för hello är null)
5. Ange rätt TokenType när du skapar begränsning krav.

        objTokenRestrictionTemplate.TokenType = TokenType.JWT;

    Sedan lägger till stöd för JWT (AAD) dessutom tooSWT (ACS), är hello standard TokenType TokenType.JWT. Om du använder SWT/ACS måste du ange tooTokenType.SWT.

## <a name="additional-topics-for-implementation"></a>Ytterligare artiklar om implementering
Vi kommer nästa iden uss vissa ytterligare avsnitt i våra designen och implementeringen.

### <a name="http-or-https"></a>HTTP eller HTTPS?
hello player ASP.NET MVC-program Vi byggde måste stödja hello följande:

1. Autentisering av användare via Azure AD som måste toobe under HTTPS;
2. JWT-token exchange mellan klienten och Azure AD som måste toobe under HTTPS;
3. DRM-licenser av hello-klient som är nödvändiga toobe under HTTPS om licensleverans tillhandahålls av Azure Media Services. Naturligtvis tvingade PlayReady produktsvit inte HTTPS för licensleverans av. Om licensservern PlayReady är utanför Azure Media Services kan användas antingen HTTP eller HTTPS.

Hello ASP.NET player-programmet kommer därför använda HTTPS som bästa praxis. Detta innebär hello Azure Media Player görs på en sida under HTTPS. Men för strömning vi föredrar HTTP måste därför vi tooconsider blandat innehåll problemet.

1. Blandat innehåll tillåts inte i webbläsaren. Men Tillåt plugin-program som Silverlight och OSMF plugin-program för smooth och STRECK. Blandat innehåll är en säkerhetsfunktion - detta är på grund av toohello hot av hello möjlighet tooinject skadliga JS vilket kan orsaka hello kunden data toobe i fara.  Webbläsare blockera det som standard och hittills hello endast sätt toowork runt har hello (ursprungliga) på serversidan, tooallow alla domäner (oavsett https eller http). Detta är förmodligen inte bra antingen.
2. Vi bör inte blandat innehåll: båda använda HTTP eller båda använder HTTPS. När du spelar blandat innehåll kräver silverlightSS teknisk att du avmarkerar en varning om blandat innehåll. flashSS teknisk hanterar blandat innehåll utan blandat innehåll varning.
3. Om din strömningsslutpunkt skapades innan augusti 2014, stöder inte HTTPS. I det här fallet, skapa och använda en ny strömmande slutpunkt för HTTPS.

I hello referens-implementering för DRM-skyddat innehåll, program och strömning inte under HTTTPS. För att öppna innehållet hello player behöver inte ha autentisering eller licens, så att du har hello liberty toouse antingen HTTP eller HTTPS.

### <a name="azure-active-directory-signing-key-rollover"></a>Azure Active Directory signering nyckelförnyelse
Det här är en viktig punkt tootake i beräkningen av implementeringen. Om du anser det i din implementering, hello slutförts system så småningom att sluta fungera helt inom högst 6 veckor.

Azure AD använder industry standard tooestablish förtroende mellan sig själv och program med hjälp av Azure AD. Mer specifikt använder Azure AD signeringsnyckel som består av en offentlig och privat nyckel. När Azure AD skapar en säkerhetstoken som innehåller information om hello användare, är denna token signerat av Azure AD med hjälp av den privata nyckeln innan den skickas tillbaka toohello program. tooverify hello token är giltig och faktiskt har sitt ursprung från Azure AD, hello programmet måste verifiera hello token signatur med hjälp av hello offentlig nyckel som exponeras av Azure AD som ingår i hello klient federation Metadatadokumentet. Den offentliga nyckeln – och hello signeringsnyckel som härleds – är hello samma som används för alla klienter i Azure AD.

Detaljerad information om Azure AD-nyckelförnyelse kan hittas i hello dokument: [viktig Information om nyckeln signering förnyelse i Azure AD](../active-directory/active-directory-signing-key-rollover.md).

Mellan hello [privat-offentligt nyckelpar](https://login.microsoftonline.com/common/discovery/keys/),

* hello privata nyckeln används av Azure Active Directory toogenerate JWT-token;
* hello offentlig nyckel som används av ett program, till exempel DRM-Licensleveranstjänster i AMS tooverify hello JWT-token;

För säkerhet ändamål roterar Azure Active Directory certificate regelbundet (var 6 veckor). Vid säkerhetsintrång, kan hello nyckelförnyelse inträffa varje gång. Hej licensleveranstjänster i AMS måste därför tooupdate hello offentlig nyckel som används som Azure AD roterar hello nyckelpar, annars misslyckas tokenautentisering i AMS och ingen licens utfärdas.

Detta uppnås genom att ange TokenRestrictionTemplate.OpenIdConnectDiscoveryDocument när du konfigurerar DRM-licensleveranstjänster.

Hej JWT tokenflöde är som nedan:

1. Azure AD utfärdar hello JWT-token med hello aktuella privat nyckel för en autentiserad användare;
2. När en spelare får en CENC med multi-DRM-skyddat innehåll, kommer den först lokalisera hello JWT-token som utfärdas av Azure AD.
3. hello player skickar licens förvärv begäran med hello JWT-token toolicense leveranstjänster i AMS;
4. Hej licensleveranstjänster i AMS använder hello aktuella giltig offentlig nyckel från Azure AD tooverify hello JWT-token innan utfärda licenser.

DRM-licensleveranstjänster kommer alltid kontroll av hello aktuella giltig offentlig nyckel från Azure AD. hello offentliga nyckeln som presenteras av Azure AD kommer att hello-nyckel som används för att verifiera en JWT-token som utfärdas av Azure AD.

Vad händer om hello nyckelförnyelse händer efter AAD genererar en JWT-token, men innan hello JWT token skickas av spelare tooDRM licensleveranstjänster i AMS för verifiering?

Eftersom en nyckel kan återställas när som helst, finns det alltid mer än en giltig offentlig nyckel i hello federation Metadatadokumentet. Azure Media Services licensleverans kan använda någon av hello nycklar anges i hello dokument, eftersom en nyckel kan återställas snart, en annan kan dess ersättare, och så vidare.

### <a name="where-is-hello-access-token"></a>Där är hello åtkomst-Token?
Om du tittar på hur en webbapp anropar en API-app under [Programidentitet med OAuth 2.0 klientens autentiseringsuppgifter Grant](../active-directory/develop/active-directory-authentication-scenarios.md#web-application-to-web-api), hello autentiseringsflödet är som nedan:

1. En användare är inloggad i tooAzure AD i hello webbprogram (se hello [webbläsare tooWeb programmet](../active-directory/develop/active-directory-authentication-scenarios.md#web-browser-to-web-application).
2. hello Azure AD autentiseringsslutpunkt dirigerar hello användaren agenten tillbaka toohello client-program med en Auktoriseringskoden. hello användaragent returnerar auktorisering kod toohello klientprogrammets omdirigerings-URI.
3. hello webbprogrammet måste tooacquire en åtkomst-token så att den kan autentisera toohello webb-API och hämta hello önskad resurs. Den gör en begäran tooAzure Annonsens tokenslutpunkten, hello autentiseringsuppgifter, klient-ID och API: er webbprogrammet ID URI. Hello auktorisering kod tooprove anger att hello användaren har godkänt.
4. Azure AD autentiserar hello program och returnerar en JWT åtkomst-token som används toocall hello webb-API.
5. Via HTTPS använder hello webbprogrammet hello returneras JWT åtkomst-token tooadd hello JWT sträng med en ”ägar” beteckning i hello Authorization-huvud för hello begäran toohello webb-API. hello webb-API verifierar sedan hello JWT-token och om verifieringen lyckas returnerar hello önskad resurs.

I det här flödet ”Programidentitet” förtroenden hello webb-API som hello web autentiserade hello programanvändare. Därför kan kallas det här mönstret betrodda undersystemet. Hej [diagram på den här sidan](https://docs.microsoft.com/azure/active-directory/active-directory-protocols-oauth-code) beskriver hur auktoriseringskod bevilja flödet fungerar.

I licenser med tokenbegränsningar vi följer hello samma betrodda undersystemet mönster. Och hello licensleveranstjänst i Azure Media Services är hello webb-API-resurs, hello ”backend resurs” ett webbprogram måste tooaccess. Var finns så hello åtkomst-token?

Faktiskt får vi åtkomst-token från Azure AD. Efter lyckad användarautentisering returneras auktoriseringskod. hello auktoriseringskod sedan används tillsammans med klient-ID och app nyckeln tooexchange för åtkomst-token. Och hello åtkomst-token för åtkomst till en ”pekaren” programmet pekar eller som representerar Azure Media Services licensleveranstjänst.

Vi behöver tooregister och konfigurera hello ”pekaren” app i Azure AD med hjälp av hello stegen nedan:

1. I hello Azure AD-klient

   * Lägg till ett program (resurs) med inloggnings-URL:

   https://[resource_name].azurewebsites.NET/ och

   * App-ID-URL:

   https://[aad_tenant_name].onmicrosoft.com/[resource_name];
2. Lägg till en ny nyckel för hello resurs app;
3. Uppdatera manifestfilen för hello app så att hello groupMembershipClaims egenskapen har hello följande värde: ”groupMembershipClaims”: ”alla”  
4. Lägg till hello resurs app som har lagts till i steg 1 ovan i hello Azure AD app pekar toohello player webbapp i hello avsnittet ”behörigheter tooother program”. Kontrollera ”åtkomst [resource_name]” markering under ”delegerad behörighet”. Detta ger hello web app behörighet toocreate åtkomst-token för att komma åt hello resurs app. Du bör göra detta för både lokala och distribuerade versionen av hello webbprogram om du utvecklar med Visual Studio och Azure webbapp.

Därför är hello JWT-token som utfärdas av Azure AD verkligen hello åtkomst-token för åtkomst till resursen ”pekaren”.

### <a name="what-about-live-streaming"></a>Vad händer om Live Streaming?
I hello ovan, har vår fokus på tillgångar på begäran. Vad händer om direktsänd strömning?

hello bra är att du kan använda exakt hello samma designen och implementeringen för att skydda direktsänd strömning i Azure Media Services genom att behandla hello tillgången som är associerad med ett program som en ”VOD tillgång”.

Det är särskilt välkända att toodo direktsänd strömning i Azure Media Services behöver toocreate en kanal och sedan ett program under hello-kanal. toocreate hello programmet måste toocreate en tillgång som innehåller hello live Arkiv för hello program. I ordning tooprovide CENC med multi-DRM-skydd av hello levande innehåll, behöver du toodo, är tooapply hello samma installationen/bearbetning toohello tillgång som om det var en ”VOD tillgång” innan du startar hello.

### <a name="what-about-license-servers-outside-of-azure-media-services"></a>Nyheter om licensservrar utanför Azure Media Services?
Kunder kan ofta har investerat i licens servergruppen antingen i sina egna data center eller värdbaserad av DRM-leverantörer. Lyckligtvis innehållsskydd för Azure Media Services kan du toooperate i hybrid läge: innehållet finns och dynamiskt skyddas i Azure Media Services medan DRM-licenser levereras av utanför Azure Media Services-servrar. Det finns i det här fallet hello följande överväganden för ändringar:

1. hello Secure Token Service måste tooissue token som accepteras och som kan verifieras av hello licens-servergrupp. Till exempel hello Widevine licensservrar som tillhandahålls av Axinom kräver en JWT-token som innehåller ”rätt meddelandet”. Du måste därför toohave en STS tooissue sådana JWT-token. hello författare har slutfört en sådan implementering och hello information finns i följande dokument i hello [Azure Documentation Center](https://azure.microsoft.com/documentation/): [med Axinom toodeliver Widevine-licenser tooAzure Media Services](media-services-axinom-integration.md).
2. Du behöver inte längre tooconfigure licensleveranstjänst (ContentKeyAuthorizationPolicy) i Azure Media Services. Vad du behöver toodo är tooprovide hello licens förvärv URL: er (för PlayReady Widevine och FairPlay) när du konfigurerar AssetDeliveryPolicy ställer in CENC med multi-DRM.

### <a name="what-if-i-want-toouse-a-custom-sts"></a>Vad händer om jag vill toouse anpassade STS?
Det kan finnas flera skäl som en kund kan välja toouse en anpassad STS (Secure Token Service) för att tillhandahålla JWT-tokens. Vissa av dessa är:

1. hello identitet Provider (IDP) används av kunden hello stöder inte STS. I det här fallet kan en anpassad STS vara ett alternativ.
2. hello kunden behöva flexibla eller strängare kontroll i integrera STS med kundens prenumeranten faktureringssystem. Till exempel operatör MVPD kan erbjuda flera OTT prenumeranten paket som premium och basic, sport kan etc. hello operatorn toomatch hello anspråk i en token med en prenumerant paketet så att endast innehållet i det högra hello-paketet har frigjorts. I det här fallet ger en anpassad STS hello behövs flexibilitet och kontroll.

Två ändringar behöver toobe görs när du använder en anpassad STS:

1. När du konfigurerar licensleveranstjänst för en tillgång behöver toospecify hello säkerhetsnyckeln används för verifiering av hello anpassade STS (Mer information finns nedan) i stället för hello aktuella nyckeln från Azure Active Directory.
2. När en JTW-token genereras har en säkerhetsnyckel angetts i stället för hello privat nyckel för hello aktuella X509 certifikat i Azure Active Directory.

Det finns två typer av säkerhetsnycklar:

1. Symmetrisk nyckel: hello samma nyckel används för både generera och verifiera en JWT-token;
2. Asymmetrisk nyckel: ett privat-offentligt nyckelpar i X509 används certifikat med privat nyckel för att kryptera/Generera en JWT-token och hello offentlig nyckel för att verifiera hello-token.

#### <a name="tech-note"></a>Teknisk kommentar
Om du använder .NET Framework / C# som utvecklingsplattform för hello X509 certifikat som används för asymmetrisk säkerhetsnyckeln måste ha en nyckellängd på minst 2048. Detta är ett krav för hello klass System.IdentityModel.Tokens.X509AsymmetricSecurityKey i .NET Framework. I annat fall genereras hello följande undantag:

IDX10630: hello 'System.IdentityModel.Tokens.X509AsymmetricSecurityKey' för signering får inte vara mindre än '2048-bitar.

## <a name="hello-completed-system-and-test"></a>hello slutförts system och testning
Går igenom några scenarier i hello slutförts slutpunkt till slutpunkt systemet så att läsare kan ha en grundläggande ”bild” hello beteenden innan du hämtar ett inloggningskonto.

hello player-webbprogram och dess inloggningen finns [här](https://openidconnectweb.azurewebsites.net/).

Om du behöver är ”icke-integrerade” scenario: video tillgångar finns i Azure Media Services som är antingen oskyddade eller DRM-skyddat men utan token autentisering (utfärdar en licens toowhoever som begär den), du kan testa den utan inloggning (genom att växla tooHTTP om din videoströmning är över HTTP).

Om du behöver är integrerad slutpunkt till slutpunkt-scenario: video tillgångar är under dynamisk DRM-skydd i Azure Media Services med token autentisering och JWT-token som genereras av Azure AD, måste du toologin.

### <a name="user-login"></a>Användarinloggning
I ordning tootest hello slutpunkt till slutpunkt integrerad DRM-systemet behöver du toohave en ”konto” har skapats eller lagts till.

Vilket konto?

Även om Azure ursprungligen åtkomst endast av Microsoft-kontoanvändare, kan nu åtkomst av användare från båda systemen. Detta görs genom att låta alla hello Azure egenskaper att lita på Azure AD för autentisering med Azure AD autentisera organisationsanvändare och genom att skapa en federationsrelation där Azure AD litar på konsumentidentitetssystemet för hello Microsoft-konto tooauthenticate konsumentanvändare. Därför är Azure AD kan tooauthenticate ”Microsoft-gästkonton samt” interna ”Azure AD-konton.

Eftersom Azure AD litar på Microsoft-konto (MSA)-domän, kan du lägga till alla konton från någon av hello följande domäner toohello anpassad Azure AD-klient och använda hello konto toologin:

| **Domännamn** | **Domän** |
| --- | --- |
| **Anpassad Azure AD-klient domän** |somename.onmicrosoft.com |
| **Företagets domän** |Microsoft.com |
| **Microsoft-konto (MSA)-domän** |Outlook.com, live.com, hotmail.com |

Du kan kontakta någon av hello författare toohave ett konto som har skapats eller lagts till för dig.

Nedan finns hello skärmdumpar av olika inloggningssidor som används av olika domänkonton.

**Anpassad Azure AD-klient domänkonto**: I detta fall kan du se hello anpassade inloggningssidan i hello anpassad Azure AD-klient domän.

![Domänkonto för anpassad Azure AD-klient](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain1.png)

**Microsoft domänkonto med smartkort**: I det här fallet visas hello inloggningssidan anpassade genom Microsoft företagets IT med tvåfaktorsautentisering.

![Domänkonto för anpassad Azure AD-klient](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain2.png)

**Microsoft-konto (MSA)**: I detta fall kan du se hello inloggningssidan för Account för konsumenter.

![Domänkonto för anpassad Azure AD-klient](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain3.png)

### <a name="using-encrypted-media-extensions-for-playready"></a>Använder krypterade Media-tillägg för PlayReady
På en modern webbläsare med krypterade Media tillägg (EME) för PlayReady-stöd, till exempel Internet Explorer 11 på Windows 8.1 och in och Microsoft Edge-webbläsaren på Windows 10, PlayReady hello underliggande DRM för EME.

![Med hjälp av EME för PlayReady](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-playready1.png)

hello mörkt player område är på grund av toohello faktum att PlayReady skydd förhindrar en gör skärmdump av skyddade video.

hello visar följande skärmbild hello player-plugin-program och mus/EME support.

![Med hjälp av EME för PlayReady](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-playready2.png)

EME i Microsoft Edge och Internet Explorer 11 på Windows 10 kan anropas av [PlayReady SL3000](https://www.microsoft.com/playready/features/EnhancedContentProtection.aspx/) på Windows 10-enheter som stöder den. PlayReady SL3000 låser upp hello flödet av förbättrade premium-innehåll (4K HDR, etc.) och nya modeller för innehållsleverans (tidig fönster för förbättrat innehåll).

Fokusera på hello Windows-enheter: PlayReady är hello endast DRM i hello-maskinvara som är tillgängliga på Windows-enheter (PlayReady SL3000). En strömmande tjänst kan använda PlayReady via EME eller en UWP-appen och ger en högre bildkvaliteten med PlayReady SL3000 än en annan DRM. Normalt 2K innehåll flödar via Chrome eller Firefox och 4 K innehåll via Microsoft Edge/IE11 eller en UWP-appen på hello samma enhet (beroende på implementering och inställningar).

#### <a name="using-eme-for-widevine"></a>Med hjälp av EME för Widevine
På en modern webbläsare med EME/Widevine-support, till exempel Chrome 41 + på Windows 10, Windows 8.1-, Mac OS x Yosemite och Chrome för Android 4.4.4 är Google Widevines hello DRM bakom EME.

![Med hjälp av EME för Widevine](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-widevine1.png)

Observera att Widevine inte hindrar något från att göra en skärmdump av skyddade video.

![Med hjälp av EME för Widevine](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-widevine2.png)

### <a name="not-entitled-users"></a>Inte berättigat till användare
Om en användare inte är medlem i gruppen ”rätt användare” hello användaren kommer inte att kunna toopass ”rätt kontrollera” och hello multi-DRM-Licenstjänsten nekar tooissue hello begärda licens enligt nedan. hello detaljerad beskrivning är ”licens hämta misslyckades”, vilket är fungerar.

![Icke rätt användare](./media/media-services-cenc-with-multidrm-access-control/media-services-unentitledusers.png)

### <a name="running-custom-secure-token-service"></a>Kör anpassade Secure Token Service
Hello scenariot anpassade Secure säkerhetstokentjänst (STS) som körs utfärdas hello JWT-token av hello anpassade STS med hjälp av antingen symmetriskt eller asymmetriskt nyckel.

hello fall med symmetriska nyckel (med hjälp av Chrome):

![Kör anpassade STS](./media/media-services-cenc-with-multidrm-access-control/media-services-running-sts1.png)

Hej fallet med hjälp av asymmetrisk nyckel via X509 certifikat (med Microsoft moderna webbläsare).

![Kör anpassade STS](./media/media-services-cenc-with-multidrm-access-control/media-services-running-sts2.png)

I båda hello ovan fall hello användaren autentisering förblir samma – via Azure AD. hello enda skillnaden är att JWT-token har utfärdats av hello anpassade STS i stället för Azure AD. Naturligtvis när du konfigurerar skydd för dynamiska CENC hello begränsning för licensleveranstjänst anger hello JWT-token, antingen symmetriskt eller asymmetriskt nyckel.

## <a name="summary"></a>Sammanfattning
I det här dokumentet beskrivs vi CENC med flera native DRM och åtkomstkontroll via tokenautentisering: sin design och dess implementering med Azure, Azure Media Services och Azure Media Player.

* En referens design visas som innehåller alla nödvändiga hello-komponenter i ett DRM/CENC undersystem;
* En referensimplementering på Azure, Azure Media Services och Azure Media Player.
* Vissa avsnitt som är direkt inblandade i hello design och implementeringslösning beskrivs också.

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
 
