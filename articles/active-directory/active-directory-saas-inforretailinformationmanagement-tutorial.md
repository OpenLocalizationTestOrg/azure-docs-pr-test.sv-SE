---
title: "Självstudier: Azure Active Directory-integrering med Infor Retail – informationshantering | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Infor Retail – informationshantering."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 5ff49168-ef81-4169-8e5e-dc86e24dd5e5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: 9cd8ab65d41d01832e0611f7f8254aa257120508
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-infor-retail--information-management"></a><span data-ttu-id="0d2e7-103">Självstudier: Azure Active Directory-integrering med Infor Retail – informationshantering</span><span class="sxs-lookup"><span data-stu-id="0d2e7-103">Tutorial: Azure Active Directory integration with Infor Retail – Information Management</span></span>

<span data-ttu-id="0d2e7-104">I kursen får du lära dig hur toointegrate Infor Retail – informationshantering med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="0d2e7-104">In this tutorial, you learn how toointegrate Infor Retail – Information Management with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="0d2e7-105">Integrera Infor Retail – informationshantering med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="0d2e7-105">Integrating Infor Retail – Information Management with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="0d2e7-106">Du kan styra i Azure AD som har åtkomst tooInfor Retail – informationshantering.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-106">You can control in Azure AD who has access tooInfor Retail – Information Management.</span></span>
- <span data-ttu-id="0d2e7-107">Du kan låta dina användare tooautomatically get inloggade tooInfor Retail – informationshantering (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-107">You can enable your users tooautomatically get signed-on tooInfor Retail – Information Management (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="0d2e7-108">Du kan hantera dina konton i en central plats - hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="0d2e7-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0d2e7-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0d2e7-110">Krav</span><span class="sxs-lookup"><span data-stu-id="0d2e7-110">Prerequisites</span></span>

<span data-ttu-id="0d2e7-111">tooconfigure Azure AD-integrering med Infor Retail-hantering, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="0d2e7-111">tooconfigure Azure AD integration with Infor Retail – Information Management, you need hello following items:</span></span>

- <span data-ttu-id="0d2e7-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="0d2e7-112">An Azure AD subscription</span></span>
- <span data-ttu-id="0d2e7-113">En Infor Retail – informationshantering enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="0d2e7-113">An Infor Retail – Information Management single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="0d2e7-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="0d2e7-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="0d2e7-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="0d2e7-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="0d2e7-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0d2e7-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="0d2e7-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="0d2e7-118">Scenario description</span></span>
<span data-ttu-id="0d2e7-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="0d2e7-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="0d2e7-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="0d2e7-121">Lägga till Infor Retail – informationshantering från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="0d2e7-121">Adding Infor Retail – Information Management from hello gallery</span></span>
2. <span data-ttu-id="0d2e7-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0d2e7-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-infor-retail--information-management-from-hello-gallery"></a><span data-ttu-id="0d2e7-123">Lägga till Infor Retail – informationshantering från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="0d2e7-123">Adding Infor Retail – Information Management from hello gallery</span></span>
<span data-ttu-id="0d2e7-124">tooconfigure hello integrering av Infor Retail – informationshantering till Azure AD måste tooadd Infor Retail – informationshantering hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-124">tooconfigure hello integration of Infor Retail – Information Management into Azure AD, you need tooadd Infor Retail – Information Management from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="0d2e7-125">**tooadd Infor Retail – informationshantering från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="0d2e7-125">**tooadd Infor Retail – Information Management from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="0d2e7-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="0d2e7-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="0d2e7-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="0d2e7-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="0d2e7-133">Skriv i sökrutan hello **Infor Retail – informationshantering**väljer **Infor Retail – informationshantering** resultatet-panelen klickar **Lägg till** knappen tooadd hello programmet.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-133">In hello search box, type **Infor Retail – Information Management**, select **Infor Retail – Information Management** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Infor Retail – hantering av Information i hello resultatlistan](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="0d2e7-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0d2e7-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="0d2e7-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Infor Retail – informationshantering baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-136">In this section, you configure and test Azure AD single sign-on with Infor Retail – Information Management based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="0d2e7-137">Azure AD måste tooknow vilka hello motsvarighet användare i Infor Retail-informationshantering är tooa användare i Azure AD för enkel inloggning toowork.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Infor Retail – Information Management is tooa user in Azure AD.</span></span> <span data-ttu-id="0d2e7-138">Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i Infor Retail – informationshantering toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-138">In other words, a link relationship between an Azure AD user and hello related user in Infor Retail – Information Management needs toobe established.</span></span>

<span data-ttu-id="0d2e7-139">I Infor Retail-hantering, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-139">In Infor Retail – Information Management, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="0d2e7-140">tooconfigure och testa Azure AD enkel inloggning med Infor Retail-hantering, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="0d2e7-140">tooconfigure and test Azure AD single sign-on with Infor Retail – Information Management, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="0d2e7-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="0d2e7-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="0d2e7-143">**[Skapa en Infor Retail – informationshantering testanvändare](#create-an-infor-retail--information-management-test-user)**  - toohave en motsvarighet för Britta Simon i Infor Retail – hantering av Information som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-143">**[Create an Infor Retail – Information Management test user](#create-an-infor-retail--information-management-test-user)** - toohave a counterpart of Britta Simon in Infor Retail – Information Management that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="0d2e7-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="0d2e7-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="0d2e7-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0d2e7-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="0d2e7-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i din Infor Retail – Information hanteringsprogram.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Infor Retail – Information Management application.</span></span>

<span data-ttu-id="0d2e7-148">**tooconfigure Azure AD enkel inloggning med Infor Retail-hantering, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="0d2e7-148">**tooconfigure Azure AD single sign-on with Infor Retail – Information Management, perform hello following steps:**</span></span>

1. <span data-ttu-id="0d2e7-149">I hello Azure-portalen på hello **Infor Retail – informationshantering** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-149">In hello Azure portal, on hello **Infor Retail – Information Management** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="0d2e7-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_samlbase.png)

3. <span data-ttu-id="0d2e7-153">På hello **Infor Retail-URL: er och domänen för hantering av Information** avsnittet, utför följande steg om du inte vill tooconfigure hello programmet i IDP initierade läge hello:</span><span class="sxs-lookup"><span data-stu-id="0d2e7-153">On hello **Infor Retail – Information Management Domain and URLs** section, perform hello following steps if you wish tooconfigure hello application in IDP initiated mode:</span></span>

    ![Med enkel inloggning information IDP Infor Retail-URL: er och domänen för hantering av Information](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_url.png)

    <span data-ttu-id="0d2e7-155">a.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-155">a.</span></span> <span data-ttu-id="0d2e7-156">I hello **identifierare** textruta, ange en Webbadress med hello följande mönster:</span><span class="sxs-lookup"><span data-stu-id="0d2e7-156">In hello **Identifier** textbox, type a URL using hello following patterns:</span></span> 
    |   |
    | -- |
    | `https://<company name>.mingle.infor.com` |
    | `http://<company name>.mingledev.infor.com` |

    <span data-ttu-id="0d2e7-157">b.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-157">b.</span></span> <span data-ttu-id="0d2e7-158">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.mingle.infor.com/sp/ACS.saml2`</span><span class="sxs-lookup"><span data-stu-id="0d2e7-158">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<company name>.mingle.infor.com/sp/ACS.saml2`</span></span>

4. <span data-ttu-id="0d2e7-159">Kontrollera **visa avancerade inställningar för URL: en** och utför följande steg om du inte vill tooconfigure hello programmet hello **SP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="0d2e7-159">Check **Show advanced URL settings** and perform hello following step if you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Infor Retail-domänen för hantering av Information och URL: er enkel inloggning information SP](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_url1.png)

    <span data-ttu-id="0d2e7-161">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.mingle.infor.com/<company code>`</span><span class="sxs-lookup"><span data-stu-id="0d2e7-161">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<company name>.mingle.infor.com/<company code>`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="0d2e7-162">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-162">These values are not real.</span></span> <span data-ttu-id="0d2e7-163">Uppdatera dessa värden med hello faktiska identifierare, Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-163">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="0d2e7-164">Kontakta [Infor Retail – Information Hanteringsklient supportteamet](mailto:innovate@infor.com) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-164">Contact [Infor Retail – Information Management Client support team](mailto:innovate@infor.com) tooget these values.</span></span> 

