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
ms.openlocfilehash: 4142111231d0256960d0c02d686d51533ba2171c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-azure-ad-application-proxy-on-an-azure-ad-domain-services-managed-domain"></a><span data-ttu-id="9f9e4-103">Distribuera Azure AD Application Proxy på en Azure AD Domain Services-hanterad domän</span><span class="sxs-lookup"><span data-stu-id="9f9e4-103">Deploy Azure AD Application Proxy on an Azure AD Domain Services managed domain</span></span>
<span data-ttu-id="9f9e4-104">Azure Active Directory (AD) Application Proxy hjälper dig att stöd för fjärranvändare genom att publicera lokala program toobe nås via hello internet.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-104">Azure Active Directory (AD) Application Proxy helps you support remote workers by publishing on-premises applications toobe accessed over hello internet.</span></span> <span data-ttu-id="9f9e4-105">Med Azure AD Domain Services kan du nu lift-och-SKIFT äldre program som körs lokalt tooAzure infrastrukturtjänster.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-105">With Azure AD Domain Services, you can now lift-and-shift legacy applications running on-premises tooAzure Infrastructure Services.</span></span> <span data-ttu-id="9f9e4-106">Du kan sedan publicera dessa program med hjälp av hello Azure AD Application Proxy, tooprovide säker fjärråtkomst toousers i din organisation.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-106">You can then publish these applications using hello Azure AD Application Proxy, tooprovide secure remote access toousers in your organization.</span></span>

