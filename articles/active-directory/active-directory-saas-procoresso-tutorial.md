---
title: "Självstudier: Azure Active Directory-integrering med Procore SSO | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Procore enkel inloggning."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9818edd3-48c0-411d-b05a-3ec805eafb2e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: jeedes
ms.openlocfilehash: bb918617f18ba3f44ddde469e6fc411977752f15
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-procore-sso"></a><span data-ttu-id="cfb07-103">Självstudier: Azure Active Directory-integrering med Procore enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cfb07-103">Tutorial: Azure Active Directory integration with Procore SSO</span></span>

<span data-ttu-id="cfb07-104">I kursen får du lära dig hur toointegrate Procore enkel inloggning med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="cfb07-104">In this tutorial, you learn how toointegrate Procore SSO with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="cfb07-105">Integrera Procore enkel inloggning med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="cfb07-105">Integrating Procore SSO with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="cfb07-106">Du kan styra i Azure AD som har åtkomst tooProcore enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cfb07-106">You can control in Azure AD who has access tooProcore SSO</span></span>
- <span data-ttu-id="cfb07-107">Du kan aktivera din användare tooautomatically get inloggade tooProcore SSO (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="cfb07-107">You can enable your users tooautomatically get signed-on tooProcore SSO (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="cfb07-108">Du kan hantera dina konton i en central plats - hello Azure Management portal</span><span class="sxs-lookup"><span data-stu-id="cfb07-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="cfb07-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="cfb07-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

<!--## Overview

tooenable single sign-on with Procore SSO, it must be configured toouse Azure Active Directory as an identity provider. This guide provides information and tips on how tooperform this configuration in Procore SSO.

>[!Note]: 
>This embedded guide is brand new in hello new Azure portal, and we’d love toohear your thoughts. Use hello Feedback ? button at hello top of hello portal tooprovide feedback. hello older guide for using hello [Azure classic portal](https://manage.windowsazure.com) tooconfigure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a><span data-ttu-id="cfb07-110">Krav</span><span class="sxs-lookup"><span data-stu-id="cfb07-110">Prerequisites</span></span>

<span data-ttu-id="cfb07-111">tooconfigure Azure AD-integrering med Procore enkel inloggning måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="cfb07-111">tooconfigure Azure AD integration with Procore SSO, you need hello following items:</span></span>

- <span data-ttu-id="cfb07-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="cfb07-112">An Azure AD subscription</span></span>
- <span data-ttu-id="cfb07-113">En Procore SSO enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="cfb07-113">A Procore SSO single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="cfb07-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="cfb07-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="cfb07-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="cfb07-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="cfb07-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="cfb07-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="cfb07-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="cfb07-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="cfb07-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="cfb07-118">Scenario description</span></span>
<span data-ttu-id="cfb07-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="cfb07-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="cfb07-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="cfb07-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="cfb07-121">Att lägga till Procore SSO från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="cfb07-121">Adding Procore SSO from hello gallery</span></span>
2. <span data-ttu-id="cfb07-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cfb07-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-procore-sso-from-hello-gallery"></a><span data-ttu-id="cfb07-123">Att lägga till Procore SSO från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="cfb07-123">Adding Procore SSO from hello gallery</span></span>
<span data-ttu-id="cfb07-124">tooconfigure hello integrering av Procore SSO i Azure AD, behöver du tooadd Procore SSO hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="cfb07-124">tooconfigure hello integration of Procore SSO into Azure AD, you need tooadd Procore SSO from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="cfb07-125">**tooadd Procore SSO från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="cfb07-125">**tooadd Procore SSO from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="cfb07-126">I hello  **[Azure-hanteringsportalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="cfb07-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="cfb07-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="cfb07-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="cfb07-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="cfb07-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="cfb07-131">Klicka på **Lägg till** hello längst upp i hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cfb07-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="cfb07-133">Skriv i sökrutan hello **Procore SSO**.</span><span class="sxs-lookup"><span data-stu-id="cfb07-133">In hello search box, type **Procore SSO**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_search.png)

5. <span data-ttu-id="cfb07-135">Markera hello resultat på panelen **Procore SSO**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="cfb07-135">In hello results panel, select **Procore SSO**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="cfb07-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cfb07-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="cfb07-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Procore SSO baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="cfb07-138">In this section, you configure and test Azure AD single sign-on with Procore SSO based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="cfb07-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren Procore SSO är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cfb07-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Procore SSO is tooa user in Azure AD.</span></span> <span data-ttu-id="cfb07-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren Procore SSO toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="cfb07-140">In other words, a link relationship between an Azure AD user and hello related user in Procore SSO needs toobe established.</span></span>

<span data-ttu-id="cfb07-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** Procore sso.</span><span class="sxs-lookup"><span data-stu-id="cfb07-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Procore SSO.</span></span>

<span data-ttu-id="cfb07-142">tooconfigure och testa Azure AD enkel inloggning med Procore enkel inloggning, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="cfb07-142">tooconfigure and test Azure AD single sign-on with Procore SSO, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="cfb07-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="cfb07-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="cfb07-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cfb07-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="cfb07-145">**[Skapa en testanvändare Procore SSO](#creating-a-procore-sso-test-user)**  -toohave en motsvarighet för Britta Simon Procore SSO som är länkade toohello Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="cfb07-145">**[Creating a Procore SSO test user](#creating-a-procore-sso-test-user)** - toohave a counterpart of Britta Simon in Procore SSO that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="cfb07-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="cfb07-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="cfb07-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="cfb07-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="cfb07-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cfb07-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="cfb07-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure Management portal och konfigurera enkel inloggning i ditt Procore SSO-program.</span><span class="sxs-lookup"><span data-stu-id="cfb07-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your Procore SSO application.</span></span>

<span data-ttu-id="cfb07-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Procore SSO:**</span><span class="sxs-lookup"><span data-stu-id="cfb07-150">**tooconfigure Azure AD single sign-on with Procore SSO, perform hello following steps:**</span></span>

1. <span data-ttu-id="cfb07-151">I hello Azure Management portal på hello **Procore SSO** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="cfb07-151">In hello Azure Management portal, on hello **Procore SSO** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="cfb07-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="cfb07-153">On hello **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_samlbase.png)

3. <span data-ttu-id="cfb07-155">På hello **Procore domän för SSO och URL: er** avsnittet, hello användaren har inte tooperform alla steg som hello app redan redan är integrerade med Azure.</span><span class="sxs-lookup"><span data-stu-id="cfb07-155">On hello **Procore SSO Domain and URLs** section, hello user does not have tooperform any steps as hello app is already pre-integrated with Azure.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_url.png)

4. <span data-ttu-id="cfb07-157">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara hello XML-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="cfb07-157">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_certificate.png) 

5. <span data-ttu-id="cfb07-159">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="cfb07-159">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-procoresso-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="cfb07-161">På hello **Procore SSO Configuration** klickar du på **Konfigurera enkel inloggning för Procore** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="cfb07-161">On hello **Procore SSO Configuration** section, click **Configure Procore SSO** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="cfb07-162">Kopiera hello **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="cfb07-162">Copy hello **SAML Entity ID and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_configure.png) 

7. <span data-ttu-id="cfb07-164">tooconfigure enkel inloggning på **Procore SSO** sida, inloggning tooyour procore företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="cfb07-164">tooconfigure single sign-on on **Procore SSO** side, login tooyour procore company site as an administrator.</span></span>

8. <span data-ttu-id="cfb07-165">Hello verktygslådan listmenyn nedåt, klickar du på **Admin** tooopen hello SSO inställningssidan.</span><span class="sxs-lookup"><span data-stu-id="cfb07-165">From hello toolbox drop down, click on **Admin** tooopen hello SSO settings page.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-procoresso-tutorial/procore_tool_admin.png)

