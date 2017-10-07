---
title: "Självstudier: Azure Active Directory-integrering med sköter Lösenordshanteraren & digitala valvet | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och sköter Lösenordshanteraren & digitala valvet."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e1a98f6a-2dae-4734-bdbf-4fba742a61d2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 01e59004e6bdccfca08094f9da6f82270d5028d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-keeper-password-manager--digital-vault"></a><span data-ttu-id="65ed2-103">Självstudier: Azure Active Directory-integrering med sköter Lösenordshanteraren & digitala valvet</span><span class="sxs-lookup"><span data-stu-id="65ed2-103">Tutorial: Azure Active Directory integration with Keeper Password Manager & Digital Vault</span></span>

<span data-ttu-id="65ed2-104">I kursen får du lära dig hur toointegrate sköter Lösenordshanteraren & digitala valvet med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="65ed2-104">In this tutorial, you learn how toointegrate Keeper Password Manager & Digital Vault with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="65ed2-105">Integrera sköter Lösenordshanteraren & digitala valvet med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="65ed2-105">Integrating Keeper Password Manager & Digital Vault with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="65ed2-106">Du kan styra i Azure AD som har åtkomst till tooKeeper Lösenordshanteraren & digitala valvet</span><span class="sxs-lookup"><span data-stu-id="65ed2-106">You can control in Azure AD who has access tooKeeper Password Manager & Digital Vault</span></span>
- <span data-ttu-id="65ed2-107">Du kan aktivera din användare tooautomatically get inloggade tooKeeper Lösenordshanteraren & digitala valvet (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="65ed2-107">You can enable your users tooautomatically get signed-on tooKeeper Password Manager & Digital Vault (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="65ed2-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="65ed2-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="65ed2-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="65ed2-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="65ed2-110">Krav</span><span class="sxs-lookup"><span data-stu-id="65ed2-110">Prerequisites</span></span>

<span data-ttu-id="65ed2-111">tooconfigure Azure AD-integrering med sköter Lösenordshanteraren & digitala valv behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="65ed2-111">tooconfigure Azure AD integration with Keeper Password Manager & Digital Vault, you need hello following items:</span></span>

- <span data-ttu-id="65ed2-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="65ed2-112">An Azure AD subscription</span></span>
- <span data-ttu-id="65ed2-113">En sköter Lösenordshanteraren & digitala valvet enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="65ed2-113">A Keeper Password Manager & Digital Vault single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="65ed2-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="65ed2-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="65ed2-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="65ed2-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="65ed2-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="65ed2-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="65ed2-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="65ed2-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="65ed2-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="65ed2-118">Scenario description</span></span>
<span data-ttu-id="65ed2-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="65ed2-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="65ed2-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="65ed2-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="65ed2-121">Att lägga till sköter Lösenordshanteraren & digitala valvet från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="65ed2-121">Adding Keeper Password Manager & Digital Vault from hello gallery</span></span>
2. <span data-ttu-id="65ed2-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="65ed2-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-keeper-password-manager--digital-vault-from-hello-gallery"></a><span data-ttu-id="65ed2-123">Att lägga till sköter Lösenordshanteraren & digitala valvet från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="65ed2-123">Adding Keeper Password Manager & Digital Vault from hello gallery</span></span>
<span data-ttu-id="65ed2-124">tooconfigure hello integrering av sköter Lösenordshanteraren & digitala valvet i Azure AD, behöver du tooadd sköter Lösenordshanteraren & digitala valvet hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="65ed2-124">tooconfigure hello integration of Keeper Password Manager & Digital Vault into Azure AD, you need tooadd Keeper Password Manager & Digital Vault from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="65ed2-125">**tooadd sköter Lösenordshanteraren & digitala valvet från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="65ed2-125">**tooadd Keeper Password Manager & Digital Vault from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="65ed2-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="65ed2-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="65ed2-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="65ed2-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="65ed2-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="65ed2-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="65ed2-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="65ed2-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="65ed2-133">Skriv i sökrutan hello **sköter Lösenordshanteraren & digitala valvet**.</span><span class="sxs-lookup"><span data-stu-id="65ed2-133">In hello search box, type **Keeper Password Manager & Digital Vault**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_keeper_search.png)

5. <span data-ttu-id="65ed2-135">Markera hello resultat på panelen **sköter Lösenordshanteraren & digitala valvet**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="65ed2-135">In hello results panel, select **Keeper Password Manager & Digital Vault**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_keeper_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="65ed2-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="65ed2-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="65ed2-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med sköter Lösenordshanteraren & digitala valvet baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="65ed2-138">In this section, you configure and test Azure AD single sign-on with Keeper Password Manager & Digital Vault based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="65ed2-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i sköter Lösenordshanteraren & digitala valvet är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="65ed2-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Keeper Password Manager & Digital Vault is tooa user in Azure AD.</span></span> <span data-ttu-id="65ed2-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i sköter Lösenordshanteraren & digitala valvet toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="65ed2-140">In other words, a link relationship between an Azure AD user and hello related user in Keeper Password Manager & Digital Vault needs toobe established.</span></span>

<span data-ttu-id="65ed2-141">I sköter Lösenordshanteraren & digitala valvet, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="65ed2-141">In Keeper Password Manager & Digital Vault, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="65ed2-142">tooconfigure och testa Azure AD enkel inloggning med sköter Lösenordshanteraren & digitala valv behöver toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="65ed2-142">tooconfigure and test Azure AD single sign-on with Keeper Password Manager & Digital Vault, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="65ed2-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="65ed2-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="65ed2-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="65ed2-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="65ed2-145">**[Skapa en testanvändare sköter Lösenordshanteraren & digitala valvet](#creating-a-keeperpasswordmanager-test-user)**  -toohave en motsvarighet för Britta Simon i sköter Lösenordshanteraren & digitala valvet som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="65ed2-145">**[Creating a Keeper Password Manager & Digital Vault test user](#creating-a-keeperpasswordmanager-test-user)** - toohave a counterpart of Britta Simon in Keeper Password Manager & Digital Vault that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="65ed2-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="65ed2-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="65ed2-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="65ed2-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="65ed2-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="65ed2-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="65ed2-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet sköter Lösenordshanteraren & digitala valvet.</span><span class="sxs-lookup"><span data-stu-id="65ed2-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Keeper Password Manager & Digital Vault application.</span></span>

<span data-ttu-id="65ed2-150">**tooconfigure Azure AD enkel inloggning med sköter Lösenordshanteraren & digitala valvet, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="65ed2-150">**tooconfigure Azure AD single sign-on with Keeper Password Manager & Digital Vault, perform hello following steps:**</span></span>

1. <span data-ttu-id="65ed2-151">I hello Azure-portalen på hello **sköter Lösenordshanteraren & digitala valvet** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="65ed2-151">In hello Azure portal, on hello **Keeper Password Manager & Digital Vault** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="65ed2-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="65ed2-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_keeper_samlbase.png)

3. <span data-ttu-id="65ed2-155">På hello **sköter Lösenordshanteraren & digitala valvet domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="65ed2-155">On hello **Keeper Password Manager & Digital Vault Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_keeper_url.png)

    <span data-ttu-id="65ed2-157">a.</span><span class="sxs-lookup"><span data-stu-id="65ed2-157">a.</span></span> <span data-ttu-id="65ed2-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://{SSO CONNECT SERVER}/sso-connect/saml/login`</span><span class="sxs-lookup"><span data-stu-id="65ed2-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://{SSO CONNECT SERVER}/sso-connect/saml/login`</span></span>

    <span data-ttu-id="65ed2-159">b.</span><span class="sxs-lookup"><span data-stu-id="65ed2-159">b.</span></span> <span data-ttu-id="65ed2-160">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://{SSO CONNECT SERVER}/sso-connect/saml/sso`</span><span class="sxs-lookup"><span data-stu-id="65ed2-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://{SSO CONNECT SERVER}/sso-connect/saml/sso`</span></span>

    <span data-ttu-id="65ed2-161">c.</span><span class="sxs-lookup"><span data-stu-id="65ed2-161">c.</span></span> <span data-ttu-id="65ed2-162">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://{SSO CONNECT SERVER}/sso-connect`</span><span class="sxs-lookup"><span data-stu-id="65ed2-162">In hello **Identifier** textbox, type a URL using hello following pattern: `https://{SSO CONNECT SERVER}/sso-connect`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="65ed2-163">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="65ed2-163">These values are not real.</span></span> <span data-ttu-id="65ed2-164">Uppdatera dessa värden med hello faktiska Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="65ed2-164">Update these values with hello actual Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="65ed2-165">Kontakta [sköter Lösenordshanteraren & digitala valvet klienten supportteam](https://keepersecurity.com/contact.html) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="65ed2-165">Contact [Keeper Password Manager & Digital Vault Client support team](https://keepersecurity.com/contact.html) tooget these values.</span></span> 

