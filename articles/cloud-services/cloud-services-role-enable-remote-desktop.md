---
title: "Aktivera Fjärrskrivbord i en Azure-molntjänst | Microsoft Docs"
description: "Hur du konfigurerar ditt tjänstprogram för azure-molnet för att tillåta anslutningar till fjärrskrivbord"
services: cloud-services
documentationcenter: 
author: thraka
manager: timlt
editor: 
ms.assetid: d3110ee8-6526-4585-aba5-d0bc9a713e9b
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: 413e72e9a39fcde84f56bfc61a6bc72dbadf1c97
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services"></a><span data-ttu-id="ca8d6-103">Aktivera anslutning till fjärrskrivbord för en roll i Azure-molntjänster</span><span class="sxs-lookup"><span data-stu-id="ca8d6-103">Enable Remote Desktop Connection for a Role in Azure Cloud Services</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="ca8d6-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ca8d6-104">Azure portal</span></span>](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [<span data-ttu-id="ca8d6-105">Klassisk Azure-portal</span><span class="sxs-lookup"><span data-stu-id="ca8d6-105">Azure classic portal</span></span>](cloud-services-role-enable-remote-desktop.md)
> * [<span data-ttu-id="ca8d6-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ca8d6-106">PowerShell</span></span>](cloud-services-role-enable-remote-desktop-powershell.md)
> * [<span data-ttu-id="ca8d6-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ca8d6-107">Visual Studio</span></span>](../vs-azure-tools-remote-desktop-roles.md)

<span data-ttu-id="ca8d6-108">Du kan aktivera anslutning till fjärrskrivbord i din roll under utvecklingen genom att inkludera Remote Desktop-moduler i tjänstedefinitionsfilen eller kan du aktivera fjärrskrivbord via Remote Desktop-tillägget.</span><span class="sxs-lookup"><span data-stu-id="ca8d6-108">You can enable a Remote Desktop connection in your role during development by including the Remote Desktop modules in your service definition or you can choose to enable Remote Desktop through the Remote Desktop Extension.</span></span> <span data-ttu-id="ca8d6-109">Det bästa sättet är att använda Remote Desktop-tillägget som du kan aktivera fjärrskrivbord även när programmet distribueras utan att behöva distribuera programmet.</span><span class="sxs-lookup"><span data-stu-id="ca8d6-109">The preferred approach is to use the Remote Desktop extension as you can enable Remote Desktop even after the application is deployed without having to redeploy your application.</span></span>

## <a name="configure-remote-desktop-from-the-azure-classic-portal"></a><span data-ttu-id="ca8d6-110">Konfigurera anslutning till fjärrskrivbord från den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="ca8d6-110">Configure Remote Desktop from the Azure classic portal</span></span>
<span data-ttu-id="ca8d6-111">Den klassiska Azure-portalen använder metoden Remote Desktop-tillägget så att du kan aktivera fjärrskrivbord även när programmet distribueras.</span><span class="sxs-lookup"><span data-stu-id="ca8d6-111">The Azure classic portal uses the Remote Desktop Extension approach so you can enable Remote Desktop even after the application is deployed.</span></span> <span data-ttu-id="ca8d6-112">Den **konfigurera** på sidan för din molntjänst kan du ändra det lokala administratörskontot som används för att ansluta till virtuella datorer om du vill aktivera Fjärrskrivbord, certifikatet används i autentisering och ange förfallodatum.</span><span class="sxs-lookup"><span data-stu-id="ca8d6-112">The **Configure** page for your cloud service allows you to enable Remote Desktop, change the local Administrator account used to connect to the virtual machines, the certificate used in authentication and set the expiration date.</span></span>

1. <span data-ttu-id="ca8d6-113">Klicka på **molntjänster**, klicka på namnet på Molntjänsten och klicka sedan på **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="ca8d6-113">Click **Cloud Services**, click the name of the cloud service, and then click **Configure**.</span></span>
2. <span data-ttu-id="ca8d6-114">Klicka på den **Remote** längst ned.</span><span class="sxs-lookup"><span data-stu-id="ca8d6-114">Click the **Remote** button at the bottom.</span></span>

    ![Fjärråtkomst-molntjänster](./media/cloud-services-role-enable-remote-desktop/CloudServices_Remote.png)

   > [!WARNING]
   > <span data-ttu-id="ca8d6-116">Alla rollinstanser kommer att startas om när du först aktivera Fjärrskrivbord och klicka på OK (markering).</span><span class="sxs-lookup"><span data-stu-id="ca8d6-116">All role instances will be restarted when you first enable Remote Desktop and click OK (checkmark).</span></span> <span data-ttu-id="ca8d6-117">Certifikatet som används för att kryptera lösenordet måste installeras på vilken roll för att förhindra att en omstart.</span><span class="sxs-lookup"><span data-stu-id="ca8d6-117">To prevent a reboot, the certificate used to encrypt the password must be installed on the role.</span></span> <span data-ttu-id="ca8d6-118">För att förhindra en omstart [överföra ett certifikat för Molntjänsten](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) och återgå sedan till den här dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ca8d6-118">To prevent a restart, [upload a certificate for the cloud service](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) and then return to this dialog.</span></span>

