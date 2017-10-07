---
title: "Självstudier: Azure Active Directory-integrering med Trello | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Trello."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: cd5ae365-9ed6-43a6-920b-f7814b993949
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: de2f2ba6a0e5545983c351f26f99d14f436618c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-trello"></a><span data-ttu-id="5c5d1-103">Självstudier: Azure Active Directory-integrering med Trello</span><span class="sxs-lookup"><span data-stu-id="5c5d1-103">Tutorial: Azure Active Directory integration with Trello</span></span>

<span data-ttu-id="5c5d1-104">I kursen får du lära dig hur toointegrate Trello med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="5c5d1-104">In this tutorial, you learn how toointegrate Trello with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5c5d1-105">Integrera Trello med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="5c5d1-105">Integrating Trello with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="5c5d1-106">Du kan styra i Azure AD som har åtkomst till tooTrello</span><span class="sxs-lookup"><span data-stu-id="5c5d1-106">You can control in Azure AD who has access tooTrello</span></span>
- <span data-ttu-id="5c5d1-107">Du kan aktivera din användare tooautomatically get inloggade tooTrello (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="5c5d1-107">You can enable your users tooautomatically get signed-on tooTrello (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5c5d1-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="5c5d1-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="5c5d1-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5c5d1-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5c5d1-110">Krav</span><span class="sxs-lookup"><span data-stu-id="5c5d1-110">Prerequisites</span></span>

<span data-ttu-id="5c5d1-111">tooconfigure Azure AD-integrering med Trello, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="5c5d1-111">tooconfigure Azure AD integration with Trello, you need hello following items:</span></span>

- <span data-ttu-id="5c5d1-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="5c5d1-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5c5d1-113">En Trello enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="5c5d1-113">A Trello single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5c5d1-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5c5d1-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="5c5d1-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5c5d1-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5c5d1-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5c5d1-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5c5d1-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="5c5d1-118">Scenario description</span></span>
<span data-ttu-id="5c5d1-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5c5d1-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="5c5d1-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5c5d1-121">Att lägga till Trello från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="5c5d1-121">Adding Trello from hello gallery</span></span>
2. <span data-ttu-id="5c5d1-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5c5d1-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-trello-from-hello-gallery"></a><span data-ttu-id="5c5d1-123">Att lägga till Trello från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="5c5d1-123">Adding Trello from hello gallery</span></span>
<span data-ttu-id="5c5d1-124">tooconfigure hello integrering av Trello i Azure AD, behöver du tooadd Trello hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-124">tooconfigure hello integration of Trello into Azure AD, you need tooadd Trello from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="5c5d1-125">**tooadd Trello från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="5c5d1-125">**tooadd Trello from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="5c5d1-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5c5d1-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="5c5d1-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="5c5d1-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="5c5d1-133">Skriv i sökrutan hello **Trello**.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-133">In hello search box, type **Trello**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-trello-tutorial/tutorial_trello_search.png)

5. <span data-ttu-id="5c5d1-135">Markera hello resultat på panelen **Trello**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-135">In hello results panel, select **Trello**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-trello-tutorial/tutorial_trello_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5c5d1-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5c5d1-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5c5d1-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Trello baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-138">In this section, you configure and test Azure AD single sign-on with Trello based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="5c5d1-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Trello är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Trello is tooa user in Azure AD.</span></span> <span data-ttu-id="5c5d1-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Trello toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-140">In other words, a link relationship between an Azure AD user and hello related user in Trello needs toobe established.</span></span>

<span data-ttu-id="5c5d1-141">I Trello, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-141">In Trello, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="5c5d1-142">tooconfigure och testa Azure AD enkel inloggning med Trello, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="5c5d1-142">tooconfigure and test Azure AD single sign-on with Trello, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="5c5d1-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="5c5d1-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5c5d1-145">**[Skapa en testanvändare Trello](#creating-a-trello-test-user)**  -toohave en motsvarighet för Britta Simon i Trello som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-145">**[Creating a Trello test user](#creating-a-trello-test-user)** - toohave a counterpart of Britta Simon in Trello that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="5c5d1-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5c5d1-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5c5d1-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5c5d1-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5c5d1-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Trello program.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Trello application.</span></span>

<span data-ttu-id="5c5d1-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Trello:**</span><span class="sxs-lookup"><span data-stu-id="5c5d1-150">**tooconfigure Azure AD single sign-on with Trello, perform hello following steps:**</span></span>

1. <span data-ttu-id="5c5d1-151">I hello Azure-portalen på hello **Trello** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-151">In hello Azure portal, on hello **Trello** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="5c5d1-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-trello-tutorial/tutorial_trello_samlbase.png)

3. <span data-ttu-id="5c5d1-155">På hello **Trello domän och URL: er** om du vill tooconfigure hello programmet i **IDP initierade läge**, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="5c5d1-155">On hello **Trello Domain and URLs** section, If you wish tooconfigure hello application in **IDP initiated mode**, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-trello-tutorial/tutorial_trello_url.png)

    <span data-ttu-id="5c5d1-157">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://trello.com/auth/saml/consume/<enterprise>`</span><span class="sxs-lookup"><span data-stu-id="5c5d1-157">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://trello.com/auth/saml/consume/<enterprise>`</span></span>

4. <span data-ttu-id="5c5d1-158">På hello **Trello domän och URL: er** om du vill tooconfigure hello programmet i **SP initierade läge**, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="5c5d1-158">On hello **Trello Domain and URLs** section, If you wish tooconfigure hello application in **SP initiated mode**, perform hello following steps:</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-trello-tutorial/tutorial_trello_url1.png)

    <span data-ttu-id="5c5d1-160">a.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-160">a.</span></span> <span data-ttu-id="5c5d1-161">Klicka på hello **visa avancerade inställningar för URL: en**.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-161">Click on hello **Show advanced URL settings**.</span></span>

    <span data-ttu-id="5c5d1-162">b.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-162">b.</span></span> <span data-ttu-id="5c5d1-163">I hello **logga URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://trello.com/auth/saml/consume/<enterprise>`</span><span class="sxs-lookup"><span data-stu-id="5c5d1-163">In hello **Sign On URL** textbox, type a URL using hello following pattern: `https://trello.com/auth/saml/consume/<enterprise>`</span></span>

    >[!NOTE]
    ><span data-ttu-id="5c5d1-164">Du bör få hello  **\<enterprise\>**  slug från Trello.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-164">You should get hello **\<enterprise\>** slug from Trello.</span></span> <span data-ttu-id="5c5d1-165">Om du inte har hello slug värde, kontakta [Trello supportteamet](mailto:support@trello.com) tooget hello slug för du enterprise.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-165">If you don't have hello slug value, contact [Trello support team](mailto:support@trello.com) tooget hello slug for you enterprise.</span></span>
    > 

