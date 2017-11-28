---
title: "Självstudier: Azure Active Directory-integrering med Icertis kontraktet plattform | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Icertis plattform för kontraktet."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6627e6dd-f559-4cd4-a509-f6d9a4961b49
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: 6eb99553ef9df732512a38fb0489e66b41ba94a2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-icertis-contract-management-platform"></a><span data-ttu-id="a34a7-103">Självstudier: Azure Active Directory-integrering med Icertis kontraktet plattform</span><span class="sxs-lookup"><span data-stu-id="a34a7-103">Tutorial: Azure Active Directory integration with Icertis Contract Management Platform</span></span>

<span data-ttu-id="a34a7-104">I kursen får du lära dig hur toointegrate Icertis kontraktet plattform med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="a34a7-104">In this tutorial, you learn how toointegrate Icertis Contract Management Platform with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a34a7-105">Integrera Icertis plattform för hantering av kontrakt med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="a34a7-105">Integrating Icertis Contract Management Platform with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="a34a7-106">Du kan styra i Azure AD som har åtkomst tooIcertis plattform för hantering av kontrakt</span><span class="sxs-lookup"><span data-stu-id="a34a7-106">You can control in Azure AD who has access tooIcertis Contract Management Platform</span></span>
- <span data-ttu-id="a34a7-107">Du kan aktivera din användare tooautomatically get inloggade tooIcertis plattform för kontrakt (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="a34a7-107">You can enable your users tooautomatically get signed-on tooIcertis Contract Management Platform (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a34a7-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="a34a7-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="a34a7-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a34a7-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a34a7-110">Krav</span><span class="sxs-lookup"><span data-stu-id="a34a7-110">Prerequisites</span></span>

<span data-ttu-id="a34a7-111">tooconfigure Azure AD-integrering med Icertis plattform för hantering av kontrakt, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="a34a7-111">tooconfigure Azure AD integration with Icertis Contract Management Platform, you need hello following items:</span></span>

- <span data-ttu-id="a34a7-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="a34a7-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a34a7-113">En plattform för hantering av Icertis kontraktet enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="a34a7-113">An Icertis Contract Management Platform single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a34a7-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="a34a7-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a34a7-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="a34a7-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a34a7-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="a34a7-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a34a7-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a34a7-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a34a7-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="a34a7-118">Scenario description</span></span>
<span data-ttu-id="a34a7-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="a34a7-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a34a7-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="a34a7-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a34a7-121">Att lägga till Icertis plattform för hantering av kontrakt från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="a34a7-121">Adding Icertis Contract Management Platform from hello gallery</span></span>
2. <span data-ttu-id="a34a7-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a34a7-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-icertis-contract-management-platform-from-hello-gallery"></a><span data-ttu-id="a34a7-123">Att lägga till Icertis plattform för hantering av kontrakt från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="a34a7-123">Adding Icertis Contract Management Platform from hello gallery</span></span>
<span data-ttu-id="a34a7-124">tooconfigure hello integrering av Icertis plattform för hantering av kontrakt i Azure AD, behöver du tooadd Icertis plattform för hantering av kontrakt hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="a34a7-124">tooconfigure hello integration of Icertis Contract Management Platform into Azure AD, you need tooadd Icertis Contract Management Platform from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="a34a7-125">**tooadd Icertis kontraktet plattform från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a34a7-125">**tooadd Icertis Contract Management Platform from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="a34a7-126">I hello ** [Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a34a7-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a34a7-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="a34a7-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="a34a7-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="a34a7-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="a34a7-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a34a7-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="a34a7-133">Skriv i sökrutan hello **Icertis plattform för hantering av kontrakt**.</span><span class="sxs-lookup"><span data-stu-id="a34a7-133">In hello search box, type **Icertis Contract Management Platform**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-icertisicm-tutorial/tutorial_icertisicm_search.png)

5. <span data-ttu-id="a34a7-135">Markera hello resultat på panelen **Icertis plattform för hantering av kontrakt**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="a34a7-135">In hello results panel, select **Icertis Contract Management Platform**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-icertisicm-tutorial/tutorial_icertisicm_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a34a7-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a34a7-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a34a7-138">I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med Icertis kontraktet plattform för hantering av baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="a34a7-138">In this section, you configure and test Azure AD single sign-on with Icertis Contract Management Platform based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a34a7-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Icertis plattform för hantering av kontrakt är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a34a7-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Icertis Contract Management Platform is tooa user in Azure AD.</span></span> <span data-ttu-id="a34a7-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Icertis plattform för hantering av kontrakt toobe upprätta.</span><span class="sxs-lookup"><span data-stu-id="a34a7-140">In other words, a link relationship between an Azure AD user and hello related user in Icertis Contract Management Platform needs toobe established.</span></span>

<span data-ttu-id="a34a7-141">I Icertis plattform för kontrakt, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="a34a7-141">In Icertis Contract Management Platform, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="a34a7-142">tooconfigure och testa Azure AD enkel inloggning med Icertis plattform för hantering av kontrakt, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="a34a7-142">tooconfigure and test Azure AD single sign-on with Icertis Contract Management Platform, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="a34a7-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on) ** -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="a34a7-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="a34a7-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user) ** -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a34a7-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a34a7-145">**[Skapa en testanvändare Icertis plattform för hantering av kontrakt](#creating-an-icertis-contract-management-platform-test-user) ** -toohave en motsvarighet för Britta Simon i Icertis kontrakt-plattform som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="a34a7-145">**[Creating an Icertis Contract Management Platform test user](#creating-an-icertis-contract-management-platform-test-user)** - toohave a counterpart of Britta Simon in Icertis Contract Management Platform that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="a34a7-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user) ** -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a34a7-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a34a7-147">**[Testa enkel inloggning](#testing-single-sign-on) ** -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="a34a7-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a34a7-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a34a7-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a34a7-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Icertis plattform för kontraktet.</span><span class="sxs-lookup"><span data-stu-id="a34a7-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Icertis Contract Management Platform application.</span></span>

<span data-ttu-id="a34a7-150">**tooconfigure Azure AD enkel inloggning med Icertis plattform för kontrakt, utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a34a7-150">**tooconfigure Azure AD single sign-on with Icertis Contract Management Platform, perform hello following steps:**</span></span>

1. <span data-ttu-id="a34a7-151">I hello Azure-portalen på hello **Icertis plattform för hantering av kontrakt** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="a34a7-151">In hello Azure portal, on hello **Icertis Contract Management Platform** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="a34a7-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a34a7-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-icertisicm-tutorial/tutorial_icertisicm_samlbase.png)

3. <span data-ttu-id="a34a7-155">På hello **URL: er och domänen för plattform för hantering av kontrakt Icertis** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a34a7-155">On hello **Icertis Contract Management Platform Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-icertisicm-tutorial/tutorial_icertisicm_url.png)

    <span data-ttu-id="a34a7-157">a.</span><span class="sxs-lookup"><span data-stu-id="a34a7-157">a.</span></span> <span data-ttu-id="a34a7-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.icertis.com`</span><span class="sxs-lookup"><span data-stu-id="a34a7-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<company name>.icertis.com`</span></span>

    <span data-ttu-id="a34a7-159">b.</span><span class="sxs-lookup"><span data-stu-id="a34a7-159">b.</span></span> <span data-ttu-id="a34a7-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.icertis.com`</span><span class="sxs-lookup"><span data-stu-id="a34a7-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<company name>.icertis.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="a34a7-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="a34a7-161">These values are not real.</span></span> <span data-ttu-id="a34a7-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="a34a7-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="a34a7-163">Kontakta [Icertis kontraktet Hanteringsklient plattform supportteamet](https://www.icertis.com/company/contact/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="a34a7-163">Contact [Icertis Contract Management Platform Client support team](https://www.icertis.com/company/contact/) tooget these values.</span></span> 

4. <span data-ttu-id="a34a7-164">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="a34a7-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-icertisicm-tutorial/tutorial_icertisicm_certificate.png) 

5. <span data-ttu-id="a34a7-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="a34a7-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-icertisicm-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="a34a7-168">På hello **Icertis kontraktet Management plattformskonfigurationen** klickar du på **konfigurera Icertis kontraktet plattform för hantering av** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="a34a7-168">On hello **Icertis Contract Management Platform Configuration** section, click **Configure Icertis Contract Management Platform** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="a34a7-169">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="a34a7-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-icertisicm-tutorial/tutorial_icertisicm_configure.png) 

7. <span data-ttu-id="a34a7-171">tooconfigure enkel inloggning på **Icertis plattform för hantering av kontrakt** sida, behöver du toosend hello hämtas **XML-Metadata för** och **Sign-Out URL SAML enhets-ID och SAML enkel inloggning Tjänst-URL** för[Icertis plattform för hantering av kontrakt supportteamet](https://www.icertis.com/company/contact/).</span><span class="sxs-lookup"><span data-stu-id="a34a7-171">tooconfigure single sign-on on **Icertis Contract Management Platform** side, you need toosend hello downloaded **Metadata XML** and **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[Icertis Contract Management Platform support team](https://www.icertis.com/company/contact/).</span></span>

> [!TIP]
> <span data-ttu-id="a34a7-172">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="a34a7-172">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="a34a7-173">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello ** Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="a34a7-173">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="a34a7-174">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a34a7-174">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a34a7-175">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="a34a7-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="a34a7-176">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a34a7-176">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="a34a7-178">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a34a7-178">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="a34a7-179">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a34a7-179">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-icertisicm-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a34a7-181">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="a34a7-181">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-icertisicm-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a34a7-183">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a34a7-183">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-icertisicm-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a34a7-185">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a34a7-185">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-icertisicm-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a34a7-187">a.</span><span class="sxs-lookup"><span data-stu-id="a34a7-187">a.</span></span> <span data-ttu-id="a34a7-188">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a34a7-188">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a34a7-189">b.</span><span class="sxs-lookup"><span data-stu-id="a34a7-189">b.</span></span> <span data-ttu-id="a34a7-190">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a34a7-190">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a34a7-191">c.</span><span class="sxs-lookup"><span data-stu-id="a34a7-191">c.</span></span> <span data-ttu-id="a34a7-192">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="a34a7-192">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="a34a7-193">d.</span><span class="sxs-lookup"><span data-stu-id="a34a7-193">d.</span></span> <span data-ttu-id="a34a7-194">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="a34a7-194">Click **Create**.</span></span>
 
### <a name="creating-an-icertis-contract-management-platform-test-user"></a><span data-ttu-id="a34a7-195">Skapa en testanvändare Icertis plattform för hantering av kontrakt</span><span class="sxs-lookup"><span data-stu-id="a34a7-195">Creating an Icertis Contract Management Platform test user</span></span>

<span data-ttu-id="a34a7-196">I det här avsnittet kan du skapa en användare som kallas Britta Simon i Icertis plattform för kontraktet.</span><span class="sxs-lookup"><span data-stu-id="a34a7-196">In this section, you create a user called Britta Simon in Icertis Contract Management Platform.</span></span> <span data-ttu-id="a34a7-197">Se tillsammans med [Icertis plattform för hantering av kontrakt supportteamet](https://www.icertis.com/company/contact/) tooadd hello användare i hello Icertis plattform för kontraktet.</span><span class="sxs-lookup"><span data-stu-id="a34a7-197">Please work with [Icertis Contract Management Platform support team](https://www.icertis.com/company/contact/) tooadd hello users in hello Icertis Contract Management Platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="a34a7-198">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="a34a7-198">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="a34a7-199">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooIcertis plattform för kontraktet.</span><span class="sxs-lookup"><span data-stu-id="a34a7-199">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooIcertis Contract Management Platform.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="a34a7-201">**tooassign Britta Simon tooIcertis plattform för hantering av kontrakt, utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a34a7-201">**tooassign Britta Simon tooIcertis Contract Management Platform, perform hello following steps:**</span></span>

1. <span data-ttu-id="a34a7-202">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="a34a7-202">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="a34a7-204">Välj i listan med program hello **Icertis plattform för hantering av kontrakt**.</span><span class="sxs-lookup"><span data-stu-id="a34a7-204">In hello applications list, select **Icertis Contract Management Platform**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-icertisicm-tutorial/tutorial_icertisicm_app.png) 

3. <span data-ttu-id="a34a7-206">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="a34a7-206">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="a34a7-208">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="a34a7-208">Click **Add** button.</span></span> <span data-ttu-id="a34a7-209">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a34a7-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="a34a7-211">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="a34a7-211">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="a34a7-212">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a34a7-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a34a7-213">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a34a7-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a34a7-214">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a34a7-214">Testing single sign-on</span></span>

<span data-ttu-id="a34a7-215">hello syftet med det här avsnittet är tootest din Azure AD SSO konfiguration av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="a34a7-215">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="a34a7-216">Du bör få automatiskt inloggade tooyour Icertis plattform för hantering av kontrakt programmet när du klickar på hello Icertis plattform för hantering av kontrakt panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="a34a7-216">When you click hello Icertis Contract Management Platform tile in hello Access Panel, you should get automatically signed-on tooyour Icertis Contract Management Platform application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a34a7-217">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="a34a7-217">Additional resources</span></span>

* [<span data-ttu-id="a34a7-218">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a34a7-218">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a34a7-219">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a34a7-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_203.png

