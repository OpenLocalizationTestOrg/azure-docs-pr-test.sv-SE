---
title: "Självstudier: Azure Active Directory-integrering med nämligen | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och nämligen."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9541d5c4-4c82-4b5b-b01a-6a3f75a2b7a1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: b0477ca6fa52a21abea7de458f8a99a01e8c25c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-namely"></a><span data-ttu-id="6aac1-103">Självstudier: Azure Active Directory-integrering med nämligen</span><span class="sxs-lookup"><span data-stu-id="6aac1-103">Tutorial: Azure Active Directory integration with Namely</span></span>

<span data-ttu-id="6aac1-104">I kursen får du lära dig hur toointegrate nämligen med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="6aac1-104">In this tutorial, you learn how toointegrate Namely with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6aac1-105">Integrera nämligen med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="6aac1-105">Integrating Namely with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="6aac1-106">Du kan styra i Azure AD som har åtkomst till tooNamely</span><span class="sxs-lookup"><span data-stu-id="6aac1-106">You can control in Azure AD who has access tooNamely</span></span>
- <span data-ttu-id="6aac1-107">Du kan aktivera din användare tooautomatically get inloggade tooNamely (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="6aac1-107">You can enable your users tooautomatically get signed-on tooNamely (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="6aac1-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="6aac1-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="6aac1-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6aac1-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6aac1-110">Krav</span><span class="sxs-lookup"><span data-stu-id="6aac1-110">Prerequisites</span></span>

<span data-ttu-id="6aac1-111">tooconfigure Azure AD-integrering med nämligen du behöver hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="6aac1-111">tooconfigure Azure AD integration with Namely, you need hello following items:</span></span>

- <span data-ttu-id="6aac1-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="6aac1-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6aac1-113">En nämligen enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="6aac1-113">A Namely single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6aac1-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="6aac1-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6aac1-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="6aac1-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6aac1-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="6aac1-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6aac1-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6aac1-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6aac1-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="6aac1-118">Scenario description</span></span>
<span data-ttu-id="6aac1-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="6aac1-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6aac1-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="6aac1-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6aac1-121">Att lägga till nämligen från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="6aac1-121">Adding Namely from hello gallery</span></span>
2. <span data-ttu-id="6aac1-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6aac1-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-namely-from-hello-gallery"></a><span data-ttu-id="6aac1-123">Att lägga till nämligen från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="6aac1-123">Adding Namely from hello gallery</span></span>
<span data-ttu-id="6aac1-124">tooconfigure hello integrering av nämligen i Azure AD, behöver du tooadd nämligen från hello galleriet tooyour lista över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="6aac1-124">tooconfigure hello integration of Namely into Azure AD, you need tooadd Namely from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="6aac1-125">**tooadd nämligen från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="6aac1-125">**tooadd Namely from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="6aac1-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="6aac1-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="6aac1-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="6aac1-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="6aac1-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="6aac1-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="6aac1-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6aac1-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="6aac1-133">Skriv i sökrutan hello **nämligen**.</span><span class="sxs-lookup"><span data-stu-id="6aac1-133">In hello search box, type **Namely**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-namely-tutorial/tutorial_namely_search.png)

5. <span data-ttu-id="6aac1-135">Markera hello resultat på panelen **nämligen**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="6aac1-135">In hello results panel, select **Namely**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-namely-tutorial/tutorial_namely_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="6aac1-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6aac1-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="6aac1-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med nämligen baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="6aac1-138">In this section, you configure and test Azure AD single sign-on with Namely based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="6aac1-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i nämligen är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6aac1-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Namely is tooa user in Azure AD.</span></span> <span data-ttu-id="6aac1-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i nämligen toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="6aac1-140">In other words, a link relationship between an Azure AD user and hello related user in Namely needs toobe established.</span></span>

<span data-ttu-id="6aac1-141">I det kan tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="6aac1-141">In Namely, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="6aac1-142">tooconfigure och testa Azure AD enkel inloggning med nämligen du behöver toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="6aac1-142">tooconfigure and test Azure AD single sign-on with Namely, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="6aac1-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="6aac1-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="6aac1-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6aac1-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6aac1-145">**[Skapa en testanvändare nämligen](#creating-a-namely-test-user)**  -toohave en motsvarighet för Britta Simon i nämligen som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="6aac1-145">**[Creating a Namely test user](#creating-a-namely-test-user)** - toohave a counterpart of Britta Simon in Namely that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="6aac1-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6aac1-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6aac1-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="6aac1-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="6aac1-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6aac1-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="6aac1-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i din nämligen program.</span><span class="sxs-lookup"><span data-stu-id="6aac1-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Namely application.</span></span>

<span data-ttu-id="6aac1-150">**tooconfigure Azure AD enkel inloggning med nämligen utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="6aac1-150">**tooconfigure Azure AD single sign-on with Namely, perform hello following steps:**</span></span>

1. <span data-ttu-id="6aac1-151">I hello Azure-portalen på hello **nämligen** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="6aac1-151">In hello Azure portal, on hello **Namely** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="6aac1-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6aac1-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-namely-tutorial/tutorial_namely_samlbase.png)

3. <span data-ttu-id="6aac1-155">På hello **nämligen domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="6aac1-155">On hello **Namely Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-namely-tutorial/tutorial_namely_url.png)

    <span data-ttu-id="6aac1-157">a.</span><span class="sxs-lookup"><span data-stu-id="6aac1-157">a.</span></span> <span data-ttu-id="6aac1-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.namely.com`</span><span class="sxs-lookup"><span data-stu-id="6aac1-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.namely.com`</span></span>

    <span data-ttu-id="6aac1-159">b.</span><span class="sxs-lookup"><span data-stu-id="6aac1-159">b.</span></span> <span data-ttu-id="6aac1-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.namely.com/saml/metadata`</span><span class="sxs-lookup"><span data-stu-id="6aac1-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.namely.com/saml/metadata`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="6aac1-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="6aac1-161">These values are not real.</span></span> <span data-ttu-id="6aac1-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="6aac1-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="6aac1-163">Kontakta [nämligen klienten supportteamet](https://www.namely.com/contact/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="6aac1-163">Contact [Namely Client support team](https://www.namely.com/contact/) tooget these values.</span></span> 
 
4. <span data-ttu-id="6aac1-164">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="6aac1-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-namely-tutorial/tutorial_namely_certificate.png) 

5. <span data-ttu-id="6aac1-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="6aac1-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-namely-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="6aac1-168">På hello **nämligen Configuration** klickar du på **konfigurera nämligen** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="6aac1-168">On hello **Namely Configuration** section, click **Configure Namely** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="6aac1-169">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="6aac1-169">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-namely-tutorial/tutorial_namely_configure.png) 

7. <span data-ttu-id="6aac1-171">I ett nytt webbläsarfönster inloggning tooyour nämligen företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="6aac1-171">In another browser window, sign on tooyour Namely company site as an administrator.</span></span>

8. <span data-ttu-id="6aac1-172">Klicka i hello verktygsfältet hello längst upp **företagets**.</span><span class="sxs-lookup"><span data-stu-id="6aac1-172">In hello toolbar on hello top, click **Company**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-namely-tutorial/tutorial_namely_06.png) 

9. <span data-ttu-id="6aac1-174">Klicka på hello **inställningar** fliken.</span><span class="sxs-lookup"><span data-stu-id="6aac1-174">Click hello **Settings** tab.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-namely-tutorial/tutorial_namely_07.png) 

