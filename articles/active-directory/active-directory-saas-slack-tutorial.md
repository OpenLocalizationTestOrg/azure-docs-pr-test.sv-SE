---
title: "Självstudier: Azure Active Directory-integrering med Slack | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Slack."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ffc5e73f-6c38-4bbb-876a-a7dd269d4e1c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 7f0151401af4dc63d2f714d4b4f66380c4b51e0d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-slack"></a><span data-ttu-id="26833-103">Självstudier: Azure Active Directory-integrering med Slack</span><span class="sxs-lookup"><span data-stu-id="26833-103">Tutorial: Azure Active Directory integration with Slack</span></span>

<span data-ttu-id="26833-104">I kursen får du lära dig hur toointegrate Slack med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="26833-104">In this tutorial, you learn how toointegrate Slack with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="26833-105">Integrera Slack med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="26833-105">Integrating Slack with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="26833-106">Du kan styra i Azure AD som har åtkomst till tooSlack</span><span class="sxs-lookup"><span data-stu-id="26833-106">You can control in Azure AD who has access tooSlack</span></span>
- <span data-ttu-id="26833-107">Du kan aktivera din användare tooautomatically get inloggade tooSlack (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="26833-107">You can enable your users tooautomatically get signed-on tooSlack (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="26833-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="26833-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="26833-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="26833-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="26833-110">Krav</span><span class="sxs-lookup"><span data-stu-id="26833-110">Prerequisites</span></span>

<span data-ttu-id="26833-111">tooconfigure Azure AD-integrering med Slack måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="26833-111">tooconfigure Azure AD integration with Slack, you need hello following items:</span></span>

- <span data-ttu-id="26833-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="26833-112">An Azure AD subscription</span></span>
- <span data-ttu-id="26833-113">En Slack enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="26833-113">A Slack single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="26833-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="26833-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="26833-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="26833-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="26833-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="26833-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="26833-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="26833-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="26833-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="26833-118">Scenario description</span></span>
<span data-ttu-id="26833-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="26833-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="26833-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="26833-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="26833-121">Att lägga till Slack från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="26833-121">Adding Slack from hello gallery</span></span>
2. <span data-ttu-id="26833-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="26833-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-slack-from-hello-gallery"></a><span data-ttu-id="26833-123">Att lägga till Slack från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="26833-123">Adding Slack from hello gallery</span></span>
<span data-ttu-id="26833-124">tooconfigure hello integration slack till Azure AD, behöver du tooadd Slack hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="26833-124">tooconfigure hello integration of Slack into Azure AD, you need tooadd Slack from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="26833-125">**tooadd Slack från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="26833-125">**tooadd Slack from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="26833-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="26833-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="26833-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="26833-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="26833-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="26833-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="26833-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="26833-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="26833-133">Skriv i sökrutan hello **Slack**.</span><span class="sxs-lookup"><span data-stu-id="26833-133">In hello search box, type **Slack**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-slack-tutorial/tutorial_slack_search.png)

5. <span data-ttu-id="26833-135">Markera hello resultat på panelen **Slack**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="26833-135">In hello results panel, select **Slack**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-slack-tutorial/tutorial_slack_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="26833-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="26833-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="26833-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Slack baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="26833-138">In this section, you configure and test Azure AD single sign-on with Slack based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="26833-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Slack är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="26833-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Slack is tooa user in Azure AD.</span></span> <span data-ttu-id="26833-140">Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i Slack toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="26833-140">In other words, a link relationship between an Azure AD user and hello related user in Slack needs toobe established.</span></span>

<span data-ttu-id="26833-141">I Slack, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="26833-141">In Slack, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="26833-142">tooconfigure och testa Azure AD enkel inloggning med Slack, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="26833-142">tooconfigure and test Azure AD single sign-on with Slack, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="26833-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="26833-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="26833-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="26833-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="26833-145">**[Skapa en Slack testanvändare](#creating-a-slack-test-user)**  -toohave en motsvarighet för Britta Simon i Slack som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="26833-145">**[Creating a Slack test user](#creating-a-slack-test-user)** - toohave a counterpart of Britta Simon in Slack that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="26833-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="26833-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="26833-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="26833-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="26833-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="26833-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="26833-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Slack.</span><span class="sxs-lookup"><span data-stu-id="26833-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Slack application.</span></span>

<span data-ttu-id="26833-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Slack:**</span><span class="sxs-lookup"><span data-stu-id="26833-150">**tooconfigure Azure AD single sign-on with Slack, perform hello following steps:**</span></span>

1. <span data-ttu-id="26833-151">I hello Azure-portalen på hello **Slack** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="26833-151">In hello Azure portal, on hello **Slack** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="26833-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="26833-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-slack-tutorial/tutorial_slack_samlbase.png)

3. <span data-ttu-id="26833-155">På hello **Slack domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="26833-155">On hello **Slack Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-slack-tutorial/tutorial_slack_url.png)

    <span data-ttu-id="26833-157">a.</span><span class="sxs-lookup"><span data-stu-id="26833-157">a.</span></span> <span data-ttu-id="26833-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.slack.com`</span><span class="sxs-lookup"><span data-stu-id="26833-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.slack.com`</span></span>

    <span data-ttu-id="26833-159">b.</span><span class="sxs-lookup"><span data-stu-id="26833-159">b.</span></span> <span data-ttu-id="26833-160">I hello **identifierare** textruta typen hello URL:`https://slack.com`</span><span class="sxs-lookup"><span data-stu-id="26833-160">In hello **Identifier** textbox, type hello URL: `https://slack.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="26833-161">hello-värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="26833-161">hello value is not real.</span></span> <span data-ttu-id="26833-162">Du har tooupdate hello-värde med hello faktiska logga URL.</span><span class="sxs-lookup"><span data-stu-id="26833-162">You have tooupdate hello value with hello actual Sign On URL.</span></span> <span data-ttu-id="26833-163">Kontakta [Slack supportteamet](https://slack.com/help/contact) tooget hello värde</span><span class="sxs-lookup"><span data-stu-id="26833-163">Contact [Slack support team](https://slack.com/help/contact) tooget hello value</span></span>
     