5. <span data-ttu-id="0d2e7-165">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-165">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_certificate.png) 

6. <span data-ttu-id="0d2e7-167">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-167">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="0d2e7-169">tooconfigure enkel inloggning på **Infor Retail – informationshantering** sida, behöver du toosend hello hämtas **XML-Metadata för** för[Infor Retail – informationshantering supportteamet ](mailto:innovate@infor.com).</span><span class="sxs-lookup"><span data-stu-id="0d2e7-169">tooconfigure single sign-on on **Infor Retail – Information Management** side, you need toosend hello downloaded **Metadata XML** too[Infor Retail – Information Management support team](mailto:innovate@infor.com).</span></span> <span data-ttu-id="0d2e7-170">De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-170">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="0d2e7-171">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="0d2e7-171">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="0d2e7-172">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-172">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="0d2e7-173">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="0d2e7-173">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="0d2e7-174">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="0d2e7-174">Create an Azure AD test user</span></span>

<span data-ttu-id="0d2e7-175">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-175">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="0d2e7-177">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="0d2e7-177">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="0d2e7-178">Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-178">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-inforretailinformationmanagement-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="0d2e7-180">toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-180">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-inforretailinformationmanagement-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="0d2e7-182">tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-182">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello webbinställningar](./media/active-directory-saas-inforretailinformationmanagement-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="0d2e7-184">I hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="0d2e7-184">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello användardialogrutan](./media/active-directory-saas-inforretailinformationmanagement-tutorial/create_aaduser_04.png)

    <span data-ttu-id="0d2e7-186">a.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-186">a.</span></span> <span data-ttu-id="0d2e7-187">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-187">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="0d2e7-188">b.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-188">b.</span></span> <span data-ttu-id="0d2e7-189">I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-189">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="0d2e7-190">c.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-190">c.</span></span> <span data-ttu-id="0d2e7-191">Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-191">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="0d2e7-192">d.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-192">d.</span></span> <span data-ttu-id="0d2e7-193">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-193">Click **Create**.</span></span>
 
