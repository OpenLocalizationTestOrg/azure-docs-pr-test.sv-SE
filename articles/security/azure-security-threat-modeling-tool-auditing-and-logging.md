---
title: aaaAuditing och loggning - hotet Modeling verktyget - Azure | Microsoft Docs
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
ms.openlocfilehash: f28988833eba251b6ceb8bbd47cde37e871af609
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="security-frame-auditing-and-logging--mitigations"></a>Säkerhet ram: Granskning och loggning | Åtgärder 
| Produkter eller tjänster | Artikel |
| --------------- | ------- |
| **Dynamics CRM**    | <ul><li>[Identifiera känsliga enheter i din lösning och implementera granskning av ändringar](#sensitive-entities)</li></ul> |
| **Webbprogram** | <ul><li>[Se till att granskning och loggning är aktiverad för programmet hello](#auditing)</li><li>[Se till att loggrotationen och uppdelning](#log-rotation)</li><li>[Kontrollera att programmet hello inte logga känslig information](#log-sensitive-data)</li><li>[Se till att granskning och loggfiler har begränsad åtkomst](#log-restricted-access)</li><li>[Se till att användaren Management händelser som loggas](#user-management)</li><li>[Kontrollera att systemet hello inbyggda försvar mot missbruk](#inbuilt-defenses)</li><li>[Aktivera diagnostikloggning för web apps i Azure App Service](#diagnostics-logging)</li></ul> |
| **Databas** | <ul><li>[Kontrollera att inloggningen granskning är aktiverat på SQL Server](#identify-sensitive-entities)</li><li>[Aktivera hotidentifiering på Azure SQL](#threat-detection)</li></ul> |
| **Azure Storage** | <ul><li>[Använd Azure Storage Analytics tooaudit åtkomst av Azure Storage](#analytics)</li></ul> |
| **WCF** | <ul><li>[Implementera tillräcklig loggning](#sufficient-logging)</li><li>[Implementera hantering av tillräcklig Audit fel](#audit-failure-handling)</li></ul> |
| **Webb-API** | <ul><li>[Se till att granskning och loggning är aktiverad för webb-API](#logging-web-api)</li></ul> |
| **Fältet för IoT-Gateway** | <ul><li>[Se till att lämpliga granskning och loggning är aktiverad för fältet Gateway](#logging-field-gateway)</li></ul> |
| **Gateway för IoT-moln** | <ul><li>[Se till att lämpliga granskning och loggning är aktiverad för Gateway för moln](#logging-cloud-gateway)</li></ul> |

## <a id="sensitive-entities"></a>Identifiera känsliga enheter i din lösning och implementera granskning av ändringar

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Dynamics CRM | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg**                   | Identifiera enheter i din lösning som innehåller känsliga data och implementera granskning av ändringar på de entiteter och fält |

## <a id="auditing"></a>Se till att granskning och loggning är aktiverad för programmet hello

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg**                   | Aktivera granskning och loggning på alla komponenter. Granskningsloggar ska avbilda användarkontext. Identifiera alla viktiga händelser och loggar dessa händelser. Implementera centraliserad loggning |

## <a id="log-rotation"></a>Se till att loggrotationen och uppdelning

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg**                   | <p>Loggrotationen är en automatiserad process som används i systemadministration datum loggfiler arkiveras. Servrar som kör stora program ofta logga varje begäran: loggrotationen är ett sätt toolimit hello i hello min skrymmande loggar, total storlek hello loggar samtidigt som analys av nya händelser. </p><p>Loggen uppdelning i princip betyder att du har toostore loggfilerna på en annan partition som där OS/tillämpningsprogrammet körs på i ordning tooavert DOS-attacker attacker eller hello nedgradera programmets prestanda</p>|

## <a id="log-sensitive-data"></a>Kontrollera att programmet hello inte logga känslig information

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg**                   | <p>Kontrollera att du inte loggar några känsliga data som en användare skickar tooyour plats. Sök efter avsiktliga loggning samt sidoeffekter på grund av problem med design. Exempel på känsliga data:</p><ul><li>Användarens autentiseringsuppgifter</li><li>Personnummer eller annan information</li><li>Kreditkortsnummer eller andra ekonomiska uppgifter</li><li>Information om hälsa</li><li>Privata nycklar eller andra data som kan använda toodecrypt krypterad information</li><li>System- eller information som kan användas för toomore effektivt angrepp hello program</li></ul>|

## <a id="log-restricted-access"></a>Se till att granskning och loggfiler har begränsad åtkomst

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg**                   | <p>Kontrollera tooensure komma åt rättigheter toolog filer konfigureras på rätt sätt. Programmet konton måste ha lässkyddad åtkomst och operatörer och supporttekniker ska ha läsåtkomst efter behov.</p><p>Administratörer konton är hello endast konton som ska ha fullständig åtkomst. Kontrollera Windows ACL på loggen filer tooensure är korrekt begränsad:</p><ul><li>Konton som programmet ska ha lässkyddad åtkomst</li><li>Operatorer och supporttekniker ska ha läsåtkomst efter behov</li><li>Administratörer är hello endast konton som ska ha fullständig åtkomst</li></ul>|

## <a id="user-management"></a>Se till att användaren Management händelser som loggas

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg**                   | <p>Se till att programmet hello övervakar användaren management händelser, till exempel lyckade och misslyckade användarinloggningar återställning av lösenord, lösenordsändringar, kontoutelåsning, användarregistrering. Detta hjälper toodetect och reagera toopotentially misstänkt beteende. Det gör också att toogather operations data. till exempel tootrack som har åtkomst till programmet hello</p>|

## <a id="inbuilt-defenses"></a>Kontrollera att systemet hello inbyggda försvar mot missbruk

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg**                   | <p>Kontroller ska finnas som utlösa säkerhetsundantag vid missbruk av programmet. T.ex. om verifiering av indata är på plats och en angripare försöker tooinject skadlig kod som inte matchar hello regex, kan ett säkerhetsundantag uppkomma som kan vara ett preliminärt system missbrukas</p><p>Exempelvis bör toohave säkerhetsundantag loggas och åtgärder som vidtas för hello följande problem:</p><ul><li>Verifiering av indata</li><li>CSRF överträdelser</li><li>Brute force (övre gräns för antalet begäranden per användare och resursen)</li><li>Överför filöverträdelser</li><ul>|

## <a id="diagnostics-logging"></a>Aktivera diagnostikloggning för web apps i Azure App Service

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | EnvironmentType – Azure |
| **Referenser**              | Saknas  |
| **Steg** | <p>Azure tillhandahåller inbyggd diagnostik tooassist med felsökning av en Apptjänst-webbapp. Det gäller även tooAPI apps och mobilappar. App Service web apps ange diagnostikfunktion för att logga information från både hello webbservern och hello webbprogram.</p><p>Dessa är logiskt indelade i web serverdiagnostik- och programdiagnostik</p>|

## <a id="identify-sensitive-entities"></a>Kontrollera att inloggningen granskning är aktiverat på SQL Server

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Databas | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [Konfigurera granskning för inloggning](https://msdn.microsoft.com/library/ms175850.aspx) |
| **Steg** | <p>Granskning för databasen Server inloggning måste vara aktiverat toodetect/Bekräfta lösenord för att gissa attacker. Det är viktigt toocapture misslyckade inloggningsförsök. Samla in både lyckade och misslyckade inloggningsförsök ger extra förmån vid kriminaltekniska undersökningar</p>|

## <a id="threat-detection"></a>Aktivera hotidentifiering på Azure SQL

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Databas | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | SQL Azure |
| **Attribut**              | Versionen av SQL - V12 |
| **Referenser**              | [Kom igång med Hotidentifiering för SQL-databas](https://azure.microsoft.com/documentation/articles/sql-database-threat-detection-get-started/)|
| **Steg** |<p>Hotidentifiering identifierar avvikande databasaktiviteter som indikerar potentiella hot toohello databasen. Det ger ett nytt lager av säkerhet som gör att kunder toodetect och svarar toopotential hot som de sker genom att tillhandahålla säkerhetsaviseringar på avvikande aktiviteter.</p><p>Användare kan utforska hello misstänkta händelser med hjälp av Azure SQL Database Auditing toodetermine om de kommer från en försök tooaccess bryta mot eller utnyttja data i hello-databas.</p><p>Hotidentifiering gör det enkelt tooaddress potentiella hot toohello databas utan hello måste toobe en säkerhetsexpert på eller hantera avancerade system för säkerhetsövervakning</p>|

## <a id="analytics"></a>Använd Azure Storage Analytics tooaudit åtkomst av Azure Storage

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Azure Storage | 
| **SDL fas**               | Distribution |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas |
| **Referenser**              | [Med Storage Analytics toomonitor auktorisering typen](https://azure.microsoft.com/documentation/articles/storage-security-guide/#storage-analytics) |
| **Steg** | <p>För varje lagringskontot en aktivera loggning för Azure Storage Analytics tooperform och lagra data för mått. hello storage analytics loggar innehåller viktig information, till exempel autentiseringsmetod som används av någon när de har åtkomst till lagring.</p><p>Det kan vara mycket användbart om du nära skyddar åtkomst toostorage. Till exempel i Blob Storage kan du ange alla hello behållare tooprivate och implementera hello användning av en SAS-tjänst i dina program. Du kan kontrollera hello loggar regelbundet toosee om dina blobbar används med hello lagringskontonycklar, vilket kan tyda på ett sekretessbrott, eller om hello blobbar är offentlig men de får inte vara.</p>|

## <a id="sufficient-logging"></a>Implementera tillräcklig loggning

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | WCF | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | .NET framework |
| **Attribut**              | Saknas  |
| **Referenser**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [spikning kungariket](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Steg** | <p>hello bristen på en korrekt verifieringskedja efter en säkerhetsincident kan hindra kriminaltekniska arbete. Windows Communication Foundation (WCF) erbjuder hello möjlighet toolog lyckade eller misslyckade autentiseringsförsök.</p><p>Loggning av misslyckade autentiseringsförsök Varna administratörer för potentiella brute force-attacker. På liknande sätt kan loggning av händelser för lyckad autentisering ge en användbar verifieringskedja när legitima konto äventyras. Aktivera granskningsfunktionen i WCF'S service säkerhet |

### <a name="example"></a>Exempel
hello följande är ett exempel på konfiguration när granskning är aktiverat
```
<system.serviceModel>
    <behaviors>
        <serviceBehaviors>
            <behavior name=""NewBehavior"">
                <serviceSecurityAudit auditLogLocation=""Default""
                suppressAuditFailure=""false"" 
                serviceAuthorizationAuditLevel=""SuccessAndFailure""
                messageAuthenticationAuditLevel=""SuccessAndFailure"" />
                ...
            </behavior>
        </servicebehaviors>
    </behaviors>
</system.serviceModel>
```

## <a id="audit-failure-handling"></a>Implementera hantering av tillräcklig Audit fel

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | WCF | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | .NET framework |
| **Attribut**              | Saknas  |
| **Referenser**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [spikning kungariket](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Steg** | <p>Utvecklade lösningen konfigureras inte toogenerate ett undantag när granskningsloggen för toowrite tooan misslyckas. Om WCF konfigureras inte toothrow ett undantag när det är toowrite tooan granskningsloggen, hello program meddelas inte om hello fel och granskning av kritiska säkerhetshändelser kanske inte upptäcks.</p>|

### <a name="example"></a>Exempel
Hej `<behavior/>` element av hello WCF-konfigurationsfil nedan instruerar WCF toonot meddela hello programmet när WCF misslyckas toowrite tooan granskningslogg.
````
<behaviors>
    <serviceBehaviors>
        <behavior name="NewBehavior">
            <serviceSecurityAudit auditLogLocation="Application"
            suppressAuditFailure="true"
            serviceAuthorizationAuditLevel="Success"
            messageAuthenticationAuditLevel="Success" />
        </behavior>
    </serviceBehaviors>
</behaviors>
````
Konfigurera WCF toonotify hello programmet när det är toowrite tooan granskningslogg. hello programmet måste ha ett schema för alternativa notification plats tooalert hello organisation att granskningshistorik som inte bevaras. 

## <a id="logging-web-api"></a>Se till att granskning och loggning är aktiverad för webb-API

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webb-API | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | Aktivera granskning och loggning på webb-API: er. Granskningsloggar ska avbilda användarkontext. Identifiera alla viktiga händelser och loggar dessa händelser. Implementera centraliserad loggning |

## <a id="logging-field-gateway"></a>Se till att lämpliga granskning och loggning är aktiverad för fältet Gateway

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Fältet för IoT-Gateway | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | <p>När flera enheter ansluter tooa fältet Gateway, kontrollera att anslutningsförsök autentiseringsstatus (lyckade eller misslyckade) för enskilda enheter som är inloggad och underhålls på hello fältet Gateway.</p><p>Även i fall där fältet Gateway för att underhålla hello IoT-hubb autentiseringsuppgifter för enskilda enheter, se till att granskning utförs när autentiseringsuppgifterna har hämtats. Utveckla en process tooperiodically överför hello loggar tooAzure IoT-hubb-lagring för långsiktig kvarhållning.</p> |

## <a id="logging-cloud-gateway"></a>Se till att lämpliga granskning och loggning är aktiverad för Gateway för moln

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Gateway för IoT-moln | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [Introduktion tooIoT hubb operations övervakning](https://azure.microsoft.com/documentation/articles/iot-hub-operations-monitoring/) |
| **Steg** | <p>Design för att samla in och lagra granskningsdata som samlas in genom övervakning för IoT-hubb åtgärder. Aktivera hello följande övervakning kategorier:</p><ul><li>Enhetens identitet åtgärder</li><li>Enhet till moln kommunikation</li><li>Moln till enhet kommunikation</li><li>Anslutningar</li><li>Filöverföringar</li></ul>|