9. <span data-ttu-id="cfb07-167">Klistra in hello värden i hello rutor som beskrivs nedan-</span><span class="sxs-lookup"><span data-stu-id="cfb07-167">Paste hello values in hello boxes as described below-</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-procoresso-tutorial/procore_setting_admin.png)    

    <span data-ttu-id="cfb07-169">a.</span><span class="sxs-lookup"><span data-stu-id="cfb07-169">a.</span></span> <span data-ttu-id="cfb07-170">I hello **enkel inloggning på utfärdar-URL** klistra in hello SAML enhets-ID som kopieras från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="cfb07-170">In hello **Single Sign On Issuer URL** box, paste hello SAML Entity ID copied from hello Azure portal.</span></span>

    <span data-ttu-id="cfb07-171">b.</span><span class="sxs-lookup"><span data-stu-id="cfb07-171">b.</span></span> <span data-ttu-id="cfb07-172">I hello **SAML-inloggning på mål-URL** klistra in hello SAML inloggning tjänst-URL för enkel kopieras från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="cfb07-172">In hello **SAML Sign On Target URL** box, paste hello SAML Single Sign-On Service URL copied from hello Azure portal.</span></span>

    <span data-ttu-id="cfb07-173">c.</span><span class="sxs-lookup"><span data-stu-id="cfb07-173">c.</span></span> <span data-ttu-id="cfb07-174">Öppna hello **XML-Metadata för** hämtade ovan från hello Azure-portalen och kopiera hello certifkatets i hello-tagg med namnet **X509Certificate**.</span><span class="sxs-lookup"><span data-stu-id="cfb07-174">Now open hello **Metadata XML** downloaded above from hello Azure portal and copy hello certficate in hello tag named **X509Certificate**.</span></span> <span data-ttu-id="cfb07-175">Klistra in hello kopieras värdet till hello **enkel inloggning x509 certifikat** rutan.</span><span class="sxs-lookup"><span data-stu-id="cfb07-175">Paste hello copied value into hello **Single Sign On x509 Certificate** box.</span></span>

