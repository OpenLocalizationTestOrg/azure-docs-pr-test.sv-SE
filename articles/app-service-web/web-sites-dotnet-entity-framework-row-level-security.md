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
# <a name="tutorial-web-app-with-a-multi-tenant-database-using-entity-framework-and-row-level-security"></a><span data-ttu-id="7188c-103">Självstudier: Webbprogram med en databas för flera innehavare med hjälp av Entity Framework och säkerhet på radnivå</span><span class="sxs-lookup"><span data-stu-id="7188c-103">Tutorial: Web app with a multi-tenant database using Entity Framework and Row-Level Security</span></span>
<span data-ttu-id="7188c-104">Den här kursen visar hur du skapar ett webbprogram med flera innehavare med en ”[delad databas, delade schema](https://msdn.microsoft.com/library/aa479086.aspx)” innehavare modell, och använder Entity Framework och [säkerhet på radnivå](https://msdn.microsoft.com/library/dn765131.aspx).</span><span class="sxs-lookup"><span data-stu-id="7188c-104">This tutorial shows how to build a multi-tenant web app with a "[shared database, shared schema](https://msdn.microsoft.com/library/aa479086.aspx)" tenancy model, using Entity Framework and [Row-Level Security](https://msdn.microsoft.com/library/dn765131.aspx).</span></span> <span data-ttu-id="7188c-105">En enskild databas innehåller för många hyresgäster i den här modellen, och varje rad i varje tabell som är associerad med en ”klient-ID”</span><span class="sxs-lookup"><span data-stu-id="7188c-105">In this model, a single database contains data for many tenants, and each row in each table is associated with a "Tenant ID."</span></span> <span data-ttu-id="7188c-106">Radnivå säkerhet (RLS), en ny funktion för Azure SQL-databas används för att hindra klienter från att komma åt varandras data.</span><span class="sxs-lookup"><span data-stu-id="7188c-106">Row-Level Security (RLS), a new feature for Azure SQL Database, is used to prevent tenants from accessing each other's data.</span></span> <span data-ttu-id="7188c-107">Detta kräver bara en enda, mindre ändring till programmet.</span><span class="sxs-lookup"><span data-stu-id="7188c-107">This requires just a single, small change to the application.</span></span> <span data-ttu-id="7188c-108">Centralisera klient åtkomst logik i själva databasen RLS förenklar programkoden och minskar risken för oavsiktliga dataläckage mellan klienter behålls.</span><span class="sxs-lookup"><span data-stu-id="7188c-108">By centralizing the tenant access logic within the database itself, RLS simplifies the application code and reduces the risk of accidental data leakage between tenants.</span></span>

<span data-ttu-id="7188c-109">Vi börjar med enkla Contact Manager programmet från [skapa en ASP.NET-MVP-app med autentisering och SQL-databas och distribuera till Azure App Service](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="7188c-109">Let's start with the simple Contact Manager application from [Create an ASP.NET MVP app with auth and SQL DB and deploy to Azure App Service](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).</span></span> <span data-ttu-id="7188c-110">Höger nu programmet gör att alla användare (klienter) för att se alla kontakter:</span><span class="sxs-lookup"><span data-stu-id="7188c-110">Right now, the application allows all users (tenants) to see all contacts:</span></span>

![Kontakta Manager-program innan du aktiverar RLS](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-Before.png)

<span data-ttu-id="7188c-112">Med bara några mindre ändringar kommer vi lägga till stöd för flera innehavare så att användarna ska kunna se kontakterna som de tillhör.</span><span class="sxs-lookup"><span data-stu-id="7188c-112">With just a few small changes, we will add support for multi-tenancy, so that users are able to see only the contacts that belong to them.</span></span>

## <a name="step-1-add-an-interceptor-class-in-the-application-to-set-the-sessioncontext"></a><span data-ttu-id="7188c-113">Steg 1: Lägg till en spärren klass i programmet för att ange SESSION_CONTEXT</span><span class="sxs-lookup"><span data-stu-id="7188c-113">Step 1: Add an Interceptor class in the application to set the SESSION_CONTEXT</span></span>
<span data-ttu-id="7188c-114">Det finns ett program ändra vi behöver göra.</span><span class="sxs-lookup"><span data-stu-id="7188c-114">There is one application change we need to make.</span></span> <span data-ttu-id="7188c-115">Eftersom alla användare att ansluta till databasen med samma anslutningssträng (d.v.s. samma SQL-inloggning), går det för närvarande inte för en princip för RLS att veta vilka användare som den ska filtrera efter.</span><span class="sxs-lookup"><span data-stu-id="7188c-115">Because all application users connect to the database using the same connection string (i.e. same SQL login), there is currently no way for an RLS policy to know which user it should filter for.</span></span> <span data-ttu-id="7188c-116">Den här metoden är väldigt vanligt i webbprogram eftersom det möjliggör effektiv anslutningspoolning, men det innebär att vi behöver ett annat sätt att identifiera aktuell programanvändare i databasen.</span><span class="sxs-lookup"><span data-stu-id="7188c-116">This approach is very common in web applications because it enables efficient connection pooling, but it means we need another way to identify the current application user within the database.</span></span> <span data-ttu-id="7188c-117">Lösningen är att ange ett nyckel / värde-par för den aktuella användar-ID i programmet i [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806) omedelbart efter att öppna en anslutning innan den kör frågor.</span><span class="sxs-lookup"><span data-stu-id="7188c-117">The solution is to have the application set a key-value pair for the current UserId in the [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806) immediately after opening a connection, before it executes any queries.</span></span> <span data-ttu-id="7188c-118">SESSION_CONTEXT är en session som definitionsområde nyckelvärdeslager och policyn RLS använder användar-ID som lagras i den för att identifiera den aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="7188c-118">SESSION_CONTEXT is a session-scoped key-value store, and our RLS policy will use the UserId stored in it to identify the current user.</span></span>

<span data-ttu-id="7188c-119">Vi lägger till en [spärren](https://msdn.microsoft.com/data/dn469464.aspx) (särskilt en [DbConnectionInterceptor](https://msdn.microsoft.com/library/system.data.entity.infrastructure.interception.idbconnectioninterceptor)), en ny funktion i Entity Framework (EF) 6, att automatiskt ange den aktuella användar-ID i SESSION_CONTEXT genom att köra ett T-SQL-instruktionen när EF öppnar en anslutning.</span><span class="sxs-lookup"><span data-stu-id="7188c-119">We will add an [interceptor](https://msdn.microsoft.com/data/dn469464.aspx) (in particular, a [DbConnectionInterceptor](https://msdn.microsoft.com/library/system.data.entity.infrastructure.interception.idbconnectioninterceptor)), a new feature in Entity Framework (EF) 6, to automatically set the current UserId in the SESSION_CONTEXT by executing a T-SQL statement whenever EF opens a connection.</span></span>

1. <span data-ttu-id="7188c-120">Öppna ContactManager projektet i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7188c-120">Open the ContactManager project in Visual Studio.</span></span>
2. <span data-ttu-id="7188c-121">Högerklicka på mappen modeller i Solution Explorer och välja Lägg till > klass.</span><span class="sxs-lookup"><span data-stu-id="7188c-121">Right-click on the Models folder in the Solution Explorer, and choose Add > Class.</span></span>
3. <span data-ttu-id="7188c-122">Namnge den nya klassen ”SessionContextInterceptor.cs” och klicka på Lägg till.</span><span class="sxs-lookup"><span data-stu-id="7188c-122">Name the new class "SessionContextInterceptor.cs" and click Add.</span></span>
4. <span data-ttu-id="7188c-123">Ersätt innehållet i SessionContextInterceptor.cs med följande kod.</span><span class="sxs-lookup"><span data-stu-id="7188c-123">Replace the contents of SessionContextInterceptor.cs with the following code.</span></span>

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

<span data-ttu-id="7188c-124">Det är endast programmet ändringen krävs.</span><span class="sxs-lookup"><span data-stu-id="7188c-124">That's the only application change required.</span></span> <span data-ttu-id="7188c-125">Gå vidare och skapa och publicera programmet.</span><span class="sxs-lookup"><span data-stu-id="7188c-125">Go ahead and build and publish the application.</span></span>

## <a name="step-2-add-a-userid-column-to-the-database-schema"></a><span data-ttu-id="7188c-126">Steg 2: Lägg till en kolumn för användar-ID databasschemat</span><span class="sxs-lookup"><span data-stu-id="7188c-126">Step 2: Add a UserId column to the database schema</span></span>
<span data-ttu-id="7188c-127">Nu ska behöver vi lägga till en användar-ID-kolumnen i tabellen kontakter associera varje rad med en användare (klient).</span><span class="sxs-lookup"><span data-stu-id="7188c-127">Next, we need to add a UserId column to the Contacts table to associate each row with a user (tenant).</span></span> <span data-ttu-id="7188c-128">Vi ändrar schemat direkt i databasen, så att vi inte behöver inkludera fältet i vår EF datamodell.</span><span class="sxs-lookup"><span data-stu-id="7188c-128">We will alter the schema directly in the database, so that we don't have to include this field in our EF data model.</span></span>

<span data-ttu-id="7188c-129">Ansluta till databasen direkt, med hjälp av SQL Server Management Studio eller Visual Studio och kör följande T-SQL:</span><span class="sxs-lookup"><span data-stu-id="7188c-129">Connect to the database directly, using either SQL Server Management Studio or Visual Studio, and then execute the following T-SQL:</span></span>

```
ALTER TABLE Contacts ADD UserId nvarchar(128)
    DEFAULT CAST(SESSION_CONTEXT(N'UserId') AS nvarchar(128))
```

<span data-ttu-id="7188c-130">Detta lägger till en användar-ID-kolumnen i tabellen kontakter.</span><span class="sxs-lookup"><span data-stu-id="7188c-130">This adds a UserId column to the Contacts table.</span></span> <span data-ttu-id="7188c-131">Vi använder datatypen nvarchar(128) för att matcha UserIds lagras i tabellen AspNetUsers och vi skapa en Standardbegränsning som automatiskt in användar-ID för nya infogade rader ska användar-ID som för närvarande lagras i SESSION_CONTEXT.</span><span class="sxs-lookup"><span data-stu-id="7188c-131">We use the nvarchar(128) data type to match the UserIds stored in the AspNetUsers table, and we create a DEFAULT constraint that will automatically set the UserId for newly inserted rows to be the UserId currently stored in SESSION_CONTEXT.</span></span>

<span data-ttu-id="7188c-132">Nu tabellen ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="7188c-132">Now the table looks like this:</span></span>

![Tabellen SSMS kontakter](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-Contacts.png)

<span data-ttu-id="7188c-134">När nya kontakter har skapats, kommer de automatiskt tilldelade rätt användar-ID.</span><span class="sxs-lookup"><span data-stu-id="7188c-134">When new contacts are created, they'll automatically be assigned the correct UserId.</span></span> <span data-ttu-id="7188c-135">Demonstration, men vi tilldela några av dessa befintliga kontakter till en befintlig användare.</span><span class="sxs-lookup"><span data-stu-id="7188c-135">For demo purposes, however, let's assign a few of these existing contacts to an existing user.</span></span>

<span data-ttu-id="7188c-136">Om du har skapat ett fåtal användare i program redan (t.ex. använda lokala Google eller Facebook konton), ser du dem i tabellen AspNetUsers.</span><span class="sxs-lookup"><span data-stu-id="7188c-136">If you've created a few users in the application already (e.g., using local, Google, or Facebook accounts), you'll see them in the AspNetUsers table.</span></span> <span data-ttu-id="7188c-137">I skärmbilden nedan finns bara en användare hittills.</span><span class="sxs-lookup"><span data-stu-id="7188c-137">In the screenshot below, there is only one user so far.</span></span>

![SSMS AspNetUsers tabell](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-AspNetUsers.png)

<span data-ttu-id="7188c-139">Kopiera Id för user1@contoso.com, och klistra in den i T-SQL-instruktionen nedan.</span><span class="sxs-lookup"><span data-stu-id="7188c-139">Copy the Id for user1@contoso.com, and paste it into the T-SQL statement below.</span></span> <span data-ttu-id="7188c-140">Köra den här instruktionen om du vill koppla tre kontakter till den här användar-ID.</span><span class="sxs-lookup"><span data-stu-id="7188c-140">Execute this statement to associate three of the Contacts with this UserId.</span></span>

```
UPDATE Contacts SET UserId = '19bc9b0d-28dd-4510-bd5e-d6b6d445f511'
WHERE ContactId IN (1, 2, 5)
```

## <a name="step-3-create-a-row-level-security-policy-in-the-database"></a><span data-ttu-id="7188c-141">Steg 3: Skapa en princip för säkerhet på radnivå i databasen</span><span class="sxs-lookup"><span data-stu-id="7188c-141">Step 3: Create a Row-Level Security policy in the database</span></span>
<span data-ttu-id="7188c-142">Det sista steget är att skapa en säkerhetsprincip som använder det användar-ID i SESSION_CONTEXT automatiskt filtrera resultaten som returnerades av frågor.</span><span class="sxs-lookup"><span data-stu-id="7188c-142">The final step is to create a security policy that uses the UserId in SESSION_CONTEXT to automatically filter the results returned by queries.</span></span>

<span data-ttu-id="7188c-143">Medan du fortfarande är anslutna till databasen, kör följande T-SQL:</span><span class="sxs-lookup"><span data-stu-id="7188c-143">While still connected to the database, execute the following T-SQL:</span></span>

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

<span data-ttu-id="7188c-144">Den här koden har tre saker.</span><span class="sxs-lookup"><span data-stu-id="7188c-144">This code does three things.</span></span> <span data-ttu-id="7188c-145">Först skapar den ett nytt schema som bästa praxis för att centralisera och begränsa åtkomsten till RLS-objekt.</span><span class="sxs-lookup"><span data-stu-id="7188c-145">First, it creates a new schema as a best practice for centralizing and limiting access to the RLS objects.</span></span> <span data-ttu-id="7188c-146">Därefter skapar den en predikatfunktionens som returnerar '1' när en rad användar-ID matchar användar-ID i SESSION_CONTEXT.</span><span class="sxs-lookup"><span data-stu-id="7188c-146">Next, it creates a predicate function that will return '1' when the UserId of a row matches the UserId in SESSION_CONTEXT.</span></span> <span data-ttu-id="7188c-147">Slutligen skapar den en säkerhetsprincip som lägger till den här funktionen som ett filter och ett block-predikat för tabellen kontakter.</span><span class="sxs-lookup"><span data-stu-id="7188c-147">Finally, it creates a security policy that adds this function as both a filter and block predicate on the Contacts table.</span></span> <span data-ttu-id="7188c-148">Filterpredikatet orsakar frågor för att returnera rader som hör till den aktuella användaren och block-predikatet fungerar som ett skydd för programmet att infoga en rad för fel användaren någonsin av misstag.</span><span class="sxs-lookup"><span data-stu-id="7188c-148">The filter predicate causes queries to return only rows that belong to the current user, and the block predicate acts as a safeguard to prevent the application from ever accidentally inserting a row for the wrong user.</span></span>

<span data-ttu-id="7188c-149">Nu kör programmet och logga in som user1@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="7188c-149">Now run the application, and sign in as user1@contoso.com.</span></span> <span data-ttu-id="7188c-150">Den här användaren ser nu endast kontakter som vi tidigare tilldelats till den här användar-ID:</span><span class="sxs-lookup"><span data-stu-id="7188c-150">This user now sees only the Contacts we assigned to this UserId earlier:</span></span>

![Kontakta Manager-program innan du aktiverar RLS](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-After.png)

<span data-ttu-id="7188c-152">Om du vill kontrollera detta ytterligare försök att registrera en ny användare.</span><span class="sxs-lookup"><span data-stu-id="7188c-152">To validate this further, try registering a new user.</span></span> <span data-ttu-id="7188c-153">Användaren ser inga kontakter eftersom ingen har tilldelats.</span><span class="sxs-lookup"><span data-stu-id="7188c-153">They will see no contacts, because none have been assigned to them.</span></span> <span data-ttu-id="7188c-154">Om de skapar en ny kontakt, tilldelas till dem och endast de kommer att kunna se den.</span><span class="sxs-lookup"><span data-stu-id="7188c-154">If they create a new contact, it will be assigned to them, and only they will be able to see it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7188c-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7188c-155">Next steps</span></span>
<span data-ttu-id="7188c-156">Klart!</span><span class="sxs-lookup"><span data-stu-id="7188c-156">That's it!</span></span> <span data-ttu-id="7188c-157">Enkel kontakta Manager webbprogrammet har konverterats till ett flera innehavare en där varje användare har sin egen kontaktlista.</span><span class="sxs-lookup"><span data-stu-id="7188c-157">The simple Contact Manager web app has been converted into a multi-tenant one where each user has its own contact list.</span></span> <span data-ttu-id="7188c-158">Med hjälp av säkerhet på radnivå har vi undvikas komplexitet tvingande klient åtkomst logiken i vår programkod.</span><span class="sxs-lookup"><span data-stu-id="7188c-158">By using Row-Level Security, we've avoided the complexity of enforcing tenant access logic in our application code.</span></span> <span data-ttu-id="7188c-159">Den här genomskinlighet tillåter programmet att fokusera på den verkliga affärsproblem till hands och minskar också risken för sprids av misstag data som programmet har codebase växer.</span><span class="sxs-lookup"><span data-stu-id="7188c-159">This transparency allows the application to focus on the real business problem at hand, and it also reduces the risk of accidentally leaking data as the application's codebase grows.</span></span>

<span data-ttu-id="7188c-160">Den här självstudiekursen har endast skadad yta vad du kan göra med RLS.</span><span class="sxs-lookup"><span data-stu-id="7188c-160">This tutorial has only scratched the surface of what's possible with RLS.</span></span> <span data-ttu-id="7188c-161">Exempelvis är det möjligt att ha fler sofistikerade eller detaljerade åtkomst logik, och den är möjligt att lagra mer än bara den aktuella användar-ID i SESSION_CONTEXT.</span><span class="sxs-lookup"><span data-stu-id="7188c-161">For instance, it's possible to have more sophisticated or granular access logic, and it's possible to store more than just the current UserId in the SESSION_CONTEXT.</span></span> <span data-ttu-id="7188c-162">Det är också möjligt att [integrera RLS med klientbibliotek för elastisk databas verktyg](../sql-database/sql-database-elastic-tools-multi-tenant-row-level-security.md) till stöd för flera innehavare delar i en skalbar datanivå.</span><span class="sxs-lookup"><span data-stu-id="7188c-162">It's also possible to [integrate RLS with the elastic database tools client libraries](../sql-database/sql-database-elastic-tools-multi-tenant-row-level-security.md) to support multi-tenant shards in a scale-out data tier.</span></span>

<span data-ttu-id="7188c-163">Utöver dessa möjligheter arbetar vi också för att göra RLS ännu bättre.</span><span class="sxs-lookup"><span data-stu-id="7188c-163">Beyond these possibilities, we're also working to make RLS even better.</span></span> <span data-ttu-id="7188c-164">Om du har några frågor, idéer eller saker som du skulle vilja se berätta i kommentarerna.</span><span class="sxs-lookup"><span data-stu-id="7188c-164">If you have any questions, ideas, or things you'd like to see, please let us know in the comments.</span></span> <span data-ttu-id="7188c-165">Vi uppskattar din feedback!</span><span class="sxs-lookup"><span data-stu-id="7188c-165">We appreciate your feedback!</span></span>

