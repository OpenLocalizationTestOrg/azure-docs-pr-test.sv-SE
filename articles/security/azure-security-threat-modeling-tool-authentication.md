---
title: aaaAuthentication - hotet Modeling verktyget - Azure | Microsoft Docs
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
ms.openlocfilehash: 06c1b1aebab25e6fb5b666d24ecd9d86085d656c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="security-frame-authentication--mitigations"></a>Säkerhet ram: Autentisering | Åtgärder 
| Produkter eller tjänster | Artikel |
| --------------- | ------- |
| **Webbprogram**    | <ul><li>[Överväg att använda en standardautentisering mekanism tooauthenticate tooWeb program](#standard-authn-web-app)</li><li>[Program måste hantera misslyckade autentiseringsscenarier på ett säkert sätt](#handle-failed-authn)</li><li>[Aktivera steg uppåt eller anpassad autentisering](#step-up-adaptive-authn)</li><li>[Se till att administrativa gränssnitt är korrekt låsta](#admin-interface-lockdown)</li><li>[Implementera har glömt lösenord funktioner på ett säkert sätt](#forgot-pword-fxn)</li><li>[Se till att lösenord och principen tillämpas](#pword-account-policy)</li><li>[Implementera kontroller tooprevent användarnamn uppräkning](#controls-username-enum)</li></ul> |
| **Databas** | <ul><li>[Använd Windows-autentisering för att ansluta tooSQL Server](#win-authn-sql)</li><li>[När det är möjligt använda Azure Active Directory-autentisering för ansluta tooSQL databas](#aad-authn-sql)</li><li>[När SQL-autentiseringsläge används, se till att konto och lösenord principen tillämpas på SQLServer](#authn-account-pword)</li><li>[Använd inte SQL-autentisering i inneslutna databaser](#autn-contained-db)</li></ul> |
| **Azure Event Hub** | <ul><li>[Använda per enhet autentiseringsuppgifter med hjälp av SaS-token](#authn-sas-tokens)</li></ul> |
| **Azure Förtroendegräns** | <ul><li>[Aktivera Azure Multi-Factor Authentication för Azure-administratörer](#multi-factor-azure-admin)</li></ul> |
| **Service Fabric-Förtroendegräns** | <ul><li>[Begränsa anonym åtkomst tooService Fabric-kluster](#anon-access-cluster)</li><li>[Se till att certifikatet för Service Fabric-klient-till-nod skiljer sig från nod till nod certifikatet](#fabric-cn-nn)</li><li>[Använd AAD tooauthenticate klienter tooservice fabric-kluster](#aad-client-fabric)</li><li>[Se till att service fabric certifikat hämtas från en godkänd certifikatutfärdare (CA)](#fabric-cert-ca)</li></ul> |
| **Identity Server** | <ul><li>[Använd standardautentisering scenarier som stöds av Identity Server](#standard-authn-id)</li><li>[Åsidosätt hello standard Identity Server token-cache med en skalbar alternativ](#override-token)</li></ul> |
| **Datorn Förtroendegräns** | <ul><li>[Se till att binärfilerna för distribuerade program är digitalt signerade](#binaries-signed)</li></ul> |
| **WCF** | <ul><li>[Aktivera autentisering när du ansluter tooMSMQ köer i WCF](#msmq-queues)</li><li>[WCF-vill inte att ange meddelande clientCredentialType toonone](#message-none)</li><li>[WCF-vill inte att ange Transport clientCredentialType toonone](#transport-none)</li></ul> |
| **Webb-API** | <ul><li>[Se till att standard autentiseringstekniker används toosecure webb-API: er](#authn-secure-api)</li></ul> |
| **Azure AD** | <ul><li>[Använd standardautentisering scenarier som stöds av Azure Active Directory](#authn-aad)</li><li>[Åsidosätt hello standard ADAL token-cache med en skalbar alternativ](#adal-scalable)</li><li>[Se till att TokenReplayCache används tooprevent hello omsändning av ADAL-autentisering-token](#tokenreplaycache-adal)</li><li>[Använd ADA-biblioteken toomanage token-förfrågningar från OAuth2 klienter tooAAD (eller lokala AD)](#adal-oauth2)</li></ul> |
| **Fältet för IoT-Gateway** | <ul><li>[Autentisera enheter som ansluter toohello fältet Gateway](#authn-devices-field)</li></ul> |
| **Gateway för IoT-moln** | <ul><li>[Se till att enheter som ansluter tooCloud gateway autentiseras](#authn-devices-cloud)</li><li>[Använd autentiseringsuppgifter för per enhet](#authn-cred)</li></ul> |
| **Azure Storage** | <ul><li>[Se till att endast hello begärda behållare och blobbar ges anonym läsbehörighet](#req-containers-anon)</li><li>[Bevilja begränsad åtkomst tooobjects i Azure storage med hjälp av SAS eller SAP](#limited-access-sas)</li></ul> |

## <a id="standard-authn-web-app"></a>Överväg att använda en standardautentisering mekanism tooauthenticate tooWeb program

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| Information | <p>Autentisering är hello process där en entitet bevisar sin identitet, vanligtvis via autentiseringsuppgifter, till exempel användarnamn och lösenord. Det finns flera autentiseringsprotokoll tillgänglig som kan betraktas. Några av dem visas nedan:</p><ul><li>Klientcertifikat</li><li>Windows-baserade</li><li>Formulärbaserad</li><li>Federation - AD FS</li><li>Federation - Azure AD</li><li>Federation - Identity Server</li></ul><p>Överväg att använda en standard mekanism tooidentify hello källa autentiseringsprocessen</p>|

## <a id="handle-failed-authn"></a>Program måste hantera misslyckade autentiseringsscenarier på ett säkert sätt

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| Information | <p>Program som uttryckligen autentiserar användare måste hantera misslyckade autentiseringsscenarier securely.hello autentiseringsmekanism måste:</p><ul><li>Neka åtkomst tooprivileged resurser om autentisering misslyckas</li><li>Visa ett allmänt felmeddelande efter misslyckade autentisering och åtkomst nekad inträffar</li></ul><p>Testa för:</p><ul><li>Skydd av Privilegierade resurser efter misslyckade inloggningar</li><li>Ett allmänt felmeddelande visas om misslyckade autentisering och åtkomst nekad händelse(r)</li><li>Konton har inaktiverats efter ett orimligt antal misslyckade försök</li><ul>|

## <a id="step-up-adaptive-authn"></a>Aktivera steg uppåt eller anpassad autentisering

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| Information | <p>Kontrollera hello program har ytterligare tillstånd (till exempel steg uppåt eller anpassad autentisering via multifaktorautentisering, till exempel skicka Engångslösenord i SMS, e-post osv eller fråga efter återautentiseringen) så hello användaren anropas innan åtkomst beviljas toosensitive information. Denna regel gäller även för att göra viktiga ändringar tooan konto eller åtgärd</p><p>Det här också innebär att hello anpassning av autentisering har införts på ett sådant toobe så toonot ett sätt att programmet hello korrekt tillämpar sammanhangsberoende auktorisering är Tillåt obehörig manipulation med hjälp av i exemplet parametern manipulation</p>|

## <a id="admin-interface-lockdown"></a>Se till att administrativa gränssnitt är korrekt låsta

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| Information | hello första lösningen är toogrant åtkomst bara från en viss källa IP-intervallet toohello administrationsgränssnittet. Om lösningen inte skulle vara möjligt än de är alltid bör tooenforce ett Step Up eller anpassad autentisering för att logga in till hello administrationsgränssnittet |

## <a id="forgot-pword-fxn"></a>Implementera har glömt lösenord funktioner på ett säkert sätt

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| Information | <p>hello är direkt tooverify har glömt lösenord och andra recovery sökvägar skicka en länk inklusive tidsbegränsade aktivering token i stället för hello lösenord sig själv. Ytterligare autentisering baserat på soft-token (t.ex. SMS-token, egna mobila program osv.) kan krävas samt innan hello länken skickas över. Andra bör du inte låsa ut hello användare konto medan hello processen för att få ett nytt lösenord.</p><p>Detta kan leda tooa Denial of service-attack när en angripare beslutar toointentionally låser hello användare med en automatisk attack. Det tredje när begäran om ny hello lösenord angavs pågår vara hello-meddelande som visas generaliserad i ordning tooprevent användarnamn uppräkningen. Fjärde alltid neka hello användning av gamla lösenord och implementera en princip för starkt lösenord.</p> |

## <a id="pword-account-policy"></a>Se till att lösenord och principen tillämpas

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| Information | <p>Lösenord och principen uppdateras organisationens policy och bästa praxis bör genomföras.</p><p>toodefend mot brute force- och ordlista baserat gissa: princip för starkt lösenord måste vara implementerad tooensure att användare skapar komplexa lösenord (t.ex. 12 tecken minimilängd, alfanumeriska och specialtecken).</p><p>Principer för kontoutelåsning kan implementeras i hello följande sätt:</p><ul><li>**Mjuka lås-timeout:** detta kan vara ett bra alternativ för att skydda dina användare mot brute force-attacker. Till exempel när hello användare anger kunde fel lösenord tre gånger hello programmet låsa hello konto under en minut i ordning tooslow ned hello processen för att tvinga sitt lösenord, vilket gör det mindre lönsamma hello angripare tooproceed brute. Om du tooimplement hårda Lås ut motåtgärder i det här exemplet skulle du kunna uppfylla en ”Dos” av permanent utelåsning konton. Du kan också program kan generera ett Engångslösenord (en gång lösenord) och skicka den out-of-band (via e-post, sms etc.) toohello användare. En annan metod kan vara tooimplement CAPTCHA när ett tröskelvärde för antal misslyckade försök har nåtts.</li><li>**Hårda Lås ut:** den här typen av kontoutelåsning ska användas när du identifierar en användare att attackera ditt program och räknare honom med hjälp av permanent utelåsning sitt konto förrän en svarsgrupp hade tid toodo sina dataforensik. Du kan bestämma toogive hello tillbaka användaren sitt konto eller vidta ytterligare rättsliga åtgärder mot honom efter den här processen. Den här typen av metoden förhindrar hello angripare ytterligare leds genom dina program och din infrastruktur.</li></ul><p>toodefend mot attacker på standard och förutsägbar konton, kontrollera att alla nycklar och lösenord är utbytbara, och genereras eller ersättas efter installationen.</p><p>Om programmet hello har tooauto-generera lösenord, se till att hello genereras lösenord är slumpmässig och har hög entropi.</p>|

## <a id="controls-username-enum"></a>Implementera kontroller tooprevent användarnamn uppräkning

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | Alla felmeddelanden ska vara generaliserad i ordning tooprevent användarnamn uppräkningen. Även undvika ibland du inte information läcker i funktioner, till exempel en registreringssidan. Här behöver du toouse hastighetsbegränsande metoder som till exempel CAPTCHA tooprevent en automatiserad attack från en angripare. |

## <a id="win-authn-sql"></a>Använd Windows-autentisering för att ansluta tooSQL Server

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Databas | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | OnPrem |
| **Attribut**              | Version av SQL - alla |
| **Referenser**              | [SQLServer - Välj ett autentiseringsläge](https://msdn.microsoft.com/library/ms144284.aspx) |
| **Steg** | Windows-autentisering använder Kerberos-säkerhetsprotokoll, tillhandahåller lösenord principtillämpning med beaktande toocomplexity verifieringen av starkt lösenord, ger stöd för kontoutelåsning och har stöd för Lösenordets giltighetstid.|

## <a id="aad-authn-sql"></a>När det är möjligt använda Azure Active Directory-autentisering för ansluta tooSQL databas

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Databas | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | SQL Azure |
| **Attribut**              | Versionen av SQL - V12 |
| **Referenser**              | [Ansluta tooSQL databasen med hjälp av Azure Active Directory-autentisering](https://azure.microsoft.com/documentation/articles/sql-database-aad-authentication/) |
| **Steg** | **Lägsta version:** tooallow Azure SQL Database toouse AAD-autentisering mot hello Microsoft Directory krävs för Azure SQL Database V12 |

## <a id="authn-account-pword"></a>När SQL-autentiseringsläge används, se till att konto och lösenord principen tillämpas på SQLServer

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Databas | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [SQL Server-lösenordsprincip](https://technet.microsoft.com/library/ms161959(v=sql.110).aspx) |
| **Steg** | När du använder SQL Server-autentisering, skapas inloggningar i SQL Server som inte är baserade på Windows-användarkonton. Både hello användarnamn och lösenord för hello skapas med hjälp av SQL Server och lagras i SQL Server. SQL Server kan använda Windows lösenord princip mekanismer. Den kan använda hello samma komplexitet och förfallodatum principer används i Windows toopasswords användas i SQL Server. |

## <a id="autn-contained-db"></a>Använd inte SQL-autentisering i inneslutna databaser

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Databas | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | OnPrem SQL Azure |
| **Attribut**              | SQL-Version - MSSQL2012, Version av SQL - V12 |
| **Referenser**              | [Rekommenderade säkerhetsmetoder med inneslutna databaser](http://msdn.microsoft.com/library/ff929055.aspx) |
| **Steg** | hello frånvaron av en tvingande lösenordsprincip kan öka hello sannolikheten för en svag autentiseringsuppgift upprättas i en innesluten databas. Använda Windows-autentisering. |

## <a id="authn-sas-tokens"></a>Använda per enhet autentiseringsuppgifter med hjälp av SaS-token

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Azure Event Hub | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [Autentisering och säkerhet modellen översikt över Event Hubs](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Steg** | <p>Hej Händelsehubbar säkerhetsmodellen bygger på en kombination av delade signatur åtkomst (SAS)-token och utgivare. hello utgivarens namn representerar hello DeviceID som tar emot hello-token. Detta kan hjälpa till att associera hello-token som genereras med respektive hello-enheter.</p><p>Alla meddelanden som är märkta med avsändaren på tjänstsidan så att identifiering av in nyttolast ursprung förfalskning försök. När autentisering enheter, generera en per enhet SaS-token begränsade tooa unika utgivare.</p>|

## <a id="multi-factor-azure-admin"></a>Aktivera Azure Multi-Factor Authentication för Azure-administratörer

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Azure Förtroendegräns | 
| **SDL fas**               | Distribution |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [Vad är Azure Multi-Factor Authentication?](https://azure.microsoft.com/documentation/articles/multi-factor-authentication/) |
| **Steg** | <p>Multifaktorautentisering (MFA) är en autentiseringsmetod som kräver mer än en verifieringsmetod och lägger till ett kritiskt andra säkerhetslager säkerhet toouser inloggningar och transaktioner. Det fungerar genom att två eller flera av följande verifieringsmetoderna hello:</p><ul><li>Något du vet (normalt ett lösenord)</li><li>Något du har (betrodda enheter som inte enkelt dubbleras, t.ex. en telefon)</li><li>Något du är (biometrik)</li><ul>|

## <a id="anon-access-cluster"></a>Begränsa anonym åtkomst tooService Fabric-kluster

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Service Fabric-Förtroendegräns | 
| **SDL fas**               | Distribution |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Miljö - Azure  |
| **Referenser**              | [Säkerhetsscenarier för Service Fabric-kluster](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security) |
| **Steg** | <p>Kluster ska alltid vara skyddad tooprevent obehöriga användare från att ansluta tooyour kluster, särskilt när den har produktionsarbetsbelastningar som körs på den.</p><p>När du skapar ett service fabric-kluster, se till att hello säkerhetsläget anges för ”säker” och konfigurera hello krävs X.509-servercertifikat. Skapa ett kluster som är ”osäker” kan en anonym användare tooconnect tooit om det visar management slutpunkter toohello offentliga Internet.</p>|

## <a id="fabric-cn-nn"></a>Se till att certifikatet för Service Fabric-klient-till-nod skiljer sig från nod till nod certifikatet

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Service Fabric-Förtroendegräns | 
| **SDL fas**               | Distribution |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Miljö - Azure, miljö - fristående |
| **Referenser**              | [Service Fabric-klient-till-nod Certifikatsäkerhet](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/#_client-to-node-certificate-security), [Anslut tooa säker kluster med hjälp av klientcertifikat](https://azure.microsoft.com/documentation/articles/service-fabric-connect-to-secure-cluster/) |
| **Steg** | <p>Certifikatsäkerhet för klient-till-nod konfigureras när du skapar klustret hello antingen via hello Azure-portalen, Resource Manager-mallar eller en fristående JSON-mall genom att ange ett klientcertifikat för admin och/eller ett klientcertifikat.</p><p>Hej administratör klient- och klientcertifikat du anger måste skilja sig från hello primära och sekundära certifikat som du anger för nod till nod säkerhet.</p>|

## <a id="aad-client-fabric"></a>Använd AAD tooauthenticate klienter tooservice fabric-kluster

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Service Fabric-Förtroendegräns | 
| **SDL fas**               | Distribution |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Miljö - Azure |
| **Referenser**              | [Klustret säkerhetsscenarier - säkerhetsrekommendationer](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/#security-recommendations) |
| **Steg** | Kluster som körs på Azure kan även skydda åtkomst toohello management slutpunkter med hjälp av Azure Active Directory (AAD), förutom klientcertifikat. Det rekommenderas att du använder AAD säkerhet tooauthenticate klienter och certifikat för nod till nod säkerhet för Azure-kluster.|

## <a id="fabric-cert-ca"></a>Se till att service fabric certifikat hämtas från en godkänd certifikatutfärdare (CA)

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Service Fabric-Förtroendegräns | 
| **SDL fas**               | Distribution |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Miljö - Azure |
| **Referenser**              | [X.509-certifikat och Service Fabric](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/#x509-certificates-and-service-fabric) |
| **Steg** | <p>Service Fabric använder X.509-certifikat för servern för att autentisera noder och klienter.</p><p>Några viktiga saker tooconsider när du använder certifikat i service fabric:</p><ul><li>Certifikat som används i kluster som kör produktionsarbetsbelastningar ska skapas med hjälp av en korrekt konfigurerade tjänsten för Windows Server-certifikat eller erhålls från en godkänd certifikatutfärdare (CA). hello Certifikatutfärdare kan vara en godkänd extern Certifikatutfärdare eller en korrekt hanterade interna Public Key Infrastructure (PKI)</li><li>Använd aldrig några tillfälligt eller testa certifikat i produktion som skapas med verktyg som MakeCert.exe</li><li>Du kan använda ett självsignerat certifikat, men bör bara göra det för testkluster och inte i produktion</li></ul>|

## <a id="standard-authn-id"></a>Använd standardautentisering scenarier som stöds av Identity Server

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Identity Server | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [IdentityServer3 - hello översikt](https://identityserver.github.io/Documentation/docsv2/overview/bigPicture.html) |
| **Steg** | <p>Nedan visas hello vanliga interaktioner som stöds av Identity Server:</p><ul><li>Webbläsare kommunicera med webbprogram</li><li>Webbprogram kommunicera med web API: er (ibland på egen hand, ibland för en användares räkning)</li><li>Webbläsarbaserade program kommunicerar med web API: er</li><li>Interna program kommunicerar med web API: er</li><li>Server-baserade program kommunicerar med web API: er</li><li>Webb-API: er kommunicera med web API: er (ibland på egen hand, ibland för en användares räkning)</li></ul>|

## <a id="override-token"></a>Åsidosätt hello standard Identity Server token-cache med en skalbar alternativ

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Identity Server | 
| **SDL fas**               | Distribution |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [Identity Server-distributionen - cachelagring](https://identityserver.github.io/Documentation/docsv2/advanced/deployment.html) |
| **Steg** | <p>IdentityServer har en enkel inbyggda i cacheminnet. Även om det är bra för interna appar i liten skala den inte kan skaländras för mid nivå och backend-program för hello följande orsaker:</p><ul><li>Dessa program kan nås av många användare samtidigt. Spara alla åtkomst-token i hello samma Arkiv skapas isolering problem och visar problem när du arbetar med skalan: många användare med så många token som hello resurser hello app-åtkomst för deras räkning kan innebära stora tal och väldigt dyr sökning åtgärder</li><li>Dessa program distribueras vanligtvis på distribuerade topologier, där flera noder måste ha åtkomst toohello samma cache</li><li>Cachelagrade token måste klara återvinns och avaktiveringar</li><li>För alla hello ovan orsaker, medan implementera web apps rekommenderas toooverride hello standard identitet serverns token-cache med en skalbar alternativ, till exempel Azure Redis-cache</li></ul>|

## <a id="binaries-signed"></a>Se till att binärfilerna för distribuerade program är digitalt signerade

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Datorn Förtroendegräns | 
| **SDL fas**               | Distribution |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | Se till att binärfilerna för distribuerade program har signerats digitalt så att hello integriteten hos hello binärfiler kan verifieras|

## <a id="msmq-queues"></a>Aktivera autentisering när du ansluter tooMSMQ köer i WCF

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | WCF | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk NET Framework 3 |
| **Attribut**              | Saknas |
| **Referenser**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx) |
| **Steg** | Programmet misslyckas tooenable autentisering när du ansluter tooMSMQ köer, kan en angripare anonymt skicka meddelanden toohello kö för bearbetning. Om autentisering inte används tooconnect tooan MSMQ kö som används för toodeliver ett meddelande tooanother program, kan en angripare skicka meddelandet anonym som är skadliga.|

### <a name="example"></a>Exempel
Hej `<netMsmqBinding/>` element av hello WCF-konfigurationsfil nedan instruerar WCF toodisable autentisering när du ansluter tooan MSMQ-kön för leverans av meddelanden.
```
<bindings>
    <netMsmqBinding>
        <binding>
            <security>
                <transport msmqAuthenticationMode=""None"" />
            </security>
        </binding>
    </netMsmqBinding>
</bindings>
```
Konfigurera MSMQ toorequire Windows-domän eller certifikat autentisering vid alla tidpunkter för inkommande eller utgående meddelanden.

### <a name="example"></a>Exempel
Hej `<netMsmqBinding/>` element av hello WCF-konfigurationsfil nedan instruerar WCF tooenable autentisering när du ansluter tooan MSMQ-kön. hello-klienten är autentiserad med X.509-certifikat. hello klientcertifikatet måste finnas i certifikatarkivet för hello hello-Server.
```
<bindings>
    <netMsmqBinding>
        <binding>
            <security>
                <transport msmqAuthenticationMode=""Certificate"" />
            </security>
        </binding>
    </netMsmqBinding>
</bindings>
```

## <a id="message-none"></a>WCF-vill inte att ange meddelande clientCredentialType toonone

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | WCF | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | .NET framework 3 |
| **Attribut**              | Klienten autentiseringstypen - ingen |
| **Referenser**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [spikning](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Steg** | hello frånvaron av autentisering innebär att alla är kan tooaccess den här tjänsten. En tjänst som inte autentiserar klienter åtkomst tooall användare. Konfigurera hello programmet tooauthenticate mot klientens autentiseringsuppgifter. Detta kan göras genom att ange hello meddelandet clientCredentialType tooWindows eller certifikat. |

### <a name="example"></a>Exempel
```
<message clientCredentialType=""Certificate""/>
```

## <a id="transport-none"></a>WCF-vill inte att ange Transport clientCredentialType toonone

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | WCF | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Allmän och .NET Framework 3 |
| **Attribut**              | Klienten autentiseringstypen - ingen |
| **Referenser**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [spikning](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Steg** | hello frånvaron av autentisering innebär att alla är kan tooaccess den här tjänsten. En tjänst som inte autentiserar klienter kan alla användare tooaccess dess funktioner. Konfigurera hello programmet tooauthenticate mot klientens autentiseringsuppgifter. Detta kan göras genom att ange hello transport clientCredentialType tooWindows eller certifikat. |

### <a name="example"></a>Exempel
```
<transport clientCredentialType=""Certificate""/>
```

## <a id="authn-secure-api"></a>Se till att standard autentiseringstekniker används toosecure webb-API: er

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webb-API | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [Autentisering och auktorisering i ASP.NET Web API](http://www.asp.net/web-api/overview/security/authentication-and-authorization-in-aspnet-web-api), [tjänster extern autentisering med ASP.NET Web API (C#)](http://www.asp.net/web-api/overview/security/external-authentication-services) |
| **Steg** | <p>Autentisering är hello process där en entitet bevisar sin identitet, vanligtvis via autentiseringsuppgifter, till exempel användarnamn och lösenord. Det finns flera autentiseringsprotokoll tillgänglig som kan betraktas. Några av dem visas nedan:</p><ul><li>Klientcertifikat</li><li>Windows-baserade</li><li>Formulärbaserad</li><li>Federation - AD FS</li><li>Federation - Azure AD</li><li>Federation - Identity Server</li></ul><p>Länkarna i avsnittet för hello referenser ange låg nivå information om hur var och en av hello autentiseringsscheman kan vara implementerad toosecure webb-API.</p>|

## <a id="authn-aad"></a>Använd standardautentisering scenarier som stöds av Azure Active Directory

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Azure AD | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [Autentiseringsscenarier för Azure AD](https://azure.microsoft.com/documentation/articles/active-directory-authentication-scenarios/), [Azure Active Directory-kodexempel](https://azure.microsoft.com/documentation/articles/active-directory-code-samples/), [Azure Active Directory-guide för utvecklare](https://azure.microsoft.com/documentation/articles/active-directory-developers-guide/) |
| **Steg** | <p>Azure Active Directory (AD Azure) gör det enklare för utvecklare genom att tillhandahålla identitet som en tjänst med stöd för standardiserade protokoll, till exempel OAuth 2.0 och OpenID Connect. Här följer hello fem primära Programscenarier som stöds av Azure AD:</p><ul><li>Web webbläsare tooWeb program: en användare behöver toosign i tooa webbprogram som skyddas av Azure AD</li><li>Sidan program (SPA): En användare behöver toosign i tooa sida program som skyddas av Azure AD</li><li>Interna program tooWeb API: en intern program som körs på en mobiltelefon, surfplatta eller dator måste tooauthenticate en användare tooget resurser från ett webb-API som skyddas av Azure AD</li><li>Webb-API för programmet tooWeb: ett webbprogram måste tooget resurser från ett webb-API som skyddas av Azure AD</li><li>Daemon eller serverprogrammet tooWeb API: en daemon-program eller ett serverprogram utan användargränssnitt för web måste tooget resurser från ett webb-API som skyddas av Azure AD</li></ul><p>Mer implementeringsinformation om lägsta finns toohello länkarna i avsnittet för hello referenser</p>|

## <a id="adal-scalable"></a>Åsidosätt hello standard ADAL token-cache med en skalbar alternativ

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Azure AD | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [Modern autentisering med Azure Active Directory för webbprogram](https://blogs.msdn.microsoft.com/microsoft_press/2016/01/04/new-book-modern-authentication-with-azure-active-directory-for-web-applications/), [med Redis som ADAL token-cache](https://blogs.msdn.microsoft.com/mrochon/2016/09/19/using-redis-as-adal-token-cache/)  |
| **Steg** | <p>hello standard cache som använder ADAL (Active Directory Authentication Library) är en minnescache som förlitar sig på en statisk store tillgängliga övergripande. När det här fungerar för interna program den inte kan skaländras för mid nivå och backend-program för hello följande orsaker:</p><ul><li>Dessa program kan nås av många användare samtidigt. Spara alla åtkomst-token i hello samma Arkiv skapas isolering problem och visar problem när du arbetar med skalan: många användare med så många token som hello resurser hello app-åtkomst för deras räkning kan innebära stora tal och väldigt dyr sökning åtgärder</li><li>Dessa program distribueras vanligtvis på distribuerade topologier, där flera noder måste ha åtkomst toohello samma cache</li><li>Cachelagrade token måste klara återvinns och avaktiveringar</li></ul><p>För alla hello ovan orsaker, medan implementera web apps bör toooverride hello standard ADAL-token-cache med en skalbar alternativ, till exempel Azure Redis-cache.</p>|

## <a id="tokenreplaycache-adal"></a>Se till att TokenReplayCache används tooprevent hello omsändning av ADAL-autentisering-token

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Azure AD | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [Modern autentisering med Azure Active Directory för webbprogram](https://blogs.msdn.microsoft.com/microsoft_press/2016/01/04/new-book-modern-authentication-with-azure-active-directory-for-web-applications/) |
| **Steg** | <p>Hej TokenReplayCache egenskap kan utvecklare toodefine token repetitionscachen, en butik som kan användas för att spara token för hello syftet med att kontrollera att inga token kan användas mer än en gång.</p><p>Detta är en åtgärd mot en gemensam attack hello aptly kallas token replay-attacker: en angripare fångar upp hello-token som skickas vid inloggning kan försöka toosend den toohello app igen (”” spela upp det igen) för att upprätta en ny session. T.ex. I OIDC kod-bevilja flödet efter autentiseringen av användaren, en begäran för ”/ signin-oidc” slutpunkten för hello förlitande part görs med ”id_token”, ”kod” och ”tillstånd” parametrar.</p><p>Hej förlitande part validerar denna begäran och upprättar en ny session. Om en angriparen spelar upp den samlar in denna begäran, kan han eller hon upprätta en lyckad session och spoof hello användare. hello förekomst av hello temporärt ID i OpenID Connect kan begränsa men inte helt eliminera hello omständigheter där hello attack kan vara har trätt i kraft. tooprotect sina program utvecklare kan tillhandahålla en implementering av ITokenReplayCache och tilldela en instans tooTokenReplayCache.</p>|

### <a name="example"></a>Exempel
```C#
// ITokenReplayCache defined in ADAL
public interface ITokenReplayCache
{
bool TryAdd(string securityToken, DateTime expiresOn);
bool TryFind(string securityToken);
}
```

### <a name="example"></a>Exempel
Här är ett exempel på implementering av hello ITokenReplayCache gränssnitt. (Du anpassa och implementera din projektspecifika cachelagring framework)
```C#
public class TokenReplayCache : ITokenReplayCache
{
    private readonly ICacheProvider cache; // Your project-specific cache provider
    public TokenReplayCache(ICacheProvider cache)
    {
        this.cache = cache;
    }
    public bool TryAdd(string securityToken, DateTime expiresOn)
    {
        if (this.cache.Get<string>(securityToken) == null)
        {
            this.cache.Set(securityToken, securityToken);
            return true;
        }
        return false;
    }
    public bool TryFind(string securityToken)
    {
        return this.cache.Get<string>(securityToken) != null;
    }
}
```
hello implementerats cache har toobe som refereras i OIDC alternativ via hello ”TokenValidationParameters”-egenskapen på följande sätt.
```C#
OpenIdConnectOptions openIdConnectOptions = new OpenIdConnectOptions
{
    AutomaticAuthenticate = true,
    ... // other configuration properties follow..
    TokenValidationParameters = new TokenValidationParameters
    {
        TokenReplayCache = new TokenReplayCache(/*Inject your cache provider*/);
    }
}
```

Kontrollera Observera att tootest hello effektiviteten i den här konfigurationen kan logga in på ditt lokala OIDC-skyddade program och samla in hello begäran för`"/signin-oidc"` slutpunkt i fiddler. När hello skydd inte är på plats kan anger spela upp den här begäran i fiddler en ny sessionscookie. När hello begäran spelas när hello TokenReplayCache skydd har lagts till, genereras hello program ett undantagsfel på följande sätt:`SecurityTokenReplayDetectedException: IDX10228: hello securityToken has previously been validated, securityToken: 'eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ1......`

## <a id="adal-oauth2"></a>Använd ADA-biblioteken toomanage token-förfrågningar från OAuth2 klienter tooAAD (eller lokala AD)

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Azure AD | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [ADAL](https://azure.microsoft.com/documentation/articles/active-directory-authentication-libraries/) |
| **Steg** | <p>hello Azure AD authentication Library (ADAL) aktiverar klienten programmet utvecklare tooeasily autentisera användare toocloud lokala Active Directory (AD) och sedan hämta åtkomsttoken för att skydda API-anrop.</p><p>ADAL har många funktioner för att kontrollera autentiseringen enklare för utvecklare som asynkron support, en konfigurerbar token-cache som lagrar åtkomst-token och uppdateringstoken, automatisk token uppdatering när en åtkomst-token upphör att gälla och en uppdateringstoken är tillgängliga, och Mer.</p><p>Genom att hantera de flesta av hello komplexitet ADAL hjälpa en utvecklare att fokusera på affärslogiken i sina program och enkelt skydda resurser utan att vara en expert på säkerhet. Separata bibliotek är tillgängliga för .NET, JavaScript (klient och Node.js), iOS, Android och Java.</p>|

## <a id="authn-devices-field"></a>Autentisera enheter som ansluter toohello fältet Gateway

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Fältet för IoT-Gateway | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | Se till att varje enhet autentiseras av hello fältet Gateway innan acceptera data från dem och underlättar överordnade kommunikation med hello Gateway moln. Kontrollera också att enheter ansluter med en per enhet autentiseringsuppgifter så att enskilda enheter kan identifieras unikt.|

## <a id="authn-devices-cloud"></a>Se till att enheter som ansluter tooCloud gateway autentiseras

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Gateway för IoT-moln | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Allmän och C#, Node.JS,  |
| **Attribut**              | Ingen, val av Gateway - Azure IoT-hubb |
| **Referenser**              | Ej tillämpligt, [Azure IoT-hubb med .NET](https://azure.microsoft.com/documentation/articles/iot-hub-csharp-csharp-getstarted/), [komma igång med IoT-hubb och Node JS](https://azure.microsoft.com/documentation/articles/iot-hub-node-node-getstarted), [skydda IoT med SAS och certifikat](https://azure.microsoft.com/documentation/articles/iot-hub-sas-tokens/), [Git-lagringsplats](https://github.com/Azure/azure-iot-sdks/tree/master/node) |
| **Steg** | <ul><li>**Generisk:** autentisera hello-enhet med Transport Layer Security (TLS) eller IPSec. Infrastrukturen ska ha stöd för med i förväg delad nyckel (PSK) på de enheter som inte kan hantera fullständig asymmetrisk kryptering. Använda Azure AD, Oauth.</li><li>**C#:** när du skapar en DeviceClient-instans som standard hello Create-metoden skapar en DeviceClient-instans som använder hello AMQP protokollet toocommunicate med IoT-hubben. toouse hello HTTPS-protokollet använder hello åsidosättning av hello skapandemetoden som gör att du toospecify hello-protokollet. Om du använder hello HTTPS-protokollet bör du också lägga till hello `Microsoft.AspNet.WebApi.Client` NuGet-paketet tooyour projekt tooinclude hello `System.Net.Http.Formatting` namnområde.</li></ul>|

### <a name="example"></a>Exempel
```C#
static DeviceClient deviceClient;

static string deviceKey = "{device key}";
static string iotHubUri = "{iot hub hostname}";

var messageString = "{message in string format}";
var message = new Message(Encoding.ASCII.GetBytes(messageString));

deviceClient = DeviceClient.Create(iotHubUri, new DeviceAuthenticationWithRegistrySymmetricKey("myFirstDevice", deviceKey));

await deviceClient.SendEventAsync(message);
```

### <a name="example"></a>Exempel
**Node.JS: autentisering**
#### <a name="symmetric-key"></a>Symmetrisk nyckel
* Skapa en IoT-hubb i azure
* Skapa en post i hello enhetsidentitetsregistret
    ```javascript
    var device = new iothub.Device(null);
    device.deviceId = <DeviceId >
    registry.create(device, function(err, deviceInfo, res) {})
    ```
* Skapa en simulerad enhet
    ```javascript
    var clientFromConnectionString = require('azure-iot-device-amqp').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    var connectionString = 'HostName=<HostName>DeviceId=<DeviceId>SharedAccessKey=<SharedAccessKey>';
    var client = clientFromConnectionString(connectionString);
    ```
#### <a name="sas-token"></a>SAS-Token
* Hämtar genereras internt när du använder symmetrisk nyckel, men vi kan du skapa och använda det explicit samt
* Definiera ett protokoll:`var Http = require('azure-iot-device-http').Http;`
* Skapa en sas-token:
    ```javascript
    resourceUri = encodeURIComponent(resourceUri.toLowerCase()).toLowerCase();
    var deviceName = "<deviceName >";
    var expires = (Date.now() / 1000) + expiresInMins * 60;
    var toSign = resourceUri + '\n' + expires;
    // using crypto
    var decodedPassword = new Buffer(signingKey, 'base64').toString('binary');
    const hmac = crypto.createHmac('sha256', decodedPassword);
    hmac.update(toSign);
    var base64signature = hmac.digest('base64');
    var base64UriEncoded = encodeURIComponent(base64signature);
    // construct authorization string
    var token = "SharedAccessSignature sr=" + resourceUri + "%2fdevices%2f"+deviceName+"&sig="
    + base64UriEncoded + "&se=" + expires;
    if (policyName) token += "&skn="+policyName;
    return token;
    ```
* Anslut med hjälp av sas-token: 
    ```javascript
    Client.fromSharedAccessSignature(sas, Http); 
    ```
#### <a name="certificates"></a>Certifikat
* Generera ett självsignerat signerade X509 certifikat med hjälp av någon verktyget, till exempel OpenSSL toogenerate en .cert och .key filer toostore hello certifikat och hello nyckel respektive
* Etablera en enhet som accepterar säker anslutning med hjälp av certifikat.
    ```javascript
    var connectionString = '<connectionString>';
    var registry = iothub.Registry.fromConnectionString(connectionString);
    var deviceJSON = {deviceId:"<deviceId>",
    authentication: {
        x509Thumbprint: {
        primaryThumbprint: "<PrimaryThumbprint>",
        secondaryThumbprint: "<SecondaryThumbprint>"
        }
    }}
    var device = deviceJSON;
    registry.create(device, function (err) {});
    ```
* Anslut en enhet som använder ett certifikat
    ```javascript
    var Protocol = require('azure-iot-device-http').Http;
    var Client = require('azure-iot-device').Client;
    var connectionString = 'HostName=<HostName>DeviceId=<DeviceId>x509=true';
    var client = Client.fromConnectionString(connectionString, Protocol);
    var options = {
        key: fs.readFileSync('./key.pem', 'utf8'),
        cert: fs.readFileSync('./server.crt', 'utf8')
    }; 
    // Calling setOptions with hello x509 certificate and key (and optionally, passphrase) will configure hello client //transport toouse x509 when connecting tooIoT Hub
    client.setOptions(options);
    //call fn tooexecute after hello connection is set up
    client.open(fn);
    ```

## <a id="authn-cred"></a>Använd autentiseringsuppgifter för per enhet

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Gateway för IoT-moln  | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Val av gateway - Azure IoT-hubb |
| **Referenser**              | [Azure IoT-hubb säkerhetstoken](https://azure.microsoft.com/documentation/articles/iot-hub-sas-tokens/) |
| **Steg** | Använd per enhet autentiseringsuppgifter med hjälp av SaS-token baserat på enhetsnyckel eller klientcertifikat, i stället för IoT-hubb-nivå delade åtkomstprinciper. Detta förhindrar hello återanvändning av Autentiseringstoken för en gateway för enhet eller fält av en annan |

## <a id="req-containers-anon"></a>Se till att endast hello begärda behållare och blobbar ges anonym läsbehörighet

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Azure Storage | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | StorageType - Blob |
| **Referenser**              | [Hantera anonym läsbehörighet toocontainers och blobbar](https://azure.microsoft.com/documentation/articles/storage-manage-access-to-resources/), [signaturer för delad åtkomst, del 1: Förstå hello SAS-modellen](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/) |
| **Steg** | <p>Som standard kan en behållare och alla blobbar i den endast användas av hello ägare hello storage-konto. toogive anonyma användare behörighet att läsa tooa behållaren och dess blobbar, en uppsättning hello behållaren behörigheter tooallow offentlig åtkomst. Anonyma användare kan läsa blobbar i en behållare som är offentligt tillgänglig utan autentiseras hello-begäran.</p><p>Behållare innehåller följande alternativ för att hantera åtkomst till behållaren hello:</p><ul><li>Fullständig offentlig läsbehörighet: behållare och blob-data kan läsas via anonym begäran. Klienter kan räkna upp blobbar i behållaren hello via anonym begäran, men det går inte att räkna upp behållare i hello storage-konto.</li><li>Offentlig läsbehörighet för blobbar endast: Blob-data i den här behållaren kan läsas via anonym begäran, men behållardata är inte tillgänglig. Klienter kan inte räkna upp blobbar i behållaren hello via anonym begäran</li><li>Ingen offentlig läsbehörighet: behållare och blob-data kan läsas av hello Kontoägare</li></ul><p>Anonym åtkomst är bäst för scenarier där vissa blobbar alltid ska tillgängliga för anonym läsbehörighet. För mer detaljerad kontroll går att skapa en signatur för delad åtkomst, vilket gör att toodelegate begränsad åtkomst med olika behörigheter och under ett angivet tidsintervall. Se till att behållare och blobbar som kan innehålla potentiellt känsliga data, inte ges anonym åtkomst av misstag</p>|

## <a id="limited-access-sas"></a>Bevilja begränsad åtkomst tooobjects i Azure storage med hjälp av SAS eller SAP

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Azure Storage | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas |
| **Referenser**              | [Signaturer för delad åtkomst, del 1: Förstå hello SAS-modellen](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/), [signaturer för delad åtkomst, del 2: skapa och använda en SAS med bloblagring](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/), [hur toodelegate åt tooobjects i ditt konto med hjälp av Signaturer för delad åtkomst och principer för lagrade åtkomst](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_how-to-delegate-access-to-objects-in-your-account-using-shared-access-signatures-and-stored-access-policies) |
| **Steg** | <p>Använda en signatur för delad åtkomst (SAS) är ett kraftfullt sätt toogrant begränsad åtkomst tooobjects i en storage-konto tooother klienter, utan att behöva tooexpose åtkomstnyckel. hello SAS är en URI som omfattar i dess frågeparametrar alla hello information som behövs för autentiserad åtkomst till tooa storage-resursen. tooaccess lagringsresurser med hello SAS hello klienten behöver bara toopass i hello SAS toohello lämplig konstruktor eller metod.</p><p>Du kan använda en SAS när du vill tooprovide åtkomst tooresources i storage-konto tooa klienten som inte är tillförlitliga med hello kontonyckel. Din lagringskontonycklar innehåller både en primär och sekundär nyckel, som båda bevilja administratörsbehörighet tooyour kontot och alla hello resurser i den. Ditt konto toohello möjligheten att skadlig eller bristande öppnas exponering av någon av dina nycklar. Signaturer för delad åtkomst är en säker alternativ som tillåter andra klienter tooread, skriva och ta bort data i ditt lagringskonto enligt toohello behörigheter som du har beviljats och utan behov av hello kontonyckel.</p><p>Om du har en logisk uppsättning parametrar som liknar varje gång är med hjälp av en lagrad åtkomst princip (SAP) en bättre uppfattning. Eftersom med hjälp av en SAS som härletts från en lagrad åtkomstprincip ger dig använda hello möjlighet toorevoke SAS omedelbart, är hello rekommenderad bästa praxis tooalways lagras åtkomstprinciper när det är möjligt.</p>|