<span data-ttu-id="9f9e4-107">Om du är ny toohello Azure AD Application Proxy kan lära dig mer om den här funktionen hello följande artikel: [hur säkra tooprovide fjärråtkomst tooon lokala program](../active-directory/active-directory-application-proxy-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="9f9e4-107">If you're new toohello Azure AD Application Proxy, learn more about this feature with hello following article: [How tooprovide secure remote access tooon-premises applications](../active-directory/active-directory-application-proxy-get-started.md).</span></span>


## <a name="before-you-begin"></a><span data-ttu-id="9f9e4-108">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="9f9e4-108">Before you begin</span></span>
<span data-ttu-id="9f9e4-109">tooperform hello uppgifterna som listas i den här artikeln, behöver du:</span><span class="sxs-lookup"><span data-stu-id="9f9e4-109">tooperform hello tasks listed in this article, you need:</span></span>

1. <span data-ttu-id="9f9e4-110">En giltig **Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-110">A valid **Azure subscription**.</span></span>
2. <span data-ttu-id="9f9e4-111">En **Azure AD-katalog** -antingen synkroniseras med en lokal katalog eller en molnbaserad katalog.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-111">An **Azure AD directory** - either synchronized with an on-premises directory or a cloud-only directory.</span></span>
3. <span data-ttu-id="9f9e4-112">En **Azure AD Basic eller Premium-licens** är obligatoriska toouse hello Azure AD Application Proxy.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-112">An **Azure AD Basic or Premium license** is required toouse hello Azure AD Application Proxy.</span></span>
4. <span data-ttu-id="9f9e4-113">**Azure AD Domain Services** måste vara aktiverat för hello Azure AD-katalogen.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-113">**Azure AD Domain Services** must be enabled for hello Azure AD directory.</span></span> <span data-ttu-id="9f9e4-114">Om du inte gjort det, följ alla hello uppgifter som beskrivs i hello [Kom igång-guiden](active-directory-ds-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="9f9e4-114">If you haven't done so, follow all hello tasks outlined in hello [Getting Started guide](active-directory-ds-getting-started.md).</span></span>

<br>

## <a name="task-1---enable-azure-ad-application-proxy-for-your-azure-ad-directory"></a><span data-ttu-id="9f9e4-115">Uppgift 1 – Aktivera Azure AD Application Proxy för din Azure AD-katalog</span><span class="sxs-lookup"><span data-stu-id="9f9e4-115">Task 1 - Enable Azure AD Application Proxy for your Azure AD directory</span></span>
<span data-ttu-id="9f9e4-116">Utföra hello följande steg tooenable hello Azure AD Application Proxy för Azure AD-katalogen.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-116">Perform hello following steps tooenable hello Azure AD Application Proxy for your Azure AD directory.</span></span>

1. <span data-ttu-id="9f9e4-117">Logga in som administratör i hello [Azure-portalen](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="9f9e4-117">Sign in as an administrator in hello [Azure portal](http://portal.azure.com).</span></span>

2. <span data-ttu-id="9f9e4-118">Klicka på **Azure Active Directory** toobring in hello directory Översikt.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-118">Click **Azure Active Directory** toobring up hello directory overview.</span></span> <span data-ttu-id="9f9e4-119">Klicka på **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-119">Click **Enterprise applications**.</span></span>

    ![Välj Azure AD-katalog](./media/app-proxy/app-proxy-enable-start.png)
3. <span data-ttu-id="9f9e4-121">Klicka på **programproxy**.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-121">Click **Application proxy**.</span></span> <span data-ttu-id="9f9e4-122">Om du inte har en Azure AD Basic eller Azure AD Premium-prenumeration kan se du en alternativet tooenable en utvärderingsversion.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-122">If you do not have an Azure AD Basic or Azure AD Premium subscription, you see an option tooenable a trial.</span></span> <span data-ttu-id="9f9e4-123">Växla **aktivera Application Proxy?** för**aktivera** och på **spara**.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-123">Toggle **Enable Application Proxy?** too**Enable** and click **Save**.</span></span>

    ![Aktivera appen Proxy](./media/app-proxy/app-proxy-enable-proxy-blade.png)
4. <span data-ttu-id="9f9e4-125">toodownload Hej koppling, klickar du på hello **Connector** knappen.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-125">toodownload hello connector, click hello **Connector** button.</span></span>

    ![Hämta connector](./media/app-proxy/app-proxy-enabled-download-connector.png)
5. <span data-ttu-id="9f9e4-127">På hämtningssidan för hello accepterar hello licensvillkoren och sekretesspolicyn avtal och klickar på hello **hämta** knappen.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-127">On hello download page, accept hello license terms and privacy agreement and click hello **Download** button.</span></span>

    ![Bekräfta hämtning](./media/app-proxy/app-proxy-enabled-confirm-download.png)


## <a name="task-2---provision-domain-joined-windows-servers-toodeploy-hello-azure-ad-application-proxy-connector"></a><span data-ttu-id="9f9e4-129">Uppgift 2 – etablera domänanslutna Windows-servrar toodeploy hello Azure AD Application Proxy connector</span><span class="sxs-lookup"><span data-stu-id="9f9e4-129">Task 2 - Provision domain-joined Windows servers toodeploy hello Azure AD Application Proxy connector</span></span>
<span data-ttu-id="9f9e4-130">Du behöver domänanslutna Windows Server virtuella datorer som du kan installera hello Azure AD Application Proxy connector.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-130">You need domain-joined Windows Server virtual machines on which you can install hello Azure AD Application Proxy connector.</span></span> <span data-ttu-id="9f9e4-131">Du kan välja flera servrar som hello connector installeras tooprovision beroende på hello program som publiceras.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-131">Depending on hello applications being published, you may choose tooprovision multiple servers on which hello connector is installed.</span></span> <span data-ttu-id="9f9e4-132">Det här distributionsalternativet ger dig större tillgänglighet och hjälper dig att hantera större belastningar för autentisering.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-132">This deployment option gives you greater availability and helps handle heavier authentication loads.</span></span>

<span data-ttu-id="9f9e4-133">Etablera hello anslutningsservrar på hello samma virtuella nätverk (eller ett virtuellt nätverk anslutet/peerkoppla) i som du har aktiverat din Azure AD Domain Services-hanterad domän.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-133">Provision hello connector servers on hello same virtual network (or a connected/peered virtual network), in which you have enabled your Azure AD Domain Services managed domain.</span></span> <span data-ttu-id="9f9e4-134">På liknande sätt hello-servrar som hyser hello-program som du publicerar via hello Application Proxy måste toobe installerad på hello samma virtuella Azure-nätverket.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-134">Similarly, hello servers hosting hello applications you publish via hello Application Proxy need toobe installed on hello same Azure virtual network.</span></span>

<span data-ttu-id="9f9e4-135">tooprovision anslutningsservrar följer hello uppgifter som beskrivs i artikeln hello [ansluta till en hanterad domän för Windows virtuella tooa](active-directory-ds-admin-guide-join-windows-vm.md).</span><span class="sxs-lookup"><span data-stu-id="9f9e4-135">tooprovision connector servers, follow hello tasks outlined in hello article titled [Join a Windows virtual machine tooa managed domain](active-directory-ds-admin-guide-join-windows-vm.md).</span></span>


## <a name="task-3---install-and-register-hello-azure-ad-application-proxy-connector"></a><span data-ttu-id="9f9e4-136">Uppgift 3 – Installera och registrera hello Azure AD Application Proxy Connector</span><span class="sxs-lookup"><span data-stu-id="9f9e4-136">Task 3 - Install and register hello Azure AD Application Proxy Connector</span></span>
<span data-ttu-id="9f9e4-137">Tidigare etableras en virtuell dator med Windows Server och ansluten toohello hanterad domän.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-137">Previously, you provisioned a Windows Server virtual machine and joined it toohello managed domain.</span></span> <span data-ttu-id="9f9e4-138">I det här steget ska du installera hello Azure AD Application Proxy connector på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-138">In this task, you will install hello Azure AD Application Proxy connector on this virtual machine.</span></span>

1. <span data-ttu-id="9f9e4-139">Kopiera hello connector installationen paketet toohello VM som du installerar hello Azure AD Web Application Proxy connector.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-139">Copy hello connector installation package toohello VM on which you install hello Azure AD Web Application Proxy connector.</span></span>

2. <span data-ttu-id="9f9e4-140">Kör **AADApplicationProxyConnectorInstaller.exe** på hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-140">Run **AADApplicationProxyConnectorInstaller.exe** on hello virtual machine.</span></span> <span data-ttu-id="9f9e4-141">Acceptera licensvillkoren för programvara hello.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-141">Accept hello software license terms.</span></span>

    ![Acceptera villkoren för installation](./media/app-proxy/app-proxy-install-connector-terms.png)
3. <span data-ttu-id="9f9e4-143">Under installationen kan du ange tooregister hello koppling med hello programproxyn för din Azure AD-katalog.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-143">During installation, you are prompted tooregister hello connector with hello Application Proxy of your Azure AD directory.</span></span>
    * <span data-ttu-id="9f9e4-144">Ange din **autentiseringsuppgifter för global administratör för Azure AD**.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-144">Provide your **Azure AD global administrator credentials**.</span></span> <span data-ttu-id="9f9e4-145">Autentiseringsuppgifterna för klienten som du är global administratör för kan skilja sig från dina Microsoft Azure-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-145">Your global administrator tenant may be different from your Microsoft Azure credentials.</span></span>
    * <span data-ttu-id="9f9e4-146">Hej administratör konto som används för tooregister hello connector måste tillhöra toohello samma katalog som du har aktiverat hello Application Proxy-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-146">hello administrator account used tooregister hello connector must belong toohello same directory where you enabled hello Application Proxy service.</span></span> <span data-ttu-id="9f9e4-147">Till exempel om hello klientdomänen är contoso.com, Hej administratör vara admin@contoso.com eller ett annat giltigt alias för domänen.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-147">For example, if hello tenant domain is contoso.com, hello admin should be admin@contoso.com or any other valid alias on that domain.</span></span>
    * <span data-ttu-id="9f9e4-148">Om Förbättrad säkerhetskonfiguration är aktiverad för hello server där du installerar hello connector blockeras hello registreringsskärmen.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-148">If IE Enhanced Security Configuration is turned on for hello server where you are installing hello connector, hello registration screen might be blocked.</span></span> <span data-ttu-id="9f9e4-149">tooallow åtkomst, hello anvisningar i hello felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-149">tooallow access, follow hello instructions in hello error message.</span></span> <span data-ttu-id="9f9e4-150">Kontrollera att Förbättrad säkerhetskonfiguration i Internet Explorer är inaktiverat.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-150">Make sure that Internet Explorer Enhanced Security is off.</span></span>
    * <span data-ttu-id="9f9e4-151">Om registreringen av anslutningsappen inte lyckas så gå till [Felsöka programproxyn](../active-directory/active-directory-application-proxy-troubleshoot.md).</span><span class="sxs-lookup"><span data-stu-id="9f9e4-151">If connector registration does not succeed, see [Troubleshoot Application Proxy](../active-directory/active-directory-application-proxy-troubleshoot.md).</span></span>

    ![Anslutning är installerad](./media/app-proxy/app-proxy-connector-installed.png)
4. <span data-ttu-id="9f9e4-153">tooensure hello connector fungerar korrekt, kör hello Azure AD Application Proxy Connector felsökaren.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-153">tooensure hello connector works properly, run hello Azure AD Application Proxy Connector Troubleshooter.</span></span> <span data-ttu-id="9f9e4-154">Du bör se en lyckad rapport efter körs hello felsökaren.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-154">You should see a successful report after running hello troubleshooter.</span></span>

    ![Felsökaren lyckades](./media/app-proxy/app-proxy-connector-troubleshooter.png)
5. <span data-ttu-id="9f9e4-156">Du bör se hello nyligen installerade koppling som visas på sidan för hello Application proxy i Azure AD-katalogen.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-156">You should see hello newly installed connector listed on hello Application proxy page in your Azure AD directory.</span></span>

    ![](./media/app-proxy/app-proxy-connector-page.png)

> [!NOTE]
> <span data-ttu-id="9f9e4-157">Du kan välja tooinstall kopplingar på flera servrar tooguarantee hög tillgänglighet för att autentisera program som publicerats via hello Azure AD Application Proxy.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-157">You may choose tooinstall connectors on multiple servers tooguarantee high availability for authenticating applications published through hello Azure AD Application Proxy.</span></span> <span data-ttu-id="9f9e4-158">Utföra hello likadant som ovan tooinstall hello connector på andra servrar kopplade tooyour hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-158">Perform hello same steps listed above tooinstall hello connector on other servers joined tooyour managed domain.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="9f9e4-159">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9f9e4-159">Next Steps</span></span>
<span data-ttu-id="9f9e4-160">Du har skapat hello Azure AD Application Proxy och integreras med din Azure AD Domain Services-hanterad domän.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-160">You have set up hello Azure AD Application Proxy and integrated it with your Azure AD Domain Services managed domain.</span></span>

* <span data-ttu-id="9f9e4-161">**Migrera dina program tooAzure virtuella datorer:** kan du lift och SKIFT dina program från lokala servrar tooAzure virtuella datorer kopplade tooyour hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-161">**Migrate your applications tooAzure virtual machines:** You can lift-and-shift your applications from on-premises servers tooAzure virtual machines joined tooyour managed domain.</span></span> <span data-ttu-id="9f9e4-162">Detta hjälper dig att ta bort hello infrastrukturkostnader körs lokalt.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-162">Doing so helps you get rid of hello infrastructure costs of running servers on-premises.</span></span>

* <span data-ttu-id="9f9e4-163">**Publicera program med Azure AD Application Proxy:** publicera program som körs på din virtuella Azure-datorer med hjälp av hello Azure AD Application Proxy.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-163">**Publish applications using Azure AD Application Proxy:** Publish applications running on your Azure virtual machines using hello Azure AD Application Proxy.</span></span> <span data-ttu-id="9f9e4-164">Mer information finns i [publicera program med Azure AD Application Proxy](../active-directory/application-proxy-publish-azure-portal.md)</span><span class="sxs-lookup"><span data-stu-id="9f9e4-164">For more information, see [publish applications using Azure AD Application Proxy](../active-directory/application-proxy-publish-azure-portal.md)</span></span>


## <a name="deployment-note---publish-iwa-integrated-windows-authentication-applications-using-azure-ad-application-proxy"></a><span data-ttu-id="9f9e4-165">Obs distribution – publicera IWA (integrerad Windows-autentisering)-program med hjälp av Azure AD Application Proxy</span><span class="sxs-lookup"><span data-stu-id="9f9e4-165">Deployment note - Publish IWA (Integrated Windows Authentication) applications using Azure AD Application Proxy</span></span>
<span data-ttu-id="9f9e4-166">Aktivera enkel inloggning tooyour program med integrerad autentisering IWA (Windows) genom att ge Application Proxy Connectors behörighet tooimpersonate användare, och skicka och ta emot token åt.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-166">Enable single sign-on tooyour applications using Integrated Windows Authentication (IWA) by granting Application Proxy Connectors permission tooimpersonate users, and send and receive tokens on their behalf.</span></span> <span data-ttu-id="9f9e4-167">Konfigurera kerberos-begränsad delegering (KCD) för hello connector toogrant hello krävs behörigheter tooaccess resurser på hello-hanterad domän.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-167">Configure kerberos constrained delegation (KCD) for hello connector toogrant hello required permissions tooaccess resources on hello managed domain.</span></span> <span data-ttu-id="9f9e4-168">Använd hello resursbaserade KCD mekanism på hanterade domäner för ökad säkerhet.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-168">Use hello resource-based KCD mechanism on managed domains for increased security.</span></span>


### <a name="enable-resource-based-kerberos-constrained-delegation-for-hello-azure-ad-application-proxy-connector"></a><span data-ttu-id="9f9e4-169">Aktivera resursbaserade kerberos-begränsad delegering för hello Azure AD Application Proxy connector</span><span class="sxs-lookup"><span data-stu-id="9f9e4-169">Enable resource-based kerberos constrained delegation for hello Azure AD Application Proxy connector</span></span>
<span data-ttu-id="9f9e4-170">hello Azure Application Proxy connector ska konfigureras för kerberos-begränsad delegering (KCD), så den kan personifiera användare på hello-hanterad domän.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-170">hello Azure Application Proxy connector should be configured for kerberos constrained delegation (KCD), so it can impersonate users on hello managed domain.</span></span> <span data-ttu-id="9f9e4-171">På en hanterad domän i Azure AD Domain Services har inte administratörsbehörighet för domänen.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-171">On an Azure AD Domain Services managed domain, you do not have domain administrator privileges.</span></span> <span data-ttu-id="9f9e4-172">Därför **traditionella kontonivå KCD kan inte konfigureras på en hanterad domän**.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-172">Therefore, **traditional account-level KCD cannot be configured on a managed domain**.</span></span>

<span data-ttu-id="9f9e4-173">Använda resursbaserade KCD som beskrivs i det här [artikel](active-directory-ds-enable-kcd.md).</span><span class="sxs-lookup"><span data-stu-id="9f9e4-173">Use resource-based KCD as described in this [article](active-directory-ds-enable-kcd.md).</span></span>

> [!NOTE]
> <span data-ttu-id="9f9e4-174">Du behöver toobe medlem av hello ' AAD DC-administratörsgruppen tooadminister hello hanterade domänen med hjälp av AD PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-174">You need toobe a member of hello 'AAD DC Administrators' group, tooadminister hello managed domain using AD PowerShell cmdlets.</span></span>
>
>

<span data-ttu-id="9f9e4-175">Använd hello Get-ADComputer PowerShell cmdlet tooretrieve hello inställningarna för hello dator på vilken hello Azure AD Application Proxy connector installeras.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-175">Use hello Get-ADComputer PowerShell cmdlet tooretrieve hello settings for hello computer on which hello Azure AD Application Proxy connector is installed.</span></span>
```
$ConnectorComputerAccount = Get-ADComputer -Identity contoso100-proxy.contoso100.com
```

<span data-ttu-id="9f9e4-176">Använd därefter hello Set-ADComputer cmdlet tooset in resursbaserade KCD för hello resursservern.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-176">Thereafter, use hello Set-ADComputer cmdlet tooset up resource-based KCD for hello resource server.</span></span>
```
Set-ADComputer contoso100-resource.contoso100.com -PrincipalsAllowedToDelegateToAccount $ConnectorComputerAccount
```

<span data-ttu-id="9f9e4-177">Om du har distribuerat flera Application Proxy-kopplingar på din hanterade domän, behöver du tooconfigure resursbaserade KCD för varje sådan connector-instans.</span><span class="sxs-lookup"><span data-stu-id="9f9e4-177">If you have deployed multiple Application Proxy connectors on your managed domain, you need tooconfigure resource-based KCD for each such connector instance.</span></span>


## <a name="related-content"></a><span data-ttu-id="9f9e4-178">Relaterat innehåll</span><span class="sxs-lookup"><span data-stu-id="9f9e4-178">Related Content</span></span>
* [<span data-ttu-id="9f9e4-179">Azure AD Domain Services - komma igång-guide</span><span class="sxs-lookup"><span data-stu-id="9f9e4-179">Azure AD Domain Services - Getting Started guide</span></span>](active-directory-ds-getting-started.md)
* [<span data-ttu-id="9f9e4-180">Konfigurera Kerberos-begränsad delegering på en hanterad domän</span><span class="sxs-lookup"><span data-stu-id="9f9e4-180">Configure Kerberos Constrained Delegation on a managed domain</span></span>](active-directory-ds-enable-kcd.md)
* [<span data-ttu-id="9f9e4-181">Kerberos-begränsad delegering: översikt</span><span class="sxs-lookup"><span data-stu-id="9f9e4-181">Kerberos Constrained Delegation Overview</span></span>](https://technet.microsoft.com/library/jj553400.aspx)
