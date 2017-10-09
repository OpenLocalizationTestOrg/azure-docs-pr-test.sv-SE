---
title: "aaaAzure Active Directory-auth - Azure SQL (översikt) | Microsoft Docs"
description: "Mer information om hur toouse Azure Active Directory för autentisering med SQL Database och SQL Data Warehouse"
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
tags: 
ms.assetid: 7e2508a1-347e-4f15-b060-d46602c5ce7e
ms.service: sql-database
ms.custom: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 08/11/2017
ms.author: rickbyh
ms.openlocfilehash: 7a63a162653b65294e11d3fa5bf39c320e742854
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-active-directory-authentication-for-authentication-with-sql-database-or-sql-data-warehouse"></a>Använda Azure Active Directory-autentisering för autentisering med SQL Database eller SQL Data Warehouse
Azure Active Directory-autentisering är en mekanism för anslutning tooMicrosoft Azure SQL Database och [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) med hjälp av identiteter i Azure Active Directory (AD Azure). Med Azure AD-autentisering kan du centralt hantera hello identiteter för databasanvändare och andra Microsoft-tjänster på en central plats. Central ID-hantering och erbjuder en enda plats toomanage databasanvändare och förenklar hantering av behörighet. Hello följande fördelar:

* Det ger ett alternativ tooSQL Server-autentisering.
* Hello ökande mängden av användaridentiteter stoppar över databasservrar.
* Tillåter lösenord rotation i en enda plats
* Kunder kan hantera databasen med hjälp av externa (AAD)-grupper.
* Men du kan inte lagra lösenord genom att aktivera integrerad Windows-autentisering och andra former av autentisering som stöds av Azure Active Directory.
* Azure AD-autentisering används innesluten databas användare tooauthenticate identiteter på hello databasnivå.
* Azure AD stöder tokenbaserad autentisering för program som ansluter tooSQL databas.
* Azure AD authentication stöder AD FS (domänfederation) eller intern användarlösenord autentisering för en lokal Active Directory med Azure utan domänsynkronisering.  
* Azure AD stöder anslutningar från SQL Server Management Studio som använder Active Directory Universal-autentisering, vilket innefattar Multi-Factor Authentication (MFA).  MFA ingår stark autentisering med ett antal alternativ för enkel verifiering – telefonsamtal, textmeddelande, smartkort och PIN-kod eller meddelande i mobilappen. Mer information finns i [SSMS stöd för Azure AD MFA med SQL Database och SQL Data Warehouse](sql-database-ssms-mfa-authentication.md).  

>  [!NOTE]  
>  Ansluter tooSQL Server som körs på en Azure VM inte stöds med ett Azure Active Directory-konto. Använd en domän Active Directory-konto i stället.  

hello konfigurationssteg omfattar följande procedurer tooconfigure hello och använda Azure Active Directory-autentisering.

1. Skapa och fylla i Azure AD.
2. Valfritt: Koppla eller ändra hello active directory som associeras med din Azure-prenumeration.
3. Skapa en Azure Active Directory-administratör för Azure SQL server eller [Azure SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/).
4. Konfigurera klientdatorer.
5. Skapa oberoende databasanvändare i din databas mappas tooAzure AD identiteter.
6. Ansluta tooyour databasen med hjälp av Azure AD identiteter.

> [!NOTE]
> toolearn hur toocreate och fylla i Azure AD och konfigurera Azure AD med Azure SQL Database och SQL Data Warehouse, se [konfigurera Azure AD med Azure SQL Database](sql-database-aad-authentication-configure.md).
>

## <a name="trust-architecture"></a>Förtroendearkitektur
hello följande Översiktsdiagram sammanfattas hello lösningsarkitektur för genom att använda Azure AD med Azure SQL Database. hello gäller samma koncept tooSQL Data Warehouse. toosupport inbyggd användare Azure AD-lösenord, endast hello molndelen och Azure AD-/ Azure SQL Database anses. toosupport federerat autentisering (eller användarlösenord för Windows-autentiseringsuppgifter) krävs hello kommunikation med AD FS-block. hello pilarna kommunikation vägarna.

![diagram för aad-autentisering][1]

hello visar följande diagram hello federation förtroende och värdrelationer som gör att en klient tooconnect tooa databasen genom att skicka en token. hello token autentiseras av en Azure AD och är betrott av hello-databasen. Kund 1 motsvarar ett Azure Active Directory med interna användare eller en Azure AD med externa användare. Kunden 2 representerar en möjlig lösning inklusive importerade användare. i det här exemplet kommer från en extern Azure Active Directory med AD FS som synkroniseras med Azure Active Directory. Det är viktigt toounderstand som har åtkomst till tooa databas med Azure AD-autentisering kräver att hello som värd för prenumerationen är associerad toohello Azure AD. hello måste samma prenumeration vara används toocreate hello SQL Server värd hello Azure SQL Database eller SQL Data Warehouse.

