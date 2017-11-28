---
title: "Självstudier: Azure Active Directory-integrering med Hightail | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Hightail."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e15206ac-74b0-46e4-9329-892c7d242ec0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2017
ms.author: jeedes
ms.openlocfilehash: 2b36fcf8d5773255fdf89de2dccdceb95c032bd8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-hightail"></a><span data-ttu-id="0ca59-103">Självstudier: Azure Active Directory-integrering med Hightail</span><span class="sxs-lookup"><span data-stu-id="0ca59-103">Tutorial: Azure Active Directory integration with Hightail</span></span>

<span data-ttu-id="0ca59-104">I kursen får du lära dig hur toointegrate Hightail med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="0ca59-104">In this tutorial, you learn how toointegrate Hightail with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="0ca59-105">Integrera Hightail med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="0ca59-105">Integrating Hightail with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="0ca59-106">Du kan styra i Azure AD som har åtkomst till tooHightail</span><span class="sxs-lookup"><span data-stu-id="0ca59-106">You can control in Azure AD who has access tooHightail</span></span>
- <span data-ttu-id="0ca59-107">Du kan aktivera din användare tooautomatically get inloggade tooHightail (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="0ca59-107">You can enable your users tooautomatically get signed-on tooHightail (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="0ca59-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="0ca59-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="0ca59-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0ca59-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0ca59-110">Krav</span><span class="sxs-lookup"><span data-stu-id="0ca59-110">Prerequisites</span></span>

<span data-ttu-id="0ca59-111">tooconfigure Azure AD-integrering med Hightail, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="0ca59-111">tooconfigure Azure AD integration with Hightail, you need hello following items:</span></span>

- <span data-ttu-id="0ca59-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="0ca59-112">An Azure AD subscription</span></span>
- <span data-ttu-id="0ca59-113">En Hightail enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="0ca59-113">A Hightail single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="0ca59-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="0ca59-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="0ca59-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="0ca59-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="0ca59-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="0ca59-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="0ca59-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0ca59-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="0ca59-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="0ca59-118">Scenario description</span></span>
<span data-ttu-id="0ca59-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="0ca59-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="0ca59-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="0ca59-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="0ca59-121">Att lägga till Hightail från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="0ca59-121">Adding Hightail from hello gallery</span></span>
2. <span data-ttu-id="0ca59-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0ca59-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-hightail-from-hello-gallery"></a><span data-ttu-id="0ca59-123">Att lägga till Hightail från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="0ca59-123">Adding Hightail from hello gallery</span></span>
<span data-ttu-id="0ca59-124">tooconfigure hello integrering av Hightail i Azure AD, behöver du tooadd Hightail hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="0ca59-124">tooconfigure hello integration of Hightail into Azure AD, you need tooadd Hightail from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="0ca59-125">**tooadd Hightail från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="0ca59-125">**tooadd Hightail from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="0ca59-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="0ca59-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="0ca59-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="0ca59-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="0ca59-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="0ca59-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="0ca59-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0ca59-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="0ca59-133">Skriv i sökrutan hello **Hightail**.</span><span class="sxs-lookup"><span data-stu-id="0ca59-133">In hello search box, type **Hightail**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_search.png)

5. <span data-ttu-id="0ca59-135">Markera hello resultat på panelen **Hightail**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="0ca59-135">In hello results panel, select **Hightail**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="0ca59-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0ca59-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="0ca59-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Hightail baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="0ca59-138">In this section, you configure and test Azure AD single sign-on with Hightail based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="0ca59-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Hightail är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0ca59-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Hightail is tooa user in Azure AD.</span></span> <span data-ttu-id="0ca59-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Hightail toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="0ca59-140">In other words, a link relationship between an Azure AD user and hello related user in Hightail needs toobe established.</span></span>

<span data-ttu-id="0ca59-141">I Hightail, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="0ca59-141">In Hightail, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="0ca59-142">tooconfigure och testa Azure AD enkel inloggning med Hightail, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="0ca59-142">tooconfigure and test Azure AD single sign-on with Hightail, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="0ca59-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="0ca59-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="0ca59-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0ca59-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="0ca59-145">**[Skapa en testanvändare Hightail](#creating-a-hightail-test-user)**  -toohave en motsvarighet för Britta Simon i Hightail som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="0ca59-145">**[Creating a Hightail test user](#creating-a-hightail-test-user)** - toohave a counterpart of Britta Simon in Hightail that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="0ca59-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="0ca59-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="0ca59-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="0ca59-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="0ca59-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0ca59-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="0ca59-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Hightail program.</span><span class="sxs-lookup"><span data-stu-id="0ca59-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Hightail application.</span></span>

<span data-ttu-id="0ca59-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Hightail:**</span><span class="sxs-lookup"><span data-stu-id="0ca59-150">**tooconfigure Azure AD single sign-on with Hightail, perform hello following steps:**</span></span>

1. <span data-ttu-id="0ca59-151">I hello Azure-portalen på hello **Hightail** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="0ca59-151">In hello Azure portal, on hello **Hightail** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="0ca59-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="0ca59-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_samlbase.png)

3. <span data-ttu-id="0ca59-155">På hello **Hightail domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="0ca59-155">On hello **Hightail Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_url.png)

     <span data-ttu-id="0ca59-157">I hello **Reply URL** textruta typen hello URL som:`https://www.hightail.com/samlLogin?phi_action=app/samlLogin&subAction=handleSamlResponse`</span><span class="sxs-lookup"><span data-stu-id="0ca59-157">In hello **Reply URL** textbox, type hello URL as: `https://www.hightail.com/samlLogin?phi_action=app/samlLogin&subAction=handleSamlResponse`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="0ca59-158">hello är föregående värde inte verkliga värde.</span><span class="sxs-lookup"><span data-stu-id="0ca59-158">hello preceding value is not real value.</span></span> <span data-ttu-id="0ca59-159">Hello värdet uppdateras med hello faktiska Reply URL, vilket beskrivs senare i hello kursen.</span><span class="sxs-lookup"><span data-stu-id="0ca59-159">You will update hello value with hello actual Reply URL, which is explained later in hello tutorial.</span></span>
 
