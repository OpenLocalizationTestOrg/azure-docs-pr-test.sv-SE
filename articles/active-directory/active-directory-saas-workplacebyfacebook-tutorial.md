---
title: "Självstudier: Azure Active Directory-integrering med arbetsplats av Facebook | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och arbetsplats med Facebook."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 30f2ee64-95d3-44ef-b832-8a0a27e2967c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: f71a59527394730757d501a973251dc293fd3683
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workplace-by-facebook"></a><span data-ttu-id="75706-103">Självstudier: Azure Active Directory-integrering med arbetsplats av Facebook</span><span class="sxs-lookup"><span data-stu-id="75706-103">Tutorial: Azure Active Directory integration with Workplace by Facebook</span></span>

<span data-ttu-id="75706-104">I kursen får du lära dig hur toointegrate arbetsplats av Facebook med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="75706-104">In this tutorial, you learn how toointegrate Workplace by Facebook with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="75706-105">Integrera arbetsplats av Facebook med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="75706-105">Integrating Workplace by Facebook with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="75706-106">Du kan styra i Azure AD som har åtkomst till tooWorkplace av Facebook</span><span class="sxs-lookup"><span data-stu-id="75706-106">You can control in Azure AD who has access tooWorkplace by Facebook</span></span>
- <span data-ttu-id="75706-107">Du kan aktivera din användare tooautomatically get inloggade tooWorkplace av Facebook (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="75706-107">You can enable your users tooautomatically get signed-on tooWorkplace by Facebook (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="75706-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="75706-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="75706-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="75706-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="75706-110">Krav</span><span class="sxs-lookup"><span data-stu-id="75706-110">Prerequisites</span></span>

<span data-ttu-id="75706-111">tooconfigure Azure AD-integrering med arbetsplats av Facebook, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="75706-111">tooconfigure Azure AD integration with Workplace by Facebook, you need hello following items:</span></span>

- <span data-ttu-id="75706-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="75706-112">An Azure AD subscription</span></span>
- <span data-ttu-id="75706-113">En arbetsplats av Facebook enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="75706-113">A Workplace by Facebook single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="75706-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="75706-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="75706-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="75706-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="75706-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="75706-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="75706-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="75706-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="75706-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="75706-118">Scenario description</span></span>
<span data-ttu-id="75706-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="75706-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="75706-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="75706-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="75706-121">Att lägga till arbetsplatsen av Facebook från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="75706-121">Adding Workplace by Facebook from hello gallery</span></span>
2. <span data-ttu-id="75706-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="75706-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-workplace-by-facebook-from-hello-gallery"></a><span data-ttu-id="75706-123">Att lägga till arbetsplatsen av Facebook från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="75706-123">Adding Workplace by Facebook from hello gallery</span></span>
<span data-ttu-id="75706-124">tooconfigure hello integrering av arbetsplatsen av Facebook i Azure AD, behöver du tooadd arbetsplats av Facebook hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="75706-124">tooconfigure hello integration of Workplace by Facebook into Azure AD, you need tooadd Workplace by Facebook from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="75706-125">**tooadd arbetsplats av Facebook från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="75706-125">**tooadd Workplace by Facebook from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="75706-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="75706-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="75706-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="75706-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="75706-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="75706-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="75706-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="75706-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="75706-133">Skriv i sökrutan hello **arbetsplats av Facebook**.</span><span class="sxs-lookup"><span data-stu-id="75706-133">In hello search box, type **Workplace by Facebook**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_search.png)

5. <span data-ttu-id="75706-135">Markera hello resultat på panelen **arbetsplats av Facebook**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="75706-135">In hello results panel, select **Workplace by Facebook**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="75706-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="75706-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="75706-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning till arbetsplats av Facebook baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="75706-138">In this section, you configure and test Azure AD single sign-on with Workplace by Facebook based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="75706-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren arbetsplatsen av Facebook är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="75706-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Workplace by Facebook is tooa user in Azure AD.</span></span> <span data-ttu-id="75706-140">Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i arbetsplats av Facebook toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="75706-140">In other words, a link relationship between an Azure AD user and hello related user in Workplace by Facebook needs toobe established.</span></span>

<span data-ttu-id="75706-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** arbetsplatsen av Facebook.</span><span class="sxs-lookup"><span data-stu-id="75706-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Workplace by Facebook.</span></span>

<span data-ttu-id="75706-142">tooconfigure och testa Azure AD enkel inloggning till arbetsplats av Facebook, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="75706-142">tooconfigure and test Azure AD single sign-on with Workplace by Facebook, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="75706-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="75706-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="75706-144">**[Konfigurera omautentisering frekvens](#configuring-reauthentication-frequency)**  -tooconfigure arbetsplats tooprompt för en SAML-kontroll.</span><span class="sxs-lookup"><span data-stu-id="75706-144">**[Configuring Reauthentication Frequency](#configuring-reauthentication-frequency)** - tooconfigure Workplace tooprompt for a SAML check.</span></span>
3. <span data-ttu-id="75706-145">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="75706-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="75706-146">**[Skapa en arbetsplats av Facebook testanvändare](#creating-a-workplace-by-facebook-test-user)**  -toohave en motsvarighet för Britta Simon arbetsplatsen av Facebook som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="75706-146">**[Creating a Workplace by Facebook test user](#creating-a-workplace-by-facebook-test-user)** - toohave a counterpart of Britta Simon in Workplace by Facebook that is linked toohello Azure AD representation of user.</span></span>
5. <span data-ttu-id="75706-147">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="75706-147">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
6. <span data-ttu-id="75706-148">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="75706-148">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="75706-149">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="75706-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="75706-150">I det här avsnittet kan du aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning på din arbetsplats genom Facebook-programmet.</span><span class="sxs-lookup"><span data-stu-id="75706-150">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Workplace by Facebook application.</span></span>

<span data-ttu-id="75706-151">**tooconfigure Azure AD enkel inloggning till arbetsplats av Facebook, utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="75706-151">**tooconfigure Azure AD single sign-on with Workplace by Facebook, perform hello following steps:**</span></span>

1. <span data-ttu-id="75706-152">I hello Azure-portalen på hello **arbetsplats av Facebook** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="75706-152">In hello Azure portal, on hello **Workplace by Facebook** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="75706-154">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="75706-154">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_samlbase.png)

3. <span data-ttu-id="75706-156">På hello **arbetsplats Facebook-domänen och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="75706-156">On hello **Workplace by Facebook Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_url.png)

    <span data-ttu-id="75706-158">a.</span><span class="sxs-lookup"><span data-stu-id="75706-158">a.</span></span> <span data-ttu-id="75706-159">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<instancename>.facebook.com`</span><span class="sxs-lookup"><span data-stu-id="75706-159">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<instancename>.facebook.com`</span></span>

    <span data-ttu-id="75706-160">b.</span><span class="sxs-lookup"><span data-stu-id="75706-160">b.</span></span> <span data-ttu-id="75706-161">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://www.facebook.com/company/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="75706-161">In hello **Identifier** textbox, type a URL using hello following pattern: `https://www.facebook.com/company/<instancename>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="75706-162">Dessa värden är inte hello verkliga.</span><span class="sxs-lookup"><span data-stu-id="75706-162">These values are not hello real.</span></span> <span data-ttu-id="75706-163">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="75706-163">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="75706-164">Kontakta [arbetsplats av Facebook klienten supportteamet](https://workplace.fb.com/faq/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="75706-164">Contact [Workplace by Facebook Client support team](https://workplace.fb.com/faq/) tooget these values.</span></span> 

