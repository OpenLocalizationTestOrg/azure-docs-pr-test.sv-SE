---
title: "Självstudier: Azure Active Directory-integrering med TalentLMS | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och TalentLMS."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c903d20d-18e3-42b0-b997-6349c5412dde
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: jeedes
ms.openlocfilehash: 25538086602e58fbaab0fbf223f5b03908a74922
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-talentlms"></a><span data-ttu-id="75368-103">Självstudier: Azure Active Directory-integrering med TalentLMS</span><span class="sxs-lookup"><span data-stu-id="75368-103">Tutorial: Azure Active Directory integration with TalentLMS</span></span>

<span data-ttu-id="75368-104">I kursen får du lära dig hur toointegrate TalentLMS med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="75368-104">In this tutorial, you learn how toointegrate TalentLMS with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="75368-105">Integrera TalentLMS med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="75368-105">Integrating TalentLMS with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="75368-106">Du kan styra i Azure AD som har åtkomst till tooTalentLMS</span><span class="sxs-lookup"><span data-stu-id="75368-106">You can control in Azure AD who has access tooTalentLMS</span></span>
- <span data-ttu-id="75368-107">Du kan aktivera din användare tooautomatically get inloggade tooTalentLMS (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="75368-107">You can enable your users tooautomatically get signed-on tooTalentLMS (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="75368-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="75368-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="75368-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="75368-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="75368-110">Krav</span><span class="sxs-lookup"><span data-stu-id="75368-110">Prerequisites</span></span>

<span data-ttu-id="75368-111">tooconfigure Azure AD-integrering med TalentLMS, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="75368-111">tooconfigure Azure AD integration with TalentLMS, you need hello following items:</span></span>

- <span data-ttu-id="75368-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="75368-112">An Azure AD subscription</span></span>
- <span data-ttu-id="75368-113">En TalentLMS enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="75368-113">A TalentLMS single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="75368-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="75368-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="75368-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="75368-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="75368-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="75368-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="75368-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="75368-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="75368-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="75368-118">Scenario description</span></span>
<span data-ttu-id="75368-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="75368-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="75368-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="75368-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="75368-121">Att lägga till TalentLMS från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="75368-121">Adding TalentLMS from hello gallery</span></span>
2. <span data-ttu-id="75368-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="75368-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-talentlms-from-hello-gallery"></a><span data-ttu-id="75368-123">Att lägga till TalentLMS från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="75368-123">Adding TalentLMS from hello gallery</span></span>
<span data-ttu-id="75368-124">tooconfigure hello integrering av TalentLMS i Azure AD, behöver du tooadd TalentLMS hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="75368-124">tooconfigure hello integration of TalentLMS into Azure AD, you need tooadd TalentLMS from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="75368-125">**tooadd TalentLMS från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="75368-125">**tooadd TalentLMS from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="75368-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="75368-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="75368-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="75368-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="75368-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="75368-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="75368-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="75368-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="75368-133">Skriv i sökrutan hello **TalentLMS**.</span><span class="sxs-lookup"><span data-stu-id="75368-133">In hello search box, type **TalentLMS**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_search.png)

5. <span data-ttu-id="75368-135">Markera hello resultat på panelen **TalentLMS**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="75368-135">In hello results panel, select **TalentLMS**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="75368-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="75368-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="75368-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med TalentLMS baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="75368-138">In this section, you configure and test Azure AD single sign-on with TalentLMS based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="75368-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i TalentLMS är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="75368-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in TalentLMS is tooa user in Azure AD.</span></span> <span data-ttu-id="75368-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i TalentLMS toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="75368-140">In other words, a link relationship between an Azure AD user and hello related user in TalentLMS needs toobe established.</span></span>

<span data-ttu-id="75368-141">I TalentLMS, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="75368-141">In TalentLMS, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="75368-142">tooconfigure och testa Azure AD enkel inloggning med TalentLMS, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="75368-142">tooconfigure and test Azure AD single sign-on with TalentLMS, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="75368-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="75368-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="75368-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="75368-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="75368-145">**[Skapa en testanvändare TalentLMS](#creating-a-talentlms-test-user)**  -toohave en motsvarighet för Britta Simon i TalentLMS som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="75368-145">**[Creating a TalentLMS test user](#creating-a-talentlms-test-user)** - toohave a counterpart of Britta Simon in TalentLMS that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="75368-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="75368-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="75368-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="75368-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="75368-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="75368-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="75368-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt TalentLMS program.</span><span class="sxs-lookup"><span data-stu-id="75368-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your TalentLMS application.</span></span>

<span data-ttu-id="75368-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med TalentLMS:**</span><span class="sxs-lookup"><span data-stu-id="75368-150">**tooconfigure Azure AD single sign-on with TalentLMS, perform hello following steps:**</span></span>

1. <span data-ttu-id="75368-151">I hello Azure-portalen på hello **TalentLMS** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="75368-151">In hello Azure portal, on hello **TalentLMS** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="75368-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="75368-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_samlbase.png)

3. <span data-ttu-id="75368-155">På hello **TalentLMS domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="75368-155">On hello **TalentLMS Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_url.png)

    <span data-ttu-id="75368-157">a.</span><span class="sxs-lookup"><span data-stu-id="75368-157">a.</span></span> <span data-ttu-id="75368-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<tenant-name>.TalentLMSapp.com`</span><span class="sxs-lookup"><span data-stu-id="75368-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenant-name>.TalentLMSapp.com`</span></span>

    <span data-ttu-id="75368-159">b.</span><span class="sxs-lookup"><span data-stu-id="75368-159">b.</span></span> <span data-ttu-id="75368-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`http://<tenant-name>.talentlms.com`</span><span class="sxs-lookup"><span data-stu-id="75368-160">In hello **Identifier** textbox, type a URL using hello following pattern: `http://<tenant-name>.talentlms.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="75368-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="75368-161">These values are not real.</span></span> <span data-ttu-id="75368-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="75368-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="75368-163">Kontakta [TalentLMS klienten supportteamet](https://www.talentlms.com/contact) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="75368-163">Contact [TalentLMS Client support team](https://www.talentlms.com/contact) tooget these values.</span></span> 
 
