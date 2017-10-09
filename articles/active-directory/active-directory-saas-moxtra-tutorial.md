---
title: "Självstudier: Azure Active Directory-integrering med Moxtra | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Moxtra."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2aed2d4b-1dcd-4839-8fed-9419d107c61c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: 82e2fcc390ba508e86a3992ec1c81d0a0ffed96b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-moxtra"></a><span data-ttu-id="1065d-103">Självstudier: Azure Active Directory-integrering med Moxtra</span><span class="sxs-lookup"><span data-stu-id="1065d-103">Tutorial: Azure Active Directory integration with Moxtra</span></span>

<span data-ttu-id="1065d-104">I kursen får du lära dig hur toointegrate Moxtra med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="1065d-104">In this tutorial, you learn how toointegrate Moxtra with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="1065d-105">Integrera Moxtra med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="1065d-105">Integrating Moxtra with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="1065d-106">Du kan styra i Azure AD som har åtkomst till tooMoxtra</span><span class="sxs-lookup"><span data-stu-id="1065d-106">You can control in Azure AD who has access tooMoxtra</span></span>
- <span data-ttu-id="1065d-107">Du kan aktivera din användare tooautomatically get inloggade tooMoxtra (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="1065d-107">You can enable your users tooautomatically get signed-on tooMoxtra (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="1065d-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="1065d-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="1065d-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="1065d-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1065d-110">Krav</span><span class="sxs-lookup"><span data-stu-id="1065d-110">Prerequisites</span></span>

<span data-ttu-id="1065d-111">tooconfigure Azure AD-integrering med Moxtra, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="1065d-111">tooconfigure Azure AD integration with Moxtra, you need hello following items:</span></span>

- <span data-ttu-id="1065d-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="1065d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="1065d-113">En Moxtra enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="1065d-113">A Moxtra single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="1065d-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="1065d-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="1065d-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="1065d-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="1065d-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="1065d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="1065d-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1065d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="1065d-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="1065d-118">Scenario description</span></span>
<span data-ttu-id="1065d-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="1065d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="1065d-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="1065d-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="1065d-121">Att lägga till Moxtra från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="1065d-121">Adding Moxtra from hello gallery</span></span>
2. <span data-ttu-id="1065d-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1065d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-moxtra-from-hello-gallery"></a><span data-ttu-id="1065d-123">Att lägga till Moxtra från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="1065d-123">Adding Moxtra from hello gallery</span></span>
<span data-ttu-id="1065d-124">tooconfigure hello integrering av Moxtra i Azure AD, behöver du tooadd Moxtra hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="1065d-124">tooconfigure hello integration of Moxtra into Azure AD, you need tooadd Moxtra from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="1065d-125">**tooadd Moxtra från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="1065d-125">**tooadd Moxtra from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="1065d-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="1065d-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="1065d-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="1065d-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="1065d-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="1065d-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="1065d-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1065d-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="1065d-133">Skriv i sökrutan hello **Moxtra**.</span><span class="sxs-lookup"><span data-stu-id="1065d-133">In hello search box, type **Moxtra**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_search.png)

5. <span data-ttu-id="1065d-135">Markera hello resultat på panelen **Moxtra**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="1065d-135">In hello results panel, select **Moxtra**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="1065d-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1065d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="1065d-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Moxtra baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="1065d-138">In this section, you configure and test Azure AD single sign-on with Moxtra based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="1065d-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Moxtra är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1065d-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Moxtra is tooa user in Azure AD.</span></span> <span data-ttu-id="1065d-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Moxtra toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="1065d-140">In other words, a link relationship between an Azure AD user and hello related user in Moxtra needs toobe established.</span></span>

<span data-ttu-id="1065d-141">I Moxtra, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="1065d-141">In Moxtra, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="1065d-142">tooconfigure och testa Azure AD enkel inloggning med Moxtra, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="1065d-142">tooconfigure and test Azure AD single sign-on with Moxtra, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="1065d-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="1065d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="1065d-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1065d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="1065d-145">**[Skapa en testanvändare Moxtra](#creating-a-moxtra-test-user)**  -toohave en motsvarighet för Britta Simon i Moxtra som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="1065d-145">**[Creating a Moxtra test user](#creating-a-moxtra-test-user)** - toohave a counterpart of Britta Simon in Moxtra that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="1065d-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="1065d-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="1065d-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="1065d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="1065d-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1065d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="1065d-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Moxtra program.</span><span class="sxs-lookup"><span data-stu-id="1065d-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Moxtra application.</span></span>

<span data-ttu-id="1065d-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Moxtra:**</span><span class="sxs-lookup"><span data-stu-id="1065d-150">**tooconfigure Azure AD single sign-on with Moxtra, perform hello following steps:**</span></span>

1. <span data-ttu-id="1065d-151">I hello Azure-portalen på hello **Moxtra** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="1065d-151">In hello Azure portal, on hello **Moxtra** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="1065d-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="1065d-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_samlbase.png)

3. <span data-ttu-id="1065d-155">På hello **Moxtra domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="1065d-155">On hello **Moxtra Domain and URLs** section, perform hello following step:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_url.png)

    <span data-ttu-id="1065d-157">I hello **inloggnings-URL** textruta Skriv en URL som:`https://www.moxtra.com/service/#login`</span><span class="sxs-lookup"><span data-stu-id="1065d-157">In hello **Sign-on URL** textbox, type a URL as: `https://www.moxtra.com/service/#login`</span></span>