4. <span data-ttu-id="75706-165">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="75706-165">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_certificate.png) 

5. <span data-ttu-id="75706-167">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="75706-167">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="75706-169">På hello **arbetsplats Facebook konfigurationen** klickar du på **konfigurera arbetsplats av Facebook** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="75706-169">On hello **Workplace by Facebook Configuration** section, click **Configure Workplace by Facebook** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="75706-170">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="75706-170">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workplacebyfacebook-tutorial/config.png) 

7. <span data-ttu-id="75706-172">I en annan webbläsarfönstret, inloggning tooyour arbetsplats av Facebook företagets plats som administratör.</span><span class="sxs-lookup"><span data-stu-id="75706-172">In a different web browser window, login tooyour Workplace by Facebook company site as an administrator.</span></span>
  
   > [!NOTE] 
   > <span data-ttu-id="75706-173">Som en del av hello SAML autentiseringsprocessen, kan arbetsplatsen använda frågesträngar för in too2.5 kilobyte i storlek i ordning toopass parametrar tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="75706-173">As part of hello SAML authentication process, Workplace may utilize query strings of up too2.5 kilobytes in size in order toopass parameters tooAzure AD.</span></span>

8. <span data-ttu-id="75706-174">I hello **företagets instrumentpanelen**, gå toohello **autentisering** fliken.</span><span class="sxs-lookup"><span data-stu-id="75706-174">In hello **Company Dashboard**, go toohello **Authentication** tab.</span></span>

