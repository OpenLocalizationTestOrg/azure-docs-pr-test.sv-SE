---
title: aaaSecuring PaaS-databaser i Azure | Microsoft Docs
description: " Läs mer om Azure SQL Database och SQL Data Warehouse Säkerhet Metodtips för att skydda din PaaS webb- och mobilprogram. "
services: security
documentationcenter: na
author: techlake
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2017
ms.author: terrylan
ms.openlocfilehash: a7f9a2846b59dcb05e82f2ad88b41c5213295603
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="securing-paas-databases-in-azure"></a>Att säkra PaaS-databaser i Azure

I den här artikeln tar vi upp en samling [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) och [SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/) säkerhetsmetoder för att skydda din PaaS webb- och mobilprogram. Följande rekommendationer härleds från våra erfarenhet av Azure och hello erfarenheter från kunder som dig själv.

## <a name="azure-sql-database-and-sql-data-warehouse"></a>Azure SQL Database och SQL Data Warehouse
[Azure SQL Database](../sql-database/sql-database-technical-overview.md) och [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) betjäna relationsdatabas för din Internet-baserade program. Nu ska vi titta på tjänster som hjälper dig skydda dina program och data när du använder Azure SQL Database och SQL Data Warehouse i en PaaS-distribution:

- Azure Active Directory-autentisering (i stället för SQL Server-autentisering)
- Azure SQL-brandväggen
- Transparent datakryptering (TDE)

## <a name="best-practices"></a>Bästa praxis

### <a name="use-a-centralized-identity-repository-for-authentication-and-authorization"></a>Använda en centraliserad identitetsdatabas för autentisering och auktorisering

Azure SQL-databaser kan vara konfigurerade toouse en av två typer av autentisering:

- **SQL-autentisering** använder ett användarnamn och lösenord. När du skapade hello logisk server för din databas, anges en ”serveradministratören” inloggning med ett användarnamn och lösenord. Du kan med dessa autentiseringsuppgifter för att autentisera tooany databas på den servern som hello databasägaren.

- **Azure Active Directory-autentisering** använder identiteter som hanteras av Azure Active Directory och stöds för hanterade och integrerad domäner. toouse Azure Active Directory-autentisering måste du skapa en annan serveradministratören kallas hello ”Azure AD admin”, vilket är tillåtet tooadminister Azure AD-användare och grupper. Den här administratören kan också utföra alla åtgärder som en vanlig serveradministratören kan.

[Azure Active Directory-autentisering](../active-directory/develop/active-directory-authentication-scenarios.md) är en mekanism för anslutning tooAzure SQL Database och SQL Data Warehouse med hjälp av identiteter i Azure Active Directory (AD). Azure AD innehåller ett alternativt tooSQL Server-autentisering så du kan stoppa hello ökande mängden av användaridentiteter över databasservrar. Azure AD authentication aktiverar du toocentrally hantera hello identiteter för databasanvändare och andra Microsoft-tjänster på en central plats. Central ID-hantering och erbjuder en enda plats toomanage databasanvändare och förenklar hantering av behörighet.  

Fördelarna med att använda Azure AD-autentisering i stället för SQL-autentisering är:

- Tillåter lösenord rotation i en enda plats.
- Hanterar databasen med hjälp av externa Azure AD-grupper.
- Eliminerar lagra lösenord genom att aktivera integrerad Windows-autentisering och andra former av autentisering som stöds av Azure AD.
- Använder finns databasen användare tooauthenticate identiteter på hello databasnivå.
- Stöder tokenbaserad autentisering för program som ansluter tooSQL databas.
- Har stöd för ADFS (domänfederation) eller intern användarlösenord autentisering för en lokal Azure AD utan domänsynkronisering.
- Stöder anslutningar från SQL Server Management Studio som använder Active Directory Universal-autentisering, vilket innefattar [Multi-Factor Authentication (MFA)](../multi-factor-authentication/multi-factor-authentication.md). MFA ingår stark autentisering med ett antal alternativ för enkel verifiering – telefonsamtal, textmeddelande, smartkort och PIN-kod eller meddelande i mobilappen. Mer information finns i [SSMS stöd för Azure AD MFA med SQL Database och SQL Data Warehouse](../sql-database/sql-database-ssms-mfa-authentication.md).

toolearn mer om Azure AD-autentisering, se:

