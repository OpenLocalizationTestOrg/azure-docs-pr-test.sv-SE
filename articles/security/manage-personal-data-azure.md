---
title: aaaManage personliga data i Microsoft Azure | Microsoft Docs
description: information om hur toocorrect, uppdatera, ta bort och exportera personliga data i Azure Active Directory och Azure SQL Database
services: security
documentationcenter: na
author: barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: barclayn
ms.openlocfilehash: 032f70d32377cb9395cb2c35c27dad05001537c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-personal-data-in-microsoft-azure"></a><span data-ttu-id="21670-103">Hantera personliga data i Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="21670-103">Manage personal data in Microsoft Azure</span></span>

<span data-ttu-id="21670-104">Den här artikeln innehåller råd om hur toocorrect, uppdatera, ta bort och exportera personliga data i Azure Active Directory och Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="21670-104">This article provides guidance on how toocorrect, update, delete, and export personal data in Azure Active Directory and Azure SQL Database.</span></span>

## <a name="scenario"></a><span data-ttu-id="21670-105">Scenario</span><span class="sxs-lookup"><span data-stu-id="21670-105">Scenario</span></span>

<span data-ttu-id="21670-106">En Dublin-baserade företaget tillhandahåller en arbetsuppgift för avancerad mål bröllop Irland och hälsningsmeddelande för både en lokal och kundbas.</span><span class="sxs-lookup"><span data-stu-id="21670-106">A Dublin-based company provides one-stop shopping for high end destination weddings in Ireland and around hello world for both a local and international customer base.</span></span> <span data-ttu-id="21670-107">De har kontor, kunder, anställda och leverantörer som finns runt hello world toofully service hello handelsplatser de erbjuder.</span><span class="sxs-lookup"><span data-stu-id="21670-107">They have offices, customers, employees, and vendors located around hello world toofully service hello venues they offer.</span></span>

<span data-ttu-id="21670-108">Hello företagets håller reda på svar som inkluderar Matallergier och dietetisk inställningar mellan många andra objekt.</span><span class="sxs-lookup"><span data-stu-id="21670-108">Among many other items, hello company keeps track of RSVPs that include food allergies and dietary preferences.</span></span> <span data-ttu-id="21670-109">Bröllop gäster kan registrera för olika aktiviteter som horseback barnet, surfa, beredskapsbåten bilar, osv., och även interagera med varandra på en central webbsida under hello månader som leder fram toohello händelse.</span><span class="sxs-lookup"><span data-stu-id="21670-109">Wedding guests can register for various activities such as horseback riding, surfing, boat rides, etc., and even interact with one another on a central web page during hello months leading up toohello event.</span></span> <span data-ttu-id="21670-110">hello företagets samlar in personlig information från anställda, leverantörer, kunder och bröllop gäster.</span><span class="sxs-lookup"><span data-stu-id="21670-110">hello company collects personal information from employees, vendors, customers, and wedding guests.</span></span> <span data-ttu-id="21670-111">På grund av hello internationella natur hello business hello företag måste vara kompatibel med flera nivåer av förordning.</span><span class="sxs-lookup"><span data-stu-id="21670-111">Because of hello international nature of hello business hello company must comply with multiple levels of regulation.</span></span>

## <a name="problem-statement"></a><span data-ttu-id="21670-112">Problembeskrivning</span><span class="sxs-lookup"><span data-stu-id="21670-112">Problem statement</span></span>

- <span data-ttu-id="21670-113">Data-administratörer måste vara kan toocorrect stämmer personlig information och uppdatera ofullständiga eller ändra personlig information.</span><span class="sxs-lookup"><span data-stu-id="21670-113">Data admins must be able toocorrect inaccurate personal information and update incomplete or changing personal information.</span></span>

- <span data-ttu-id="21670-114">Behovet av data-administratörer måste vara kan toodelete personlig information på hello-begäran för en registrerade.</span><span class="sxs-lookup"><span data-stu-id="21670-114">Data admins need must be able toodelete personal information upon hello request of a data subject.</span></span>

