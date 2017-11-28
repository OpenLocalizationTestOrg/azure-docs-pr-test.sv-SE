---
title: "Självstudier: Azure Active Directory-integrering med EmpCenter | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och EmpCenter."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a00ecf6e-917a-4284-b998-41506931585e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: cce5d86db550327eb73660fb78cea7e6d5428890
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-empcenter"></a><span data-ttu-id="f3e07-103">Självstudier: Azure Active Directory-integrering med EmpCenter</span><span class="sxs-lookup"><span data-stu-id="f3e07-103">Tutorial: Azure Active Directory integration with EmpCenter</span></span>

<span data-ttu-id="f3e07-104">I kursen får du lära dig hur toointegrate EmpCenter med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="f3e07-104">In this tutorial, you learn how toointegrate EmpCenter with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f3e07-105">Integrera EmpCenter med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="f3e07-105">Integrating EmpCenter with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="f3e07-106">Du kan styra i Azure AD som har åtkomst till tooEmpCenter</span><span class="sxs-lookup"><span data-stu-id="f3e07-106">You can control in Azure AD who has access tooEmpCenter</span></span>
- <span data-ttu-id="f3e07-107">Du kan aktivera din användare tooautomatically get inloggade tooEmpCenter (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="f3e07-107">You can enable your users tooautomatically get signed-on tooEmpCenter (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f3e07-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="f3e07-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="f3e07-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f3e07-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f3e07-110">Krav</span><span class="sxs-lookup"><span data-stu-id="f3e07-110">Prerequisites</span></span>

<span data-ttu-id="f3e07-111">tooconfigure Azure AD-integrering med EmpCenter, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="f3e07-111">tooconfigure Azure AD integration with EmpCenter, you need hello following items:</span></span>

- <span data-ttu-id="f3e07-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="f3e07-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f3e07-113">En EmpCenter enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="f3e07-113">An EmpCenter single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f3e07-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="f3e07-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f3e07-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="f3e07-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f3e07-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="f3e07-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f3e07-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f3e07-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f3e07-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="f3e07-118">Scenario description</span></span>
<span data-ttu-id="f3e07-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="f3e07-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f3e07-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="f3e07-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f3e07-121">Att lägga till EmpCenter från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="f3e07-121">Adding EmpCenter from hello gallery</span></span>
2. <span data-ttu-id="f3e07-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f3e07-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-empcenter-from-hello-gallery"></a><span data-ttu-id="f3e07-123">Att lägga till EmpCenter från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="f3e07-123">Adding EmpCenter from hello gallery</span></span>
<span data-ttu-id="f3e07-124">tooconfigure hello integrering av EmpCenter i Azure AD, behöver du tooadd EmpCenter hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="f3e07-124">tooconfigure hello integration of EmpCenter into Azure AD, you need tooadd EmpCenter from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="f3e07-125">**tooadd EmpCenter från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="f3e07-125">**tooadd EmpCenter from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="f3e07-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="f3e07-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f3e07-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="f3e07-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="f3e07-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="f3e07-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="f3e07-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f3e07-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="f3e07-133">Skriv i sökrutan hello **EmpCenter**.</span><span class="sxs-lookup"><span data-stu-id="f3e07-133">In hello search box, type **EmpCenter**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-EmpCenter-tutorial/tutorial_EmpCenter_search.png)

5. <span data-ttu-id="f3e07-135">Markera hello resultat på panelen **EmpCenter**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="f3e07-135">In hello results panel, select **EmpCenter**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-EmpCenter-tutorial/tutorial_EmpCenter_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f3e07-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f3e07-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f3e07-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med EmpCenter baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="f3e07-138">In this section, you configure and test Azure AD single sign-on with EmpCenter based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="f3e07-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i EmpCenter är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f3e07-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in EmpCenter is tooa user in Azure AD.</span></span> <span data-ttu-id="f3e07-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i EmpCenter toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="f3e07-140">In other words, a link relationship between an Azure AD user and hello related user in EmpCenter needs toobe established.</span></span>

<span data-ttu-id="f3e07-141">I EmpCenter, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="f3e07-141">In EmpCenter, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="f3e07-142">tooconfigure och testa Azure AD enkel inloggning med EmpCenter, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="f3e07-142">tooconfigure and test Azure AD single sign-on with EmpCenter, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="f3e07-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="f3e07-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="f3e07-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f3e07-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f3e07-145">**[Skapa en testanvändare EmpCenter](#creating-an-empcenter-test-user)**  -toohave en motsvarighet för Britta Simon i EmpCenter som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="f3e07-145">**[Creating an EmpCenter test user](#creating-an-empcenter-test-user)** - toohave a counterpart of Britta Simon in EmpCenter that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="f3e07-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="f3e07-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f3e07-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="f3e07-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f3e07-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f3e07-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f3e07-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt EmpCenter program.</span><span class="sxs-lookup"><span data-stu-id="f3e07-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your EmpCenter application.</span></span>

<span data-ttu-id="f3e07-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med EmpCenter:**</span><span class="sxs-lookup"><span data-stu-id="f3e07-150">**tooconfigure Azure AD single sign-on with EmpCenter, perform hello following steps:**</span></span>

1. <span data-ttu-id="f3e07-151">I hello Azure-portalen på hello **EmpCenter** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="f3e07-151">In hello Azure portal, on hello **EmpCenter** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="f3e07-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="f3e07-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-EmpCenter-tutorial/tutorial_EmpCenter_samlbase.png)

3. <span data-ttu-id="f3e07-155">På hello **EmpCenter domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="f3e07-155">On hello **EmpCenter Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-EmpCenter-tutorial/tutorial_EmpCenter_url.png)

    <span data-ttu-id="f3e07-157">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:</span><span class="sxs-lookup"><span data-stu-id="f3e07-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<subdomain>.EmpCenter.com/<instancename>` |
    | `https://<subdomain>.workforcehosting.com/<instancename>` |

    > [!NOTE] 
    > <span data-ttu-id="f3e07-158">hello-värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="f3e07-158">hello value is not real.</span></span> <span data-ttu-id="f3e07-159">Hello uppdateringsvärde med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="f3e07-159">Update hello value with hello actual Sign-On URL.</span></span> <span data-ttu-id="f3e07-160">Kontakta [EmpCenter klienten supportteamet](http://www.workforcesoftware.com/services/customer-support/) tooget hello värde.</span><span class="sxs-lookup"><span data-stu-id="f3e07-160">Contact [EmpCenter Client support team](http://www.workforcesoftware.com/services/customer-support/) tooget hello value.</span></span> 
 
4. <span data-ttu-id="f3e07-161">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="f3e07-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-EmpCenter-tutorial/tutorial_EmpCenter_certificate.png) 

5. <span data-ttu-id="f3e07-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="f3e07-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-EmpCenter-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="f3e07-165">tooconfigure enkel inloggning på **EmpCenter** sida, behöver du toosend hello hämtas **XML-Metadata för** för[EmpCenter supportteamet](http://www.workforcesoftware.com/services/customer-support/).</span><span class="sxs-lookup"><span data-stu-id="f3e07-165">tooconfigure single sign-on on **EmpCenter** side, you need toosend hello downloaded **Metadata XML** too[EmpCenter support team](http://www.workforcesoftware.com/services/customer-support/).</span></span> <span data-ttu-id="f3e07-166">De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="f3e07-166">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="f3e07-167">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="f3e07-167">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="f3e07-168">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="f3e07-168">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="f3e07-169">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f3e07-169">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f3e07-170">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="f3e07-170">Creating an Azure AD test user</span></span>
<span data-ttu-id="f3e07-171">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f3e07-171">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="f3e07-173">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="f3e07-173">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="f3e07-174">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="f3e07-174">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-EmpCenter-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f3e07-176">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="f3e07-176">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-EmpCenter-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f3e07-178">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f3e07-178">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-EmpCenter-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f3e07-180">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="f3e07-180">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-EmpCenter-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f3e07-182">a.</span><span class="sxs-lookup"><span data-stu-id="f3e07-182">a.</span></span> <span data-ttu-id="f3e07-183">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f3e07-183">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f3e07-184">b.</span><span class="sxs-lookup"><span data-stu-id="f3e07-184">b.</span></span> <span data-ttu-id="f3e07-185">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f3e07-185">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f3e07-186">c.</span><span class="sxs-lookup"><span data-stu-id="f3e07-186">c.</span></span> <span data-ttu-id="f3e07-187">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="f3e07-187">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="f3e07-188">d.</span><span class="sxs-lookup"><span data-stu-id="f3e07-188">d.</span></span> <span data-ttu-id="f3e07-189">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="f3e07-189">Click **Create**.</span></span>
 
### <a name="creating-an-empcenter-test-user"></a><span data-ttu-id="f3e07-190">Skapa en testanvändare EmpCenter</span><span class="sxs-lookup"><span data-stu-id="f3e07-190">Creating an EmpCenter test user</span></span>

<span data-ttu-id="f3e07-191">I ordning tooenable Azure AD-användare toolog i tooEmpCenter, måste de etableras i EmpCenter.</span><span class="sxs-lookup"><span data-stu-id="f3e07-191">In order tooenable Azure AD users toolog in tooEmpCenter, they must be provisioned into EmpCenter.</span></span> <span data-ttu-id="f3e07-192">Hello gäller EmpCenter, hello användarkonton måste toobe som skapats av din [EmpCenter supportteamet](http://www.workforcesoftware.com/services/customer-support/).</span><span class="sxs-lookup"><span data-stu-id="f3e07-192">In hello case of EmpCenter, hello user accounts need toobe created by your [EmpCenter support team](http://www.workforcesoftware.com/services/customer-support/).</span></span>

> [!NOTE]
> <span data-ttu-id="f3e07-193">Du kan använda något annat EmpCenter användarens konto skapas verktyg eller API: er som tillhandahålls av EmpCenter tooprovision Azure Active Directory användarkonton.</span><span class="sxs-lookup"><span data-stu-id="f3e07-193">You can use any other EmpCenter user account creation tools or APIs provided by EmpCenter tooprovision Azure Active Directory user accounts.</span></span>
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="f3e07-194">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="f3e07-194">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="f3e07-195">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooEmpCenter.</span><span class="sxs-lookup"><span data-stu-id="f3e07-195">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooEmpCenter.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="f3e07-197">**tooassign Britta Simon tooEmpCenter utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="f3e07-197">**tooassign Britta Simon tooEmpCenter, perform hello following steps:**</span></span>

1. <span data-ttu-id="f3e07-198">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="f3e07-198">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="f3e07-200">Välj i listan med program hello **EmpCenter**.</span><span class="sxs-lookup"><span data-stu-id="f3e07-200">In hello applications list, select **EmpCenter**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-EmpCenter-tutorial/tutorial_EmpCenter_app.png) 

3. <span data-ttu-id="f3e07-202">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="f3e07-202">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="f3e07-204">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="f3e07-204">Click **Add** button.</span></span> <span data-ttu-id="f3e07-205">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f3e07-205">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="f3e07-207">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="f3e07-207">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="f3e07-208">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f3e07-208">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f3e07-209">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f3e07-209">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f3e07-210">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f3e07-210">Testing single sign-on</span></span>


<span data-ttu-id="f3e07-211">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="f3e07-211">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="f3e07-212">Du bör få automatiskt inloggade tooyour EmpCenter programmet när du klickar på hello EmpCenter panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="f3e07-212">When you click hello EmpCenter tile in hello Access Panel, you should get automatically signed-on tooyour EmpCenter application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f3e07-213">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="f3e07-213">Additional resources</span></span>

* [<span data-ttu-id="f3e07-214">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f3e07-214">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f3e07-215">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="f3e07-215">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-EmpCenter-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-EmpCenter-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-EmpCenter-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-EmpCenter-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-EmpCenter-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-EmpCenter-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-EmpCenter-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-EmpCenter-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-EmpCenter-tutorial/tutorial_general_203.png