4. <span data-ttu-id="0ca59-160">På hello **Hightail domän och URL: er** om du vill tooconfigure hello programmet i **SP initierade läge**, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="0ca59-160">On hello **Hightail Domain and URLs** section, If you wish tooconfigure hello application in **SP initiated mode**, perform hello following steps:</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_url1.png)

    <span data-ttu-id="0ca59-162">a.</span><span class="sxs-lookup"><span data-stu-id="0ca59-162">a.</span></span> <span data-ttu-id="0ca59-163">Klicka på hello **visa avancerade inställningar för URL: en**.</span><span class="sxs-lookup"><span data-stu-id="0ca59-163">Click hello **Show advanced URL settings**.</span></span>

    <span data-ttu-id="0ca59-164">b.</span><span class="sxs-lookup"><span data-stu-id="0ca59-164">b.</span></span> <span data-ttu-id="0ca59-165">I hello **logga URL** textruta typen hello URL som:`https://www.hightail.com/loginSSO`</span><span class="sxs-lookup"><span data-stu-id="0ca59-165">In hello **Sign On URL** textbox, type hello URL as: `https://www.hightail.com/loginSSO`</span></span>

4. <span data-ttu-id="0ca59-166">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="0ca59-166">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_certificate.png) 

5. <span data-ttu-id="0ca59-168">Hightail program förväntar hello SAML intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="0ca59-168">Hightail application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="0ca59-169">Konfigurera hello följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="0ca59-169">Please configure hello following claims for this application.</span></span> <span data-ttu-id="0ca59-170">Du kan hantera hello värden för attributen från hello **”Atrribute”** fliken av programmet hello.</span><span class="sxs-lookup"><span data-stu-id="0ca59-170">You can manage hello values of these attributes from hello **"Atrribute"** tab of hello application.</span></span> <span data-ttu-id="0ca59-171">hello följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="0ca59-171">hello following screenshot shows an example for this.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_attribute.png) 

