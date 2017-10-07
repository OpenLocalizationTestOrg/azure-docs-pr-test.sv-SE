---
title: "aaaCreate och hantera Azure-databas för MySQL-servern med hjälp av Azure portal | Microsoft Docs"
description: "Den här artikeln beskriver hur du kan snabbt skapa en ny Azure-databas för MySQL-servern och hanterar hello servern hello Azure-portalen."
services: mysql
author: v-chenyh
ms.author: nolanwu
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: c532df43b3d2124d7bd34875b32d52357f162af8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-azure-database-for-mysql-server-using-azure-portal"></a><span data-ttu-id="aa1f5-103">Skapa och hantera Azure-databas för MySQL-servern med hjälp av Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="aa1f5-103">Create and manage Azure Database for MySQL server using Azure portal</span></span>
<span data-ttu-id="aa1f5-104">Den här artikeln beskriver hur du kan snabbt skapa en ny Azure-databas för MySQL-servern och hanterar hello servern hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="aa1f5-104">This article describes how you can quickly create a new Azure Database for MySQL server and manage hello server using hello Azure Portal.</span></span> <span data-ttu-id="aa1f5-105">Server management innehåller visa serverinformation och databaser, återställa lösenord och ta bort hello-server.</span><span class="sxs-lookup"><span data-stu-id="aa1f5-105">Server management includes viewing server details & databases, resetting password and deleting hello server.</span></span>

