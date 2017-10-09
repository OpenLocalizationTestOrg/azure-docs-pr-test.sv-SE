---
title: "Självstudier: Azure Active Directory-integrering med UltiPro | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och UltiPro."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: afc0f2b9-2eac-47ec-af04-65ed0fb0ca5a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.openlocfilehash: 8cb613228893e8cbf997957452d55d0ab7939171
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ultipro"></a><span data-ttu-id="aa343-103">Självstudier: Azure Active Directory-integrering med UltiPro</span><span class="sxs-lookup"><span data-stu-id="aa343-103">Tutorial: Azure Active Directory integration with UltiPro</span></span>

<span data-ttu-id="aa343-104">I kursen får du lära dig hur toointegrate UltiPro med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="aa343-104">In this tutorial, you learn how toointegrate UltiPro with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="aa343-105">Integrera UltiPro med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="aa343-105">Integrating UltiPro with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="aa343-106">Du kan styra i Azure AD som har åtkomst till tooUltiPro</span><span class="sxs-lookup"><span data-stu-id="aa343-106">You can control in Azure AD who has access tooUltiPro</span></span>
- <span data-ttu-id="aa343-107">Du kan aktivera din användare tooautomatically get inloggade tooUltiPro (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="aa343-107">You can enable your users tooautomatically get signed-on tooUltiPro (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="aa343-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="aa343-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="aa343-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="aa343-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="aa343-110">Krav</span><span class="sxs-lookup"><span data-stu-id="aa343-110">Prerequisites</span></span>

<span data-ttu-id="aa343-111">tooconfigure Azure AD-integrering med UltiPro, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="aa343-111">tooconfigure Azure AD integration with UltiPro, you need hello following items:</span></span>

- <span data-ttu-id="aa343-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="aa343-112">An Azure AD subscription</span></span>
- <span data-ttu-id="aa343-113">En UltiPro enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="aa343-113">A UltiPro single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="aa343-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="aa343-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="aa343-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="aa343-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="aa343-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="aa343-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="aa343-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="aa343-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="aa343-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="aa343-118">Scenario description</span></span>
<span data-ttu-id="aa343-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="aa343-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="aa343-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="aa343-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="aa343-121">Att lägga till UltiPro från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="aa343-121">Adding UltiPro from hello gallery</span></span>
2. <span data-ttu-id="aa343-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="aa343-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-ultipro-from-hello-gallery"></a><span data-ttu-id="aa343-123">Att lägga till UltiPro från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="aa343-123">Adding UltiPro from hello gallery</span></span>
<span data-ttu-id="aa343-124">tooconfigure hello integrering av UltiPro i Azure AD, behöver du tooadd UltiPro hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="aa343-124">tooconfigure hello integration of UltiPro into Azure AD, you need tooadd UltiPro from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="aa343-125">**tooadd UltiPro från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="aa343-125">**tooadd UltiPro from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="aa343-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="aa343-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="aa343-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="aa343-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="aa343-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="aa343-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="aa343-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="aa343-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="aa343-133">Skriv i sökrutan hello **UltiPro**.</span><span class="sxs-lookup"><span data-stu-id="aa343-133">In hello search box, type **UltiPro**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ultipro-tutorial/tutorial_ultipro_search.png)

5. <span data-ttu-id="aa343-135">Markera hello resultat på panelen **UltiPro**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="aa343-135">In hello results panel, select **UltiPro**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ultipro-tutorial/tutorial_ultipro_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="aa343-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="aa343-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="aa343-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med UltiPro baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="aa343-138">In this section, you configure and test Azure AD single sign-on with UltiPro based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="aa343-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i UltiPro är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="aa343-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in UltiPro is tooa user in Azure AD.</span></span> <span data-ttu-id="aa343-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i UltiPro toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="aa343-140">In other words, a link relationship between an Azure AD user and hello related user in UltiPro needs toobe established.</span></span>

<span data-ttu-id="aa343-141">I UltiPro, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="aa343-141">In UltiPro, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="aa343-142">tooconfigure och testa Azure AD enkel inloggning med UltiPro, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="aa343-142">tooconfigure and test Azure AD single sign-on with UltiPro, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="aa343-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="aa343-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="aa343-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="aa343-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="aa343-145">**[Skapa en testanvändare UltiPro](#creating-a-ultipro-test-user)**  -toohave en motsvarighet för Britta Simon i UltiPro som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="aa343-145">**[Creating a UltiPro test user](#creating-a-ultipro-test-user)** - toohave a counterpart of Britta Simon in UltiPro that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="aa343-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="aa343-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="aa343-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="aa343-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="aa343-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="aa343-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="aa343-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt UltiPro program.</span><span class="sxs-lookup"><span data-stu-id="aa343-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your UltiPro application.</span></span>

<span data-ttu-id="aa343-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med UltiPro:**</span><span class="sxs-lookup"><span data-stu-id="aa343-150">**tooconfigure Azure AD single sign-on with UltiPro, perform hello following steps:**</span></span>

1. <span data-ttu-id="aa343-151">I hello Azure-portalen på hello **UltiPro** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="aa343-151">In hello Azure portal, on hello **UltiPro** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="aa343-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="aa343-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-ultipro-tutorial/tutorial_ultipro_samlbase.png)

3. <span data-ttu-id="aa343-155">På hello **UltiPro domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="aa343-155">On hello **UltiPro Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ultipro-tutorial/tutorial_ultipro_url.png)

    <span data-ttu-id="aa343-157">a.</span><span class="sxs-lookup"><span data-stu-id="aa343-157">a.</span></span> <span data-ttu-id="aa343-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:</span><span class="sxs-lookup"><span data-stu-id="aa343-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.ultipro.com/`|
    | `https://<companyname>.ultiproworkplace.com?cpi=AZUREADISSSUERURL`|
    | ` https://<companyname>.ultipro.ca`|
    
    <span data-ttu-id="aa343-159">b.</span><span class="sxs-lookup"><span data-stu-id="aa343-159">b.</span></span> <span data-ttu-id="aa343-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:</span><span class="sxs-lookup"><span data-stu-id="aa343-160">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.ultipro.com/adfs/services/trust`|
    | `https://<companyname>.ultiproworkplace.com/adfs/services/trust`|
    | `https://<companyname>.ultipro.ca/adfs/services/trust`|
    
    <span data-ttu-id="aa343-161">c.</span><span class="sxs-lookup"><span data-stu-id="aa343-161">c.</span></span> <span data-ttu-id="aa343-162">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:</span><span class="sxs-lookup"><span data-stu-id="aa343-162">In hello **Reply URL** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.ultipro.com/<instancename>`|
    | `https://<companyname>.ultiproworkplace.com/<instancename>`|
    | `https://<companyname>.ultipro.ca/<instancename>`|
    
    > [!NOTE] 
    > <span data-ttu-id="aa343-163">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="aa343-163">These values are not real.</span></span> <span data-ttu-id="aa343-164">Uppdatera dessa värden med hello faktiska identifierare, Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="aa343-164">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="aa343-165">Kontakta [UltiPro klienten supportteamet](https://www.ultimatesoftware.com/ContactUs) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="aa343-165">Contact [UltiPro Client support team](https://www.ultimatesoftware.com/ContactUs) tooget these values.</span></span> 

5. <span data-ttu-id="aa343-166">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="aa343-166">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ultipro-tutorial/tutorial_ultipro_certificate.png) 

