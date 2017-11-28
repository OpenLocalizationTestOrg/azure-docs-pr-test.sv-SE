---
title: "Självstudier: Azure Active Directory-integrering med Freshservice | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Freshservice."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3dd22b1f-445d-45c6-8eda-30207eb9a1a8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: d73624b87d058f66885ae72fda69a0aacc89c1ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-freshservice"></a><span data-ttu-id="95478-103">Självstudier: Azure Active Directory-integrering med Freshservice</span><span class="sxs-lookup"><span data-stu-id="95478-103">Tutorial: Azure Active Directory integration with Freshservice</span></span>

<span data-ttu-id="95478-104">I kursen får du lära dig hur toointegrate Freshservice med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="95478-104">In this tutorial, you learn how toointegrate Freshservice with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="95478-105">Integrera Freshservice med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="95478-105">Integrating Freshservice with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="95478-106">Du kan styra i Azure AD som har åtkomst till tooFreshservice</span><span class="sxs-lookup"><span data-stu-id="95478-106">You can control in Azure AD who has access tooFreshservice</span></span>
- <span data-ttu-id="95478-107">Du kan aktivera din användare tooautomatically get inloggade tooFreshservice (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="95478-107">You can enable your users tooautomatically get signed-on tooFreshservice (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="95478-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="95478-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="95478-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="95478-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="95478-110">Krav</span><span class="sxs-lookup"><span data-stu-id="95478-110">Prerequisites</span></span>

<span data-ttu-id="95478-111">tooconfigure Azure AD-integrering med Freshservice, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="95478-111">tooconfigure Azure AD integration with Freshservice, you need hello following items:</span></span>

- <span data-ttu-id="95478-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="95478-112">An Azure AD subscription</span></span>
- <span data-ttu-id="95478-113">En Freshservice enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="95478-113">A Freshservice single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="95478-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="95478-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="95478-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="95478-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="95478-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="95478-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="95478-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="95478-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="95478-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="95478-118">Scenario description</span></span>
<span data-ttu-id="95478-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="95478-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="95478-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="95478-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="95478-121">Att lägga till Freshservice från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="95478-121">Adding Freshservice from hello gallery</span></span>
2. <span data-ttu-id="95478-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="95478-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-freshservice-from-hello-gallery"></a><span data-ttu-id="95478-123">Att lägga till Freshservice från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="95478-123">Adding Freshservice from hello gallery</span></span>
<span data-ttu-id="95478-124">tooconfigure hello integrering av Freshservice i Azure AD, behöver du tooadd Freshservice hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="95478-124">tooconfigure hello integration of Freshservice into Azure AD, you need tooadd Freshservice from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="95478-125">**tooadd Freshservice från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="95478-125">**tooadd Freshservice from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="95478-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="95478-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="95478-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="95478-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="95478-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="95478-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="95478-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="95478-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="95478-133">Skriv i sökrutan hello **Freshservice**.</span><span class="sxs-lookup"><span data-stu-id="95478-133">In hello search box, type **Freshservice**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_search.png)

5. <span data-ttu-id="95478-135">Markera hello resultat på panelen **Freshservice**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="95478-135">In hello results panel, select **Freshservice**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="95478-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="95478-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="95478-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Freshservice baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="95478-138">In this section, you configure and test Azure AD single sign-on with Freshservice based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="95478-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Freshservice är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="95478-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Freshservice is tooa user in Azure AD.</span></span> <span data-ttu-id="95478-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Freshservice toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="95478-140">In other words, a link relationship between an Azure AD user and hello related user in Freshservice needs toobe established.</span></span>

<span data-ttu-id="95478-141">I Freshservice, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="95478-141">In Freshservice, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="95478-142">tooconfigure och testa Azure AD enkel inloggning med Freshservice, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="95478-142">tooconfigure and test Azure AD single sign-on with Freshservice, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="95478-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="95478-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="95478-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="95478-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="95478-145">**[Skapa en testanvändare Freshservice](#creating-a-freshservice-test-user)**  -toohave en motsvarighet för Britta Simon i Freshservice som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="95478-145">**[Creating a Freshservice test user](#creating-a-freshservice-test-user)** - toohave a counterpart of Britta Simon in Freshservice that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="95478-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="95478-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="95478-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="95478-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="95478-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="95478-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="95478-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Freshservice program.</span><span class="sxs-lookup"><span data-stu-id="95478-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Freshservice application.</span></span>

<span data-ttu-id="95478-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Freshservice:**</span><span class="sxs-lookup"><span data-stu-id="95478-150">**tooconfigure Azure AD single sign-on with Freshservice, perform hello following steps:**</span></span>

1. <span data-ttu-id="95478-151">I hello Azure-portalen på hello **Freshservice** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="95478-151">In hello Azure portal, on hello **Freshservice** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="95478-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="95478-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_samlbase.png)

3. <span data-ttu-id="95478-155">På hello **Freshservice domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="95478-155">On hello **Freshservice Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_url.png)

    <span data-ttu-id="95478-157">a.</span><span class="sxs-lookup"><span data-stu-id="95478-157">a.</span></span> <span data-ttu-id="95478-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<democompany>.freshservice.com`</span><span class="sxs-lookup"><span data-stu-id="95478-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<democompany>.freshservice.com`</span></span>

    <span data-ttu-id="95478-159">b.</span><span class="sxs-lookup"><span data-stu-id="95478-159">b.</span></span> <span data-ttu-id="95478-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<democompany>.freshservice.com`</span><span class="sxs-lookup"><span data-stu-id="95478-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<democompany>.freshservice.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="95478-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="95478-161">These values are not real.</span></span> <span data-ttu-id="95478-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="95478-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="95478-163">Kontakta [Freshservice klienten supportteamet](https://support.freshservice.com/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="95478-163">Contact [Freshservice Client support team](https://support.freshservice.com/) tooget these values.</span></span> 
 
4. <span data-ttu-id="95478-164">På hello **SAML-signeringscertifikat** avsnittet, kopiera **TUMAVTRYCKET** värdet för certifikatet.</span><span class="sxs-lookup"><span data-stu-id="95478-164">On hello **SAML Signing Certificate** section, copy **THUMBPRINT** value of certificate.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_certificate.png) 

