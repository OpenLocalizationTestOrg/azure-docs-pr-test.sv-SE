---
title: 'Azure Active Directory Domain Services: Distribuera Azure Active Directory Application Proxy | Microsoft Docs'
description: "Använda Azure AD Application Proxy på Azure Active Directory Domain Services hanterade domäner"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: c158c67a82e12501386179e19bc75fd852d7e308
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-azure-ad-application-proxy-on-an-azure-ad-domain-services-managed-domain"></a><span data-ttu-id="5d4f0-103">Distribuera Azure AD Application Proxy på en Azure AD Domain Services-hanterad domän</span><span class="sxs-lookup"><span data-stu-id="5d4f0-103">Deploy Azure AD Application Proxy on an Azure AD Domain Services managed domain</span></span>
<span data-ttu-id="5d4f0-104">Azure Active Directory (AD) Application Proxy hjälper dig att stöd för fjärranvändare genom att publicera lokala program som kan nås via internet.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-104">Azure Active Directory (AD) Application Proxy helps you support remote workers by publishing on-premises applications to be accessed over the internet.</span></span> <span data-ttu-id="5d4f0-105">Med Azure AD Domain Services kan du nu lift-och-SKIFT äldre program som körs lokalt Azure Infrastructure Services.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-105">With Azure AD Domain Services, you can now lift-and-shift legacy applications running on-premises to Azure Infrastructure Services.</span></span> <span data-ttu-id="5d4f0-106">Du kan sedan publicera dessa program med Azure AD Application Proxy för att tillhandahålla säker fjärråtkomst till användare i din organisation.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-106">You can then publish these applications using the Azure AD Application Proxy, to provide secure remote access to users in your organization.</span></span>