4. <span data-ttu-id="26833-164">Slack program förväntar hello SAML intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="26833-164">Slack application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="26833-165">Konfigurera hello följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="26833-165">Configure hello following claims for this application.</span></span> <span data-ttu-id="26833-166">Du kan hantera hello värden för attributen från hello ”**användarattribut**” avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="26833-166">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="26833-167">hello följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="26833-167">hello following screenshot shows an example for this.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-slack-tutorial/tutorial_slack_attribute.png)

5. <span data-ttu-id="26833-169">I hello **användarattribut** avsnittet hello **enkel inloggning** markerar **user.mail** som **användar-ID** och för varje rad som visas i hello tabellen nedan, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="26833-169">In hello **User Attributes** section on hello **Single sign-on** dialog, select **user.mail**  as **User Identifier** and for each row shown in hello table below, perform hello following steps:</span></span>
    
    | <span data-ttu-id="26833-170">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="26833-170">Attribute Name</span></span> | <span data-ttu-id="26833-171">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="26833-171">Attribute Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="26833-172">Förnamn</span><span class="sxs-lookup"><span data-stu-id="26833-172">first_name</span></span> | <span data-ttu-id="26833-173">User.givenName</span><span class="sxs-lookup"><span data-stu-id="26833-173">user.givenname</span></span> |
    | <span data-ttu-id="26833-174">Efternamn</span><span class="sxs-lookup"><span data-stu-id="26833-174">last_name</span></span> | <span data-ttu-id="26833-175">User.surname</span><span class="sxs-lookup"><span data-stu-id="26833-175">user.surname</span></span> |
    | <span data-ttu-id="26833-176">User.Email</span><span class="sxs-lookup"><span data-stu-id="26833-176">User.Email</span></span> | <span data-ttu-id="26833-177">User.Mail</span><span class="sxs-lookup"><span data-stu-id="26833-177">user.mail</span></span> |  
    | <span data-ttu-id="26833-178">User.Username</span><span class="sxs-lookup"><span data-stu-id="26833-178">User.Username</span></span> | <span data-ttu-id="26833-179">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="26833-179">user.userprincipalname</span></span> |

    <span data-ttu-id="26833-180">a.</span><span class="sxs-lookup"><span data-stu-id="26833-180">a.</span></span> <span data-ttu-id="26833-181">Klicka på **attributet** tooopen **Redigera attribut** dialogrutan rutan och utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="26833-181">Click on **Attribute** tooopen **Edit Attribute** dialog box and perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-slack-tutorial/tutorial_slack_attribute1.png)

    <span data-ttu-id="26833-183">a.</span><span class="sxs-lookup"><span data-stu-id="26833-183">a.</span></span> <span data-ttu-id="26833-184">I hello **namn** textruta hello attributnamn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="26833-184">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="26833-185">b.</span><span class="sxs-lookup"><span data-stu-id="26833-185">b.</span></span> <span data-ttu-id="26833-186">Från hello **värdet** listan, Välj hello-attributvärde som visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="26833-186">From hello **Value** list, select hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="26833-187">c.</span><span class="sxs-lookup"><span data-stu-id="26833-187">c.</span></span> <span data-ttu-id="26833-188">Klicka på **OK**</span><span class="sxs-lookup"><span data-stu-id="26833-188">Click **OK**</span></span>

