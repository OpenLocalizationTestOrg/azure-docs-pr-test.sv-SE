---
title: "Självstudier: Azure Active Directory-integrering med Sciforma | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Sciforma."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: abbfb5ac-7687-4153-b263-8090102dae37
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2017
ms.author: jeedes
ms.openlocfilehash: fca6237196061355e38d431e958964a45246f965
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sciforma"></a><span data-ttu-id="7f50e-103">Självstudier: Azure Active Directory-integrering med Sciforma</span><span class="sxs-lookup"><span data-stu-id="7f50e-103">Tutorial: Azure Active Directory integration with Sciforma</span></span>

<span data-ttu-id="7f50e-104">I kursen får du lära dig hur toointegrate Sciforma med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="7f50e-104">In this tutorial, you learn how toointegrate Sciforma with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7f50e-105">Integrera Sciforma med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="7f50e-105">Integrating Sciforma with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="7f50e-106">Du kan styra i Azure AD som har åtkomst till tooSciforma</span><span class="sxs-lookup"><span data-stu-id="7f50e-106">You can control in Azure AD who has access tooSciforma</span></span>
- <span data-ttu-id="7f50e-107">Du kan aktivera din användare tooautomatically get inloggade tooSciforma (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="7f50e-107">You can enable your users tooautomatically get signed-on tooSciforma (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7f50e-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="7f50e-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="7f50e-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7f50e-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7f50e-110">Krav</span><span class="sxs-lookup"><span data-stu-id="7f50e-110">Prerequisites</span></span>

<span data-ttu-id="7f50e-111">tooconfigure Azure AD-integrering med Sciforma, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="7f50e-111">tooconfigure Azure AD integration with Sciforma, you need hello following items:</span></span>

- <span data-ttu-id="7f50e-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="7f50e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7f50e-113">En Sciforma enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="7f50e-113">A Sciforma single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7f50e-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="7f50e-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7f50e-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="7f50e-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7f50e-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="7f50e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7f50e-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7f50e-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7f50e-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="7f50e-118">Scenario description</span></span>
<span data-ttu-id="7f50e-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="7f50e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7f50e-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="7f50e-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7f50e-121">Att lägga till Sciforma från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="7f50e-121">Adding Sciforma from hello gallery</span></span>
2. <span data-ttu-id="7f50e-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7f50e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sciforma-from-hello-gallery"></a><span data-ttu-id="7f50e-123">Att lägga till Sciforma från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="7f50e-123">Adding Sciforma from hello gallery</span></span>
<span data-ttu-id="7f50e-124">tooconfigure hello integrering av Sciforma i Azure AD, behöver du tooadd Sciforma hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="7f50e-124">tooconfigure hello integration of Sciforma into Azure AD, you need tooadd Sciforma from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="7f50e-125">**tooadd Sciforma från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="7f50e-125">**tooadd Sciforma from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="7f50e-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="7f50e-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7f50e-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="7f50e-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="7f50e-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="7f50e-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="7f50e-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7f50e-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="7f50e-133">Skriv i sökrutan hello **Sciforma**.</span><span class="sxs-lookup"><span data-stu-id="7f50e-133">In hello search box, type **Sciforma**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sciforma-tutorial/tutorial_sciforma_search.png)

5. <span data-ttu-id="7f50e-135">Markera hello resultat på panelen **Sciforma**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="7f50e-135">In hello results panel, select **Sciforma**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sciforma-tutorial/tutorial_sciforma_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="7f50e-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7f50e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="7f50e-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Sciforma baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="7f50e-138">In this section, you configure and test Azure AD single sign-on with Sciforma based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="7f50e-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Sciforma är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7f50e-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Sciforma is tooa user in Azure AD.</span></span> <span data-ttu-id="7f50e-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Sciforma toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="7f50e-140">In other words, a link relationship between an Azure AD user and hello related user in Sciforma needs toobe established.</span></span>

<span data-ttu-id="7f50e-141">I Sciforma, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="7f50e-141">In Sciforma, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="7f50e-142">tooconfigure och testa Azure AD enkel inloggning med Sciforma, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="7f50e-142">tooconfigure and test Azure AD single sign-on with Sciforma, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="7f50e-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="7f50e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="7f50e-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7f50e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7f50e-145">**[Skapa en testanvändare Sciforma](#creating-a-sciforma-test-user)**  -toohave en motsvarighet för Britta Simon i Sciforma som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="7f50e-145">**[Creating a Sciforma test user](#creating-a-sciforma-test-user)** - toohave a counterpart of Britta Simon in Sciforma that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="7f50e-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="7f50e-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7f50e-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="7f50e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="7f50e-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7f50e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="7f50e-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Sciforma program.</span><span class="sxs-lookup"><span data-stu-id="7f50e-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Sciforma application.</span></span>

<span data-ttu-id="7f50e-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Sciforma:**</span><span class="sxs-lookup"><span data-stu-id="7f50e-150">**tooconfigure Azure AD single sign-on with Sciforma, perform hello following steps:**</span></span>

1. <span data-ttu-id="7f50e-151">I hello Azure-portalen på hello **Sciforma** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="7f50e-151">In hello Azure portal, on hello **Sciforma** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="7f50e-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="7f50e-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sciforma-tutorial/tutorial_sciforma_samlbase.png)

3. <span data-ttu-id="7f50e-155">På hello **Sciforma domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="7f50e-155">On hello **Sciforma Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sciforma-tutorial/tutorial_sciforma_url.png)

    <span data-ttu-id="7f50e-157">a.</span><span class="sxs-lookup"><span data-stu-id="7f50e-157">a.</span></span> <span data-ttu-id="7f50e-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.sciforma.net/sciforma/main.html`</span><span class="sxs-lookup"><span data-stu-id="7f50e-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.sciforma.net/sciforma/main.html`</span></span>

    <span data-ttu-id="7f50e-159">b.</span><span class="sxs-lookup"><span data-stu-id="7f50e-159">b.</span></span> <span data-ttu-id="7f50e-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.sciforma.net/sciforma/saml`</span><span class="sxs-lookup"><span data-stu-id="7f50e-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.sciforma.net/sciforma/saml`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="7f50e-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="7f50e-161">These values are not real.</span></span> <span data-ttu-id="7f50e-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="7f50e-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="7f50e-163">Kontakta [Sciforma klienten supportteamet](http://www.sciforma.com/company/contact_us) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="7f50e-163">Contact [Sciforma Client support team](http://www.sciforma.com/company/contact_us) tooget these values.</span></span> 
 


4. <span data-ttu-id="7f50e-164">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="7f50e-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sciforma-tutorial/tutorial_sciforma_certificate.png) 

5. <span data-ttu-id="7f50e-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="7f50e-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sciforma-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="7f50e-168">tooconfigure enkel inloggning på **Sciforma** sida, behöver du toosend hello hämtas **XML-Metadata för** för[Sciforma supportteamet](http://www.sciforma.com/company/contact_us).</span><span class="sxs-lookup"><span data-stu-id="7f50e-168">tooconfigure single sign-on on **Sciforma** side, you need toosend hello downloaded **Metadata XML** too[Sciforma support team](http://www.sciforma.com/company/contact_us).</span></span>

> [!TIP]
> <span data-ttu-id="7f50e-169">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="7f50e-169">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="7f50e-170">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="7f50e-170">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="7f50e-171">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7f50e-171">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="7f50e-172">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="7f50e-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="7f50e-173">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7f50e-173">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="7f50e-175">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="7f50e-175">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="7f50e-176">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="7f50e-176">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sciforma-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7f50e-178">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="7f50e-178">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sciforma-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7f50e-180">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7f50e-180">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sciforma-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7f50e-182">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="7f50e-182">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sciforma-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7f50e-184">a.</span><span class="sxs-lookup"><span data-stu-id="7f50e-184">a.</span></span> <span data-ttu-id="7f50e-185">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7f50e-185">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7f50e-186">b.</span><span class="sxs-lookup"><span data-stu-id="7f50e-186">b.</span></span> <span data-ttu-id="7f50e-187">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="7f50e-187">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="7f50e-188">c.</span><span class="sxs-lookup"><span data-stu-id="7f50e-188">c.</span></span> <span data-ttu-id="7f50e-189">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="7f50e-189">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="7f50e-190">d.</span><span class="sxs-lookup"><span data-stu-id="7f50e-190">d.</span></span> <span data-ttu-id="7f50e-191">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="7f50e-191">Click **Create**.</span></span>
 
### <a name="creating-a-sciforma-test-user"></a><span data-ttu-id="7f50e-192">Skapa en testanvändare Sciforma</span><span class="sxs-lookup"><span data-stu-id="7f50e-192">Creating a Sciforma test user</span></span>

<span data-ttu-id="7f50e-193">Det finns inga uppgiften för du tooconfigure användaretablering tooSciforma.</span><span class="sxs-lookup"><span data-stu-id="7f50e-193">There is no action item for you tooconfigure user provisioning tooSciforma.</span></span> <span data-ttu-id="7f50e-194">När en tilldelad användare försöker toolog i tooSciforma med hello åtkomstpanelen, kontrollerar Sciforma om hello användaren finns.</span><span class="sxs-lookup"><span data-stu-id="7f50e-194">When an assigned user tries toolog in tooSciforma using hello access panel, Sciforma checks whether hello user exists.</span></span>  

* <span data-ttu-id="7f50e-195">Om det finns inget användarkonto ännu, skapas den automatiskt av Sciforma.</span><span class="sxs-lookup"><span data-stu-id="7f50e-195">If there is no user account available yet, it is automatically created by Sciforma.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="7f50e-196">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="7f50e-196">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="7f50e-197">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSciforma.</span><span class="sxs-lookup"><span data-stu-id="7f50e-197">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSciforma.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="7f50e-199">**tooassign Britta Simon tooSciforma utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="7f50e-199">**tooassign Britta Simon tooSciforma, perform hello following steps:**</span></span>

1. <span data-ttu-id="7f50e-200">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="7f50e-200">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="7f50e-202">Välj i listan med program hello **Sciforma**.</span><span class="sxs-lookup"><span data-stu-id="7f50e-202">In hello applications list, select **Sciforma**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sciforma-tutorial/tutorial_sciforma_app.png) 

3. <span data-ttu-id="7f50e-204">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="7f50e-204">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="7f50e-206">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="7f50e-206">Click **Add** button.</span></span> <span data-ttu-id="7f50e-207">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7f50e-207">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="7f50e-209">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="7f50e-209">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="7f50e-210">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7f50e-210">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7f50e-211">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7f50e-211">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="7f50e-212">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7f50e-212">Testing single sign-on</span></span>

<span data-ttu-id="7f50e-213">Om du vill tootest dina inställningar för enkel inloggning, öppna hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="7f50e-213">If you want tootest your single sign-on settings, open hello Access Panel.</span></span> <span data-ttu-id="7f50e-214">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="7f50e-214">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7f50e-215">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="7f50e-215">Additional resources</span></span>

* [<span data-ttu-id="7f50e-216">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7f50e-216">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7f50e-217">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7f50e-217">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sciforma-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sciforma-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sciforma-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sciforma-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sciforma-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sciforma-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sciforma-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sciforma-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sciforma-tutorial/tutorial_general_203.png