## <a name="log-in-toohello-azure-portal"></a><span data-ttu-id="aa1f5-106">Logga in toohello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="aa1f5-106">Log in toohello Azure portal</span></span>
<span data-ttu-id="aa1f5-107">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="aa1f5-107">Log in toohello [Azure portal](https://portal.azure.com).</span></span>

## <a name="create-an-azure-database-for-mysql-server"></a><span data-ttu-id="aa1f5-108">Skapa en Azure Database för MySQL-server</span><span class="sxs-lookup"><span data-stu-id="aa1f5-108">Create an Azure Database for MySQL server</span></span>
<span data-ttu-id="aa1f5-109">Följ dessa steg toocreate en Azure-databas för MySQL-server med namnet ”mysqlserver4demo”</span><span class="sxs-lookup"><span data-stu-id="aa1f5-109">Follow these steps toocreate an Azure Database for MySQL server named “mysqlserver4demo”</span></span>

<span data-ttu-id="aa1f5-110">1-Klicka **ny** knappen hittades på hello övre vänstra hörnet av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="aa1f5-110">1- Click **New** button found on hello upper left-hand corner of hello Azure portal.</span></span>

<span data-ttu-id="aa1f5-111">2 – Välj **databaser** hello ny sida och välj **Azure-databas för MySQL** från hello databaser sida.</span><span class="sxs-lookup"><span data-stu-id="aa1f5-111">2- Select **Databases** from hello New page, and select **Azure Database for MySQL** from hello Databases page.</span></span>

> <span data-ttu-id="aa1f5-112">En Azure-databas för MySQL-server har skapats med en definierad uppsättning [beräkning och lagring](./concepts-compute-unit-and-storage.md) resurser.</span><span class="sxs-lookup"><span data-stu-id="aa1f5-112">An Azure Database for MySQL server is created with a defined set of [compute and storage](./concepts-compute-unit-and-storage.md) resources.</span></span> <span data-ttu-id="aa1f5-113">hello-databasen har skapats i en Azure-resursgrupp och i Azure-databasen för MySQL-servern.</span><span class="sxs-lookup"><span data-stu-id="aa1f5-113">hello database is created within an Azure resource group and in an Azure Database for MySQL server.</span></span>

![Skapa nya-server](./media/howto-create-manage-server-portal/create-new-server.png)

<span data-ttu-id="aa1f5-115">3 - fylla hello Azure-databas för MySQL formulär med hello följande information:</span><span class="sxs-lookup"><span data-stu-id="aa1f5-115">3- Fill out hello Azure Database for MySQL form with hello following information:</span></span>

| <span data-ttu-id="aa1f5-116">**Formulärfält**</span><span class="sxs-lookup"><span data-stu-id="aa1f5-116">**Form Field**</span></span> | <span data-ttu-id="aa1f5-117">**Fältbeskrivning**</span><span class="sxs-lookup"><span data-stu-id="aa1f5-117">**Field Description**</span></span> |
|----------------|-----------------------|
| <span data-ttu-id="aa1f5-118">*Servernamn*</span><span class="sxs-lookup"><span data-stu-id="aa1f5-118">*Server name*</span></span> | <span data-ttu-id="aa1f5-119">Azure-mysql (servernamnet är globalt unik)</span><span class="sxs-lookup"><span data-stu-id="aa1f5-119">azure-mysql (server name is globally unique)</span></span> |
| <span data-ttu-id="aa1f5-120">*Prenumeration*</span><span class="sxs-lookup"><span data-stu-id="aa1f5-120">*Subscription*</span></span> | <span data-ttu-id="aa1f5-121">MySQLaaS (Välj listrutan)</span><span class="sxs-lookup"><span data-stu-id="aa1f5-121">MySQLaaS (select from drop down)</span></span> |
| <span data-ttu-id="aa1f5-122">*Resursgrupp*</span><span class="sxs-lookup"><span data-stu-id="aa1f5-122">*Resource group*</span></span> | <span data-ttu-id="aa1f5-123">MyResource (skapa en ny resursgrupp eller använda en befintlig)</span><span class="sxs-lookup"><span data-stu-id="aa1f5-123">myresource (create a new resource group or use an existing one)</span></span> |
| <span data-ttu-id="aa1f5-124">*Inloggning för serveradministratör*</span><span class="sxs-lookup"><span data-stu-id="aa1f5-124">*Server admin login*</span></span> | <span data-ttu-id="aa1f5-125">myadmin (konfigurera namn på administratörskonto)</span><span class="sxs-lookup"><span data-stu-id="aa1f5-125">myadmin (setup admin account name)</span></span> |
| <span data-ttu-id="aa1f5-126">*Lösenord*</span><span class="sxs-lookup"><span data-stu-id="aa1f5-126">*Password*</span></span> | <span data-ttu-id="aa1f5-127">kontolösenord för installationsprogrammet admin</span><span class="sxs-lookup"><span data-stu-id="aa1f5-127">setup admin account password</span></span> |
| <span data-ttu-id="aa1f5-128">*Bekräfta lösenord*</span><span class="sxs-lookup"><span data-stu-id="aa1f5-128">*Confirm password*</span></span> | <span data-ttu-id="aa1f5-129">bekräfta lösenord för administratörskonto</span><span class="sxs-lookup"><span data-stu-id="aa1f5-129">confirm admin account password</span></span> |
| <span data-ttu-id="aa1f5-130">*Plats*</span><span class="sxs-lookup"><span data-stu-id="aa1f5-130">*Location*</span></span> | <span data-ttu-id="aa1f5-131">Norra Europa (Välj mellan Norra Europa och västra USA)</span><span class="sxs-lookup"><span data-stu-id="aa1f5-131">North Europe (select between North Europe and West US)</span></span> |
| <span data-ttu-id="aa1f5-132">*Version*</span><span class="sxs-lookup"><span data-stu-id="aa1f5-132">*Version*</span></span> | <span data-ttu-id="aa1f5-133">5.6 (Välj Azure-databas för MySQL server-version)</span><span class="sxs-lookup"><span data-stu-id="aa1f5-133">5.6 (choose Azure Database for MySQL server version)</span></span> |

<span data-ttu-id="aa1f5-134">4-Klicka **prisnivå** toospecify hello tjänstnivå och prestandanivå servicenivå för den nya servern.</span><span class="sxs-lookup"><span data-stu-id="aa1f5-134">4- Click **Pricing tier** toospecify hello service tier and performance level for your new server.</span></span> <span data-ttu-id="aa1f5-135">Compute-enhet kan konfigureras mellan 50 och 100 i grundläggande nivån, 100 och 200 i standardnivån, och lagringsutrymme kan läggas till baserat på inkluderade belopp.</span><span class="sxs-lookup"><span data-stu-id="aa1f5-135">Compute Unit can be configured between 50 and 100 in Basic tier, 100 and 200 in Standard tier, and storage can be added based on included amount.</span></span> <span data-ttu-id="aa1f5-136">Den här guiden Ta väljer vi 50 beräknings-enhet och 50GB.</span><span class="sxs-lookup"><span data-stu-id="aa1f5-136">For this HowTo guide, let’s choose 50 Compute Unit and 50GB.</span></span> <span data-ttu-id="aa1f5-137">Klicka på **OK** toosave valet.</span><span class="sxs-lookup"><span data-stu-id="aa1f5-137">Click **OK** toosave your selection.</span></span>
<span data-ttu-id="aa1f5-138">![Skapa-server--prisnivån](./media/howto-create-manage-server-portal/create-server-pricing-tier.png)</span><span class="sxs-lookup"><span data-stu-id="aa1f5-138">![create-server-pricing-tier](./media/howto-create-manage-server-portal/create-server-pricing-tier.png)</span></span>

<span data-ttu-id="aa1f5-139">5-Klicka **skapa** tooprovision hello server.</span><span class="sxs-lookup"><span data-stu-id="aa1f5-139">5- Click **Create** tooprovision hello server.</span></span> <span data-ttu-id="aa1f5-140">Etableringen tar några minuter.</span><span class="sxs-lookup"><span data-stu-id="aa1f5-140">Provisioning takes a few minutes.</span></span>

> <span data-ttu-id="aa1f5-141">Kontrollera hello **PIN-kod toodashboard** alternativet tooallow enkel spårning av dina distributioner.</span><span class="sxs-lookup"><span data-stu-id="aa1f5-141">Check hello **Pin toodashboard** option tooallow easy tracking of your deployments.</span></span>
> [!NOTE]
> <span data-ttu-id="aa1f5-142">Även om too1000GB i grundläggande nivån och 10000GB i Standard nivå ha stöd för lagring, för Public Preview är hello maximalt lagringsutrymme fortfarande begränsad too1000GB tillfälligt.</span><span class="sxs-lookup"><span data-stu-id="aa1f5-142">Although up too1000GB in Basic tier and 10000GB in Standard tier will be supported for storage, for Public Preview, hello maximum storage is still limited too1000GB temporarily.</span></span> 
</Include>

## <a name="update-an-azure-database-for-mysql-server"></a><span data-ttu-id="aa1f5-143">Uppdatera en Azure-databas för MySQL-server</span><span class="sxs-lookup"><span data-stu-id="aa1f5-143">Update an Azure Database for MySQL server</span></span>
<span data-ttu-id="aa1f5-144">När nya servern är etablerad användare har 2 alternativ tooedit en befintlig server: återställa administratörslösenord eller skala upp eller ned hello server genom att ändra hello beräknings-enheter.</span><span class="sxs-lookup"><span data-stu-id="aa1f5-144">After new server is provisioned, user has 2 options tooedit an existing server: reset administrator password or scale up/down hello server by changing hello compute-units.</span></span>

### <a name="change-hello-administrator-user-password"></a><span data-ttu-id="aa1f5-145">Ändra Hej administratör användarens lösenord</span><span class="sxs-lookup"><span data-stu-id="aa1f5-145">Change hello administrator user password</span></span>
<span data-ttu-id="aa1f5-146">1 - på hello server **översikt** bladet, klickar du på **Återställ lösenord** toopopulate ett lösenord indata och bekräftelsen fönster.</span><span class="sxs-lookup"><span data-stu-id="aa1f5-146">1- On hello server **Overview** blade, click **Reset password** toopopulate a password input and confirmation window.</span></span>

<span data-ttu-id="aa1f5-147">2 - Ange nytt lösenord och bekräfta hello lösenord i hello fönster enligt nedan: ![Återställ lösenord](./media/howto-create-manage-server-portal/reset-password.png)</span><span class="sxs-lookup"><span data-stu-id="aa1f5-147">2- Enter new password and confirm hello password in hello window as below: ![reset-password](./media/howto-create-manage-server-portal/reset-password.png)</span></span>

<span data-ttu-id="aa1f5-148">3-Klicka **OK** toosave hello nytt lösenord.</span><span class="sxs-lookup"><span data-stu-id="aa1f5-148">3- Click **OK** toosave hello new password.</span></span>

### <a name="scale-updown-by-changing-compute-units"></a><span data-ttu-id="aa1f5-149">Skala upp eller ned genom att ändra Compute-enheter</span><span class="sxs-lookup"><span data-stu-id="aa1f5-149">Scale up/down by changing Compute Units</span></span>

<span data-ttu-id="aa1f5-150">1 - på hello serverblad under **inställningar**, klickar du på **prisnivå** tooopen hello priser nivå bladet för hello Azure-databas för MySQL-servern.</span><span class="sxs-lookup"><span data-stu-id="aa1f5-150">1- On hello server blade, under **Settings**, click **Pricing tier** tooopen hello Pricing tier blade for hello Azure Database for MySQL server.</span></span>

<span data-ttu-id="aa1f5-151">2-Följ steg 4 i **skapa en Azure-databas för MySQL server** toochange Compute enheter i hello samma prisnivån.</span><span class="sxs-lookup"><span data-stu-id="aa1f5-151">2- Follow Step 4 in **Create an Azure Database for MySQL server** toochange Compute Units in hello same pricing tier.</span></span>

## <a name="delete-an-azure-database-for-mysql-server"></a><span data-ttu-id="aa1f5-152">Ta bort en Azure-databas för MySQL-server</span><span class="sxs-lookup"><span data-stu-id="aa1f5-152">Delete an Azure Database for MySQL server</span></span>

<span data-ttu-id="aa1f5-153">1 - på hello server **översikt** bladet, klickar du på **ta bort** kommando knappen tooopen hello raderas bekräftelse bladet.</span><span class="sxs-lookup"><span data-stu-id="aa1f5-153">1- On hello server **Overview** blade, click **Delete** command button tooopen hello Deleting confirmation blade.</span></span>

<span data-ttu-id="aa1f5-154">2-typen hello rätt servernamn i textrutan för hello bladet dubbla bekräftelse.</span><span class="sxs-lookup"><span data-stu-id="aa1f5-154">2- Type hello correct server name in input box of hello blade for double confirmation.</span></span>

<span data-ttu-id="aa1f5-155">3-Klicka **ta bort** knappen igen tooconfirm bort åtgärd och vänta tills ”ta bort lyckades” popup på hello meddelandefältet.</span><span class="sxs-lookup"><span data-stu-id="aa1f5-155">3- Click **Delete** button again tooconfirm deleting action and wait for “Deleting success” popup on hello notification bar.</span></span>

## <a name="list-hello-azure-database-for-mysql-databases"></a><span data-ttu-id="aa1f5-156">Lista hello Azure-databas för MySQL-databaser</span><span class="sxs-lookup"><span data-stu-id="aa1f5-156">List hello Azure Database for MySQL databases</span></span>
<span data-ttu-id="aa1f5-157">På hello server **översikt** bladet Bläddra nedåt tills du ser hello databasen panelen på hello längst ned.</span><span class="sxs-lookup"><span data-stu-id="aa1f5-157">On hello server **Overview** blade, scroll down until you see hello database tile on hello bottom.</span></span> <span data-ttu-id="aa1f5-158">Alla hello databaser visas i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="aa1f5-158">All hello databases will be listed in hello table.</span></span> <span data-ttu-id="aa1f5-159">Klicka på **ta bort** kommando knappen tooopen hello raderas bekräftelse bladet.</span><span class="sxs-lookup"><span data-stu-id="aa1f5-159">click **Delete** command button tooopen hello Deleting confirmation blade.</span></span>

![Visa-databaser](./media/howto-create-manage-server-portal/show-databases.png)

## <a name="show-details-of-an-azure-database-for-mysql-server"></a><span data-ttu-id="aa1f5-161">Visa detaljer för en Azure-databas för MySQL-server</span><span class="sxs-lookup"><span data-stu-id="aa1f5-161">Show details of an Azure Database for MySQL server</span></span>
<span data-ttu-id="aa1f5-162">Klicka på **egenskaper** under **inställningar** på hello servern öppnas bladet hello **egenskaper** bladet.</span><span class="sxs-lookup"><span data-stu-id="aa1f5-162">Click **Properties** under **Settings** on hello server blade will open hello **Properties** blade.</span></span> <span data-ttu-id="aa1f5-163">Sedan kan du visa alla detaljerad information om hello-server.</span><span class="sxs-lookup"><span data-stu-id="aa1f5-163">Then you can view all detailed information about hello server.</span></span>

## <a name="next-steps"></a><span data-ttu-id="aa1f5-164">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="aa1f5-164">Next steps</span></span>

[<span data-ttu-id="aa1f5-165">Snabbstart: Skapa Azure-databas för MySQL-server med hjälp av Azure portal</span><span class="sxs-lookup"><span data-stu-id="aa1f5-165">Quickstart: Create Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
