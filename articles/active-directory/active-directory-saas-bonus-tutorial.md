---
title: "Självstudier: Azure Active Directory-integrering med Bonusly | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Bonusly."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 29fea32a-fa20-47b2-9e24-26feb47b0ae6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 60ad06c028463af81a7901ab321c4ae9346798ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bonusly"></a><span data-ttu-id="07a00-103">Självstudier: Azure Active Directory-integrering med Bonusly</span><span class="sxs-lookup"><span data-stu-id="07a00-103">Tutorial: Azure Active Directory integration with Bonusly</span></span>

<span data-ttu-id="07a00-104">I kursen får du lära dig hur toointegrate Bonusly med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="07a00-104">In this tutorial, you learn how toointegrate Bonusly with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="07a00-105">Integrera Bonusly med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="07a00-105">Integrating Bonusly with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="07a00-106">Du kan styra i Azure AD som har åtkomst till tooBonusly</span><span class="sxs-lookup"><span data-stu-id="07a00-106">You can control in Azure AD who has access tooBonusly</span></span>
- <span data-ttu-id="07a00-107">Du kan aktivera din användare tooautomatically get inloggade tooBonusly (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="07a00-107">You can enable your users tooautomatically get signed-on tooBonusly (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="07a00-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="07a00-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="07a00-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="07a00-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="07a00-110">Krav</span><span class="sxs-lookup"><span data-stu-id="07a00-110">Prerequisites</span></span>

<span data-ttu-id="07a00-111">tooconfigure Azure AD-integrering med Bonusly, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="07a00-111">tooconfigure Azure AD integration with Bonusly, you need hello following items:</span></span>

- <span data-ttu-id="07a00-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="07a00-112">An Azure AD subscription</span></span>
- <span data-ttu-id="07a00-113">En Bonusly enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="07a00-113">A Bonusly single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="07a00-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="07a00-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="07a00-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="07a00-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="07a00-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="07a00-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="07a00-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="07a00-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="07a00-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="07a00-118">Scenario description</span></span>
<span data-ttu-id="07a00-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="07a00-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="07a00-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="07a00-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="07a00-121">Att lägga till Bonusly från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="07a00-121">Adding Bonusly from hello gallery</span></span>
2. <span data-ttu-id="07a00-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="07a00-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-bonusly-from-hello-gallery"></a><span data-ttu-id="07a00-123">Att lägga till Bonusly från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="07a00-123">Adding Bonusly from hello gallery</span></span>
<span data-ttu-id="07a00-124">tooconfigure hello integrering av Bonusly i Azure AD, behöver du tooadd Bonusly hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="07a00-124">tooconfigure hello integration of Bonusly into Azure AD, you need tooadd Bonusly from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="07a00-125">**tooadd Bonusly från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="07a00-125">**tooadd Bonusly from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="07a00-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="07a00-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="07a00-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="07a00-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="07a00-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="07a00-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="07a00-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="07a00-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="07a00-133">Skriv i sökrutan hello **Bonusly**väljer **Bonusly** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="07a00-133">In hello search box, type **Bonusly**, select **Bonusly** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Bonusly i hello resultatlistan](./media/active-directory-saas-bonus-tutorial/tutorial_bonusly_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="07a00-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="07a00-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="07a00-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Bonusly baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="07a00-136">In this section, you configure and test Azure AD single sign-on with Bonusly based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="07a00-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Bonusly är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="07a00-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Bonusly is tooa user in Azure AD.</span></span> <span data-ttu-id="07a00-138">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Bonusly toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="07a00-138">In other words, a link relationship between an Azure AD user and hello related user in Bonusly needs toobe established.</span></span>

<span data-ttu-id="07a00-139">I Bonusly, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="07a00-139">In Bonusly, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="07a00-140">tooconfigure och testa Azure AD enkel inloggning med Bonusly, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="07a00-140">tooconfigure and test Azure AD single sign-on with Bonusly, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="07a00-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="07a00-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="07a00-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="07a00-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="07a00-143">**[Skapa en Bonusly testanvändare](#create-a-bonusly-test-user)**  -toohave en motsvarighet för Britta Simon i Bonusly som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="07a00-143">**[Create a Bonusly test user](#create-a-bonusly-test-user)** - toohave a counterpart of Britta Simon in Bonusly that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="07a00-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="07a00-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="07a00-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="07a00-145">**[Test Single Sign-On](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="07a00-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="07a00-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="07a00-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Bonusly program.</span><span class="sxs-lookup"><span data-stu-id="07a00-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Bonusly application.</span></span>

<span data-ttu-id="07a00-148">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Bonusly:**</span><span class="sxs-lookup"><span data-stu-id="07a00-148">**tooconfigure Azure AD single sign-on with Bonusly, perform hello following steps:**</span></span>

1. <span data-ttu-id="07a00-149">I hello Azure-portalen på hello **Bonusly** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="07a00-149">In hello Azure portal, on hello **Bonusly** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="07a00-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="07a00-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-bonus-tutorial/tutorial_bonusly_samlbase.png)

3. <span data-ttu-id="07a00-153">På hello **Bonusly domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="07a00-153">On hello **Bonusly Domain and URLs** section, perform hello following steps:</span></span>

    ![URL: er och bonusly domän med enkel inloggning information](./media/active-directory-saas-bonus-tutorial/tutorial_bonusly_url.png)

    <span data-ttu-id="07a00-155">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://Bonus.ly/saml/<tenant-name>`</span><span class="sxs-lookup"><span data-stu-id="07a00-155">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://Bonus.ly/saml/<tenant-name>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="07a00-156">hello-värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="07a00-156">hello value is not real.</span></span> <span data-ttu-id="07a00-157">Hello uppdateringsvärde med hello faktiska Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="07a00-157">Update hello value with hello actual Reply URL.</span></span> <span data-ttu-id="07a00-158">Kontakta [Bonusly supportteamet](https://Bonusly/contact) tooget hello värde.</span><span class="sxs-lookup"><span data-stu-id="07a00-158">Contact [Bonusly support team](https://Bonusly/contact) tooget hello value.</span></span>
 
4. <span data-ttu-id="07a00-159">På hello **SAML-signeringscertifikat** avsnitt, kopiera hello **TUMAVTRYCKET** värdet från hello-certifikatet.</span><span class="sxs-lookup"><span data-stu-id="07a00-159">On hello **SAML Signing Certificate** section, copy hello **THUMBPRINT** value from hello certificate.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-bonus-tutorial/tutorial_bonusly_certificate.png) 

5. <span data-ttu-id="07a00-161">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="07a00-161">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-bonus-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="07a00-163">På hello **Bonusly Configuration** klickar du på **konfigurera Bonusly** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="07a00-163">On hello **Bonusly Configuration** section, click **Configure Bonusly** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="07a00-164">Kopiera hello **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="07a00-164">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Bonusly konfiguration](./media/active-directory-saas-bonus-tutorial/tutorial_bonusly_configure.png) 

