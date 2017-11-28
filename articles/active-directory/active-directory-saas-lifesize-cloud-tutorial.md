---
title: "Självstudier: Azure Active Directory-integrering med molntjänster Lifesize | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Lifesize moln."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 75fab335-fdcd-4066-b42c-cc738fcb6513
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: ae599907e872571b3220de7122006c7db8db4a2b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lifesize-cloud"></a><span data-ttu-id="155b9-103">Självstudier: Azure Active Directory-integrering med Lifesize moln</span><span class="sxs-lookup"><span data-stu-id="155b9-103">Tutorial: Azure Active Directory integration with Lifesize Cloud</span></span>

<span data-ttu-id="155b9-104">I kursen får du lära dig hur toointegrate Lifesize moln med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="155b9-104">In this tutorial, you learn how toointegrate Lifesize Cloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="155b9-105">Integrera Lifesize moln med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="155b9-105">Integrating Lifesize Cloud with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="155b9-106">Du kan styra i Azure AD som har åtkomst tooLifesize moln</span><span class="sxs-lookup"><span data-stu-id="155b9-106">You can control in Azure AD who has access tooLifesize Cloud</span></span>
- <span data-ttu-id="155b9-107">Du kan aktivera din användare tooautomatically get inloggade tooLifesize molnet (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="155b9-107">You can enable your users tooautomatically get signed-on tooLifesize Cloud (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="155b9-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="155b9-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="155b9-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="155b9-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="155b9-110">Krav</span><span class="sxs-lookup"><span data-stu-id="155b9-110">Prerequisites</span></span>

<span data-ttu-id="155b9-111">tooconfigure Azure AD-integrering med Lifesize molnet, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="155b9-111">tooconfigure Azure AD integration with Lifesize Cloud, you need hello following items:</span></span>

- <span data-ttu-id="155b9-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="155b9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="155b9-113">En Lifesize enkel inloggning aktiverad molnprenumeration</span><span class="sxs-lookup"><span data-stu-id="155b9-113">A Lifesize Cloud single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="155b9-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="155b9-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="155b9-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="155b9-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="155b9-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="155b9-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="155b9-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="155b9-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="155b9-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="155b9-118">Scenario description</span></span>
<span data-ttu-id="155b9-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="155b9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="155b9-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="155b9-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="155b9-121">Att lägga till Lifesize moln från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="155b9-121">Adding Lifesize Cloud from hello gallery</span></span>
2. <span data-ttu-id="155b9-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="155b9-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-lifesize-cloud-from-hello-gallery"></a><span data-ttu-id="155b9-123">Att lägga till Lifesize moln från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="155b9-123">Adding Lifesize Cloud from hello gallery</span></span>
<span data-ttu-id="155b9-124">tooconfigure hello integrering av Lifesize moln i Azure AD, behöver du tooadd Lifesize moln hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="155b9-124">tooconfigure hello integration of Lifesize Cloud into Azure AD, you need tooadd Lifesize Cloud from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="155b9-125">**tooadd Lifesize moln från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="155b9-125">**tooadd Lifesize Cloud from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="155b9-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="155b9-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="155b9-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="155b9-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="155b9-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="155b9-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="155b9-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="155b9-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="155b9-133">Skriv i sökrutan hello **Lifesize moln**.</span><span class="sxs-lookup"><span data-stu-id="155b9-133">In hello search box, type **Lifesize Cloud**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_search.png)

5. <span data-ttu-id="155b9-135">Markera hello resultat på panelen **Lifesize moln**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="155b9-135">In hello results panel, select **Lifesize Cloud**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="155b9-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="155b9-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="155b9-138">Du konfigurera och testa Azure AD enkel inloggning med Lifesize molnet baserat på en testanvändare som kallas ”Britta Simon” i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="155b9-138">In this section, you configure and test Azure AD single sign-on with Lifesize Cloud based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="155b9-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Lifesize molnet är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="155b9-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Lifesize Cloud is tooa user in Azure AD.</span></span> <span data-ttu-id="155b9-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i Lifesize molnet toobe upprätta.</span><span class="sxs-lookup"><span data-stu-id="155b9-140">In other words, a link relationship between an Azure AD user and hello related user in Lifesize Cloud needs toobe established.</span></span>

<span data-ttu-id="155b9-141">Tilldela hello värdet hello i Lifesize molnet **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="155b9-141">In Lifesize Cloud, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="155b9-142">tooconfigure och testa Azure AD enkel inloggning med Lifesize molnet, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="155b9-142">tooconfigure and test Azure AD single sign-on with Lifesize Cloud, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="155b9-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="155b9-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="155b9-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="155b9-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="155b9-145">**[Skapa en testanvändare Lifesize moln](#creating-a-lifesize-cloud-test-user)**  -toohave en motsvarighet för Britta Simon i Lifesize moln som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="155b9-145">**[Creating a Lifesize Cloud test user](#creating-a-lifesize-cloud-test-user)** - toohave a counterpart of Britta Simon in Lifesize Cloud that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="155b9-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="155b9-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="155b9-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="155b9-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="155b9-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="155b9-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="155b9-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Lifesize moln.</span><span class="sxs-lookup"><span data-stu-id="155b9-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Lifesize Cloud application.</span></span>

<span data-ttu-id="155b9-150">**Utför följande hello tooconfigure Azure AD enkel inloggning med Lifesize molnet:**</span><span class="sxs-lookup"><span data-stu-id="155b9-150">**tooconfigure Azure AD single sign-on with Lifesize Cloud, perform hello following steps:**</span></span>

1. <span data-ttu-id="155b9-151">I hello Azure-portalen på hello **Lifesize moln** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="155b9-151">In hello Azure portal, on hello **Lifesize Cloud** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="155b9-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="155b9-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_samlbase.png)

3. <span data-ttu-id="155b9-155">På hello **Lifesize moln domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="155b9-155">On hello **Lifesize Cloud Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_url.png)

    <span data-ttu-id="155b9-157">a.</span><span class="sxs-lookup"><span data-stu-id="155b9-157">a.</span></span> <span data-ttu-id="155b9-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://login.lifesizecloud.com/ls/?acs`</span><span class="sxs-lookup"><span data-stu-id="155b9-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://login.lifesizecloud.com/ls/?acs`</span></span>

    <span data-ttu-id="155b9-159">b.</span><span class="sxs-lookup"><span data-stu-id="155b9-159">b.</span></span> <span data-ttu-id="155b9-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://login.lifesizecloud.com/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="155b9-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://login.lifesizecloud.com/<companyname>`</span></span>

     