6. <span data-ttu-id="26833-189">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="26833-189">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-slack-tutorial/tutorial_slack_certificate.png)

7. <span data-ttu-id="26833-191">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="26833-191">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-slack-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="26833-193">På hello **Slack Configuration** klickar du på **konfigurera Slack** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="26833-193">On hello **Slack Configuration** section, click **Configure Slack** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="26833-194">Kopiera hello **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="26833-194">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-slack-tutorial/tutorial_slack_configure.png) 

9.  <span data-ttu-id="26833-196">Logga in tooyour Slack företagets webbplats som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="26833-196">In a different web browser window, log in tooyour Slack company site as an administrator.</span></span>

10.  <span data-ttu-id="26833-197">Navigera för**Microsoft Azure AD** gå för**inställningar för Team**.</span><span class="sxs-lookup"><span data-stu-id="26833-197">Navigate too**Microsoft Azure AD** then go too**Team Settings**.</span></span>

     ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-slack-tutorial/tutorial_slack_001.png)

11.  <span data-ttu-id="26833-199">I hello **inställningar för Team** klickar du på hello **autentisering** fliken och klicka sedan på **ändra inställningar för**.</span><span class="sxs-lookup"><span data-stu-id="26833-199">In hello **Team Settings** section, click hello **Authentication** tab, and then click **Change Settings**.</span></span>

     ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-slack-tutorial/tutorial_slack_002.png)

12. <span data-ttu-id="26833-201">På hello **SAML autentiseringsinställningar** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="26833-201">On hello **SAML Authentication Settings** dialog, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-slack-tutorial/tutorial_slack_003.png)

    <span data-ttu-id="26833-203">a.</span><span class="sxs-lookup"><span data-stu-id="26833-203">a.</span></span>  <span data-ttu-id="26833-204">I hello **SAML 2.0 slutpunkt (HTTP)** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="26833-204">In hello **SAML 2.0 Endpoint (HTTP)** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="26833-205">b.</span><span class="sxs-lookup"><span data-stu-id="26833-205">b.</span></span>  <span data-ttu-id="26833-206">I hello **identitet providern utfärdaren** textruta klistra in hello värdet för **SAML enhets-ID**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="26833-206">In hello **Identity Provider Issuer** textbox, paste hello value of **SAML Entity ID**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="26833-207">c.</span><span class="sxs-lookup"><span data-stu-id="26833-207">c.</span></span>  <span data-ttu-id="26833-208">Öppna filen hämtat certifikat i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den toohello **certifikat med offentlig** textruta.</span><span class="sxs-lookup"><span data-stu-id="26833-208">Open your downloaded certificate file in notepad, copy hello content of it into your clipboard, and then paste it toohello **Public Certificate** textbox.</span></span>

    <span data-ttu-id="26833-209">d.</span><span class="sxs-lookup"><span data-stu-id="26833-209">d.</span></span> <span data-ttu-id="26833-210">Konfigurera hello tre inställningarna ovan som passar din Slack-teamet.</span><span class="sxs-lookup"><span data-stu-id="26833-210">Configure hello above three settings as appropriate for your Slack team.</span></span> <span data-ttu-id="26833-211">Mer information om inställningar för hello hitta hello **Slacks guiden för konfiguration av SSO** här.</span><span class="sxs-lookup"><span data-stu-id="26833-211">For more information about hello settings, please find hello **Slack's SSO configuration guide** here.</span></span> `https://get.slack.help/hc/articles/220403548-Guide-to-single-sign-on-with-Slack%60`

    <span data-ttu-id="26833-212">e.</span><span class="sxs-lookup"><span data-stu-id="26833-212">e.</span></span>  <span data-ttu-id="26833-213">Klicka på **spara konfigurationen**.</span><span class="sxs-lookup"><span data-stu-id="26833-213">Click **Save Configuration**.</span></span>
     
    <!-- Deselect **Allow users toochange their email address**.

    e.  Select **Allow users toochoose their own username**.

    f.  As **Authentication for your team must be used by**, select **It’s optional**. -->

