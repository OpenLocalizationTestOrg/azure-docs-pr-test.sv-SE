---
title: "Självstudier: Azure Active Directory-integrering med Mimecast personliga Portal | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Mimecast personliga Portal."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 345b22be-d87e-45a4-b4c0-70a67eaf9bfd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: ee2a8edcab36f295732ac1ebe641ed7fcfc1f2a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mimecast-personal-portal"></a><span data-ttu-id="1bbe8-103">Självstudier: Azure Active Directory-integrering med Mimecast personliga Portal</span><span class="sxs-lookup"><span data-stu-id="1bbe8-103">Tutorial: Azure Active Directory integration with Mimecast Personal Portal</span></span>

<span data-ttu-id="1bbe8-104">I kursen får du lära dig hur toointegrate Mimecast personliga Portal med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="1bbe8-104">In this tutorial, you learn how toointegrate Mimecast Personal Portal with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="1bbe8-105">Integrera Mimecast personliga Portal med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="1bbe8-105">Integrating Mimecast Personal Portal with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="1bbe8-106">Du kan styra i Azure AD som har åtkomst till tooMimecast personliga Portal</span><span class="sxs-lookup"><span data-stu-id="1bbe8-106">You can control in Azure AD who has access tooMimecast Personal Portal</span></span>
- <span data-ttu-id="1bbe8-107">Du kan aktivera din användare tooautomatically get inloggade tooMimecast personliga Portal (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="1bbe8-107">You can enable your users tooautomatically get signed-on tooMimecast Personal Portal (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="1bbe8-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="1bbe8-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="1bbe8-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="1bbe8-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1bbe8-110">Krav</span><span class="sxs-lookup"><span data-stu-id="1bbe8-110">Prerequisites</span></span>

<span data-ttu-id="1bbe8-111">tooconfigure Azure AD-integrering med Mimecast personliga Portal, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="1bbe8-111">tooconfigure Azure AD integration with Mimecast Personal Portal, you need hello following items:</span></span>

- <span data-ttu-id="1bbe8-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="1bbe8-112">An Azure AD subscription</span></span>
- <span data-ttu-id="1bbe8-113">En Mimecast personliga Portal enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="1bbe8-113">A Mimecast Personal Portal single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="1bbe8-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="1bbe8-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="1bbe8-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="1bbe8-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="1bbe8-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1bbe8-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="1bbe8-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="1bbe8-118">Scenario description</span></span>
<span data-ttu-id="1bbe8-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="1bbe8-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="1bbe8-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="1bbe8-121">Att lägga till Mimecast personliga portalen från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="1bbe8-121">Adding Mimecast Personal Portal from hello gallery</span></span>
2. <span data-ttu-id="1bbe8-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1bbe8-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-mimecast-personal-portal-from-hello-gallery"></a><span data-ttu-id="1bbe8-123">Att lägga till Mimecast personliga portalen från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="1bbe8-123">Adding Mimecast Personal Portal from hello gallery</span></span>
<span data-ttu-id="1bbe8-124">tooconfigure hello integrering av Mimecast personliga portalen i Azure AD, behöver du tooadd Mimecast personliga Portal hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-124">tooconfigure hello integration of Mimecast Personal Portal into Azure AD, you need tooadd Mimecast Personal Portal from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="1bbe8-125">**tooadd Mimecast personliga portalen från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="1bbe8-125">**tooadd Mimecast Personal Portal from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="1bbe8-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="1bbe8-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="1bbe8-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="1bbe8-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="1bbe8-133">Skriv i sökrutan hello **Mimecast personliga Portal**.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-133">In hello search box, type **Mimecast Personal Portal**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_search.png)

5. <span data-ttu-id="1bbe8-135">Markera hello resultat på panelen **Mimecast personliga Portal**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-135">In hello results panel, select **Mimecast Personal Portal**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="1bbe8-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1bbe8-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="1bbe8-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Mimecast personliga Portal baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-138">In this section, you configure and test Azure AD single sign-on with Mimecast Personal Portal based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="1bbe8-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Mimecast personliga Portal är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Mimecast Personal Portal is tooa user in Azure AD.</span></span> <span data-ttu-id="1bbe8-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i Mimecast personliga Portal toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-140">In other words, a link relationship between an Azure AD user and hello related user in Mimecast Personal Portal needs toobe established.</span></span>

<span data-ttu-id="1bbe8-141">I Mimecast personliga Portal, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-141">In Mimecast Personal Portal, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="1bbe8-142">tooconfigure och testa Azure AD enkel inloggning med Mimecast personliga Portal, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="1bbe8-142">tooconfigure and test Azure AD single sign-on with Mimecast Personal Portal, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="1bbe8-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="1bbe8-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="1bbe8-145">**[Skapa en testanvändare Mimecast personliga Portal](#creating-a-mimecast-personal-portal-test-user)**  -toohave en motsvarighet för Britta Simon i Mimecast personliga Portal som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-145">**[Creating a Mimecast Personal Portal test user](#creating-a-mimecast-personal-portal-test-user)** - toohave a counterpart of Britta Simon in Mimecast Personal Portal that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="1bbe8-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="1bbe8-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="1bbe8-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1bbe8-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="1bbe8-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Mimecast personliga Portal.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Mimecast Personal Portal application.</span></span>

<span data-ttu-id="1bbe8-150">**tooconfigure Azure AD enkel inloggning med Mimecast personliga Portal, utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="1bbe8-150">**tooconfigure Azure AD single sign-on with Mimecast Personal Portal, perform hello following steps:**</span></span>

1. <span data-ttu-id="1bbe8-151">I hello Azure-portalen på hello **Mimecast personliga Portal** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-151">In hello Azure portal, on hello **Mimecast Personal Portal** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="1bbe8-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_samlbase.png)

3. <span data-ttu-id="1bbe8-155">På hello **Mimecast personliga Portal domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="1bbe8-155">On hello **Mimecast Personal Portal Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_url.png)

    <span data-ttu-id="1bbe8-157">a.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-157">a.</span></span> <span data-ttu-id="1bbe8-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:</span><span class="sxs-lookup"><span data-stu-id="1bbe8-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern:</span></span> 
    | |     
    | ----------------------------------------|
    | `https://webmail-uk.mimecast.com`|
    | `https://webmail-us.mimecast.com`|
    | |
   
    <span data-ttu-id="1bbe8-159">b.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-159">b.</span></span> <span data-ttu-id="1bbe8-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:</span><span class="sxs-lookup"><span data-stu-id="1bbe8-160">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>

    | |     
    | --- |
    | `https://webmail-us.mimecast.com/sso/<companyname>`|
    | `https://webmail-uk.mimecast.com/sso/<companyname>`|    
    | `https://webmail-za.mimecast.com/sso/<companyname>`|
    | `https://webmail.mimecast-offshore.com/sso/<companyname>`|
    ||                                                 
    
    > [!NOTE] 
    > <span data-ttu-id="1bbe8-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-161">These values are not real.</span></span> <span data-ttu-id="1bbe8-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="1bbe8-163">Kontakta [Mimecast personliga Portal klienten supportteamet](https://www.mimecast.com/customer-success/technical-support/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-163">Contact [Mimecast Personal Portal Client support team](https://www.mimecast.com/customer-success/technical-support/) tooget these values.</span></span> 
 


4. <span data-ttu-id="1bbe8-164">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_certificate.png) 

