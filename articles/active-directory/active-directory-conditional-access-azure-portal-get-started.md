---
title: "aaaGet igång med villkorlig åtkomst i Azure Active Directory | Microsoft Docs"
description: "Testa villkorlig åtkomst med ett villkor för platsen."
services: active-directory
keywords: "villkorlig åtkomst tooapps, villkorlig åtkomst med Azure AD, säker åtkomst toocompany resurser, principer för villkorlig åtkomst"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/31/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 4521f5a34f5882e026f5e58a7127d8c55cba2f0b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-conditional-access-in-azure-active-directory"></a><span data-ttu-id="17833-104">Kom igång med villkorlig åtkomst i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="17833-104">Get started with conditional access in Azure Active Directory</span></span>

<span data-ttu-id="17833-105">Villkorlig åtkomst är en funktion av Azure Active Directory som gör att du toodefine villkor under vilka behöriga användare kan komma åt dina appar.</span><span class="sxs-lookup"><span data-stu-id="17833-105">Conditional access is a capability of Azure Active Directory that enables you toodefine conditions under which authorized users can access your apps.</span></span> 

<span data-ttu-id="17833-106">Det här avsnittet innehåller anvisningar för att testa en villkorad tillgång baserat på en plats-villkor i din miljö.</span><span class="sxs-lookup"><span data-stu-id="17833-106">This topic provides you with instructions for testing a conditional access based on a location condition in your environment.</span></span>  


## <a name="scenario-description"></a><span data-ttu-id="17833-107">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="17833-107">Scenario description</span></span>

<span data-ttu-id="17833-108">Ett vanligt krav i många organisationer är tooonly kräver Multi-Factor authentication för åtkomst tooapps som inte utförs från hello företagsintranät.</span><span class="sxs-lookup"><span data-stu-id="17833-108">One common requirement in many organizations is tooonly require multi-factor authentication for access tooapps that is not performed from hello corporate intranet.</span></span> <span data-ttu-id="17833-109">Du kan enkelt göra det här målet genom att konfigurera en princip för plats-baserad villkorlig åtkomst med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="17833-109">With Azure Active Directory, you can easily accomplish this goal by configuring a location-based conditional access policy.</span></span> <span data-ttu-id="17833-110">Det här avsnittet ger detaljerade anvisningar för att konfigurera en relaterad metod.</span><span class="sxs-lookup"><span data-stu-id="17833-110">This topic provides you with detailed instructions for configuring a related policy.</span></span> <span data-ttu-id="17833-111">Hej princip använder [tillförlitliga IP-adresser](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips) toodistinguish mellan åtkomstförsök från hello företagets har alla andra platser i intranätet och.</span><span class="sxs-lookup"><span data-stu-id="17833-111">hello policy leverages [Trusted IPs](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips) toodistinguish between access attempts made from hello corporate's intranet and all other locations.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="17833-112">Krav</span><span class="sxs-lookup"><span data-stu-id="17833-112">Prerequisites</span></span>

<span data-ttu-id="17833-113">hello scenariot beskrivs i det här avsnittet förutsätter att du är bekant med hello begrepp som beskrivs i [villkorlig åtkomst i Azure Active Directory](active-directory-conditional-access-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="17833-113">hello scenario outlined in this topic assumes that you are familiar with hello concepts outlined in [Azure Active Directory conditional access](active-directory-conditional-access-azure-portal.md).</span></span>

<span data-ttu-id="17833-114">tootest i det här scenariot måste du:</span><span class="sxs-lookup"><span data-stu-id="17833-114">tootest this scenario, you need to:</span></span>

- <span data-ttu-id="17833-115">Skapa en testanvändare</span><span class="sxs-lookup"><span data-stu-id="17833-115">Create a test user</span></span> 

- <span data-ttu-id="17833-116">Tilldela en Azure AD Premium-licens toohello testanvändare</span><span class="sxs-lookup"><span data-stu-id="17833-116">Assign an Azure AD Premium license toohello test user</span></span>

- <span data-ttu-id="17833-117">Konfigurera en hanterad app och tilldela din test användaren tooit</span><span class="sxs-lookup"><span data-stu-id="17833-117">Configure a managed app and assign your test user tooit</span></span>

- <span data-ttu-id="17833-118">Konfigurera tillförlitliga IP-adresser</span><span class="sxs-lookup"><span data-stu-id="17833-118">Configure trusted IPs</span></span>

<span data-ttu-id="17833-119">Om du behöver mer information om betrodda IP-adresser, se [tillförlitliga IP-adresser](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).</span><span class="sxs-lookup"><span data-stu-id="17833-119">If you need more details about Trusted IPs, see [Trusted IPs](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).</span></span>


## <a name="policy-configuration-steps"></a><span data-ttu-id="17833-120">Konfigurationssteg för principen</span><span class="sxs-lookup"><span data-stu-id="17833-120">Policy configuration steps</span></span>

<span data-ttu-id="17833-121">**tooconfigure principen för villkorlig åtkomst gör du:**</span><span class="sxs-lookup"><span data-stu-id="17833-121">**tooconfigure your conditional access policy, do:**</span></span>

1. <span data-ttu-id="17833-122">I hello Azure-portalen på hello vänstra navigeringsfält för, klickar du på **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="17833-122">In hello Azure portal, on hello left navbar, click **Azure Active Directory**.</span></span> 

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/01.png)