4. <span data-ttu-id="75368-164">På hello **SAML-signeringscertifikat** avsnitt, kopiera hello **TUMAVTRYCKET** värdet från hello-certifikatet.</span><span class="sxs-lookup"><span data-stu-id="75368-164">On hello **SAML Signing Certificate** section, copy hello **THUMBPRINT** value from hello certificate.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_certificate.png) 

5. <span data-ttu-id="75368-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="75368-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-talentlms-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="75368-168">På hello **TalentLMS Configuration** klickar du på **konfigurera TalentLMS** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="75368-168">On hello **TalentLMS Configuration** section, click **Configure TalentLMS** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="75368-169">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="75368-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_configure.png)  

7. <span data-ttu-id="75368-171">Logga in tooyour TalentLMS företagets webbplats som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="75368-171">In a different web browser window, log in tooyour TalentLMS company site as an administrator.</span></span>

8. <span data-ttu-id="75368-172">I hello **konto & inställningar** klickar du på hello **användare** fliken.</span><span class="sxs-lookup"><span data-stu-id="75368-172">In hello **Account & Settings** section, click hello **Users** tab.</span></span>
   
    <span data-ttu-id="75368-173">![Konto & inställningar](./media/active-directory-saas-talentlms-tutorial/IC777296.png "konto & inställningar")</span><span class="sxs-lookup"><span data-stu-id="75368-173">![Account & Settings](./media/active-directory-saas-talentlms-tutorial/IC777296.png "Account & Settings")</span></span>

9. <span data-ttu-id="75368-174">Klicka på **enkel inloggning (SSO)**,</span><span class="sxs-lookup"><span data-stu-id="75368-174">Click **Single Sign-On (SSO)**,</span></span>

