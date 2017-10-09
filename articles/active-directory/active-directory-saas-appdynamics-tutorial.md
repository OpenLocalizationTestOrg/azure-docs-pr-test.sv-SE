---
title: "Självstudier: Azure Active Directory-integrering med AppDynamics | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och AppDynamics."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 25fd1df0-411c-4f55-8be3-4273b543100f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 9b63afec73d7442e6ac1ce34b511beea6f43ffe4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-appdynamics"></a><span data-ttu-id="fbd07-103">Självstudier: Azure Active Directory-integrering med AppDynamics</span><span class="sxs-lookup"><span data-stu-id="fbd07-103">Tutorial: Azure Active Directory integration with AppDynamics</span></span>

<span data-ttu-id="fbd07-104">I kursen får du lära dig hur toointegrate AppDynamics med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="fbd07-104">In this tutorial, you learn how toointegrate AppDynamics with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="fbd07-105">Integrera AppDynamics med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="fbd07-105">Integrating AppDynamics with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="fbd07-106">Du kan styra i Azure AD som har åtkomst till tooAppDynamics</span><span class="sxs-lookup"><span data-stu-id="fbd07-106">You can control in Azure AD who has access tooAppDynamics</span></span>
- <span data-ttu-id="fbd07-107">Du kan aktivera din användare tooautomatically get inloggade tooAppDynamics (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="fbd07-107">You can enable your users tooautomatically get signed-on tooAppDynamics (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="fbd07-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="fbd07-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="fbd07-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="fbd07-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fbd07-110">Krav</span><span class="sxs-lookup"><span data-stu-id="fbd07-110">Prerequisites</span></span>

<span data-ttu-id="fbd07-111">tooconfigure Azure AD-integrering med AppDynamics, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="fbd07-111">tooconfigure Azure AD integration with AppDynamics, you need hello following items:</span></span>

- <span data-ttu-id="fbd07-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="fbd07-112">An Azure AD subscription</span></span>
- <span data-ttu-id="fbd07-113">En AppDynamics enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="fbd07-113">An AppDynamics single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="fbd07-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="fbd07-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="fbd07-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="fbd07-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="fbd07-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="fbd07-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="fbd07-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="fbd07-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="fbd07-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="fbd07-118">Scenario description</span></span>
<span data-ttu-id="fbd07-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="fbd07-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="fbd07-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="fbd07-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="fbd07-121">Att lägga till AppDynamics från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="fbd07-121">Adding AppDynamics from hello gallery</span></span>
2. <span data-ttu-id="fbd07-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="fbd07-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-appdynamics-from-hello-gallery"></a><span data-ttu-id="fbd07-123">Att lägga till AppDynamics från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="fbd07-123">Adding AppDynamics from hello gallery</span></span>
<span data-ttu-id="fbd07-124">tooconfigure hello integrering av AppDynamics i Azure AD, behöver du tooadd AppDynamics hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="fbd07-124">tooconfigure hello integration of AppDynamics into Azure AD, you need tooadd AppDynamics from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="fbd07-125">**tooadd AppDynamics från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="fbd07-125">**tooadd AppDynamics from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="fbd07-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="fbd07-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="fbd07-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="fbd07-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="fbd07-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="fbd07-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="fbd07-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fbd07-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="fbd07-133">Skriv i sökrutan hello **AppDynamics**.</span><span class="sxs-lookup"><span data-stu-id="fbd07-133">In hello search box, type **AppDynamics**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-appdynamics-tutorial/tutorial_appdynamics_search.png)

5. <span data-ttu-id="fbd07-135">Markera hello resultat på panelen **AppDynamics**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="fbd07-135">In hello results panel, select **AppDynamics**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-appdynamics-tutorial/tutorial_appdynamics_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="fbd07-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="fbd07-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="fbd07-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med AppDynamics baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="fbd07-138">In this section, you configure and test Azure AD single sign-on with AppDynamics based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="fbd07-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i AppDynamics är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fbd07-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in AppDynamics is tooa user in Azure AD.</span></span> <span data-ttu-id="fbd07-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i AppDynamics toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="fbd07-140">In other words, a link relationship between an Azure AD user and hello related user in AppDynamics needs toobe established.</span></span>

<span data-ttu-id="fbd07-141">I AppDynamics, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="fbd07-141">In AppDynamics, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="fbd07-142">tooconfigure och testa Azure AD enkel inloggning med AppDynamics, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="fbd07-142">tooconfigure and test Azure AD single sign-on with AppDynamics, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="fbd07-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="fbd07-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="fbd07-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="fbd07-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="fbd07-145">**[Skapa en testanvändare AppDynamics](#creating-an-appdynamics-test-user)**  -toohave en motsvarighet för Britta Simon i AppDynamics som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="fbd07-145">**[Creating an AppDynamics test user](#creating-an-appdynamics-test-user)** - toohave a counterpart of Britta Simon in AppDynamics that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="fbd07-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="fbd07-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="fbd07-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="fbd07-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="fbd07-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="fbd07-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="fbd07-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt AppDynamics program.</span><span class="sxs-lookup"><span data-stu-id="fbd07-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your AppDynamics application.</span></span>

<span data-ttu-id="fbd07-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med AppDynamics:**</span><span class="sxs-lookup"><span data-stu-id="fbd07-150">**tooconfigure Azure AD single sign-on with AppDynamics, perform hello following steps:**</span></span>

1. <span data-ttu-id="fbd07-151">I hello Azure-portalen på hello **AppDynamics** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="fbd07-151">In hello Azure portal, on hello **AppDynamics** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="fbd07-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="fbd07-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-appdynamics-tutorial/tutorial_appdynamics_samlbase.png)

3. <span data-ttu-id="fbd07-155">På hello **AppDynamics domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="fbd07-155">On hello **AppDynamics Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-appdynamics-tutorial/tutorial_appdynamics_url.png)

    <span data-ttu-id="fbd07-157">a.</span><span class="sxs-lookup"><span data-stu-id="fbd07-157">a.</span></span> <span data-ttu-id="fbd07-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.saas.appdynamics.com`</span><span class="sxs-lookup"><span data-stu-id="fbd07-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.saas.appdynamics.com`</span></span>

    <span data-ttu-id="fbd07-159">b.</span><span class="sxs-lookup"><span data-stu-id="fbd07-159">b.</span></span> <span data-ttu-id="fbd07-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.saas.appdynamics.com/controller`</span><span class="sxs-lookup"><span data-stu-id="fbd07-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.saas.appdynamics.com/controller`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="fbd07-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="fbd07-161">These values are not real.</span></span> <span data-ttu-id="fbd07-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="fbd07-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="fbd07-163">Kontakta [AppDynamics klienten supportteamet](https://www.appdynamics.com/support/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="fbd07-163">Contact [AppDynamics Client support team](https://www.appdynamics.com/support/) tooget these values.</span></span> 
 
4. <span data-ttu-id="fbd07-164">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="fbd07-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-appdynamics-tutorial/tutorial_appdynamics_certificate.png) 

5. <span data-ttu-id="fbd07-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="fbd07-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-appdynamics-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="fbd07-168">På hello **AppDynamics Configuration** klickar du på **konfigurera AppDynamics** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="fbd07-168">On hello **AppDynamics Configuration** section, click **Configure AppDynamics** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="fbd07-169">Kopiera hello **Sign-Out URL och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="fbd07-169">Copy hello **Sign-Out URL, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-appdynamics-tutorial/tutorial_appdynamics_configure.png) 

7. <span data-ttu-id="fbd07-171">Logga in tooyour AppDynamics företagets webbplats som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="fbd07-171">In a different web browser window, log in tooyour AppDynamics company site as an administrator.</span></span>

8. <span data-ttu-id="fbd07-172">Klicka i hello verktygsfältet hello längst upp **inställningar**, och klicka sedan på **Administration**.</span><span class="sxs-lookup"><span data-stu-id="fbd07-172">In hello toolbar on hello top, click **Settings**, and then click **Administration**.</span></span>
   
    <span data-ttu-id="fbd07-173">![Administration](./media/active-directory-saas-appdynamics-tutorial/ic790216.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="fbd07-173">![Administration](./media/active-directory-saas-appdynamics-tutorial/ic790216.png "Administration")</span></span>

9. <span data-ttu-id="fbd07-174">Klicka på hello **autentiseringsprovider** fliken.</span><span class="sxs-lookup"><span data-stu-id="fbd07-174">Click hello **Authentication Provider** tab.</span></span>
   
    <span data-ttu-id="fbd07-175">![Autentiseringsprovider](./media/active-directory-saas-appdynamics-tutorial/ic790224.png "autentiseringsprovider")</span><span class="sxs-lookup"><span data-stu-id="fbd07-175">![Authentication Provider](./media/active-directory-saas-appdynamics-tutorial/ic790224.png "Authentication Provider")</span></span>

10. <span data-ttu-id="fbd07-176">I hello **autentiseringsprovider** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="fbd07-176">In hello **Authentication Provider** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="fbd07-177">![SAML-konfiguration](./media/active-directory-saas-appdynamics-tutorial/ic790225.png "SAML-konfiguration")</span><span class="sxs-lookup"><span data-stu-id="fbd07-177">![SAML Configuration](./media/active-directory-saas-appdynamics-tutorial/ic790225.png "SAML Configuration")</span></span>   

    <span data-ttu-id="fbd07-178">a.</span><span class="sxs-lookup"><span data-stu-id="fbd07-178">a.</span></span> <span data-ttu-id="fbd07-179">Som **autentiseringsprovider**väljer **SAML**.</span><span class="sxs-lookup"><span data-stu-id="fbd07-179">As **Authentication Provider**, select **SAML**.</span></span>

    <span data-ttu-id="fbd07-180">b.</span><span class="sxs-lookup"><span data-stu-id="fbd07-180">b.</span></span> <span data-ttu-id="fbd07-181">I hello **inloggnings-URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="fbd07-181">In hello **Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="fbd07-182">c.</span><span class="sxs-lookup"><span data-stu-id="fbd07-182">c.</span></span> <span data-ttu-id="fbd07-183">I hello **logga ut URL** textruta klistra in hello värdet för **Sign-Out URL** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="fbd07-183">In hello **Logout URL** textbox, paste hello value of **Sign-Out URL** which you have copied from Azure portal.</span></span>
       
    <span data-ttu-id="fbd07-184">d.</span><span class="sxs-lookup"><span data-stu-id="fbd07-184">d.</span></span> <span data-ttu-id="fbd07-185">Öppna din Base64-kodade certifikatet i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den toohello **certifikat** textruta</span><span class="sxs-lookup"><span data-stu-id="fbd07-185">Open your base-64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it toohello **Certificate** textbox</span></span>

    <span data-ttu-id="fbd07-186">e.</span><span class="sxs-lookup"><span data-stu-id="fbd07-186">e.</span></span> <span data-ttu-id="fbd07-187">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="fbd07-187">Click **Save**.</span></span>

     <span data-ttu-id="fbd07-188">![Spara](./media/active-directory-saas-appdynamics-tutorial/ic777673.png "spara")</span><span class="sxs-lookup"><span data-stu-id="fbd07-188">![Save](./media/active-directory-saas-appdynamics-tutorial/ic777673.png "Save")</span></span>

> [!TIP]
> <span data-ttu-id="fbd07-189">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="fbd07-189">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="fbd07-190">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="fbd07-190">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="fbd07-191">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="fbd07-191">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="fbd07-192">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="fbd07-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="fbd07-193">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="fbd07-193">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="fbd07-195">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="fbd07-195">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="fbd07-196">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="fbd07-196">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-appdynamics-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="fbd07-198">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="fbd07-198">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-appdynamics-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="fbd07-200">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fbd07-200">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-appdynamics-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="fbd07-202">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="fbd07-202">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-appdynamics-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="fbd07-204">a.</span><span class="sxs-lookup"><span data-stu-id="fbd07-204">a.</span></span> <span data-ttu-id="fbd07-205">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="fbd07-205">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="fbd07-206">b.</span><span class="sxs-lookup"><span data-stu-id="fbd07-206">b.</span></span> <span data-ttu-id="fbd07-207">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="fbd07-207">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="fbd07-208">c.</span><span class="sxs-lookup"><span data-stu-id="fbd07-208">c.</span></span> <span data-ttu-id="fbd07-209">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="fbd07-209">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="fbd07-210">d.</span><span class="sxs-lookup"><span data-stu-id="fbd07-210">d.</span></span> <span data-ttu-id="fbd07-211">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="fbd07-211">Click **Create**.</span></span>
 
### <a name="creating-an-appdynamics-test-user"></a><span data-ttu-id="fbd07-212">Skapa en testanvändare AppDynamics</span><span class="sxs-lookup"><span data-stu-id="fbd07-212">Creating an AppDynamics test user</span></span>

<span data-ttu-id="fbd07-213">tooenable Azure AD-användare toolog i tooAppDynamics, måste de etableras i AppDynamics.</span><span class="sxs-lookup"><span data-stu-id="fbd07-213">tooenable Azure AD users toolog in tooAppDynamics, they must be provisioned into AppDynamics.</span></span> <span data-ttu-id="fbd07-214">Hello gäller AppDynamics är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="fbd07-214">In hello case of AppDynamics, provisioning is a manual task.</span></span>

<span data-ttu-id="fbd07-215">**tooconfigure användaretablering, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="fbd07-215">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="fbd07-216">Logga in tooyour AppDynamics företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="fbd07-216">Log in tooyour AppDynamics company site as an administrator.</span></span>

2. <span data-ttu-id="fbd07-217">Gå för**användare**, och klicka sedan på  **+**  tooopen hello **skapa användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fbd07-217">Go too**Users**, and then click **+** tooopen hello **Create User** dialog.</span></span>
   
    <span data-ttu-id="fbd07-218">![Användare](./media/active-directory-saas-appdynamics-tutorial/ic790229.png "användare")</span><span class="sxs-lookup"><span data-stu-id="fbd07-218">![Users](./media/active-directory-saas-appdynamics-tutorial/ic790229.png "Users")</span></span>

3. <span data-ttu-id="fbd07-219">I hello **skapa användare** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="fbd07-219">In hello **Create User** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="fbd07-220">![Skapa användare](./media/active-directory-saas-appdynamics-tutorial/ic790230.png "skapa användare")</span><span class="sxs-lookup"><span data-stu-id="fbd07-220">![Create User](./media/active-directory-saas-appdynamics-tutorial/ic790230.png "Create User")</span></span>
   
    <span data-ttu-id="fbd07-221">a.</span><span class="sxs-lookup"><span data-stu-id="fbd07-221">a.</span></span> <span data-ttu-id="fbd07-222">Typen hello **användarnamn**, **namn**, **e-post**, **nytt lösenord**, **Upprepa nytt lösenord** av en giltig AAD kontot som du vill använda tooprovision hello relaterade textrutor.</span><span class="sxs-lookup"><span data-stu-id="fbd07-222">Type hello **Username**, **Name**, **Email**, **New Password**, **Repeat New Password** of a valid AAD account you want tooprovision into hello related textboxes.</span></span>

    <span data-ttu-id="fbd07-223">b.</span><span class="sxs-lookup"><span data-stu-id="fbd07-223">b.</span></span> <span data-ttu-id="fbd07-224">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="fbd07-224">Click **Save**.</span></span>

    >[!NOTE]
    ><span data-ttu-id="fbd07-225">Du kan använda något annat AppDynamics användarens konto skapas verktyg eller API: er som tillhandahålls av AppDynamics tooprovision användarkonton i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fbd07-225">You can use any other AppDynamics user account creation tools or APIs provided by AppDynamics tooprovision Azure AD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="fbd07-226">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="fbd07-226">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="fbd07-227">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooAppDynamics.</span><span class="sxs-lookup"><span data-stu-id="fbd07-227">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAppDynamics.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="fbd07-229">**tooassign Britta Simon tooAppDynamics utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="fbd07-229">**tooassign Britta Simon tooAppDynamics, perform hello following steps:**</span></span>

1. <span data-ttu-id="fbd07-230">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="fbd07-230">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="fbd07-232">Välj i listan med program hello **AppDynamics**.</span><span class="sxs-lookup"><span data-stu-id="fbd07-232">In hello applications list, select **AppDynamics**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-appdynamics-tutorial/tutorial_appdynamics_app.png) 

3. <span data-ttu-id="fbd07-234">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="fbd07-234">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="fbd07-236">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="fbd07-236">Click **Add** button.</span></span> <span data-ttu-id="fbd07-237">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fbd07-237">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="fbd07-239">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="fbd07-239">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="fbd07-240">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fbd07-240">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="fbd07-241">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fbd07-241">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="fbd07-242">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="fbd07-242">Testing single sign-on</span></span>

<span data-ttu-id="fbd07-243">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="fbd07-243">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="fbd07-244">Du bör få automatiskt inloggade tooyour AppDynamics programmet när du klickar på hello AppDynamics panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="fbd07-244">When you click hello AppDynamics tile in hello Access Panel, you should get automatically signed-on tooyour AppDynamics application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fbd07-245">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="fbd07-245">Additional resources</span></span>

* [<span data-ttu-id="fbd07-246">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="fbd07-246">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="fbd07-247">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="fbd07-247">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-appdynamics-tutorial/tutorial_general_203.png