- <span data-ttu-id="21670-115">Data administratörer behöver tooexport data och tillhandahålla tooa registrerade i ett vanligt, strukturerade format på sin begäran.</span><span class="sxs-lookup"><span data-stu-id="21670-115">Data admins need tooexport data and provide it tooa data subject in a common, structured format upon his or her request.</span></span>

## <a name="company-goals"></a><span data-ttu-id="21670-116">Företagets mål</span><span class="sxs-lookup"><span data-stu-id="21670-116">Company goals</span></span>

- <span data-ttu-id="21670-117">Felaktiga eller ofullständiga kund, Bröllop gäst, medarbetare och personlig information om leverantören måste korrigeras eller uppdateras i Azure Active Directory och Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="21670-117">Inaccurate or incomplete customer, wedding guest, employee, and vendor personal information must be corrected or updated in Azure Active Directory and Azure SQL Database.</span></span>

- <span data-ttu-id="21670-118">Personlig information måste tas bort i Azure Active Directory och Azure SQL Database på hello-begäran för en registrerade.</span><span class="sxs-lookup"><span data-stu-id="21670-118">Personal information must be deleted in Azure Active Directory and Azure SQL Database upon hello request of a data subject.</span></span>

- <span data-ttu-id="21670-119">Personliga data måste exporteras i formatet vanliga, strukturerade på hello-begäran för en registrerade.</span><span class="sxs-lookup"><span data-stu-id="21670-119">Personal data must be exported in a common, structured format upon hello request of a data subject.</span></span>

## <a name="solutions"></a><span data-ttu-id="21670-120">Lösningar</span><span class="sxs-lookup"><span data-stu-id="21670-120">Solutions</span></span>

### <a name="azure-active-directory-rectifycorrect-inaccurate-or-incomplete-personal-data-and-erasedelete-personal-datauser-profiles"></a><span data-ttu-id="21670-121">Azure Active Directory: åtgärda/korrigera felaktiga eller ofullständiga personliga data och radera/ta bort personliga data/användarprofiler</span><span class="sxs-lookup"><span data-stu-id="21670-121">Azure Active Directory: rectify/correct inaccurate or incomplete personal data and erase/delete personal data/user profiles</span></span>