2. <span data-ttu-id="17833-124">På hello **Azure Active Directory** bladet i hello **hantera** klickar du på **villkorlig åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="17833-124">On hello **Azure Active Directory** blade, in hello **Manage** section, click **Conditional access**.</span></span>

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/02.png)
 
3. <span data-ttu-id="17833-126">På hello **villkorlig åtkomst** bladet, tooopen hello **ny** bladet i hello verktygsfältet hello överst, klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="17833-126">On hello **Conditional Access** blade, tooopen hello **New** blade, in hello toolbar on hello top, click **Add**.</span></span>

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/03.png)

4. <span data-ttu-id="17833-128">På hello **ny** bladet i hello **namn** textruta, ange ett namn för principen.</span><span class="sxs-lookup"><span data-stu-id="17833-128">On hello **New** blade, in hello **Name** textbox, type a name for your policy.</span></span>

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/04.png)

5. <span data-ttu-id="17833-130">I hello **tilldelning** klickar du på **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="17833-130">In hello **Assignment** section, click **Users and groups**.</span></span>

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/05.png)

6. <span data-ttu-id="17833-132">På hello **användare och grupper** bladet utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="17833-132">On hello **Users and groups** blade, perform hello following steps:</span></span>

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/06.png)

    <span data-ttu-id="17833-134">a.</span><span class="sxs-lookup"><span data-stu-id="17833-134">a.</span></span> <span data-ttu-id="17833-135">Klicka på **Välj användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="17833-135">Click **Select users and groups**.</span></span>

    <span data-ttu-id="17833-136">b.</span><span class="sxs-lookup"><span data-stu-id="17833-136">b.</span></span> <span data-ttu-id="17833-137">Klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="17833-137">Click **Select**.</span></span>

    <span data-ttu-id="17833-138">c.</span><span class="sxs-lookup"><span data-stu-id="17833-138">c.</span></span> <span data-ttu-id="17833-139">På hello **Välj** bladet Välj din testanvändare och klicka sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="17833-139">On hello **Select** blade, select your test user, and then click **Select**.</span></span>

    <span data-ttu-id="17833-140">d.</span><span class="sxs-lookup"><span data-stu-id="17833-140">d.</span></span> <span data-ttu-id="17833-141">På hello **användare och grupper** bladet, klickar du på **klar**.</span><span class="sxs-lookup"><span data-stu-id="17833-141">On hello **Users and groups** blade, click **Done**.</span></span>

7. <span data-ttu-id="17833-142">På hello **ny** bladet, tooopen hello **Molnappar** bladet i hello **tilldelning** klickar du på **Molnappar**.</span><span class="sxs-lookup"><span data-stu-id="17833-142">On hello **New** blade, tooopen hello **Cloud apps** blade, in hello **Assignment** section, click **Cloud apps**.</span></span>

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/07.png)

8. <span data-ttu-id="17833-144">På hello **Molnappar** bladet utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="17833-144">On hello **Cloud apps** blade, perform hello following steps:</span></span>

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/08.png)

    <span data-ttu-id="17833-146">a.</span><span class="sxs-lookup"><span data-stu-id="17833-146">a.</span></span> <span data-ttu-id="17833-147">Klicka på **Välj appar**.</span><span class="sxs-lookup"><span data-stu-id="17833-147">Click **Select apps**.</span></span>

    <span data-ttu-id="17833-148">b.</span><span class="sxs-lookup"><span data-stu-id="17833-148">b.</span></span> <span data-ttu-id="17833-149">Klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="17833-149">Click **Select**.</span></span>

    <span data-ttu-id="17833-150">c.</span><span class="sxs-lookup"><span data-stu-id="17833-150">c.</span></span> <span data-ttu-id="17833-151">På hello **Välj** bladet Välj en molnapp i och klicka sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="17833-151">On hello **Select** blade, select your cloud app, and then click **Select**.</span></span>

    <span data-ttu-id="17833-152">d.</span><span class="sxs-lookup"><span data-stu-id="17833-152">d.</span></span> <span data-ttu-id="17833-153">På hello **Molnappar** bladet, klickar du på **klar**.</span><span class="sxs-lookup"><span data-stu-id="17833-153">On hello **Cloud apps** blade, click **Done**.</span></span>

9. <span data-ttu-id="17833-154">På hello **ny** bladet, tooopen hello **villkor** bladet i hello **tilldelning** klickar du på **villkor**.</span><span class="sxs-lookup"><span data-stu-id="17833-154">On hello **New** blade, tooopen hello **Conditions** blade, in hello **Assignment** section, click **Conditions**.</span></span>

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/09.png)

10. <span data-ttu-id="17833-156">På hello **villkor** bladet, tooopen hello **platser** bladet, klickar du på **platser**.</span><span class="sxs-lookup"><span data-stu-id="17833-156">On hello **Conditions** blade, tooopen hello **Locations** blade, click **Locations**.</span></span>

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/10.png)

