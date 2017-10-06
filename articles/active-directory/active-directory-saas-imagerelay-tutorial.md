---
title: "Självstudier: Azure Active Directory-integrering med bild Relay | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och avbildningen Relay."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 65bb5990-07ef-4244-9f41-cd28fc2cb5a2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: baf39e4437b85c2de5b524984ad5ca39badbab63
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-image-relay"></a><span data-ttu-id="803ac-103">Självstudier: Azure Active Directory-integrering med bild Relay</span><span class="sxs-lookup"><span data-stu-id="803ac-103">Tutorial: Azure Active Directory integration with Image Relay</span></span>

<span data-ttu-id="803ac-104">I kursen får du lära dig hur toointegrate avbildningen Relay med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="803ac-104">In this tutorial, you learn how toointegrate Image Relay with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="803ac-105">Integrera avbildningen Relay med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="803ac-105">Integrating Image Relay with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="803ac-106">Du kan styra i Azure AD som har åtkomst tooImage Relay</span><span class="sxs-lookup"><span data-stu-id="803ac-106">You can control in Azure AD who has access tooImage Relay</span></span>
- <span data-ttu-id="803ac-107">Du kan aktivera din användare tooautomatically get inloggade tooImage Relay (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="803ac-107">You can enable your users tooautomatically get signed-on tooImage Relay (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="803ac-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="803ac-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="803ac-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="803ac-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="803ac-110">Krav</span><span class="sxs-lookup"><span data-stu-id="803ac-110">Prerequisites</span></span>

<span data-ttu-id="803ac-111">tooconfigure Azure AD-integrering med bild Relay måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="803ac-111">tooconfigure Azure AD integration with Image Relay, you need hello following items:</span></span>

- <span data-ttu-id="803ac-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="803ac-112">An Azure AD subscription</span></span>
- <span data-ttu-id="803ac-113">En avbildning Relay enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="803ac-113">An Image Relay single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="803ac-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="803ac-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="803ac-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="803ac-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="803ac-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="803ac-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="803ac-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="803ac-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="803ac-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="803ac-118">Scenario description</span></span>
<span data-ttu-id="803ac-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="803ac-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="803ac-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="803ac-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="803ac-121">Att lägga till bilden Relay från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="803ac-121">Adding Image Relay from hello gallery</span></span>
2. <span data-ttu-id="803ac-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="803ac-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-image-relay-from-hello-gallery"></a><span data-ttu-id="803ac-123">Att lägga till bilden Relay från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="803ac-123">Adding Image Relay from hello gallery</span></span>
<span data-ttu-id="803ac-124">tooconfigure hello integrering av avbildningen Relay i Azure AD, behöver du tooadd avbildningen Relay hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="803ac-124">tooconfigure hello integration of Image Relay into Azure AD, you need tooadd Image Relay from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="803ac-125">**tooadd bild Relay från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="803ac-125">**tooadd Image Relay from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="803ac-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="803ac-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="803ac-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="803ac-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="803ac-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="803ac-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="803ac-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="803ac-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="803ac-133">Skriv i sökrutan hello **avbildningen Relay**.</span><span class="sxs-lookup"><span data-stu-id="803ac-133">In hello search box, type **Image Relay**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_search.png)

5. <span data-ttu-id="803ac-135">Markera hello resultat på panelen **avbildningen Relay**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="803ac-135">In hello results panel, select **Image Relay**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="803ac-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="803ac-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="803ac-138">I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med bild Relay baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="803ac-138">In this section, you configure and test Azure AD single sign-on with Image Relay based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="803ac-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i avbildningen Relay är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="803ac-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Image Relay is tooa user in Azure AD.</span></span> <span data-ttu-id="803ac-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i avbildningen Relay toobe upprätta.</span><span class="sxs-lookup"><span data-stu-id="803ac-140">In other words, a link relationship between an Azure AD user and hello related user in Image Relay needs toobe established.</span></span>

<span data-ttu-id="803ac-141">I bild Relay tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="803ac-141">In Image Relay, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="803ac-142">tooconfigure och testa Azure AD enkel inloggning med bild Relay måste toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="803ac-142">tooconfigure and test Azure AD single sign-on with Image Relay, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="803ac-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="803ac-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="803ac-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="803ac-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="803ac-145">**[Skapa en avbildning Relay testanvändare](#creating-an-image-relay-test-user)**  -toohave en motsvarighet för Britta Simon i avbildningen Relay som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="803ac-145">**[Creating an Image Relay test user](#creating-an-image-relay-test-user)** - toohave a counterpart of Britta Simon in Image Relay that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="803ac-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="803ac-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="803ac-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="803ac-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="803ac-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="803ac-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="803ac-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i avbildningen Relay-program.</span><span class="sxs-lookup"><span data-stu-id="803ac-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Image Relay application.</span></span>

<span data-ttu-id="803ac-150">**Utför följande hello tooconfigure Azure AD enkel inloggning med bild Relay:**</span><span class="sxs-lookup"><span data-stu-id="803ac-150">**tooconfigure Azure AD single sign-on with Image Relay, perform hello following steps:**</span></span>

1. <span data-ttu-id="803ac-151">I hello Azure-portalen på hello **avbildningen Relay** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="803ac-151">In hello Azure portal, on hello **Image Relay** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="803ac-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="803ac-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_samlbase.png)

3. <span data-ttu-id="803ac-155">På hello **avbildningen Relay domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="803ac-155">On hello **Image Relay Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_url.png)

    <span data-ttu-id="803ac-157">a.</span><span class="sxs-lookup"><span data-stu-id="803ac-157">a.</span></span> <span data-ttu-id="803ac-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.imagerelay.com/`</span><span class="sxs-lookup"><span data-stu-id="803ac-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.imagerelay.com/`</span></span>

    <span data-ttu-id="803ac-159">b.</span><span class="sxs-lookup"><span data-stu-id="803ac-159">b.</span></span> <span data-ttu-id="803ac-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.imagerelay.com/sso/metadata`</span><span class="sxs-lookup"><span data-stu-id="803ac-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.imagerelay.com/sso/metadata`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="803ac-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="803ac-161">These values are not real.</span></span> <span data-ttu-id="803ac-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="803ac-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="803ac-163">Kontakta [avbildningen Relay klienten supportteamet](http://support.imagerelay.com/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="803ac-163">Contact [Image Relay Client support team](http://support.imagerelay.com/) tooget these values.</span></span> 
 


4. <span data-ttu-id="803ac-164">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="803ac-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_certificate.png) 

5. <span data-ttu-id="803ac-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="803ac-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="803ac-168">På hello **konfiguration av avbildningen Relay** klickar du på **konfigurera avbildningen Relay** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="803ac-168">On hello **Image Relay Configuration** section, click **Configure Image Relay** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="803ac-169">Kopiera hello **Sign-Out URL: en och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="803ac-169">Copy hello **Sign-Out Service URL and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_configure.png) 

