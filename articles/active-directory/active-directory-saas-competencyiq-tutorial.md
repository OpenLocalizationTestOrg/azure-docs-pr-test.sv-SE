---
title: "Självstudier: Azure Active Directory-integrering med CompetencyIQ | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och CompetencyIQ."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e262bf7e-cc7d-4d0e-aea7-861f00d8837d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2017
ms.author: jeedes
ms.openlocfilehash: c032884b092da85684e24e98f75371475e09322d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-competencyiq"></a><span data-ttu-id="93f6b-103">Självstudier: Azure Active Directory-integrering med CompetencyIQ</span><span class="sxs-lookup"><span data-stu-id="93f6b-103">Tutorial: Azure Active Directory integration with CompetencyIQ</span></span>

<span data-ttu-id="93f6b-104">I kursen får du lära dig hur toointegrate CompetencyIQ med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="93f6b-104">In this tutorial, you learn how toointegrate CompetencyIQ with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="93f6b-105">Integrera CompetencyIQ med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="93f6b-105">Integrating CompetencyIQ with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="93f6b-106">Du kan styra i Azure AD som har åtkomst till tooCompetencyIQ</span><span class="sxs-lookup"><span data-stu-id="93f6b-106">You can control in Azure AD who has access tooCompetencyIQ</span></span>
- <span data-ttu-id="93f6b-107">Du kan aktivera din användare tooautomatically get inloggade tooCompetencyIQ (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="93f6b-107">You can enable your users tooautomatically get signed-on tooCompetencyIQ (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="93f6b-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="93f6b-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="93f6b-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="93f6b-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="93f6b-110">Krav</span><span class="sxs-lookup"><span data-stu-id="93f6b-110">Prerequisites</span></span>

<span data-ttu-id="93f6b-111">tooconfigure Azure AD-integrering med CompetencyIQ, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="93f6b-111">tooconfigure Azure AD integration with CompetencyIQ, you need hello following items:</span></span>

- <span data-ttu-id="93f6b-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="93f6b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="93f6b-113">En CompetencyIQ enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="93f6b-113">A CompetencyIQ single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="93f6b-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="93f6b-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="93f6b-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="93f6b-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="93f6b-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="93f6b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="93f6b-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="93f6b-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="93f6b-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="93f6b-118">Scenario description</span></span>
<span data-ttu-id="93f6b-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="93f6b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="93f6b-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="93f6b-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="93f6b-121">Att lägga till CompetencyIQ från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="93f6b-121">Adding CompetencyIQ from hello gallery</span></span>
2. <span data-ttu-id="93f6b-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="93f6b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-competencyiq-from-hello-gallery"></a><span data-ttu-id="93f6b-123">Att lägga till CompetencyIQ från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="93f6b-123">Adding CompetencyIQ from hello gallery</span></span>
<span data-ttu-id="93f6b-124">tooconfigure hello integrering av CompetencyIQ i Azure AD, behöver du tooadd CompetencyIQ hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="93f6b-124">tooconfigure hello integration of CompetencyIQ into Azure AD, you need tooadd CompetencyIQ from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="93f6b-125">**tooadd CompetencyIQ från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="93f6b-125">**tooadd CompetencyIQ from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="93f6b-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="93f6b-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="93f6b-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="93f6b-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="93f6b-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="93f6b-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="93f6b-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="93f6b-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="93f6b-133">Skriv i sökrutan hello **CompetencyIQ**.</span><span class="sxs-lookup"><span data-stu-id="93f6b-133">In hello search box, type **CompetencyIQ**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-competencyiq-tutorial/tutorial_competencyiq_search.png)

5. <span data-ttu-id="93f6b-135">Markera hello resultat på panelen **CompetencyIQ**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="93f6b-135">In hello results panel, select **CompetencyIQ**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-competencyiq-tutorial/tutorial_competencyiq_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="93f6b-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="93f6b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="93f6b-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med CompetencyIQ baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="93f6b-138">In this section, you configure and test Azure AD single sign-on with CompetencyIQ based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="93f6b-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i CompetencyIQ är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="93f6b-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in CompetencyIQ is tooa user in Azure AD.</span></span> <span data-ttu-id="93f6b-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i CompetencyIQ toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="93f6b-140">In other words, a link relationship between an Azure AD user and hello related user in CompetencyIQ needs toobe established.</span></span>

<span data-ttu-id="93f6b-141">I CompetencyIQ, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="93f6b-141">In CompetencyIQ, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="93f6b-142">tooconfigure och testa Azure AD enkel inloggning med CompetencyIQ, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="93f6b-142">tooconfigure and test Azure AD single sign-on with CompetencyIQ, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="93f6b-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="93f6b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="93f6b-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="93f6b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="93f6b-145">**[Skapa en testanvändare CompetencyIQ](#creating-a-competencyiq-test-user)**  -toohave en motsvarighet för Britta Simon i CompetencyIQ som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="93f6b-145">**[Creating a CompetencyIQ test user](#creating-a-competencyiq-test-user)** - toohave a counterpart of Britta Simon in CompetencyIQ that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="93f6b-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="93f6b-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="93f6b-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="93f6b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="93f6b-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="93f6b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="93f6b-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt CompetencyIQ program.</span><span class="sxs-lookup"><span data-stu-id="93f6b-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your CompetencyIQ application.</span></span>

<span data-ttu-id="93f6b-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med CompetencyIQ:**</span><span class="sxs-lookup"><span data-stu-id="93f6b-150">**tooconfigure Azure AD single sign-on with CompetencyIQ, perform hello following steps:**</span></span>

1. <span data-ttu-id="93f6b-151">I hello Azure-portalen på hello **CompetencyIQ** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="93f6b-151">In hello Azure portal, on hello **CompetencyIQ** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="93f6b-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="93f6b-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-competencyiq-tutorial/tutorial_competencyiq_samlbase.png)

3. <span data-ttu-id="93f6b-155">På hello **CompetencyIQ domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="93f6b-155">On hello **CompetencyIQ Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-competencyiq-tutorial/tutorial_competencyiq_url1.png)

    <span data-ttu-id="93f6b-157">a.</span><span class="sxs-lookup"><span data-stu-id="93f6b-157">a.</span></span> <span data-ttu-id="93f6b-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<customer>.competencyiq.com/`</span><span class="sxs-lookup"><span data-stu-id="93f6b-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<customer>.competencyiq.com/`</span></span>
    
    <span data-ttu-id="93f6b-159">b.</span><span class="sxs-lookup"><span data-stu-id="93f6b-159">b.</span></span> <span data-ttu-id="93f6b-160">I hello **identifierare** textruta typen hello URL:`https://www.competencyiq.com/`</span><span class="sxs-lookup"><span data-stu-id="93f6b-160">In hello **Identifier** textbox, type hello URL: `https://www.competencyiq.com/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="93f6b-161">hello inloggning URL-värdet är inte riktigt så att uppdatera det med faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="93f6b-161">hello Sign-on URL value is not real so update this with actual Sign-On URL.</span></span> <span data-ttu-id="93f6b-162">Kontakta [CompetencyIQ klienten supportteamet](https://www.competencyiq.com/) tooget detta.</span><span class="sxs-lookup"><span data-stu-id="93f6b-162">Contact [CompetencyIQ Client support team](https://www.competencyiq.com/) tooget this.</span></span> 
 
