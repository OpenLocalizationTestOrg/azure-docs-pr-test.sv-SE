---
title: "aaaDesign ditt första Azure-databas för MySQL - databas i Azure Portal | Microsoft Docs"
description: "Den här självstudiekursen beskrivs hur toocreate och hantera Azure-databas för MySQL-servern och databasen med hjälp av Azure Portal."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 06/06/2017
ms.custom: mvc
ms.openlocfilehash: 06dd952acc5356b8cccaf36917df1ff8db4f7139
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="design-your-first-azure-database-for-mysql-database"></a><span data-ttu-id="cc3e2-103">Utforma din första Azure-databas för MySQL-databas</span><span class="sxs-lookup"><span data-stu-id="cc3e2-103">Design your first Azure Database for MySQL database</span></span>
<span data-ttu-id="cc3e2-104">Azure MySQL-databas är en hanterad tjänst som gör att du toorun, hantera och skala högtillgänglig MySQL-databaser i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-104">Azure Database for MySQL is a managed service that enables you toorun, manage, and scale highly available MySQL databases in hello cloud.</span></span> <span data-ttu-id="cc3e2-105">Med hello Azure-portalen kan du enkelt hantera servern och skapar en databas.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-105">Using hello Azure portal, you can easily manage your server and design a database.</span></span>

<span data-ttu-id="cc3e2-106">I den här kursen använder du hello Azure portal toolearn hur till:</span><span class="sxs-lookup"><span data-stu-id="cc3e2-106">In this tutorial, you use hello Azure portal toolearn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="cc3e2-107">Skapa en Azure-databas för MySQL</span><span class="sxs-lookup"><span data-stu-id="cc3e2-107">Create an Azure Database for MySQL</span></span>
> * <span data-ttu-id="cc3e2-108">Konfigurera hello serverbrandvägg</span><span class="sxs-lookup"><span data-stu-id="cc3e2-108">Configure hello server firewall</span></span>
> * <span data-ttu-id="cc3e2-109">Använda mysql kommandoradsverktyget toocreate en databas</span><span class="sxs-lookup"><span data-stu-id="cc3e2-109">Use mysql command-line tool toocreate a database</span></span>
> * <span data-ttu-id="cc3e2-110">Läs in exempeldata</span><span class="sxs-lookup"><span data-stu-id="cc3e2-110">Load sample data</span></span>
> * <span data-ttu-id="cc3e2-111">Frågedata</span><span class="sxs-lookup"><span data-stu-id="cc3e2-111">Query data</span></span>
> * <span data-ttu-id="cc3e2-112">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="cc3e2-112">Update data</span></span>
> * <span data-ttu-id="cc3e2-113">Återställa data</span><span class="sxs-lookup"><span data-stu-id="cc3e2-113">Restore data</span></span>

