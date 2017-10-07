---
title: "Självstudier: Azure Active Directory-integrering med ITRP | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och ITRP."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e09716a3-4200-4853-9414-2390e6c10d98
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: 35463a55fcfc1e55c90700737961c1ff2e58992a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-itrp"></a><span data-ttu-id="df481-103">Självstudier: Azure Active Directory-integrering med ITRP</span><span class="sxs-lookup"><span data-stu-id="df481-103">Tutorial: Azure Active Directory integration with ITRP</span></span>

<span data-ttu-id="df481-104">I kursen får du lära dig hur toointegrate ITRP med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="df481-104">In this tutorial, you learn how toointegrate ITRP with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="df481-105">Integrera ITRP med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="df481-105">Integrating ITRP with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="df481-106">Du kan styra i Azure AD som har åtkomst till tooITRP</span><span class="sxs-lookup"><span data-stu-id="df481-106">You can control in Azure AD who has access tooITRP</span></span>
- <span data-ttu-id="df481-107">Du kan aktivera din användare tooautomatically get inloggade tooITRP (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="df481-107">You can enable your users tooautomatically get signed-on tooITRP (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="df481-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="df481-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="df481-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="df481-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="df481-110">Krav</span><span class="sxs-lookup"><span data-stu-id="df481-110">Prerequisites</span></span>

<span data-ttu-id="df481-111">tooconfigure Azure AD-integrering med ITRP, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="df481-111">tooconfigure Azure AD integration with ITRP, you need hello following items:</span></span>

- <span data-ttu-id="df481-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="df481-112">An Azure AD subscription</span></span>
- <span data-ttu-id="df481-113">En ITRP enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="df481-113">An ITRP single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="df481-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="df481-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="df481-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="df481-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="df481-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="df481-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="df481-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="df481-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="df481-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="df481-118">Scenario description</span></span>
<span data-ttu-id="df481-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="df481-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="df481-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="df481-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="df481-121">Att lägga till ITRP från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="df481-121">Adding ITRP from hello gallery</span></span>
2. <span data-ttu-id="df481-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="df481-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-itrp-from-hello-gallery"></a><span data-ttu-id="df481-123">Att lägga till ITRP från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="df481-123">Adding ITRP from hello gallery</span></span>
<span data-ttu-id="df481-124">tooconfigure hello integrering av ITRP i tooAzure AD, behöver du tooadd ITRP hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="df481-124">tooconfigure hello integration of ITRP in tooAzure AD, you need tooadd ITRP from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="df481-125">**tooadd ITRP från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="df481-125">**tooadd ITRP from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="df481-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="df481-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="df481-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="df481-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="df481-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="df481-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="df481-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="df481-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="df481-133">Skriv i sökrutan hello **ITRP**.</span><span class="sxs-lookup"><span data-stu-id="df481-133">In hello search box, type **ITRP**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-itrp-tutorial/tutorial_itrp_search.png)

5. <span data-ttu-id="df481-135">Markera hello resultat på panelen **ITRP**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="df481-135">In hello results panel, select **ITRP**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-itrp-tutorial/tutorial_itrp_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="df481-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="df481-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="df481-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med ITRP baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="df481-138">In this section, you configure and test Azure AD single sign-on with ITRP based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="df481-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i ITRP är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="df481-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in ITRP is tooa user in Azure AD.</span></span> <span data-ttu-id="df481-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i ITRP toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="df481-140">In other words, a link relationship between an Azure AD user and hello related user in ITRP needs toobe established.</span></span>

<span data-ttu-id="df481-141">I ITRP, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="df481-141">In ITRP, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="df481-142">tooconfigure och testa Azure AD enkel inloggning med ITRP, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="df481-142">tooconfigure and test Azure AD single sign-on with ITRP, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="df481-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="df481-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="df481-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="df481-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="df481-145">**[Skapa en ITRP testanvändare](#creating-an-itrp-test-user)**  -toohave en motsvarighet för Britta Simon i ITRP som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="df481-145">**[Creating an ITRP test user](#creating-an-itrp-test-user)** - toohave a counterpart of Britta Simon in ITRP that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="df481-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="df481-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="df481-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="df481-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="df481-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="df481-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="df481-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt ITRP program.</span><span class="sxs-lookup"><span data-stu-id="df481-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your ITRP application.</span></span>

<span data-ttu-id="df481-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med ITRP:**</span><span class="sxs-lookup"><span data-stu-id="df481-150">**tooconfigure Azure AD single sign-on with ITRP, perform hello following steps:**</span></span>

1. <span data-ttu-id="df481-151">I hello Azure-portalen på hello **ITRP** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="df481-151">In hello Azure portal, on hello **ITRP** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="df481-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="df481-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-itrp-tutorial/tutorial_itrp_samlbase.png)

3. <span data-ttu-id="df481-155">På hello **ITRP domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="df481-155">On hello **ITRP Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-itrp-tutorial/tutorial_itrp_url.png)

    <span data-ttu-id="df481-157">a.</span><span class="sxs-lookup"><span data-stu-id="df481-157">a.</span></span> <span data-ttu-id="df481-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<tenant-name>.itrp.com`</span><span class="sxs-lookup"><span data-stu-id="df481-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenant-name>.itrp.com`</span></span>

    <span data-ttu-id="df481-159">b.</span><span class="sxs-lookup"><span data-stu-id="df481-159">b.</span></span> <span data-ttu-id="df481-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<tenant-name>.itrp.com`</span><span class="sxs-lookup"><span data-stu-id="df481-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<tenant-name>.itrp.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="df481-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="df481-161">These values are not real.</span></span> <span data-ttu-id="df481-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="df481-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="df481-163">Kontakta [ITRP klienten supportteamet](https://www.itrp.com/support) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="df481-163">Contact [ITRP Client support team](https://www.itrp.com/support) tooget these values.</span></span> 
 
4. <span data-ttu-id="df481-164">På hello **SAML-signeringscertifikat** avsnitt, kopiera hello **TUMAVTRYCKET** värdet för certifikatet.</span><span class="sxs-lookup"><span data-stu-id="df481-164">On hello **SAML Signing Certificate** section, copy hello **THUMBPRINT** value of certificate.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-itrp-tutorial/tutorial_itrp_certificate.png) 

