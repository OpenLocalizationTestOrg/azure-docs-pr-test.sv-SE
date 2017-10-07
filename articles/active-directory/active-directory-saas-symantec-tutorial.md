---
title: "Självstudier: Azure Active Directory-integrering med Symantec Web Security Service (WSS) | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Symantec Web Security Service (WSS)."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d6e4d893-1f14-4522-ac20-0c73b18c72a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 9f02b3d4ce2073110c55af4b567b0e3b5a88404f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-symantec-web-security-service-wss"></a><span data-ttu-id="9578b-103">Självstudier: Azure Active Directory-integrering med Symantec Web Security Service (WSS)</span><span class="sxs-lookup"><span data-stu-id="9578b-103">Tutorial: Azure Active Directory integration with Symantec Web Security Service (WSS)</span></span>

<span data-ttu-id="9578b-104">I den här kursen får du lära dig hur toointegrate ditt Symantec Web Security Service (WSS) konto med ditt konto i Azure Active Directory (AD Azure) så att WSS kan autentisera en användare etablerad på hello Azure AD med hjälp av SAML-autentisering och tvinga användaren eller säkerhetsnivå för gruppregler.</span><span class="sxs-lookup"><span data-stu-id="9578b-104">In this tutorial, you will learn how toointegrate your Symantec Web Security Service (WSS) account with your Azure Active Directory (Azure AD) account so that WSS can authenticate an end user provisioned in hello Azure AD using SAML authentication and enforce user or group level policy rules.</span></span>

<span data-ttu-id="9578b-105">Integrera Symantec Web Security Service (WSS) med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="9578b-105">Integrating Symantec Web Security Service (WSS) with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="9578b-106">Hantera alla hello användare och grupper som används av ditt konto för WSS från Azure AD-portalen.</span><span class="sxs-lookup"><span data-stu-id="9578b-106">Manage all of hello end users and groups used by your WSS account from your Azure AD portal.</span></span> 

- <span data-ttu-id="9578b-107">Tillåt hello slutet användare tooauthenticate sig själva i WSS med sina Azure AD-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="9578b-107">Allow hello end users tooauthenticate themselves in WSS using their Azure AD credentials.</span></span>

- <span data-ttu-id="9578b-108">Aktivera hello tvingande för användare och grupp säkerhetsnivå för regler som definierats i WSS-konto.</span><span class="sxs-lookup"><span data-stu-id="9578b-108">Enable hello enforcement of user and group level policy rules defined in your WSS account.</span></span>

<span data-ttu-id="9578b-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9578b-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9578b-110">Krav</span><span class="sxs-lookup"><span data-stu-id="9578b-110">Prerequisites</span></span>

<span data-ttu-id="9578b-111">tooconfigure Azure AD-integrering med Symantec Web Security Service (WSS) behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="9578b-111">tooconfigure Azure AD integration with Symantec Web Security Service (WSS), you need hello following items:</span></span>

- <span data-ttu-id="9578b-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="9578b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9578b-113">Ett konto med Symantec Web Security Service (WSS)</span><span class="sxs-lookup"><span data-stu-id="9578b-113">A Symantec Web Security Service (WSS) account</span></span>

> [!NOTE]
> <span data-ttu-id="9578b-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte användning av ett WSS-konto som används för närvarande för produktion ändamål.</span><span class="sxs-lookup"><span data-stu-id="9578b-114">tootest hello steps in this tutorial, we do not recommend using a WSS account that is currently being used for production purpose.</span></span>