<span data-ttu-id="21670-122">[Azure Active Directory](https://azure.microsoft.com/services/active-directory/) är Microsofts molnbaserade, flera innehavare katalog och identity management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="21670-122">[Azure Active Directory](https://azure.microsoft.com/services/active-directory/) is Microsoft’s cloud-based, multi-tenant directory and identity management service.</span></span>
<span data-ttu-id="21670-123">Du kan korrigera, uppdatera eller ta bort kund- och användarprofiler och arbete användarinformation som innehåller dina personliga data, till exempel en användares namn, arbete rubrik, adress eller telefonnummer i din [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) (AAD) miljö med hjälp av hello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="21670-123">You can correct, update, or delete customer and employee user profiles and user work information that contain personal data, such as a user’s name, work title, address, or phone number, in your [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) (AAD) environment by using hello [Azure portal](https://portal.azure.com/).</span></span>

<span data-ttu-id="21670-124">Du måste logga in med ett konto som är en global administratör för hello-katalogen.</span><span class="sxs-lookup"><span data-stu-id="21670-124">You must sign in with an account that’s a global admin for hello directory.</span></span>

#### <a name="how-do-i-correct-or-update-user-profile-and-work-information-in-azure-active-directory"></a><span data-ttu-id="21670-125">Hur jag korrigera eller uppdatera användarprofil och arbete information i Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="21670-125">How do I correct or update user profile and work information in Azure Active Directory?</span></span>

1. <span data-ttu-id="21670-126">Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för hello-katalogen.</span><span class="sxs-lookup"><span data-stu-id="21670-126">Sign in toohello [Azure portal](https://portal.azure.com) with an account that's a global admin for hello directory.</span></span>

2. <span data-ttu-id="21670-127">Välj **fler tjänster**, ange **användare och grupper** i hello textruta och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="21670-127">Select **More services**, enter **Users and groups** in hello text box, and then select **Enter**.</span></span>

    ![Media/image1.png](media/manage-personal-data-azure/image001.png)

3. <span data-ttu-id="21670-129">På hello **användare och grupper** bladet väljer **användare**.</span><span class="sxs-lookup"><span data-stu-id="21670-129">On hello **Users and groups** blade, select **Users**.</span></span>

    ![Media/image2.png](media/manage-personal-data-azure/image003.png)

4. <span data-ttu-id="21670-131">På hello **användare och grupper – användare** bladet Välj en användare hello listan, och på hello bladet för hello vald användare Markera **profil** tooview hello användarprofilsinformation som behöver rättas toobe eller uppdaterade.</span><span class="sxs-lookup"><span data-stu-id="21670-131">On hello **Users and groups - Users** blade, select a user from hello list, and then, on hello blade for hello selected user, select **Profile** tooview hello user profile information that needs toobe corrected or updated.</span></span>

    ![Media/image3.png](media/manage-personal-data-azure/image005.png)

5. <span data-ttu-id="21670-133">Korrigera eller uppdatera hello information och markera i kommandofältet hello **spara.**</span><span class="sxs-lookup"><span data-stu-id="21670-133">Correct or update hello information, and then, in hello command bar, select **Save.**</span></span>

6.  <span data-ttu-id="21670-134">På hello bladet för valda hello-användaren väljer **fungerar Info** tooview arbete användarinformation som behöver toobe åtgärdat eller uppdateras.</span><span class="sxs-lookup"><span data-stu-id="21670-134">On hello blade for hello selected user, select **Work Info** tooview user work information that needs toobe corrected or updated.</span></span>

    ![Media/image4.png](media/manage-personal-data-azure/image007.png)

7. <span data-ttu-id="21670-136">Korrigera eller uppdatera hello användarinformation för arbete och markera i kommandofältet hello **spara.**</span><span class="sxs-lookup"><span data-stu-id="21670-136">Correct or update hello user work information, and then, in hello command bar, select **Save.**</span></span>

#### <a name="how-do-i-delete-a-user-profile-in-azure-active-directory"></a><span data-ttu-id="21670-137">Hur tar jag bort en användarprofil i Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="21670-137">How do I delete a user profile in Azure Active Directory?</span></span>

1. <span data-ttu-id="21670-138">Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för hello-katalogen.</span><span class="sxs-lookup"><span data-stu-id="21670-138">Sign in toohello [Azure portal](https://portal.azure.com) with an account that's a global admin for hello directory.</span></span>

2. <span data-ttu-id="21670-139">Välj **fler tjänster**, ange **användare och grupper** i hello textruta och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="21670-139">Select **More services**, enter **Users and groups** in hello text box, and then select **Enter**.</span></span>

    ![](media/manage-personal-data-azure/image001.png)

3. <span data-ttu-id="21670-140">På hello **användare och grupper** bladet väljer **användare**.</span><span class="sxs-lookup"><span data-stu-id="21670-140">On hello **Users and groups** blade, select **Users**.</span></span>

    ![Media/image2.png](media/manage-personal-data-azure/image003.png)

4. <span data-ttu-id="21670-142">På hello **användare och grupper – användare** bladet Välj en användare hello listan.</span><span class="sxs-lookup"><span data-stu-id="21670-142">On hello **Users and groups - Users** blade, select a user from hello list.</span></span>

    ![Media/image3.png](media/manage-personal-data-azure/image007.png)

5. <span data-ttu-id="21670-144">På hello bladet för valda hello-användaren väljer **översikt**, och välj sedan i kommandofältet hello **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="21670-144">On hello blade for hello selected user, select **Overview**, and then in hello command bar, select **Delete**.</span></span>

    ![](media/manage-personal-data-azure/image013.png)

### <a name="sql-database-rectifycorrect-inaccurate-or-incomplete-personal-data-erasedelete-personal-data-export-personal-data"></a><span data-ttu-id="21670-145">SQL Database: åtgärda/korrigera felaktiga eller ofullständiga personuppgifter. Radera/ta bort personuppgifter. Exportera personliga data</span><span class="sxs-lookup"><span data-stu-id="21670-145">SQL Database: rectify/correct inaccurate or incomplete personal data; erase/delete personal data; export personal data</span></span> 

<span data-ttu-id="21670-146">[Azure SQL Database](https://azure.microsoft.com/services/sql-database/?v=16.50) är en databas i molnet som gör att utvecklare kan skapa och hantera program.</span><span class="sxs-lookup"><span data-stu-id="21670-146">[Azure SQL Database](https://azure.microsoft.com/services/sql-database/?v=16.50) is a cloud database that helps developers build and maintain applications.</span></span>

<span data-ttu-id="21670-147">Personliga data uppdateras i [Azure SQL Database](https://azure.microsoft.com/services/sql-database/?v=16.50) med standard SQL-frågor och kan också tas bort.</span><span class="sxs-lookup"><span data-stu-id="21670-147">Personal data can be updated in [Azure SQL Database](https://azure.microsoft.com/services/sql-database/?v=16.50) using standard SQL queries, and it can also be deleted.</span></span> <span data-ttu-id="21670-148">Dessutom kan personuppgifter exporteras från SQL-databas med olika metoder, inklusive hello Azure SQL Server-import och export-guiden och i olika format, inklusive en BACPAC-fil.</span><span class="sxs-lookup"><span data-stu-id="21670-148">Additionally, personal data can be exported from SQL Database using a variety of methods, including hello Azure SQL Server import and export wizard, and in a variety of formats, including a BACPAC file.</span></span>

#### <a name="how-do-i-correct-update-or-erase-personal-data-in-sql-database"></a><span data-ttu-id="21670-149">Hur jag korrigera, uppdatera eller ta bort personliga data i SQL-databas?</span><span class="sxs-lookup"><span data-stu-id="21670-149">How do I correct, update, or erase personal data in SQL Database?</span></span>

<span data-ttu-id="21670-150">toolearn hur toocorrect eller uppdatera personliga data i SQL-databas finns hello [uppdatering (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/queries/update-transact-sql), [uppdatera Text](https://docs.microsoft.com/sql/t-sql/queries/updatetext-transact-sql), [uppdatera med vanliga tabelluttryck](https://docs.microsoft.com/sql/t-sql/queries/with-common-table-expression-transact-sql), eller [ Uppdatera skriva Text](https://docs.microsoft.com/sql/t-sql/queries/writetext-transact-sql) dokumentation.</span><span class="sxs-lookup"><span data-stu-id="21670-150">toolearn how toocorrect or update personal data in SQL Database, visit hello [Update (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/queries/update-transact-sql), [Update Text](https://docs.microsoft.com/sql/t-sql/queries/updatetext-transact-sql), [Update with Common Table Expression](https://docs.microsoft.com/sql/t-sql/queries/with-common-table-expression-transact-sql), or [Update Write Text](https://docs.microsoft.com/sql/t-sql/queries/writetext-transact-sql) documentation.</span></span>

<span data-ttu-id="21670-151">toolearn hur toodelete personliga data i SQL-databas finns hello [ta bort (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/delete-transact-sql) dokumentation.</span><span class="sxs-lookup"><span data-stu-id="21670-151">toolearn how toodelete personal data in SQL Database, visit hello [Delete (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/delete-transact-sql) documentation.</span></span>

#### <a name="how-do-i-export-personal-data-tooa-bacpac-file-in-sql-database"></a><span data-ttu-id="21670-152">Hur exporterar personuppgifter tooa BACPAC filen i SQL-databasen?</span><span class="sxs-lookup"><span data-stu-id="21670-152">How do I export personal data tooa BACPAC file in SQL Database?</span></span>

<span data-ttu-id="21670-153">En BACPAC-fil innehåller hello SQL-databas data och metadata och är en zip-fil med filnamnstillägget BACPAC.</span><span class="sxs-lookup"><span data-stu-id="21670-153">A BACPAC file includes hello SQL Database data and metadata and is a zip file with a BACPAC extension.</span></span> <span data-ttu-id="21670-154">Detta kan göras med hjälp av hello [Azure-portalen](https://portal.azure.com/), hello SQLPackage kommandoradsverktyg, SQL Server Management Studio (SSMS) eller PowerShell.</span><span class="sxs-lookup"><span data-stu-id="21670-154">This can be done using hello [Azure portal](https://portal.azure.com/), hello SQLPackage command-line utility, SQL Server Management Studio (SSMS), or PowerShell.</span></span>

<span data-ttu-id="21670-155">toolearn hur tooexport tooa BACPAC datafilen, besök hello [exportera en Azure SQL database tooa BACPAC fil](https://docs.microsoft.com/azure/sql-database/sql-database-export) som innehåller detaljerade anvisningar för varje metod som anges ovan.</span><span class="sxs-lookup"><span data-stu-id="21670-155">toolearn how tooexport data tooa BACPAC file, visit hello [Export an Azure SQL database tooa BACPAC file](https://docs.microsoft.com/azure/sql-database/sql-database-export) page, which includes detailed instructions for each method listed above.</span></span>

<span data-ttu-id="21670-156">Hur exporterar personliga data från SQL-databas med hello SQL Server importera och exportera guiden?</span><span class="sxs-lookup"><span data-stu-id="21670-156">How do I export personal data from SQL Database with hello SQL Server Import and Export Wizard?</span></span>

<span data-ttu-id="21670-157">Den här guiden hjälper dig att kopiera data från en källa tooa mål.</span><span class="sxs-lookup"><span data-stu-id="21670-157">This wizard helps you copy data from a source tooa destination.</span></span> <span data-ttu-id="21670-158">För en introduktion toohello guiden, inklusive hur tooget den behörighetsinformation och hur tooget med hello-verktyget finns hello [importera och exportera Data med hello SQL Server importera och exportera guiden](https://docs.microsoft.com/sql/integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard) webbsida.</span><span class="sxs-lookup"><span data-stu-id="21670-158">For an introduction toohello wizard, including how tooget it, permissions information, and how tooget help with hello tool, visit hello [Import and Export Data with hello SQL Server Import and Export Wizard](https://docs.microsoft.com/sql/integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard) web page.</span></span>

<span data-ttu-id="21670-159">En översikt över stegen för hello guiden finns hello [steg i hello SQL Server importera och exportera guiden](https://docs.microsoft.com/sql/integration-services/import-export-data/steps-in-the-sql-server-import-and-export-wizard) webbsida.</span><span class="sxs-lookup"><span data-stu-id="21670-159">For an overview of steps for hello wizard, visit hello [Steps in hello SQL Server Import and Export Wizard](https://docs.microsoft.com/sql/integration-services/import-export-data/steps-in-the-sql-server-import-and-export-wizard) web page.</span></span>

## <a name="next-steps"></a><span data-ttu-id="21670-160">Nästa steg:</span><span class="sxs-lookup"><span data-stu-id="21670-160">Next Steps:</span></span>

[<span data-ttu-id="21670-161">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="21670-161">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/?v=16.50) 

[<span data-ttu-id="21670-162">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="21670-162">Azure Active Directory</span></span>](https://azure.microsoft.com/services/active-directory/)