10. <span data-ttu-id="6aac1-176">Klicka på **SAML**.</span><span class="sxs-lookup"><span data-stu-id="6aac1-176">Click **SAML**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-namely-tutorial/tutorial_namely_08.png) 

11. <span data-ttu-id="6aac1-178">På hello **SAML inställningar** utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="6aac1-178">On hello **SAML Settings** page, perform hello following steps:</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-namely-tutorial/tutorial_namely_09.png)
 
    <span data-ttu-id="6aac1-180">a.</span><span class="sxs-lookup"><span data-stu-id="6aac1-180">a.</span></span> <span data-ttu-id="6aac1-181">Klicka på **aktivera SAML**.</span><span class="sxs-lookup"><span data-stu-id="6aac1-181">Click **Enable SAML**.</span></span> 

    <span data-ttu-id="6aac1-182">b.</span><span class="sxs-lookup"><span data-stu-id="6aac1-182">b.</span></span> <span data-ttu-id="6aac1-183">I hello **identitet providern SSO url** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6aac1-183">In hello **Identity provider SSO url** textbox,  paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="6aac1-184">c.</span><span class="sxs-lookup"><span data-stu-id="6aac1-184">c.</span></span> <span data-ttu-id="6aac1-185">Öppna din hämtat certifikat i anteckningar, kopiera hello innehåll, och klistra in den i hello **providern identitetscertifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="6aac1-185">Open your downloaded certificate in Notepad, copy hello content, and then paste it into hello **Identity provider certificate** textbox.</span></span>
     
    <span data-ttu-id="6aac1-186">d.</span><span class="sxs-lookup"><span data-stu-id="6aac1-186">d.</span></span> <span data-ttu-id="6aac1-187">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="6aac1-187">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="6aac1-188">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="6aac1-188">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="6aac1-189">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="6aac1-189">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="6aac1-190">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="6aac1-190">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="6aac1-191">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="6aac1-191">Creating an Azure AD test user</span></span>