5. <span data-ttu-id="1bbe8-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="1bbe8-168">På hello **Mimecast personliga Portal Configuration** klickar du på **konfigurera Mimecast personliga Portal** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-168">On hello **Mimecast Personal Portal Configuration** section, click **Configure Mimecast Personal Portal** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="1bbe8-169">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="1bbe8-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_configure.png) 

7. <span data-ttu-id="1bbe8-171">Logga in på ditt personliga Mimecast Portal som administratör i ett annat webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-171">In a different web browser window, log into your Mimecast Personal Portal as an administrator.</span></span>

8. <span data-ttu-id="1bbe8-172">Gå för**Services \> program**.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-172">Go too**Services \> Applications**.</span></span>
   
    <span data-ttu-id="1bbe8-173">![Program](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic794998.png "program")</span><span class="sxs-lookup"><span data-stu-id="1bbe8-173">![Applications](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic794998.png "Applications")</span></span>

9. <span data-ttu-id="1bbe8-174">Klicka på **autentisering profiler**.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-174">Click **Authentication Profiles**.</span></span>
   
    <span data-ttu-id="1bbe8-175">![Profiler för autentisering](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic794999.png "profiler för autentisering")</span><span class="sxs-lookup"><span data-stu-id="1bbe8-175">![Authentication Profiles](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic794999.png "Authentication Profiles")</span></span>

10. <span data-ttu-id="1bbe8-176">Klicka på **nya autentiseringsprofil**.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-176">Click **New Authentication Profile**.</span></span>
   
    <span data-ttu-id="1bbe8-177">![Nya autentiseringsprofil](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795000.png "ny autentiseringsprofil")</span><span class="sxs-lookup"><span data-stu-id="1bbe8-177">![New Authentication Profile](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795000.png "New Authentication Profile")</span></span>