10. <span data-ttu-id="75368-175">I hello Single Sign-On-avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="75368-175">In hello Single Sign-On section, perform hello following steps:</span></span>
   
    <span data-ttu-id="75368-176">![Enkel inloggning](./media/active-directory-saas-talentlms-tutorial/IC777297.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="75368-176">![Single Sign-On](./media/active-directory-saas-talentlms-tutorial/IC777297.png "Single Sign-On")</span></span>   

    <span data-ttu-id="75368-177">a.</span><span class="sxs-lookup"><span data-stu-id="75368-177">a.</span></span> <span data-ttu-id="75368-178">Från hello **SSO integration typen** väljer **SAML 2.0**.</span><span class="sxs-lookup"><span data-stu-id="75368-178">From hello **SSO integration type** list, select **SAML 2.0**.</span></span>

    <span data-ttu-id="75368-179">b.</span><span class="sxs-lookup"><span data-stu-id="75368-179">b.</span></span> <span data-ttu-id="75368-180">I hello **identitetsprovider (IDP)** textruta klistra in hello värdet för **SAML enhets-ID**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="75368-180">In hello **Identity provider (IDP)** textbox, paste hello value of **SAML Entity ID**, which you have copied from Azure portal.</span></span>
 
    <span data-ttu-id="75368-181">c.</span><span class="sxs-lookup"><span data-stu-id="75368-181">c.</span></span> <span data-ttu-id="75368-182">Klistra in hello **tumavtrycket** värdet från Azure-portalen i hello **certifikat fingeravtryck** textruta.</span><span class="sxs-lookup"><span data-stu-id="75368-182">Paste hello **Thumbprint** value from Azure portal into hello **Certificate fingerprint** textbox.</span></span>    

    <span data-ttu-id="75368-183">d.</span><span class="sxs-lookup"><span data-stu-id="75368-183">d.</span></span>  <span data-ttu-id="75368-184">I hello **Remote inloggning URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="75368-184">In hello **Remote sign-in URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
 
    <span data-ttu-id="75368-185">e.</span><span class="sxs-lookup"><span data-stu-id="75368-185">e.</span></span> <span data-ttu-id="75368-186">I hello **extern URL för utloggning** textruta klistra in hello värdet för **Sign-Out URL**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="75368-186">In hello **Remote sign-out URL** textbox, paste hello value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="75368-187">f.</span><span class="sxs-lookup"><span data-stu-id="75368-187">f.</span></span> <span data-ttu-id="75368-188">Fyll i hello följande:</span><span class="sxs-lookup"><span data-stu-id="75368-188">Fill in hello following:</span></span> 

    * <span data-ttu-id="75368-189">I hello **TargetedID** textruta typ`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`</span><span class="sxs-lookup"><span data-stu-id="75368-189">In hello **TargetedID** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`</span></span>
     
    * <span data-ttu-id="75368-190">I hello **Förnamn** textruta typ`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`</span><span class="sxs-lookup"><span data-stu-id="75368-190">In hello **First name** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`</span></span>
    
    * <span data-ttu-id="75368-191">I hello **efternamn** textruta typ`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`</span><span class="sxs-lookup"><span data-stu-id="75368-191">In hello **Last name** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`</span></span>
    
    * <span data-ttu-id="75368-192">I hello **e-post** textruta typ`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`</span><span class="sxs-lookup"><span data-stu-id="75368-192">In hello **Email** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`</span></span>
    
11. <span data-ttu-id="75368-193">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="75368-193">Click **Save**.</span></span>
 
> [!TIP]
> <span data-ttu-id="75368-194">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="75368-194">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="75368-195">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="75368-195">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="75368-196">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="75368-196">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="75368-197">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="75368-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="75368-198">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="75368-198">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="75368-200">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="75368-200">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="75368-201">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="75368-201">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-talentlms-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="75368-203">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="75368-203">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-talentlms-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="75368-205">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="75368-205">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-talentlms-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="75368-207">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="75368-207">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-talentlms-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="75368-209">a.</span><span class="sxs-lookup"><span data-stu-id="75368-209">a.</span></span> <span data-ttu-id="75368-210">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="75368-210">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="75368-211">b.</span><span class="sxs-lookup"><span data-stu-id="75368-211">b.</span></span> <span data-ttu-id="75368-212">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="75368-212">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="75368-213">c.</span><span class="sxs-lookup"><span data-stu-id="75368-213">c.</span></span> <span data-ttu-id="75368-214">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="75368-214">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="75368-215">d.</span><span class="sxs-lookup"><span data-stu-id="75368-215">d.</span></span> <span data-ttu-id="75368-216">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="75368-216">Click **Create**.</span></span>
 
### <a name="creating-a-talentlms-test-user"></a><span data-ttu-id="75368-217">Skapa en testanvändare TalentLMS</span><span class="sxs-lookup"><span data-stu-id="75368-217">Creating a TalentLMS test user</span></span>

<span data-ttu-id="75368-218">tooenable Azure AD-användare toolog i tooTalentLMS, måste de etableras i TalentLMS.</span><span class="sxs-lookup"><span data-stu-id="75368-218">tooenable Azure AD users toolog in tooTalentLMS, they must be provisioned into TalentLMS.</span></span> <span data-ttu-id="75368-219">Hello gäller TalentLMS är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="75368-219">In hello case of TalentLMS, provisioning is a manual task.</span></span>

<span data-ttu-id="75368-220">**tooprovision ett användarkonto, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="75368-220">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="75368-221">Logga in tooyour **TalentLMS** klient.</span><span class="sxs-lookup"><span data-stu-id="75368-221">Log in tooyour **TalentLMS** tenant.</span></span>

2. <span data-ttu-id="75368-222">Klicka på **användare**, och klicka sedan på **Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="75368-222">Click **Users**, and then click **Add User**.</span></span>

3. <span data-ttu-id="75368-223">På hello **Lägg till användare** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="75368-223">On hello **Add user** dialog page, perform hello following steps:</span></span>
   
    <span data-ttu-id="75368-224">![Lägg till användare](./media/active-directory-saas-talentlms-tutorial/IC777299.png "lägga till användare")</span><span class="sxs-lookup"><span data-stu-id="75368-224">![Add User](./media/active-directory-saas-talentlms-tutorial/IC777299.png "Add User")</span></span>  

    <span data-ttu-id="75368-225">a.</span><span class="sxs-lookup"><span data-stu-id="75368-225">a.</span></span> <span data-ttu-id="75368-226">I hello **Förnamn** textruta anger hello först namnet på användaren som **Britta**.</span><span class="sxs-lookup"><span data-stu-id="75368-226">In hello **First name** textbox, enter hello first name of user like **Britta**.</span></span>

    <span data-ttu-id="75368-227">b.</span><span class="sxs-lookup"><span data-stu-id="75368-227">b.</span></span> <span data-ttu-id="75368-228">I hello **efternamn** textruta ange hello efternamn för användaren som **Simon**.</span><span class="sxs-lookup"><span data-stu-id="75368-228">In hello **Last name** textbox, enter hello last name of user like **Simon**.</span></span>
 
    <span data-ttu-id="75368-229">c.</span><span class="sxs-lookup"><span data-stu-id="75368-229">c.</span></span> <span data-ttu-id="75368-230">I hello **e-postadress** textruta ange hello e-postadress för användaren som  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="75368-230">In hello **Email address** textbox, enter hello email of user like **brittasimon@contoso.com**.</span></span>

    <span data-ttu-id="75368-231">d.</span><span class="sxs-lookup"><span data-stu-id="75368-231">d.</span></span> <span data-ttu-id="75368-232">Klicka på **lägga till användare**.</span><span class="sxs-lookup"><span data-stu-id="75368-232">Click **Add User**.</span></span>

>[!NOTE]
><span data-ttu-id="75368-233">Du kan använda något annat TalentLMS användarens konto skapas verktyg eller API: er som tillhandahålls av TalentLMS tooprovision AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="75368-233">You can use any other TalentLMS user account creation tools or APIs provided by TalentLMS tooprovision AAD user accounts.</span></span>
 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="75368-234">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="75368-234">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="75368-235">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooTalentLMS.</span><span class="sxs-lookup"><span data-stu-id="75368-235">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooTalentLMS.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="75368-237">**tooassign Britta Simon tooTalentLMS utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="75368-237">**tooassign Britta Simon tooTalentLMS, perform hello following steps:**</span></span>

1. <span data-ttu-id="75368-238">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="75368-238">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="75368-240">Välj i listan med program hello **TalentLMS**.</span><span class="sxs-lookup"><span data-stu-id="75368-240">In hello applications list, select **TalentLMS**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-talentlms-tutorial/tutorial_talentlms_app.png) 

3. <span data-ttu-id="75368-242">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="75368-242">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="75368-244">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="75368-244">Click **Add** button.</span></span> <span data-ttu-id="75368-245">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="75368-245">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="75368-247">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="75368-247">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="75368-248">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="75368-248">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="75368-249">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="75368-249">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="75368-250">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="75368-250">Testing single sign-on</span></span>

<span data-ttu-id="75368-251">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="75368-251">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="75368-252">När du klickar på hello TalentLMS panelen i hello åtkomstpanelen bör du få automatiskt inloggade tooyour TalentLMS program</span><span class="sxs-lookup"><span data-stu-id="75368-252">When you click hello TalentLMS tile in hello Access Panel, you should get automatically signed-on tooyour TalentLMS application</span></span>

## <a name="additional-resources"></a><span data-ttu-id="75368-253">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="75368-253">Additional resources</span></span>

* [<span data-ttu-id="75368-254">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="75368-254">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="75368-255">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="75368-255">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-talentlms-tutorial/tutorial_general_203.png

