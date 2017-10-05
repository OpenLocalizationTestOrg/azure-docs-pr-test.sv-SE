---
title: Active Directory Federation Services-hantering och anpassning med Azure AD Connect | Microsoft Docs
description: "AD FS-hantering med Azure AD Connect och anpassning av AD FS-inloggning användarupplevelsen med Azure AD Connect och PowerShell."
keywords: "ADFS, ADFS, AD FS management, AAD Connect Connect, inloggning, AD FS anpassning, reparera förtroendet, O365, federation, förlitande part"
services: active-directory
documentationcenter: 
author: anandyadavmsft
manager: femila
editor: 
ms.assetid: 2593b6c6-dc3f-46ef-8e02-a8e2dc4e9fb9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 14f03542a6553c5bb697192828368ffe6b96441c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="manage-and-customize-active-directory-federation-services-by-using-azure-ad-connect"></a><span data-ttu-id="fc000-104">Hantera och anpassa Active Directory Federation Services med hjälp av Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="fc000-104">Manage and customize Active Directory Federation Services by using Azure AD Connect</span></span>
<span data-ttu-id="fc000-105">Den här artikeln beskriver hur du hanterar och anpassa Active Directory Federation Services (AD FS) med hjälp av Azure Active Directory (AD Azure) Anslut.</span><span class="sxs-lookup"><span data-stu-id="fc000-105">This article describes how to manage and customize Active Directory Federation Services (AD FS) by using Azure Active Directory (Azure AD) Connect.</span></span> <span data-ttu-id="fc000-106">Den omfattar också andra vanliga AD FS-uppgifter som du kan behöva göra för att slutföra konfigurationen av en AD FS-servergrupp.</span><span class="sxs-lookup"><span data-stu-id="fc000-106">It also includes other common AD FS tasks that you might need to do for a complete configuration of an AD FS farm.</span></span>