![prenumerationen relation][2]

## <a name="administrator-structure"></a>Administratören struktur
När du använder Azure AD-autentisering, finns det två administratörskonton för hello SQL Database-server. Hej ursprungliga SQL Server-administratören och hello Azure AD-administratör. hello gäller samma koncept tooSQL Data Warehouse. Endast Hej administratör baserat på en Azure AD-kontot kan skapa databasanvändare hello första Azure AD finns i en användardatabas. hello Azure AD-administratörsinloggning kan vara en Azure AD-användare eller en grupp i Azure AD. När Hej administratör är ett gruppkonto, kan den användas av alla gruppmedlemmen så att flera Azure AD-administratörer för hello SQL Server-instansen. Med gruppen som en administratör förstärker hanterbarhet genom att tillåta toocentrally Lägg till och ta bort medlemmar i Azure AD utan att ändra hello användare eller behörighet i SQL-databas. Endast en Azure AD-administratör (en användare eller grupp) kan konfigureras när som helst.

![Admin-struktur][3]

## <a name="permissions"></a>Behörigheter
toocreate nya användare, måste du ha hello `ALTER ANY USER` behörighet i hello-databasen. Hej `ALTER ANY USER` behörighet kan beviljas tooany databasanvändare. Hej `ALTER ANY USER` behörighet också innehas av hello server-administratörskonton och databasanvändare med hello `CONTROL ON DATABASE` eller `ALTER ON DATABASE` behörighet för den här databasen och av medlemmar i hello `db_owner` databasrollen.

toocreate en innesluten databasanvändare i Azure SQL Database eller SQL Data Warehouse, måste du ansluta toohello databasen med hjälp av en Azure AD-identitet. toocreate hello första innesluten databasanvändare, måste du ansluta toohello databasen med hjälp av en Azure AD-administratör (som äger hello hello databas). Detta visas i steg 4 och 5 nedan. Det går bara att någon Azure AD-autentisering om hello Azure AD admin har skapats för Azure SQL Database eller SQL Data Warehouse-server. Om hello Azure Active Directory-administratör har tagits bort från hello-servern, kan befintliga Azure Active Directory-användare som skapade tidigare i SQL Server inte längre att ansluta toohello databasen med hjälp av sina Azure Active Directory-autentiseringsuppgifter.

## <a name="azure-ad-features-and-limitations"></a>Azure AD-funktioner och begränsningar
hello följande medlemmar i Azure AD kan etableras i Azure SQL-server eller SQL Data Warehouse:

* Intern medlemmar: en medlem som skapats i Azure AD i hello-hanterad domän eller i en kund-domän. Mer information finns i [lägga till egna domänen namnet tooAzure AD](../active-directory/active-directory-add-domain.md).
* Federerad domänmedlemmar: en medlem som skapats i Azure AD med en federerad domän. Mer information finns i [Microsoft Azure stöder nu federation med Windows Server Active Directory](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/).
* Importerade medlemmar från andra Azure AD som är medlemmar i en intern eller extern domän.
* Active Directory-grupper skapas som säkerhetsgrupper.

