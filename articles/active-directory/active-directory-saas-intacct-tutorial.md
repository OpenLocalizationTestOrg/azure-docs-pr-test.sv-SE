---
title: "Självstudier: Azure Active Directory-integrering med Intacct | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Intacct."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 92518e02-a62c-4b1b-a8e9-2803eb2b49ac
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 3500039615166c2f61fb408d85bb82dfaefba134
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-intacct"></a><span data-ttu-id="f63cf-103">Självstudier: Azure Active Directory-integrering med Intacct</span><span class="sxs-lookup"><span data-stu-id="f63cf-103">Tutorial: Azure Active Directory integration with Intacct</span></span>

<span data-ttu-id="f63cf-104">I kursen får du lära dig hur toointegrate Intacct med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="f63cf-104">In this tutorial, you learn how toointegrate Intacct with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f63cf-105">Integrera Intacct med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="f63cf-105">Integrating Intacct with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="f63cf-106">Du kan styra i Azure AD som har åtkomst till tooIntacct</span><span class="sxs-lookup"><span data-stu-id="f63cf-106">You can control in Azure AD who has access tooIntacct</span></span>
- <span data-ttu-id="f63cf-107">Du kan aktivera din användare tooautomatically get inloggade tooIntacct (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="f63cf-107">You can enable your users tooautomatically get signed-on tooIntacct (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f63cf-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="f63cf-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="f63cf-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f63cf-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f63cf-110">Krav</span><span class="sxs-lookup"><span data-stu-id="f63cf-110">Prerequisites</span></span>

<span data-ttu-id="f63cf-111">tooconfigure Azure AD-integrering med Intacct, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="f63cf-111">tooconfigure Azure AD integration with Intacct, you need hello following items:</span></span>

- <span data-ttu-id="f63cf-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="f63cf-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f63cf-113">En Intacct enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="f63cf-113">An Intacct single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f63cf-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="f63cf-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f63cf-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="f63cf-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f63cf-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="f63cf-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f63cf-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f63cf-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f63cf-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="f63cf-118">Scenario description</span></span>
<span data-ttu-id="f63cf-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="f63cf-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f63cf-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="f63cf-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f63cf-121">Att lägga till Intacct från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="f63cf-121">Adding Intacct from hello gallery</span></span>
2. <span data-ttu-id="f63cf-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f63cf-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-intacct-from-hello-gallery"></a><span data-ttu-id="f63cf-123">Att lägga till Intacct från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="f63cf-123">Adding Intacct from hello gallery</span></span>
<span data-ttu-id="f63cf-124">tooconfigure hello integrering av Intacct i Azure AD, behöver du tooadd Intacct hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="f63cf-124">tooconfigure hello integration of Intacct into Azure AD, you need tooadd Intacct from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="f63cf-125">**tooadd Intacct från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="f63cf-125">**tooadd Intacct from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="f63cf-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="f63cf-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f63cf-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="f63cf-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="f63cf-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="f63cf-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="f63cf-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f63cf-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="f63cf-133">Skriv i sökrutan hello **Intacct**.</span><span class="sxs-lookup"><span data-stu-id="f63cf-133">In hello search box, type **Intacct**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_search.png)

5. <span data-ttu-id="f63cf-135">Markera hello resultat på panelen **Intacct**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="f63cf-135">In hello results panel, select **Intacct**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f63cf-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f63cf-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f63cf-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Intacct baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="f63cf-138">In this section, you configure and test Azure AD single sign-on with Intacct based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="f63cf-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Intacct är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f63cf-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Intacct is tooa user in Azure AD.</span></span> <span data-ttu-id="f63cf-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Intacct toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="f63cf-140">In other words, a link relationship between an Azure AD user and hello related user in Intacct needs toobe established.</span></span>

<span data-ttu-id="f63cf-141">I Intacct, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="f63cf-141">In Intacct, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="f63cf-142">tooconfigure och testa Azure AD enkel inloggning med Intacct, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="f63cf-142">tooconfigure and test Azure AD single sign-on with Intacct, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="f63cf-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="f63cf-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="f63cf-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f63cf-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f63cf-145">**[Skapa en testanvändare Intacct](#creating-an-intacct-test-user)**  -toohave en motsvarighet för Britta Simon i Intacct som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="f63cf-145">**[Creating an Intacct test user](#creating-an-intacct-test-user)** - toohave a counterpart of Britta Simon in Intacct that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="f63cf-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="f63cf-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f63cf-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="f63cf-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f63cf-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f63cf-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f63cf-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Intacct program.</span><span class="sxs-lookup"><span data-stu-id="f63cf-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Intacct application.</span></span>

<span data-ttu-id="f63cf-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Intacct:**</span><span class="sxs-lookup"><span data-stu-id="f63cf-150">**tooconfigure Azure AD single sign-on with Intacct, perform hello following steps:**</span></span>

1. <span data-ttu-id="f63cf-151">I hello Azure-portalen på hello **Intacct** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="f63cf-151">In hello Azure portal, on hello **Intacct** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="f63cf-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="f63cf-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_samlbase.png)

3. <span data-ttu-id="f63cf-155">På hello **Intacct domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="f63cf-155">On hello **Intacct Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_url.png)

    <span data-ttu-id="f63cf-157">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:</span><span class="sxs-lookup"><span data-stu-id="f63cf-157">In hello **Reply URL** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.intacct.com/ia/acct/sso_response.phtml`|
    | `https://www.intacct.com/ia/acct/sso_response.phtml` |

    > [!NOTE] 
    > <span data-ttu-id="f63cf-158">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="f63cf-158">This value is not real.</span></span> <span data-ttu-id="f63cf-159">Uppdatera det här värdet med hello faktiska Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="f63cf-159">Update this value with hello actual Reply URL.</span></span> <span data-ttu-id="f63cf-160">Kontakta [Intacct supportteamet](https://us.intacct.com/support) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="f63cf-160">Contact [Intacct support team](https://us.intacct.com/support) tooget this value.</span></span>

4. <span data-ttu-id="f63cf-161">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="f63cf-161">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_certificate.png) 

