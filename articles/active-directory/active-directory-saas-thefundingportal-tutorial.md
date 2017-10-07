---
title: "Självstudier: Azure Active Directory-integrering med hello finansiering Portal | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och hello finansiering Portal."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4663cc8a-976a-4c6c-b3b4-1e5df9b66744
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.openlocfilehash: 9f4329e02f91eb6d8034f17646ac7d15afe503e8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-hello-funding-portal"></a><span data-ttu-id="f0462-103">Självstudier: Azure Active Directory-integrering med hello finansiering Portal</span><span class="sxs-lookup"><span data-stu-id="f0462-103">Tutorial: Azure Active Directory integration with hello Funding Portal</span></span>

<span data-ttu-id="f0462-104">I kursen får du lära dig hur toointegrate hello finansiering Portal med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="f0462-104">In this tutorial, you learn how toointegrate hello Funding Portal with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f0462-105">Integrera hello innehåller finansiering Portal med Azure AD hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="f0462-105">Integrating hello Funding Portal with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="f0462-106">Du kan styra i Azure AD som har åtkomst toohello Funding Portal</span><span class="sxs-lookup"><span data-stu-id="f0462-106">You can control in Azure AD who has access toohello Funding Portal</span></span>
- <span data-ttu-id="f0462-107">Du kan aktivera din användare tooautomatically get inloggade toohello Funding Portal (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="f0462-107">You can enable your users tooautomatically get signed-on toohello Funding Portal (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f0462-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="f0462-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="f0462-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f0462-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f0462-110">Krav</span><span class="sxs-lookup"><span data-stu-id="f0462-110">Prerequisites</span></span>

<span data-ttu-id="f0462-111">tooconfigure Azure AD-integrering med hello Funding Portal, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="f0462-111">tooconfigure Azure AD integration with hello Funding Portal, you need hello following items:</span></span>

- <span data-ttu-id="f0462-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="f0462-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f0462-113">En hello finansiering Portal enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="f0462-113">A hello Funding Portal single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f0462-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="f0462-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f0462-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="f0462-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f0462-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="f0462-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f0462-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f0462-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f0462-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="f0462-118">Scenario description</span></span>
<span data-ttu-id="f0462-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="f0462-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f0462-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="f0462-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f0462-121">Att lägga till hello Funding portalen från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="f0462-121">Adding hello Funding Portal from hello gallery</span></span>
2. <span data-ttu-id="f0462-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f0462-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-hello-funding-portal-from-hello-gallery"></a><span data-ttu-id="f0462-123">Att lägga till hello Funding portalen från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="f0462-123">Adding hello Funding Portal from hello gallery</span></span>
<span data-ttu-id="f0462-124">tooconfigure hello integrering av hello Funding portalen i Azure AD, behöver du tooadd hello Funding Portal hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="f0462-124">tooconfigure hello integration of hello Funding Portal into Azure AD, you need tooadd hello Funding Portal from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="f0462-125">**tooadd Hej finansiering portalen från hello-galleriet, utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="f0462-125">**tooadd hello Funding Portal from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="f0462-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="f0462-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f0462-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="f0462-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="f0462-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="f0462-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="f0462-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f0462-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="f0462-133">Skriv i sökrutan hello **hello finansiering Portal**.</span><span class="sxs-lookup"><span data-stu-id="f0462-133">In hello search box, type **hello Funding Portal**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_search.png)

5. <span data-ttu-id="f0462-135">Markera hello resultat på panelen **hello finansiering Portal**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="f0462-135">In hello results panel, select **hello Funding Portal**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f0462-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f0462-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f0462-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med hello finansiering Portal baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="f0462-138">In this section, you configure and test Azure AD single sign-on with hello Funding Portal based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="f0462-139">För enkel inloggning toowork Azure AD måste tooknow vilken användare som hello motsvarighet i hello finansiering Portal är tooa användare i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f0462-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in hello Funding Portal is tooa user in Azure AD.</span></span> <span data-ttu-id="f0462-140">Med andra ord en länk relationen mellan en Azure AD-användare och hello relaterade användare i hello finansiering portalen måste toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="f0462-140">In other words, a link relationship between an Azure AD user and hello related user in hello Funding Portal needs toobe established.</span></span>

<span data-ttu-id="f0462-141">I Hej finansiering Portal, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="f0462-141">In hello Funding Portal, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="f0462-142">tooconfigure och testa Azure AD enkel inloggning med hello Funding Portal, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="f0462-142">tooconfigure and test Azure AD single sign-on with hello Funding Portal, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="f0462-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="f0462-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="f0462-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f0462-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f0462-145">**[Skapa hello finansiering Portal testanvändare](#creating-the-funding-portal-test-user)**  -toohave en motsvarighet för Britta Simon i hello Funding Portal som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="f0462-145">**[Creating hello Funding Portal test user](#creating-the-funding-portal-test-user)** - toohave a counterpart of Britta Simon in hello Funding Portal that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="f0462-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="f0462-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f0462-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="f0462-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f0462-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f0462-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f0462-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i din hello finansiering Portal-programmet.</span><span class="sxs-lookup"><span data-stu-id="f0462-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your hello Funding Portal application.</span></span>

<span data-ttu-id="f0462-150">**Hej finansiering Portal, utför följande steg hello tooconfigure Azure AD enkel inloggning med:**</span><span class="sxs-lookup"><span data-stu-id="f0462-150">**tooconfigure Azure AD single sign-on with hello Funding Portal, perform hello following steps:**</span></span>

1. <span data-ttu-id="f0462-151">I hello Azure-portalen på hello **hello finansiering Portal** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="f0462-151">In hello Azure portal, on hello **hello Funding Portal** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="f0462-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="f0462-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_samlbase.png)

3. <span data-ttu-id="f0462-155">På hello **hello finansiering Portal domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="f0462-155">On hello **hello Funding Portal Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_url.png)

    <span data-ttu-id="f0462-157">a.</span><span class="sxs-lookup"><span data-stu-id="f0462-157">a.</span></span> <span data-ttu-id="f0462-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.regenteducation.net/`</span><span class="sxs-lookup"><span data-stu-id="f0462-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.regenteducation.net/`</span></span>

    <span data-ttu-id="f0462-159">b.</span><span class="sxs-lookup"><span data-stu-id="f0462-159">b.</span></span> <span data-ttu-id="f0462-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.regenteducation.net`</span><span class="sxs-lookup"><span data-stu-id="f0462-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.regenteducation.net`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="f0462-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="f0462-161">These values are not real.</span></span> <span data-ttu-id="f0462-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="f0462-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="f0462-163">Kontakta [hello finansiering Portal klienten supportteamet](mailto:info@regenteducation.com) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="f0462-163">Contact [hello Funding Portal Client support team](mailto:info@regenteducation.com) tooget these values.</span></span> 

