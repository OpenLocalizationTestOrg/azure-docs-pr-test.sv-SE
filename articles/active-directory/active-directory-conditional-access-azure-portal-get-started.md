---
title: "Kom igång med villkorlig åtkomst i Azure Active Directory | Microsoft Docs"
description: "Testa villkorlig åtkomst med ett villkor för platsen."
services: active-directory
keywords: "villkorlig åtkomst till appar, villkorlig åtkomst med Azure AD, säker åtkomst till företagets resurser, principer för villkorlig åtkomst"
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
ms.openlocfilehash: cd53e8be32d1e98aaf9f72177895871dba69df86
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="get-started-with-conditional-access-in-azure-active-directory"></a><span data-ttu-id="8d6c0-104">Kom igång med villkorlig åtkomst i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8d6c0-104">Get started with conditional access in Azure Active Directory</span></span>

<span data-ttu-id="8d6c0-105">Villkorlig åtkomst är en funktion i Azure Active Directory som gör att du kan definiera villkor under vilka behöriga användare kan komma åt dina appar.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-105">Conditional access is a capability of Azure Active Directory that enables you to define conditions under which authorized users can access your apps.</span></span> 

<span data-ttu-id="8d6c0-106">Det här avsnittet innehåller anvisningar för att testa en villkorad tillgång baserat på en plats-villkor i din miljö.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-106">This topic provides you with instructions for testing a conditional access based on a location condition in your environment.</span></span>  


## <a name="scenario-description"></a><span data-ttu-id="8d6c0-107">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="8d6c0-107">Scenario description</span></span>

<span data-ttu-id="8d6c0-108">Ett vanligt krav i många organisationer är att bara kräver Multi-Factor authentication för åtkomst till appar som inte utförs från företagets intranät.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-108">One common requirement in many organizations is to only require multi-factor authentication for access to apps that is not performed from the corporate intranet.</span></span> <span data-ttu-id="8d6c0-109">Du kan enkelt göra det här målet genom att konfigurera en princip för plats-baserad villkorlig åtkomst med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-109">With Azure Active Directory, you can easily accomplish this goal by configuring a location-based conditional access policy.</span></span> <span data-ttu-id="8d6c0-110">Det här avsnittet ger detaljerade anvisningar för att konfigurera en relaterad metod.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-110">This topic provides you with detailed instructions for configuring a related policy.</span></span> <span data-ttu-id="8d6c0-111">Principen använder [tillförlitliga IP-adresser](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips) har alla andra platser i intranätet och för att särskilja åtkomstförsök från företaget.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-111">The policy leverages [Trusted IPs](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips) to distinguish between access attempts made from the corporate's intranet and all other locations.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="8d6c0-112">Krav</span><span class="sxs-lookup"><span data-stu-id="8d6c0-112">Prerequisites</span></span>

<span data-ttu-id="8d6c0-113">Det scenario som beskrivs i det här avsnittet förutsätter att du är bekant med principerna som beskrivs i [villkorlig åtkomst i Azure Active Directory](active-directory-conditional-access-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="8d6c0-113">The scenario outlined in this topic assumes that you are familiar with the concepts outlined in [Azure Active Directory conditional access](active-directory-conditional-access-azure-portal.md).</span></span>

<span data-ttu-id="8d6c0-114">Om du vill testa det här scenariot måste du:</span><span class="sxs-lookup"><span data-stu-id="8d6c0-114">To test this scenario, you need to:</span></span>

- <span data-ttu-id="8d6c0-115">Skapa en testanvändare</span><span class="sxs-lookup"><span data-stu-id="8d6c0-115">Create a test user</span></span> 

- <span data-ttu-id="8d6c0-116">Tilldela en Azure AD Premium-licens till testanvändaren</span><span class="sxs-lookup"><span data-stu-id="8d6c0-116">Assign an Azure AD Premium license to the test user</span></span>

- <span data-ttu-id="8d6c0-117">Konfigurera en hanterad app och tilldela den din testanvändare</span><span class="sxs-lookup"><span data-stu-id="8d6c0-117">Configure a managed app and assign your test user to it</span></span>

- <span data-ttu-id="8d6c0-118">Konfigurera tillförlitliga IP-adresser</span><span class="sxs-lookup"><span data-stu-id="8d6c0-118">Configure trusted IPs</span></span>

<span data-ttu-id="8d6c0-119">Om du behöver mer information om betrodda IP-adresser, se [tillförlitliga IP-adresser](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).</span><span class="sxs-lookup"><span data-stu-id="8d6c0-119">If you need more details about Trusted IPs, see [Trusted IPs](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).</span></span>


## <a name="policy-configuration-steps"></a><span data-ttu-id="8d6c0-120">Konfigurationssteg för principen</span><span class="sxs-lookup"><span data-stu-id="8d6c0-120">Policy configuration steps</span></span>

<span data-ttu-id="8d6c0-121">**Om du vill konfigurera din princip för villkorlig åtkomst, gör du:**</span><span class="sxs-lookup"><span data-stu-id="8d6c0-121">**To configure your conditional access policy, do:**</span></span>

1. <span data-ttu-id="8d6c0-122">I Azure-portalen på den vänstra navigeringsfält för, klickar du på **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-122">In the Azure portal, on the left navbar, click **Azure Active Directory**.</span></span> 

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/01.png)

