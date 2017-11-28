---
title: "Självstudier: Azure Active Directory-integrering med Domo | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Domo."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 058626e4-73b3-4dc2-86ca-b060d002d70a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2017
ms.author: jeedes
ms.openlocfilehash: cc70f8e5013f864d275762bbc1f84bd9677e8c16
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-domo"></a><span data-ttu-id="29489-103">Självstudier: Azure Active Directory-integrering med Domo</span><span class="sxs-lookup"><span data-stu-id="29489-103">Tutorial: Azure Active Directory integration with Domo</span></span>

<span data-ttu-id="29489-104">I kursen får du lära dig hur toointegrate Domo med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="29489-104">In this tutorial, you learn how toointegrate Domo with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="29489-105">Integrera Domo med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="29489-105">Integrating Domo with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="29489-106">Du kan styra i Azure AD som har åtkomst till tooDomo</span><span class="sxs-lookup"><span data-stu-id="29489-106">You can control in Azure AD who has access tooDomo</span></span>
- <span data-ttu-id="29489-107">Du kan aktivera din användare tooautomatically get inloggade tooDomo (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="29489-107">You can enable your users tooautomatically get signed-on tooDomo (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="29489-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="29489-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="29489-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="29489-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="29489-110">Krav</span><span class="sxs-lookup"><span data-stu-id="29489-110">Prerequisites</span></span>

<span data-ttu-id="29489-111">tooconfigure Azure AD-integrering med Domo, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="29489-111">tooconfigure Azure AD integration with Domo, you need hello following items:</span></span>

- <span data-ttu-id="29489-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="29489-112">An Azure AD subscription</span></span>
- <span data-ttu-id="29489-113">En Domo enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="29489-113">A Domo single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="29489-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="29489-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="29489-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="29489-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="29489-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="29489-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="29489-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="29489-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="29489-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="29489-118">Scenario description</span></span>
<span data-ttu-id="29489-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="29489-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="29489-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="29489-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="29489-121">Att lägga till Domo från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="29489-121">Adding Domo from hello gallery</span></span>
2. <span data-ttu-id="29489-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="29489-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-domo-from-hello-gallery"></a><span data-ttu-id="29489-123">Att lägga till Domo från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="29489-123">Adding Domo from hello gallery</span></span>
<span data-ttu-id="29489-124">tooconfigure hello integrering av Domo i Azure AD, behöver du tooadd Domo hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="29489-124">tooconfigure hello integration of Domo into Azure AD, you need tooadd Domo from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="29489-125">**tooadd Domo från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="29489-125">**tooadd Domo from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="29489-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="29489-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="29489-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="29489-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="29489-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="29489-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="29489-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="29489-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="29489-133">Skriv i sökrutan hello **Domo**.</span><span class="sxs-lookup"><span data-stu-id="29489-133">In hello search box, type **Domo**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-domo-tutorial/tutorial_domo_search.png)

5. <span data-ttu-id="29489-135">Markera hello resultat på panelen **Domo**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="29489-135">In hello results panel, select **Domo**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-domo-tutorial/tutorial_domo_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="29489-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="29489-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="29489-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Domo baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="29489-138">In this section, you configure and test Azure AD single sign-on with Domo based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="29489-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Domo är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="29489-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Domo is tooa user in Azure AD.</span></span> <span data-ttu-id="29489-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Domo toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="29489-140">In other words, a link relationship between an Azure AD user and hello related user in Domo needs toobe established.</span></span>

<span data-ttu-id="29489-141">I Domo, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="29489-141">In Domo, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="29489-142">tooconfigure och testa Azure AD enkel inloggning med Domo, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="29489-142">tooconfigure and test Azure AD single sign-on with Domo, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="29489-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="29489-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="29489-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="29489-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="29489-145">**[Skapa en testanvändare Domo](#creating-a-domo-test-user)**  -toohave en motsvarighet för Britta Simon i Domo som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="29489-145">**[Creating a Domo test user](#creating-a-domo-test-user)** - toohave a counterpart of Britta Simon in Domo that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="29489-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="29489-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="29489-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="29489-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="29489-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="29489-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="29489-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Domo program.</span><span class="sxs-lookup"><span data-stu-id="29489-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Domo application.</span></span>

<span data-ttu-id="29489-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Domo:**</span><span class="sxs-lookup"><span data-stu-id="29489-150">**tooconfigure Azure AD single sign-on with Domo, perform hello following steps:**</span></span>

1. <span data-ttu-id="29489-151">I hello Azure-portalen på hello **Domo** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="29489-151">In hello Azure portal, on hello **Domo** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="29489-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="29489-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-domo-tutorial/tutorial_domo_samlbase.png)

3. <span data-ttu-id="29489-155">På hello **Domo domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="29489-155">On hello **Domo Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-domo-tutorial/tutorial_domo_url.png)

    <span data-ttu-id="29489-157">a.</span><span class="sxs-lookup"><span data-stu-id="29489-157">a.</span></span> <span data-ttu-id="29489-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.domo.com`</span><span class="sxs-lookup"><span data-stu-id="29489-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.domo.com`</span></span>

    <span data-ttu-id="29489-159">b.</span><span class="sxs-lookup"><span data-stu-id="29489-159">b.</span></span> <span data-ttu-id="29489-160">I hello **identifierare** textruta, ange en Webbadress med hello följande mönster:</span><span class="sxs-lookup"><span data-stu-id="29489-160">In hello **Identifier** textbox, type a URL using hello following patterns:</span></span>     

    | |
    |--|    
    | `https://<companyname>.domo.com` |
    | `https://<companyname>.beta.domo.com` |
    | `https://<companyname>.demo.domo.com` |
    | `https://<companyname>.dev.domo.com` | 
    | `https://<companyname>.fastage1.domo.com` |       
    | `https://<companyname>.frdev.domo.com` |       
    | `https://<companyname>.gastage.domo.com` |       
    | `https://<companyname>.load.domo.com` |       
    | `https://<companyname>.local.domo.com` |       
    | `https://<companyname>.qa.domo.com` |
    | `https://<companyname>.stage.domo.com` |
    
    > [!NOTE] 
    > <span data-ttu-id="29489-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="29489-161">These values are not real.</span></span> <span data-ttu-id="29489-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="29489-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="29489-163">Kontakta [Domo klienten supportteamet](mailto:support@domo.com) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="29489-163">Contact [Domo Client support team](mailto:support@domo.com) tooget these values.</span></span>

