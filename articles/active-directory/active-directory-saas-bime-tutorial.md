---
title: "Självstudier: Azure Active Directory-integrering med Bime | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Bime."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bdcf0729-c880-4c95-b739-0f6345b17dd8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: jeedes
ms.openlocfilehash: 1213725028dd8ce90f22fa6e9c50ffabebc8f3fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bime"></a><span data-ttu-id="7339f-103">Självstudier: Azure Active Directory-integrering med Bime</span><span class="sxs-lookup"><span data-stu-id="7339f-103">Tutorial: Azure Active Directory integration with Bime</span></span>

<span data-ttu-id="7339f-104">I kursen får du lära dig hur toointegrate Bime med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="7339f-104">In this tutorial, you learn how toointegrate Bime with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7339f-105">Integrera Bime med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="7339f-105">Integrating Bime with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="7339f-106">Du kan styra i Azure AD som har åtkomst till tooBime</span><span class="sxs-lookup"><span data-stu-id="7339f-106">You can control in Azure AD who has access tooBime</span></span>
- <span data-ttu-id="7339f-107">Du kan aktivera din användare tooautomatically get inloggade tooBime (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="7339f-107">You can enable your users tooautomatically get signed-on tooBime (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7339f-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="7339f-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="7339f-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7339f-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7339f-110">Krav</span><span class="sxs-lookup"><span data-stu-id="7339f-110">Prerequisites</span></span>

<span data-ttu-id="7339f-111">tooconfigure Azure AD-integrering med Bime, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="7339f-111">tooconfigure Azure AD integration with Bime, you need hello following items:</span></span>

- <span data-ttu-id="7339f-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="7339f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7339f-113">En Bime enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="7339f-113">A Bime single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7339f-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="7339f-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7339f-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="7339f-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7339f-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="7339f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7339f-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7339f-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7339f-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="7339f-118">Scenario description</span></span>
<span data-ttu-id="7339f-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="7339f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7339f-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="7339f-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7339f-121">Att lägga till Bime från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="7339f-121">Adding Bime from hello gallery</span></span>
2. <span data-ttu-id="7339f-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7339f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-bime-from-hello-gallery"></a><span data-ttu-id="7339f-123">Att lägga till Bime från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="7339f-123">Adding Bime from hello gallery</span></span>
<span data-ttu-id="7339f-124">tooconfigure hello integrering av Bime i Azure AD, behöver du tooadd Bime hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="7339f-124">tooconfigure hello integration of Bime into Azure AD, you need tooadd Bime from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="7339f-125">**tooadd Bime från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="7339f-125">**tooadd Bime from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="7339f-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="7339f-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7339f-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="7339f-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="7339f-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="7339f-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="7339f-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7339f-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="7339f-133">Skriv i sökrutan hello **Bime**.</span><span class="sxs-lookup"><span data-stu-id="7339f-133">In hello search box, type **Bime**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bime-tutorial/tutorial_bime_search.png)

5. <span data-ttu-id="7339f-135">Markera hello resultat på panelen **Bime**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="7339f-135">In hello results panel, select **Bime**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bime-tutorial/tutorial_bime_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="7339f-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7339f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="7339f-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Bime baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="7339f-138">In this section, you configure and test Azure AD single sign-on with Bime based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="7339f-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Bime är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7339f-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Bime is tooa user in Azure AD.</span></span> <span data-ttu-id="7339f-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Bime toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="7339f-140">In other words, a link relationship between an Azure AD user and hello related user in Bime needs toobe established.</span></span>

<span data-ttu-id="7339f-141">I Bime, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="7339f-141">In Bime, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="7339f-142">tooconfigure och testa Azure AD enkel inloggning med Bime, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="7339f-142">tooconfigure and test Azure AD single sign-on with Bime, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="7339f-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="7339f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="7339f-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7339f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7339f-145">**[Skapa en testanvändare Bime](#creating-a-bime-test-user)**  -toohave en motsvarighet för Britta Simon i Bime som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="7339f-145">**[Creating a Bime test user](#creating-a-bime-test-user)** - toohave a counterpart of Britta Simon in Bime that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="7339f-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="7339f-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7339f-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="7339f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="7339f-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7339f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="7339f-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Bime program.</span><span class="sxs-lookup"><span data-stu-id="7339f-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Bime application.</span></span>

<span data-ttu-id="7339f-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Bime:**</span><span class="sxs-lookup"><span data-stu-id="7339f-150">**tooconfigure Azure AD single sign-on with Bime, perform hello following steps:**</span></span>

1. <span data-ttu-id="7339f-151">I hello Azure-portalen på hello **Bime** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="7339f-151">In hello Azure portal, on hello **Bime** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="7339f-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="7339f-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-bime-tutorial/tutorial_bime_samlbase.png)

3. <span data-ttu-id="7339f-155">På hello **Bime domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="7339f-155">On hello **Bime Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bime-tutorial/tutorial_bime_url.png)

    <span data-ttu-id="7339f-157">a.</span><span class="sxs-lookup"><span data-stu-id="7339f-157">a.</span></span> <span data-ttu-id="7339f-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<tenant-name>.Bimeapp.com`</span><span class="sxs-lookup"><span data-stu-id="7339f-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenant-name>.Bimeapp.com`</span></span>

    <span data-ttu-id="7339f-159">b.</span><span class="sxs-lookup"><span data-stu-id="7339f-159">b.</span></span> <span data-ttu-id="7339f-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<tenant-name>.Bimeapp.com`</span><span class="sxs-lookup"><span data-stu-id="7339f-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<tenant-name>.Bimeapp.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="7339f-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="7339f-161">These values are not real.</span></span> <span data-ttu-id="7339f-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="7339f-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="7339f-163">Kontakta [Bime klienten supportteamet](https://bime.zendesk.com/hc/categories/202604307-Support-tech-notes-and-tips-) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="7339f-163">Contact [Bime Client support team](https://bime.zendesk.com/hc/categories/202604307-Support-tech-notes-and-tips-) tooget these values.</span></span> 
 
4. <span data-ttu-id="7339f-164">På hello **SAML-signeringscertifikat** avsnitt, kopiera hello **TUMAVTRYCKET** värdet från hello-certifikatet.</span><span class="sxs-lookup"><span data-stu-id="7339f-164">On hello **SAML Signing Certificate** section, copy hello **THUMBPRINT** value from hello certificate.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bime-tutorial/tutorial_bime_certificate.png) 

