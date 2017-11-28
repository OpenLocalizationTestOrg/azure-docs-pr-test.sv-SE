---
title: "Självstudier: Azure Active Directory-integrering med Kindling | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Kindling."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 71229751-74eb-4c2c-abb4-07caa95754c7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: 32ad6116c2690aea2060a2dd56e052f233ad7ae3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kindling"></a><span data-ttu-id="2a452-103">Självstudier: Azure Active Directory-integrering med Kindling</span><span class="sxs-lookup"><span data-stu-id="2a452-103">Tutorial: Azure Active Directory integration with Kindling</span></span>

<span data-ttu-id="2a452-104">I kursen får du lära dig hur toointegrate Kindling med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="2a452-104">In this tutorial, you learn how toointegrate Kindling with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2a452-105">Integrera Kindling med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="2a452-105">Integrating Kindling with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="2a452-106">Du kan styra i Azure AD som har åtkomst till tooKindling</span><span class="sxs-lookup"><span data-stu-id="2a452-106">You can control in Azure AD who has access tooKindling</span></span>
- <span data-ttu-id="2a452-107">Du kan aktivera din användare tooautomatically get inloggade tooKindling (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="2a452-107">You can enable your users tooautomatically get signed-on tooKindling (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2a452-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="2a452-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="2a452-109">Om du vill tooknow finns mer information om SaaS appintegrering med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2a452-109">If you want tooknow more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="2a452-110">[Vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2a452-110">[what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2a452-111">Krav</span><span class="sxs-lookup"><span data-stu-id="2a452-111">Prerequisites</span></span>

<span data-ttu-id="2a452-112">tooconfigure Azure AD-integrering med Kindling, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="2a452-112">tooconfigure Azure AD integration with Kindling, you need hello following items:</span></span>

- <span data-ttu-id="2a452-113">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="2a452-113">An Azure AD subscription</span></span>
- <span data-ttu-id="2a452-114">En Kindling enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="2a452-114">A Kindling single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="2a452-115">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="2a452-115">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="2a452-116">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="2a452-116">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2a452-117">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="2a452-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="2a452-118">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2a452-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2a452-119">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="2a452-119">Scenario description</span></span>
<span data-ttu-id="2a452-120">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="2a452-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2a452-121">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="2a452-121">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2a452-122">Att lägga till Kindling från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="2a452-122">Adding Kindling from hello gallery</span></span>
2. <span data-ttu-id="2a452-123">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2a452-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-kindling-from-hello-gallery"></a><span data-ttu-id="2a452-124">Att lägga till Kindling från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="2a452-124">Adding Kindling from hello gallery</span></span>
<span data-ttu-id="2a452-125">tooconfigure hello integrering av Kindling till Azure AD, behöver du tooadd Kindling hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="2a452-125">tooconfigure hello integration of Kindling into Azure AD, you need tooadd Kindling from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="2a452-126">**tooadd Kindling från galleriet hello utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="2a452-126">**tooadd Kindling from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="2a452-127">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="2a452-127">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="2a452-129">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="2a452-129">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="2a452-130">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="2a452-130">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="2a452-132">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2a452-132">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="2a452-134">Skriv i sökrutan hello **Kindling**.</span><span class="sxs-lookup"><span data-stu-id="2a452-134">In hello search box, type **Kindling**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kindling-tutorial/tutorial_kindling_search.png)

5. <span data-ttu-id="2a452-136">Markera hello resultat på panelen **Kindling**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="2a452-136">In hello results panel, select **Kindling**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kindling-tutorial/tutorial_kindling_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2a452-138">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2a452-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="2a452-139">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Kindling baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="2a452-139">In this section, you configure and test Azure AD single sign-on with Kindling based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="2a452-140">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Kindling är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2a452-140">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Kindling is tooa user in Azure AD.</span></span> <span data-ttu-id="2a452-141">Med andra ord en länk relationen mellan en Azure AD-användare och hello relaterade användare i Kindling behov toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="2a452-141">In other words, a link relationship between an Azure AD user and hello related user in Kindling needs toobe established.</span></span>

<span data-ttu-id="2a452-142">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Kindling.</span><span class="sxs-lookup"><span data-stu-id="2a452-142">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Kindling.</span></span>

<span data-ttu-id="2a452-143">tooconfigure och testa Azure AD enkel inloggning med Kindling, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="2a452-143">tooconfigure and test Azure AD single sign-on with Kindling, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="2a452-144">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="2a452-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="2a452-145">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2a452-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2a452-146">**[Skapa en testanvändare Kindling](#creating-a-kindling-test-user)**  -toohave en motsvarighet för Britta Simon i Kindling som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="2a452-146">**[Creating a Kindling test user](#creating-a-kindling-test-user)** - toohave a counterpart of Britta Simon in Kindling that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="2a452-147">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="2a452-147">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2a452-148">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="2a452-148">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="2a452-149">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2a452-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="2a452-150">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Kindling program.</span><span class="sxs-lookup"><span data-stu-id="2a452-150">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Kindling application.</span></span>

<span data-ttu-id="2a452-151">**tooconfigure Azure AD enkel inloggning med Kindling, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="2a452-151">**tooconfigure Azure AD single sign-on with Kindling, perform hello following steps:**</span></span>

1. <span data-ttu-id="2a452-152">I hello Azure-portalen på hello **Kindling** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="2a452-152">In hello Azure portal, on hello **Kindling** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="2a452-154">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="2a452-154">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-kindling-tutorial/tutorial_kindling_samlbase.png)

3. <span data-ttu-id="2a452-156">På hello **Kindling domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="2a452-156">On hello **Kindling Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kindling-tutorial/tutorial_kindling_url.png)

    <span data-ttu-id="2a452-158">a.</span><span class="sxs-lookup"><span data-stu-id="2a452-158">a.</span></span> <span data-ttu-id="2a452-159">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.kindlingapp.com`</span><span class="sxs-lookup"><span data-stu-id="2a452-159">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.kindlingapp.com`</span></span>

    <span data-ttu-id="2a452-160">b.</span><span class="sxs-lookup"><span data-stu-id="2a452-160">b.</span></span>  <span data-ttu-id="2a452-161">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.kindlingapp.com/saml/module.php/saml/sp/metadata.php/clientIDP`</span><span class="sxs-lookup"><span data-stu-id="2a452-161">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.kindlingapp.com/saml/module.php/saml/sp/metadata.php/clientIDP`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="2a452-162">Dessa värden är inte hello verkliga.</span><span class="sxs-lookup"><span data-stu-id="2a452-162">These values are not hello real.</span></span> <span data-ttu-id="2a452-163">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="2a452-163">Update these values with hello actual Sign-on URL and Identifier.</span></span> <span data-ttu-id="2a452-164">Kontakta [Kindling supportteamet](mailto:support@kindlingapp.com) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="2a452-164">Contact [Kindling support team](mailto:support@kindlingapp.com) tooget these values.</span></span>
 
4. <span data-ttu-id="2a452-165">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="2a452-165">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kindling-tutorial/tutorial_kindling_certificate.png) 

5. <span data-ttu-id="2a452-167">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="2a452-167">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kindling-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="2a452-169">På hello **Kindling Configuration** klickar du på **konfigurera Kindling** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="2a452-169">On hello **Kindling Configuration** section, click **Configure Kindling** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="2a452-170">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="2a452-170">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kindling-tutorial/tutorial_kindling_configure.png) 