- [Ansluter tooSQL Database eller SQL Data Warehouse med hjälp av Azure Active Directory Authentication](../sql-database/sql-database-aad-authentication.md)
- [Autentisering tooAzure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-authentication.md)
- [Stöd för tokenbaserad autentisering för Azure SQL-databas med hjälp av Azure AD-autentisering](https://blogs.msdn.microsoft.com/sqlsecurity/2016/02/09/token-based-authentication-support-for-azure-sql-db-using-azure-ad-auth/)

> [!NOTE]
> tooensure att Azure Active Directory passar bra för din miljö finns [Azure AD-funktioner och begränsningar](../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations), särskilt hello ytterligare överväganden.
>
>

### <a name="restrict-access-based-on-ip-address"></a>Begränsa åtkomst baserat på IP-adress
Du kan skapa brandväggsregler som anger godkända IP-adressintervall. Dessa regler kan tillämpas på databasen nivå och hello-server. Vi rekommenderar att du använder brandväggsregler på databasnivå när det är möjligt tooenhance säkerhets- och toomake databasen fler bärbara. Brandväggsregler på servernivå är bäst för administratörer och när du har många databaser som har hello samma behörigheter som krävs, men du inte vill toospend tid konfigurerar varje databas individuellt.

SQL-databas standard källa IP-adressbegränsningar tillåta åtkomst från alla Azure-adresser (inklusive andra prenumerationer och klienter). Du kan begränsa den här tooonly Tillåt din IP-adresser tooaccess hello-instans. Även om din SQL-brandvägg och IP-adressbegränsningar behövs stark autentisering fortfarande. Se hello rekommendationer tidigare i den här artikeln.

toolearn mer om Azure SQL-brandvägg och IP-begränsningar finns:

- [Azure SQL Database-åtkomstkontroll](../sql-database/sql-database-control-access.md)
- [Konfigurera brandväggsregler för Azure SQL Database - översikt](../sql-database/sql-database-firewall-configure.md)
- [Konfigurera en Azure SQL Database servernivå brandväggsregel med hello Azure-portalen](../sql-database/sql-database-configure-firewall-settings.md)

### <a name="encryption-of-data-at-rest"></a>Kryptering av vilande data
[Transparent Data kryptering (TDE)](https://msdn.microsoft.com/library/azure/bb934049) är aktiverad som standard. TDE krypterar transparent SQL Server, Azure SQL Database och Azure SQL Data Warehouse data och loggfilen filer. TDE skyddar mot en kompromettering av direktåtkomst toohello filer eller deras säkerhetskopiering. Detta gör tooencrypt vilande data utan att ändra befintliga program. TDE ska alltid vara aktiverad; Detta kommer dock inte att stoppa en angripare som använder hello normal komma åt sökvägen. TDE ger hello möjlighet toocomply med många lagar, och riktlinjerna i olika branscher.

Azure SQL hanterar viktiga problem för TDE. Som med TDE, lokalt måste vara extra försiktig tooensure återställning och när du flyttar databaser. I mer avancerade scenarier, hello nycklar kan uttryckligen hanteras i Azure Key Vault via extensible key management (se [aktivera TDE på SQL Server med hjälp av EKM](/security/encryption/enable-tde-on-sql-server-using-ekm)). Detta kan också för ta med din egen nyckel (BYOK) via Azure nyckeln valv BYOK kapaciteten.

Azure SQL ger dig kryptering för kolumnerna till [Always Encrypted](/sql/relational-databases/security/encryption/always-encrypted-database-engine). På så sätt kan endast auktoriserade program åtkomst toosensitive kolumner. Med den här typen av kryptering begränsar SQL-frågor för krypterade kolumner tooequality-baserade värden.

Programmet kryptering bör också användas för dataåtkomst. Ibland kan du minimera data suveränitet problem genom att kryptera data med en nyckel som används i hello rätt land. Detta förhindrar att även oavsiktliga dataöverföring orsakar problem Eftersom det är omöjligt toodecrypt hello data utan hello nyckel, förutsatt att en stark algoritm används (till exempel AES 256).

Du kan använda ytterligare säkerhetsåtgärder toohelp säker hello databasen som en säker system utformas, kryptera konfidentiell tillgångar och skapa en brandvägg runt hello databasservrar.

## <a name="next-steps"></a>Nästa steg
Den här artikeln introduceras du tooa samling SQL Database och SQL Data Warehouse säkerhetsmetoder för att skydda din PaaS webb- och mobilprogram. toolearn mer om hur du skyddar dina PaaS-distributioner finns:

- [Skydda PaaS-distributioner](security-paas-deployments.md)
- [Att säkra PaaS-webb- och mobila program med Azure App Service](security-paas-applications-using-app-services.md)
