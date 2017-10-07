---
title: aaaMulti Multi-Factor authentication - Azure SQL | Microsoft Docs
description: "Använda flera Inberäknade autentisering med SSMS för SQL Database och SQL Data Warehouse."
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
tags: 
ms.assetid: fbd6e644-0520-439c-8304-2e4fb6d6eb91
ms.service: sql-database
ms.custom: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/19/2017
ms.author: rickbyh
ms.openlocfilehash: 57ef63b7c7f2c5cf64c3e1ee194d865ee5b14177
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="universal-authentication-with-sql-database-and-sql-data-warehouse-ssms-support-for-mfa"></a>Universal autentisering med SQL Database och SQL Data Warehouse (SSMS stöd för MFA)
Azure SQL Database och Azure SQL Data Warehouse stöder anslutningar från SQL Server Management Studio (SSMS) med hjälp av *Active Directory Universal autentisering*. 
**Hämta hello senaste SSMS** - hello klientdator, hämta hello senaste versionen av SSMS, från [Hämta SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx). För alla hello-funktioner i det här avsnittet, använda minst version 17,2 juli 2017.  hello senaste dialogrutan anslutning ser ut så här: ![1mfa universal ansluta](./media/sql-database-ssms-mfa-auth/1mfa-universal-connect.png "fyller hello användarnamnsrutan.")  

## <a name="hello-five-authentication-options"></a>hello fem autentiseringsalternativ  
- Active Directory Universal autentisering stöder hello två icke-interaktiv autentiseringsmetoder (`Active Directory - Password` autentisering och `Active Directory - Integrated` autentisering). Icke-interaktiv `Active Directory - Password` och `Active Directory - Integrated` autentiseringsmetoder kan användas i många olika program (ADO.NET, JDBC, ODBC, etc.). Dessa två metoder innebär aldrig popup-dialogrutor.

- `Active Directory - Universal with MFA`autentisering är en interaktiv metod som också stöder *Azure Multi-Factor Authentication* (MFA). Azure MFA hjälper dig att skydda åtkomst toodata och program och uppfyller efterfrågan från användarna för en process för enkel inloggning. Den ger stark autentisering med olika alternativ för enkel verifiering (telefonsamtal, textmeddelande, smartkort och PIN-kod eller mobilapp), vilket gör att användare toochoose hello metod. Interaktiv MFA med Azure AD kan resultera i en dialogruta för verifiering.

En beskrivning av Multi-Factor Authentication finns [Multifaktorautentisering](../multi-factor-authentication/multi-factor-authentication.md).
Konfigurationssteg finns [konfigurera Azure SQL Database Multi-Factor authentication för SQL Server Management Studio](sql-database-ssms-mfa-authentication-configure.md).

### <a name="azure-ad-domain-name-or-tenant-id-parameter"></a>Azure AD domain name eller klient ID-parametern   

Från och med [SSMS version 17](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms), användare som har importerats till hello aktuell Active Directory från andra Azure Active kataloger som gästanvändare, kan tillhandahålla hello Azure AD-domännamn eller klient-ID när de ansluter. Gästanvändare innehåller användare som bjudits in från andra Azure Active Directory, Microsoft-konton som outlook.com, hotmail.com, live.com eller andra konton som gmail.com. Den här informationen kan **Active Directory Universal med MFA autentisering** tooidentify hello rätt autentisera utfärdare. Det här alternativet är också nödvändig toosupport Microsoft-konton (MSA), till exempel outlook.com, hotmail.com, live.com eller andra mejlkonton än Microsoft-konton. Dessa användare som vill toobe autentiseras med Universal autentisering måste ange sina Azure AD-domännamnet eller klient-ID. Den här parametern representerar hello aktuella Azure AD domain name-klient-ID hello Azure-Server som är kopplad till. Om Azure-Server som är associerad med Azure AD-domänen till exempel `contosotest.onmicrosoft.com` där användaren `joe@contosodev.onmicrosoft.com` värdbaserad som en importerad användare från Azure AD-domänen `contosodev.onmicrosoft.com`, hello domän måste ange namnet på tooauthenticate som den här användaren är `contosotest.onmicrosoft.com`. När hello användare är en inbyggd hello Azure AD länkade tooAzure Server-användare och är inte en MSA-konto, krävs ingen domän namn eller klient-ID. tooenter hello parametern (som börjar med SSMS version 17,2), i hello **ansluta tooDatabase** dialogrutan, fullständig hello dialogrutan, välja **Active Directory - Universal med MFA** -autentisering Klicka på **alternativ**, fullständig hello **användarnamn** rutan och klicka på hello **anslutningsegenskaper** fliken. Kontrollera hello **AD domän namn eller klient-ID** rutan och ange autentiseringsutfärdare saknas, till exempel hello domännamn (**contosotest.onmicrosoft.com**) eller hello GUID för hello klient-ID.  
   ![mfa-klient-ssms](./media/sql-database-ssms-mfa-auth/mfa-tenant-ssms.png)   