### <a name="create-an-infor-retail--information-management-test-user"></a><span data-ttu-id="0d2e7-194">Skapa en Infor Retail – informationshantering testanvändare</span><span class="sxs-lookup"><span data-stu-id="0d2e7-194">Create an Infor Retail – Information Management test user</span></span>

<span data-ttu-id="0d2e7-195">I det här avsnittet kan du skapa en användare som kallas Britta Simon i Infor Retail – informationshantering.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-195">In this section, you create a user called Britta Simon in Infor Retail – Information Management.</span></span> <span data-ttu-id="0d2e7-196">Se tillsammans med [Infor Retail – informationshantering supportteamet](mailto:innovate@infor.com) tooadd hello användare i hello Infor Retail-plattform för Information.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-196">Please work with [Infor Retail – Information Management support team](mailto:innovate@infor.com) tooadd hello users in hello Infor Retail – Information Management platform.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="0d2e7-197">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="0d2e7-197">Assign hello Azure AD test user</span></span>

<span data-ttu-id="0d2e7-198">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooInfor Retail – informationshantering.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-198">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooInfor Retail – Information Management.</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="0d2e7-200">**tooassign Britta Simon tooInfor Retail-hantering, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="0d2e7-200">**tooassign Britta Simon tooInfor Retail – Information Management, perform hello following steps:**</span></span>

1. <span data-ttu-id="0d2e7-201">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-201">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="0d2e7-203">Välj i listan med program hello **Infor Retail – informationshantering**.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-203">In hello applications list, select **Infor Retail – Information Management**.</span></span>

    ![hello Infor Retail – länken informationshantering i listan med program hello](./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_inforretailinformationmanagement_app.png)  

3. <span data-ttu-id="0d2e7-205">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-205">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202]

4. <span data-ttu-id="0d2e7-207">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-207">Click **Add** button.</span></span> <span data-ttu-id="0d2e7-208">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="0d2e7-210">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-210">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="0d2e7-211">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="0d2e7-212">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="0d2e7-213">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0d2e7-213">Test single sign-on</span></span>

<span data-ttu-id="0d2e7-214">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-214">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="0d2e7-215">När du klickar på hello Infor Retail – informationshantering panelen i hello åtkomstpanelen, bör du hämta automatiskt inloggade tooyour Infor Retail – Information hanteringsprogram.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-215">When you click hello Infor Retail – Information Management tile in hello Access Panel, you should get automatically signed-on tooyour Infor Retail – Information Management application.</span></span>
<span data-ttu-id="0d2e7-216">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="0d2e7-216">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="0d2e7-217">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="0d2e7-217">Additional resources</span></span>

* [<span data-ttu-id="0d2e7-218">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0d2e7-218">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0d2e7-219">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="0d2e7-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-inforretailinformationmanagement-tutorial/tutorial_general_203.png