4. <span data-ttu-id="93f6b-163">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="93f6b-163">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-competencyiq-tutorial/tutorial_competencyiq_certificate.png) 

5. <span data-ttu-id="93f6b-165">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="93f6b-165">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-competencyiq-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="93f6b-167">På hello **CompetencyIQ Configuration** klickar du på **konfigurera CompetencyIQ** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="93f6b-167">On hello **CompetencyIQ Configuration** section, click **Configure CompetencyIQ** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="93f6b-168">Kopiera hello **SAML enhets-ID**, och **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="93f6b-168">Copy hello **SAML Entity ID**, and **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-competencyiq-tutorial/tutorial_competencyiq_configure.png) 

7. <span data-ttu-id="93f6b-170">tooconfigure enkel inloggning på **CompetencyIQ** sida, behöver du toosend hello hämtas **XML-Metadata för**, **SAML enhets-ID** och **SAML enkel inloggning Tjänst-URL** för[CompetencyIQ supportteamet](https://www.competencyiq.com/).</span><span class="sxs-lookup"><span data-stu-id="93f6b-170">tooconfigure single sign-on on **CompetencyIQ** side, you need toosend hello downloaded **Metadata XML**, **SAML Entity ID** and **SAML Single Sign-On Service URL** too[CompetencyIQ support team](https://www.competencyiq.com/).</span></span> <span data-ttu-id="93f6b-171">De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="93f6b-171">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="93f6b-172">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="93f6b-172">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="93f6b-173">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="93f6b-173">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="93f6b-174">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="93f6b-174">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="93f6b-175">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="93f6b-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="93f6b-176">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="93f6b-176">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="93f6b-178">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="93f6b-178">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="93f6b-179">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="93f6b-179">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-competencyiq-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="93f6b-181">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="93f6b-181">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-competencyiq-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="93f6b-183">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="93f6b-183">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-competencyiq-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="93f6b-185">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="93f6b-185">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-competencyiq-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="93f6b-187">a.</span><span class="sxs-lookup"><span data-stu-id="93f6b-187">a.</span></span> <span data-ttu-id="93f6b-188">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="93f6b-188">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="93f6b-189">b.</span><span class="sxs-lookup"><span data-stu-id="93f6b-189">b.</span></span> <span data-ttu-id="93f6b-190">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="93f6b-190">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="93f6b-191">c.</span><span class="sxs-lookup"><span data-stu-id="93f6b-191">c.</span></span> <span data-ttu-id="93f6b-192">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="93f6b-192">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="93f6b-193">d.</span><span class="sxs-lookup"><span data-stu-id="93f6b-193">d.</span></span> <span data-ttu-id="93f6b-194">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="93f6b-194">Click **Create**.</span></span>
 
### <a name="creating-a-competencyiq-test-user"></a><span data-ttu-id="93f6b-195">Skapa en testanvändare CompetencyIQ</span><span class="sxs-lookup"><span data-stu-id="93f6b-195">Creating a CompetencyIQ test user</span></span>

<span data-ttu-id="93f6b-196">tooenable Azure AD-användare toolog i tooCompetencyIQ, måste de etableras i CompetencyIQ.</span><span class="sxs-lookup"><span data-stu-id="93f6b-196">tooenable Azure AD users toolog in tooCompetencyIQ, they must be provisioned into CompetencyIQ.</span></span> <span data-ttu-id="93f6b-197">Kontakta [CompetencyIQ supportteamet](https://www.competencyiq.com/) toocreate användare i hello program.</span><span class="sxs-lookup"><span data-stu-id="93f6b-197">Contact [CompetencyIQ support team](https://www.competencyiq.com/) toocreate users in hello application.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="93f6b-198">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="93f6b-198">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="93f6b-199">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooCompetencyIQ.</span><span class="sxs-lookup"><span data-stu-id="93f6b-199">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCompetencyIQ.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="93f6b-201">**tooassign Britta Simon tooCompetencyIQ utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="93f6b-201">**tooassign Britta Simon tooCompetencyIQ, perform hello following steps:**</span></span>

1. <span data-ttu-id="93f6b-202">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="93f6b-202">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="93f6b-204">Välj i listan med program hello **CompetencyIQ**.</span><span class="sxs-lookup"><span data-stu-id="93f6b-204">In hello applications list, select **CompetencyIQ**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-competencyiq-tutorial/tutorial_competencyiq_app.png) 

3. <span data-ttu-id="93f6b-206">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="93f6b-206">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="93f6b-208">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="93f6b-208">Click **Add** button.</span></span> <span data-ttu-id="93f6b-209">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="93f6b-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="93f6b-211">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="93f6b-211">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="93f6b-212">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="93f6b-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="93f6b-213">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="93f6b-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="93f6b-214">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="93f6b-214">Testing single sign-on</span></span>

<span data-ttu-id="93f6b-215">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="93f6b-215">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="93f6b-216">När du klickar på hello CompetencyIQ panelen i hello åtkomstpanelen bör du få automatiskt inloggad i hello program.</span><span class="sxs-lookup"><span data-stu-id="93f6b-216">When you click hello CompetencyIQ tile in hello Access Panel, you should get automatically logged into hello application.</span></span>
<span data-ttu-id="93f6b-217">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="93f6b-217">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="93f6b-218">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="93f6b-218">Additional resources</span></span>

* [<span data-ttu-id="93f6b-219">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="93f6b-219">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="93f6b-220">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="93f6b-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-competencyiq-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-competencyiq-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-competencyiq-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-competencyiq-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-competencyiq-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-competencyiq-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-competencyiq-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-competencyiq-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-competencyiq-tutorial/tutorial_general_203.png