4. <span data-ttu-id="1065d-158">Moxtra program förväntar hello SAML intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="1065d-158">Moxtra application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="1065d-159">Konfigurera hello följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="1065d-159">Configure hello following claims for this application.</span></span> <span data-ttu-id="1065d-160">Du kan hantera hello värden för attributen från hello ”**användarattribut**” avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="1065d-160">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="1065d-161">hello följande skärmbild visar ett exempel på den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="1065d-161">hello following screenshot shows an example for this configuration.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_attributes.png)
    
5. <span data-ttu-id="1065d-163">I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan Konfigurera SAML-token attributet enligt hello bild och utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="1065d-163">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image and perform hello following steps:</span></span>
    
    | <span data-ttu-id="1065d-164">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="1065d-164">Attribute Name</span></span> | <span data-ttu-id="1065d-165">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="1065d-165">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="1065d-166">Förnamn</span><span class="sxs-lookup"><span data-stu-id="1065d-166">firstname</span></span> | <span data-ttu-id="1065d-167">User.givenName</span><span class="sxs-lookup"><span data-stu-id="1065d-167">user.givenname</span></span> |
    | <span data-ttu-id="1065d-168">Efternamn</span><span class="sxs-lookup"><span data-stu-id="1065d-168">lastname</span></span> | <span data-ttu-id="1065d-169">User.surname</span><span class="sxs-lookup"><span data-stu-id="1065d-169">user.surname</span></span> |
    | <span data-ttu-id="1065d-170">idpid</span><span class="sxs-lookup"><span data-stu-id="1065d-170">idpid</span></span>    | <span data-ttu-id="1065d-171">< SAML enhets-ID ></span><span class="sxs-lookup"><span data-stu-id="1065d-171">< SAML Entity ID ></span></span> 

    > [!Note]
    > <span data-ttu-id="1065d-172">Hej värdet för **idpid** attributet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="1065d-172">hello value of **idpid** attribute is not real.</span></span> <span data-ttu-id="1065d-173">Du kan hämta hello faktiskt värde från **Snabbreferens** avsnittet **Moxtra Configuration**.</span><span class="sxs-lookup"><span data-stu-id="1065d-173">You can get hello actual value from **Quick reference** section under **Moxtra Configuration**.</span></span>
    
    <span data-ttu-id="1065d-174">a.</span><span class="sxs-lookup"><span data-stu-id="1065d-174">a.</span></span> <span data-ttu-id="1065d-175">Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1065d-175">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-moxtra-tutorial/tutorial_attribute_04.png)

    <span data-ttu-id="1065d-177">b.</span><span class="sxs-lookup"><span data-stu-id="1065d-177">b.</span></span> <span data-ttu-id="1065d-178">I hello **namn** textruta hello attributnamn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="1065d-178">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-moxtra-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="1065d-180">c.</span><span class="sxs-lookup"><span data-stu-id="1065d-180">c.</span></span> <span data-ttu-id="1065d-181">Från hello **värdet** listan attributvärde för typ hello visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="1065d-181">From hello **Value** list, type hello attribute value shown for that row.</span></span>

    <span data-ttu-id="1065d-182">d.</span><span class="sxs-lookup"><span data-stu-id="1065d-182">d.</span></span> <span data-ttu-id="1065d-183">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="1065d-183">Click **Ok**.</span></span>
    