9. <span data-ttu-id="75706-175">Under **SAML-autentisering**väljer **SSO endast** hello nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="75706-175">Under **SAML Authentication**, select **SSO Only** from hello drop-down list.</span></span>

10. <span data-ttu-id="75706-176">Inkommande hello värden kopieras från **arbetsplats Facebook konfigurationen** avsnitt i hello Azure-portalen i hello motsvarande fält:</span><span class="sxs-lookup"><span data-stu-id="75706-176">Input hello values copied from **Workplace by Facebook Configuration** section of hello Azure portal into hello corresponding fields:</span></span>

    *   <span data-ttu-id="75706-177">I **SAML URL** textruta klistra in hello värdet för **inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="75706-177">In **SAML URL** textbox, paste hello value of **Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
    *   <span data-ttu-id="75706-178">I **utfärdar-URL för SAML-textrutan**, klistra in hello värdet för **SAML enhets-ID**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="75706-178">In **SAML Issuer URL textbox**, paste hello value of **SAML Entity ID**, which you have copied from Azure portal.</span></span>
    *   <span data-ttu-id="75706-179">I **SAML logga ut omdirigera** (valfritt), klistra in hello värdet för **Sign-Out URL**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="75706-179">In **SAML Logout Redirect** (Optional), paste hello value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>
    *   <span data-ttu-id="75706-180">Öppna din **Base64-kodat certifikat** i anteckningar som hämtas från Azure-portalen, kopiera hello innehållet i den till Urklipp och klistra in den toothe **SAML certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="75706-180">Open your **base-64 encoded certificate** in notepad downloaded from Azure portal, copy hello content of it into your clipboard, and then paste it toothe **SAML Certificate** textbox.</span></span>

11. <span data-ttu-id="75706-181">Du kan behöva tooenter hello målgruppen URL mottagaren URL och ACS (Assertion konsumenten Service)-URL som visas under hello **SAML-konfiguration** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="75706-181">You may need tooenter hello Audience URL, Recipient URL, and ACS (Assertion Consumer Service) URL listed under hello **SAML Configuration** section.</span></span>

12. <span data-ttu-id="75706-182">Rulla toohello längst ned på hello avsnittet och klicka på hello **Test SSO** knappen.</span><span class="sxs-lookup"><span data-stu-id="75706-182">Scroll toohello bottom of hello section and click hello **Test SSO** button.</span></span> <span data-ttu-id="75706-183">Detta resulterar i ett popup-fönster som visas med Azure AD-inloggningssidan visas.</span><span class="sxs-lookup"><span data-stu-id="75706-183">This results in a popup window appearing with Azure AD login page presented.</span></span> <span data-ttu-id="75706-184">Ange dina autentiseringsuppgifter i som normal tooauthenticate.</span><span class="sxs-lookup"><span data-stu-id="75706-184">Enter your credentials in as normal tooauthenticate.</span></span> 

    <span data-ttu-id="75706-185">**Felsöka:** Kontrollera hello e-postadress som returneras från Azure AD är hello samma som hello arbetsplatskonto som du är inloggad med.</span><span class="sxs-lookup"><span data-stu-id="75706-185">**Troubleshooting:** Ensure hello email address being returned back from Azure AD is hello same as hello Workplace account you are logged in with.</span></span>