6. <span data-ttu-id="0ca59-173">I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan Konfigurera SAML-token attributet enligt hello bild och utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="0ca59-173">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image and perform hello following steps:</span></span>
    
    | <span data-ttu-id="0ca59-174">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="0ca59-174">Attribute Name</span></span> | <span data-ttu-id="0ca59-175">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="0ca59-175">Attribute Value</span></span> |
    | ------------------- | -------------------- |
    | <span data-ttu-id="0ca59-176">Förnamn</span><span class="sxs-lookup"><span data-stu-id="0ca59-176">FirstName</span></span> | <span data-ttu-id="0ca59-177">User.givenName</span><span class="sxs-lookup"><span data-stu-id="0ca59-177">user.givenname</span></span> |
    | <span data-ttu-id="0ca59-178">Efternamn</span><span class="sxs-lookup"><span data-stu-id="0ca59-178">LastName</span></span> | <span data-ttu-id="0ca59-179">User.surname</span><span class="sxs-lookup"><span data-stu-id="0ca59-179">user.surname</span></span> |
    | <span data-ttu-id="0ca59-180">E-post</span><span class="sxs-lookup"><span data-stu-id="0ca59-180">Email</span></span> | <span data-ttu-id="0ca59-181">User.Mail</span><span class="sxs-lookup"><span data-stu-id="0ca59-181">user.mail</span></span> |    
    | <span data-ttu-id="0ca59-182">Användaridentiteten</span><span class="sxs-lookup"><span data-stu-id="0ca59-182">UserIdentity</span></span> | <span data-ttu-id="0ca59-183">User.Mail</span><span class="sxs-lookup"><span data-stu-id="0ca59-183">user.mail</span></span> |
    
    <span data-ttu-id="0ca59-184">a.</span><span class="sxs-lookup"><span data-stu-id="0ca59-184">a.</span></span> <span data-ttu-id="0ca59-185">Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0ca59-185">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_officespace_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_officespace_05.png)

    <span data-ttu-id="0ca59-188">b.</span><span class="sxs-lookup"><span data-stu-id="0ca59-188">b.</span></span> <span data-ttu-id="0ca59-189">I hello **namn** textruta hello attributnamn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="0ca59-189">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="0ca59-190">c.</span><span class="sxs-lookup"><span data-stu-id="0ca59-190">c.</span></span> <span data-ttu-id="0ca59-191">Från hello **värdet** listan attributvärde för typ hello visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="0ca59-191">From hello **Value** list, type hello attribute value shown for that row.</span></span>

    <span data-ttu-id="0ca59-192">d.</span><span class="sxs-lookup"><span data-stu-id="0ca59-192">d.</span></span> <span data-ttu-id="0ca59-193">Lämna hello **Namespace** tomt.</span><span class="sxs-lookup"><span data-stu-id="0ca59-193">Leave hello **Namespace** blank.</span></span>
    
    <span data-ttu-id="0ca59-194">e.</span><span class="sxs-lookup"><span data-stu-id="0ca59-194">e.</span></span> <span data-ttu-id="0ca59-195">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="0ca59-195">Click **Ok**.</span></span>

7. <span data-ttu-id="0ca59-196">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="0ca59-196">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="0ca59-198">På hello **Hightail Configuration** klickar du på **konfigurera Hightail** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="0ca59-198">On hello **Hightail Configuration** section, click **Configure Hightail** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="0ca59-199">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="0ca59-199">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_configure.png) 

    >[!NOTE] 
    ><span data-ttu-id="0ca59-201">Innan du konfigurerar hello enkel inloggning på Hightail app kan kan du lista för tillåten din e-postdomän med Hightail team så att alla hello användare som använder den här domänen använda enkel inloggning funktionen.</span><span class="sxs-lookup"><span data-stu-id="0ca59-201">Before configuring hello Single Sign On at Hightail app, please white list your email domain with Hightail team so that all hello users who are using this domain can use Single Sign On functionality.</span></span>