4. <span data-ttu-id="65ed2-166">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="65ed2-166">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_keeper_certificate.png) 

5. <span data-ttu-id="65ed2-168">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="65ed2-168">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="65ed2-170">På hello **sköter Lösenordshanteraren & digitala valvet Configuration** klickar du på **konfigurera sköter Lösenordshanteraren & digitala valvet** tooopen **konfigurera inloggning**fönster.</span><span class="sxs-lookup"><span data-stu-id="65ed2-170">On hello **Keeper Password Manager & Digital Vault Configuration** section, click **Configure Keeper Password Manager & Digital Vault** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="65ed2-171">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="65ed2-171">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_keeper_configure.png) 

7. <span data-ttu-id="65ed2-173">tooconfigure enkel inloggning på **sköter Lösenordshanteraren & digitala valvet Configuration** sida, Följ riktlinjerna i hello [sköter supportguide](https://keepersecurity.com/assets/pdf/SettingupAzurewithKeeperSSOConnect.pdf)</span><span class="sxs-lookup"><span data-stu-id="65ed2-173">tooconfigure single sign-on on **Keeper Password Manager & Digital Vault Configuration** side, follow hello guidelines given at [Keeper Support Guide](https://keepersecurity.com/assets/pdf/SettingupAzurewithKeeperSSOConnect.pdf)</span></span>

> [!TIP]
> <span data-ttu-id="65ed2-174">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="65ed2-174">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="65ed2-175">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="65ed2-175">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="65ed2-176">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="65ed2-176">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="65ed2-177">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="65ed2-177">Creating an Azure AD test user</span></span>
<span data-ttu-id="65ed2-178">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="65ed2-178">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="65ed2-180">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="65ed2-180">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="65ed2-181">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="65ed2-181">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-keeperpasswordmanager-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="65ed2-183">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="65ed2-183">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-keeperpasswordmanager-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="65ed2-185">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="65ed2-185">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-keeperpasswordmanager-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="65ed2-187">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="65ed2-187">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-keeperpasswordmanager-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="65ed2-189">a.</span><span class="sxs-lookup"><span data-stu-id="65ed2-189">a.</span></span> <span data-ttu-id="65ed2-190">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="65ed2-190">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="65ed2-191">b.</span><span class="sxs-lookup"><span data-stu-id="65ed2-191">b.</span></span> <span data-ttu-id="65ed2-192">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="65ed2-192">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="65ed2-193">c.</span><span class="sxs-lookup"><span data-stu-id="65ed2-193">c.</span></span> <span data-ttu-id="65ed2-194">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="65ed2-194">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="65ed2-195">d.</span><span class="sxs-lookup"><span data-stu-id="65ed2-195">d.</span></span> <span data-ttu-id="65ed2-196">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="65ed2-196">Click **Create**.</span></span>
 
### <a name="creating-a-keeper-password-manager--digital-vault-test-user"></a><span data-ttu-id="65ed2-197">Skapa en testanvändare sköter Lösenordshanteraren & digitala valvet</span><span class="sxs-lookup"><span data-stu-id="65ed2-197">Creating a Keeper Password Manager & Digital Vault test user</span></span>

<span data-ttu-id="65ed2-198">tooenable Azure AD-användare toolog i tooKeeper Lösenordshanteraren & digitala valvet, måste de etableras i sköter Lösenordshanteraren & digitala valvet.</span><span class="sxs-lookup"><span data-stu-id="65ed2-198">tooenable Azure AD users toolog in tooKeeper Password Manager & Digital Vault, they must be provisioned into Keeper Password Manager & Digital Vault.</span></span> <span data-ttu-id="65ed2-199">Programmet stöder bara i tid användaretablering och authentication-användare kommer att skapas i hello program automatiskt.</span><span class="sxs-lookup"><span data-stu-id="65ed2-199">Application supports Just in time user provisioning and after authentication users will be created in hello application automatically.</span></span> <span data-ttu-id="65ed2-200">Du kan kontakta [sköter stöd](https://keepersecurity.com/contact.html), om du vill toosetup användare manuellt.</span><span class="sxs-lookup"><span data-stu-id="65ed2-200">You can contact [Keeper Support](https://keepersecurity.com/contact.html), if you want toosetup users manually.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="65ed2-201">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="65ed2-201">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="65ed2-202">I det här avsnittet kan du aktivera Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst till tooKeeper Lösenordshanteraren & digitala valvet.</span><span class="sxs-lookup"><span data-stu-id="65ed2-202">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooKeeper Password Manager & Digital Vault.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="65ed2-204">**tooassign Britta Simon tooKeeper Lösenordshanteraren & digitala valvet, utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="65ed2-204">**tooassign Britta Simon tooKeeper Password Manager & Digital Vault, perform hello following steps:**</span></span>

1. <span data-ttu-id="65ed2-205">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="65ed2-205">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="65ed2-207">Välj i listan med program hello **sköter Lösenordshanteraren & digitala valvet**.</span><span class="sxs-lookup"><span data-stu-id="65ed2-207">In hello applications list, select **Keeper Password Manager & Digital Vault**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_keeper_app.png) 

3. <span data-ttu-id="65ed2-209">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="65ed2-209">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="65ed2-211">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="65ed2-211">Click **Add** button.</span></span> <span data-ttu-id="65ed2-212">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="65ed2-212">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="65ed2-214">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="65ed2-214">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="65ed2-215">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="65ed2-215">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="65ed2-216">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="65ed2-216">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="65ed2-217">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="65ed2-217">Testing single sign-on</span></span>

<span data-ttu-id="65ed2-218">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="65ed2-218">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="65ed2-219">Du bör få inloggningssidan för sköter Lösenordshanteraren & digitala valvet program när du klickar på hello sköter Lösenordshanteraren & digitala valvet panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="65ed2-219">When you click hello Keeper Password Manager & Digital Vault tile in hello Access Panel, you should get login page of Keeper Password Manager & Digital Vault application.</span></span> <span data-ttu-id="65ed2-220">Du bör få till hello program vid autentiseringen lyckas.</span><span class="sxs-lookup"><span data-stu-id="65ed2-220">Upon successful authentication, you should get into hello application.</span></span> <span data-ttu-id="65ed2-221">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="65ed2-221">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="65ed2-222">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="65ed2-222">Additional resources</span></span>

* [<span data-ttu-id="65ed2-223">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="65ed2-223">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="65ed2-224">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="65ed2-224">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-keeperpasswordmanager-tutorial/tutorial_general_203.png

