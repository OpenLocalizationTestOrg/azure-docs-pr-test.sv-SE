---
title: "aaaAzure översikt över säkerheten i SQL-databasen | Microsoft Docs"
description: "Läs mer om Azure SQL Database och SQL Server-säkerhet, inklusive hello skillnaderna mellan hello moln och SQL Server lokalt när det gäller tooauthentication, auktorisering, anslutningssäkerhet, kryptering och kompatibilitet."
services: sql-database
documentationcenter: 
author: tmullaney
manager: jhubbard
editor: 
ms.assetid: a012bb85-7fb4-4fde-a2fc-cf426c0a56bb
ms.service: sql-database
ms.custom: security
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 07/05/2017
ms.author: thmullan;jackr
ms.openlocfilehash: 7b0b0d1b59ec4018d9fb668bc8b6b56c9907e982
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="securing-your-sql-database"></a>Säkra din SQL Database

Den här artikeln beskriver hur hello grunderna i att skydda hello datanivå av ett program med Azure SQL Database. Den här artikeln i synnerhet, hjälper dig att komma igång med resurser för att skydda data, kontrollera åtkomst och proaktiv övervakning. 

En fullständig översikt över säkerhetsfunktionerna som är tillgängliga på alla varianter av SQL Server finns i hello [Security Center för SQL Server Database Engine och Azure SQL Database](https://msdn.microsoft.com/library/bb510589). Mer information finns också i hello [säkerhets- och Azure SQL Database faktablad](https://download.microsoft.com/download/A/C/3/AC305059-2B3F-4B08-9952-34CDCA8115A9/Security_and_Azure_SQL_Database_White_paper.pdf) (PDF).

## <a name="protect-data"></a>Skydda data
SQL Database skyddar dina data genom att tillhandahålla kryptering för data i rörelse med [Transport Layer Security](https://support.microsoft.com/kb/3135244), för vilande data med [transparent datakryptering](http://go.microsoft.com/fwlink/?LinkId=526242) och för data under användning med [alltid krypterad](https://msdn.microsoft.com/library/mt163865.aspx). 

> [!IMPORTANT]
>Alla anslutningar tooAzure SQL-databas Kräv kryptering (SSL/TLS) på alla tider när data är tooand ”under överföring” hello-databasen. I anslutningssträngen för ditt program, måste du ange parametrarna tooencrypt hello anslutning och *inte* tootrust hello servercertifikat (detta görs för du om du kopierar din anslutningssträng ut hello klassiska Azure-portalen) Annars hello anslutningen kommer inte att verifiera hello hello serverns identitet och kommer att utsättas för ”man-in-the-middle”-attacker. För hello ADO.NET drivrutinen exempelvis dessa anslutning string-parametrar är **Encrypt = True** och **TrustServerCertificate = False**. 

För andra sätt tooencrypt Överväg dina data

* [Kryptering på cellnivå](https://msdn.microsoft.com/library/ms179331.aspx) tooencrypt specifika kolumner eller även celler med olika krypteringsnycklar.
* Om du behöver en Hardware Security Module eller central hantering av krypteringsnyckelns hierarki, bör du använda [Azure Key Vault med SQL Server i en virtuell Azure-dator](http://blogs.technet.com/b/kv/archive/2015/01/12/using-the-key-vault-for-sql-server-encryption.aspx).

## <a name="control-access"></a>Styr åtkomst
SQL-databas skyddar dina data genom att begränsa åtkomst tooyour databasen med hjälp av brandväggsregler, autentiseringsmekanismer som kräver användare tooprove deras identitet och auktorisering toodata genom rollbaserad medlemskap och behörigheter, samt via säkerhet på radnivå och dynamisk datamaskning. En beskrivning av hello användning av access control-funktioner i SQL-databas finns [åtkomstkontroll](sql-database-control-access.md).

> [!IMPORTANT]
> Hantering av databaser och logiska servrar inom Azure kontrolleras av din portalanvändarkontos rolltilldelningar. Mer information om det här avsnittet finns i [Rollbaserad åtkomstkontroll i Azure Portal](../active-directory/role-based-access-control-what-is.md).
>

### <a name="firewall-and-firewall-rules"></a>Brandvägg och brandväggsregler
toohelp skydda dina data, brandväggar förhindra all åtkomst tooyour databasserver förrän du anger vilka datorer som har behörigheten med [regler i brandväggen](sql-database-firewall-configure.md). hello brandväggen beviljar åtkomst toodatabases baserat på hello kommer IP-adressen för varje begäran.

### <a name="authentication"></a>Autentisering
SQL-autentisering för databasen refererar toohow du styrka din identitet när du ansluter toohello databas. SQL Database stöder två typer av autentisering:

* **SQL-autentisering**, som använder ett användarnamn och lösenord. När du skapade hello logisk server för din databas, anges en ”serveradministratören” inloggning med ett användarnamn och lösenord. Med dessa autentiseringsuppgifter, kan du autentisera tooany databasen på den servern som hello databasens ägare eller ”dbo”. 
* **Azure Active Directory-autentisering**, som använder identiteter som hanteras av Azure Active Directory och stöder hanterade och integrerade domäner. Använd Active Directory-autentisering (integrerad säkerhet) [närhelst det går](https://msdn.microsoft.com/library/ms144284.aspx). Om du vill toouse Azure Active Directory-autentisering, måste du skapa en annan serveradministratören kallas hello ”Azure AD admin”, vilket är tillåtet tooadminister Azure AD-användare och grupper. Den här administratören kan också utföra alla åtgärder som en vanlig serveradministratören kan. Se [ansluter tooSQL databasen med hjälp av Azure Active Directory Authentication](sql-database-aad-authentication.md) för en genomgång av hur toocreate en Azure AD admin tooenable Azure Active Directory-autentisering.

### <a name="authorization"></a>Auktorisering
Auktorisering refererar toowhat som en användare kan utföra i en Azure SQL Database och kontrolleras av rollmedlemskap för ditt användarkonto databasen och på objektnivå behörigheter. Som bästa praxis bör du bevilja användare hello lägsta behörighet. administratörskonto för hello servern du ansluter med är medlem i db_owner som har myndigheten toodo något hello-databasen. Spara det här kontot för att distribuera schemauppgraderingar och andra hanteringsåtgärder. Använda hello ”ApplicationUser” konto med mer begränsade behörigheter tooconnect från programmet toohello databasen med hello lägsta privilegier som behövs i programmet.

### <a name="row-level-security"></a>Säkerhet på radnivå
Säkerhet på radnivå gör det möjligt för kunder toocontrol åtkomst toorows i en databastabell baserat på hello egenskaper för hello användaren kör en fråga (t.ex. gruppen medlemskap eller köra sammanhang). Mer information finns i [Säkerhet på radnivå](https://msdn.microsoft.com/library/dn765131).

### <a name="data-masking"></a>Datamaskning 
SQL-databas dynamisk datamaskning begränsar exponering av känsliga data genom att kombinera den toonon-Privilegierade användare. Dynamisk datamaskering automatiskt upptäcker potentiellt känsliga data i Azure SQL Database och ger rekommenderade åtgärder toomask dessa fält med minimal påverkan på hello programnivå. Det fungerar genom obfuscating hello känsliga data i hello resultatet av en fråga över avsedda databasfält medan hello data i hello databas inte ändras. Mer information finns i [Kom igång med SQL Database dynamisk datamaskning](sql-database-dynamic-data-masking-get-started.md) kan vara används toolimit exponering av känsliga data.

## <a name="proactive-monitoring"></a>Proaktiv övervakning
SQL Database skyddar dina data genom att tillhandahålla funktioner för granskning och hotidentifiering. 

### <a name="auditing"></a>Granskning
SQL Database Auditing spårar databasaktiviteter och hjälper dig att toomaintain regelefterlevnad, genom att registrera databasen händelser tooan granskningslogg i ditt Azure Storage-konto. Granskning kan du toounderstand pågående databasaktiviteter samt analysera och undersök historiska aktiviteten tooidentify potentiella hot eller misstänkta överträdelser av missbruk och säkerhet. Ytterligare information finns i [Kom igång med SQL Database-granskning](sql-database-auditing.md).  

### <a name="threat-detection"></a>Hotidentifiering
Hotidentifiering kompletterar granskning genom att tillhandahålla ett extra lager av säkerhet för tillgångsinformation är inbyggda i hello Azure SQL Database-tjänsten som identifierar onormal och potentiellt skadliga försök tooaccess eller utnyttja databaser. Du får ett meddelande om misstänkta aktiviteter, potentiella säkerhetsproblem och SQL injection attacker samt avvikande databasen åtkomstmönster. Hotidentifieringsaviseringar kan visas från [Azure Security Center](https://azure.microsoft.com/services/security-center/) och ange information om misstänkt aktivitet och rekommenderar åtgärd på hur tooinvestigate och minimera hotet hello. Hotidentifiering kostnader $15/server/månad. Det är fritt för hello första 60 dagarna. Mer information finns i [Kom igång med SQL Database Threat Detection](sql-database-threat-detection.md).
 
### <a name="data-masking"></a>Datamaskning 
SQL-databas dynamisk datamaskning begränsar exponering av känsliga data genom att kombinera den toonon-Privilegierade användare. Dynamisk datamaskering automatiskt upptäcker potentiellt känsliga data i Azure SQL Database och ger rekommenderade åtgärder toomask dessa fält med minimal påverkan på hello programnivå. Det fungerar genom obfuscating hello känsliga data i hello resultatet av en fråga över avsedda databasfält medan hello data i hello databas inte ändras. Mer information, se komma igång med [SQL-databas dynamisk datamaskning](sql-database-dynamic-data-masking-get-started.md)
 
## <a name="compliance"></a>Efterlevnad
Dessutom deltar i reguljära granskningar toohello ovan egenskaper och funktioner som kan hjälpa ditt program också uppfylla olika säkerhetskrav, Azure SQL Database och har certifierats mot ett antal efterlevnadsstandarder. Mer information finns i hello [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/), där du kan hitta hello mest aktuella listan över [SQL-databas efterlevnad certifieringar](https://azure.microsoft.com/support/trust-center/services/).

## <a name="next-steps"></a>Nästa steg

- En beskrivning av hello användning av access control-funktioner i SQL-databas finns [åtkomstkontroll](sql-database-control-access.md).
- En beskrivning av databasen granskning finns [SQL Database auditing](sql-database-auditing.md).
- En beskrivning av hotidentifiering finns [SQL-databas hotidentifiering](sql-database-threat-detection.md).