Microsoft-konton (till exempel outlook.com, hotmail.com, live.com) eller andra gästkonton (till exempel gmail.com, yahoo.com) stöds inte. Om du kan logga in för[https://login.live.com](https://login.live.com) använder hello konto och lösenord, och du använder ett Microsoft-konto, vilket inte stöds för Azure AD-autentisering för Azure SQL Database eller Azure SQL Data Warehouse.

## <a name="connecting-using-azure-ad-identities"></a>Ansluta med hjälp av Azure AD identiteter

Azure Active Directory-autentisering stöder följande metoder för anslutande tooa databasen med hjälp av Azure AD identiteter hello:

* Med hjälp av integrerad Windows-autentisering
* Med hjälp av en Azure AD-huvudnamn och ett lösenord
* Med hjälp av autentisering för token

### <a name="additional-considerations"></a>Annat som är bra att tänka på

* tooenhance hanterbarhet, rekommenderar vi du etablerar en dedikerad Azure AD som en administratör.   
* Endast en Azure AD-administratör (en användare eller grupp) kan konfigureras för en Azure SQL-server eller Azure SQL Data Warehouse när som helst.   
* Endast en Azure AD-administratör för SQL Server kan först ansluta toohello Azure SQL server eller Azure SQL Data Warehouse med ett Azure Active Directory-konto. hello Active Directory-administratören kan konfigurera följande Azure AD-databas användare.   
* Vi rekommenderar hello anslutning timeout too30 sekunder.   
* SQL Server 2016 Management Studio och SQL Server Data Tools för Visual Studio 2015 (version 14.0.60311.1April 2016 eller senare) stöder Azure Active Directory-autentisering. (Azure AD-autentisering stöds av hello **.NET Framework-dataprovider för SqlServer**; minst version .NET Framework 4.6). Därför hello nyaste versionerna av dessa verktyg och -datanivåprogram (DAC och .bacpac) kan använda Azure AD-autentisering.   
* [ODBC version 13,1](https://www.microsoft.com/download/details.aspx?id=53339) stöder Azure Active Directory-autentisering men `bcp.exe` kan inte ansluta med Azure Active Directory-autentisering, eftersom den använder en äldre ODBC-provider.   
* `sqlcmd`stöder Azure Active Directory-autentisering från och med version tillgänglig från hello 13,1 [Download Center](http://go.microsoft.com/fwlink/?LinkID=825643).   
* SQL Server Data Tools för Visual Studio 2015 kräver minst hello April 2016-versionen av hello Data Tools (version 14.0.60311.1). Azure AD-användare visas för närvarande inte i SSDT Object Explorer. Som en tillfällig lösning kan visa hello användare i [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).   
* [Microsoft JDBC Driver 6.0 för SQL Server](https://www.microsoft.com/download/details.aspx?id=11774) stöder Azure AD-autentisering. Se även [inställning hello anslutningsegenskaper](https://msdn.microsoft.com/library/ms378988.aspx).   
* PolyBase kan inte autentiseras med hjälp av Azure AD-autentisering.   
* Azure AD-autentisering stöds för SQL-databas av hello Azure-portalen **Importera databas** och **exportera databasen** blad. Importera och exportera med Azure AD-autentisering stöds också från hello PowerShell-kommando.   
* Azure AD-autentisering stöds för SQL Database och SQL Data Warehouse med hjälp av CLI. Mer information finns i [konfigurera och hantera Azure Active Directory-autentisering med SQL Database eller SQL Data Warehouse](sql-database-aad-authentication-configure.md) och [SQL Server - az SQLServer](https://docs.microsoft.com/en-us/cli/azure/sql/server).

## <a name="next-steps"></a>Nästa steg
- toolearn hur toocreate och fylla i Azure AD och konfigurera Azure AD med Azure SQL Database eller Azure SQL Data Warehouse, se [konfigurera och hantera Azure Active Directory-autentisering med SQL Database eller SQL Data Warehouse](sql-database-aad-authentication-configure.md).
- En översikt över åtkomst och kontroll i SQL Database finns i [Åtkomst och kontroll för SQL Database](sql-database-control-access.md).
- En översikt över inloggningar, användare och databasroller i SQL Database finns i [Inloggningar, användare och databasroller](sql-database-manage-logins.md).
- Mer information om huvudkonton finns i [Huvudkonton](https://msdn.microsoft.com/library/ms181127.aspx).
- Mer information om databasroller finns [Databasroller](https://msdn.microsoft.com/library/ms189121.aspx).
- Mer information om brandväggsregler i SQL Database finns [SQL Database-brandväggsregler](sql-database-firewall-configure.md).

<!--Image references-->

[1]: ./media/sql-database-aad-authentication/1aad-auth-diagram.png
[2]: ./media/sql-database-aad-authentication/2subscription-relationship.png
[3]: ./media/sql-database-aad-authentication/3admin-structure.png
[4]: ./media/sql-database-aad-authentication/4select-subscription.png
[5]: ./media/sql-database-aad-authentication/5ad-settings-portal.png
[6]: ./media/sql-database-aad-authentication/6edit-directory-select.png
[7]: ./media/sql-database-aad-authentication/7edit-directory-confirm.png
[8]: ./media/sql-database-aad-authentication/8choose-ad.png
[9]: ./media/sql-database-aad-authentication/9ad-settings.png
[10]: ./media/sql-database-aad-authentication/10choose-admin.png
[11]: ./media/sql-database-aad-authentication/11connect-using-int-auth.png
[12]: ./media/sql-database-aad-authentication/12connect-using-pw-auth.png
[13]: ./media/sql-database-aad-authentication/13connect-to-db.png