10. <span data-ttu-id="cfb07-176">Klicka på **spara ändringar**.</span><span class="sxs-lookup"><span data-stu-id="cfb07-176">Click on **Save Changes**.</span></span>

11. <span data-ttu-id="cfb07-177">När dessa inställningar måste du toosend hello **domännamn** (t.ex **contoso.com**) via som du loggar in Procore toohello [Procore supportteamet](https://support.procore.com/) och de kommer Aktivera federerad enkel inloggning för domänen.</span><span class="sxs-lookup"><span data-stu-id="cfb07-177">After these settings, you needs toosend hello **domain name** (e.g **contoso.com**) through which you are logging into Procore toohello [Procore Support team](https://support.procore.com/) and they will activate federated SSO for that domain.</span></span>

<!--### Next steps

tooensure users can sign-in tooProcore SSO after it has been configured toouse Azure Active Directory, review hello following tasks and topics:

- User accounts must be pre-provisioned into Procore SSO prior toosign-in. tooset this up, see Provisioning.
 
- Users must be assigned access tooProcore SSO in Azure AD toosign-in. tooassign users, see Users.
 
- tooconfigure access polices for Procore SSO users, see Access Policies.
 
- For additional information on deploying single sign-on toousers, see [this article](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="cfb07-178">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="cfb07-178">Creating an Azure AD test user</span></span>
<span data-ttu-id="cfb07-179">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure Management portal kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cfb07-179">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="cfb07-181">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="cfb07-181">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="cfb07-182">I hello **Azure-hanteringsportalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="cfb07-182">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-procoresso-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="cfb07-184">Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.</span><span class="sxs-lookup"><span data-stu-id="cfb07-184">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-procoresso-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="cfb07-186">Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cfb07-186">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-procoresso-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="cfb07-188">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="cfb07-188">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-procoresso-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="cfb07-190">a.</span><span class="sxs-lookup"><span data-stu-id="cfb07-190">a.</span></span> <span data-ttu-id="cfb07-191">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="cfb07-191">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="cfb07-192">b.</span><span class="sxs-lookup"><span data-stu-id="cfb07-192">b.</span></span> <span data-ttu-id="cfb07-193">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="cfb07-193">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="cfb07-194">c.</span><span class="sxs-lookup"><span data-stu-id="cfb07-194">c.</span></span> <span data-ttu-id="cfb07-195">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="cfb07-195">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="cfb07-196">d.</span><span class="sxs-lookup"><span data-stu-id="cfb07-196">d.</span></span> <span data-ttu-id="cfb07-197">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="cfb07-197">Click **Create**.</span></span>
 
### <a name="creating-a-procore-sso-test-user"></a><span data-ttu-id="cfb07-198">Skapa en testanvändare Procore SSO</span><span class="sxs-lookup"><span data-stu-id="cfb07-198">Creating a Procore SSO test user</span></span>

<span data-ttu-id="cfb07-199">Följ hello under steg toocreate en Procore testanvändare på sidan.</span><span class="sxs-lookup"><span data-stu-id="cfb07-199">Please follow hello below steps toocreate a Procore test user on their side.</span></span>

1. <span data-ttu-id="cfb07-200">Inloggningen tooyour procore företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="cfb07-200">Login tooyour procore company site as an administrator.</span></span>  

2. <span data-ttu-id="cfb07-201">Hello verktygslådan listmenyn nedåt, klickar du på **Directory** tooopen hello företagets katalogsidan.</span><span class="sxs-lookup"><span data-stu-id="cfb07-201">From hello toolbox drop down, click on **Directory** tooopen hello company directory page.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-procoresso-tutorial/Procore_sso_directory.png)

3. <span data-ttu-id="cfb07-203">Klicka på **lägga till en Person** tooopen hello formuläret och ange utföra följande alternativ -</span><span class="sxs-lookup"><span data-stu-id="cfb07-203">Click on **Add a Person** option tooopen hello form and enter perform following options -</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-procoresso-tutorial/Procore_user_add.png)

    <span data-ttu-id="cfb07-205">a.</span><span class="sxs-lookup"><span data-stu-id="cfb07-205">a.</span></span> <span data-ttu-id="cfb07-206">I hello **Förnamn** textruta typen användarens förnamn som **Britta**.</span><span class="sxs-lookup"><span data-stu-id="cfb07-206">In hello **First Name** textbox, type user's first name like **Britta**.</span></span>

    <span data-ttu-id="cfb07-207">b.</span><span class="sxs-lookup"><span data-stu-id="cfb07-207">b.</span></span> <span data-ttu-id="cfb07-208">I hello **efternamn** textruta typen användarens efternamn som **Simon**.</span><span class="sxs-lookup"><span data-stu-id="cfb07-208">In hello **Last name** textbox, type user's last name like **Simon**.</span></span>

    <span data-ttu-id="cfb07-209">c.</span><span class="sxs-lookup"><span data-stu-id="cfb07-209">c.</span></span> <span data-ttu-id="cfb07-210">I hello **e-postadress** textruta typen användarens e-postadress som  **BrittaSimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="cfb07-210">In hello **Email Address** textbox, type user's email address like **BrittaSimon@contoso.com**.</span></span>

    <span data-ttu-id="cfb07-211">d.</span><span class="sxs-lookup"><span data-stu-id="cfb07-211">d.</span></span> <span data-ttu-id="cfb07-212">Välj **behörighet mallen** som **tillämpa behörighet mall senare**.</span><span class="sxs-lookup"><span data-stu-id="cfb07-212">Select **Permission Template** as **Apply Permission Template Later**.</span></span>

    <span data-ttu-id="cfb07-213">e.</span><span class="sxs-lookup"><span data-stu-id="cfb07-213">e.</span></span> <span data-ttu-id="cfb07-214">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="cfb07-214">Click **Create**.</span></span>