2. <span data-ttu-id="8d6c0-124">På den **Azure Active Directory** blad i den **hantera** klickar du på **villkorlig åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-124">On the **Azure Active Directory** blade, in the **Manage** section, click **Conditional access**.</span></span>

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/02.png)
 
3. <span data-ttu-id="8d6c0-126">På den **villkorlig åtkomst** bladet för att öppna den **ny** bladet i verktygsfältet högst upp, klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-126">On the **Conditional Access** blade, to open the **New** blade, in the toolbar on the top, click **Add**.</span></span>

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/03.png)

4. <span data-ttu-id="8d6c0-128">På den **ny** blad i den **namn** textruta, ange ett namn för principen.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-128">On the **New** blade, in the **Name** textbox, type a name for your policy.</span></span>

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/04.png)

5. <span data-ttu-id="8d6c0-130">I den **tilldelning** klickar du på **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-130">In the **Assignment** section, click **Users and groups**.</span></span>

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/05.png)

6. <span data-ttu-id="8d6c0-132">På den **användare och grupper** bladet utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="8d6c0-132">On the **Users and groups** blade, perform the following steps:</span></span>

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/06.png)

    <span data-ttu-id="8d6c0-134">a.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-134">a.</span></span> <span data-ttu-id="8d6c0-135">Klicka på **Välj användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-135">Click **Select users and groups**.</span></span>

    <span data-ttu-id="8d6c0-136">b.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-136">b.</span></span> <span data-ttu-id="8d6c0-137">Klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-137">Click **Select**.</span></span>

    <span data-ttu-id="8d6c0-138">c.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-138">c.</span></span> <span data-ttu-id="8d6c0-139">På den **Välj** bladet Välj din testanvändare och klicka sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-139">On the **Select** blade, select your test user, and then click **Select**.</span></span>

    <span data-ttu-id="8d6c0-140">d.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-140">d.</span></span> <span data-ttu-id="8d6c0-141">På den **användare och grupper** bladet, klickar du på **klar**.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-141">On the **Users and groups** blade, click **Done**.</span></span>

7. <span data-ttu-id="8d6c0-142">På den **ny** bladet för att öppna den **Molnappar** blad i den **tilldelning** klickar du på **Molnappar**.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-142">On the **New** blade, to open the **Cloud apps** blade, in the **Assignment** section, click **Cloud apps**.</span></span>

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/07.png)

8. <span data-ttu-id="8d6c0-144">På den **Molnappar** bladet utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="8d6c0-144">On the **Cloud apps** blade, perform the following steps:</span></span>

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/08.png)

    <span data-ttu-id="8d6c0-146">a.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-146">a.</span></span> <span data-ttu-id="8d6c0-147">Klicka på **Välj appar**.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-147">Click **Select apps**.</span></span>

    <span data-ttu-id="8d6c0-148">b.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-148">b.</span></span> <span data-ttu-id="8d6c0-149">Klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-149">Click **Select**.</span></span>

    <span data-ttu-id="8d6c0-150">c.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-150">c.</span></span> <span data-ttu-id="8d6c0-151">På den **Välj** bladet Välj en molnapp i och klicka sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-151">On the **Select** blade, select your cloud app, and then click **Select**.</span></span>

    <span data-ttu-id="8d6c0-152">d.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-152">d.</span></span> <span data-ttu-id="8d6c0-153">På den **Molnappar** bladet, klickar du på **klar**.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-153">On the **Cloud apps** blade, click **Done**.</span></span>

9. <span data-ttu-id="8d6c0-154">På den **ny** bladet för att öppna den **villkor** blad i den **tilldelning** klickar du på **villkor**.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-154">On the **New** blade, to open the **Conditions** blade, in the **Assignment** section, click **Conditions**.</span></span>

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/09.png)