11. <span data-ttu-id="1bbe8-178">I hello **autentiseringsprofil** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="1bbe8-178">In hello **Authentication Profile** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="1bbe8-179">![Autentiseringsprofil](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795001.png "autentiseringsprofil")</span><span class="sxs-lookup"><span data-stu-id="1bbe8-179">![Authentication Profile](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795001.png "Authentication Profile")</span></span>
   
    <span data-ttu-id="1bbe8-180">a.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-180">a.</span></span> <span data-ttu-id="1bbe8-181">I hello **beskrivning** textruta, ange ett namn för din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-181">In hello **Description** textbox, type a name for your configuration.</span></span>
   
    <span data-ttu-id="1bbe8-182">b.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-182">b.</span></span> <span data-ttu-id="1bbe8-183">Välj **genomdriva SAML-autentisering för Mimecast Personal Portal**.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-183">Select **Enforce SAML Authentication for Mimecast Personal Portal**.</span></span>
   
    <span data-ttu-id="1bbe8-184">c.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-184">c.</span></span> <span data-ttu-id="1bbe8-185">Som **Provider**väljer **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-185">As **Provider**, select **Azure Active Directory**.</span></span>
   
    <span data-ttu-id="1bbe8-186">d.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-186">d.</span></span> <span data-ttu-id="1bbe8-187">I **utfärdar-URL** textruta klistra in hello värdet för **SAML enhets-ID** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-187">In **Issuer URL** textbox, paste hello value of **SAML Entity ID** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="1bbe8-188">e.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-188">e.</span></span> <span data-ttu-id="1bbe8-189">I **inloggnings-URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-189">In **Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="1bbe8-190">f.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-190">f.</span></span> <span data-ttu-id="1bbe8-191">I **logga ut URL** textruta klistra in hello värdet för **Sign-Out URL** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-191">In **Logout URL** textbox, paste hello value of **Sign-Out URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="1bbe8-192">g.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-192">g.</span></span> <span data-ttu-id="1bbe8-193">Öppna din **Base64-** kodat certifikat i anteckningar som hämtas från Azure-portalen kopiera hello innehållet i den i Urklipp, och klistra in den toohello **identitetscertifikat Provider (Metadata)** textruta.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-193">Open your **base-64** encoded certificate in notepad downloaded from Azure portal, copy hello content of it into your clipboard, and then paste it toohello **Identity Provider Certificate (Metadata)** textbox.</span></span>

    <span data-ttu-id="1bbe8-194">h.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-194">h.</span></span> <span data-ttu-id="1bbe8-195">Välj **tillåta enkel inloggning på**.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-195">Select **Allow Single Sign On**.</span></span>
   
    <span data-ttu-id="1bbe8-196">Jag.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-196">i.</span></span> <span data-ttu-id="1bbe8-197">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-197">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="1bbe8-198">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="1bbe8-198">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="1bbe8-199">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-199">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="1bbe8-200">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="1bbe8-200">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="1bbe8-201">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="1bbe8-201">Creating an Azure AD test user</span></span>
<span data-ttu-id="1bbe8-202">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-202">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="1bbe8-204">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="1bbe8-204">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="1bbe8-205">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-205">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mimecast-personal-portal-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="1bbe8-207">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-207">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mimecast-personal-portal-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="1bbe8-209">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-209">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mimecast-personal-portal-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="1bbe8-211">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="1bbe8-211">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mimecast-personal-portal-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="1bbe8-213">a.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-213">a.</span></span> <span data-ttu-id="1bbe8-214">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-214">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="1bbe8-215">b.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-215">b.</span></span> <span data-ttu-id="1bbe8-216">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-216">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="1bbe8-217">c.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-217">c.</span></span> <span data-ttu-id="1bbe8-218">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-218">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="1bbe8-219">d.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-219">d.</span></span> <span data-ttu-id="1bbe8-220">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-220">Click **Create**.</span></span>
 
### <a name="creating-a-mimecast-personal-portal-test-user"></a><span data-ttu-id="1bbe8-221">Skapa en testanvändare Mimecast personliga Portal</span><span class="sxs-lookup"><span data-stu-id="1bbe8-221">Creating a Mimecast Personal Portal test user</span></span>

<span data-ttu-id="1bbe8-222">I ordning tooenable Azure AD-användare toolog till Mimecast personliga Portal, måste de etableras i Mimecast personliga Portal.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-222">In order tooenable Azure AD users toolog into Mimecast Personal Portal, they must be provisioned into Mimecast Personal Portal.</span></span> <span data-ttu-id="1bbe8-223">Hello gäller Mimecast personliga Portal är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-223">In hello case of Mimecast Personal Portal, provisioning is a manual task.</span></span>

<span data-ttu-id="1bbe8-224">Du måste tooregister en domän innan du kan skapa användare.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-224">You need tooregister a domain before you can create users.</span></span>

<span data-ttu-id="1bbe8-225">**tooconfigure användaretablering, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="1bbe8-225">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="1bbe8-226">Logga in tooyour **Mimecast personliga Portal** som administratör.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-226">Sign on tooyour **Mimecast Personal Portal** as administrator.</span></span>