3. <span data-ttu-id="ca8d6-119">I **roller**, väljer du den roll som du vill uppdatera eller välj **alla** för alla roller.</span><span class="sxs-lookup"><span data-stu-id="ca8d6-119">In **Roles**, select the role you want to update or select **All** for all roles.</span></span>
4. <span data-ttu-id="ca8d6-120">Gör något av följande ändringar:</span><span class="sxs-lookup"><span data-stu-id="ca8d6-120">Make any of the following changes:</span></span>

   * <span data-ttu-id="ca8d6-121">Välj för att aktivera Fjärrskrivbord på **aktivera Fjärrskrivbord** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="ca8d6-121">To enable Remote Desktop, select the **Enable Remote Desktop** check box.</span></span> <span data-ttu-id="ca8d6-122">Avmarkera kryssrutan om du vill inaktivera Fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="ca8d6-122">To disable Remote Desktop, clear the check box.</span></span>
   * <span data-ttu-id="ca8d6-123">Skapa ett konto som ska användas i anslutning till fjärrskrivbord till rollinstanserna.</span><span class="sxs-lookup"><span data-stu-id="ca8d6-123">Create an account to use in Remote Desktop connections to the role instances.</span></span>
   * <span data-ttu-id="ca8d6-124">Uppdatera lösenordet för det befintliga kontot.</span><span class="sxs-lookup"><span data-stu-id="ca8d6-124">Update the password for the existing account.</span></span>
   * <span data-ttu-id="ca8d6-125">Välj en överförda certifikat ska användas för autentisering (Överför certifikat med hjälp av **överför** på den **certifikat** sidan) eller skapa ett nytt certifikat.</span><span class="sxs-lookup"><span data-stu-id="ca8d6-125">Select an uploaded certificate to use for authentication (upload the certificate using **Upload** on the **Certificates** page) or create a new certificate.</span></span>
   * <span data-ttu-id="ca8d6-126">Ändra utgångsdatumet för Remote Desktop-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="ca8d6-126">Change the expiration date for the Remote Desktop configuration.</span></span>

5. <span data-ttu-id="ca8d6-127">När du är klar configuration-uppdateringar, klickar du på **OK** (markering).</span><span class="sxs-lookup"><span data-stu-id="ca8d6-127">When you finish your configuration updates, click **OK** (checkmark).</span></span>

## <a name="remote-into-role-instances"></a><span data-ttu-id="ca8d6-128">Remote i rollinstanser</span><span class="sxs-lookup"><span data-stu-id="ca8d6-128">Remote into role instances</span></span>
<span data-ttu-id="ca8d6-129">När fjärrskrivbord är aktiverat på rollerna kan du fjärråtkomst till en rollinstans via olika verktyg.</span><span class="sxs-lookup"><span data-stu-id="ca8d6-129">Once Remote Desktop is enabled on the roles you can remote into a role instance through various tools.</span></span>

<span data-ttu-id="ca8d6-130">Att ansluta till en rollinstans från den klassiska Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="ca8d6-130">To connect to a role instance from the Azure classic portal:</span></span>

1. <span data-ttu-id="ca8d6-131">Klicka på **instanser** att öppna den **instanser** sidan.</span><span class="sxs-lookup"><span data-stu-id="ca8d6-131">Click **Instances** to open the **Instances** page.</span></span>
2. <span data-ttu-id="ca8d6-132">Välj en rollinstans som har konfigurerats för fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="ca8d6-132">Select a role instance that has Remote Desktop configured.</span></span>
3. <span data-ttu-id="ca8d6-133">Klicka på **Anslut**, och följ instruktionerna för att öppna skrivbordet.</span><span class="sxs-lookup"><span data-stu-id="ca8d6-133">Click **Connect**, and follow the instructions to open the desktop.</span></span>
4. <span data-ttu-id="ca8d6-134">Klicka på **öppna** och sedan **Anslut** fjärrskrivbord-anslutningen ska upprättas.</span><span class="sxs-lookup"><span data-stu-id="ca8d6-134">Click **Open** and then **Connect** to start the Remote Desktop connection.</span></span>

