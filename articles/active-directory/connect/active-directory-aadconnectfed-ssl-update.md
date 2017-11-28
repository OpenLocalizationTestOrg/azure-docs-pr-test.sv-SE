---
title: "Azure AD Connect: Uppdatera hello SSL-certifikat för en grupp i Active Directory Federation Services (AD FS) | Microsoft Docs"
description: "Det här dokumentet information hello steg tooupdate hello SSL-certifikatet för en AD FS-servergrupp med hjälp av Azure AD Connect."
services: active-directory
keywords: "Azure ad connect, AD FS ssl-uppdatering, uppdatering för AD FS-certifikat, ändra AD FS-certifikat, nya AD FS-certifikat, adfs certifikat, uppdatera AD FS ssl-certifikat, uppdatera ssl-certifikat adfs, konfigurera AD FS ssl-certifikat, AD FS, ssl, certifikat, adfs-tjänsten certifikat för kommunikation, update-federation, konfigurera federation, aad-anslutning"
authors: anandyadavmsft
manager: femila
editor: billmath
ms.assetid: 7c781f61-848a-48ad-9863-eb29da78f53c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: anandy
ms.openlocfilehash: bce7f75aab83b6abacb8472a6895054d137e10e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="update-hello-ssl-certificate-for-an-active-directory-federation-services-ad-fs-farm"></a><span data-ttu-id="8246a-104">Uppdatera hello SSL-certifikat för en grupp i Active Directory Federation Services (AD FS)</span><span class="sxs-lookup"><span data-stu-id="8246a-104">Update hello SSL certificate for an Active Directory Federation Services (AD FS) farm</span></span>

## <a name="overview"></a><span data-ttu-id="8246a-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="8246a-105">Overview</span></span>
<span data-ttu-id="8246a-106">Den här artikeln beskriver hur du kan använda Azure AD Connect tooupdate hello SSL-certifikat för en grupp i Active Directory Federation Services (AD FS).</span><span class="sxs-lookup"><span data-stu-id="8246a-106">This article describes how you can use Azure AD Connect tooupdate hello SSL certificate for an Active Directory Federation Services (AD FS) farm.</span></span> <span data-ttu-id="8246a-107">Du kan använda hello Azure AD Connect-verktyget tooeasily uppdatering hello SSL-certifikat för hello AD FS-servergrupp, även om hello användaren inloggningsmetod valt inte är AD FS.</span><span class="sxs-lookup"><span data-stu-id="8246a-107">You can use hello Azure AD Connect tool tooeasily update hello SSL certificate for hello AD FS farm even if hello user sign-in method selected is not AD FS.</span></span>

<span data-ttu-id="8246a-108">Du kan utföra hello hela åtgärden för att uppdatera SSL-certifikat för hello AD FS-servergrupp över alla federation och Webbprogramproxy (WAP) servrar i tre enkla steg:</span><span class="sxs-lookup"><span data-stu-id="8246a-108">You can perform hello whole operation of updating SSL certificate for hello AD FS farm across all federation and Web Application Proxy (WAP) servers in three simple steps:</span></span>

![Tre steg](./media/active-directory-aadconnectfed-ssl-update/threesteps.png)