4. <span data-ttu-id="f0462-164">hello finansiering portalprogram förväntar hello SAML intyg toocontain ett attribut med namnet ”externalId1”.</span><span class="sxs-lookup"><span data-stu-id="f0462-164">hello Funding Portal application expects hello SAML assertions toocontain an attribute named "externalId1".</span></span> <span data-ttu-id="f0462-165">hello-värdet för ”externalId1” ska vara en känd studentID.</span><span class="sxs-lookup"><span data-stu-id="f0462-165">hello value of "externalId1" should be a recognized studentID.</span></span> <span data-ttu-id="f0462-166">Konfigurera hello ”externalId1” anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="f0462-166">Configure hello "externalId1" claim for this application.</span></span> <span data-ttu-id="f0462-167">Du kan hantera hello värden för attributen från hello **användarattribut** av programmet hello.</span><span class="sxs-lookup"><span data-stu-id="f0462-167">You can manage hello values of these attributes from hello **User Attributes** of hello application.</span></span> <span data-ttu-id="f0462-168">hello följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="f0462-168">hello following screenshot shows an example for this.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_attribute.png)

5. <span data-ttu-id="f0462-170">I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan Konfigurera SAML-token attributet enligt hello bild och utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="f0462-170">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image and perform hello following steps:</span></span>

    | <span data-ttu-id="f0462-171">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="f0462-171">Attribute Name</span></span> | <span data-ttu-id="f0462-172">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="f0462-172">Attribute Value</span></span> |
    | ------------------- | ---------------- |
    | <span data-ttu-id="f0462-173">externalId1</span><span class="sxs-lookup"><span data-stu-id="f0462-173">externalId1</span></span> | <span data-ttu-id="f0462-174">User.extensionattribute1</span><span class="sxs-lookup"><span data-stu-id="f0462-174">user.extensionattribute1</span></span> |

    <span data-ttu-id="f0462-175">a.</span><span class="sxs-lookup"><span data-stu-id="f0462-175">a.</span></span> <span data-ttu-id="f0462-176">Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f0462-176">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-thefundingportal-tutorial/tutorial_attribute_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-thefundingportal-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="f0462-179">b.</span><span class="sxs-lookup"><span data-stu-id="f0462-179">b.</span></span> <span data-ttu-id="f0462-180">I hello **namn** textruta hello attributnamn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="f0462-180">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="f0462-181">c.</span><span class="sxs-lookup"><span data-stu-id="f0462-181">c.</span></span> <span data-ttu-id="f0462-182">Från hello **attributvärdet** listan, Välj hello-attribut som du vill toouse för din implementering.</span><span class="sxs-lookup"><span data-stu-id="f0462-182">From hello **Attribute Value** list, select hello attribute you want toouse for your implementation.</span></span> <span data-ttu-id="f0462-183">Till exempel om du har lagrat hello StudentID värde i hello ExtensionAttribute1, väljer du user.extensionattribute1.</span><span class="sxs-lookup"><span data-stu-id="f0462-183">For example, if you have stored hello StudentID value in hello ExtensionAttribute1, then select user.extensionattribute1.</span></span>
    
    <span data-ttu-id="f0462-184">d.</span><span class="sxs-lookup"><span data-stu-id="f0462-184">d.</span></span> <span data-ttu-id="f0462-185">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="f0462-185">Click **Ok**.</span></span>
 
