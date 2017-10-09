---
title: "Självstudier: Azure Active Directory-integrering med Tidemark | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Tidemark."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5cf80d4e-6e8b-48ec-81c8-27872af5e5d5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: jeedes
ms.openlocfilehash: 4fffee30370d928ae8070a5904c58ba8a7a06343
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tidemark"></a><span data-ttu-id="86dd1-103">Självstudier: Azure Active Directory-integrering med Tidemark</span><span class="sxs-lookup"><span data-stu-id="86dd1-103">Tutorial: Azure Active Directory integration with Tidemark</span></span>

<span data-ttu-id="86dd1-104">I kursen får du lära dig hur toointegrate Tidemark med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="86dd1-104">In this tutorial, you learn how toointegrate Tidemark with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="86dd1-105">Integrera Tidemark med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="86dd1-105">Integrating Tidemark with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="86dd1-106">Du kan styra i Azure AD som har åtkomst till tooTidemark</span><span class="sxs-lookup"><span data-stu-id="86dd1-106">You can control in Azure AD who has access tooTidemark</span></span>
- <span data-ttu-id="86dd1-107">Du kan aktivera din användare tooautomatically get inloggade tooTidemark (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="86dd1-107">You can enable your users tooautomatically get signed-on tooTidemark (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="86dd1-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="86dd1-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="86dd1-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="86dd1-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="86dd1-110">Krav</span><span class="sxs-lookup"><span data-stu-id="86dd1-110">Prerequisites</span></span>

<span data-ttu-id="86dd1-111">tooconfigure Azure AD-integrering med Tidemark, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="86dd1-111">tooconfigure Azure AD integration with Tidemark, you need hello following items:</span></span>

- <span data-ttu-id="86dd1-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="86dd1-112">An Azure AD subscription</span></span>
- <span data-ttu-id="86dd1-113">En Tidemark enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="86dd1-113">A Tidemark single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="86dd1-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="86dd1-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="86dd1-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="86dd1-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="86dd1-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="86dd1-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="86dd1-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="86dd1-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="86dd1-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="86dd1-118">Scenario description</span></span>
<span data-ttu-id="86dd1-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="86dd1-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="86dd1-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="86dd1-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="86dd1-121">Att lägga till Tidemark från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="86dd1-121">Adding Tidemark from hello gallery</span></span>
2. <span data-ttu-id="86dd1-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="86dd1-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-tidemark-from-hello-gallery"></a><span data-ttu-id="86dd1-123">Att lägga till Tidemark från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="86dd1-123">Adding Tidemark from hello gallery</span></span>
<span data-ttu-id="86dd1-124">tooconfigure hello integrering av Tidemark i Azure AD, behöver du tooadd Tidemark hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="86dd1-124">tooconfigure hello integration of Tidemark into Azure AD, you need tooadd Tidemark from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="86dd1-125">**tooadd Tidemark från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="86dd1-125">**tooadd Tidemark from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="86dd1-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="86dd1-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="86dd1-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="86dd1-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="86dd1-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="86dd1-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="86dd1-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="86dd1-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="86dd1-133">Skriv i sökrutan hello **Tidemark**.</span><span class="sxs-lookup"><span data-stu-id="86dd1-133">In hello search box, type **Tidemark**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tidemark-tutorial/tutorial_tidemark_search.png)

5. <span data-ttu-id="86dd1-135">Markera hello resultat på panelen **Tidemark**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="86dd1-135">In hello results panel, select **Tidemark**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tidemark-tutorial/tutorial_tidemark_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="86dd1-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="86dd1-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="86dd1-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Tidemark baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="86dd1-138">In this section, you configure and test Azure AD single sign-on with Tidemark based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="86dd1-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Tidemark är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="86dd1-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Tidemark is tooa user in Azure AD.</span></span> <span data-ttu-id="86dd1-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Tidemark toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="86dd1-140">In other words, a link relationship between an Azure AD user and hello related user in Tidemark needs toobe established.</span></span>

<span data-ttu-id="86dd1-141">I Tidemark, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="86dd1-141">In Tidemark, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="86dd1-142">tooconfigure och testa Azure AD enkel inloggning med Tidemark, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="86dd1-142">tooconfigure and test Azure AD single sign-on with Tidemark, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="86dd1-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="86dd1-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="86dd1-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="86dd1-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="86dd1-145">**[Skapa en testanvändare Tidemark](#creating-a-tidemark-test-user)**  -toohave en motsvarighet för Britta Simon i Tidemark som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="86dd1-145">**[Creating a Tidemark test user](#creating-a-tidemark-test-user)** - toohave a counterpart of Britta Simon in Tidemark that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="86dd1-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="86dd1-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="86dd1-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="86dd1-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="86dd1-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="86dd1-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="86dd1-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Tidemark program.</span><span class="sxs-lookup"><span data-stu-id="86dd1-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Tidemark application.</span></span>

<span data-ttu-id="86dd1-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Tidemark:**</span><span class="sxs-lookup"><span data-stu-id="86dd1-150">**tooconfigure Azure AD single sign-on with Tidemark, perform hello following steps:**</span></span>

1. <span data-ttu-id="86dd1-151">I hello Azure-portalen på hello **Tidemark** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="86dd1-151">In hello Azure portal, on hello **Tidemark** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="86dd1-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="86dd1-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-tidemark-tutorial/tutorial_tidemark_samlbase.png)

3. <span data-ttu-id="86dd1-155">På hello **Tidemark domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="86dd1-155">On hello **Tidemark Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tidemark-tutorial/tutorial_tidemark_url.png)

    <span data-ttu-id="86dd1-157">a.</span><span class="sxs-lookup"><span data-stu-id="86dd1-157">a.</span></span> <span data-ttu-id="86dd1-158">I hello **inloggnings-URL** textruta, ange en Webbadress med hello följande mönster:</span><span class="sxs-lookup"><span data-stu-id="86dd1-158">In hello **Sign-on URL** textbox, type a URL using hello following patterns:</span></span> 
    | |
    |--|
    | `https://<subdomain>.tidemark.com/login` |
    | `https://<subdomain>.tidemark.net/login` |

    <span data-ttu-id="86dd1-159">b.</span><span class="sxs-lookup"><span data-stu-id="86dd1-159">b.</span></span> <span data-ttu-id="86dd1-160">I hello **identifierare** textruta, ange en Webbadress med hello följande mönster:</span><span class="sxs-lookup"><span data-stu-id="86dd1-160">In hello **Identifier** textbox, type a URL using hello following patterns:</span></span> 
    | |
    |--|
    | `https://<subdomain>.tidemark.com/saml` |
    | `https://<subdomain>.tidemark.net/saml` |

    > [!NOTE] 
    > <span data-ttu-id="86dd1-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="86dd1-161">These values are not real.</span></span> <span data-ttu-id="86dd1-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="86dd1-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="86dd1-163">Kontakta [Tidemark klienten supportteamet](http://www.tidemark.com/contact-us) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="86dd1-163">Contact [Tidemark Client support team](http://www.tidemark.com/contact-us) tooget these values.</span></span> 
 
