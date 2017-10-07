---
title: "aaaConditional åtkomst - Azure SQL Database och Data Warehouse | Microsoft-dokument"
description: "Lär dig hur tooconfigure villkorlig åtkomst för Azure SQL Database och datalagret."
services: sql-database
author: BYHAM
manager: jhubbard
ms.custom: security
ms.service: sql-database
ms.topic: article
ms.date: 06/07/2017
ms.author: rickbyh
ms.openlocfilehash: f49f4708c0f1b3cad1539d630c2efd919f8ece68
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="conditional-access-mfa-with-azure-sql-database-and-data-warehouse"></a>Villkorlig åtkomst (MFA) med Azure SQL-databas och informationslager  

Både SQL Database och SQL Data Warehouse stöder Microsoft villkorlig åtkomst. Hej följande steg visar hur tooconfigure SQL-databas tooenforce en princip för villkorlig åtkomst.  

## <a name="prerequisites"></a>Krav  
- Du måste konfigurera din toosupport SQL Database eller SQL Data Warehouse Azure Active Directory-autentisering. Specifika anvisningar finns [konfigurera och hantera Azure Active Directory-autentisering med SQL Database eller SQL Data Warehouse](sql-database-aad-authentication-configure.md).  
- När multifaktorautentisering har aktiverats måste du ansluta med vid stöds verktyg, till exempel hello senaste SSMS. Mer information finns i [konfigurera Azure SQL Database Multi-Factor authentication för SQL Server Management Studio](sql-database-ssms-mfa-authentication-configure.md).  

## <a name="configure-ca-for-azure-sql-dbdw"></a>Konfigurera Certifikatutfärdaren för Azure SQL DB/DW  
1.  Logga in toohello Portal, Välj **Azure Active Directory**, och välj sedan **villkorlig åtkomst**. Mer information finns i [Teknisk referens för Azure Active Directory villkorlig åtkomst](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access-technical-reference).  
  ![bladet för villkorlig åtkomst](./media/sql-database-conditional-access/conditional-access-blade.png) 
     
2.  I hello **villkorliga åtkomstprinciper** bladet, klickar du på **ny princip**, ange ett namn och klicka sedan på **konfigurera regler**.  
3.  Under **tilldelningar**väljer **användare och grupper**, kontrollera **Välj användare och grupper**, och välj sedan hello användare eller grupp för villkorlig åtkomst. Klicka på **Välj**, och klicka sedan på **klar** tooaccept valet.  
  ![Välj användare och grupper](./media/sql-database-conditional-access/select-users-and-groups.png)  

4.  Välj **Molnappar**, klickar du på **Välj appar**. Du kan se alla appar som är tillgängliga för villkorlig åtkomst. Välj **Azure SQL Database**, längst ned hello klickar du på **Välj**, och klicka sedan på **klar**.  
  ![Välj SQL-databas](./media/sql-database-conditional-access/select-sql-database.png)  
  Om du inte hittar **Azure SQL Database** listas i följande tredje skärmbild som visar hello slutföra hello följande steg:   
  - Logga in tooyour Azure SQL DB/DW-instans med SSMS med ett administratörskonto för AAD.  
  - Köra `CREATE USER [user@yourtenant.com] FROM EXTERNAL PROVIDER`.  
  - Logga in tooAAD och kontrollera att Azure SQL Database och datalagret visas i hello program i din AAD.  

5.  Välj **åtkomstkontroller**väljer **bevilja**, och sedan kontrollera hello-princip som du vill tooapply. Det här exemplet väljer vi **kräver Multi-Factor authentication**.  
  ![Välj bevilja åtkomst](./media/sql-database-conditional-access/grant-access.png)  

## <a name="summary"></a>Sammanfattning  
hello valt program (Azure SQL Database) så att tooconnect tooAzure SQL DB/DW med hjälp av Azure AD Premium, tillämpar nu hello valda principen för villkorlig åtkomst, **krävs multifaktorautentisering.**  
Frågor om Azure SQL Database och datalagret för multifaktorautentisering Kontakta MFAforSQLDB@microsoft.com.  

## <a name="next-steps"></a>Nästa steg  

En självstudiekurs finns [skydda din Azure SQL Database](sql-database-security-tutorial.md).