| <span data-ttu-id="fc000-107">Avsnitt</span><span class="sxs-lookup"><span data-stu-id="fc000-107">Topic</span></span> | <span data-ttu-id="fc000-108">Det täcker</span><span class="sxs-lookup"><span data-stu-id="fc000-108">What it covers</span></span> |
|:--- |:--- |
| <span data-ttu-id="fc000-109">**Hantera AD FS**</span><span class="sxs-lookup"><span data-stu-id="fc000-109">**Manage AD FS**</span></span> | |
| [<span data-ttu-id="fc000-110">Reparera förtroendet</span><span class="sxs-lookup"><span data-stu-id="fc000-110">Repair the trust</span></span>](#repairthetrust) |<span data-ttu-id="fc000-111">Så här reparerar federationsförtroende med Office 365.</span><span class="sxs-lookup"><span data-stu-id="fc000-111">How to repair the federation trust with Office 365.</span></span> |
| [<span data-ttu-id="fc000-112">Federera med Azure AD med hjälp av alternativa inloggnings-ID</span><span class="sxs-lookup"><span data-stu-id="fc000-112">Federate with Azure AD using alternate login ID </span></span>](#alternateid) | <span data-ttu-id="fc000-113">Konfigurera federation med hjälp av alternativa inloggnings-ID</span><span class="sxs-lookup"><span data-stu-id="fc000-113">Configure federation using alternate login ID</span></span>  |
| [<span data-ttu-id="fc000-114">Lägga till en AD FS-server</span><span class="sxs-lookup"><span data-stu-id="fc000-114">Add an AD FS server</span></span>](#addadfsserver) |<span data-ttu-id="fc000-115">Hur du expanderar en AD FS-servergrupp med en ytterligare AD FS-servern.</span><span class="sxs-lookup"><span data-stu-id="fc000-115">How to expand an AD FS farm with an additional AD FS server.</span></span> |
| [<span data-ttu-id="fc000-116">Lägga till en AD FS Web Application Proxy-server</span><span class="sxs-lookup"><span data-stu-id="fc000-116">Add an AD FS Web Application Proxy server</span></span>](#addwapserver) |<span data-ttu-id="fc000-117">Hur du expanderar en AD FS-servergrupp med ytterligare en Webbprogramproxy (WAP) server.</span><span class="sxs-lookup"><span data-stu-id="fc000-117">How to expand an AD FS farm with an additional Web Application Proxy (WAP) server.</span></span> |
| [<span data-ttu-id="fc000-118">Lägga till en federerad domän</span><span class="sxs-lookup"><span data-stu-id="fc000-118">Add a federated domain</span></span>](#addfeddomain) |<span data-ttu-id="fc000-119">Hur du lägger till en federerad domän.</span><span class="sxs-lookup"><span data-stu-id="fc000-119">How to add a federated domain.</span></span> |
| [<span data-ttu-id="fc000-120">Uppdatera SSL-certifikatet</span><span class="sxs-lookup"><span data-stu-id="fc000-120">Update the SSL certificate</span></span>](active-directory-aadconnectfed-ssl-update.md)| <span data-ttu-id="fc000-121">Så här uppdaterar SSL-certifikatet för en AD FS-servergrupp.</span><span class="sxs-lookup"><span data-stu-id="fc000-121">How to update the SSL certificate for an AD FS farm.</span></span> |
| <span data-ttu-id="fc000-122">**Anpassa AD FS**</span><span class="sxs-lookup"><span data-stu-id="fc000-122">**Customize AD FS**</span></span> | |
| [<span data-ttu-id="fc000-123">Lägga till en anpassad logotyp eller bild</span><span class="sxs-lookup"><span data-stu-id="fc000-123">Add a custom company logo or illustration</span></span>](#customlogo) |<span data-ttu-id="fc000-124">Hur du anpassar en AD FS-inloggningssida med företagets logotyp och illustration.</span><span class="sxs-lookup"><span data-stu-id="fc000-124">How to customize an AD FS sign-in page with a company logo and illustration.</span></span> |
| [<span data-ttu-id="fc000-125">Lägg till en beskrivning för inloggning</span><span class="sxs-lookup"><span data-stu-id="fc000-125">Add a sign-in description</span></span>](#addsignindescription) |<span data-ttu-id="fc000-126">Hur du lägger till en beskrivning på inloggningssidan.</span><span class="sxs-lookup"><span data-stu-id="fc000-126">How to add a sign-in page description.</span></span> |
| [<span data-ttu-id="fc000-127">Ändra anspråksregler i AD FS</span><span class="sxs-lookup"><span data-stu-id="fc000-127">Modify AD FS claim rules</span></span>](#modclaims) |<span data-ttu-id="fc000-128">Så här ändrar du AD FS-anspråk för olika federationsscenarier.</span><span class="sxs-lookup"><span data-stu-id="fc000-128">How to modify AD FS claims for various federation scenarios.</span></span> |

## <a name="manage-ad-fs"></a><span data-ttu-id="fc000-129">Hantera AD FS</span><span class="sxs-lookup"><span data-stu-id="fc000-129">Manage AD FS</span></span>
<span data-ttu-id="fc000-130">Du kan utföra olika AD FS-relaterade uppgifter i Azure AD Connect med minimal användaren med hjälp av Azure AD Connect-guiden.</span><span class="sxs-lookup"><span data-stu-id="fc000-130">You can perform various AD FS-related tasks in Azure AD Connect with minimal user intervention by using the Azure AD Connect wizard.</span></span> <span data-ttu-id="fc000-131">När du är klar installerar Azure AD Connect genom att köra guiden kan köra du guiden igen om du vill utföra ytterligare åtgärder.</span><span class="sxs-lookup"><span data-stu-id="fc000-131">After you've finished installing Azure AD Connect by running the wizard, you can run the wizard again to perform additional tasks.</span></span>

## <span data-ttu-id="fc000-132">Reparera förtroendet<a name=repairthetrust></a></span><span class="sxs-lookup"><span data-stu-id="fc000-132">Repair the trust <a name=repairthetrust></a></span></span>
<span data-ttu-id="fc000-133">Du kan använda Azure AD Connect för att kontrollera det aktuella hälsotillståndet för AD FS och Azure AD litar på och vidta lämpliga åtgärder att reparera förtroendet.</span><span class="sxs-lookup"><span data-stu-id="fc000-133">You can use Azure AD Connect to check the current health of the AD FS and Azure AD trust and take appropriate actions to repair the trust.</span></span> <span data-ttu-id="fc000-134">Följ dessa steg om du vill reparera din Azure AD och AD FS-förtroende.</span><span class="sxs-lookup"><span data-stu-id="fc000-134">Follow these steps to repair your Azure AD and AD FS trust.</span></span>

1. <span data-ttu-id="fc000-135">Välj **reparera AAD och litar på AD FS** från listan över ytterligare aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="fc000-135">Select **Repair AAD and ADFS Trust** from the list of additional tasks.</span></span>
   <span data-ttu-id="fc000-136">![Reparera AAD och ADFS förtroende](media/active-directory-aadconnect-federation-management/RepairADTrust1.PNG)</span><span class="sxs-lookup"><span data-stu-id="fc000-136">![Repair AAD and ADFS Trust](media/active-directory-aadconnect-federation-management/RepairADTrust1.PNG)</span></span>

2. <span data-ttu-id="fc000-137">På den **Anslut till Azure AD** anger dina autentiseringsuppgifter som global administratör för Azure AD, och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="fc000-137">On the **Connect to Azure AD** page, provide your global administrator credentials for Azure AD, and click **Next**.</span></span>
   <span data-ttu-id="fc000-138">![Anslut till Azure AD](media/active-directory-aadconnect-federation-management/RepairADTrust2.PNG)</span><span class="sxs-lookup"><span data-stu-id="fc000-138">![Connect to Azure AD](media/active-directory-aadconnect-federation-management/RepairADTrust2.PNG)</span></span>

3. <span data-ttu-id="fc000-139">På den **fjärråtkomst autentiseringsuppgifter** ange autentiseringsuppgifter för domänadministratör.</span><span class="sxs-lookup"><span data-stu-id="fc000-139">On the **Remote access credentials** page, enter the credentials for the domain administrator.</span></span>

   ![Autentiseringsuppgifter för fjärråtkomst](media/active-directory-aadconnect-federation-management/RepairADTrust3.PNG)

    <span data-ttu-id="fc000-141">När du klickar på **nästa**, Azure AD Connect söker efter certifikat hälsa och visar eventuella problem.</span><span class="sxs-lookup"><span data-stu-id="fc000-141">After you click **Next**, Azure AD Connect checks for certificate health and shows any issues.</span></span>

    ![Tillståndet för certifikat](media/active-directory-aadconnect-federation-management/RepairADTrust4.PNG)

    <span data-ttu-id="fc000-143">Den **redo att konfigurera** sidan innehåller en lista med åtgärder som kommer att utföras för att reparera förtroendet.</span><span class="sxs-lookup"><span data-stu-id="fc000-143">The **Ready to configure** page shows the list of actions that will be performed to repair the trust.</span></span>

    ![Klart att konfigurera](media/active-directory-aadconnect-federation-management/RepairADTrust5.PNG)

4. <span data-ttu-id="fc000-145">Klicka på **installera** att reparera förtroendet.</span><span class="sxs-lookup"><span data-stu-id="fc000-145">Click **Install** to repair the trust.</span></span>

> [!NOTE]
> <span data-ttu-id="fc000-146">Azure AD Connect kan bara reparera eller agera på självsignerade certifikat.</span><span class="sxs-lookup"><span data-stu-id="fc000-146">Azure AD Connect can only repair or act on certificates that are self-signed.</span></span> <span data-ttu-id="fc000-147">Azure AD Connect kan inte åtgärda certifikat från tredjepart.</span><span class="sxs-lookup"><span data-stu-id="fc000-147">Azure AD Connect can't repair third-party certificates.</span></span>

## <span data-ttu-id="fc000-148">Federera med Azure AD med hjälp av AlternateID<a name=alternateid></a></span><span class="sxs-lookup"><span data-stu-id="fc000-148">Federate with Azure AD using AlternateID <a name=alternateid></a></span></span>
<span data-ttu-id="fc000-149">Du rekommenderas att lokalt användarens huvudnamn Name(UPN) och molnet UPN-namnet blir desamma.</span><span class="sxs-lookup"><span data-stu-id="fc000-149">It is recommended that the on-premises User Principal Name(UPN) and the cloud User Principal Name are kept the same.</span></span> <span data-ttu-id="fc000-150">Om lokala UPN använder en icke-dirigerbara domän (t.ex.</span><span class="sxs-lookup"><span data-stu-id="fc000-150">If the on-premises UPN uses a non-routable domain (ex.</span></span> <span data-ttu-id="fc000-151">Contoso.local) eller kan inte ändras på grund av lokala programberoenden, rekommenderar vi att konfigurera alternativa inloggnings-ID.</span><span class="sxs-lookup"><span data-stu-id="fc000-151">Contoso.local) or cannot be changed due to local application dependencies, we recommend setting up alternate login ID.</span></span> <span data-ttu-id="fc000-152">Alternativt inloggnings-ID kan du konfigurera en inloggning där användare kan logga in med ett attribut än sina UPN-namnet, till exempel e-post.</span><span class="sxs-lookup"><span data-stu-id="fc000-152">Alternate login ID allows you to configure a sign-in experience where users can sign in with an attribute other than their UPN, such as mail.</span></span> <span data-ttu-id="fc000-153">Alternativ för UPN-namnet i Azure AD Connect som standard attributet userPrincipalName i Active Directory.</span><span class="sxs-lookup"><span data-stu-id="fc000-153">The choice for User Principal Name in Azure AD Connect defaults to the userPrincipalName attribute in Active Directory.</span></span> <span data-ttu-id="fc000-154">Om du väljer andra attribut för UPN-namnet och federerar med AD FS, sedan Azure AD Connect kommer att konfigurera AD FS alternativt inloggnings-ID</span><span class="sxs-lookup"><span data-stu-id="fc000-154">If you choose any other attribute for User Principal Name and are federating using AD FS, then Azure AD Connect will configure AD FS for alternate login ID.</span></span> <span data-ttu-id="fc000-155">Ett exempel på att välja ett annat attribut för UPN-namnet visas nedan:</span><span class="sxs-lookup"><span data-stu-id="fc000-155">An example of choosing a different attribute for User Principal Name is shown below:</span></span>

![Val av alternativa ID-attribut](media/active-directory-aadconnect-federation-management/attributeselection.png)

<span data-ttu-id="fc000-157">Konfigurera alternativt inloggnings-ID för AD FS består av två Huvudsteg:</span><span class="sxs-lookup"><span data-stu-id="fc000-157">Configuring alternate login ID for AD FS consists of two main steps:</span></span>
1. <span data-ttu-id="fc000-158">**Konfigurera rätt uppsättning utfärdande anspråk**: utfärdande anspråksregler i Azure AD förlitande part förtroende har ändrats för att använda det markerade attributet UserPrincipalName som alternativt ID för användaren.</span><span class="sxs-lookup"><span data-stu-id="fc000-158">**Configure the right set of issuance claims**: The issuance claim rules in the Azure AD relying party trust are modified to use the selected UserPrincipalName attribute as the alternate ID of the user.</span></span>
2. <span data-ttu-id="fc000-159">**Aktivera alternativa inloggnings-ID i AD FS-konfigurationen**: AD FS-konfigurationen har uppdaterats så att AD FS kan söka efter användare i lämplig skogar med alternativa-ID.</span><span class="sxs-lookup"><span data-stu-id="fc000-159">**Enable alternate login ID in the AD FS configuration**: The AD FS configuration is updated so that AD FS can look up users in the appropriate forests using the alternate ID.</span></span> <span data-ttu-id="fc000-160">Den här konfigurationen stöds för AD FS i Windows Server 2012 R2 (med KB2919355) eller senare.</span><span class="sxs-lookup"><span data-stu-id="fc000-160">This configuration is supported for AD FS on Windows Server 2012 R2 (with KB2919355) or later.</span></span> <span data-ttu-id="fc000-161">Om AD FS-servrar 2012 R2, kontrollerar Azure AD Connect förekomsten av de nödvändiga KB.</span><span class="sxs-lookup"><span data-stu-id="fc000-161">If the AD FS servers are 2012 R2, Azure AD Connect checks for the presence of the required KB.</span></span> <span data-ttu-id="fc000-162">Om KB identifieras, visas en varning när konfigurationen är klar, enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="fc000-162">If the KB is not detected, a warning will be displayed after configuration completes, as shown below:</span></span>

    ![Varning för saknas KB på 2012R2](media/active-directory-aadconnect-federation-management/kbwarning.png)

    <span data-ttu-id="fc000-164">Installera de nödvändiga för att åtgärda konfigurationen vid saknas KB [KB2919355](http://go.microsoft.com/fwlink/?LinkID=396590) och reparera sedan den betrodda med hjälp av [reparera AAD och AD FS-förtroende](#repairthetrust).</span><span class="sxs-lookup"><span data-stu-id="fc000-164">To rectify the configuration in case of missing KB, install the required [KB2919355](http://go.microsoft.com/fwlink/?LinkID=396590) and then repair the trust using [Repair AAD and AD FS Trust](#repairthetrust).</span></span>

> [!NOTE]
> <span data-ttu-id="fc000-165">Mer information om alternateID och steg för att manuellt konfigurera [konfigurera alternativa inloggnings-ID](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configuring-alternate-login-id)</span><span class="sxs-lookup"><span data-stu-id="fc000-165">For more information on alternateID and steps to manually configure, read [Configuring Alternate Login ID](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configuring-alternate-login-id)</span></span>

## <span data-ttu-id="fc000-166">Lägga till en AD FS-server<a name=addadfsserver></a></span><span class="sxs-lookup"><span data-stu-id="fc000-166">Add an AD FS server <a name=addadfsserver></a></span></span>

> [!NOTE]
> <span data-ttu-id="fc000-167">Om du vill lägga till en AD FS-server, kräver Azure AD Connect PFX-certifikat.</span><span class="sxs-lookup"><span data-stu-id="fc000-167">To add an AD FS server, Azure AD Connect requires the PFX certificate.</span></span> <span data-ttu-id="fc000-168">Därför kan utföra du den här åtgärden endast om du konfigurerat AD FS-servergrupp med hjälp av Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="fc000-168">Therefore, you can perform this operation only if you configured the AD FS farm by using Azure AD Connect.</span></span>

1. <span data-ttu-id="fc000-169">Välj **distribuera ytterligare en federationsserver**, och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="fc000-169">Select **Deploy an additional Federation Server**, and click **Next**.</span></span>

   ![Ytterligare federationsserver](media/active-directory-aadconnect-federation-management/AddNewADFSServer1.PNG)

2. <span data-ttu-id="fc000-171">På den **Anslut till Azure AD** , ange dina autentiseringsuppgifter som global administratör för Azure AD och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="fc000-171">On the **Connect to Azure AD** page, enter your global administrator credentials for Azure AD, and click **Next**.</span></span>

   ![Anslut till Azure AD](media/active-directory-aadconnect-federation-management/AddNewADFSServer2.PNG)

3. <span data-ttu-id="fc000-173">Ange administratörsautentiseringsuppgifter för domänen.</span><span class="sxs-lookup"><span data-stu-id="fc000-173">Provide the domain administrator credentials.</span></span>

   ![Domänadministratörsbehörighet](media/active-directory-aadconnect-federation-management/AddNewADFSServer3.PNG)

4. <span data-ttu-id="fc000-175">Azure AD Connect begär lösenordet för PFX-filen som du angav när du konfigurerar din nya AD FS-grupp med Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="fc000-175">Azure AD Connect asks for the password of the PFX file that you provided while configuring your new AD FS farm with Azure AD Connect.</span></span> <span data-ttu-id="fc000-176">Klicka på **ange lösenord för** att ange lösenord för PFX-fil.</span><span class="sxs-lookup"><span data-stu-id="fc000-176">Click **Enter Password** to provide the password for the PFX file.</span></span>

   ![Lösenord för certifikatet](media/active-directory-aadconnect-federation-management/AddNewADFSServer4.PNG)

    ![Ange SSL-certifikat](media/active-directory-aadconnect-federation-management/AddNewADFSServer5.PNG)

5. <span data-ttu-id="fc000-179">På den **AD FS-servrar** anger servernamnet eller IP-adress som ska läggas till i AD FS-servergruppen.</span><span class="sxs-lookup"><span data-stu-id="fc000-179">On the **AD FS Servers** page, enter the server name or IP address to be added to the AD FS farm.</span></span>

   ![AD FS-servrar](media/active-directory-aadconnect-federation-management/AddNewADFSServer6.PNG)

6. <span data-ttu-id="fc000-181">Klicka på **nästa**, och gå igenom slutliga **konfigurera** sidan.</span><span class="sxs-lookup"><span data-stu-id="fc000-181">Click **Next**, and go through the final **Configure** page.</span></span> <span data-ttu-id="fc000-182">När Azure AD Connect är klar att lägga till servrar i AD FS-servergruppen, får du alternativet för att verifiera anslutning.</span><span class="sxs-lookup"><span data-stu-id="fc000-182">After Azure AD Connect has finished adding the servers to the AD FS farm, you will be given the option to verify the connectivity.</span></span>

   ![Klart att konfigurera](media/active-directory-aadconnect-federation-management/AddNewADFSServer7.PNG)

    ![Installationen är klar](media/active-directory-aadconnect-federation-management/AddNewADFSServer8.PNG)

## <span data-ttu-id="fc000-185">Lägga till en AD FS WAP-server<a name=addwapserver></a></span><span class="sxs-lookup"><span data-stu-id="fc000-185">Add an AD FS WAP server <a name=addwapserver></a></span></span>

> [!NOTE]
> <span data-ttu-id="fc000-186">Om du vill lägga till en server för WAP, kräver Azure AD Connect PFX-certifikat.</span><span class="sxs-lookup"><span data-stu-id="fc000-186">To add a WAP server, Azure AD Connect requires the PFX certificate.</span></span> <span data-ttu-id="fc000-187">Du kan därför bara utföra den här åtgärden om du konfigurerat AD FS-servergrupp med hjälp av Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="fc000-187">Therefore, you can only perform this operation if you configured the AD FS farm by using Azure AD Connect.</span></span>

1. <span data-ttu-id="fc000-188">Välj **distribuera Webbprogramproxy** från listan över tillgängliga uppgifter.</span><span class="sxs-lookup"><span data-stu-id="fc000-188">Select **Deploy Web Application Proxy** from the list of available tasks.</span></span>

   ![Distribuera Webbprogramproxy](media/active-directory-aadconnect-federation-management/WapServer1.PNG)

2. <span data-ttu-id="fc000-190">Ange autentiseringsuppgifter för global administratör för Azure.</span><span class="sxs-lookup"><span data-stu-id="fc000-190">Provide the Azure global administrator credentials.</span></span>

   ![Anslut till Azure AD](media/active-directory-aadconnect-federation-management/wapserver2.PNG)

3. <span data-ttu-id="fc000-192">På den **ange SSL-certifikat** anger lösenordet för PFX-fil som du angav när du konfigurerat AD FS-servergruppen med Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="fc000-192">On the **Specify SSL certificate** page, provide the password for the PFX file that you provided when you configured the AD FS farm with Azure AD Connect.</span></span>
   <span data-ttu-id="fc000-193">![Lösenord för certifikatet](media/active-directory-aadconnect-federation-management/WapServer3.PNG)</span><span class="sxs-lookup"><span data-stu-id="fc000-193">![Certificate password](media/active-directory-aadconnect-federation-management/WapServer3.PNG)</span></span>

    ![Ange SSL-certifikat](media/active-directory-aadconnect-federation-management/WapServer4.PNG)

4. <span data-ttu-id="fc000-195">Lägg till servern som ska läggas till som en server för WAP.</span><span class="sxs-lookup"><span data-stu-id="fc000-195">Add the server to be added as a WAP server.</span></span> <span data-ttu-id="fc000-196">Eftersom WAP-servern inte kan anslutas till domänen, frågar guiden för administrativa autentiseringsuppgifter för servern som läggs till.</span><span class="sxs-lookup"><span data-stu-id="fc000-196">Because the WAP server might not be joined to the domain, the wizard asks for administrative credentials to the server being added.</span></span>

   ![Administrativa autentiseringsuppgifter](media/active-directory-aadconnect-federation-management/WapServer5.PNG)

5. <span data-ttu-id="fc000-198">På den **autentiseringsuppgifter för Proxy förtroende** ange administrativa autentiseringsuppgifter för att konfigurera proxyn förtroende och åtkomst till den primära servern i AD FS-servergruppen.</span><span class="sxs-lookup"><span data-stu-id="fc000-198">On the **Proxy trust credentials** page, provide administrative credentials to configure the proxy trust and access the primary server in the AD FS farm.</span></span>

   ![Autentiseringsuppgifter för proxy-förtroende](media/active-directory-aadconnect-federation-management/WapServer6.PNG)

6. <span data-ttu-id="fc000-200">På den **redo att konfigurera** sidan guiden visar en lista med åtgärder som kommer att utföras.</span><span class="sxs-lookup"><span data-stu-id="fc000-200">On the **Ready to configure** page, the wizard shows the list of actions that will be performed.</span></span>

   ![Klart att konfigurera](media/active-directory-aadconnect-federation-management/WapServer7.PNG)

7. <span data-ttu-id="fc000-202">Klicka på **installera** att slutföra konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="fc000-202">Click **Install** to finish the configuration.</span></span> <span data-ttu-id="fc000-203">När konfigurationen är klar, kan guiden välja att verifiera anslutning till servrar.</span><span class="sxs-lookup"><span data-stu-id="fc000-203">After the configuration is complete, the wizard gives you the option to verify the connectivity to the servers.</span></span> <span data-ttu-id="fc000-204">Klicka på **Kontrollera** vill kontrollera anslutningen.</span><span class="sxs-lookup"><span data-stu-id="fc000-204">Click **Verify** to check connectivity.</span></span>

   ![Installationen är klar](media/active-directory-aadconnect-federation-management/WapServer8.PNG)

## <span data-ttu-id="fc000-206">Lägga till en federerad domän<a name=addfeddomain></a></span><span class="sxs-lookup"><span data-stu-id="fc000-206">Add a federated domain <a name=addfeddomain></a></span></span>

<span data-ttu-id="fc000-207">Det är lätt att lägga till en domän att bli federerad med Azure AD med hjälp av Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="fc000-207">It's easy to add a domain to be federated with Azure AD by using Azure AD Connect.</span></span> <span data-ttu-id="fc000-208">Azure AD Connect lägger du till domänen för federation och ändrar anspråksregler för att återspegla utfärdaren korrekt när du har flera domäner federerade med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fc000-208">Azure AD Connect adds the domain for federation and modifies the claim rules to correctly reflect the issuer when you have multiple domains federated with Azure AD.</span></span>

1. <span data-ttu-id="fc000-209">Om du vill lägga till en federerad domän, väljer du uppgiften **Lägg till en Azure AD-domänen**.</span><span class="sxs-lookup"><span data-stu-id="fc000-209">To add a federated domain, select the task **Add an additional Azure AD domain**.</span></span>

   ![Ytterligare Azure AD-domän](media/active-directory-aadconnect-federation-management/AdditionalDomain1.PNG)

2. <span data-ttu-id="fc000-211">På nästa sida i guiden anger du autentiseringsuppgifterna som global administratör för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fc000-211">On the next page of the wizard, provide the global administrator credentials for Azure AD.</span></span>

   ![Anslut till Azure AD](media/active-directory-aadconnect-federation-management/AdditionalDomain2.PNG)

3. <span data-ttu-id="fc000-213">På den **fjärråtkomst autentiseringsuppgifter** ange administratörsbehörighet för domänen.</span><span class="sxs-lookup"><span data-stu-id="fc000-213">On the **Remote access credentials** page, provide the domain administrator credentials.</span></span>

   ![Autentiseringsuppgifter för fjärråtkomst](media/active-directory-aadconnect-federation-management/additionaldomain3.PNG)

4. <span data-ttu-id="fc000-215">Guiden innehåller en lista över Azure AD-domäner som du kan federera din lokala katalog med på nästa sida.</span><span class="sxs-lookup"><span data-stu-id="fc000-215">On the next page, the wizard provides a list of Azure AD domains that you can federate your on-premises directory with.</span></span> <span data-ttu-id="fc000-216">Välj domänen i listan.</span><span class="sxs-lookup"><span data-stu-id="fc000-216">Choose the domain from the list.</span></span>

   ![Azure AD-domän](media/active-directory-aadconnect-federation-management/AdditionalDomain4.PNG)

    <span data-ttu-id="fc000-218">När du väljer domänen, ger guiden lämplig information om ytterligare åtgärder som kommer att utföras i guiden och effekten av konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="fc000-218">After you choose the domain, the wizard provides you with appropriate information about further actions that the wizard will take and the impact of the configuration.</span></span> <span data-ttu-id="fc000-219">I vissa fall, om du väljer en domän som inte har verifierats i Azure AD finns guiden information som hjälper dig att verifiera domänen.</span><span class="sxs-lookup"><span data-stu-id="fc000-219">In some cases, if you select a domain that isn't yet verified in Azure AD, the wizard provides you with information to help you verify the domain.</span></span> <span data-ttu-id="fc000-220">Se [lägga till ett eget domännamn i Azure Active Directory](../active-directory-add-domain.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="fc000-220">See [Add your custom domain name to Azure Active Directory](../active-directory-add-domain.md) for more details.</span></span>

5. <span data-ttu-id="fc000-221">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="fc000-221">Click **Next**.</span></span> <span data-ttu-id="fc000-222">Den **redo att konfigurera** sidan innehåller en lista med åtgärder som kommer att utföras i Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="fc000-222">The **Ready to configure** page shows the list of actions that Azure AD Connect will perform.</span></span> <span data-ttu-id="fc000-223">Klicka på **installera** att slutföra konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="fc000-223">Click **Install** to finish the configuration.</span></span>

   ![Klart att konfigurera](media/active-directory-aadconnect-federation-management/AdditionalDomain5.PNG)

> [!NOTE]
> <span data-ttu-id="fc000-225">Användare från den tillagda federerade domänen måste synkroniseras innan de kan logga in på Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fc000-225">Users from the added federated domain must be synchronized before they will be able to login to Azure AD.</span></span>

## <a name="ad-fs-customization"></a><span data-ttu-id="fc000-226">AD FS-anpassning</span><span class="sxs-lookup"><span data-stu-id="fc000-226">AD FS customization</span></span>
<span data-ttu-id="fc000-227">Följande avsnitt innehåller information om några av de aktiviteter du kan behöva utföra när du anpassar din AD FS-inloggningssida.</span><span class="sxs-lookup"><span data-stu-id="fc000-227">The following sections provide details about some of the common tasks that you might have to perform when you customize your AD FS sign-in page.</span></span>

## <span data-ttu-id="fc000-228">Lägga till en anpassad logotyp eller bild<a name=customlogo></a></span><span class="sxs-lookup"><span data-stu-id="fc000-228">Add a custom company logo or illustration <a name=customlogo></a></span></span>
<span data-ttu-id="fc000-229">Ändra logotypen för det företag som visas på den **inloggning** använder följande Windows PowerShell-cmdlet och syntax.</span><span class="sxs-lookup"><span data-stu-id="fc000-229">To change the logo of the company that's displayed on the **Sign-in** page, use the following Windows PowerShell cmdlet and syntax.</span></span>

> [!NOTE]
> <span data-ttu-id="fc000-230">De rekommenderade måtten på logotypen är 260 x 35 96 dpi, samt med en filstorlek som är större än 10 KB.</span><span class="sxs-lookup"><span data-stu-id="fc000-230">The recommended dimensions for the logo are 260 x 35 @ 96 dpi with a file size no greater than 10 KB.</span></span>

    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.PNG"}

> [!NOTE]
> <span data-ttu-id="fc000-231">Den *TargetName* parametern är obligatorisk.</span><span class="sxs-lookup"><span data-stu-id="fc000-231">The *TargetName* parameter is required.</span></span> <span data-ttu-id="fc000-232">Standardtemat som släpps med AD FS kallas standard.</span><span class="sxs-lookup"><span data-stu-id="fc000-232">The default theme that's released with AD FS is named Default.</span></span>

## <span data-ttu-id="fc000-233">Lägg till en beskrivning för inloggning<a name=addsignindescription></a></span><span class="sxs-lookup"><span data-stu-id="fc000-233">Add a sign-in description <a name=addsignindescription></a></span></span>
<span data-ttu-id="fc000-234">Att lägga till en inloggningssida beskrivning av den **inloggningssidan**, Använd följande Windows PowerShell-cmdlet och syntax.</span><span class="sxs-lookup"><span data-stu-id="fc000-234">To add a sign-in page description to the **Sign-in page**, use the following Windows PowerShell cmdlet and syntax.</span></span>

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>"

## <span data-ttu-id="fc000-235">Ändra anspråksregler i AD FS<a name=modclaims></a></span><span class="sxs-lookup"><span data-stu-id="fc000-235">Modify AD FS claim rules <a name=modclaims></a></span></span>
<span data-ttu-id="fc000-236">AD FS stöder ett omfattande anspråk språk som du kan använda för att skapa anpassade anspråksregler.</span><span class="sxs-lookup"><span data-stu-id="fc000-236">AD FS supports a rich claim language that you can use to create custom claim rules.</span></span> <span data-ttu-id="fc000-237">Mer information finns i [roll Anspråksregelspråket](https://technet.microsoft.com/library/dd807118.aspx).</span><span class="sxs-lookup"><span data-stu-id="fc000-237">For more information, see [The Role of the Claim Rule Language](https://technet.microsoft.com/library/dd807118.aspx).</span></span>

<span data-ttu-id="fc000-238">I följande avsnitt beskrivs hur du kan skriva anpassade regler för vissa scenarier som relaterar till Azure AD och AD FS-federation.</span><span class="sxs-lookup"><span data-stu-id="fc000-238">The following sections describe how you can write custom rules for some scenarios that relate to Azure AD and AD FS federation.</span></span>

### <a name="immutable-id-conditional-on-a-value-being-present-in-the-attribute"></a><span data-ttu-id="fc000-239">Oåterkalleliga villkorlig, baserat på ett värde i attributet ID</span><span class="sxs-lookup"><span data-stu-id="fc000-239">Immutable ID conditional on a value being present in the attribute</span></span>
<span data-ttu-id="fc000-240">Azure AD Connect kan du ange ett attribut som ska användas som en källfästpunkt när objekt synkroniseras till Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fc000-240">Azure AD Connect lets you specify an attribute to be used as a source anchor when objects are synced to Azure AD.</span></span> <span data-ttu-id="fc000-241">Om värdet i det anpassade attributet inte är tom, kanske du vill utfärda ett ändras ID-anspråk.</span><span class="sxs-lookup"><span data-stu-id="fc000-241">If the value in the custom attribute is not empty, you might want to issue an immutable ID claim.</span></span>

<span data-ttu-id="fc000-242">Du kan till exempel välja **ms-ds-consistencyguid** som attribut för källfästpunkten och utfärda **ImmutableID** som **ms-ds-consistencyguid** om attributet har ett värde med den.</span><span class="sxs-lookup"><span data-stu-id="fc000-242">For example, you might select **ms-ds-consistencyguid** as the attribute for the source anchor and issue **ImmutableID** as **ms-ds-consistencyguid** in case the attribute has a value against it.</span></span> <span data-ttu-id="fc000-243">Om det finns inget värde mot attributet, utfärda **objectGuid** som ändras-ID.</span><span class="sxs-lookup"><span data-stu-id="fc000-243">If there's no value against the attribute, issue **objectGuid** as the immutable ID.</span></span> <span data-ttu-id="fc000-244">Du kan skapa en uppsättning anpassade anspråksregler som beskrivs i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="fc000-244">You can construct the set of custom claim rules as described in the following section.</span></span>

<span data-ttu-id="fc000-245">**Regel 1: Frågan attribut**</span><span class="sxs-lookup"><span data-stu-id="fc000-245">**Rule 1: Query attributes**</span></span>

    c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]
    => add(store = "Active Directory", types = ("http://contoso.com/ws/2016/02/identity/claims/objectguid", "http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid"), query = "; objectGuid,ms-ds-consistencyguid;{0}", param = c.Value);

<span data-ttu-id="fc000-246">I den här regeln du frågar värdena för **ms-ds-consistencyguid** och **objectGuid** för användare från Active Directory.</span><span class="sxs-lookup"><span data-stu-id="fc000-246">In this rule, you're querying the values of **ms-ds-consistencyguid** and **objectGuid** for the user from Active Directory.</span></span> <span data-ttu-id="fc000-247">Ändra namnet på det Arkiv till en lämplig store-namn i AD FS-distribution.</span><span class="sxs-lookup"><span data-stu-id="fc000-247">Change the store name to an appropriate store name in your AD FS deployment.</span></span> <span data-ttu-id="fc000-248">Ändra anspråk typ till en korrekt anspråk också typ för din federationsserver som definierats för **objectGuid** och **ms-ds-consistencyguid**.</span><span class="sxs-lookup"><span data-stu-id="fc000-248">Also change the claims type to a proper claims type for your federation, as defined for **objectGuid** and **ms-ds-consistencyguid**.</span></span>

<span data-ttu-id="fc000-249">Dessutom med hjälp av **lägga till** och inte **problemet**, du undvika att lägga till ett utgående problem för entiteten och använda värden som mellanliggande värden.</span><span class="sxs-lookup"><span data-stu-id="fc000-249">Also, by using **add** and not **issue**, you avoid adding an outgoing issue for the entity, and can use the values as intermediate values.</span></span> <span data-ttu-id="fc000-250">Du tänker utfärda anspråk i en senare regel när du har skapat vilket värde som ska användas som ändras-ID.</span><span class="sxs-lookup"><span data-stu-id="fc000-250">You will issue the claim in a later rule after you establish which value to use as the immutable ID.</span></span>

<span data-ttu-id="fc000-251">**Regel 2: Kontrollera om det finns ms-ds-consistencyguid för användaren**</span><span class="sxs-lookup"><span data-stu-id="fc000-251">**Rule 2: Check if ms-ds-consistencyguid exists for the user**</span></span>

    NOT EXISTS([Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid"])
    => add(Type = "urn:anandmsft:tmp/idflag", Value = "useguid");

<span data-ttu-id="fc000-252">Den här regeln som definierar en tillfällig flagga kallas **idflag** som har angetts till **useguid** om det finns inga **ms-ds-consistencyguid** fyllts i för användaren.</span><span class="sxs-lookup"><span data-stu-id="fc000-252">This rule defines a temporary flag called **idflag** that is set to **useguid** if there's no **ms-ds-consistencyguid** populated for the user.</span></span> <span data-ttu-id="fc000-253">Logiken bakom detta är det faktum att AD FS inte tillåter tomma anspråk.</span><span class="sxs-lookup"><span data-stu-id="fc000-253">The logic behind this is the fact that AD FS doesn't allow empty claims.</span></span> <span data-ttu-id="fc000-254">Så när du lägger till anspråk http://contoso.com/ws/2016/02/identity/claims/objectguid och http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid i regel 1 kan du få ett **msdsconsistencyguid** anspråk endast om värdet fylls för användaren.</span><span class="sxs-lookup"><span data-stu-id="fc000-254">So when you add claims http://contoso.com/ws/2016/02/identity/claims/objectguid and http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid in Rule 1, you end up with an **msdsconsistencyguid** claim only if the value is populated for the user.</span></span> <span data-ttu-id="fc000-255">Om det inte är ifylld ser att den har ett tomt värde och släpper den direkt i AD FS.</span><span class="sxs-lookup"><span data-stu-id="fc000-255">If it isn't populated, AD FS sees that it will have an empty value and drops it immediately.</span></span> <span data-ttu-id="fc000-256">Alla objekt har **objectGuid**, så att anspråk kommer alltid att det efter regel 1 körs.</span><span class="sxs-lookup"><span data-stu-id="fc000-256">All objects will have **objectGuid**, so that claim will always be there after Rule 1 is executed.</span></span>

<span data-ttu-id="fc000-257">**Regel 3: Utfärda ms-ds-consistencyguid-ID som inte ändras om det finns**</span><span class="sxs-lookup"><span data-stu-id="fc000-257">**Rule 3: Issue ms-ds-consistencyguid as immutable ID if it's present**</span></span>

    c:[Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c.Value);

<span data-ttu-id="fc000-258">Detta är en implicit **finns** kontrollera.</span><span class="sxs-lookup"><span data-stu-id="fc000-258">This is an implicit **Exist** check.</span></span> <span data-ttu-id="fc000-259">Om värdet för anspråket finns sedan utfärda som som ändras-ID.</span><span class="sxs-lookup"><span data-stu-id="fc000-259">If the value for the claim exists, then issue that as the immutable ID.</span></span> <span data-ttu-id="fc000-260">Det föregående exemplet används den **nameidentifier** anspråk.</span><span class="sxs-lookup"><span data-stu-id="fc000-260">The previous example uses the **nameidentifier** claim.</span></span> <span data-ttu-id="fc000-261">Du måste ändra det till lämplig Anspråkstypen för ändras ID i din miljö.</span><span class="sxs-lookup"><span data-stu-id="fc000-261">You'll have to change this to the appropriate claim type for the immutable ID in your environment.</span></span>

<span data-ttu-id="fc000-262">**Regel 4: Utfärda objectGuid-ID som inte ändras om det inte finns någon ms-ds-consistencyGuid**</span><span class="sxs-lookup"><span data-stu-id="fc000-262">**Rule 4: Issue objectGuid as immutable ID if ms-ds-consistencyGuid is not present**</span></span>

    c1:[Type == "urn:anandmsft:tmp/idflag", Value =~ "useguid"]
    && c2:[Type == "http://contoso.com/ws/2016/02/identity/claims/objectguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c2.Value);

<span data-ttu-id="fc000-263">I den här regeln du helt enkelt kontrollerar flaggan tillfälliga **idflag**.</span><span class="sxs-lookup"><span data-stu-id="fc000-263">In this rule, you're simply checking the temporary flag **idflag**.</span></span> <span data-ttu-id="fc000-264">Du bestämma om du vill utfärda anspråk baserat på dess värde.</span><span class="sxs-lookup"><span data-stu-id="fc000-264">You decide whether to issue the claim based on its value.</span></span>

> [!NOTE]
> <span data-ttu-id="fc000-265">Sekvens med dessa regler är viktigt.</span><span class="sxs-lookup"><span data-stu-id="fc000-265">The sequence of these rules is important.</span></span>

### <a name="sso-with-a-subdomain-upn"></a><span data-ttu-id="fc000-266">Enkel inloggning med en underdomän UPN</span><span class="sxs-lookup"><span data-stu-id="fc000-266">SSO with a subdomain UPN</span></span>
<span data-ttu-id="fc000-267">Du kan lägga till fler än en domän att bli federerad med hjälp av Azure AD Connect, enligt beskrivningen i [lägga till en ny extern domän](active-directory-aadconnect-federation-management.md#addfeddomain).</span><span class="sxs-lookup"><span data-stu-id="fc000-267">You can add more than one domain to be federated by using Azure AD Connect, as described in [Add a new federated domain](active-directory-aadconnect-federation-management.md#addfeddomain).</span></span> <span data-ttu-id="fc000-268">Eftersom federerade rotdomänen omfattar även underordnat måste du ändra användarens huvudnamn (UPN) anspråk så att utfärdaren-ID motsvarar rotdomän och inte underdomänen.</span><span class="sxs-lookup"><span data-stu-id="fc000-268">You must modify the user principal name (UPN) claim so that the issuer ID corresponds to the root domain and not the subdomain, because the federated root domain also covers the child.</span></span>

<span data-ttu-id="fc000-269">Anspråksregel för utfärdaren ID är som standard som:</span><span class="sxs-lookup"><span data-stu-id="fc000-269">By default, the claim rule for issuer ID is set as:</span></span>

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

![Standard utfärdaren ID-anspråk](media/active-directory-aadconnect-federation-management/issuer_id_default.png)

<span data-ttu-id="fc000-271">Standardregeln bara tar UPN-suffix och används i ID-anspråk utfärdare.</span><span class="sxs-lookup"><span data-stu-id="fc000-271">The default rule simply takes the UPN suffix and uses it in the issuer ID claim.</span></span> <span data-ttu-id="fc000-272">Till exempel John är en användare i sub.contoso.com och contoso.com är federerat med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fc000-272">For example, John is a user in sub.contoso.com, and contoso.com is federated with Azure AD.</span></span> <span data-ttu-id="fc000-273">John anger john@sub.contoso.com som användarnamn när du loggar in på Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fc000-273">John enters john@sub.contoso.com as the username while signing in to Azure AD.</span></span> <span data-ttu-id="fc000-274">Utfärdaren ID anspråk Standardregeln i AD FS hanterar den på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="fc000-274">The default issuer ID claim rule in AD FS handles it in the following manner:</span></span>

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(john@sub.contoso.com, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

<span data-ttu-id="fc000-275">**Anspråksvärde:** http://sub.contoso.com/adfs/services/trust/</span><span class="sxs-lookup"><span data-stu-id="fc000-275">**Claim value:**  http://sub.contoso.com/adfs/services/trust/</span></span>

<span data-ttu-id="fc000-276">Ändra regel för anspråk för att matcha följande om du vill att endast rotdomänen i anspråksvärdet utfärdare:</span><span class="sxs-lookup"><span data-stu-id="fc000-276">To have only the root domain in the issuer claim value, change the claim rule to match the following:</span></span>

    c:[Type == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “^((.*)([.|@]))?(?<domain>[^.]*[.].*)$”, “http://${domain}/adfs/services/trust/“));

## <a name="next-steps"></a><span data-ttu-id="fc000-277">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fc000-277">Next steps</span></span>
<span data-ttu-id="fc000-278">Lär dig mer om [användaren inloggningsalternativ](active-directory-aadconnect-user-signin.md).</span><span class="sxs-lookup"><span data-stu-id="fc000-278">Learn more about [user sign-in options](active-directory-aadconnect-user-signin.md).</span></span>