5. <span data-ttu-id="1065d-184">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="1065d-184">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_certificate.png) 

6. <span data-ttu-id="1065d-186">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="1065d-186">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-moxtra-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="1065d-188">På hello **Moxtra Configuration** klickar du på **konfigurera Moxtra** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="1065d-188">On hello **Moxtra Configuration** section, click **Configure Moxtra** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="1065d-189">Kopiera hello **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="1065d-189">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_configure.png) 

8. <span data-ttu-id="1065d-191">Logga in tooyour Moxtra företagets webbplats som en administratör i ett nytt webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="1065d-191">In another browser window, sign on tooyour Moxtra company site as an administrator.</span></span>

9. <span data-ttu-id="1065d-192">I verktygsfältet hello hello vänster, klickar du på **administratörskonsolen > SAML enkel inloggning**, och klicka sedan på **ny**.</span><span class="sxs-lookup"><span data-stu-id="1065d-192">In hello toolbar on hello left, click **Admin Console > SAML Single Sign-on**, and then click **New**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_06.png) 

10. <span data-ttu-id="1065d-194">På hello **SAML** utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="1065d-194">On hello **SAML** page, perform hello following steps:</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_08.png)   
 
    <span data-ttu-id="1065d-196">a.</span><span class="sxs-lookup"><span data-stu-id="1065d-196">a.</span></span> <span data-ttu-id="1065d-197">I hello **namn** textruta, ange ett namn för din konfiguration (t.ex.: *SAML*).</span><span class="sxs-lookup"><span data-stu-id="1065d-197">In hello **Name** textbox, type a name for your configuration (e.g.: *SAML*).</span></span> 
  
    <span data-ttu-id="1065d-198">b.</span><span class="sxs-lookup"><span data-stu-id="1065d-198">b.</span></span> <span data-ttu-id="1065d-199">I hello **IdP enhets-ID** textruta klistra in hello värdet för **SAML enhets-ID** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1065d-199">In hello **IdP Entity ID** textbox, paste hello value of **SAML Entity ID** which you have copied from Azure portal.</span></span> 
 
    <span data-ttu-id="1065d-200">c.</span><span class="sxs-lookup"><span data-stu-id="1065d-200">c.</span></span> <span data-ttu-id="1065d-201">I **inloggnings-URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1065d-201">In **Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span> 
 
    <span data-ttu-id="1065d-202">d.</span><span class="sxs-lookup"><span data-stu-id="1065d-202">d.</span></span> <span data-ttu-id="1065d-203">I hello **AuthnContextClassRef** textruta typen **urn: oasis: namn: tc: SAML:2.0:ac:classes:Password**.</span><span class="sxs-lookup"><span data-stu-id="1065d-203">In hello **AuthnContextClassRef** textbox, type **urn:oasis:names:tc:SAML:2.0:ac:classes:Password**.</span></span> 
 
    <span data-ttu-id="1065d-204">e.</span><span class="sxs-lookup"><span data-stu-id="1065d-204">e.</span></span> <span data-ttu-id="1065d-205">I hello **NameID Format** textruta typen **urn: oasis: namn: tc: SAML:1.1:nameid-format: e-postadress**.</span><span class="sxs-lookup"><span data-stu-id="1065d-205">In hello **NameID Format** textbox, type **urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress**.</span></span> 
 
    <span data-ttu-id="1065d-206">f.</span><span class="sxs-lookup"><span data-stu-id="1065d-206">f.</span></span> <span data-ttu-id="1065d-207">Öppna certifikat som du har hämtat från Azure-portalen i anteckningar, kopiera hello innehållet och klistra in den i hello **certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="1065d-207">Open certificate which you have downloaded from Azure portal in notepad, copy hello content, and then paste it into hello **Certificate** textbox.</span></span>    
 
    <span data-ttu-id="1065d-208">g.</span><span class="sxs-lookup"><span data-stu-id="1065d-208">g.</span></span> <span data-ttu-id="1065d-209">Skriv din e-postdomän SAML i hello SAML e-domän textruta.</span><span class="sxs-lookup"><span data-stu-id="1065d-209">In hello SAML email domain textbox, type your SAML email domain.</span></span>    
  
    >[!NOTE]
    ><span data-ttu-id="1065d-210">toosee hello steg tooverify hello domän, klickar du på hello ”**jag**” nedan.</span><span class="sxs-lookup"><span data-stu-id="1065d-210">toosee hello steps tooverify hello domain, click hello "**i**" below.</span></span>

    <span data-ttu-id="1065d-211">h.</span><span class="sxs-lookup"><span data-stu-id="1065d-211">h.</span></span> <span data-ttu-id="1065d-212">Klicka på **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="1065d-212">Click **Update**.</span></span>