10. <span data-ttu-id="8d6c0-156">På den **villkor** bladet för att öppna den **platser** bladet, klickar du på **platser**.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-156">On the **Conditions** blade, to open the **Locations** blade, click **Locations**.</span></span>

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/10.png)

11. <span data-ttu-id="8d6c0-158">På den **platser** bladet utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="8d6c0-158">On the **Locations** blade, perform the following steps:</span></span>

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/11.png)

    <span data-ttu-id="8d6c0-160">a.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-160">a.</span></span> <span data-ttu-id="8d6c0-161">Under **konfigurera**, klickar du på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-161">Under **Configure**, click **Yes**.</span></span>

    <span data-ttu-id="8d6c0-162">b.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-162">b.</span></span> <span data-ttu-id="8d6c0-163">Under **inkludera**, klickar du på **alla platser**.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-163">Under **Include**, click **All locations**.</span></span>

    <span data-ttu-id="8d6c0-164">c.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-164">c.</span></span> <span data-ttu-id="8d6c0-165">Klicka på **undanta**, och klicka sedan på **alla betrodda IP-adresser**.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-165">Click **Exclude**, and then click **All trusted IPs**.</span></span>

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/12.png)

    <span data-ttu-id="8d6c0-167">d.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-167">d.</span></span> <span data-ttu-id="8d6c0-168">Klicka på **Klar**.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-168">Click **Done**.</span></span>

12. <span data-ttu-id="8d6c0-169">På den **villkor** bladet, klickar du på **klar**.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-169">On the **Conditions** blade, click **Done**.</span></span>

13. <span data-ttu-id="8d6c0-170">På den **ny** bladet för att öppna den **bevilja** blad i den **kontroller** klickar du på **bevilja**.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-170">On the **New** blade, to open the **Grant** blade, in the **Controls** section, click **Grant**.</span></span>

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/13.png)

14. <span data-ttu-id="8d6c0-172">På den **bevilja** bladet utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="8d6c0-172">On the **Grant** blade, perform the following steps:</span></span>

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/14.png)

    <span data-ttu-id="8d6c0-174">a.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-174">a.</span></span> <span data-ttu-id="8d6c0-175">Välj **kräver Multi-Factor authentication**.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-175">Select **Require multi-factor authentication**.</span></span>

    <span data-ttu-id="8d6c0-176">b.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-176">b.</span></span> <span data-ttu-id="8d6c0-177">Klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-177">Click **Select**.</span></span>

15. <span data-ttu-id="8d6c0-178">På den **ny** bladet under **aktiverar principen**, klickar du på **på**.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-178">On the **New** blade, under **Enable policy**, click **On**.</span></span>

    ![Villkorlig åtkomst](./media/active-directory-conditional-access-azure-portal-get-started/15.png)

16. <span data-ttu-id="8d6c0-180">På den **ny** bladet, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-180">On the **New** blade, click **Create**.</span></span>


## <a name="testing-the-policy"></a><span data-ttu-id="8d6c0-181">Testa principen</span><span class="sxs-lookup"><span data-stu-id="8d6c0-181">Testing the policy</span></span>

<span data-ttu-id="8d6c0-182">Om du vill testa din princip bör du komma åt din app från en enhet som:</span><span class="sxs-lookup"><span data-stu-id="8d6c0-182">To test your policy, you should access your app from a device that:</span></span> 

1. <span data-ttu-id="8d6c0-183">Har en IP-adress som ligger inom den konfigurerade betrodda IP-adresser</span><span class="sxs-lookup"><span data-stu-id="8d6c0-183">Has an IP address that is within your configured Trusted IPs</span></span> 

1. <span data-ttu-id="8d6c0-184">Har en IP-adress som inte ligger inom den konfigurerade betrodda IP-adresser</span><span class="sxs-lookup"><span data-stu-id="8d6c0-184">Has an IP address that is not within your configured Trusted IPs</span></span>

<span data-ttu-id="8d6c0-185">Multifaktorautentisering bör endast krävas under ett anslutningsförsök gjordes från en enhet som inte ligger inom den konfigurerade betrodda IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="8d6c0-185">Multi-factor authentication should only be required during a connection attempt that was made from a device that is not within your configured Trusted IPs.</span></span> 


## <a name="next-steps"></a><span data-ttu-id="8d6c0-186">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8d6c0-186">Next steps</span></span>

<span data-ttu-id="8d6c0-187">Om du vill lära dig mer om villkorlig åtkomst kan se [villkorlig åtkomst i Azure Active Directory](active-directory-conditional-access-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="8d6c0-187">If you would like to learn more about conditional access, see [Azure Active Directory conditional access](active-directory-conditional-access-azure-portal.md).</span></span>

