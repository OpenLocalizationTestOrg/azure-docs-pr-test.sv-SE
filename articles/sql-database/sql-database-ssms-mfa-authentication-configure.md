---
title: aaaConfigure multifaktorautentisering - Azure SQL | Microsoft Docs
description: "Använda flera Inberäknade autentisering med SSMS för SQL Database och SQL Data Warehouse."
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/17/2017
ms.author: rickbyh
ms.openlocfilehash: acb275965f4199f7d5e1378d5077824a9bbc80e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-multi-factor-authentication-for-sql-server-management-studio-and-azure-ad"></a>Konfigurera Multi-Factor authentication för SQL Server Management Studio och Azure AD

Det här avsnittet beskrivs hur du toouse Azure Active Directory multifaktorautentisering (MFA) med SQL Server Management Studio. Azure AD MFA kan användas när du ansluter SSMS eller SqlPackage.exe tooAzure SQL Database och Azure SQL Data Warehouse.

En översikt över Azure SQL Database multifaktorautentisering finns [Universal autentisering med SQL Database och SQL Data Warehouse (SSMS stöd för MFA)](sql-database-ssms-mfa-authentication.md).

## <a name="configuration-steps"></a>Konfigurationssteg

1. **Konfigurera ett Azure Active Directory** – mer information finns i [administrera Azure AD-katalogen](https://msdn.microsoft.com/library/azure/hh967611.aspx), [integrera dina lokala identiteter med Azure Active Directory](../active-directory/active-directory-aadconnect.md), [Lägga till egna domänen namnet tooAzure AD](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [Microsoft Azure stöder nu federation med Windows Server Active Directory](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), och [hantera Azure AD med hjälp av Windows PowerShell ](https://msdn.microsoft.com/library/azure/jj151815.aspx).
2. **Konfigurera MFA** - stegvisa anvisningar finns i [vad är Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md), [villkorlig åtkomst (MFA) med Azure SQL Database och datalagret](sql-database-conditional-access.md). (Fullständig villkorlig åtkomst kräver en Premium Azure Active Directory (AD Azure). Begränsad MFA är tillgängligt med en Azure AD som standard.)
3. **Konfigurera SQL Database eller SQL Data Warehouse för Azure AD Authentication** - stegvisa anvisningar finns i [ansluta tooSQL Database eller SQL Data Warehouse med hjälp av Azure Active Directory Authentication](sql-database-aad-authentication.md).
4. **Hämta SSMS** - hello klientdator, hämtar hello senaste SSMS från [Hämta SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx). För alla hello-funktioner i det här avsnittet, använda minst version 17,2 juli 2017.  

## <a name="connecting-by-using-universal-authentication-with-ssms"></a>Ansluta med hjälp av universal autentisering med SSMS

hello följande steg visar hur tooconnect tooSQL Database eller SQL Data Warehouse med hjälp av hello senaste SSMS.

1. tooconnect med Universal autentisering på hello **ansluta tooServer** dialogrutan **Active Directory - Universal med stöd för MFA**. (Om du ser **Active Directory Universal autentisering** du är inte på hello senaste versionen av SSMS.)  
   ![1mfa universal ansluta][1]  
2. Fullständig hello **användarnamn** rutan med hello Azure Active Directory-autentiseringsuppgifter, hello format `user_name@domain.com`.  
   ![1mfa-universal-ansluta-användare](./media/sql-database-ssms-mfa-auth/1mfa-universal-connect-user.png)   
3. Om du ansluter som gästanvändare, måste du klicka **alternativ**, och på hello **Anslutningsegenskapen** dialogrutan, fullständig hello **AD domän namn eller klient-ID** rutan. Mer information finns i [Universal autentisering med SQL Database och SQL Data Warehouse (SSMS stöd för MFA)](sql-database-ssms-mfa-authentication.md).
   ![mfa-klient-ssms](./media/sql-database-ssms-mfa-auth/mfa-tenant-ssms.png)   
4. Som vanligt för SQL Database och SQL Data Warehouse, måste du klicka på **alternativ** och ange hello databasen i hello **alternativ** dialogrutan. (Om hello anslutna användare är en gästanvändare (d.v.s. joe@outlook.com), måste du kryssrutan hello och lägga till hello aktuella AD-domän namn eller klient-ID som en del av alternativen. Se [Universal autentisering med SQL Database och SQL Data Warehouse (SSMS stöd för MFA)]()(sql-databasen-ssms-mfa-authentication.md. Klicka på **Anslut**.  
5. När hello **logga in tooyour konto** i dialogrutan Ange hello-konto och lösenord för din Azure Active Directory-identitet. Inget lösenord krävs om en användare är en del av en domän som federerade med Azure AD.  
   ![2mfa-inloggning][2]  

   > [!NOTE]
   > För Universal autentisering med ett konto som inte kräver MFA, ansluter du nu. För användare som kräver MFA, fortsätter du med hello följande steg:
   >  
   
6. Två MFA dialogrutor visas. En gång åtgärden beror på hello MFA administratören ställa in och kan därför valfria. För en domän för MFA är aktiverat i det här steget ibland är fördefinierad (till exempel hello domän kräver användare toouse smartkort och PIN-kod).  
   ![3mfa-installationen][3]  
7. hello andra möjliga en gång dialogrutan kan du tooselect hello information om din autentiseringsmetod. hello möjliga alternativ har konfigurerats av administratören.  
   ![4mfa kontrollera 1][4]  
8. hello Azure Active Directory skickar hello bekräftar information tooyou. När du får hello Verifieringskod kan du ange det i hello **ange verifieringskoden** och på **logga in**.  
   ![5mfa Kontrollera 2][5]  

När verifieringen är klar ansluter SSMS normalt förutsatt att giltiga autentiseringsuppgifter och åtkomst för brandväggen.

## <a name="next-steps"></a>Nästa steg

* En översikt över Azure SQL Database multifaktorautentisering finns i Universal autentisering med [SQL Database och SQL Data Warehouse (SSMS stöd för MFA)](sql-database-ssms-mfa-authentication.md).
* Ge andra åtkomst till databasen tooyour: [SQL Database-autentisering och auktorisering: bevilja åtkomst](sql-database-manage-logins.md)  
Kontrollera att andra kan ansluta genom brandväggen hello: [konfigurera Azure SQL Database-servernivå brandväggen en regel med hjälp av hello Azure-portalen](sql-database-configure-firewall-settings.md)


[1]: ./media/sql-database-ssms-mfa-auth/1mfa-universal-connect.png
[2]: ./media/sql-database-ssms-mfa-auth/2mfa-sign-in.png
[3]: ./media/sql-database-ssms-mfa-auth/3mfa-setup.png
[4]: ./media/sql-database-ssms-mfa-auth/4mfa-verify-1.png
[5]: ./media/sql-database-ssms-mfa-auth/5mfa-verify-2.png

