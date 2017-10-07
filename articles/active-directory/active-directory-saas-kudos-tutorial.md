---
title: "Självstudier: Azure Active Directory-integrering med bra jobbat | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Bra jobbat."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 39c47ce6-4944-47ba-8f53-3afa95398034
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jeedes
ms.openlocfilehash: c1b481463574461f9948db2b83843fefa5d74e99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kudos"></a><span data-ttu-id="574d0-103">Självstudier: Azure Active Directory-integrering med bra jobbat</span><span class="sxs-lookup"><span data-stu-id="574d0-103">Tutorial: Azure Active Directory integration with Kudos</span></span>

<span data-ttu-id="574d0-104">I kursen får du lära dig hur toointegrate Bra jobbat med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="574d0-104">In this tutorial, you learn how toointegrate Kudos with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="574d0-105">Integrera Bra jobbat med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="574d0-105">Integrating Kudos with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="574d0-106">Du kan styra i Azure AD som har åtkomst till tooKudos</span><span class="sxs-lookup"><span data-stu-id="574d0-106">You can control in Azure AD who has access tooKudos</span></span>
- <span data-ttu-id="574d0-107">Du kan aktivera din användare tooautomatically get inloggade tooKudos (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="574d0-107">You can enable your users tooautomatically get signed-on tooKudos (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="574d0-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="574d0-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="574d0-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="574d0-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="574d0-110">Krav</span><span class="sxs-lookup"><span data-stu-id="574d0-110">Prerequisites</span></span>

<span data-ttu-id="574d0-111">tooconfigure Azure AD-integrering med bra jobbat måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="574d0-111">tooconfigure Azure AD integration with Kudos, you need hello following items:</span></span>

- <span data-ttu-id="574d0-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="574d0-112">An Azure AD subscription</span></span>
- <span data-ttu-id="574d0-113">En bra jobbat enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="574d0-113">A Kudos single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="574d0-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="574d0-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="574d0-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="574d0-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="574d0-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="574d0-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="574d0-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="574d0-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="574d0-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="574d0-118">Scenario description</span></span>
<span data-ttu-id="574d0-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="574d0-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="574d0-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="574d0-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="574d0-121">Att lägga till Bra jobbat från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="574d0-121">Adding Kudos from hello gallery</span></span>
2. <span data-ttu-id="574d0-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="574d0-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-kudos-from-hello-gallery"></a><span data-ttu-id="574d0-123">Att lägga till Bra jobbat från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="574d0-123">Adding Kudos from hello gallery</span></span>
<span data-ttu-id="574d0-124">tooconfigure hello integrering av Bra jobbat i Azure AD, behöver du tooadd Bra jobbat hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="574d0-124">tooconfigure hello integration of Kudos into Azure AD, you need tooadd Kudos from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="574d0-125">**tooadd Bra jobbat från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="574d0-125">**tooadd Kudos from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="574d0-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="574d0-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="574d0-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="574d0-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="574d0-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="574d0-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="574d0-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="574d0-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="574d0-133">Skriv i sökrutan hello **Bra jobbat**.</span><span class="sxs-lookup"><span data-stu-id="574d0-133">In hello search box, type **Kudos**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kudos-tutorial/tutorial_kudos_search.png)

5. <span data-ttu-id="574d0-135">Markera hello resultat på panelen **Bra jobbat**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="574d0-135">In hello results panel, select **Kudos**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kudos-tutorial/tutorial_kudos_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="574d0-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="574d0-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="574d0-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med bra jobbat baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="574d0-138">In this section, you configure and test Azure AD single sign-on with Kudos based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="574d0-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Bra jobbat är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="574d0-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Kudos is tooa user in Azure AD.</span></span> <span data-ttu-id="574d0-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Bra jobbat toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="574d0-140">In other words, a link relationship between an Azure AD user and hello related user in Kudos needs toobe established.</span></span>

<span data-ttu-id="574d0-141">I Bra jobbat, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="574d0-141">In Kudos, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="574d0-142">tooconfigure och testa Azure AD enkel inloggning med bra jobbat, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="574d0-142">tooconfigure and test Azure AD single sign-on with Kudos, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="574d0-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="574d0-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="574d0-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="574d0-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="574d0-145">**[Skapa en bra jobbat testanvändare](#creating-a-kudos-test-user)**  -toohave en motsvarighet för Britta Simon i Bra jobbat som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="574d0-145">**[Creating a Kudos test user](#creating-a-kudos-test-user)** - toohave a counterpart of Britta Simon in Kudos that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="574d0-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="574d0-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="574d0-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="574d0-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="574d0-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="574d0-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="574d0-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Bra jobbat.</span><span class="sxs-lookup"><span data-stu-id="574d0-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Kudos application.</span></span>

<span data-ttu-id="574d0-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med bra jobbat:**</span><span class="sxs-lookup"><span data-stu-id="574d0-150">**tooconfigure Azure AD single sign-on with Kudos, perform hello following steps:**</span></span>

1. <span data-ttu-id="574d0-151">I hello Azure-portalen på hello **Bra jobbat** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="574d0-151">In hello Azure portal, on hello **Kudos** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="574d0-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="574d0-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-kudos-tutorial/tutorial_kudos_samlbase.png)

3. <span data-ttu-id="574d0-155">På hello **Bra jobbat domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="574d0-155">On hello **Kudos Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kudos-tutorial/tutorial_kudos_url.png)

    <span data-ttu-id="574d0-157">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company>.kudosnow.com`</span><span class="sxs-lookup"><span data-stu-id="574d0-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<company>.kudosnow.com`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="574d0-158">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="574d0-158">This value is not real.</span></span> <span data-ttu-id="574d0-159">Uppdatera det här värdet med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="574d0-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="574d0-160">Kontakta [Bra jobbat klienten supportteamet](http://success.kudosnow.com/home) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="574d0-160">Contact [Kudos Client support team](http://success.kudosnow.com/home) tooget this value.</span></span> 
 