4. <span data-ttu-id="29489-164">Domo program förväntar hello SAML intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="29489-164">Domo application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="29489-165">Konfigurera hello följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="29489-165">Configure hello following claims for this application.</span></span> <span data-ttu-id="29489-166">Du kan hantera hello värden för attributen från hello ”**användarattribut**” avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="29489-166">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="29489-167">hello följande skärmbild visar ett exempel på den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="29489-167">hello following screenshot shows an example for this configuration.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-domo-tutorial/tutorial_domo_attributes.png)
    
5. <span data-ttu-id="29489-169">I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan Konfigurera SAML-token attributet enligt hello bild och utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="29489-169">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image and perform hello following steps:</span></span>
    
    | <span data-ttu-id="29489-170">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="29489-170">Attribute Name</span></span> | <span data-ttu-id="29489-171">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="29489-171">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="29489-172">namn</span><span class="sxs-lookup"><span data-stu-id="29489-172">name</span></span> | <span data-ttu-id="29489-173">User.DisplayName</span><span class="sxs-lookup"><span data-stu-id="29489-173">user.displayname</span></span> |
    | <span data-ttu-id="29489-174">E-post</span><span class="sxs-lookup"><span data-stu-id="29489-174">email</span></span> | <span data-ttu-id="29489-175">User.Mail</span><span class="sxs-lookup"><span data-stu-id="29489-175">user.mail</span></span> |
    
    <span data-ttu-id="29489-176">a.</span><span class="sxs-lookup"><span data-stu-id="29489-176">a.</span></span> <span data-ttu-id="29489-177">Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="29489-177">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-domo-tutorial/tutorial_attribute_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-domo-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="29489-180">b.</span><span class="sxs-lookup"><span data-stu-id="29489-180">b.</span></span> <span data-ttu-id="29489-181">I hello **namn** textruta hello attributnamn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="29489-181">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="29489-182">c.</span><span class="sxs-lookup"><span data-stu-id="29489-182">c.</span></span> <span data-ttu-id="29489-183">Från hello **värdet** listan attributvärde för typ hello visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="29489-183">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="29489-184">d.</span><span class="sxs-lookup"><span data-stu-id="29489-184">d.</span></span> <span data-ttu-id="29489-185">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="29489-185">Click **Ok**.</span></span> 
 
6. <span data-ttu-id="29489-186">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="29489-186">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-domo-tutorial/tutorial_domo_certificate.png) 

7. <span data-ttu-id="29489-188">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="29489-188">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-domo-tutorial/tutorial_general_400.png)


