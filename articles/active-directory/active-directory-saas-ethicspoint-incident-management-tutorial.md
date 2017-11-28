---
title: "Självstudier: Azure Active Directory-integrering med EthicsPoint Incident Management (EPIM) | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och EthicsPoint Incident Management (EPIM)."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8cb31a4c-9309-469b-93ac-daf0d3c7a3e6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: 73ef5fab815cddb3728f4b23173f99e62aec5bd0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ethicspoint-incident-management-epim"></a><span data-ttu-id="d8ca5-103">Självstudier: Azure Active Directory-integrering med EthicsPoint Incident Management (EPIM)</span><span class="sxs-lookup"><span data-stu-id="d8ca5-103">Tutorial: Azure Active Directory integration with EthicsPoint Incident Management (EPIM)</span></span>

<span data-ttu-id="d8ca5-104">I kursen får du lära dig hur toointegrate EthicsPoint Incident Management (EPIM) med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="d8ca5-104">In this tutorial, you learn how toointegrate EthicsPoint Incident Management (EPIM) with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d8ca5-105">Integrera EthicsPoint Incident Management (EPIM) med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="d8ca5-105">Integrating EthicsPoint Incident Management (EPIM) with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="d8ca5-106">Du kan styra i Azure AD som har åtkomst tooEthicsPoint Incident Management (EPIM)</span><span class="sxs-lookup"><span data-stu-id="d8ca5-106">You can control in Azure AD who has access tooEthicsPoint Incident Management (EPIM)</span></span>
- <span data-ttu-id="d8ca5-107">Du kan aktivera din användare tooautomatically get inloggade tooEthicsPoint Incident Management (EPIM) (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="d8ca5-107">You can enable your users tooautomatically get signed-on tooEthicsPoint Incident Management (EPIM) (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d8ca5-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="d8ca5-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="d8ca5-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d8ca5-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d8ca5-110">Krav</span><span class="sxs-lookup"><span data-stu-id="d8ca5-110">Prerequisites</span></span>

<span data-ttu-id="d8ca5-111">tooconfigure Azure AD-integrering med EthicsPoint Incident Management (EPIM), behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="d8ca5-111">tooconfigure Azure AD integration with EthicsPoint Incident Management (EPIM), you need hello following items:</span></span>

- <span data-ttu-id="d8ca5-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="d8ca5-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d8ca5-113">En EthicsPoint Incident Management (EPIM) enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="d8ca5-113">A EthicsPoint Incident Management (EPIM) single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d8ca5-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d8ca5-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="d8ca5-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d8ca5-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d8ca5-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d8ca5-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d8ca5-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="d8ca5-118">Scenario description</span></span>
<span data-ttu-id="d8ca5-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d8ca5-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="d8ca5-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d8ca5-121">Att lägga till EthicsPoint Incident Management (EPIM) från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="d8ca5-121">Adding EthicsPoint Incident Management (EPIM) from hello gallery</span></span>
2. <span data-ttu-id="d8ca5-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d8ca5-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-ethicspoint-incident-management-epim-from-hello-gallery"></a><span data-ttu-id="d8ca5-123">Att lägga till EthicsPoint Incident Management (EPIM) från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="d8ca5-123">Adding EthicsPoint Incident Management (EPIM) from hello gallery</span></span>
<span data-ttu-id="d8ca5-124">tooconfigure hello integrering av EthicsPoint Incident Management (EPIM) i Azure AD, behöver du tooadd EthicsPoint Incident Management (EPIM) från hello galleriet tooyour lista över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-124">tooconfigure hello integration of EthicsPoint Incident Management (EPIM) into Azure AD, you need tooadd EthicsPoint Incident Management (EPIM) from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="d8ca5-125">**tooadd EthicsPoint Incident Management (EPIM) från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d8ca5-125">**tooadd EthicsPoint Incident Management (EPIM) from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="d8ca5-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d8ca5-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="d8ca5-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="d8ca5-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="d8ca5-133">Skriv i sökrutan hello **EthicsPoint Incident Management (EPIM)**.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-133">In hello search box, type **EthicsPoint Incident Management (EPIM)**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_ethicspoint_search.png)

5. <span data-ttu-id="d8ca5-135">Markera hello resultat på panelen **EthicsPoint Incident Management (EPIM)**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-135">In hello results panel, select **EthicsPoint Incident Management (EPIM)**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_ethicspoint_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d8ca5-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d8ca5-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d8ca5-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med EthicsPoint Incident Management (EPIM) baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-138">In this section, you configure and test Azure AD single sign-on with EthicsPoint Incident Management (EPIM) based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="d8ca5-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i EthicsPoint Incident Management (EPIM) är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in EthicsPoint Incident Management (EPIM) is tooa user in Azure AD.</span></span> <span data-ttu-id="d8ca5-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i EthicsPoint Incident Management (EPIM) toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-140">In other words, a link relationship between an Azure AD user and hello related user in EthicsPoint Incident Management (EPIM) needs toobe established.</span></span>

<span data-ttu-id="d8ca5-141">I EthicsPoint Incident Management (EPIM) tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-141">In EthicsPoint Incident Management (EPIM), assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="d8ca5-142">tooconfigure och testa Azure AD enkel inloggning med EthicsPoint Incident Management (EPIM), behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="d8ca5-142">tooconfigure and test Azure AD single sign-on with EthicsPoint Incident Management (EPIM), you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="d8ca5-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="d8ca5-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d8ca5-145">**[Skapa en testanvändare EthicsPoint Incident Management (EPIM)](#creating-a-ethicspoint-incident-management-epim-test-user)**  -toohave en motsvarighet för Britta Simon i EthicsPoint Incident Management (EPIM) och som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-145">**[Creating a EthicsPoint Incident Management (EPIM) test user](#creating-a-ethicspoint-incident-management-epim-test-user)** - toohave a counterpart of Britta Simon in EthicsPoint Incident Management (EPIM) that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="d8ca5-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d8ca5-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d8ca5-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d8ca5-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d8ca5-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet EthicsPoint Incident Management (EPIM).</span><span class="sxs-lookup"><span data-stu-id="d8ca5-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your EthicsPoint Incident Management (EPIM) application.</span></span>

<span data-ttu-id="d8ca5-150">**tooconfigure Azure AD enkel inloggning med EthicsPoint Incident Management (EPIM) utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d8ca5-150">**tooconfigure Azure AD single sign-on with EthicsPoint Incident Management (EPIM), perform hello following steps:**</span></span>

1. <span data-ttu-id="d8ca5-151">I hello Azure-portalen på hello **EthicsPoint Incident Management (EPIM)** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-151">In hello Azure portal, on hello **EthicsPoint Incident Management (EPIM)** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="d8ca5-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_ethicspoint_samlbase.png)

3. <span data-ttu-id="d8ca5-155">På hello **EthicsPoint Incident Management (EPIM)-domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="d8ca5-155">On hello **EthicsPoint Incident Management (EPIM) Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_ethicspoint_url.png)

    <span data-ttu-id="d8ca5-157">a.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-157">a.</span></span> <span data-ttu-id="d8ca5-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:</span><span class="sxs-lookup"><span data-stu-id="d8ca5-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.navexglobal.com`|
    | `https://<companyname>.ethicspointvp.com`|

    <span data-ttu-id="d8ca5-159">b.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-159">b.</span></span> <span data-ttu-id="d8ca5-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.navexglobal.com/adfs/services/trust`</span><span class="sxs-lookup"><span data-stu-id="d8ca5-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.navexglobal.com/adfs/services/trust`</span></span>

    <span data-ttu-id="d8ca5-161">c.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-161">c.</span></span> <span data-ttu-id="d8ca5-162">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<servername>.navexglobal.com/adfs/ls/`</span><span class="sxs-lookup"><span data-stu-id="d8ca5-162">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<servername>.navexglobal.com/adfs/ls/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d8ca5-163">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-163">These values are not real.</span></span> <span data-ttu-id="d8ca5-164">Uppdatera dessa värden med hello faktiska Reply URL och identifierare inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-164">Update these values with hello actual Reply URL, Identifier, and Sign-On URL.</span></span> <span data-ttu-id="d8ca5-165">Kontakta [EthicsPoint Incident Management (EPIM) klienten supportteamet](http://www.navexglobal.com/company/contact-us) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-165">Contact [EthicsPoint Incident Management (EPIM) Client support team](http://www.navexglobal.com/company/contact-us) tooget these values.</span></span> 

4. <span data-ttu-id="d8ca5-166">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-166">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_ethicspoint_certificate.png) 

5. <span data-ttu-id="d8ca5-168">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-168">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="d8ca5-170">tooconfigure enkel inloggning på **EthicsPoint Incident Management (EPIM)** sida, behöver du toosend hello hämtas **XML-Metadata för** för[EthicsPoint Incident Management (EPIM)-support team](http://www.navexglobal.com/company/contact-us).</span><span class="sxs-lookup"><span data-stu-id="d8ca5-170">tooconfigure single sign-on on **EthicsPoint Incident Management (EPIM)** side, you need toosend hello downloaded **Metadata XML** too[EthicsPoint Incident Management (EPIM) support team](http://www.navexglobal.com/company/contact-us).</span></span>

> [!TIP]
> <span data-ttu-id="d8ca5-171">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="d8ca5-171">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="d8ca5-172">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-172">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="d8ca5-173">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d8ca5-173">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d8ca5-174">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="d8ca5-174">Creating an Azure AD test user</span></span>
<span data-ttu-id="d8ca5-175">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-175">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="d8ca5-177">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d8ca5-177">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="d8ca5-178">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-178">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ethicspoint-incident-management-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d8ca5-180">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-180">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ethicspoint-incident-management-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d8ca5-182">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-182">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ethicspoint-incident-management-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d8ca5-184">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="d8ca5-184">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ethicspoint-incident-management-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d8ca5-186">a.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-186">a.</span></span> <span data-ttu-id="d8ca5-187">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-187">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d8ca5-188">b.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-188">b.</span></span> <span data-ttu-id="d8ca5-189">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-189">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d8ca5-190">c.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-190">c.</span></span> <span data-ttu-id="d8ca5-191">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-191">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="d8ca5-192">d.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-192">d.</span></span> <span data-ttu-id="d8ca5-193">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-193">Click **Create**.</span></span>
 
### <a name="creating-a-ethicspoint-incident-management-epim-test-user"></a><span data-ttu-id="d8ca5-194">Skapa en testanvändare EthicsPoint Incident Management (EPIM)</span><span class="sxs-lookup"><span data-stu-id="d8ca5-194">Creating a EthicsPoint Incident Management (EPIM) test user</span></span>

<span data-ttu-id="d8ca5-195">I det här avsnittet skapar du en användare som kallas Britta Simon i EthicsPoint Incident Management (EPIM).</span><span class="sxs-lookup"><span data-stu-id="d8ca5-195">In this section, you create a user called Britta Simon in EthicsPoint Incident Management (EPIM).</span></span> <span data-ttu-id="d8ca5-196">Se tillsammans med [EthicsPoint Incident Management (EPIM) supportteamet](http://www.navexglobal.com/company/contact-us) tooadd hello användare i hello EthicsPoint Incident Management (EPIM)-plattformen.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-196">Please work with [EthicsPoint Incident Management (EPIM) support team](http://www.navexglobal.com/company/contact-us) tooadd hello users in hello EthicsPoint Incident Management (EPIM) platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="d8ca5-197">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="d8ca5-197">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="d8ca5-198">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooEthicsPoint Incident Management (EPIM).</span><span class="sxs-lookup"><span data-stu-id="d8ca5-198">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooEthicsPoint Incident Management (EPIM).</span></span>

![Tilldela användare][200] 

<span data-ttu-id="d8ca5-200">**tooassign Britta Simon tooEthicsPoint Incident Management (EPIM), utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="d8ca5-200">**tooassign Britta Simon tooEthicsPoint Incident Management (EPIM), perform hello following steps:**</span></span>

1. <span data-ttu-id="d8ca5-201">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-201">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="d8ca5-203">Välj i listan med program hello **EthicsPoint Incident Management (EPIM)**.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-203">In hello applications list, select **EthicsPoint Incident Management (EPIM)**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_ethicspoint_app.png) 

3. <span data-ttu-id="d8ca5-205">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-205">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="d8ca5-207">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-207">Click **Add** button.</span></span> <span data-ttu-id="d8ca5-208">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="d8ca5-210">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-210">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="d8ca5-211">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d8ca5-212">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d8ca5-213">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d8ca5-213">Testing single sign-on</span></span>

<span data-ttu-id="d8ca5-214">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-214">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>
<span data-ttu-id="d8ca5-215">När du klickar på hello EthicsPoint Incident Management (EPIM)-panelen i hello åtkomstpanelen får automatiskt inloggade tooyour EthicsPoint Incident Management (EPIM) program.</span><span class="sxs-lookup"><span data-stu-id="d8ca5-215">When you click hello EthicsPoint Incident Management (EPIM) tile in hello Access Panel, you should get automatically signed-on tooyour EthicsPoint Incident Management (EPIM) application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d8ca5-216">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="d8ca5-216">Additional resources</span></span>

* [<span data-ttu-id="d8ca5-217">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d8ca5-217">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d8ca5-218">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d8ca5-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_203.png