5. <span data-ttu-id="95478-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="95478-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-freshservice-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="95478-168">På hello **Freshservice Configuration** klickar du på **konfigurera Freshservice** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="95478-168">On hello **Freshservice Configuration** section, click **Configure Freshservice** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="95478-169">Kopiera hello **Sign-Out URL och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="95478-169">Copy hello **Sign-Out URL, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_configure.png) 

7. <span data-ttu-id="95478-171">Logga in tooyour Freshservice företagets webbplats som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="95478-171">In a different web browser window, log in tooyour Freshservice company site as an administrator.</span></span>

8. <span data-ttu-id="95478-172">Hello-menyn överst hello **Admin**.</span><span class="sxs-lookup"><span data-stu-id="95478-172">In hello menu on hello top, click **Admin**.</span></span>
   
    <span data-ttu-id="95478-173">![Admin](./media/active-directory-saas-freshservice-tutorial/ic790814.png "Admin")</span><span class="sxs-lookup"><span data-stu-id="95478-173">![Admin](./media/active-directory-saas-freshservice-tutorial/ic790814.png "Admin")</span></span>

9. <span data-ttu-id="95478-174">I hello **Kundportal**, klickar du på **säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="95478-174">In hello **Customer Portal**, click **Security**.</span></span>
   
    <span data-ttu-id="95478-175">![Säkerhet](./media/active-directory-saas-freshservice-tutorial/ic790815.png "säkerhet")</span><span class="sxs-lookup"><span data-stu-id="95478-175">![Security](./media/active-directory-saas-freshservice-tutorial/ic790815.png "Security")</span></span>

