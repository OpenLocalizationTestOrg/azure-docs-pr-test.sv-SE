---
title: "aaaSigning nyckel förnyelse i Azure AD | Microsoft Docs"
description: "Den här artikeln beskrivs hello signering nyckelförnyelse bästa praxis för Azure Active Directory"
services: active-directory
documentationcenter: .net
author: dstrockis
manager: krassk
editor: 
ms.assetid: ed964056-0723-42fe-bb69-e57323b9407f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2016
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: ac6ade7f3ba2fbd22ea6d447aa5d07a2d6bdd451
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="signing-key-rollover-in-azure-active-directory"></a>Signering nyckelförnyelse i Azure Active Directory
Det här avsnittet beskrivs vad du behöver tooknow om hello offentliga nycklar som används i Azure Active Directory (AD Azure) toosign säkerhetstoken. Det är viktigt toonote som dessa nycklar förnyelse regelbundet och, i nödfall, kan samlas över omedelbart. Alla program som använder Azure AD ska kunna tooprogrammatically referensen hello nyckelförnyelse processen eller upprätta en process som regelbundet manuell förnyelse. Fortsätta att läsa toounderstand hur hello fungerar, hur tooassess hello effekten av hello förnyelse tooyour program och tooupdate tillämpningsprogrammet eller upprätta nyckelförnyelse en periodiska manuell förnyelse processen toohandle om det behövs.

