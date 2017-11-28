---
title: aaaSecure molnresurser med Azure MFA och AD FS | Microsoft Docs
description: "Det här är hello Azure Multi-Factor authentication sida som beskriver hur tooget igång med Azure MFA och AD FS i hello molnet."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 0927fc67-8090-4fdd-913a-b3cfed3fbe77
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/29/2017
ms.author: kgremban
ms.openlocfilehash: 8d38d6a4af63ddcaf0fefded0b73d82d5178aa36
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="securing-cloud-resources-with-azure-multi-factor-authentication-and-ad-fs"></a><span data-ttu-id="175d3-103">Skydda molnresurser med Azure Multi-Factor Authentication och AD FS</span><span class="sxs-lookup"><span data-stu-id="175d3-103">Securing cloud resources with Azure Multi-Factor Authentication and AD FS</span></span>
<span data-ttu-id="175d3-104">Om din organisation är federerat med Azure Active Directory, använder du Azure Multi-Factor Authentication eller Active Directory Federation Services (AD FS) toosecure resurser som kan nås av Azure AD.</span><span class="sxs-lookup"><span data-stu-id="175d3-104">If your organization is federated with Azure Active Directory, use Azure Multi-Factor Authentication or Active Directory Federation Services (AD FS) toosecure resources that are accessed by Azure AD.</span></span> <span data-ttu-id="175d3-105">Använd följande procedurer toosecure Azure Active Directory-resurser med Azure Multi-Factor Authentication eller Active Directory Federation Services hello.</span><span class="sxs-lookup"><span data-stu-id="175d3-105">Use hello following procedures toosecure Azure Active Directory resources with either Azure Multi-Factor Authentication or Active Directory Federation Services.</span></span>

## <a name="secure-azure-ad-resources-using-ad-fs"></a><span data-ttu-id="175d3-106">Skydda Azure AD-resurser med hjälp av AD FS</span><span class="sxs-lookup"><span data-stu-id="175d3-106">Secure Azure AD resources using AD FS</span></span>
<span data-ttu-id="175d3-107">toosecure din molnresursen, konfigurera en regel för anspråk så att Active Directory Federation Services avger hello multipleauthn anspråk när en användare utför tvåstegsverifiering har.</span><span class="sxs-lookup"><span data-stu-id="175d3-107">toosecure your cloud resource, set up a claims rule so that Active Directory Federation Services emits hello multipleauthn claim when a user performs two-step verification successfully.</span></span> <span data-ttu-id="175d3-108">Denna begäran har skickats på tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="175d3-108">This claim is passed on tooAzure AD.</span></span> <span data-ttu-id="175d3-109">Följ den här proceduren toowalk hello stegen:</span><span class="sxs-lookup"><span data-stu-id="175d3-109">Follow this procedure toowalk through hello steps:</span></span>


1. <span data-ttu-id="175d3-110">Öppna AD FS-hantering.</span><span class="sxs-lookup"><span data-stu-id="175d3-110">Open AD FS Management.</span></span>
2. <span data-ttu-id="175d3-111">Hello vänster markerar **förtroende för förlitande part**.</span><span class="sxs-lookup"><span data-stu-id="175d3-111">On hello left, select **Relying Party Trusts**.</span></span>
3. <span data-ttu-id="175d3-112">Högerklicka på **Microsoft Office 365 Identity Platform** och välj **Redigera anspråksregler**.</span><span class="sxs-lookup"><span data-stu-id="175d3-112">Right-click on **Microsoft Office 365 Identity Platform** and select **Edit Claim Rules**.</span></span>

   ![Molnet](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip1.png)

4. <span data-ttu-id="175d3-114">För Utfärdande av transformeringsregler klickar du på **Lägg till regel**.</span><span class="sxs-lookup"><span data-stu-id="175d3-114">On Issuance Transform Rules, click **Add Rule**.</span></span>

   ![Molnet](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip2.png)

