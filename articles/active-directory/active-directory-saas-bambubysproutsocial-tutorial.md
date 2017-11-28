---
title: "Självstudier: Azure Active Directory-integrering med Bambu av brysselkål sociala | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Bambu av brysselkål sociala."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d2b9ddbc-cab7-40d6-aca1-5b171cab4199
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2017
ms.author: jeedes
ms.openlocfilehash: c9998772c032476f9a18054873022f55b4bb78be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bambu-by-sprout-social"></a><span data-ttu-id="613e4-103">Självstudier: Azure Active Directory-integrering med Bambu av brysselkål sociala</span><span class="sxs-lookup"><span data-stu-id="613e4-103">Tutorial: Azure Active Directory integration with Bambu by Sprout Social</span></span>

<span data-ttu-id="613e4-104">I kursen får du lära dig hur toointegrate Bambu av brysselkål sociala med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="613e4-104">In this tutorial, you learn how toointegrate Bambu by Sprout Social with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="613e4-105">Integrera Bambu av brysselkål sociala med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="613e4-105">Integrating Bambu by Sprout Social with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="613e4-106">Du kan styra i Azure AD som har åtkomst till tooBambu av brysselkål sociala</span><span class="sxs-lookup"><span data-stu-id="613e4-106">You can control in Azure AD who has access tooBambu by Sprout Social</span></span>
- <span data-ttu-id="613e4-107">Du kan aktivera din användare tooautomatically get inloggade tooBambu av brysselkål sociala (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="613e4-107">You can enable your users tooautomatically get signed-on tooBambu by Sprout Social (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="613e4-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="613e4-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="613e4-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="613e4-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

<!--## Overview

tooenable single sign-on with Bambu by Sprout Social, it must be configured toouse Azure Active Directory as an identity provider. This guide provides information and tips on how tooperform this configuration in Bambu by Sprout Social.

>[!Note]: 
>This embedded guide is brand new in hello new Azure portal, and we’d love toohear your thoughts. Use hello Feedback ? button at hello top of hello portal tooprovide feedback. hello older guide for using hello [Azure classic portal](https://manage.windowsazure.com) tooconfigure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a><span data-ttu-id="613e4-110">Krav</span><span class="sxs-lookup"><span data-stu-id="613e4-110">Prerequisites</span></span>

<span data-ttu-id="613e4-111">tooconfigure Azure AD-integrering med Bambu av brysselkål sociala måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="613e4-111">tooconfigure Azure AD integration with Bambu by Sprout Social, you need hello following items:</span></span>

- <span data-ttu-id="613e4-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="613e4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="613e4-113">En Bambu av brysselkål sociala enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="613e4-113">A Bambu by Sprout Social single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="613e4-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="613e4-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="613e4-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="613e4-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="613e4-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="613e4-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="613e4-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="613e4-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="613e4-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="613e4-118">Scenario description</span></span>
<span data-ttu-id="613e4-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="613e4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="613e4-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="613e4-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="613e4-121">Att lägga till Bambu av brysselkål sociala från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="613e4-121">Adding Bambu by Sprout Social from hello gallery</span></span>
2. <span data-ttu-id="613e4-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="613e4-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-bambu-by-sprout-social-from-hello-gallery"></a><span data-ttu-id="613e4-123">Att lägga till Bambu av brysselkål sociala från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="613e4-123">Adding Bambu by Sprout Social from hello gallery</span></span>
<span data-ttu-id="613e4-124">tooconfigure hello integrering av Bambu av brysselkål sociala i Azure AD, behöver du tooadd Bambu av brysselkål sociala hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="613e4-124">tooconfigure hello integration of Bambu by Sprout Social into Azure AD, you need tooadd Bambu by Sprout Social from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="613e4-125">**tooadd Bambu av brysselkål sociala från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="613e4-125">**tooadd Bambu by Sprout Social from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="613e4-126">I hello  **[Azure Portal](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="613e4-126">In hello **[Azure Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="613e4-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="613e4-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="613e4-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="613e4-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="613e4-131">Klicka på **nytt program** hello längst upp i hello dialogrutan tooadd nytt program.</span><span class="sxs-lookup"><span data-stu-id="613e4-131">Click **New application** button on hello top of hello dialog tooadd new application.</span></span>

    ![Program][3]

4. <span data-ttu-id="613e4-133">Skriv i sökrutan hello **Bambu av brysselkål sociala**.</span><span class="sxs-lookup"><span data-stu-id="613e4-133">In hello search box, type **Bambu by Sprout Social**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_bambubysproutsocial_search.png)

5. <span data-ttu-id="613e4-135">Markera hello resultat på panelen **Bambu av brysselkål sociala**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="613e4-135">In hello results panel, select **Bambu by Sprout Social**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_bambubysproutsocial_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="613e4-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="613e4-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="613e4-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Bambu av brysselkål sociala baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="613e4-138">In this section, you configure and test Azure AD single sign-on with Bambu by Sprout Social based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="613e4-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Bambu av brysselkål sociala är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="613e4-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Bambu by Sprout Social is tooa user in Azure AD.</span></span> <span data-ttu-id="613e4-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Bambu av brysselkål sociala toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="613e4-140">In other words, a link relationship between an Azure AD user and hello related user in Bambu by Sprout Social needs toobe established.</span></span>

<span data-ttu-id="613e4-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Bambu av brysselkål sociala.</span><span class="sxs-lookup"><span data-stu-id="613e4-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Bambu by Sprout Social.</span></span>

<span data-ttu-id="613e4-142">tooconfigure och testa Azure AD enkel inloggning med Bambu av brysselkål sociala, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="613e4-142">tooconfigure and test Azure AD single sign-on with Bambu by Sprout Social, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="613e4-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="613e4-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="613e4-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="613e4-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="613e4-145">**[Skapa en Bambu av brysselkål sociala testanvändare](#creating-a-bambu-by-sprout-social-test-user)**  -toohave en motsvarighet för Britta Simon i Bambu av brysselkål sociala som är länkade toohello Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="613e4-145">**[Creating a Bambu by Sprout Social test user](#creating-a-bambu-by-sprout-social-test-user)** - toohave a counterpart of Britta Simon in Bambu by Sprout Social that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="613e4-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="613e4-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="613e4-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="613e4-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="613e4-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="613e4-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="613e4-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i din Bambu av brysselkål sociala program.</span><span class="sxs-lookup"><span data-stu-id="613e4-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Bambu by Sprout Social application.</span></span>

<span data-ttu-id="613e4-150">**tooconfigure Azure AD enkel inloggning med Bambu av brysselkål sociala utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="613e4-150">**tooconfigure Azure AD single sign-on with Bambu by Sprout Social, perform hello following steps:**</span></span>

1. <span data-ttu-id="613e4-151">I hello Azure-portalen på hello **Bambu av brysselkål sociala** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="613e4-151">In hello Azure portal, on hello **Bambu by Sprout Social** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="613e4-153">På hello **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** tooenable för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="613e4-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_bambubysproutsocial_samlbase.png)

3. <span data-ttu-id="613e4-155">På hello **Bambu brysselkål sociala domänen och URL: er** avsnittet, hello användaren har inte tooperform alla steg som hello app redan redan är integrerade med Azure.</span><span class="sxs-lookup"><span data-stu-id="613e4-155">On hello **Bambu by Sprout Social Domain and URLs** section, hello user does not have tooperform any steps as hello app is already pre-integrated with Azure.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_bambubysproutsocial_url.png)

4. <span data-ttu-id="613e4-157">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara hello XML-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="613e4-157">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_bambubysproutsocial_certificate.png) 

5. <span data-ttu-id="613e4-159">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="613e4-159">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="613e4-161">På hello **Bambu brysselkål sociala konfigurationen** klickar du på **konfigurera Bambu av brysselkål sociala** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="613e4-161">On hello **Bambu by Sprout Social Configuration** section, click **Configure Bambu by Sprout Social** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="613e4-162">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="613e4-162">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_bambubysproutsocial_configure.png) 

7. <span data-ttu-id="613e4-164">tooconfigure enkel inloggning på **Bambu av brysselkål sociala** sida, behöver du toosend hello hämtas **XML-Metadata för** och **SAML inloggning tjänst-URL för enkel** för[Bambu av brysselkål sociala support](mailto:support@getbambu.com).</span><span class="sxs-lookup"><span data-stu-id="613e4-164">tooconfigure single sign-on on **Bambu by Sprout Social** side, you need toosend hello downloaded **Metadata XML** and **SAML Single Sign-On Service URL** too[Bambu by Sprout Social support](mailto:support@getbambu.com).</span></span> <span data-ttu-id="613e4-165">De ska ställa in detta i ordning toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="613e4-165">They will set this up in order toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="613e4-166">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="613e4-166">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="613e4-167">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka på hello **enkel inloggning** fliken och åtkomst hello inbäddade dokumentationen via hello **Configuration** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="613e4-167">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click on hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="613e4-168">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="613e4-168">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

<!--### Next steps

tooensure users can sign-in tooBambu by Sprout Social after it has been configured toouse Azure Active Directory, review hello following tasks and topics:

- User accounts must be pre-provisioned into Bambu by Sprout Social prior toosign-in. tooset this up, see Provisioning.
 
- Users must be assigned access tooBambu by Sprout Social in Azure AD toosign-in. tooassign users, see Users.
 
- tooconfigure access polices for Bambu by Sprout Social users, see Access Policies.
 
- For additional information on deploying single sign-on toousers, see [this article](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="613e4-169">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="613e4-169">Creating an Azure AD test user</span></span>
<span data-ttu-id="613e4-170">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="613e4-170">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="613e4-172">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="613e4-172">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="613e4-173">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="613e4-173">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bambubysproutsocial-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="613e4-175">Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.</span><span class="sxs-lookup"><span data-stu-id="613e4-175">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bambubysproutsocial-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="613e4-177">Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="613e4-177">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bambubysproutsocial-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="613e4-179">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="613e4-179">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bambubysproutsocial-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="613e4-181">a.</span><span class="sxs-lookup"><span data-stu-id="613e4-181">a.</span></span> <span data-ttu-id="613e4-182">I hello **namn** textruta typen **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="613e4-182">In hello **Name** textbox, type **Britta Simon**.</span></span>

    <span data-ttu-id="613e4-183">b.</span><span class="sxs-lookup"><span data-stu-id="613e4-183">b.</span></span> <span data-ttu-id="613e4-184">I hello **användarnamn** textruta typen hello **e-postadress** av Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="613e4-184">In hello **User name** textbox, type hello **email address** of Britta Simon.</span></span>

    <span data-ttu-id="613e4-185">c.</span><span class="sxs-lookup"><span data-stu-id="613e4-185">c.</span></span> <span data-ttu-id="613e4-186">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="613e4-186">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="613e4-187">d.</span><span class="sxs-lookup"><span data-stu-id="613e4-187">d.</span></span> <span data-ttu-id="613e4-188">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="613e4-188">Click **Create**.</span></span>
 
### <a name="creating-a-bambu-by-sprout-social-test-user"></a><span data-ttu-id="613e4-189">Skapa en Bambu av brysselkål sociala testanvändare</span><span class="sxs-lookup"><span data-stu-id="613e4-189">Creating a Bambu by Sprout Social test user</span></span>

<span data-ttu-id="613e4-190">Programmet stöder bara i tid användaretablering och authentication-användare kommer att skapas i hello program automatiskt.</span><span class="sxs-lookup"><span data-stu-id="613e4-190">Application supports Just in time user provisioning and after authentication users will be created in hello application automatically.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="613e4-191">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="613e4-191">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="613e4-192">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att ge sina access tooBambu av brysselkål sociala.</span><span class="sxs-lookup"><span data-stu-id="613e4-192">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooBambu by Sprout Social.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="613e4-194">**tooassign Britta Simon tooBambu av brysselkål sociala utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="613e4-194">**tooassign Britta Simon tooBambu by Sprout Social, perform hello following steps:**</span></span>

1. <span data-ttu-id="613e4-195">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="613e4-195">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="613e4-197">Välj i listan med program hello **Bambu av brysselkål sociala**.</span><span class="sxs-lookup"><span data-stu-id="613e4-197">In hello applications list, select **Bambu by Sprout Social**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_bambubysproutsocial_app.png) 

3. <span data-ttu-id="613e4-199">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="613e4-199">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="613e4-201">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="613e4-201">Click **Add** button.</span></span> <span data-ttu-id="613e4-202">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="613e4-202">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="613e4-204">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="613e4-204">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="613e4-205">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="613e4-205">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="613e4-206">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="613e4-206">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="613e4-207">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="613e4-207">Testing single sign-on</span></span>

<span data-ttu-id="613e4-208">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="613e4-208">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="613e4-209">Du bör få automatiskt inloggade tooyour Bambu av brysselkål sociala program när du klickar på hello Bambu av brysselkål sociala panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="613e4-209">When you click hello Bambu by Sprout Social tile in hello Access Panel, you should get automatically signed-on tooyour Bambu by Sprout Social application.</span></span> <span data-ttu-id="613e4-210">Mer information om åtkomstpanelen finns [introduktion till åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="613e4-210">For more details about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="613e4-211">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="613e4-211">Additional resources</span></span>

* [<span data-ttu-id="613e4-212">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="613e4-212">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="613e4-213">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="613e4-213">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_203.png