5. <span data-ttu-id="5c5d1-166">Trello program förväntar hello SAML intyg toocontain specifika attribut.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-166">Trello application expects hello SAML assertions toocontain specific attributes.</span></span> <span data-ttu-id="5c5d1-167">Konfigurera hello följande attribut för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-167">Configure hello following attributes  for this application.</span></span> <span data-ttu-id="5c5d1-168">Du kan hantera hello värden för attributen från hello **”användarattribut”** av programmet hello.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-168">You can manage hello values of these attributes from hello **"User Attributes"** of hello application.</span></span> <span data-ttu-id="5c5d1-169">hello följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-169">hello following screenshot shows an example for this.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-trello-tutorial/tutorial_trello_attribute.png)

6. <span data-ttu-id="5c5d1-171">På hello **SAML-token attribut** dialogrutan för varje rad som visas i hello nedan, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="5c5d1-171">On hello **SAML token attributes** dialog, for each row shown in hello table below, perform hello following steps:</span></span>
 
    | <span data-ttu-id="5c5d1-172">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="5c5d1-172">Attribute Name</span></span> | <span data-ttu-id="5c5d1-173">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="5c5d1-173">Attribute Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="5c5d1-174">User.Email</span><span class="sxs-lookup"><span data-stu-id="5c5d1-174">User.Email</span></span> | <span data-ttu-id="5c5d1-175">User.Mail</span><span class="sxs-lookup"><span data-stu-id="5c5d1-175">user.mail</span></span> |
    | <span data-ttu-id="5c5d1-176">User.FirstName</span><span class="sxs-lookup"><span data-stu-id="5c5d1-176">User.FirstName</span></span> | <span data-ttu-id="5c5d1-177">User.givenName</span><span class="sxs-lookup"><span data-stu-id="5c5d1-177">user.givenname</span></span> |
    | <span data-ttu-id="5c5d1-178">User.LastName</span><span class="sxs-lookup"><span data-stu-id="5c5d1-178">User.LastName</span></span> | <span data-ttu-id="5c5d1-179">User.surname</span><span class="sxs-lookup"><span data-stu-id="5c5d1-179">user.surname</span></span> |

    <span data-ttu-id="5c5d1-180">a.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-180">a.</span></span> <span data-ttu-id="5c5d1-181">Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-181">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-trello-tutorial/tutorial_officespace_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-trello-tutorial/tutorial_officespace_05.png)

    <span data-ttu-id="5c5d1-184">b.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-184">b.</span></span> <span data-ttu-id="5c5d1-185">I hello **namn** textruta hello attributnamn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-185">In hello **Name** textbox, type hello attribute name shown for that row.</span></span> 

    <span data-ttu-id="5c5d1-186">c.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-186">c.</span></span> <span data-ttu-id="5c5d1-187">Från hello **värdet** listan attributvärde för typ hello visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-187">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="5c5d1-188">d.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-188">d.</span></span> <span data-ttu-id="5c5d1-189">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-189">Click **Ok**.</span></span> 
 