5. <span data-ttu-id="175d3-116">På Hej guiden Lägg till transformera anspråk, Välj **släpp igenom eller filtrera ett inkommande anspråk** hello listrutan och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="175d3-116">On hello Add Transform Claim Rule Wizard, select **Pass Through or Filter an Incoming Claim** from hello drop-down and click **Next**.</span></span>

   ![Molnet](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip3.png)

6. <span data-ttu-id="175d3-118">Namnge din regel.</span><span class="sxs-lookup"><span data-stu-id="175d3-118">Give your rule a name.</span></span> 
7. <span data-ttu-id="175d3-119">Välj **autentiseringsmetodernas referenser** hello inkommande anspråkstyp.</span><span class="sxs-lookup"><span data-stu-id="175d3-119">Select **Authentication Methods References** as hello Incoming claim type.</span></span>
8. <span data-ttu-id="175d3-120">Välj **Släpp igenom alla anspråksvärden**.</span><span class="sxs-lookup"><span data-stu-id="175d3-120">Select **Pass through all claim values**.</span></span>
    <span data-ttu-id="175d3-121">![Guiden Lägg till anspråksregel för transformering](./media/multi-factor-authentication-get-started-adfs-cloud/configurewizard.png)</span><span class="sxs-lookup"><span data-stu-id="175d3-121">![Add Transform Claim Rule Wizard](./media/multi-factor-authentication-get-started-adfs-cloud/configurewizard.png)</span></span>
9. <span data-ttu-id="175d3-122">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="175d3-122">Click **Finish**.</span></span> <span data-ttu-id="175d3-123">Stäng hello AD FS-hanteringskonsol.</span><span class="sxs-lookup"><span data-stu-id="175d3-123">Close hello AD FS Management console.</span></span>

## <a name="trusted-ips-for-federated-users"></a><span data-ttu-id="175d3-124">Tillförlitliga IP-adresser för federerade användare</span><span class="sxs-lookup"><span data-stu-id="175d3-124">Trusted IPs for federated users</span></span>
<span data-ttu-id="175d3-125">Tillförlitliga IP-adresser kan administratörer tooby pass tvåstegsverifiering för specifika IP-adresser eller externa användare som har begäranden från inom sin egen intranät.</span><span class="sxs-lookup"><span data-stu-id="175d3-125">Trusted IPs allow administrators tooby-pass two-step verification for specific IP addresses, or for federated users that have requests originating from within their own intranet.</span></span> <span data-ttu-id="175d3-126">hello följande avsnitt beskrivs hur tooconfigure Azure Multi-Factor Authentication tillförlitliga IP-adresser med externa användare och hoppa över tvåstegsverifiering när en begäran kommer från inom en federerad användare intranät.</span><span class="sxs-lookup"><span data-stu-id="175d3-126">hello following sections describe how tooconfigure Azure Multi-Factor Authentication Trusted IPs with federated users and by-pass two-step verification when a request originates from within a federated users intranet.</span></span> <span data-ttu-id="175d3-127">Detta uppnås genom att konfigurera AD FS toouse en direktlagringsdisk eller filtrera ett inkommande anspråk mallen med hello inuti företagsnätverket anspråkstyp.</span><span class="sxs-lookup"><span data-stu-id="175d3-127">This is achieved by configuring AD FS toouse a pass-through or filter an incoming claim template with hello Inside Corporate Network claim type.</span></span>

<span data-ttu-id="175d3-128">I det här exemplet används Office 365 för våra förlitande partsförtroenden.</span><span class="sxs-lookup"><span data-stu-id="175d3-128">This example uses Office 365 for our Relying Party Trusts.</span></span>

