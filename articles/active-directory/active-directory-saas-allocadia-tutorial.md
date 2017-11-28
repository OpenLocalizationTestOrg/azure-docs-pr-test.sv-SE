---
title: "Självstudier: Azure Active Directory-integrering med Allocadia | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Allocadia."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c415fc55-6dc1-49f2-a8a2-2fc6e3790d65
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: jeedes
ms.openlocfilehash: 9a01c232f9dc50e690dd348430899db9c13f1564
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-allocadia"></a><span data-ttu-id="029ad-103">Självstudier: Azure Active Directory-integrering med Allocadia</span><span class="sxs-lookup"><span data-stu-id="029ad-103">Tutorial: Azure Active Directory integration with Allocadia</span></span>

<span data-ttu-id="029ad-104">I kursen får du lära dig hur toointegrate Allocadia med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="029ad-104">In this tutorial, you learn how toointegrate Allocadia with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="029ad-105">Integrera Allocadia med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="029ad-105">Integrating Allocadia with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="029ad-106">Du kan styra i Azure AD som har åtkomst till tooAllocadia</span><span class="sxs-lookup"><span data-stu-id="029ad-106">You can control in Azure AD who has access tooAllocadia</span></span>
- <span data-ttu-id="029ad-107">Du kan aktivera din användare tooautomatically get inloggade tooAllocadia (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="029ad-107">You can enable your users tooautomatically get signed-on tooAllocadia (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="029ad-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="029ad-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="029ad-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="029ad-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="029ad-110">Krav</span><span class="sxs-lookup"><span data-stu-id="029ad-110">Prerequisites</span></span>

<span data-ttu-id="029ad-111">tooconfigure Azure AD-integrering med Allocadia, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="029ad-111">tooconfigure Azure AD integration with Allocadia, you need hello following items:</span></span>

- <span data-ttu-id="029ad-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="029ad-112">An Azure AD subscription</span></span>
- <span data-ttu-id="029ad-113">En Allocadia enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="029ad-113">An Allocadia single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="029ad-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="029ad-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="029ad-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="029ad-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="029ad-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="029ad-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="029ad-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="029ad-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="029ad-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="029ad-118">Scenario description</span></span>
<span data-ttu-id="029ad-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="029ad-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="029ad-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="029ad-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="029ad-121">Att lägga till Allocadia från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="029ad-121">Adding Allocadia from hello gallery</span></span>
2. <span data-ttu-id="029ad-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="029ad-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-allocadia-from-hello-gallery"></a><span data-ttu-id="029ad-123">Att lägga till Allocadia från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="029ad-123">Adding Allocadia from hello gallery</span></span>
<span data-ttu-id="029ad-124">tooconfigure hello integrering av Allocadia i Azure AD, behöver du tooadd Allocadia hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="029ad-124">tooconfigure hello integration of Allocadia into Azure AD, you need tooadd Allocadia from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="029ad-125">**tooadd Allocadia från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="029ad-125">**tooadd Allocadia from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="029ad-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="029ad-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="029ad-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="029ad-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="029ad-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="029ad-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="029ad-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="029ad-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="029ad-133">Skriv i sökrutan hello **Allocadia**.</span><span class="sxs-lookup"><span data-stu-id="029ad-133">In hello search box, type **Allocadia**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_search.png)

5. <span data-ttu-id="029ad-135">Markera hello resultat på panelen **Allocadia**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="029ad-135">In hello results panel, select **Allocadia**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="029ad-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="029ad-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="029ad-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Allocadia baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="029ad-138">In this section, you configure and test Azure AD single sign-on with Allocadia based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="029ad-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Allocadia är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="029ad-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Allocadia is tooa user in Azure AD.</span></span> <span data-ttu-id="029ad-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Allocadia toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="029ad-140">In other words, a link relationship between an Azure AD user and hello related user in Allocadia needs toobe established.</span></span>

<span data-ttu-id="029ad-141">I Allocadia, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="029ad-141">In Allocadia, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="029ad-142">tooconfigure och testa Azure AD enkel inloggning med Allocadia, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="029ad-142">tooconfigure and test Azure AD single sign-on with Allocadia, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="029ad-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="029ad-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="029ad-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="029ad-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="029ad-145">**[Skapa en testanvändare Allocadia](#creating-an-allocadia-test-user)**  -toohave en motsvarighet för Britta Simon i Allocadia som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="029ad-145">**[Creating an Allocadia test user](#creating-an-allocadia-test-user)** - toohave a counterpart of Britta Simon in Allocadia that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="029ad-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="029ad-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="029ad-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="029ad-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="029ad-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="029ad-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="029ad-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Allocadia program.</span><span class="sxs-lookup"><span data-stu-id="029ad-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Allocadia application.</span></span>

<span data-ttu-id="029ad-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Allocadia:**</span><span class="sxs-lookup"><span data-stu-id="029ad-150">**tooconfigure Azure AD single sign-on with Allocadia, perform hello following steps:**</span></span>

1. <span data-ttu-id="029ad-151">I hello Azure-portalen på hello **Allocadia** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="029ad-151">In hello Azure portal, on hello **Allocadia** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="029ad-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="029ad-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_samlbase.png)

3. <span data-ttu-id="029ad-155">På hello **Allocadia domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="029ad-155">On hello **Allocadia Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_url.png)

    <span data-ttu-id="029ad-157">a.</span><span class="sxs-lookup"><span data-stu-id="029ad-157">a.</span></span> <span data-ttu-id="029ad-158">I hello **identifierare** textruta, ange en Webbadress med hello följande mönster:</span><span class="sxs-lookup"><span data-stu-id="029ad-158">In hello **Identifier** textbox, type a URL using hello following patterns:</span></span> 
       
     <span data-ttu-id="029ad-159">För testmiljö-`https://na2standby.allocadia.com`</span><span class="sxs-lookup"><span data-stu-id="029ad-159">For test environment -  `https://na2standby.allocadia.com`</span></span>
    
     <span data-ttu-id="029ad-160">För produktionsmiljö-`https://na2.allocadia.com`</span><span class="sxs-lookup"><span data-stu-id="029ad-160">For production environment - `https://na2.allocadia.com`</span></span>

    <span data-ttu-id="029ad-161">b.</span><span class="sxs-lookup"><span data-stu-id="029ad-161">b.</span></span> <span data-ttu-id="029ad-162">I hello **Reply URL** textruta, ange en Webbadress med hello följande mönster:</span><span class="sxs-lookup"><span data-stu-id="029ad-162">In hello **Reply URL** textbox, type a URL using hello following patterns:</span></span> 
    
     <span data-ttu-id="029ad-163">För testmiljö-`https://na2standby.allocadia.com/allocadia/saml/SSO`</span><span class="sxs-lookup"><span data-stu-id="029ad-163">For test environment - `https://na2standby.allocadia.com/allocadia/saml/SSO`</span></span>
    
     <span data-ttu-id="029ad-164">För produktionsmiljö-`https://na2.allocadia.com/allocadia/saml/SSO`</span><span class="sxs-lookup"><span data-stu-id="029ad-164">For production environment - `https://na2.allocadia.com/allocadia/saml/SSO`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="029ad-165">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="029ad-165">These values are not real.</span></span> <span data-ttu-id="029ad-166">Uppdatera dessa värden med hello faktiska identifierare och svars-URL.</span><span class="sxs-lookup"><span data-stu-id="029ad-166">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="029ad-167">Kontakta [Allocadia supportteamet](mailTo:support@allocadia.com) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="029ad-167">Contact [Allocadia support team](mailTo:support@allocadia.com) tooget these values.</span></span>