7. <span data-ttu-id="07a00-166">I en annan webbläsare och logga in tooyour **Bonusly** klient.</span><span class="sxs-lookup"><span data-stu-id="07a00-166">In a different browser window, log in tooyour **Bonusly** tenant.</span></span>

8. <span data-ttu-id="07a00-167">Klicka i hello verktygsfältet hello längst upp **inställningar**, och välj sedan **integreringar och appar**.</span><span class="sxs-lookup"><span data-stu-id="07a00-167">In hello toolbar on hello top, click **Settings**, and then select **Integrations and apps**.</span></span>
   
    <span data-ttu-id="07a00-168">![Bonusly sociala avsnittet](./media/active-directory-saas-bonus-tutorial/ic773686.png "Bonusly")</span><span class="sxs-lookup"><span data-stu-id="07a00-168">![Bonusly Social Section](./media/active-directory-saas-bonus-tutorial/ic773686.png "Bonusly")</span></span>
9. <span data-ttu-id="07a00-169">Under **enkel inloggning**väljer **SAML**.</span><span class="sxs-lookup"><span data-stu-id="07a00-169">Under **Single Sign-On**, select **SAML**.</span></span>

10. <span data-ttu-id="07a00-170">På hello **SAML** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="07a00-170">On hello **SAML** dialog page, perform hello following steps:</span></span>
   
    <span data-ttu-id="07a00-171">![Bonusly Saml dialogrutan sidan](./media/active-directory-saas-bonus-tutorial/ic773687.png "Bonusly")</span><span class="sxs-lookup"><span data-stu-id="07a00-171">![Bonusly Saml Dialog page](./media/active-directory-saas-bonus-tutorial/ic773687.png "Bonusly")</span></span>
   
    <span data-ttu-id="07a00-172">a.</span><span class="sxs-lookup"><span data-stu-id="07a00-172">a.</span></span> <span data-ttu-id="07a00-173">I hello **IdP SSO mål-URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="07a00-173">In hello **IdP SSO target URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="07a00-174">b.</span><span class="sxs-lookup"><span data-stu-id="07a00-174">b.</span></span> <span data-ttu-id="07a00-175">I hello **IdP utfärdaren** textruta klistra in hello värdet för **SAML enhets-ID**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="07a00-175">In hello **IdP Issuer** textbox, paste hello value of **SAML Entity ID**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="07a00-176">c.</span><span class="sxs-lookup"><span data-stu-id="07a00-176">c.</span></span> <span data-ttu-id="07a00-177">I hello **IdP inloggnings-URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="07a00-177">In hello **IdP Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="07a00-178">d.</span><span class="sxs-lookup"><span data-stu-id="07a00-178">d.</span></span> <span data-ttu-id="07a00-179">Klistra in den **tumavtrycket** kopieras värdet från Azure-portalen till hello **Cert fingeravtryck** textruta.</span><span class="sxs-lookup"><span data-stu-id="07a00-179">Paste the **Thumbprint** value copied from Azure portal into hello **Cert Fingerprint** textbox.</span></span>
   