5. <span data-ttu-id="f63cf-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="f63cf-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-intacct-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="f63cf-165">På hello **Intacct Configuration** klickar du på **konfigurera Intacct** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="f63cf-165">On hello **Intacct Configuration** section, click **Configure Intacct** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="f63cf-166">Kopiera hello **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="f63cf-166">Copy hello **SAML Entity ID and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_configure.png) 

7. <span data-ttu-id="f63cf-168">Logga in tooyour Intacct företagets webbplats som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="f63cf-168">In a different web browser window, sign in tooyour Intacct company site as an administrator.</span></span>

8. <span data-ttu-id="f63cf-169">Klicka på hello **företagets** fliken och klicka sedan på **företagets information**.</span><span class="sxs-lookup"><span data-stu-id="f63cf-169">Click hello **Company** tab, and then click **Company Info**.</span></span>

    <span data-ttu-id="f63cf-170">![Företagets](./media/active-directory-saas-intacct-tutorial/ic790037.png "företag")</span><span class="sxs-lookup"><span data-stu-id="f63cf-170">![Company](./media/active-directory-saas-intacct-tutorial/ic790037.png "Company")</span></span>

9. <span data-ttu-id="f63cf-171">Klicka på hello **säkerhet** fliken och klicka sedan på **redigera**.</span><span class="sxs-lookup"><span data-stu-id="f63cf-171">Click hello **Security** tab, and then click **Edit**.</span></span>

    <span data-ttu-id="f63cf-172">![Säkerhet](./media/active-directory-saas-intacct-tutorial/ic790038.png "säkerhet")</span><span class="sxs-lookup"><span data-stu-id="f63cf-172">![Security](./media/active-directory-saas-intacct-tutorial/ic790038.png "Security")</span></span>