13. <span data-ttu-id="75706-186">När hello test har slutförts, bläddra toohello längst ned på sidan för hello och klicka på hello **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="75706-186">Once hello test has been completed successfully, scroll toohello bottom of hello page and click hello **Save** button.</span></span>

14. <span data-ttu-id="75706-187">Alla användare som använder arbetsplats visas nu med Azure AD-inloggningssidan för autentisering.</span><span class="sxs-lookup"><span data-stu-id="75706-187">All users using Workplace will now be presented with Azure AD login page for authentication.</span></span>

15. <span data-ttu-id="75706-188">**SAML logga ut omdirigera (valfritt)** -</span><span class="sxs-lookup"><span data-stu-id="75706-188">**SAML Logout Redirect (optional)** -</span></span> 

    <span data-ttu-id="75706-189">Du kan välja toooptionally konfigurera en Url för SAML logga ut, vilket kan vara används toopoint på sidan för Azure AD logga ut.</span><span class="sxs-lookup"><span data-stu-id="75706-189">You can choose toooptionally configure a SAML Logout Url, which can be used toopoint at Azure AD's logout page.</span></span> <span data-ttu-id="75706-190">När den här inställningen har aktiverats och konfigurerats, kommer hello användaren inte längre dirigerad toohello arbetsplats logga ut sidan.</span><span class="sxs-lookup"><span data-stu-id="75706-190">When this setting is enabled and configured, hello user will no longer be directed toohello Workplace logout page.</span></span> <span data-ttu-id="75706-191">Hello användaren kommer i stället att omdirigerade toohello url som har lagts till i hello SAML logga ut omdirigera inställningen.</span><span class="sxs-lookup"><span data-stu-id="75706-191">Instead, hello user will be redirected toohello url that was added in hello SAML Logout Redirect setting.</span></span>


> [!TIP]
> <span data-ttu-id="75706-192">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="75706-192">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="75706-193">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="75706-193">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="75706-194">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="75706-194">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="configuring-reauthentication-frequency"></a><span data-ttu-id="75706-195">Konfigurera omautentisering frekvens</span><span class="sxs-lookup"><span data-stu-id="75706-195">Configuring Reauthentication Frequency</span></span>

<span data-ttu-id="75706-196">Du kan konfigurera arbetsplats tooprompt för SAML-kontrollen varje dag, tre dagar i veckan, vecka, månad eller aldrig.</span><span class="sxs-lookup"><span data-stu-id="75706-196">You can configure Workplace tooprompt for a SAML check every day, three days, week, two weeks, month or never.</span></span>

> [!NOTE] 
><span data-ttu-id="75706-197">hello minsta hello SAML-kontroll av mobila program värdet för tooone vecka.</span><span class="sxs-lookup"><span data-stu-id="75706-197">hello minimum value for hello SAML check on mobile applications is set tooone week.</span></span>