4. <span data-ttu-id="029ad-168">Allocadia program förväntar hello SAML intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="029ad-168">Allocadia application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="029ad-169">Konfigurera hello följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="029ad-169">Configure hello following claims for this application.</span></span> <span data-ttu-id="029ad-170">Du kan hantera hello värden för attributen från hello ”**användarattribut**” avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="029ad-170">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="029ad-171">hello följande skärmbild visar ett exempel på den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="029ad-171">hello following screenshot shows an example for this configuration.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_attributes.png)
    
5. <span data-ttu-id="029ad-173">I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan Konfigurera SAML-token attributet enligt hello bild och utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="029ad-173">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image and perform hello following steps:</span></span>
    
    | <span data-ttu-id="029ad-174">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="029ad-174">Attribute Name</span></span> | <span data-ttu-id="029ad-175">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="029ad-175">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="029ad-176">Förnamn</span><span class="sxs-lookup"><span data-stu-id="029ad-176">firstname</span></span> | <span data-ttu-id="029ad-177">User.givenName</span><span class="sxs-lookup"><span data-stu-id="029ad-177">user.givenname</span></span> |
    | <span data-ttu-id="029ad-178">Efternamn</span><span class="sxs-lookup"><span data-stu-id="029ad-178">lastname</span></span> | <span data-ttu-id="029ad-179">User.surname</span><span class="sxs-lookup"><span data-stu-id="029ad-179">user.surname</span></span> |
    | <span data-ttu-id="029ad-180">E-post</span><span class="sxs-lookup"><span data-stu-id="029ad-180">email</span></span> | <span data-ttu-id="029ad-181">User.Mail</span><span class="sxs-lookup"><span data-stu-id="029ad-181">user.mail</span></span> |
    
    <span data-ttu-id="029ad-182">a.</span><span class="sxs-lookup"><span data-stu-id="029ad-182">a.</span></span> <span data-ttu-id="029ad-183">Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="029ad-183">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-allocadia-tutorial/tutorial_attribute_04.png)

    <span data-ttu-id="029ad-185">b.</span><span class="sxs-lookup"><span data-stu-id="029ad-185">b.</span></span> <span data-ttu-id="029ad-186">I hello **namn** textruta hello attributnamn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="029ad-186">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-allocadia-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="029ad-188">c.</span><span class="sxs-lookup"><span data-stu-id="029ad-188">c.</span></span> <span data-ttu-id="029ad-189">Från hello **värdet** listan attributvärde för typ hello visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="029ad-189">From hello **Value** list, type hello attribute value shown for that row.</span></span>
 
    <span data-ttu-id="029ad-190">d.</span><span class="sxs-lookup"><span data-stu-id="029ad-190">d.</span></span> <span data-ttu-id="029ad-191">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="029ad-191">Click **Ok**.</span></span>