4. <span data-ttu-id="574d0-161">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="574d0-161">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kudos-tutorial/tutorial_kudos_certificate.png) 

5. <span data-ttu-id="574d0-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="574d0-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kudos-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="574d0-165">På hello **Bra jobbat Configuration** klickar du på **konfigurera Bra jobbat** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="574d0-165">On hello **Kudos Configuration** section, click **Configure Kudos** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="574d0-166">Kopiera hello **Sign-Out Webbadressen och SAML enkel inloggning Service** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="574d0-166">Copy hello **Sign-Out URL and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kudos-tutorial/tutorial_kudos_configure.png) 

7. <span data-ttu-id="574d0-168">Logga in på webbplatsen Bra jobbat företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="574d0-168">In a different web browser window, log into your Kudos company site as an administrator.</span></span>

8. <span data-ttu-id="574d0-169">Hello-menyn överst hello **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="574d0-169">In hello menu on hello top, click **Settings**.</span></span>
   
    <span data-ttu-id="574d0-170">![Inställningar för](./media/active-directory-saas-kudos-tutorial/ic787806.png "inställningar")</span><span class="sxs-lookup"><span data-stu-id="574d0-170">![Settings](./media/active-directory-saas-kudos-tutorial/ic787806.png "Settings")</span></span>

9. <span data-ttu-id="574d0-171">Klicka på **integreringar \> SSO**.</span><span class="sxs-lookup"><span data-stu-id="574d0-171">Click **Integrations \> SSO**.</span></span>