<span data-ttu-id="75706-198">Du kan också tvinga en SAML återställa för alla användare som använder hello knappen: kräver SAML-autentisering för alla användare nu.</span><span class="sxs-lookup"><span data-stu-id="75706-198">You can also force a SAML reset for all users using hello button: Require SAML authentication for all users now.</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="75706-199">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="75706-199">Creating an Azure AD test user</span></span>
<span data-ttu-id="75706-200">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="75706-200">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="75706-202">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="75706-202">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="75706-203">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="75706-203">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="75706-205">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="75706-205">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="75706-207">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="75706-207">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="75706-209">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="75706-209">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="75706-211">a.</span><span class="sxs-lookup"><span data-stu-id="75706-211">a.</span></span> <span data-ttu-id="75706-212">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="75706-212">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="75706-213">b.</span><span class="sxs-lookup"><span data-stu-id="75706-213">b.</span></span> <span data-ttu-id="75706-214">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="75706-214">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="75706-215">c.</span><span class="sxs-lookup"><span data-stu-id="75706-215">c.</span></span> <span data-ttu-id="75706-216">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="75706-216">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="75706-217">d.</span><span class="sxs-lookup"><span data-stu-id="75706-217">d.</span></span> <span data-ttu-id="75706-218">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="75706-218">Click **Create**.</span></span>
 
### <a name="creating-a-workplace-by-facebook-test-user"></a><span data-ttu-id="75706-219">Skapa en arbetsplats av Facebook testanvändare</span><span class="sxs-lookup"><span data-stu-id="75706-219">Creating a Workplace by Facebook test user</span></span>

<span data-ttu-id="75706-220">I det här avsnittet skapas en användare som kallas Britta Simon i arbetsplats med Facebook.</span><span class="sxs-lookup"><span data-stu-id="75706-220">In this section, a user called Britta Simon is created in Workplace by Facebook.</span></span> <span data-ttu-id="75706-221">Arbetsplats av Facebook stöder just-in-time-allokering som är aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="75706-221">Workplace by Facebook supports just-in-time provisioning, which is enabled by default.</span></span>

<span data-ttu-id="75706-222">Det finns inga åtgärder i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="75706-222">There is no action for you in this section.</span></span> <span data-ttu-id="75706-223">Om en användare inte finns i arbetsplats av Facebook, skapas en ny när du försöker tooaccess arbetsplats med Facebook.</span><span class="sxs-lookup"><span data-stu-id="75706-223">If a user doesn't exist in Workplace by Facebook, a new one is created when you attempt tooaccess Workplace by Facebook.</span></span>

>[!Note]
><span data-ttu-id="75706-224">Om du behöver en användare manuellt, kontakta toocreate [arbetsplats av Facebook klienten supportteamet](https://workplace.fb.com/faq/)</span><span class="sxs-lookup"><span data-stu-id="75706-224">If you need toocreate a user manually, Contact [Workplace by Facebook Client support team](https://workplace.fb.com/faq/)</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="75706-225">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="75706-225">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="75706-226">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooWorkplace av Facebook.</span><span class="sxs-lookup"><span data-stu-id="75706-226">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooWorkplace by Facebook.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="75706-228">**tooassign Britta Simon tooWorkplace av Facebook, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="75706-228">**tooassign Britta Simon tooWorkplace by Facebook, perform hello following steps:**</span></span>

1. <span data-ttu-id="75706-229">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="75706-229">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="75706-231">Välj i listan med program hello **arbetsplats av Facebook**.</span><span class="sxs-lookup"><span data-stu-id="75706-231">In hello applications list, select **Workplace by Facebook**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_app.png) 

3. <span data-ttu-id="75706-233">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="75706-233">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="75706-235">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="75706-235">Click **Add** button.</span></span> <span data-ttu-id="75706-236">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="75706-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="75706-238">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="75706-238">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="75706-239">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="75706-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="75706-240">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="75706-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="75706-241">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="75706-241">Testing single sign-on</span></span>

<span data-ttu-id="75706-242">Om du vill tootest dina inställningar för enkel inloggning, öppna hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="75706-242">If you want tootest your single sign-on settings, open hello Access Panel.</span></span>
<span data-ttu-id="75706-243">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="75706-243">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="75706-244">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="75706-244">Additional resources</span></span>

* [<span data-ttu-id="75706-245">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="75706-245">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="75706-246">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="75706-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="75706-247">Konfigurera Användaretablering</span><span class="sxs-lookup"><span data-stu-id="75706-247">Configure User Provisioning</span></span>](active-directory-saas-workplacebyfacebook-provisioning-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_203.png