7. <span data-ttu-id="803ac-171">Logga in tooyour avbildningen Relay företagets webbplats som en administratör i ett nytt webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="803ac-171">In another browser window, sign in tooyour Image Relay company site as an administrator.</span></span>

8. <span data-ttu-id="803ac-172">Klicka på hello i hello verktygsfältet överst hello **användare och behörigheter** arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="803ac-172">In hello toolbar on hello top, click hello **Users & Permissions** workload.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_06.png) 

9. <span data-ttu-id="803ac-174">Klicka på **nya behörighet att skapa**.</span><span class="sxs-lookup"><span data-stu-id="803ac-174">Click **Create New Permission**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_08.png)

10. <span data-ttu-id="803ac-176">I hello **enkel inloggning på inställningar** arbetsbelastning, Välj hello **den här gruppen kan bara logga in via enkel inloggning** kryssrutan och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="803ac-176">In hello **Single Sign On Settings** workload, select hello **This Group can only sign-in via Single Sign On** check box, and then click **Save**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_09.png) 

11. <span data-ttu-id="803ac-178">Gå för**kontoinställningar**.</span><span class="sxs-lookup"><span data-stu-id="803ac-178">Go too**Account Settings**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_10.png) 

12. <span data-ttu-id="803ac-180">Gå toohello **enkel inloggning på inställningar** arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="803ac-180">Go toohello **Single Sign On Settings** workload.</span></span>
    
     ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_11.png)