> [!TIP]
> <span data-ttu-id="1065d-213">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="1065d-213">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="1065d-214">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="1065d-214">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="1065d-215">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="1065d-215">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="1065d-216">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="1065d-216">Creating an Azure AD test user</span></span>
<span data-ttu-id="1065d-217">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1065d-217">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="1065d-219">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="1065d-219">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="1065d-220">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="1065d-220">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-moxtra-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="1065d-222">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="1065d-222">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-moxtra-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="1065d-224">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1065d-224">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-moxtra-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="1065d-226">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="1065d-226">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-moxtra-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="1065d-228">a.</span><span class="sxs-lookup"><span data-stu-id="1065d-228">a.</span></span> <span data-ttu-id="1065d-229">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="1065d-229">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="1065d-230">b.</span><span class="sxs-lookup"><span data-stu-id="1065d-230">b.</span></span> <span data-ttu-id="1065d-231">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="1065d-231">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="1065d-232">c.</span><span class="sxs-lookup"><span data-stu-id="1065d-232">c.</span></span> <span data-ttu-id="1065d-233">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="1065d-233">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="1065d-234">d.</span><span class="sxs-lookup"><span data-stu-id="1065d-234">d.</span></span> <span data-ttu-id="1065d-235">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="1065d-235">Click **Create**.</span></span>
 
### <a name="creating-a-moxtra-test-user"></a><span data-ttu-id="1065d-236">Skapa en testanvändare Moxtra</span><span class="sxs-lookup"><span data-stu-id="1065d-236">Creating a Moxtra test user</span></span>

<span data-ttu-id="1065d-237">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i Moxtra.</span><span class="sxs-lookup"><span data-stu-id="1065d-237">hello objective of this section is toocreate a user called Britta Simon in Moxtra.</span></span>

<span data-ttu-id="1065d-238">**toocreate en användare som kallas Britta Simon i Moxtra, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="1065d-238">**toocreate a user called Britta Simon in Moxtra, perform hello following steps:**</span></span>

1. <span data-ttu-id="1065d-239">Inloggning tooyour Moxtra företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="1065d-239">Sign on tooyour Moxtra company site as an administrator.</span></span>

2. <span data-ttu-id="1065d-240">I verktygsfältet hello hello vänster, klickar du på **administratörskonsolen > Användarhantering**, och sedan **Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="1065d-240">In hello toolbar on hello left, click **Admin Console > User Management**, and then **Add User**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_10.png) 