>[!NOTE]
><span data-ttu-id="8246a-110">toolearn mer information om certifikat som används av AD FS finns [Förstå certifikat som används av AD FS](https://technet.microsoft.com/library/cc730660.aspx).</span><span class="sxs-lookup"><span data-stu-id="8246a-110">toolearn more about certificates that are used by AD FS, see [Understanding certificates used by AD FS](https://technet.microsoft.com/library/cc730660.aspx).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8246a-111">Krav</span><span class="sxs-lookup"><span data-stu-id="8246a-111">Prerequisites</span></span>

* <span data-ttu-id="8246a-112">**AD FS-servergrupp**: Kontrollera att AD FS-gruppen är baserade på Windows Server 2012 R2 eller senare.</span><span class="sxs-lookup"><span data-stu-id="8246a-112">**AD FS Farm**: Make sure that your AD FS farm is Windows Server 2012 R2-based or later.</span></span>
* <span data-ttu-id="8246a-113">**Azure AD Connect**: se till att hello versionen av Azure AD Connect är 1.1.443.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="8246a-113">**Azure AD Connect**: Ensure that hello version of Azure AD Connect is 1.1.443.0 or later.</span></span> <span data-ttu-id="8246a-114">Du ska använda hello uppgiften **uppdatering AD FS SSL-certifikat**.</span><span class="sxs-lookup"><span data-stu-id="8246a-114">You'll use hello task **Update AD FS SSL certificate**.</span></span>

![Uppdatera SSL-aktivitet](./media/active-directory-aadconnectfed-ssl-update/updatessltask.png)

## <a name="step-1-provide-ad-fs-farm-information"></a><span data-ttu-id="8246a-116">Steg 1: Ange information för AD FS-servergrupp</span><span class="sxs-lookup"><span data-stu-id="8246a-116">Step 1: Provide AD FS farm information</span></span>

<span data-ttu-id="8246a-117">Azure AD Connect försöker automatiskt av tooobtain information om hello AD FS-servergrupp:</span><span class="sxs-lookup"><span data-stu-id="8246a-117">Azure AD Connect attempts tooobtain information about hello AD FS farm automatically by:</span></span>
1. <span data-ttu-id="8246a-118">Hämtar information om hello servergruppen från AD FS (Windows Server 2016 eller senare).</span><span class="sxs-lookup"><span data-stu-id="8246a-118">Querying hello farm information from AD FS (Windows Server 2016 or later).</span></span>
2. <span data-ttu-id="8246a-119">Refererar till hello information från tidigare körs som lagras lokalt med Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="8246a-119">Referencing hello information from previous runs, which are stored locally with Azure AD Connect.</span></span>

<span data-ttu-id="8246a-120">Du kan ändra hello listan över servrar som visas genom att lägga till eller ta bort hello servrar tooreflect hello aktuella konfiguration hello AD FS-servergrupp.</span><span class="sxs-lookup"><span data-stu-id="8246a-120">You can modify hello list of servers that are displayed by adding or removing hello servers tooreflect hello current configuration of hello AD FS farm.</span></span> <span data-ttu-id="8246a-121">Så snart hello serverinformation anges, visar Azure AD Connect hello anslutning och aktuell status för SSL-certifikat.</span><span class="sxs-lookup"><span data-stu-id="8246a-121">As soon as hello server information is provided, Azure AD Connect displays hello connectivity and current SSL certificate status.</span></span>

![AD FS-serverinformation](./media/active-directory-aadconnectfed-ssl-update/adfsserverinfo.png)

<span data-ttu-id="8246a-123">Om hello listan innehåller en server som inte längre tillhör hello AD FS-servergrupp, klickar du på **ta bort** toodelete hello server hello listan över servrar i din AD FS-servergrupp.</span><span class="sxs-lookup"><span data-stu-id="8246a-123">If hello list contains a server that's no longer part of hello AD FS farm, click **Remove** toodelete hello server from hello list of servers in your AD FS farm.</span></span>

![Offline-server i listan](./media/active-directory-aadconnectfed-ssl-update/offlineserverlist.png)

>[!NOTE]
> <span data-ttu-id="8246a-125">Ta bort en server hello listan över servrar för en AD FS-grupp i Azure AD Connect är en lokal åtgärd och uppdateringar hello information för hello AD FS-servergrupp som Azure AD Connect underhåller lokalt.</span><span class="sxs-lookup"><span data-stu-id="8246a-125">Removing a server from hello list of servers for an AD FS farm in Azure AD Connect is a local operation and updates hello information for hello AD FS farm that Azure AD Connect maintains locally.</span></span> <span data-ttu-id="8246a-126">Azure AD Connect ändra inte hello konfigurationen på AD FS tooreflect hello ändras.</span><span class="sxs-lookup"><span data-stu-id="8246a-126">Azure AD Connect doesn't modify hello configuration on AD FS tooreflect hello change.</span></span>    

## <a name="step-2-provide-a-new-ssl-certificate"></a><span data-ttu-id="8246a-127">Steg 2: Ange ett nytt SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="8246a-127">Step 2: Provide a new SSL certificate</span></span>

<span data-ttu-id="8246a-128">När du har bekräftat hello information om AD FS-servergruppen, begär Azure AD Connect hello nytt SSL-certifikat.</span><span class="sxs-lookup"><span data-stu-id="8246a-128">After you've confirmed hello information about AD FS farm servers, Azure AD Connect asks for hello new SSL certificate.</span></span> <span data-ttu-id="8246a-129">Ange en lösenordsskyddad PFX toocontinue hello certifikatinstallation.</span><span class="sxs-lookup"><span data-stu-id="8246a-129">Provide a password-protected PFX certificate toocontinue hello installation.</span></span>

![SSL-certifikat](./media/active-directory-aadconnectfed-ssl-update/certificate.png)

<span data-ttu-id="8246a-131">När du har angett hello certifikat går Azure AD Connect igenom ett antal krav.</span><span class="sxs-lookup"><span data-stu-id="8246a-131">After you provide hello certificate, Azure AD Connect goes through a series of prerequisites.</span></span> <span data-ttu-id="8246a-132">Kontrollera hello certifikat tooensure som hello certifikatet är korrekt för hello AD FS-grupp:</span><span class="sxs-lookup"><span data-stu-id="8246a-132">Verify hello certificate tooensure that hello certificate is correct for hello AD FS farm:</span></span>

-   <span data-ttu-id="8246a-133">hello är ämne namn/alternativa ämnesnamnet för certifikatet hello antingen hello samma som hello federationstjänstens namn eller så är ett jokerteckencertifikat.</span><span class="sxs-lookup"><span data-stu-id="8246a-133">hello subject name/alternate subject name for hello certificate is either hello same as hello federation service name, or it's a wildcard certificate.</span></span>
-   <span data-ttu-id="8246a-134">hello certifikatet är giltigt för mer än 30 dagar.</span><span class="sxs-lookup"><span data-stu-id="8246a-134">hello certificate is valid for more than 30 days.</span></span>
-   <span data-ttu-id="8246a-135">hello certifikatkedjan för certifikatet är giltigt.</span><span class="sxs-lookup"><span data-stu-id="8246a-135">hello certificate trust chain is valid.</span></span>
-   <span data-ttu-id="8246a-136">hello certifikatet är lösenordsskyddat.</span><span class="sxs-lookup"><span data-stu-id="8246a-136">hello certificate is password protected.</span></span>

## <a name="step-3-select-servers-for-hello-update"></a><span data-ttu-id="8246a-137">Steg 3: Välj servrar för hello uppdatering</span><span class="sxs-lookup"><span data-stu-id="8246a-137">Step 3: Select servers for hello update</span></span>

<span data-ttu-id="8246a-138">Välj hello-servrar som behöver toohave hello SSL-certifikat uppdateras i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="8246a-138">In hello next step, select hello servers that need toohave hello SSL certificate updated.</span></span> <span data-ttu-id="8246a-139">Servrar som är offline kan inte väljas för hello uppdatering.</span><span class="sxs-lookup"><span data-stu-id="8246a-139">Servers that are offline can't be selected for hello update.</span></span>

![Välj servrar tooupdate](./media/active-directory-aadconnectfed-ssl-update/selectservers.png)

<span data-ttu-id="8246a-141">När du har slutfört konfigurationen hello visar Azure AD Connect hello-meddelande som anger hello status för hello-uppdatering och ger en alternativ tooverify hello AD FS-inloggning.</span><span class="sxs-lookup"><span data-stu-id="8246a-141">After you complete hello configuration, Azure AD Connect displays hello message that indicates hello status of hello update and provides an option tooverify hello AD FS sign-in.</span></span>

![Slutför konfiguration](./media/active-directory-aadconnectfed-ssl-update/configurecomplete.png)   

## <a name="faqs"></a><span data-ttu-id="8246a-143">Vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="8246a-143">FAQs</span></span>

* <span data-ttu-id="8246a-144">**Vad bör vara hello ämnesnamnet för certifikatet för hello för hello nya AD FS SSL-certifikat?**</span><span class="sxs-lookup"><span data-stu-id="8246a-144">**What should be hello subject name of hello certificate for hello new AD FS SSL certificate?**</span></span>

    <span data-ttu-id="8246a-145">Azure AD Connect kontrollerar om hello Certifikatämnets namn/alternativa ämnesnamnet för certifikatet hello innehåller hello federationstjänstens namn.</span><span class="sxs-lookup"><span data-stu-id="8246a-145">Azure AD Connect checks if hello subject name/alternate subject name of hello certificate contains hello federation service name.</span></span> <span data-ttu-id="8246a-146">Om ditt namn för federationstjänsten är fs.contoso.com, till exempel vara hello Certifikatämnets namn/alternativa ämnesnamn fs.contoso.com.  Jokerteckencertifikat också accepteras.</span><span class="sxs-lookup"><span data-stu-id="8246a-146">For example, if your federation service name is fs.contoso.com, hello subject name/alternate subject name must be fs.contoso.com.  Wildcard certificates are also accepted.</span></span>

* <span data-ttu-id="8246a-147">**Varför jag tillfrågas om autentiseringsuppgifter igen på serversidan för hello WAP?**</span><span class="sxs-lookup"><span data-stu-id="8246a-147">**Why am I asked for credentials again on hello WAP server page?**</span></span>

    <span data-ttu-id="8246a-148">Om hello autentiseringsuppgifter som du anger för att ansluta tooAD FS-servrarna inte också har hello privilegium toomanage hello WAP-servrar, frågar Azure AD Connect för autentiseringsuppgifter som har administratörsbehörighet på hello WAP-servrar.</span><span class="sxs-lookup"><span data-stu-id="8246a-148">If hello credentials you provide for connecting tooAD FS servers don't also have hello privilege toomanage hello WAP servers, then Azure AD Connect asks for credentials that have administrative privilege on hello WAP servers.</span></span>

* <span data-ttu-id="8246a-149">**hello-server visas som offline. Vad ska jag göra?**</span><span class="sxs-lookup"><span data-stu-id="8246a-149">**hello server is shown as offline. What should I do?**</span></span>

    <span data-ttu-id="8246a-150">Azure AD Connect kan inte utföra någon åtgärd om hello-servern är offline.</span><span class="sxs-lookup"><span data-stu-id="8246a-150">Azure AD Connect can't perform any operation if hello server is offline.</span></span> <span data-ttu-id="8246a-151">Om hello server är en del av hello AD FS-servergrupp, kontrollerar du hello anslutningen toohello server.</span><span class="sxs-lookup"><span data-stu-id="8246a-151">If hello server is part of hello AD FS farm, then check hello connectivity toohello server.</span></span> <span data-ttu-id="8246a-152">När du har löst problemet hello, trycker du på hello uppdatera ikonen tooupdate hello status i hello guiden.</span><span class="sxs-lookup"><span data-stu-id="8246a-152">After you've resolved hello issue, press hello refresh icon tooupdate hello status in hello wizard.</span></span> <span data-ttu-id="8246a-153">Om hello servern var en del av hello servergruppen tidigare men nu finns inte längre, klickar på **ta bort** toodelete den hello listan över servrar som Azure AD Connect upprätthåller.</span><span class="sxs-lookup"><span data-stu-id="8246a-153">If hello server was part of hello farm earlier but now no longer exists, click **Remove** toodelete it from hello list of servers that Azure AD Connect maintains.</span></span> <span data-ttu-id="8246a-154">Ta bort hello server hello listan i Azure AD Connect ändra inte hello AD FS-konfigurationen sig själv.</span><span class="sxs-lookup"><span data-stu-id="8246a-154">Removing hello server from hello list in Azure AD Connect doesn't alter hello AD FS configuration itself.</span></span> <span data-ttu-id="8246a-155">Om du använder AD FS i Windows Server 2016 eller senare, hello server finns kvar i hello konfigurationsinställningar och kommer att visas igen körs hello nästa gång hello aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="8246a-155">If you're using AD FS in Windows Server 2016 or later, hello server remains in hello configuration settings and will be shown again hello next time hello task is run.</span></span>

* <span data-ttu-id="8246a-156">**Kan jag uppdatera en delmängd av servrarna i servergruppen med hello nytt SSL-certifikat?**</span><span class="sxs-lookup"><span data-stu-id="8246a-156">**Can I update a subset of my farm servers with hello new SSL certificate?**</span></span>

    <span data-ttu-id="8246a-157">Ja.</span><span class="sxs-lookup"><span data-stu-id="8246a-157">Yes.</span></span> <span data-ttu-id="8246a-158">Du kan alltid köra hello uppgiften **uppdatering SSL-certifikat** igen tooupdate hello återstående servrar.</span><span class="sxs-lookup"><span data-stu-id="8246a-158">You can always run hello task **Update SSL Certificate** again tooupdate hello remaining servers.</span></span> <span data-ttu-id="8246a-159">På hello **väljer du servrar för SSL-certifikatet uppdatering** sidan kan du sortera hello listan över servrar **SSL utgångsdatum** tooeasily hello klientåtkomstservrar som ännu inte har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="8246a-159">On hello **Select servers for SSL certificate update** page, you can sort hello list of servers on **SSL Expiry date** tooeasily access hello servers that aren't updated yet.</span></span>

* <span data-ttu-id="8246a-160">**Jag bort hello servern i hello tidigare kör, men det fortfarande visas som offline och visas på sidan för hello AD FS-servrarna. Varför är hello kvar offline servern även när du tagit bort det?**</span><span class="sxs-lookup"><span data-stu-id="8246a-160">**I removed hello server in hello previous run, but it's still being shown as offline and listed on hello AD FS Servers page. Why is hello offline server still there even after I removed it?**</span></span>

    <span data-ttu-id="8246a-161">Ta bort hello server hello listan i Azure AD Connect inte ta bort den i hello AD FS-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="8246a-161">Removing hello server from hello list in Azure AD Connect doesn't remove it in hello AD FS configuration.</span></span> <span data-ttu-id="8246a-162">Azure AD Connect refererar till AD FS (Windows Server 2016 eller senare) för information om hello servergruppen.</span><span class="sxs-lookup"><span data-stu-id="8246a-162">Azure AD Connect references AD FS (Windows Server 2016 or higher) for any information about hello farm.</span></span> <span data-ttu-id="8246a-163">Om hello server fortfarande är kvar i hello AD FS-konfigurationen, visas den i hello lista.</span><span class="sxs-lookup"><span data-stu-id="8246a-163">If hello server is still present in hello AD FS configuration, it will be listed back in hello list.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="8246a-164">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8246a-164">Next steps</span></span>

- [<span data-ttu-id="8246a-165">Azure AD Connect och federation</span><span class="sxs-lookup"><span data-stu-id="8246a-165">Azure AD Connect and federation</span></span>](active-directory-aadconnectfed-whatis.md)
- [<span data-ttu-id="8246a-166">Active Directory Federation Services-hantering och anpassning med Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="8246a-166">Active Directory Federation Services management and customization with Azure AD Connect</span></span>](active-directory-aadconnect-federation-management.md)
