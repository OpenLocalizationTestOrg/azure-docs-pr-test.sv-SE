---
title: "Konfigurera roller för en Azure-molntjänst med Visual Studio | Microsoft Docs"
description: "Lär dig hur du skapar och konfigurerar roller för Azure-molntjänster med Visual Studio."
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
ms.openlocfilehash: 17da71ac0c5ab9330b9244c0354e4d161d98229e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="configure-azure-cloud-service-roles-with-visual-studio"></a><span data-ttu-id="29df5-103">Konfigurera roller för Azure cloud service med Visual Studio</span><span class="sxs-lookup"><span data-stu-id="29df5-103">Configure Azure cloud service roles with Visual Studio</span></span>
<span data-ttu-id="29df5-104">En Azure-molntjänst kan ha en eller flera worker webbroller.</span><span class="sxs-lookup"><span data-stu-id="29df5-104">An Azure cloud service can have one or more worker or web roles.</span></span> <span data-ttu-id="29df5-105">För varje roll måste du definiera hur rollen har ställts in och även konfigurera hur rollen körs.</span><span class="sxs-lookup"><span data-stu-id="29df5-105">For each role, you need to define how that role is set up and also configure how that role runs.</span></span> <span data-ttu-id="29df5-106">Mer information om roller i molntjänster finns videon [introduktion till Azure Cloud Services](https://channel9.msdn.com/Series/Windows-Azure-Cloud-Services-Tutorials/Introduction-to-Windows-Azure-Cloud-Services).</span><span class="sxs-lookup"><span data-stu-id="29df5-106">To learn more about roles in cloud services, see the video [Introduction to Azure Cloud Services](https://channel9.msdn.com/Series/Windows-Azure-Cloud-Services-Tutorials/Introduction-to-Windows-Azure-Cloud-Services).</span></span> 

<span data-ttu-id="29df5-107">Information för din molntjänst lagras i följande filer:</span><span class="sxs-lookup"><span data-stu-id="29df5-107">The information for your cloud service is stored in the following files:</span></span>

- <span data-ttu-id="29df5-108">**ServiceDefinition.csdef** -tjänstdefinitionsfilen definierar körningsinställningar för Molntjänsten, inklusive vilka roller är obligatoriska, slutpunkter och storlek på virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="29df5-108">**ServiceDefinition.csdef** - The service definition file defines the runtime settings for your cloud service including what roles are required, endpoints, and virtual machine size.</span></span> <span data-ttu-id="29df5-109">Ingen av de data som lagras i `ServiceDefinition.csdef` kan ändras när den körs.</span><span class="sxs-lookup"><span data-stu-id="29df5-109">None of the data stored in `ServiceDefinition.csdef` can be changed when your role is running.</span></span>
- <span data-ttu-id="29df5-110">**ServiceConfiguration.cscfg** - tjänstkonfigurationsfilen konfigurerar hur många instanser av en roll körs och värden för inställningarna som har definierats för en roll.</span><span class="sxs-lookup"><span data-stu-id="29df5-110">**ServiceConfiguration.cscfg** - The service configuration file configures how many instances of a role are run and the values of the settings defined for a role.</span></span> <span data-ttu-id="29df5-111">Data som lagras i `ServiceConfiguration.cscfg` kan ändras när din roll körs.</span><span class="sxs-lookup"><span data-stu-id="29df5-111">The data stored in `ServiceConfiguration.cscfg` can be changed while your role is running.</span></span>

<span data-ttu-id="29df5-112">Du kan definiera flera konfigurationer för att lagra olika värden för de inställningar som styr hur en roll körs.</span><span class="sxs-lookup"><span data-stu-id="29df5-112">To store different values for the settings that control how a role runs, you can define multiple service configurations.</span></span> <span data-ttu-id="29df5-113">Du kan använda en annan tjänstkonfiguration för varje distributionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="29df5-113">You can use a different service configuration for each deployment environment.</span></span> <span data-ttu-id="29df5-114">Du kan till exempel ange anslutningssträngen för lagring kontot du använder lokala Azure storage-emulatorn i en lokal tjänst-konfiguration och skapa en annan tjänstkonfiguration om du vill använda Azure storage i molnet.</span><span class="sxs-lookup"><span data-stu-id="29df5-114">For example, you can set your storage account connection string to use the local Azure storage emulator in a local service configuration and create another service configuration to use Azure storage in the cloud.</span></span>

<span data-ttu-id="29df5-115">När du skapar en Azure-molntjänst i Visual Studio, skapas och läggs till din Azure-projekt automatiskt två konfigurationer:</span><span class="sxs-lookup"><span data-stu-id="29df5-115">When you create an Azure cloud service in Visual Studio, two service configurations are automatically created and added to your Azure project:</span></span>

- `ServiceConfiguration.Cloud.cscfg`
- `ServiceConfiguration.Local.cscfg`

## <a name="configure-an-azure-cloud-service"></a><span data-ttu-id="29df5-116">Konfigurera en Azure-molntjänst</span><span class="sxs-lookup"><span data-stu-id="29df5-116">Configure an Azure cloud service</span></span>
<span data-ttu-id="29df5-117">Du kan konfigurera en Azure-molntjänst från Solution Explorer i Visual Studio enligt följande steg:</span><span class="sxs-lookup"><span data-stu-id="29df5-117">You can configure an Azure cloud service from Solution Explorer in Visual Studio, as shown in the following steps:</span></span>

1. <span data-ttu-id="29df5-118">Skapa eller öppna ett Azure cloud service-projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="29df5-118">Create or open an Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="29df5-119">I **Solution Explorer**, högerklicka på projektet och, från snabbmenyn, Välj **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="29df5-119">In **Solution Explorer**, right-click the project, and, from the context menu, select **Properties**.</span></span>
   
    ![Snabbmenyn för Solution Explorer-projekt](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-project-context-menu.png)

1. <span data-ttu-id="29df5-121">Välj i projektets egenskapssida i **Development** fliken.</span><span class="sxs-lookup"><span data-stu-id="29df5-121">In the project's properties page, select the **Development** tab.</span></span> 

    ![Projektet egenskapssidan - utveckling fliken](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-development-tab.png)

1. <span data-ttu-id="29df5-123">I den **tjänstkonfiguration** väljer du namnet på tjänstkonfigurationen som du vill redigera.</span><span class="sxs-lookup"><span data-stu-id="29df5-123">In the **Service Configuration** list, select the name of the service configuration that you want to edit.</span></span> <span data-ttu-id="29df5-124">(Om du vill göra ändringar i alla tjänstkonfiguration för den här rollen, välja **alla konfigurationer av**.)</span><span class="sxs-lookup"><span data-stu-id="29df5-124">(If you want to make changes to all the service configurations for this role, select **All Configurations**.)</span></span>
   
    > [!IMPORTANT]
    > <span data-ttu-id="29df5-125">Om du väljer en specifik tjänstkonfiguration har vissa egenskaper inaktiverats eftersom de kan bara anges för alla konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="29df5-125">If you choose a specific service configuration, some properties are disabled because they can only be set for all configurations.</span></span> <span data-ttu-id="29df5-126">Om du vill redigera dessa egenskaper, måste du välja **alla konfigurationer av**.</span><span class="sxs-lookup"><span data-stu-id="29df5-126">To edit these properties, you must select **All Configurations**.</span></span>
    > 
    > 
   
    ![Tjänstkonfigurationen listan för en Azure-molntjänst](./media/vs-azure-tools-configure-roles-for-cloud-service/cloud-service-service-configuration-property.png)

## <a name="change-the-number-of-role-instances"></a><span data-ttu-id="29df5-128">Ändra antalet rollinstanser</span><span class="sxs-lookup"><span data-stu-id="29df5-128">Change the number of role instances</span></span>
<span data-ttu-id="29df5-129">Du kan ändra antalet instanser av en roll som körs, baserat på antalet användare eller belastning som förväntas för en viss roll för att förbättra prestandan för din tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="29df5-129">To improve the performance of your cloud service, you can change the number of instances of a role that are running, based on the number of users or the load expected for a particular role.</span></span> <span data-ttu-id="29df5-130">En separat virtuell dator skapas för varje instans av rollen när Molntjänsten som körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="29df5-130">A separate virtual machine is created for each instance of a role when the cloud service runs in Azure.</span></span> <span data-ttu-id="29df5-131">Detta påverkar fakturering för distribution av den här Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="29df5-131">This affects the billing for the deployment of this cloud service.</span></span> <span data-ttu-id="29df5-132">Mer information om fakturering finns [förstå fakturan för Microsoft Azure](billing/billing-understand-your-bill.md).</span><span class="sxs-lookup"><span data-stu-id="29df5-132">For more information about billing, see [Understand your bill for Microsoft Azure](billing/billing-understand-your-bill.md).</span></span>

1. <span data-ttu-id="29df5-133">Skapa eller öppna ett Azure cloud service-projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="29df5-133">Create or open an Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="29df5-134">I **Solution Explorer**, expandera projektnoden.</span><span class="sxs-lookup"><span data-stu-id="29df5-134">In **Solution Explorer**, expand the project node.</span></span> <span data-ttu-id="29df5-135">Under den **roller** nod, högerklicka på rollen du vill uppdatera och på snabbmenyn Välj **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="29df5-135">Under the **Roles** node, right-click the role you want to update, and, from the context menu, select **Properties**.</span></span>

    ![Snabbmenyn för Solution Explorer Azure roll](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. <span data-ttu-id="29df5-137">Välj den **Configuration** fliken.</span><span class="sxs-lookup"><span data-stu-id="29df5-137">Select the **Configuration** tab.</span></span>

    ![Konfigurationsfliken](./media/vs-azure-tools-configure-roles-for-cloud-service/role-configuration-properties-page.png)

1. <span data-ttu-id="29df5-139">I den **tjänstkonfiguration** väljer du konfigurationen för tjänsten som du vill uppdatera.</span><span class="sxs-lookup"><span data-stu-id="29df5-139">In the **Service Configuration** list, select the service configuration that you want to update.</span></span>
   
    ![Tjänstkonfigurationen lista](./media/vs-azure-tools-configure-roles-for-cloud-service/role-configuration-properties-page-select-configuration.png)

1. <span data-ttu-id="29df5-141">I den **instansen antal** text anger antalet instanser som du vill starta för den här rollen.</span><span class="sxs-lookup"><span data-stu-id="29df5-141">In the **Instance count** text box, enter the number of instances that you want to start for this role.</span></span> <span data-ttu-id="29df5-142">Varje instans som körs på en separat virtuell dator när du publicerar Molntjänsten på Azure.</span><span class="sxs-lookup"><span data-stu-id="29df5-142">Each instance runs on a separate virtual machine when you publish the cloud service to Azure.</span></span>

    ![Uppdatera Instansantalet](./media/vs-azure-tools-configure-roles-for-cloud-service/role-configuration-properties-page-instance-count.png)

1. <span data-ttu-id="29df5-144">Från Visual Studio i verktygsfältet väljer **spara**.</span><span class="sxs-lookup"><span data-stu-id="29df5-144">From the Visual Studio, toolbar, select **Save**.</span></span>

## <a name="manage-connection-strings-for-storage-accounts"></a><span data-ttu-id="29df5-145">Hantera anslutningssträngar för storage-konton</span><span class="sxs-lookup"><span data-stu-id="29df5-145">Manage connection strings for storage accounts</span></span>
<span data-ttu-id="29df5-146">Du kan lägga till, ta bort eller ändra anslutningssträngar för service-konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="29df5-146">You can add, remove, or modify connection strings for your service configurations.</span></span> <span data-ttu-id="29df5-147">Du kan exempelvis lokala anslutningssträngen för en lokal tjänst-konfiguration som har värdet `UseDevelopmentStorage=true`.</span><span class="sxs-lookup"><span data-stu-id="29df5-147">For example, you might want a local connection string for a local service configuration that has a value of `UseDevelopmentStorage=true`.</span></span> <span data-ttu-id="29df5-148">Du kan också konfigurera en tjänst molnkonfiguration som använder ett lagringskonto i Azure.</span><span class="sxs-lookup"><span data-stu-id="29df5-148">You might also want to configure a cloud service configuration that uses a storage account in Azure.</span></span>

> [!WARNING]
> <span data-ttu-id="29df5-149">När du registrerar Azure storage-konto viktig information för en anslutningssträng för storage-konto, lagras informationen lokalt i tjänstekonfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="29df5-149">When you enter the Azure storage account key information for a storage account connection string, this information is stored locally in the service configuration file.</span></span> <span data-ttu-id="29df5-150">Men den här informationen lagras för närvarande inte som krypterad text.</span><span class="sxs-lookup"><span data-stu-id="29df5-150">However, this information is currently not stored as encrypted text.</span></span>
> 
> 

<span data-ttu-id="29df5-151">Med hjälp av ett annat värde för varje tjänstkonfiguration behöver du inte använda olika anslutningssträngar i din tjänst i molnet eller ändra koden när du publicerar din tjänst i molnet till Azure.</span><span class="sxs-lookup"><span data-stu-id="29df5-151">By using a different value for each service configuration, you do not have to use different connection strings in your cloud service or modify your code when you publish your cloud service to Azure.</span></span> <span data-ttu-id="29df5-152">Du kan använda samma namn för anslutningssträngen i koden och värdet är olika, baserat på konfigurationen av tjänsten som du väljer när du skapar din molntjänst eller när du publicerar den.</span><span class="sxs-lookup"><span data-stu-id="29df5-152">You can use the same name for the connection string in your code and the value is different, based on the service configuration that you select when you build your cloud service or when you publish it.</span></span>

1. <span data-ttu-id="29df5-153">Skapa eller öppna ett Azure cloud service-projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="29df5-153">Create or open an Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="29df5-154">I **Solution Explorer**, expandera projektnoden.</span><span class="sxs-lookup"><span data-stu-id="29df5-154">In **Solution Explorer**, expand the project node.</span></span> <span data-ttu-id="29df5-155">Under den **roller** nod, högerklicka på rollen du vill uppdatera och på snabbmenyn Välj **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="29df5-155">Under the **Roles** node, right-click the role you want to update, and, from the context menu, select **Properties**.</span></span>

    ![Snabbmenyn för Solution Explorer Azure roll](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. <span data-ttu-id="29df5-157">Välj den **inställningar** fliken.</span><span class="sxs-lookup"><span data-stu-id="29df5-157">Select the **Settings** tab.</span></span>

    ![Fliken Inställningar](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab.png)

1. <span data-ttu-id="29df5-159">I den **tjänstkonfiguration** väljer du konfigurationen för tjänsten som du vill uppdatera.</span><span class="sxs-lookup"><span data-stu-id="29df5-159">In the **Service Configuration** list, select the service configuration that you want to update.</span></span>

    ![Tjänstkonfiguration](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-select-configuration.png)

1. <span data-ttu-id="29df5-161">Om du vill lägga till en anslutningssträng, Välj **Lägg till inställning**.</span><span class="sxs-lookup"><span data-stu-id="29df5-161">To add a connection string, select **Add Setting**.</span></span>

    ![Lägg till anslutningssträng](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting.png)

1. <span data-ttu-id="29df5-163">När den nya inställningen har lagts till i listan, uppdatera en rad i listan med nödvändig information.</span><span class="sxs-lookup"><span data-stu-id="29df5-163">Once the new setting has been added to the list, update the row in the list with the necessary information.</span></span>

    ![Ny anslutningssträng](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting-new-setting.png)

    - <span data-ttu-id="29df5-165">**Namnet** – ange namnet som du vill använda för anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="29df5-165">**Name** - Enter the name that you want to use for the connection string.</span></span>
    - <span data-ttu-id="29df5-166">**Typen** – Välj **anslutningssträngen** från den nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="29df5-166">**Type** - Select **Connection String** from the drop-down list.</span></span>
    - <span data-ttu-id="29df5-167">**Värdet** -kan du antingen ange anslutningssträngen direkt i den **värdet** celler eller välj ellips (...) att arbeta i den **skapa Lagringsanslutningssträng** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="29df5-167">**Value** - You can either enter the connection string directly into the **Value** cell, or select the ellipsis (...) to work in the **Create Storage Connection String** dialog.</span></span>  

1. <span data-ttu-id="29df5-168">I den **skapa Lagringsanslutningssträng** väljer ett alternativ för **ansluta med**.</span><span class="sxs-lookup"><span data-stu-id="29df5-168">In the **Create Storage Connection String** dialog, select an option for **Connect using**.</span></span> <span data-ttu-id="29df5-169">Följ sedan anvisningarna för det alternativ du väljer:</span><span class="sxs-lookup"><span data-stu-id="29df5-169">Then follow the instructions for the option you select:</span></span>

    - <span data-ttu-id="29df5-170">**Microsoft Azure storage-emulatorn** -om du väljer det här alternativet om de återstående inställningarna i dialogrutan har inaktiverats eftersom de tillämpas endast för Azure.</span><span class="sxs-lookup"><span data-stu-id="29df5-170">**Microsoft Azure storage emulator** - If you select this option, the remaining settings on the dialog are disabled as they apply only to Azure.</span></span> <span data-ttu-id="29df5-171">Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="29df5-171">Select **OK**.</span></span>
    - <span data-ttu-id="29df5-172">**Din prenumeration** - om du väljer det här alternativet använder den nedrullningsbara listan för att antingen välja och logga in på ett Microsoft-konto eller Lägg till ett Microsoft-konto.</span><span class="sxs-lookup"><span data-stu-id="29df5-172">**Your subscription** - If you select this option, use the dropdown list to either select and sign into a Microsoft account, or add a Microsoft account.</span></span> <span data-ttu-id="29df5-173">Välj ett Azure-prenumeration och storage-konto.</span><span class="sxs-lookup"><span data-stu-id="29df5-173">Select an Azure subscription and storage account.</span></span> <span data-ttu-id="29df5-174">Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="29df5-174">Select **OK**.</span></span>
    - <span data-ttu-id="29df5-175">**Autentiseringsuppgifterna anges manuellt** -ange lagringskontonamn och den primära eller den andra nyckeln.</span><span class="sxs-lookup"><span data-stu-id="29df5-175">**Manually entered credentials** - Enter the storage account name, and either the primary or second key.</span></span> <span data-ttu-id="29df5-176">Välj ett alternativ för **anslutning** (HTTPS rekommenderas för de flesta fall.) Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="29df5-176">Select an option for **Connection** (HTTPS is recommended for most scenarios.) Select **OK**.</span></span>

1. <span data-ttu-id="29df5-177">Välj anslutningssträngen för att ta bort en anslutningssträng och välj sedan **ta bort inställningen**.</span><span class="sxs-lookup"><span data-stu-id="29df5-177">To delete a connection string, select the connection string, and then select **Remove Setting**.</span></span>

1. <span data-ttu-id="29df5-178">Från Visual Studio i verktygsfältet väljer **spara**.</span><span class="sxs-lookup"><span data-stu-id="29df5-178">From the Visual Studio, toolbar, select **Save**.</span></span>

## <a name="programmatically-access-a-connection-string"></a><span data-ttu-id="29df5-179">Komma åt en anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="29df5-179">Programmatically access a connection string</span></span>

<span data-ttu-id="29df5-180">Följande steg visar hur programmatisk åtkomst till en anslutningssträng med C#.</span><span class="sxs-lookup"><span data-stu-id="29df5-180">The following steps show how to programmatically access a connection string using C#.</span></span>

1. <span data-ttu-id="29df5-181">Lägg till följande med hjälp av direktiven till en C#-fil där du ska använda inställningen:</span><span class="sxs-lookup"><span data-stu-id="29df5-181">Add the following using directives to a C# file where you are going to use the setting:</span></span>

    ```csharp
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. <span data-ttu-id="29df5-182">Följande kod visar ett exempel på hur du kommer åt en anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="29df5-182">The following code illustrates an example of how to access a connection string.</span></span> <span data-ttu-id="29df5-183">Ersätt den &lt;ConnectionStringName > med lämpligt värde.</span><span class="sxs-lookup"><span data-stu-id="29df5-183">Replace the &lt;ConnectionStringName> placeholder with the appropriate value.</span></span> 

    ```csharp
    // Setup the connection to Azure Storage
    var storageAccount = CloudStorageAccount.Parse(RoleEnvironment.GetConfigurationSettingValue("<ConnectionStringName>"));
    ```

## <a name="add-custom-settings-to-use-in-your-azure-cloud-service"></a><span data-ttu-id="29df5-184">Lägg till anpassade inställningar som ska användas i Azure-molntjänst</span><span class="sxs-lookup"><span data-stu-id="29df5-184">Add custom settings to use in your Azure cloud service</span></span>
<span data-ttu-id="29df5-185">Anpassade inställningar i tjänstkonfigurationsfilen kan du lägga till ett namn och värde för en sträng för en specifik tjänst-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="29df5-185">Custom settings in the service configuration file let you add a name and value for a string for a specific service configuration.</span></span> <span data-ttu-id="29df5-186">Du kan välja att använda den här inställningen för att konfigurera en funktion i din molntjänst genom att läsa värdet för inställningen och använder det här värdet för att styra logiken i din kod.</span><span class="sxs-lookup"><span data-stu-id="29df5-186">You might choose to use this setting to configure a feature in your cloud service by reading the value of the setting and using this value to control the logic in your code.</span></span> <span data-ttu-id="29df5-187">Du kan ändra dessa värden för konfiguration av tjänsten utan att behöva återskapa ditt abonnemang eller när Molntjänsten körs.</span><span class="sxs-lookup"><span data-stu-id="29df5-187">You can change these service configuration values without having to rebuild your service package or when your cloud service is running.</span></span> <span data-ttu-id="29df5-188">Koden kan söka efter meddelanden om ändringar i en inställning.</span><span class="sxs-lookup"><span data-stu-id="29df5-188">Your code can check for notifications of when a setting changes.</span></span> <span data-ttu-id="29df5-189">Se [RoleEnvironment.Changing händelse](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx).</span><span class="sxs-lookup"><span data-stu-id="29df5-189">See [RoleEnvironment.Changing Event](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx).</span></span>

<span data-ttu-id="29df5-190">Du kan lägga till, ta bort eller ändra anpassade inställningar för din tjänstkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="29df5-190">You can add, remove, or modify custom settings for your service configurations.</span></span> <span data-ttu-id="29df5-191">Du kanske vill olika värden för de här strängarna för olika konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="29df5-191">You might want different values for these strings for different service configurations.</span></span>

<span data-ttu-id="29df5-192">Med hjälp av ett annat värde för varje tjänstkonfiguration behöver du inte använda olika strängar i din tjänst i molnet eller ändra koden när du publicerar din tjänst i molnet till Azure.</span><span class="sxs-lookup"><span data-stu-id="29df5-192">By using a different value for each service configuration, you do not have to use different strings in your cloud service or modify your code when you publish your cloud service to Azure.</span></span> <span data-ttu-id="29df5-193">Du kan använda samma namn för strängen i koden och värdet är olika, baserat på konfigurationen av tjänsten som du väljer när du skapar din molntjänst eller när du publicerar den.</span><span class="sxs-lookup"><span data-stu-id="29df5-193">You can use the same name for the string in your code and the value is different, based on the service configuration that you select when you build your cloud service or when you publish it.</span></span>

1. <span data-ttu-id="29df5-194">Skapa eller öppna ett Azure cloud service-projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="29df5-194">Create or open an Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="29df5-195">I **Solution Explorer**, expandera projektnoden.</span><span class="sxs-lookup"><span data-stu-id="29df5-195">In **Solution Explorer**, expand the project node.</span></span> <span data-ttu-id="29df5-196">Under den **roller** nod, högerklicka på rollen du vill uppdatera och på snabbmenyn Välj **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="29df5-196">Under the **Roles** node, right-click the role you want to update, and, from the context menu, select **Properties**.</span></span>

    ![Snabbmenyn för Solution Explorer Azure roll](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. <span data-ttu-id="29df5-198">Välj den **inställningar** fliken.</span><span class="sxs-lookup"><span data-stu-id="29df5-198">Select the **Settings** tab.</span></span>

    ![Fliken Inställningar](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab.png)

1. <span data-ttu-id="29df5-200">I den **tjänstkonfiguration** väljer du konfigurationen för tjänsten som du vill uppdatera.</span><span class="sxs-lookup"><span data-stu-id="29df5-200">In the **Service Configuration** list, select the service configuration that you want to update.</span></span>

    ![Tjänstkonfigurationen lista](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-select-configuration.png)

1. <span data-ttu-id="29df5-202">Om du vill lägga till en anpassad inställning, Välj **Lägg till inställning**.</span><span class="sxs-lookup"><span data-stu-id="29df5-202">To add a custom setting, select **Add Setting**.</span></span>

    ![Lägg till anpassad inställning](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting.png)

1. <span data-ttu-id="29df5-204">När den nya inställningen har lagts till i listan, uppdatera en rad i listan med nödvändig information.</span><span class="sxs-lookup"><span data-stu-id="29df5-204">Once the new setting has been added to the list, update the row in the list with the necessary information.</span></span>

    ![Ny anpassad inställning](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting-new-setting.png)

    - <span data-ttu-id="29df5-206">**Namnet** – ange namnet på inställningen.</span><span class="sxs-lookup"><span data-stu-id="29df5-206">**Name** - Enter the name of the setting.</span></span>
    - <span data-ttu-id="29df5-207">**Typen** – Välj **sträng** från den nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="29df5-207">**Type** - Select **String** from the drop-down list.</span></span>
    - <span data-ttu-id="29df5-208">**Värdet** -ange värdet för inställningen.</span><span class="sxs-lookup"><span data-stu-id="29df5-208">**Value** - Enter the value of the setting.</span></span> <span data-ttu-id="29df5-209">Du kan antingen ange värde direkt i den **värdet** cell eller markera ellips (...) för att ange värdet i den **Redigera sträng** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="29df5-209">You can either enter the value directly into the **Value** cell, or select the ellipsis (...) to enter the value in the **Edit String** dialog.</span></span>  

1. <span data-ttu-id="29df5-210">Välj inställning för att ta bort en anpassad inställning, och välj sedan **ta bort inställningen**.</span><span class="sxs-lookup"><span data-stu-id="29df5-210">To delete a custom setting, select the setting, and then select **Remove Setting**.</span></span>

1. <span data-ttu-id="29df5-211">Från Visual Studio i verktygsfältet väljer **spara**.</span><span class="sxs-lookup"><span data-stu-id="29df5-211">From the Visual Studio, toolbar, select **Save**.</span></span>

## <a name="programmatically-access-a-custom-settings-value"></a><span data-ttu-id="29df5-212">Komma åt en inställningens värde</span><span class="sxs-lookup"><span data-stu-id="29df5-212">Programmatically access a custom setting's value</span></span>
 
<span data-ttu-id="29df5-213">Följande steg visar hur programmatisk åtkomst till en anpassad inställning med C#.</span><span class="sxs-lookup"><span data-stu-id="29df5-213">The following steps show how to programmatically access a custom setting using C#.</span></span>

1. <span data-ttu-id="29df5-214">Lägg till följande med hjälp av direktiven till en C#-fil där du ska använda inställningen:</span><span class="sxs-lookup"><span data-stu-id="29df5-214">Add the following using directives to a C# file where you are going to use the setting:</span></span>

    ```csharp
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. <span data-ttu-id="29df5-215">Följande kod visar ett exempel på hur du kommer åt en anpassad inställning.</span><span class="sxs-lookup"><span data-stu-id="29df5-215">The following code illustrates an example of how to access a custom setting.</span></span> <span data-ttu-id="29df5-216">Ersätt den &lt;SettingName > med lämpligt värde.</span><span class="sxs-lookup"><span data-stu-id="29df5-216">Replace the &lt;SettingName> placeholder with the appropriate value.</span></span> 
    
    ```csharp
    var settingValue = RoleEnvironment.GetConfigurationSettingValue("<SettingName>");
    ```

## <a name="manage-local-storage-for-each-role-instance"></a><span data-ttu-id="29df5-217">Hantera lokal lagring för varje rollinstans</span><span class="sxs-lookup"><span data-stu-id="29df5-217">Manage local storage for each role instance</span></span>
<span data-ttu-id="29df5-218">Du kan lägga till lokala system fillagring för varje instans av en roll.</span><span class="sxs-lookup"><span data-stu-id="29df5-218">You can add local file system storage for each instance of a role.</span></span> <span data-ttu-id="29df5-219">De data som lagras i denna lagring inte är tillgänglig i andra instanser av rollen för vilket data lagras eller av andra roller.</span><span class="sxs-lookup"><span data-stu-id="29df5-219">The data stored in that storage is not accessible by other instances of the role for which the data is stored, or by other roles.</span></span>  

1. <span data-ttu-id="29df5-220">Skapa eller öppna ett Azure cloud service-projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="29df5-220">Create or open an Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="29df5-221">I **Solution Explorer**, expandera projektnoden.</span><span class="sxs-lookup"><span data-stu-id="29df5-221">In **Solution Explorer**, expand the project node.</span></span> <span data-ttu-id="29df5-222">Under den **roller** nod, högerklicka på rollen du vill uppdatera och på snabbmenyn Välj **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="29df5-222">Under the **Roles** node, right-click the role you want to update, and, from the context menu, select **Properties**.</span></span>

    ![Snabbmenyn för Solution Explorer Azure roll](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. <span data-ttu-id="29df5-224">Välj den **lokal lagring** fliken.</span><span class="sxs-lookup"><span data-stu-id="29df5-224">Select the **Local Storage** tab.</span></span>

    ![Fliken lokal lagring](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab.png)

1. <span data-ttu-id="29df5-226">I den **tjänstkonfiguration** lista, se till att **alla konfigurationer av** är markerad som lokal Lagringsinställningar gäller för alla konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="29df5-226">In the **Service Configuration** list, ensure that **All Configurations** is selected as the local storage settings apply to all service configurations.</span></span> <span data-ttu-id="29df5-227">Ett annat värde resulterar i indatafält på sidan håller på att inaktiveras.</span><span class="sxs-lookup"><span data-stu-id="29df5-227">Any other value results in all the input fields on the page being disabled.</span></span> 

    ![Tjänstkonfigurationen lista](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab-service-configuration.png)

1. <span data-ttu-id="29df5-229">Om du vill lägga till en post för lokal lagring, Välj **Lägg till lokal lagring**.</span><span class="sxs-lookup"><span data-stu-id="29df5-229">To add a local storage entry, select **Add Local Storage**.</span></span>

    ![Lägg till lokal lagring](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab-add-local-storage.png)

1. <span data-ttu-id="29df5-231">När den nya posten för lokal lagring har lagts till i listan, uppdatera en rad i listan med nödvändig information.</span><span class="sxs-lookup"><span data-stu-id="29df5-231">Once the new local storage entry has been added to the list, update the row in the list with the necessary information.</span></span>

    ![Ny post för lokal lagring](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab-new-local-storage.png)

    - <span data-ttu-id="29df5-233">**Namnet** – ange namnet som du vill använda för den nya lokal lagringen.</span><span class="sxs-lookup"><span data-stu-id="29df5-233">**Name** - Enter the name that you want to use for the new local storage.</span></span>
    - <span data-ttu-id="29df5-234">**Storlek (MB)** -ange en storlek i MB som du behöver för den nya lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="29df5-234">**Size (MB)** - Enter the size in MB that you need for the new local storage.</span></span>
    - <span data-ttu-id="29df5-235">**Rensa på rollen återvinning** -Välj detta alternativ för att ta bort data i den nya lokal lagringen när den virtuella datorn för rollen återanvänds.</span><span class="sxs-lookup"><span data-stu-id="29df5-235">**Clean on role recycle** - Select this option to remove the data in the new local storage when the virtual machine for the role is recycled.</span></span>

1. <span data-ttu-id="29df5-236">Om du vill ta bort en post för lokal lagring, Välj posten och välj sedan **ta bort lokal lagring**.</span><span class="sxs-lookup"><span data-stu-id="29df5-236">To delete a local storage entry, select the entry, and then select **Remove Local Storage**.</span></span>

1. <span data-ttu-id="29df5-237">Från Visual Studio i verktygsfältet väljer **spara**.</span><span class="sxs-lookup"><span data-stu-id="29df5-237">From the Visual Studio, toolbar, select **Save**.</span></span>

## <a name="programmatically-accessing-local-storage"></a><span data-ttu-id="29df5-238">Att komma åt lokal lagring</span><span class="sxs-lookup"><span data-stu-id="29df5-238">Programmatically accessing local storage</span></span>

<span data-ttu-id="29df5-239">Det här avsnittet visar hur programmatisk åtkomst till lokal lagring med hjälp av C# genom att skriva en test-textfil `MyLocalStorageTest.txt`.</span><span class="sxs-lookup"><span data-stu-id="29df5-239">This section illustrates how to programmatically access local storage using C# by writing a test text file `MyLocalStorageTest.txt`.</span></span>  

### <a name="write-a-text-file-to-local-storage"></a><span data-ttu-id="29df5-240">Skriv en textfil till lokal lagring</span><span class="sxs-lookup"><span data-stu-id="29df5-240">Write a text file to local storage</span></span>

<span data-ttu-id="29df5-241">Följande kod visar ett exempel på hur du skriver en textfil till lokal lagring.</span><span class="sxs-lookup"><span data-stu-id="29df5-241">The following code shows an example of how to write a text file to local storage.</span></span> <span data-ttu-id="29df5-242">Ersätt den &lt;LocalStorageName > med lämpligt värde.</span><span class="sxs-lookup"><span data-stu-id="29df5-242">Replace the &lt;LocalStorageName> placeholder with the appropriate value.</span></span> 

    ```csharp
    // Retrieve an object that points to the local storage resource
    LocalResource localResource = RoleEnvironment.GetLocalResource("<LocalStorageName>");
    
    //Define the file name and path
    string[] paths = { localResource.RootPath, "MyLocalStorageTest.txt" };
    String filePath = Path.Combine(paths);
    
    using (FileStream writeStream = File.Create(filePath))
    {
        Byte[] textToWrite = new UTF8Encoding(true).GetBytes("Testing Web role storage");
        writeStream.Write(textToWrite, 0, textToWrite.Length);
    }

    ```

### <a name="find-a-file-written-to-local-storage"></a><span data-ttu-id="29df5-243">Hitta en fil som skrivs till lokal lagring</span><span class="sxs-lookup"><span data-stu-id="29df5-243">Find a file written to local storage</span></span>

<span data-ttu-id="29df5-244">Följ dessa steg om du vill visa filen som skapas av koden i föregående avsnitt:</span><span class="sxs-lookup"><span data-stu-id="29df5-244">To view the file created by the code in the previous section, follow these steps:</span></span>
    
1.  <span data-ttu-id="29df5-245">I meddelandefältet i Windows högerklickar du på ikonen för Azure och på snabbmenyn Välj **visa Compute Emulator UI**.</span><span class="sxs-lookup"><span data-stu-id="29df5-245">In the Windows notification area, right-click the Azure icon, and, from the context menu, select **Show Compute Emulator UI**.</span></span> 

    ![Visa Azure-beräkningsemulatorn](./media/vs-azure-tools-configure-roles-for-cloud-service/show-compute-emulator.png)

1. <span data-ttu-id="29df5-247">Välj rollen webbserver.</span><span class="sxs-lookup"><span data-stu-id="29df5-247">Select the web role.</span></span>

    ![Azure-beräkningsemulatorn](./media/vs-azure-tools-configure-roles-for-cloud-service/compute-emulator.png)

1. <span data-ttu-id="29df5-249">På den **Microsoft Azure Compute Emulator** väljer du **verktyg** > **öppna lokalt Arkiv**.</span><span class="sxs-lookup"><span data-stu-id="29df5-249">On the **Microsoft Azure Compute Emulator** menu, select **Tools** > **Open local store**.</span></span>

    ![Öppna lokalt Arkiv menyobjekt](./media/vs-azure-tools-configure-roles-for-cloud-service/compute-emulator-open-local-store-menu.png)

1. <span data-ttu-id="29df5-251">När fönstret Utforskaren öppnas, ange ' MyLocalStorageTest.txt'' i den **Sök** text och markera **RETUR** sökningen ska startas.</span><span class="sxs-lookup"><span data-stu-id="29df5-251">When the Windows Explorer window opens, enter `MyLocalStorageTest.txt`\` into the **Search** text box, and select **Enter** to start the search.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="29df5-252">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="29df5-252">Next steps</span></span>
<span data-ttu-id="29df5-253">Lär dig mer om Azure-projekt i Visual Studio genom att läsa [konfigurera ett Azure-projekt](vs-azure-tools-configuring-an-azure-project.md).</span><span class="sxs-lookup"><span data-stu-id="29df5-253">Learn more about Azure projects in Visual Studio by reading [Configuring an Azure Project](vs-azure-tools-configuring-an-azure-project.md).</span></span> <span data-ttu-id="29df5-254">Mer information om cloud service schemat genom att läsa [Schemareferens](https://msdn.microsoft.com/library/azure/dd179398).</span><span class="sxs-lookup"><span data-stu-id="29df5-254">Learn more about the cloud service schema by reading [Schema Reference](https://msdn.microsoft.com/library/azure/dd179398).</span></span>