9. <span data-ttu-id="0ca59-202">tooget SSO konfigurerats för ditt program måste toosign på tooyour Hightail klient som administratör.</span><span class="sxs-lookup"><span data-stu-id="0ca59-202">tooget SSO configured for your application, you need toosign-on tooyour Hightail tenant as an administrator.</span></span>
   
    <span data-ttu-id="0ca59-203">a.</span><span class="sxs-lookup"><span data-stu-id="0ca59-203">a.</span></span> <span data-ttu-id="0ca59-204">Hello hello överst klickar du på menyn hello **konto** fliken och markera **konfigurera SAML**.</span><span class="sxs-lookup"><span data-stu-id="0ca59-204">In hello menu on hello top, click hello **Account** tab and select **Configure SAML**.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_001.png) 

    <span data-ttu-id="0ca59-206">b.</span><span class="sxs-lookup"><span data-stu-id="0ca59-206">b.</span></span> <span data-ttu-id="0ca59-207">Markera kryssrutan för hello av **aktivera SAML-autentisering**.</span><span class="sxs-lookup"><span data-stu-id="0ca59-207">Select hello checkbox of **Enable SAML Authentication**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_002.png) 

    <span data-ttu-id="0ca59-209">c.</span><span class="sxs-lookup"><span data-stu-id="0ca59-209">c.</span></span> <span data-ttu-id="0ca59-210">Öppna din Base64-kodade certifikatet i anteckningar som hämtas från Azure-portalen kopiera hello innehållet i den i Urklipp, och klistra in den toohello **SAML Tokensigneringscertifikatet** textruta.</span><span class="sxs-lookup"><span data-stu-id="0ca59-210">Open your base-64 encoded certificate in notepad downloaded from Azure portal, copy hello content of it into your clipboard, and then paste it toohello **SAML Token Signing Certificate** textbox.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_003.png) 

    <span data-ttu-id="0ca59-212">d.</span><span class="sxs-lookup"><span data-stu-id="0ca59-212">d.</span></span> <span data-ttu-id="0ca59-213">I hello **SAML Authority (identitetsleverantör)** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel** kopieras från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0ca59-213">In hello **SAML Authority (Identity Provider)** textbox, paste hello value of **SAML Single Sign-On Service URL** copied from Azure portal.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_004.png)

    <span data-ttu-id="0ca59-215">e.</span><span class="sxs-lookup"><span data-stu-id="0ca59-215">e.</span></span> <span data-ttu-id="0ca59-216">Om du inte vill tooconfigure hello programmet i **IDP initierade läge** Välj **”identitetsprovider (IdP) initierade logga in”**.</span><span class="sxs-lookup"><span data-stu-id="0ca59-216">If you wish tooconfigure hello application in **IDP initiated mode** select **"Identity Provider (IdP) initiated log in"**.</span></span> <span data-ttu-id="0ca59-217">Om **SP initierade läge** Välj **”ServiceProvider (SP) initierade logga in”**.</span><span class="sxs-lookup"><span data-stu-id="0ca59-217">If **SP initiated mode** select **"Service Provider (SP) initiated log in"**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_006.png)

    <span data-ttu-id="0ca59-219">f.</span><span class="sxs-lookup"><span data-stu-id="0ca59-219">f.</span></span> <span data-ttu-id="0ca59-220">Kopiera hello SAML konsument-URL för din instans och klistra in den i **Reply URL** TextBox-kontroll i **Hightail domän och URL: er** avsnitt på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0ca59-220">Copy hello SAML consumer URL for your instance and paste it in **Reply URL** textbox in **Hightail Domain and URLs** section on Azure portal.</span></span>
    
    <span data-ttu-id="0ca59-221">g.</span><span class="sxs-lookup"><span data-stu-id="0ca59-221">g.</span></span> <span data-ttu-id="0ca59-222">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="0ca59-222">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="0ca59-223">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="0ca59-223">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="0ca59-224">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="0ca59-224">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="0ca59-225">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="0ca59-225">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="0ca59-226">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="0ca59-226">Creating an Azure AD test user</span></span>