<span data-ttu-id="5d4f0-107">Om du har använt Azure AD Application Proxy, mer information om den här funktionen med följande artikel: [ge säker fjärråtkomst till lokala program](../active-directory/active-directory-application-proxy-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="5d4f0-107">If you're new to the Azure AD Application Proxy, learn more about this feature with the following article: [How to provide secure remote access to on-premises applications](../active-directory/active-directory-application-proxy-get-started.md).</span></span>


## <a name="before-you-begin"></a><span data-ttu-id="5d4f0-108">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="5d4f0-108">Before you begin</span></span>
<span data-ttu-id="5d4f0-109">Om du vill utföra åtgärderna i den här artikeln behöver du:</span><span class="sxs-lookup"><span data-stu-id="5d4f0-109">To perform the tasks listed in this article, you need:</span></span>

1. <span data-ttu-id="5d4f0-110">En giltig **Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-110">A valid **Azure subscription**.</span></span>
2. <span data-ttu-id="5d4f0-111">En **Azure AD-katalog** -antingen synkroniseras med en lokal katalog eller en molnbaserad katalog.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-111">An **Azure AD directory** - either synchronized with an on-premises directory or a cloud-only directory.</span></span>
3. <span data-ttu-id="5d4f0-112">En **Azure AD Basic eller Premium-licens** krävs för att använda Azure AD Application Proxy.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-112">An **Azure AD Basic or Premium license** is required to use the Azure AD Application Proxy.</span></span>
4. <span data-ttu-id="5d4f0-113">**Azure AD Domain Services** måste vara aktiverat för Azure AD-katalog.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-113">**Azure AD Domain Services** must be enabled for the Azure AD directory.</span></span> <span data-ttu-id="5d4f0-114">Om du inte gjort det, följer du de uppgifter som beskrivs i den [Kom igång-guiden](active-directory-ds-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="5d4f0-114">If you haven't done so, follow all the tasks outlined in the [Getting Started guide](active-directory-ds-getting-started.md).</span></span>

<br>

## <a name="task-1---enable-azure-ad-application-proxy-for-your-azure-ad-directory"></a><span data-ttu-id="5d4f0-115">Uppgift 1 – Aktivera Azure AD Application Proxy för din Azure AD-katalog</span><span class="sxs-lookup"><span data-stu-id="5d4f0-115">Task 1 - Enable Azure AD Application Proxy for your Azure AD directory</span></span>
<span data-ttu-id="5d4f0-116">Utför följande steg om du vill aktivera Azure AD Application Proxy för Azure AD-katalogen.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-116">Perform the following steps to enable the Azure AD Application Proxy for your Azure AD directory.</span></span>

1. <span data-ttu-id="5d4f0-117">Logga in som administratör i den [Azure-portalen](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="5d4f0-117">Sign in as an administrator in the [Azure portal](http://portal.azure.com).</span></span>

2. <span data-ttu-id="5d4f0-118">Klicka på **Azure Active Directory** så att översikt över directory.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-118">Click **Azure Active Directory** to bring up the directory overview.</span></span> <span data-ttu-id="5d4f0-119">Klicka på **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-119">Click **Enterprise applications**.</span></span>

    ![Välj Azure AD-katalog](./media/app-proxy/app-proxy-enable-start.png)
3. <span data-ttu-id="5d4f0-121">Klicka på **programproxy**.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-121">Click **Application proxy**.</span></span> <span data-ttu-id="5d4f0-122">Om du inte har en Azure AD Basic eller Azure AD Premium-prenumeration kan se du ett alternativ för att aktivera en utvärderingsversion.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-122">If you do not have an Azure AD Basic or Azure AD Premium subscription, you see an option to enable a trial.</span></span> <span data-ttu-id="5d4f0-123">Växla **aktivera Application Proxy?** till **aktivera** och på **spara**.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-123">Toggle **Enable Application Proxy?** to **Enable** and click **Save**.</span></span>

    ![Aktivera appen Proxy](./media/app-proxy/app-proxy-enable-proxy-blade.png)
4. <span data-ttu-id="5d4f0-125">Hämta kopplingen, klicka på den **Connector** knappen.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-125">To download the connector, click the **Connector** button.</span></span>

    ![Hämta connector](./media/app-proxy/app-proxy-enabled-download-connector.png)
5. <span data-ttu-id="5d4f0-127">Acceptera licensvillkoren och sekretesspolicyn avtalet på hämtningssidan, och klicka på den **hämta** knappen.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-127">On the download page, accept the license terms and privacy agreement and click the **Download** button.</span></span>

    ![Bekräfta hämtning](./media/app-proxy/app-proxy-enabled-confirm-download.png)


## <a name="task-2---provision-domain-joined-windows-servers-to-deploy-the-azure-ad-application-proxy-connector"></a><span data-ttu-id="5d4f0-129">Uppgift 2 – etablera domänanslutna Windows-servrar för att distribuera Azure AD Application Proxy connector</span><span class="sxs-lookup"><span data-stu-id="5d4f0-129">Task 2 - Provision domain-joined Windows servers to deploy the Azure AD Application Proxy connector</span></span>
<span data-ttu-id="5d4f0-130">Du behöver domänanslutna Windows Server virtuella datorer som du installerar Azure AD Application Proxy connector.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-130">You need domain-joined Windows Server virtual machines on which you can install the Azure AD Application Proxy connector.</span></span> <span data-ttu-id="5d4f0-131">Beroende på vilka program som publiceras, kan du välja att etablera flera servrar där kopplingen är installerad.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-131">Depending on the applications being published, you may choose to provision multiple servers on which the connector is installed.</span></span> <span data-ttu-id="5d4f0-132">Det här distributionsalternativet ger dig större tillgänglighet och hjälper dig att hantera större belastningar för autentisering.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-132">This deployment option gives you greater availability and helps handle heavier authentication loads.</span></span>

<span data-ttu-id="5d4f0-133">Etablera anslutningstjänstservrarna på samma virtuella nätverk (eller ett virtuellt nätverk anslutet/peerkoppla) som du har aktiverat din Azure AD Domain Services-hanterad domän.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-133">Provision the connector servers on the same virtual network (or a connected/peered virtual network), in which you have enabled your Azure AD Domain Services managed domain.</span></span> <span data-ttu-id="5d4f0-134">På liknande sätt kan måste de servrar som hyser program som du publicerar via Application Proxy installeras på samma virtuella Azure-nätverket.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-134">Similarly, the servers hosting the applications you publish via the Application Proxy need to be installed on the same Azure virtual network.</span></span>

<span data-ttu-id="5d4f0-135">Följ de uppgifter som beskrivs i artikel med rubriken för att etablera anslutningsservrar [Anslut en Windows-dator till en hanterad domän](active-directory-ds-admin-guide-join-windows-vm.md).</span><span class="sxs-lookup"><span data-stu-id="5d4f0-135">To provision connector servers, follow the tasks outlined in the article titled [Join a Windows virtual machine to a managed domain](active-directory-ds-admin-guide-join-windows-vm.md).</span></span>


## <a name="task-3---install-and-register-the-azure-ad-application-proxy-connector"></a><span data-ttu-id="5d4f0-136">Uppgift 3 – Installera och registrera Azure AD Application Proxy Connector</span><span class="sxs-lookup"><span data-stu-id="5d4f0-136">Task 3 - Install and register the Azure AD Application Proxy Connector</span></span>
<span data-ttu-id="5d4f0-137">Tidigare etableras en virtuell dator med Windows Server och anslutna till den hanterade domänen.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-137">Previously, you provisioned a Windows Server virtual machine and joined it to the managed domain.</span></span> <span data-ttu-id="5d4f0-138">I det här steget ska du installera Azure AD Application Proxy connector på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-138">In this task, you will install the Azure AD Application Proxy connector on this virtual machine.</span></span>

1. <span data-ttu-id="5d4f0-139">Kopiera installationspaketet anslutningen till den virtuella datorn som du installerar Azure AD Web Application Proxy connector.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-139">Copy the connector installation package to the VM on which you install the Azure AD Web Application Proxy connector.</span></span>

2. <span data-ttu-id="5d4f0-140">Kör **AADApplicationProxyConnectorInstaller.exe** på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-140">Run **AADApplicationProxyConnectorInstaller.exe** on the virtual machine.</span></span> <span data-ttu-id="5d4f0-141">Acceptera licensvillkoren för programvara.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-141">Accept the software license terms.</span></span>

    ![Acceptera villkoren för installation](./media/app-proxy/app-proxy-install-connector-terms.png)
3. <span data-ttu-id="5d4f0-143">Under installationen uppmanas du att registrera anslutningsverktyget med programproxyn för din Azure AD-katalog.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-143">During installation, you are prompted to register the connector with the Application Proxy of your Azure AD directory.</span></span>
    * <span data-ttu-id="5d4f0-144">Ange din **autentiseringsuppgifter för global administratör för Azure AD**.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-144">Provide your **Azure AD global administrator credentials**.</span></span> <span data-ttu-id="5d4f0-145">Autentiseringsuppgifterna för klienten som du är global administratör för kan skilja sig från dina Microsoft Azure-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-145">Your global administrator tenant may be different from your Microsoft Azure credentials.</span></span>
    * <span data-ttu-id="5d4f0-146">Administratörskontot som används för att registrera anslutningsverktyget måste tillhöra samma katalog som du har aktiverat tjänsten Application Proxy.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-146">The administrator account used to register the connector must belong to the same directory where you enabled the Application Proxy service.</span></span> <span data-ttu-id="5d4f0-147">Till exempel om klientdomänen är contoso.com, administratören vara admin@contoso.com eller ett annat giltigt alias för domänen.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-147">For example, if the tenant domain is contoso.com, the admin should be admin@contoso.com or any other valid alias on that domain.</span></span>
    * <span data-ttu-id="5d4f0-148">Om Förbättrad säkerhetskonfiguration är aktiverad för servern där du installerar anslutningen kan registreringsskärmen blockeras.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-148">If IE Enhanced Security Configuration is turned on for the server where you are installing the connector, the registration screen might be blocked.</span></span> <span data-ttu-id="5d4f0-149">Följ instruktionerna i felmeddelandet för att tillåta åtkomst.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-149">To allow access, follow the instructions in the error message.</span></span> <span data-ttu-id="5d4f0-150">Kontrollera att Förbättrad säkerhetskonfiguration i Internet Explorer är inaktiverat.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-150">Make sure that Internet Explorer Enhanced Security is off.</span></span>
    * <span data-ttu-id="5d4f0-151">Om registreringen av anslutningsappen inte lyckas så gå till [Felsöka programproxyn](../active-directory/active-directory-application-proxy-troubleshoot.md).</span><span class="sxs-lookup"><span data-stu-id="5d4f0-151">If connector registration does not succeed, see [Troubleshoot Application Proxy](../active-directory/active-directory-application-proxy-troubleshoot.md).</span></span>

    ![Anslutning är installerad](./media/app-proxy/app-proxy-connector-installed.png)
4. <span data-ttu-id="5d4f0-153">Om du vill kontrollera anslutningen kör fungerar korrekt, Azure AD Application Proxy Connector felsökaren.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-153">To ensure the connector works properly, run the Azure AD Application Proxy Connector Troubleshooter.</span></span> <span data-ttu-id="5d4f0-154">Du bör se en lyckad rapport efter felsökaren.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-154">You should see a successful report after running the troubleshooter.</span></span>

    ![Felsökaren lyckades](./media/app-proxy/app-proxy-connector-troubleshooter.png)
5. <span data-ttu-id="5d4f0-156">Du bör se nyligen installerade koppling som visas på sidan Application proxy i Azure AD-katalogen.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-156">You should see the newly installed connector listed on the Application proxy page in your Azure AD directory.</span></span>

    ![](./media/app-proxy/app-proxy-connector-page.png)

> [!NOTE]
> <span data-ttu-id="5d4f0-157">Du kan välja att installera kopplingar på flera servrar för att garantera hög tillgänglighet för att autentisera program som publicerats via Azure AD Application Proxy.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-157">You may choose to install connectors on multiple servers to guarantee high availability for authenticating applications published through the Azure AD Application Proxy.</span></span> <span data-ttu-id="5d4f0-158">Utför stegen ovan för att installera anslutningen på andra servrar som är anslutna till din hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-158">Perform the same steps listed above to install the connector on other servers joined to your managed domain.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="5d4f0-159">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5d4f0-159">Next Steps</span></span>
<span data-ttu-id="5d4f0-160">Du har konfigurerat Azure AD Application Proxy och integreras med din Azure AD Domain Services-hanterad domän.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-160">You have set up the Azure AD Application Proxy and integrated it with your Azure AD Domain Services managed domain.</span></span>

* <span data-ttu-id="5d4f0-161">**Migrera dina program på virtuella Azure-datorer:** kan du lift-och-SKIFT dina program från lokala servrar till Azure virtuella datorer som är anslutna till din hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-161">**Migrate your applications to Azure virtual machines:** You can lift-and-shift your applications from on-premises servers to Azure virtual machines joined to your managed domain.</span></span> <span data-ttu-id="5d4f0-162">Detta hjälper dig att ta bort infrastrukturkostnader körs lokalt.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-162">Doing so helps you get rid of the infrastructure costs of running servers on-premises.</span></span>

* <span data-ttu-id="5d4f0-163">**Publicera program med Azure AD Application Proxy:** publicera program som körs på din virtuella Azure-datorer med hjälp av Azure AD Application Proxy.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-163">**Publish applications using Azure AD Application Proxy:** Publish applications running on your Azure virtual machines using the Azure AD Application Proxy.</span></span> <span data-ttu-id="5d4f0-164">Mer information finns i [publicera program med Azure AD Application Proxy](../active-directory/application-proxy-publish-azure-portal.md)</span><span class="sxs-lookup"><span data-stu-id="5d4f0-164">For more information, see [publish applications using Azure AD Application Proxy](../active-directory/application-proxy-publish-azure-portal.md)</span></span>


## <a name="deployment-note---publish-iwa-integrated-windows-authentication-applications-using-azure-ad-application-proxy"></a><span data-ttu-id="5d4f0-165">Obs distribution – publicera IWA (integrerad Windows-autentisering)-program med hjälp av Azure AD Application Proxy</span><span class="sxs-lookup"><span data-stu-id="5d4f0-165">Deployment note - Publish IWA (Integrated Windows Authentication) applications using Azure AD Application Proxy</span></span>
<span data-ttu-id="5d4f0-166">Aktivera enkel inloggning till dina program med integrerad autentisering IWA (Windows) genom att ge Application Proxy Connectors behörighet att personifiera användare, och skicka och ta emot token åt.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-166">Enable single sign-on to your applications using Integrated Windows Authentication (IWA) by granting Application Proxy Connectors permission to impersonate users, and send and receive tokens on their behalf.</span></span> <span data-ttu-id="5d4f0-167">Konfigurera kerberos-begränsad delegering (KCD) att bevilja behörigheterna som krävs för åtkomst till resurser på den hanterade domänen-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-167">Configure kerberos constrained delegation (KCD) for the connector to grant the required permissions to access resources on the managed domain.</span></span> <span data-ttu-id="5d4f0-168">Använda mekanismen resursbaserade KCD på hanterade domäner för ökad säkerhet.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-168">Use the resource-based KCD mechanism on managed domains for increased security.</span></span>


### <a name="enable-resource-based-kerberos-constrained-delegation-for-the-azure-ad-application-proxy-connector"></a><span data-ttu-id="5d4f0-169">Aktivera resursbaserade kerberos-begränsad delegering för Azure AD Application Proxy connector</span><span class="sxs-lookup"><span data-stu-id="5d4f0-169">Enable resource-based kerberos constrained delegation for the Azure AD Application Proxy connector</span></span>
<span data-ttu-id="5d4f0-170">Azure Application Proxy connector ska konfigureras för kerberos-begränsad delegering (KCD), så den kan personifiera användare på den hanterade domänen.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-170">The Azure Application Proxy connector should be configured for kerberos constrained delegation (KCD), so it can impersonate users on the managed domain.</span></span> <span data-ttu-id="5d4f0-171">På en hanterad domän i Azure AD Domain Services har inte administratörsbehörighet för domänen.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-171">On an Azure AD Domain Services managed domain, you do not have domain administrator privileges.</span></span> <span data-ttu-id="5d4f0-172">Därför **traditionella kontonivå KCD kan inte konfigureras på en hanterad domän**.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-172">Therefore, **traditional account-level KCD cannot be configured on a managed domain**.</span></span>

<span data-ttu-id="5d4f0-173">Använda resursbaserade KCD som beskrivs i det här [artikel](active-directory-ds-enable-kcd.md).</span><span class="sxs-lookup"><span data-stu-id="5d4f0-173">Use resource-based KCD as described in this [article](active-directory-ds-enable-kcd.md).</span></span>

> [!NOTE]
> <span data-ttu-id="5d4f0-174">Du måste vara medlem i gruppen AAD DC-administratörer för att administrera hanterade domänen med AD PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-174">You need to be a member of the 'AAD DC Administrators' group, to administer the managed domain using AD PowerShell cmdlets.</span></span>
>
>

<span data-ttu-id="5d4f0-175">Använd Get-ADComputer PowerShell-cmdlet för att hämta inställningarna för den dator där Azure AD Application Proxy connector är installerad.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-175">Use the Get-ADComputer PowerShell cmdlet to retrieve the settings for the computer on which the Azure AD Application Proxy connector is installed.</span></span>
```
$ConnectorComputerAccount = Get-ADComputer -Identity contoso100-proxy.contoso100.com
```

<span data-ttu-id="5d4f0-176">Därefter, Använd cmdlet Set-ADComputer att ställa in resursbaserade KCD för resursservern.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-176">Thereafter, use the Set-ADComputer cmdlet to set up resource-based KCD for the resource server.</span></span>
```
Set-ADComputer contoso100-resource.contoso100.com -PrincipalsAllowedToDelegateToAccount $ConnectorComputerAccount
```

<span data-ttu-id="5d4f0-177">Om du har distribuerat flera Application Proxy-kopplingar på din hanterade domän, måste du konfigurera resursbaserade KCD för varje sådan connector-instans.</span><span class="sxs-lookup"><span data-stu-id="5d4f0-177">If you have deployed multiple Application Proxy connectors on your managed domain, you need to configure resource-based KCD for each such connector instance.</span></span>


## <a name="related-content"></a><span data-ttu-id="5d4f0-178">Relaterat innehåll</span><span class="sxs-lookup"><span data-stu-id="5d4f0-178">Related Content</span></span>
* [<span data-ttu-id="5d4f0-179">Azure AD Domain Services - komma igång-guide</span><span class="sxs-lookup"><span data-stu-id="5d4f0-179">Azure AD Domain Services - Getting Started guide</span></span>](active-directory-ds-getting-started.md)
* [<span data-ttu-id="5d4f0-180">Konfigurera Kerberos-begränsad delegering på en hanterad domän</span><span class="sxs-lookup"><span data-stu-id="5d4f0-180">Configure Kerberos Constrained Delegation on a managed domain</span></span>](active-directory-ds-enable-kcd.md)
* [<span data-ttu-id="5d4f0-181">Kerberos-begränsad delegering: översikt</span><span class="sxs-lookup"><span data-stu-id="5d4f0-181">Kerberos Constrained Delegation Overview</span></span>](https://technet.microsoft.com/library/jj553400.aspx)