### <a name="azure-ad-business-toobusiness-support"></a>Stöd för Azure AD business toobusiness   
Azure AD-användare som stöds i Azure AD B2B-scenarier som gästanvändare (se [vad är Azure B2B-samarbete](../active-directory/active-directory-b2b-what-is-azure-ad-b2b.md)) kan ansluta tooSQL databas och SQL Data Warehouse endast som en del av medlemmar i en grupp som skapats i aktuell Azure AD och mappa manuellt med hjälp av hello Transact-SQL `CREATE USER` instruktionen i en viss databas. Till exempel om `steve@gmail.com` är inbjudna tooAzure AD `contosotest` (med hello Azure Ad domain `contosotest.onmicrosoft.com`), en Azure AD gruppen som `usergroup` måste skapas i hello Azure AD som innehåller hello `steve@gmail.com` medlem. Sedan den här gruppen måste skapas för en viss databas (d.v.s. mindatabas) av Azure AD-SQL-administratör eller Azure AD DBO genom att köra Transact-SQL `CREATE USER [usergroup] FROM EXTERNAL PROVIDER` instruktionen. När du har skapat hello databasanvändare sedan hello användaren `steve@gmail.com` kan logga in för`MyDatabase` med hello SSMS autentiseringsalternativet `Active Directory – Universal with MFA support`. Hej usergroup, har som standard bara hello ansluta behörighet och eventuella ytterligare dataåtkomst som behöver toobe som beviljas i hello normalt sätt. Observera att användaren `steve@gmail.com` som gästanvändare måste kryssrutan hello och lägga till domännamn hello AD `contosotest.onmicrosoft.com` i hello SSMS **Anslutningsegenskapen** dialogrutan. Hej **AD domän namn eller klient-ID** alternativet stöds bara för hello Universal med MFA anslutningsalternativ, annars den är nedtonat.

## <a name="universal-authentication-limitations-for-sql-database-and-sql-data-warehouse"></a>Universal autentisering begränsningar för SQL Database och SQL Data Warehouse
* SSMS och SqlPackage.exe är hello endast verktyg som aktiverats för MFA via Active Directory Universal autentisering.
* SSMS version 17,2, stöder flera samtidiga användaråtkomst med Universal autentisering med MFA. Version 17,0 och 17,1, begränsad en logg i för en instans av SSMS med Universal autentisering tooa enda Azure Active Directory-konto. Du måste använda en annan instans av SSMS toolog i som en annan Azure AD-kontot. (Den här begränsningen är begränsad tooActive Directory Universal autentisering, du kan logga in toodifferent servrar med hjälp av Active Directory-lösenordsautentisering, Active Directory-integrerad autentisering eller SQL Server-autentisering).
* SSMS stöder Active Directory Universal autentisering för visualisering Object Explorer och frågeredigeraren Query Store.
* SSMS version 17,2 stöder DacFx guiden Distribuera-Export/extrahera Data-databas. När en viss användare har autentiserats genom hello första autentiseringsdialogrutan med Universal autentisering hello DacFx guiden funktioner hello samma sätt för alla andra metoder för autentisering.
* Hej SSMS tabelldesignern stöder inte universella autentisering.
* Det finns inga ytterligare programvarukrav för Active Directory Universal autentisering förutom att du måste använda en version som stöds av SSMS.  
* hello Active Directory Authentication Library (ADAL) version för Universal autentisering var uppdaterade tooits senaste publicerat ADAL.dll 3.13.9 tillgängliga versionen. Se [Active Directory Authentication Library 3.14.1](http://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/).


## <a name="next-steps"></a>Nästa steg

- Konfigurationssteg finns [konfigurera Azure SQL Database Multi-Factor authentication för SQL Server Management Studio](sql-database-ssms-mfa-authentication-configure.md).
- Ge andra åtkomst till databasen tooyour: [SQL Database-autentisering och auktorisering: bevilja åtkomst](sql-database-manage-logins.md)  
- Kontrollera att andra kan ansluta genom brandväggen hello: [konfigurera Azure SQL Database-servernivå brandväggen en regel med hjälp av hello Azure-portalen](sql-database-configure-firewall-settings.md)  
- [Konfigurera och hantera Azure Active Directory-autentisering med SQL Database eller SQL Data Warehouse](sql-database-aad-authentication-configure.md)  
- [Microsoft SQL Server Data-Tier Application Framework (17.0.0 GA)](https://www.microsoft.com/download/details.aspx?id=55088)  
- [SQLPackage.exe](https://msdn.microsoft.com/library/hh550080.aspx)  
- [Importera en BACPAC filen tooa ny Azure SQL-databas](../sql-database/sql-database-import.md)  
- [Exportera en Azure SQL database tooa BACPAC fil](../sql-database/sql-database-export.md)  
- C#-gränssnitt [IUniversalAuthProvider gränssnitt](https://msdn.microsoft.com/library/microsoft.sqlserver.dac.iuniversalauthprovider.aspx)  