4. <span data-ttu-id="155b9-161">Kontrollera **visa avancerade inställningar för URL: en**, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="155b9-161">Check **Show advanced URL settings**, perform hello following step:</span></span>  
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_url1.png)

    <span data-ttu-id="155b9-163">I hello **Relay tillstånd** textruta, ange ett URL-Adressen med hello följer mönstret:`https://webapp.lifesizecloud.com/?ent=<identifier>`</span><span class="sxs-lookup"><span data-stu-id="155b9-163">In hello **Relay state** textbox, type a URL using hello following pattern: `https://webapp.lifesizecloud.com/?ent=<identifier>`</span></span>
   
   > [!NOTE] 
   ><span data-ttu-id="155b9-164">Observera att detta inte är hello verkliga värden.</span><span class="sxs-lookup"><span data-stu-id="155b9-164">Please note that these are not hello real values.</span></span> <span data-ttu-id="155b9-165">du har tooupdate dessa värden med hello faktiska inloggnings-URL, Relay tillstånd och identifierare.</span><span class="sxs-lookup"><span data-stu-id="155b9-165">you have tooupdate these values with hello actual Sign-On URL, Relay State, and Identifier.</span></span> <span data-ttu-id="155b9-166">Kontakta [Lifesize Cloud klienten supportteamet](https://www.lifesize.com/support) tooget inloggnings-URL, och identifierar-värden och du kan hämta Relay tillstånd värde från SSO-konfigurationen som beskrivs senare i självstudiekursen hello.</span><span class="sxs-lookup"><span data-stu-id="155b9-166">Contact [Lifesize Cloud Client support team](https://www.lifesize.com/support) tooget Sign-On URL, and Identifier values and you can get Relay State  value from SSO Configuration that is explained later in hello tutorial.</span></span>

4. <span data-ttu-id="155b9-167">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="155b9-167">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_certificate.png) 

5. <span data-ttu-id="155b9-169">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="155b9-169">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="155b9-171">På hello **Lifesize Molnkonfigurationen** klickar du på **konfigurera Lifesize moln** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="155b9-171">On hello **Lifesize Cloud Configuration** section, click **Configure Lifesize Cloud** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="155b9-172">Kopiera hello **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="155b9-172">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_configure.png) 

7. <span data-ttu-id="155b9-174">tooget SSO konfigurerats för ditt program, logga in på hello Lifesize molnapp med administratörsrättigheter.</span><span class="sxs-lookup"><span data-stu-id="155b9-174">tooget SSO configured for your application, login into hello Lifesize Cloud application with Admin privileges.</span></span>