10. <span data-ttu-id="95478-176">I hello **säkerhet** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="95478-176">In hello **Security** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="95478-177">![Enkel inloggning](./media/active-directory-saas-freshservice-tutorial/ic790816.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="95478-177">![Single Sign On](./media/active-directory-saas-freshservice-tutorial/ic790816.png "Single Sign On")</span></span>
   
    <span data-ttu-id="95478-178">a.</span><span class="sxs-lookup"><span data-stu-id="95478-178">a.</span></span> <span data-ttu-id="95478-179">Växeln **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="95478-179">Switch **Single Sign On**.</span></span>

    <span data-ttu-id="95478-180">b.</span><span class="sxs-lookup"><span data-stu-id="95478-180">b.</span></span> <span data-ttu-id="95478-181">Välj **SAML SSO**.</span><span class="sxs-lookup"><span data-stu-id="95478-181">Select **SAML SSO**.</span></span>

    <span data-ttu-id="95478-182">c.</span><span class="sxs-lookup"><span data-stu-id="95478-182">c.</span></span> <span data-ttu-id="95478-183">I hello **inloggnings-URL för SAML** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="95478-183">In hello **SAML Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="95478-184">d.</span><span class="sxs-lookup"><span data-stu-id="95478-184">d.</span></span> <span data-ttu-id="95478-185">I hello **logga ut URL** textruta klistra in hello värdet för **Sign-Out URL** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="95478-185">In hello **Logout URL** textbox, paste hello value of **Sign-Out URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="95478-186">e.</span><span class="sxs-lookup"><span data-stu-id="95478-186">e.</span></span> <span data-ttu-id="95478-187">I **säkerhet certifikat fingeravtryck** textruta klistra in hello **TUMAVTRYCKET** värdet för certifikat som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="95478-187">In **Security Certificate Fingerprint** textbox, paste hello **THUMBPRINT** value of certificate which you have copied from Azure portal.</span></span>

    <span data-ttu-id="95478-188">f.</span><span class="sxs-lookup"><span data-stu-id="95478-188">f.</span></span> <span data-ttu-id="95478-189">Klicka på **spara**</span><span class="sxs-lookup"><span data-stu-id="95478-189">Click **Save**</span></span>
   
> [!TIP]
> <span data-ttu-id="95478-190">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="95478-190">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="95478-191">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="95478-191">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="95478-192">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="95478-192">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="95478-193">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="95478-193">Creating an Azure AD test user</span></span>
<span data-ttu-id="95478-194">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="95478-194">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="95478-196">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="95478-196">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="95478-197">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="95478-197">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-freshservice-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="95478-199">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="95478-199">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-freshservice-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="95478-201">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="95478-201">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-freshservice-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="95478-203">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="95478-203">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-freshservice-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="95478-205">a.</span><span class="sxs-lookup"><span data-stu-id="95478-205">a.</span></span> <span data-ttu-id="95478-206">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="95478-206">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="95478-207">b.</span><span class="sxs-lookup"><span data-stu-id="95478-207">b.</span></span> <span data-ttu-id="95478-208">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="95478-208">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="95478-209">c.</span><span class="sxs-lookup"><span data-stu-id="95478-209">c.</span></span> <span data-ttu-id="95478-210">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="95478-210">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="95478-211">d.</span><span class="sxs-lookup"><span data-stu-id="95478-211">d.</span></span> <span data-ttu-id="95478-212">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="95478-212">Click **Create**.</span></span>
 
### <a name="creating-a-freshservice-test-user"></a><span data-ttu-id="95478-213">Skapa en testanvändare Freshservice</span><span class="sxs-lookup"><span data-stu-id="95478-213">Creating a Freshservice test user</span></span>

<span data-ttu-id="95478-214">tooenable Azure AD-användare toolog i tooFreshService, måste de etableras i FreshService.</span><span class="sxs-lookup"><span data-stu-id="95478-214">tooenable Azure AD users toolog in tooFreshService, they must be provisioned into FreshService.</span></span> <span data-ttu-id="95478-215">Hello gäller FreshService är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="95478-215">In hello case of FreshService, provisioning is a manual task.</span></span>

<span data-ttu-id="95478-216">**tooprovision ett användarkonto, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="95478-216">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="95478-217">Logga in tooyour **FreshService** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="95478-217">Log in tooyour **FreshService** company site as an administrator.</span></span>

2. <span data-ttu-id="95478-218">Hello-menyn överst hello **Admin**.</span><span class="sxs-lookup"><span data-stu-id="95478-218">In hello menu on hello top, click **Admin**.</span></span>
   
    <span data-ttu-id="95478-219">![Admin](./media/active-directory-saas-freshservice-tutorial/ic790814.png "Admin")</span><span class="sxs-lookup"><span data-stu-id="95478-219">![Admin](./media/active-directory-saas-freshservice-tutorial/ic790814.png "Admin")</span></span>

3. <span data-ttu-id="95478-220">I hello **Användarhantering** klickar du på **beställare**.</span><span class="sxs-lookup"><span data-stu-id="95478-220">In hello **User Management** section, click **Requesters**.</span></span>
   
    <span data-ttu-id="95478-221">![Beställare](./media/active-directory-saas-freshservice-tutorial/ic790818.png "beställare")</span><span class="sxs-lookup"><span data-stu-id="95478-221">![Requesters](./media/active-directory-saas-freshservice-tutorial/ic790818.png "Requesters")</span></span>

4. <span data-ttu-id="95478-222">Klicka på **ny frågeställare**.</span><span class="sxs-lookup"><span data-stu-id="95478-222">Click **New Requester**.</span></span>
   
    <span data-ttu-id="95478-223">![Nya beställare](./media/active-directory-saas-freshservice-tutorial/ic790819.png "nya beställare")</span><span class="sxs-lookup"><span data-stu-id="95478-223">![New Requesters](./media/active-directory-saas-freshservice-tutorial/ic790819.png "New Requesters")</span></span>

5. <span data-ttu-id="95478-224">I hello **ny frågeställare** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="95478-224">In hello **New Requester** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="95478-225">![Ny frågeställare](./media/active-directory-saas-freshservice-tutorial/ic790820.png "ny frågeställare")</span><span class="sxs-lookup"><span data-stu-id="95478-225">![New Requester](./media/active-directory-saas-freshservice-tutorial/ic790820.png "New Requester")</span></span>   

    <span data-ttu-id="95478-226">a.</span><span class="sxs-lookup"><span data-stu-id="95478-226">a.</span></span> <span data-ttu-id="95478-227">Ange hello **Förnamn** och **e-post** attribut för ett giltigt Azure Active Directory-konto som du vill använda tooprovision hello relaterade textrutor.</span><span class="sxs-lookup"><span data-stu-id="95478-227">Enter hello **First Name** and **Email** attributes of a valid Azure Active Directory account you want tooprovision into hello related textboxes.</span></span>

    <span data-ttu-id="95478-228">b.</span><span class="sxs-lookup"><span data-stu-id="95478-228">b.</span></span> <span data-ttu-id="95478-229">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="95478-229">Click **Save**.</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="95478-230">hello Azure Active Directory användare får ett e-postmeddelande inklusive en länk tooconfirm hello kontot innan det blir aktiv</span><span class="sxs-lookup"><span data-stu-id="95478-230">hello Azure Active Directory account holder gets an email including a link tooconfirm hello account before it becomes active</span></span>
    >  

>[!NOTE]
><span data-ttu-id="95478-231">Du kan använda något annat FreshService användarens konto skapas verktyg eller API: er som tillhandahålls av FreshService tooprovision AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="95478-231">You can use any other FreshService user account creation tools or APIs provided by FreshService tooprovision AAD user accounts.</span></span>
>  

![Tilldela användare][200] 

<span data-ttu-id="95478-233">**tooassign Britta Simon tooFreshservice utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="95478-233">**tooassign Britta Simon tooFreshservice, perform hello following steps:**</span></span>

1. <span data-ttu-id="95478-234">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="95478-234">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="95478-236">Välj i listan med program hello **Freshservice**.</span><span class="sxs-lookup"><span data-stu-id="95478-236">In hello applications list, select **Freshservice**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_app.png) 

3. <span data-ttu-id="95478-238">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="95478-238">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="95478-240">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="95478-240">Click **Add** button.</span></span> <span data-ttu-id="95478-241">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="95478-241">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="95478-243">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="95478-243">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="95478-244">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="95478-244">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="95478-245">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="95478-245">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="95478-246">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="95478-246">Testing single sign-on</span></span>

<span data-ttu-id="95478-247">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="95478-247">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="95478-248">Du bör få automatiskt inloggade tooyour Freshservice programmet när du klickar på hello Freshservice panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="95478-248">When you click hello Freshservice tile in hello Access Panel, you should get automatically signed-on tooyour Freshservice application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="95478-249">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="95478-249">Additional resources</span></span>

* [<span data-ttu-id="95478-250">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="95478-250">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="95478-251">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="95478-251">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_203.png

