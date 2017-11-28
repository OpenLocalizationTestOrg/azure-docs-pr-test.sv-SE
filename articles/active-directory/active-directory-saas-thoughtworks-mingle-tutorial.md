---
title: "Självstudier: Azure Active Directory-integrering med Thoughtworks Mingle | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Thoughtworks Mingle."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 69d859d9-b7f7-4c42-bc8c-8036138be586
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: c17f8e13d2db3de7d228d9b27128d134f98d6cdf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-thoughtworks-mingle"></a><span data-ttu-id="52129-103">Självstudier: Azure Active Directory-integrering med Thoughtworks Mingle</span><span class="sxs-lookup"><span data-stu-id="52129-103">Tutorial: Azure Active Directory integration with Thoughtworks Mingle</span></span>

<span data-ttu-id="52129-104">I kursen får du lära dig hur toointegrate Thoughtworks Mingle med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="52129-104">In this tutorial, you learn how toointegrate Thoughtworks Mingle with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="52129-105">Integrera Thoughtworks Mingle med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="52129-105">Integrating Thoughtworks Mingle with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="52129-106">Du kan styra i Azure AD som har åtkomst tooThoughtworks Mingle</span><span class="sxs-lookup"><span data-stu-id="52129-106">You can control in Azure AD who has access tooThoughtworks Mingle</span></span>
- <span data-ttu-id="52129-107">Du kan aktivera din användare tooautomatically get inloggade tooThoughtworks Mingle (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="52129-107">You can enable your users tooautomatically get signed-on tooThoughtworks Mingle (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="52129-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="52129-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="52129-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="52129-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="52129-110">Krav</span><span class="sxs-lookup"><span data-stu-id="52129-110">Prerequisites</span></span>

<span data-ttu-id="52129-111">tooconfigure Azure AD-integrering med Thoughtworks Mingle, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="52129-111">tooconfigure Azure AD integration with Thoughtworks Mingle, you need hello following items:</span></span>

- <span data-ttu-id="52129-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="52129-112">An Azure AD subscription</span></span>
- <span data-ttu-id="52129-113">En Thoughtworks Mingle enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="52129-113">A Thoughtworks Mingle single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="52129-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="52129-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="52129-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="52129-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="52129-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="52129-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="52129-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="52129-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="52129-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="52129-118">Scenario description</span></span>
<span data-ttu-id="52129-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="52129-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="52129-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="52129-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="52129-121">Att lägga till Thoughtworks Mingle från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="52129-121">Adding Thoughtworks Mingle from hello gallery</span></span>
2. <span data-ttu-id="52129-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="52129-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-thoughtworks-mingle-from-hello-gallery"></a><span data-ttu-id="52129-123">Att lägga till Thoughtworks Mingle från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="52129-123">Adding Thoughtworks Mingle from hello gallery</span></span>
<span data-ttu-id="52129-124">tooconfigure hello integrering av Thoughtworks Mingle i Azure AD, behöver du tooadd Thoughtworks Mingle hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="52129-124">tooconfigure hello integration of Thoughtworks Mingle into Azure AD, you need tooadd Thoughtworks Mingle from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="52129-125">**tooadd Thoughtworks Mingle från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="52129-125">**tooadd Thoughtworks Mingle from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="52129-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="52129-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="52129-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="52129-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="52129-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="52129-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="52129-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="52129-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="52129-133">Skriv i sökrutan hello **Thoughtworks Mingle**väljer **Thoughtworks Mingle** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="52129-133">In hello search box, type **Thoughtworks Mingle**, select **Thoughtworks Mingle** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Thoughtworks Mingle i hello resultatlistan](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="52129-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="52129-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="52129-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Thoughtworks Mingle baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="52129-136">In this section, you configure and test Azure AD single sign-on with Thoughtworks Mingle based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="52129-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Thoughtworks Mingle är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="52129-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Thoughtworks Mingle is tooa user in Azure AD.</span></span> <span data-ttu-id="52129-138">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Thoughtworks Mingle toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="52129-138">In other words, a link relationship between an Azure AD user and hello related user in Thoughtworks Mingle needs toobe established.</span></span>

<span data-ttu-id="52129-139">Tilldela hello värdet för hello i Thoughtworks Mingle **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="52129-139">In Thoughtworks Mingle, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="52129-140">tooconfigure och testa Azure AD enkel inloggning med Thoughtworks Mingle, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="52129-140">tooconfigure and test Azure AD single sign-on with Thoughtworks Mingle, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="52129-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="52129-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="52129-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="52129-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="52129-143">**[Skapa en testanvändare Thoughtworks Mingle](#create-a-thoughtworks-mingle-test-user)**  -toohave en motsvarighet för Britta Simon i Thoughtworks Mingle som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="52129-143">**[Create a Thoughtworks Mingle test user](#create-a-thoughtworks-mingle-test-user)** - toohave a counterpart of Britta Simon in Thoughtworks Mingle that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="52129-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="52129-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="52129-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="52129-145">**[Test Single Sign-On](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="52129-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="52129-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="52129-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Thoughtworks Mingle program.</span><span class="sxs-lookup"><span data-stu-id="52129-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Thoughtworks Mingle application.</span></span>

<span data-ttu-id="52129-148">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Thoughtworks Mingle:**</span><span class="sxs-lookup"><span data-stu-id="52129-148">**tooconfigure Azure AD single sign-on with Thoughtworks Mingle, perform hello following steps:**</span></span>

1. <span data-ttu-id="52129-149">I hello Azure-portalen på hello **Thoughtworks Mingle** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="52129-149">In hello Azure portal, on hello **Thoughtworks Mingle** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="52129-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="52129-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_samlbase.png)

3. <span data-ttu-id="52129-153">På hello **Thoughtworks Mingle domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="52129-153">On hello **Thoughtworks Mingle Domain and URLs** section, perform hello following steps:</span></span>

    ![URL: er och Thoughtworks Mingle domän med enkel inloggning information](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_url.png)

    <span data-ttu-id="52129-155">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.mingle.thoughtworks.com`</span><span class="sxs-lookup"><span data-stu-id="52129-155">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.mingle.thoughtworks.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="52129-156">hello-värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="52129-156">hello value is not real.</span></span> <span data-ttu-id="52129-157">Hello uppdateringsvärde med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="52129-157">Update hello value with hello actual Sign-On URL.</span></span> <span data-ttu-id="52129-158">Kontakta [Thoughtworks Mingle klienten supportteamet](https://support.thoughtworks.com/hc/categories/201743486-Mingle-Community-Support) tooget hello värde.</span><span class="sxs-lookup"><span data-stu-id="52129-158">Contact [Thoughtworks Mingle Client support team](https://support.thoughtworks.com/hc/categories/201743486-Mingle-Community-Support) tooget hello value.</span></span> 
 
4. <span data-ttu-id="52129-159">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="52129-159">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_certificate.png) 

5. <span data-ttu-id="52129-161">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="52129-161">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="52129-163">Logga in tooyour **Thoughtworks Mingle** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="52129-163">Log in tooyour **Thoughtworks Mingle** company site as administrator.</span></span>

7. <span data-ttu-id="52129-164">Klicka på hello **Admin** fliken och klicka sedan på **SSO Config**.</span><span class="sxs-lookup"><span data-stu-id="52129-164">Click hello **Admin** tab, and then, click **SSO Config**.</span></span>
   
    <span data-ttu-id="52129-165">![Fliken Administration](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785157.png "SSO Config")</span><span class="sxs-lookup"><span data-stu-id="52129-165">![Admin tab](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785157.png "SSO Config")</span></span>

8. <span data-ttu-id="52129-166">I hello **SSO Config** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="52129-166">In hello **SSO Config** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="52129-167">![SSO Config](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785158.png "SSO Config")</span><span class="sxs-lookup"><span data-stu-id="52129-167">![SSO Config](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785158.png "SSO Config")</span></span>
    
    <span data-ttu-id="52129-168">a.</span><span class="sxs-lookup"><span data-stu-id="52129-168">a.</span></span> <span data-ttu-id="52129-169">metadatafil för tooupload hello klickar du på **Välj fil**.</span><span class="sxs-lookup"><span data-stu-id="52129-169">tooupload hello metadata file, click **Choose file**.</span></span> 

    <span data-ttu-id="52129-170">b.</span><span class="sxs-lookup"><span data-stu-id="52129-170">b.</span></span> <span data-ttu-id="52129-171">Klicka på **spara ändringar**.</span><span class="sxs-lookup"><span data-stu-id="52129-171">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="52129-172">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="52129-172">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="52129-173">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="52129-173">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="52129-174">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="52129-174">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="52129-175">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="52129-175">Create an Azure AD test user</span></span>
<span data-ttu-id="52129-176">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="52129-176">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="52129-178">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="52129-178">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="52129-179">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="52129-179">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-thoughtworks-mingle-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="52129-181">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="52129-181">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-thoughtworks-mingle-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="52129-183">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="52129-183">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![hello webbinställningar](./media/active-directory-saas-thoughtworks-mingle-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="52129-185">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="52129-185">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![hello användardialogrutan](./media/active-directory-saas-thoughtworks-mingle-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="52129-187">a.</span><span class="sxs-lookup"><span data-stu-id="52129-187">a.</span></span> <span data-ttu-id="52129-188">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="52129-188">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="52129-189">b.</span><span class="sxs-lookup"><span data-stu-id="52129-189">b.</span></span> <span data-ttu-id="52129-190">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="52129-190">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="52129-191">c.</span><span class="sxs-lookup"><span data-stu-id="52129-191">c.</span></span> <span data-ttu-id="52129-192">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="52129-192">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="52129-193">d.</span><span class="sxs-lookup"><span data-stu-id="52129-193">d.</span></span> <span data-ttu-id="52129-194">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="52129-194">Click **Create**.</span></span>
 
### <a name="create-a-thoughtworks-mingle-test-user"></a><span data-ttu-id="52129-195">Skapa en Thoughtworks Mingle testanvändare</span><span class="sxs-lookup"><span data-stu-id="52129-195">Create a Thoughtworks Mingle test user</span></span>

<span data-ttu-id="52129-196">Azure AD-användare toobe kan toosign i, måste de vara etablerade toohello Thoughtworks Mingle program med hjälp av deras användarnamn för Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="52129-196">For Azure AD users toobe able toosign in, they must be provisioned toohello Thoughtworks Mingle application using their Azure Active Directory user names.</span></span> <span data-ttu-id="52129-197">Hello gäller Thoughtworks Mingle är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="52129-197">In hello case of Thoughtworks Mingle, provisioning is a manual task.</span></span>

<span data-ttu-id="52129-198">**tooconfigure användaretablering, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="52129-198">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="52129-199">Logga in tooyour Thoughtworks Mingle företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="52129-199">Log in tooyour Thoughtworks Mingle company site as administrator.</span></span>

2. <span data-ttu-id="52129-200">Klicka på **profil**.</span><span class="sxs-lookup"><span data-stu-id="52129-200">Click **Profile**.</span></span>
   
    <span data-ttu-id="52129-201">![Ditt första projekt](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785160.png "ditt första projekt")</span><span class="sxs-lookup"><span data-stu-id="52129-201">![Your First Project](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785160.png "Your First Project")</span></span>

3. <span data-ttu-id="52129-202">Klicka på hello **Admin** fliken och klicka sedan på **användare**.</span><span class="sxs-lookup"><span data-stu-id="52129-202">Click hello **Admin** tab, and then click **Users**.</span></span>
   
    <span data-ttu-id="52129-203">![Användare](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785161.png "användare")</span><span class="sxs-lookup"><span data-stu-id="52129-203">![Users](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785161.png "Users")</span></span>

4. <span data-ttu-id="52129-204">Klicka på **ny användare**.</span><span class="sxs-lookup"><span data-stu-id="52129-204">Click **New User**.</span></span>
   
    <span data-ttu-id="52129-205">![Ny användare](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785162.png "ny användare")</span><span class="sxs-lookup"><span data-stu-id="52129-205">![New User](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785162.png "New User")</span></span>

5. <span data-ttu-id="52129-206">På hello **ny användare** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="52129-206">On hello **New User** dialog page, perform hello following steps:</span></span>
   
    <span data-ttu-id="52129-207">![Dialogrutan Ny användare](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785163.png "ny användare")</span><span class="sxs-lookup"><span data-stu-id="52129-207">![New User dialog](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785163.png "New User")</span></span>  
 
    <span data-ttu-id="52129-208">a.</span><span class="sxs-lookup"><span data-stu-id="52129-208">a.</span></span> <span data-ttu-id="52129-209">Typen hello **inloggningsnamn**, **visningsnamn**, **Välj lösenord**, **Bekräfta lösenord** av ett giltigt Azure AD-kontot som du vill tooprovision relaterade textrutor till hello.</span><span class="sxs-lookup"><span data-stu-id="52129-209">Type hello **Sign-in name**, **Display name**, **Choose password**, **Confirm password** of a valid Azure AD account you want tooprovision into hello related textboxes.</span></span> 

    <span data-ttu-id="52129-210">b.</span><span class="sxs-lookup"><span data-stu-id="52129-210">b.</span></span> <span data-ttu-id="52129-211">Som **användartyp**väljer **fullständig användaren**.</span><span class="sxs-lookup"><span data-stu-id="52129-211">As **User type**, select **Full user**.</span></span>

    <span data-ttu-id="52129-212">c.</span><span class="sxs-lookup"><span data-stu-id="52129-212">c.</span></span> <span data-ttu-id="52129-213">Klicka på **skapa den här profilen**.</span><span class="sxs-lookup"><span data-stu-id="52129-213">Click **Create This Profile**.</span></span>

>[!NOTE]
><span data-ttu-id="52129-214">Du kan använda något annat Thoughtworks Mingle användarens konto skapas verktyg eller API: er som tillhandahålls av Thoughtworks Mingle tooprovision AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="52129-214">You can use any other Thoughtworks Mingle user account creation tools or APIs provided by Thoughtworks Mingle tooprovision AAD user accounts.</span></span>
> 

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="52129-215">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="52129-215">Assign hello Azure AD test user</span></span>

<span data-ttu-id="52129-216">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooThoughtworks Mingle.</span><span class="sxs-lookup"><span data-stu-id="52129-216">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooThoughtworks Mingle.</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="52129-218">**tooassign Britta Simon tooThoughtworks Mingle, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="52129-218">**tooassign Britta Simon tooThoughtworks Mingle, perform hello following steps:**</span></span>

1. <span data-ttu-id="52129-219">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="52129-219">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="52129-221">Välj i listan med program hello **Thoughtworks Mingle**.</span><span class="sxs-lookup"><span data-stu-id="52129-221">In hello applications list, select **Thoughtworks Mingle**.</span></span>

    ![Hej Thoughtworks Mingle länken i listan med program hello](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_app.png) 

3. <span data-ttu-id="52129-223">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="52129-223">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202] 

4. <span data-ttu-id="52129-225">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="52129-225">Click **Add** button.</span></span> <span data-ttu-id="52129-226">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="52129-226">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="52129-228">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="52129-228">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="52129-229">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="52129-229">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="52129-230">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="52129-230">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="52129-231">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="52129-231">Test single sign-on</span></span>

<span data-ttu-id="52129-232">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="52129-232">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="52129-233">Du bör få automatiskt inloggade tooyour Thoughtworks Mingle programmet när du klickar på hello Thoughtworks Mingle panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="52129-233">When you click hello Thoughtworks Mingle tile in hello Access Panel, you should get automatically signed-on tooyour Thoughtworks Mingle application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="52129-234">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="52129-234">Additional resources</span></span>

* [<span data-ttu-id="52129-235">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="52129-235">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="52129-236">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="52129-236">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_203.png

