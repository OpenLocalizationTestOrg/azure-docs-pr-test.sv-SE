---
title: "Självstudier: Azure Active Directory-integrering med Symantec Web Security Service (WSS) | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Symantec Web Security Service (WSS)."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d6e4d893-1f14-4522-ac20-0c73b18c72a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: f4575e6f5eb63cd0e1d63edd35e30be3cacc3382
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-symantec-web-security-service-wss"></a><span data-ttu-id="87c64-103">Självstudier: Azure Active Directory-integrering med Symantec Web Security Service (WSS)</span><span class="sxs-lookup"><span data-stu-id="87c64-103">Tutorial: Azure Active Directory integration with Symantec Web Security Service (WSS)</span></span>

<span data-ttu-id="87c64-104">I kursen får du lära dig hur toointegrate Symantec Web Security Service (WSS) med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="87c64-104">In this tutorial, you learn how toointegrate Symantec Web Security Service (WSS) with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="87c64-105">Integrera Symantec Web Security Service (WSS) med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="87c64-105">Integrating Symantec Web Security Service (WSS) with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="87c64-106">Du kan styra i Azure AD som har åtkomst tooSymantec Web Security Service (WSS)</span><span class="sxs-lookup"><span data-stu-id="87c64-106">You can control in Azure AD who has access tooSymantec Web Security Service (WSS)</span></span>
- <span data-ttu-id="87c64-107">Du kan aktivera din användare tooautomatically get inloggade tooSymantec Web Security Service (WSS) (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="87c64-107">You can enable your users tooautomatically get signed-on tooSymantec Web Security Service (WSS) (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="87c64-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="87c64-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="87c64-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="87c64-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="87c64-110">Krav</span><span class="sxs-lookup"><span data-stu-id="87c64-110">Prerequisites</span></span>

<span data-ttu-id="87c64-111">tooconfigure Azure AD-integrering med Symantec Web Security Service (WSS) behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="87c64-111">tooconfigure Azure AD integration with Symantec Web Security Service (WSS), you need hello following items:</span></span>

- <span data-ttu-id="87c64-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="87c64-112">An Azure AD subscription</span></span>
- <span data-ttu-id="87c64-113">En Symantec Web Security Service (WSS) enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="87c64-113">A Symantec Web Security Service (WSS) single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="87c64-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="87c64-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="87c64-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="87c64-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="87c64-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="87c64-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="87c64-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="87c64-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="87c64-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="87c64-118">Scenario description</span></span>
<span data-ttu-id="87c64-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="87c64-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="87c64-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="87c64-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="87c64-121">Att lägga till Symantec Web Security Service (WSS) från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="87c64-121">Adding Symantec Web Security Service (WSS) from hello gallery</span></span>
2. <span data-ttu-id="87c64-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="87c64-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-symantec-web-security-service-wss-from-hello-gallery"></a><span data-ttu-id="87c64-123">Att lägga till Symantec Web Security Service (WSS) från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="87c64-123">Adding Symantec Web Security Service (WSS) from hello gallery</span></span>
<span data-ttu-id="87c64-124">tooconfigure hello integrering av Symantec Web Security Service (WSS) i Azure AD, behöver du tooadd Symantec Web Security Service (WSS) hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="87c64-124">tooconfigure hello integration of Symantec Web Security Service (WSS) into Azure AD, you need tooadd Symantec Web Security Service (WSS) from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="87c64-125">**tooadd Symantec Web Security Service (WSS) från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="87c64-125">**tooadd Symantec Web Security Service (WSS) from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="87c64-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="87c64-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="87c64-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="87c64-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="87c64-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="87c64-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="87c64-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="87c64-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="87c64-133">Skriv i sökrutan hello **Symantec Web Security Service (WSS)**.</span><span class="sxs-lookup"><span data-stu-id="87c64-133">In hello search box, type **Symantec Web Security Service (WSS)**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_symantecwebsecurityservicewss_search.png)

5. <span data-ttu-id="87c64-135">Markera hello resultat på panelen **Symantec Web Security Service (WSS)**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="87c64-135">In hello results panel, select **Symantec Web Security Service (WSS)**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_symantecwebsecurityservicewss_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="87c64-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="87c64-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="87c64-138">I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med Symantec Web Security Service (WSS) baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="87c64-138">In this section, you configure and test Azure AD single sign-on with Symantec Web Security Service (WSS) based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="87c64-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Symantec Web Security Service (WSS) är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="87c64-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Symantec Web Security Service (WSS) is tooa user in Azure AD.</span></span> <span data-ttu-id="87c64-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i Symantec Web Security Service (WSS) toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="87c64-140">In other words, a link relationship between an Azure AD user and hello related user in Symantec Web Security Service (WSS) needs toobe established.</span></span>

<span data-ttu-id="87c64-141">I Symantec Web Security Service (WSS), tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="87c64-141">In Symantec Web Security Service (WSS), assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="87c64-142">tooconfigure och testa Azure AD enkel inloggning med Symantec Web Security Service (WSS), behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="87c64-142">tooconfigure and test Azure AD single sign-on with Symantec Web Security Service (WSS), you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="87c64-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="87c64-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="87c64-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="87c64-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="87c64-145">**[Skapa en testanvändare Symantec Web Security Service (WSS)](#creating-a-symantec-web-security-service-wss-test-user)**  -toohave en motsvarighet för Britta Simon i Symantec Web Security Service (WSS) som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="87c64-145">**[Creating a Symantec Web Security Service (WSS) test user](#creating-a-symantec-web-security-service-wss-test-user)** - toohave a counterpart of Britta Simon in Symantec Web Security Service (WSS) that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="87c64-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="87c64-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="87c64-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="87c64-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="87c64-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="87c64-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="87c64-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Symantec Web Security Service (WSS)-program.</span><span class="sxs-lookup"><span data-stu-id="87c64-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Symantec Web Security Service (WSS) application.</span></span>

<span data-ttu-id="87c64-150">**tooconfigure Azure AD enkel inloggning med Symantec Web Security Service (WSS), utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="87c64-150">**tooconfigure Azure AD single sign-on with Symantec Web Security Service (WSS), perform hello following steps:**</span></span>

1. <span data-ttu-id="87c64-151">I hello Azure-portalen på hello **Symantec Web Security Service (WSS)** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="87c64-151">In hello Azure portal, on hello **Symantec Web Security Service (WSS)** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="87c64-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="87c64-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_symantecwebsecurityservicewss_samlbase.png)

3. <span data-ttu-id="87c64-155">På hello **URL: er och Symantec Web Service (WSS) säkerhetsdomän** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="87c64-155">On hello **Symantec Web Security Service (WSS) Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_symantecwebsecurityservicewss_url.png)

    <span data-ttu-id="87c64-157">a.</span><span class="sxs-lookup"><span data-stu-id="87c64-157">a.</span></span> <span data-ttu-id="87c64-158">I hello **inloggnings-URL** textruta uppgift hello url som du vill tooblock enligt toohello blockerande princip hello Symantec Web Security Service (WSS).</span><span class="sxs-lookup"><span data-stu-id="87c64-158">In hello **Sign-on URL** textbox, mention hello url, which you want tooblock according toohello blocking policy of hello Symantec Web Security Service (WSS).</span></span>  
    
    <span data-ttu-id="87c64-159">b.</span><span class="sxs-lookup"><span data-stu-id="87c64-159">b.</span></span> <span data-ttu-id="87c64-160">I hello **identifierare** textruta typen hello URL:`https://saml.threatpulse.net:8443/saml/saml_realm`</span><span class="sxs-lookup"><span data-stu-id="87c64-160">In hello **Identifier** textbox, type hello URL: `https://saml.threatpulse.net:8443/saml/saml_realm`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="87c64-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="87c64-161">These values are not real.</span></span> <span data-ttu-id="87c64-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="87c64-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="87c64-163">Kontakta [Symantec Web Security Service (WSS) klienten supportteamet](https://www.symantec.com/contact-us) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="87c64-163">Contact [Symantec Web Security Service (WSS) Client support team](https://www.symantec.com/contact-us) tooget these values.</span></span> 

4. <span data-ttu-id="87c64-164">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="87c64-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_symantecwebsecurityservicewss_certificate.png) 

5. <span data-ttu-id="87c64-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="87c64-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="87c64-168">tooconfigure enkel inloggning på **Symantec Web Security Service (WSS)** sida, behöver du toosend hello hämtas **XML-Metadata för** för[Symantec Web Security Service (WSS) supportteam](https://www.symantec.com/contact-us).</span><span class="sxs-lookup"><span data-stu-id="87c64-168">tooconfigure single sign-on on **Symantec Web Security Service (WSS)** side, you need toosend hello downloaded **Metadata XML** too[Symantec Web Security Service (WSS) support team](https://www.symantec.com/contact-us).</span></span>

> [!TIP]
> <span data-ttu-id="87c64-169">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="87c64-169">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="87c64-170">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="87c64-170">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="87c64-171">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="87c64-171">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="87c64-172">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="87c64-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="87c64-173">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="87c64-173">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="87c64-175">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="87c64-175">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="87c64-176">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="87c64-176">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="87c64-178">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="87c64-178">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="87c64-180">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="87c64-180">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="87c64-182">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="87c64-182">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="87c64-184">a.</span><span class="sxs-lookup"><span data-stu-id="87c64-184">a.</span></span> <span data-ttu-id="87c64-185">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="87c64-185">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="87c64-186">b.</span><span class="sxs-lookup"><span data-stu-id="87c64-186">b.</span></span> <span data-ttu-id="87c64-187">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="87c64-187">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="87c64-188">c.</span><span class="sxs-lookup"><span data-stu-id="87c64-188">c.</span></span> <span data-ttu-id="87c64-189">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="87c64-189">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="87c64-190">d.</span><span class="sxs-lookup"><span data-stu-id="87c64-190">d.</span></span> <span data-ttu-id="87c64-191">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="87c64-191">Click **Create**.</span></span>
 
### <a name="creating-a-symantec-web-security-service-wss-test-user"></a><span data-ttu-id="87c64-192">Skapa en testanvändare Symantec Web Security Service (WSS)</span><span class="sxs-lookup"><span data-stu-id="87c64-192">Creating a Symantec Web Security Service (WSS) test user</span></span>

<span data-ttu-id="87c64-193">I det här avsnittet skapar du en användare som kallas Britta Simon i Symantec Web Security Service (WSS).</span><span class="sxs-lookup"><span data-stu-id="87c64-193">In this section, you create a user called Britta Simon in Symantec Web Security Service (WSS).</span></span> <span data-ttu-id="87c64-194">Arbeta med [Symantec Web Security Service (WSS) supportteam](https://www.symantec.com/contact-us) att lägga till hello användare i hello Symantec Web Security Service (WSS)-plattformen.</span><span class="sxs-lookup"><span data-stu-id="87c64-194">Work with [Symantec Web Security Service (WSS) support team](https://www.symantec.com/contact-us) to add hello users in hello Symantec Web Security Service (WSS) platform.</span></span> <span data-ttu-id="87c64-195">Användare måste skapas och aktiveras innan du använder enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="87c64-195">Users must be created and activated before you use single sign-on.</span></span> <span data-ttu-id="87c64-196">Också tillsammans med hello användaren information du behöver toosend hello offentliga IP-adress för hello datorn från vilken du försöker tooaccess hello program.</span><span class="sxs-lookup"><span data-stu-id="87c64-196">Also along with hello user details you need toosend hello public IPaddress of hello machine from which you are trying tooaccess hello application.</span></span>

> [!NOTE]
> <span data-ttu-id="87c64-197">Kontrollera [Klicka här](http://www.bing.com/search?q=my+ip+address&qs=AS&pq=my+ip+a&sc=8-7&cvid=29A720C95C78488CA3F9A6BA0B3F98C5&FORM=QBLH&sp=1) tooget din dator är offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="87c64-197">Please [click here](http://www.bing.com/search?q=my+ip+address&qs=AS&pq=my+ip+a&sc=8-7&cvid=29A720C95C78488CA3F9A6BA0B3F98C5&FORM=QBLH&sp=1) tooget your machine's public IPaddress.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="87c64-198">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="87c64-198">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="87c64-199">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSymantec Web Security Service (WSS).</span><span class="sxs-lookup"><span data-stu-id="87c64-199">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSymantec Web Security Service (WSS).</span></span>

![Tilldela användare][200] 

<span data-ttu-id="87c64-201">**tooassign Britta Simon tooSymantec Web Security Service (WSS) utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="87c64-201">**tooassign Britta Simon tooSymantec Web Security Service (WSS), perform hello following steps:**</span></span>

1. <span data-ttu-id="87c64-202">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="87c64-202">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="87c64-204">Välj i listan med program hello **Symantec Web Security Service (WSS)**.</span><span class="sxs-lookup"><span data-stu-id="87c64-204">In hello applications list, select **Symantec Web Security Service (WSS)**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_symantecwebsecurityservicewss_app.png) 

3. <span data-ttu-id="87c64-206">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="87c64-206">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="87c64-208">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="87c64-208">Click **Add** button.</span></span> <span data-ttu-id="87c64-209">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="87c64-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="87c64-211">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="87c64-211">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="87c64-212">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="87c64-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="87c64-213">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="87c64-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="87c64-214">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="87c64-214">Testing single sign-on</span></span>

<span data-ttu-id="87c64-215">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="87c64-215">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="87c64-216">När du klickar på hello Symantec Web Security Service (WSS) panelen i hello åtkomstpanelen hämta omdirigerade toohello blockerar sida som hello blockerar principen har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="87c64-216">When you click hello Symantec Web Security Service (WSS) tile in hello Access Panel, you get redirected toohello blocking page for which hello blocking policy has been configured.</span></span>
<span data-ttu-id="87c64-217">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="87c64-217">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="87c64-218">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="87c64-218">Additional resources</span></span>

* [<span data-ttu-id="87c64-219">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="87c64-219">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="87c64-220">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="87c64-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_203.png

