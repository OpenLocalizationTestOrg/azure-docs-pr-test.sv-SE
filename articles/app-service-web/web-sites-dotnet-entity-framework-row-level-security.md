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
# <a name="tutorial-web-app-with-a-multi-tenant-database-using-entity-framework-and-row-level-security"></a><span data-ttu-id="318dc-103">Självstudier: Webbprogram med en databas för flera innehavare med hjälp av Entity Framework och säkerhet på radnivå</span><span class="sxs-lookup"><span data-stu-id="318dc-103">Tutorial: Web app with a multi-tenant database using Entity Framework and Row-Level Security</span></span>
<span data-ttu-id="318dc-104">Den här kursen visar hur toobuild flera innehavare web app med en ”[delad databas, delade schema](https://msdn.microsoft.com/library/aa479086.aspx)” innehavare modell, och använder Entity Framework och [säkerhet på radnivå](https://msdn.microsoft.com/library/dn765131.aspx).</span><span class="sxs-lookup"><span data-stu-id="318dc-104">This tutorial shows how toobuild a multi-tenant web app with a "[shared database, shared schema](https://msdn.microsoft.com/library/aa479086.aspx)" tenancy model, using Entity Framework and [Row-Level Security](https://msdn.microsoft.com/library/dn765131.aspx).</span></span> <span data-ttu-id="318dc-105">En enskild databas innehåller för många hyresgäster i den här modellen, och varje rad i varje tabell som är associerad med en ”klient-ID”</span><span class="sxs-lookup"><span data-stu-id="318dc-105">In this model, a single database contains data for many tenants, and each row in each table is associated with a "Tenant ID."</span></span> <span data-ttu-id="318dc-106">Radnivå säkerhet (RLS), en ny funktion för Azure SQL-databas är används tooprevent klienter från att komma åt varandras data.</span><span class="sxs-lookup"><span data-stu-id="318dc-106">Row-Level Security (RLS), a new feature for Azure SQL Database, is used tooprevent tenants from accessing each other's data.</span></span> <span data-ttu-id="318dc-107">Detta kräver en enskild, mindre ändring toohello tillämpning.</span><span class="sxs-lookup"><span data-stu-id="318dc-107">This requires just a single, small change toohello application.</span></span> <span data-ttu-id="318dc-108">Genom att centralisera hello klient åtkomst logik i själva hello-databasen, RLS förenklar hello programkod och minskar hello risk för oavsiktliga dataläckage mellan klienter behålls.</span><span class="sxs-lookup"><span data-stu-id="318dc-108">By centralizing hello tenant access logic within hello database itself, RLS simplifies hello application code and reduces hello risk of accidental data leakage between tenants.</span></span>

<span data-ttu-id="318dc-109">Vi börjar med hello enkelt Contact Manager program från [skapa en ASP.NET-MVP-app med autentisering och SQL-databas och distribuera tooAzure Apptjänst](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="318dc-109">Let's start with hello simple Contact Manager application from [Create an ASP.NET MVP app with auth and SQL DB and deploy tooAzure App Service](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).</span></span> <span data-ttu-id="318dc-110">Höger nu hello program kan alla användare (klienter) toosee alla kontakter:</span><span class="sxs-lookup"><span data-stu-id="318dc-110">Right now, hello application allows all users (tenants) toosee all contacts:</span></span>

![Kontakta Manager-program innan du aktiverar RLS](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-Before.png)

<span data-ttu-id="318dc-112">Med bara några mindre ändringar kommer vi lägga till stöd för flera innehavare så att användare kan toosee endast hello kontakter som tillhör toothem.</span><span class="sxs-lookup"><span data-stu-id="318dc-112">With just a few small changes, we will add support for multi-tenancy, so that users are able toosee only hello contacts that belong toothem.</span></span>

## <a name="step-1-add-an-interceptor-class-in-hello-application-tooset-hello-sessioncontext"></a><span data-ttu-id="318dc-113">Steg 1: Lägg till en spärren klass i hello programmet tooset hello SESSION_CONTEXT</span><span class="sxs-lookup"><span data-stu-id="318dc-113">Step 1: Add an Interceptor class in hello application tooset hello SESSION_CONTEXT</span></span>
<span data-ttu-id="318dc-114">Det finns ett program ändra vi behöver toomake.</span><span class="sxs-lookup"><span data-stu-id="318dc-114">There is one application change we need toomake.</span></span> <span data-ttu-id="318dc-115">Eftersom alla programanvändare ansluta toohello databas med hjälp av Hej samma anslutningssträng (d.v.s. samma SQL-inloggning), det går för närvarande inte för en RLS princip tooknow som den ska filtrera efter.</span><span class="sxs-lookup"><span data-stu-id="318dc-115">Because all application users connect toohello database using hello same connection string (i.e. same SQL login), there is currently no way for an RLS policy tooknow which user it should filter for.</span></span> <span data-ttu-id="318dc-116">Den här metoden är väldigt vanligt i webbprogram eftersom det möjliggör effektiv anslutningspoolning, men det innebär att vi behöver ett annat sätt tooidentify hello aktuell programanvändare hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="318dc-116">This approach is very common in web applications because it enables efficient connection pooling, but it means we need another way tooidentify hello current application user within hello database.</span></span> <span data-ttu-id="318dc-117">hello lösningen är toohave hello program ange ett nyckel / värde-par för hello aktuella användar-ID i hello [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806) omedelbart efter att öppna en anslutning innan den kör frågor.</span><span class="sxs-lookup"><span data-stu-id="318dc-117">hello solution is toohave hello application set a key-value pair for hello current UserId in hello [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806) immediately after opening a connection, before it executes any queries.</span></span> <span data-ttu-id="318dc-118">SESSION_CONTEXT är en session som definitionsområde nyckelvärdeslager och policyn RLS använder hello UserId lagras i den aktuella användaren för tooidentify hello.</span><span class="sxs-lookup"><span data-stu-id="318dc-118">SESSION_CONTEXT is a session-scoped key-value store, and our RLS policy will use hello UserId stored in it tooidentify hello current user.</span></span>

<span data-ttu-id="318dc-119">Vi lägger till en [spärren](https://msdn.microsoft.com/data/dn469464.aspx) (särskilt en [DbConnectionInterceptor](https://msdn.microsoft.com/library/system.data.entity.infrastructure.interception.idbconnectioninterceptor)), en ny funktion i Entity Framework (EF) 6, tooautomatically set hello aktuella användar-ID i hello SESSION_CONTEXT genom att köra en T-SQL-instruktionen när EF öppnar en anslutning.</span><span class="sxs-lookup"><span data-stu-id="318dc-119">We will add an [interceptor](https://msdn.microsoft.com/data/dn469464.aspx) (in particular, a [DbConnectionInterceptor](https://msdn.microsoft.com/library/system.data.entity.infrastructure.interception.idbconnectioninterceptor)), a new feature in Entity Framework (EF) 6, tooautomatically set hello current UserId in hello SESSION_CONTEXT by executing a T-SQL statement whenever EF opens a connection.</span></span>

1. <span data-ttu-id="318dc-120">Öppna hello ContactManager projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="318dc-120">Open hello ContactManager project in Visual Studio.</span></span>
2. <span data-ttu-id="318dc-121">Högerklicka på hello modellmappen i hello Solution Explorer och välj sedan Lägg till > klass.</span><span class="sxs-lookup"><span data-stu-id="318dc-121">Right-click on hello Models folder in hello Solution Explorer, and choose Add > Class.</span></span>
3. <span data-ttu-id="318dc-122">Namnge hello nya klassen ”SessionContextInterceptor.cs” och klicka på Lägg till.</span><span class="sxs-lookup"><span data-stu-id="318dc-122">Name hello new class "SessionContextInterceptor.cs" and click Add.</span></span>
4. <span data-ttu-id="318dc-123">Ersätt hello innehållet i SessionContextInterceptor.cs med hello följande kod.</span><span class="sxs-lookup"><span data-stu-id="318dc-123">Replace hello contents of SessionContextInterceptor.cs with hello following code.</span></span>

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

<span data-ttu-id="318dc-124">Det är endast programmet hello ändras.</span><span class="sxs-lookup"><span data-stu-id="318dc-124">That's hello only application change required.</span></span> <span data-ttu-id="318dc-125">Gå vidare och skapa och publicera programmet hello.</span><span class="sxs-lookup"><span data-stu-id="318dc-125">Go ahead and build and publish hello application.</span></span>

## <a name="step-2-add-a-userid-column-toohello-database-schema"></a><span data-ttu-id="318dc-126">Steg 2: Lägg till ett användar-ID för kolumnen toohello databasschema</span><span class="sxs-lookup"><span data-stu-id="318dc-126">Step 2: Add a UserId column toohello database schema</span></span>
<span data-ttu-id="318dc-127">Därefter måste tooadd en UserId kolumnen toohello kontakter tabell tooassociate varje rad med en användare (klient).</span><span class="sxs-lookup"><span data-stu-id="318dc-127">Next, we need tooadd a UserId column toohello Contacts table tooassociate each row with a user (tenant).</span></span> <span data-ttu-id="318dc-128">Vi ändrar hello schema direkt i hello-databasen, så att vi inte har tooinclude det här fältet i vår EF datamodell.</span><span class="sxs-lookup"><span data-stu-id="318dc-128">We will alter hello schema directly in hello database, so that we don't have tooinclude this field in our EF data model.</span></span>

<span data-ttu-id="318dc-129">Ansluta toohello databasen direkt, med hjälp av SQL Server Management Studio eller Visual Studio och kör följande T-SQL hello:</span><span class="sxs-lookup"><span data-stu-id="318dc-129">Connect toohello database directly, using either SQL Server Management Studio or Visual Studio, and then execute hello following T-SQL:</span></span>

```
ALTER TABLE Contacts ADD UserId nvarchar(128)
    DEFAULT CAST(SESSION_CONTEXT(N'UserId') AS nvarchar(128))
```

<span data-ttu-id="318dc-130">Detta lägger till en tabell med användar-ID kolumn toohello kontakter.</span><span class="sxs-lookup"><span data-stu-id="318dc-130">This adds a UserId column toohello Contacts table.</span></span> <span data-ttu-id="318dc-131">Vi använder hello nvarchar(128) data typen toomatch hello UserIds lagras i hello AspNetUsers tabell och vi skapa en Standardbegränsning som automatiskt in hello användar-ID för nya infogade rader toobe hello användar-ID som för närvarande lagras i SESSION_CONTEXT.</span><span class="sxs-lookup"><span data-stu-id="318dc-131">We use hello nvarchar(128) data type toomatch hello UserIds stored in hello AspNetUsers table, and we create a DEFAULT constraint that will automatically set hello UserId for newly inserted rows toobe hello UserId currently stored in SESSION_CONTEXT.</span></span>

<span data-ttu-id="318dc-132">Nu hello tabellen ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="318dc-132">Now hello table looks like this:</span></span>

![Tabellen SSMS kontakter](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-Contacts.png)

<span data-ttu-id="318dc-134">När nya kontakter skapas ska tilldelas de automatiskt att hello korrigera användar-ID.</span><span class="sxs-lookup"><span data-stu-id="318dc-134">When new contacts are created, they'll automatically be assigned hello correct UserId.</span></span> <span data-ttu-id="318dc-135">För demonstration, men vi tilldela några av dessa befintliga kontakter tooan befintliga användare.</span><span class="sxs-lookup"><span data-stu-id="318dc-135">For demo purposes, however, let's assign a few of these existing contacts tooan existing user.</span></span>

<span data-ttu-id="318dc-136">Om du har skapat ett fåtal användare i hello programmet redan (t.ex. använda lokala Google eller Facebook konton), ser du dem i hello AspNetUsers tabell.</span><span class="sxs-lookup"><span data-stu-id="318dc-136">If you've created a few users in hello application already (e.g., using local, Google, or Facebook accounts), you'll see them in hello AspNetUsers table.</span></span> <span data-ttu-id="318dc-137">I hello skärmbilden nedan finns bara en användare hittills.</span><span class="sxs-lookup"><span data-stu-id="318dc-137">In hello screenshot below, there is only one user so far.</span></span>

![SSMS AspNetUsers tabell](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-AspNetUsers.png)

<span data-ttu-id="318dc-139">Kopiera hello Id för user1@contoso.com, och klistra in den i hello T-SQL-instruktionen nedan.</span><span class="sxs-lookup"><span data-stu-id="318dc-139">Copy hello Id for user1@contoso.com, and paste it into hello T-SQL statement below.</span></span> <span data-ttu-id="318dc-140">Köra den här instruktionen tooassociate tre hello kontakter med det här användar-ID.</span><span class="sxs-lookup"><span data-stu-id="318dc-140">Execute this statement tooassociate three of hello Contacts with this UserId.</span></span>

```
UPDATE Contacts SET UserId = '19bc9b0d-28dd-4510-bd5e-d6b6d445f511'
WHERE ContactId IN (1, 2, 5)
```

## <a name="step-3-create-a-row-level-security-policy-in-hello-database"></a><span data-ttu-id="318dc-141">Steg 3: Skapa en princip för säkerhet på radnivå i hello-databas</span><span class="sxs-lookup"><span data-stu-id="318dc-141">Step 3: Create a Row-Level Security policy in hello database</span></span>
<span data-ttu-id="318dc-142">hello sista steget är toocreate en säkerhetsprincip som använder hello användar-ID i SESSION_CONTEXT tooautomatically filter hello resultaten som returnerades av frågor.</span><span class="sxs-lookup"><span data-stu-id="318dc-142">hello final step is toocreate a security policy that uses hello UserId in SESSION_CONTEXT tooautomatically filter hello results returned by queries.</span></span>

<span data-ttu-id="318dc-143">När fortfarande anslutna toohello databas kör du hello följande T-SQL:</span><span class="sxs-lookup"><span data-stu-id="318dc-143">While still connected toohello database, execute hello following T-SQL:</span></span>

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

<span data-ttu-id="318dc-144">Den här koden har tre saker.</span><span class="sxs-lookup"><span data-stu-id="318dc-144">This code does three things.</span></span> <span data-ttu-id="318dc-145">Först skapar den ett nytt schema som bästa praxis för att centralisera och begränsa åtkomst toohello RLS objekt.</span><span class="sxs-lookup"><span data-stu-id="318dc-145">First, it creates a new schema as a best practice for centralizing and limiting access toohello RLS objects.</span></span> <span data-ttu-id="318dc-146">Därefter skapar den en predikatfunktionens som returnerar '1' när hello användar-ID för en rad matchar hello användar-ID i SESSION_CONTEXT.</span><span class="sxs-lookup"><span data-stu-id="318dc-146">Next, it creates a predicate function that will return '1' when hello UserId of a row matches hello UserId in SESSION_CONTEXT.</span></span> <span data-ttu-id="318dc-147">Slutligen skapar den en säkerhetsprincip som lägger till den här funktionen som ett filter och ett block-predikat för tabellen för kontakter hello.</span><span class="sxs-lookup"><span data-stu-id="318dc-147">Finally, it creates a security policy that adds this function as both a filter and block predicate on hello Contacts table.</span></span> <span data-ttu-id="318dc-148">hello filterpredikat orsakar frågor tooreturn endast rader som tillhör toohello aktuell användare och hello block-predikatet fungerar som skyddar tooprevent hello program från att någonsin av misstag infoga en rad för hello fel användaren.</span><span class="sxs-lookup"><span data-stu-id="318dc-148">hello filter predicate causes queries tooreturn only rows that belong toohello current user, and hello block predicate acts as a safeguard tooprevent hello application from ever accidentally inserting a row for hello wrong user.</span></span>

<span data-ttu-id="318dc-149">Nu kör hello programmet och logga in som user1@contoso.com. Den här användaren ser nu endast hello kontakter som har tilldelats tidigare toothis användar-ID:</span><span class="sxs-lookup"><span data-stu-id="318dc-149">Now run hello application, and sign in as user1@contoso.com. This user now sees only hello Contacts we assigned toothis UserId earlier:</span></span>

![Kontakta Manager-program innan du aktiverar RLS](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-After.png)

<span data-ttu-id="318dc-151">toovalidate detta ytterligare försök att registrera en ny användare.</span><span class="sxs-lookup"><span data-stu-id="318dc-151">toovalidate this further, try registering a new user.</span></span> <span data-ttu-id="318dc-152">Användaren ser inga kontakter eftersom ingen har tilldelats toothem.</span><span class="sxs-lookup"><span data-stu-id="318dc-152">They will see no contacts, because none have been assigned toothem.</span></span> <span data-ttu-id="318dc-153">Om de skapar en ny kontakt, tilldelas toothem och endast de kommer att kunna toosee den.</span><span class="sxs-lookup"><span data-stu-id="318dc-153">If they create a new contact, it will be assigned toothem, and only they will be able toosee it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="318dc-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="318dc-154">Next steps</span></span>
<span data-ttu-id="318dc-155">Klart!</span><span class="sxs-lookup"><span data-stu-id="318dc-155">That's it!</span></span> <span data-ttu-id="318dc-156">hello enkla kontakta Manager webbprogrammet har konverterats till ett flera innehavare en där varje användare har sin egen kontaktlista.</span><span class="sxs-lookup"><span data-stu-id="318dc-156">hello simple Contact Manager web app has been converted into a multi-tenant one where each user has its own contact list.</span></span> <span data-ttu-id="318dc-157">Med hjälp av säkerhet på radnivå har vi undvikas hello komplexitet tvingande klient åtkomst logiken i vår programkod.</span><span class="sxs-lookup"><span data-stu-id="318dc-157">By using Row-Level Security, we've avoided hello complexity of enforcing tenant access logic in our application code.</span></span> <span data-ttu-id="318dc-158">Den här genomskinlighet kan hello programmet toofocus på hello verkliga affärsproblem till hands och hello risken för sprids av misstag data som programmet hello har codebase minskar också växer.</span><span class="sxs-lookup"><span data-stu-id="318dc-158">This transparency allows hello application toofocus on hello real business problem at hand, and it also reduces hello risk of accidentally leaking data as hello application's codebase grows.</span></span>

<span data-ttu-id="318dc-159">Den här självstudiekursen har endast repad hello yta vad du kan göra med RLS.</span><span class="sxs-lookup"><span data-stu-id="318dc-159">This tutorial has only scratched hello surface of what's possible with RLS.</span></span> <span data-ttu-id="318dc-160">Exempelvis är det möjligt toohave mer avancerade eller detaljerade åtkomst logik och möjliga toostore mer än bara hello aktuella användar-ID i hello SESSION_CONTEXT.</span><span class="sxs-lookup"><span data-stu-id="318dc-160">For instance, it's possible toohave more sophisticated or granular access logic, and it's possible toostore more than just hello current UserId in hello SESSION_CONTEXT.</span></span> <span data-ttu-id="318dc-161">Det är också möjligt för[integrera RLS med hello klientbibliotek för elastisk databas verktyg](../sql-database/sql-database-elastic-tools-multi-tenant-row-level-security.md) toosupport flera innehavare delar i en skalbar datanivå.</span><span class="sxs-lookup"><span data-stu-id="318dc-161">It's also possible too[integrate RLS with hello elastic database tools client libraries](../sql-database/sql-database-elastic-tools-multi-tenant-row-level-security.md) toosupport multi-tenant shards in a scale-out data tier.</span></span>

<span data-ttu-id="318dc-162">Utöver dessa möjligheter arbetar vi också toomake RLS ännu bättre.</span><span class="sxs-lookup"><span data-stu-id="318dc-162">Beyond these possibilities, we're also working toomake RLS even better.</span></span> <span data-ttu-id="318dc-163">Om du har några frågor, idéer eller saker som du vill att toosee berätta i hello kommentarer.</span><span class="sxs-lookup"><span data-stu-id="318dc-163">If you have any questions, ideas, or things you'd like toosee, please let us know in hello comments.</span></span> <span data-ttu-id="318dc-164">Vi uppskattar din feedback!</span><span class="sxs-lookup"><span data-stu-id="318dc-164">We appreciate your feedback!</span></span>

