---
title: aaaAuthentication tooAzure SQL Data Warehouse | Microsoft Docs
description: Azure Active Directory (AAD) och SQL Server-autentisering tooAzure SQL Data Warehouse.
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: jhubbard
editor: 
tags: 
ms.assetid: fefaaa75-2d0c-4e5d-aadb-410342d1ad73
ms.service: sql-data-warehouse
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.custom: security
ms.date: 03/21/2017
ms.author: rortloff;barbkess
ms.openlocfilehash: 66673ce1d4761243755254c8f64de1078a0ea82e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="authentication-tooazure-sql-data-warehouse"></a>Autentisering tooAzure SQL Data Warehouse
> [!div class="op_single_selector"]
> * [Översikt över säkerheten](sql-data-warehouse-overview-manage-security.md)
> * [Autentisering](sql-data-warehouse-authentication.md)
> * [Kryptering (Portal)](sql-data-warehouse-encryption-tde.md)
> * [Kryptering (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

tooconnect tooSQL Data Warehouse, du måste ange säkerhetsreferenser för autentisering. Vissa inställningar är konfigurerade som en del av upprätta sessionen frågan när en anslutning.  

Mer information om säkerhet och hur tooenable anslutningar tooyour datalagret finns [skydda en databas i SQL Data Warehouse][Secure a database in SQL Data Warehouse].

## <a name="sql-authentication"></a>SQL-autentisering
tooconnect tooSQL datalagret, måste du ange hello följande information:

* Fullständigt kvalificerade servernamn
* Ange SQL-autentisering
* Användarnamn
* Lösenord
* Standarddatabasen (valfritt)

Din anslutning ansluter som standard toohello *master* databasen och inte databasen. tooconnect tooyour användardatabas, du kan välja toodo något av följande:

* Ange hello standarddatabasen när du registrerar din server med hello SQL Server Object Explorer i SSDT SSMS, eller i anslutningssträngen program. Till exempel omfattar hello InitialCatalog parameter för en ODBC-anslutning.
* Markera hello-databasen innan du skapar en session i SSDT.

> [!NOTE]
> hello Transact-SQL-instruktionen **Använd mindatabas;** stöds inte för att ändra hello databasen för en anslutning. Riktlinjer som ansluter tooSQL Data Warehouse med SSDT finns toohello [fråga med Visual Studio] [ Query with Visual Studio] artikel.
> 
> 

## <a name="azure-active-directory-aad-authentication"></a>Azure Active Directory (AAD)-autentisering
[Azure Active Directory] [ What is Azure Active Directory] autentisering är en mekanism för anslutning tooMicrosoft Azure SQL Data Warehouse med hjälp av identiteter i Azure Active Directory (AD Azure). Med Azure Active Directory-autentisering, kan du centralt hantera hello identiteter för databasanvändare och andra Microsoft-tjänster på en central plats. Central ID-hantering och erbjuder en enda plats toomanage SQL Data Warehouse användare och förenklar hantering av behörighet. 

### <a name="benefits"></a>Fördelar
Azure fördelar Active Directory:

* Ger en alternativ tooSQL Server-autentisering.
* Hello ökande mängden av användaridentiteter stoppar över databasservrar.
* Tillåter lösenord rotation i en enda plats
* Hantera databasbehörigheter använder externa (AAD)-grupper.
* Eliminerar lagra lösenord genom att aktivera integrerad Windows-autentisering och andra former av autentisering som stöds av Azure Active Directory.
* Använder finns databasen användare tooauthenticate identiteter på hello databasnivå.
* Stöder tokenbaserad autentisering för program som ansluter tooSQL Data Warehouse.
* Stöder multifaktorautentisering via Active Directory Universal autentisering för SQL Server Management Studio. En beskrivning av Multi-Factor Authentication finns [SSMS stöd för Azure AD MFA med SQL Database och SQL Data Warehouse](../sql-database/sql-database-ssms-mfa-authentication.md).

> [!NOTE]
> Azure Active Directory är fortfarande relativt nytt och har vissa begränsningar. tooensure att Azure Active Directory passar bra för din miljö finns [Azure AD-funktioner och begränsningar][Azure AD features and limitations], särskilt hello ytterligare överväganden.
> 
> 

### <a name="configuration-steps"></a>Konfigurationssteg
Följ dessa steg tooconfigure Azure Active Directory-autentisering.

1. Skapa och fylla i ett Azure Active Directory
2. Valfritt: Koppla eller ändra hello active directory som associeras med din Azure-prenumeration
3. Skapa en Azure Active Directory-administratör för Azure SQL Data Warehouse.
4. Konfigurera klientdatorer
5. Skapa oberoende databasanvändare i din databas mappas tooAzure AD identiteter
6. Ansluta tooyour data warehouse med hjälp av Azure AD identiteter

Azure Active Directory-användare visas för närvarande inte i SSDT Object Explorer. Som en tillfällig lösning kan visa hello användare i [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).

### <a name="find-hello-details"></a>Hello-information
* hello steg tooconfigure och använda Azure Active Directory-autentisering är nästan identisk för Azure SQL Database och Azure SQL Data Warehouse. Följ hello detaljerade anvisningarna i avsnittet om hello [ansluta tooSQL Database eller SQL Data Warehouse med hjälp av Azure Active Directory Authentication](../sql-database/sql-database-aad-authentication.md).
* Skapa anpassade databasroller och Lägg till användare toohello roller. Ge detaljerade behörigheter toohello roller. Mer information finns i [komma igång med motorn databasbehörighet](https://msdn.microsoft.com/library/mt667986.aspx).

## <a name="next-steps"></a>Nästa steg
toostart fråga ditt data warehouse med Visual Studio och andra program, se [fråga med Visual Studio][Query with Visual Studio].

<!-- Article references -->
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md
[Query with Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[What is Azure Active Directory]: ../active-directory/active-directory-whatis.md
[Azure AD features and limitations]: ../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations
