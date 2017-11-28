---
title: "Självstudier: Azure Active Directory-integrering med svart tavla Läs - Shibboleth | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och svart tavla Läs - Shibboleth."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e435cbb4-c0f0-400e-943c-5c923fa8ddf2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: jeedes
ms.openlocfilehash: 40aa3ec5f42b93157af3c56daaadfa66203b21d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-blackboard-learn---shibboleth"></a><span data-ttu-id="db34b-103">Självstudier: Azure Active Directory-integrering med svart tavla Läs - Shibboleth</span><span class="sxs-lookup"><span data-stu-id="db34b-103">Tutorial: Azure Active Directory integration with Blackboard Learn - Shibboleth</span></span>

<span data-ttu-id="db34b-104">I kursen får du lära dig hur toointegrate svart tavla Läs - Shibboleth med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="db34b-104">In this tutorial, you learn how toointegrate Blackboard Learn - Shibboleth with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="db34b-105">Integrera svart tavla Läs - Shibboleth med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="db34b-105">Integrating Blackboard Learn - Shibboleth with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="db34b-106">Du kan styra i Azure AD som har åtkomst tooBlackboard Läs - Shibboleth</span><span class="sxs-lookup"><span data-stu-id="db34b-106">You can control in Azure AD who has access tooBlackboard Learn - Shibboleth</span></span>
- <span data-ttu-id="db34b-107">Du kan aktivera din användare tooautomatically get inloggade tooBlackboard Läs - Shibboleth (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="db34b-107">You can enable your users tooautomatically get signed-on tooBlackboard Learn - Shibboleth (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="db34b-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="db34b-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="db34b-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="db34b-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="db34b-110">Krav</span><span class="sxs-lookup"><span data-stu-id="db34b-110">Prerequisites</span></span>

<span data-ttu-id="db34b-111">tooconfigure Azure AD-integrering med svart tavla Läs - Shibboleth, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="db34b-111">tooconfigure Azure AD integration with Blackboard Learn - Shibboleth, you need hello following items:</span></span>

- <span data-ttu-id="db34b-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="db34b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="db34b-113">En svart tavla Läs - Shibboleth enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="db34b-113">A Blackboard Learn - Shibboleth single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="db34b-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="db34b-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="db34b-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="db34b-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="db34b-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="db34b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="db34b-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="db34b-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="db34b-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="db34b-118">Scenario description</span></span>
<span data-ttu-id="db34b-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="db34b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="db34b-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="db34b-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="db34b-121">Lägger till svart tavla Läs - Shibboleth från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="db34b-121">Adding Blackboard Learn - Shibboleth from hello gallery</span></span>
2. <span data-ttu-id="db34b-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="db34b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-blackboard-learn---shibboleth-from-hello-gallery"></a><span data-ttu-id="db34b-123">Lägger till svart tavla Läs - Shibboleth från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="db34b-123">Adding Blackboard Learn - Shibboleth from hello gallery</span></span>
<span data-ttu-id="db34b-124">tooconfigure hello integrering av svart tavla Läs - Shibboleth till Azure AD måste tooadd svart tavla Läs - Shibboleth hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="db34b-124">tooconfigure hello integration of Blackboard Learn - Shibboleth into Azure AD, you need tooadd Blackboard Learn - Shibboleth from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="db34b-125">**svart tavla Läs - tooadd Shibboleth från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="db34b-125">**tooadd Blackboard Learn - Shibboleth from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="db34b-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="db34b-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="db34b-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="db34b-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="db34b-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="db34b-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="db34b-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="db34b-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="db34b-133">Skriv i sökrutan hello **svart tavla Läs - Shibboleth**.</span><span class="sxs-lookup"><span data-stu-id="db34b-133">In hello search box, type **Blackboard Learn - Shibboleth**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_blackboardlearn-shibboleth_search.png)

5. <span data-ttu-id="db34b-135">Markera hello resultat på panelen **svart tavla Läs - Shibboleth**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="db34b-135">In hello results panel, select **Blackboard Learn - Shibboleth**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_blackboardlearn-shibboleth_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="db34b-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="db34b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="db34b-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med svart tavla Läs - Shibboleth baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="db34b-138">In this section, you configure and test Azure AD single sign-on with Blackboard Learn - Shibboleth based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="db34b-139">Azure AD måste tooknow vilka hello motsvarighet användare i svart tavla Läs för enkel inloggning toowork - Shibboleth är tooa användare i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="db34b-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Blackboard Learn - Shibboleth is tooa user in Azure AD.</span></span> <span data-ttu-id="db34b-140">Med andra ord en länk mellan en Azure AD-användare och hello relaterade användare i svart tavla Läs - Shibboleth måste toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="db34b-140">In other words, a link relationship between an Azure AD user and hello related user in Blackboard Learn - Shibboleth needs toobe established.</span></span>

<span data-ttu-id="db34b-141">Tilldela hello värdet för hello i svart tavla Läs - Shibboleth, **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="db34b-141">In Blackboard Learn - Shibboleth, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="db34b-142">tooconfigure och testa Azure AD enkel inloggning med svart tavla Läs - Shibboleth, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="db34b-142">tooconfigure and test Azure AD single sign-on with Blackboard Learn - Shibboleth, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="db34b-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="db34b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="db34b-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="db34b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="db34b-145">**[Skapa en svart tavla Läs - Shibboleth testanvändare](#creating-a-blackboard-learn---shibboleth-test-user)**  - toohave en motsvarighet för Britta Simon i svart tavla Läs - Shibboleth som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="db34b-145">**[Creating a Blackboard Learn - Shibboleth test user](#creating-a-blackboard-learn---shibboleth-test-user)** - toohave a counterpart of Britta Simon in Blackboard Learn - Shibboleth that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="db34b-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="db34b-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="db34b-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="db34b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="db34b-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="db34b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="db34b-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i din svart tavla Läs - Shibboleth program.</span><span class="sxs-lookup"><span data-stu-id="db34b-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Blackboard Learn - Shibboleth application.</span></span>

<span data-ttu-id="db34b-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med svart tavla Läs - Shibboleth:**</span><span class="sxs-lookup"><span data-stu-id="db34b-150">**tooconfigure Azure AD single sign-on with Blackboard Learn - Shibboleth, perform hello following steps:**</span></span>

1. <span data-ttu-id="db34b-151">I hello Azure-portalen på hello **svart tavla Läs - Shibboleth** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="db34b-151">In hello Azure portal, on hello **Blackboard Learn - Shibboleth** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="db34b-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="db34b-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_blackboardlearn-shibboleth_samlbase.png)

3. <span data-ttu-id="db34b-155">På hello **svart tavla Läs - URL: er och Shibboleth domän** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="db34b-155">On hello **Blackboard Learn - Shibboleth Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_blackboardlearn-shibboleth_url.png)

    <span data-ttu-id="db34b-157">a.</span><span class="sxs-lookup"><span data-stu-id="db34b-157">a.</span></span> <span data-ttu-id="db34b-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<yourblackoardlearnserver>.blackboardlearn.com/Shibboleth.sso/Login`</span><span class="sxs-lookup"><span data-stu-id="db34b-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<yourblackoardlearnserver>.blackboardlearn.com/Shibboleth.sso/Login`</span></span>

    <span data-ttu-id="db34b-159">b.</span><span class="sxs-lookup"><span data-stu-id="db34b-159">b.</span></span> <span data-ttu-id="db34b-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<yourblackoardlearnserver>.blackboardlearn.com/shibboleth-sp`</span><span class="sxs-lookup"><span data-stu-id="db34b-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<yourblackoardlearnserver>.blackboardlearn.com/shibboleth-sp`</span></span>

    <span data-ttu-id="db34b-161">c.</span><span class="sxs-lookup"><span data-stu-id="db34b-161">c.</span></span> <span data-ttu-id="db34b-162">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<yourblackoardlearnserver>.blackboardlearn.com/Shibboleth.sso/SAML2/POST`</span><span class="sxs-lookup"><span data-stu-id="db34b-162">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<yourblackoardlearnserver>.blackboardlearn.com/Shibboleth.sso/SAML2/POST`</span></span>
 
    > [!NOTE] 
    > <span data-ttu-id="db34b-163">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="db34b-163">These values are not real.</span></span> <span data-ttu-id="db34b-164">Uppdatera dessa värden med hello faktiska identifierare, Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="db34b-164">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="db34b-165">Kontakta [svart tavla Läs - Shibboleth klienten supportteamet](https://www.blackboard.com/forms/contact-us_form.aspx) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="db34b-165">Contact [Blackboard Learn - Shibboleth Client support team](https://www.blackboard.com/forms/contact-us_form.aspx) tooget these values.</span></span> 

4. <span data-ttu-id="db34b-166">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="db34b-166">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_blackboardlearn-shibboleth_certificate.png) 

5. <span data-ttu-id="db34b-168">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="db34b-168">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="db34b-170">På hello **svart tavla Läs - Shibboleth Configuration** klickar du på **konfigurera svart tavla Läs - Shibboleth** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="db34b-170">On hello **Blackboard Learn - Shibboleth Configuration** section, click **Configure Blackboard Learn - Shibboleth** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="db34b-171">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="db34b-171">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_blackboardlearn-shibboleth_configure.png) 

7. <span data-ttu-id="db34b-173">tooconfigure enkel inloggning på **svart tavla Läs - Shibboleth** sida, behöver du toosend hello hämtas **XML-Metadata för** och **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning URL: en** för[svart tavla Läs - Shibboleth supportteamet](https://www.blackboard.com/forms/contact-us_form.aspx).</span><span class="sxs-lookup"><span data-stu-id="db34b-173">tooconfigure single sign-on on **Blackboard Learn - Shibboleth** side, you need toosend hello downloaded **Metadata XML** and **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[Blackboard Learn - Shibboleth support team](https://www.blackboard.com/forms/contact-us_form.aspx).</span></span>

> [!TIP]
> <span data-ttu-id="db34b-174">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="db34b-174">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="db34b-175">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="db34b-175">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="db34b-176">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="db34b-176">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="db34b-177">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="db34b-177">Creating an Azure AD test user</span></span>
<span data-ttu-id="db34b-178">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="db34b-178">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="db34b-180">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="db34b-180">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="db34b-181">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="db34b-181">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="db34b-183">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="db34b-183">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="db34b-185">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="db34b-185">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="db34b-187">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="db34b-187">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="db34b-189">a.</span><span class="sxs-lookup"><span data-stu-id="db34b-189">a.</span></span> <span data-ttu-id="db34b-190">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="db34b-190">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="db34b-191">b.</span><span class="sxs-lookup"><span data-stu-id="db34b-191">b.</span></span> <span data-ttu-id="db34b-192">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="db34b-192">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="db34b-193">c.</span><span class="sxs-lookup"><span data-stu-id="db34b-193">c.</span></span> <span data-ttu-id="db34b-194">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="db34b-194">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="db34b-195">d.</span><span class="sxs-lookup"><span data-stu-id="db34b-195">d.</span></span> <span data-ttu-id="db34b-196">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="db34b-196">Click **Create**.</span></span>
 
### <a name="creating-a-blackboard-learn---shibboleth-test-user"></a><span data-ttu-id="db34b-197">Skapa en svart tavla Läs - Shibboleth testanvändare</span><span class="sxs-lookup"><span data-stu-id="db34b-197">Creating a Blackboard Learn - Shibboleth test user</span></span>

<span data-ttu-id="db34b-198">I det här avsnittet skapar du en användare som kallas Britta Simon i svart tavla Läs - Shibboleth.</span><span class="sxs-lookup"><span data-stu-id="db34b-198">In this section, you create a user called Britta Simon in Blackboard Learn - Shibboleth.</span></span> <span data-ttu-id="db34b-199">Arbeta med din [svart tavla Läs - Shibboleth supportteamet](https://www.blackboard.com/forms/contact-us_form.aspx) tooadd hello användare i hello svart tavla Läs - Shibboleth plattform.</span><span class="sxs-lookup"><span data-stu-id="db34b-199">Work with your [Blackboard Learn - Shibboleth support team](https://www.blackboard.com/forms/contact-us_form.aspx) tooadd hello users in hello Blackboard Learn - Shibboleth platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="db34b-200">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="db34b-200">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="db34b-201">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooBlackboard Läs - Shibboleth.</span><span class="sxs-lookup"><span data-stu-id="db34b-201">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooBlackboard Learn - Shibboleth.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="db34b-203">**tooassign Britta Simon tooBlackboard Läs - Shibboleth, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="db34b-203">**tooassign Britta Simon tooBlackboard Learn - Shibboleth, perform hello following steps:**</span></span>

1. <span data-ttu-id="db34b-204">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="db34b-204">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="db34b-206">Välj i listan med program hello **svart tavla Läs - Shibboleth**.</span><span class="sxs-lookup"><span data-stu-id="db34b-206">In hello applications list, select **Blackboard Learn - Shibboleth**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_blackboardlearn-shibboleth_app.png) 

3. <span data-ttu-id="db34b-208">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="db34b-208">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="db34b-210">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="db34b-210">Click **Add** button.</span></span> <span data-ttu-id="db34b-211">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="db34b-211">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="db34b-213">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="db34b-213">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="db34b-214">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="db34b-214">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="db34b-215">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="db34b-215">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="db34b-216">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="db34b-216">Testing single sign-on</span></span>

<span data-ttu-id="db34b-217">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="db34b-217">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="db34b-218">När du klickar på hello svart tavla Läs - Shibboleth panelen i hello åtkomstpanelen, bör du hämta automatiskt inloggade tooyour svart tavla Läs - Shibboleth program.</span><span class="sxs-lookup"><span data-stu-id="db34b-218">When you click hello Blackboard Learn - Shibboleth tile in hello Access Panel, you should get automatically signed-on tooyour Blackboard Learn - Shibboleth application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="db34b-219">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="db34b-219">Additional resources</span></span>

* [<span data-ttu-id="db34b-220">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="db34b-220">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="db34b-221">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="db34b-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-blackboard-learn-shibboleth-tutorial/tutorial_general_203.png

