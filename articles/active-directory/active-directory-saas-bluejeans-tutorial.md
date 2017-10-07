---
title: "Självstudier: Azure Active Directory-integrering med BlueJeans | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och BlueJeans."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: dfc634fd-1b55-4ba8-94a8-b8288429b6a9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: jeedes
ms.openlocfilehash: 67613303a9f854afbf4619418cc1607d329caf94
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bluejeans"></a><span data-ttu-id="a0a62-103">Självstudier: Azure Active Directory-integrering med BlueJeans</span><span class="sxs-lookup"><span data-stu-id="a0a62-103">Tutorial: Azure Active Directory integration with BlueJeans</span></span>

<span data-ttu-id="a0a62-104">I kursen får du lära dig hur toointegrate BlueJeans med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="a0a62-104">In this tutorial, you learn how toointegrate BlueJeans with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a0a62-105">Integrera BlueJeans med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="a0a62-105">Integrating BlueJeans with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="a0a62-106">Du kan styra i Azure AD som har åtkomst till tooBlueJeans</span><span class="sxs-lookup"><span data-stu-id="a0a62-106">You can control in Azure AD who has access tooBlueJeans</span></span>
- <span data-ttu-id="a0a62-107">Du kan aktivera din användare tooautomatically get inloggade tooBlueJeans (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="a0a62-107">You can enable your users tooautomatically get signed-on tooBlueJeans (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a0a62-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="a0a62-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="a0a62-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a0a62-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a0a62-110">Krav</span><span class="sxs-lookup"><span data-stu-id="a0a62-110">Prerequisites</span></span>

<span data-ttu-id="a0a62-111">tooconfigure Azure AD-integrering med BlueJeans, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="a0a62-111">tooconfigure Azure AD integration with BlueJeans, you need hello following items:</span></span>

- <span data-ttu-id="a0a62-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="a0a62-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a0a62-113">En BlueJeans enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="a0a62-113">A BlueJeans single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a0a62-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="a0a62-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a0a62-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="a0a62-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a0a62-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="a0a62-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a0a62-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a0a62-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a0a62-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="a0a62-118">Scenario description</span></span>
<span data-ttu-id="a0a62-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="a0a62-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a0a62-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="a0a62-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a0a62-121">Att lägga till BlueJeans från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="a0a62-121">Adding BlueJeans from hello gallery</span></span>
2. <span data-ttu-id="a0a62-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a0a62-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-bluejeans-from-hello-gallery"></a><span data-ttu-id="a0a62-123">Att lägga till BlueJeans från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="a0a62-123">Adding BlueJeans from hello gallery</span></span>
<span data-ttu-id="a0a62-124">tooconfigure hello integrering av BlueJeans i Azure AD, behöver du tooadd BlueJeans hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="a0a62-124">tooconfigure hello integration of BlueJeans into Azure AD, you need tooadd BlueJeans from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="a0a62-125">**tooadd BlueJeans från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a0a62-125">**tooadd BlueJeans from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="a0a62-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a0a62-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a0a62-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="a0a62-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="a0a62-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="a0a62-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="a0a62-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a0a62-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="a0a62-133">Skriv i sökrutan hello **BlueJeans**.</span><span class="sxs-lookup"><span data-stu-id="a0a62-133">In hello search box, type **BlueJeans**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_search.png)

5. <span data-ttu-id="a0a62-135">Markera hello resultat på panelen **BlueJeans**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="a0a62-135">In hello results panel, select **BlueJeans**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a0a62-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a0a62-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a0a62-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med BlueJeans baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="a0a62-138">In this section, you configure and test Azure AD single sign-on with BlueJeans based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="a0a62-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i BlueJeans är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a0a62-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in BlueJeans is tooa user in Azure AD.</span></span> <span data-ttu-id="a0a62-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i BlueJeans toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="a0a62-140">In other words, a link relationship between an Azure AD user and hello related user in BlueJeans needs toobe established.</span></span>

<span data-ttu-id="a0a62-141">I BlueJeans, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="a0a62-141">In BlueJeans, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="a0a62-142">tooconfigure och testa Azure AD enkel inloggning med BlueJeans, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="a0a62-142">tooconfigure and test Azure AD single sign-on with BlueJeans, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="a0a62-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="a0a62-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="a0a62-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a0a62-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a0a62-145">**[Skapa en testanvändare BlueJeans](#creating-a-bluejeans-test-user)**  -toohave en motsvarighet för Britta Simon i BlueJeans som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="a0a62-145">**[Creating a BlueJeans test user](#creating-a-bluejeans-test-user)** - toohave a counterpart of Britta Simon in BlueJeans that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="a0a62-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a0a62-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a0a62-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="a0a62-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a0a62-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a0a62-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a0a62-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt BlueJeans program.</span><span class="sxs-lookup"><span data-stu-id="a0a62-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your BlueJeans application.</span></span>

<span data-ttu-id="a0a62-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med BlueJeans:**</span><span class="sxs-lookup"><span data-stu-id="a0a62-150">**tooconfigure Azure AD single sign-on with BlueJeans, perform hello following steps:**</span></span>

1. <span data-ttu-id="a0a62-151">I hello Azure-portalen på hello **BlueJeans** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="a0a62-151">In hello Azure portal, on hello **BlueJeans** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="a0a62-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a0a62-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_samlbase.png)

3. <span data-ttu-id="a0a62-155">På hello **BlueJeans domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a0a62-155">On hello **BlueJeans Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_url.png)

    <span data-ttu-id="a0a62-157">a.</span><span class="sxs-lookup"><span data-stu-id="a0a62-157">a.</span></span> <span data-ttu-id="a0a62-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.BlueJeans.com`</span><span class="sxs-lookup"><span data-stu-id="a0a62-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.BlueJeans.com`</span></span>

    <span data-ttu-id="a0a62-159">b.</span><span class="sxs-lookup"><span data-stu-id="a0a62-159">b.</span></span> <span data-ttu-id="a0a62-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.BlueJeans.com`</span><span class="sxs-lookup"><span data-stu-id="a0a62-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.BlueJeans.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="a0a62-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="a0a62-161">These values are not real.</span></span> <span data-ttu-id="a0a62-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="a0a62-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="a0a62-163">Kontakta [BlueJeans klienten supportteamet](https://support.bluejeans.com/contact) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="a0a62-163">Contact [BlueJeans Client support team](https://support.bluejeans.com/contact) tooget these values.</span></span> 
 
4. <span data-ttu-id="a0a62-164">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="a0a62-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_certificate.png) 

5. <span data-ttu-id="a0a62-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="a0a62-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bluejeans-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="a0a62-168">På hello **BlueJeans Configuration** klickar du på **konfigurera BlueJeans** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="a0a62-168">On hello **BlueJeans Configuration** section, click **Configure BlueJeans** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="a0a62-169">Kopiera hello **Sign-Out URL, ändra lösenord URL och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="a0a62-169">Copy hello **Sign-Out URL, Change Password URL and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_configure.png) 

7. <span data-ttu-id="a0a62-171">Logga in tooyour i ett annat webbläsarfönster **BlueJeans** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="a0a62-171">In a different web browser window, log in tooyour **BlueJeans** company site as an administrator.</span></span>

8. <span data-ttu-id="a0a62-172">Gå för**ADMIN \> gruppinställningar \> säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="a0a62-172">Go too**ADMIN \> Group Settings \> Security**.</span></span>
   
   <span data-ttu-id="a0a62-173">![Admin](./media/active-directory-saas-bluejeans-tutorial/IC785868.png "Admin")</span><span class="sxs-lookup"><span data-stu-id="a0a62-173">![Admin](./media/active-directory-saas-bluejeans-tutorial/IC785868.png "Admin")</span></span>

9. <span data-ttu-id="a0a62-174">I hello **säkerhet** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a0a62-174">In hello **Security** section, perform hello following steps:</span></span>
   
   <span data-ttu-id="a0a62-175">![SAML för enkel inloggning](./media/active-directory-saas-bluejeans-tutorial/IC785869.png "SAML för enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="a0a62-175">![SAML Single Sign On](./media/active-directory-saas-bluejeans-tutorial/IC785869.png "SAML Single Sign On")</span></span>   
   
   <span data-ttu-id="a0a62-176">a.</span><span class="sxs-lookup"><span data-stu-id="a0a62-176">a.</span></span> <span data-ttu-id="a0a62-177">Välj **SAML för enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="a0a62-177">Select **SAML Single Sign On**.</span></span>
  
   <span data-ttu-id="a0a62-178">b.</span><span class="sxs-lookup"><span data-stu-id="a0a62-178">b.</span></span> <span data-ttu-id="a0a62-179">Välj **aktivera automatisk etablering**.</span><span class="sxs-lookup"><span data-stu-id="a0a62-179">Select **Enable automatic provisioning**.</span></span>

10. <span data-ttu-id="a0a62-180">Gå vidare med hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a0a62-180">Move on with hello following steps:</span></span>

    <span data-ttu-id="a0a62-181">![Certifikat sökvägen](./media/active-directory-saas-bluejeans-tutorial/IC785870.png "certifikat sökväg")</span><span class="sxs-lookup"><span data-stu-id="a0a62-181">![Certificate Path](./media/active-directory-saas-bluejeans-tutorial/IC785870.png "Certificate Path")</span></span>
    
    <span data-ttu-id="a0a62-182">a.</span><span class="sxs-lookup"><span data-stu-id="a0a62-182">a.</span></span> <span data-ttu-id="a0a62-183">Klicka på **Välj fil**, och sedan ladda upp hello hämtat certifikat.</span><span class="sxs-lookup"><span data-stu-id="a0a62-183">Click **Choose File**, and then upload hello downloaded certificate.</span></span>
   
    <span data-ttu-id="a0a62-184">b.</span><span class="sxs-lookup"><span data-stu-id="a0a62-184">b.</span></span> <span data-ttu-id="a0a62-185">Klistra in **SAML enkel inloggning Tjänstwebbadress** till hello **inloggnings-URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="a0a62-185">Paste **SAML Single Sign-On Service URL** into hello **Login URL** textbox.</span></span>
   
    <span data-ttu-id="a0a62-186">c.</span><span class="sxs-lookup"><span data-stu-id="a0a62-186">c.</span></span> <span data-ttu-id="a0a62-187">Klistra in **ändra lösenord URL** till hello **lösenord ändra URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="a0a62-187">Paste **Change Password URL** into hello **Password Change URL** textbox.</span></span>
   
    <span data-ttu-id="a0a62-188">d.</span><span class="sxs-lookup"><span data-stu-id="a0a62-188">d.</span></span> <span data-ttu-id="a0a62-189">Klistra in **Sign-Out URL** till hello **logga ut URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="a0a62-189">Paste **Sign-Out URL** into hello **Logout URL** textbox.</span></span>

11. <span data-ttu-id="a0a62-190">Gå vidare med hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a0a62-190">Move on with hello following steps:</span></span>
    
    <span data-ttu-id="a0a62-191">![Spara ändringarna](./media/active-directory-saas-bluejeans-tutorial/IC785874.png "spara ändringar")</span><span class="sxs-lookup"><span data-stu-id="a0a62-191">![Save Changes](./media/active-directory-saas-bluejeans-tutorial/IC785874.png "Save Changes")</span></span>
    
    <span data-ttu-id="a0a62-192">a.</span><span class="sxs-lookup"><span data-stu-id="a0a62-192">a.</span></span> <span data-ttu-id="a0a62-193">I hello **användar-id** textruta typen `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span><span class="sxs-lookup"><span data-stu-id="a0a62-193">In hello **User id** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span></span>
   
    <span data-ttu-id="a0a62-194">b.</span><span class="sxs-lookup"><span data-stu-id="a0a62-194">b.</span></span> <span data-ttu-id="a0a62-195">I hello **e-post** textruta typen `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span><span class="sxs-lookup"><span data-stu-id="a0a62-195">In hello **Email** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span></span>
   
    <span data-ttu-id="a0a62-196">c.</span><span class="sxs-lookup"><span data-stu-id="a0a62-196">c.</span></span> <span data-ttu-id="a0a62-197">Klicka på **spara ändringar**.</span><span class="sxs-lookup"><span data-stu-id="a0a62-197">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="a0a62-198">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="a0a62-198">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="a0a62-199">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="a0a62-199">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="a0a62-200">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a0a62-200">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a0a62-201">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="a0a62-201">Creating an Azure AD test user</span></span>
<span data-ttu-id="a0a62-202">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a0a62-202">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="a0a62-204">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a0a62-204">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="a0a62-205">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a0a62-205">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a0a62-207">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="a0a62-207">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a0a62-209">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a0a62-209">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a0a62-211">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a0a62-211">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a0a62-213">a.</span><span class="sxs-lookup"><span data-stu-id="a0a62-213">a.</span></span> <span data-ttu-id="a0a62-214">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a0a62-214">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a0a62-215">b.</span><span class="sxs-lookup"><span data-stu-id="a0a62-215">b.</span></span> <span data-ttu-id="a0a62-216">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a0a62-216">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a0a62-217">c.</span><span class="sxs-lookup"><span data-stu-id="a0a62-217">c.</span></span> <span data-ttu-id="a0a62-218">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="a0a62-218">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="a0a62-219">d.</span><span class="sxs-lookup"><span data-stu-id="a0a62-219">d.</span></span> <span data-ttu-id="a0a62-220">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="a0a62-220">Click **Create**.</span></span>
 
### <a name="creating-a-bluejeans-test-user"></a><span data-ttu-id="a0a62-221">Skapa en testanvändare BlueJeans</span><span class="sxs-lookup"><span data-stu-id="a0a62-221">Creating a BlueJeans test user</span></span>

<span data-ttu-id="a0a62-222">tooenable Azure AD-användare toolog i tooBlueJeans, måste de etableras i BlueJeans.</span><span class="sxs-lookup"><span data-stu-id="a0a62-222">tooenable Azure AD users toolog in tooBlueJeans, they must be provisioned into BlueJeans.</span></span>  

<span data-ttu-id="a0a62-223">Vid BlueJeans är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="a0a62-223">In case of BlueJeans, provisioning is a manual task.</span></span>

<span data-ttu-id="a0a62-224">**tooprovision användarkonton, utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a0a62-224">**tooprovision a user accounts, perform hello following steps:**</span></span>

1. <span data-ttu-id="a0a62-225">Logga in tooyour **BlueJeans** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="a0a62-225">Log in tooyour **BlueJeans** company site as an administrator.</span></span>

2. <span data-ttu-id="a0a62-226">Gå för**ADMIN \> hantera användare \> Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="a0a62-226">Go too**ADMIN \> Manage Users \> Add User**.</span></span>
   
   <span data-ttu-id="a0a62-227">![Admin](./media/active-directory-saas-bluejeans-tutorial/IC785877.png "Admin")</span><span class="sxs-lookup"><span data-stu-id="a0a62-227">![Admin](./media/active-directory-saas-bluejeans-tutorial/IC785877.png "Admin")</span></span>
   
   >[!IMPORTANT]
   ><span data-ttu-id="a0a62-228">Hej **Lägg till användare** fliken är endast tillgänglig om i hello **säkerhetsfliken**, **aktivera automatisk etablering** är avmarkerad.</span><span class="sxs-lookup"><span data-stu-id="a0a62-228">hello **Add User** tab is only available if, in hello **Security tab**, **Enable automatic provisioning** is unchecked.</span></span> 
   
3. <span data-ttu-id="a0a62-229">I hello **Lägg till användare** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a0a62-229">In hello **Add User** section, perform hello following steps:</span></span>

    <span data-ttu-id="a0a62-230">![Lägg till användare](./media/active-directory-saas-bluejeans-tutorial/IC785886.png "lägga till användare")</span><span class="sxs-lookup"><span data-stu-id="a0a62-230">![Add User](./media/active-directory-saas-bluejeans-tutorial/IC785886.png "Add User")</span></span>
    
    <span data-ttu-id="a0a62-231">a.</span><span class="sxs-lookup"><span data-stu-id="a0a62-231">a.</span></span> <span data-ttu-id="a0a62-232">Ange en **BlueJeans användarnamn**, en **e-postadress**, **BlueJeans möte ID**, **kontrollant lösenord**, en **fullständigt namn** , hello **företagets** av en giltig AAD-konto som du vill använda tooprovision hello relaterade textrutor.</span><span class="sxs-lookup"><span data-stu-id="a0a62-232">Type a **BlueJeans Username**, an **Email address**, a **BlueJeans Meeting ID**, a **Moderator Passcode**, a **Full Name**, hello **Company** of a valid AAD account you want tooprovision into hello related textboxes.</span></span>
    
    <span data-ttu-id="a0a62-233">b.</span><span class="sxs-lookup"><span data-stu-id="a0a62-233">b.</span></span> <span data-ttu-id="a0a62-234">Klicka på **lägga till användare**.</span><span class="sxs-lookup"><span data-stu-id="a0a62-234">Click **Add User**.</span></span>

>[!NOTE]
><span data-ttu-id="a0a62-235">Du kan använda något annat BlueJeans användarens konto skapas verktyg eller API: er som tillhandahålls av BlueJeans tooprovision AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="a0a62-235">You can use any other BlueJeans user account creation tools or APIs provided by BlueJeans tooprovision AAD user accounts.</span></span> 
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="a0a62-236">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="a0a62-236">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="a0a62-237">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooBlueJeans.</span><span class="sxs-lookup"><span data-stu-id="a0a62-237">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooBlueJeans.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="a0a62-239">**tooassign Britta Simon tooBlueJeans utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a0a62-239">**tooassign Britta Simon tooBlueJeans, perform hello following steps:**</span></span>

1. <span data-ttu-id="a0a62-240">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="a0a62-240">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="a0a62-242">Välj i listan med program hello **BlueJeans**.</span><span class="sxs-lookup"><span data-stu-id="a0a62-242">In hello applications list, select **BlueJeans**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_app.png) 

3. <span data-ttu-id="a0a62-244">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="a0a62-244">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="a0a62-246">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="a0a62-246">Click **Add** button.</span></span> <span data-ttu-id="a0a62-247">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a0a62-247">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="a0a62-249">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="a0a62-249">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="a0a62-250">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a0a62-250">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a0a62-251">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a0a62-251">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a0a62-252">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a0a62-252">Testing single sign-on</span></span>

<span data-ttu-id="a0a62-253">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="a0a62-253">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="a0a62-254">När du klickar på hello BlueJeans panelen i hello åtkomstpanelen bör du hämta inloggningssidan för BlueJeans program.</span><span class="sxs-lookup"><span data-stu-id="a0a62-254">When you click hello BlueJeans tile in hello Access Panel, you should get login page of BlueJeans application.</span></span>
<span data-ttu-id="a0a62-255">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a0a62-255">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="a0a62-256">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="a0a62-256">Additional resources</span></span>

* [<span data-ttu-id="a0a62-257">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a0a62-257">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a0a62-258">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a0a62-258">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_203.png