<span data-ttu-id="9578b-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="9578b-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9578b-116">Använd inte din WSS-konto som används för produktion syftet med det här testet om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="9578b-116">Do not use your WSS account that is currently being used for production purpose for this test unless it is necessary.</span></span>
- <span data-ttu-id="9578b-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9578b-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9578b-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="9578b-118">Scenario description</span></span>
<span data-ttu-id="9578b-119">I den här självstudiekursen kommer du konfigurera din Azure AD tooenable enkel inloggning tooWSS med hello slutanvändaren autentiseringsuppgifter som definierats i Azure AD-kontot.</span><span class="sxs-lookup"><span data-stu-id="9578b-119">In this tutorial, you will configure your Azure AD tooenable single sign-on tooWSS using hello end user credentials defined in your Azure AD account.</span></span>
<span data-ttu-id="9578b-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="9578b-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9578b-121">Att lägga till hello Symantec Web Security Service (WSS) app från galleriet hello</span><span class="sxs-lookup"><span data-stu-id="9578b-121">Adding hello Symantec Web Security Service (WSS) app from hello gallery</span></span>
2. <span data-ttu-id="9578b-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9578b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-symantec-web-security-service-wss-from-hello-gallery"></a><span data-ttu-id="9578b-123">Att lägga till Symantec Web Security Service (WSS) från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="9578b-123">Adding Symantec Web Security Service (WSS) from hello gallery</span></span>
<span data-ttu-id="9578b-124">tooconfigure hello integrering av Symantec Web Security Service (WSS) i Azure AD, behöver du tooadd Symantec Web Security Service (WSS) hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="9578b-124">tooconfigure hello integration of Symantec Web Security Service (WSS) into Azure AD, you need tooadd Symantec Web Security Service (WSS) from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="9578b-125">**tooadd Symantec Web Security Service (WSS) från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="9578b-125">**tooadd Symantec Web Security Service (WSS) from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="9578b-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="9578b-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="9578b-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="9578b-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="9578b-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="9578b-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="9578b-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9578b-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="9578b-133">Skriv i sökrutan hello **Symantec Web Security Service (WSS)**väljer **Symantec Web Security Service (WSS)** resultatet-panelen klickar **Lägg till** knappen tooadd hello programmet.</span><span class="sxs-lookup"><span data-stu-id="9578b-133">In hello search box, type **Symantec Web Security Service (WSS)**, select **Symantec Web Security Service (WSS)** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Symantec Web Security Service (WSS) i hello resultatlistan](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="9578b-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9578b-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="9578b-136">I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med Symantec Web Security Service (WSS) baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="9578b-136">In this section, you configure and test Azure AD single sign-on with Symantec Web Security Service (WSS) based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="9578b-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Symantec Web Security Service (WSS) är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9578b-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Symantec Web Security Service (WSS) is tooa user in Azure AD.</span></span> <span data-ttu-id="9578b-138">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i Symantec Web Security Service (WSS) toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="9578b-138">In other words, a link relationship between an Azure AD user and hello related user in Symantec Web Security Service (WSS) needs toobe established.</span></span>