10. <span data-ttu-id="574d0-172">I hello **SSO** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="574d0-172">In hello **SSO** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="574d0-173">![SSO](./media/active-directory-saas-kudos-tutorial/ic787807.png "ENKEL INLOGGNING")</span><span class="sxs-lookup"><span data-stu-id="574d0-173">![SSO](./media/active-directory-saas-kudos-tutorial/ic787807.png "SSO")</span></span>
   
    <span data-ttu-id="574d0-174">a.</span><span class="sxs-lookup"><span data-stu-id="574d0-174">a.</span></span> <span data-ttu-id="574d0-175">I **inloggning URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="574d0-175">In **Sign on URL** textbox, paste hello value of  **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="574d0-176">b.</span><span class="sxs-lookup"><span data-stu-id="574d0-176">b.</span></span> <span data-ttu-id="574d0-177">Öppna din Base64-kodade certifikatet i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den toohello **X.509-certifikat** textruta</span><span class="sxs-lookup"><span data-stu-id="574d0-177">Open your base-64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it toohello **X.509 certificate** textbox</span></span>
   
    <span data-ttu-id="574d0-178">c.</span><span class="sxs-lookup"><span data-stu-id="574d0-178">c.</span></span> <span data-ttu-id="574d0-179">I **logga ut tooURL**, klistra in hello värdet för **Sign-Out URL** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="574d0-179">In **Logout tooURL**, paste hello value of  **Sign-Out URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="574d0-180">d.</span><span class="sxs-lookup"><span data-stu-id="574d0-180">d.</span></span> <span data-ttu-id="574d0-181">I hello **din Bra jobbat URL** textruta Ange företagets namn.</span><span class="sxs-lookup"><span data-stu-id="574d0-181">In hello **Your Kudos URL** textbox, type your company name.</span></span>
   
    <span data-ttu-id="574d0-182">e.</span><span class="sxs-lookup"><span data-stu-id="574d0-182">e.</span></span> <span data-ttu-id="574d0-183">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="574d0-183">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="574d0-184">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="574d0-184">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="574d0-185">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="574d0-185">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="574d0-186">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="574d0-186">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="574d0-187">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="574d0-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="574d0-188">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="574d0-188">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="574d0-190">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="574d0-190">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="574d0-191">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="574d0-191">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kudos-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="574d0-193">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="574d0-193">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kudos-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="574d0-195">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="574d0-195">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kudos-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="574d0-197">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="574d0-197">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kudos-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="574d0-199">a.</span><span class="sxs-lookup"><span data-stu-id="574d0-199">a.</span></span> <span data-ttu-id="574d0-200">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="574d0-200">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="574d0-201">b.</span><span class="sxs-lookup"><span data-stu-id="574d0-201">b.</span></span> <span data-ttu-id="574d0-202">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="574d0-202">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="574d0-203">c.</span><span class="sxs-lookup"><span data-stu-id="574d0-203">c.</span></span> <span data-ttu-id="574d0-204">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="574d0-204">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="574d0-205">d.</span><span class="sxs-lookup"><span data-stu-id="574d0-205">d.</span></span> <span data-ttu-id="574d0-206">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="574d0-206">Click **Create**.</span></span>
 
### <a name="creating-a-kudos-test-user"></a><span data-ttu-id="574d0-207">Skapa en bra jobbat testanvändare</span><span class="sxs-lookup"><span data-stu-id="574d0-207">Creating a Kudos test user</span></span>

<span data-ttu-id="574d0-208">I ordning tooenable Azure AD-användare toolog till Bra jobbat, måste de etableras i Bra jobbat.</span><span class="sxs-lookup"><span data-stu-id="574d0-208">In order tooenable Azure AD users toolog into Kudos, they must be provisioned into Kudos.</span></span> 

<span data-ttu-id="574d0-209">Bra jobbat hello gäller är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="574d0-209">In hello case of Kudos, provisioning is a manual task.</span></span>

<span data-ttu-id="574d0-210">**tooprovision ett användarkonto, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="574d0-210">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="574d0-211">Logga in tooyour **Bra jobbat** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="574d0-211">Log in tooyour **Kudos** company site as administrator.</span></span>

2. <span data-ttu-id="574d0-212">Hello-menyn överst hello **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="574d0-212">In hello menu on hello top, click **Settings**.</span></span>
   
   <span data-ttu-id="574d0-213">![Inställningar för](./media/active-directory-saas-kudos-tutorial/ic787806.png "inställningar")</span><span class="sxs-lookup"><span data-stu-id="574d0-213">![Settings](./media/active-directory-saas-kudos-tutorial/ic787806.png "Settings")</span></span>