8. <span data-ttu-id="29489-190">På hello **Domo Configuration** klickar du på **konfigurera Domo** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="29489-190">On hello **Domo Configuration** section, click **Configure Domo** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="29489-191">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="29489-191">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span> 

   ![Konfigurera enkel inloggning](./media/active-directory-saas-domo-tutorial/tutorial_domo_configure.png) 

9. <span data-ttu-id="29489-193">tooconfigure enkel inloggning på **Domo** sida, behöver du toosend hello hämtas **certifikat**, **SAML enhets-ID**, hello **SAML enkel inloggning URL: en** och hello **Sign-Out URL** för[Domo supportteamet](mailto:support@domo.com).</span><span class="sxs-lookup"><span data-stu-id="29489-193">tooconfigure single sign-on on **Domo** side, you need toosend hello downloaded **Certificate**, **SAML Entity ID**, hello **SAML Single Sign-On Service URL** and hello **Sign-Out URL** too[Domo support team](mailto:support@domo.com).</span></span> <span data-ttu-id="29489-194">De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="29489-194">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="29489-195">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="29489-195">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="29489-196">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="29489-196">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="29489-197">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="29489-197">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="29489-198">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="29489-198">Creating an Azure AD test user</span></span>
<span data-ttu-id="29489-199">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="29489-199">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="29489-201">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="29489-201">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="29489-202">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="29489-202">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-domo-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="29489-204">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="29489-204">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-domo-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="29489-206">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="29489-206">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-domo-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="29489-208">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="29489-208">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-domo-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="29489-210">a.</span><span class="sxs-lookup"><span data-stu-id="29489-210">a.</span></span> <span data-ttu-id="29489-211">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="29489-211">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="29489-212">b.</span><span class="sxs-lookup"><span data-stu-id="29489-212">b.</span></span> <span data-ttu-id="29489-213">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="29489-213">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="29489-214">c.</span><span class="sxs-lookup"><span data-stu-id="29489-214">c.</span></span> <span data-ttu-id="29489-215">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="29489-215">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="29489-216">d.</span><span class="sxs-lookup"><span data-stu-id="29489-216">d.</span></span> <span data-ttu-id="29489-217">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="29489-217">Click **Create**.</span></span>
 
### <a name="creating-a-domo-test-user"></a><span data-ttu-id="29489-218">Skapa en testanvändare Domo</span><span class="sxs-lookup"><span data-stu-id="29489-218">Creating a Domo test user</span></span>

<span data-ttu-id="29489-219">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i Domo.</span><span class="sxs-lookup"><span data-stu-id="29489-219">hello objective of this section is toocreate a user called Britta Simon in Domo.</span></span> <span data-ttu-id="29489-220">Domo stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="29489-220">Domo supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="29489-221">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="29489-221">There is no action item for you in this section.</span></span> <span data-ttu-id="29489-222">En ny användare skapas under ett försök tooaccess Domo om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="29489-222">A new user is created during an attempt tooaccess Domo if it doesn't exist yet.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="29489-223">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="29489-223">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="29489-224">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooDomo.</span><span class="sxs-lookup"><span data-stu-id="29489-224">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooDomo.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="29489-226">**tooassign Britta Simon tooDomo utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="29489-226">**tooassign Britta Simon tooDomo, perform hello following steps:**</span></span>

1. <span data-ttu-id="29489-227">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="29489-227">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="29489-229">Välj i listan med program hello **Domo**.</span><span class="sxs-lookup"><span data-stu-id="29489-229">In hello applications list, select **Domo**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-domo-tutorial/tutorial_domo_app.png) 

3. <span data-ttu-id="29489-231">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="29489-231">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="29489-233">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="29489-233">Click **Add** button.</span></span> <span data-ttu-id="29489-234">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="29489-234">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="29489-236">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="29489-236">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="29489-237">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="29489-237">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="29489-238">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="29489-238">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="29489-239">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="29489-239">Testing single sign-on</span></span>

<span data-ttu-id="29489-240">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="29489-240">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>
<span data-ttu-id="29489-241">Du bör få automatiskt inloggade tooyour Domo programmet när du klickar på hello Domo panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="29489-241">When you click hello Domo tile in hello Access Panel, you should get automatically signed-on tooyour Domo application.</span></span>

<span data-ttu-id="29489-242">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="29489-242">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="29489-243">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="29489-243">Additional resources</span></span>

* [<span data-ttu-id="29489-244">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="29489-244">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="29489-245">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="29489-245">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-domo-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-domo-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-domo-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-domo-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-domo-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-domo-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-domo-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-domo-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-domo-tutorial/tutorial_general_203.png