<span data-ttu-id="9578b-139">I Symantec Web Security Service (WSS), tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="9578b-139">In Symantec Web Security Service (WSS), assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="9578b-140">tooconfigure och testa Azure AD enkel inloggning med Symantec Web Security Service (WSS), behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="9578b-140">tooconfigure and test Azure AD single sign-on with Symantec Web Security Service (WSS), you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="9578b-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="9578b-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="9578b-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9578b-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9578b-143">**[Skapa en testanvändare Symantec Web Security Service (WSS)](#create-a-symantec-web-security-service-wss-test-user)**  -toohave en motsvarighet för Britta Simon i Symantec Web Security Service (WSS) som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="9578b-143">**[Create a Symantec Web Security Service (WSS) test user](#create-a-symantec-web-security-service-wss-test-user)** - toohave a counterpart of Britta Simon in Symantec Web Security Service (WSS) that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="9578b-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9578b-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9578b-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="9578b-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="9578b-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9578b-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="9578b-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Symantec Web Security Service (WSS)-program.</span><span class="sxs-lookup"><span data-stu-id="9578b-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Symantec Web Security Service (WSS) application.</span></span>

<span data-ttu-id="9578b-148">**tooconfigure Azure AD enkel inloggning med Symantec Web Security Service (WSS), utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="9578b-148">**tooconfigure Azure AD single sign-on with Symantec Web Security Service (WSS), perform hello following steps:**</span></span>

1. <span data-ttu-id="9578b-149">I hello Azure-portalen på hello **Symantec Web Security Service (WSS)** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="9578b-149">In hello Azure portal, on hello **Symantec Web Security Service (WSS)** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="9578b-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9578b-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_samlbase.png)

3. <span data-ttu-id="9578b-153">På hello **URL: er och Symantec Web Service (WSS) säkerhetsdomän** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9578b-153">On hello **Symantec Web Security Service (WSS) Domain and URLs** section, perform hello following steps:</span></span>

    ![URL: er och Symantec Web Security Service (WSS)-domän med enkel inloggning information](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_url.png)

    <span data-ttu-id="9578b-155">a.</span><span class="sxs-lookup"><span data-stu-id="9578b-155">a.</span></span> <span data-ttu-id="9578b-156">I hello **identifierare** textruta typen hello URL:`https://saml.threatpulse.net:8443/saml/saml_realm`</span><span class="sxs-lookup"><span data-stu-id="9578b-156">In hello **Identifier** textbox, type hello URL: `https://saml.threatpulse.net:8443/saml/saml_realm`</span></span>

    <span data-ttu-id="9578b-157">b.</span><span class="sxs-lookup"><span data-stu-id="9578b-157">b.</span></span> <span data-ttu-id="9578b-158">I hello **Reply URL** textruta typen hello URL:`https://saml.threatpulse.net:8443/saml/saml_realm/bcsamlpost`</span><span class="sxs-lookup"><span data-stu-id="9578b-158">In hello **Reply URL** textbox, type hello URL: `https://saml.threatpulse.net:8443/saml/saml_realm/bcsamlpost`</span></span>

    > [!NOTE]
    > <span data-ttu-id="9578b-159">Kontakta hello [Symantec Web Security Service (WSS) klienten supportteamet](https://www.symantec.com/contact-us) om hello värden för hello **identifierare** och **Reply URL** inte är av någon anledning.</span><span class="sxs-lookup"><span data-stu-id="9578b-159">Please contact hello [Symantec Web Security Service (WSS) Client support team](https://www.symantec.com/contact-us) if hello values for hello **Identifier** and **Reply URL** are not working for some reason.</span></span>

4. <span data-ttu-id="9578b-160">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="9578b-160">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_certificate.png) 

5. <span data-ttu-id="9578b-162">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="9578b-162">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-symantec-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="9578b-164">Läs toohello WSS onlinedokumentation tooconfigure enkel inloggning på hello Symantec Web Security Service (WSS) sida.</span><span class="sxs-lookup"><span data-stu-id="9578b-164">tooconfigure single sign-on on hello Symantec Web Security Service (WSS) side, refer toohello WSS online documentation.</span></span> <span data-ttu-id="9578b-165">hello hämtas **XML-Metadata för** filen måste toobe importeras till hello WSS portal.</span><span class="sxs-lookup"><span data-stu-id="9578b-165">hello downloaded **Metadata XML** file will need toobe imported into hello WSS portal.</span></span> <span data-ttu-id="9578b-166">Kontakta hello [Symantec Web Security Service (WSS) supportteam](https://www.symantec.com/contact-us) om du behöver hjälp med hello konfiguration på hello WSS-portalen.</span><span class="sxs-lookup"><span data-stu-id="9578b-166">Contact hello [Symantec Web Security Service (WSS) support team](https://www.symantec.com/contact-us) if you need assistance with hello configuration on hello WSS portal.</span></span>

> [!TIP]
> <span data-ttu-id="9578b-167">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="9578b-167">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="9578b-168">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="9578b-168">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="9578b-169">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9578b-169">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="9578b-170">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="9578b-170">Create an Azure AD test user</span></span>

<span data-ttu-id="9578b-171">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9578b-171">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="9578b-173">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="9578b-173">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="9578b-174">Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="9578b-174">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-symantec-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="9578b-176">toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="9578b-176">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-symantec-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="9578b-178">tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9578b-178">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello webbinställningar](./media/active-directory-saas-symantec-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="9578b-180">I hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9578b-180">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello användardialogrutan](./media/active-directory-saas-symantec-tutorial/create_aaduser_04.png)

    <span data-ttu-id="9578b-182">a.</span><span class="sxs-lookup"><span data-stu-id="9578b-182">a.</span></span> <span data-ttu-id="9578b-183">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9578b-183">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9578b-184">b.</span><span class="sxs-lookup"><span data-stu-id="9578b-184">b.</span></span> <span data-ttu-id="9578b-185">I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9578b-185">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="9578b-186">c.</span><span class="sxs-lookup"><span data-stu-id="9578b-186">c.</span></span> <span data-ttu-id="9578b-187">Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="9578b-187">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="9578b-188">d.</span><span class="sxs-lookup"><span data-stu-id="9578b-188">d.</span></span> <span data-ttu-id="9578b-189">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="9578b-189">Click **Create**.</span></span>
 
### <a name="create-a-symantec-web-security-service-wss-test-user"></a><span data-ttu-id="9578b-190">Skapa en testanvändare Symantec Web Security Service (WSS)</span><span class="sxs-lookup"><span data-stu-id="9578b-190">Create a Symantec Web Security Service (WSS) test user</span></span>

<span data-ttu-id="9578b-191">I det här avsnittet skapar du en användare som kallas Britta Simon i Symantec Web Security Service (WSS).</span><span class="sxs-lookup"><span data-stu-id="9578b-191">In this section, you create a user called Britta Simon in Symantec Web Security Service (WSS).</span></span> <span data-ttu-id="9578b-192">hello motsvarande end användarnamn kan skapas manuellt i hello WSS-portalen eller vänta tills hello användare eller grupper som är etablerad på hello Azure AD-toobe synkroniseras toohello WSS portalen efter några minuter (~ 15 minuter).</span><span class="sxs-lookup"><span data-stu-id="9578b-192">hello corresponding end username can be manually created in hello WSS portal or you can wait for hello users/groups provisioned in hello Azure AD toobe synchronized toohello WSS portal after a few minutes (~15 minutes).</span></span> <span data-ttu-id="9578b-193">Användare måste skapas och aktiveras innan du använder enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9578b-193">Users must be created and activated before you use single sign-on.</span></span> <span data-ttu-id="9578b-194">hello offentliga IP-adress hello slutanvändarens dator som kommer att använda toobrowse webbplatser måste också toobe etableras i hello Symantec Web Security Service (WSS)-portalen.</span><span class="sxs-lookup"><span data-stu-id="9578b-194">hello public IP address of hello end user machine, which will be used toobrowse websites also need toobe provisioned in hello Symantec Web Security Service (WSS) portal.</span></span>

> [!NOTE]
> <span data-ttu-id="9578b-195">Kontrollera [Klicka här](http://www.bing.com/search?q=my+ip+address&qs=AS&pq=my+ip+a&sc=8-7&cvid=29A720C95C78488CA3F9A6BA0B3F98C5&FORM=QBLH&sp=1) tooget din dator är offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="9578b-195">Please [click here](http://www.bing.com/search?q=my+ip+address&qs=AS&pq=my+ip+a&sc=8-7&cvid=29A720C95C78488CA3F9A6BA0B3F98C5&FORM=QBLH&sp=1) tooget your machine's public IPaddress.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="9578b-196">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="9578b-196">Assign hello Azure AD test user</span></span>

<span data-ttu-id="9578b-197">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSymantec Web Security Service (WSS).</span><span class="sxs-lookup"><span data-stu-id="9578b-197">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSymantec Web Security Service (WSS).</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="9578b-199">**tooassign Britta Simon tooSymantec Web Security Service (WSS) utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="9578b-199">**tooassign Britta Simon tooSymantec Web Security Service (WSS), perform hello following steps:**</span></span>

1. <span data-ttu-id="9578b-200">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="9578b-200">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="9578b-202">Välj i listan med program hello **Symantec Web Security Service (WSS)**.</span><span class="sxs-lookup"><span data-stu-id="9578b-202">In hello applications list, select **Symantec Web Security Service (WSS)**.</span></span>

    ![hello Symantec Web Security Service (WSS) länken i listan med program hello](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_app.png)  

3. <span data-ttu-id="9578b-204">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="9578b-204">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202]

4. <span data-ttu-id="9578b-206">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="9578b-206">Click **Add** button.</span></span> <span data-ttu-id="9578b-207">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9578b-207">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="9578b-209">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="9578b-209">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="9578b-210">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9578b-210">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9578b-211">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9578b-211">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="9578b-212">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9578b-212">Test single sign-on</span></span>

<span data-ttu-id="9578b-213">I det här avsnittet, du kan testa hello enkel inloggning nu när du har konfigurerat din WSS konto toouse din Azure AD för SAML-autentisering.</span><span class="sxs-lookup"><span data-stu-id="9578b-213">In this section, you'll test hello single sign-on functionality now that you've configured your WSS account toouse your Azure AD for SAML authentication.</span></span>

<span data-ttu-id="9578b-214">När du har konfigurerat dirigeras din webbläsare tooproxy webbtrafik tooWSS när du öppnar din webbläsare och försök toobrowse tooa plats och du kommer att toohello Azure inloggning sidan.</span><span class="sxs-lookup"><span data-stu-id="9578b-214">After you have configured your web browser tooproxy traffic tooWSS, when you open your web browser and try toobrowse tooa site then you'll be redirected toohello Azure sign-on page.</span></span> <span data-ttu-id="9578b-215">Ange hello autentiseringsuppgifterna för slutanvändare för hello test som har etablerats i hello Azure AD (det vill säga BrittaSimon) och tillhörande lösenord.</span><span class="sxs-lookup"><span data-stu-id="9578b-215">Enter hello credentials of hello test end user that has been provisioned in hello Azure AD (that is, BrittaSimon) and associated password.</span></span> <span data-ttu-id="9578b-216">När autentiseringen är kommer du att kunna toobrowse toohello webbplats som du har valt.</span><span class="sxs-lookup"><span data-stu-id="9578b-216">Once authenticated, you'll be able toobrowse toohello website that you chose.</span></span> <span data-ttu-id="9578b-217">Du bör skapa en regel på hello WSS sida tooblock BrittaSimon Internet tooa viss plats bör sidan hello WSS block när du försöker toobrowse toothat webbplats som användare BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9578b-217">Should you create a policy rule on hello WSS side tooblock BrittaSimon from browsing tooa particular site then you should see hello WSS block page when you attempt toobrowse toothat site as user BrittaSimon.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9578b-218">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="9578b-218">Additional resources</span></span>

* [<span data-ttu-id="9578b-219">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9578b-219">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9578b-220">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9578b-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_203.png

