---
title: "Självstudier: Webbprogram med en databas för flera innehavare med hjälp av Entity Framework och säkerhet på radnivå"
description: "Lär dig hur du utvecklar en ASP.NET MVC 5 webbapp med en SQL-databas backent, med hjälp av Entity Framework och säkerhet på radnivå med flera innehavare."
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
ms.openlocfilehash: ba1bb3d84b462dfebbb2564569517d7336bf54fd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-web-app-with-a-multi-tenant-database-using-entity-framework-and-row-level-security"></a>Självstudier: Webbprogram med en databas för flera innehavare med hjälp av Entity Framework och säkerhet på radnivå
Den här kursen visar hur du skapar ett webbprogram med flera innehavare med en ”[delad databas, delade schema](https://msdn.microsoft.com/library/aa479086.aspx)” innehavare modell, och använder Entity Framework och [säkerhet på radnivå](https://msdn.microsoft.com/library/dn765131.aspx). En enskild databas innehåller för många hyresgäster i den här modellen, och varje rad i varje tabell som är associerad med en ”klient-ID” Radnivå säkerhet (RLS), en ny funktion för Azure SQL-databas används för att hindra klienter från att komma åt varandras data. Detta kräver bara en enda, mindre ändring till programmet. Centralisera klient åtkomst logik i själva databasen RLS förenklar programkoden och minskar risken för oavsiktliga dataläckage mellan klienter behålls.

Vi börjar med enkla Contact Manager programmet från [skapa en ASP.NET-MVP-app med autentisering och SQL-databas och distribuera till Azure App Service](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md). Höger nu programmet gör att alla användare (klienter) för att se alla kontakter:

![Kontakta Manager-program innan du aktiverar RLS](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-Before.png)

Med bara några mindre ändringar kommer vi lägga till stöd för flera innehavare så att användarna ska kunna se kontakterna som de tillhör.

## <a name="step-1-add-an-interceptor-class-in-the-application-to-set-the-sessioncontext"></a>Steg 1: Lägg till en spärren klass i programmet för att ange SESSION_CONTEXT
Det finns ett program ändra vi behöver göra. Eftersom alla användare att ansluta till databasen med samma anslutningssträng (d.v.s. samma SQL-inloggning), går det för närvarande inte för en princip för RLS att veta vilka användare som den ska filtrera efter. Den här metoden är väldigt vanligt i webbprogram eftersom det möjliggör effektiv anslutningspoolning, men det innebär att vi behöver ett annat sätt att identifiera aktuell programanvändare i databasen. Lösningen är att ange ett nyckel / värde-par för den aktuella användar-ID i programmet i [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806) omedelbart efter att öppna en anslutning innan den kör frågor. SESSION_CONTEXT är en session som definitionsområde nyckelvärdeslager och policyn RLS använder användar-ID som lagras i den för att identifiera den aktuella användaren.

Vi lägger till en [spärren](https://msdn.microsoft.com/data/dn469464.aspx) (särskilt en [DbConnectionInterceptor](https://msdn.microsoft.com/library/system.data.entity.infrastructure.interception.idbconnectioninterceptor)), en ny funktion i Entity Framework (EF) 6, att automatiskt ange den aktuella användar-ID i SESSION_CONTEXT genom att köra ett T-SQL-instruktionen när EF öppnar en anslutning.

1. Öppna ContactManager projektet i Visual Studio.
2. Högerklicka på mappen modeller i Solution Explorer och välja Lägg till > klass.
3. Namnge den nya klassen ”SessionContextInterceptor.cs” och klicka på Lägg till.
4. Ersätt innehållet i SessionContextInterceptor.cs med följande kod.

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
            // Set SESSION_CONTEXT to current UserId whenever EF opens a connection
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

Det är endast programmet ändringen krävs. Gå vidare och skapa och publicera programmet.

## <a name="step-2-add-a-userid-column-to-the-database-schema"></a>Steg 2: Lägg till en kolumn för användar-ID databasschemat
Nu ska behöver vi lägga till en användar-ID-kolumnen i tabellen kontakter associera varje rad med en användare (klient). Vi ändrar schemat direkt i databasen, så att vi inte behöver inkludera fältet i vår EF datamodell.

Ansluta till databasen direkt, med hjälp av SQL Server Management Studio eller Visual Studio och kör följande T-SQL:

```
ALTER TABLE Contacts ADD UserId nvarchar(128)
    DEFAULT CAST(SESSION_CONTEXT(N'UserId') AS nvarchar(128))
```

Detta lägger till en användar-ID-kolumnen i tabellen kontakter. Vi använder datatypen nvarchar(128) för att matcha UserIds lagras i tabellen AspNetUsers och vi skapa en Standardbegränsning som automatiskt in användar-ID för nya infogade rader ska användar-ID som för närvarande lagras i SESSION_CONTEXT.

Nu tabellen ser ut så här:

![Tabellen SSMS kontakter](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-Contacts.png)

När nya kontakter har skapats, kommer de automatiskt tilldelade rätt användar-ID. Demonstration, men vi tilldela några av dessa befintliga kontakter till en befintlig användare.

Om du har skapat ett fåtal användare i program redan (t.ex. använda lokala Google eller Facebook konton), ser du dem i tabellen AspNetUsers. I skärmbilden nedan finns bara en användare hittills.

![SSMS AspNetUsers tabell](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-AspNetUsers.png)

Kopiera Id för user1@contoso.com, och klistra in den i T-SQL-instruktionen nedan. Köra den här instruktionen om du vill koppla tre kontakter till den här användar-ID.

```
UPDATE Contacts SET UserId = '19bc9b0d-28dd-4510-bd5e-d6b6d445f511'
WHERE ContactId IN (1, 2, 5)
```

## <a name="step-3-create-a-row-level-security-policy-in-the-database"></a>Steg 3: Skapa en princip för säkerhet på radnivå i databasen
Det sista steget är att skapa en säkerhetsprincip som använder det användar-ID i SESSION_CONTEXT automatiskt filtrera resultaten som returnerades av frågor.

Medan du fortfarande är anslutna till databasen, kör följande T-SQL:

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

Den här koden har tre saker. Först skapar den ett nytt schema som bästa praxis för att centralisera och begränsa åtkomsten till RLS-objekt. Därefter skapar den en predikatfunktionens som returnerar '1' när en rad användar-ID matchar användar-ID i SESSION_CONTEXT. Slutligen skapar den en säkerhetsprincip som lägger till den här funktionen som ett filter och ett block-predikat för tabellen kontakter. Filterpredikatet orsakar frågor för att returnera rader som hör till den aktuella användaren och block-predikatet fungerar som ett skydd för programmet att infoga en rad för fel användaren någonsin av misstag.

Nu kör programmet och logga in som user1@contoso.com. Den här användaren ser nu endast kontakter som vi tidigare tilldelats till den här användar-ID:

![Kontakta Manager-program innan du aktiverar RLS](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-After.png)

Om du vill kontrollera detta ytterligare försök att registrera en ny användare. Användaren ser inga kontakter eftersom ingen har tilldelats. Om de skapar en ny kontakt, tilldelas till dem och endast de kommer att kunna se den.

## <a name="next-steps"></a>Nästa steg
Klart! Enkel kontakta Manager webbprogrammet har konverterats till ett flera innehavare en där varje användare har sin egen kontaktlista. Med hjälp av säkerhet på radnivå har vi undvikas komplexitet tvingande klient åtkomst logiken i vår programkod. Den här genomskinlighet tillåter programmet att fokusera på den verkliga affärsproblem till hands och minskar också risken för sprids av misstag data som programmet har codebase växer.

Den här självstudiekursen har endast skadad yta vad du kan göra med RLS. Exempelvis är det möjligt att ha fler sofistikerade eller detaljerade åtkomst logik, och den är möjligt att lagra mer än bara den aktuella användar-ID i SESSION_CONTEXT. Det är också möjligt att [integrera RLS med klientbibliotek för elastisk databas verktyg](../sql-database/sql-database-elastic-tools-multi-tenant-row-level-security.md) till stöd för flera innehavare delar i en skalbar datanivå.

Utöver dessa möjligheter arbetar vi också för att göra RLS ännu bättre. Om du har några frågor, idéer eller saker som du skulle vilja se berätta i kommentarerna. Vi uppskattar din feedback!