7. <span data-ttu-id="2a452-172">tooconfigure enkel inloggning på **Kindling** sida, behöver du toosend hello hämtas **certifikat (Base64)**, **Sign-Out URL, SAML enhets-ID och SAML inloggning tjänst-URL för enkel**för[Kindling supportteamet](mailto:support@kindlingapp.com).</span><span class="sxs-lookup"><span data-stu-id="2a452-172">tooconfigure single sign-on on **Kindling** side, you need toosend hello downloaded **Certificate (Base64)**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[Kindling support team](mailto:support@kindlingapp.com).</span></span>

> [!TIP]
> <span data-ttu-id="2a452-173">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="2a452-173">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="2a452-174">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="2a452-174">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="2a452-175">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="2a452-175">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2a452-176">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="2a452-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="2a452-177">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2a452-177">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="2a452-179">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="2a452-179">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="2a452-180">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="2a452-180">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kindling-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="2a452-182">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="2a452-182">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kindling-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2a452-184">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2a452-184">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kindling-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2a452-186">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="2a452-186">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kindling-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="2a452-188">a.</span><span class="sxs-lookup"><span data-stu-id="2a452-188">a.</span></span> <span data-ttu-id="2a452-189">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2a452-189">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2a452-190">b.</span><span class="sxs-lookup"><span data-stu-id="2a452-190">b.</span></span> <span data-ttu-id="2a452-191">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="2a452-191">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="2a452-192">c.</span><span class="sxs-lookup"><span data-stu-id="2a452-192">c.</span></span> <span data-ttu-id="2a452-193">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="2a452-193">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="2a452-194">d.</span><span class="sxs-lookup"><span data-stu-id="2a452-194">d.</span></span> <span data-ttu-id="2a452-195">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="2a452-195">Click **Create**.</span></span>
 
