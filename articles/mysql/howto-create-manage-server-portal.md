---
title: "Skapa och hantera Azure-databas för MySQL-servern med hjälp av Azure portal | Microsoft Docs"
description: "Den här artikeln beskriver hur du kan snabbt skapa en ny Azure-databas för MySQL-server och hantera servern med hjälp av Azure-portalen."
services: mysql
author: v-chenyh
ms.author: nolanwu
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 4605518b6955d9943e76c25df2d4105a6a94433d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-manage-azure-database-for-mysql-server-using-azure-portal"></a><span data-ttu-id="47687-103">Skapa och hantera Azure-databas för MySQL-servern med hjälp av Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="47687-103">Create and manage Azure Database for MySQL server using Azure portal</span></span>
<span data-ttu-id="47687-104">Den här artikeln beskriver hur du kan snabbt skapa en ny Azure-databas för MySQL-server och hantera servern med hjälp av Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="47687-104">This article describes how you can quickly create a new Azure Database for MySQL server and manage the server using the Azure Portal.</span></span> <span data-ttu-id="47687-105">Server management innehåller visa serverinformation och databaser, återställa lösenord och ta bort servern.</span><span class="sxs-lookup"><span data-stu-id="47687-105">Server management includes viewing server details & databases, resetting password and deleting the server.</span></span>

