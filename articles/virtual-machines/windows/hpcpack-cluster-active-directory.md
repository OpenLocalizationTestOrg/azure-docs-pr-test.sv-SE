---
title: aaaHPC Pack kluster med Azure Active Directory | Microsoft Docs
description: "Lär dig hur toointegrate ett HPC Pack 2016 kluster i Azure med Azure Active Directory"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
ms.assetid: 9edf9559-db02-438b-8268-a6cba7b5c8b7
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 11/14/2016
ms.author: danlep
ms.openlocfilehash: 0806e544a468e27ca0567e18c55554811584fbc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-an-hpc-pack-cluster-in-azure-using-azure-active-directory"></a><span data-ttu-id="8645e-103">Hantera ett HPC Pack kluster i Azure med hjälp av Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8645e-103">Manage an HPC Pack cluster in Azure using Azure Active Directory</span></span>
<span data-ttu-id="8645e-104">[Microsoft HPC Pack 2016](https://technet.microsoft.com/library/cc514029) stöder integration med [Azure Active Directory](../../active-directory/index.md) (Azure AD) för administratörer som distribuerar ett HPC Pack kluster i Azure.</span><span class="sxs-lookup"><span data-stu-id="8645e-104">[Microsoft HPC Pack 2016](https://technet.microsoft.com/library/cc514029) supports integration with [Azure Active Directory](../../active-directory/index.md) (Azure AD) for administrators who deploy an HPC Pack cluster in Azure.</span></span>



<span data-ttu-id="8645e-105">Följ hello stegen i den här artikeln för följande uppgifter på hög hello:</span><span class="sxs-lookup"><span data-stu-id="8645e-105">Follow hello steps in this article for hello following high level tasks:</span></span> 
* <span data-ttu-id="8645e-106">Integrera manuellt HPC Pack klustret med Azure AD-klient</span><span class="sxs-lookup"><span data-stu-id="8645e-106">Manually integrate your HPC Pack cluster with your Azure AD tenant</span></span>
* <span data-ttu-id="8645e-107">Hantera och schemalägga jobb i HPC Pack klustret i Azure</span><span class="sxs-lookup"><span data-stu-id="8645e-107">Manage and schedule jobs in your HPC Pack cluster in Azure</span></span> 

<span data-ttu-id="8645e-108">En klusterlösning HPC Pack integrera med Azure AD följer standard steg toointegrate andra program och tjänster.</span><span class="sxs-lookup"><span data-stu-id="8645e-108">Integrating an HPC Pack cluster solution with Azure AD follows standard steps toointegrate other applications and services.</span></span> <span data-ttu-id="8645e-109">Den här artikeln förutsätter att du är bekant med grundläggande användarhantering i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8645e-109">This article assumes you are familiar with basic user management in Azure AD.</span></span> <span data-ttu-id="8645e-110">Mer information och bakgrunden finns hello [dokumentationen för Azure Active Directory](../../active-directory/index.md) och hello efter avsnittet.</span><span class="sxs-lookup"><span data-stu-id="8645e-110">For more information and background, see hello [Azure Active Directory documentation](../../active-directory/index.md) and hello following section.</span></span>

## <a name="benefits-of-integration"></a><span data-ttu-id="8645e-111">Fördelar med integrering</span><span class="sxs-lookup"><span data-stu-id="8645e-111">Benefits of integration</span></span>


<span data-ttu-id="8645e-112">Azure Active Directory (AD Azure) är en flera innehavare molnbaserade katalog och identity management-tjänst som tillhandahåller enkel inloggning (SSO) toocloud lösningar.</span><span class="sxs-lookup"><span data-stu-id="8645e-112">Azure Active Directory (Azure AD) is a multi-tenant cloud-based directory and identity management service that provides single sign-on (SSO) access toocloud solutions.</span></span>

<span data-ttu-id="8645e-113">Integrering av ett HPC Pack kluster med Azure AD kan hjälpa dig att uppnå följande mål hello:</span><span class="sxs-lookup"><span data-stu-id="8645e-113">Integration of an HPC Pack cluster with Azure AD can help you achieve hello following goals:</span></span>

* <span data-ttu-id="8645e-114">Ta bort hello traditionella Active Directory-domänkontrollant från hello HPC Pack kluster.</span><span class="sxs-lookup"><span data-stu-id="8645e-114">Remove hello traditional Active Directory domain controller from hello HPC Pack cluster.</span></span> <span data-ttu-id="8645e-115">Det kan minska hello driftskostnader hello klustret om det inte är nödvändiga för ditt företag och påskynda hello distributionsprocessen.</span><span class="sxs-lookup"><span data-stu-id="8645e-115">This can help reduce hello costs of maintaining hello cluster if this is not necessary for your business, and speed-up hello deployment process.</span></span>
* <span data-ttu-id="8645e-116">Använda hello följande fördelar som ansluts av Azure AD:</span><span class="sxs-lookup"><span data-stu-id="8645e-116">Leverage hello following benefits that are brought by Azure AD:</span></span>
    *   <span data-ttu-id="8645e-117">Enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8645e-117">Single sign-on</span></span> 
    *   <span data-ttu-id="8645e-118">Med hjälp av en lokal AD-identitet för hello HPC Pack kluster i Azure</span><span class="sxs-lookup"><span data-stu-id="8645e-118">Using a local AD identity for hello HPC Pack cluster in Azure</span></span> 

    ![Azure Active Directory-miljö](./media/hpcpack-cluster-active-directory/aad.png)


## <a name="prerequisites"></a><span data-ttu-id="8645e-120">Krav</span><span class="sxs-lookup"><span data-stu-id="8645e-120">Prerequisites</span></span>
* <span data-ttu-id="8645e-121">**HPC Pack 2016-kluster som distribueras på Azure-datorer** – anvisningar finns i [distribuera ett HPC Pack 2016-kluster i Azure](hpcpack-2016-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="8645e-121">**HPC Pack 2016 cluster deployed in Azure virtual machines** - For steps, see [Deploy an HPC Pack 2016 cluster in Azure](hpcpack-2016-cluster.md).</span></span> <span data-ttu-id="8645e-122">Du behöver hello DNS-namnet på hello huvudnod och hello autentiseringsuppgifterna för en Klusteradministratör för att slutföra hello stegen i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="8645e-122">You need hello DNS name of hello head node and hello credentials of a cluster administrator to complete hello steps in this article.</span></span>

  > [!NOTE]
  > <span data-ttu-id="8645e-123">Azure Active Directory-integrering stöds inte i versioner av HPC Pack före HPC Pack 2016.</span><span class="sxs-lookup"><span data-stu-id="8645e-123">Azure Active Directory integration is not supported in versions of HPC Pack before HPC Pack 2016.</span></span>



* <span data-ttu-id="8645e-124">**Klientdatorn** -du behöver ett Windows eller Windows Server klienten datorn kör för HPC Pack klientverktyg.</span><span class="sxs-lookup"><span data-stu-id="8645e-124">**Client computer** - You need a Windows or Windows Server client computer too run HPC Pack client utilities.</span></span> <span data-ttu-id="8645e-125">Om du bara vill toouse hello HPC Pack webbportal eller REST API toosubmit jobb kan du använda alla klientdatorer som du väljer.</span><span class="sxs-lookup"><span data-stu-id="8645e-125">If you only want toouse hello HPC Pack web portal or REST API toosubmit jobs, you can use any client computer of your choice.</span></span>

* <span data-ttu-id="8645e-126">**HPC Pack klientverktyg** -installera hello HPC Pack klientverktyg på hello klientdatorn med hello ledigt installationspaket från hello Microsoft Download Center.</span><span class="sxs-lookup"><span data-stu-id="8645e-126">**HPC Pack client utilities** - Install hello HPC Pack client utilities on hello client computer, using hello free installation package available from hello Microsoft Download Center.</span></span>


## <a name="step-1-register-hello-hpc-cluster-server-with-your-azure-ad-tenant"></a><span data-ttu-id="8645e-127">Steg 1: Registrera hello HPC cluster server med Azure AD-klient</span><span class="sxs-lookup"><span data-stu-id="8645e-127">Step 1: Register hello HPC cluster server with your Azure AD tenant</span></span>
1. <span data-ttu-id="8645e-128">Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="8645e-128">Sign in toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="8645e-129">Klicka på **Active Directory** i hello vänstra menyn och klicka sedan på önskad hello-katalogen i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="8645e-129">Click **Active Directory** in hello left menu, and then click hello desired directory in your subscription.</span></span> <span data-ttu-id="8645e-130">Du måste ha behörigheten tooaccess resurser i hello directory.</span><span class="sxs-lookup"><span data-stu-id="8645e-130">You must have permission tooaccess resources in hello directory.</span></span>
3. <span data-ttu-id="8645e-131">Klicka på **användare**, och kontrollera att det finns användarkonton redan skapats eller konfigurerade.</span><span class="sxs-lookup"><span data-stu-id="8645e-131">Click **Users**, and make sure there are user accounts already created or configured.</span></span>
4. <span data-ttu-id="8645e-132">Klicka på **program** > **Lägg till**, och klicka sedan på **Lägg till ett program som min organisation utvecklar**.</span><span class="sxs-lookup"><span data-stu-id="8645e-132">Click **Applications** > **Add**, and then click **Add an application my organization is developing**.</span></span> <span data-ttu-id="8645e-133">Ange följande information i guiden hello hello:</span><span class="sxs-lookup"><span data-stu-id="8645e-133">Enter hello following information in hello wizard:</span></span>
    * <span data-ttu-id="8645e-134">**Namnet** -HPCPackClusterServer</span><span class="sxs-lookup"><span data-stu-id="8645e-134">**Name** - HPCPackClusterServer</span></span>
    * <span data-ttu-id="8645e-135">**Typen** – Välj **webb-program och/eller webb-API**</span><span class="sxs-lookup"><span data-stu-id="8645e-135">**Type** - Select **Web Application and/or Web API**</span></span>
    * <span data-ttu-id="8645e-136">**URL: en inloggning**- hello bas-URL för hello-exemplet som är som standard`https://hpcserver`</span><span class="sxs-lookup"><span data-stu-id="8645e-136">**Sign-on URL**- hello base URL for hello sample, which is by default `https://hpcserver`</span></span>
    * <span data-ttu-id="8645e-137">**App-ID URI** - `https://<Directory_name>/<application_name>`.</span><span class="sxs-lookup"><span data-stu-id="8645e-137">**App ID URI** - `https://<Directory_name>/<application_name>`.</span></span> <span data-ttu-id="8645e-138">Ersätt `<Directory_name`> med hello fullständigt namn för din Azure AD-klient, till exempel `hpclocal.onmicrosoft.com`, och Ersätt `<application_name>` med hello-namn som du valde tidigare.</span><span class="sxs-lookup"><span data-stu-id="8645e-138">Replace `<Directory_name`> with hello full name of your Azure AD tenant, for example, `hpclocal.onmicrosoft.com`, and replace `<application_name>` with hello name you chose previously.</span></span>

5. <span data-ttu-id="8645e-139">När hello app läggs klickar du på **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="8645e-139">After hello app is added, click **Configure**.</span></span> <span data-ttu-id="8645e-140">Konfigurera hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="8645e-140">Configure hello following properties:</span></span>
    * <span data-ttu-id="8645e-141">Välj **Ja** för **program är flera innehavare**</span><span class="sxs-lookup"><span data-stu-id="8645e-141">Select **Yes** for **Application is multi-tenant**</span></span>
    * <span data-ttu-id="8645e-142">Välj **Ja** för **användaren tilldelning krävs tooaccess app**.</span><span class="sxs-lookup"><span data-stu-id="8645e-142">Select **Yes** for **User assignment required tooaccess app**.</span></span>

6. <span data-ttu-id="8645e-143">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="8645e-143">Click **Save**.</span></span> <span data-ttu-id="8645e-144">När du sparar är klar klickar du på **hantera Manifest**.</span><span class="sxs-lookup"><span data-stu-id="8645e-144">When saving completes, click **Manage Manifest**.</span></span> <span data-ttu-id="8645e-145">Den här åtgärden hämtar programmets JavaScript object notation (JSON) manifestfilen.</span><span class="sxs-lookup"><span data-stu-id="8645e-145">This action downloads your application’s manifest JavaScript object notation (JSON) file.</span></span> <span data-ttu-id="8645e-146">Redigera hello hämtas manifestet genom att söka efter hello `appRoles` inställning och lägga till följande programrollen hello:</span><span class="sxs-lookup"><span data-stu-id="8645e-146">Edit hello downloaded manifest by locating hello `appRoles` setting and adding hello following application role:</span></span>
    ```json
    "appRoles": [
        {
        "allowedMemberTypes": [
            "User",
            "Application"
        ],
        "displayName": "HpcAdminMirror",
        "id": "61e10148-16a8-432a-b86d-ef620c3e48ef",
        "isEnabled": true,
        "description": "HpcAdminMirror",
        "value": "HpcAdminMirror"
        },
        {
        "allowedMemberTypes": [
            "User",
            "Application"
        ],
        "description": "HpcUsers",
        "displayName": "HpcUsers",
        "id": "91e10148-16a8-432a-b86d-ef620c3e48ef",
        "isEnabled": true,
        "value": "HpcUsers"
        }
    ],
    ```
7. <span data-ttu-id="8645e-147">Spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="8645e-147">Save hello file.</span></span> <span data-ttu-id="8645e-148">Klicka sedan på hello portal **hantera Manifest** > **överför Manifest**.</span><span class="sxs-lookup"><span data-stu-id="8645e-148">Then in hello portal, click **Manage Manifest** > **Upload Manifest**.</span></span> <span data-ttu-id="8645e-149">Du kan sedan överföra hello redigerade manifest.</span><span class="sxs-lookup"><span data-stu-id="8645e-149">You can then upload hello edited manifest.</span></span>
8. <span data-ttu-id="8645e-150">Klicka på **användare**, Välj en användare och klicka sedan på **tilldela**.</span><span class="sxs-lookup"><span data-stu-id="8645e-150">Click **Users**, select a user, and then click **Assign**.</span></span> <span data-ttu-id="8645e-151">Tilldela en hello tillgängliga roller (HpcUsers eller HpcAdminMirror) toohello användare.</span><span class="sxs-lookup"><span data-stu-id="8645e-151">Assign one of hello available roles (HpcUsers or HpcAdminMirror) toohello user.</span></span> <span data-ttu-id="8645e-152">Upprepa detta steg med ytterligare användare i hello directory.</span><span class="sxs-lookup"><span data-stu-id="8645e-152">Repeat this step with additional users in hello directory.</span></span> <span data-ttu-id="8645e-153">Bakgrundsinformation om klustret användare finns [hantera klustret användare](https://technet.microsoft.com/library/ff919335(v=ws.11).aspx).</span><span class="sxs-lookup"><span data-stu-id="8645e-153">For background information about cluster users, see [Managing Cluster Users](https://technet.microsoft.com/library/ff919335(v=ws.11).aspx).</span></span>

   > [!NOTE] 
   > <span data-ttu-id="8645e-154">toomanage användare, bör du använda hello Azure Active Directory preview bladet i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8645e-154">toomanage users, we recommend using hello Azure Active Directory preview blade in hello [Azure portal](https://portal.azure.com).</span></span>
   >


## <a name="step-2-register-hello-hpc-cluster-client-with-your-azure-ad-tenant"></a><span data-ttu-id="8645e-155">Steg 2: Registrera hello HPC-kluster klienten med Azure AD-klient</span><span class="sxs-lookup"><span data-stu-id="8645e-155">Step 2: Register hello HPC cluster client with your Azure AD tenant</span></span>

1. <span data-ttu-id="8645e-156">Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="8645e-156">Sign in toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="8645e-157">Klicka på **Active Directory** i hello vänstra menyn och klicka sedan på önskad hello-katalogen i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="8645e-157">Click **Active Directory** in hello left menu, and then click hello desired directory in your subscription.</span></span> <span data-ttu-id="8645e-158">Du måste ha behörigheten tooaccess resurser i hello directory.</span><span class="sxs-lookup"><span data-stu-id="8645e-158">You must have permission tooaccess resources in hello directory.</span></span>
3. <span data-ttu-id="8645e-159">Klicka på **program** > **Lägg till**, och klicka sedan på **Lägg till ett program som min organisation utvecklar**.</span><span class="sxs-lookup"><span data-stu-id="8645e-159">Click **Applications** > **Add**, and then click **Add an application my organization is developing**.</span></span> <span data-ttu-id="8645e-160">Ange följande information i guiden hello hello:</span><span class="sxs-lookup"><span data-stu-id="8645e-160">Enter hello following information in hello wizard:</span></span>

    * <span data-ttu-id="8645e-161">**Namnet** -HPCPackClusterClient</span><span class="sxs-lookup"><span data-stu-id="8645e-161">**Name** - HPCPackClusterClient</span></span>
    * <span data-ttu-id="8645e-162">**Typen** – Välj **Native Client-program**</span><span class="sxs-lookup"><span data-stu-id="8645e-162">**Type** - Select **Native Client Application**</span></span>
    * <span data-ttu-id="8645e-163">**Omdirigerings-URI** - `http://hpcclient`</span><span class="sxs-lookup"><span data-stu-id="8645e-163">**Redirect URI** - `http://hpcclient`</span></span>

4. <span data-ttu-id="8645e-164">När hello app läggs klickar du på **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="8645e-164">After hello app is added, click **Configure**.</span></span> <span data-ttu-id="8645e-165">Kopiera hello **klient-ID** värde och spara den.</span><span class="sxs-lookup"><span data-stu-id="8645e-165">Copy hello **Client ID** value and save it.</span></span> <span data-ttu-id="8645e-166">Du behöver det senare när du konfigurerar ditt program.</span><span class="sxs-lookup"><span data-stu-id="8645e-166">You need this later when configuring your application.</span></span>

5. <span data-ttu-id="8645e-167">I **behörigheter tooother program**, klickar du på **Lägg till program**.</span><span class="sxs-lookup"><span data-stu-id="8645e-167">In **Permissions tooother applications**, click **Add Application**.</span></span> <span data-ttu-id="8645e-168">Söka och lägga till hello HpcPackClusterServer program (som du skapade i steg 1).</span><span class="sxs-lookup"><span data-stu-id="8645e-168">Search and add hello  HpcPackClusterServer application (created in Step 1).</span></span>

6. <span data-ttu-id="8645e-169">I hello **delegerade behörigheter** listrutan **åtkomst HpcClusterServer**.</span><span class="sxs-lookup"><span data-stu-id="8645e-169">In hello **Delegated Permissions** dropdown, select **Access HpcClusterServer**.</span></span> <span data-ttu-id="8645e-170">Klicka sedan på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="8645e-170">Then click **Save**.</span></span>


## <a name="step-3-configure-hello-hpc-cluster"></a><span data-ttu-id="8645e-171">Steg 3: Konfigurera hello HPC-kluster</span><span class="sxs-lookup"><span data-stu-id="8645e-171">Step 3: Configure hello HPC cluster</span></span>

1. <span data-ttu-id="8645e-172">Ansluta toohello HPC Pack 2016 huvudnod i Azure.</span><span class="sxs-lookup"><span data-stu-id="8645e-172">Connect toohello HPC Pack 2016 head node in Azure.</span></span>

2. <span data-ttu-id="8645e-173">Starta HPC PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8645e-173">Start HPC PowerShell.</span></span>

3. <span data-ttu-id="8645e-174">Kör följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="8645e-174">Run hello following command:</span></span>

    ```powershell

    Set-HpcClusterRegistry -SupportAAD true -AADInstance https://login.microsoftonline.com/ -AADAppName HpcClusterServer -AADTenant <your AAD tenant name> -AADClientAppId <client ID> -AADClientAppRedirectUri http://hpcclient
    ```
    <span data-ttu-id="8645e-175">där</span><span class="sxs-lookup"><span data-stu-id="8645e-175">where</span></span>

    * <span data-ttu-id="8645e-176">`AADTenant`Anger hello Azure AD-klientnamn, exempel`hpclocal.onmicrosoft.com`</span><span class="sxs-lookup"><span data-stu-id="8645e-176">`AADTenant` specifies hello Azure AD tenant name, such as `hpclocal.onmicrosoft.com`</span></span>
    * <span data-ttu-id="8645e-177">`AADClientAppId`Anger hello klient-ID för hello app skapade i steg 2.</span><span class="sxs-lookup"><span data-stu-id="8645e-177">`AADClientAppId` specifies hello client ID for hello app created in Step 2.</span></span>

4. <span data-ttu-id="8645e-178">Starta om hello HpcSchedulerStateful.</span><span class="sxs-lookup"><span data-stu-id="8645e-178">Restart hello HpcSchedulerStateful service.</span></span>

    <span data-ttu-id="8645e-179">I ett kluster med flera huvudnoderna kan du köra följande PowerShell-kommandon på hello huvudnod tooswitch hello primära repliken för hello HpcSchedulerStateful tjänsten hello:</span><span class="sxs-lookup"><span data-stu-id="8645e-179">In a cluster with multiple head nodes, you can run hello following PowerShell commands on hello head node tooswitch hello primary replica for hello HpcSchedulerStateful service:</span></span>

    ```powershell
    Connect-ServiceFabricCluster

    Move-ServiceFabricPrimaryReplica –ServiceName “fabric:/HpcApplication/SchedulerStatefulService”

    ```

## <a name="step-4-manage-and-submit-jobs-from-hello-client"></a><span data-ttu-id="8645e-180">Steg 4: Hantera och skicka jobb hello-klient</span><span class="sxs-lookup"><span data-stu-id="8645e-180">Step 4: Manage and submit jobs from hello client</span></span>

<span data-ttu-id="8645e-181">Hej tooinstall HPC Pack klientverktyg på datorn, hämta HPC Pack 2016-installationsfiler (fullständig installation) från hello Microsoft Download Center.</span><span class="sxs-lookup"><span data-stu-id="8645e-181">tooinstall hello HPC Pack client utilities on your computer, download the HPC Pack 2016 setup files (full installation) from hello Microsoft Download Center.</span></span> <span data-ttu-id="8645e-182">När du börjar installationen hello väljer hello installationsalternativ för hello **HPC Pack klientverktyg**.</span><span class="sxs-lookup"><span data-stu-id="8645e-182">When you begin hello installation, choose hello setup option for hello **HPC Pack client utilities**.</span></span>

<span data-ttu-id="8645e-183">tooprepare hello klientdatorn, installera hello-certifikat som används under [konfiguration av HPC](hpcpack-2016-cluster.md) på hello-klientdator.</span><span class="sxs-lookup"><span data-stu-id="8645e-183">tooprepare hello client computer, install hello certificate used during [HPC cluster setup](hpcpack-2016-cluster.md) on hello client computer.</span></span> <span data-ttu-id="8645e-184">Använd standard Windows certificate management procedurer tooinstall hello certifikatets offentliga toohello **certifikat – aktuell användare** > **betrodda rotcertifikatutfärdare** lagras.</span><span class="sxs-lookup"><span data-stu-id="8645e-184">Use standard Windows certificate management procedures tooinstall hello public certificate toohello **Certificates – Current user** > **Trusted Root Certification Authorities** store.</span></span> 

<span data-ttu-id="8645e-185">Du kan nu köra hello HPC Pack kommandon eller använda hello HPC Pack Job manager GUI toosubmit och hantera kluster-jobb med hjälp av hello Azure AD-konto.</span><span class="sxs-lookup"><span data-stu-id="8645e-185">You can now run hello HPC Pack commands or use hello HPC Pack Job manager GUI toosubmit and manage cluster jobs by using hello Azure AD account.</span></span> <span data-ttu-id="8645e-186">Skicka alternativ, se [skicka HPC jobb tooan HPC Pack kluster i Azure](hpcpack-cluster-submit-jobs.md#step-3-run-test-jobs-on-the-cluster).</span><span class="sxs-lookup"><span data-stu-id="8645e-186">For job submission options, see [Submit HPC jobs tooan HPC Pack cluster in Azure](hpcpack-cluster-submit-jobs.md#step-3-run-test-jobs-on-the-cluster).</span></span>

> [!NOTE]
> <span data-ttu-id="8645e-187">När du försöker tooconnect toohello HPC Pack kluster i Azure för hello första gången, visas ett popup-fönster.</span><span class="sxs-lookup"><span data-stu-id="8645e-187">When you try tooconnect toohello HPC Pack cluster in Azure for hello first time, a popup windows appears.</span></span> <span data-ttu-id="8645e-188">Ange dina autentiseringsuppgifter toolog i Azure AD i.</span><span class="sxs-lookup"><span data-stu-id="8645e-188">Enter your Azure AD credentials toolog in.</span></span> <span data-ttu-id="8645e-189">hello token cachelagras sedan.</span><span class="sxs-lookup"><span data-stu-id="8645e-189">hello token is then cached.</span></span> <span data-ttu-id="8645e-190">Senare anslutningar toohello kluster i Azure används hello cachelagras token om autentisering ändringar eller hello cachelagras är avmarkerad.</span><span class="sxs-lookup"><span data-stu-id="8645e-190">Later connections toohello cluster in Azure use hello cached token unless authentication changes or hello cached is cleared.</span></span>
>
  
<span data-ttu-id="8645e-191">Till exempel när du har slutfört hello föregående steg kan du fråga efter jobb från en lokal klient på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="8645e-191">For example, after completing hello previous steps, you can query for jobs from an on-premises client as follows:</span></span>

```powershell 
Get-HpcJob –State All –Scheduler https://<Azure load balancer DNS name> -Owner <Azure AD account>
```

## <a name="useful-cmdlets-for-job-submission-with-azure-ad-integration"></a><span data-ttu-id="8645e-192">Användbar cmdlets för jobbet med Azure AD-integrering</span><span class="sxs-lookup"><span data-stu-id="8645e-192">Useful cmdlets for job submission with Azure AD integration</span></span> 

### <a name="manage-hello-local-token-cache"></a><span data-ttu-id="8645e-193">Hantera hello lokala token-cache</span><span class="sxs-lookup"><span data-stu-id="8645e-193">Manage hello local token cache</span></span>

<span data-ttu-id="8645e-194">HPC Pack 2016 innehåller två nya HPC PowerShell-cmdlets toomanage hello lokala token-cache.</span><span class="sxs-lookup"><span data-stu-id="8645e-194">HPC Pack 2016 provides two new HPC PowerShell cmdlets toomanage hello local token cache.</span></span> <span data-ttu-id="8645e-195">Dessa cmdlet: ar är användbara för att skicka jobb icke-interaktivt.</span><span class="sxs-lookup"><span data-stu-id="8645e-195">These cmdlets are useful for submitting jobs non-interactively.</span></span> <span data-ttu-id="8645e-196">Se följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="8645e-196">See hello following example:</span></span>

```powershell
Remove-HpcTokenCache

$SecurePassword = "<password>" | ConvertTo-SecureString -AsPlainText -Force

Set-HpcTokenCache -UserName <AADUsername> -Password $SecurePassword -scheduler https://<Azure load balancer DNS name> 
```

### <a name="set-hello-credentials-for-submitting-jobs-using-hello-azure-ad-account"></a><span data-ttu-id="8645e-197">Ange hello autentiseringsuppgifter för att skicka jobb med hjälp av hello Azure AD-konto</span><span class="sxs-lookup"><span data-stu-id="8645e-197">Set hello credentials for submitting jobs using hello Azure AD account</span></span> 

<span data-ttu-id="8645e-198">Du kan ibland vill toorun hello jobbet under hello HPC-kluster användare (för en domänansluten HPC-kluster körs som en domänanvändare; för ett icke-domänanslutna HPC-kluster kör som en lokal användare på hello huvudnod).</span><span class="sxs-lookup"><span data-stu-id="8645e-198">Sometimes, you may want toorun hello job under hello HPC cluster user (for a domain-joined HPC cluster, run as one domain user; for a non-domain-joined HPC cluster, run as one local user on hello head node).</span></span>

1. <span data-ttu-id="8645e-199">Använd följande kommandon tooset hello autentiseringsuppgifter hello:</span><span class="sxs-lookup"><span data-stu-id="8645e-199">Use hello following commands tooset hello credentials:</span></span>

    ```powershell
    $localUser = “<username>”

    $localUserPassword=”<password>”

    $secpasswd = ConvertTo-SecureString $localUserPassword -AsPlainText -Force

    $mycreds = New-Object System.Management.Automation.PSCredential ($localUser, $secpasswd)

    Set-HpcJobCredential -Credential $mycreds -Scheduler https://<Azure load balancer DNS name>
    ```

2. <span data-ttu-id="8645e-200">Sedan skicka hello jobb.</span><span class="sxs-lookup"><span data-stu-id="8645e-200">Then submit hello job as follows.</span></span> <span data-ttu-id="8645e-201">hello projektaktivitet körs under $localUser på hello compute-noder.</span><span class="sxs-lookup"><span data-stu-id="8645e-201">hello job/task runs under $localUser on hello compute nodes.</span></span>

    ```powershell
    $emptycreds = New-Object System.Management.Automation.PSCredential ($localUser, (new-object System.Security.SecureString))
    ...
    $job = New-HpcJob –Scheduler https://<Azure load balancer DNS name>

    Add-HpcTask -Job $job -CommandLine "ping localhost" -Scheduler https://<Azure load balancer DNS name>

    Submit-HpcJob -Job $job -Scheduler https://<Azure load balancer DNS name> -Credential $emptycreds
    ```
    
   <span data-ttu-id="8645e-202">Om `–Credential` inte anges med `Submit-HpcJob`, hello jobbet körs under en lokal mappade användare som hello Azure AD-kontot.</span><span class="sxs-lookup"><span data-stu-id="8645e-202">If `–Credential` is not specified with `Submit-HpcJob`, hello job or task runs under a local mapped user as hello Azure AD account.</span></span> <span data-ttu-id="8645e-203">(hello HPC-kluster skapas en lokal användare med hello samma namn som hello Azure AD-kontot toorun hello aktivitet.)</span><span class="sxs-lookup"><span data-stu-id="8645e-203">(hello HPC cluster creates a local user with hello same name as hello Azure AD account toorun hello task.)</span></span>
    
3. <span data-ttu-id="8645e-204">Ange utökade data för hello Azure AD-kontot.</span><span class="sxs-lookup"><span data-stu-id="8645e-204">Set extended data for hello Azure AD account.</span></span> <span data-ttu-id="8645e-205">Detta är användbart när du kör ett MPI-jobb på Linux-noder med hello Azure AD-konto.</span><span class="sxs-lookup"><span data-stu-id="8645e-205">This is useful when running an MPI job on Linux nodes using hello Azure AD account.</span></span>

   * <span data-ttu-id="8645e-206">Ange utökade data för hello Azure AD-kontot sig själv</span><span class="sxs-lookup"><span data-stu-id="8645e-206">Set extended data for hello Azure AD account itself</span></span>

      ```powershell
      Set-HpcJobCredential -Scheduler https://<Azure load balancer DNS name> -ExtendedData <data> -AadUser
      ```
      
   * <span data-ttu-id="8645e-207">Ange utökade data och köra som användaren för HPC-kluster</span><span class="sxs-lookup"><span data-stu-id="8645e-207">Set extended data and run as HPC cluster user</span></span>
   
      ```powershell
      Set-HpcJobCredential -Credential $mycreds -Scheduler https://<Azure load balancer DNS name> -ExtendedData <data>
      ```