## <a name="overview-of-signing-keys-in-azure-ad"></a>Översikt över Signeringsnycklar i Azure AD
Azure AD använder offentliga nycklar som bygger på branschen standarder tooestablish förtroende mellan sig själv och hello program som använder den. I praktiken detta fungerar hello följande sätt: Azure AD använder en signeringsnyckel som består av en offentlig och privat nyckel. När en användare loggar in tooan program som använder Azure AD för autentisering, skapar en säkerhetstoken som innehåller information om hello användare i Azure AD. Denna token är signerat av Azure AD med hjälp av den privata nyckeln innan den skickas tillbaka toohello program. tooverify hello token är giltig och faktiskt har sitt ursprung från Azure AD, hello programmet måste verifiera hello token signatur med hjälp av hello offentlig nyckel som exponeras av Azure AD som ingår i hello klient [OpenID Connect discovery-dokumentet](http://openid.net/specs/openid-connect-discovery-1_0.html) eller SAML/WS-Fed [federation Metadatadokumentet](active-directory-federation-metadata.md).

Av säkerhetsskäl kunde Azure AD-signering nyckel samlar regelbundet och hello gäller en nödsituation, återställas omedelbart. Alla program som kan integreras med Azure AD bör förberedas toohandle en nyckelförnyelse händelse, oavsett hur ofta kan det uppstå. Om det inte, och försöker toouse en utgången viktiga tooverify hello signatur på en token för ditt program, misslyckas hello inloggning begäran.

Det finns alltid mer än en giltig nyckel i hello OpenID Connect discovery-dokumentet och hello federation metadata. Ditt program bör förberedas toouse någon hello nycklar anges i hello dokument, eftersom en nyckel kan återställas snart, en annan kan dess ersättare, och så vidare.

## <a name="how-tooassess-if-your-application-will-be-affected-and-what-toodo-about-it"></a>Hur tooassess om ditt program kommer att påverkas och vilka toodo om det.
Hur programmet hanterar nyckelförnyelse beror på olika faktorer, till exempel hello typ av program eller vilka identity-protokollet och biblioteket har använts. hello avsnitten nedan bedöma om hello de vanligaste typerna av program som påverkas av hello nyckelförnyelse och ger riktlinjer för hur tooupdate hello programmet toosupport automatisk förnyelse eller manuellt uppdatera hello nyckeln.

* [Intern klientprogram att komma åt resurser](#nativeclient)
* [Webbprogram / API: er som har åtkomst till resurser](#webclient)
* [Webbprogram / API: er skydda resurser och skapats med Azure App Service](#appservices)
* [Webbprogram / skydda resurser med hjälp av .NET OWIN OpenID Connect, WS-Fed eller WindowsAzureActiveDirectoryBearerAuthentication mellanprogram-API: er](#owin)
* [Webbprogram / skydda resurser med hjälp av .NET Core OpenID Connect eller JwtBearerAuthentication mellanprogram-API: er](#owincore)
* [Webbprogram / skydda resurser med hjälp av passport-azure-ad-modulen för Node.js-API: er](#passport)
* [Webbprogram / API: er skydda resurser och skapats med Visual Studio 2015 eller Visual Studio 2017](#vs2015)
* [Webbprogram skydda resurser och skapas med Visual Studio 2013](#vs2013)
* [Webb-API: er skydda resurser och skapas med Visual Studio 2013](#vs2013_webapi)
* [Webbprogram skydda resurser och skapas med Visual Studio 2012](#vs2012)
* [Webbprogram skydda resurser och skapas med Visual Studio 2010, 2008-o med hjälp av Windows Identity Foundation](#vs2010)
* [Webbprogram / API: er som skyddar resurser med andra bibliotek eller implementera manuellt någon hello stöds protokoll](#other)

Den här vägledningen är **inte** gäller för:

* Program som lagts till från Azure AD Application Gallery (inklusive anpassade) ha separata vägledning för angående toosigning nycklar. [Mer information.](../active-directory-sso-certs.md)
* Lokalt program som publicerats via programproxy inte har tooworry om Signeringsnycklar.

### <a name="nativeclient"></a>Intern klientprogram att komma åt resurser
Program som kommer endast åt resurser (dvs Microsoft Graph, KeyVault, API för Outlook och andra Microsoft-APIs) vanligtvis endast hämta en token och skickar vidare toohello resurs-ägare. Med tanke på att de inte skyddar några resurser, inspektera inte hello token och därför behöver inte tooensure som den är korrekt signerad.

Native client-program, om desktop eller mobile, tillhör den här kategorin och därför påverkas inte av hello förnyelse.

### <a name="webclient"></a>Webbprogram / API: er som har åtkomst till resurser
Program som kommer endast åt resurser (dvs Microsoft Graph, KeyVault, API för Outlook och andra Microsoft-APIs) vanligtvis endast hämta en token och skickar vidare toohello resurs-ägare. Med tanke på att de inte skyddar några resurser, inspektera inte hello token och därför behöver inte tooensure som den är korrekt signerad.

Webbprogram och webb-API: er som använder hello endast app-flöde (klientens autentiseringsuppgifter / klientcertifikat), tillhör den här kategorin och därför inte påverkas av hello förnyelse.

### <a name="appservices"></a>Webbprogram / API: er skydda resurser och skapats med Azure App Service
Azure Apptjänst autentisering / auktorisering (EasyAuth)-funktioner redan har nödvändiga logiken hello toohandle nyckelförnyelse automatiskt.

### <a name="owin"></a>Webbprogram / skydda resurser med hjälp av .NET OWIN OpenID Connect, WS-Fed eller WindowsAzureActiveDirectoryBearerAuthentication mellanprogram-API: er
Om ditt program använder hello .NET OWIN OpenID Connect, WS-Fed eller WindowsAzureActiveDirectoryBearerAuthentication mellanprogram har den redan nödvändiga logiken hello toohandle nyckelförnyelse automatiskt.

Du kan bekräfta att programmet använder någon av dessa genom att söka efter någon av följande kodavsnitt i ditt program Startup.cs eller Startup.Auth.cs hello

```
app.UseOpenIdConnectAuthentication(
     new OpenIdConnectAuthenticationOptions
     {
         // ...
     });
```
```
app.UseWsFederationAuthentication(
    new WsFederationAuthenticationOptions
    {
     // ...
     });
```
```
 app.UseWindowsAzureActiveDirectoryBearerAuthentication(
     new WindowsAzureActiveDirectoryBearerAuthenticationOptions
     {
     // ...
     });
```

### <a name="owincore"></a>Webbprogram / skydda resurser med hjälp av .NET Core OpenID Connect eller JwtBearerAuthentication mellanprogram-API: er
Om ditt program använder hello .NET Core OWIN OpenID Connect eller JwtBearerAuthentication mellanprogram, har den redan nödvändiga logiken hello toohandle nyckelförnyelse automatiskt.

Du kan bekräfta att programmet använder någon av dessa genom att söka efter någon av följande kodavsnitt i ditt program Startup.cs eller Startup.Auth.cs hello

```
app.UseOpenIdConnectAuthentication(
     new OpenIdConnectAuthenticationOptions
     {
         // ...
     });
```
```
app.UseJwtBearerAuthentication(
    new JwtBearerAuthenticationOptions
    {
     // ...
     });
```

### <a name="passport"></a>Webbprogram / skydda resurser med hjälp av passport-azure-ad-modulen för Node.js-API: er
Om ditt program använder hello Node.js passport ad-modulen, har den redan nödvändiga logiken hello toohandle nyckelförnyelse automatiskt.

Du kan bekräfta att din ansökan passport-ad genom att söka efter hello följande kodavsnitt i ditt program app.js

```
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

passport.use(new OIDCStrategy({
    //...
));
```

### <a name="vs2015"></a>Webbprogram / API: er skydda resurser och skapats med Visual Studio 2015 eller Visual Studio 2017
Om programmet har skapats med en mall för program i Visual Studio 2015 eller Visual Studio 2017 och du har valt **arbets-och Skolkonton** från hello **ändra autentisering** menyn redan har nödvändiga logiken hello toohandle nyckelförnyelse automatiskt. Den här logiken inbäddat i hello OWIN OpenID Connect mellanprogram hämtar och cachelagrar hello nycklar från hello OpenID Connect discovery-dokumentet och uppdaterar regelbundet hur dem.

Om du har lagt till tooyour autentiseringslösning manuellt, kanske inte programmet hello nödvändiga nyckelförnyelse logik. Du behöver toowrite den själv eller hello Följ stegen i [webbprogram / API: er med hjälp av andra bibliotek eller implementera manuellt någon hello stöds protokoll.](#other).

### <a name="vs2013"></a>Webbprogram skydda resurser och skapas med Visual Studio 2013
Om programmet har skapats med en mall för program i Visual Studio 2013 och du har valt **Organisationskonton** från hello **ändra autentisering** menyn har redan hello behov logik toohandle key förnyelse automatiskt. Den här logiken lagrar Unik identifierare för din organisation och hello signering nyckelinformation i två databastabeller som är associerade med hello-projekt. Du hittar hello anslutningssträngen för databasen hello i hello projektet Web.config-filen.

Om du har lagt till tooyour autentiseringslösning manuellt, kanske inte programmet hello nödvändiga nyckelförnyelse logik. Du behöver toowrite den själv eller hello Följ stegen i [webbprogram / API: er med hjälp av andra bibliotek eller implementera manuellt någon hello stöds protokoll.](#other).

hello följande steg hjälper dig att kontrollera att hello logik fungerar korrekt i ditt program.

1. Öppna hello lösningen i Visual Studio 2013 och klicka sedan på hello **Server Explorer** fliken hello högra fönstret.
2. Expandera **dataanslutningar**, **DefaultConnection**, och sedan **tabeller**. Leta upp hello **IssuingAuthorityKeys** tabell, högerklicka på den och klicka sedan på **visa tabelldata**.
3. I hello **IssuingAuthorityKeys** tabell, ska det finnas minst en rad som motsvarar toohello tumavtrycksvärde för hello nyckeln. Ta bort alla rader i tabellen hello.
4. Högerklicka på hello **klienter** tabell och klicka sedan på **visa tabelldata**.
5. I hello **hyresgäster** tabell, kommer det att minst en rad som motsvarar tooa unik katalog klient identifierare. Ta bort alla rader i tabellen hello. Om du inte tar bort hello rader i båda hello **klienter** tabell och **IssuingAuthorityKeys** tabell, du får ett fel vid körning.
6. Skapa och köra programmet hello. När du har loggat in tooyour konto kan stoppa du hello program.
7. Returnera toohello **Server Explorer** och titta på hello värden i hello **IssuingAuthorityKeys** och **hyresgäster** tabell. Lägg märke till att de har varit automatiskt fylls med hello lämplig information från hello federation metadata dokument.

### <a name="vs2013"></a>Webb-API: er skydda resurser och skapas med Visual Studio 2013
Om du har skapat ett webb-API-program i Visual Studio 2013 med hello Web API-mall och sedan markerade **Organisationskonton** från hello **ändra autentisering** menyn du redan har hello nödvändiga logiken i ditt program.

Om du har manuellt konfigurerade autentisering, följer du anvisningarna för hello nedan toolearn hur tooconfigure Web API-tooautomatically uppdatera dess viktig information.

hello följande kodavsnitt visar hur tooget hello senaste nycklar från hello federation metadata dokument och sedan använda hello [JWT-Token hanteraren](https://msdn.microsoft.com/library/dn205065.aspx) toovalidate hello-token. hello kodstycke förutsätter att du ska använda egna cachelagring för bestående hello viktiga toovalidate framtida token från Azure AD oavsett om den är i en databas, konfigurationsfilen eller någon annanstans.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.IdentityModel.Tokens;
using System.Configuration;
using System.Security.Cryptography.X509Certificates;
using System.Xml;
using System.IdentityModel.Metadata;
using System.ServiceModel.Security;
using System.Threading;

namespace JWTValidation
{
    public class JWTValidator
    {
        private string MetadataAddress = "[Your Federation Metadata document address goes here]";

        // Validates hello JWT Token that's part of hello Authorization header in an HTTP request.
        public void ValidateJwtToken(string token)
        {
            JwtSecurityTokenHandler tokenHandler = new JwtSecurityTokenHandler()
            {
                // Do not disable for production code
                CertificateValidator = X509CertificateValidator.None
            };

            TokenValidationParameters validationParams = new TokenValidationParameters()
            {
                AllowedAudience = "[Your App ID URI goes here, as registered in hello Azure Classic Portal]",
                ValidIssuer = "[hello issuer for hello token goes here, such as https://sts.windows.net/68b98905-130e-4d7c-b6e1-a158a9ed8449/]",
                SigningTokens = GetSigningCertificates(MetadataAddress)

                // Cache hello signing tokens by your desired mechanism
            };

            Thread.CurrentPrincipal = tokenHandler.ValidateToken(token, validationParams);
        }

        // Returns a list of certificates from hello specified metadata document.
        public List<X509SecurityToken> GetSigningCertificates(string metadataAddress)
        {
            List<X509SecurityToken> tokens = new List<X509SecurityToken>();

            if (metadataAddress == null)
            {
                throw new ArgumentNullException(metadataAddress);
            }

            using (XmlReader metadataReader = XmlReader.Create(metadataAddress))
            {
                MetadataSerializer serializer = new MetadataSerializer()
                {
                    // Do not disable for production code
                    CertificateValidationMode = X509CertificateValidationMode.None
                };

                EntityDescriptor metadata = serializer.ReadMetadata(metadataReader) as EntityDescriptor;

                if (metadata != null)
                {
                    SecurityTokenServiceDescriptor stsd = metadata.RoleDescriptors.OfType<SecurityTokenServiceDescriptor>().First();

                    if (stsd != null)
                    {
                        IEnumerable<X509RawDataKeyIdentifierClause> x509DataClauses = stsd.Keys.Where(key => key.KeyInfo != null && (key.Use == KeyType.Signing || key.Use == KeyType.Unspecified)).
                                                             Select(key => key.KeyInfo.OfType<X509RawDataKeyIdentifierClause>().First());

                        tokens.AddRange(x509DataClauses.Select(token => new X509SecurityToken(new X509Certificate2(token.GetX509RawData()))));
                    }
                    else
                    {
                        throw new InvalidOperationException("There is no RoleDescriptor of type SecurityTokenServiceType in hello metadata");
                    }
                }
                else
                {
                    throw new Exception("Invalid Federation Metadata document");
                }
            }
            return tokens;
        }
    }
}
```

### <a name="vs2012"></a>Webbprogram skydda resurser och skapas med Visual Studio 2012
Om programmet har skapats i Visual Studio 2012 används du förmodligen hello identitet och åtkomst verktyget tooconfigure ditt program. Det är också troligt att du använder hello [verifierar Utfärdarens namn registret (VINR)](https://msdn.microsoft.com/library/dn205067.aspx). Hej VINR ansvarar för att underhålla information om betrodda identitetsleverantörer (Azure AD) och hello nycklar används toovalidate token som utfärdats av dem. Hej VINR gör det också enkelt tooautomatically uppdatering hello viktig information lagras i en Web.config-fil genom att hämta hello senaste federation metadata dokument som är associerat med din katalog, kontrollerar om hello konfigurationen är inaktuell med hello senaste dokumentet och uppdaterar hello programmet toouse hello ny nyckel som behövs.

Om du har skapat ditt program med hjälp av hello kodexempel eller genomgången dokumentation som tillhandahålls av Microsoft ingår hello nyckelförnyelse logik redan i projektet. Du kommer att märka att hello koden nedan finns redan i projektet. Om programmet inte redan har denna logik åtgärderna hello nedan tooadd den och tooverify fungerar korrekt.

1. I **Solution Explorer**, lägga till en referens toohello **System.IdentityModel** sammansättningen för hello rätt projekt.
2. Öppna hello **Global.asax.cs** och Lägg till följande hello med direktiven:
   ```
   using System.Configuration;
   using System.IdentityModel.Tokens;
   ```
3. Lägg till följande metod toohello hello **Global.asax.cs** fil:
   ```
   protected void RefreshValidationSettings()
   {
    string configPath = AppDomain.CurrentDomain.BaseDirectory + "\\" + "Web.config";
    string metadataAddress =
                  ConfigurationManager.AppSettings["ida:FederationMetadataLocation"];
    ValidatingIssuerNameRegistry.WriteToConfig(metadataAddress, configPath);
   }
   ```
4. Anropa hello **RefreshValidationSettings()** metod i hello **Application_Start()** metod i **Global.asax.cs** som visas:
   ```
   protected void Application_Start()
   {
    AreaRegistration.RegisterAllAreas();
    ...
    RefreshValidationSettings();
   }
   ```

När du har följt de här stegen kan uppdateras programmets Web.config med hello senaste informationen från hello federation metadata dokument, inklusive hello senaste nycklar. Den här uppdateringen sker varje gång en programpool återanvänds i IIS. IIS är som standard toorecycle program var 29: e timme.

Följ hello steg nedan tooverify hello nyckelförnyelse logiken fungerar.

1. När du har kontrollerat att programmet använder hello koden ovan, öppna hello **Web.config** fil och navigera toohello  **<issuerNameRegistry>**  block, särskilt söker efter hello följa några få rader:
   ```
   <issuerNameRegistry type="System.IdentityModel.Tokens.ValidatingIssuerNameRegistry, System.IdentityModel.Tokens.ValidatingIssuerNameRegistry">
        <authority name="https://sts.windows.net/ec4187af-07da-4f01-b18f-64c2f5abecea/">
          <keys>
            <add thumbprint="3A38FA984E8560F19AADC9F86FE9594BB6AD049B" />
          </keys>
   ```
2. I hello  **<add thumbprint=””>**  ändra hello tumavtrycksvärde genom att ersätta ett tecken med ett annat namn. Spara hello **Web.config** fil.
3. Skapa hello program och sedan köra den. Om du kan slutföra hello inloggningsprocessen, uppdaterar har programmet hello nyckel genom att hämta hello krävs information från din katalog federation metadata dokument. Om du har problem att logga in, kontrollera hello ändringar i ditt program är korrekta genom att läsa hello [att lägga till inloggning tooYour Web program med hjälp av Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) avsnittet eller ladda ned och inspektera hello följande kodexempel: [ Flera innehavare molnet program för Azure Active Directory](https://code.msdn.microsoft.com/multi-tenant-cloud-8015b84b).

### <a name="vs2010"></a>Webbprogram skydda resurser och skapas med Visual Studio 2008 eller 2010 och Windows Identity Foundation (WIF) version 1.0 för .NET 3.5
Om du har skapat ett program på WIF v1.0 finns inga angivna mekanism tooautomatically uppdatera ditt programs konfiguration toouse en ny nyckel.

* *Enklast* använda hello FedUtil verktygsuppsättning som ingår i hello WIF SDK, som kan hämta hello senaste metadata dokument och uppdatera din konfiguration.
* Uppdatera ditt program too.NET 4.5, vilket innefattar hello senaste versionen av WIF finns i hello-namnområde. Du kan sedan använda hello [verifierar Utfärdarens namn registret (VINR)](https://msdn.microsoft.com/library/dn205067.aspx) tooperform automatiska uppdateringar av hello programkonfigurationen.
* Utföra en manuell förnyelse enligt hello instruktioner hello slutet av dokumentet vägledning.

Instruktioner toouse hello FedUtil tooupdate konfigurationen:

1. Kontrollera att du har hello WIF v1.0 SDK är installerat på utvecklingsdatorn för Visual Studio 2008 eller 2010. Du kan [ladda ned det från den här](https://www.microsoft.com/en-us/download/details.aspx?id=4451) om du ännu inte har installerat den.
2. Öppna hello lösningen i Visual Studio och högerklicka hello tillämpliga projektet och välj **uppdatera federationsmetadata**. Om det här alternativet inte är tillgängligt har FedUtil och/eller hello WIF v1.0 SDK inte installerats.
3. Hello-Kommandotolken, Välj **uppdatering** toobegin uppdaterar din federationsmetadata. Om du har åtkomst toohello servermiljö där programmet hello finns kan du också använda Fedutil's [uppdateringsschema för automatisk metadata](https://msdn.microsoft.com/library/ee517272.aspx).
4. Klicka på **Slutför** toocomplete hello uppdateringsprocessen.

### <a name="other"></a>Webbprogram / API: er som skyddar resurser med andra bibliotek eller implementera manuellt någon hello stöds protokoll
Om du använder vissa andra bibliotek eller implementeras manuellt hello stöds protokollen, behöver du tooreview hello biblioteket eller din implementering tooensure som hello nyckeln hämtas från hello OpenID Connect discovery-dokumentet eller hello Federation metadata dokument. Enkelriktade toocheck för det här är toodo en sökning i koden eller hello biblioteket kod för varje anrop ut tooeither hello OpenID identifiering eller hello federation metadata dokument.

Hämta hello nyckel och uppdatera den i enlighet med detta genom att utföra en manuell förnyelse enligt hello instruktioner hello slutet av dokumentet vägledning om de nycklar lagras någonstans eller hårdkodad i ditt program kan du manuellt. **Det rekommenderas starkt att du förbättra ditt program toosupport automatisk förnyelse** med hjälp av hello närmar sig disposition i den här artikeln tooavoid framtida avbrott och omkostnader om Azure AD ökar dess förnyade takt eller har ett nödfall out-of-band-förnyelse.

## <a name="how-tootest-your-application-toodetermine-if-it-will-be-affected"></a>Hur tootest ditt program toodetermine om kommer att påverkas
Du kan kontrollera om ditt program stöder automatisk nyckelförnyelse genom att hämta hello skript och hello instruktionerna i [GitHub-lagringsplatsen.](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey)

## <a name="how-tooperform-a-manual-rollover-if-you-application-does-not-support-automatic-rollover"></a>Hur tooperform en manuell förnyelse om du programmet inte stöder automatisk förnyelse
Om programmet inte **inte** stöd för automatisk förnyelse, måste tooestablish en process som regelbundet övervakar Azure AD signering nycklar och utför en manuell förnyelse därefter. [Den här GitHub-lagringsplatsen](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey) innehåller skript och instruktioner om hur toodo detta.