4. <span data-ttu-id="cfb07-215">Kontrollera och uppdatera hello information för hello nyligen tillagda kontakta.</span><span class="sxs-lookup"><span data-stu-id="cfb07-215">Check and update hello details for hello newly added contact.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-procoresso-tutorial/Procore_user_check.png)

5. <span data-ttu-id="cfb07-217">Klicka på **spara och skicka Invitiation** (om det krävs en inbjudan via e-post) eller **spara** (spara direkt) toocomplete hello användarregistrering.</span><span class="sxs-lookup"><span data-stu-id="cfb07-217">Click on **Save and Send Invitiation** (if an invite through mail is required) or **Save** (Save directly) toocomplete hello user registration.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-procoresso-tutorial/Procore_user_save.png)    

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="cfb07-219">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="cfb07-219">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="cfb07-220">I det här avsnittet Aktivera Britta Simon toouse Azure enkel inloggning genom att ge sina access tooProcore enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="cfb07-220">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooProcore SSO.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="cfb07-222">**tooassign Britta Simon tooProcore SSO utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="cfb07-222">**tooassign Britta Simon tooProcore SSO, perform hello following steps:**</span></span>

1. <span data-ttu-id="cfb07-223">I hello Azure Management portal öppnar du hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="cfb07-223">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="cfb07-225">Välj i listan med program hello **Procore SSO**.</span><span class="sxs-lookup"><span data-stu-id="cfb07-225">In hello applications list, select **Procore SSO**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_app.png) 

3. <span data-ttu-id="cfb07-227">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="cfb07-227">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="cfb07-229">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="cfb07-229">Click **Add** button.</span></span> <span data-ttu-id="cfb07-230">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cfb07-230">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="cfb07-232">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="cfb07-232">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="cfb07-233">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cfb07-233">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="cfb07-234">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cfb07-234">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="cfb07-235">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cfb07-235">Testing single sign-on</span></span>

<span data-ttu-id="cfb07-236">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="cfb07-236">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="cfb07-237">Om du vill testa dina inställningar för enkel inloggning, öppna åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="cfb07-237">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="cfb07-238">Mer information om åtkomstpanelen finns [introduktion till åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="cfb07-238">For more details about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> <span data-ttu-id="cfb07-239">När du klickar på hello Procore SSO-panelen i hello åtkomstpanelen får automatiskt inloggade tooyour Procore SSO-program.</span><span class="sxs-lookup"><span data-stu-id="cfb07-239">When you click hello Procore SSO tile in hello Access Panel, you should get automatically signed-on tooyour Procore SSO application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cfb07-240">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="cfb07-240">Additional resources</span></span>

* [<span data-ttu-id="cfb07-241">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="cfb07-241">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="cfb07-242">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="cfb07-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_203.png