4. <span data-ttu-id="86dd1-164">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="86dd1-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tidemark-tutorial/tutorial_tidemark_certificate.png) 

5. <span data-ttu-id="86dd1-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="86dd1-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tidemark-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="86dd1-168">På hello **Tidemark Configuration** klickar du på **konfigurera Tidemark** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="86dd1-168">On hello **Tidemark Configuration** section, click **Configure Tidemark** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="86dd1-169">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="86dd1-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tidemark-tutorial/tutorial_tidemark_configure.png) 

7. <span data-ttu-id="86dd1-171">tooconfigure enkel inloggning på **Tidemark** sida, behöver du toosend hello hämtas **Certificate(Base64), SAML enhets-ID, Sign-Out URL och SAML enkel inloggning Tjänstwebbadress** för[Tidemark supportteam](http://www.tidemark.com/contact-us).</span><span class="sxs-lookup"><span data-stu-id="86dd1-171">tooconfigure single sign-on on **Tidemark** side, you need toosend hello downloaded **Certificate(Base64), SAML Entity ID, Sign-Out URL and SAML Single Sign-On Service URL** too[Tidemark support team](http://www.tidemark.com/contact-us).</span></span> <span data-ttu-id="86dd1-172">De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="86dd1-172">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="86dd1-173">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="86dd1-173">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="86dd1-174">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="86dd1-174">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="86dd1-175">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="86dd1-175">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="86dd1-176">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="86dd1-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="86dd1-177">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="86dd1-177">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="86dd1-179">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="86dd1-179">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="86dd1-180">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="86dd1-180">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tidemark-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="86dd1-182">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="86dd1-182">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tidemark-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="86dd1-184">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="86dd1-184">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tidemark-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="86dd1-186">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="86dd1-186">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tidemark-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="86dd1-188">a.</span><span class="sxs-lookup"><span data-stu-id="86dd1-188">a.</span></span> <span data-ttu-id="86dd1-189">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="86dd1-189">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="86dd1-190">b.</span><span class="sxs-lookup"><span data-stu-id="86dd1-190">b.</span></span> <span data-ttu-id="86dd1-191">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="86dd1-191">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="86dd1-192">c.</span><span class="sxs-lookup"><span data-stu-id="86dd1-192">c.</span></span> <span data-ttu-id="86dd1-193">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="86dd1-193">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="86dd1-194">d.</span><span class="sxs-lookup"><span data-stu-id="86dd1-194">d.</span></span> <span data-ttu-id="86dd1-195">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="86dd1-195">Click **Create**.</span></span>
 
### <a name="creating-a-tidemark-test-user"></a><span data-ttu-id="86dd1-196">Skapa en testanvändare Tidemark</span><span class="sxs-lookup"><span data-stu-id="86dd1-196">Creating a Tidemark test user</span></span>

<span data-ttu-id="86dd1-197">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i Tidemark.</span><span class="sxs-lookup"><span data-stu-id="86dd1-197">hello objective of this section is toocreate a user called Britta Simon in Tidemark.</span></span> <span data-ttu-id="86dd1-198">Se tillsammans med [Tidemark supportteamet](http://www.tidemark.com/contact-us) tooadd hello användare i hello Tidemark konto.</span><span class="sxs-lookup"><span data-stu-id="86dd1-198">Please work with [Tidemark support team](http://www.tidemark.com/contact-us) tooadd hello users in hello Tidemark account.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="86dd1-199">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="86dd1-199">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="86dd1-200">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooTidemark.</span><span class="sxs-lookup"><span data-stu-id="86dd1-200">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooTidemark.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="86dd1-202">**tooassign Britta Simon tooTidemark utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="86dd1-202">**tooassign Britta Simon tooTidemark, perform hello following steps:**</span></span>

1. <span data-ttu-id="86dd1-203">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="86dd1-203">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="86dd1-205">Välj i listan med program hello **Tidemark**.</span><span class="sxs-lookup"><span data-stu-id="86dd1-205">In hello applications list, select **Tidemark**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tidemark-tutorial/tutorial_tidemark_app.png) 

3. <span data-ttu-id="86dd1-207">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="86dd1-207">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="86dd1-209">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="86dd1-209">Click **Add** button.</span></span> <span data-ttu-id="86dd1-210">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="86dd1-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="86dd1-212">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="86dd1-212">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="86dd1-213">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="86dd1-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="86dd1-214">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="86dd1-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="86dd1-215">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="86dd1-215">Testing single sign-on</span></span>

<span data-ttu-id="86dd1-216">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="86dd1-216">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="86dd1-217">Du bör få automatiskt inloggade tooyour Tidemark programmet när du klickar på hello Tidemark panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="86dd1-217">When you click hello Tidemark tile in hello Access Panel, you should get automatically signed-on tooyour Tidemark application.</span></span>
<span data-ttu-id="86dd1-218">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="86dd1-218">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="86dd1-219">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="86dd1-219">Additional resources</span></span>

* [<span data-ttu-id="86dd1-220">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="86dd1-220">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="86dd1-221">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="86dd1-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tidemark-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tidemark-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tidemark-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tidemark-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tidemark-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tidemark-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tidemark-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tidemark-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tidemark-tutorial/tutorial_general_203.png