<span data-ttu-id="6aac1-192">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6aac1-192">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="6aac1-194">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="6aac1-194">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="6aac1-195">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="6aac1-195">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-namely-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="6aac1-197">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="6aac1-197">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-namely-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="6aac1-199">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6aac1-199">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-namely-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="6aac1-201">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="6aac1-201">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-namely-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="6aac1-203">a.</span><span class="sxs-lookup"><span data-stu-id="6aac1-203">a.</span></span> <span data-ttu-id="6aac1-204">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6aac1-204">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6aac1-205">b.</span><span class="sxs-lookup"><span data-stu-id="6aac1-205">b.</span></span> <span data-ttu-id="6aac1-206">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="6aac1-206">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="6aac1-207">c.</span><span class="sxs-lookup"><span data-stu-id="6aac1-207">c.</span></span> <span data-ttu-id="6aac1-208">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="6aac1-208">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="6aac1-209">d.</span><span class="sxs-lookup"><span data-stu-id="6aac1-209">d.</span></span> <span data-ttu-id="6aac1-210">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="6aac1-210">Click **Create**.</span></span>
 
### <a name="creating-a-namely-test-user"></a><span data-ttu-id="6aac1-211">Skapa en nämligen testanvändare</span><span class="sxs-lookup"><span data-stu-id="6aac1-211">Creating a Namely test user</span></span>

<span data-ttu-id="6aac1-212">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i nämligen.</span><span class="sxs-lookup"><span data-stu-id="6aac1-212">hello objective of this section is toocreate a user called Britta Simon in Namely.</span></span>

<span data-ttu-id="6aac1-213">**toocreate en användare som kallas Britta Simon i nämligen utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="6aac1-213">**toocreate a user called Britta Simon in Namely, perform hello following steps:**</span></span>

1. <span data-ttu-id="6aac1-214">Inloggning tooyour företagets nämligen platsen som en administratör.</span><span class="sxs-lookup"><span data-stu-id="6aac1-214">Sign-on tooyour Namely company site as an administrator.</span></span>