5. <span data-ttu-id="df481-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="df481-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-itrp-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="df481-168">På hello **ITRP Configuration** klickar du på **konfigurera ITRP** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="df481-168">On hello **ITRP Configuration** section, click **Configure ITRP** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="df481-169">Kopiera hello **SAML enkel inloggning Service Webbadressen och Sign-Out** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="df481-169">Copy hello **SAML Single Sign-On Service URL and Sign-Out URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-itrp-tutorial/tutorial_itrp_configure.png) 

7. <span data-ttu-id="df481-171">Logga in tooyour ITRP företagets webbplats som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="df481-171">In a different web browser window, log in tooyour ITRP company site as an administrator.</span></span>

8. <span data-ttu-id="df481-172">Klicka i hello verktygsfältet hello längst upp **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="df481-172">In hello toolbar on hello top, click **Settings**.</span></span>
   
    <span data-ttu-id="df481-173">![ITRP](./media/active-directory-saas-itrp-tutorial/ic775570.png "ITRP")</span><span class="sxs-lookup"><span data-stu-id="df481-173">![ITRP](./media/active-directory-saas-itrp-tutorial/ic775570.png "ITRP")</span></span>

8. <span data-ttu-id="df481-174">Hello vänstra navigeringsfönstret och välj **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="df481-174">In hello left navigation pane, select **Single Sign-On**.</span></span>
   
    <span data-ttu-id="df481-175">![Enkel inloggning](./media/active-directory-saas-itrp-tutorial/ic775571.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="df481-175">![Single Sign-On](./media/active-directory-saas-itrp-tutorial/ic775571.png "Single Sign-On")</span></span>

9. <span data-ttu-id="df481-176">I hello enkel inloggning konfigurationsavsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="df481-176">In hello Single Sign-On configuration section, perform hello following steps:</span></span>
   
    <span data-ttu-id="df481-177">![Enkel inloggning](./media/active-directory-saas-itrp-tutorial/ic775572.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="df481-177">![Single Sign-On](./media/active-directory-saas-itrp-tutorial/ic775572.png "Single Sign-On")</span></span>
    
    <span data-ttu-id="df481-178">![Enkel inloggning](./media/active-directory-saas-itrp-tutorial/ic775573.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="df481-178">![Single Sign-On](./media/active-directory-saas-itrp-tutorial/ic775573.png "Single Sign-On")</span></span>   

    <span data-ttu-id="df481-179">a.</span><span class="sxs-lookup"><span data-stu-id="df481-179">a.</span></span> <span data-ttu-id="df481-180">Klicka på **Aktivera**.</span><span class="sxs-lookup"><span data-stu-id="df481-180">Click **Enable**.</span></span>

    <span data-ttu-id="df481-181">b.</span><span class="sxs-lookup"><span data-stu-id="df481-181">b.</span></span> <span data-ttu-id="df481-182">I **Remote logga ut URL** textruta klistra in hello värdet för **Sign-Out URL**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="df481-182">In **Remote Log Out URL** textbox, paste hello value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="df481-183">c.</span><span class="sxs-lookup"><span data-stu-id="df481-183">c.</span></span> <span data-ttu-id="df481-184">I **SAML SSO URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="df481-184">In **SAML SSO URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="df481-185">d.In **certifikat fingeravtryck** textruta klistra in hello **tumavtrycket** värdet för certifikat som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="df481-185">d.In **Certificate Fingerprint** textbox, paste hello **Thumbprint** value of certificate, which you have copied from Azure portal.</span></span> 
      
10. <span data-ttu-id="df481-186">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="df481-186">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="df481-187">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="df481-187">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="df481-188">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="df481-188">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="df481-189">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="df481-189">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="df481-190">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="df481-190">Creating an Azure AD test user</span></span>
<span data-ttu-id="df481-191">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="df481-191">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="df481-193">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="df481-193">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="df481-194">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="df481-194">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-itrp-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="df481-196">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="df481-196">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-itrp-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="df481-198">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="df481-198">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-itrp-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="df481-200">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="df481-200">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-itrp-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="df481-202">a.</span><span class="sxs-lookup"><span data-stu-id="df481-202">a.</span></span> <span data-ttu-id="df481-203">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="df481-203">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="df481-204">b.</span><span class="sxs-lookup"><span data-stu-id="df481-204">b.</span></span> <span data-ttu-id="df481-205">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="df481-205">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="df481-206">c.</span><span class="sxs-lookup"><span data-stu-id="df481-206">c.</span></span> <span data-ttu-id="df481-207">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="df481-207">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="df481-208">d.</span><span class="sxs-lookup"><span data-stu-id="df481-208">d.</span></span> <span data-ttu-id="df481-209">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="df481-209">Click **Create**.</span></span>
 
### <a name="creating-an-itrp-test-user"></a><span data-ttu-id="df481-210">Skapa en testanvändare ITRP</span><span class="sxs-lookup"><span data-stu-id="df481-210">Creating an ITRP test user</span></span>

<span data-ttu-id="df481-211">tooenable Azure AD-användare toolog i tooITRP, måste de vara etablerade i tooITRP.</span><span class="sxs-lookup"><span data-stu-id="df481-211">tooenable Azure AD users toolog in tooITRP, they must be provisioned in tooITRP.</span></span>  

<span data-ttu-id="df481-212">Hello gäller ITRP är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="df481-212">In hello case of ITRP, provisioning is a manual task.</span></span>

<span data-ttu-id="df481-213">**tooprovision ett användarkonto, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="df481-213">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="df481-214">Logga in tooyour **ITRP** klient.</span><span class="sxs-lookup"><span data-stu-id="df481-214">Log in tooyour **ITRP** tenant.</span></span>

2. <span data-ttu-id="df481-215">Klicka i hello verktygsfältet hello längst upp **poster**.</span><span class="sxs-lookup"><span data-stu-id="df481-215">In hello toolbar on hello top, click **Records**.</span></span>
   
    <span data-ttu-id="df481-216">![Admin](./media/active-directory-saas-itrp-tutorial/ic775575.png "Admin")</span><span class="sxs-lookup"><span data-stu-id="df481-216">![Admin](./media/active-directory-saas-itrp-tutorial/ic775575.png "Admin")</span></span>

3. <span data-ttu-id="df481-217">Hello popup-menyn, Välj **personer**.</span><span class="sxs-lookup"><span data-stu-id="df481-217">From hello popup menu, select **People**.</span></span>
   
    <span data-ttu-id="df481-218">![Personer](./media/active-directory-saas-itrp-tutorial/ic775587.png "personer")</span><span class="sxs-lookup"><span data-stu-id="df481-218">![People](./media/active-directory-saas-itrp-tutorial/ic775587.png "People")</span></span>

4. <span data-ttu-id="df481-219">Klicka på **Lägg till ny Person** (”+”).</span><span class="sxs-lookup"><span data-stu-id="df481-219">Click **Add New Person** (“+”).</span></span>
   
    <span data-ttu-id="df481-220">![Admin](./media/active-directory-saas-itrp-tutorial/ic775576.png "Admin")</span><span class="sxs-lookup"><span data-stu-id="df481-220">![Admin](./media/active-directory-saas-itrp-tutorial/ic775576.png "Admin")</span></span>

5. <span data-ttu-id="df481-221">Dialogrutan Lägg till ny Person hello utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="df481-221">On hello Add New Person dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="df481-222">![Användaren](./media/active-directory-saas-itrp-tutorial/ic775577.png "användare")</span><span class="sxs-lookup"><span data-stu-id="df481-222">![User](./media/active-directory-saas-itrp-tutorial/ic775577.png "User")</span></span> 
      
    <span data-ttu-id="df481-223">a.</span><span class="sxs-lookup"><span data-stu-id="df481-223">a.</span></span> <span data-ttu-id="df481-224">Typen hello **namn**, **e-post** på en giltig AAD-konto som du vill tooprovision.</span><span class="sxs-lookup"><span data-stu-id="df481-224">Type hello **Name**, **Email** of a valid AAD account you want tooprovision.</span></span>

    <span data-ttu-id="df481-225">b.</span><span class="sxs-lookup"><span data-stu-id="df481-225">b.</span></span> <span data-ttu-id="df481-226">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="df481-226">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="df481-227">Du kan använda något annat ITRP användarens konto skapas verktyg eller API: er som tillhandahålls av ITRP tooprovision AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="df481-227">You can use any other ITRP user account creation tools or APIs provided by ITRP tooprovision AAD user accounts.</span></span> 
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="df481-228">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="df481-228">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="df481-229">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooITRP.</span><span class="sxs-lookup"><span data-stu-id="df481-229">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooITRP.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="df481-231">**tooassign Britta Simon tooITRP utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="df481-231">**tooassign Britta Simon tooITRP, perform hello following steps:**</span></span>

1. <span data-ttu-id="df481-232">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="df481-232">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="df481-234">Välj i listan med program hello **ITRP**.</span><span class="sxs-lookup"><span data-stu-id="df481-234">In hello applications list, select **ITRP**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-itrp-tutorial/tutorial_itrp_app.png) 

3. <span data-ttu-id="df481-236">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="df481-236">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="df481-238">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="df481-238">Click **Add** button.</span></span> <span data-ttu-id="df481-239">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="df481-239">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="df481-241">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="df481-241">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="df481-242">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="df481-242">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="df481-243">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="df481-243">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="df481-244">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="df481-244">Testing single sign-on</span></span>

<span data-ttu-id="df481-245">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="df481-245">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="df481-246">Du bör få automatiskt inloggade tooyour ITRP programmet när du klickar på hello ITRP panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="df481-246">When you click hello ITRP tile in hello Access Panel, you should get automatically signed-on tooyour ITRP application.</span></span>
<span data-ttu-id="df481-247">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="df481-247">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="df481-248">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="df481-248">Additional resources</span></span>

* [<span data-ttu-id="df481-249">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="df481-249">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="df481-250">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="df481-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_203.png