13. <span data-ttu-id="803ac-182">På hello **SAML inställningar** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="803ac-182">On hello **SAML Settings** dialog, perform hello following steps:</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_12.png)
    
    <span data-ttu-id="803ac-184">a.</span><span class="sxs-lookup"><span data-stu-id="803ac-184">a.</span></span> <span data-ttu-id="803ac-185">I **inloggnings-URL** textruta klistra in hello värdet för **inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="803ac-185">In **Login URL** textbox, paste hello value of **Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="803ac-186">b.</span><span class="sxs-lookup"><span data-stu-id="803ac-186">b.</span></span> <span data-ttu-id="803ac-187">I **logga ut URL** textruta klistra in hello värdet för **tjänst-URL för enkel Sign-Out** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="803ac-187">In **Logout URL**  textbox, paste hello value of **Single Sign-Out Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="803ac-188">c.</span><span class="sxs-lookup"><span data-stu-id="803ac-188">c.</span></span> <span data-ttu-id="803ac-189">Som **Format för namn-Id**väljer **urn: oasis: namn: tc: SAML:1.1:nameid-format: e-postadress**.</span><span class="sxs-lookup"><span data-stu-id="803ac-189">As **Name Id Format**, select **urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress**.</span></span>

    <span data-ttu-id="803ac-190">d.</span><span class="sxs-lookup"><span data-stu-id="803ac-190">d.</span></span> <span data-ttu-id="803ac-191">Som **bindning alternativ för begäranden från hello Service Provider (bild relä)**väljer **POST bindning**.</span><span class="sxs-lookup"><span data-stu-id="803ac-191">As **Binding Options for Requests from hello Service Provider (Image Relay)**, select **POST Binding**.</span></span>

    <span data-ttu-id="803ac-192">e.</span><span class="sxs-lookup"><span data-stu-id="803ac-192">e.</span></span> <span data-ttu-id="803ac-193">Under **x.509-certifikat**, klickar du på **uppdatering certifikat**.</span><span class="sxs-lookup"><span data-stu-id="803ac-193">Under **x.509 Certificate**, click **Update Certificate**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_17.png)

    <span data-ttu-id="803ac-195">f.</span><span class="sxs-lookup"><span data-stu-id="803ac-195">f.</span></span> <span data-ttu-id="803ac-196">Öppna hello hämtat certifikat i anteckningar, kopiera hello innehållet och klistra in den i textrutan för hello x.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="803ac-196">Open hello downloaded certificate in notepad, copy hello content, and then paste it into hello x.509 Certificate textbox.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_18.png)

    <span data-ttu-id="803ac-198">g.</span><span class="sxs-lookup"><span data-stu-id="803ac-198">g.</span></span> <span data-ttu-id="803ac-199">I **in Användaretablering** avsnitt, Välj hello **Aktivera in Användaretablering**.</span><span class="sxs-lookup"><span data-stu-id="803ac-199">In **Just-In-Time User Provisioning** section, select hello **Enable Just-In-Time User Provisioning**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_19.png)

    <span data-ttu-id="803ac-201">h.</span><span class="sxs-lookup"><span data-stu-id="803ac-201">h.</span></span> <span data-ttu-id="803ac-202">Välj hello behörighetsgruppen (till exempel **SSO grundläggande**) som tillåts toosign i endast via enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="803ac-202">Select hello permission group (for example, **SSO Basic**) which is allowed toosign in only through single sign-on.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_20.png)

    <span data-ttu-id="803ac-204">Jag.</span><span class="sxs-lookup"><span data-stu-id="803ac-204">i.</span></span> <span data-ttu-id="803ac-205">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="803ac-205">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="803ac-206">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="803ac-206">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="803ac-207">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="803ac-207">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="803ac-208">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="803ac-208">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="803ac-209">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="803ac-209">Creating an Azure AD test user</span></span>