### <a name="configure-hello-ad-fs-claims-rules"></a><span data-ttu-id="175d3-129">Konfigurera regler för hello AD FS-anspråk</span><span class="sxs-lookup"><span data-stu-id="175d3-129">Configure hello AD FS claims rules</span></span>
<span data-ttu-id="175d3-130">hello första vi behöver toodo är tooconfigure hello AD FS-anspråk.</span><span class="sxs-lookup"><span data-stu-id="175d3-130">hello first thing we need toodo is tooconfigure hello AD FS claims.</span></span> <span data-ttu-id="175d3-131">Skapa två anspråksregler, en för hello inuti företagsnätverket Anspråkstypen och ytterligare en för att hålla våra användare som har loggat in.</span><span class="sxs-lookup"><span data-stu-id="175d3-131">Create two claims rules, one for hello Inside Corporate Network claim type and an additional one for keeping our users signed in.</span></span>

1. <span data-ttu-id="175d3-132">Öppna AD FS-hantering.</span><span class="sxs-lookup"><span data-stu-id="175d3-132">Open AD FS Management.</span></span>
2. <span data-ttu-id="175d3-133">Hello vänster markerar **förtroende för förlitande part**.</span><span class="sxs-lookup"><span data-stu-id="175d3-133">On hello left, select **Relying Party Trusts**.</span></span>
3. <span data-ttu-id="175d3-134">Högerklicka på **Microsoft Office 365-identitetsplattform** och välj **Redigera anspråksregler...**
   ![Molnet](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip1.png)</span><span class="sxs-lookup"><span data-stu-id="175d3-134">Right-click on **Microsoft Office 365 Identity Platform** and select **Edit Claim Rules…**
![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip1.png)</span></span>
4. <span data-ttu-id="175d3-135">För Utfärdande av transformeringsregler klickar du på **Lägg till regel.**
   ![Molnet](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip2.png)</span><span class="sxs-lookup"><span data-stu-id="175d3-135">On Issuance Transform Rules, click **Add Rule.**
![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip2.png)</span></span>
5. <span data-ttu-id="175d3-136">På Hej guiden Lägg till transformera anspråk, Välj **släpp igenom eller filtrera ett inkommande anspråk** hello listrutan och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="175d3-136">On hello Add Transform Claim Rule Wizard, select **Pass Through or Filter an Incoming Claim** from hello drop-down and click **Next**.</span></span>
   <span data-ttu-id="175d3-137">![Molnet](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip3.png)</span><span class="sxs-lookup"><span data-stu-id="175d3-137">![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip3.png)</span></span>
6. <span data-ttu-id="175d3-138">Hello rutan nästa tooClaim Regelnamn, ge regeln ett namn.</span><span class="sxs-lookup"><span data-stu-id="175d3-138">In hello box next tooClaim rule name, give your rule a name.</span></span> <span data-ttu-id="175d3-139">Exempel: InsideCorpNet.</span><span class="sxs-lookup"><span data-stu-id="175d3-139">For example: InsideCorpNet.</span></span>
7. <span data-ttu-id="175d3-140">Från hello nedrullningsbara nästa tooIncoming Anspråkstyp, Välj **inuti företagsnätverket**.</span><span class="sxs-lookup"><span data-stu-id="175d3-140">From hello drop-down, next tooIncoming claim type, select **Inside Corporate Network**.</span></span>
   <span data-ttu-id="175d3-141">![Molnet](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip4.png)</span><span class="sxs-lookup"><span data-stu-id="175d3-141">![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip4.png)</span></span>