6. <span data-ttu-id="aa343-168">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="aa343-168">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ultipro-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="aa343-170">På hello **UltiPro Configuration** klickar du på **konfigurera UltiPro** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="aa343-170">On hello **UltiPro Configuration** section, click **Configure UltiPro** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="aa343-171">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="aa343-171">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ultipro-tutorial/tutorial_ultipro_configure.png) 

8. <span data-ttu-id="aa343-173">tooconfigure enkel inloggning på **UltiPro** sida, behöver du toosend hello hämtas **Certificate(Base64), Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** för[UltiPro supportteam](https://www.ultimatesoftware.com/ContactUs).</span><span class="sxs-lookup"><span data-stu-id="aa343-173">tooconfigure single sign-on on **UltiPro** side, you need toosend hello downloaded **Certificate(Base64), Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[UltiPro support team](https://www.ultimatesoftware.com/ContactUs).</span></span>

> [!TIP]
> <span data-ttu-id="aa343-174">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="aa343-174">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="aa343-175">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="aa343-175">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="aa343-176">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="aa343-176">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="aa343-177">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="aa343-177">Creating an Azure AD test user</span></span>
<span data-ttu-id="aa343-178">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="aa343-178">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="aa343-180">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="aa343-180">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="aa343-181">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="aa343-181">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ultipro-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="aa343-183">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="aa343-183">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ultipro-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="aa343-185">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="aa343-185">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ultipro-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="aa343-187">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="aa343-187">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ultipro-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="aa343-189">a.</span><span class="sxs-lookup"><span data-stu-id="aa343-189">a.</span></span> <span data-ttu-id="aa343-190">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="aa343-190">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="aa343-191">b.</span><span class="sxs-lookup"><span data-stu-id="aa343-191">b.</span></span> <span data-ttu-id="aa343-192">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="aa343-192">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="aa343-193">c.</span><span class="sxs-lookup"><span data-stu-id="aa343-193">c.</span></span> <span data-ttu-id="aa343-194">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="aa343-194">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="aa343-195">d.</span><span class="sxs-lookup"><span data-stu-id="aa343-195">d.</span></span> <span data-ttu-id="aa343-196">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="aa343-196">Click **Create**.</span></span>
 
### <a name="creating-a-ultipro-test-user"></a><span data-ttu-id="aa343-197">Skapa en testanvändare UltiPro</span><span class="sxs-lookup"><span data-stu-id="aa343-197">Creating a UltiPro test user</span></span>

<span data-ttu-id="aa343-198">I det här avsnittet skapar du en användare som kallas Britta Simon i UltiPro.</span><span class="sxs-lookup"><span data-stu-id="aa343-198">In this section, you create a user called Britta Simon in UltiPro.</span></span> <span data-ttu-id="aa343-199">Arbeta med [UltiPro supportteamet](https://www.ultimatesoftware.com/ContactUs) att lägga till hello användare i hello UltiPro plattform.</span><span class="sxs-lookup"><span data-stu-id="aa343-199">Work with [UltiPro support team](https://www.ultimatesoftware.com/ContactUs) to add hello users in hello UltiPro platform.</span></span> <span data-ttu-id="aa343-200">Användare måste skapas och aktiveras innan du använder enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="aa343-200">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="aa343-201">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="aa343-201">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="aa343-202">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooUltiPro.</span><span class="sxs-lookup"><span data-stu-id="aa343-202">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooUltiPro.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="aa343-204">**tooassign Britta Simon tooUltiPro utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="aa343-204">**tooassign Britta Simon tooUltiPro, perform hello following steps:**</span></span>

1. <span data-ttu-id="aa343-205">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="aa343-205">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="aa343-207">Välj i listan med program hello **UltiPro**.</span><span class="sxs-lookup"><span data-stu-id="aa343-207">In hello applications list, select **UltiPro**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ultipro-tutorial/tutorial_ultipro_app.png) 

3. <span data-ttu-id="aa343-209">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="aa343-209">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="aa343-211">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="aa343-211">Click **Add** button.</span></span> <span data-ttu-id="aa343-212">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="aa343-212">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="aa343-214">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="aa343-214">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="aa343-215">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="aa343-215">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="aa343-216">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="aa343-216">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="aa343-217">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="aa343-217">Testing single sign-on</span></span>

<span data-ttu-id="aa343-218">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="aa343-218">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>  
<span data-ttu-id="aa343-219">Du bör få automatiskt inloggade tooyour UltiPro programmet när du klickar på hello UltiPro panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="aa343-219">When you click hello UltiPro tile in hello Access Panel, you should get automatically signed-on tooyour UltiPro application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="aa343-220">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="aa343-220">Additional resources</span></span>

* [<span data-ttu-id="aa343-221">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="aa343-221">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="aa343-222">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="aa343-222">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-ultipro-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ultipro-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ultipro-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ultipro-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ultipro-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ultipro-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ultipro-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ultipro-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ultipro-tutorial/tutorial_general_203.png