### <a name="creating-a-kindling-test-user"></a><span data-ttu-id="2a452-196">Skapa en testanvändare Kindling</span><span class="sxs-lookup"><span data-stu-id="2a452-196">Creating a Kindling test user</span></span>

<span data-ttu-id="2a452-197">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i Kindling.</span><span class="sxs-lookup"><span data-stu-id="2a452-197">hello objective of this section is toocreate a user called Britta Simon in Kindling.</span></span> <span data-ttu-id="2a452-198">Kindling stöder just-in-time-etablering.</span><span class="sxs-lookup"><span data-stu-id="2a452-198">Kindling supports just-in-time provisioning.</span></span> <span data-ttu-id="2a452-199">Du har redan aktiverats i [konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="2a452-199">You have already enabled it in [Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on).</span></span>

<span data-ttu-id="2a452-200">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="2a452-200">There is no action item for you in this section.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="2a452-201">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="2a452-201">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="2a452-202">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooKindling.</span><span class="sxs-lookup"><span data-stu-id="2a452-202">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooKindling.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="2a452-204">**tooassign Britta Simon tooKindling utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="2a452-204">**tooassign Britta Simon tooKindling, perform hello following steps:**</span></span>

1. <span data-ttu-id="2a452-205">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="2a452-205">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="2a452-207">Välj i listan med program hello **Kindling**.</span><span class="sxs-lookup"><span data-stu-id="2a452-207">In hello applications list, select **Kindling**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kindling-tutorial/tutorial_kindling_app.png) 

3. <span data-ttu-id="2a452-209">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="2a452-209">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="2a452-211">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="2a452-211">Click **Add** button.</span></span> <span data-ttu-id="2a452-212">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2a452-212">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="2a452-214">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="2a452-214">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="2a452-215">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2a452-215">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2a452-216">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2a452-216">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="2a452-217">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2a452-217">Testing single sign-on</span></span>

<span data-ttu-id="2a452-218">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="2a452-218">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="2a452-219">Du bör få automatiskt inloggade tooyour Kindling programmet när du klickar på hello Kindling panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="2a452-219">When you click hello Kindling tile in hello Access Panel, you should get automatically signed-on tooyour Kindling application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="2a452-220">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="2a452-220">Additional resources</span></span>

* [<span data-ttu-id="2a452-221">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2a452-221">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2a452-222">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="2a452-222">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_203.png

