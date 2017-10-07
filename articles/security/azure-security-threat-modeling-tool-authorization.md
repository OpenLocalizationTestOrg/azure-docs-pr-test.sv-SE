---
title: aaaAuthorization - hotet Modeling verktyget - Azure | Microsoft Docs
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
ms.openlocfilehash: 3ea7ae2b46baa8578e574e6006b98dfe172829e7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="security-frame-authorization--mitigations"></a>Säkerhet ram: Auktorisering | Åtgärder 
| Produkter eller tjänster | Artikel |
| --------------- | ------- |
| **Datorn Förtroendegräns** | <ul><li>[Se till att rätt ACL: er är konfigurerade toorestrict obehörig åtkomst toodata på hello-enhet](#acl-restricted-access)</li><li>[Se till att känslig användarspecifika programinnehållet lagras i användarprofil katalog](#sensitive-directory)</li><li>[Se till att hello distribuerat program körs med de lägsta privilegierna](#deployed-privileges)</li></ul> |
| **Webbprogram** | <ul><li>[Tillämpa sekventiella steg ordning vid bearbetning av business logic flöden](#sequential-logic)</li><li>[Implementera hastighet begränsa mekanism tooprevent uppräkning](#rate-enumeration)</li><li>[Se till att rätt behörighet är på plats och principen lägsta privilegier följs](#principle-least-privilege)</li><li>[Företag logik och resursen åtkomst auktoriseringsbeslut ska inte baseras på parametrar för inkommande begäran](#logic-request-parameters)</li><li>[Kontrollera innehållet och resurser är inte enumerable eller är tillgängliga via kraftfullt surfning](#enumerable-browsing)</li></ul> |
| **Databas** | <ul><li>[Se till att minst Privilegierade konton används tooconnect tooDatabase server](#privileged-server)</li><li>[Implementera raden nivå säkerhet RLS tooprevent klienter från att komma åt varandras data](#rls-tenants)</li><li>[Rollen sysadmin får endast ha giltiga nödvändiga användare](#sysadmin-users)</li></ul> |
| **Gateway för IoT-moln** | <ul><li>[Ansluta tooCloud Gateway med lägsta behörighet token](#cloud-least-privileged)</li></ul> |
| **Azure Event Hub** | <ul><li>[Använda en skicka-behörigheter endast SAS-nyckel för att skapa enhetstoken](#sendonly-sas)</li><li>[Använd inte åtkomsttoken som ger direktåtkomst toohello Event Hub](#access-tokens-hub)</li><li>[Ansluta tooEvent hubb med hjälp av SAS-nycklar för att ha hello minsta behörigheter som krävs](#sas-minimum-permissions)</li></ul> |
| **Azure dokumentet DB** | <ul><li>[Använd resource token tooconnect tooDocumentDB när det är möjligt](#resource-docdb)</li></ul> |
| **Azure Förtroendegräns** | <ul><li>[Aktivera detaljerad access management tooAzure prenumerationen med RBAC](#grained-rbac)</li></ul> |
| **Service Fabric-Förtroendegräns** | <ul><li>[Begränsa klientens toocluster åtkomståtgärder med RBAC](#cluster-rbac)</li></ul> |
| **Dynamics CRM** | <ul><li>[Utföra security modellering och använder fältet Level Security behov](#modeling-field)</li></ul> |
| **Dynamics CRM-portalen** | <ul><li>[Utföra security modellering portal och Tänk på att hello säkerhetsmodell för hello portal skiljer sig från hello resten av CRM-konton](#portal-security)</li></ul> |
| **Azure Storage** | <ul><li>[Ger detaljerad behörighet på ett intervall med enheter i Azure-tabellagring](#permission-entities)</li><li>[Aktivera rollbaserad åtkomstkontroll (RBAC) tooAzure storage-konto med Azure Resource Manager](#rbac-azure-manager)</li></ul> |
| **Mobil klient** | <ul><li>[Implementera implicit jailbreak eller rota identifiering](#rooting-detection)</li></ul> |
| **WCF** | <ul><li>[Svag klassreferens i WCF](#weak-class-wcf)</li><li>[Implementera WCF-auktorisering kontroll](#wcf-authz)</li></ul> |
| **Webb-API** | <ul><li>[Implementera rätt mekanism i ASP.NET Web API](#authz-aspnet)</li></ul> |
| **IoT-enhet** | <ul><li>[Utför auktoriseringskontroller i hello enheten om den har stöd för olika åtgärder som kräver olika behörighetsnivåer](#device-permission)</li></ul> |
| **Fältet för IoT-Gateway** | <ul><li>[Utför auktoriseringskontroller i hello fältet Gateway om det har stöd för olika åtgärder som kräver olika behörighetsnivåer](#field-permission)</li></ul> |

## <a id="acl-restricted-access"></a>Se till att rätt ACL: er är konfigurerade toorestrict obehörig åtkomst toodata på hello-enhet

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Datorn Förtroendegräns | 
| **SDL fas**               | Distribution |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | Se till att rätt ACL: er är konfigurerade toorestrict obehörig åtkomst toodata på hello-enhet|

## <a id="sensitive-directory"></a>Se till att känslig användarspecifika programinnehållet lagras i användarprofil katalog

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Datorn Förtroendegräns | 
| **SDL fas**               | Distribution |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | Se till att känslig användarspecifika programinnehåll som lagras i användarprofil katalogen. Detta är tooprevent flera användare av hello datorn från att komma åt varandras data.|

## <a id="deployed-privileges"></a>Se till att hello distribuerat program körs med de lägsta privilegierna

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Datorn Förtroendegräns | 
| **SDL fas**               | Distribution |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | Se till att hello distribuerat program körs med de lägsta privilegierna. |

## <a id="sequential-logic"></a>Tillämpa sekventiella steg ordning vid bearbetning av business logic flöden

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | I ordning tooverify som det här steget körs en äkta användare du vill tooenforce hello programmet tooonly business logic processflödena i sekventiella steg ordning med alla steg som bearbetas i realistiska mänsklig tid och bearbetar inte i rätt ordning, hoppades över anvisningar för snabbt skickats transaktioner eller bearbetades steg från en annan användare.|

## <a id="rate-enumeration"></a>Implementera hastighet begränsa mekanism tooprevent uppräkning

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | Kontrollera att känslig identifierare är slumpmässig. Implementera CAPTCHA-kontroll på anonym sidor. Se till att fel och undantag inte bör avslöjar specifika data|

## <a id="principle-least-privilege"></a>Se till att rätt behörighet är på plats och principen lägsta privilegier följs

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | <p>hello principen innebär att ge ett användarkonto behörighet som är viktiga toothat användare arbete. En säkerhetskopiering användare behöver till exempel inte tooinstall programvara: därför hello säkerhetskopiering användaren har rättigheter endast toorun säkerhetskopiering och backup-relaterade program. Privilegier, till exempel installera ny programvara har blockerats. hello principen gäller även tooa persondator användare som vanligtvis fungerar i ett vanligt användarkonto och öppnar ett konto med privilegier, lösenordsskyddat (det vill säga en superanvändare) endast när hello situationen absolut begär den. </p><p>Den här principen kan också vara tillämpade tooyour-webbprogram. I stället för enbart utifrån rollbaserade autentiseringsmetoder som använder sessioner och, vi i stället vill tooassign behörighet toousers med hjälp av ett system för databas-baserad autentisering. Vi kan fortfarande använda sessioner i ordning tooidentify om hello användaren var inloggad på rätt sätt, endast nu i stället för att tilldela användaren med en viss roll som vi tilldela honom med privilegier tooverify vilka åtgärder som han Privilegierade tooperform på hello system. En stor pro för den här metoden är också, när en användare har toobe färre behörigheter ändringarna tillämpas på hello direkt eftersom hello tilldela inte är beroende hello-session som annars hade tooexpire först.</p>|

## <a id="logic-request-parameters"></a>Företag logik och resursen åtkomst auktoriseringsbeslut ska inte baseras på parametrar för inkommande begäran

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | När du kontrollerar om en användare är begränsade tooreview bearbetas vissa data, hello åtkomst begränsningar bör vara servern. hello användar-ID ska lagras i en sessionsvariabel vid inloggningen och ska använda tooretrieve användardata från hello-databas |

### <a name="example"></a>Exempel
```SQL
SELECT data 
FROM personaldata 
WHERE userID=:id < - session var 
```
Angriparen möjliga kan nu inte säkerhetsförsluten och ändra hello programmet åtgärden eftersom hello identifierare för att hämta hello data hanteras serversidan.

## <a id="enumerable-browsing"></a>Kontrollera innehållet och resurser är inte enumerable eller är tillgängliga via kraftfullt surfning

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webbprogram | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | <p>Känsliga lagras statiska och konfigurationen inte i hello web-rot. För innehåll krävs inte toobe offentliga, tillgång kontroller ska användas eller borttagning av hello innehåll sig själv.</p><p>Dessutom är kraftfullt bläddring vanligtvis kombineras med Brute Force tekniker toogather information genom att försöka tooaccess så många URL: er som möjligt tooenumerate kataloger och filer på en server. Angripare kan söka efter alla variationer av ofta befintliga filer. Ett lösenord för sökning skulle till exempel omfattar filer inklusive psswd.txt, password.htm, password.dat och andra ändringar.</p><p>toomitigate detta funktioner för identifiering av brute force försöker ska tas med.</p>|

## <a id="privileged-server"></a>Se till att minst Privilegierade konton används tooconnect tooDatabase server

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Databas | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [SQL-databas behörigheter hierarkin](https://msdn.microsoft.com/library/ms191465), [objekt för SQL-databas](https://msdn.microsoft.com/library/ms190401) |
| **Steg** | Lägsta behörighet konton ska använda tooconnect toohello databas. Inloggning som programmet ska begränsas i hello-databasen och bör endast köra valda lagrade procedurer. Programmets inloggningen bör ha inte direkt tabellåtkomst. |

## <a id="rls-tenants"></a>Implementera raden nivå säkerhet RLS tooprevent klienter från att komma åt varandras data

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Databas | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | SQL Azure bör OnPrem |
| **Attribut**              | SQL-Version - V12, Version av SQL - MsSQL2016 |
| **Referenser**              | [SQL Server säkerheten på radnivå (RLS)](https://msdn.microsoft.com/library/azure/dn765131.aspx) |
| **Steg** | <p>Säkerhet på radnivå gör det möjligt för kunder toocontrol åtkomst toorows i en databastabell baserat på hello egenskaper för hello användaren kör en fråga (t.ex. gruppen medlemskap eller köra sammanhang).</p><p>Säkerhet på radnivå (RLS) förenklar hello design och kodning av säkerhet i ditt program. RLS kan tooimplement begränsningar för dataåtkomst för raden. Till exempel att säkerställa att anställda kan komma åt de datarader som är relevanta tootheir avdelning eller begränsa kundens data access tooonly hello data relevanta tootheir företag.</p><p>hello åtkomst begränsning logik är finns i hello databasnivå i stället för avstånd från hello data i ett annat program skikt. hello databassystemet gäller hello åtkomstbegränsningar varje gång ett försök gjordes att dataåtkomst från alla skikt. Detta gör hello säkerhetssystem mer tillförlitlig och stabil genom att minska hello ytan på hello säkerhetssystem.</p><p>|

Observera att RLS som en out box databasfunktion är tillämpliga endast tooSQL Server från 2016 och Azure SQL-databas. Om hello out box RLS funktionen inte har implementerats, viktigt att se till att dataåtkomst är begränsad använda vyer och procedurer

## <a id="sysadmin-users"></a>Rollen sysadmin får endast ha giltiga nödvändiga användare

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Databas | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [SQL-databas behörigheter hierarkin](https://msdn.microsoft.com/library/ms191465), [objekt för SQL-databas](https://msdn.microsoft.com/library/ms190401) |
| **Steg** | Medlemmar i hello fasta serverrollen SysAdmin bör vara mycket begränsad och innehåller aldrig konton som används av program.  Granska hello lista över användare i rollen hello och ta bort alla onödiga konton|

## <a id="cloud-least-privileged"></a>Ansluta tooCloud Gateway med lägsta behörighet token

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Gateway för IoT-moln | 
| **SDL fas**               | Distribution |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Val av gateway - Azure IoT-hubb |
| **Referenser**              | [Åtkomstkontroll för IOT-hubb](https://azure.microsoft.com/documentation/articles/iot-hub-devguide/#Security) |
| **Steg** | Ange minsta behörighet behörigheter toovarious komponenter som ansluter tooCloud Gateway (IoT-hubb). Vanliga exempel är – enheten/etablering komponenten använder registryread/skrivning, händelse Processor (ASA) använder tjänsten ansluta. Enskilda enheter ansluter med autentiseringsuppgifter för enhet|

## <a id="sendonly-sas"></a>Använda en skicka-behörigheter endast SAS-nyckel för att skapa enhetstoken

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Azure Event Hub | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [Autentisering och säkerhet modellen översikt över Event Hubs](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Steg** | En SAS-nyckel är används toogenerate enskilda enhetstoken. Använda en behörigheter endast skicka SAS-nyckel när genererar hello enhetstoken för den angivna utgivaren|

## <a id="access-tokens-hub"></a>Använd inte åtkomsttoken som ger direktåtkomst toohello Event Hub

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Azure Event Hub | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [Autentisering och säkerhet modellen översikt över Event Hubs](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Steg** | En token som ger direktåtkomst toohello händelsehubb bör inte ges toohello enhet. Med en mindre privilegierad token för hello-enhet som ger tillgång endast tooa publisher skulle att identifiera och svartlista det om hitta toobe en falsk eller komprometterats enhet.|

## <a id="sas-minimum-permissions"></a>Ansluta tooEvent hubb med hjälp av SAS-nycklar för att ha hello minsta behörigheter som krävs

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Azure Event Hub | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [Autentisering och säkerhet modellen översikt över Event Hubs](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Steg** | Ange minsta behörighet behörigheter toovarious backend-program som ansluter toohello Event Hub. Generera nycklar för separat SAS för varje backend-program och ge endast hello krävs behörigheter - skicka, ta emot eller hantera toothem.|

## <a id="resource-docdb"></a>Använd resource token tooconnect tooCosmos DB när det är möjligt

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Azure dokumentet DB | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | En resurs-token är kopplad till en resurs för DocumentDB-behörighet och registrerar hello förhållandet mellan hello användare för en databas och hello behörighet att användaren har för en specifik DocumentDB programresurs (t.ex. samling och dokument). Använd alltid en resurs token tooaccess hello DocumentDB om hello-klienten inte är betrott med hantering av master eller skrivskyddad nycklar – som ett program för slutanvändare som en mobil- eller datorprogram klient. Använd huvudnyckeln eller skrivskyddade nycklar från backend-program som kan lagra dessa nycklar på ett säkert sätt.|

## <a id="grained-rbac"></a>Aktivera detaljerad access management tooAzure prenumerationen med RBAC

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Azure Förtroendegräns | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [Använda rollen tilldelningar toomanage åtkomst tooyour Azure-prenumerationsresurser](https://azure.microsoft.com/documentation/articles/role-based-access-control-configure/)  |
| **Steg** | Rollbaserad åtkomstkontroll (RBAC) i Azure ger tillgång till ingående åtkomsthantering för Azure. Med RBAC kan bevilja du endast hello mängd åtkomst att användarna måste tooperform sitt arbete.|

## <a id="cluster-rbac"></a>Begränsa klientens toocluster åtkomståtgärder med RBAC

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Service Fabric-Förtroendegräns | 
| **SDL fas**               | Distribution |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Miljö - Azure |
| **Referenser**              | [Rollbaserad åtkomstkontroll för Service Fabric-klienter](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security-roles/) |
| **Steg** | <p>Azure Service Fabric stöder två typer av olika åtkomstkontroll för klienter som är anslutna tooa Service Fabric-kluster: administratörs- och. Åtkomstkontroll kan Hej administratör toolimit åtkomst toocertain klustret klusteråtgärder för olika grupper av användare, göra hello klustret säkrare.</p><p>Administratörer har fullständig åtkomst toomanagement (inklusive funktioner för läsning och skrivning). Användare, har som standard bara läsbehörighet toomanagement funktioner (till exempel frågefunktioner) och hello möjlighet tooresolve program och tjänster.</p><p>Du kan ange hello två klienten roller (administratör och klient) när hello klustret har skapats genom att tillhandahålla olika certifikat för varje.</p>|

## <a id="modeling-field"></a>Utföra security modellering och använder fältet Level Security behov

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Dynamics CRM | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | Utföra security modellering och använder fältet Level Security behov|

## <a id="portal-security"></a>Utföra security modellering portal och Tänk på att hello säkerhetsmodell för hello portal skiljer sig från hello resten av CRM-konton

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Dynamics CRM-portalen | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | Utföra security modellering portal och Tänk på att hello säkerhetsmodell för hello portal skiljer sig från hello resten av CRM-konton|

## <a id="permission-entities"></a>Ger detaljerad behörighet på ett intervall med enheter i Azure-tabellagring

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Azure Storage | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | StorageType - tabell |
| **Referenser**              | [Hur toodelegate åt tooobjects i ditt Azure storage-konto med hjälp av SAS](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_data-plane-security) |
| **Steg** | I vissa affärsscenarier vara Azure Table Storage nödvändiga toostore känsliga data som caters toodifferent parter. Till exempel känsliga data som rör toodifferent länder. I sådana fall kan SAS signaturer konstrueras genom att ange hello partition och raden nyckelintervall, så att en användare kan komma åt data specifika tooa visst land.| 

## <a id="rbac-azure-manager"></a>Aktivera rollbaserad åtkomstkontroll (RBAC) tooAzure storage-konto med Azure Resource Manager

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Azure Storage | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | [Hur toosecure lagringen konto med rollbaserad åtkomstkontroll (RBAC)](https://azure.microsoft.com/documentation/articles/storage-security-guide/#management-plane-security) |
| **Steg** | <p>När du skapar ett nytt lagringskonto, Välj en distributionsmodell för klassisk eller Azure Resource Manager. hello modell för att skapa resurser i Azure kan bara fullständiga åtkomst toohello prenumerationen och i sin tur hello storage-konto.</p><p>Med hello Azure Resource Manager-modellen placera hello storage-konto i en resurs grupp och kontroll åtkomst toohello management plan för den specifika storage-konto med Azure Active Directory. Exempelvis kan du ge specifika användare hello möjlighet tooaccess hello lagringskontonycklar, medan andra användare kan visa information om hello storage-konto, men kan inte komma åt hello lagringskontonycklar.</p>|

## <a id="rooting-detection"></a>Implementera implicit jailbreak eller rota identifiering

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Mobil klient | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | <p>Programmet ska skydda sina egna konfigurations- och data i fallet om telefon är rotad eller jailbroken. Rota/jailbreakad bryter innebär obehörig åtkomst, vilka normal användare inte på sina egna telefoner. Programmet ska därför ha implicit identifieringslogik för start av program, toodetect om hello phone har blivit rotad.</p><p>hello identifieringslogik kan bara komma åt filer som normalt endast rotanvändare kan komma åt, till exempel:</p><ul><li>/system/App/superuser.APK</li><li>/ sbin/su</li><li>/system/bin/su</li><li>/system/xbin/su</li><li>/data/Local/xbin/su</li><li>/data/Local/bin/su</li><li>/system/SD/xbin/su</li><li>/system/bin/Failsafe/su</li><li>/data/Local/su</li></ul><p>Om programmet hello kan komma åt någon av dessa filer, anger att programmet hello körs som rotanvändare.</p>|

## <a id="weak-class-wcf"></a>Svag klassreferens i WCF

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | WCF | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk NET Framework 3 |
| **Attribut**              | Saknas  |
| **Referenser**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [spikning kungariket](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Steg** | <p>hello används en svag klassreferens som gör att en angripare obehörig tooexecute kod. hello program refererar till en användardefinierad klass som inte identifieras unikt. När .NET läser in den här svagt identifierade klassen hello CLR typen inläsaren söker efter hello i hello följande platser i hello angiven ordning:</p><ol><li>Om hello sammansättningen av hello typen är känt hello inläsaren sökningar hello konfigurationsfilen omdirigering platser, GAC, hello aktuella sammansättningen med hjälp av konfigurationsinformation och hello programmets baskatalog</li><li>Om hello sammansättningen är okänt hello hello inläsaren sökningar aktuella sammansättning, mscorlib och hello-plats som returneras av hello TypeResolve händelsehanterare</li><li>Den här CLR-sökordningen kan ändras med hook, till exempel hello vidarebefordran av typen mekanism och hello AppDomain.TypeResolve händelse</li></ol><p>Om en angripare utnyttjar hello CLR Sökordning genom att skapa en annan klass Hej samma namn och att den placeras på en annan plats att hello CLR läser in först, hello CLR körs oavsiktligt hello angripare har angett koden</p>|

### <a name="example"></a>Exempel
Hej `<behaviorExtensions/>` element av hello WCF-konfigurationsfil nedan instruerar WCF tooadd en anpassad klass tooa viss WCF beteendetillägg.
```
<system.serviceModel>
    <extensions>
        <behaviorExtensions>
            <add name=""myBehavior"" type=""MyBehavior"" />
        </behaviorExtensions>
    </extensions>
</system.serviceModel>
```
Med hjälp av fullständiga (stark) namn unikt identifierar en typ och ökar ytterligare för systemets säkerhet. Använd fullständigt kvalificerat sammansättningsnamn när typer i hello machine.config och app.config-filer.

### <a name="example"></a>Exempel
Hej `<behaviorExtensions/>` element av hello WCF-konfigurationsfil nedan instruerar WCF tooadd starkt refereras anpassad klass tooa viss WCF beteendetillägg.
```
<system.serviceModel>
    <extensions>
        <behaviorExtensions>
            <add name=""myBehavior"" type=""Microsoft.ServiceModel.Samples.MyBehaviorSection, MyBehavior,
            Version=1.0.0.0, Culture=neutral, PublicKeyToken=null"" />
        </behaviorExtensions>
    </extensions>
</system.serviceModel>
```

## <a id="wcf-authz"></a>Implementera WCF-auktorisering kontroll

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | WCF | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk NET Framework 3 |
| **Attribut**              | Saknas  |
| **Referenser**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [spikning kungariket](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Steg** | <p>Den här tjänsten används inte av en kontroll för auktorisering. När en klient anropar en viss WCF-tjänst, ger WCF olika för auktorisering som verifierar att hello anroparen har behörighet tooexecute hello webbtjänstmetoden på hello-servern. Om tillståndet kontroller inte är aktiverade för WCF-tjänster, kan en autentiserad användare få eskalering.</p>|

### <a name="example"></a>Exempel
hello följande konfiguration instruerar WCF toonot Kontrollera hello åtkomstnivån för hello klienten när du kör hello-tjänsten:
```
<behaviors>
    <serviceBehaviors>
        <behavior>
            ...
            <serviceAuthorization principalPermissionMode=""None"" />
        </behavior>
    </serviceBehaviors>
</behaviors>
```
Använd en service auktorisering schemat tooverify som hello anroparen av hello webbtjänstmetoden är auktoriserade toodo så. WCF tillhandahåller två lägen och gör hello definitionen av ett schema för anpassad auktorisering. Hej UseWindowsGroups läge använder Windows-roller och användare och hello UseAspNetRoles läge använder en provider för ASP.NET-rollen, till exempel SQL Server, tooauthenticate.

### <a name="example"></a>Exempel
hello instruerar följande konfiguration WCF toomake hello klienten måste ingå i administratörsgruppen för hello innan du kör hello Lägg till tjänsten:
```
<behaviors>
    <serviceBehaviors>
        <behavior>
            ...
            <serviceAuthorization principalPermissionMode=""UseWindowsGroups"" />
        </behavior>
    </serviceBehaviors>
</behaviors>
```
hello-tjänsten har sedan deklarerats som hello följande:
```
[PrincipalPermission(SecurityAction.Demand,
Role = ""Builtin\\Administrators"")]
public double Add(double n1, double n2)
{
double result = n1 + n2;
return result;
}
```

## <a id="authz-aspnet"></a>Implementera rätt mekanism i ASP.NET Web API

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Webb-API | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Allmän och MVC5 |
| **Attribut**              | Ingen, Provider - ADFS, identitet identitetsleverantör - Azure AD |
| **Referenser**              | [Autentisering och auktorisering i ASP.NET webb-API](http://www.asp.net/web-api/overview/security/authentication-and-authorization-in-aspnet-web-api) |
| **Steg** | <p>Rollinformation om för hello programanvändare kan härledas från Azure AD eller AD FS-anspråk om hello program som förlitar sig på dem som identitetsleverantör eller själva hello programmet under förutsättning att den. I alla dessa fall kan ska hello anpassad auktorisering implementering verifiera hello användaren rollen.</p><p>Rollinformation om för hello programanvändare kan härledas från Azure AD eller AD FS-anspråk om hello program som förlitar sig på dem som identitetsleverantör eller själva hello programmet under förutsättning att den. I alla dessa fall kan ska hello anpassad auktorisering implementering verifiera hello användaren rollen.</p>

### <a name="example"></a>Exempel
```C#
[AttributeUsage(AttributeTargets.Class | AttributeTargets.Method, Inherited = true, AllowMultiple = true)]
public class ApiAuthorizeAttribute : System.Web.Http.AuthorizeAttribute
{
        public async override Task OnAuthorizationAsync(HttpActionContext actionContext, CancellationToken cancellationToken)
        {
            if (actionContext == null)
            {
                throw new Exception();
            }

            if (!string.IsNullOrEmpty(base.Roles))
            {
                bool isAuthorized = ValidateRoles(actionContext);
                if (!isAuthorized)
                {
                    HandleUnauthorizedRequest(actionContext);
                }
            }

            base.OnAuthorization(actionContext);
        }

public bool ValidateRoles(actionContext)
{
   //Authorization logic here; returns true or false
}

}
```
Alla hello domänkontrollanter och åtgärdsmetoder som måste tooprotected bör vara dekorerad med ovan attribut.
```C#
[ApiAuthorize]
public class CustomController : ApiController
{
     //Application code goes here
}
```

## <a id="device-permission"></a>Utför auktoriseringskontroller i hello enheten om den har stöd för olika åtgärder som kräver olika behörighetsnivåer

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | IoT-enhet | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | <p>hello enheten ska godkänna hello anroparen toocheck om hello anroparen har begärda hello krävs behörigheter tooperform hello åtgärden. För t.ex. kan hello enheten är säga en Smart Lock dörren som kan övervakas från hello molnet, plus det ger funktioner som att låsa dörren hello via fjärranslutning.</p><p>Hej Smartlock dörren innehåller upplåsning funktion endast när någon fysiskt medföljer nära hello dörren ett kort. I det här fallet bör hello implementering av hello kommando- och utföras så att den inte innehåller några funktioner toounlock hello dörren hello molngateway inte är auktoriserade toosend ett kommando toounlock hello dörren.</p>|

## <a id="field-permission"></a>Utför auktoriseringskontroller i hello fältet Gateway om det har stöd för olika åtgärder som kräver olika behörighetsnivåer

| Rubrik                   | Information      |
| ----------------------- | ------------ |
| **Komponent**               | Fältet för IoT-Gateway | 
| **SDL fas**               | Utveckla |  
| **Tillämpliga tekniker** | Generisk |
| **Attribut**              | Saknas  |
| **Referenser**              | Saknas  |
| **Steg** | hello fältet Gateway ska godkänna hello anroparen toocheck om hello anroparen har begärda hello krävs behörigheter tooperform hello åtgärden. För t.ex. Det måste vara olika behörigheter för administratörsanvändare använda API och interface tooconfigure en fältet gateway v/s-enheter som ansluter tooit.|