6. <span data-ttu-id="029ad-192">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="029ad-192">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_certificate.png) 


7. <span data-ttu-id="029ad-194">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="029ad-194">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-allocadia-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="029ad-196">tooconfigure enkel inloggning på **Allocadia** sida, behöver du toosend hello hämtas **XML-Metadata för** för[Allocadia supportteamet](mailTo:support@allocadia.com).</span><span class="sxs-lookup"><span data-stu-id="029ad-196">tooconfigure single sign-on on **Allocadia** side, you need toosend hello downloaded **Metadata XML** too[Allocadia support team](mailTo:support@allocadia.com).</span></span> <span data-ttu-id="029ad-197">De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="029ad-197">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="029ad-198">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="029ad-198">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="029ad-199">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="029ad-199">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="029ad-200">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="029ad-200">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="029ad-201">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="029ad-201">Creating an Azure AD test user</span></span>
<span data-ttu-id="029ad-202">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="029ad-202">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="029ad-204">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="029ad-204">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="029ad-205">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="029ad-205">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-allocadia-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="029ad-207">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="029ad-207">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-allocadia-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="029ad-209">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="029ad-209">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-allocadia-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="029ad-211">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="029ad-211">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-allocadia-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="029ad-213">a.</span><span class="sxs-lookup"><span data-stu-id="029ad-213">a.</span></span> <span data-ttu-id="029ad-214">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="029ad-214">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="029ad-215">b.</span><span class="sxs-lookup"><span data-stu-id="029ad-215">b.</span></span> <span data-ttu-id="029ad-216">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="029ad-216">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="029ad-217">c.</span><span class="sxs-lookup"><span data-stu-id="029ad-217">c.</span></span> <span data-ttu-id="029ad-218">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="029ad-218">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="029ad-219">d.</span><span class="sxs-lookup"><span data-stu-id="029ad-219">d.</span></span> <span data-ttu-id="029ad-220">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="029ad-220">Click **Create**.</span></span>
 
### <a name="creating-an-allocadia-test-user"></a><span data-ttu-id="029ad-221">Skapa en testanvändare Allocadia</span><span class="sxs-lookup"><span data-stu-id="029ad-221">Creating an Allocadia test user</span></span>

<span data-ttu-id="029ad-222">Programmet stöder bara i tid användaretablering.</span><span class="sxs-lookup"><span data-stu-id="029ad-222">Application supports Just in time user provisioning.</span></span> <span data-ttu-id="029ad-223">Efter autentisering skapas användare i hello program automatiskt.</span><span class="sxs-lookup"><span data-stu-id="029ad-223">After authentication users are created in hello application automatically.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="029ad-224">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="029ad-224">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="029ad-225">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooAllocadia.</span><span class="sxs-lookup"><span data-stu-id="029ad-225">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAllocadia.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="029ad-227">**tooassign Britta Simon tooAllocadia utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="029ad-227">**tooassign Britta Simon tooAllocadia, perform hello following steps:**</span></span>

1. <span data-ttu-id="029ad-228">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="029ad-228">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="029ad-230">Välj i listan med program hello **Allocadia**.</span><span class="sxs-lookup"><span data-stu-id="029ad-230">In hello applications list, select **Allocadia**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_app.png) 

3. <span data-ttu-id="029ad-232">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="029ad-232">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="029ad-234">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="029ad-234">Click **Add** button.</span></span> <span data-ttu-id="029ad-235">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="029ad-235">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="029ad-237">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="029ad-237">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="029ad-238">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="029ad-238">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="029ad-239">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="029ad-239">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="029ad-240">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="029ad-240">Testing single sign-on</span></span>

<span data-ttu-id="029ad-241">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="029ad-241">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="029ad-242">Du bör få automatiskt inloggade tooyour Allocadia programmet när du klickar på hello Allocadia panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="029ad-242">When you click hello Allocadia tile in hello Access Panel, you should get automatically signed-on tooyour Allocadia application.</span></span>
<span data-ttu-id="029ad-243">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="029ad-243">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md)</span></span>

## <a name="additional-resources"></a><span data-ttu-id="029ad-244">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="029ad-244">Additional resources</span></span>

* [<span data-ttu-id="029ad-245">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="029ad-245">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="029ad-246">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="029ad-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_203.png

