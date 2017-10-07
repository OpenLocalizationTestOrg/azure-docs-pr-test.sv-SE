---
title: aaaSensitive Data - hotet Modeling verktyget - Azure | Microsoft Docs
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
ms.openlocfilehash: 8a18f43af439241ba193ccf668971ddc4655355f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="security-frame-sensitive-data--mitigations"></a>Säkerhet ram: Känsliga Data | Åtgärder 
| Produkter eller tjänster | Artikel |
| --------------- | ------- |
| **Datorn Förtroendegräns** | <ul><li>[Se till att binärfilerna har dolts om de innehåller känslig information](#binaries-info)</li><li>[Fundera över med krypterade filsystem (EFS) är används tooprotect konfidentiell användarspecifika data](#efs-user)</li><li>[Se till att känsliga data som lagras av programmet hello på hello filsystemet är krypterad.](#filesystem)</li></ul> | 
| **Webbprogram** | <ul><li>[Se till att känsligt innehåll inte cachelagras hello webbläsare](#cache-browser)</li><li>[Kryptera avsnitt i konfigurationsfilerna för Web App som innehåller känsliga data](#encrypt-data)</li><li>[Inaktivera uttryckligen hello autocomplete HTML-attributet i känsliga formulär och indata](#autocomplete-input)</li><li>[Se till att känsliga data som visas på skärmen för hello användaren maskeras](#data-mask)</li></ul> | 
| **Databas** | <ul><li>[Implementera dynamiska data maskering toolimit känsliga data exponering icke privilegierad användare](#dynamic-users)</li><li>[Kontrollera att lösenorden lagras i saltat hash-format](#salted-hash)</li><li>[Se till att känsliga data i databaskolumner krypteras](#db-encrypted)</li><li>[Se till att databasnivå-kryptering (TDE) är aktiverad](#tde-enabled)</li><li>[Se till att databassäkerhetskopiorna krypteras](#backup)</li></ul> | 
| **Webb-API** | <ul><li>[Se till att känsliga data relevanta tooWeb API inte lagras i webbläsarens](#api-browser)</li></ul> | 
| Azure dokumentet DB | <ul><li>[Kryptera känsliga data som lagras i DocumentDB](#encrypt-docdb)</li></ul> | 
| **Azure IaaS-VM Förtroendegräns** | <ul><li>[Använd Azure Disk Encryption tooencrypt diskar som används av virtuella datorer](#disk-vm)</li></ul> | 
| **Service Fabric-Förtroendegräns** | <ul><li>[Kryptera hemligheter i Service Fabric-program](#fabric-apps)</li></ul> | 
| **Dynamics CRM** | <ul><li>[Utför säkerhet modellering och använder företagets enheter/team behov](#modeling-teams)</li><li>[Minimera tooshare funktionen på kritiska entiteter](#entities)</li><li>[Train användare på hello risker associerade med hello Dynamics CRM resursen funktionen och säkerhetsprinciper](#good-practices)</li><li>[En regel för utveckling-standarder som proscribing med config information i hantering av undantag](#exception-mgmt)</li></ul> | 
| **Azure Storage** | <ul><li>[Använd Azure Storage Service-kryptering (SSE) för Data i vila (förhandsgranskning)](#sse-preview)</li><li>[Använd kryptering för klientsidan toostore känsliga data i Azure Storage](#client-storage)</li></ul> | 
| **Mobil klient** | <ul><li>[Kryptera känsliga eller PII-data som skrivs toophones lokal lagring](#pii-phones)</li><li>[Obfuscate genererade binärfiler innan du distribuerar tooend användare](#binaries-end)</li></ul> | 
| **WCF** | <ul><li>[Ange clientCredentialType tooCertificate eller Windows](#cert)</li><li>[WCF-säkerhetsläget har inte aktiverats](#security)</li></ul> | 

## <a id="binaries-info"></a>Se till att binärfilerna har dolts om de innehåller känslig information

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Datorn Förtroendegräns | 
| **SDL fas**               | Distribution |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | Se till att binärfilerna har dolts om de innehåller känslig information, till exempel affärshemligheter, känsliga affärslogik som bör inte ångras. Detta är toostop omvänd konstruktion av sammansättningar. Verktyg som `CryptoObfuscator` kan användas för detta ändamål. |

## <a id="efs-user"></a>Fundera över med krypterade filsystem (EFS) är används tooprotect konfidentiell användarspecifika data

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Datorn Förtroendegräns | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | Överväg att med krypterade filsystem (EFS) är används tooprotect konfidentiell användarspecifika data från motståndare med fysisk åtkomst toohello dator. |

## <a id="filesystem"></a>Se till att känsliga data som lagras av programmet hello på hello filsystemet är krypterad.

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Datorn Förtroendegräns | 
| **SDL fas**               | Distribution |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | Se till att känsliga data som lagras av programmet hello på hello filsystemet är krypterad (t.ex. med DPAPI) om EFS inte kan tillämpas |

## <a id="cache-browser"></a>Se till att känsligt innehåll inte cachelagras hello webbläsare

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk Web Forms, MVC5, MVC6 |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | Webbläsare kan lagra information för tillämpning av cachelagring och historik. De här filerna lagras i en mapp som hello tillfälliga Internetfiler mapp i hello fall av Internet Explorer. När dessa sidor kallas igen, visas hello webbläsaren dem från sin cache. Om känslig information är visas toohello användare (till exempel sina adress, kreditkortsinformation, personnummer eller användarnamn), så den här informationen kan vara lagras i webbläsarens cacheminne, och därför strängfält genom att undersöka hello webbläsarens cache eller genom att helt enkelt trycka hello webbläsarens Bakåt-knappen. Ange cache-control svar huvudets värde för ”no-store” för alla sidor. |

### <a name="example"></a>Exempel
```XML
<configuration>
  <system.webServer>
   <httpProtocol>
    <customHeaders>
        <add name="Cache-Control" value="no-cache" />
        <add name="Pragma" value="no-cache" />
        <add name="Expires" value="-1" />
    </customHeaders>
  </httpProtocol>
 </system.webServer>
</configuration>
```

### <a name="example"></a>Exempel
Detta kan genomföras med ett filter. Följande exempel kan användas: 
```C#
public override void OnActionExecuting(ActionExecutingContext filterContext)
        {
            if (filterContext == null || (filterContext.HttpContext != null && filterContext.HttpContext.Response != null && filterContext.HttpContext.Response.IsRequestBeingRedirected))
            {
                //// Since this is MVC pipeline, this should never be null.
                return;
            }

            var attributes = filterContext.ActionDescriptor.GetCustomAttributes(typeof(System.Web.Mvc.OutputCacheAttribute), false);
            if (attributes == null || **Attributes**.Count() == 0)
            {
                filterContext.HttpContext.Response.Cache.SetNoStore();
                filterContext.HttpContext.Response.Cache.SetCacheability(HttpCacheability.NoCache);
                filterContext.HttpContext.Response.Cache.SetExpires(DateTime.UtcNow.AddHours(-1));
                if (!filterContext.IsChildAction)
                {
                    filterContext.HttpContext.Response.AppendHeader("Pragma", "no-cache");
                }
            }

            base.OnActionExecuting(filterContext);
        }
``` 

## <a id="encrypt-data"></a>Kryptera avsnitt i konfigurationsfilerna för Web App som innehåller känsliga data

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [Så här: Kryptera konfigurationsavsnitt i ASP.NET 2.0 med hjälp av DPAPI](https://msdn.microsoft.com/library/ff647398.aspx), [att ange en Konfigurationsprovider för skyddade](https://msdn.microsoft.com/library/68ze1hb2.aspx), [med hjälp av Azure Key Vault tooprotect programmet hemligheter](https://azure.microsoft.com/documentation/articles/guidance-multitenant-identity-keyvault/) |
| **Steg** | Konfigurationsfiler, till exempel hello Web.config appsettings.json är ofta används toohold känslig information, inklusive användarnamn, lösenord, databasanslutningssträngar och krypteringsnycklar. Om du inte skyddar informationen är ditt program sårbar tooattackers eller angripare erhålla känslig information, till exempel användarnamn och lösenord, databasnamn och servernamn. Baserat på hello distributionstypen (azure/lokalt), kryptera hello känsliga grupper i config-filer med hjälp av DPAPI eller tjänster som Azure Key Vault. |

## <a id="autocomplete-input"></a>Inaktivera uttryckligen hello autocomplete HTML-attributet i känsliga formulär och indata

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [MSDN: autocomplete attributet](http://msdn.microsoft.com/library/ms533486(VS.85).aspx), [med Komplettera automatiskt i HTML](http://msdn.microsoft.com/library/ms533032.aspx), [HTML-rensning säkerhetsproblem](http://technet.microsoft.com/security/bulletin/MS10-071), [Autocomplete., igen?](http://blog.mindedsecurity.com/2011/10/autocompleteagain.html) |
| **Steg** | hello autocomplete attributet anger om ett formulär ska ha autocomplete eller inaktivera. När autocomplete på hello webbläsaren automatiskt fullständig värden baserat på värden som hello användaren har angett innan. Till exempel när ett nytt namn och lösenord har angetts i ett formulär och hello formuläret skickas, så frågar hello webbläsaren om hello lösenord ska sparas. Därefter när hello formuläret visas hello namn och lösenord fylls i automatiskt eller är slutförda som hello namn har angetts. En angripare med lokal åtkomst gick att hämta lösenord i klartext hello från hello webbläsarens cacheminne. Autocomplete är aktiverat som standard och den måste uttryckligen inaktiveras. |

### <a name="example"></a>Exempel
```C#
<form action="Login.aspx" method="post " autocomplete="off" >
      Social Security Number: <input type="text" name="ssn" />
      <input type="submit" value="Submit" />    
</form>
```

## <a id="data-mask"></a>Se till att känsliga data som visas på skärmen för hello användaren maskeras

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | Känsliga data, till exempel lösenord, kreditkortsnummer, SSN etc. ska maskeras när visas på hello-skärmen. Detta är tooprevent auktoriserad personal från att komma åt hello data (t.ex. axel surfning lösenord, supporttekniker visa SSN antal användare). Se till att dessa dataelement visas inte i klartext och maskeras på lämpligt sätt. Detta har toobe tagit hand vid accepterande av organisationens dem som indata (t.ex. input type = ”password”) samt visa tillbaka på hello-skärmen (till exempel visa endast hello 4 sista siffrorna i kreditkortsnumret hello). |

## <a id="dynamic-users"></a>Implementera dynamiska data maskering toolimit känsliga data exponering icke privilegierad användare

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Databas | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | SQL Azure bör OnPrem |
| **Attribut**              | SQL-Version - V12, Version av SQL - MsSQL2016 |
| **Referenser**              | [Dynamisk Datamaskning](https://msdn.microsoft.com/library/mt130841) |
| **Steg** | hello syftar dynamisk datamaskering toolimit exponering av känsliga data, att förhindra att användare som inte ska ha åtkomst till toohello data från att visa den. Dynamisk datamaskning syftar inte tooprevent databasanvändare från ansluter direkt toohello databasen och kör fullständig frågor som exponerar delar av hello känsliga data. Dynamisk datamaskning är kompletterande tooother SQL Server-säkerhetsfunktioner (granskning, kryptering och säkerhet på radnivå...) och det rekommenderas starkt toouse funktionen tillsammans med dem också i ordning toobetter skydda hello känsliga data i hello databas. Observera att den här funktionen stöds bara av SQL Server från och med 2016 och Azure SQL Database. |

## <a id="salted-hash"></a>Kontrollera att lösenorden lagras i saltat hash-format

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Databas | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [Lösenord Hashing med .NET Crypto API: er](http://docs.asp.net/en/latest/security/data-protection/consumer-apis/password-hashing.html) |
| **Steg** | Lösenord bör inte lagras i arkivet för anpassade användardatabaser. Lösenordshashvärden bör lagras med salt värden i stället. Kontrollera att hello salt för hello användare alltid är unikt och du använder b-crypt, s crypt eller PBKDF2 innan lagra hello lösenord med en minsta arbete faktor iterationsantal 150 000 slinga tooeliminate hello möjlighet att tvinga brute.| 

## <a id="db-encrypted"></a>Se till att känsliga data i databaskolumner krypteras

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Databas | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Version av SQL - alla |
| **Referenser**              | [Kryptera känsliga data i SQLServer](https://technet.microsoft.com/library/ff848751(v=sql.105).aspx), [så här: kryptera en kolumn med Data i SQL Server](https://msdn.microsoft.com/library/ms179331), [kryptera av certifikat](https://msdn.microsoft.com/library/ms188061) |
| **Steg** | Känsliga data, till exempel kreditkortsnummer har toobe krypterade i hello-databasen. Data kan krypteras med kryptering på kolumnnivå eller av ett program-funktionen med hjälp av hello kryptering funktioner. |

## <a id="tde-enabled"></a>Se till att databasnivå-kryptering (TDE) är aktiverad

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Databas | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [Förstå Transparent datakryptering för SQL Server (TDE)](https://technet.microsoft.com/library/bb934049(v=sql.105).aspx) |
| **Steg** | Transparent Data kryptering (TDE) funktioner i SQL server hjälper i kryptera känsliga data i en databas och skydda hello nycklar som används tooencrypt hello data med ett certifikat. Detta förhindrar att någon utan hello nycklar med hjälp av hello data. TDE skyddar data ”i vila”, vilket innebär att hello data och loggfilen filer. Det ger hello möjlighet toocomply många lagar, och riktlinjerna i olika branscher. |

## <a id="backup"></a>Se till att databassäkerhetskopiorna krypteras

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Databas | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | SQL Azure bör OnPrem |
| **Attribut**              | SQL-Version - V12, Version av SQL - MsSQL2014 |
| **Referenser**              | [Kryptering av säkerhetskopior i SQL-databas](https://msdn.microsoft.com/library/dn449489) |
| **Steg** | SQL Server har hello möjlighet tooencrypt hello data när du skapar en säkerhetskopia. Genom att ange hello krypteringsalgoritmen och hello krypterare (ett certifikat eller en asymmetrisk nyckel) när du skapar en säkerhetskopia, går att skapa en krypterad säkerhetskopiefil. |

## <a id="api-browser"></a>Se till att känsliga data relevanta tooWeb API inte lagras i webbläsarens

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webb-API | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | MVC 5, MVC 6 |
| **Attribut**              | Identitet Provider - ADFS, identitetsleverantör - Azure AD |
| **Referenser**              | Saknas  |
| **Steg** | <p>I vissa implementeringar autentisering för känsliga artefakter relevanta tooWeb API: er lagras i webbläsarens lokal lagring. Azure AD authentication artefakter som t.ex. adal.idtoken, adal.nonce.idtoken, adal.access.token.key, adal.token.keys, adal.state.login, adal.session.state, adal.expiration.key osv.</p><p>Alla dessa artefakter finns även efter att logga ut eller webbläsaren stängs. Om en angriparen får tillgång till toothese artefakter, kan han eller hon återanvända dem tooaccess hello skyddade resurser (API). Se till att alla känsliga artefakter relaterade tooWeb API inte lagras i webbläsarens. I de fall där klientens lagring är oundvikligt (t.ex. enda sidan program (SPA) som utnyttjar Implicit OpenIdConnect/OAuth-flöden måste toostore åtkomsttoken lokalt), Använd lagringsalternativ med har inte beständig. exempelvis föredra SessionStorage tooLocalStorage.</p>| 

### <a name="example"></a>Exempel
hello nedan JavaScript-kodfragment är från ett bibliotek för anpassad autentisering som lagrar autentiseringsartefakter i lokal lagring. Sådana implementeringar bör undvikas. 
```javascript
ns.AuthHelper.Authenticate = function () {
window.config = {
instance: 'https://login.microsoftonline.com/',
tenant: ns.Configurations.Tenant,
clientId: ns.Configurations.AADApplicationClientID,
postLogoutRedirectUri: window.location.origin,
cacheLocation: 'localStorage', // enable this for IE, as sessionStorage does not work for localhost.
};
```

## <a id="encrypt-docdb"></a>Kryptera känsliga data som lagras i Cosmos-DB

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Azure dokumentet DB | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | Kryptera känsliga data på programnivå innan de lagras i dokumentet DB eller lagra några känsliga data i andra lagringslösningar som Azure Storage eller Azure SQL| 

## <a id="disk-vm"></a>Använd Azure Disk Encryption tooencrypt diskar som används av virtuella datorer

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Azure IaaS-VM Förtroendegräns | 
| **SDL fas**               | Distribution |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [Med hjälp av Azure Disk Encryption tooencrypt diskar som används av virtuella datorer](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines) |
| **Steg** | <p>Azure Disk Encryption är en ny funktion som används för närvarande under förhandsgranskning. Den här funktionen kan du tooencrypt hello OS diskar och datadiskar som används av en virtuell IaaS-dator. För Windows krypteras hello-enheter med BitLocker-kryptering branschstandard. För Linux krypteras hello diskar med hello DM-Crypt teknik. Detta är integrerad med Azure Key Vault tooallow du toocontrol och hantera hello disk krypteringsnycklar. hello Azure Disk Encryption lösning stöder följande tre scenarier för kryptering av kunden hello:</p><ul><li>Aktivera kryptering på den nya virtuella IaaS-datorer skapas från kund-krypterad VHD-filer och som tillhandahålls av kunden krypteringsnycklar som lagras i Azure Key Vault.</li><li>Aktivera kryptering på den nya virtuella IaaS-datorer skapas från hello Azure Marketplace.</li><li>Aktivera kryptering på befintliga virtuella IaaS-datorer körs i Azure.</li></ul>| 

## <a id="fabric-apps"></a>Kryptera hemligheter i Service Fabric-program

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Service Fabric-Förtroendegräns | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Miljö - Azure |
| **Referenser**              | [Hantera hemligheter i Service Fabric-program](https://azure.microsoft.com/documentation/articles/service-fabric-application-secret-management/) |
| **Steg** | Hemligheter kan vara känslig information, till exempel storage-anslutningssträngar, lösenord eller andra värden som inte ska hanteras i oformaterad text. Använd Azure Key Vault toomanage nycklar och hemligheter i fabric-tjänstprogram. |

## <a id="modeling-teams"></a>Utför säkerhet modellering och använder företagets enheter/team behov

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Dynamics CRM | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | Utför säkerhet modellering och använder företagets enheter/team behov |

## <a id="entities"></a>Minimera tooshare funktionen på kritiska entiteter

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Dynamics CRM | 
| **SDL fas**               | Distribution |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | Minimera tooshare funktionen på kritiska entiteter |

## <a id="good-practices"></a>Train användare på hello risker associerade med hello Dynamics CRM resursen funktionen och säkerhetsprinciper

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Dynamics CRM | 
| **SDL fas**               | Distribution |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | Train användare på hello risker associerade med hello Dynamics CRM resursen funktionen och säkerhetsprinciper |

## <a id="exception-mgmt"></a>En regel för utveckling-standarder som proscribing med config information i hantering av undantag

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Dynamics CRM | 
| **SDL fas**               | Distribution |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | Inkludera en utveckling standarder regel proscribing med config information i hantering av undantag utanför utveckling. Testa detta som en del av koden granskningar eller återkommande kontroll.|

## <a id="sse-preview"></a>Använd Azure Storage Service-kryptering (SSE) för Data i vila (förhandsgranskning)

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Azure Storage | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | StorageType - Blob |
| **Referenser**              | [Azure Storage Service-kryptering för Data i vila (förhandsgranskning)](https://azure.microsoft.com/documentation/articles/storage-service-encryption/) |
| **Steg** | <p>Azure Storage Service kryptering (SSE) för Data i vila hjälper dig att skydda och skydda dina data toomeet din organisations säkerhet och efterlevnad åtaganden. Med den här funktionen Azure Storage krypterar dina data tidigare toopersisting toostorage och automatiskt dekrypterar tidigare tooretrieval. Hej kryptering, dekryptering och nyckeln är helt transparent toousers. SSE gäller endast tooblock blobbar, sidblobar, och tilläggsblobar. hello krypteras andra typer av data, inklusive tabeller, köer och filer, inte.</p><p>Kryptering och dekryptering arbetsflöde:</p><ul><li>hello kunden aktiverar kryptering på hello storage-konto</li><li>När hello kunden skriver nya data (PLACERA Blob, PLACERA Block, PLACERA sidan o.s.v.) tooBlob lagring. varje skrivning har krypterats med 256-bitars AES-kryptering, en hello starkaste block chiffer tillgängliga</li><li>När hello kunden måste tooaccess data (hämta Blob, etc.), dekrypteras data automatiskt innan den returnerar toohello användare</li><li>Om kryptering har inaktiverats kan nya skrivningar krypteras inte längre och befintliga krypterade data förblir krypterade tills omarbetning hello användare. Även om kryptering har aktiverats, krypteras skrivningar tooBlob lagring. hello tillstånd av data ändras inte med hello användaren växla mellan aktivering/inaktivering av kryptering för hello storage-konto</li><li>Alla krypteringsnycklar lagras, krypteras och hanteras av Microsoft</li></ul><p>Observera att hello nycklar som används för kryptering av hello hanteras av Microsoft för tillfället. Microsoft ursprungligen genererar hello nycklar och hantera hello säker lagring av hello nycklar samt hello reguljära rotation som definieras av intern Microsoft-princip. I framtida hello, kunderna får hello möjlighet toomanage sina egna > krypteringsnycklar och ange en migreringssökväg till från Microsoft-hanterad nycklar toocustomer-hanterade nycklar.</p>| 

## <a id="client-storage"></a>Använd kryptering för klientsidan toostore känsliga data i Azure Storage

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Azure Storage | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [Kryptering på klientsidan och Azure Key Vault för Microsoft Azure Storage](https://azure.microsoft.com/documentation/articles/storage-client-side-encryption/), [Självstudier: kryptera och dekryptera blobbar i Microsoft Azure Storage med hjälp av Azure Key Vault](https://azure.microsoft.com/documentation/articles/storage-encrypt-decrypt-blobs-key-vault/), [lagra Data på ett säkert sätt i Azure Blob Storage med tillägg för Azure-kryptering](https://blogs.msdn.microsoft.com/partnercatalystteam/2015/06/17/storing-data-securely-in-azure-blob-storage-with-azure-encryption-extensions/) |
| **Steg** | <p>hello Azure Storage-klientbibliotek för .NET-Nuget-paketet har stöd för kryptering av data i klientprogram före överföringen tooAzure lagring och dekryptera data vid hämtning av toohello klienten. hello biblioteket stöder även integrering med Azure Key Vault storage-konto för hantering av nycklar. Här är en kort beskrivning av hur kryptering på serversidan för klienten fungerar:</p><ul><li>hello Azure Storage client SDK genererar en innehåll krypteringsnyckel (CEK), vilket är en gång Använd symmetrisk nyckel</li><li>Kundinformation krypteras med den här CEK</li><li>Hej kapslas CEK sedan (krypterade) med hjälp av krypteringsnyckeln för hello nyckel (KEK). Hej KEK identifieras med en nyckelidentifierare och kan vara ett asymmetriskt nyckelpar eller en symmetrisk nyckel och kan hanteras lokalt eller lagras i Azure Key Vault. hello lagring själva klienten har aldrig åtkomst toohello KEK. Den anropar bara hello viktiga radbrytning algoritmen som tillhandahålls av Key Vault. Kunder kan välja toouse anpassade providers för nyckeln radbrytning/uppackning om de vill</li><li>hello krypterade data har sedan överföra toohello Azure Storage-tjänst. Kontrollera hello länkar i hello referensavsnittet för låg nivå implementeringsdetaljer.</li></ul>|

## <a id="pii-phones"></a>Kryptera känsliga eller PII-data som skrivs toophones lokal lagring

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Mobil klient | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk Xamarin  |
| **Attribut**              | Saknas  |
| **Referenser**              | [Hantera inställningar och funktioner på dina enheter med Microsoft Intune-principer](https://docs.microsoft.com/intune/deploy-use/manage-settings-and-features-on-your-devices-with-microsoft-intune-policies#create-a-configuration-policy), [nyckelringar Valet](https://components.xamarin.com/view/square.valet) |
| **Steg** | <p>Om programmet hello skriver känslig information, till exempel användarens personligt identifierbar information (e-post, telefonnummer, förnamn, efternamn, inställningar osv.) -på mobile filsystem, sedan den ska krypteras innan du skriver toohello lokala filsystem. Om programmet hello ett företagsprogram, utforska du hello möjligheten att publicera program med hjälp av Windows Intune.</p>|

### <a name="example"></a>Exempel
Intune kan konfigureras med följande säkerhet principer toosafeguard känsliga data: 
```C#
Require encryption on mobile device    
Require encryption on storage cards
Allow screen capture
```

### <a name="example"></a>Exempel
Om programmet hello inte är ett företagsprogram och sedan använda plattform keystore kan nyckelringar toostore krypteringsnycklar, med hjälp av vilken kryptografisk åtgärd utföras på hello-filsystemet. Utdrag visar hur tooaccess nyckeln från nyckelringar med xamarin följande kod: 
```C#
        protected static string EncryptionKey
        {
            get
            {
                if (String.IsNullOrEmpty(_Key))
                {
                    var query = new SecRecord(SecKind.GenericPassword);
                    query.Service = NSBundle.MainBundle.BundleIdentifier;
                    query.Account = "UniqueID";

                    NSData uniqueId = SecKeyChain.QueryAsData(query);
                    if (uniqueId == null)
                    {
                        query.ValueData = NSData.FromString(System.Guid.NewGuid().ToString());
                        var err = SecKeyChain.Add(query);
                        _Key = query.ValueData.ToString();
                    }
                    else
                    {
                        _Key = uniqueId.ToString();
                    }
                }

                return _Key;
            }
        }
```

## <a id="binaries-end"></a>Obfuscate genererade binärfiler innan du distribuerar tooend användare

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Mobil klient | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [Kryptografi döljande för .net](http://www.ssware.com/cryptoobfuscator/obfuscator-net.htm) |
| **Steg** | Genererade binärfiler (sammansättningar i apk) ska vara dolda toostop omvänd konstruktion av sammansättningar. Verktyg som `CryptoObfuscator` kan användas för detta ändamål. |

## <a id="cert"></a>Ange clientCredentialType tooCertificate eller Windows

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | WCF | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | .NET framework 3 |
| **Attribut**              | Saknas  |
| **Referenser**              | [Spikning](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Steg** | Använda en UsernameToken med lösenord i klartext via en okrypterad kanal visar hello lösenord tooattackers som kan söka hello SOAP-meddelanden. Leverantörer som använder hello UsernameToken kan acceptera lösenord skickas i klartext. Skicka lösenord i klartext via en okrypterad kanal kan exponera hello autentiseringsuppgifter tooattackers som kan söka hello SOAP-meddelandet. | 

### <a name="example"></a>Exempel
hello använder följande WCF konfiguration av ServiceProvider hello UsernameToken: 
```
<security mode="Message"> 
<message clientCredentialType="UserName" />
``` 
Ange clientCredentialType tooCertificate eller Windows. 

## <a id="security"></a>WCF-säkerhetsläget har inte aktiverats

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | WCF | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Allmän och .NET Framework 3 |
| **Attribut**              | Säkerhet-läge - Transport, säkerhetsläget - meddelande |
| **Referenser**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [spikning kungariket](https://vulncat.fortify.com/en/vulncat/index.html), [grunderna i säkerhet för WCF CoDe Magazine](http://www.codemag.com/article/0611051) |
| **Steg** | Ingen transport eller meddelandet säkerhet har definierats. Program som överför meddelanden utan transport eller meddelande säkerhet inte kan garantera hello integritets- eller konfidentiellt hälsningsmeddelande. När en bindning för WCF-säkerheten är inställd tooNone, säkerhet på både transport och meddelandet har inaktiverats. |

### <a name="example"></a>Exempel
hello konfigurationsuppsättningar följande hello säkerhet läge tooNone. 
```
<system.serviceModel> 
  <bindings> 
    <wsHttpBinding> 
      <binding name=""MyBinding""> 
        <security mode=""None""/> 
      </binding> 
  </bindings> 
</system.serviceModel> 
```

### <a name="example"></a>Exempel
Säkerhetsläge över alla tjänstbindningar det finns fem säkerhetsrisker lägen: 
* Ingen. Inaktiverar säkerhet. 
* Transport. Använder transport säkerhet för ömsesidig autentisering och meddelandet skydd. 
* Meddelande. Använder meddelandesäkerhet för ömsesidig autentisering och meddelandet skydd. 
* Båda. Gör toosupply inställningar för transport och meddelandenivå säkerhet (endast MSMQ stöder detta). 
* TransportWithMessageCredential. Autentiseringsuppgifter skickas med hello-meddelande och meddelandet skydd och serverautentisering som tillhandahålls av hello Transportskiktet. 
* TransportCredentialOnly. Klientens autentiseringsuppgifter skickas med hello Transportskiktet och inget meddelande skydd används. Använd transport och meddelandet säkerhet tooprotect hello integritet och sekretess för meddelanden. hello configuration nedan visar hello service toouse transportsäkerhet med autentiseringsuppgifter för meddelandet.
```
<system.serviceModel>
  <bindings>
    <wsHttpBinding>
    <binding name=""MyBinding""> 
    <security mode=""TransportWithMessageCredential""/> 
    <message clientCredentialType=""Windows""/> 
    </binding> 
  </bindings> 
</system.serviceModel> 
```
