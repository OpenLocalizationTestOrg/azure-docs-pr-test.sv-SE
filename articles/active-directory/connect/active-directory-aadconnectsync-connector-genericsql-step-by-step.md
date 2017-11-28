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
# <a name="generic-sql-connector-step-by-step"></a><span data-ttu-id="0ed98-103">Stegvisa anvisningar för allmän SQL-anslutningsapp</span><span class="sxs-lookup"><span data-stu-id="0ed98-103">Generic SQL Connector step-by-step</span></span>
<span data-ttu-id="0ed98-104">Det här avsnittet finns stegvisa instruktioner.</span><span class="sxs-lookup"><span data-stu-id="0ed98-104">This topic is a step-by-step guide.</span></span> <span data-ttu-id="0ed98-105">Den skapar en enkel HR-exempeldatabas och använda den för att importera vissa användare och deras gruppmedlemskap.</span><span class="sxs-lookup"><span data-stu-id="0ed98-105">It creates a simple sample HR database and use it for importing some users and their group membership.</span></span>

## <a name="prepare-hello-sample-database"></a><span data-ttu-id="0ed98-106">Förbereda hello exempeldatabasen</span><span class="sxs-lookup"><span data-stu-id="0ed98-106">Prepare hello sample database</span></span>
<span data-ttu-id="0ed98-107">På en server som kör SQL Server, kör du hello SQL-skript finns i [bilaga A](#appendix-a). Det här skriptet skapar en exempeldatabas med hello namnet GSQLDEMO.</span><span class="sxs-lookup"><span data-stu-id="0ed98-107">On a server running SQL Server, run hello SQL script found in [Appendix A](#appendix-a). This script creates a sample database with hello name GSQLDEMO.</span></span> <span data-ttu-id="0ed98-108">hello objektmodell för hello skapade databasen ser ut som om den här bilden:</span><span class="sxs-lookup"><span data-stu-id="0ed98-108">hello object model for hello created database looks like this picture:</span></span>  
<span data-ttu-id="0ed98-109">![Objektmodell](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/objectmodel.png)</span><span class="sxs-lookup"><span data-stu-id="0ed98-109">![Object Model](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/objectmodel.png)</span></span>

<span data-ttu-id="0ed98-110">Skapa även en användare som du vill toouse tooconnect toohello databasen.</span><span class="sxs-lookup"><span data-stu-id="0ed98-110">Also create a user you want toouse tooconnect toohello database.</span></span> <span data-ttu-id="0ed98-111">I den här genomgången är hello användaren kallas FABRIKAM\SQLUser och finns i hello domän.</span><span class="sxs-lookup"><span data-stu-id="0ed98-111">In this walkthrough, hello user is called FABRIKAM\SQLUser and located in hello domain.</span></span>

## <a name="create-hello-odbc-connection-file"></a><span data-ttu-id="0ed98-112">Skapa hello ODBC-anslutningsfil</span><span class="sxs-lookup"><span data-stu-id="0ed98-112">Create hello ODBC connection file</span></span>
<span data-ttu-id="0ed98-113">hello allmän SQL-anslutningen använder ODBC tooconnect toohello-fjärrservern.</span><span class="sxs-lookup"><span data-stu-id="0ed98-113">hello Generic SQL Connector is using ODBC tooconnect toohello remote server.</span></span> <span data-ttu-id="0ed98-114">Vi måste först toocreate en fil med hello anslutningsinformationen för ODBC.</span><span class="sxs-lookup"><span data-stu-id="0ed98-114">First we need toocreate a file with hello ODBC connection information.</span></span>

1. <span data-ttu-id="0ed98-115">Starta hello ODBC-hanteringsverktyg på servern:</span><span class="sxs-lookup"><span data-stu-id="0ed98-115">Start hello ODBC management utility on your server:</span></span>  
   ![ODBC](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc.png)
2. <span data-ttu-id="0ed98-117">Välj hello fliken **fil-DSN**.</span><span class="sxs-lookup"><span data-stu-id="0ed98-117">Select hello tab **File DSN**.</span></span> <span data-ttu-id="0ed98-118">Klicka på **Lägg till... **.</span><span class="sxs-lookup"><span data-stu-id="0ed98-118">Click **Add...**.</span></span>  
   <span data-ttu-id="0ed98-119">![ODBC1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc1.png)</span><span class="sxs-lookup"><span data-stu-id="0ed98-119">![ODBC1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc1.png)</span></span>
3. <span data-ttu-id="0ed98-120">hello out-of-box-drivrutinen fungerar finjustering så markerar du den och klicka på **Nästa >**.</span><span class="sxs-lookup"><span data-stu-id="0ed98-120">hello out-of-box driver works fine, so select it and click **Next>**.</span></span>  
   <span data-ttu-id="0ed98-121">![ODBC2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc2.png)</span><span class="sxs-lookup"><span data-stu-id="0ed98-121">![ODBC2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc2.png)</span></span>
4. <span data-ttu-id="0ed98-122">Ge hello filen ett namn som **GenericSQL**.</span><span class="sxs-lookup"><span data-stu-id="0ed98-122">Give hello file a name, such as **GenericSQL**.</span></span>  
   <span data-ttu-id="0ed98-123">![ODBC3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc3.png)</span><span class="sxs-lookup"><span data-stu-id="0ed98-123">![ODBC3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc3.png)</span></span>
5. <span data-ttu-id="0ed98-124">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="0ed98-124">Click **Finish**.</span></span>  
   <span data-ttu-id="0ed98-125">![ODBC4](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc4.png)</span><span class="sxs-lookup"><span data-stu-id="0ed98-125">![ODBC4](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc4.png)</span></span>
6. <span data-ttu-id="0ed98-126">Tid tooconfigure hello-anslutning.</span><span class="sxs-lookup"><span data-stu-id="0ed98-126">Time tooconfigure hello connection.</span></span> <span data-ttu-id="0ed98-127">Ger en bra beskrivning för hello datakällan och ange hello namnet på hello-server som kör SQL Server.</span><span class="sxs-lookup"><span data-stu-id="0ed98-127">Give hello data source a good description and provide hello name of hello server running SQL Server.</span></span>  
   ![ODBC5](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc5.png)
7. <span data-ttu-id="0ed98-129">Välj hur tooauthenticate med SQL.</span><span class="sxs-lookup"><span data-stu-id="0ed98-129">Select how tooauthenticate with SQL.</span></span> <span data-ttu-id="0ed98-130">I detta fall kan använda vi Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="0ed98-130">In this case, we use Windows Authentication.</span></span>  
   ![ODBC6](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc6.png)
8. <span data-ttu-id="0ed98-132">Ange hello namnet på hello exempeldatabasen **GSQLDEMO**.</span><span class="sxs-lookup"><span data-stu-id="0ed98-132">Provide hello name of hello sample database, **GSQLDEMO**.</span></span>  
   <span data-ttu-id="0ed98-133">![ODBC7](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc7.png)</span><span class="sxs-lookup"><span data-stu-id="0ed98-133">![ODBC7](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc7.png)</span></span>
9. <span data-ttu-id="0ed98-134">Behåll allt standardvärdet på den här skärmen.</span><span class="sxs-lookup"><span data-stu-id="0ed98-134">Keep everything default on this screen.</span></span> <span data-ttu-id="0ed98-135">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="0ed98-135">Click **Finish**.</span></span>  
   <span data-ttu-id="0ed98-136">![ODBC8](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc8.png)</span><span class="sxs-lookup"><span data-stu-id="0ed98-136">![ODBC8](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc8.png)</span></span>
10. <span data-ttu-id="0ed98-137">tooverify allt fungerar som förväntat, klickar du på **testa datakällan**.</span><span class="sxs-lookup"><span data-stu-id="0ed98-137">tooverify everything is working as expected, click **Test Data Source**.</span></span>  
    <span data-ttu-id="0ed98-138">![ODBC9](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc9.png)</span><span class="sxs-lookup"><span data-stu-id="0ed98-138">![ODBC9](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc9.png)</span></span>
11. <span data-ttu-id="0ed98-139">Kontrollera att hello test har lyckats.</span><span class="sxs-lookup"><span data-stu-id="0ed98-139">Make sure hello test is successful.</span></span>  
    ![ODBC10](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc10.png)
12. <span data-ttu-id="0ed98-141">konfigurationsfilen för hello ODBC ska nu visas i fil-DSN.</span><span class="sxs-lookup"><span data-stu-id="0ed98-141">hello ODBC configuration file should now be visible in File DSN.</span></span>  
    ![ODBC11](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc11.png)

<span data-ttu-id="0ed98-143">Nu har vi hello filen vi behöver och kan börja skapa hello Connector.</span><span class="sxs-lookup"><span data-stu-id="0ed98-143">We now have hello file we need and can start creating hello Connector.</span></span>

## <a name="create-hello-generic-sql-connector"></a><span data-ttu-id="0ed98-144">Skapa hello allmän SQL-koppling</span><span class="sxs-lookup"><span data-stu-id="0ed98-144">Create hello Generic SQL Connector</span></span>
1. <span data-ttu-id="0ed98-145">Välj i hello Synchronization Service Manager UI, **kopplingar** och **skapa**.</span><span class="sxs-lookup"><span data-stu-id="0ed98-145">In hello Synchronization Service Manager UI, select **Connectors** and **Create**.</span></span> <span data-ttu-id="0ed98-146">Välj **generiskt SQL (Microsoft)** och ge det ett beskrivande namn.</span><span class="sxs-lookup"><span data-stu-id="0ed98-146">Select **Generic SQL (Microsoft)** and give it a descriptive name.</span></span>  
   <span data-ttu-id="0ed98-147">![Connector1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector1.png)</span><span class="sxs-lookup"><span data-stu-id="0ed98-147">![Connector1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector1.png)</span></span>
2. <span data-ttu-id="0ed98-148">Hitta hello DSN-fil som du skapade i föregående avsnitt i hello och överför den toohello server.</span><span class="sxs-lookup"><span data-stu-id="0ed98-148">Find hello DSN file you created in hello previous section and upload it toohello server.</span></span> <span data-ttu-id="0ed98-149">Ange hello referenser tooconnect toohello i databasen.</span><span class="sxs-lookup"><span data-stu-id="0ed98-149">Provide hello credentials tooconnect toohello database.</span></span>  
   ![Connector2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector2.png)
3. <span data-ttu-id="0ed98-151">I den här genomgången vi gör det enkelt för oss och säger att det finns två objekttyper **användare** och **gruppen**.</span><span class="sxs-lookup"><span data-stu-id="0ed98-151">In this walkthrough, we are making it easy for us and say that there are two object types, **User** and **Group**.</span></span>
   <span data-ttu-id="0ed98-152">![Connector3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector3.png)</span><span class="sxs-lookup"><span data-stu-id="0ed98-152">![Connector3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector3.png)</span></span>
4. <span data-ttu-id="0ed98-153">toofind hello attribut, och vi vill hello Connector toodetect attributen genom att titta på själva hello-tabellen.</span><span class="sxs-lookup"><span data-stu-id="0ed98-153">toofind hello attributes, we want hello Connector toodetect those attributes by looking at hello table itself.</span></span> <span data-ttu-id="0ed98-154">Eftersom **användare** är ett reserverat ord i SQL, behöver vi tooprovide i kvadrat klammerparenteser [].</span><span class="sxs-lookup"><span data-stu-id="0ed98-154">Since **Users** is a reserved word in SQL, we need tooprovide it in square brackets [ ].</span></span>  
   <span data-ttu-id="0ed98-155">![Connector4](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector4.png)</span><span class="sxs-lookup"><span data-stu-id="0ed98-155">![Connector4](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector4.png)</span></span>
5. <span data-ttu-id="0ed98-156">Tid toodefine hello fästpunktsattributet och hello DN-attribut.</span><span class="sxs-lookup"><span data-stu-id="0ed98-156">Time toodefine hello anchor attribute and hello DN attribute.</span></span> <span data-ttu-id="0ed98-157">För **användare**, vi använda hello kombination av hello två attribut användarnamn och EmployeeID.</span><span class="sxs-lookup"><span data-stu-id="0ed98-157">For **Users**, we use hello combination of hello two attributes username and EmployeeID.</span></span> <span data-ttu-id="0ed98-158">För **grupp**, använder vi GroupName (inte realistiska i praktiken, men i den här genomgången fungerar).</span><span class="sxs-lookup"><span data-stu-id="0ed98-158">For **group**, we use GroupName (not realistic in real-life, but for this walkthrough it works).</span></span>
   <span data-ttu-id="0ed98-159">![Connector5](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector5.png)</span><span class="sxs-lookup"><span data-stu-id="0ed98-159">![Connector5](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector5.png)</span></span>
6. <span data-ttu-id="0ed98-160">Inte alla attributtyper kan identifieras i en SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="0ed98-160">Not all attribute types can be detected in a SQL database.</span></span> <span data-ttu-id="0ed98-161">hello referenstyp attribut kan inte särskilt.</span><span class="sxs-lookup"><span data-stu-id="0ed98-161">hello reference attribute type in particular cannot.</span></span> <span data-ttu-id="0ed98-162">För hello gruppen objekttyp behöver vi toochange hello OwnerID och MemberID tooreference.</span><span class="sxs-lookup"><span data-stu-id="0ed98-162">For hello group object type, we need toochange hello OwnerID and MemberID tooreference.</span></span>  
   ![Connector6](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector6.png)
7. <span data-ttu-id="0ed98-164">hello-attribut som vi markerad som referensattribut i föregående steg i hello kräver hello objekttyp dessa värden är en referens till.</span><span class="sxs-lookup"><span data-stu-id="0ed98-164">hello attributes we selected as reference attributes in hello previous step require hello object type these values are a reference to.</span></span> <span data-ttu-id="0ed98-165">I vårt fall hello objekttyp för användaren.</span><span class="sxs-lookup"><span data-stu-id="0ed98-165">In our case, hello User object type.</span></span>  
   ![Connector7](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector7.png)
8. <span data-ttu-id="0ed98-167">Hello globala parametrar på sidan Välj **vattenstämpel** som hello delta strategi.</span><span class="sxs-lookup"><span data-stu-id="0ed98-167">On hello Global Parameters page, select **Watermark** as hello delta strategy.</span></span> <span data-ttu-id="0ed98-168">Också skriva i hello datum/tid-formatet **åååå-MM-dd: mm: ss**.</span><span class="sxs-lookup"><span data-stu-id="0ed98-168">Also type in hello date/time format **yyyy-MM-dd HH:mm:ss**.</span></span>
   <span data-ttu-id="0ed98-169">![Connector8](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector8.png)</span><span class="sxs-lookup"><span data-stu-id="0ed98-169">![Connector8](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector8.png)</span></span>
9. <span data-ttu-id="0ed98-170">På hello **konfigurera partitioner och hierarkier** markerar du båda objekttyper.</span><span class="sxs-lookup"><span data-stu-id="0ed98-170">On hello **Configure Partitions and Hierarchies** page, select both object types.</span></span>
   <span data-ttu-id="0ed98-171">![Connector9](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector9.png)</span><span class="sxs-lookup"><span data-stu-id="0ed98-171">![Connector9](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector9.png)</span></span>
10. <span data-ttu-id="0ed98-172">På hello **Välj objekttyper** och **Välj attribut**, väljer både objekttyper och alla attribut.</span><span class="sxs-lookup"><span data-stu-id="0ed98-172">On hello **Select Object Types** and **Select Attributes**, select both object types and all attributes.</span></span> <span data-ttu-id="0ed98-173">På hello **konfigurera ankare** klickar du på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="0ed98-173">On hello **Configure Anchors** page, click **Finish**.</span></span>

## <a name="create-run-profiles"></a><span data-ttu-id="0ed98-174">Skapa körningsprofiler</span><span class="sxs-lookup"><span data-stu-id="0ed98-174">Create Run Profiles</span></span>
1. <span data-ttu-id="0ed98-175">Välj i hello Synchronization Service Manager UI, **kopplingar**, och **Konfigurera körningsprofiler**.</span><span class="sxs-lookup"><span data-stu-id="0ed98-175">In hello Synchronization Service Manager UI, select **Connectors**, and **Configure Run Profiles**.</span></span> <span data-ttu-id="0ed98-176">Klicka på **ny profil**.</span><span class="sxs-lookup"><span data-stu-id="0ed98-176">Click **New Profile**.</span></span> <span data-ttu-id="0ed98-177">Vi börjar med **fullständig Import**.</span><span class="sxs-lookup"><span data-stu-id="0ed98-177">We start with **Full Import**.</span></span>  
   <span data-ttu-id="0ed98-178">![Runprofile1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile1.png)</span><span class="sxs-lookup"><span data-stu-id="0ed98-178">![Runprofile1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile1.png)</span></span>
2. <span data-ttu-id="0ed98-179">Välj typ av hello **fullständig Import (endast steget)**.</span><span class="sxs-lookup"><span data-stu-id="0ed98-179">Select hello type **Full Import (Stage Only)**.</span></span>  
   <span data-ttu-id="0ed98-180">![Runprofile2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile2.png)</span><span class="sxs-lookup"><span data-stu-id="0ed98-180">![Runprofile2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile2.png)</span></span>
3. <span data-ttu-id="0ed98-181">Välj hello partition **objekt = användare**.</span><span class="sxs-lookup"><span data-stu-id="0ed98-181">Select hello partition **OBJECT=User**.</span></span>  
   <span data-ttu-id="0ed98-182">![Runprofile3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile3.png)</span><span class="sxs-lookup"><span data-stu-id="0ed98-182">![Runprofile3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile3.png)</span></span>
4. <span data-ttu-id="0ed98-183">Välj **tabell** och skriv **[användare]**.</span><span class="sxs-lookup"><span data-stu-id="0ed98-183">Select **Table** and type **[USERS]**.</span></span> <span data-ttu-id="0ed98-184">Rulla ned toohello med flera värden objektet typen avsnittet och ange hello data enligt följande bild hello.</span><span class="sxs-lookup"><span data-stu-id="0ed98-184">Scroll down toohello multi-valued object type section and enter hello data as in hello following picture.</span></span> <span data-ttu-id="0ed98-185">Välj **Slutför** toosave hello steg.</span><span class="sxs-lookup"><span data-stu-id="0ed98-185">Select **Finish** toosave hello step.</span></span>  
   <span data-ttu-id="0ed98-186">![Runprofile4a](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile4a.png)</span><span class="sxs-lookup"><span data-stu-id="0ed98-186">![Runprofile4a](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile4a.png)</span></span>  
   <span data-ttu-id="0ed98-187">![Runprofile4b](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile4b.png)</span><span class="sxs-lookup"><span data-stu-id="0ed98-187">![Runprofile4b](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile4b.png)</span></span>  
5. <span data-ttu-id="0ed98-188">Välj **nytt steg**.</span><span class="sxs-lookup"><span data-stu-id="0ed98-188">Select **New Step**.</span></span> <span data-ttu-id="0ed98-189">Den här gången väljer **objekt = grupp**.</span><span class="sxs-lookup"><span data-stu-id="0ed98-189">This time, select **OBJECT=Group**.</span></span> <span data-ttu-id="0ed98-190">Använd hello konfiguration enligt följande bild hello hello sista sidan.</span><span class="sxs-lookup"><span data-stu-id="0ed98-190">On hello last page, use hello configuration as in hello following picture.</span></span> <span data-ttu-id="0ed98-191">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="0ed98-191">Click **Finish**.</span></span>  
   <span data-ttu-id="0ed98-192">![Runprofile5a](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile5a.png)</span><span class="sxs-lookup"><span data-stu-id="0ed98-192">![Runprofile5a](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile5a.png)</span></span>  
   <span data-ttu-id="0ed98-193">![Runprofile5b](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile5b.png)</span><span class="sxs-lookup"><span data-stu-id="0ed98-193">![Runprofile5b](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile5b.png)</span></span>  
6. <span data-ttu-id="0ed98-194">Valfritt: Om du vill kan du kan konfigurera ytterligare körning av profiler.</span><span class="sxs-lookup"><span data-stu-id="0ed98-194">Optional: If you want to, you can configure additional run profiles.</span></span> <span data-ttu-id="0ed98-195">Den här genomgången används endast hello fullständig Import.</span><span class="sxs-lookup"><span data-stu-id="0ed98-195">For this walkthrough, only hello Full Import is used.</span></span>
7. <span data-ttu-id="0ed98-196">Klicka på **OK** toofinish ändra körningsprofiler.</span><span class="sxs-lookup"><span data-stu-id="0ed98-196">Click **OK** toofinish changing run profiles.</span></span>

## <a name="add-some-test-data-and-test-hello-import"></a><span data-ttu-id="0ed98-197">Lägg till import för hello av data och vissa test</span><span class="sxs-lookup"><span data-stu-id="0ed98-197">Add some test data and test hello import</span></span>
<span data-ttu-id="0ed98-198">Fylla lite testdata i din exempeldatabas.</span><span class="sxs-lookup"><span data-stu-id="0ed98-198">Fill out some test data in your sample database.</span></span> <span data-ttu-id="0ed98-199">När du är klar väljer du **kör** och **fullständig import**.</span><span class="sxs-lookup"><span data-stu-id="0ed98-199">When you are ready, select **Run** and **Full import**.</span></span>

<span data-ttu-id="0ed98-200">Här är en användare med två telefonnummer och en grupp med medlemmar.</span><span class="sxs-lookup"><span data-stu-id="0ed98-200">Here is a user with two phone numbers and a group with some members.</span></span>  
![cs1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/cs1.png)  
![CS2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/cs2.png)  

## <a name="appendix-a"></a><span data-ttu-id="0ed98-203">Bilaga A</span><span class="sxs-lookup"><span data-stu-id="0ed98-203">Appendix A</span></span>
<span data-ttu-id="0ed98-204">**SQL-skript toocreate hello exempeldatabasen**</span><span class="sxs-lookup"><span data-stu-id="0ed98-204">**SQL script toocreate hello sample database**</span></span>

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
