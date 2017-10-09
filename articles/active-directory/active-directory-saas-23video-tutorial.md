---
title: "Självstudier: Azure Active Directory-integrering med 23 Video | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och 23 Video."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5e73dd1d-3995-4a73-b9cf-1b2318d49cb3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: jeedes
ms.openlocfilehash: 3430e4db3cd1114db62233e6699618071a3646ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-23-video"></a><span data-ttu-id="a8940-103">Självstudier: Azure Active Directory-integrering med 23 Video</span><span class="sxs-lookup"><span data-stu-id="a8940-103">Tutorial: Azure Active Directory integration with 23 Video</span></span>

<span data-ttu-id="a8940-104">I kursen får du lära dig hur toointegrate 23 Video med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="a8940-104">In this tutorial, you learn how toointegrate 23 Video with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a8940-105">Integrera 23 innehåller Video med Azure AD hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="a8940-105">Integrating 23 Video with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="a8940-106">Du kan styra i Azure AD som har åtkomst too23 Video</span><span class="sxs-lookup"><span data-stu-id="a8940-106">You can control in Azure AD who has access too23 Video</span></span>
- <span data-ttu-id="a8940-107">Du kan aktivera användarna tooautomatically hämta inloggade too23 Video (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="a8940-107">You can enable your users tooautomatically get signed-on too23 Video (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a8940-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="a8940-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="a8940-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a8940-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a8940-110">Krav</span><span class="sxs-lookup"><span data-stu-id="a8940-110">Prerequisites</span></span>

<span data-ttu-id="a8940-111">tooconfigure Azure AD-integrering med 23 Video, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="a8940-111">tooconfigure Azure AD integration with 23 Video, you need hello following items:</span></span>

- <span data-ttu-id="a8940-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="a8940-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a8940-113">En 23 Video enkel inloggning på aktiverat prenumeration</span><span class="sxs-lookup"><span data-stu-id="a8940-113">A 23 Video single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a8940-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="a8940-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a8940-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="a8940-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a8940-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="a8940-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a8940-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a8940-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a8940-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="a8940-118">Scenario description</span></span>
<span data-ttu-id="a8940-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="a8940-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a8940-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="a8940-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a8940-121">Att lägga till 23 Video från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="a8940-121">Adding 23 Video from hello gallery</span></span>
2. <span data-ttu-id="a8940-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a8940-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-23-video-from-hello-gallery"></a><span data-ttu-id="a8940-123">Att lägga till 23 Video från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="a8940-123">Adding 23 Video from hello gallery</span></span>
<span data-ttu-id="a8940-124">tooconfigure hello integrering av 23 Video i Azure AD, behöver du tooadd 23 Video hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="a8940-124">tooconfigure hello integration of 23 Video into Azure AD, you need tooadd 23 Video from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="a8940-125">**tooadd 23 Video från galleriet hello utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a8940-125">**tooadd 23 Video from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="a8940-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a8940-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a8940-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="a8940-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="a8940-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="a8940-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="a8940-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a8940-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="a8940-133">Skriv i sökrutan hello **23 Video**.</span><span class="sxs-lookup"><span data-stu-id="a8940-133">In hello search box, type **23 Video**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-23video-tutorial/tutorial_23video_search.png)

5. <span data-ttu-id="a8940-135">Markera hello resultat på panelen **23 Video**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="a8940-135">In hello results panel, select **23 Video**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-23video-tutorial/tutorial_23video_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a8940-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a8940-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a8940-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med 23 Video baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="a8940-138">In this section, you configure and test Azure AD single sign-on with 23 Video based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="a8940-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i 23 videon är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a8940-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in 23 Video is tooa user in Azure AD.</span></span> <span data-ttu-id="a8940-140">Med andra ord en länk relationen mellan en Azure AD-användare och hello relaterade användaren i 23 Video måste toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="a8940-140">In other words, a link relationship between an Azure AD user and hello related user in 23 Video needs toobe established.</span></span>

<span data-ttu-id="a8940-141">Tilldela hello värdet hello i 23 videon **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="a8940-141">In 23 Video, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="a8940-142">tooconfigure och testa Azure AD enkel inloggning med 23 Video, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="a8940-142">tooconfigure and test Azure AD single sign-on with 23 Video, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="a8940-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="a8940-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="a8940-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a8940-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a8940-145">**[Skapa en 23 Video testanvändare](#creating-a-23-video-test-user)**  -toohave en motsvarighet för Britta Simon i 23 Video som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="a8940-145">**[Creating a 23 Video test user](#creating-a-23-video-test-user)** - toohave a counterpart of Britta Simon in 23 Video that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="a8940-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a8940-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a8940-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="a8940-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a8940-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a8940-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a8940-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet 23 Video.</span><span class="sxs-lookup"><span data-stu-id="a8940-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your 23 Video application.</span></span>

<span data-ttu-id="a8940-150">**Utför följande hello tooconfigure Azure AD enkel inloggning med 23 Video:**</span><span class="sxs-lookup"><span data-stu-id="a8940-150">**tooconfigure Azure AD single sign-on with 23 Video, perform hello following steps:**</span></span>

1. <span data-ttu-id="a8940-151">I hello Azure-portalen på hello **23 Video** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="a8940-151">In hello Azure portal, on hello **23 Video** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="a8940-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a8940-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-23video-tutorial/tutorial_23video_samlbase.png)

3. <span data-ttu-id="a8940-155">På hello **23 Video domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a8940-155">On hello **23 Video Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-23video-tutorial/tutorial_23video_url.png)

    <span data-ttu-id="a8940-157">a.</span><span class="sxs-lookup"><span data-stu-id="a8940-157">a.</span></span> <span data-ttu-id="a8940-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.23video.com`</span><span class="sxs-lookup"><span data-stu-id="a8940-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.23video.com`</span></span>

    <span data-ttu-id="a8940-159">b.</span><span class="sxs-lookup"><span data-stu-id="a8940-159">b.</span></span> <span data-ttu-id="a8940-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://www.23video.com/saml/trust/<uniqueid>`</span><span class="sxs-lookup"><span data-stu-id="a8940-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://www.23video.com/saml/trust/<uniqueid>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="a8940-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="a8940-161">These values are not real.</span></span> <span data-ttu-id="a8940-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="a8940-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="a8940-163">Kontakta [23 Video klienten supportteamet](mailto:support@23company.com) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="a8940-163">Contact [23 Video Client support team](mailto:support@23company.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="a8940-164">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="a8940-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-23video-tutorial/tutorial_23video_certificate.png) 

5. <span data-ttu-id="a8940-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="a8940-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-23video-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="a8940-168">På hello **23 Video Configuration** klickar du på **konfigurera 23 Video** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="a8940-168">On hello **23 Video Configuration** section, click **Configure 23 Video** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="a8940-169">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="a8940-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-23video-tutorial/tutorial_23video_configure.png) 

7. <span data-ttu-id="a8940-171">tooconfigure enkel inloggning på **23 Video** sida, behöver du toosend hello hämtas **certifikat (Base64)**, **Sign-Out URL, SAML enhets-ID och SAML inloggning tjänst-URL för enkel**för[23 Video supportteamet](mailto:support@23company.com).</span><span class="sxs-lookup"><span data-stu-id="a8940-171">tooconfigure single sign-on on **23 Video** side, you need toosend hello downloaded **Certificate (Base64)**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[23 Video support team](mailto:support@23company.com).</span></span> 


> [!TIP]
> <span data-ttu-id="a8940-172">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="a8940-172">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="a8940-173">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="a8940-173">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="a8940-174">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a8940-174">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a8940-175">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="a8940-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="a8940-176">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a8940-176">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="a8940-178">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a8940-178">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="a8940-179">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a8940-179">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-23video-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a8940-181">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="a8940-181">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-23video-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a8940-183">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a8940-183">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-23video-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a8940-185">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a8940-185">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-23video-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a8940-187">a.</span><span class="sxs-lookup"><span data-stu-id="a8940-187">a.</span></span> <span data-ttu-id="a8940-188">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a8940-188">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a8940-189">b.</span><span class="sxs-lookup"><span data-stu-id="a8940-189">b.</span></span> <span data-ttu-id="a8940-190">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a8940-190">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a8940-191">c.</span><span class="sxs-lookup"><span data-stu-id="a8940-191">c.</span></span> <span data-ttu-id="a8940-192">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="a8940-192">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="a8940-193">d.</span><span class="sxs-lookup"><span data-stu-id="a8940-193">d.</span></span> <span data-ttu-id="a8940-194">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="a8940-194">Click **Create**.</span></span>
 
### <a name="creating-a-23-video-test-user"></a><span data-ttu-id="a8940-195">Skapa en 23 Video testanvändare</span><span class="sxs-lookup"><span data-stu-id="a8940-195">Creating a 23 Video test user</span></span>

<span data-ttu-id="a8940-196">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i 23 videon.</span><span class="sxs-lookup"><span data-stu-id="a8940-196">hello objective of this section is toocreate a user called Britta Simon in 23 Video.</span></span>

<span data-ttu-id="a8940-197">**toocreate en användare som kallas Britta Simon i 23 videon utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a8940-197">**toocreate a user called Britta Simon in 23 Video, perform hello following steps:**</span></span>

1. <span data-ttu-id="a8940-198">Inloggning tooyour 23 Video företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="a8940-198">Sign on tooyour 23 Video company site as administrator.</span></span>

2. <span data-ttu-id="a8940-199">Gå för**inställningar**.</span><span class="sxs-lookup"><span data-stu-id="a8940-199">Go too**Settings**.</span></span>
 
3. <span data-ttu-id="a8940-200">I **användare** klickar du på **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="a8940-200">In **Users** section, click **Configure**.</span></span>
   
    ![Tilldela användare][400]

4. <span data-ttu-id="a8940-202">Klicka på **lägga till en ny användare**.</span><span class="sxs-lookup"><span data-stu-id="a8940-202">Click **Add a new user**.</span></span> 
   
    ![Tilldela användare][401]

5. <span data-ttu-id="a8940-204">I hello **bjuda in någon toojoin platsen** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a8940-204">In hello **Invite someone toojoin this site** section, perform hello following steps:</span></span>
   
    ![Tilldela användare][402]

    <span data-ttu-id="a8940-206">a.</span><span class="sxs-lookup"><span data-stu-id="a8940-206">a.</span></span> <span data-ttu-id="a8940-207">I hello **e-postadresser** textruta Skriv Britta Simon e-postadress i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a8940-207">In hello **E-mail addresses** textbox, type Britta Simon's email address in Azure AD.</span></span>  
 
    <span data-ttu-id="a8940-208">b.</span><span class="sxs-lookup"><span data-stu-id="a8940-208">b.</span></span> <span data-ttu-id="a8940-209">Klicka på **Lägg till hello användare**.</span><span class="sxs-lookup"><span data-stu-id="a8940-209">Click **Add hello user**.</span></span>   

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="a8940-210">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="a8940-210">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="a8940-211">I det här avsnittet kan du aktivera Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst till too23 Video.</span><span class="sxs-lookup"><span data-stu-id="a8940-211">In this section, you enable Britta Simon toouse Azure single sign-on by granting access too23 Video.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="a8940-213">**tooassign Britta Simon too23 Video, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="a8940-213">**tooassign Britta Simon too23 Video, perform hello following steps:**</span></span>

1. <span data-ttu-id="a8940-214">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="a8940-214">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="a8940-216">Välj i listan med program hello **23 Video**.</span><span class="sxs-lookup"><span data-stu-id="a8940-216">In hello applications list, select **23 Video**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-23video-tutorial/tutorial_23video_app.png) 

3. <span data-ttu-id="a8940-218">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="a8940-218">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="a8940-220">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="a8940-220">Click **Add** button.</span></span> <span data-ttu-id="a8940-221">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a8940-221">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="a8940-223">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="a8940-223">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="a8940-224">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a8940-224">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a8940-225">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a8940-225">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a8940-226">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a8940-226">Testing single sign-on</span></span>

<span data-ttu-id="a8940-227">hello syftet med det här avsnittet är tootest din Azure AD SSO konfiguration av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="a8940-227">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="a8940-228">Du bör få automatiskt inloggade tooyour 23 Video programmet när du klickar på hello 23 Video-panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="a8940-228">When you click hello 23 Video tile in hello Access Panel, you should get automatically signed-on tooyour 23 Video application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="a8940-229">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="a8940-229">Additional resources</span></span>

* [<span data-ttu-id="a8940-230">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a8940-230">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a8940-231">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a8940-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-23video-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-23video-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-23video-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-23video-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-23video-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-23video-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-23video-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-23video-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-23video-tutorial/tutorial_general_203.png

[400]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_10.png
[401]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_11.png
[402]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_12.png
