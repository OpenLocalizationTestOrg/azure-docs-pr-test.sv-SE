---
title: "Självstudier: Azure Active Directory-integrering med Neota logik Studio | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Neota logik Studio."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 842605e6-a91d-42cc-a0bb-e23e67173ae2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: jeedes
ms.openlocfilehash: a479ee49618de8275132dbc2c21a8ef723d92d02
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-neota-logic-studio"></a><span data-ttu-id="6b52f-103">Självstudier: Azure Active Directory-integrering med Neota logik Studio</span><span class="sxs-lookup"><span data-stu-id="6b52f-103">Tutorial: Azure Active Directory integration with Neota Logic Studio</span></span>

<span data-ttu-id="6b52f-104">I kursen får du lära dig hur toointegrate Neota logik Studio med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="6b52f-104">In this tutorial, you learn how toointegrate Neota Logic Studio with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6b52f-105">Integrera Neota logik Studio med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="6b52f-105">Integrating Neota Logic Studio with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="6b52f-106">Du kan styra i Azure AD som har åtkomst tooNeota logik Studio</span><span class="sxs-lookup"><span data-stu-id="6b52f-106">You can control in Azure AD who has access tooNeota Logic Studio</span></span>
- <span data-ttu-id="6b52f-107">Du kan aktivera din användare tooautomatically get inloggade tooNeota logik Studio (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="6b52f-107">You can enable your users tooautomatically get signed-on tooNeota Logic Studio (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="6b52f-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="6b52f-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="6b52f-109">Om du vill tooknow finns mer information om SaaS appintegrering med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6b52f-109">If you want tooknow more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="6b52f-110">[Vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6b52f-110">[What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6b52f-111">Krav</span><span class="sxs-lookup"><span data-stu-id="6b52f-111">Prerequisites</span></span>

<span data-ttu-id="6b52f-112">tooconfigure Azure AD-integrering med Neota logik Studio, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="6b52f-112">tooconfigure Azure AD integration with Neota Logic Studio, you need hello following items:</span></span>

- <span data-ttu-id="6b52f-113">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="6b52f-113">An Azure AD subscription</span></span>
- <span data-ttu-id="6b52f-114">En Neota logik Studio enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="6b52f-114">A Neota Logic Studio single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6b52f-115">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="6b52f-115">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6b52f-116">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="6b52f-116">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6b52f-117">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="6b52f-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6b52f-118">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6b52f-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6b52f-119">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="6b52f-119">Scenario description</span></span>

<span data-ttu-id="6b52f-120">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="6b52f-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6b52f-121">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="6b52f-121">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6b52f-122">Att lägga till Neota logik Studio från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="6b52f-122">Adding Neota Logic Studio from hello gallery</span></span>
2. <span data-ttu-id="6b52f-123">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6b52f-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-neota-logic-studio-from-hello-gallery"></a><span data-ttu-id="6b52f-124">Att lägga till Neota logik Studio från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="6b52f-124">Adding Neota Logic Studio from hello gallery</span></span>

<span data-ttu-id="6b52f-125">tooconfigure hello integrering av Neota logik Studio i Azure AD, behöver du tooadd Neota logik Studio hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="6b52f-125">tooconfigure hello integration of Neota Logic Studio into Azure AD, you need tooadd Neota Logic Studio from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="6b52f-126">**tooadd Neota logik Studio från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="6b52f-126">**tooadd Neota Logic Studio from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="6b52f-127">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="6b52f-127">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="6b52f-129">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="6b52f-129">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="6b52f-130">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="6b52f-130">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="6b52f-132">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6b52f-132">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="6b52f-134">Skriv i sökrutan hello **Neota logik Studio**.</span><span class="sxs-lookup"><span data-stu-id="6b52f-134">In hello search box, type **Neota Logic Studio**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_neotalogicstudio_search.png)

5. <span data-ttu-id="6b52f-136">Markera hello resultat på panelen **Neota logik Studio**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="6b52f-136">In hello results panel, select **Neota Logic Studio**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_neotalogicstudio_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="6b52f-138">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6b52f-138">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="6b52f-139">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Neota logik Studio baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="6b52f-139">In this section, you configure and test Azure AD single sign-on with Neota Logic Studio based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="6b52f-140">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Neota logik Studio är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6b52f-140">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Neota Logic Studio is tooa user in Azure AD.</span></span> <span data-ttu-id="6b52f-141">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i Neota logik Studio toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="6b52f-141">In other words, a link relationship between an Azure AD user and hello related user in Neota Logic Studio needs toobe established.</span></span>

<span data-ttu-id="6b52f-142">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Neota logik Studio.</span><span class="sxs-lookup"><span data-stu-id="6b52f-142">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Neota Logic Studio.</span></span>

<span data-ttu-id="6b52f-143">tooconfigure och testa Azure AD enkel inloggning med Neota logik Studio, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="6b52f-143">tooconfigure and test Azure AD single sign-on with Neota Logic Studio, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="6b52f-144">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="6b52f-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="6b52f-145">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6b52f-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6b52f-146">**[Skapa en testanvändare Neota logik Studio](#creating-a-neota-logic-studio-test-user)**  -toohave en motsvarighet för Britta Simon i Neota logik Studio som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="6b52f-146">**[Creating a Neota Logic Studio test user](#creating-a-neota-logic-studio-test-user)** - toohave a counterpart of Britta Simon in Neota Logic Studio that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="6b52f-147">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6b52f-147">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6b52f-148">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="6b52f-148">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="6b52f-149">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6b52f-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="6b52f-150">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Neota logik Studio.</span><span class="sxs-lookup"><span data-stu-id="6b52f-150">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Neota Logic Studio application.</span></span>

<span data-ttu-id="6b52f-151">**tooconfigure Azure AD enkel inloggning med Neota logik Studio utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="6b52f-151">**tooconfigure Azure AD single sign-on with Neota Logic Studio, perform hello following steps:**</span></span>

1. <span data-ttu-id="6b52f-152">I hello Azure-portalen på hello **Neota logik Studio** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="6b52f-152">In hello Azure portal, on hello **Neota Logic Studio** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="6b52f-154">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6b52f-154">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_neotalogicstudio_samlbase.png)

3. <span data-ttu-id="6b52f-156">På hello **Neota logik Studio domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="6b52f-156">On hello **Neota Logic Studio Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_neotalogicstudio_url.png)

    <span data-ttu-id="6b52f-158">a.</span><span class="sxs-lookup"><span data-stu-id="6b52f-158">a.</span></span> <span data-ttu-id="6b52f-159">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<sub domain>.neotalogic.com/a/<sub application>`</span><span class="sxs-lookup"><span data-stu-id="6b52f-159">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<sub domain>.neotalogic.com/a/<sub application>`</span></span>

    <span data-ttu-id="6b52f-160">b.</span><span class="sxs-lookup"><span data-stu-id="6b52f-160">b.</span></span> <span data-ttu-id="6b52f-161">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<sub domain>.neotalogic.com/wb`</span><span class="sxs-lookup"><span data-stu-id="6b52f-161">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<sub domain>.neotalogic.com/wb`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="6b52f-162">Dessa värden är inte hello verkliga.</span><span class="sxs-lookup"><span data-stu-id="6b52f-162">These values are not hello real.</span></span> <span data-ttu-id="6b52f-163">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="6b52f-163">Update these values with hello actual Sign-On URL & Identifier.</span></span> <span data-ttu-id="6b52f-164">Vi rekommenderar här du toouse hello unikt värde i strängen i hello identifierare.</span><span class="sxs-lookup"><span data-stu-id="6b52f-164">Here we suggest you toouse hello unique value of string in hello Identifier.</span></span> <span data-ttu-id="6b52f-165">Kontakta [Neota logik Studio klienten supportteamet](https://www.neotalogic.com/contact-us/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="6b52f-165">Contact [Neota Logic Studio Client support team](https://www.neotalogic.com/contact-us/) tooget these values.</span></span> 
 
4. <span data-ttu-id="6b52f-166">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="6b52f-166">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_neotalogicstudio_certificate.png) 

5. <span data-ttu-id="6b52f-168">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="6b52f-168">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="6b52f-170">tooget SSO konfigurerats för ditt program, kontakta [Neota logik Studio stöd](https://www.neotalogic.com/contact-us/) grupp- och ge dem med hämtade **XML-Metadata för** fil.</span><span class="sxs-lookup"><span data-stu-id="6b52f-170">tooget SSO configured for your application, Contact [Neota Logic Studio support](https://www.neotalogic.com/contact-us/) team and provide them with downloaded **Metadata XML** file.</span></span>

> [!TIP]
> <span data-ttu-id="6b52f-171">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="6b52f-171">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="6b52f-172">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="6b52f-172">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="6b52f-173">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="6b52f-173">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="6b52f-174">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="6b52f-174">Creating an Azure AD test user</span></span>
<span data-ttu-id="6b52f-175">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6b52f-175">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="6b52f-177">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="6b52f-177">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="6b52f-178">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="6b52f-178">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-neotalogicstudio-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="6b52f-180">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="6b52f-180">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-neotalogicstudio-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="6b52f-182">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6b52f-182">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-neotalogicstudio-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="6b52f-184">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="6b52f-184">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-neotalogicstudio-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="6b52f-186">a.</span><span class="sxs-lookup"><span data-stu-id="6b52f-186">a.</span></span> <span data-ttu-id="6b52f-187">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6b52f-187">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6b52f-188">b.</span><span class="sxs-lookup"><span data-stu-id="6b52f-188">b.</span></span> <span data-ttu-id="6b52f-189">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="6b52f-189">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="6b52f-190">c.</span><span class="sxs-lookup"><span data-stu-id="6b52f-190">c.</span></span> <span data-ttu-id="6b52f-191">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="6b52f-191">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="6b52f-192">d.</span><span class="sxs-lookup"><span data-stu-id="6b52f-192">d.</span></span> <span data-ttu-id="6b52f-193">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="6b52f-193">Click **Create**.</span></span>
 
### <a name="creating-a-neota-logic-studio-test-user"></a><span data-ttu-id="6b52f-194">Skapa en testanvändare Neota logik Studio</span><span class="sxs-lookup"><span data-stu-id="6b52f-194">Creating a Neota Logic Studio test user</span></span>

<span data-ttu-id="6b52f-195">I det här avsnittet skapar du en användare som kallas Britta Simon i Neota logik Studio.</span><span class="sxs-lookup"><span data-stu-id="6b52f-195">In this section, you create a user called Britta Simon in Neota Logic Studio.</span></span> <span data-ttu-id="6b52f-196">Arbeta med [Neota logik Studio klienten supportteamet](https://www.neotalogic.com/contact-us/) att lägga till hello användare i hello Neota logik Studio plattform.</span><span class="sxs-lookup"><span data-stu-id="6b52f-196">work with [Neota Logic Studio Client support team](https://www.neotalogic.com/contact-us/) to add hello users in hello Neota Logic Studio platform.</span></span> <span data-ttu-id="6b52f-197">Användare måste skapas och aktiveras innan du använder enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6b52f-197">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="6b52f-198">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="6b52f-198">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="6b52f-199">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooNeota logik Studio.</span><span class="sxs-lookup"><span data-stu-id="6b52f-199">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooNeota Logic Studio.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="6b52f-201">**tooassign Britta Simon tooNeota logik Studio utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="6b52f-201">**tooassign Britta Simon tooNeota Logic Studio, perform hello following steps:**</span></span>

1. <span data-ttu-id="6b52f-202">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="6b52f-202">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="6b52f-204">Välj i listan med program hello **Neota logik Studio**.</span><span class="sxs-lookup"><span data-stu-id="6b52f-204">In hello applications list, select **Neota Logic Studio**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_neotalogicstudio_app.png) 

3. <span data-ttu-id="6b52f-206">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="6b52f-206">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="6b52f-208">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="6b52f-208">Click **Add** button.</span></span> <span data-ttu-id="6b52f-209">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6b52f-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="6b52f-211">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="6b52f-211">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="6b52f-212">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6b52f-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6b52f-213">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6b52f-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="6b52f-214">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6b52f-214">Testing single sign-on</span></span>

<span data-ttu-id="6b52f-215">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="6b52f-215">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="6b52f-216">Klicka på hello Neota logik Studio panelen i hello åtkomstpanelen, kommer du att omdirigerade tooOrganization inloggning sidan.</span><span class="sxs-lookup"><span data-stu-id="6b52f-216">Click hello Neota Logic Studio tile in hello Access Panel, you will be redirected tooOrganization sign-on page.</span></span> <span data-ttu-id="6b52f-217">Du kommer att inloggade tooyour Neota logik Studio program efter genomförd inloggning.</span><span class="sxs-lookup"><span data-stu-id="6b52f-217">After successful login, you will be signed-on tooyour Neota Logic Studio application.</span></span> <span data-ttu-id="6b52f-218">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="6b52f-218">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

<span data-ttu-id="6b52f-219">Läs mer om hello åtkomstpanelen [introduktion till åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="6b52f-219">For more information about hello Access Panel, see [introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="6b52f-220">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="6b52f-220">Additional resources</span></span>

* [<span data-ttu-id="6b52f-221">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6b52f-221">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6b52f-222">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="6b52f-222">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_203.png