2. <span data-ttu-id="6aac1-215">Klicka i hello verktygsfältet hello längst upp **personer**.</span><span class="sxs-lookup"><span data-stu-id="6aac1-215">In hello toolbar on hello top, click **People**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-namely-tutorial/tutorial_namely_10.png) 

3. <span data-ttu-id="6aac1-217">Klicka på hello **Directory** fliken.</span><span class="sxs-lookup"><span data-stu-id="6aac1-217">Click hello **Directory** tab.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-namely-tutorial/tutorial_namely_11.png) 

4. <span data-ttu-id="6aac1-219">Klicka på **Lägg till ny Person**.</span><span class="sxs-lookup"><span data-stu-id="6aac1-219">Click **Add New Person**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-namely-tutorial/tutorial_namely_12.png)

5. <span data-ttu-id="6aac1-221">På hello **Lägg till ny Person** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="6aac1-221">On hello **Add New Person** dialog, perform hello following steps:</span></span>

    <span data-ttu-id="6aac1-222">a.</span><span class="sxs-lookup"><span data-stu-id="6aac1-222">a.</span></span> <span data-ttu-id="6aac1-223">I hello **Förnamn** textruta typen **Britta**.</span><span class="sxs-lookup"><span data-stu-id="6aac1-223">In hello **First name** textbox, type **Britta**.</span></span>

    <span data-ttu-id="6aac1-224">b.</span><span class="sxs-lookup"><span data-stu-id="6aac1-224">b.</span></span> <span data-ttu-id="6aac1-225">I hello **efternamn** textruta typen **Simon**.</span><span class="sxs-lookup"><span data-stu-id="6aac1-225">In hello **Last name** textbox, type **Simon**.</span></span>

    <span data-ttu-id="6aac1-226">c.</span><span class="sxs-lookup"><span data-stu-id="6aac1-226">c.</span></span> <span data-ttu-id="6aac1-227">I hello **e-post** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="6aac1-227">In hello **Email** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="6aac1-228">d.</span><span class="sxs-lookup"><span data-stu-id="6aac1-228">d.</span></span> <span data-ttu-id="6aac1-229">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="6aac1-229">Click **Save**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="6aac1-230">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="6aac1-230">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="6aac1-231">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooNamely.</span><span class="sxs-lookup"><span data-stu-id="6aac1-231">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooNamely.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="6aac1-233">**tooassign Britta Simon tooNamely utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="6aac1-233">**tooassign Britta Simon tooNamely, perform hello following steps:**</span></span>

1. <span data-ttu-id="6aac1-234">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="6aac1-234">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="6aac1-236">Välj i listan med program hello **nämligen**.</span><span class="sxs-lookup"><span data-stu-id="6aac1-236">In hello applications list, select **Namely**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-namely-tutorial/tutorial_namely_app.png) 

3. <span data-ttu-id="6aac1-238">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="6aac1-238">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="6aac1-240">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="6aac1-240">Click **Add** button.</span></span> <span data-ttu-id="6aac1-241">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6aac1-241">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="6aac1-243">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="6aac1-243">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="6aac1-244">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6aac1-244">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6aac1-245">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6aac1-245">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="6aac1-246">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6aac1-246">Testing single sign-on</span></span>

<span data-ttu-id="6aac1-247">hello syftet med det här avsnittet är tootest din Azure AD SSO konfiguration av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="6aac1-247">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="6aac1-248">När du klickar på hello nämligen panelen i hello åtkomstpanelen, du får automatiskt inloggade tooyour nämligen program</span><span class="sxs-lookup"><span data-stu-id="6aac1-248">When you click hello Namely tile in hello Access Panel, you should get automatically signed-on tooyour Namely application</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6aac1-249">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="6aac1-249">Additional resources</span></span>

* [<span data-ttu-id="6aac1-250">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6aac1-250">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6aac1-251">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="6aac1-251">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-namely-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-namely-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-namely-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-namely-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-namely-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-namely-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-namely-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-namely-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-namely-tutorial/tutorial_general_203.png

