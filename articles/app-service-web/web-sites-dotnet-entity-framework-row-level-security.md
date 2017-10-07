---
title: "Självstudier: Webbprogram med en databas för flera innehavare med hjälp av Entity Framework och säkerhet på radnivå"
description: "Lär dig hur toodevelop en ASP.NET MVC 5 web app med en SQL-databas backent, med hjälp av Entity Framework och säkerhet på radnivå med flera innehavare."
metakeywords: azure asp.net mvc entity framework multi tenant row level security rls sql database
services: app-service\web
documentationcenter: .net
manager: jeffreyg
author: tmullaney
ms.assetid: 8fdc47a5-6fc3-4d29-ab6a-33e79f50699f
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 04/25/2016
ms.author: thmullan
ms.openlocfilehash: 1b715e01807032c3f6497c374ce427dd762af141
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-web-app-with-a-multi-tenant-database-using-entity-framework-and-row-level-security"></a>Självstudier: Webbprogram med en databas för flera innehavare med hjälp av Entity Framework och säkerhet på radnivå
Den här kursen visar hur toobuild flera innehavare web app med en ”[delad databas, delade schema](https://msdn.microsoft.com/library/aa479086.aspx)” innehavare modell, och använder Entity Framework och [säkerhet på radnivå](https://msdn.microsoft.com/library/dn765131.aspx). En enskild databas innehåller för många hyresgäster i den här modellen, och varje rad i varje tabell som är associerad med en ”klient-ID” Radnivå säkerhet (RLS), en ny funktion för Azure SQL-databas är används tooprevent klienter från att komma åt varandras data. Detta kräver en enskild, mindre ändring toohello tillämpning. Genom att centralisera hello klient åtkomst logik i själva hello-databasen, RLS förenklar hello programkod och minskar hello risk för oavsiktliga dataläckage mellan klienter behålls.

Vi börjar med hello enkelt Contact Manager program från [skapa en ASP.NET-MVP-app med autentisering och SQL-databas och distribuera tooAzure Apptjänst](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md). Höger nu hello program kan alla användare (klienter) toosee alla kontakter:

![Kontakta Manager-program innan du aktiverar RLS](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-Before.png)

Med bara några mindre ändringar kommer vi lägga till stöd för flera innehavare så att användare kan toosee endast hello kontakter som tillhör toothem.

## <a name="step-1-add-an-interceptor-class-in-hello-application-tooset-hello-sessioncontext"></a>Steg 1: Lägg till en spärren klass i hello programmet tooset hello SESSION_CONTEXT
Det finns ett program ändra vi behöver toomake. Eftersom alla programanvändare ansluta toohello databas med hjälp av Hej samma anslutningssträng (d.v.s. samma SQL-inloggning), det går för närvarande inte för en RLS princip tooknow som den ska filtrera efter. Den här metoden är väldigt vanligt i webbprogram eftersom det möjliggör effektiv anslutningspoolning, men det innebär att vi behöver ett annat sätt tooidentify hello aktuell programanvändare hello-databasen. hello lösningen är toohave hello program ange ett nyckel / värde-par för hello aktuella användar-ID i hello [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806) omedelbart efter att öppna en anslutning innan den kör frågor. SESSION_CONTEXT är en session som definitionsområde nyckelvärdeslager och policyn RLS använder hello UserId lagras i den aktuella användaren för tooidentify hello.

Vi lägger till en [spärren](https://msdn.microsoft.com/data/dn469464.aspx) (särskilt en [DbConnectionInterceptor](https://msdn.microsoft.com/library/system.data.entity.infrastructure.interception.idbconnectioninterceptor)), en ny funktion i Entity Framework (EF) 6, tooautomatically set hello aktuella användar-ID i hello SESSION_CONTEXT genom att köra en T-SQL-instruktionen när EF öppnar en anslutning.

1. Öppna hello ContactManager projekt i Visual Studio.
2. Högerklicka på hello modellmappen i hello Solution Explorer och välj sedan Lägg till > klass.
3. Namnge hello nya klassen ”SessionContextInterceptor.cs” och klicka på Lägg till.
4. Ersätt hello innehållet i SessionContextInterceptor.cs med hello följande kod.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Data.Common;
using System.Data.Entity;
using System.Data.Entity.Infrastructure.Interception;
using Microsoft.AspNet.Identity;

namespace ContactManager.Models
{
    public class SessionContextInterceptor : IDbConnectionInterceptor
    {
        public void Opened(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
            // Set SESSION_CONTEXT toocurrent UserId whenever EF opens a connection
            try
            {
                var userId = System.Web.HttpContext.Current.User.Identity.GetUserId();
                if (userId != null)
                {
                    DbCommand cmd = connection.CreateCommand();
                    cmd.CommandText = "EXEC sp_set_session_context @key=N'UserId', @value=@UserId";
                    DbParameter param = cmd.CreateParameter();
                    param.ParameterName = "@UserId";
                    param.Value = userId;
                    cmd.Parameters.Add(param);
                    cmd.ExecuteNonQuery();
                }
            }
            catch (System.NullReferenceException)
            {
                // If no user is logged in, leave SESSION_CONTEXT null (all rows will be filtered)
            }
        }

        public void Opening(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void BeganTransaction(DbConnection connection, BeginTransactionInterceptionContext interceptionContext)
        {
        }

        public void BeginningTransaction(DbConnection connection, BeginTransactionInterceptionContext interceptionContext)
        {
        }

        public void Closed(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void Closing(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void ConnectionStringGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionStringGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionStringSet(DbConnection connection, DbConnectionPropertyInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionStringSetting(DbConnection connection, DbConnectionPropertyInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionTimeoutGetting(DbConnection connection, DbConnectionInterceptionContext<int> interceptionContext)
        {
        }

        public void ConnectionTimeoutGot(DbConnection connection, DbConnectionInterceptionContext<int> interceptionContext)
        {
        }

        public void DataSourceGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void DataSourceGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void DatabaseGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void DatabaseGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void Disposed(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void Disposing(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void EnlistedTransaction(DbConnection connection, EnlistTransactionInterceptionContext interceptionContext)
        {
        }

        public void EnlistingTransaction(DbConnection connection, EnlistTransactionInterceptionContext interceptionContext)
        {
        }

        public void ServerVersionGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void ServerVersionGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void StateGetting(DbConnection connection, DbConnectionInterceptionContext<System.Data.ConnectionState> interceptionContext)
        {
        }

        public void StateGot(DbConnection connection, DbConnectionInterceptionContext<System.Data.ConnectionState> interceptionContext)
        {
        }
    }

    public class SessionContextConfiguration : DbConfiguration
    {
        public SessionContextConfiguration()
        {
            AddInterceptor(new SessionContextInterceptor());
        }
    }
}
```

Det är endast programmet hello ändras. Gå vidare och skapa och publicera programmet hello.

## <a name="step-2-add-a-userid-column-toohello-database-schema"></a>Steg 2: Lägg till ett användar-ID för kolumnen toohello databasschema
Därefter måste tooadd en UserId kolumnen toohello kontakter tabell tooassociate varje rad med en användare (klient). Vi ändrar hello schema direkt i hello-databasen, så att vi inte har tooinclude det här fältet i vår EF datamodell.

Ansluta toohello databasen direkt, med hjälp av SQL Server Management Studio eller Visual Studio och kör följande T-SQL hello:

```
ALTER TABLE Contacts ADD UserId nvarchar(128)
    DEFAULT CAST(SESSION_CONTEXT(N'UserId') AS nvarchar(128))
```

Detta lägger till en tabell med användar-ID kolumn toohello kontakter. Vi använder hello nvarchar(128) data typen toomatch hello UserIds lagras i hello AspNetUsers tabell och vi skapa en Standardbegränsning som automatiskt in hello användar-ID för nya infogade rader toobe hello användar-ID som för närvarande lagras i SESSION_CONTEXT.

Nu hello tabellen ser ut så här:

![Tabellen SSMS kontakter](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-Contacts.png)

När nya kontakter skapas ska tilldelas de automatiskt att hello korrigera användar-ID. För demonstration, men vi tilldela några av dessa befintliga kontakter tooan befintliga användare.

Om du har skapat ett fåtal användare i hello programmet redan (t.ex. använda lokala Google eller Facebook konton), ser du dem i hello AspNetUsers tabell. I hello skärmbilden nedan finns bara en användare hittills.

![SSMS AspNetUsers tabell](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-AspNetUsers.png)

Kopiera hello Id för user1@contoso.com, och klistra in den i hello T-SQL-instruktionen nedan. Köra den här instruktionen tooassociate tre hello kontakter med det här användar-ID.

```
UPDATE Contacts SET UserId = '19bc9b0d-28dd-4510-bd5e-d6b6d445f511'
WHERE ContactId IN (1, 2, 5)
```

## <a name="step-3-create-a-row-level-security-policy-in-hello-database"></a>Steg 3: Skapa en princip för säkerhet på radnivå i hello-databas
hello sista steget är toocreate en säkerhetsprincip som använder hello användar-ID i SESSION_CONTEXT tooautomatically filter hello resultaten som returnerades av frågor.

När fortfarande anslutna toohello databas kör du hello följande T-SQL:

```
CREATE SCHEMA Security
go

CREATE FUNCTION Security.userAccessPredicate(@UserId nvarchar(128))
    RETURNS TABLE
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS accessResult
    WHERE @UserId = CAST(SESSION_CONTEXT(N'UserId') AS nvarchar(128))
go

CREATE SECURITY POLICY Security.userSecurityPolicy
    ADD FILTER PREDICATE Security.userAccessPredicate(UserId) ON dbo.Contacts,
    ADD BLOCK PREDICATE Security.userAccessPredicate(UserId) ON dbo.Contacts
go

```

Den här koden har tre saker. Först skapar den ett nytt schema som bästa praxis för att centralisera och begränsa åtkomst toohello RLS objekt. Därefter skapar den en predikatfunktionens som returnerar '1' när hello användar-ID för en rad matchar hello användar-ID i SESSION_CONTEXT. Slutligen skapar den en säkerhetsprincip som lägger till den här funktionen som ett filter och ett block-predikat för tabellen för kontakter hello. hello filterpredikat orsakar frågor tooreturn endast rader som tillhör toohello aktuell användare och hello block-predikatet fungerar som skyddar tooprevent hello program från att någonsin av misstag infoga en rad för hello fel användaren.

Nu kör hello programmet och logga in som user1@contoso.com. Den här användaren ser nu endast hello kontakter som har tilldelats tidigare toothis användar-ID:

![Kontakta Manager-program innan du aktiverar RLS](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-After.png)

toovalidate detta ytterligare försök att registrera en ny användare. Användaren ser inga kontakter eftersom ingen har tilldelats toothem. Om de skapar en ny kontakt, tilldelas toothem och endast de kommer att kunna toosee den.

## <a name="next-steps"></a>Nästa steg
Klart! hello enkla kontakta Manager webbprogrammet har konverterats till ett flera innehavare en där varje användare har sin egen kontaktlista. Med hjälp av säkerhet på radnivå har vi undvikas hello komplexitet tvingande klient åtkomst logiken i vår programkod. Den här genomskinlighet kan hello programmet toofocus på hello verkliga affärsproblem till hands och hello risken för sprids av misstag data som programmet hello har codebase minskar också växer.

Den här självstudiekursen har endast repad hello yta vad du kan göra med RLS. Exempelvis är det möjligt toohave mer avancerade eller detaljerade åtkomst logik och möjliga toostore mer än bara hello aktuella användar-ID i hello SESSION_CONTEXT. Det är också möjligt för[integrera RLS med hello klientbibliotek för elastisk databas verktyg](../sql-database/sql-database-elastic-tools-multi-tenant-row-level-security.md) toosupport flera innehavare delar i en skalbar datanivå.

Utöver dessa möjligheter arbetar vi också toomake RLS ännu bättre. Om du har några frågor, idéer eller saker som du vill att toosee berätta i hello kommentarer. Vi uppskattar din feedback!

