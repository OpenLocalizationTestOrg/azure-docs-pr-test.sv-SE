---
title: "aaaEnable fjärrskrivbord i en Azure-molntjänst | Microsoft Docs"
description: "Hur tooconfigure din azure cloud service programmet tooallow anslutningar till fjärrskrivbord"
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
ms.openlocfilehash: b3c0180bf5ad29cb77e5303ccbd6f9ccc44b7b0a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services"></a><span data-ttu-id="7b15b-103">Aktivera anslutning till fjärrskrivbord för en roll i Azure-molntjänster</span><span class="sxs-lookup"><span data-stu-id="7b15b-103">Enable Remote Desktop Connection for a Role in Azure Cloud Services</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="7b15b-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="7b15b-104">Azure portal</span></span>](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [<span data-ttu-id="7b15b-105">Klassisk Azure-portal</span><span class="sxs-lookup"><span data-stu-id="7b15b-105">Azure classic portal</span></span>](cloud-services-role-enable-remote-desktop.md)
> * [<span data-ttu-id="7b15b-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="7b15b-106">PowerShell</span></span>](cloud-services-role-enable-remote-desktop-powershell.md)
> * [<span data-ttu-id="7b15b-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7b15b-107">Visual Studio</span></span>](../vs-azure-tools-remote-desktop-roles.md)

<span data-ttu-id="7b15b-108">Du kan aktivera anslutning till fjärrskrivbord i din roll under utvecklingen av inklusive hello fjärrskrivbord moduler i tjänstedefinitionsfilen eller välja tooenable fjärrskrivbord via hello Remote Desktop-tillägget.</span><span class="sxs-lookup"><span data-stu-id="7b15b-108">You can enable a Remote Desktop connection in your role during development by including hello Remote Desktop modules in your service definition or you can choose tooenable Remote Desktop through hello Remote Desktop Extension.</span></span> <span data-ttu-id="7b15b-109">hello är föredragen metod toouse hello Remote Desktop-tillägg som du kan aktivera fjärrskrivbord även efter hello programmet distribueras utan att behöva tooredeploy ditt program.</span><span class="sxs-lookup"><span data-stu-id="7b15b-109">hello preferred approach is toouse hello Remote Desktop extension as you can enable Remote Desktop even after hello application is deployed without having tooredeploy your application.</span></span>

## <a name="configure-remote-desktop-from-hello-azure-classic-portal"></a><span data-ttu-id="7b15b-110">Konfigurera anslutning till fjärrskrivbord från hello klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="7b15b-110">Configure Remote Desktop from hello Azure classic portal</span></span>
<span data-ttu-id="7b15b-111">hello klassiska Azure-portalen använder hello Remote Desktop-tillägget metoden så att du kan aktivera fjärrskrivbord även efter hello programmet distribueras.</span><span class="sxs-lookup"><span data-stu-id="7b15b-111">hello Azure classic portal uses hello Remote Desktop Extension approach so you can enable Remote Desktop even after hello application is deployed.</span></span> <span data-ttu-id="7b15b-112">Hej **konfigurera** på sidan för din molntjänst kan tooenable fjärrskrivbord, ändra hello lokala administratörskontot används tooconnect toohello virtuella datorer, hello certifikat används för autentisering och ange hello utgångsdatum.</span><span class="sxs-lookup"><span data-stu-id="7b15b-112">hello **Configure** page for your cloud service allows you tooenable Remote Desktop, change hello local Administrator account used tooconnect toohello virtual machines, hello certificate used in authentication and set hello expiration date.</span></span>

1. <span data-ttu-id="7b15b-113">Klicka på **molntjänster**hello namnet på hello-Molntjänsten och klicka sedan på **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="7b15b-113">Click **Cloud Services**, click hello name of hello cloud service, and then click **Configure**.</span></span>
2. <span data-ttu-id="7b15b-114">Klicka på hello **Remote** knappen längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="7b15b-114">Click hello **Remote** button at hello bottom.</span></span>

    ![Fjärråtkomst-molntjänster](./media/cloud-services-role-enable-remote-desktop/CloudServices_Remote.png)

   > [!WARNING]
   > <span data-ttu-id="7b15b-116">Alla rollinstanser kommer att startas om när du först aktivera Fjärrskrivbord och klicka på OK (markering).</span><span class="sxs-lookup"><span data-stu-id="7b15b-116">All role instances will be restarted when you first enable Remote Desktop and click OK (checkmark).</span></span> <span data-ttu-id="7b15b-117">tooprevent en omstart, hello certifikatlösenord används tooencrypt hello måste installeras på hello roll.</span><span class="sxs-lookup"><span data-stu-id="7b15b-117">tooprevent a reboot, hello certificate used tooencrypt hello password must be installed on hello role.</span></span> <span data-ttu-id="7b15b-118">tooprevent en omstart [överföra ett certifikat för hello Molntjänsten](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) och returnerar sedan toothis dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7b15b-118">tooprevent a restart, [upload a certificate for hello cloud service](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) and then return toothis dialog.</span></span>

3. <span data-ttu-id="7b15b-119">I **roller**väljer hello rollen tooupdate eller välj **alla** för alla roller.</span><span class="sxs-lookup"><span data-stu-id="7b15b-119">In **Roles**, select hello role you want tooupdate or select **All** for all roles.</span></span>
4. <span data-ttu-id="7b15b-120">Göra hello följande ändringar:</span><span class="sxs-lookup"><span data-stu-id="7b15b-120">Make any of hello following changes:</span></span>

   * <span data-ttu-id="7b15b-121">tooenable fjärrskrivbord, Välj hello **aktivera Fjärrskrivbord** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="7b15b-121">tooenable Remote Desktop, select hello **Enable Remote Desktop** check box.</span></span> <span data-ttu-id="7b15b-122">toodisable Rensa hello kryssrutan Fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="7b15b-122">toodisable Remote Desktop, clear hello check box.</span></span>
   * <span data-ttu-id="7b15b-123">Skapa ett konto toouse i anslutning till fjärrskrivbord toohello rollinstanser.</span><span class="sxs-lookup"><span data-stu-id="7b15b-123">Create an account toouse in Remote Desktop connections toohello role instances.</span></span>
   * <span data-ttu-id="7b15b-124">Uppdatera hello lösenord för hello befintliga konto.</span><span class="sxs-lookup"><span data-stu-id="7b15b-124">Update hello password for hello existing account.</span></span>
   * <span data-ttu-id="7b15b-125">Välj en toouse överförda certifikat för autentisering (Överför hello certifikat med **överför** på hello **certifikat** sidan) eller skapa ett nytt certifikat.</span><span class="sxs-lookup"><span data-stu-id="7b15b-125">Select an uploaded certificate toouse for authentication (upload hello certificate using **Upload** on hello **Certificates** page) or create a new certificate.</span></span>
   * <span data-ttu-id="7b15b-126">Ändra hello utgångsdatum hello konfigurationen av fjärrskrivbordet.</span><span class="sxs-lookup"><span data-stu-id="7b15b-126">Change hello expiration date for hello Remote Desktop configuration.</span></span>

5. <span data-ttu-id="7b15b-127">När du är klar configuration-uppdateringar, klickar du på **OK** (markering).</span><span class="sxs-lookup"><span data-stu-id="7b15b-127">When you finish your configuration updates, click **OK** (checkmark).</span></span>

## <a name="remote-into-role-instances"></a><span data-ttu-id="7b15b-128">Remote i rollinstanser</span><span class="sxs-lookup"><span data-stu-id="7b15b-128">Remote into role instances</span></span>
<span data-ttu-id="7b15b-129">När fjärrskrivbord är aktiverat på hello roller kan du fjärråtkomst till en rollinstans via olika verktyg.</span><span class="sxs-lookup"><span data-stu-id="7b15b-129">Once Remote Desktop is enabled on hello roles you can remote into a role instance through various tools.</span></span>

<span data-ttu-id="7b15b-130">tooconnect tooa rollinstansen från hello klassiska Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="7b15b-130">tooconnect tooa role instance from hello Azure classic portal:</span></span>

1. <span data-ttu-id="7b15b-131">Klicka på **instanser** tooopen hello **instanser** sidan.</span><span class="sxs-lookup"><span data-stu-id="7b15b-131">Click **Instances** tooopen hello **Instances** page.</span></span>
2. <span data-ttu-id="7b15b-132">Välj en rollinstans som har konfigurerats för fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="7b15b-132">Select a role instance that has Remote Desktop configured.</span></span>
3. <span data-ttu-id="7b15b-133">Klicka på **Anslut**, och följ hello instruktioner tooopen hello skrivbordet.</span><span class="sxs-lookup"><span data-stu-id="7b15b-133">Click **Connect**, and follow hello instructions tooopen hello desktop.</span></span>
4. <span data-ttu-id="7b15b-134">Klicka på **öppna** och sedan **Anslut** toostart hello fjärrskrivbordsanslutning.</span><span class="sxs-lookup"><span data-stu-id="7b15b-134">Click **Open** and then **Connect** toostart hello Remote Desktop connection.</span></span>

### <a name="use-visual-studio-tooremote-into-a-role-instance"></a><span data-ttu-id="7b15b-135">Använd Visual Studio tooremote till en rollinstans</span><span class="sxs-lookup"><span data-stu-id="7b15b-135">Use Visual Studio tooremote into a role instance</span></span>
<span data-ttu-id="7b15b-136">I Visual Studio Server Explorer:</span><span class="sxs-lookup"><span data-stu-id="7b15b-136">In Visual Studio, Server Explorer:</span></span>

1. <span data-ttu-id="7b15b-137">Expandera hello **Azure** > **molntjänster** > **[molntjänstnamnet]** nod.</span><span class="sxs-lookup"><span data-stu-id="7b15b-137">Expand hello **Azure** > **Cloud Services** > **[cloud service name]** node.</span></span>
2. <span data-ttu-id="7b15b-138">Expandera antingen **mellanlagring** eller **produktion**.</span><span class="sxs-lookup"><span data-stu-id="7b15b-138">Expand either **Staging** or **Production**.</span></span>
3. <span data-ttu-id="7b15b-139">Expandera hello enskilda roll.</span><span class="sxs-lookup"><span data-stu-id="7b15b-139">Expand hello individual role.</span></span>
4. <span data-ttu-id="7b15b-140">Högerklicka på en av hello rollinstanser, klickar du på **ansluta med hjälp av fjärrskrivbord...** , och ange sedan hello användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="7b15b-140">Right-click one of hello role instances, click **Connect using Remote Desktop...**, and then enter hello user name and password.</span></span>

![Fjärrskrivbord i Server explorer](./media/cloud-services-role-enable-remote-desktop/ServerExplorer_RemoteDesktop.png)

### <a name="use-powershell-tooget-hello-rdp-file"></a><span data-ttu-id="7b15b-142">Använd PowerShell tooget hello RDP-filen</span><span class="sxs-lookup"><span data-stu-id="7b15b-142">Use PowerShell tooget hello RDP file</span></span>
<span data-ttu-id="7b15b-143">Du kan använda hello [Get-AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) cmdlet tooretrieve hello RDP-filen.</span><span class="sxs-lookup"><span data-stu-id="7b15b-143">You can use hello [Get-AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) cmdlet tooretrieve hello RDP file.</span></span> <span data-ttu-id="7b15b-144">Du kan sedan använda hello RDP-filen med anslutning till fjärrskrivbord tooaccess hello-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="7b15b-144">You can then use hello RDP file with Remote Desktop Connection tooaccess hello cloud service.</span></span>

### <a name="programmatically-download-hello-rdp-file-through-hello-service-management-rest-api"></a><span data-ttu-id="7b15b-145">Hämta programmässigt hello RDP-fil med hello Service Management REST API</span><span class="sxs-lookup"><span data-stu-id="7b15b-145">Programmatically download hello RDP file through hello Service Management REST API</span></span>
<span data-ttu-id="7b15b-146">Du kan använda hello [ladda ned RDP-filen](https://msdn.microsoft.com/library/jj157183.aspx) REST åtgärden toodownload hello RDP-filen.</span><span class="sxs-lookup"><span data-stu-id="7b15b-146">You can use hello [Download RDP File](https://msdn.microsoft.com/library/jj157183.aspx) REST operation toodownload hello RDP file.</span></span>

## <a name="tooconfigure-remote-desktop-in-hello-service-definition-file"></a><span data-ttu-id="7b15b-147">tooconfigure fjärrskrivbord i hello tjänstdefinitionsfilen</span><span class="sxs-lookup"><span data-stu-id="7b15b-147">tooconfigure Remote Desktop in hello service definition file</span></span>
<span data-ttu-id="7b15b-148">Den här metoden kan du tooenable Remote Desktop för hello program under utveckling.</span><span class="sxs-lookup"><span data-stu-id="7b15b-148">This method allows you tooenable Remote Desktop for hello application during development.</span></span> <span data-ttu-id="7b15b-149">Den här metoden kräver krypterade lösenord lagras i tjänstkonfigurationen av fil- och eventuella uppdateringar toohello konfiguration av fjärrskrivbord kräver en omdistribution av programmet hello.</span><span class="sxs-lookup"><span data-stu-id="7b15b-149">This approach requires encrypted passwords be stored in your service configuration file and any updates toohello remote desktop configuration would require a redeployment of hello application.</span></span> <span data-ttu-id="7b15b-150">Om du vill tooavoid baserat dessa downsides bör du använda hello remote desktop tillägget metod som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="7b15b-150">If you want tooavoid these downsides you should use hello remote desktop extension based approach described above.</span></span>  

<span data-ttu-id="7b15b-151">Du kan använda Visual Studio för[aktivera en fjärrskrivbordsanslutning](../vs-azure-tools-remote-desktop-roles.md) med hello service definition filen metoden.</span><span class="sxs-lookup"><span data-stu-id="7b15b-151">You can use Visual Studio too[enable a remote desktop connection](../vs-azure-tools-remote-desktop-roles.md) using hello service definition file approach.</span></span>  
<span data-ttu-id="7b15b-152">hello stegen nedan beskriver hello ändringar behövs toohello service model filer tooenable fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="7b15b-152">hello steps below describe hello changes needed toohello service model files tooenable remote desktop.</span></span> <span data-ttu-id="7b15b-153">Visual Studio gör automatiskt dessa ändringar när du publicerar.</span><span class="sxs-lookup"><span data-stu-id="7b15b-153">Visual Studio will automatically makes these changes when publishing.</span></span>

### <a name="set-up-hello-connection-in-hello-service-model"></a><span data-ttu-id="7b15b-154">Konfigurera hello anslutning i hello tjänstmodell</span><span class="sxs-lookup"><span data-stu-id="7b15b-154">Set up hello connection in hello service model</span></span>
<span data-ttu-id="7b15b-155">Använd hello **import** elementet tooimport hello **RemoteAccess** modulen och hello **RemoteForwarder** modulen toohello [ServiceDefinition.csdef](cloud-services-model-and-package.md#csdef) fil.</span><span class="sxs-lookup"><span data-stu-id="7b15b-155">Use hello **Imports** element tooimport hello **RemoteAccess** module and hello **RemoteForwarder** module toohello [ServiceDefinition.csdef](cloud-services-model-and-package.md#csdef) file.</span></span>

<span data-ttu-id="7b15b-156">Hej tjänstdefinitionsfilen ska vara liknande toohello följande exempel med hello `<Imports>` element som har lagts till.</span><span class="sxs-lookup"><span data-stu-id="7b15b-156">hello service definition file should be similar toohello following example with hello `<Imports>` element added.</span></span>

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
<span data-ttu-id="7b15b-157">Hej [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#cscfg) filen bör vara liknande toohello som följande exempel, Observera hello `<ConfigurationSettings>` och `<Certificates>` element.</span><span class="sxs-lookup"><span data-stu-id="7b15b-157">hello [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#cscfg) file should be similar toohello following example, note hello `<ConfigurationSettings>` and `<Certificates>` elements.</span></span> <span data-ttu-id="7b15b-158">hello certifikatet som anges måste vara [upp toohello Molntjänsten](cloud-services-how-to-create-deploy.md#how-to-upload-a-certificate-for-a-cloud-service).</span><span class="sxs-lookup"><span data-stu-id="7b15b-158">hello Certificate specified must be [uploaded toohello cloud service](cloud-services-how-to-create-deploy.md#how-to-upload-a-certificate-for-a-cloud-service).</span></span>

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


## <a name="additional-resources"></a><span data-ttu-id="7b15b-159">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="7b15b-159">Additional Resources</span></span>
<span data-ttu-id="7b15b-160">[Hur tooConfigure molntjänster](cloud-services-how-to-configure.md)
[Cloud services vanliga frågor och svar - fjärrskrivbord](cloud-services-faq.md)</span><span class="sxs-lookup"><span data-stu-id="7b15b-160">[How tooConfigure Cloud Services](cloud-services-how-to-configure.md)
[Cloud services FAQ - Remote Desktop](cloud-services-faq.md)</span></span>