## <a name="log-in-to-the-azure-portal"></a><span data-ttu-id="47687-106">Logga in på Azure Portal</span><span class="sxs-lookup"><span data-stu-id="47687-106">Log in to the Azure portal</span></span>
<span data-ttu-id="47687-107">Logga in på [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="47687-107">Log in to the [Azure portal](https://portal.azure.com).</span></span>

## <a name="create-an-azure-database-for-mysql-server"></a><span data-ttu-id="47687-108">Skapa en Azure Database för MySQL-server</span><span class="sxs-lookup"><span data-stu-id="47687-108">Create an Azure Database for MySQL server</span></span>
<span data-ttu-id="47687-109">Följ dessa steg om du vill skapa en Azure-databas för MySQL-server med namnet ”mysqlserver4demo”</span><span class="sxs-lookup"><span data-stu-id="47687-109">Follow these steps to create an Azure Database for MySQL server named “mysqlserver4demo”</span></span>

<span data-ttu-id="47687-110">1-Klicka **ny** knapp hittades i det övre vänstra hörnet i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="47687-110">1- Click **New** button found on the upper left-hand corner of the Azure portal.</span></span>

<span data-ttu-id="47687-111">2 – Välj **databaser** ny sida och välj **Azure-databas för MySQL** från sidan databaser.</span><span class="sxs-lookup"><span data-stu-id="47687-111">2- Select **Databases** from the New page, and select **Azure Database for MySQL** from the Databases page.</span></span>

> <span data-ttu-id="47687-112">En Azure-databas för MySQL-server har skapats med en definierad uppsättning [beräkning och lagring](./concepts-compute-unit-and-storage.md) resurser.</span><span class="sxs-lookup"><span data-stu-id="47687-112">An Azure Database for MySQL server is created with a defined set of [compute and storage](./concepts-compute-unit-and-storage.md) resources.</span></span> <span data-ttu-id="47687-113">Databasen har skapats i en Azure-resursgrupp och i Azure-databasen för MySQL-servern.</span><span class="sxs-lookup"><span data-stu-id="47687-113">The database is created within an Azure resource group and in an Azure Database for MySQL server.</span></span>

![Skapa nya-server](./media/howto-create-manage-server-portal/create-new-server.png)

<span data-ttu-id="47687-115">3 - fylla i Azure-databasen för MySQL formulär med följande information:</span><span class="sxs-lookup"><span data-stu-id="47687-115">3- Fill out the Azure Database for MySQL form with the following information:</span></span>

| <span data-ttu-id="47687-116">**Formulärfält**</span><span class="sxs-lookup"><span data-stu-id="47687-116">**Form Field**</span></span> | <span data-ttu-id="47687-117">**Fältbeskrivning**</span><span class="sxs-lookup"><span data-stu-id="47687-117">**Field Description**</span></span> |
|----------------|-----------------------|
| <span data-ttu-id="47687-118">*Servernamn*</span><span class="sxs-lookup"><span data-stu-id="47687-118">*Server name*</span></span> | <span data-ttu-id="47687-119">Azure-mysql (servernamnet är globalt unik)</span><span class="sxs-lookup"><span data-stu-id="47687-119">azure-mysql (server name is globally unique)</span></span> |
| <span data-ttu-id="47687-120">*Prenumeration*</span><span class="sxs-lookup"><span data-stu-id="47687-120">*Subscription*</span></span> | <span data-ttu-id="47687-121">MySQLaaS (Välj listrutan)</span><span class="sxs-lookup"><span data-stu-id="47687-121">MySQLaaS (select from drop down)</span></span> |
| <span data-ttu-id="47687-122">*Resursgrupp*</span><span class="sxs-lookup"><span data-stu-id="47687-122">*Resource group*</span></span> | <span data-ttu-id="47687-123">MyResource (skapa en ny resursgrupp eller använda en befintlig)</span><span class="sxs-lookup"><span data-stu-id="47687-123">myresource (create a new resource group or use an existing one)</span></span> |
| <span data-ttu-id="47687-124">*Inloggning för serveradministratör*</span><span class="sxs-lookup"><span data-stu-id="47687-124">*Server admin login*</span></span> | <span data-ttu-id="47687-125">myadmin (konfigurera namn på administratörskonto)</span><span class="sxs-lookup"><span data-stu-id="47687-125">myadmin (setup admin account name)</span></span> |
| <span data-ttu-id="47687-126">*Lösenord*</span><span class="sxs-lookup"><span data-stu-id="47687-126">*Password*</span></span> | <span data-ttu-id="47687-127">kontolösenord för installationsprogrammet admin</span><span class="sxs-lookup"><span data-stu-id="47687-127">setup admin account password</span></span> |
| <span data-ttu-id="47687-128">*Bekräfta lösenord*</span><span class="sxs-lookup"><span data-stu-id="47687-128">*Confirm password*</span></span> | <span data-ttu-id="47687-129">bekräfta lösenord för administratörskonto</span><span class="sxs-lookup"><span data-stu-id="47687-129">confirm admin account password</span></span> |
| <span data-ttu-id="47687-130">*Plats*</span><span class="sxs-lookup"><span data-stu-id="47687-130">*Location*</span></span> | <span data-ttu-id="47687-131">Norra Europa (Välj mellan Norra Europa och västra USA)</span><span class="sxs-lookup"><span data-stu-id="47687-131">North Europe (select between North Europe and West US)</span></span> |
| <span data-ttu-id="47687-132">*Version*</span><span class="sxs-lookup"><span data-stu-id="47687-132">*Version*</span></span> | <span data-ttu-id="47687-133">5.6 (Välj Azure-databas för MySQL server-version)</span><span class="sxs-lookup"><span data-stu-id="47687-133">5.6 (choose Azure Database for MySQL server version)</span></span> |

<span data-ttu-id="47687-134">4-Klicka **prisnivå** att ange tjänstenivå för tjänstnivå och prestandanivå för den nya servern.</span><span class="sxs-lookup"><span data-stu-id="47687-134">4- Click **Pricing tier** to specify the service tier and performance level for your new server.</span></span> <span data-ttu-id="47687-135">Compute-enhet kan konfigureras mellan 50 och 100 i grundläggande nivån, 100 och 200 i standardnivån, och lagringsutrymme kan läggas till baserat på inkluderade belopp.</span><span class="sxs-lookup"><span data-stu-id="47687-135">Compute Unit can be configured between 50 and 100 in Basic tier, 100 and 200 in Standard tier, and storage can be added based on included amount.</span></span> <span data-ttu-id="47687-136">Den här guiden Ta väljer vi 50 beräknings-enhet och 50GB.</span><span class="sxs-lookup"><span data-stu-id="47687-136">For this HowTo guide, let’s choose 50 Compute Unit and 50GB.</span></span> <span data-ttu-id="47687-137">Klicka på **OK** att spara ditt val.</span><span class="sxs-lookup"><span data-stu-id="47687-137">Click **OK** to save your selection.</span></span>
<span data-ttu-id="47687-138">![Skapa-server--prisnivån](./media/howto-create-manage-server-portal/create-server-pricing-tier.png)</span><span class="sxs-lookup"><span data-stu-id="47687-138">![create-server-pricing-tier](./media/howto-create-manage-server-portal/create-server-pricing-tier.png)</span></span>

<span data-ttu-id="47687-139">5-Klicka **skapa** att etablera servern.</span><span class="sxs-lookup"><span data-stu-id="47687-139">5- Click **Create** to provision the server.</span></span> <span data-ttu-id="47687-140">Etableringen tar några minuter.</span><span class="sxs-lookup"><span data-stu-id="47687-140">Provisioning takes a few minutes.</span></span>

> <span data-ttu-id="47687-141">Markera alternativet **Fäst på instrumentpanelen** för att enkelt kunna spåra dina distributioner.</span><span class="sxs-lookup"><span data-stu-id="47687-141">Check the **Pin to dashboard** option to allow easy tracking of your deployments.</span></span>
> [!NOTE]
> <span data-ttu-id="47687-142">Även om kommer du stöd för lagring, upp till 1 000 GB i grundläggande nivån och 10000 i standardnivån för Public Preview är maximalt lagringsutrymme fortfarande begränsat till 1 000 GB tillfälligt.</span><span class="sxs-lookup"><span data-stu-id="47687-142">Although up to 1000GB in Basic tier and 10000GB in Standard tier will be supported for storage, for Public Preview, the maximum storage is still limited to 1000GB temporarily.</span></span> 
</Include>

## <a name="update-an-azure-database-for-mysql-server"></a><span data-ttu-id="47687-143">Uppdatera en Azure-databas för MySQL-server</span><span class="sxs-lookup"><span data-stu-id="47687-143">Update an Azure Database for MySQL server</span></span>
<span data-ttu-id="47687-144">När nya servern är etablerad användare har 2 alternativ för att redigera en befintlig server: återställa administratörslösenord eller skala upp eller ned servern genom att ändra beräknings-enheter.</span><span class="sxs-lookup"><span data-stu-id="47687-144">After new server is provisioned, user has 2 options to edit an existing server: reset administrator password or scale up/down the server by changing the compute-units.</span></span>

### <a name="change-the-administrator-user-password"></a><span data-ttu-id="47687-145">Ändra administratörslösenordet för användaren</span><span class="sxs-lookup"><span data-stu-id="47687-145">Change the administrator user password</span></span>
<span data-ttu-id="47687-146">1 - på servern **översikt** bladet, klickar du på **Återställ lösenord** att fylla i ett lösenord indata och bekräftelsen fönster.</span><span class="sxs-lookup"><span data-stu-id="47687-146">1- On the server **Overview** blade, click **Reset password** to populate a password input and confirmation window.</span></span>

<span data-ttu-id="47687-147">2 - Ange nya lösenord och bekräfta lösenordet i fönstret enligt nedan: ![Återställ lösenord](./media/howto-create-manage-server-portal/reset-password.png)</span><span class="sxs-lookup"><span data-stu-id="47687-147">2- Enter new password and confirm the password in the window as below: ![reset-password](./media/howto-create-manage-server-portal/reset-password.png)</span></span>

<span data-ttu-id="47687-148">3-Klicka **OK** att spara det nya lösenordet.</span><span class="sxs-lookup"><span data-stu-id="47687-148">3- Click **OK** to save the new password.</span></span>

### <a name="scale-updown-by-changing-compute-units"></a><span data-ttu-id="47687-149">Skala upp eller ned genom att ändra Compute-enheter</span><span class="sxs-lookup"><span data-stu-id="47687-149">Scale up/down by changing Compute Units</span></span>

<span data-ttu-id="47687-150">1 - på server-bladet under **inställningar**, klickar du på **prisnivå** att öppna bladet nivå priser för Azure-databas för MySQL-servern.</span><span class="sxs-lookup"><span data-stu-id="47687-150">1- On the server blade, under **Settings**, click **Pricing tier** to open the Pricing tier blade for the Azure Database for MySQL server.</span></span>

<span data-ttu-id="47687-151">2-Följ steg 4 i **skapa en Azure-databas för MySQL server** ändra Compute enheter i samma prisnivå.</span><span class="sxs-lookup"><span data-stu-id="47687-151">2- Follow Step 4 in **Create an Azure Database for MySQL server** to change Compute Units in the same pricing tier.</span></span>

## <a name="delete-an-azure-database-for-mysql-server"></a><span data-ttu-id="47687-152">Ta bort en Azure-databas för MySQL-server</span><span class="sxs-lookup"><span data-stu-id="47687-152">Delete an Azure Database for MySQL server</span></span>

<span data-ttu-id="47687-153">1 - på servern **översikt** bladet, klickar du på **ta bort** kommandot för att öppna bladet om du tar bort bekräftelse.</span><span class="sxs-lookup"><span data-stu-id="47687-153">1- On the server **Overview** blade, click **Delete** command button to open the Deleting confirmation blade.</span></span>

<span data-ttu-id="47687-154">2 - Skriv rätt servernamn i indata i bladet för dubbla bekräftelse.</span><span class="sxs-lookup"><span data-stu-id="47687-154">2- Type the correct server name in input box of the blade for double confirmation.</span></span>

<span data-ttu-id="47687-155">3-Klicka **ta bort** knappen igen för att bekräfta åtgärden tar bort och vänta tills ”ta bort lyckades” popup på meddelandefältet.</span><span class="sxs-lookup"><span data-stu-id="47687-155">3- Click **Delete** button again to confirm deleting action and wait for “Deleting success” popup on the notification bar.</span></span>

## <a name="list-the-azure-database-for-mysql-databases"></a><span data-ttu-id="47687-156">Visa en lista med Azure-databas för MySQL-databaser</span><span class="sxs-lookup"><span data-stu-id="47687-156">List the Azure Database for MySQL databases</span></span>
<span data-ttu-id="47687-157">På servern **översikt** bladet Bläddra nedåt tills du ser ikonen längst ned i databasen.</span><span class="sxs-lookup"><span data-stu-id="47687-157">On the server **Overview** blade, scroll down until you see the database tile on the bottom.</span></span> <span data-ttu-id="47687-158">Alla databaser visas i tabellen.</span><span class="sxs-lookup"><span data-stu-id="47687-158">All the databases will be listed in the table.</span></span> <span data-ttu-id="47687-159">Klicka på **ta bort** kommandot för att öppna bladet om du tar bort bekräftelse.</span><span class="sxs-lookup"><span data-stu-id="47687-159">click **Delete** command button to open the Deleting confirmation blade.</span></span>

![Visa-databaser](./media/howto-create-manage-server-portal/show-databases.png)

## <a name="show-details-of-an-azure-database-for-mysql-server"></a><span data-ttu-id="47687-161">Visa detaljer för en Azure-databas för MySQL-server</span><span class="sxs-lookup"><span data-stu-id="47687-161">Show details of an Azure Database for MySQL server</span></span>
<span data-ttu-id="47687-162">Klicka på **egenskaper** under **inställningar** på servern öppnas bladet den **egenskaper** bladet.</span><span class="sxs-lookup"><span data-stu-id="47687-162">Click **Properties** under **Settings** on the server blade will open the **Properties** blade.</span></span> <span data-ttu-id="47687-163">Sedan kan du visa alla detaljerad information om servern.</span><span class="sxs-lookup"><span data-stu-id="47687-163">Then you can view all detailed information about the server.</span></span>

## <a name="next-steps"></a><span data-ttu-id="47687-164">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="47687-164">Next steps</span></span>

[<span data-ttu-id="47687-165">Snabbstart: Skapa Azure-databas för MySQL-server med hjälp av Azure portal</span><span class="sxs-lookup"><span data-stu-id="47687-165">Quickstart: Create Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