5. <span data-ttu-id="7339f-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="7339f-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bime-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="7339f-168">På hello **Bime Configuration** klickar du på **konfigurera Bime** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="7339f-168">On hello **Bime Configuration** section, click **Configure Bime** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="7339f-169">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="7339f-169">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bime-tutorial/tutorial_bime_configure.png) 

7. <span data-ttu-id="7339f-171">Logga in på webbplatsen Bime företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="7339f-171">In a different web browser window, log into your Bime company site as an administrator.</span></span>

8. <span data-ttu-id="7339f-172">Klicka i verktygsfältet hello **Admin**, och sedan **konto**.</span><span class="sxs-lookup"><span data-stu-id="7339f-172">In hello toolbar, click **Admin**, and then **Account**.</span></span>
   
    <span data-ttu-id="7339f-173">![Admin](./media/active-directory-saas-bime-tutorial/ic775558.png "Admin")</span><span class="sxs-lookup"><span data-stu-id="7339f-173">![Admin](./media/active-directory-saas-bime-tutorial/ic775558.png "Admin")</span></span>

9. <span data-ttu-id="7339f-174">På konfigurationssidan för hello konto utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="7339f-174">On hello account configuration page, perform hello following steps:</span></span>
   
    <span data-ttu-id="7339f-175">![Konfigurera enkel inloggning](./media/active-directory-saas-bime-tutorial/ic775559.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="7339f-175">![Configure Single Sign-On](./media/active-directory-saas-bime-tutorial/ic775559.png "Configure Single Sign-On")</span></span>
   
    <span data-ttu-id="7339f-176">a.</span><span class="sxs-lookup"><span data-stu-id="7339f-176">a.</span></span> <span data-ttu-id="7339f-177">Välj **aktivera SAML-autentisering**.</span><span class="sxs-lookup"><span data-stu-id="7339f-177">Select **Enable SAML authentication**.</span></span>

    <span data-ttu-id="7339f-178">b.</span><span class="sxs-lookup"><span data-stu-id="7339f-178">b.</span></span> <span data-ttu-id="7339f-179">I hello **Remote inloggnings-URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7339f-179">In hello **Remote Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="7339f-180">c.</span><span class="sxs-lookup"><span data-stu-id="7339f-180">c.</span></span>  <span data-ttu-id="7339f-181">Klistra in hello **tumavtrycket** värdet från Azure-portalen i hello **certifikat fingeravtryck** textruta.</span><span class="sxs-lookup"><span data-stu-id="7339f-181">Paste hello **Thumbprint** value from Azure portal into hello **Certificate Fingerprint** textbox.</span></span>       
   
    <span data-ttu-id="7339f-182">d.</span><span class="sxs-lookup"><span data-stu-id="7339f-182">d.</span></span> <span data-ttu-id="7339f-183">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="7339f-183">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="7339f-184">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="7339f-184">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="7339f-185">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="7339f-185">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="7339f-186">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7339f-186">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="7339f-187">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="7339f-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="7339f-188">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7339f-188">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="7339f-190">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="7339f-190">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="7339f-191">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="7339f-191">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bime-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7339f-193">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="7339f-193">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bime-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7339f-195">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7339f-195">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bime-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7339f-197">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="7339f-197">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bime-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7339f-199">a.</span><span class="sxs-lookup"><span data-stu-id="7339f-199">a.</span></span> <span data-ttu-id="7339f-200">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7339f-200">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7339f-201">b.</span><span class="sxs-lookup"><span data-stu-id="7339f-201">b.</span></span> <span data-ttu-id="7339f-202">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="7339f-202">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="7339f-203">c.</span><span class="sxs-lookup"><span data-stu-id="7339f-203">c.</span></span> <span data-ttu-id="7339f-204">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="7339f-204">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="7339f-205">d.</span><span class="sxs-lookup"><span data-stu-id="7339f-205">d.</span></span> <span data-ttu-id="7339f-206">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="7339f-206">Click **Create**.</span></span>
 
### <a name="creating-a-bime-test-user"></a><span data-ttu-id="7339f-207">Skapa en testanvändare Bime</span><span class="sxs-lookup"><span data-stu-id="7339f-207">Creating a Bime test user</span></span>

<span data-ttu-id="7339f-208">I ordning tooenable Azure AD-användare toolog i tooBime, måste de etableras i Bime.</span><span class="sxs-lookup"><span data-stu-id="7339f-208">In order tooenable Azure AD users toolog in tooBime, they must be provisioned into Bime.</span></span> <span data-ttu-id="7339f-209">Hello gäller Bime är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="7339f-209">In hello case of Bime, provisioning is a manual task.</span></span>

<span data-ttu-id="7339f-210">**tooconfigure användaretablering, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="7339f-210">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="7339f-211">Logga in tooyour **Bime** klient.</span><span class="sxs-lookup"><span data-stu-id="7339f-211">Log in tooyour **Bime** tenant.</span></span>

2. <span data-ttu-id="7339f-212">Klicka i verktygsfältet hello **Admin**, och sedan **användare**.</span><span class="sxs-lookup"><span data-stu-id="7339f-212">In hello toolbar, click **Admin**, and then **Users**.</span></span>
   
    <span data-ttu-id="7339f-213">![Admin](./media/active-directory-saas-bime-tutorial/ic775561.png "Admin")</span><span class="sxs-lookup"><span data-stu-id="7339f-213">![Admin](./media/active-directory-saas-bime-tutorial/ic775561.png "Admin")</span></span>

3. <span data-ttu-id="7339f-214">I hello **användarlistan**, klickar du på **Lägg till nya användare** (”+”).</span><span class="sxs-lookup"><span data-stu-id="7339f-214">In hello **Users List**, click **Add New User** (“+”).</span></span>
   
    <span data-ttu-id="7339f-215">![Användare](./media/active-directory-saas-bime-tutorial/ic775562.png "användare")</span><span class="sxs-lookup"><span data-stu-id="7339f-215">![Users](./media/active-directory-saas-bime-tutorial/ic775562.png "Users")</span></span>

4. <span data-ttu-id="7339f-216">På hello **användarinformation** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="7339f-216">On hello **User Details** dialog page, perform hello following steps:</span></span>
   
    <span data-ttu-id="7339f-217">![Användarinformation](./media/active-directory-saas-bime-tutorial/ic775563.png "användarinformation")</span><span class="sxs-lookup"><span data-stu-id="7339f-217">![User Details](./media/active-directory-saas-bime-tutorial/ic775563.png "User Details")</span></span>
   
    <span data-ttu-id="7339f-218">a.</span><span class="sxs-lookup"><span data-stu-id="7339f-218">a.</span></span> <span data-ttu-id="7339f-219">I hello **Förnamn** textruta anger hello först namnet på användaren som **Britta**.</span><span class="sxs-lookup"><span data-stu-id="7339f-219">In hello **First name** textbox, enter hello first name of user like **Britta**.</span></span>

    <span data-ttu-id="7339f-220">b.</span><span class="sxs-lookup"><span data-stu-id="7339f-220">b.</span></span> <span data-ttu-id="7339f-221">I hello **efternamn** textruta ange hello efternamn för användaren som **Simon**.</span><span class="sxs-lookup"><span data-stu-id="7339f-221">In hello **Last name** textbox, enter hello last name of user like **Simon**.</span></span>
 
    <span data-ttu-id="7339f-222">c.</span><span class="sxs-lookup"><span data-stu-id="7339f-222">c.</span></span> <span data-ttu-id="7339f-223">I hello **e-post** textruta ange hello e-postadress för användaren som  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="7339f-223">In hello **Email** textbox, enter hello email of user like **brittasimon@contoso.com**.</span></span>

    <span data-ttu-id="7339f-224">d.</span><span class="sxs-lookup"><span data-stu-id="7339f-224">d.</span></span> <span data-ttu-id="7339f-225">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="7339f-225">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="7339f-226">Du kan använda något annat Bime användarens konto skapas verktyg eller API: er som tillhandahålls av Bime tooprovision AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="7339f-226">You can use any other Bime user account creation tools or APIs provided by Bime tooprovision AAD user accounts.</span></span>
>  

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="7339f-227">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="7339f-227">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="7339f-228">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooBime.</span><span class="sxs-lookup"><span data-stu-id="7339f-228">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooBime.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="7339f-230">**tooassign Britta Simon tooBime utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="7339f-230">**tooassign Britta Simon tooBime, perform hello following steps:**</span></span>

1. <span data-ttu-id="7339f-231">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="7339f-231">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="7339f-233">Välj i listan med program hello **Bime**.</span><span class="sxs-lookup"><span data-stu-id="7339f-233">In hello applications list, select **Bime**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bime-tutorial/tutorial_bime_app.png) 

3. <span data-ttu-id="7339f-235">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="7339f-235">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="7339f-237">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="7339f-237">Click **Add** button.</span></span> <span data-ttu-id="7339f-238">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7339f-238">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="7339f-240">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="7339f-240">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="7339f-241">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7339f-241">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7339f-242">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7339f-242">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="7339f-243">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7339f-243">Testing single sign-on</span></span>

<span data-ttu-id="7339f-244">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="7339f-244">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="7339f-245">Du bör få automatiskt inloggade tooyour Bime programmet när du klickar på hello Bime panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="7339f-245">When you click hello Bime tile in hello Access Panel, you should get automatically signed-on tooyour Bime application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7339f-246">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="7339f-246">Additional resources</span></span>

* [<span data-ttu-id="7339f-247">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7339f-247">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7339f-248">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7339f-248">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bime-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bime-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bime-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bime-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bime-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bime-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bime-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bime-tutorial/tutorial_general_203.png