### <a name="use-visual-studio-to-remote-into-a-role-instance"></a><span data-ttu-id="ca8d6-135">Använd Visual Studio för att till en rollinstans</span><span class="sxs-lookup"><span data-stu-id="ca8d6-135">Use Visual Studio to remote into a role instance</span></span>
<span data-ttu-id="ca8d6-136">I Visual Studio Server Explorer:</span><span class="sxs-lookup"><span data-stu-id="ca8d6-136">In Visual Studio, Server Explorer:</span></span>

1. <span data-ttu-id="ca8d6-137">Expandera den **Azure** > **molntjänster** > **[molntjänstnamnet]** nod.</span><span class="sxs-lookup"><span data-stu-id="ca8d6-137">Expand the **Azure** > **Cloud Services** > **[cloud service name]** node.</span></span>
2. <span data-ttu-id="ca8d6-138">Expandera antingen **mellanlagring** eller **produktion**.</span><span class="sxs-lookup"><span data-stu-id="ca8d6-138">Expand either **Staging** or **Production**.</span></span>
3. <span data-ttu-id="ca8d6-139">Expandera enskilda rollen.</span><span class="sxs-lookup"><span data-stu-id="ca8d6-139">Expand the individual role.</span></span>
4. <span data-ttu-id="ca8d6-140">Högerklicka på en av rollinstanserna klickar du på **ansluta med hjälp av fjärrskrivbord...** , och ange användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="ca8d6-140">Right-click one of the role instances, click **Connect using Remote Desktop...**, and then enter the user name and password.</span></span>

![Fjärrskrivbord i Server explorer](./media/cloud-services-role-enable-remote-desktop/ServerExplorer_RemoteDesktop.png)

### <a name="use-powershell-to-get-the-rdp-file"></a><span data-ttu-id="ca8d6-142">Använd PowerShell för att hämta RDP-filen</span><span class="sxs-lookup"><span data-stu-id="ca8d6-142">Use PowerShell to get the RDP file</span></span>
<span data-ttu-id="ca8d6-143">Du kan använda den [Get-AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) för att hämta RDP-filen.</span><span class="sxs-lookup"><span data-stu-id="ca8d6-143">You can use the [Get-AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) cmdlet to retrieve the RDP file.</span></span> <span data-ttu-id="ca8d6-144">Du kan sedan använda RDP-filen med anslutning till fjärrskrivbord för att få åtkomst till Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="ca8d6-144">You can then use the RDP file with Remote Desktop Connection to access the cloud service.</span></span>

