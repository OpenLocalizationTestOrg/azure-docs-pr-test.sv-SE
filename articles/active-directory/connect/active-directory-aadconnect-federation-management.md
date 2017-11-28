---
title: aaaActive Directory Federation Services hantering och anpassning med Azure AD Connect | Microsoft Docs
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
ms.openlocfilehash: 361a2bfd6d7a6993dbe773d6ea039ad1afc6346a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-and-customize-active-directory-federation-services-by-using-azure-ad-connect"></a><span data-ttu-id="661a0-104">Hantera och anpassa Active Directory Federation Services med hjälp av Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="661a0-104">Manage and customize Active Directory Federation Services by using Azure AD Connect</span></span>
<span data-ttu-id="661a0-105">Den här artikeln beskriver hur toomanage och anpassa Active Directory Federation Services (AD FS) med hjälp av Azure Active Directory (AD Azure) Connect.</span><span class="sxs-lookup"><span data-stu-id="661a0-105">This article describes how toomanage and customize Active Directory Federation Services (AD FS) by using Azure Active Directory (Azure AD) Connect.</span></span> <span data-ttu-id="661a0-106">Den omfattar också andra vanliga AD FS-aktiviteter som att du kanske behöver toodo för att slutföra konfigurationen av en AD FS-servergrupp.</span><span class="sxs-lookup"><span data-stu-id="661a0-106">It also includes other common AD FS tasks that you might need toodo for a complete configuration of an AD FS farm.</span></span>