2. <span data-ttu-id="1bbe8-227">Gå för**kataloger \> internt**.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-227">Go too**Directories \> Internal**.</span></span>
   
    <span data-ttu-id="1bbe8-228">![Kataloger](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795003.png "kataloger")</span><span class="sxs-lookup"><span data-stu-id="1bbe8-228">![Directories](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795003.png "Directories")</span></span>

3. <span data-ttu-id="1bbe8-229">Klicka på **registrera nya domän**.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-229">Click **Register New Domain**.</span></span>
   
    <span data-ttu-id="1bbe8-230">![Registrera en ny domän](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795004.png "registrera ny domän")</span><span class="sxs-lookup"><span data-stu-id="1bbe8-230">![Register New Domain](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795004.png "Register New Domain")</span></span>

4. <span data-ttu-id="1bbe8-231">När den nya domänen har skapats, klickar du på **ny adress**.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-231">After your new domain has been created, click **New Address**.</span></span>
   
    <span data-ttu-id="1bbe8-232">![Ny adress](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795005.png "ny adress")</span><span class="sxs-lookup"><span data-stu-id="1bbe8-232">![New Address](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795005.png "New Address")</span></span>

5. <span data-ttu-id="1bbe8-233">Hello nya adress dialog, utföra hello följa stegen i ett giltigt Azure AD-kontot som du vill tooprovision:</span><span class="sxs-lookup"><span data-stu-id="1bbe8-233">In hello new address dialog, perform hello following steps of a valid Azure AD account you want tooprovision:</span></span>
   
    <span data-ttu-id="1bbe8-234">![Spara](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795006.png "spara")</span><span class="sxs-lookup"><span data-stu-id="1bbe8-234">![Save](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795006.png "Save")</span></span>
   
    <span data-ttu-id="1bbe8-235">a.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-235">a.</span></span> <span data-ttu-id="1bbe8-236">I hello **e-postadress** textruta typen **e-postadress** för hello användare som  **BrittaSimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="1bbe8-236">In hello **Email Address** textbox, type **Email Address** of hello user as **BrittaSimon@contoso.com**.</span></span>
    
    <span data-ttu-id="1bbe8-237">b.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-237">b.</span></span> <span data-ttu-id="1bbe8-238">I hello **globalt namn** textruta typen hello **användarnamn** som **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-238">In hello **Global Name** textbox, type hello **username** as **BrittaSimon**.</span></span>

    <span data-ttu-id="1bbe8-239">c.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-239">c.</span></span> <span data-ttu-id="1bbe8-240">I hello **lösenord**, och **Bekräfta lösenord** textrutor, typen hello **lösenord** för hello användare.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-240">In hello **Password**, and **Confirm Password** textboxes, type hello **Password** of hello user.</span></span>
   
    <span data-ttu-id="1bbe8-241">b.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-241">b.</span></span> <span data-ttu-id="1bbe8-242">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-242">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="1bbe8-243">Du kan använda andra Mimecast personliga Portal användare skapa verktyg eller API: er som tillhandahålls av Mimecast personliga Portal tooprovision Azure AD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-243">You can use any other Mimecast Personal Portal user account creation tools or APIs provided by Mimecast Personal Portal tooprovision Azure AD user accounts.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="1bbe8-244">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="1bbe8-244">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="1bbe8-245">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooMimecast personliga Portal.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-245">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooMimecast Personal Portal.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="1bbe8-247">**tooassign Britta Simon tooMimecast personliga Portal utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="1bbe8-247">**tooassign Britta Simon tooMimecast Personal Portal, perform hello following steps:**</span></span>

1. <span data-ttu-id="1bbe8-248">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-248">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="1bbe8-250">Välj i listan med program hello **Mimecast personliga Portal**.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-250">In hello applications list, select **Mimecast Personal Portal**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_app.png) 

3. <span data-ttu-id="1bbe8-252">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-252">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="1bbe8-254">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-254">Click **Add** button.</span></span> <span data-ttu-id="1bbe8-255">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-255">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="1bbe8-257">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-257">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="1bbe8-258">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-258">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="1bbe8-259">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-259">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="1bbe8-260">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1bbe8-260">Testing single sign-on</span></span>
<span data-ttu-id="1bbe8-261">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-261">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="1bbe8-262">Du bör få automatiskt inloggade tooyour Mimecast personliga portalprogram när du klickar på hello Mimecast personliga Portal-panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="1bbe8-262">When you click hello Mimecast Personal Portal tile in hello Access Panel, you should get automatically signed-on tooyour Mimecast Personal Portal application.</span></span> <span data-ttu-id="1bbe8-263">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="1bbe8-263">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1bbe8-264">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="1bbe8-264">Additional resources</span></span>

* [<span data-ttu-id="1bbe8-265">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1bbe8-265">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="1bbe8-266">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="1bbe8-266">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_203.png