## <a name="sign-in-toohello-azure-portal"></a><span data-ttu-id="cc3e2-114">Logga in toohello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="cc3e2-114">Sign in toohello Azure portal</span></span>
<span data-ttu-id="cc3e2-115">Öppna valfri webbläsare och gå hello [Microsoft Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="cc3e2-115">Open your favorite web browser, and visit hello [Microsoft Azure portal](https://portal.azure.com/).</span></span> <span data-ttu-id="cc3e2-116">Ange dina autentiseringsuppgifter toosign toohello-portalen.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-116">Enter your credentials toosign in toohello portal.</span></span> <span data-ttu-id="cc3e2-117">hello standardvyn är instrumentpanelen service.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-117">hello default view is your service dashboard.</span></span>

## <a name="create-an-azure-database-for-mysql-server"></a><span data-ttu-id="cc3e2-118">Skapa en Azure Database för MySQL-server</span><span class="sxs-lookup"><span data-stu-id="cc3e2-118">Create an Azure Database for MySQL server</span></span>
<span data-ttu-id="cc3e2-119">En Azure Database för MySQL-server skapas med en definierad uppsättning [compute- och lagringsresurser](./concepts-compute-unit-and-storage.md).</span><span class="sxs-lookup"><span data-stu-id="cc3e2-119">An Azure Database for MySQL server is created with a defined set of [compute and storage resources](./concepts-compute-unit-and-storage.md).</span></span> <span data-ttu-id="cc3e2-120">hello server skapas inom en [Azure-resursgrupp](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span><span class="sxs-lookup"><span data-stu-id="cc3e2-120">hello server is created within an [Azure resource group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span></span>

1. <span data-ttu-id="cc3e2-121">Navigera för**databaser** > **Azure-databas för MySQL**.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-121">Navigate too**Databases** > **Azure Database for MySQL**.</span></span> <span data-ttu-id="cc3e2-122">Om du inte hittar MySQL-servern under **databaser** kategori, klickar du på **se alla** tooshow alla tillgängliga services-databas.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-122">If you cannot find MySQL Server under **Databases** category, click **See all** tooshow all available database services.</span></span> <span data-ttu-id="cc3e2-123">Du kan också skriva **Azure-databas för MySQL** hello Sök rutan tooquickly hitta hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-123">You can also type **Azure Database for MySQL** in hello search box tooquickly find hello service.</span></span>
<span data-ttu-id="cc3e2-124">![2-1 analysera tooMySQL](./media/tutorial-design-database-using-portal/2_1-Navigate-to-MySQL.png)</span><span class="sxs-lookup"><span data-stu-id="cc3e2-124">![2-1 Navigate tooMySQL](./media/tutorial-design-database-using-portal/2_1-Navigate-to-MySQL.png)</span></span>

2. <span data-ttu-id="cc3e2-125">Klicka på **Azure-databas för MySQL** panelen och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-125">Click **Azure Database for MySQL** tile, and then click **Create**.</span></span>

<span data-ttu-id="cc3e2-126">I vårt exempel fyller du i hello Azure-databas för MySQL formulär med hello följande information:</span><span class="sxs-lookup"><span data-stu-id="cc3e2-126">In our example, fill out hello Azure Database for MySQL form with hello following information:</span></span>

| <span data-ttu-id="cc3e2-127">**Inställning**</span><span class="sxs-lookup"><span data-stu-id="cc3e2-127">**Setting**</span></span> | <span data-ttu-id="cc3e2-128">**Föreslaget värde**</span><span class="sxs-lookup"><span data-stu-id="cc3e2-128">**Suggested value**</span></span> | <span data-ttu-id="cc3e2-129">**Fältbeskrivning**</span><span class="sxs-lookup"><span data-stu-id="cc3e2-129">**Field Description**</span></span> |
|---|---|---|
| <span data-ttu-id="cc3e2-130">*Servernamn*</span><span class="sxs-lookup"><span data-stu-id="cc3e2-130">*Server name*</span></span> | <span data-ttu-id="cc3e2-131">myserver4demo</span><span class="sxs-lookup"><span data-stu-id="cc3e2-131">myserver4demo</span></span>  | <span data-ttu-id="cc3e2-132">Servernamn har toobe globalt unika.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-132">Server name has toobe globally unique.</span></span> |
| <span data-ttu-id="cc3e2-133">*Prenumeration*</span><span class="sxs-lookup"><span data-stu-id="cc3e2-133">*Subscription*</span></span> | <span data-ttu-id="cc3e2-134">mysubscription</span><span class="sxs-lookup"><span data-stu-id="cc3e2-134">mysubscription</span></span> | <span data-ttu-id="cc3e2-135">Välj prenumerationen hello i listrutan.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-135">Select your subscription from hello drop-down.</span></span> |
| <span data-ttu-id="cc3e2-136">*Resursgrupp*</span><span class="sxs-lookup"><span data-stu-id="cc3e2-136">*Resource group*</span></span> | <span data-ttu-id="cc3e2-137">myresourcegroup</span><span class="sxs-lookup"><span data-stu-id="cc3e2-137">myresourcegroup</span></span> | <span data-ttu-id="cc3e2-138">Skapa en resursgrupp eller välj en befintlig.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-138">Create a resource group or use an existing one.</span></span> |
| <span data-ttu-id="cc3e2-139">*Inloggning för serveradministratör*</span><span class="sxs-lookup"><span data-stu-id="cc3e2-139">*Server admin login*</span></span> | <span data-ttu-id="cc3e2-140">myadmin</span><span class="sxs-lookup"><span data-stu-id="cc3e2-140">myadmin</span></span> | <span data-ttu-id="cc3e2-141">Konfigurera admin kontonamn.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-141">Setup admin account name.</span></span> |
| <span data-ttu-id="cc3e2-142">*Lösenord*</span><span class="sxs-lookup"><span data-stu-id="cc3e2-142">*Password*</span></span> |  | <span data-ttu-id="cc3e2-143">Ange ett starkt administratörslösenord.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-143">Set a strong admin account password.</span></span> |
| <span data-ttu-id="cc3e2-144">*Bekräfta lösenord*</span><span class="sxs-lookup"><span data-stu-id="cc3e2-144">*Confirm password*</span></span> |  | <span data-ttu-id="cc3e2-145">Bekräfta hello admin kontolösenord.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-145">Confirm hello admin account password.</span></span> |
| <span data-ttu-id="cc3e2-146">*Plats*</span><span class="sxs-lookup"><span data-stu-id="cc3e2-146">*Location*</span></span> |  | <span data-ttu-id="cc3e2-147">Välj en tillgänglig region.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-147">Select an available region.</span></span> |
| <span data-ttu-id="cc3e2-148">*Version*</span><span class="sxs-lookup"><span data-stu-id="cc3e2-148">*Version*</span></span> | <span data-ttu-id="cc3e2-149">5.7</span><span class="sxs-lookup"><span data-stu-id="cc3e2-149">5.7</span></span> | <span data-ttu-id="cc3e2-150">Välj hello senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-150">Choose hello latest version.</span></span> |
| <span data-ttu-id="cc3e2-151">*Konfigurera prestanda*</span><span class="sxs-lookup"><span data-stu-id="cc3e2-151">*Configure performance*</span></span> | <span data-ttu-id="cc3e2-152">Basic, 50 compute enheter, 50 GB</span><span class="sxs-lookup"><span data-stu-id="cc3e2-152">Basic, 50 compute units, 50 GB</span></span>  | <span data-ttu-id="cc3e2-153">Välj **Prisnivå**, **Compute-enheter**, **Lagring (GB)** och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-153">Choose **Pricing tier**, **Compute Units**, **Storage (GB)**, and then click **OK**.</span></span> |
| <span data-ttu-id="cc3e2-154">*PIN-kod tooDashboard*</span><span class="sxs-lookup"><span data-stu-id="cc3e2-154">*Pin tooDashboard*</span></span> | <span data-ttu-id="cc3e2-155">Markera</span><span class="sxs-lookup"><span data-stu-id="cc3e2-155">Check</span></span> | <span data-ttu-id="cc3e2-156">Rekommenderade toocheck den här rutan så att du kan hitta hello server enkelt vid ett senare tillfälle</span><span class="sxs-lookup"><span data-stu-id="cc3e2-156">Recommended toocheck this box so you may find hello server easily later on</span></span> |
<span data-ttu-id="cc3e2-157">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-157">Then, click **Create**.</span></span> <span data-ttu-id="cc3e2-158">En ny Azure-databas för MySQL-servern körs i en minut eller två i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-158">In a minute or two, a new Azure Database for MySQL server is running in hello cloud.</span></span> <span data-ttu-id="cc3e2-159">Du kan klicka på **meddelanden** distributionsprocessen för hello verktygsfältet toomonitor hello-knappen.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-159">You can click **Notifications** button on hello toolbar toomonitor hello deployment process.</span></span>

## <a name="configure-firewall"></a><span data-ttu-id="cc3e2-160">Konfigurera brandväggen</span><span class="sxs-lookup"><span data-stu-id="cc3e2-160">Configure firewall</span></span>
<span data-ttu-id="cc3e2-161">Azure-databaser för MySQL skyddas av en brandvägg.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-161">Azure Databases for MySQL are protected by a firewall.</span></span> <span data-ttu-id="cc3e2-162">Som standard är alla anslutningar toohello server och hello databaser hello-servern avvisade.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-162">By default, all connections toohello server and hello databases inside hello server are rejected.</span></span> <span data-ttu-id="cc3e2-163">Innan du ansluter tooAzure databas för MySQL för hello första gången, konfigurera hello brandväggen tooadd hello klienten datorns offentliga IP-adress (eller IP-adressintervall).</span><span class="sxs-lookup"><span data-stu-id="cc3e2-163">Before connecting tooAzure Database for MySQL for hello first time, configure hello firewall tooadd hello client machine's public network IP address (or IP address range).</span></span>

1. <span data-ttu-id="cc3e2-164">Klicka på din nya server och klicka sedan på **anslutningssäkerhet**.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-164">Click your newly created server, and then click **Connection security**.</span></span>
   <span data-ttu-id="cc3e2-165">![3-1 anslutningssäkerhet](./media/tutorial-design-database-using-portal/3_1-Connection-security.png)</span><span class="sxs-lookup"><span data-stu-id="cc3e2-165">![3-1 Connection security](./media/tutorial-design-database-using-portal/3_1-Connection-security.png)</span></span>
2. <span data-ttu-id="cc3e2-166">Du kan **Lägg till Min IP**, eller konfigurera brandväggsregler här.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-166">You can **Add My IP**, or configure firewall rules here.</span></span> <span data-ttu-id="cc3e2-167">Kom ihåg tooclick **spara** när du har skapat hello regler.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-167">Remember tooclick **Save** after you have created hello rules.</span></span>
<span data-ttu-id="cc3e2-168">Nu kan du ansluta toohello server med mysql-kommandoradsverktyget eller MySQL arbetsstationen GUI-verktyg.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-168">You can now connect toohello server using mysql command-line tool or MySQL Workbench GUI tool.</span></span>

> [!TIP]
> <span data-ttu-id="cc3e2-169">Azure-databas för MySQL-servern kommunicerar via port 3306.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-169">Azure Database for MySQL server communicates over port 3306.</span></span> <span data-ttu-id="cc3e2-170">Om du försöker tooconnect från ett företagsnätverk, tillåtas utgående trafik via port 3306 inte av ditt nätverks brandvägg.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-170">If you are trying tooconnect from within a corporate network, outbound traffic over port 3306 may not be allowed by your network's firewall.</span></span> <span data-ttu-id="cc3e2-171">I så fall, kan du inte ansluta tooAzure MySQL-servern om din IT-avdelning öppnar port 3306.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-171">If so, you cannot connect tooAzure MySQL server unless your IT department opens port 3306.</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="cc3e2-172">Hämta anslutningsinformation</span><span class="sxs-lookup"><span data-stu-id="cc3e2-172">Get connection information</span></span>
<span data-ttu-id="cc3e2-173">Get-hello fullständigt kvalificerade **servernamn** och **serverinloggningsnamnet för admin** för din Azure-databas för MySQL-servern från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-173">Get hello fully qualified **Server name** and **Server admin login name** for your Azure Database for MySQL server from hello Azure portal.</span></span> <span data-ttu-id="cc3e2-174">Du använder kommandoradsverktyget för mysql hello fullständiga server name tooconnect tooyour server.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-174">You use hello fully qualified server name tooconnect tooyour server using mysql command-line tool.</span></span> 

1. <span data-ttu-id="cc3e2-175">I [Azure-portalen](https://portal.azure.com/), klickar du på **alla resurser** hello vänstra menyn, hello typnamn och Sök efter din Azure-databas för MySQL-servern.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-175">In [Azure portal](https://portal.azure.com/), click **All resources** from hello left-hand menu, type hello name, and search for your Azure Database for MySQL server.</span></span> <span data-ttu-id="cc3e2-176">Välj hello namn tooview hello serverinformation.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-176">Select hello server name tooview hello details.</span></span>

2. <span data-ttu-id="cc3e2-177">Under hello inställningar rubrik, klickar du på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-177">Under hello Settings heading, click **Properties**.</span></span> <span data-ttu-id="cc3e2-178">Notera **servernamn** och **SERVERINLOGGNINGSNAMNET för ADMIN**.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-178">Note down **SERVER NAME** and **SERVER ADMIN LOGIN NAME**.</span></span> <span data-ttu-id="cc3e2-179">Du kan klicka på hello Kopiera knappen Nästa tooeach fältet toocopy toohello Urklipp.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-179">You may click hello copy button next tooeach field toocopy toohello clipboard.</span></span>
   <span data-ttu-id="cc3e2-180">![4-2-serveregenskaper](./media/tutorial-design-database-using-portal/4_2-server-properties.png)</span><span class="sxs-lookup"><span data-stu-id="cc3e2-180">![4-2 server properties](./media/tutorial-design-database-using-portal/4_2-server-properties.png)</span></span>

<span data-ttu-id="cc3e2-181">I det här exemplet hello servernamnet är *myserver4demo.mysql.database.azure.com*, och hello inloggning för serveradministratör är  *myadmin@myserver4demo* .</span><span class="sxs-lookup"><span data-stu-id="cc3e2-181">In this example, hello server name is *myserver4demo.mysql.database.azure.com*, and hello server admin login is *myadmin@myserver4demo*.</span></span>

## <a name="connect-toohello-server-using-mysql"></a><span data-ttu-id="cc3e2-182">Ansluta toohello servern med hjälp av mysql</span><span class="sxs-lookup"><span data-stu-id="cc3e2-182">Connect toohello server using mysql</span></span>
<span data-ttu-id="cc3e2-183">Använd [mysql kommandoradsverktyget](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) tooestablish anslutning tooyour Azure-databas för MySQL-servern.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-183">Use [mysql command-line tool](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) tooestablish a connection tooyour Azure Database for MySQL server.</span></span> <span data-ttu-id="cc3e2-184">Du kan köra hello mysql-kommandoradsverktyget från hello Azure Cloud Shell i hello webbläsare eller från din egen dator med hjälp av mysql-verktygen som installeras lokalt.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-184">You can run hello mysql command-line tool from hello Azure Cloud Shell in hello browser or from your own machine using mysql tools installed locally.</span></span> <span data-ttu-id="cc3e2-185">toolaunch hello Azure Cloud Shell, klicka på hello `Try It` på ett kodblock i den här artikeln eller besök hello Azure-portalen och klicka sedan på hello `>_` ikon i hello översta högra verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-185">toolaunch hello Azure Cloud Shell, click hello `Try It` button on a code block in this article, or visit hello Azure portal and click hello `>_` icon in hello top right toolbar.</span></span> 

<span data-ttu-id="cc3e2-186">Skriv hello kommandot tooconnect:</span><span class="sxs-lookup"><span data-stu-id="cc3e2-186">Type hello command tooconnect:</span></span>
```azurecli-interactive
mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p
```

## <a name="create-a-blank-database"></a><span data-ttu-id="cc3e2-187">Skapa en tom databas</span><span class="sxs-lookup"><span data-stu-id="cc3e2-187">Create a blank database</span></span>
<span data-ttu-id="cc3e2-188">När du är ansluten toohello server kan du skapa en tom databas toowork med.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-188">Once you’re connected toohello server, create a blank database toowork with.</span></span>
```sql
CREATE DATABASE mysampledb;
```

<span data-ttu-id="cc3e2-189">Kör hello efter kommandot tooswitch toothis nyskapad databas hello Kommandotolken:</span><span class="sxs-lookup"><span data-stu-id="cc3e2-189">At hello prompt, run hello following command tooswitch connection toothis newly created database:</span></span>
```sql
USE mysampledb;
```

## <a name="create-tables-in-hello-database"></a><span data-ttu-id="cc3e2-190">Skapa tabeller i hello-databas</span><span class="sxs-lookup"><span data-stu-id="cc3e2-190">Create tables in hello database</span></span>
<span data-ttu-id="cc3e2-191">Nu när du vet hur tooconnect toohello Azure-databas för MySQL-databas, det kan gå igenom hur toocomplete vissa grundläggande uppgifter.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-191">Now that you know how tooconnect toohello Azure Database for MySQL database, we can go over how toocomplete some basic tasks.</span></span>

<span data-ttu-id="cc3e2-192">Vi kan först skapa en tabell och läsa in den med vissa data.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-192">First, we can create a table and load it with some data.</span></span> <span data-ttu-id="cc3e2-193">Nu ska vi skapa en tabell som innehåller information om maskinvaruinventering.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-193">Let's create a table that stores inventory information.</span></span>
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

## <a name="load-data-into-hello-tables"></a><span data-ttu-id="cc3e2-194">Läs in data till hello tabeller</span><span class="sxs-lookup"><span data-stu-id="cc3e2-194">Load data into hello tables</span></span>
<span data-ttu-id="cc3e2-195">Nu när vi har en tabell kan vi infoga vissa data i den.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-195">Now that we have a table, we can insert some data into it.</span></span> <span data-ttu-id="cc3e2-196">Kör följande fråga tooinsert hello vissa rader med data vid hello öppna Kommandotolkens fönster.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-196">At hello open command prompt window, run hello following query tooinsert some rows of data.</span></span>
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

<span data-ttu-id="cc3e2-197">Nu har du två rader med exempeldata till hello-tabell som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-197">Now you have two rows of sample data into hello table you created earlier.</span></span>

## <a name="query-and-update-hello-data-in-hello-tables"></a><span data-ttu-id="cc3e2-198">Fråga efter och uppdatera hello data i hello tabeller</span><span class="sxs-lookup"><span data-stu-id="cc3e2-198">Query and update hello data in hello tables</span></span>
<span data-ttu-id="cc3e2-199">Kör följande fråga tooretrieve information från hello databastabell hello.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-199">Execute hello following query tooretrieve information from hello database table.</span></span>
```sql
SELECT * FROM inventory;
```

<span data-ttu-id="cc3e2-200">Du kan också uppdatera hello data i hello tabeller.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-200">You can also update hello data in hello tables.</span></span>
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

<span data-ttu-id="cc3e2-201">hello rad uppdateras i enlighet med detta när du hämtar data.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-201">hello row gets updated accordingly when you retrieve data.</span></span>
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-tooa-previous-point-in-time"></a><span data-ttu-id="cc3e2-202">Återställa en databas tooa tidigare punkt i tiden</span><span class="sxs-lookup"><span data-stu-id="cc3e2-202">Restore a database tooa previous point in time</span></span>
<span data-ttu-id="cc3e2-203">Anta att du har tagits bort av misstag en viktigt-databastabell och kan inte återställa hello data enkelt.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-203">Imagine you have accidentally deleted an important database table, and cannot recover hello data easily.</span></span> <span data-ttu-id="cc3e2-204">Azure MySQL-databas kan du toorestore hello server tooa punkt i tiden, skapar en kopia av hello databaser till en ny server.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-204">Azure Database for MySQL allows you toorestore hello server tooa point in time, creating a copy of hello databases into new server.</span></span> <span data-ttu-id="cc3e2-205">Du kan använda den här nya servern toorecover dina data.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-205">You can use this new server toorecover your deleted data.</span></span> <span data-ttu-id="cc3e2-206">hello följande steg hello exempel server tooa återställningspunkt innan hello tabell har lagts till.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-206">hello following steps restore hello sample server tooa point before hello table was added.</span></span>

1. <span data-ttu-id="cc3e2-207">Leta upp din Azure-databas för MySQL i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-207">In hello Azure portal, locate your Azure Database for MySQL.</span></span> <span data-ttu-id="cc3e2-208">På hello **översikt** klickar du på **återställa** hello i verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-208">On hello **Overview** page, click **Restore** on hello toolbar.</span></span> <span data-ttu-id="cc3e2-209">hello återställning sidan öppnas.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-209">hello Restore page opens.</span></span>

   ![10-1 Återställ en databas](./media/tutorial-design-database-using-portal/10_1-restore-a-db.png)

2. <span data-ttu-id="cc3e2-211">Fyll i hello **återställa** formulär med hello krävs information.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-211">Fill out hello **Restore** form with hello required information.</span></span>
   
   ![10-2-formulär för återställning](./media/tutorial-design-database-using-portal/10_2-restore-form.png)
   
   - <span data-ttu-id="cc3e2-213">**Återställningspunkt**: Välj en i tidpunkt som du vill toorestore, inom hello tidsintervall som anges.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-213">**Restore point**: Select a point-in-time that you want toorestore to, within hello timeframe listed.</span></span> <span data-ttu-id="cc3e2-214">Se till att tooconvert tooUTC din lokala tidszon.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-214">Make sure tooconvert your local timezone tooUTC.</span></span>
   - <span data-ttu-id="cc3e2-215">**Återställa toonew server**: Ange ett nytt servernamn som du vill toorestore till.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-215">**Restore toonew server**: Provide a new server name you want toorestore to.</span></span>
   - <span data-ttu-id="cc3e2-216">**Plats**: hello region är samma som källservern hello och kan inte ändras.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-216">**Location**: hello region is same as hello source server, and cannot be changed.</span></span>
   - <span data-ttu-id="cc3e2-217">**Prisnivån**: hello prisnivån är hello samma som hello datakälla server och kan inte ändras.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-217">**Pricing tier**: hello pricing tier is hello same as hello source server, and cannot be changed.</span></span>
   
3. <span data-ttu-id="cc3e2-218">Klicka på **OK** toorestore hello server för[tooa återställningspunkt i tid](./howto-restore-server-portal.md) innan hello tabellen har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-218">Click **OK** toorestore hello server too[restore tooa point in time](./howto-restore-server-portal.md) before hello table was deleted.</span></span> <span data-ttu-id="cc3e2-219">Återställa en server skapar en ny kopia av hello-servern, hello tidpunkt du anger.</span><span class="sxs-lookup"><span data-stu-id="cc3e2-219">Restoring a server creates a new copy of hello server, as of hello point in time you specify.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="cc3e2-220">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cc3e2-220">Next steps</span></span>
<span data-ttu-id="cc3e2-221">I den här kursen använder du hello Azure portal toolearned hur till:</span><span class="sxs-lookup"><span data-stu-id="cc3e2-221">In this tutorial, you use hello Azure portal toolearned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="cc3e2-222">Skapa en Azure-databas för MySQL</span><span class="sxs-lookup"><span data-stu-id="cc3e2-222">Create an Azure Database for MySQL</span></span>
> * <span data-ttu-id="cc3e2-223">Konfigurera hello serverbrandvägg</span><span class="sxs-lookup"><span data-stu-id="cc3e2-223">Configure hello server firewall</span></span>
> * <span data-ttu-id="cc3e2-224">Använda mysql kommandoradsverktyget toocreate en databas</span><span class="sxs-lookup"><span data-stu-id="cc3e2-224">Use mysql command-line tool toocreate a database</span></span>
> * <span data-ttu-id="cc3e2-225">Läs in exempeldata</span><span class="sxs-lookup"><span data-stu-id="cc3e2-225">Load sample data</span></span>
> * <span data-ttu-id="cc3e2-226">Frågedata</span><span class="sxs-lookup"><span data-stu-id="cc3e2-226">Query data</span></span>
> * <span data-ttu-id="cc3e2-227">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="cc3e2-227">Update data</span></span>
> * <span data-ttu-id="cc3e2-228">Återställa data</span><span class="sxs-lookup"><span data-stu-id="cc3e2-228">Restore data</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="cc3e2-229">Hur tooconnect program tooAzure databas för MySQL</span><span class="sxs-lookup"><span data-stu-id="cc3e2-229">How tooconnect applications tooAzure Database for MySQL</span></span>](./howto-connection-string.md)