8. <span data-ttu-id="155b9-175">Klicka på ditt namn i hello övre högra hörnet och klicka sedan på hello **avancerade inställningar**.</span><span class="sxs-lookup"><span data-stu-id="155b9-175">In hello top right corner click on your name and then click on hello **Advance Settings**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_06.png)

9. <span data-ttu-id="155b9-177">I avancerade inställningar nu klickar du på hello hello **SSO Configuration** länk.</span><span class="sxs-lookup"><span data-stu-id="155b9-177">In hello Advance Settings now click on hello **SSO Configuration** link.</span></span> <span data-ttu-id="155b9-178">Hello SSO konfigurationssidan för din instans öppnas.</span><span class="sxs-lookup"><span data-stu-id="155b9-178">It will open hello SSO Configuration page for your instance.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_07.png)

10. <span data-ttu-id="155b9-180">Konfigurera följande värden i hello SSO Konfigurationsgränssnittet hello.</span><span class="sxs-lookup"><span data-stu-id="155b9-180">Now configure hello following values in hello SSO configuration UI.</span></span>    
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_08.png)
    
    <span data-ttu-id="155b9-182">a.</span><span class="sxs-lookup"><span data-stu-id="155b9-182">a.</span></span> <span data-ttu-id="155b9-183">I **identitet providern utfärdaren** textruta klistra in hello värdet för **SAML enhets-ID** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="155b9-183">In **Identity Provider Issuer** textbox, paste hello value of **SAML Entity ID** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="155b9-184">b.</span><span class="sxs-lookup"><span data-stu-id="155b9-184">b.</span></span>  <span data-ttu-id="155b9-185">I **inloggnings-URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="155b9-185">In **Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="155b9-186">c.</span><span class="sxs-lookup"><span data-stu-id="155b9-186">c.</span></span> <span data-ttu-id="155b9-187">Öppna din Base64-kodade certifikatet i anteckningar som hämtas från Azure-portalen kopiera hello innehållet i den i Urklipp, och klistra in den toohello **X.509-certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="155b9-187">Open your base-64 encoded certificate in notepad downloaded from Azure portal, copy hello content of it into your clipboard, and then paste it toohello **X.509 Certificate** textbox.</span></span>
  
    <span data-ttu-id="155b9-188">d.</span><span class="sxs-lookup"><span data-stu-id="155b9-188">d.</span></span> <span data-ttu-id="155b9-189">I hello SAML-attributet mappningar för hello förnamn textrutan anger hello-värde som **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**</span><span class="sxs-lookup"><span data-stu-id="155b9-189">In hello SAML Attribute mappings for hello First Name text box enter hello value as **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**</span></span>
    
    <span data-ttu-id="155b9-190">e.</span><span class="sxs-lookup"><span data-stu-id="155b9-190">e.</span></span> <span data-ttu-id="155b9-191">I hello SAML attributmappning för hello **efternamn** textruta ange hello-värde som **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**</span><span class="sxs-lookup"><span data-stu-id="155b9-191">In hello SAML Attribute mapping for hello **Last Name** text box enter hello value as **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**</span></span>
    
    <span data-ttu-id="155b9-192">f.</span><span class="sxs-lookup"><span data-stu-id="155b9-192">f.</span></span> <span data-ttu-id="155b9-193">I hello SAML attributmappning för hello **e-post** textruta ange hello-värde som **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**</span><span class="sxs-lookup"><span data-stu-id="155b9-193">In hello SAML Attribute mapping for hello **Email** text box enter hello value as **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**</span></span>

11. <span data-ttu-id="155b9-194">toocheck hello konfiguration kan du klicka på hello **Test** knappen.</span><span class="sxs-lookup"><span data-stu-id="155b9-194">toocheck hello configuration you can click on hello **Test** button.</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="155b9-195">För lyckad testning behöver toocomplete hello konfigurationsguiden i Azure AD och även ange åtkomst toousers eller grupper som kan utföra hello test.</span><span class="sxs-lookup"><span data-stu-id="155b9-195">For successful testing you need toocomplete hello configuration wizard in Azure AD and also provide access toousers or groups who can perform hello test.</span></span>

12. <span data-ttu-id="155b9-196">Aktivera hello SSO genom att kontrollera på hello **aktivera enkel inloggning** knappen.</span><span class="sxs-lookup"><span data-stu-id="155b9-196">Enable hello SSO by checking on hello **Enable SSO** button.</span></span>

