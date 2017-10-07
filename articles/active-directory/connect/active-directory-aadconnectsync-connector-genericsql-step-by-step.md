---
title: "aaaGeneric SQL Connector steg för steg | Microsoft Docs"
description: "Den här artikeln är guida dig via en enkel HR-system med stegvisa hello allmän SQL-anslutningen."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 28c1cc60-24fd-4d0d-a36d-b4aba6de86e7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: b1b5f89ab588de6f92f173a7bc00f97180067669
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="generic-sql-connector-step-by-step"></a>Stegvisa anvisningar för allmän SQL-anslutningsapp
Det här avsnittet finns stegvisa instruktioner. Den skapar en enkel HR-exempeldatabas och använda den för att importera vissa användare och deras gruppmedlemskap.

## <a name="prepare-hello-sample-database"></a>Förbereda hello exempeldatabasen
På en server som kör SQL Server, kör du hello SQL-skript finns i [bilaga A](#appendix-a). Det här skriptet skapar en exempeldatabas med hello namnet GSQLDEMO. hello objektmodell för hello skapade databasen ser ut som om den här bilden:  
![Objektmodell](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/objectmodel.png)

Skapa även en användare som du vill toouse tooconnect toohello databasen. I den här genomgången är hello användaren kallas FABRIKAM\SQLUser och finns i hello domän.

## <a name="create-hello-odbc-connection-file"></a>Skapa hello ODBC-anslutningsfil
hello allmän SQL-anslutningen använder ODBC tooconnect toohello-fjärrservern. Vi måste först toocreate en fil med hello anslutningsinformationen för ODBC.

1. Starta hello ODBC-hanteringsverktyg på servern:  
   ![ODBC](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc.png)
2. Välj hello fliken **fil-DSN**. Klicka på **Lägg till... **.  
   ![ODBC1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc1.png)
3. hello out-of-box-drivrutinen fungerar finjustering så markerar du den och klicka på **Nästa >**.  
   ![ODBC2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc2.png)
4. Ge hello filen ett namn som **GenericSQL**.  
   ![ODBC3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc3.png)
5. Klicka på **Slutför**.  
   ![ODBC4](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc4.png)
6. Tid tooconfigure hello-anslutning. Ger en bra beskrivning för hello datakällan och ange hello namnet på hello-server som kör SQL Server.  
   ![ODBC5](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc5.png)
7. Välj hur tooauthenticate med SQL. I detta fall kan använda vi Windows-autentisering.  
   ![ODBC6](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc6.png)
8. Ange hello namnet på hello exempeldatabasen **GSQLDEMO**.  
   ![ODBC7](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc7.png)
9. Behåll allt standardvärdet på den här skärmen. Klicka på **Slutför**.  
   ![ODBC8](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc8.png)
10. tooverify allt fungerar som förväntat, klickar du på **testa datakällan**.  
    ![ODBC9](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc9.png)
11. Kontrollera att hello test har lyckats.  
    ![ODBC10](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc10.png)
12. konfigurationsfilen för hello ODBC ska nu visas i fil-DSN.  
    ![ODBC11](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc11.png)

Nu har vi hello filen vi behöver och kan börja skapa hello Connector.

## <a name="create-hello-generic-sql-connector"></a>Skapa hello allmän SQL-koppling
1. Välj i hello Synchronization Service Manager UI, **kopplingar** och **skapa**. Välj **generiskt SQL (Microsoft)** och ge det ett beskrivande namn.  
   ![Connector1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector1.png)
2. Hitta hello DSN-fil som du skapade i föregående avsnitt i hello och överför den toohello server. Ange hello referenser tooconnect toohello i databasen.  
   ![Connector2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector2.png)
3. I den här genomgången vi gör det enkelt för oss och säger att det finns två objekttyper **användare** och **gruppen**.
   ![Connector3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector3.png)
4. toofind hello attribut, och vi vill hello Connector toodetect attributen genom att titta på själva hello-tabellen. Eftersom **användare** är ett reserverat ord i SQL, behöver vi tooprovide i kvadrat klammerparenteser [].  
   ![Connector4](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector4.png)
5. Tid toodefine hello fästpunktsattributet och hello DN-attribut. För **användare**, vi använda hello kombination av hello två attribut användarnamn och EmployeeID. För **grupp**, använder vi GroupName (inte realistiska i praktiken, men i den här genomgången fungerar).
   ![Connector5](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector5.png)
6. Inte alla attributtyper kan identifieras i en SQL-databas. hello referenstyp attribut kan inte särskilt. För hello gruppen objekttyp behöver vi toochange hello OwnerID och MemberID tooreference.  
   ![Connector6](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector6.png)
7. hello-attribut som vi markerad som referensattribut i föregående steg i hello kräver hello objekttyp dessa värden är en referens till. I vårt fall hello objekttyp för användaren.  
   ![Connector7](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector7.png)
8. Hello globala parametrar på sidan Välj **vattenstämpel** som hello delta strategi. Också skriva i hello datum/tid-formatet **åååå-MM-dd: mm: ss**.
   ![Connector8](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector8.png)
9. På hello **konfigurera partitioner och hierarkier** markerar du båda objekttyper.
   ![Connector9](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector9.png)
10. På hello **Välj objekttyper** och **Välj attribut**, väljer både objekttyper och alla attribut. På hello **konfigurera ankare** klickar du på **Slutför**.

## <a name="create-run-profiles"></a>Skapa körningsprofiler
1. Välj i hello Synchronization Service Manager UI, **kopplingar**, och **Konfigurera körningsprofiler**. Klicka på **ny profil**. Vi börjar med **fullständig Import**.  
   ![Runprofile1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile1.png)
2. Välj typ av hello **fullständig Import (endast steget)**.  
   ![Runprofile2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile2.png)
3. Välj hello partition **objekt = användare**.  
   ![Runprofile3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile3.png)
4. Välj **tabell** och skriv **[användare]**. Rulla ned toohello med flera värden objektet typen avsnittet och ange hello data enligt följande bild hello. Välj **Slutför** toosave hello steg.  
   ![Runprofile4a](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile4a.png)  
   ![Runprofile4b](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile4b.png)  
5. Välj **nytt steg**. Den här gången väljer **objekt = grupp**. Använd hello konfiguration enligt följande bild hello hello sista sidan. Klicka på **Slutför**.  
   ![Runprofile5a](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile5a.png)  
   ![Runprofile5b](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile5b.png)  
6. Valfritt: Om du vill kan du kan konfigurera ytterligare körning av profiler. Den här genomgången används endast hello fullständig Import.
7. Klicka på **OK** toofinish ändra körningsprofiler.

## <a name="add-some-test-data-and-test-hello-import"></a>Lägg till import för hello av data och vissa test
Fylla lite testdata i din exempeldatabas. När du är klar väljer du **kör** och **fullständig import**.

Här är en användare med två telefonnummer och en grupp med medlemmar.  
![cs1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/cs1.png)  
![CS2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/cs2.png)  

## <a name="appendix-a"></a>Bilaga A
**SQL-skript toocreate hello exempeldatabasen**

```SQL
---Creating hello Database---------
Create Database GSQLDEMO
Go
-------Using hello Database-----------
Use [GSQLDEMO]
Go
-------------------------------------
USE [GSQLDEMO]
GO
/****** Object:  Table [dbo].[GroupMembers]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[GroupMembers](
    [MemberID] [int] NOT NULL,
    [Group_ID] [int] NOT NULL
) ON [PRIMARY]

GO
/****** Object:  Table [dbo].[GROUPS]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[GROUPS](
    [GroupID] [int] NOT NULL,
    [GROUPNAME] [nvarchar](200) NOT NULL,
    [DESCRIPTION] [nvarchar](200) NULL,
    [WATERMARK] [datetime] NULL,
    [OwnerID] [int] NULL,
PRIMARY KEY CLUSTERED
(
    [GroupID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
/****** Object:  Table [dbo].[USERPHONE]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
SET ANSI_PADDING ON
GO
CREATE TABLE [dbo].[USERPHONE](
    [USER_ID] [int] NULL,
    [Phone] [varchar](20) NULL
) ON [PRIMARY]

GO
SET ANSI_PADDING OFF
GO
/****** Object:  Table [dbo].[USERS]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[USERS](
    [USERID] [int] NOT NULL,
    [USERNAME] [nvarchar](200) NOT NULL,
    [FirstName] [nvarchar](100) NULL,
    [LastName] [nvarchar](100) NULL,
    [DisplayName] [nvarchar](100) NULL,
    [ACCOUNTDISABLED] [bit] NULL,
    [EMPLOYEEID] [int] NOT NULL,
    [WATERMARK] [datetime] NULL,
PRIMARY KEY CLUSTERED
(
    [USERID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
ALTER TABLE [dbo].[GroupMembers]  WITH CHECK ADD  CONSTRAINT [FK_GroupMembers_GROUPS] FOREIGN KEY([Group_ID])
REFERENCES [dbo].[GROUPS] ([GroupID])
GO
ALTER TABLE [dbo].[GroupMembers] CHECK CONSTRAINT [FK_GroupMembers_GROUPS]
GO
ALTER TABLE [dbo].[GroupMembers]  WITH CHECK ADD  CONSTRAINT [FK_GroupMembers_USERS] FOREIGN KEY([MemberID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[GroupMembers] CHECK CONSTRAINT [FK_GroupMembers_USERS]
GO
ALTER TABLE [dbo].[GROUPS]  WITH CHECK ADD  CONSTRAINT [FK_GROUPS_USERS] FOREIGN KEY([OwnerID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[GROUPS] CHECK CONSTRAINT [FK_GROUPS_USERS]
GO
ALTER TABLE [dbo].[USERPHONE]  WITH CHECK ADD  CONSTRAINT [FK_USERPHONE_USER] FOREIGN KEY([USER_ID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[USERPHONE] CHECK CONSTRAINT [FK_USERPHONE_USER]
GO
```