11. <span data-ttu-id="07a00-180">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="07a00-180">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="07a00-181">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="07a00-181">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="07a00-182">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="07a00-182">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="07a00-183">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="07a00-183">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="07a00-184">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="07a00-184">Create an Azure AD test user</span></span>
<span data-ttu-id="07a00-185">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="07a00-185">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="07a00-187">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="07a00-187">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="07a00-188">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="07a00-188">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-bonus-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="07a00-190">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="07a00-190">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-bonus-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="07a00-192">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="07a00-192">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![hello webbinställningar](./media/active-directory-saas-bonus-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="07a00-194">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="07a00-194">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![hello användardialogrutan](./media/active-directory-saas-bonus-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="07a00-196">a.</span><span class="sxs-lookup"><span data-stu-id="07a00-196">a.</span></span> <span data-ttu-id="07a00-197">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="07a00-197">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="07a00-198">b.</span><span class="sxs-lookup"><span data-stu-id="07a00-198">b.</span></span> <span data-ttu-id="07a00-199">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="07a00-199">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="07a00-200">c.</span><span class="sxs-lookup"><span data-stu-id="07a00-200">c.</span></span> <span data-ttu-id="07a00-201">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="07a00-201">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="07a00-202">d.</span><span class="sxs-lookup"><span data-stu-id="07a00-202">d.</span></span> <span data-ttu-id="07a00-203">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="07a00-203">Click **Create**.</span></span>
 
### <a name="create-a-bonusly-test-user"></a><span data-ttu-id="07a00-204">Skapa en Bonusly testanvändare</span><span class="sxs-lookup"><span data-stu-id="07a00-204">Create a Bonusly test user</span></span>

<span data-ttu-id="07a00-205">I ordning tooenable Azure AD-användare toolog i tooBonusly, måste de etableras i Bonusly.</span><span class="sxs-lookup"><span data-stu-id="07a00-205">In order tooenable Azure AD users toolog in tooBonusly, they must be provisioned into Bonusly.</span></span> <span data-ttu-id="07a00-206">Hello gäller Bonusly är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="07a00-206">In hello case of Bonusly, provisioning is a manual task.</span></span>

>[!NOTE]
><span data-ttu-id="07a00-207">Du kan använda något annat Bonusly användarens konto skapas verktyg eller API: er som tillhandahålls av Bonusly tooprovision AAD användarkonton.</span><span class="sxs-lookup"><span data-stu-id="07a00-207">You can use any other Bonusly user account creation tools or APIs provided by Bonusly tooprovision AAD user accounts.</span></span>
>  

<span data-ttu-id="07a00-208">**tooconfigure användaretablering, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="07a00-208">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="07a00-209">Logga in tooyour Bonusly innehavare i ett webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="07a00-209">In a web browser window, log in tooyour Bonusly tenant.</span></span>

2. <span data-ttu-id="07a00-210">Klicka på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="07a00-210">Click **Settings**.</span></span>
 
    <span data-ttu-id="07a00-211">![Inställningar för](./media/active-directory-saas-bonus-tutorial/ic781041.png "inställningar")</span><span class="sxs-lookup"><span data-stu-id="07a00-211">![Settings](./media/active-directory-saas-bonus-tutorial/ic781041.png "Settings")</span></span>

3. <span data-ttu-id="07a00-212">Klicka på hello **användare och tillägg** fliken.</span><span class="sxs-lookup"><span data-stu-id="07a00-212">Click hello **Users and bonuses** tab.</span></span>
   
    <span data-ttu-id="07a00-213">![Användare och tillägg](./media/active-directory-saas-bonus-tutorial/ic781042.png "användare och tillägg")</span><span class="sxs-lookup"><span data-stu-id="07a00-213">![Users and bonuses](./media/active-directory-saas-bonus-tutorial/ic781042.png "Users and bonuses")</span></span>

4. <span data-ttu-id="07a00-214">Klicka på **hantera användare**.</span><span class="sxs-lookup"><span data-stu-id="07a00-214">Click **Manage Users**.</span></span>
   
    <span data-ttu-id="07a00-215">![Hantera användare](./media/active-directory-saas-bonus-tutorial/ic781043.png "hantera användare")</span><span class="sxs-lookup"><span data-stu-id="07a00-215">![Manage Users](./media/active-directory-saas-bonus-tutorial/ic781043.png "Manage Users")</span></span>

5. <span data-ttu-id="07a00-216">Klicka på **lägga till användare**.</span><span class="sxs-lookup"><span data-stu-id="07a00-216">Click **Add User**.</span></span>
   
    <span data-ttu-id="07a00-217">![Lägg till användare](./media/active-directory-saas-bonus-tutorial/ic781044.png "lägga till användare")</span><span class="sxs-lookup"><span data-stu-id="07a00-217">![Add User](./media/active-directory-saas-bonus-tutorial/ic781044.png "Add User")</span></span>

6. <span data-ttu-id="07a00-218">På hello **Lägg till användare** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="07a00-218">On hello **Add User** dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="07a00-219">![Lägg till användare](./media/active-directory-saas-bonus-tutorial/ic781045.png "lägga till användare")</span><span class="sxs-lookup"><span data-stu-id="07a00-219">![Add User](./media/active-directory-saas-bonus-tutorial/ic781045.png "Add User")</span></span>  

    <span data-ttu-id="07a00-220">a.</span><span class="sxs-lookup"><span data-stu-id="07a00-220">a.</span></span> <span data-ttu-id="07a00-221">I hello **Förnamn** textruta anger hello först namnet på användaren som **Britta**.</span><span class="sxs-lookup"><span data-stu-id="07a00-221">In hello **First name** textbox, enter hello first name of user like **Britta**.</span></span>

    <span data-ttu-id="07a00-222">b.</span><span class="sxs-lookup"><span data-stu-id="07a00-222">b.</span></span> <span data-ttu-id="07a00-223">I hello **efternamn** textruta ange hello efternamn för användaren som **Simon**.</span><span class="sxs-lookup"><span data-stu-id="07a00-223">In hello **Last name** textbox, enter hello last name of user like **Simon**.</span></span>
 
    <span data-ttu-id="07a00-224">c.</span><span class="sxs-lookup"><span data-stu-id="07a00-224">c.</span></span> <span data-ttu-id="07a00-225">I hello **e-post** textruta ange hello e-postadress för användaren som  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="07a00-225">In hello **Email** textbox, enter hello email of user like **brittasimon@contoso.com**.</span></span>

    <span data-ttu-id="07a00-226">d.</span><span class="sxs-lookup"><span data-stu-id="07a00-226">d.</span></span> <span data-ttu-id="07a00-227">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="07a00-227">Click **Save**.</span></span>
   
     >[!NOTE]
     ><span data-ttu-id="07a00-228">hello Azure AD användare får ett e-postmeddelande som innehåller en länk tooconfirm hello-konto innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="07a00-228">hello Azure AD account holder receives an email that includes a link tooconfirm hello account before it becomes active.</span></span>
     >  

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="07a00-229">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="07a00-229">Assign hello Azure AD test user</span></span>

<span data-ttu-id="07a00-230">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooBonusly.</span><span class="sxs-lookup"><span data-stu-id="07a00-230">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooBonusly.</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="07a00-232">**tooassign Britta Simon tooBonusly utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="07a00-232">**tooassign Britta Simon tooBonusly, perform hello following steps:**</span></span>

1. <span data-ttu-id="07a00-233">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="07a00-233">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="07a00-235">Välj i listan med program hello **Bonusly**.</span><span class="sxs-lookup"><span data-stu-id="07a00-235">In hello applications list, select **Bonusly**.</span></span>

    ![Hej Bonusly länken i listan med program hello](./media/active-directory-saas-bonus-tutorial/tutorial_bonusly_app.png) 

3. <span data-ttu-id="07a00-237">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="07a00-237">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202] 

4. <span data-ttu-id="07a00-239">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="07a00-239">Click **Add** button.</span></span> <span data-ttu-id="07a00-240">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="07a00-240">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="07a00-242">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="07a00-242">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="07a00-243">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="07a00-243">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="07a00-244">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="07a00-244">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="07a00-245">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="07a00-245">Test single sign-on</span></span>

<span data-ttu-id="07a00-246">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="07a00-246">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="07a00-247">När du klickar på hello Bonusly panelen i Hej åtkomstpanelen, du får automatiskt inloggade tooyour Bonusly program.</span><span class="sxs-lookup"><span data-stu-id="07a00-247">When you click hello Bonusly tile in hello Access Panel, you should get automatically signed-on tooyour Bonusly application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="07a00-248">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="07a00-248">Additional resources</span></span>

* [<span data-ttu-id="07a00-249">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="07a00-249">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="07a00-250">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="07a00-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_203.png

