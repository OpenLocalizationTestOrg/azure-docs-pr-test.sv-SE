---
title: "Självstudier: Azure Active Directory-integrering med tillgångsinformation Bank | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och tillgångsinformation Bank."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3006ad6e-8831-41cd-94aa-7e7ae770ce7b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 32cb355fbe16557eca69dbad1d3e6fbe19b53517
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-asset-bank"></a><span data-ttu-id="ad357-103">Självstudier: Azure Active Directory-integrering med tillgångsinformation Bank</span><span class="sxs-lookup"><span data-stu-id="ad357-103">Tutorial: Azure Active Directory integration with Asset Bank</span></span>

<span data-ttu-id="ad357-104">I kursen får du lära dig hur toointegrate tillgången Bank med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="ad357-104">In this tutorial, you learn how toointegrate Asset Bank with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ad357-105">Integrera tillgången Bank med Azure AD innehåller hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="ad357-105">Integrating Asset Bank with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="ad357-106">Du kan styra i Azure AD som har åtkomst tooAsset Bank</span><span class="sxs-lookup"><span data-stu-id="ad357-106">You can control in Azure AD who has access tooAsset Bank</span></span>
- <span data-ttu-id="ad357-107">Du kan aktivera din användare tooautomatically get inloggade tooAsset Bank (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="ad357-107">You can enable your users tooautomatically get signed-on tooAsset Bank (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ad357-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="ad357-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="ad357-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ad357-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ad357-110">Krav</span><span class="sxs-lookup"><span data-stu-id="ad357-110">Prerequisites</span></span>

<span data-ttu-id="ad357-111">tooconfigure Azure AD-integrering med tillgångsinformation Bank måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="ad357-111">tooconfigure Azure AD integration with Asset Bank, you need hello following items:</span></span>

- <span data-ttu-id="ad357-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="ad357-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ad357-113">En tillgång Bank enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="ad357-113">An Asset Bank single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ad357-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="ad357-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ad357-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="ad357-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ad357-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="ad357-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ad357-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ad357-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ad357-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="ad357-118">Scenario description</span></span>
<span data-ttu-id="ad357-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="ad357-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ad357-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="ad357-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ad357-121">Lägger till tillgången Bank från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="ad357-121">Adding Asset Bank from hello gallery</span></span>
2. <span data-ttu-id="ad357-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ad357-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-asset-bank-from-hello-gallery"></a><span data-ttu-id="ad357-123">Lägger till tillgången Bank från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="ad357-123">Adding Asset Bank from hello gallery</span></span>
<span data-ttu-id="ad357-124">tooconfigure hello integrering av tillgångsinformation Bank i Azure AD, behöver du tooadd tillgången Bank hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="ad357-124">tooconfigure hello integration of Asset Bank into Azure AD, you need tooadd Asset Bank from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="ad357-125">**tooadd tillgången Bank från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="ad357-125">**tooadd Asset Bank from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="ad357-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="ad357-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ad357-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="ad357-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="ad357-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="ad357-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="ad357-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ad357-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="ad357-133">Skriv i sökrutan hello **tillgången Bank**.</span><span class="sxs-lookup"><span data-stu-id="ad357-133">In hello search box, type **Asset Bank**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-assetbank-tutorial/tutorial_assetbank_search.png)

5. <span data-ttu-id="ad357-135">Markera hello resultat på panelen **tillgången Bank**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="ad357-135">In hello results panel, select **Asset Bank**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-assetbank-tutorial/tutorial_assetbank_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ad357-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ad357-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ad357-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med tillgångsinformation Bank baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="ad357-138">In this section, you configure and test Azure AD single sign-on with Asset Bank based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="ad357-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i tillgångsinformation Bank är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ad357-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Asset Bank is tooa user in Azure AD.</span></span> <span data-ttu-id="ad357-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i tillgångsinformation Bank toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="ad357-140">In other words, a link relationship between an Azure AD user and hello related user in Asset Bank needs toobe established.</span></span>

<span data-ttu-id="ad357-141">Tilldela hello värdet för hello i tillgångsinformation Bank **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="ad357-141">In Asset Bank, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="ad357-142">tooconfigure och testa Azure AD enkel inloggning med tillgångsinformation Bank, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="ad357-142">tooconfigure and test Azure AD single sign-on with Asset Bank, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="ad357-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="ad357-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="ad357-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ad357-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ad357-145">**[Skapa en tillgång Bank testanvändare](#creating-an-asset-bank-test-user)**  -toohave en motsvarighet för Britta Simon i tillgångsinformation Bank som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="ad357-145">**[Creating an Asset Bank test user](#creating-an-asset-bank-test-user)** - toohave a counterpart of Britta Simon in Asset Bank that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="ad357-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ad357-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ad357-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="ad357-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ad357-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ad357-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ad357-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillgångsinformation Bank-program.</span><span class="sxs-lookup"><span data-stu-id="ad357-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Asset Bank application.</span></span>

<span data-ttu-id="ad357-150">**Utför följande hello tooconfigure Azure AD enkel inloggning med tillgångsinformation Bank:**</span><span class="sxs-lookup"><span data-stu-id="ad357-150">**tooconfigure Azure AD single sign-on with Asset Bank, perform hello following steps:**</span></span>

1. <span data-ttu-id="ad357-151">I hello Azure-portalen på hello **tillgången Bank** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="ad357-151">In hello Azure portal, on hello **Asset Bank** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="ad357-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ad357-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-assetbank-tutorial/tutorial_assetbank_samlbase.png)

3. <span data-ttu-id="ad357-155">På hello **tillgången Bank domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="ad357-155">On hello **Asset Bank Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-assetbank-tutorial/tutorial_assetbank_url.png)

    <span data-ttu-id="ad357-157">a.</span><span class="sxs-lookup"><span data-stu-id="ad357-157">a.</span></span> <span data-ttu-id="ad357-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.assetbank-server.com`</span><span class="sxs-lookup"><span data-stu-id="ad357-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.assetbank-server.com`</span></span>

    <span data-ttu-id="ad357-159">b.</span><span class="sxs-lookup"><span data-stu-id="ad357-159">b.</span></span> <span data-ttu-id="ad357-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.assetbank-server.com/shibboleth`</span><span class="sxs-lookup"><span data-stu-id="ad357-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.assetbank-server.com/shibboleth`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="ad357-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="ad357-161">These values are not real.</span></span> <span data-ttu-id="ad357-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="ad357-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="ad357-163">Kontakta [tillgången Bank klienten supportteamet](mailto:support@assetbank.co.uk) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="ad357-163">Contact [Asset Bank Client support team](mailto:support@assetbank.co.uk) tooget these values.</span></span> 
 
4. <span data-ttu-id="ad357-164">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="ad357-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-assetbank-tutorial/tutorial_assetbank_certificate.png) 

5. <span data-ttu-id="ad357-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="ad357-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-assetbank-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="ad357-168">tooconfigure enkel inloggning på **tillgången Bank** sida, behöver du toosend hello hämtas **XML-Metadata för** för[tillgången Bank supportteamet](mailto:support@assetbank.co.uk).</span><span class="sxs-lookup"><span data-stu-id="ad357-168">tooconfigure single sign-on on **Asset Bank** side, you need toosend hello downloaded **Metadata XML** too[Asset Bank support team](mailto:support@assetbank.co.uk).</span></span> 


> [!TIP]
> <span data-ttu-id="ad357-169">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="ad357-169">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="ad357-170">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="ad357-170">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="ad357-171">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ad357-171">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ad357-172">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="ad357-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="ad357-173">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ad357-173">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="ad357-175">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="ad357-175">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="ad357-176">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="ad357-176">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-assetbank-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ad357-178">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="ad357-178">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-assetbank-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ad357-180">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ad357-180">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-assetbank-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ad357-182">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="ad357-182">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-assetbank-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ad357-184">a.</span><span class="sxs-lookup"><span data-stu-id="ad357-184">a.</span></span> <span data-ttu-id="ad357-185">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ad357-185">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ad357-186">b.</span><span class="sxs-lookup"><span data-stu-id="ad357-186">b.</span></span> <span data-ttu-id="ad357-187">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ad357-187">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ad357-188">c.</span><span class="sxs-lookup"><span data-stu-id="ad357-188">c.</span></span> <span data-ttu-id="ad357-189">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="ad357-189">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="ad357-190">d.</span><span class="sxs-lookup"><span data-stu-id="ad357-190">d.</span></span> <span data-ttu-id="ad357-191">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="ad357-191">Click **Create**.</span></span>
 
### <a name="creating-an-asset-bank-test-user"></a><span data-ttu-id="ad357-192">Skapa en tillgång Bank testanvändare</span><span class="sxs-lookup"><span data-stu-id="ad357-192">Creating an Asset Bank test user</span></span>

<span data-ttu-id="ad357-193">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i tillgångsinformation Bank.</span><span class="sxs-lookup"><span data-stu-id="ad357-193">hello objective of this section is toocreate a user called Britta Simon in Asset Bank.</span></span> <span data-ttu-id="ad357-194">Tillgångsinformation banken stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="ad357-194">Asset Bank supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="ad357-195">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="ad357-195">There is no action item for you in this section.</span></span> <span data-ttu-id="ad357-196">En ny användare skapas under ett försök tooaccess tillgången Bank om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="ad357-196">A new user is created during an attempt tooaccess Asset Bank if it doesn't exist yet.</span></span> 

>[!NOTE]
><span data-ttu-id="ad357-197">Om du behöver toocreate en användare manuellt, måste toocontact hello [tillgången Bank supportteamet](mailto:support@assetbank.co.uk).</span><span class="sxs-lookup"><span data-stu-id="ad357-197">If you need toocreate a user manually, you need toocontact hello [Asset Bank support team](mailto:support@assetbank.co.uk).</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="ad357-198">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="ad357-198">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="ad357-199">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooAsset Bank.</span><span class="sxs-lookup"><span data-stu-id="ad357-199">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAsset Bank.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="ad357-201">**tooassign Britta Simon tooAsset Bank, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="ad357-201">**tooassign Britta Simon tooAsset Bank, perform hello following steps:**</span></span>

1. <span data-ttu-id="ad357-202">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="ad357-202">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="ad357-204">Välj i listan med program hello **tillgången Bank**.</span><span class="sxs-lookup"><span data-stu-id="ad357-204">In hello applications list, select **Asset Bank**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-assetbank-tutorial/tutorial_assetbank_app.png) 

3. <span data-ttu-id="ad357-206">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="ad357-206">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="ad357-208">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="ad357-208">Click **Add** button.</span></span> <span data-ttu-id="ad357-209">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ad357-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="ad357-211">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="ad357-211">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="ad357-212">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ad357-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ad357-213">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ad357-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ad357-214">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ad357-214">Testing single sign-on</span></span>

<span data-ttu-id="ad357-215">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="ad357-215">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="ad357-216">Du bör få automatiskt inloggade tooyour tillgången Bank programmet när du klickar på hello tillgången Bank panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="ad357-216">When you click hello Asset Bank tile in hello Access Panel, you should get automatically signed-on tooyour Asset Bank application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="ad357-217">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="ad357-217">Additional resources</span></span>

* [<span data-ttu-id="ad357-218">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ad357-218">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ad357-219">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ad357-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_203.png