### <a name="programmatically-download-the-rdp-file-through-the-service-management-rest-api"></a><span data-ttu-id="ca8d6-145">Hämta programmässigt RDP-filen via Service Management REST API</span><span class="sxs-lookup"><span data-stu-id="ca8d6-145">Programmatically download the RDP file through the Service Management REST API</span></span>
<span data-ttu-id="ca8d6-146">Du kan använda den [ladda ned RDP-filen](https://msdn.microsoft.com/library/jj157183.aspx) REST-åtgärden för att ladda ned RDP-filen.</span><span class="sxs-lookup"><span data-stu-id="ca8d6-146">You can use the [Download RDP File](https://msdn.microsoft.com/library/jj157183.aspx) REST operation to download the RDP file.</span></span>

## <a name="to-configure-remote-desktop-in-the-service-definition-file"></a><span data-ttu-id="ca8d6-147">Konfigurera fjärrskrivbord i tjänstdefinitionsfilen</span><span class="sxs-lookup"><span data-stu-id="ca8d6-147">To configure Remote Desktop in the service definition file</span></span>
<span data-ttu-id="ca8d6-148">Den här metoden kan du aktivera Fjärrskrivbord för program under utveckling.</span><span class="sxs-lookup"><span data-stu-id="ca8d6-148">This method allows you to enable Remote Desktop for the application during development.</span></span> <span data-ttu-id="ca8d6-149">Den här metoden kräver krypterade lösenord lagras i tjänstkonfigurationen av fil- och eventuella uppdateringar till konfigurationen av fjärrskrivbord kräver en omdistribution av programmet.</span><span class="sxs-lookup"><span data-stu-id="ca8d6-149">This approach requires encrypted passwords be stored in your service configuration file and any updates to the remote desktop configuration would require a redeployment of the application.</span></span> <span data-ttu-id="ca8d6-150">Om du vill undvika dessa downsides bör du använda remote desktop tillägget baserat-metod som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="ca8d6-150">If you want to avoid these downsides you should use the remote desktop extension based approach described above.</span></span>  

<span data-ttu-id="ca8d6-151">Du kan använda Visual Studio för att [aktivera en fjärrskrivbordsanslutning](../vs-azure-tools-remote-desktop-roles.md) med service definition file-metoden.</span><span class="sxs-lookup"><span data-stu-id="ca8d6-151">You can use Visual Studio to [enable a remote desktop connection](../vs-azure-tools-remote-desktop-roles.md) using the service definition file approach.</span></span>  
<span data-ttu-id="ca8d6-152">Stegen nedan beskriver de ändringar som krävs för service model-filer till aktivera Fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="ca8d6-152">The steps below describe the changes needed to the service model files to enable remote desktop.</span></span> <span data-ttu-id="ca8d6-153">Visual Studio gör automatiskt dessa ändringar när du publicerar.</span><span class="sxs-lookup"><span data-stu-id="ca8d6-153">Visual Studio will automatically makes these changes when publishing.</span></span>

### <a name="set-up-the-connection-in-the-service-model"></a><span data-ttu-id="ca8d6-154">Upprätta anslutningen i tjänstemodellen</span><span class="sxs-lookup"><span data-stu-id="ca8d6-154">Set up the connection in the service model</span></span>
<span data-ttu-id="ca8d6-155">Använd den **import** element att importera den **RemoteAccess** modulen och **RemoteForwarder** modulen till den [ServiceDefinition.csdef](cloud-services-model-and-package.md#csdef) filen.</span><span class="sxs-lookup"><span data-stu-id="ca8d6-155">Use the **Imports** element to import the **RemoteAccess** module and the **RemoteForwarder** module to the [ServiceDefinition.csdef](cloud-services-model-and-package.md#csdef) file.</span></span>

<span data-ttu-id="ca8d6-156">Tjänstdefinitionsfilen bör likna exemplet nedan med den `<Imports>` element som har lagts till.</span><span class="sxs-lookup"><span data-stu-id="ca8d6-156">The service definition file should be similar to the following example with the `<Imports>` element added.</span></span>

```xml
<ServiceDefinition name="<name-of-cloud-service>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2013-03.2.0">
    <WebRole name="WebRole1" vmsize="Small">
        <Sites>
            <Site name="Web">
                <Bindings>
                    <Binding name="Endpoint1" endpointName="Endpoint1" />
                </Bindings>
            </Site>
        </Sites>
        <Endpoints>
            <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
            <Import moduleName="Diagnostics" />
            <Import moduleName="RemoteAccess" />
            <Import moduleName="RemoteForwarder" />
        </Imports>
    </WebRole>
</ServiceDefinition>
```
<span data-ttu-id="ca8d6-157">Den [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#cscfg) fil bör likna följande exempel, notera den `<ConfigurationSettings>` och `<Certificates>` element.</span><span class="sxs-lookup"><span data-stu-id="ca8d6-157">The [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#cscfg) file should be similar to the following example, note the `<ConfigurationSettings>` and `<Certificates>` elements.</span></span> <span data-ttu-id="ca8d6-158">Det angivna certifikatet måste vara [har överförts till Molntjänsten](cloud-services-how-to-create-deploy.md#how-to-upload-a-certificate-for-a-cloud-service).</span><span class="sxs-lookup"><span data-stu-id="ca8d6-158">The Certificate specified must be [uploaded to the cloud service](cloud-services-how-to-create-deploy.md#how-to-upload-a-certificate-for-a-cloud-service).</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceConfiguration serviceName="<name-of-cloud-service>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="3" osVersion="*" schemaVersion="2013-03.2.0">
    <Role name="WebRole1">
        <Instances count="2" />
        <ConfigurationSettings>
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.Enabled" value="true" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountUsername" value="[name-of-user-account]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountEncryptedPassword" value="[base-64-encrypted-user-password]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountExpiration" value="[certificate-expiration]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteForwarder.Enabled" value="true" />
        </ConfigurationSettings>
        <Certificates>
            <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="[certificate-thumbprint]" thumbprintAlgorithm="sha1" />
        </Certificates>
    </Role>
</ServiceConfiguration>
```


## <a name="additional-resources"></a><span data-ttu-id="ca8d6-159">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="ca8d6-159">Additional Resources</span></span>
<span data-ttu-id="ca8d6-160">[Hur du konfigurerar molntjänster](cloud-services-how-to-configure.md)
[Cloud services vanliga frågor och svar - fjärrskrivbord](cloud-services-faq.md)</span><span class="sxs-lookup"><span data-stu-id="ca8d6-160">[How to Configure Cloud Services](cloud-services-how-to-configure.md)
[Cloud services FAQ - Remote Desktop](cloud-services-faq.md)</span></span>
