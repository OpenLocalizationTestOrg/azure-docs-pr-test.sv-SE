---
title: "Självstudier: Azure Active Directory-integrering med Workfront | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Workfront."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: aab8bd2f-f9dd-42da-a18e-d707865687d7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: e7249b9ec769f19cf5aa7d44ff6f58705df4020a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workfront"></a><span data-ttu-id="70c9a-103">Självstudier: Azure Active Directory-integrering med Workfront</span><span class="sxs-lookup"><span data-stu-id="70c9a-103">Tutorial: Azure Active Directory integration with Workfront</span></span>

<span data-ttu-id="70c9a-104">I kursen får du lära dig hur toointegrate Workfront med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="70c9a-104">In this tutorial, you learn how toointegrate Workfront with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="70c9a-105">Integrera Workfront med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="70c9a-105">Integrating Workfront with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="70c9a-106">Du kan styra i Azure AD som har åtkomst till tooWorkfront</span><span class="sxs-lookup"><span data-stu-id="70c9a-106">You can control in Azure AD who has access tooWorkfront</span></span>
- <span data-ttu-id="70c9a-107">Du kan aktivera din användare tooautomatically get inloggade tooWorkfront (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="70c9a-107">You can enable your users tooautomatically get signed-on tooWorkfront (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="70c9a-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="70c9a-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="70c9a-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="70c9a-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="70c9a-110">Krav</span><span class="sxs-lookup"><span data-stu-id="70c9a-110">Prerequisites</span></span>

<span data-ttu-id="70c9a-111">tooconfigure Azure AD-integrering med Workfront, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="70c9a-111">tooconfigure Azure AD integration with Workfront, you need hello following items:</span></span>

- <span data-ttu-id="70c9a-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="70c9a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="70c9a-113">En Workfront enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="70c9a-113">A Workfront single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="70c9a-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="70c9a-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="70c9a-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="70c9a-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="70c9a-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="70c9a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="70c9a-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="70c9a-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="70c9a-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="70c9a-118">Scenario description</span></span>
<span data-ttu-id="70c9a-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="70c9a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="70c9a-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="70c9a-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="70c9a-121">Att lägga till Workfront från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="70c9a-121">Adding Workfront from hello gallery</span></span>
2. <span data-ttu-id="70c9a-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="70c9a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-workfront-from-hello-gallery"></a><span data-ttu-id="70c9a-123">Att lägga till Workfront från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="70c9a-123">Adding Workfront from hello gallery</span></span>
<span data-ttu-id="70c9a-124">tooconfigure hello integrering av Workfront i Azure AD, behöver du tooadd Workfront hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="70c9a-124">tooconfigure hello integration of Workfront into Azure AD, you need tooadd Workfront from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="70c9a-125">**tooadd Workfront från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="70c9a-125">**tooadd Workfront from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="70c9a-126">I hello ** [Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="70c9a-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="70c9a-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="70c9a-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="70c9a-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="70c9a-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="70c9a-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="70c9a-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="70c9a-133">Skriv i sökrutan hello **Workfront**.</span><span class="sxs-lookup"><span data-stu-id="70c9a-133">In hello search box, type **Workfront**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workfront-tutorial/tutorial_workfront_search.png)

5. <span data-ttu-id="70c9a-135">Markera hello resultat på panelen **Workfront**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="70c9a-135">In hello results panel, select **Workfront**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workfront-tutorial/tutorial_workfront_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="70c9a-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="70c9a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="70c9a-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Workfront baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="70c9a-138">In this section, you configure and test Azure AD single sign-on with Workfront based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="70c9a-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Workfront är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="70c9a-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Workfront is tooa user in Azure AD.</span></span> <span data-ttu-id="70c9a-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Workfront toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="70c9a-140">In other words, a link relationship between an Azure AD user and hello related user in Workfront needs toobe established.</span></span>

<span data-ttu-id="70c9a-141">I Workfront, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="70c9a-141">In Workfront, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="70c9a-142">tooconfigure och testa Azure AD enkel inloggning med Workfront, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="70c9a-142">tooconfigure and test Azure AD single sign-on with Workfront, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="70c9a-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on) ** -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="70c9a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="70c9a-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user) ** -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="70c9a-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="70c9a-145">**[Skapa en testanvändare Workfront](#creating-a-workfront-test-user) ** -toohave en motsvarighet för Britta Simon i Workfront som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="70c9a-145">**[Creating a Workfront test user](#creating-a-workfront-test-user)** - toohave a counterpart of Britta Simon in Workfront that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="70c9a-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user) ** -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="70c9a-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="70c9a-147">**[Testa enkel inloggning](#testing-single-sign-on) ** -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="70c9a-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="70c9a-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="70c9a-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="70c9a-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Workfront program.</span><span class="sxs-lookup"><span data-stu-id="70c9a-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Workfront application.</span></span>

<span data-ttu-id="70c9a-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Workfront:**</span><span class="sxs-lookup"><span data-stu-id="70c9a-150">**tooconfigure Azure AD single sign-on with Workfront, perform hello following steps:**</span></span>

1. <span data-ttu-id="70c9a-151">I hello Azure-portalen på hello **Workfront** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="70c9a-151">In hello Azure portal, on hello **Workfront** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="70c9a-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="70c9a-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-workfront-tutorial/tutorial_workfront_samlbase.png)

3. <span data-ttu-id="70c9a-155">På hello **Workfront domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="70c9a-155">On hello **Workfront Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workfront-tutorial/tutorial_workfront_url.png)

    <span data-ttu-id="70c9a-157">a.</span><span class="sxs-lookup"><span data-stu-id="70c9a-157">a.</span></span> <span data-ttu-id="70c9a-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.attask-ondemand.com`</span><span class="sxs-lookup"><span data-stu-id="70c9a-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.attask-ondemand.com`</span></span>

    <span data-ttu-id="70c9a-159">b.</span><span class="sxs-lookup"><span data-stu-id="70c9a-159">b.</span></span> <span data-ttu-id="70c9a-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.attasksandbox.com/SAML2`</span><span class="sxs-lookup"><span data-stu-id="70c9a-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.attasksandbox.com/SAML2`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="70c9a-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="70c9a-161">These values are not real.</span></span> <span data-ttu-id="70c9a-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="70c9a-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="70c9a-163">Kontakta [Workfront klienten supportteamet](https://www.workfront.com/contact-us/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="70c9a-163">Contact [Workfront Client support team](https://www.workfront.com/contact-us/) tooget these values.</span></span> 
 
4. <span data-ttu-id="70c9a-164">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="70c9a-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello Certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workfront-tutorial/tutorial_workfront_certificate.png) 

5. <span data-ttu-id="70c9a-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="70c9a-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workfront-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="70c9a-168">På hello **Workfront Configuration** klickar du på **konfigurera Workfront** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="70c9a-168">On hello **Workfront Configuration** section, click **Configure Workfront** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="70c9a-169">Kopiera hello **Sign-Out URL och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="70c9a-169">Copy hello **Sign-Out URL, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workfront-tutorial/tutorial_workfront_configure.png) 

7. <span data-ttu-id="70c9a-171">Inloggning tooyour Workfront företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="70c9a-171">Sign-on tooyour Workfront company site as administrator.</span></span>

8. <span data-ttu-id="70c9a-172">Gå för**enkel inloggning på konfigurationen**.</span><span class="sxs-lookup"><span data-stu-id="70c9a-172">Go too**Single Sign On Configuration**.</span></span>

9. <span data-ttu-id="70c9a-173">På hello **enkel inloggning** dialogrutan, utför följande steg hello</span><span class="sxs-lookup"><span data-stu-id="70c9a-173">On hello **Single Sign-On** dialog, perform hello following steps</span></span>
    
    ![Konfigurera enkel inloggning][23]
   
    <span data-ttu-id="70c9a-175">a.</span><span class="sxs-lookup"><span data-stu-id="70c9a-175">a.</span></span> <span data-ttu-id="70c9a-176">Som **typen**väljer **SAML 2.0**.</span><span class="sxs-lookup"><span data-stu-id="70c9a-176">As **Type**, select **SAML 2.0**.</span></span>
   
    <span data-ttu-id="70c9a-177">b.</span><span class="sxs-lookup"><span data-stu-id="70c9a-177">b.</span></span> <span data-ttu-id="70c9a-178">Välj **Service Provider-ID**.</span><span class="sxs-lookup"><span data-stu-id="70c9a-178">Select **Service Provider ID**.</span></span>
   
    <span data-ttu-id="70c9a-179">c.</span><span class="sxs-lookup"><span data-stu-id="70c9a-179">c.</span></span> <span data-ttu-id="70c9a-180">Klistra in hello **SAML enkel inloggning Tjänstwebbadress** till hello **Portal Inloggningswebbadressen** textruta.</span><span class="sxs-lookup"><span data-stu-id="70c9a-180">Paste hello **SAML Single Sign-On Service URL** into hello **Login Portal URL** textbox.</span></span>
   
    <span data-ttu-id="70c9a-181">d.</span><span class="sxs-lookup"><span data-stu-id="70c9a-181">d.</span></span> <span data-ttu-id="70c9a-182">Klistra in **tjänst-URL för enkel Sign-Out** till hello **Sign-Out URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="70c9a-182">Paste **Single Sign-Out Service URL** into hello **Sign-Out URL** textbox.</span></span>
   
    <span data-ttu-id="70c9a-183">e.</span><span class="sxs-lookup"><span data-stu-id="70c9a-183">e.</span></span> <span data-ttu-id="70c9a-184">Klistra in **ändra lösenord URL** till hello **ändra lösenord URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="70c9a-184">Paste **Change Password URL** into hello **Change Password URL** textbox.</span></span>
   
    <span data-ttu-id="70c9a-185">f.</span><span class="sxs-lookup"><span data-stu-id="70c9a-185">f.</span></span> <span data-ttu-id="70c9a-186">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="70c9a-186">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="70c9a-187">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="70c9a-187">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="70c9a-188">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello ** Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="70c9a-188">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="70c9a-189">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="70c9a-189">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="70c9a-190">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="70c9a-190">Creating an Azure AD test user</span></span>
<span data-ttu-id="70c9a-191">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="70c9a-191">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="70c9a-193">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="70c9a-193">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="70c9a-194">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="70c9a-194">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workfront-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="70c9a-196">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="70c9a-196">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workfront-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="70c9a-198">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="70c9a-198">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workfront-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="70c9a-200">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="70c9a-200">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workfront-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="70c9a-202">a.</span><span class="sxs-lookup"><span data-stu-id="70c9a-202">a.</span></span> <span data-ttu-id="70c9a-203">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="70c9a-203">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="70c9a-204">b.</span><span class="sxs-lookup"><span data-stu-id="70c9a-204">b.</span></span> <span data-ttu-id="70c9a-205">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="70c9a-205">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="70c9a-206">c.</span><span class="sxs-lookup"><span data-stu-id="70c9a-206">c.</span></span> <span data-ttu-id="70c9a-207">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="70c9a-207">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="70c9a-208">d.</span><span class="sxs-lookup"><span data-stu-id="70c9a-208">d.</span></span> <span data-ttu-id="70c9a-209">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="70c9a-209">Click **Create**.</span></span>
 
### <a name="creating-a-workfront-test-user"></a><span data-ttu-id="70c9a-210">Skapa en testanvändare Workfront</span><span class="sxs-lookup"><span data-stu-id="70c9a-210">Creating a Workfront test user</span></span>

<span data-ttu-id="70c9a-211">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i Workfront.</span><span class="sxs-lookup"><span data-stu-id="70c9a-211">hello objective of this section is toocreate a user called Britta Simon in Workfront.</span></span>

<span data-ttu-id="70c9a-212">**toocreate en användare som kallas Britta Simon i Workfront, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="70c9a-212">**toocreate a user called Britta Simon in Workfront, perform hello following steps:**</span></span>

1. <span data-ttu-id="70c9a-213">Inloggning tooyour Workfront företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="70c9a-213">Sign on tooyour Workfront company site as administrator.</span></span>
2. <span data-ttu-id="70c9a-214">Hello-menyn överst hello **personer**.</span><span class="sxs-lookup"><span data-stu-id="70c9a-214">In hello menu on hello top, click **People**.</span></span>
3. <span data-ttu-id="70c9a-215">Klicka på **ny Person**.</span><span class="sxs-lookup"><span data-stu-id="70c9a-215">Click **New Person**.</span></span> 
4. <span data-ttu-id="70c9a-216">Utför följande hello på hello ny Person dialogrutan:</span><span class="sxs-lookup"><span data-stu-id="70c9a-216">On hello New Person dialog, perform hello following steps:</span></span>
   
    ![Skapa en testanvändare Workfront][21] 
   
    <span data-ttu-id="70c9a-218">a.</span><span class="sxs-lookup"><span data-stu-id="70c9a-218">a.</span></span> <span data-ttu-id="70c9a-219">I hello **Förnamn** textruta skriver ”Britta”.</span><span class="sxs-lookup"><span data-stu-id="70c9a-219">In hello **First Name** textbox, type "Britta."</span></span>
   
    <span data-ttu-id="70c9a-220">b.</span><span class="sxs-lookup"><span data-stu-id="70c9a-220">b.</span></span> <span data-ttu-id="70c9a-221">I hello **efternamn** textruta skriver ”Simon”.</span><span class="sxs-lookup"><span data-stu-id="70c9a-221">In hello **Last Name** textbox, type "Simon."</span></span>
   
    <span data-ttu-id="70c9a-222">c.</span><span class="sxs-lookup"><span data-stu-id="70c9a-222">c.</span></span> <span data-ttu-id="70c9a-223">I hello **e-postadress** textruta Skriv Britta Simon e-postadress i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="70c9a-223">In hello **Email Address** textbox, type Britta Simon's email address in Azure Active Directory.</span></span>
   
    <span data-ttu-id="70c9a-224">d.</span><span class="sxs-lookup"><span data-stu-id="70c9a-224">d.</span></span> <span data-ttu-id="70c9a-225">Klicka på **lägga till personen**.</span><span class="sxs-lookup"><span data-stu-id="70c9a-225">Click **Add Person**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="70c9a-226">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="70c9a-226">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="70c9a-227">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooWorkfront.</span><span class="sxs-lookup"><span data-stu-id="70c9a-227">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooWorkfront.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="70c9a-229">**tooassign Britta Simon tooWorkfront utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="70c9a-229">**tooassign Britta Simon tooWorkfront, perform hello following steps:**</span></span>

1. <span data-ttu-id="70c9a-230">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="70c9a-230">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="70c9a-232">Välj i listan med program hello **Workfront**.</span><span class="sxs-lookup"><span data-stu-id="70c9a-232">In hello applications list, select **Workfront**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workfront-tutorial/tutorial_workfront_app.png) 

3. <span data-ttu-id="70c9a-234">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="70c9a-234">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="70c9a-236">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="70c9a-236">Click **Add** button.</span></span> <span data-ttu-id="70c9a-237">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="70c9a-237">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="70c9a-239">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="70c9a-239">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="70c9a-240">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="70c9a-240">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="70c9a-241">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="70c9a-241">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="70c9a-242">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="70c9a-242">Testing single sign-on</span></span>

<span data-ttu-id="70c9a-243">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="70c9a-243">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="70c9a-244">När du klickar på hello Workfront panelen i hello åtkomstpanelen bör du hämta inloggningssidan för Workfront program.</span><span class="sxs-lookup"><span data-stu-id="70c9a-244">When you click hello Workfront tile in hello Access Panel, you should get login page of Workfront application.</span></span>
<span data-ttu-id="70c9a-245">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="70c9a-245">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="70c9a-246">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="70c9a-246">Additional resources</span></span>

* [<span data-ttu-id="70c9a-247">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="70c9a-247">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="70c9a-248">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="70c9a-248">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_04.png
[21]:./media/active-directory-saas-workfront-tutorial/tutorial_attask_08.png
[23]:./media/active-directory-saas-workfront-tutorial/tutorial_attask_06.png
[100]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workfront-tutorial/tutorial_general_203.png