10. <span data-ttu-id="f63cf-173">I hello **enkel inloggning (SSO)** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="f63cf-173">In hello **Single sign on (SSO)** section, perform hello following steps:</span></span>

    <span data-ttu-id="f63cf-174">![Enkel inloggning](./media/active-directory-saas-intacct-tutorial/ic790039.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="f63cf-174">![Single sign on](./media/active-directory-saas-intacct-tutorial/ic790039.png "single sign on")</span></span>

    <span data-ttu-id="f63cf-175">a.</span><span class="sxs-lookup"><span data-stu-id="f63cf-175">a.</span></span> <span data-ttu-id="f63cf-176">Välj **aktivera enkel inloggning på**.</span><span class="sxs-lookup"><span data-stu-id="f63cf-176">Select **Enable single sign on**.</span></span>

    <span data-ttu-id="f63cf-177">b.</span><span class="sxs-lookup"><span data-stu-id="f63cf-177">b.</span></span> <span data-ttu-id="f63cf-178">Som **identitet providertyp**väljer **SAML 2.0**.</span><span class="sxs-lookup"><span data-stu-id="f63cf-178">As **Identity provider type**, select **SAML 2.0**.</span></span>

    <span data-ttu-id="f63cf-179">c.</span><span class="sxs-lookup"><span data-stu-id="f63cf-179">c.</span></span> <span data-ttu-id="f63cf-180">I **utfärdar-URL** textruta klistra in hello värdet för **SAML enhets-ID** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f63cf-180">In **Issuer URL** textbox, paste hello value of **SAML Entity ID** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="f63cf-181">d.</span><span class="sxs-lookup"><span data-stu-id="f63cf-181">d.</span></span> <span data-ttu-id="f63cf-182">I **inloggnings-URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f63cf-182">In **Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="f63cf-183">e.</span><span class="sxs-lookup"><span data-stu-id="f63cf-183">e.</span></span> <span data-ttu-id="f63cf-184">Öppna din **Base64-** kodat certifikat i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den toohello **certifikat** rutan.</span><span class="sxs-lookup"><span data-stu-id="f63cf-184">Open your **base-64** encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it toohello **Certificate** box.</span></span>
   
    <span data-ttu-id="f63cf-185">f.</span><span class="sxs-lookup"><span data-stu-id="f63cf-185">f.</span></span> <span data-ttu-id="f63cf-186">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="f63cf-186">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="f63cf-187">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="f63cf-187">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="f63cf-188">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="f63cf-188">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="f63cf-189">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f63cf-189">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f63cf-190">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="f63cf-190">Creating an Azure AD test user</span></span>
<span data-ttu-id="f63cf-191">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f63cf-191">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="f63cf-193">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="f63cf-193">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="f63cf-194">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="f63cf-194">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-intacct-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f63cf-196">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="f63cf-196">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-intacct-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f63cf-198">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f63cf-198">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-intacct-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f63cf-200">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="f63cf-200">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-intacct-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f63cf-202">a.</span><span class="sxs-lookup"><span data-stu-id="f63cf-202">a.</span></span> <span data-ttu-id="f63cf-203">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f63cf-203">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f63cf-204">b.</span><span class="sxs-lookup"><span data-stu-id="f63cf-204">b.</span></span> <span data-ttu-id="f63cf-205">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f63cf-205">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f63cf-206">c.</span><span class="sxs-lookup"><span data-stu-id="f63cf-206">c.</span></span> <span data-ttu-id="f63cf-207">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="f63cf-207">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="f63cf-208">d.</span><span class="sxs-lookup"><span data-stu-id="f63cf-208">d.</span></span> <span data-ttu-id="f63cf-209">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="f63cf-209">Click **Create**.</span></span>
 
### <a name="creating-an-intacct-test-user"></a><span data-ttu-id="f63cf-210">Skapa en testanvändare Intacct</span><span class="sxs-lookup"><span data-stu-id="f63cf-210">Creating an Intacct test user</span></span>

<span data-ttu-id="f63cf-211">tooset konfigurerar Azure AD-användare så att de kan logga in tooIntacct, de måste etableras i Intacct.</span><span class="sxs-lookup"><span data-stu-id="f63cf-211">tooset up Azure AD users so they can sign in tooIntacct, they must be provisioned into Intacct.</span></span> <span data-ttu-id="f63cf-212">Intacct är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="f63cf-212">For Intacct, provisioning is a manual task.</span></span>

<span data-ttu-id="f63cf-213">**tooprovision användarkonton, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="f63cf-213">**tooprovision user accounts, perform hello following steps:**</span></span>

1. <span data-ttu-id="f63cf-214">Logga in tooyour **Intacct** klient.</span><span class="sxs-lookup"><span data-stu-id="f63cf-214">Sign in tooyour **Intacct** tenant.</span></span>

2. <span data-ttu-id="f63cf-215">Klicka på hello **företagets** fliken och klicka sedan på **användare**.</span><span class="sxs-lookup"><span data-stu-id="f63cf-215">Click hello **Company** tab, and then click **Users**.</span></span>

    <span data-ttu-id="f63cf-216">![Användare](./media/active-directory-saas-intacct-tutorial/ic790041.png "användare")</span><span class="sxs-lookup"><span data-stu-id="f63cf-216">![Users](./media/active-directory-saas-intacct-tutorial/ic790041.png "Users")</span></span>
3. <span data-ttu-id="f63cf-217">Klicka på hello **Lägg till** fliken.</span><span class="sxs-lookup"><span data-stu-id="f63cf-217">Click hello **Add** tab.</span></span>

    <span data-ttu-id="f63cf-218">![Lägg till](./media/active-directory-saas-intacct-tutorial/ic790042.png "Lägg till")</span><span class="sxs-lookup"><span data-stu-id="f63cf-218">![Add](./media/active-directory-saas-intacct-tutorial/ic790042.png "Add")</span></span>
4. <span data-ttu-id="f63cf-219">I hello **användarinformation** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="f63cf-219">In hello **User Information** section, perform hello following steps:</span></span>

    <span data-ttu-id="f63cf-220">![Användarinformation](./media/active-directory-saas-intacct-tutorial/ic790043.png "användarinformation")</span><span class="sxs-lookup"><span data-stu-id="f63cf-220">![User Information](./media/active-directory-saas-intacct-tutorial/ic790043.png "User Information")</span></span>

    <span data-ttu-id="f63cf-221">a.</span><span class="sxs-lookup"><span data-stu-id="f63cf-221">a.</span></span> <span data-ttu-id="f63cf-222">Ange hello **användar-ID**, hello **efternamn**, **Förnamn**, hello **e-postadress**, hello **rubrik**, och hello **Phone** för ett Azure AD-konto som du vill tooprovision till hello **användarinformation** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="f63cf-222">Enter hello **User ID**, hello **Last name**, **First name**, hello **Email address**, hello **Title**, and hello **Phone** of an Azure AD account that you want tooprovision into hello **User Information** section.</span></span>

    <span data-ttu-id="f63cf-223">b.</span><span class="sxs-lookup"><span data-stu-id="f63cf-223">b.</span></span> <span data-ttu-id="f63cf-224">Välj hello **administratörsrättigheter** för ett Azure AD-konto som du vill tooprovision.</span><span class="sxs-lookup"><span data-stu-id="f63cf-224">Select hello **Admin privileges** of an Azure AD account that you want tooprovision.</span></span>
   
    <span data-ttu-id="f63cf-225">c.</span><span class="sxs-lookup"><span data-stu-id="f63cf-225">c.</span></span> <span data-ttu-id="f63cf-226">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="f63cf-226">Click **Save**.</span></span> <span data-ttu-id="f63cf-227">hello Azure AD-kontot innehavaren får ett e-postmeddelande och följer en länk tooconfirm sitt konto innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="f63cf-227">hello Azure AD account holder receives an email and follows a link tooconfirm their account before it becomes active.</span></span>

>[!NOTE]
><span data-ttu-id="f63cf-228">tooprovision användarkonton i Azure AD, som du kan använda andra Intacct användare skapa verktyg och API: er som tillhandahålls av Intacct.</span><span class="sxs-lookup"><span data-stu-id="f63cf-228">tooprovision Azure AD user accounts, you can use other Intacct user account creation tools or APIs that are provided by Intacct.</span></span>
        
### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="f63cf-229">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="f63cf-229">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="f63cf-230">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooIntacct.</span><span class="sxs-lookup"><span data-stu-id="f63cf-230">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooIntacct.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="f63cf-232">**tooassign Britta Simon tooIntacct utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="f63cf-232">**tooassign Britta Simon tooIntacct, perform hello following steps:**</span></span>

1. <span data-ttu-id="f63cf-233">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="f63cf-233">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="f63cf-235">Välj i listan med program hello **Intacct**.</span><span class="sxs-lookup"><span data-stu-id="f63cf-235">In hello applications list, select **Intacct**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_app.png) 

3. <span data-ttu-id="f63cf-237">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="f63cf-237">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="f63cf-239">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="f63cf-239">Click **Add** button.</span></span> <span data-ttu-id="f63cf-240">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f63cf-240">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="f63cf-242">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="f63cf-242">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="f63cf-243">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f63cf-243">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f63cf-244">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f63cf-244">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f63cf-245">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f63cf-245">Testing single sign-on</span></span>

<span data-ttu-id="f63cf-246">I det här avsnittet testa du Azure AD enkel inloggning konfigurationen med hjälp av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="f63cf-246">In this section, you test your Azure AD single sign-on configuration by using hello Access Panel.</span></span>

<span data-ttu-id="f63cf-247">När du klickar på hello Intacct panelen i hello åtkomstpanelen bör vara loggas du automatiskt tooyour Intacct program.</span><span class="sxs-lookup"><span data-stu-id="f63cf-247">When you click hello Intacct tile in hello Access Panel, you should be automatically signed in tooyour Intacct application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f63cf-248">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="f63cf-248">Additional resources</span></span>

* [<span data-ttu-id="f63cf-249">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f63cf-249">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f63cf-250">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="f63cf-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_203.png