3. <span data-ttu-id="1065d-242">På hello **Lägg till användare** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="1065d-242">On hello **Add User** dialog, perform hello following steps:</span></span>
  
    <span data-ttu-id="1065d-243">a.</span><span class="sxs-lookup"><span data-stu-id="1065d-243">a.</span></span> <span data-ttu-id="1065d-244">I hello **Förnamn** textruta typen **Britta**.</span><span class="sxs-lookup"><span data-stu-id="1065d-244">In hello **First Name** textbox, type **Britta**.</span></span>
  
    <span data-ttu-id="1065d-245">b.</span><span class="sxs-lookup"><span data-stu-id="1065d-245">b.</span></span> <span data-ttu-id="1065d-246">I hello **efternamn** textruta typen **Simon**.</span><span class="sxs-lookup"><span data-stu-id="1065d-246">In hello **Last Name** textbox, type **Simon**.</span></span>
  
    <span data-ttu-id="1065d-247">c.</span><span class="sxs-lookup"><span data-stu-id="1065d-247">c.</span></span> <span data-ttu-id="1065d-248">I hello **e-post** textruta typen Brittas e-postadressen samma som på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1065d-248">In hello **Email** textbox, type Britta's email address same as on Azure portal.</span></span>
  
    <span data-ttu-id="1065d-249">d.</span><span class="sxs-lookup"><span data-stu-id="1065d-249">d.</span></span> <span data-ttu-id="1065d-250">I hello **Division** textruta typen **Dev**.</span><span class="sxs-lookup"><span data-stu-id="1065d-250">In hello **Division** textbox, type **Dev**.</span></span>
  
    <span data-ttu-id="1065d-251">e.</span><span class="sxs-lookup"><span data-stu-id="1065d-251">e.</span></span> <span data-ttu-id="1065d-252">I hello **avdelning** textruta typen **IT**.</span><span class="sxs-lookup"><span data-stu-id="1065d-252">In hello **Department** textbox, type **IT**.</span></span>
  
    <span data-ttu-id="1065d-253">f.</span><span class="sxs-lookup"><span data-stu-id="1065d-253">f.</span></span> <span data-ttu-id="1065d-254">Välj **administratör**.</span><span class="sxs-lookup"><span data-stu-id="1065d-254">Select **Administrator**.</span></span>
  
    <span data-ttu-id="1065d-255">g.</span><span class="sxs-lookup"><span data-stu-id="1065d-255">g.</span></span> <span data-ttu-id="1065d-256">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="1065d-256">Click **Add**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="1065d-257">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="1065d-257">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="1065d-258">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooMoxtra.</span><span class="sxs-lookup"><span data-stu-id="1065d-258">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooMoxtra.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="1065d-260">**tooassign Britta Simon tooMoxtra utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="1065d-260">**tooassign Britta Simon tooMoxtra, perform hello following steps:**</span></span>

1. <span data-ttu-id="1065d-261">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="1065d-261">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="1065d-263">Välj i listan med program hello **Moxtra**.</span><span class="sxs-lookup"><span data-stu-id="1065d-263">In hello applications list, select **Moxtra**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_app.png) 

3. <span data-ttu-id="1065d-265">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="1065d-265">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="1065d-267">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="1065d-267">Click **Add** button.</span></span> <span data-ttu-id="1065d-268">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1065d-268">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="1065d-270">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="1065d-270">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="1065d-271">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1065d-271">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="1065d-272">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1065d-272">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="1065d-273">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1065d-273">Testing single sign-on</span></span>

<span data-ttu-id="1065d-274">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="1065d-274">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="1065d-275">Du bör få automatiskt inloggade tooyour Moxtra programmet när du klickar på hello Moxtra panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="1065d-275">When you click hello Moxtra tile in hello Access Panel, you should get automatically signed-on tooyour Moxtra application.</span></span>
<span data-ttu-id="1065d-276">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="1065d-276">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1065d-277">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="1065d-277">Additional resources</span></span>

* [<span data-ttu-id="1065d-278">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1065d-278">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="1065d-279">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="1065d-279">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_203.png

