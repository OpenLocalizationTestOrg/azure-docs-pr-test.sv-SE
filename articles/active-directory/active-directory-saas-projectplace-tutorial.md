---
title: "Självstudier: Azure Active Directory-integrering med Projectplace | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Projectplace."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 298059ca-b652-4577-916a-c31393d53d7a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: 95d109052096161f995ff26a18f8d64f0c4a3dc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-projectplace"></a><span data-ttu-id="7adb7-103">Självstudier: Azure Active Directory-integrering med Projectplace</span><span class="sxs-lookup"><span data-stu-id="7adb7-103">Tutorial: Azure Active Directory integration with Projectplace</span></span>

<span data-ttu-id="7adb7-104">I kursen får du lära dig hur toointegrate Projectplace med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="7adb7-104">In this tutorial, you learn how toointegrate Projectplace with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7adb7-105">Integrera Projectplace med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="7adb7-105">Integrating Projectplace with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="7adb7-106">Du kan styra i Azure AD som har åtkomst till tooProjectplace</span><span class="sxs-lookup"><span data-stu-id="7adb7-106">You can control in Azure AD who has access tooProjectplace</span></span>
- <span data-ttu-id="7adb7-107">Du kan aktivera din användare tooautomatically get inloggade tooProjectplace (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="7adb7-107">You can enable your users tooautomatically get signed-on tooProjectplace (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7adb7-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="7adb7-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="7adb7-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7adb7-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7adb7-110">Krav</span><span class="sxs-lookup"><span data-stu-id="7adb7-110">Prerequisites</span></span>

<span data-ttu-id="7adb7-111">tooconfigure Azure AD-integrering med Projectplace, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="7adb7-111">tooconfigure Azure AD integration with Projectplace, you need hello following items:</span></span>

- <span data-ttu-id="7adb7-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="7adb7-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7adb7-113">En Projectplace enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="7adb7-113">A Projectplace single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7adb7-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="7adb7-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7adb7-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="7adb7-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7adb7-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="7adb7-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7adb7-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7adb7-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7adb7-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="7adb7-118">Scenario description</span></span>
<span data-ttu-id="7adb7-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="7adb7-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7adb7-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="7adb7-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7adb7-121">Att lägga till Projectplace från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="7adb7-121">Adding Projectplace from hello gallery</span></span>
2. <span data-ttu-id="7adb7-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7adb7-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-projectplace-from-hello-gallery"></a><span data-ttu-id="7adb7-123">Att lägga till Projectplace från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="7adb7-123">Adding Projectplace from hello gallery</span></span>
<span data-ttu-id="7adb7-124">tooconfigure hello integrering av Projectplace i Azure AD, behöver du tooadd Projectplace hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="7adb7-124">tooconfigure hello integration of Projectplace into Azure AD, you need tooadd Projectplace from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="7adb7-125">**tooadd Projectplace från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="7adb7-125">**tooadd Projectplace from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="7adb7-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="7adb7-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7adb7-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="7adb7-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="7adb7-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="7adb7-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="7adb7-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7adb7-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="7adb7-133">Skriv i sökrutan hello **Projectplace**.</span><span class="sxs-lookup"><span data-stu-id="7adb7-133">In hello search box, type **Projectplace**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-projectplace-tutorial/tutorial_projectplace_search.png)

5. <span data-ttu-id="7adb7-135">Markera hello resultat på panelen **Projectplace**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="7adb7-135">In hello results panel, select **Projectplace**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-projectplace-tutorial/tutorial_projectplace_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="7adb7-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7adb7-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="7adb7-138">I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med Projectplace baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="7adb7-138">In this section, you configure and test Azure AD single sign-on with Projectplace based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="7adb7-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Projectplace är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7adb7-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Projectplace is tooa user in Azure AD.</span></span> <span data-ttu-id="7adb7-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Projectplace toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="7adb7-140">In other words, a link relationship between an Azure AD user and hello related user in Projectplace needs toobe established.</span></span>

<span data-ttu-id="7adb7-141">I Projectplace, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="7adb7-141">In Projectplace, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="7adb7-142">tooconfigure och testa Azure AD enkel inloggning med Projectplace, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="7adb7-142">tooconfigure and test Azure AD single sign-on with Projectplace, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="7adb7-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="7adb7-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="7adb7-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7adb7-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7adb7-145">**[Skapa en testanvändare Projectplace](#creating-a-projectplace-test-user)**  -toohave en motsvarighet för Britta Simon i Projectplace som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="7adb7-145">**[Creating a Projectplace test user](#creating-a-projectplace-test-user)** - toohave a counterpart of Britta Simon in Projectplace that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="7adb7-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="7adb7-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7adb7-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="7adb7-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="7adb7-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7adb7-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="7adb7-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Projectplace-program.</span><span class="sxs-lookup"><span data-stu-id="7adb7-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Projectplace application.</span></span>

<span data-ttu-id="7adb7-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Projectplace:**</span><span class="sxs-lookup"><span data-stu-id="7adb7-150">**tooconfigure Azure AD single sign-on with Projectplace, perform hello following steps:**</span></span>

1. <span data-ttu-id="7adb7-151">I hello Azure-portalen på hello **Projectplace** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="7adb7-151">In hello Azure portal, on hello **Projectplace** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="7adb7-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="7adb7-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-projectplace-tutorial/tutorial_projectplace_samlbase.png)

3. <span data-ttu-id="7adb7-155">På hello **Projectplace-domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="7adb7-155">On hello **Projectplace Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-projectplace-tutorial/tutorial_projectplace_url.png)

    <span data-ttu-id="7adb7-157">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company>.projectplace.com`</span><span class="sxs-lookup"><span data-stu-id="7adb7-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<company>.projectplace.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="7adb7-158">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="7adb7-158">This value is not real.</span></span> <span data-ttu-id="7adb7-159">Uppdatera det här värdet med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="7adb7-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="7adb7-160">Kontakta [Projectplace klienten supportteamet](https://success.planview.com/Projectplace/Support) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="7adb7-160">Contact [Projectplace Client support team](https://success.planview.com/Projectplace/Support) tooget this value.</span></span> 
 
4. <span data-ttu-id="7adb7-161">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="7adb7-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-projectplace-tutorial/tutorial_projectplace_certificate.png) 

5. <span data-ttu-id="7adb7-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="7adb7-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-projectplace-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="7adb7-165">tooconfigure enkel inloggning på **Projectplace** sida, behöver du toosend hello hämtas **XML-Metadata för** för[Projectplace-supportteamet](https://success.planview.com/Projectplace/Support).</span><span class="sxs-lookup"><span data-stu-id="7adb7-165">tooconfigure single sign-on on **Projectplace** side, you need toosend hello downloaded **Metadata XML** too[Projectplace support team](https://success.planview.com/Projectplace/Support).</span></span> <span data-ttu-id="7adb7-166">De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="7adb7-166">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

>[!NOTE]
><span data-ttu-id="7adb7-167">hello enkel inloggning konfigurationen har toobe som utförs av hello [Projectplace-supportteamet](https://success.planview.com/Projectplace/Support).</span><span class="sxs-lookup"><span data-stu-id="7adb7-167">hello single sign-on configuration has toobe performed by hello [Projectplace support team](https://success.planview.com/Projectplace/Support).</span></span> <span data-ttu-id="7adb7-168">Du får ett meddelande som hello konfigurationen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="7adb7-168">You will get a notification as soon as hello configuration has been completed.</span></span>

> [!TIP]
> <span data-ttu-id="7adb7-169">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="7adb7-169">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="7adb7-170">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="7adb7-170">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="7adb7-171">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7adb7-171">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="7adb7-172">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="7adb7-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="7adb7-173">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7adb7-173">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="7adb7-175">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="7adb7-175">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="7adb7-176">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="7adb7-176">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-projectplace-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7adb7-178">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="7adb7-178">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-projectplace-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7adb7-180">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7adb7-180">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-projectplace-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7adb7-182">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="7adb7-182">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-projectplace-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7adb7-184">a.</span><span class="sxs-lookup"><span data-stu-id="7adb7-184">a.</span></span> <span data-ttu-id="7adb7-185">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7adb7-185">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7adb7-186">b.</span><span class="sxs-lookup"><span data-stu-id="7adb7-186">b.</span></span> <span data-ttu-id="7adb7-187">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="7adb7-187">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="7adb7-188">c.</span><span class="sxs-lookup"><span data-stu-id="7adb7-188">c.</span></span> <span data-ttu-id="7adb7-189">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="7adb7-189">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="7adb7-190">d.</span><span class="sxs-lookup"><span data-stu-id="7adb7-190">d.</span></span> <span data-ttu-id="7adb7-191">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="7adb7-191">Click **Create**.</span></span>
 
### <a name="creating-a-projectplace-test-user"></a><span data-ttu-id="7adb7-192">Skapa en Projectplace-testanvändare</span><span class="sxs-lookup"><span data-stu-id="7adb7-192">Creating a Projectplace test user</span></span>

<span data-ttu-id="7adb7-193">I ordning tooenable Azure AD-användare toolog i Projectplace, måste de etableras i Projectplace.</span><span class="sxs-lookup"><span data-stu-id="7adb7-193">In order tooenable Azure AD users toolog into Projectplace, they must be provisioned into Projectplace.</span></span> <span data-ttu-id="7adb7-194">Hello gäller Projectplace är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="7adb7-194">In hello case of Projectplace, provisioning is a manual task.</span></span>

<span data-ttu-id="7adb7-195">**tooprovision ett användarkonto, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="7adb7-195">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="7adb7-196">Logga in tooyour **Projectplace** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="7adb7-196">Log in tooyour **Projectplace** company site as an administrator.</span></span>

2. <span data-ttu-id="7adb7-197">Gå för**personer**, och klicka sedan på **medlemmar**.</span><span class="sxs-lookup"><span data-stu-id="7adb7-197">Go too**People**, and then click **Members**.</span></span>
   
    <span data-ttu-id="7adb7-198">![Personer](./media/active-directory-saas-projectplace-tutorial/ic790228.png "personer")</span><span class="sxs-lookup"><span data-stu-id="7adb7-198">![People](./media/active-directory-saas-projectplace-tutorial/ic790228.png "People")</span></span>

3. <span data-ttu-id="7adb7-199">Klicka på **lägga till medlem**.</span><span class="sxs-lookup"><span data-stu-id="7adb7-199">Click **Add Member**.</span></span>
   
    <span data-ttu-id="7adb7-200">![Lägga till medlemmar](./media/active-directory-saas-projectplace-tutorial/ic790232.png "lägga till medlemmar")</span><span class="sxs-lookup"><span data-stu-id="7adb7-200">![Add Members](./media/active-directory-saas-projectplace-tutorial/ic790232.png "Add Members")</span></span>

4. <span data-ttu-id="7adb7-201">I hello **Lägg till medlem** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="7adb7-201">In hello **Add Member** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="7adb7-202">![Nya medlemmar](./media/active-directory-saas-projectplace-tutorial/ic790233.png "nya medlemmar")</span><span class="sxs-lookup"><span data-stu-id="7adb7-202">![New Members](./media/active-directory-saas-projectplace-tutorial/ic790233.png "New Members")</span></span>
   
    <span data-ttu-id="7adb7-203">a.</span><span class="sxs-lookup"><span data-stu-id="7adb7-203">a.</span></span> <span data-ttu-id="7adb7-204">I hello **nya medlemmar** textruta typen hello e-postadressen till en giltig AAD-konto som du vill använda tooprovision hello relaterade textrutor.</span><span class="sxs-lookup"><span data-stu-id="7adb7-204">In hello **New Members** textbox, type hello email address of a valid AAD account you want tooprovision into hello related textboxes.</span></span>
   
    <span data-ttu-id="7adb7-205">b.</span><span class="sxs-lookup"><span data-stu-id="7adb7-205">b.</span></span> <span data-ttu-id="7adb7-206">Klicka på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="7adb7-206">Click **Send**.</span></span>

   <span data-ttu-id="7adb7-207">Ett e-postmeddelande inklusive en länk tooconfirm hello kontot innan det blir aktiv skickas toohello Azure Active Directory-konto innehavaren.</span><span class="sxs-lookup"><span data-stu-id="7adb7-207">An email including a link tooconfirm hello account before it becomes active is sent toohello Azure Active Directory account holder.</span></span>

>[!NOTE]
><span data-ttu-id="7adb7-208">Du kan använda något annat Projectplace användarens konto skapas verktyg eller API: er som tillhandahålls av Projectplace tooprovision AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="7adb7-208">You can use any other Projectplace user account creation tools or APIs provided by Projectplace tooprovision AAD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="7adb7-209">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="7adb7-209">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="7adb7-210">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooProjectplace.</span><span class="sxs-lookup"><span data-stu-id="7adb7-210">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooProjectplace.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="7adb7-212">**tooassign Britta Simon tooProjectplace utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="7adb7-212">**tooassign Britta Simon tooProjectplace, perform hello following steps:**</span></span>

1. <span data-ttu-id="7adb7-213">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="7adb7-213">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="7adb7-215">Välj i listan med program hello **Projectplace**.</span><span class="sxs-lookup"><span data-stu-id="7adb7-215">In hello applications list, select **Projectplace**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-projectplace-tutorial/tutorial_projectplace_app.png) 

3. <span data-ttu-id="7adb7-217">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="7adb7-217">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="7adb7-219">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="7adb7-219">Click **Add** button.</span></span> <span data-ttu-id="7adb7-220">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7adb7-220">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="7adb7-222">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="7adb7-222">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="7adb7-223">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7adb7-223">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7adb7-224">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7adb7-224">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="7adb7-225">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7adb7-225">Testing single sign-on</span></span>

<span data-ttu-id="7adb7-226">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="7adb7-226">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="7adb7-227">När du klickar på hello Projectplace-panelen i hello åtkomstpanelen får automatiskt inloggade tooyour Projectplace-programmet.</span><span class="sxs-lookup"><span data-stu-id="7adb7-227">When you click hello Projectplace tile in hello Access Panel, you should get automatically signed-on tooyour Projectplace application.</span></span>
<span data-ttu-id="7adb7-228">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="7adb7-228">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7adb7-229">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="7adb7-229">Additional resources</span></span>

* [<span data-ttu-id="7adb7-230">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7adb7-230">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7adb7-231">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7adb7-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_203.png