13. <span data-ttu-id="155b9-197">Klicka på hello **uppdatering** knappen så att alla hello inställningarna sparas.</span><span class="sxs-lookup"><span data-stu-id="155b9-197">Now click on hello **Update** button so that all hello settings are saved.</span></span> <span data-ttu-id="155b9-198">Detta genererar hello RelayState värde.</span><span class="sxs-lookup"><span data-stu-id="155b9-198">This will generate hello RelayState value.</span></span> <span data-ttu-id="155b9-199">Kopiera hello RelayState-värde som har genererats i textrutan för hello, klistra in den i hello **Relay tillstånd** textruta under **Lifesize moln domän och URL: er** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="155b9-199">Copy hello RelayState value, which is generated in hello text box, paste it in hello **Relay State** textbox under **Lifesize Cloud Domain and URLs** section.</span></span> 

> [!TIP]
> <span data-ttu-id="155b9-200">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="155b9-200">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="155b9-201">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="155b9-201">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="155b9-202">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="155b9-202">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="155b9-203">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="155b9-203">Creating an Azure AD test user</span></span>

<span data-ttu-id="155b9-204">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="155b9-204">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="155b9-206">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="155b9-206">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="155b9-207">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="155b9-207">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="155b9-209">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="155b9-209">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="155b9-211">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="155b9-211">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="155b9-213">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="155b9-213">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="155b9-215">a.</span><span class="sxs-lookup"><span data-stu-id="155b9-215">a.</span></span> <span data-ttu-id="155b9-216">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="155b9-216">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="155b9-217">b.</span><span class="sxs-lookup"><span data-stu-id="155b9-217">b.</span></span> <span data-ttu-id="155b9-218">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="155b9-218">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="155b9-219">c.</span><span class="sxs-lookup"><span data-stu-id="155b9-219">c.</span></span> <span data-ttu-id="155b9-220">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="155b9-220">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="155b9-221">d.</span><span class="sxs-lookup"><span data-stu-id="155b9-221">d.</span></span> <span data-ttu-id="155b9-222">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="155b9-222">Click **Create**.</span></span>
 
### <a name="creating-a-lifesize-cloud-test-user"></a><span data-ttu-id="155b9-223">Skapa en testanvändare Lifesize moln</span><span class="sxs-lookup"><span data-stu-id="155b9-223">Creating a Lifesize Cloud test user</span></span>

<span data-ttu-id="155b9-224">I det här avsnittet kan du skapa en användare som kallas Britta Simon i Lifesize molnet.</span><span class="sxs-lookup"><span data-stu-id="155b9-224">In this section, you create a user called Britta Simon in Lifesize Cloud.</span></span> <span data-ttu-id="155b9-225">Lifesize molnet stöder automatisk användaretablering.</span><span class="sxs-lookup"><span data-stu-id="155b9-225">Lifesize cloud does support automatic user provisioning.</span></span> <span data-ttu-id="155b9-226">Efter en lyckad autentisering vid Azure AD hello användaren automatiskt att etableras i hello program.</span><span class="sxs-lookup"><span data-stu-id="155b9-226">After successful authentication at Azure AD, hello user will be automatically provisioned in hello application.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="155b9-227">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="155b9-227">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="155b9-228">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooLifesize moln.</span><span class="sxs-lookup"><span data-stu-id="155b9-228">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLifesize Cloud.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="155b9-230">**tooassign Britta Simon tooLifesize moln, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="155b9-230">**tooassign Britta Simon tooLifesize Cloud, perform hello following steps:**</span></span>

1. <span data-ttu-id="155b9-231">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="155b9-231">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="155b9-233">Välj i listan med program hello **Lifesize moln**.</span><span class="sxs-lookup"><span data-stu-id="155b9-233">In hello applications list, select **Lifesize Cloud**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_app.png) 

3. <span data-ttu-id="155b9-235">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="155b9-235">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="155b9-237">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="155b9-237">Click **Add** button.</span></span> <span data-ttu-id="155b9-238">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="155b9-238">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="155b9-240">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="155b9-240">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="155b9-241">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="155b9-241">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="155b9-242">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="155b9-242">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="155b9-243">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="155b9-243">Testing single sign-on</span></span>

<span data-ttu-id="155b9-244">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="155b9-244">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="155b9-245">När du klickar på hello Lifesize moln panelen i hello åtkomstpanelen bör du hämta inloggningssidan för Lifesize molnapp.</span><span class="sxs-lookup"><span data-stu-id="155b9-245">When you click hello Lifesize Cloud tile in hello Access Panel, you should get login page of Lifesize Cloud application.</span></span>
<span data-ttu-id="155b9-246">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="155b9-246">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="155b9-247">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="155b9-247">Additional resources</span></span>

* [<span data-ttu-id="155b9-248">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="155b9-248">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="155b9-249">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="155b9-249">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_203.png