6. <span data-ttu-id="f0462-186">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="f0462-186">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_certificate.png) 

7. <span data-ttu-id="f0462-188">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="f0462-188">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="f0462-190">tooconfigure enkel inloggning på **hello finansiering Portal** sida, behöver du toosend hello hämtas **XML-Metadata för** för[hello finansiering Portal supportteamet](mailto:info@regenteducation.com).</span><span class="sxs-lookup"><span data-stu-id="f0462-190">tooconfigure single sign-on on **hello Funding Portal** side, you need toosend hello downloaded **Metadata XML** too[hello Funding Portal support team](mailto:info@regenteducation.com).</span></span> <span data-ttu-id="f0462-191">De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="f0462-191">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="f0462-192">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="f0462-192">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="f0462-193">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="f0462-193">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="f0462-194">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f0462-194">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f0462-195">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="f0462-195">Creating an Azure AD test user</span></span>
<span data-ttu-id="f0462-196">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f0462-196">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="f0462-198">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="f0462-198">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="f0462-199">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="f0462-199">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f0462-201">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="f0462-201">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f0462-203">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f0462-203">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f0462-205">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="f0462-205">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f0462-207">a.</span><span class="sxs-lookup"><span data-stu-id="f0462-207">a.</span></span> <span data-ttu-id="f0462-208">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f0462-208">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f0462-209">b.</span><span class="sxs-lookup"><span data-stu-id="f0462-209">b.</span></span> <span data-ttu-id="f0462-210">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f0462-210">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f0462-211">c.</span><span class="sxs-lookup"><span data-stu-id="f0462-211">c.</span></span> <span data-ttu-id="f0462-212">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="f0462-212">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="f0462-213">d.</span><span class="sxs-lookup"><span data-stu-id="f0462-213">d.</span></span> <span data-ttu-id="f0462-214">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="f0462-214">Click **Create**.</span></span>
 
### <a name="creating-hello-funding-portal-test-user"></a><span data-ttu-id="f0462-215">Skapa hello finansiering Portal testanvändare</span><span class="sxs-lookup"><span data-stu-id="f0462-215">Creating hello Funding Portal test user</span></span>

<span data-ttu-id="f0462-216">I det här avsnittet kan du skapa en användare som kallas Britta Simon i hello Funding Portal.</span><span class="sxs-lookup"><span data-stu-id="f0462-216">In this section, you create a user called Britta Simon in hello Funding Portal.</span></span> <span data-ttu-id="f0462-217">Arbeta med [hello finansiering Portal supportteamet](mailto:info@regenteducation.com) tooadd hello testanvändare och aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="f0462-217">Work with [hello Funding Portal support team](mailto:info@regenteducation.com) tooadd hello test user and enable SSO.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="f0462-218">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="f0462-218">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="f0462-219">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst toohello Funding Portal.</span><span class="sxs-lookup"><span data-stu-id="f0462-219">In this section, you enable Britta Simon toouse Azure single sign-on by granting access toohello Funding Portal.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="f0462-221">**tooassign Britta Simon toohello Funding Portal utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="f0462-221">**tooassign Britta Simon toohello Funding Portal, perform hello following steps:**</span></span>

1. <span data-ttu-id="f0462-222">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="f0462-222">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="f0462-224">Välj i listan med program hello **hello finansiering Portal**.</span><span class="sxs-lookup"><span data-stu-id="f0462-224">In hello applications list, select **hello Funding Portal**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_app.png) 

3. <span data-ttu-id="f0462-226">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="f0462-226">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="f0462-228">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="f0462-228">Click **Add** button.</span></span> <span data-ttu-id="f0462-229">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f0462-229">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="f0462-231">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="f0462-231">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="f0462-232">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f0462-232">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f0462-233">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f0462-233">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f0462-234">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f0462-234">Testing single sign-on</span></span>

<span data-ttu-id="f0462-235">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="f0462-235">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="f0462-236">Du bör få automatiskt inloggade tooyour hello finansiering portalprogram när du klickar på hello hello finansiering Portal panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="f0462-236">When you click hello hello Funding Portal tile in hello Access Panel, you should get automatically signed-on tooyour hello Funding Portal application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f0462-237">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="f0462-237">Additional resources</span></span>

* [<span data-ttu-id="f0462-238">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f0462-238">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f0462-239">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="f0462-239">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_203.png