> [!TIP]
> <span data-ttu-id="26833-214">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="26833-214">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="26833-215">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="26833-215">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="26833-216">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="26833-216">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="26833-217">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="26833-217">Creating an Azure AD test user</span></span>
<span data-ttu-id="26833-218">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="26833-218">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="26833-220">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="26833-220">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="26833-221">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="26833-221">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-slack-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="26833-223">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="26833-223">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-slack-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="26833-225">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="26833-225">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-slack-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="26833-227">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="26833-227">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-slack-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="26833-229">a.</span><span class="sxs-lookup"><span data-stu-id="26833-229">a.</span></span> <span data-ttu-id="26833-230">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="26833-230">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="26833-231">b.</span><span class="sxs-lookup"><span data-stu-id="26833-231">b.</span></span> <span data-ttu-id="26833-232">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="26833-232">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="26833-233">c.</span><span class="sxs-lookup"><span data-stu-id="26833-233">c.</span></span> <span data-ttu-id="26833-234">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="26833-234">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="26833-235">d.</span><span class="sxs-lookup"><span data-stu-id="26833-235">d.</span></span> <span data-ttu-id="26833-236">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="26833-236">Click **Create**.</span></span>
 
### <a name="creating-a-slack-test-user"></a><span data-ttu-id="26833-237">Skapa en Slack testanvändare</span><span class="sxs-lookup"><span data-stu-id="26833-237">Creating a Slack test user</span></span>

<span data-ttu-id="26833-238">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i Slack.</span><span class="sxs-lookup"><span data-stu-id="26833-238">hello objective of this section is toocreate a user called Britta Simon in Slack.</span></span> <span data-ttu-id="26833-239">Slack stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="26833-239">Slack supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="26833-240">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="26833-240">There is no action item for you in this section.</span></span> <span data-ttu-id="26833-241">En ny användare skapas under ett försök tooaccess Slack om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="26833-241">A new user is created during an attempt tooaccess Slack if it doesn't exist yet.</span></span>

> [!NOTE]
> <span data-ttu-id="26833-242">Om du behöver toocreate en användare manuellt, måste tooContact [Slack supportteamet](https://slack.com/help/contact).</span><span class="sxs-lookup"><span data-stu-id="26833-242">If you need toocreate a user manually, you need tooContact [Slack support team](https://slack.com/help/contact).</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="26833-243">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="26833-243">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="26833-244">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSlack.</span><span class="sxs-lookup"><span data-stu-id="26833-244">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSlack.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="26833-246">**tooassign Britta Simon tooSlack utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="26833-246">**tooassign Britta Simon tooSlack, perform hello following steps:**</span></span>

1. <span data-ttu-id="26833-247">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="26833-247">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="26833-249">Välj i listan med program hello **Slack**.</span><span class="sxs-lookup"><span data-stu-id="26833-249">In hello applications list, select **Slack**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-slack-tutorial/tutorial_slack_app.png) 

3. <span data-ttu-id="26833-251">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="26833-251">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="26833-253">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="26833-253">Click **Add** button.</span></span> <span data-ttu-id="26833-254">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="26833-254">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="26833-256">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="26833-256">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="26833-257">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="26833-257">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="26833-258">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="26833-258">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="26833-259">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="26833-259">Testing single sign-on</span></span>

<span data-ttu-id="26833-260">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="26833-260">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="26833-261">När du klickar på hello Slack-panelen i Hej åtkomstpanelen, du får automatiskt inloggade tooyour Slack program.</span><span class="sxs-lookup"><span data-stu-id="26833-261">When you click hello Slack tile in hello Access Panel, you should get automatically signed-on tooyour Slack application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="26833-262">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="26833-262">Additional resources</span></span>

* [<span data-ttu-id="26833-263">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="26833-263">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="26833-264">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="26833-264">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-slack-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-slack-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-slack-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-slack-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-slack-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-slack-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-slack-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-slack-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-slack-tutorial/tutorial_general_203.png

