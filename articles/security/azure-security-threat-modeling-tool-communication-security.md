---
title: aaaCommunication Security - hotet Modeling verktyget - Azure | Microsoft Docs
description: "ändringar för hot som exponeras i hello hot Modeling verktyget"
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 667829c75123f4dbe0b383fdaf8cd899802f9b16
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="security-frame-communication-security--mitigations"></a>Säkerhet ram: KOMMUNIKATIONSSÄKERHET | Åtgärder 
| Produkter eller tjänster | Artikel |
| --------------- | ------- |
| **Azure Event Hub** | <ul><li>[Säker kommunikation tooEvent Händelsehubben med hjälp av SSL/TLS](#comm-ssltls)</li></ul> |
| **Dynamics CRM** | <ul><li>[Kontrollera tjänstkontot behörighet och kontrollera att hello anpassade Services eller ASP.NET-sidor respektera CRM-säkerhet](#priv-aspnet)</li></ul> |
| **Azure Data Factory** | <ul><li>[Använda Data management gateway vid anslutning på lokal SQL Server tooAzure Data Factory](#sqlserver-factory)</li></ul> |
| **Identity Server** | <ul><li>[Se till att all trafik tooIdentity Server via HTTPS-anslutning](#identity-https)</li></ul> |
| **Webbprogram** | <ul><li>[Kontrollera X.509-certifikat används tooauthenticate SSL, TLS och DTLS anslutningar](#x509-ssltls)</li><li>[Konfigurera SSL-certifikat för den anpassade domänen i Azure App Service](#ssl-appservice)</li><li>[Tvinga alla trafik tooAzure Apptjänst via HTTPS-anslutning](#appservice-https)</li><li>[Aktivera HTTP strikt transportsäkerhet (HSTS)](#http-hsts)</li></ul> |
| **Databas** | <ul><li>[Se till att SQL server-kryptering och certifikat anslutningsverifiering](#sqlserver-validation)</li><li>[Tvinga krypterad kommunikation tooSQL server](#encrypted-sqlserver)</li></ul> |
| **Azure Storage** | <ul><li>[Se till att kommunikationen tooAzure lagring är över HTTPS](#comm-storage)</li><li>[Validera MD5-hash när du har hämtat blob om det inte går att aktivera HTTPS](#md5-https)</li><li>[Använd SMB 3.0 kompatibel klient tooensure transit data kryptering tooAzure filresurser](#smb-shares)</li></ul> |
| **Mobil klient** | <ul><li>[Implementera fästa certifikat](#cert-pinning)</li></ul> |
| **WCF** | <ul><li>[Aktivera HTTPS - Secure Transportkanalen](#https-transport)</li><li>[WCF: Ange meddelande säkerhet skydd nivå tooEncryptAndSign](#message-protection)</li><li>[WCF: Använda en lägsta behörighet konto toorun WCF-tjänst](#least-account-wcf)</li></ul> |
| **Webb-API** | <ul><li>[Tvinga alla trafik tooWeb API: er via HTTPS-anslutning](#webapi-https)</li></ul> |
| **Azure Redis Cache** | <ul><li>[Se till att kommunikationen tooAzure Redis-Cache är via SSL](#redis-ssl)</li></ul> |
| **Fältet för IoT-Gateway** | <ul><li>[Säker kommunikation mellan tooField Gateway](#device-field)</li></ul> |
| **Gateway för IoT-moln** | <ul><li>[Skydda enheter tooCloud Gateway kommunikation via SSL/TLS](#device-cloud)</li></ul> |

## <a id="comm-ssltls"></a>Säker kommunikation tooEvent Händelsehubben med hjälp av SSL/TLS

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Azure Event Hub | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [Autentisering och säkerhet modellen översikt över Event Hubs](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Steg** | Säker AMQP eller HTTP-anslutningar tooEvent Händelsehubben med hjälp av SSL/TLS |

## <a id="priv-aspnet"></a>Kontrollera tjänstkontot behörighet och kontrollera att hello anpassade Services eller ASP.NET-sidor respektera CRM-säkerhet

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Dynamics CRM | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | Kontrollera tjänstkontot behörighet och kontrollera att hello anpassade Services eller ASP.NET-sidor respektera CRM-säkerhet |

## <a id="sqlserver-factory"></a>Använda Data management gateway vid anslutning på lokal SQL Server tooAzure Data Factory

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Azure Data Factory | 
| **SDL fas**               | Distribution |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Länkade tjänsttyper - Azure och på lokal |
| **Referenser**              |[Flytta data mellan för lokal och Azure Data Factory](https://azure.microsoft.com/documentation/articles/data-factory-move-data-between-onprem-and-cloud/#create-gateway), [Data management gateway](https://azure.microsoft.com/documentation/articles/data-factory-data-management-gateway/) |
| **Steg** | <p>hello Data Management Gateway (DMG)-verktyget är obligatoriska tooconnect toodata källor som är skyddade bakom corpnet eller brandvägg.</p><ol><li>Låsa datorn hello isolerar hello DMG-verktyget och förhindrar att felaktiga program från att skadas eller snooping på källdatorn för hello data. (T.ex.) senaste uppdateringar måste installeras, aktivera minsta nödvändiga portar kontrollerade konton allokering granskning aktiverad disk Aktivera kryptering osv.)</li><li>Data Gateway-nyckeln måste roteras med återkommande intervall eller när förnyas hello DMG kontolösenord</li><li>Data eltransit via länken Service måste krypteras</li></ol> |

## <a id="identity-https"></a>Se till att all trafik tooIdentity Server via HTTPS-anslutning

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Identity Server | 
| **SDL fas**               | Distribution |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [IdentityServer3 - nycklar, signaturer och kryptografi](https://identityserver.github.io/Documentation/docsv2/configuration/crypto.html), [IdentityServer3 - distribution](https://identityserver.github.io/Documentation/docsv2/advanced/deployment.html) |
| **Steg** | Som standard måste IdentityServer toocome för alla inkommande anslutningar via HTTPS. Det är absolut nödvändigt att kommunikation med IdentityServer görs över skyddade transporter. Det finns vissa scenarier för distribution som SSL-avlastning där detta krav kan mjukas upp. Se hello Identity Server-distribution sida i hello referenser till mer information. |

## <a id="x509-ssltls"></a>Kontrollera X.509-certifikat används tooauthenticate SSL, TLS och DTLS anslutningar

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | <p>Program som använder SSL, TLS och DTLS Kontrollera fullständigt hello X.509-certifikat för hello entiteter som de ansluter till. Detta inkluderar verifiering av hello certifikat för:</p><ul><li>Domännamn</li><li>Giltighetsdatum (både början och förfallodatum datum)</li><li>Återställningsstatus</li><li>Användning (exempelvis serverautentisering för servrar, klientautentisering för klienter)</li><li>Förtroende kedjan. Certifikat måste kedja tooa rotcertifikatutfärdare (CA) som är betrodda av hello plattformen eller uttryckligen har konfigurerats av Hej administratör</li><li>Nyckellängden på certifikatets offentliga nyckel måste vara > 2 048 bitar</li><li>Hash-algoritmen måste vara SHA256 och senare |

## <a id="ssl-appservice"></a>Konfigurera SSL-certifikat för den anpassade domänen i Azure App Service

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | EnvironmentType – Azure |
| **Referenser**              | [Aktivera HTTPS för en app i Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-configure-ssl-certificate/) |
| **Steg** | Som standard redan kan HTTPS för varje app med ett jokerteckencertifikat för hello *. azurewebsites.net domän. Precis som alla Jokerteckendomäner det är dock inte lika säker som använder en anpassad domän med egna certifikat [finns](https://casecurity.org/2014/02/26/pros-and-cons-of-single-domain-multi-domain-and-wildcard-certificates/). Det rekommenderas tooenable SSL för hello anpassad domän för vilken hello distribuerad app kommer nås via|

## <a id="appservice-https"></a>Tvinga alla trafik tooAzure Apptjänst via HTTPS-anslutning

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | EnvironmentType – Azure |
| **Referenser**              | [Tillämpa HTTPS i Azure Apptjänst] https://azure.microsoft.com/documentation/articles/web-sites-configure-ssl-certificate/#4-enforce-https-on-your-app) |
| **Steg** | <p>Även om Azure aktiverar redan HTTPS för Azure-app-tjänster med ett jokerteckencertifikat för hello domänen *. azurewebsites.net, tillämpar inte HTTPS. Besökare kan fortfarande komma åt hello appen med hjälp av HTTP, vilket kan äventyra säkerheten hello app och därför HTTPS har toobe tillämpas explicit. ASP.NET MVC-program ska använda hello [RequireHttps filter](http://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) som tvingar en osäker HTTP-begäran toobe skickas på nytt via HTTPS.</p><p>Hello URL-modulen för omarbetning, som ingår i Azure App Service kan alternativt vara används tooenforce HTTPS. URL-omskrivning om modulen kan utvecklare toodefine regler som är kopplade tooincoming begäranden innan hello begäranden lämnas tooyour program. URL-omskrivning om regler har definierats i en web.config-fil som lagras i hello programmet hello rot</p>|

### <a name="example"></a>Exempel
hello innehåller följande exempel en grundläggande URL-omskrivning om regeln som tvingar all inkommande trafik toouse HTTPS
```XML
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <system.webServer>
    <rewrite>
      <rules>
        <rule name="Force HTTPS" enabled="true">
          <match url="(.*)" ignoreCase="false" />
          <conditions>
            <add input="{HTTPS}" pattern="off" />
          </conditions>
          <action type="Redirect" url="https://{HTTP_HOST}/{R:1}" appendQueryString="true" redirectType="Permanent" />
        </rule>
      </rules>
    </rewrite>
  </system.webServer>
</configuration>
```
Den här regeln fungerar genom att returnera ett HTTP-statuskoden 301 (permanent omdirigering) när hello användare begär en sida med HTTP. hello 301 omdirigeringar hello begäran toohello samma Webbadress som hello besökare begärdes, men ersätter hello HTTP-delen av hello begäran med HTTPS. Till exempel är HTTP://contoso.com omdirigerade tooHTTPS://contoso.com. 

## <a id="http-hsts"></a>Aktivera HTTP strikt transportsäkerhet (HSTS)

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [OWASP HTTP strikt transportsäkerhet Cheat blad](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) |
| **Steg** | <p>HTTP strikt Transport säkerhet (HSTS) är en säkerhetsförbättring av opt-in som anges av ett webbprogram via hello användning av en särskild Svarsrubrik. När en webbläsare som stöds tar emot det här sidhuvudet webbläsaren förhindrar all kommunikation som skickas via HTTP toohello domänen och i stället skickar all kommunikation via HTTPS. Det förhindrar också HTTPS klicka dig igenom anvisningarna i webbläsare.</p><p>tooimplement HSTS hello följande svarshuvud har toobe som är konfigurerad för en webbplats, i kod eller i konfig. Strikt--transportsäkerhet: max-ålder = 300; includeSubDomains HSTS löser hello efter hot:</p><ul><li>Användaren bokmärken eller manuellt skriver http://example.com och är ämne tooa man-in-the-middle angripare: HSTS omdirigerar automatiskt tooHTTPS för HTTP-begäranden för hello måldomänen</li><li>Webbprogram som är avsedda toobe enbart HTTPS-oavsiktligt innehåller http-länkar eller hanterar innehåll över HTTP: HSTS omdirigerar automatiskt tooHTTPS för HTTP-begäranden för hello måldomänen</li><li>En man-in-the-middle-angripare försöker toointercept trafik från en drabbade användare som använder ett ogiltigt certifikat och hopes hello användaren accepterar hello ogiltigt certifikat: HSTS tillåter inte en användare toooverride ogiltiga certifikatet hälsningsmeddelande</li></ul>|

## <a id="sqlserver-validation"></a>Se till att SQL server-kryptering och certifikat anslutningsverifiering

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Databas | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | SQL Azure  |
| **Attribut**              | Versionen av SQL - V12 |
| **Referenser**              | [Bästa metoder för att skriva säker anslutningssträngar för SQL-databas](http://social.technet.microsoft.com/wiki/contents/articles/2951.windows-azure-sql-database-connection-security.aspx#best) |
| **Steg** | <p>All kommunikation mellan SQL-databas och ett klientprogram krypteras med Secure Sockets Layer (SSL) vid alla tidpunkter. SQL Database stöder inte okrypterade anslutningar. toovalidate certifikat med programkod eller verktyg, uttryckligen begära en krypterad anslutning och litar hello-servercertifikat. Om din programkod eller verktyg inte begär en krypterad anslutning, får de fortfarande krypterade anslutningar</p><p>Men de kan inte verifiera servercertifikat hello och därmed kommer att utsättas för ”man in hello middle”-attacker. Ange toovalidate certifikat med ADO.NET programkod `Encrypt=True` och `TrustServerCertificate=False` i hello databasanslutningssträng. toovalidate certifikat via SQL Server Management Studio, öppna dialogrutan för hello Anslut tooServer. Klicka på kryptera anslutning hello anslutningsegenskaper på fliken</p>|

## <a id="encrypted-sqlserver"></a>Tvinga krypterad kommunikation tooSQL server

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Databas | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | OnPrem |
| **Attribut**              | SQL-Version - MsSQL2016, SQL-Version - MsSQL2012, Version av SQL - MsSQL2014 |
| **Referenser**              | [Aktivera krypterade anslutningar toohello databasmotor](https://msdn.microsoft.com/library/ms191192)  |
| **Steg** | Om du aktiverar SSL-kryptering ökar hello säkerheten för data som överförs över nätverk mellan instanser av SQL Server och program. |

## <a id="comm-storage"></a>Se till att kommunikationen tooAzure lagring är över HTTPS

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Azure Storage | 
| **SDL fas**               | Distribution |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [Azure Storage transportnivå kryptering – med hjälp av HTTPS](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_encryption-in-transit) |
| **Steg** | tooensure hello säkerheten för Azure Storage data under överföring, Använd alltid hello HTTPS-protokollet anropar hello REST API: er eller få åtkomst till objekt i lagringen. Dessutom signaturer för delad åtkomst, vilket kan vara används toodelegate komma åt tooAzure lagringsobjekt får innehålla en alternativet toospecify som endast hello HTTPS-protokollet kan användas när du använder signaturer för delad åtkomst, se till att vem som helst skicka länkar med SAS-token kommer använda rätt hello-protokollet.|

## <a id="md5-https"></a>Validera MD5-hash när du har hämtat blob om det inte går att aktivera HTTPS

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Azure Storage | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | StorageType - Blob |
| **Referenser**              | [Översikt över Windows Azure Blob-MD5](https://blogs.msdn.microsoft.com/windowsazurestorage/2011/02/17/windows-azure-blob-md5-overview/) |
| **Steg** | <p>Windows Azure Blob-tjänsten tillhandahåller mekanismer tooensure dataintegritet både på hello program och transport lager. Om du av någon anledning som du behöver toouse HTTP i stället för HTTPS och du arbetar med blockblobbar, du kan använda MD5 kontrollerar toohelp verifiera hello integritet hello blob som överförs</p><p>Detta hjälper med skydd från nätverket/transport layer fel, men inte nödvändigtvis med mellanliggande attacker. Om du kan använda HTTPS, vilket ger säkerhet på radnivå transport är med hjälp av MD5 kontrollerar redundant och onödiga.</p>|

## <a id="smb-shares"></a>Använd SMB 3.0 kompatibel klient tooensure data under överföring kryptering tooAzure filresurser

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Mobil klient | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | StorageType - fil |
| **Referenser**              | [Azure File Storage](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/#comment-2529238931), [Azure File Storage SMB-stöd för Windows-klienter](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-files/#_mount-the-file-share) |
| **Steg** | Azure File Storage stöder HTTPS när du använder hello REST-API, men är vanliga som en SMB filresurs kopplad tooa VM. SMB 2.1 stöder inte kryptering, så anslutningar är bara tillåtna inom hello samma region i Azure. Dock SMB 3.0 stöder kryptering och kan användas med Windows Server 2012 R2, Windows 8, Windows 8.1 och Windows 10, vilket gör att flera region åtkomst och även åtkomst på hello skrivbordet. |

## <a id="cert-pinning"></a>Implementera fästa certifikat

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Azure Storage | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Allmän och Windows Phone |
| **Attribut**              | Saknas  |
| **Referenser**              | [Certifikat och offentlig nyckel fästning](https://www.owasp.org/index.php/Certificate_and_Public_Key_Pinning#.Net) |
| **Steg** | <p>Certifikatet fästning skyddar mot Man-In-The-Middle attacker. Fästning är hello process för att associera en värd med deras förväntade X509 certifikat eller offentlig nyckel. När ett certifikat eller en offentlig nyckel är känt eller för en värd, är hello certifikat eller offentlig nyckel associerade eller 'Fäst' toohello värden. </p><p>Därför när en angriparen försöker toodo SSL MITM-attacker under nyckeln för SSL-handskakning hello från angriparens server inte skiljer sig från hello Fäst certifikatets nyckel och hello begäran kommer att tas bort, vilket gör MITM certifikat fästning kan uppnås genom Implementera Servicepointmanager's `ServerCertificateValidationCallback` delegera.</p>|

### <a name="example"></a>Exempel
```C#
using System;
using System.Net;
using System.Net.Security;
using System.Security.Cryptography;

namespace CertificatePinningExample
{
    class CertificatePinningExample
    {
        /* Note: In this example, we're hardcoding a hello certificate's public key and algorithm for 
           demonstration purposes. In a real-world application, this should be stored in a secure
           configuration area that can be updated as needed. */

        private static readonly string PINNED_ALGORITHM = "RSA";

        private static readonly string PINNED_PUBLIC_KEY = "3082010A0282010100B0E75B7CBE56D31658EF79B3A1" +
            "294D506A88DFCDD603F6EF15E7F5BCBDF32291EC50B2B82BA158E905FE6A83EE044A48258B07FAC3D6356AF09B2" +
            "3EDAB15D00507B70DB08DB9A20C7D1201417B3071A346D663A241061C151B6EC5B5B4ECCCDCDBEA24F051962809" +
            "FEC499BF2D093C06E3BDA7D0BB83CDC1C2C6660B8ECB2EA30A685ADE2DC83C88314010FFC7F4F0F895EDDBE5C02" +
            "ABF78E50B708E0A0EB984A9AA536BCE61A0C31DB95425C6FEE5A564B158EE7C4F0693C439AE010EF83CA8155750" +
            "09B17537C29F86071E5DD8CA50EBD8A409494F479B07574D83EDCE6F68A8F7D40447471D05BC3F5EAD7862FA748" +
            "EA3C92A60A128344B1CEF7A0B0D94E50203010001";


        public static void Main(string[] args)
        {
            HttpWebRequest request = (HttpWebRequest)WebRequest.Create("https://azure.microsoft.com");
            request.ServerCertificateValidationCallback = (sender, certificate, chain, sslPolicyErrors) =>
            {
                if (certificate == null || sslPolicyErrors != SslPolicyErrors.None)
                {
                    // Error getting certificate or hello certificate failed basic validation
                    return false;
                }

                var targetKeyAlgorithm = new Oid(certificate.GetKeyAlgorithm()).FriendlyName;
                var targetPublicKey = certificate.GetPublicKeyString();
                
                if (targetKeyAlgorithm == PINNED_ALGORITHM &&
                    targetPublicKey == PINNED_PUBLIC_KEY)
                {
                    // Success, hello certificate matches hello pinned value.
                    return true;
                }
                // Reject, either hello key or hello algorithm does not match hello expected value.
                return false;
            };

            try
            {
                var response = (HttpWebResponse)request.GetResponse();
                Console.WriteLine($"Success, HTTP status code: {response.StatusCode}");
            }
            catch(Exception ex)
            {
                Console.WriteLine($"Failure, {ex.Message}");
            }
            Console.WriteLine("Press any key tooend.");
            Console.ReadKey();
        }
    }
}
```

## <a id="https-transport"></a>Aktivera HTTPS - Secure Transportkanalen

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | WCF | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | NET Framework 3 |
| **Attribut**              | Saknas  |
| **Referenser**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [spikning kungariket](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Steg** | hello programkonfigurationen bör se till att HTTPS används för all åtkomst toosensitive information.<ul><li>**Förklaring:** om ett program som hanterar känslig information och meddelandenivå kryptering inte används, så ska endast beviljas toocommunicate via en krypterad transport-kanal.</li><li>**REKOMMENDATIONER:** se till att HTTP-transport är inaktiverat och aktivera HTTPS-transport i stället. Ersätt till exempel hello `<httpTransport/>` med `<httpsTransport/>` tagg. Förlita dig inte på en network configuration (brandvägg) tooguarantee att hello programmet bara kan nås via en säker kanal. Från filosofisk synvinkel bör hello program inte beroende hello nätverk för säkerheten.</li></ul><p>Ur en praktisk synvinkel spåra hello personer som ansvarar för att skydda nätverket hello alltid inte hello programmet hello säkerhetskrav som de utvecklas.</p>|

## <a id="message-protection"></a>WCF: Ange meddelande säkerhet skydd nivå tooEncryptAndSign

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | WCF | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | .NET framework 3 |
| **Attribut**              | Saknas  |
| **Referenser**              | [MSDN](https://msdn.microsoft.com/library/ff650862.aspx) |
| **Steg** | <ul><li>**Förklaring:** när skyddsnivån är inställd för meddelandet ”none” inaktiverar skydd. Sekretess och integritet uppnås med rätt nivå av inställningen.</li><li>**REKOMMENDATIONER:**<ul><li>När `Mode=None` -inaktiverar skydd för meddelandet</li><li>När `Mode=Sign` -tecken men krypterar inte hello-meddelande; ska användas när det är viktigt att dataintegritet</li><li>När `Mode=EncryptAndSign` -loggar och krypterar hello-meddelande</li></ul></li></ul><p>Överväg att stänga av kryptering och signering meddelandet endast när du bara behöver toovalidate hello integriteten hos hello information utan att bekymra dig för sekretess. Detta kan vara användbart för åtgärder eller servicekontrakt där du behöver toovalidate hello ursprungliga avsändarens men inga känsliga data skickas. När du minskar hello skyddsnivån var noga med att hello-meddelande inte innehåller någon personlig information (PII).</p>|

### <a name="example"></a>Exempel
Konfigurera hello-tjänsten och hello åtgärden tooonly logga hello-meddelande visas i följande exempel hello. Tjänsten kontraktet exempel på `ProtectionLevel.Sign`: hello följande är ett exempel på hur ProtectionLevel.Sign på hello servicekontraktet nivå: 
```
[ServiceContract(Protection Level=ProtectionLevel.Sign] 
public interface IService 
  { 
  string GetData(int value); 
  } 
```

### <a name="example"></a>Exempel
Åtgärden kontraktet exempel på `ProtectionLevel.Sign` (för Granulär kontroll): hello följande är ett exempel på hur du använder `ProtectionLevel.Sign` på hello OperationContract nivå:

```
[OperationContract(ProtectionLevel=ProtectionLevel.Sign] 
string GetData(int value);
``` 

## <a id="least-account-wcf"></a>WCF: Använda en lägsta behörighet konto toorun WCF-tjänst

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | WCF | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | .NET framework 3 |
| **Attribut**              | Saknas  |
| **Referenser**              | [MSDN](https://msdn.microsoft.com/library/ff648826.aspx ) |
| **Steg** | <ul><li>**Förklaring:** kör inte WCF-tjänster under administratörer eller Privilegierade konto. vid röjande tjänster leder den till Hög inverkan.</li><li>**REKOMMENDATIONER:** använder en lägsta behörighet konto toohost dina WCF-tjänsten eftersom den minska angreppsytan för ditt program och minska hello eventuella skador om du angrepp. Om hello-tjänstkontot kräver ytterligare behörighet på infrastrukturresurser, till exempel MSMQ, bör hello händelseloggen, prestandaräknare och hello filsystem, behörighet ges toothese resurser så att hello WCF-tjänst körs har ändrats.</li></ul><p>Om din tjänst måste tooaccess specifika resurser åt hello ursprungliga anroparen, Använd personifiering och delegering tooflow hello Anroparens identitet för en underordnad auktorisering kontroll. Använd hello lokala Nätverkstjänstkonto, vilket är en särskild inbyggt konto som har lägre privilegier i ett utvecklingsscenario med. Skapa en anpassad domän för lägsta behörighet tjänstkonto i ett produktionsscenario.</p>|

## <a id="webapi-https"></a>Tvinga alla trafik tooWeb API: er via HTTPS-anslutning

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webb-API | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | MVC5 MVC6 |
| **Attribut**              | Saknas  |
| **Referenser**              | [Framtvinga SSL i en Web API-styrenhet](http://www.asp.net/web-api/overview/security/working-with-ssl-in-web-api) |
| **Steg** | Om ett program har en HTTPS- och en HTTP-bindning, kan klienter fortfarande använda HTTP tooaccess hello plats. tooprevent denna, Använd en åtgärd filter tooensure som begär tooprotected API: er är alltid över HTTPS.|

### <a name="example"></a>Exempel 
hello visar följande kod ett Web API-autentisering filter som söker efter SSL: 
```C#
public class RequireHttpsAttribute : AuthorizationFilterAttribute
{
    public override void OnAuthorization(HttpActionContext actionContext)
    {
        if (actionContext.Request.RequestUri.Scheme != Uri.UriSchemeHttps)
        {
            actionContext.Response = new HttpResponseMessage(System.Net.HttpStatusCode.Forbidden)
            {
                ReasonPhrase = "HTTPS Required"
            };
        }
        else
        {
            base.OnAuthorization(actionContext);
        }
    }
}
```
Lägg till det här filtret tooany Web API-åtgärder som kräver SSL: 
```C#
public class ValuesController : ApiController
{
    [RequireHttps]
    public HttpResponseMessage Get() { ... }
}
```
 
## <a id="redis-ssl"></a>Se till att kommunikationen tooAzure Redis-Cache är via SSL

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Azure Redis Cache | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [Azure Redis-SSL-stöd](https://azure.microsoft.com/documentation/articles/cache-faq/#when-should-i-enable-the-non-ssl-port-for-connecting-to-redis) |
| **Steg** | Redis-servern stöder inte SSL out of box hello men Azure Redis-Cache har. Om du ansluter tooAzure Redis-Cache och klienten stöder SSL, som StackExchange.Redis, bör du använda SSL. Icke-SSL-porten inaktiveras som standard för nya Azure Redis-Cache-instanser. Se till att säkra standardinställningar för hello inte ändras om det inte finns ett beroende på SSL-stöd för redis-klienter. |

Observera att Redis är utformad toobe nås av betrodda klienter i betrodda miljöer. Det innebär att vanligtvis det inte är en bra idé tooexpose hello Redis instansen direkt toohello internet eller i allmänhet tooan miljö där icke-betrodda klienter direkt åtkomst till hello Redis TCP-port eller UNIX-socket. 

## <a id="device-field"></a>Säker kommunikation mellan tooField Gateway

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Fältet för IoT-Gateway | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | För IP-baserade enheter kan du vanligtvis inkapslade hello kommunikationsprotokoll i SSL/TLS-kanalen tooprotect data under överföringen. Undersök om säker versioner av hello-protokollet som tillhandahåller säkerheten på transport eller meddelandet nivå för andra protokoll som inte stöder SSL/TLS. |

## <a id="device-cloud"></a>Skydda enheter tooCloud Gateway kommunikation via SSL/TLS

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Gateway för IoT-moln | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [Välj din kommunikationsprotokoll](https://azure.microsoft.com/documentation/articles/iot-hub-devguide/#messaging) |
| **Steg** | Säker HTTP/AMQP eller MQTT protokoll med hjälp av SSL/TLS. |