11. <span data-ttu-id="17833-158">På hello **platser** bladet utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="17833-158">On hello **Locations** blade, perform hello following steps:</span></span>

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/11.png)

    <span data-ttu-id="17833-160">a.</span><span class="sxs-lookup"><span data-stu-id="17833-160">a.</span></span> <span data-ttu-id="17833-161">Under **konfigurera**, klickar du på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="17833-161">Under **Configure**, click **Yes**.</span></span>

    <span data-ttu-id="17833-162">b.</span><span class="sxs-lookup"><span data-stu-id="17833-162">b.</span></span> <span data-ttu-id="17833-163">Under **inkludera**, klickar du på **alla platser**.</span><span class="sxs-lookup"><span data-stu-id="17833-163">Under **Include**, click **All locations**.</span></span>

    <span data-ttu-id="17833-164">c.</span><span class="sxs-lookup"><span data-stu-id="17833-164">c.</span></span> <span data-ttu-id="17833-165">Klicka på **undanta**, och klicka sedan på **alla betrodda IP-adresser**.</span><span class="sxs-lookup"><span data-stu-id="17833-165">Click **Exclude**, and then click **All trusted IPs**.</span></span>

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/12.png)

    <span data-ttu-id="17833-167">d.</span><span class="sxs-lookup"><span data-stu-id="17833-167">d.</span></span> <span data-ttu-id="17833-168">Klicka på **Klar**.</span><span class="sxs-lookup"><span data-stu-id="17833-168">Click **Done**.</span></span>

12. <span data-ttu-id="17833-169">På hello **villkor** bladet, klickar du på **klar**.</span><span class="sxs-lookup"><span data-stu-id="17833-169">On hello **Conditions** blade, click **Done**.</span></span>

13. <span data-ttu-id="17833-170">På hello **ny** bladet, tooopen hello **bevilja** bladet i hello **kontroller** klickar du på **bevilja**.</span><span class="sxs-lookup"><span data-stu-id="17833-170">On hello **New** blade, tooopen hello **Grant** blade, in hello **Controls** section, click **Grant**.</span></span>

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/13.png)

14. <span data-ttu-id="17833-172">På hello **bevilja** bladet utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="17833-172">On hello **Grant** blade, perform hello following steps:</span></span>

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/14.png)

    <span data-ttu-id="17833-174">a.</span><span class="sxs-lookup"><span data-stu-id="17833-174">a.</span></span> <span data-ttu-id="17833-175">Välj **kräver Multi-Factor authentication**.</span><span class="sxs-lookup"><span data-stu-id="17833-175">Select **Require multi-factor authentication**.</span></span>

    <span data-ttu-id="17833-176">b.</span><span class="sxs-lookup"><span data-stu-id="17833-176">b.</span></span> <span data-ttu-id="17833-177">Klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="17833-177">Click **Select**.</span></span>

15. <span data-ttu-id="17833-178">På hello **ny** bladet under **aktiverar principen**, klickar du på **på**.</span><span class="sxs-lookup"><span data-stu-id="17833-178">On hello **New** blade, under **Enable policy**, click **On**.</span></span>

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/15.png)

16. <span data-ttu-id="17833-180">På hello **ny** bladet, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="17833-180">On hello **New** blade, click **Create**.</span></span>


## <a name="testing-hello-policy"></a><span data-ttu-id="17833-181">Testar hello policy</span><span class="sxs-lookup"><span data-stu-id="17833-181">Testing hello policy</span></span>

<span data-ttu-id="17833-182">tootest din princip du ska komma åt din app från en enhet som:</span><span class="sxs-lookup"><span data-stu-id="17833-182">tootest your policy, you should access your app from a device that:</span></span> 

1. <span data-ttu-id="17833-183">Har en IP-adress som ligger inom den konfigurerade betrodda IP-adresser</span><span class="sxs-lookup"><span data-stu-id="17833-183">Has an IP address that is within your configured Trusted IPs</span></span> 

1. <span data-ttu-id="17833-184">Har en IP-adress som inte ligger inom den konfigurerade betrodda IP-adresser</span><span class="sxs-lookup"><span data-stu-id="17833-184">Has an IP address that is not within your configured Trusted IPs</span></span>

<span data-ttu-id="17833-185">Multifaktorautentisering bör endast krävas under ett anslutningsförsök gjordes från en enhet som inte ligger inom den konfigurerade betrodda IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="17833-185">Multi-factor authentication should only be required during a connection attempt that was made from a device that is not within your configured Trusted IPs.</span></span> 


## <a name="next-steps"></a><span data-ttu-id="17833-186">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="17833-186">Next steps</span></span>

<span data-ttu-id="17833-187">Om du vill läsa mer om villkorlig åtkomst toolearn finns [villkorlig åtkomst i Azure Active Directory](active-directory-conditional-access-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="17833-187">If you would like toolearn more about conditional access, see [Azure Active Directory conditional access](active-directory-conditional-access-azure-portal.md).</span></span>