7. <span data-ttu-id="5c5d1-190">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-190">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-trello-tutorial/tutorial_trello_certificate.png) 

8. <span data-ttu-id="5c5d1-192">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-192">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-trello-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="5c5d1-194">På hello **Trello Configuration** klickar du på **konfigurera Trello** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-194">On hello **Trello Configuration** section, click **Configure Trello** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="5c5d1-195">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="5c5d1-195">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-trello-tutorial/tutorial_trello_configure.png) 

9. <span data-ttu-id="5c5d1-197">Gå tooget SSO konfigurerats för ditt program för[Trello SSO företagskonfiguration](https://trello.com/sso-configuration) sidan toosend [Trello supportteamet](mailto:support@trello.com) hello **SAML inloggning tjänst-URL för enkel** och Koppla hello **certifikat (Base64)**.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-197">tooget SSO configured for your application, go too[Trello enterprise SSO configuration](https://trello.com/sso-configuration) page toosend [Trello support team](mailto:support@trello.com) hello **SAML Single Sign-On Service URL** and attach hello **Certificate (Base64)**.</span></span>

> [!TIP]
> <span data-ttu-id="5c5d1-198">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="5c5d1-198">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="5c5d1-199">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-199">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="5c5d1-200">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5c5d1-200">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5c5d1-201">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="5c5d1-201">Creating an Azure AD test user</span></span>
<span data-ttu-id="5c5d1-202">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-202">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="5c5d1-204">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="5c5d1-204">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="5c5d1-205">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-205">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-trello-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5c5d1-207">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-207">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-trello-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5c5d1-209">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-209">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-trello-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5c5d1-211">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="5c5d1-211">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-trello-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5c5d1-213">a.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-213">a.</span></span> <span data-ttu-id="5c5d1-214">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-214">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5c5d1-215">b.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-215">b.</span></span> <span data-ttu-id="5c5d1-216">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-216">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5c5d1-217">c.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-217">c.</span></span> <span data-ttu-id="5c5d1-218">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-218">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="5c5d1-219">d.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-219">d.</span></span> <span data-ttu-id="5c5d1-220">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-220">Click **Create**.</span></span>
 
### <a name="creating-a-trello-test-user"></a><span data-ttu-id="5c5d1-221">Skapa en testanvändare Trello</span><span class="sxs-lookup"><span data-stu-id="5c5d1-221">Creating a Trello test user</span></span>

<span data-ttu-id="5c5d1-222">I det här avsnittet skapar du en användare som kallas Britta Simon i Trello.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-222">In this section, you create a user called Britta Simon in Trello.</span></span> <span data-ttu-id="5c5d1-223">I det här avsnittet skapar du en användare som kallas Britta Simon i Trello.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-223">In this section, you create a user called Britta Simon in Trello.</span></span> <span data-ttu-id="5c5d1-224">Trello stöder just-in-time-etablering och ett nytt konto skapas hello första gången du loggar in från Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-224">Trello supports just-in-time provisioning and a new account is created hello first time you sign in from Azure AD.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="5c5d1-225">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="5c5d1-225">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="5c5d1-226">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooTrello.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-226">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooTrello.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="5c5d1-228">**tooassign Britta Simon tooTrello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="5c5d1-228">**tooassign Britta Simon tooTrello, perform hello following steps:**</span></span>

1. <span data-ttu-id="5c5d1-229">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-229">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="5c5d1-231">Välj i listan med program hello **Trello**.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-231">In hello applications list, select **Trello**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-trello-tutorial/tutorial_trello_app.png) 

3. <span data-ttu-id="5c5d1-233">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-233">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="5c5d1-235">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-235">Click **Add** button.</span></span> <span data-ttu-id="5c5d1-236">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="5c5d1-238">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-238">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="5c5d1-239">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5c5d1-240">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5c5d1-241">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5c5d1-241">Testing single sign-on</span></span>

<span data-ttu-id="5c5d1-242">hello syftet med det här avsnittet är tootest din Azure AD SSO konfiguration av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-242">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="5c5d1-243">Du bör få automatiskt inloggade tooyour Trello programmet när du klickar på hello Trello panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="5c5d1-243">When you click hello Trello tile in hello Access Panel, you should get automatically signed-on tooyour Trello application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5c5d1-244">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="5c5d1-244">Additional resources</span></span>

* [<span data-ttu-id="5c5d1-245">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5c5d1-245">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5c5d1-246">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5c5d1-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-trello-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-trello-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-trello-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-trello-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-trello-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-trello-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-trello-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-trello-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-trello-tutorial/tutorial_general_203.png