| <span data-ttu-id="661a0-107">Avsnitt</span><span class="sxs-lookup"><span data-stu-id="661a0-107">Topic</span></span> | <span data-ttu-id="661a0-108">Det täcker</span><span class="sxs-lookup"><span data-stu-id="661a0-108">What it covers</span></span> |
|:--- |:--- |
| <span data-ttu-id="661a0-109">**Hantera AD FS**</span><span class="sxs-lookup"><span data-stu-id="661a0-109">**Manage AD FS**</span></span> | |
| [<span data-ttu-id="661a0-110">Reparera hello förtroende</span><span class="sxs-lookup"><span data-stu-id="661a0-110">Repair hello trust</span></span>](#repairthetrust) |<span data-ttu-id="661a0-111">Hur toorepair hello federation förtroende med Office 365.</span><span class="sxs-lookup"><span data-stu-id="661a0-111">How toorepair hello federation trust with Office 365.</span></span> |
| [<span data-ttu-id="661a0-112">Federera med Azure AD med hjälp av alternativa inloggnings-ID</span><span class="sxs-lookup"><span data-stu-id="661a0-112">Federate with Azure AD using alternate login ID </span></span>](#alternateid) | <span data-ttu-id="661a0-113">Konfigurera federation med hjälp av alternativa inloggnings-ID</span><span class="sxs-lookup"><span data-stu-id="661a0-113">Configure federation using alternate login ID</span></span>  |
| [<span data-ttu-id="661a0-114">Lägga till en AD FS-server</span><span class="sxs-lookup"><span data-stu-id="661a0-114">Add an AD FS server</span></span>](#addadfsserver) |<span data-ttu-id="661a0-115">Hur tooexpand en AD FS servergruppen med en ytterligare AD FS-servern.</span><span class="sxs-lookup"><span data-stu-id="661a0-115">How tooexpand an AD FS farm with an additional AD FS server.</span></span> |
| [<span data-ttu-id="661a0-116">Lägga till en AD FS Web Application Proxy-server</span><span class="sxs-lookup"><span data-stu-id="661a0-116">Add an AD FS Web Application Proxy server</span></span>](#addwapserver) |<span data-ttu-id="661a0-117">Hur tooexpand en AD FS servergruppen med ytterligare en Webbprogramproxy (WAP) server.</span><span class="sxs-lookup"><span data-stu-id="661a0-117">How tooexpand an AD FS farm with an additional Web Application Proxy (WAP) server.</span></span> |
| [<span data-ttu-id="661a0-118">Lägga till en federerad domän</span><span class="sxs-lookup"><span data-stu-id="661a0-118">Add a federated domain</span></span>](#addfeddomain) |<span data-ttu-id="661a0-119">Hur tooadd en federerad domän.</span><span class="sxs-lookup"><span data-stu-id="661a0-119">How tooadd a federated domain.</span></span> |
| [<span data-ttu-id="661a0-120">Uppdatera hello SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="661a0-120">Update hello SSL certificate</span></span>](active-directory-aadconnectfed-ssl-update.md)| <span data-ttu-id="661a0-121">Hur tooupdate hello SSL-certifikatet för en AD FS-servergrupp.</span><span class="sxs-lookup"><span data-stu-id="661a0-121">How tooupdate hello SSL certificate for an AD FS farm.</span></span> |
| <span data-ttu-id="661a0-122">**Anpassa AD FS**</span><span class="sxs-lookup"><span data-stu-id="661a0-122">**Customize AD FS**</span></span> | |
| [<span data-ttu-id="661a0-123">Lägga till en anpassad logotyp eller bild</span><span class="sxs-lookup"><span data-stu-id="661a0-123">Add a custom company logo or illustration</span></span>](#customlogo) |<span data-ttu-id="661a0-124">Hur toocustomize en AD FS-inloggning sidan med företagets logotyp och illustration.</span><span class="sxs-lookup"><span data-stu-id="661a0-124">How toocustomize an AD FS sign-in page with a company logo and illustration.</span></span> |
| [<span data-ttu-id="661a0-125">Lägg till en beskrivning för inloggning</span><span class="sxs-lookup"><span data-stu-id="661a0-125">Add a sign-in description</span></span>](#addsignindescription) |<span data-ttu-id="661a0-126">Hur sidan tooadd en inloggning beskrivning.</span><span class="sxs-lookup"><span data-stu-id="661a0-126">How tooadd a sign-in page description.</span></span> |
| [<span data-ttu-id="661a0-127">Ändra anspråksregler i AD FS</span><span class="sxs-lookup"><span data-stu-id="661a0-127">Modify AD FS claim rules</span></span>](#modclaims) |<span data-ttu-id="661a0-128">Hur toomodify AD FS-anspråk för olika federationsscenarier.</span><span class="sxs-lookup"><span data-stu-id="661a0-128">How toomodify AD FS claims for various federation scenarios.</span></span> |

## <a name="manage-ad-fs"></a><span data-ttu-id="661a0-129">Hantera AD FS</span><span class="sxs-lookup"><span data-stu-id="661a0-129">Manage AD FS</span></span>
<span data-ttu-id="661a0-130">Du kan utföra olika AD FS-relaterade uppgifter i Azure AD Connect med minimal användaråtgärder med hello Azure AD Connect-guiden.</span><span class="sxs-lookup"><span data-stu-id="661a0-130">You can perform various AD FS-related tasks in Azure AD Connect with minimal user intervention by using hello Azure AD Connect wizard.</span></span> <span data-ttu-id="661a0-131">När du är klar med installationen av Azure AD Connect körs hello-guiden kan du köra guiden hello igen tooperform ytterligare aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="661a0-131">After you've finished installing Azure AD Connect by running hello wizard, you can run hello wizard again tooperform additional tasks.</span></span>

## <span data-ttu-id="661a0-132">Reparera hello förtroende<a name=repairthetrust></a></span><span class="sxs-lookup"><span data-stu-id="661a0-132">Repair hello trust <a name=repairthetrust></a></span></span>
<span data-ttu-id="661a0-133">Du kan använda Azure AD Connect toocheck hello aktuellt hälsotillstånd för hello AD FS och Azure AD-förtroende och vidta lämpliga åtgärder toorepair hello förtroende.</span><span class="sxs-lookup"><span data-stu-id="661a0-133">You can use Azure AD Connect toocheck hello current health of hello AD FS and Azure AD trust and take appropriate actions toorepair hello trust.</span></span> <span data-ttu-id="661a0-134">Följ dessa steg toorepair din Azure AD och AD FS förtroende.</span><span class="sxs-lookup"><span data-stu-id="661a0-134">Follow these steps toorepair your Azure AD and AD FS trust.</span></span>

1. <span data-ttu-id="661a0-135">Välj **reparera AAD och litar på AD FS** hello listan med ytterligare åtgärder.</span><span class="sxs-lookup"><span data-stu-id="661a0-135">Select **Repair AAD and ADFS Trust** from hello list of additional tasks.</span></span>
   <span data-ttu-id="661a0-136">![Reparera AAD och ADFS förtroende](media/active-directory-aadconnect-federation-management/RepairADTrust1.PNG)</span><span class="sxs-lookup"><span data-stu-id="661a0-136">![Repair AAD and ADFS Trust](media/active-directory-aadconnect-federation-management/RepairADTrust1.PNG)</span></span>

2. <span data-ttu-id="661a0-137">På hello **ansluta tooAzure AD** anger dina autentiseringsuppgifter som global administratör för Azure AD, och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="661a0-137">On hello **Connect tooAzure AD** page, provide your global administrator credentials for Azure AD, and click **Next**.</span></span>
   <span data-ttu-id="661a0-138">![Ansluta tooAzure AD](media/active-directory-aadconnect-federation-management/RepairADTrust2.PNG)</span><span class="sxs-lookup"><span data-stu-id="661a0-138">![Connect tooAzure AD](media/active-directory-aadconnect-federation-management/RepairADTrust2.PNG)</span></span>

3. <span data-ttu-id="661a0-139">På hello **fjärråtkomst autentiseringsuppgifter** ange hello autentiseringsuppgifter för domänadministratör hello.</span><span class="sxs-lookup"><span data-stu-id="661a0-139">On hello **Remote access credentials** page, enter hello credentials for hello domain administrator.</span></span>

   ![Autentiseringsuppgifter för fjärråtkomst](media/active-directory-aadconnect-federation-management/RepairADTrust3.PNG)

    <span data-ttu-id="661a0-141">När du klickar på **nästa**, Azure AD Connect söker efter certifikat hälsa och visar eventuella problem.</span><span class="sxs-lookup"><span data-stu-id="661a0-141">After you click **Next**, Azure AD Connect checks for certificate health and shows any issues.</span></span>

    ![Tillståndet för certifikat](media/active-directory-aadconnect-federation-management/RepairADTrust4.PNG)

    <span data-ttu-id="661a0-143">Hej **klar tooconfigure** sidan visar hello lista med åtgärder som kommer att utföras toorepair hello förtroende.</span><span class="sxs-lookup"><span data-stu-id="661a0-143">hello **Ready tooconfigure** page shows hello list of actions that will be performed toorepair hello trust.</span></span>

    ![Redo tooconfigure](media/active-directory-aadconnect-federation-management/RepairADTrust5.PNG)

4. <span data-ttu-id="661a0-145">Klicka på **installera** toorepair hello förtroende.</span><span class="sxs-lookup"><span data-stu-id="661a0-145">Click **Install** toorepair hello trust.</span></span>

> [!NOTE]
> <span data-ttu-id="661a0-146">Azure AD Connect kan bara reparera eller agera på självsignerade certifikat.</span><span class="sxs-lookup"><span data-stu-id="661a0-146">Azure AD Connect can only repair or act on certificates that are self-signed.</span></span> <span data-ttu-id="661a0-147">Azure AD Connect kan inte åtgärda certifikat från tredjepart.</span><span class="sxs-lookup"><span data-stu-id="661a0-147">Azure AD Connect can't repair third-party certificates.</span></span>

## <span data-ttu-id="661a0-148">Federera med Azure AD med hjälp av AlternateID<a name=alternateid></a></span><span class="sxs-lookup"><span data-stu-id="661a0-148">Federate with Azure AD using AlternateID <a name=alternateid></a></span></span>
<span data-ttu-id="661a0-149">Du rekommenderas att hello lokala UPN Name(UPN) och hello molnet UPN-namnet hålls hello samma.</span><span class="sxs-lookup"><span data-stu-id="661a0-149">It is recommended that hello on-premises User Principal Name(UPN) and hello cloud User Principal Name are kept hello same.</span></span> <span data-ttu-id="661a0-150">Om hello lokala UPN använder en icke-dirigerbara domän (t.ex.</span><span class="sxs-lookup"><span data-stu-id="661a0-150">If hello on-premises UPN uses a non-routable domain (ex.</span></span> <span data-ttu-id="661a0-151">Contoso.local) eller kan inte ändras på grund av toolocal programberoenden, rekommenderar vi att konfigurera alternativa inloggnings-ID.</span><span class="sxs-lookup"><span data-stu-id="661a0-151">Contoso.local) or cannot be changed due toolocal application dependencies, we recommend setting up alternate login ID.</span></span> <span data-ttu-id="661a0-152">Alternativt inloggnings-ID kan du tooconfigure en inloggningen där användare kan logga in med ett attribut än sina UPN-namnet, till exempel e-post.</span><span class="sxs-lookup"><span data-stu-id="661a0-152">Alternate login ID allows you tooconfigure a sign-in experience where users can sign in with an attribute other than their UPN, such as mail.</span></span> <span data-ttu-id="661a0-153">hello val för UPN-namnet i Azure AD Connect standardvärden toohello userPrincipalName attribut i Active Directory.</span><span class="sxs-lookup"><span data-stu-id="661a0-153">hello choice for User Principal Name in Azure AD Connect defaults toohello userPrincipalName attribute in Active Directory.</span></span> <span data-ttu-id="661a0-154">Om du väljer andra attribut för UPN-namnet och federerar med AD FS, sedan Azure AD Connect kommer att konfigurera AD FS alternativt inloggnings-ID</span><span class="sxs-lookup"><span data-stu-id="661a0-154">If you choose any other attribute for User Principal Name and are federating using AD FS, then Azure AD Connect will configure AD FS for alternate login ID.</span></span> <span data-ttu-id="661a0-155">Ett exempel på att välja ett annat attribut för UPN-namnet visas nedan:</span><span class="sxs-lookup"><span data-stu-id="661a0-155">An example of choosing a different attribute for User Principal Name is shown below:</span></span>

![Val av alternativa ID-attribut](media/active-directory-aadconnect-federation-management/attributeselection.png)

<span data-ttu-id="661a0-157">Konfigurera alternativt inloggnings-ID för AD FS består av två Huvudsteg:</span><span class="sxs-lookup"><span data-stu-id="661a0-157">Configuring alternate login ID for AD FS consists of two main steps:</span></span>
1. <span data-ttu-id="661a0-158">**Konfigurera hello rätt uppsättning utfärdande anspråk**: hello utfärdande anspråksregler i hello Azure AD förlitande part förtroende är attributet för ändrade toouse hello valt UserPrincipalName som hello Alternativt ID för hello användare.</span><span class="sxs-lookup"><span data-stu-id="661a0-158">**Configure hello right set of issuance claims**: hello issuance claim rules in hello Azure AD relying party trust are modified toouse hello selected UserPrincipalName attribute as hello alternate ID of hello user.</span></span>
2. <span data-ttu-id="661a0-159">**Aktivera alternativt inloggnings-ID i hello AD FS-konfigurationen**: hello AD FS-konfigurationen har uppdaterats så att AD FS kan söka efter användare i hello lämpliga skogar med hello alternativa-ID.</span><span class="sxs-lookup"><span data-stu-id="661a0-159">**Enable alternate login ID in hello AD FS configuration**: hello AD FS configuration is updated so that AD FS can look up users in hello appropriate forests using hello alternate ID.</span></span> <span data-ttu-id="661a0-160">Den här konfigurationen stöds för AD FS i Windows Server 2012 R2 (med KB2919355) eller senare.</span><span class="sxs-lookup"><span data-stu-id="661a0-160">This configuration is supported for AD FS on Windows Server 2012 R2 (with KB2919355) or later.</span></span> <span data-ttu-id="661a0-161">Om hello AD FS-servrarna är 2012 R2, för Azure AD Connect kontrollerar hello förekomst av hello KB.</span><span class="sxs-lookup"><span data-stu-id="661a0-161">If hello AD FS servers are 2012 R2, Azure AD Connect checks for hello presence of hello required KB.</span></span> <span data-ttu-id="661a0-162">Om hello KB identifieras, visas en varning när konfigurationen är klar, enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="661a0-162">If hello KB is not detected, a warning will be displayed after configuration completes, as shown below:</span></span>

    ![Varning för saknas KB på 2012R2](media/active-directory-aadconnect-federation-management/kbwarning.png)

    <span data-ttu-id="661a0-164">toorectify hello konfiguration vid saknas KB installera hello krävs [KB2919355](http://go.microsoft.com/fwlink/?LinkID=396590) och reparera sedan hello förtroende med [reparera AAD och AD FS-förtroende](#repairthetrust).</span><span class="sxs-lookup"><span data-stu-id="661a0-164">toorectify hello configuration in case of missing KB, install hello required [KB2919355](http://go.microsoft.com/fwlink/?LinkID=396590) and then repair hello trust using [Repair AAD and AD FS Trust](#repairthetrust).</span></span>

> [!NOTE]
> <span data-ttu-id="661a0-165">Mer information om alternateID och steg toomanually konfigurera kan läsa [konfigurera alternativa inloggnings-ID](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configuring-alternate-login-id)</span><span class="sxs-lookup"><span data-stu-id="661a0-165">For more information on alternateID and steps toomanually configure, read [Configuring Alternate Login ID](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configuring-alternate-login-id)</span></span>

## <span data-ttu-id="661a0-166">Lägga till en AD FS-server<a name=addadfsserver></a></span><span class="sxs-lookup"><span data-stu-id="661a0-166">Add an AD FS server <a name=addadfsserver></a></span></span>

> [!NOTE]
> <span data-ttu-id="661a0-167">tooadd en AD FS-servern, Azure AD Connect kräver hello PFX-certifikat.</span><span class="sxs-lookup"><span data-stu-id="661a0-167">tooadd an AD FS server, Azure AD Connect requires hello PFX certificate.</span></span> <span data-ttu-id="661a0-168">Därför kan utföra du den här åtgärden endast om du har konfigurerat hello AD FS-servergrupp med hjälp av Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="661a0-168">Therefore, you can perform this operation only if you configured hello AD FS farm by using Azure AD Connect.</span></span>

1. <span data-ttu-id="661a0-169">Välj **distribuera ytterligare en federationsserver**, och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="661a0-169">Select **Deploy an additional Federation Server**, and click **Next**.</span></span>

   ![Ytterligare federationsserver](media/active-directory-aadconnect-federation-management/AddNewADFSServer1.PNG)

2. <span data-ttu-id="661a0-171">På hello **ansluta tooAzure AD** , ange dina autentiseringsuppgifter som global administratör för Azure AD och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="661a0-171">On hello **Connect tooAzure AD** page, enter your global administrator credentials for Azure AD, and click **Next**.</span></span>

   ![Ansluta tooAzure AD](media/active-directory-aadconnect-federation-management/AddNewADFSServer2.PNG)

3. <span data-ttu-id="661a0-173">Ange hello domänadministratörens autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="661a0-173">Provide hello domain administrator credentials.</span></span>

   ![Domänadministratörsbehörighet](media/active-directory-aadconnect-federation-management/AddNewADFSServer3.PNG)

4. <span data-ttu-id="661a0-175">Azure AD Connect begär hello lösenordet för hello PFX-filen som du angav när du konfigurerar din nya AD FS-grupp med Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="661a0-175">Azure AD Connect asks for hello password of hello PFX file that you provided while configuring your new AD FS farm with Azure AD Connect.</span></span> <span data-ttu-id="661a0-176">Klicka på **ange lösenord för** tooprovide hello lösenord för hello PFX-filen.</span><span class="sxs-lookup"><span data-stu-id="661a0-176">Click **Enter Password** tooprovide hello password for hello PFX file.</span></span>

   ![Lösenord för certifikatet](media/active-directory-aadconnect-federation-management/AddNewADFSServer4.PNG)

    ![Ange SSL-certifikat](media/active-directory-aadconnect-federation-management/AddNewADFSServer5.PNG)

5. <span data-ttu-id="661a0-179">På hello **AD FS-servrar** anger hello-servernamn eller IP-adress toobe läggs toohello AD FS-servergrupp.</span><span class="sxs-lookup"><span data-stu-id="661a0-179">On hello **AD FS Servers** page, enter hello server name or IP address toobe added toohello AD FS farm.</span></span>

   ![AD FS-servrar](media/active-directory-aadconnect-federation-management/AddNewADFSServer6.PNG)

6. <span data-ttu-id="661a0-181">Klicka på **nästa**, och gå igenom hello slutliga **konfigurera** sidan.</span><span class="sxs-lookup"><span data-stu-id="661a0-181">Click **Next**, and go through hello final **Configure** page.</span></span> <span data-ttu-id="661a0-182">När Azure AD Connect har lagt till hello servrar toohello AD FS-servergrupp, får du hello alternativet tooverify hello anslutningen.</span><span class="sxs-lookup"><span data-stu-id="661a0-182">After Azure AD Connect has finished adding hello servers toohello AD FS farm, you will be given hello option tooverify hello connectivity.</span></span>

   ![Redo tooconfigure](media/active-directory-aadconnect-federation-management/AddNewADFSServer7.PNG)

    ![Installationen är klar](media/active-directory-aadconnect-federation-management/AddNewADFSServer8.PNG)

## <span data-ttu-id="661a0-185">Lägga till en AD FS WAP-server<a name=addwapserver></a></span><span class="sxs-lookup"><span data-stu-id="661a0-185">Add an AD FS WAP server <a name=addwapserver></a></span></span>

> [!NOTE]
> <span data-ttu-id="661a0-186">tooadd en server för WAP Azure AD Connect kräver hello PFX-certifikat.</span><span class="sxs-lookup"><span data-stu-id="661a0-186">tooadd a WAP server, Azure AD Connect requires hello PFX certificate.</span></span> <span data-ttu-id="661a0-187">Du kan därför bara utföra den här åtgärden om du har konfigurerat hello AD FS-servergrupp med hjälp av Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="661a0-187">Therefore, you can only perform this operation if you configured hello AD FS farm by using Azure AD Connect.</span></span>

1. <span data-ttu-id="661a0-188">Välj **distribuera Webbprogramproxy** hello listan över tillgängliga uppgifter.</span><span class="sxs-lookup"><span data-stu-id="661a0-188">Select **Deploy Web Application Proxy** from hello list of available tasks.</span></span>

   ![Distribuera Webbprogramproxy](media/active-directory-aadconnect-federation-management/WapServer1.PNG)

2. <span data-ttu-id="661a0-190">Ange hello global administratör för Azure-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="661a0-190">Provide hello Azure global administrator credentials.</span></span>

   ![Ansluta tooAzure AD](media/active-directory-aadconnect-federation-management/wapserver2.PNG)

3. <span data-ttu-id="661a0-192">På hello **ange SSL-certifikat** anger hello lösenord för hello PFX-fil som du angav när du har konfigurerat hello AD FS-servergrupp med Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="661a0-192">On hello **Specify SSL certificate** page, provide hello password for hello PFX file that you provided when you configured hello AD FS farm with Azure AD Connect.</span></span>
   <span data-ttu-id="661a0-193">![Lösenord för certifikatet](media/active-directory-aadconnect-federation-management/WapServer3.PNG)</span><span class="sxs-lookup"><span data-stu-id="661a0-193">![Certificate password](media/active-directory-aadconnect-federation-management/WapServer3.PNG)</span></span>

    ![Ange SSL-certifikat](media/active-directory-aadconnect-federation-management/WapServer4.PNG)

4. <span data-ttu-id="661a0-195">Lägg till hello server toobe läggas till som en server för WAP.</span><span class="sxs-lookup"><span data-stu-id="661a0-195">Add hello server toobe added as a WAP server.</span></span> <span data-ttu-id="661a0-196">Eftersom hello WAP servern inte kanske är domänansluten toohello domän, frågar hello guiden för administrativa autentiseringsuppgifter toohello server som läggs till.</span><span class="sxs-lookup"><span data-stu-id="661a0-196">Because hello WAP server might not be joined toohello domain, hello wizard asks for administrative credentials toohello server being added.</span></span>

   ![Administrativa autentiseringsuppgifter](media/active-directory-aadconnect-federation-management/WapServer5.PNG)

5. <span data-ttu-id="661a0-198">På hello **autentiseringsuppgifter för Proxy förtroende** Tillhandahåll administratörsautentiseringsuppgifter tooconfigure hello proxy förtroende och åtkomst hello primärservern i hello AD FS-servergruppen.</span><span class="sxs-lookup"><span data-stu-id="661a0-198">On hello **Proxy trust credentials** page, provide administrative credentials tooconfigure hello proxy trust and access hello primary server in hello AD FS farm.</span></span>

   ![Autentiseringsuppgifter för proxy-förtroende](media/active-directory-aadconnect-federation-management/WapServer6.PNG)

6. <span data-ttu-id="661a0-200">På hello **klar tooconfigure** sidan hello guiden visar hello lista med åtgärder som kommer att utföras.</span><span class="sxs-lookup"><span data-stu-id="661a0-200">On hello **Ready tooconfigure** page, hello wizard shows hello list of actions that will be performed.</span></span>

   ![Redo tooconfigure](media/active-directory-aadconnect-federation-management/WapServer7.PNG)

7. <span data-ttu-id="661a0-202">Klicka på **installera** toofinish hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="661a0-202">Click **Install** toofinish hello configuration.</span></span> <span data-ttu-id="661a0-203">När hello konfigurationen är klar hello hello guiden ger du hello alternativet tooverify anslutning toohello servrar.</span><span class="sxs-lookup"><span data-stu-id="661a0-203">After hello configuration is complete, hello wizard gives you hello option tooverify hello connectivity toohello servers.</span></span> <span data-ttu-id="661a0-204">Klicka på **Kontrollera** toocheck anslutning.</span><span class="sxs-lookup"><span data-stu-id="661a0-204">Click **Verify** toocheck connectivity.</span></span>

   ![Installationen är klar](media/active-directory-aadconnect-federation-management/WapServer8.PNG)

## <span data-ttu-id="661a0-206">Lägga till en federerad domän<a name=addfeddomain></a></span><span class="sxs-lookup"><span data-stu-id="661a0-206">Add a federated domain <a name=addfeddomain></a></span></span>

<span data-ttu-id="661a0-207">Det är enkelt tooadd en domän toobe federerade med Azure AD med hjälp av Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="661a0-207">It's easy tooadd a domain toobe federated with Azure AD by using Azure AD Connect.</span></span> <span data-ttu-id="661a0-208">Azure AD Connect lägger till hello domänen för federation och ändrar hello anspråk regler toocorrectly återspeglar hello utfärdaren när du har flera domäner federerade med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="661a0-208">Azure AD Connect adds hello domain for federation and modifies hello claim rules toocorrectly reflect hello issuer when you have multiple domains federated with Azure AD.</span></span>

1. <span data-ttu-id="661a0-209">tooadd en federerad domän, väljer hello uppgiften **Lägg till en Azure AD-domänen**.</span><span class="sxs-lookup"><span data-stu-id="661a0-209">tooadd a federated domain, select hello task **Add an additional Azure AD domain**.</span></span>

   ![Ytterligare Azure AD-domän](media/active-directory-aadconnect-federation-management/AdditionalDomain1.PNG)

2. <span data-ttu-id="661a0-211">Hello nästa sida av hello guiden, anger du autentiseringsuppgifter för hello global administratör för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="661a0-211">On hello next page of hello wizard, provide hello global administrator credentials for Azure AD.</span></span>

   ![Ansluta tooAzure AD](media/active-directory-aadconnect-federation-management/AdditionalDomain2.PNG)

3. <span data-ttu-id="661a0-213">På hello **fjärråtkomst autentiseringsuppgifter** anger hello domänadministratörens autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="661a0-213">On hello **Remote access credentials** page, provide hello domain administrator credentials.</span></span>

   ![Autentiseringsuppgifter för fjärråtkomst](media/active-directory-aadconnect-federation-management/additionaldomain3.PNG)

4. <span data-ttu-id="661a0-215">På nästa sida hello tillhandahåller hello guiden en lista över Azure AD-domäner som du kan federera din lokala katalog med.</span><span class="sxs-lookup"><span data-stu-id="661a0-215">On hello next page, hello wizard provides a list of Azure AD domains that you can federate your on-premises directory with.</span></span> <span data-ttu-id="661a0-216">Välj hello domän hello-listan.</span><span class="sxs-lookup"><span data-stu-id="661a0-216">Choose hello domain from hello list.</span></span>

   ![Azure AD-domän](media/active-directory-aadconnect-federation-management/AdditionalDomain4.PNG)

    <span data-ttu-id="661a0-218">När du har valt hello domän ger hello guiden du med lämplig information om ytterligare åtgärder som hello guiden tar och hello effekten av hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="661a0-218">After you choose hello domain, hello wizard provides you with appropriate information about further actions that hello wizard will take and hello impact of hello configuration.</span></span> <span data-ttu-id="661a0-219">I vissa fall, om du väljer en domän som ännu inte verifierats i Azure AD hello guiden ger information toohelp verifiera du hello domän.</span><span class="sxs-lookup"><span data-stu-id="661a0-219">In some cases, if you select a domain that isn't yet verified in Azure AD, hello wizard provides you with information toohelp you verify hello domain.</span></span> <span data-ttu-id="661a0-220">Se [lägga till din anpassade domän namn tooAzure Active Directory](../active-directory-add-domain.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="661a0-220">See [Add your custom domain name tooAzure Active Directory](../active-directory-add-domain.md) for more details.</span></span>

5. <span data-ttu-id="661a0-221">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="661a0-221">Click **Next**.</span></span> <span data-ttu-id="661a0-222">Hej **klar tooconfigure** visar hello lista med åtgärder som kommer att utföras i Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="661a0-222">hello **Ready tooconfigure** page shows hello list of actions that Azure AD Connect will perform.</span></span> <span data-ttu-id="661a0-223">Klicka på **installera** toofinish hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="661a0-223">Click **Install** toofinish hello configuration.</span></span>

   ![Redo tooconfigure](media/active-directory-aadconnect-federation-management/AdditionalDomain5.PNG)

> [!NOTE]
> <span data-ttu-id="661a0-225">Användare från hello läggas till federerade domänen måste synkroniseras innan de kan toologin tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="661a0-225">Users from hello added federated domain must be synchronized before they will be able toologin tooAzure AD.</span></span>

## <a name="ad-fs-customization"></a><span data-ttu-id="661a0-226">AD FS-anpassning</span><span class="sxs-lookup"><span data-stu-id="661a0-226">AD FS customization</span></span>
<span data-ttu-id="661a0-227">hello följande avsnitt innehåller information om några av hello vanliga uppgifter som du kanske tooperform när du anpassar din AD FS-inloggningssida.</span><span class="sxs-lookup"><span data-stu-id="661a0-227">hello following sections provide details about some of hello common tasks that you might have tooperform when you customize your AD FS sign-in page.</span></span>

## <span data-ttu-id="661a0-228">Lägga till en anpassad logotyp eller bild<a name=customlogo></a></span><span class="sxs-lookup"><span data-stu-id="661a0-228">Add a custom company logo or illustration <a name=customlogo></a></span></span>
<span data-ttu-id="661a0-229">toochange hello logotyp hello företag som visas på hello **inloggning** använder hello följande Windows PowerShell-cmdlet och syntax.</span><span class="sxs-lookup"><span data-stu-id="661a0-229">toochange hello logo of hello company that's displayed on hello **Sign-in** page, use hello following Windows PowerShell cmdlet and syntax.</span></span>

> [!NOTE]
> <span data-ttu-id="661a0-230">hello rekommenderas dimensioner för hello logotypen är 260 x 35 96 dpi, samt med en filstorlek som är större än 10 KB.</span><span class="sxs-lookup"><span data-stu-id="661a0-230">hello recommended dimensions for hello logo are 260 x 35 @ 96 dpi with a file size no greater than 10 KB.</span></span>

    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.PNG"}

> [!NOTE]
> <span data-ttu-id="661a0-231">Hej *TargetName* parametern är obligatorisk.</span><span class="sxs-lookup"><span data-stu-id="661a0-231">hello *TargetName* parameter is required.</span></span> <span data-ttu-id="661a0-232">hello standardtemat som släpps med AD FS kallas standard.</span><span class="sxs-lookup"><span data-stu-id="661a0-232">hello default theme that's released with AD FS is named Default.</span></span>

## <span data-ttu-id="661a0-233">Lägg till en beskrivning för inloggning<a name=addsignindescription></a></span><span class="sxs-lookup"><span data-stu-id="661a0-233">Add a sign-in description <a name=addsignindescription></a></span></span>
<span data-ttu-id="661a0-234">tooadd en inloggningssida beskrivning toohello **inloggningssidan**, använda hello följande Windows PowerShell-cmdlet och syntax.</span><span class="sxs-lookup"><span data-stu-id="661a0-234">tooadd a sign-in page description toohello **Sign-in page**, use hello following Windows PowerShell cmdlet and syntax.</span></span>

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in tooContoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>"

## <span data-ttu-id="661a0-235">Ändra anspråksregler i AD FS<a name=modclaims></a></span><span class="sxs-lookup"><span data-stu-id="661a0-235">Modify AD FS claim rules <a name=modclaims></a></span></span>
<span data-ttu-id="661a0-236">AD FS stöder ett omfattande anspråk språk som du kan använda toocreate anpassade anspråksregler.</span><span class="sxs-lookup"><span data-stu-id="661a0-236">AD FS supports a rich claim language that you can use toocreate custom claim rules.</span></span> <span data-ttu-id="661a0-237">Mer information finns i [hello roll hello Anspråksregelspråket](https://technet.microsoft.com/library/dd807118.aspx).</span><span class="sxs-lookup"><span data-stu-id="661a0-237">For more information, see [hello Role of hello Claim Rule Language](https://technet.microsoft.com/library/dd807118.aspx).</span></span>

<span data-ttu-id="661a0-238">hello följande avsnitt beskrivs hur du kan skriva anpassade regler för vissa scenarier som handlar om tooAzure AD och AD FS-federation.</span><span class="sxs-lookup"><span data-stu-id="661a0-238">hello following sections describe how you can write custom rules for some scenarios that relate tooAzure AD and AD FS federation.</span></span>

### <a name="immutable-id-conditional-on-a-value-being-present-in-hello-attribute"></a><span data-ttu-id="661a0-239">Oåterkalleliga villkorlig, baserat på ett värde som finns i hello attribut-ID</span><span class="sxs-lookup"><span data-stu-id="661a0-239">Immutable ID conditional on a value being present in hello attribute</span></span>
<span data-ttu-id="661a0-240">Azure AD Connect kan du ange en toobe för attributet som används som en källfästpunkt när objekt har synkroniserats tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="661a0-240">Azure AD Connect lets you specify an attribute toobe used as a source anchor when objects are synced tooAzure AD.</span></span> <span data-ttu-id="661a0-241">Om värdet för hello hello attributet inte är tom, kanske du vill tooissue ett ändras ID-anspråk.</span><span class="sxs-lookup"><span data-stu-id="661a0-241">If hello value in hello custom attribute is not empty, you might want tooissue an immutable ID claim.</span></span>

<span data-ttu-id="661a0-242">Du kan till exempel välja **ms-ds-consistencyguid** som hello attribut för hello källfästpunkten och utfärda **ImmutableID** som **ms-ds-consistencyguid** i case hello attributet har ett värde med den.</span><span class="sxs-lookup"><span data-stu-id="661a0-242">For example, you might select **ms-ds-consistencyguid** as hello attribute for hello source anchor and issue **ImmutableID** as **ms-ds-consistencyguid** in case hello attribute has a value against it.</span></span> <span data-ttu-id="661a0-243">Om det finns inget värde mot hello-attribut, utfärda **objectGuid** som hello ändras ID.</span><span class="sxs-lookup"><span data-stu-id="661a0-243">If there's no value against hello attribute, issue **objectGuid** as hello immutable ID.</span></span> <span data-ttu-id="661a0-244">Du kan skapa hello uppsättning anpassade anspråksregler som beskrivs i följande avsnitt hello.</span><span class="sxs-lookup"><span data-stu-id="661a0-244">You can construct hello set of custom claim rules as described in hello following section.</span></span>

<span data-ttu-id="661a0-245">**Regel 1: Frågan attribut**</span><span class="sxs-lookup"><span data-stu-id="661a0-245">**Rule 1: Query attributes**</span></span>

    c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]
    => add(store = "Active Directory", types = ("http://contoso.com/ws/2016/02/identity/claims/objectguid", "http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid"), query = "; objectGuid,ms-ds-consistencyguid;{0}", param = c.Value);

<span data-ttu-id="661a0-246">I den här regeln du frågar hello värdena för **ms-ds-consistencyguid** och **objectGuid** för hello användare från Active Directory.</span><span class="sxs-lookup"><span data-stu-id="661a0-246">In this rule, you're querying hello values of **ms-ds-consistencyguid** and **objectGuid** for hello user from Active Directory.</span></span> <span data-ttu-id="661a0-247">Ändra hello store tooan butiken namn i AD FS-distribution.</span><span class="sxs-lookup"><span data-stu-id="661a0-247">Change hello store name tooan appropriate store name in your AD FS deployment.</span></span> <span data-ttu-id="661a0-248">Även ändra hello anspråk tooa anspråk i rätt typ för din federationsserver som definierats för **objectGuid** och **ms-ds-consistencyguid**.</span><span class="sxs-lookup"><span data-stu-id="661a0-248">Also change hello claims type tooa proper claims type for your federation, as defined for **objectGuid** and **ms-ds-consistencyguid**.</span></span>

<span data-ttu-id="661a0-249">Dessutom med hjälp av **lägga till** och inte **problemet**, du undvika att lägga till ett utgående problem för hello entiteten och kan använda hello värden som mellanliggande värden.</span><span class="sxs-lookup"><span data-stu-id="661a0-249">Also, by using **add** and not **issue**, you avoid adding an outgoing issue for hello entity, and can use hello values as intermediate values.</span></span> <span data-ttu-id="661a0-250">Du tänker utfärda hello anspråk i en senare regel när du har skapat toouse vilket värde som hello ändras ID.</span><span class="sxs-lookup"><span data-stu-id="661a0-250">You will issue hello claim in a later rule after you establish which value toouse as hello immutable ID.</span></span>

<span data-ttu-id="661a0-251">**Regel 2: Kontrollera om det finns ms-ds-consistencyguid för hello användare**</span><span class="sxs-lookup"><span data-stu-id="661a0-251">**Rule 2: Check if ms-ds-consistencyguid exists for hello user**</span></span>

    NOT EXISTS([Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid"])
    => add(Type = "urn:anandmsft:tmp/idflag", Value = "useguid");

<span data-ttu-id="661a0-252">Den här regeln som definierar en tillfällig flagga som kallas **idflag** som har angetts för**useguid** om det finns inga **ms-ds-consistencyguid** fyllts i för hello användare.</span><span class="sxs-lookup"><span data-stu-id="661a0-252">This rule defines a temporary flag called **idflag** that is set too**useguid** if there's no **ms-ds-consistencyguid** populated for hello user.</span></span> <span data-ttu-id="661a0-253">hello logiken bakom detta är hello faktum att AD FS inte tillåter tomma anspråk.</span><span class="sxs-lookup"><span data-stu-id="661a0-253">hello logic behind this is hello fact that AD FS doesn't allow empty claims.</span></span> <span data-ttu-id="661a0-254">Så när du lägger till anspråk http://contoso.com/ws/2016/02/identity/claims/objectguid och http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid i regel 1 kan du få ett **msdsconsistencyguid** anspråk endast om hello värdet fylls för hello användare.</span><span class="sxs-lookup"><span data-stu-id="661a0-254">So when you add claims http://contoso.com/ws/2016/02/identity/claims/objectguid and http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid in Rule 1, you end up with an **msdsconsistencyguid** claim only if hello value is populated for hello user.</span></span> <span data-ttu-id="661a0-255">Om det inte är ifylld ser att den har ett tomt värde och släpper den direkt i AD FS.</span><span class="sxs-lookup"><span data-stu-id="661a0-255">If it isn't populated, AD FS sees that it will have an empty value and drops it immediately.</span></span> <span data-ttu-id="661a0-256">Alla objekt har **objectGuid**, så att anspråk kommer alltid att det efter regel 1 körs.</span><span class="sxs-lookup"><span data-stu-id="661a0-256">All objects will have **objectGuid**, so that claim will always be there after Rule 1 is executed.</span></span>

<span data-ttu-id="661a0-257">**Regel 3: Utfärda ms-ds-consistencyguid-ID som inte ändras om det finns**</span><span class="sxs-lookup"><span data-stu-id="661a0-257">**Rule 3: Issue ms-ds-consistencyguid as immutable ID if it's present**</span></span>

    c:[Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c.Value);

<span data-ttu-id="661a0-258">Detta är en implicit **finns** kontrollera.</span><span class="sxs-lookup"><span data-stu-id="661a0-258">This is an implicit **Exist** check.</span></span> <span data-ttu-id="661a0-259">Om hello värdet för hello anspråk finns sedan utfärda att som ändras hello-ID.</span><span class="sxs-lookup"><span data-stu-id="661a0-259">If hello value for hello claim exists, then issue that as hello immutable ID.</span></span> <span data-ttu-id="661a0-260">hello föregående exemplet används hello **nameidentifier** anspråk.</span><span class="sxs-lookup"><span data-stu-id="661a0-260">hello previous example uses hello **nameidentifier** claim.</span></span> <span data-ttu-id="661a0-261">Har du toochange denna toohello lämplig Anspråkstyp för hello ändras ID i din miljö.</span><span class="sxs-lookup"><span data-stu-id="661a0-261">You'll have toochange this toohello appropriate claim type for hello immutable ID in your environment.</span></span>

<span data-ttu-id="661a0-262">**Regel 4: Utfärda objectGuid-ID som inte ändras om det inte finns någon ms-ds-consistencyGuid**</span><span class="sxs-lookup"><span data-stu-id="661a0-262">**Rule 4: Issue objectGuid as immutable ID if ms-ds-consistencyGuid is not present**</span></span>

    c1:[Type == "urn:anandmsft:tmp/idflag", Value =~ "useguid"]
    && c2:[Type == "http://contoso.com/ws/2016/02/identity/claims/objectguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c2.Value);

<span data-ttu-id="661a0-263">I den här regeln du helt enkelt kontrollerar hello tillfälliga flaggan **idflag**.</span><span class="sxs-lookup"><span data-stu-id="661a0-263">In this rule, you're simply checking hello temporary flag **idflag**.</span></span> <span data-ttu-id="661a0-264">Du bestämmer dig för om tooissue hello anspråk baserat på dess värde.</span><span class="sxs-lookup"><span data-stu-id="661a0-264">You decide whether tooissue hello claim based on its value.</span></span>

> [!NOTE]
> <span data-ttu-id="661a0-265">hello sekvens med dessa regler är viktigt.</span><span class="sxs-lookup"><span data-stu-id="661a0-265">hello sequence of these rules is important.</span></span>

### <a name="sso-with-a-subdomain-upn"></a><span data-ttu-id="661a0-266">Enkel inloggning med en underdomän UPN</span><span class="sxs-lookup"><span data-stu-id="661a0-266">SSO with a subdomain UPN</span></span>
<span data-ttu-id="661a0-267">Du kan lägga till fler än en domän toobe federerade med Azure AD Connect, enligt beskrivningen i [lägga till en ny extern domän](active-directory-aadconnect-federation-management.md#addfeddomain).</span><span class="sxs-lookup"><span data-stu-id="661a0-267">You can add more than one domain toobe federated by using Azure AD Connect, as described in [Add a new federated domain](active-directory-aadconnect-federation-management.md#addfeddomain).</span></span> <span data-ttu-id="661a0-268">Du måste ändra hello användaranspråk huvudnamn (UPN) så att hello utfärdaren ID motsvarar toohello rotdomän och inte hello underdomän eftersom hello federerade rotdomänen omfattar även hello underordnade.</span><span class="sxs-lookup"><span data-stu-id="661a0-268">You must modify hello user principal name (UPN) claim so that hello issuer ID corresponds toohello root domain and not hello subdomain, because hello federated root domain also covers hello child.</span></span>

<span data-ttu-id="661a0-269">Som standard hello anspråksregelmallar för utfärdaren-ID har angetts som:</span><span class="sxs-lookup"><span data-stu-id="661a0-269">By default, hello claim rule for issuer ID is set as:</span></span>

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

![Standard utfärdaren ID-anspråk](media/active-directory-aadconnect-federation-management/issuer_id_default.png)

<span data-ttu-id="661a0-271">hello standardregel bara tar hello UPN-suffix och använder i hello utfärdaren ID-anspråk.</span><span class="sxs-lookup"><span data-stu-id="661a0-271">hello default rule simply takes hello UPN suffix and uses it in hello issuer ID claim.</span></span> <span data-ttu-id="661a0-272">Till exempel John är en användare i sub.contoso.com och contoso.com är federerat med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="661a0-272">For example, John is a user in sub.contoso.com, and contoso.com is federated with Azure AD.</span></span> <span data-ttu-id="661a0-273">John anger john@sub.contoso.com som hello användarnamn när du loggar in tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="661a0-273">John enters john@sub.contoso.com as hello username while signing in tooAzure AD.</span></span> <span data-ttu-id="661a0-274">hello standard utfärdaren regel-ID anspråk i AD FS hanteras det i hello följande sätt:</span><span class="sxs-lookup"><span data-stu-id="661a0-274">hello default issuer ID claim rule in AD FS handles it in hello following manner:</span></span>

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(john@sub.contoso.com, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

<span data-ttu-id="661a0-275">**Anspråksvärde:** http://sub.contoso.com/adfs/services/trust/</span><span class="sxs-lookup"><span data-stu-id="661a0-275">**Claim value:**  http://sub.contoso.com/adfs/services/trust/</span></span>

<span data-ttu-id="661a0-276">toohave endast hello rotdomänen i hello utfärdaren anspråksvärde, ändra hello anspråk regeln toomatch hello följande:</span><span class="sxs-lookup"><span data-stu-id="661a0-276">toohave only hello root domain in hello issuer claim value, change hello claim rule toomatch hello following:</span></span>

    c:[Type == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “^((.*)([.|@]))?(?<domain>[^.]*[.].*)$”, “http://${domain}/adfs/services/trust/“));

## <a name="next-steps"></a><span data-ttu-id="661a0-277">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="661a0-277">Next steps</span></span>
<span data-ttu-id="661a0-278">Lär dig mer om [användaren inloggningsalternativ](active-directory-aadconnect-user-signin.md).</span><span class="sxs-lookup"><span data-stu-id="661a0-278">Learn more about [user sign-in options](active-directory-aadconnect-user-signin.md).</span></span>