3. <span data-ttu-id="574d0-214">Klicka på **Användaradministration**.</span><span class="sxs-lookup"><span data-stu-id="574d0-214">Click **User Admin**.</span></span>

4. <span data-ttu-id="574d0-215">Klicka på hello **användare** fliken och klicka sedan på **lägga till en användare**.</span><span class="sxs-lookup"><span data-stu-id="574d0-215">Click hello **Users** tab, and then click **Add a User**.</span></span>
   
   <span data-ttu-id="574d0-216">![Användaradministration](./media/active-directory-saas-kudos-tutorial/ic787809.png "Användaradministration")</span><span class="sxs-lookup"><span data-stu-id="574d0-216">![User Admin](./media/active-directory-saas-kudos-tutorial/ic787809.png "User Admin")</span></span>

5. <span data-ttu-id="574d0-217">I hello **lägga till en användare** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="574d0-217">In hello **Add a User** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="574d0-218">![Lägga till en användare](./media/active-directory-saas-kudos-tutorial/ic787810.png "lägga till en användare")</span><span class="sxs-lookup"><span data-stu-id="574d0-218">![Add a User](./media/active-directory-saas-kudos-tutorial/ic787810.png "Add a User")</span></span>
   
    <span data-ttu-id="574d0-219">a.</span><span class="sxs-lookup"><span data-stu-id="574d0-219">a.</span></span> <span data-ttu-id="574d0-220">Typen hello **Förnamn**, **efternamn**, **e-post** och annan information om ett giltigt Azure Active Directory-konto som du vill använda tooprovision hello relaterade textrutor.</span><span class="sxs-lookup"><span data-stu-id="574d0-220">Type hello **First Name**, **Last Name**, **Email** and other details of a valid Azure Active Directory account you want tooprovision into hello related textboxes.</span></span>
   
    <span data-ttu-id="574d0-221">b.</span><span class="sxs-lookup"><span data-stu-id="574d0-221">b.</span></span> <span data-ttu-id="574d0-222">Klicka på **skapa användare**.</span><span class="sxs-lookup"><span data-stu-id="574d0-222">Click **Create User**.</span></span>

>[!NOTE]
><span data-ttu-id="574d0-223">Du kan använda något annat Bra jobbat användarens konto skapas verktyg eller API: er som tillhandahålls av Bra jobbat tooprovision AAD användarkonton.</span><span class="sxs-lookup"><span data-stu-id="574d0-223">You can use any other Kudos user account creation tools or APIs provided by Kudos tooprovision AAD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="574d0-224">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="574d0-224">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="574d0-225">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooKudos.</span><span class="sxs-lookup"><span data-stu-id="574d0-225">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooKudos.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="574d0-227">**tooassign Britta Simon tooKudos utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="574d0-227">**tooassign Britta Simon tooKudos, perform hello following steps:**</span></span>

1. <span data-ttu-id="574d0-228">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="574d0-228">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="574d0-230">Välj i listan med program hello **Bra jobbat**.</span><span class="sxs-lookup"><span data-stu-id="574d0-230">In hello applications list, select **Kudos**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kudos-tutorial/tutorial_kudos_app.png) 

3. <span data-ttu-id="574d0-232">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="574d0-232">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="574d0-234">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="574d0-234">Click **Add** button.</span></span> <span data-ttu-id="574d0-235">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="574d0-235">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="574d0-237">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="574d0-237">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="574d0-238">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="574d0-238">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="574d0-239">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="574d0-239">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="574d0-240">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="574d0-240">Testing single sign-on</span></span>

<span data-ttu-id="574d0-241">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="574d0-241">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="574d0-242">Du bör få automatiskt inloggade tooyour Bra jobbat programmet när du klickar på hello Bra jobbat panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="574d0-242">When you click hello Kudos tile in hello Access Panel, you should get automatically signed-on tooyour Kudos application.</span></span> <span data-ttu-id="574d0-243">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="574d0-243">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="574d0-244">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="574d0-244">Additional resources</span></span>

* [<span data-ttu-id="574d0-245">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="574d0-245">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="574d0-246">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="574d0-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kudos-tutorial/tutorial_general_203.png

