---
title: "Självstudier: Azure Active Directory-integrering med 10 000 ft planer | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och 10 000 ft planer."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b60c955e-8fa3-4872-a897-c4e81fd7beac
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: 9aa6fd079da4f931d4dd7277407a07e56091d7c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-10000ft-plans"></a><span data-ttu-id="6debd-103">Självstudier: Azure Active Directory-integrering med 10 000 ft planer</span><span class="sxs-lookup"><span data-stu-id="6debd-103">Tutorial: Azure Active Directory integration with 10,000ft Plans</span></span>

<span data-ttu-id="6debd-104">I kursen får du lära dig hur toointegrate 10 000 ft planerar med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="6debd-104">In this tutorial, you learn how toointegrate 10,000ft Plans with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6debd-105">10 000 ft planer integrera med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="6debd-105">Integrating 10,000ft Plans with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="6debd-106">Du kan styra i Azure AD som har åtkomst till too10, 000ft planer</span><span class="sxs-lookup"><span data-stu-id="6debd-106">You can control in Azure AD who has access too10,000ft Plans</span></span>
- <span data-ttu-id="6debd-107">Du kan aktivera din användare tooautomatically get inloggade too10, 000ft planer (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="6debd-107">You can enable your users tooautomatically get signed-on too10,000ft Plans (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="6debd-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="6debd-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="6debd-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6debd-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6debd-110">Krav</span><span class="sxs-lookup"><span data-stu-id="6debd-110">Prerequisites</span></span>

<span data-ttu-id="6debd-111">tooconfigure Azure AD-integrering med 10 000 ft planer, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="6debd-111">tooconfigure Azure AD integration with 10,000ft Plans, you need hello following items:</span></span>

- <span data-ttu-id="6debd-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="6debd-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6debd-113">En 10 000 ft planer för enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="6debd-113">A 10,000ft Plans single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6debd-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="6debd-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6debd-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="6debd-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6debd-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="6debd-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6debd-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här [utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6debd-117">If you don't have an Azure AD trial environment, you can get a one-month trial here [trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6debd-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="6debd-118">Scenario description</span></span>
<span data-ttu-id="6debd-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="6debd-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6debd-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="6debd-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6debd-121">Lägger till 10 000 ft planer från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="6debd-121">Adding 10,000ft Plans from hello gallery</span></span>
2. <span data-ttu-id="6debd-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6debd-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-10000ft-plans-from-hello-gallery"></a><span data-ttu-id="6debd-123">Lägger till 10 000 ft planer från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="6debd-123">Adding 10,000ft Plans from hello gallery</span></span>
<span data-ttu-id="6debd-124">tooconfigure hello integrering av 10 000 ft planer i Azure AD, behöver du tooadd 10 000 ft planer hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="6debd-124">tooconfigure hello integration of 10,000ft Plans into Azure AD, you need tooadd 10,000ft Plans from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="6debd-125">**tooadd 10 000 ft planer från hello-galleriet, utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="6debd-125">**tooadd 10,000ft Plans from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="6debd-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="6debd-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="6debd-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="6debd-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="6debd-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="6debd-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="6debd-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6debd-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="6debd-133">Skriv i sökrutan hello **10 000 ft planer**.</span><span class="sxs-lookup"><span data-stu-id="6debd-133">In hello search box, type **10,000ft Plans**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_search.png)

5. <span data-ttu-id="6debd-135">Markera hello resultat på panelen **10 000 ft planer**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="6debd-135">In hello results panel, select **10,000ft Plans**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="6debd-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6debd-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="6debd-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med 10 000 ft planer baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="6debd-138">In this section, you configure and test Azure AD single sign-on with 10,000ft Plans based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="6debd-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i 10 000 ft planer är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6debd-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in 10,000ft Plans is tooa user in Azure AD.</span></span> <span data-ttu-id="6debd-140">Med andra ord en länk mellan en Azure AD-användare och hello relaterade användare i 10 000 ft planer måste toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="6debd-140">In other words, a link relationship between an Azure AD user and hello related user in 10,000ft Plans needs toobe established.</span></span>

<span data-ttu-id="6debd-141">Tilldela hello värdet hello i 10 000 ft planer **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="6debd-141">In 10,000ft Plans, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="6debd-142">tooconfigure och testa Azure AD enkel inloggning med 10 000 ft planer, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="6debd-142">tooconfigure and test Azure AD single sign-on with 10,000ft Plans, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="6debd-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="6debd-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="6debd-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6debd-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6debd-145">**[Skapa en 10 000 ft planer testanvändare](#creating-a-10000ft-plans-test-user)**  -toohave en motsvarighet för Britta Simon i 10 000 ft planer som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="6debd-145">**[Creating a 10,000ft Plans test user](#creating-a-10000ft-plans-test-user)** - toohave a counterpart of Britta Simon in 10,000ft Plans that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="6debd-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6debd-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6debd-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="6debd-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="6debd-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6debd-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="6debd-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i 10 000 ft planer för programmet.</span><span class="sxs-lookup"><span data-stu-id="6debd-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your 10,000ft Plans application.</span></span>

<span data-ttu-id="6debd-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med 10 000 ft planer:**</span><span class="sxs-lookup"><span data-stu-id="6debd-150">**tooconfigure Azure AD single sign-on with 10,000ft Plans, perform hello following steps:**</span></span>

1. <span data-ttu-id="6debd-151">I hello Azure-portalen på hello **10 000 ft planer** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="6debd-151">In hello Azure portal, on hello **10,000ft Plans** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="6debd-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6debd-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_samlbase.png)

3. <span data-ttu-id="6debd-155">På hello **10 000 ft planer domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="6debd-155">On hello **10,000ft Plans Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_url.png)

    <span data-ttu-id="6debd-157">a.</span><span class="sxs-lookup"><span data-stu-id="6debd-157">a.</span></span> <span data-ttu-id="6debd-158">I hello **inloggnings-URL** textruta typen hello URL:`https://app.10000ft.com`</span><span class="sxs-lookup"><span data-stu-id="6debd-158">In hello **Sign-on URL** textbox, type hello URL: `https://app.10000ft.com`</span></span>

    <span data-ttu-id="6debd-159">b.</span><span class="sxs-lookup"><span data-stu-id="6debd-159">b.</span></span> <span data-ttu-id="6debd-160">I hello **identifierare** textruta typen hello URL:`https://app.10000ft.com/saml/metadata`</span><span class="sxs-lookup"><span data-stu-id="6debd-160">In hello **Identifier** textbox, type hello URL: `https://app.10000ft.com/saml/metadata`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="6debd-161">Hej värde för **identifierare** skiljer sig om du har en anpassad domän.</span><span class="sxs-lookup"><span data-stu-id="6debd-161">hello value for **Identifier** is different if you have a custom domain.</span></span> <span data-ttu-id="6debd-162">Kontakta [10 000 ft planer supportteamet](https://www.10000ft.com/plans/support) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="6debd-162">Contact [10,000ft Plans support team](https://www.10000ft.com/plans/support) tooget this value.</span></span> 
 
4. <span data-ttu-id="6debd-163">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Raw)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="6debd-163">On hello **SAML Signing Certificate** section, click **Certificate(Raw)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_certificate.png) 

5. <span data-ttu-id="6debd-165">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="6debd-165">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="6debd-167">På hello **10 000 ft planer Configuration** klickar du på **konfigurera 10 000 ft planer** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="6debd-167">On hello **10,000ft Plans Configuration** section, click **Configure 10,000ft Plans** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="6debd-168">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="6debd-168">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_configure.png) 

7. <span data-ttu-id="6debd-170">tooconfigure enkel inloggning på **10 000 ft planer** sida, behöver du toosend hello hämtas **Certificate(Raw), Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** för[ 10 000 ft planer supportteamet](https://www.10000ft.com/plans/support).</span><span class="sxs-lookup"><span data-stu-id="6debd-170">tooconfigure single sign-on on **10,000ft Plans** side, you need toosend hello downloaded **Certificate(Raw), Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[10,000ft Plans support team](https://www.10000ft.com/plans/support).</span></span>

> [!TIP]
> <span data-ttu-id="6debd-171">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="6debd-171">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="6debd-172">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="6debd-172">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="6debd-173">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="6debd-173">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="6debd-174">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="6debd-174">Creating an Azure AD test user</span></span>
<span data-ttu-id="6debd-175">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6debd-175">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="6debd-177">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="6debd-177">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="6debd-178">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="6debd-178">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-10000ftplans-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="6debd-180">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="6debd-180">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-10000ftplans-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="6debd-182">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6debd-182">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-10000ftplans-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="6debd-184">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="6debd-184">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-10000ftplans-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="6debd-186">a.</span><span class="sxs-lookup"><span data-stu-id="6debd-186">a.</span></span> <span data-ttu-id="6debd-187">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6debd-187">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6debd-188">b.</span><span class="sxs-lookup"><span data-stu-id="6debd-188">b.</span></span> <span data-ttu-id="6debd-189">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="6debd-189">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="6debd-190">c.</span><span class="sxs-lookup"><span data-stu-id="6debd-190">c.</span></span> <span data-ttu-id="6debd-191">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="6debd-191">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="6debd-192">d.</span><span class="sxs-lookup"><span data-stu-id="6debd-192">d.</span></span> <span data-ttu-id="6debd-193">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="6debd-193">Click **Create**.</span></span>
 
### <a name="creating-a-10000ft-plans-test-user"></a><span data-ttu-id="6debd-194">Skapa en 10 000 ft testanvändare planer</span><span class="sxs-lookup"><span data-stu-id="6debd-194">Creating a 10,000ft Plans test user</span></span>

<span data-ttu-id="6debd-195">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i 10 000 ft planer.</span><span class="sxs-lookup"><span data-stu-id="6debd-195">hello objective of this section is toocreate a user called Britta Simon in 10,000ft Plans.</span></span> <span data-ttu-id="6debd-196">10 000 ft planer stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="6debd-196">10,000ft Plans supports just-in-time provisioning, which is by default enabled.</span></span> <span data-ttu-id="6debd-197">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="6debd-197">There is no action item for you in this section.</span></span> <span data-ttu-id="6debd-198">En ny användare skapas under ett försök tooaccess 10 000 ft planer om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="6debd-198">A new user is created during an attempt tooaccess 10,000ft Plans if it doesn't exist yet.</span></span> 

> [!NOTE]
> <span data-ttu-id="6debd-199">Om du behöver toocreate en användare manuellt, måste toocontact hello [10 000 ft planer supportteamet](https://www.10000ft.com/plans/support).</span><span class="sxs-lookup"><span data-stu-id="6debd-199">If you need toocreate a user manually, you need toocontact hello [10,000ft Plans support team](https://www.10000ft.com/plans/support).</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="6debd-200">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="6debd-200">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="6debd-201">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst too10, 000ft planer.</span><span class="sxs-lookup"><span data-stu-id="6debd-201">In this section, you enable Britta Simon toouse Azure single sign-on by granting access too10,000ft Plans.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="6debd-203">**tooassign Britta Simon too10 000ft planer utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="6debd-203">**tooassign Britta Simon too10,000ft Plans, perform hello following steps:**</span></span>

1. <span data-ttu-id="6debd-204">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="6debd-204">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="6debd-206">Välj i listan med program hello **10 000 ft planer**.</span><span class="sxs-lookup"><span data-stu-id="6debd-206">In hello applications list, select **10,000ft Plans**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_app.png) 

3. <span data-ttu-id="6debd-208">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="6debd-208">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="6debd-210">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="6debd-210">Click **Add** button.</span></span> <span data-ttu-id="6debd-211">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6debd-211">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="6debd-213">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="6debd-213">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="6debd-214">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6debd-214">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6debd-215">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6debd-215">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="6debd-216">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6debd-216">Testing single sign-on</span></span>

<span data-ttu-id="6debd-217">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="6debd-217">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>  
<span data-ttu-id="6debd-218">Du bör få automatiskt inloggade tooyour 10 000 ft planer programmet när du klickar på hello 10 000 ft planer panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="6debd-218">When you click hello 10,000ft Plans tile in hello Access Panel, you should get automatically signed-on tooyour 10,000ft Plans application.</span></span>
 
## <a name="additional-resources"></a><span data-ttu-id="6debd-219">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="6debd-219">Additional resources</span></span>

* [<span data-ttu-id="6debd-220">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6debd-220">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6debd-221">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="6debd-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_203.png

