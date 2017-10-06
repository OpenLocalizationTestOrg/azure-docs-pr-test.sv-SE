---
title: "Självstudier: Azure Active Directory-integrering med Mindflash | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Mindflash."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bdf91993-aaaa-4598-89b7-77ef8ca065d5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: a1bc327ea3867287103acbb64d30f0a8d7d4c5e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mindflash"></a><span data-ttu-id="4aaf5-103">Självstudier: Azure Active Directory-integrering med Mindflash</span><span class="sxs-lookup"><span data-stu-id="4aaf5-103">Tutorial: Azure Active Directory integration with Mindflash</span></span>

<span data-ttu-id="4aaf5-104">I kursen får du lära dig hur toointegrate Mindflash med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="4aaf5-104">In this tutorial, you learn how toointegrate Mindflash with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="4aaf5-105">Integrera Mindflash med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="4aaf5-105">Integrating Mindflash with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="4aaf5-106">Du kan styra i Azure AD som har åtkomst till tooMindflash</span><span class="sxs-lookup"><span data-stu-id="4aaf5-106">You can control in Azure AD who has access tooMindflash</span></span>
- <span data-ttu-id="4aaf5-107">Du kan aktivera din användare tooautomatically get inloggade tooMindflash (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="4aaf5-107">You can enable your users tooautomatically get signed-on tooMindflash (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="4aaf5-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="4aaf5-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="4aaf5-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4aaf5-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4aaf5-110">Krav</span><span class="sxs-lookup"><span data-stu-id="4aaf5-110">Prerequisites</span></span>

<span data-ttu-id="4aaf5-111">tooconfigure Azure AD-integrering med Mindflash, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="4aaf5-111">tooconfigure Azure AD integration with Mindflash, you need hello following items:</span></span>

- <span data-ttu-id="4aaf5-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="4aaf5-112">An Azure AD subscription</span></span>
- <span data-ttu-id="4aaf5-113">En Mindflash enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="4aaf5-113">A Mindflash single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="4aaf5-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="4aaf5-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="4aaf5-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="4aaf5-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="4aaf5-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4aaf5-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4aaf5-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="4aaf5-118">Scenario description</span></span>
<span data-ttu-id="4aaf5-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="4aaf5-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="4aaf5-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4aaf5-121">Att lägga till Mindflash från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="4aaf5-121">Adding Mindflash from hello gallery</span></span>
2. <span data-ttu-id="4aaf5-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4aaf5-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-mindflash-from-hello-gallery"></a><span data-ttu-id="4aaf5-123">Att lägga till Mindflash från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="4aaf5-123">Adding Mindflash from hello gallery</span></span>
<span data-ttu-id="4aaf5-124">tooconfigure hello integrering av Mindflash i Azure AD, behöver du tooadd Mindflash hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-124">tooconfigure hello integration of Mindflash into Azure AD, you need tooadd Mindflash from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="4aaf5-125">**tooadd Mindflash från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="4aaf5-125">**tooadd Mindflash from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="4aaf5-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="4aaf5-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="4aaf5-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="4aaf5-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="4aaf5-133">Skriv i sökrutan hello **Mindflash**.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-133">In hello search box, type **Mindflash**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mindflash-tutorial/tutorial_mindflash_search.png)

5. <span data-ttu-id="4aaf5-135">Markera hello resultat på panelen **Mindflash**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-135">In hello results panel, select **Mindflash**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mindflash-tutorial/tutorial_mindflash_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="4aaf5-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4aaf5-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="4aaf5-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Mindflash baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-138">In this section, you configure and test Azure AD single sign-on with Mindflash based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="4aaf5-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Mindflash är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Mindflash is tooa user in Azure AD.</span></span> <span data-ttu-id="4aaf5-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Mindflash toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-140">In other words, a link relationship between an Azure AD user and hello related user in Mindflash needs toobe established.</span></span>

<span data-ttu-id="4aaf5-141">I Mindflash, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-141">In Mindflash, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="4aaf5-142">tooconfigure och testa Azure AD enkel inloggning med Mindflash, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="4aaf5-142">tooconfigure and test Azure AD single sign-on with Mindflash, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="4aaf5-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="4aaf5-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="4aaf5-145">**[Skapa en testanvändare Mindflash](#creating-a-mindflash-test-user)**  -toohave en motsvarighet för Britta Simon i Mindflash som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-145">**[Creating a Mindflash test user](#creating-a-mindflash-test-user)** - toohave a counterpart of Britta Simon in Mindflash that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="4aaf5-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4aaf5-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="4aaf5-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4aaf5-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="4aaf5-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Mindflash program.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Mindflash application.</span></span>

<span data-ttu-id="4aaf5-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Mindflash:**</span><span class="sxs-lookup"><span data-stu-id="4aaf5-150">**tooconfigure Azure AD single sign-on with Mindflash, perform hello following steps:**</span></span>

1. <span data-ttu-id="4aaf5-151">I hello Azure-portalen på hello **Mindflash** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-151">In hello Azure portal, on hello **Mindflash** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="4aaf5-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-mindflash-tutorial/tutorial_mindflash_samlbase.png)

3. <span data-ttu-id="4aaf5-155">På hello **Mindflash domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="4aaf5-155">On hello **Mindflash Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mindflash-tutorial/tutorial_mindflash_url.png)

    <span data-ttu-id="4aaf5-157">a.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-157">a.</span></span> <span data-ttu-id="4aaf5-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.mindflash.com`</span><span class="sxs-lookup"><span data-stu-id="4aaf5-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.mindflash.com`</span></span>

    <span data-ttu-id="4aaf5-159">b.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-159">b.</span></span> <span data-ttu-id="4aaf5-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.mindflash.com`</span><span class="sxs-lookup"><span data-stu-id="4aaf5-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.mindflash.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="4aaf5-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-161">These values are not real.</span></span> <span data-ttu-id="4aaf5-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="4aaf5-163">Kontakta [Mindflash klienten supportteamet](https://www.mindflash.com/contact/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-163">Contact [Mindflash Client support team](https://www.mindflash.com/contact/) tooget these values.</span></span> 
 


4. <span data-ttu-id="4aaf5-164">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mindflash-tutorial/tutorial_mindflash_certificate.png) 

5. <span data-ttu-id="4aaf5-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mindflash-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="4aaf5-168">tooconfigure enkel inloggning på **Mindflash** sida, behöver du toosend hello hämtas **XML-Metadata för** för[Mindflash supportteamet](https://www.mindflash.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="4aaf5-168">tooconfigure single sign-on on **Mindflash** side, you need toosend hello downloaded **Metadata XML** too[Mindflash support team](https://www.mindflash.com/contact/).</span></span>

> [!TIP]
> <span data-ttu-id="4aaf5-169">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="4aaf5-169">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="4aaf5-170">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-170">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="4aaf5-171">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="4aaf5-171">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="4aaf5-172">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="4aaf5-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="4aaf5-173">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-173">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="4aaf5-175">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="4aaf5-175">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="4aaf5-176">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-176">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mindflash-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="4aaf5-178">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-178">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mindflash-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="4aaf5-180">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-180">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mindflash-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="4aaf5-182">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="4aaf5-182">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mindflash-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="4aaf5-184">a.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-184">a.</span></span> <span data-ttu-id="4aaf5-185">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-185">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="4aaf5-186">b.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-186">b.</span></span> <span data-ttu-id="4aaf5-187">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-187">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="4aaf5-188">c.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-188">c.</span></span> <span data-ttu-id="4aaf5-189">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-189">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="4aaf5-190">d.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-190">d.</span></span> <span data-ttu-id="4aaf5-191">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-191">Click **Create**.</span></span>
 
### <a name="creating-a-mindflash-test-user"></a><span data-ttu-id="4aaf5-192">Skapa en testanvändare Mindflash</span><span class="sxs-lookup"><span data-stu-id="4aaf5-192">Creating a Mindflash test user</span></span>

<span data-ttu-id="4aaf5-193">I ordning tooenable Azure AD-användare toolog i Mindflash, måste de etableras i Mindflash.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-193">In order tooenable Azure AD users toolog into Mindflash, they must be provisioned into Mindflash.</span></span> <span data-ttu-id="4aaf5-194">Hello gäller Mindflash är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-194">In hello case of Mindflash, provisioning is a manual task.</span></span>

### <a name="tooprovision-a-user-accounts-perform-hello-following-steps"></a><span data-ttu-id="4aaf5-195">tooprovision användarkonton, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="4aaf5-195">tooprovision a user accounts, perform hello following steps:</span></span>

1. <span data-ttu-id="4aaf5-196">Logga in tooyour **Mindflash** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-196">Log in tooyour **Mindflash** company site as an administrator.</span></span>

2. <span data-ttu-id="4aaf5-197">Gå för**hantera användare**.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-197">Go too**Manage Users**.</span></span>
   
    <span data-ttu-id="4aaf5-198">![Hantera användare](./media/active-directory-saas-mindflash-tutorial/ic787140.png "hantera användare")</span><span class="sxs-lookup"><span data-stu-id="4aaf5-198">![Manage Users](./media/active-directory-saas-mindflash-tutorial/ic787140.png "Manage Users")</span></span>

3. <span data-ttu-id="4aaf5-199">Klicka på hello **Lägg till användare**, och klicka sedan på **ny**.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-199">Click hello **Add Users**, and then click **New**.</span></span>

4. <span data-ttu-id="4aaf5-200">I hello **Lägg till nya användare** avsnittet, utföra hello följa stegen i ett giltigt Azure AD-kontot som du vill tooprovision:</span><span class="sxs-lookup"><span data-stu-id="4aaf5-200">In hello **Add New Users** section, perform hello following steps of a valid Azure AD account you want tooprovision:</span></span>
   
    <span data-ttu-id="4aaf5-201">![Lägga till nya användare](./media/active-directory-saas-mindflash-tutorial/ic787141.png "lägga till nya användare")</span><span class="sxs-lookup"><span data-stu-id="4aaf5-201">![Add New Users](./media/active-directory-saas-mindflash-tutorial/ic787141.png "Add New Users")</span></span>
   
    <span data-ttu-id="4aaf5-202">a.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-202">a.</span></span> <span data-ttu-id="4aaf5-203">I hello **Förnamn** textruta typen **Förnamn** för hello användare som **Britta**.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-203">In hello **First name** textbox, type **First name** of hello user as **Britta**.</span></span>

    <span data-ttu-id="4aaf5-204">b.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-204">b.</span></span> <span data-ttu-id="4aaf5-205">I hello **efternamn** textruta typen **efternamn** för hello användare som **Simon**.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-205">In hello **Last name** textbox, type **Last name** of hello user as **Simon**.</span></span>
    
    <span data-ttu-id="4aaf5-206">c.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-206">c.</span></span> <span data-ttu-id="4aaf5-207">I hello **e-post** textruta typen **e-postadress** för hello användare som  **BrittaSimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="4aaf5-207">In hello **Email** textbox, type **Email Address** of hello user as **BrittaSimon@contoso.com**.</span></span>

    <span data-ttu-id="4aaf5-208">b.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-208">b.</span></span> <span data-ttu-id="4aaf5-209">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-209">Click **Add**.</span></span>

>[!NOTE]
><span data-ttu-id="4aaf5-210">Du kan använda något annat Mindflash användarens konto skapas verktyg eller API: er som tillhandahålls av Mindflash tooprovision AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-210">You can use any other Mindflash user account creation tools or APIs provided by Mindflash tooprovision AAD user accounts.</span></span> 
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="4aaf5-211">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="4aaf5-211">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="4aaf5-212">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooMindflash.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-212">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooMindflash.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="4aaf5-214">**tooassign Britta Simon tooMindflash utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="4aaf5-214">**tooassign Britta Simon tooMindflash, perform hello following steps:**</span></span>

1. <span data-ttu-id="4aaf5-215">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-215">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="4aaf5-217">Välj i listan med program hello **Mindflash**.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-217">In hello applications list, select **Mindflash**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mindflash-tutorial/tutorial_mindflash_app.png) 

3. <span data-ttu-id="4aaf5-219">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-219">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="4aaf5-221">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-221">Click **Add** button.</span></span> <span data-ttu-id="4aaf5-222">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-222">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="4aaf5-224">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-224">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="4aaf5-225">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-225">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4aaf5-226">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-226">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="4aaf5-227">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4aaf5-227">Testing single sign-on</span></span>

<span data-ttu-id="4aaf5-228">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-228">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="4aaf5-229">När du klickar på hello Mindflash panelen i hello åtkomstpanelen bör du hämta inloggningssidan för Mindflash program.</span><span class="sxs-lookup"><span data-stu-id="4aaf5-229">When you click hello Mindflash tile in hello Access Panel, you should get login page of Mindflash application.</span></span>
<span data-ttu-id="4aaf5-230">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="4aaf5-230">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4aaf5-231">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="4aaf5-231">Additional resources</span></span>

* [<span data-ttu-id="4aaf5-232">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="4aaf5-232">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4aaf5-233">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="4aaf5-233">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_203.png

