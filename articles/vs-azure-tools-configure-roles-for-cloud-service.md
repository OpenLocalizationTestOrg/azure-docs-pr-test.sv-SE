---
title: "aaaConfigure hello roller för en Azure cloud service med Visual Studio | Microsoft Docs"
description: "Lär dig hur tooset upp och konfigurera roller för Azure-molntjänster med Visual Studio."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: d397ef87-64e5-401a-aad5-7f83f1022e16
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/21/2017
ms.author: kraigb
ms.openlocfilehash: d3c62eb57040ebe987787e73b17b468bb82122bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-cloud-service-roles-with-visual-studio"></a><span data-ttu-id="bff1f-103">Konfigurera roller för Azure cloud service med Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bff1f-103">Configure Azure cloud service roles with Visual Studio</span></span>
<span data-ttu-id="bff1f-104">En Azure-molntjänst kan ha en eller flera worker webbroller.</span><span class="sxs-lookup"><span data-stu-id="bff1f-104">An Azure cloud service can have one or more worker or web roles.</span></span> <span data-ttu-id="bff1f-105">För varje roll måste toodefine hur rollen har ställts in och även konfigurera hur rollen körs.</span><span class="sxs-lookup"><span data-stu-id="bff1f-105">For each role, you need toodefine how that role is set up and also configure how that role runs.</span></span> <span data-ttu-id="bff1f-106">toolearn mer information om roller i molntjänster, se hello video [introduktion tooAzure molntjänster](https://channel9.msdn.com/Series/Windows-Azure-Cloud-Services-Tutorials/Introduction-to-Windows-Azure-Cloud-Services).</span><span class="sxs-lookup"><span data-stu-id="bff1f-106">toolearn more about roles in cloud services, see hello video [Introduction tooAzure Cloud Services](https://channel9.msdn.com/Series/Windows-Azure-Cloud-Services-Tutorials/Introduction-to-Windows-Azure-Cloud-Services).</span></span> 

<span data-ttu-id="bff1f-107">hello-information för din molntjänst lagras i hello följande filer:</span><span class="sxs-lookup"><span data-stu-id="bff1f-107">hello information for your cloud service is stored in hello following files:</span></span>

- <span data-ttu-id="bff1f-108">**ServiceDefinition.csdef** -hello tjänstdefinitionsfilen definierar hello körningsinställningar för Molntjänsten, inklusive vilka roller är obligatoriska, slutpunkter och storlek på virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="bff1f-108">**ServiceDefinition.csdef** - hello service definition file defines hello runtime settings for your cloud service including what roles are required, endpoints, and virtual machine size.</span></span> <span data-ttu-id="bff1f-109">Ingen av hello data som lagras i `ServiceDefinition.csdef` kan ändras när den körs.</span><span class="sxs-lookup"><span data-stu-id="bff1f-109">None of hello data stored in `ServiceDefinition.csdef` can be changed when your role is running.</span></span>
- <span data-ttu-id="bff1f-110">**ServiceConfiguration.cscfg** - hello tjänstkonfigurationsfilen konfigurerar hur många instanser av en roll körs och hello värden för hello inställningar för en roll.</span><span class="sxs-lookup"><span data-stu-id="bff1f-110">**ServiceConfiguration.cscfg** - hello service configuration file configures how many instances of a role are run and hello values of hello settings defined for a role.</span></span> <span data-ttu-id="bff1f-111">Hej data som lagras i `ServiceConfiguration.cscfg` kan ändras när din roll körs.</span><span class="sxs-lookup"><span data-stu-id="bff1f-111">hello data stored in `ServiceConfiguration.cscfg` can be changed while your role is running.</span></span>

<span data-ttu-id="bff1f-112">toostore olika värden för hello inställningar som styr hur en roll körs, kan du definiera flera konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="bff1f-112">toostore different values for hello settings that control how a role runs, you can define multiple service configurations.</span></span> <span data-ttu-id="bff1f-113">Du kan använda en annan tjänstkonfiguration för varje distributionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="bff1f-113">You can use a different service configuration for each deployment environment.</span></span> <span data-ttu-id="bff1f-114">Du kan till exempel ange din lagring konto anslutning sträng toouse hello lokala Azure storage-emulatorn i en lokal tjänst-konfiguration och skapa en annan service configuration toouse Azure storage i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="bff1f-114">For example, you can set your storage account connection string toouse hello local Azure storage emulator in a local service configuration and create another service configuration toouse Azure storage in hello cloud.</span></span>

<span data-ttu-id="bff1f-115">När du skapar en Azure-molntjänst i Visual Studio två tjänstkonfiguration skapas automatiskt och läggas till tooyour Azure-projekt:</span><span class="sxs-lookup"><span data-stu-id="bff1f-115">When you create an Azure cloud service in Visual Studio, two service configurations are automatically created and added tooyour Azure project:</span></span>

- `ServiceConfiguration.Cloud.cscfg`
- `ServiceConfiguration.Local.cscfg`

## <a name="configure-an-azure-cloud-service"></a><span data-ttu-id="bff1f-116">Konfigurera en Azure-molntjänst</span><span class="sxs-lookup"><span data-stu-id="bff1f-116">Configure an Azure cloud service</span></span>
<span data-ttu-id="bff1f-117">Du kan konfigurera en Azure-molntjänst från Solution Explorer i Visual Studio enligt hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="bff1f-117">You can configure an Azure cloud service from Solution Explorer in Visual Studio, as shown in hello following steps:</span></span>

1. <span data-ttu-id="bff1f-118">Skapa eller öppna ett Azure cloud service-projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bff1f-118">Create or open an Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="bff1f-119">I **Solution Explorer**, högerklicka på hello-projektet och, hello snabbmenyn väljer **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="bff1f-119">In **Solution Explorer**, right-click hello project, and, from hello context menu, select **Properties**.</span></span>
   
    ![Snabbmenyn för Solution Explorer-projekt](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-project-context-menu.png)

1. <span data-ttu-id="bff1f-121">Välj hello i hello projektets egenskapssida **Development** fliken.</span><span class="sxs-lookup"><span data-stu-id="bff1f-121">In hello project's properties page, select hello **Development** tab.</span></span> 

    ![Projektet egenskapssidan - utveckling fliken](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-development-tab.png)

1. <span data-ttu-id="bff1f-123">I hello **tjänstkonfiguration** listan, Välj hello namnet på tjänstkonfigurationen för hello som du vill tooedit.</span><span class="sxs-lookup"><span data-stu-id="bff1f-123">In hello **Service Configuration** list, select hello name of hello service configuration that you want tooedit.</span></span> <span data-ttu-id="bff1f-124">(Om du vill toomake ändras tooall hello tjänstkonfiguration för den här rollen, Välj **alla konfigurationer av**.)</span><span class="sxs-lookup"><span data-stu-id="bff1f-124">(If you want toomake changes tooall hello service configurations for this role, select **All Configurations**.)</span></span>
   
    > [!IMPORTANT]
    > <span data-ttu-id="bff1f-125">Om du väljer en specifik tjänstkonfiguration har vissa egenskaper inaktiverats eftersom de kan bara anges för alla konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="bff1f-125">If you choose a specific service configuration, some properties are disabled because they can only be set for all configurations.</span></span> <span data-ttu-id="bff1f-126">tooedit dessa egenskaper, måste du välja **alla konfigurationer av**.</span><span class="sxs-lookup"><span data-stu-id="bff1f-126">tooedit these properties, you must select **All Configurations**.</span></span>
    > 
    > 
   
    ![Tjänstkonfigurationen listan för en Azure-molntjänst](./media/vs-azure-tools-configure-roles-for-cloud-service/cloud-service-service-configuration-property.png)

## <a name="change-hello-number-of-role-instances"></a><span data-ttu-id="bff1f-128">Ändra hello antalet rollinstanser</span><span class="sxs-lookup"><span data-stu-id="bff1f-128">Change hello number of role instances</span></span>
<span data-ttu-id="bff1f-129">Du kan ändra hello antal instanser av en roll som körs, baserat på hello antalet användare eller hello belastning som förväntas för en viss roll tooimprove hello prestanda för Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="bff1f-129">tooimprove hello performance of your cloud service, you can change hello number of instances of a role that are running, based on hello number of users or hello load expected for a particular role.</span></span> <span data-ttu-id="bff1f-130">En separat virtuell dator skapas för varje instans av rollen när hello molnbaserad tjänst som körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="bff1f-130">A separate virtual machine is created for each instance of a role when hello cloud service runs in Azure.</span></span> <span data-ttu-id="bff1f-131">Detta påverkar hello fakturering för hello distribution av den här Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="bff1f-131">This affects hello billing for hello deployment of this cloud service.</span></span> <span data-ttu-id="bff1f-132">Mer information om fakturering finns [förstå fakturan för Microsoft Azure](billing/billing-understand-your-bill.md).</span><span class="sxs-lookup"><span data-stu-id="bff1f-132">For more information about billing, see [Understand your bill for Microsoft Azure](billing/billing-understand-your-bill.md).</span></span>

1. <span data-ttu-id="bff1f-133">Skapa eller öppna ett Azure cloud service-projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bff1f-133">Create or open an Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="bff1f-134">I **Solution Explorer**, expandera hello projektnoden.</span><span class="sxs-lookup"><span data-stu-id="bff1f-134">In **Solution Explorer**, expand hello project node.</span></span> <span data-ttu-id="bff1f-135">Under hello **roller** nod, hello genom att högerklicka på rollen du vill tooupdate och, hello snabbmenyn väljer **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="bff1f-135">Under hello **Roles** node, right-click hello role you want tooupdate, and, from hello context menu, select **Properties**.</span></span>

    ![Snabbmenyn för Solution Explorer Azure roll](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. <span data-ttu-id="bff1f-137">Välj hello **Configuration** fliken.</span><span class="sxs-lookup"><span data-stu-id="bff1f-137">Select hello **Configuration** tab.</span></span>

    ![Konfigurationsfliken](./media/vs-azure-tools-configure-roles-for-cloud-service/role-configuration-properties-page.png)

1. <span data-ttu-id="bff1f-139">I hello **tjänstkonfiguration** listan, Välj hello tjänstkonfiguration som du vill tooupdate.</span><span class="sxs-lookup"><span data-stu-id="bff1f-139">In hello **Service Configuration** list, select hello service configuration that you want tooupdate.</span></span>
   
    ![Tjänstkonfigurationen lista](./media/vs-azure-tools-configure-roles-for-cloud-service/role-configuration-properties-page-select-configuration.png)

1. <span data-ttu-id="bff1f-141">I hello **instansen antal** text Ange hello antalet instanser som du vill toostart för den här rollen.</span><span class="sxs-lookup"><span data-stu-id="bff1f-141">In hello **Instance count** text box, enter hello number of instances that you want toostart for this role.</span></span> <span data-ttu-id="bff1f-142">När du publicerar hello cloud service tooAzure körs varje instans på en separat virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="bff1f-142">Each instance runs on a separate virtual machine when you publish hello cloud service tooAzure.</span></span>

    ![Uppdatera hello antal instanser](./media/vs-azure-tools-configure-roles-for-cloud-service/role-configuration-properties-page-instance-count.png)

1. <span data-ttu-id="bff1f-144">Från hello Visual Studio i verktygsfältet väljer **spara**.</span><span class="sxs-lookup"><span data-stu-id="bff1f-144">From hello Visual Studio, toolbar, select **Save**.</span></span>

## <a name="manage-connection-strings-for-storage-accounts"></a><span data-ttu-id="bff1f-145">Hantera anslutningssträngar för storage-konton</span><span class="sxs-lookup"><span data-stu-id="bff1f-145">Manage connection strings for storage accounts</span></span>
<span data-ttu-id="bff1f-146">Du kan lägga till, ta bort eller ändra anslutningssträngar för service-konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="bff1f-146">You can add, remove, or modify connection strings for your service configurations.</span></span> <span data-ttu-id="bff1f-147">Du kan exempelvis lokala anslutningssträngen för en lokal tjänst-konfiguration som har värdet `UseDevelopmentStorage=true`.</span><span class="sxs-lookup"><span data-stu-id="bff1f-147">For example, you might want a local connection string for a local service configuration that has a value of `UseDevelopmentStorage=true`.</span></span> <span data-ttu-id="bff1f-148">Du kan också tooconfigure en tjänstkonfiguration i molnet som använder ett lagringskonto i Azure.</span><span class="sxs-lookup"><span data-stu-id="bff1f-148">You might also want tooconfigure a cloud service configuration that uses a storage account in Azure.</span></span>

> [!WARNING]
> <span data-ttu-id="bff1f-149">När du anger hello Azure storage-konto viktig information för en anslutningssträng för storage-konto, lagras informationen lokalt i konfigurationsfilen för hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="bff1f-149">When you enter hello Azure storage account key information for a storage account connection string, this information is stored locally in hello service configuration file.</span></span> <span data-ttu-id="bff1f-150">Men den här informationen lagras för närvarande inte som krypterad text.</span><span class="sxs-lookup"><span data-stu-id="bff1f-150">However, this information is currently not stored as encrypted text.</span></span>
> 
> 

<span data-ttu-id="bff1f-151">Genom att använda ett annat värde för varje service-konfiguration kan du inte har toouse olika anslutningssträngar i Molntjänsten eller ändra koden när du publicerar ditt moln tjänsten tooAzure.</span><span class="sxs-lookup"><span data-stu-id="bff1f-151">By using a different value for each service configuration, you do not have toouse different connection strings in your cloud service or modify your code when you publish your cloud service tooAzure.</span></span> <span data-ttu-id="bff1f-152">Du kan använda hello samma namn för hello anslutningssträngen i koden och hello-värdet är olika, baserat på hello tjänstkonfiguration som du väljer när du skapar din molntjänst eller när du publicerar den.</span><span class="sxs-lookup"><span data-stu-id="bff1f-152">You can use hello same name for hello connection string in your code and hello value is different, based on hello service configuration that you select when you build your cloud service or when you publish it.</span></span>

1. <span data-ttu-id="bff1f-153">Skapa eller öppna ett Azure cloud service-projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bff1f-153">Create or open an Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="bff1f-154">I **Solution Explorer**, expandera hello projektnoden.</span><span class="sxs-lookup"><span data-stu-id="bff1f-154">In **Solution Explorer**, expand hello project node.</span></span> <span data-ttu-id="bff1f-155">Under hello **roller** nod, hello genom att högerklicka på rollen du vill tooupdate och, hello snabbmenyn väljer **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="bff1f-155">Under hello **Roles** node, right-click hello role you want tooupdate, and, from hello context menu, select **Properties**.</span></span>

    ![Snabbmenyn för Solution Explorer Azure roll](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. <span data-ttu-id="bff1f-157">Välj hello **inställningar** fliken.</span><span class="sxs-lookup"><span data-stu-id="bff1f-157">Select hello **Settings** tab.</span></span>

    ![Fliken Inställningar](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab.png)

1. <span data-ttu-id="bff1f-159">I hello **tjänstkonfiguration** listan, Välj hello tjänstkonfiguration som du vill tooupdate.</span><span class="sxs-lookup"><span data-stu-id="bff1f-159">In hello **Service Configuration** list, select hello service configuration that you want tooupdate.</span></span>

    ![Tjänstkonfiguration](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-select-configuration.png)

1. <span data-ttu-id="bff1f-161">Välj tooadd en anslutningssträng **Lägg till inställning**.</span><span class="sxs-lookup"><span data-stu-id="bff1f-161">tooadd a connection string, select **Add Setting**.</span></span>

    ![Lägg till anslutningssträng](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting.png)

1. <span data-ttu-id="bff1f-163">När en ny inställning för hello har lagts toohello lista, uppdatera hello rad i hello lista hello nödvändig information.</span><span class="sxs-lookup"><span data-stu-id="bff1f-163">Once hello new setting has been added toohello list, update hello row in hello list with hello necessary information.</span></span>

    ![Ny anslutningssträng](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting-new-setting.png)

    - <span data-ttu-id="bff1f-165">**Namnet** -ange hello namn som du vill toouse för hello anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="bff1f-165">**Name** - Enter hello name that you want toouse for hello connection string.</span></span>
    - <span data-ttu-id="bff1f-166">**Typen** – Välj **anslutningssträngen** hello nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="bff1f-166">**Type** - Select **Connection String** from hello drop-down list.</span></span>
    - <span data-ttu-id="bff1f-167">**Värdet** -du kan antingen ange hello anslutningssträngen direkt i hello **värdet** cell eller välj hello ellips (...) toowork i hello **skapa Lagringsanslutningssträng** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bff1f-167">**Value** - You can either enter hello connection string directly into hello **Value** cell, or select hello ellipsis (...) toowork in hello **Create Storage Connection String** dialog.</span></span>  

1. <span data-ttu-id="bff1f-168">I hello **skapa Lagringsanslutningssträng** väljer ett alternativ för **ansluta med**.</span><span class="sxs-lookup"><span data-stu-id="bff1f-168">In hello **Create Storage Connection String** dialog, select an option for **Connect using**.</span></span> <span data-ttu-id="bff1f-169">Följ sedan instruktionerna för hello för hello alternativ du väljer:</span><span class="sxs-lookup"><span data-stu-id="bff1f-169">Then follow hello instructions for hello option you select:</span></span>

    - <span data-ttu-id="bff1f-170">**Microsoft Azure storage-emulatorn** -om du väljer det här alternativet hello återstående inställningar i dialogrutan hello har inaktiverats eftersom de gäller endast tooAzure.</span><span class="sxs-lookup"><span data-stu-id="bff1f-170">**Microsoft Azure storage emulator** - If you select this option, hello remaining settings on hello dialog are disabled as they apply only tooAzure.</span></span> <span data-ttu-id="bff1f-171">Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="bff1f-171">Select **OK**.</span></span>
    - <span data-ttu-id="bff1f-172">**Din prenumeration** – om du väljer det här alternativet, väljer du Använd hello listrutan tooeither och logga in ett Microsoft-konto eller Lägg till ett Microsoft-konto.</span><span class="sxs-lookup"><span data-stu-id="bff1f-172">**Your subscription** - If you select this option, use hello dropdown list tooeither select and sign into a Microsoft account, or add a Microsoft account.</span></span> <span data-ttu-id="bff1f-173">Välj ett Azure-prenumeration och storage-konto.</span><span class="sxs-lookup"><span data-stu-id="bff1f-173">Select an Azure subscription and storage account.</span></span> <span data-ttu-id="bff1f-174">Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="bff1f-174">Select **OK**.</span></span>
    - <span data-ttu-id="bff1f-175">**Autentiseringsuppgifterna anges manuellt** – ange hello lagringskontonamn och antingen hello primärnyckel eller andra.</span><span class="sxs-lookup"><span data-stu-id="bff1f-175">**Manually entered credentials** - Enter hello storage account name, and either hello primary or second key.</span></span> <span data-ttu-id="bff1f-176">Välj ett alternativ för **anslutning** (HTTPS rekommenderas för de flesta fall.) Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="bff1f-176">Select an option for **Connection** (HTTPS is recommended for most scenarios.) Select **OK**.</span></span>

1. <span data-ttu-id="bff1f-177">toodelete en anslutningssträng Välj hello anslutningssträngen och välj sedan **ta bort inställningen**.</span><span class="sxs-lookup"><span data-stu-id="bff1f-177">toodelete a connection string, select hello connection string, and then select **Remove Setting**.</span></span>

1. <span data-ttu-id="bff1f-178">Från hello Visual Studio i verktygsfältet väljer **spara**.</span><span class="sxs-lookup"><span data-stu-id="bff1f-178">From hello Visual Studio, toolbar, select **Save**.</span></span>

## <a name="programmatically-access-a-connection-string"></a><span data-ttu-id="bff1f-179">Komma åt en anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="bff1f-179">Programmatically access a connection string</span></span>

<span data-ttu-id="bff1f-180">hello följande steg visar hur tooprogrammatically åt en anslutningssträng med C#.</span><span class="sxs-lookup"><span data-stu-id="bff1f-180">hello following steps show how tooprogrammatically access a connection string using C#.</span></span>

1. <span data-ttu-id="bff1f-181">Lägg till följande hello med direktiven tooa C#-filen där du ska toouse hello inställningen:</span><span class="sxs-lookup"><span data-stu-id="bff1f-181">Add hello following using directives tooa C# file where you are going toouse hello setting:</span></span>

    ```csharp
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. <span data-ttu-id="bff1f-182">hello följande kod visar ett exempel på hur tooaccess en anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="bff1f-182">hello following code illustrates an example of how tooaccess a connection string.</span></span> <span data-ttu-id="bff1f-183">Ersätt hello &lt;ConnectionStringName > med hello lämpligt värde.</span><span class="sxs-lookup"><span data-stu-id="bff1f-183">Replace hello &lt;ConnectionStringName> placeholder with hello appropriate value.</span></span> 

    ```csharp
    // Setup hello connection tooAzure Storage
    var storageAccount = CloudStorageAccount.Parse(RoleEnvironment.GetConfigurationSettingValue("<ConnectionStringName>"));
    ```

## <a name="add-custom-settings-toouse-in-your-azure-cloud-service"></a><span data-ttu-id="bff1f-184">Lägga till anpassade inställningar toouse i Azure-molntjänst</span><span class="sxs-lookup"><span data-stu-id="bff1f-184">Add custom settings toouse in your Azure cloud service</span></span>
<span data-ttu-id="bff1f-185">Anpassade inställningar i konfigurationsfilen för hello service kan du lägga till ett namn och värde för en sträng för en specifik tjänst-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="bff1f-185">Custom settings in hello service configuration file let you add a name and value for a string for a specific service configuration.</span></span> <span data-ttu-id="bff1f-186">Du kan välja toouse den här inställningen tooconfigure som en funktion i din molntjänst genom att läsa värdet hello hello inställningen och använda det här värdet toocontrol hello logik i koden.</span><span class="sxs-lookup"><span data-stu-id="bff1f-186">You might choose toouse this setting tooconfigure a feature in your cloud service by reading hello value of hello setting and using this value toocontrol hello logic in your code.</span></span> <span data-ttu-id="bff1f-187">Du kan ändra konfigurationsvärdena tjänsten utan att behöva toorebuild ditt abonnemang eller när Molntjänsten körs.</span><span class="sxs-lookup"><span data-stu-id="bff1f-187">You can change these service configuration values without having toorebuild your service package or when your cloud service is running.</span></span> <span data-ttu-id="bff1f-188">Koden kan söka efter meddelanden om ändringar i en inställning.</span><span class="sxs-lookup"><span data-stu-id="bff1f-188">Your code can check for notifications of when a setting changes.</span></span> <span data-ttu-id="bff1f-189">Se [RoleEnvironment.Changing händelse](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx).</span><span class="sxs-lookup"><span data-stu-id="bff1f-189">See [RoleEnvironment.Changing Event](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx).</span></span>

<span data-ttu-id="bff1f-190">Du kan lägga till, ta bort eller ändra anpassade inställningar för din tjänstkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="bff1f-190">You can add, remove, or modify custom settings for your service configurations.</span></span> <span data-ttu-id="bff1f-191">Du kanske vill olika värden för de här strängarna för olika konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="bff1f-191">You might want different values for these strings for different service configurations.</span></span>

<span data-ttu-id="bff1f-192">Genom att använda ett annat värde för varje service-konfiguration kan du inte har toouse olika strängar i Molntjänsten eller ändra koden när du publicerar ditt moln tjänsten tooAzure.</span><span class="sxs-lookup"><span data-stu-id="bff1f-192">By using a different value for each service configuration, you do not have toouse different strings in your cloud service or modify your code when you publish your cloud service tooAzure.</span></span> <span data-ttu-id="bff1f-193">Du kan använda hello samma namn för hello sträng i koden och hello-värdet är olika, baserat på hello tjänstkonfiguration som du väljer när du skapar din molntjänst eller när du publicerar den.</span><span class="sxs-lookup"><span data-stu-id="bff1f-193">You can use hello same name for hello string in your code and hello value is different, based on hello service configuration that you select when you build your cloud service or when you publish it.</span></span>

1. <span data-ttu-id="bff1f-194">Skapa eller öppna ett Azure cloud service-projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bff1f-194">Create or open an Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="bff1f-195">I **Solution Explorer**, expandera hello projektnoden.</span><span class="sxs-lookup"><span data-stu-id="bff1f-195">In **Solution Explorer**, expand hello project node.</span></span> <span data-ttu-id="bff1f-196">Under hello **roller** nod, hello genom att högerklicka på rollen du vill tooupdate och, hello snabbmenyn väljer **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="bff1f-196">Under hello **Roles** node, right-click hello role you want tooupdate, and, from hello context menu, select **Properties**.</span></span>

    ![Snabbmenyn för Solution Explorer Azure roll](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. <span data-ttu-id="bff1f-198">Välj hello **inställningar** fliken.</span><span class="sxs-lookup"><span data-stu-id="bff1f-198">Select hello **Settings** tab.</span></span>

    ![Fliken Inställningar](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab.png)

1. <span data-ttu-id="bff1f-200">I hello **tjänstkonfiguration** listan, Välj hello tjänstkonfiguration som du vill tooupdate.</span><span class="sxs-lookup"><span data-stu-id="bff1f-200">In hello **Service Configuration** list, select hello service configuration that you want tooupdate.</span></span>

    ![Tjänstkonfigurationen lista](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-select-configuration.png)

1. <span data-ttu-id="bff1f-202">Välj tooadd en anpassad inställning **Lägg till inställning**.</span><span class="sxs-lookup"><span data-stu-id="bff1f-202">tooadd a custom setting, select **Add Setting**.</span></span>

    ![Lägg till anpassad inställning](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting.png)

1. <span data-ttu-id="bff1f-204">När en ny inställning för hello har lagts toohello lista, uppdatera hello rad i hello lista hello nödvändig information.</span><span class="sxs-lookup"><span data-stu-id="bff1f-204">Once hello new setting has been added toohello list, update hello row in hello list with hello necessary information.</span></span>

    ![Ny anpassad inställning](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting-new-setting.png)

    - <span data-ttu-id="bff1f-206">**Namnet** -ange hello namn hello-inställning.</span><span class="sxs-lookup"><span data-stu-id="bff1f-206">**Name** - Enter hello name of hello setting.</span></span>
    - <span data-ttu-id="bff1f-207">**Typen** – Välj **sträng** hello nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="bff1f-207">**Type** - Select **String** from hello drop-down list.</span></span>
    - <span data-ttu-id="bff1f-208">**Värdet** -ange hello hello inställningens värde.</span><span class="sxs-lookup"><span data-stu-id="bff1f-208">**Value** - Enter hello value of hello setting.</span></span> <span data-ttu-id="bff1f-209">Du kan antingen ange hello värde direkt i hello **värdet** cell eller välj hello ellips (...) tooenter hello värde i hello **Redigera sträng** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bff1f-209">You can either enter hello value directly into hello **Value** cell, or select hello ellipsis (...) tooenter hello value in hello **Edit String** dialog.</span></span>  

1. <span data-ttu-id="bff1f-210">toodelete en anpassad inställning Välj hello inställning och välj sedan **ta bort inställningen**.</span><span class="sxs-lookup"><span data-stu-id="bff1f-210">toodelete a custom setting, select hello setting, and then select **Remove Setting**.</span></span>

1. <span data-ttu-id="bff1f-211">Från hello Visual Studio i verktygsfältet väljer **spara**.</span><span class="sxs-lookup"><span data-stu-id="bff1f-211">From hello Visual Studio, toolbar, select **Save**.</span></span>

## <a name="programmatically-access-a-custom-settings-value"></a><span data-ttu-id="bff1f-212">Komma åt en inställningens värde</span><span class="sxs-lookup"><span data-stu-id="bff1f-212">Programmatically access a custom setting's value</span></span>
 
<span data-ttu-id="bff1f-213">hello följande steg visar hur tooprogrammatically åt en anpassad inställning med C#.</span><span class="sxs-lookup"><span data-stu-id="bff1f-213">hello following steps show how tooprogrammatically access a custom setting using C#.</span></span>

1. <span data-ttu-id="bff1f-214">Lägg till följande hello med direktiven tooa C#-filen där du ska toouse hello inställningen:</span><span class="sxs-lookup"><span data-stu-id="bff1f-214">Add hello following using directives tooa C# file where you are going toouse hello setting:</span></span>

    ```csharp
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. <span data-ttu-id="bff1f-215">hello följande kod visar ett exempel på hur tooaccess anpassade inställningen.</span><span class="sxs-lookup"><span data-stu-id="bff1f-215">hello following code illustrates an example of how tooaccess a custom setting.</span></span> <span data-ttu-id="bff1f-216">Ersätt hello &lt;SettingName > med hello lämpligt värde.</span><span class="sxs-lookup"><span data-stu-id="bff1f-216">Replace hello &lt;SettingName> placeholder with hello appropriate value.</span></span> 
    
    ```csharp
    var settingValue = RoleEnvironment.GetConfigurationSettingValue("<SettingName>");
    ```

## <a name="manage-local-storage-for-each-role-instance"></a><span data-ttu-id="bff1f-217">Hantera lokal lagring för varje rollinstans</span><span class="sxs-lookup"><span data-stu-id="bff1f-217">Manage local storage for each role instance</span></span>
<span data-ttu-id="bff1f-218">Du kan lägga till lokala system fillagring för varje instans av en roll.</span><span class="sxs-lookup"><span data-stu-id="bff1f-218">You can add local file system storage for each instance of a role.</span></span> <span data-ttu-id="bff1f-219">hello data lagras i denna lagring inte är tillgänglig i andra instanser av hello roll för vilka hello data lagras eller av andra roller.</span><span class="sxs-lookup"><span data-stu-id="bff1f-219">hello data stored in that storage is not accessible by other instances of hello role for which hello data is stored, or by other roles.</span></span>  

1. <span data-ttu-id="bff1f-220">Skapa eller öppna ett Azure cloud service-projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bff1f-220">Create or open an Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="bff1f-221">I **Solution Explorer**, expandera hello projektnoden.</span><span class="sxs-lookup"><span data-stu-id="bff1f-221">In **Solution Explorer**, expand hello project node.</span></span> <span data-ttu-id="bff1f-222">Under hello **roller** nod, hello genom att högerklicka på rollen du vill tooupdate och, hello snabbmenyn väljer **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="bff1f-222">Under hello **Roles** node, right-click hello role you want tooupdate, and, from hello context menu, select **Properties**.</span></span>

    ![Snabbmenyn för Solution Explorer Azure roll](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. <span data-ttu-id="bff1f-224">Välj hello **lokal lagring** fliken.</span><span class="sxs-lookup"><span data-stu-id="bff1f-224">Select hello **Local Storage** tab.</span></span>

    ![Fliken lokal lagring](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab.png)

1. <span data-ttu-id="bff1f-226">I hello **tjänstkonfiguration** lista, se till att **alla konfigurationer av** är markerad som hello lokal Lagringsinställningar gäller tooall tjänstkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="bff1f-226">In hello **Service Configuration** list, ensure that **All Configurations** is selected as hello local storage settings apply tooall service configurations.</span></span> <span data-ttu-id="bff1f-227">Ett annat värde resulterar i alla hello inmatningsfält i hello sidan håller på att inaktiveras.</span><span class="sxs-lookup"><span data-stu-id="bff1f-227">Any other value results in all hello input fields on hello page being disabled.</span></span> 

    ![Tjänstkonfigurationen lista](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab-service-configuration.png)

1. <span data-ttu-id="bff1f-229">tooadd en lokal lagring-post, Välj **Lägg till lokal lagring**.</span><span class="sxs-lookup"><span data-stu-id="bff1f-229">tooadd a local storage entry, select **Add Local Storage**.</span></span>

    ![Lägg till lokal lagring](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab-add-local-storage.png)

1. <span data-ttu-id="bff1f-231">När hello ny lokal lagring post har lagts till toohello lista, uppdatera hello rad i hello lista hello nödvändig information.</span><span class="sxs-lookup"><span data-stu-id="bff1f-231">Once hello new local storage entry has been added toohello list, update hello row in hello list with hello necessary information.</span></span>

    ![Ny post för lokal lagring](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab-new-local-storage.png)

    - <span data-ttu-id="bff1f-233">**Namnet** -ange hello namn som du vill toouse för hello ny lokal lagring.</span><span class="sxs-lookup"><span data-stu-id="bff1f-233">**Name** - Enter hello name that you want toouse for hello new local storage.</span></span>
    - <span data-ttu-id="bff1f-234">**Storlek (MB)** -ange hello storlek i MB som du behöver för hello ny lokal lagring.</span><span class="sxs-lookup"><span data-stu-id="bff1f-234">**Size (MB)** - Enter hello size in MB that you need for hello new local storage.</span></span>
    - <span data-ttu-id="bff1f-235">**Rensa på rollen återvinning** -Markera det här alternativet tooremove hello data i hello ny lokal lagring när hello virtuell dator för hello rollen återanvänds.</span><span class="sxs-lookup"><span data-stu-id="bff1f-235">**Clean on role recycle** - Select this option tooremove hello data in hello new local storage when hello virtual machine for hello role is recycled.</span></span>

1. <span data-ttu-id="bff1f-236">toodelete lokal lagring-posten väljer hello-post och välj sedan **ta bort lokal lagring**.</span><span class="sxs-lookup"><span data-stu-id="bff1f-236">toodelete a local storage entry, select hello entry, and then select **Remove Local Storage**.</span></span>

1. <span data-ttu-id="bff1f-237">Från hello Visual Studio i verktygsfältet väljer **spara**.</span><span class="sxs-lookup"><span data-stu-id="bff1f-237">From hello Visual Studio, toolbar, select **Save**.</span></span>

## <a name="programmatically-accessing-local-storage"></a><span data-ttu-id="bff1f-238">Att komma åt lokal lagring</span><span class="sxs-lookup"><span data-stu-id="bff1f-238">Programmatically accessing local storage</span></span>

<span data-ttu-id="bff1f-239">Detta avsnitt visar hur tooprogrammatically åt lokal lagring med hjälp av C# genom att skriva en test-textfil `MyLocalStorageTest.txt`.</span><span class="sxs-lookup"><span data-stu-id="bff1f-239">This section illustrates how tooprogrammatically access local storage using C# by writing a test text file `MyLocalStorageTest.txt`.</span></span>  

### <a name="write-a-text-file-toolocal-storage"></a><span data-ttu-id="bff1f-240">Skriv en toolocal fillagring text</span><span class="sxs-lookup"><span data-stu-id="bff1f-240">Write a text file toolocal storage</span></span>

<span data-ttu-id="bff1f-241">hello följande kod visar ett exempel på hur toowrite text fillagring toolocal.</span><span class="sxs-lookup"><span data-stu-id="bff1f-241">hello following code shows an example of how toowrite a text file toolocal storage.</span></span> <span data-ttu-id="bff1f-242">Ersätt hello &lt;LocalStorageName > med hello lämpligt värde.</span><span class="sxs-lookup"><span data-stu-id="bff1f-242">Replace hello &lt;LocalStorageName> placeholder with hello appropriate value.</span></span> 

    ```csharp
    // Retrieve an object that points toohello local storage resource
    LocalResource localResource = RoleEnvironment.GetLocalResource("<LocalStorageName>");
    
    //Define hello file name and path
    string[] paths = { localResource.RootPath, "MyLocalStorageTest.txt" };
    String filePath = Path.Combine(paths);
    
    using (FileStream writeStream = File.Create(filePath))
    {
        Byte[] textToWrite = new UTF8Encoding(true).GetBytes("Testing Web role storage");
        writeStream.Write(textToWrite, 0, textToWrite.Length);
    }

    ```

### <a name="find-a-file-written-toolocal-storage"></a><span data-ttu-id="bff1f-243">Hitta en fil skrivs toolocal lagring</span><span class="sxs-lookup"><span data-stu-id="bff1f-243">Find a file written toolocal storage</span></span>

<span data-ttu-id="bff1f-244">tooview hello-filen som skapas av hello koden i hello föregående avsnitt, Följ dessa steg:</span><span class="sxs-lookup"><span data-stu-id="bff1f-244">tooview hello file created by hello code in hello previous section, follow these steps:</span></span>
    
1.  <span data-ttu-id="bff1f-245">I meddelandefältet i Windows hello, högerklicka hello Azure ikon och, hello snabbmenyn, Välj **visa Compute Emulator UI**.</span><span class="sxs-lookup"><span data-stu-id="bff1f-245">In hello Windows notification area, right-click hello Azure icon, and, from hello context menu, select **Show Compute Emulator UI**.</span></span> 

    ![Visa Azure-beräkningsemulatorn](./media/vs-azure-tools-configure-roles-for-cloud-service/show-compute-emulator.png)

1. <span data-ttu-id="bff1f-247">Välj hello webbroll.</span><span class="sxs-lookup"><span data-stu-id="bff1f-247">Select hello web role.</span></span>

    ![Azure-beräkningsemulatorn](./media/vs-azure-tools-configure-roles-for-cloud-service/compute-emulator.png)

1. <span data-ttu-id="bff1f-249">På hello **Microsoft Azure Compute Emulator** väljer du **verktyg** > **öppna lokalt Arkiv**.</span><span class="sxs-lookup"><span data-stu-id="bff1f-249">On hello **Microsoft Azure Compute Emulator** menu, select **Tools** > **Open local store**.</span></span>

    ![Öppna lokalt Arkiv menyobjekt](./media/vs-azure-tools-configure-roles-for-cloud-service/compute-emulator-open-local-store-menu.png)

1. <span data-ttu-id="bff1f-251">Ange när hello Windows Explorer öppnas ' MyLocalStorageTest.txt'' i hello **Sök** text och markera **RETUR** toostart hello sökning.</span><span class="sxs-lookup"><span data-stu-id="bff1f-251">When hello Windows Explorer window opens, enter `MyLocalStorageTest.txt`\` into hello **Search** text box, and select **Enter** toostart hello search.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="bff1f-252">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bff1f-252">Next steps</span></span>
<span data-ttu-id="bff1f-253">Lär dig mer om Azure-projekt i Visual Studio genom att läsa [konfigurera ett Azure-projekt](vs-azure-tools-configuring-an-azure-project.md).</span><span class="sxs-lookup"><span data-stu-id="bff1f-253">Learn more about Azure projects in Visual Studio by reading [Configuring an Azure Project](vs-azure-tools-configuring-an-azure-project.md).</span></span> <span data-ttu-id="bff1f-254">Mer information om hello cloud service schemat genom att läsa [Schemareferens](https://msdn.microsoft.com/library/azure/dd179398).</span><span class="sxs-lookup"><span data-stu-id="bff1f-254">Learn more about hello cloud service schema by reading [Schema Reference](https://msdn.microsoft.com/library/azure/dd179398).</span></span>