<span data-ttu-id="0ca59-227">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0ca59-227">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="0ca59-229">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="0ca59-229">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="0ca59-230">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="0ca59-230">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hightail-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="0ca59-232">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="0ca59-232">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hightail-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="0ca59-234">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0ca59-234">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hightail-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="0ca59-236">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="0ca59-236">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hightail-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="0ca59-238">a.</span><span class="sxs-lookup"><span data-stu-id="0ca59-238">a.</span></span> <span data-ttu-id="0ca59-239">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="0ca59-239">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="0ca59-240">b.</span><span class="sxs-lookup"><span data-stu-id="0ca59-240">b.</span></span> <span data-ttu-id="0ca59-241">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="0ca59-241">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="0ca59-242">c.</span><span class="sxs-lookup"><span data-stu-id="0ca59-242">c.</span></span> <span data-ttu-id="0ca59-243">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="0ca59-243">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="0ca59-244">d.</span><span class="sxs-lookup"><span data-stu-id="0ca59-244">d.</span></span> <span data-ttu-id="0ca59-245">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="0ca59-245">Click **Create**.</span></span>
 
### <a name="creating-a-hightail-test-user"></a><span data-ttu-id="0ca59-246">Skapa en testanvändare Hightail</span><span class="sxs-lookup"><span data-stu-id="0ca59-246">Creating a Hightail test user</span></span>

<span data-ttu-id="0ca59-247">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i Hightail.</span><span class="sxs-lookup"><span data-stu-id="0ca59-247">hello objective of this section is toocreate a user called Britta Simon in Hightail.</span></span> 

<span data-ttu-id="0ca59-248">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="0ca59-248">There is no action item for you in this section.</span></span> <span data-ttu-id="0ca59-249">Hightail stöder just-in-time-användaretablering baserat på hello anpassade anspråk.</span><span class="sxs-lookup"><span data-stu-id="0ca59-249">Hightail supports just-in-time user provisioning based on hello custom claims.</span></span> <span data-ttu-id="0ca59-250">Om du har konfigurerat hello anpassade anspråk som visas i avsnittet hello  **[konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  ovan, skapas automatiskt en användare i hello-program som det inte finns.</span><span class="sxs-lookup"><span data-stu-id="0ca59-250">If you have configured hello custom claims as shown in hello section **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** above, a user is automatically created in hello application it doesn't exist yet.</span></span> 

>[!NOTE]
><span data-ttu-id="0ca59-251">Om du behöver toocreate en användare manuellt, måste toocontact hello [Hightail supportteamet](mailto:support@hightail.com).</span><span class="sxs-lookup"><span data-stu-id="0ca59-251">If you need toocreate a user manually, you need toocontact hello [Hightail support team](mailto:support@hightail.com).</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="0ca59-252">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="0ca59-252">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="0ca59-253">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooHightail.</span><span class="sxs-lookup"><span data-stu-id="0ca59-253">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooHightail.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="0ca59-255">**tooassign Britta Simon tooHightail utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="0ca59-255">**tooassign Britta Simon tooHightail, perform hello following steps:**</span></span>

1. <span data-ttu-id="0ca59-256">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="0ca59-256">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="0ca59-258">Välj i listan med program hello **Hightail**.</span><span class="sxs-lookup"><span data-stu-id="0ca59-258">In hello applications list, select **Hightail**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_app.png) 

3. <span data-ttu-id="0ca59-260">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="0ca59-260">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="0ca59-262">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="0ca59-262">Click **Add** button.</span></span> <span data-ttu-id="0ca59-263">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0ca59-263">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="0ca59-265">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="0ca59-265">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="0ca59-266">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0ca59-266">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="0ca59-267">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0ca59-267">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="0ca59-268">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0ca59-268">Testing single sign-on</span></span>

<span data-ttu-id="0ca59-269">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="0ca59-269">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="0ca59-270">Du bör få automatiskt inloggade tooyour Hightail programmet när du klickar på hello Hightail panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="0ca59-270">When you click hello Hightail tile in hello Access Panel, you should get automatically signed-on tooyour Hightail application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="0ca59-271">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="0ca59-271">Additional resources</span></span>

* [<span data-ttu-id="0ca59-272">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0ca59-272">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0ca59-273">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="0ca59-273">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_203.png