8. <span data-ttu-id="175d3-142">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="175d3-142">Click **Finish**.</span></span>
9. <span data-ttu-id="175d3-143">För Utfärdande av transformeringsregler klickar du på **Lägg till regel**.</span><span class="sxs-lookup"><span data-stu-id="175d3-143">On Issuance Transform Rules, click **Add Rule**.</span></span>
10. <span data-ttu-id="175d3-144">På Hej guiden Lägg till transformera anspråk, Välj **skicka anspråk med en anpassad regel** hello listrutan och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="175d3-144">On hello Add Transform Claim Rule Wizard, select **Send Claims Using a Custom Rule** from hello drop-down and click **Next**.</span></span>
11. <span data-ttu-id="175d3-145">I rutan hello under Regelnamn för anspråk: Ange *hålla användare inloggad i*.</span><span class="sxs-lookup"><span data-stu-id="175d3-145">In hello box under Claim rule name: enter *Keep Users Signed In*.</span></span>
12. <span data-ttu-id="175d3-146">Skriv följande i hello anpassad regel:</span><span class="sxs-lookup"><span data-stu-id="175d3-146">In hello Custom rule box, enter:</span></span>

        c:[Type == "http://schemas.microsoft.com/2014/03/psso"]
            => issue(claim = c);
    ![Molnet](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip5.png)
13. <span data-ttu-id="175d3-148">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="175d3-148">Click **Finish**.</span></span>
14. <span data-ttu-id="175d3-149">Klicka på **Använd**.</span><span class="sxs-lookup"><span data-stu-id="175d3-149">Click **Apply**.</span></span>
15. <span data-ttu-id="175d3-150">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="175d3-150">Click **Ok**.</span></span>
16. <span data-ttu-id="175d3-151">Stäng AD FS-hantering.</span><span class="sxs-lookup"><span data-stu-id="175d3-151">Close AD FS Management.</span></span>

### <a name="configure-azure-multi-factor-authentication-trusted-ips-with-federated-users"></a><span data-ttu-id="175d3-152">Konfigurera tillförlitliga IP-adresser med federerade användare i Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="175d3-152">Configure Azure Multi-Factor Authentication Trusted IPs with Federated Users</span></span>
<span data-ttu-id="175d3-153">Nu när hello anspråk är på plats kan konfigurera vi tillförlitliga IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="175d3-153">Now that hello claims are in place, we can configure trusted IPs.</span></span>

1. <span data-ttu-id="175d3-154">Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="175d3-154">Sign in toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="175d3-155">Klicka på vänster hello **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="175d3-155">On hello left, click **Active Directory**.</span></span>
3. <span data-ttu-id="175d3-156">Välj hello katalog där du vill tooset in tillförlitliga IP-adresser under katalog.</span><span class="sxs-lookup"><span data-stu-id="175d3-156">Under Directory, select hello directory where you want tooset up trusted IPs.</span></span>
4. <span data-ttu-id="175d3-157">Klicka på hello katalog som du har valt, **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="175d3-157">On hello Directory you have selected, click **Configure**.</span></span>
5. <span data-ttu-id="175d3-158">På hello multifaktorautentisering under **hantera tjänstinställningar**.</span><span class="sxs-lookup"><span data-stu-id="175d3-158">In hello multi-factor authentication section, click **Manage service settings**.</span></span>
6. <span data-ttu-id="175d3-159">Hello-tjänsten på sidan Inställningar under tillförlitliga IP-adresser, Välj **hoppa över flera-factor-autentisering för förfrågningar från externa användare i intranätet**.</span><span class="sxs-lookup"><span data-stu-id="175d3-159">On hello Service Settings page, under trusted IPs, select **Skip multi-factor-authentication for requests from federated users on my intranet**.</span></span>  

   ![Molnet](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip6.png)
   
7. <span data-ttu-id="175d3-161">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="175d3-161">Click **save**.</span></span>
8. <span data-ttu-id="175d3-162">När hello-uppdateringarna har tillämpats, klickar du på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="175d3-162">Once hello updates have been applied, click **close**.</span></span>

<span data-ttu-id="175d3-163">Klart!</span><span class="sxs-lookup"><span data-stu-id="175d3-163">That’s it!</span></span> <span data-ttu-id="175d3-164">Federerade Office 365-användare ska nu endast ha toouse MFA när ett anspråk som kommer från utanför hello företagets intranät.</span><span class="sxs-lookup"><span data-stu-id="175d3-164">At this point, federated Office 365 users should only have toouse MFA when a claim originates from outside hello corporate intranet.</span></span>
