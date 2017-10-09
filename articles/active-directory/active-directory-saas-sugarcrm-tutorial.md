---
title: "Självstudier: Azure Active Directory-integrering med socker CRM | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och socker CRM."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3331b9fc-ebc0-4a3a-9f7b-bf20ee35d180
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: jeedes
ms.openlocfilehash: 108d2f8125e410743ee7bc48883a1d0b00602615
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sugar-crm"></a><span data-ttu-id="c593b-103">Självstudier: Azure Active Directory-integrering med socker CRM</span><span class="sxs-lookup"><span data-stu-id="c593b-103">Tutorial: Azure Active Directory integration with Sugar CRM</span></span>

<span data-ttu-id="c593b-104">I kursen får du lära dig hur toointegrate socker CRM med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="c593b-104">In this tutorial, you learn how toointegrate Sugar CRM with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c593b-105">Integrera socker CRM med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="c593b-105">Integrating Sugar CRM with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="c593b-106">Du kan styra i Azure AD som har åtkomst tooSugar CRM</span><span class="sxs-lookup"><span data-stu-id="c593b-106">You can control in Azure AD who has access tooSugar CRM</span></span>
- <span data-ttu-id="c593b-107">Du kan aktivera din användare tooautomatically get inloggade tooSugar CRM (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="c593b-107">You can enable your users tooautomatically get signed-on tooSugar CRM (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c593b-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="c593b-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="c593b-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c593b-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c593b-110">Krav</span><span class="sxs-lookup"><span data-stu-id="c593b-110">Prerequisites</span></span>

<span data-ttu-id="c593b-111">tooconfigure Azure AD-integrering med socker CRM, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="c593b-111">tooconfigure Azure AD integration with Sugar CRM, you need hello following items:</span></span>

- <span data-ttu-id="c593b-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="c593b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c593b-113">En socker CRM enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="c593b-113">A Sugar CRM single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c593b-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="c593b-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c593b-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="c593b-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c593b-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="c593b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c593b-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c593b-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c593b-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="c593b-118">Scenario description</span></span>
<span data-ttu-id="c593b-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="c593b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c593b-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="c593b-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c593b-121">Att lägga till socker CRM från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="c593b-121">Adding Sugar CRM from hello gallery</span></span>
2. <span data-ttu-id="c593b-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c593b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sugar-crm-from-hello-gallery"></a><span data-ttu-id="c593b-123">Att lägga till socker CRM från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="c593b-123">Adding Sugar CRM from hello gallery</span></span>
<span data-ttu-id="c593b-124">tooconfigure hello integrering av socker CRM i Azure AD, behöver du tooadd socker CRM hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="c593b-124">tooconfigure hello integration of Sugar CRM into Azure AD, you need tooadd Sugar CRM from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="c593b-125">**tooadd socker CRM från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c593b-125">**tooadd Sugar CRM from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="c593b-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="c593b-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c593b-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="c593b-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="c593b-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="c593b-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="c593b-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c593b-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="c593b-133">Skriv i sökrutan hello **socker CRM**.</span><span class="sxs-lookup"><span data-stu-id="c593b-133">In hello search box, type **Sugar CRM**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_search.png)

5. <span data-ttu-id="c593b-135">Markera hello resultat på panelen **socker CRM**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="c593b-135">In hello results panel, select **Sugar CRM**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c593b-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c593b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c593b-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med socker CRM baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="c593b-138">In this section, you configure and test Azure AD single sign-on with Sugar CRM based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c593b-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i socker CRM är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c593b-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Sugar CRM is tooa user in Azure AD.</span></span> <span data-ttu-id="c593b-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i socker CRM toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="c593b-140">In other words, a link relationship between an Azure AD user and hello related user in Sugar CRM needs toobe established.</span></span>

<span data-ttu-id="c593b-141">Tilldela hello värdet hello i socker CRM **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="c593b-141">In Sugar CRM, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="c593b-142">tooconfigure och testa Azure AD enkel inloggning med socker CRM, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="c593b-142">tooconfigure and test Azure AD single sign-on with Sugar CRM, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="c593b-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="c593b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="c593b-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c593b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c593b-145">**[Skapa en testanvändare socker CRM](#creating-a-sugar-crm-test-user)**  -toohave en motsvarighet för Britta Simon i socker CRM som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="c593b-145">**[Creating a Sugar CRM test user](#creating-a-sugar-crm-test-user)** - toohave a counterpart of Britta Simon in Sugar CRM that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="c593b-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c593b-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c593b-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="c593b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c593b-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c593b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c593b-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt socker CRM-program.</span><span class="sxs-lookup"><span data-stu-id="c593b-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Sugar CRM application.</span></span>

<span data-ttu-id="c593b-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med socker CRM:**</span><span class="sxs-lookup"><span data-stu-id="c593b-150">**tooconfigure Azure AD single sign-on with Sugar CRM, perform hello following steps:**</span></span>

1. <span data-ttu-id="c593b-151">I hello Azure-portalen på hello **socker CRM** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="c593b-151">In hello Azure portal, on hello **Sugar CRM** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="c593b-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c593b-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_samlbase.png)

3. <span data-ttu-id="c593b-155">På hello **socker CRM-domänen och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="c593b-155">On hello **Sugar CRM Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_url.png)

    <span data-ttu-id="c593b-157">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:</span><span class="sxs-lookup"><span data-stu-id="c593b-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.sugarondemand.com` |
    | `https://<companyname>.trial.sugarcrm` |

    > [!NOTE] 
    > <span data-ttu-id="c593b-158">hello-värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="c593b-158">hello value is not real.</span></span> <span data-ttu-id="c593b-159">Hello uppdateringsvärde med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="c593b-159">Update hello value with hello actual Sign-On URL.</span></span> <span data-ttu-id="c593b-160">Kontakta [socker CRM-klienten supportteamet](https://support.sugarcrm.com/) tooget hello värde.</span><span class="sxs-lookup"><span data-stu-id="c593b-160">Contact [Sugar CRM Client support team](https://support.sugarcrm.com/) tooget hello value.</span></span> 
 
4. <span data-ttu-id="c593b-161">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="c593b-161">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_certificate.png) 

5. <span data-ttu-id="c593b-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="c593b-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="c593b-165">På hello **socker CRM Configuration** klickar du på **konfigurera socker CRM** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="c593b-165">On hello **Sugar CRM Configuration** section, click **Configure Sugar CRM** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="c593b-166">Kopiera hello **Sign-Out URL och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="c593b-166">Copy hello **Sign-Out URL, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_configure.png) 

7. <span data-ttu-id="c593b-168">Logga in tooyour socker CRM företagets webbplats som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="c593b-168">In a different web browser window, log in tooyour Sugar CRM company site as an administrator.</span></span>

8. <span data-ttu-id="c593b-169">Gå för**Admin**.</span><span class="sxs-lookup"><span data-stu-id="c593b-169">Go too**Admin**.</span></span>
   
    <span data-ttu-id="c593b-170">![Admin](./media/active-directory-saas-sugarcrm-tutorial/ic795888.png "Admin")</span><span class="sxs-lookup"><span data-stu-id="c593b-170">![Admin](./media/active-directory-saas-sugarcrm-tutorial/ic795888.png "Admin")</span></span>

9. <span data-ttu-id="c593b-171">I hello **Administration** klickar du på **lösenordshantering**.</span><span class="sxs-lookup"><span data-stu-id="c593b-171">In hello **Administration** section, click **Password Management**.</span></span>
   
    <span data-ttu-id="c593b-172">![Administration](./media/active-directory-saas-sugarcrm-tutorial/ic795889.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="c593b-172">![Administration](./media/active-directory-saas-sugarcrm-tutorial/ic795889.png "Administration")</span></span>

10. <span data-ttu-id="c593b-173">Välj **aktivera SAML-autentisering**.</span><span class="sxs-lookup"><span data-stu-id="c593b-173">Select **Enable SAML Authentication**.</span></span>
   
    <span data-ttu-id="c593b-174">![Administration](./media/active-directory-saas-sugarcrm-tutorial/ic795890.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="c593b-174">![Administration](./media/active-directory-saas-sugarcrm-tutorial/ic795890.png "Administration")</span></span>

11. <span data-ttu-id="c593b-175">I hello **SAML-autentisering** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="c593b-175">In hello **SAML Authentication** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="c593b-176">![SAML-autentisering](./media/active-directory-saas-sugarcrm-tutorial/ic795891.png "SAML-autentisering")</span><span class="sxs-lookup"><span data-stu-id="c593b-176">![SAML Authentication](./media/active-directory-saas-sugarcrm-tutorial/ic795891.png "SAML Authentication")</span></span>  
 
    <span data-ttu-id="c593b-177">a.</span><span class="sxs-lookup"><span data-stu-id="c593b-177">a.</span></span> <span data-ttu-id="c593b-178">I hello **inloggnings-URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c593b-178">In hello **Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
  
    <span data-ttu-id="c593b-179">b.</span><span class="sxs-lookup"><span data-stu-id="c593b-179">b.</span></span> <span data-ttu-id="c593b-180">I hello **Servicenivåmål URL** textruta klistra in hello värdet för **Sign-Out URL**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c593b-180">In hello **SLO URL** textbox, paste hello value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>
  
    <span data-ttu-id="c593b-181">c.</span><span class="sxs-lookup"><span data-stu-id="c593b-181">c.</span></span> <span data-ttu-id="c593b-182">Öppna din Base64-kodade certifikatet i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in hello hela certifikatet till **X.509-certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="c593b-182">Open your base-64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste hello entire Certificate into **X.509 Certificate** textbox.</span></span>
  
    <span data-ttu-id="c593b-183">d.</span><span class="sxs-lookup"><span data-stu-id="c593b-183">d.</span></span> <span data-ttu-id="c593b-184">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="c593b-184">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="c593b-185">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="c593b-185">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="c593b-186">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="c593b-186">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="c593b-187">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c593b-187">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c593b-188">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="c593b-188">Creating an Azure AD test user</span></span>
<span data-ttu-id="c593b-189">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c593b-189">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="c593b-191">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c593b-191">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="c593b-192">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="c593b-192">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sugarcrm-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c593b-194">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="c593b-194">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sugarcrm-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c593b-196">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c593b-196">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sugarcrm-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c593b-198">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="c593b-198">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sugarcrm-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c593b-200">a.</span><span class="sxs-lookup"><span data-stu-id="c593b-200">a.</span></span> <span data-ttu-id="c593b-201">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c593b-201">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c593b-202">b.</span><span class="sxs-lookup"><span data-stu-id="c593b-202">b.</span></span> <span data-ttu-id="c593b-203">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c593b-203">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c593b-204">c.</span><span class="sxs-lookup"><span data-stu-id="c593b-204">c.</span></span> <span data-ttu-id="c593b-205">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="c593b-205">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="c593b-206">d.</span><span class="sxs-lookup"><span data-stu-id="c593b-206">d.</span></span> <span data-ttu-id="c593b-207">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="c593b-207">Click **Create**.</span></span>
 
### <a name="creating-a-sugar-crm-test-user"></a><span data-ttu-id="c593b-208">Skapa en testanvändare socker CRM</span><span class="sxs-lookup"><span data-stu-id="c593b-208">Creating a Sugar CRM test user</span></span>

<span data-ttu-id="c593b-209">I ordning tooenable Azure AD-användare toolog i tooSugar CRM, måste de vara etablerade tooSugar CRM.</span><span class="sxs-lookup"><span data-stu-id="c593b-209">In order tooenable Azure AD users toolog in tooSugar CRM, they must be provisioned tooSugar CRM.</span></span>

<span data-ttu-id="c593b-210">Hello gäller socker CRM är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="c593b-210">In hello case of Sugar CRM, provisioning is a manual task.</span></span>

<span data-ttu-id="c593b-211">**tooprovision ett användarkonto, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="c593b-211">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="c593b-212">Logga in tooyour **socker CRM** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="c593b-212">Log in tooyour **Sugar CRM** company site as administrator.</span></span>

2. <span data-ttu-id="c593b-213">Gå för**Admin**.</span><span class="sxs-lookup"><span data-stu-id="c593b-213">Go too**Admin**.</span></span>
   
    <span data-ttu-id="c593b-214">![Admin](./media/active-directory-saas-sugarcrm-tutorial/ic795888.png "Admin")</span><span class="sxs-lookup"><span data-stu-id="c593b-214">![Admin](./media/active-directory-saas-sugarcrm-tutorial/ic795888.png "Admin")</span></span>

3. <span data-ttu-id="c593b-215">I hello **Administration** klickar du på **Användarhantering**.</span><span class="sxs-lookup"><span data-stu-id="c593b-215">In hello **Administration** section, click **User Management**.</span></span>
   
    <span data-ttu-id="c593b-216">![Administration](./media/active-directory-saas-sugarcrm-tutorial/ic795893.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="c593b-216">![Administration](./media/active-directory-saas-sugarcrm-tutorial/ic795893.png "Administration")</span></span>

4. <span data-ttu-id="c593b-217">Gå för**användare \> skapa nya användare**.</span><span class="sxs-lookup"><span data-stu-id="c593b-217">Go too**Users \> Create New User**.</span></span>
   
    <span data-ttu-id="c593b-218">![Skapa ny användare](./media/active-directory-saas-sugarcrm-tutorial/ic795894.png "Skapa ny användare")</span><span class="sxs-lookup"><span data-stu-id="c593b-218">![Create New User](./media/active-directory-saas-sugarcrm-tutorial/ic795894.png "Create New User")</span></span>

5. <span data-ttu-id="c593b-219">På hello **användarprofil** fliken, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="c593b-219">On hello **User Profile** tab, perform hello following steps:</span></span>
   
    <span data-ttu-id="c593b-220">![Ny användare](./media/active-directory-saas-sugarcrm-tutorial/ic795895.png "ny användare")</span><span class="sxs-lookup"><span data-stu-id="c593b-220">![New User](./media/active-directory-saas-sugarcrm-tutorial/ic795895.png "New User")</span></span>

    <span data-ttu-id="c593b-221">a.</span><span class="sxs-lookup"><span data-stu-id="c593b-221">a.</span></span> <span data-ttu-id="c593b-222">Typen hello **användarnamn**, **efternamn**, och **e-postadress** av en giltig Azure Active Directory-användare i hello relaterade textrutor.</span><span class="sxs-lookup"><span data-stu-id="c593b-222">Type hello **user name**, **last name**, and **email address** of a valid Azure Active Directory user into hello related textboxes.</span></span>
  
6. <span data-ttu-id="c593b-223">Som **Status**väljer **Active**.</span><span class="sxs-lookup"><span data-stu-id="c593b-223">As **Status**, select **Active**.</span></span>

7. <span data-ttu-id="c593b-224">Utför följande steg hello hello lösenord på fliken:</span><span class="sxs-lookup"><span data-stu-id="c593b-224">On hello Password tab, perform hello following steps:</span></span>
   
    <span data-ttu-id="c593b-225">![Ny användare](./media/active-directory-saas-sugarcrm-tutorial/ic795896.png "ny användare")</span><span class="sxs-lookup"><span data-stu-id="c593b-225">![New User](./media/active-directory-saas-sugarcrm-tutorial/ic795896.png "New User")</span></span>

    <span data-ttu-id="c593b-226">a.</span><span class="sxs-lookup"><span data-stu-id="c593b-226">a.</span></span> <span data-ttu-id="c593b-227">Ange hello lösenordet till hello relaterade textruta.</span><span class="sxs-lookup"><span data-stu-id="c593b-227">Type hello password into hello related textbox.</span></span>

    <span data-ttu-id="c593b-228">b.</span><span class="sxs-lookup"><span data-stu-id="c593b-228">b.</span></span> <span data-ttu-id="c593b-229">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="c593b-229">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="c593b-230">Du kan använda något annat socker CRM användarens konto skapas verktyg eller API: er som tillhandahålls av socker CRM tooprovision AAD användarkonton.</span><span class="sxs-lookup"><span data-stu-id="c593b-230">You can use any other Sugar CRM user account creation tools or APIs provided by Sugar CRM tooprovision AAD user accounts.</span></span> 
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="c593b-231">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="c593b-231">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="c593b-232">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSugar CRM.</span><span class="sxs-lookup"><span data-stu-id="c593b-232">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSugar CRM.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="c593b-234">**tooassign Britta Simon tooSugar CRM, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="c593b-234">**tooassign Britta Simon tooSugar CRM, perform hello following steps:**</span></span>

1. <span data-ttu-id="c593b-235">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="c593b-235">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="c593b-237">Välj i listan med program hello **socker CRM**.</span><span class="sxs-lookup"><span data-stu-id="c593b-237">In hello applications list, select **Sugar CRM**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_app.png) 

3. <span data-ttu-id="c593b-239">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="c593b-239">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="c593b-241">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="c593b-241">Click **Add** button.</span></span> <span data-ttu-id="c593b-242">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c593b-242">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="c593b-244">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="c593b-244">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="c593b-245">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c593b-245">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c593b-246">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c593b-246">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c593b-247">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c593b-247">Testing single sign-on</span></span>

<span data-ttu-id="c593b-248">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="c593b-248">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="c593b-249">När du klickar på hello socker CRM-panelen i hello åtkomstpanelen får automatiskt inloggade tooyour socker CRM-programmet.</span><span class="sxs-lookup"><span data-stu-id="c593b-249">When you click hello Sugar CRM tile in hello Access Panel, you should get automatically signed-on tooyour Sugar CRM application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c593b-250">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="c593b-250">Additional resources</span></span>

* [<span data-ttu-id="c593b-251">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c593b-251">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c593b-252">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c593b-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_203.png