<span data-ttu-id="803ac-210">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="803ac-210">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="803ac-212">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="803ac-212">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="803ac-213">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="803ac-213">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="803ac-215">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="803ac-215">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="803ac-217">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="803ac-217">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="803ac-219">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="803ac-219">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="803ac-221">a.</span><span class="sxs-lookup"><span data-stu-id="803ac-221">a.</span></span> <span data-ttu-id="803ac-222">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="803ac-222">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="803ac-223">b.</span><span class="sxs-lookup"><span data-stu-id="803ac-223">b.</span></span> <span data-ttu-id="803ac-224">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="803ac-224">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="803ac-225">c.</span><span class="sxs-lookup"><span data-stu-id="803ac-225">c.</span></span> <span data-ttu-id="803ac-226">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="803ac-226">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="803ac-227">d.</span><span class="sxs-lookup"><span data-stu-id="803ac-227">d.</span></span> <span data-ttu-id="803ac-228">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="803ac-228">Click **Create**.</span></span>
 
### <a name="creating-an-image-relay-test-user"></a><span data-ttu-id="803ac-229">Skapa en avbildning Relay testanvändare</span><span class="sxs-lookup"><span data-stu-id="803ac-229">Creating an Image Relay test user</span></span>

<span data-ttu-id="803ac-230">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i avbildningen Relay.</span><span class="sxs-lookup"><span data-stu-id="803ac-230">hello objective of this section is toocreate a user called Britta Simon in Image Relay.</span></span>

<span data-ttu-id="803ac-231">**toocreate en användare som kallas Britta Simon i avbildningen Relay utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="803ac-231">**toocreate a user called Britta Simon in Image Relay, perform hello following steps:**</span></span>

1. <span data-ttu-id="803ac-232">Inloggning tooyour avbildningen Relay företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="803ac-232">Sign-on tooyour Image Relay company site as an administrator.</span></span>

2. <span data-ttu-id="803ac-233">Gå för**användare och behörigheter** och välj **skapa SSO användare**.</span><span class="sxs-lookup"><span data-stu-id="803ac-233">Go too**Users & Permissions**     and select **Create SSO User**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_21.png) 

3. <span data-ttu-id="803ac-235">Ange hello **e-post**, **Förnamn**, **efternamn**, och **företagets** hello användare som du vill tooprovision och välj hello behörighetsgruppen (till exempel SSO grundläggande) vilket är hello-grupp som kan logga in endast via enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="803ac-235">Enter hello **Email**, **First Name**, **Last Name**, and **Company** of hello user you want tooprovision and select hello permission group (for example, SSO Basic) which is hello group that can sign in only through single sign-on.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_22.png) 

4. <span data-ttu-id="803ac-237">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="803ac-237">Click **Create**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="803ac-238">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="803ac-238">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="803ac-239">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooImage Relay.</span><span class="sxs-lookup"><span data-stu-id="803ac-239">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooImage Relay.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="803ac-241">**tooassign Britta Simon tooImage Relay, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="803ac-241">**tooassign Britta Simon tooImage Relay, perform hello following steps:**</span></span>

1. <span data-ttu-id="803ac-242">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="803ac-242">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="803ac-244">Välj i listan med program hello **avbildningen Relay**.</span><span class="sxs-lookup"><span data-stu-id="803ac-244">In hello applications list, select **Image Relay**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_app.png) 

3. <span data-ttu-id="803ac-246">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="803ac-246">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="803ac-248">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="803ac-248">Click **Add** button.</span></span> <span data-ttu-id="803ac-249">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="803ac-249">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="803ac-251">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="803ac-251">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="803ac-252">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="803ac-252">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="803ac-253">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="803ac-253">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="803ac-254">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="803ac-254">Testing single sign-on</span></span>

<span data-ttu-id="803ac-255">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="803ac-255">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>    

<span data-ttu-id="803ac-256">Du bör få automatiskt inloggade tooyour avbildningen Relay programmet när du klickar på hello avbildningen Relay-panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="803ac-256">When you click hello Image Relay tile in hello Access Panel, you should get automatically signed-on tooyour Image Relay application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="803ac-257">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="803ac-257">Additional resources</span></span>

* [<span data-ttu-id="803ac-258">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="803ac-258">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="803ac-259">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="803ac-259">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_04.png


[100]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_203.png

