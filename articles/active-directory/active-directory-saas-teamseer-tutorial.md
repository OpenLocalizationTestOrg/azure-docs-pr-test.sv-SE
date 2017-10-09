---
title: "Självstudier: Azure Active Directory-integrering med TeamSeer | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och TeamSeer."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6ec4806f-fe0f-4ed7-8cfa-32d1c840433f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 876d13e446115acd50b01c7f44db99357045e429
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-teamseer"></a><span data-ttu-id="119e9-103">Självstudier: Azure Active Directory-integrering med TeamSeer</span><span class="sxs-lookup"><span data-stu-id="119e9-103">Tutorial: Azure Active Directory integration with TeamSeer</span></span>

<span data-ttu-id="119e9-104">I kursen får du lära dig hur toointegrate TeamSeer med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="119e9-104">In this tutorial, you learn how toointegrate TeamSeer with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="119e9-105">Integrera TeamSeer med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="119e9-105">Integrating TeamSeer with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="119e9-106">Du kan styra i Azure AD som har åtkomst till tooTeamSeer</span><span class="sxs-lookup"><span data-stu-id="119e9-106">You can control in Azure AD who has access tooTeamSeer</span></span>
- <span data-ttu-id="119e9-107">Du kan aktivera din användare tooautomatically get inloggade tooTeamSeer (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="119e9-107">You can enable your users tooautomatically get signed-on tooTeamSeer (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="119e9-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="119e9-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="119e9-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="119e9-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="119e9-110">Krav</span><span class="sxs-lookup"><span data-stu-id="119e9-110">Prerequisites</span></span>

<span data-ttu-id="119e9-111">tooconfigure Azure AD-integrering med TeamSeer, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="119e9-111">tooconfigure Azure AD integration with TeamSeer, you need hello following items:</span></span>

- <span data-ttu-id="119e9-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="119e9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="119e9-113">En TeamSeer enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="119e9-113">A TeamSeer single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="119e9-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="119e9-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="119e9-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="119e9-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="119e9-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="119e9-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="119e9-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="119e9-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="119e9-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="119e9-118">Scenario description</span></span>
<span data-ttu-id="119e9-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="119e9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="119e9-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="119e9-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="119e9-121">Att lägga till TeamSeer från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="119e9-121">Adding TeamSeer from hello gallery</span></span>
2. <span data-ttu-id="119e9-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="119e9-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-teamseer-from-hello-gallery"></a><span data-ttu-id="119e9-123">Att lägga till TeamSeer från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="119e9-123">Adding TeamSeer from hello gallery</span></span>
<span data-ttu-id="119e9-124">tooconfigure hello integrering av TeamSeer i tooAzure AD, behöver du tooadd TeamSeer hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="119e9-124">tooconfigure hello integration of TeamSeer in tooAzure AD, you need tooadd TeamSeer from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="119e9-125">**tooadd TeamSeer från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="119e9-125">**tooadd TeamSeer from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="119e9-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="119e9-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="119e9-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="119e9-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="119e9-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="119e9-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="119e9-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="119e9-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="119e9-133">Skriv i sökrutan hello **TeamSeer**.</span><span class="sxs-lookup"><span data-stu-id="119e9-133">In hello search box, type **TeamSeer**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_search.png)

5. <span data-ttu-id="119e9-135">Markera hello resultat på panelen **TeamSeer**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="119e9-135">In hello results panel, select **TeamSeer**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="119e9-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="119e9-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="119e9-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med TeamSeer baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="119e9-138">In this section, you configure and test Azure AD single sign-on with TeamSeer based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="119e9-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i TeamSeer är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="119e9-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in TeamSeer is tooa user in Azure AD.</span></span> <span data-ttu-id="119e9-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i TeamSeer toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="119e9-140">In other words, a link relationship between an Azure AD user and hello related user in TeamSeer needs toobe established.</span></span>

<span data-ttu-id="119e9-141">I TeamSeer, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="119e9-141">In TeamSeer, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="119e9-142">tooconfigure och testa Azure AD enkel inloggning med TeamSeer, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="119e9-142">tooconfigure and test Azure AD single sign-on with TeamSeer, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="119e9-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="119e9-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="119e9-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="119e9-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="119e9-145">**[Skapa en testanvändare TeamSeer](#creating-a-teamseer-test-user)**  -toohave en motsvarighet för Britta Simon i TeamSeer som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="119e9-145">**[Creating a TeamSeer test user](#creating-a-teamseer-test-user)** - toohave a counterpart of Britta Simon in TeamSeer that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="119e9-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="119e9-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="119e9-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="119e9-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="119e9-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="119e9-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="119e9-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt TeamSeer program.</span><span class="sxs-lookup"><span data-stu-id="119e9-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your TeamSeer application.</span></span>

<span data-ttu-id="119e9-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med TeamSeer:**</span><span class="sxs-lookup"><span data-stu-id="119e9-150">**tooconfigure Azure AD single sign-on with TeamSeer, perform hello following steps:**</span></span>

1. <span data-ttu-id="119e9-151">I hello Azure-portalen på hello **TeamSeer** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="119e9-151">In hello Azure portal, on hello **TeamSeer** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="119e9-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="119e9-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_samlbase.png)

3. <span data-ttu-id="119e9-155">På hello **TeamSeer domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="119e9-155">On hello **TeamSeer Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_url.png)

     <span data-ttu-id="119e9-157">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://www.teamseer.com/<companyid>`</span><span class="sxs-lookup"><span data-stu-id="119e9-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://www.teamseer.com/<companyid>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="119e9-158">hello-värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="119e9-158">hello value is not real.</span></span> <span data-ttu-id="119e9-159">Hello uppdateringsvärde med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="119e9-159">Update hello value with hello actual Sign-On URL.</span></span> <span data-ttu-id="119e9-160">Kontakta [TeamSeer klienten supportteamet](http://pages.theaccessgroup.com/solutions_business-suite_absence-management_contact.html) tooget hello värde.</span><span class="sxs-lookup"><span data-stu-id="119e9-160">Contact [TeamSeer Client support team](http://pages.theaccessgroup.com/solutions_business-suite_absence-management_contact.html) tooget hello value.</span></span> 
 
4. <span data-ttu-id="119e9-161">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="119e9-161">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_certificate.png) 

5. <span data-ttu-id="119e9-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="119e9-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamseer-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="119e9-165">På hello **TeamSeer Configuration** klickar du på **konfigurera TeamSeer** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="119e9-165">On hello **TeamSeer Configuration** section, click **Configure TeamSeer** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="119e9-166">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="119e9-166">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_configure.png)

7. <span data-ttu-id="119e9-168">Logga in tooyour TeamSeer företagets webbplats som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="119e9-168">In a different web browser window, log in tooyour TeamSeer company site as an administrator.</span></span>

8. <span data-ttu-id="119e9-169">Gå för**HR Admin**.</span><span class="sxs-lookup"><span data-stu-id="119e9-169">Go too**HR Admin**.</span></span>
   
    <span data-ttu-id="119e9-170">![HR Admin](./media/active-directory-saas-teamseer-tutorial/ic789634.png "HR Admin")</span><span class="sxs-lookup"><span data-stu-id="119e9-170">![HR Admin](./media/active-directory-saas-teamseer-tutorial/ic789634.png "HR Admin")</span></span>

9. <span data-ttu-id="119e9-171">Klicka på **installationsprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="119e9-171">Click **Setup**.</span></span>
   
    <span data-ttu-id="119e9-172">![Installationsprogrammet](./media/active-directory-saas-teamseer-tutorial/ic789635.png "installationen")</span><span class="sxs-lookup"><span data-stu-id="119e9-172">![Setup](./media/active-directory-saas-teamseer-tutorial/ic789635.png "Setup")</span></span>

10. <span data-ttu-id="119e9-173">Klicka på **ställa in information om SAML provider**.</span><span class="sxs-lookup"><span data-stu-id="119e9-173">Click **Set up SAML provider details**.</span></span>
   
    <span data-ttu-id="119e9-174">![Inställningar för SAML](./media/active-directory-saas-teamseer-tutorial/ic789636.png "SAML-inställningar")</span><span class="sxs-lookup"><span data-stu-id="119e9-174">![SAML Settings](./media/active-directory-saas-teamseer-tutorial/ic789636.png "SAML Settings")</span></span>

11. <span data-ttu-id="119e9-175">I hello informationsavsnittet för SAML-providern, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="119e9-175">In hello SAML provider details section, perform hello following steps:</span></span>
   
    <span data-ttu-id="119e9-176">![Inställningar för SAML](./media/active-directory-saas-teamseer-tutorial/ic789637.png "SAML-inställningar")</span><span class="sxs-lookup"><span data-stu-id="119e9-176">![SAML Settings](./media/active-directory-saas-teamseer-tutorial/ic789637.png "SAML Settings")</span></span>   

    <span data-ttu-id="119e9-177">a.</span><span class="sxs-lookup"><span data-stu-id="119e9-177">a.</span></span> <span data-ttu-id="119e9-178">Klistra in hello **inloggning tjänst-URL för enkel** värdet i toohello **URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="119e9-178">Paste hello **Single Sign-On Service URL** value in toohello **URL** textbox.</span></span>
          
    <span data-ttu-id="119e9-179">b.</span><span class="sxs-lookup"><span data-stu-id="119e9-179">b.</span></span> <span data-ttu-id="119e9-180">Öppna din Base64-kodade certifikatet i anteckningar, kopiera hello innehållet i den i tooyour Urklipp och klistra in den toohello **IdP offentliga certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="119e9-180">Open your base-64 encoded certificate in notepad, copy hello content of it in tooyour clipboard, and then paste it toohello **IdP Public Certificate** textbox.</span></span>

12. <span data-ttu-id="119e9-181">toocomplete hello SAML providerkonfigurationen utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="119e9-181">toocomplete hello SAML provider configuration, perform hello following steps:</span></span>
    
    <span data-ttu-id="119e9-182">![Inställningar för SAML](./media/active-directory-saas-teamseer-tutorial/ic789638.png "SAML-inställningar")</span><span class="sxs-lookup"><span data-stu-id="119e9-182">![SAML Settings](./media/active-directory-saas-teamseer-tutorial/ic789638.png "SAML Settings")</span></span> 

    <span data-ttu-id="119e9-183">a.</span><span class="sxs-lookup"><span data-stu-id="119e9-183">a.</span></span> <span data-ttu-id="119e9-184">I hello **testa e-postadresser**, Skriv hello test användarens e-postadress.</span><span class="sxs-lookup"><span data-stu-id="119e9-184">In hello **Test Email Addresses**, type hello test user’s email address.</span></span> 
  
    <span data-ttu-id="119e9-185">b.</span><span class="sxs-lookup"><span data-stu-id="119e9-185">b.</span></span> <span data-ttu-id="119e9-186">I hello **utfärdaren** textruta typen hello utfärdar-URL för hello-leverantör.</span><span class="sxs-lookup"><span data-stu-id="119e9-186">In hello **Issuer** textbox, type hello Issuer URL of hello service provider.</span></span> 
  
    <span data-ttu-id="119e9-187">c.</span><span class="sxs-lookup"><span data-stu-id="119e9-187">c.</span></span> <span data-ttu-id="119e9-188">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="119e9-188">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="119e9-189">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="119e9-189">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="119e9-190">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="119e9-190">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="119e9-191">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="119e9-191">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="119e9-192">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="119e9-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="119e9-193">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="119e9-193">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="119e9-195">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="119e9-195">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="119e9-196">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="119e9-196">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamseer-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="119e9-198">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="119e9-198">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamseer-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="119e9-200">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="119e9-200">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamseer-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="119e9-202">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="119e9-202">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamseer-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="119e9-204">a.</span><span class="sxs-lookup"><span data-stu-id="119e9-204">a.</span></span> <span data-ttu-id="119e9-205">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="119e9-205">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="119e9-206">b.</span><span class="sxs-lookup"><span data-stu-id="119e9-206">b.</span></span> <span data-ttu-id="119e9-207">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="119e9-207">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="119e9-208">c.</span><span class="sxs-lookup"><span data-stu-id="119e9-208">c.</span></span> <span data-ttu-id="119e9-209">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="119e9-209">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="119e9-210">d.</span><span class="sxs-lookup"><span data-stu-id="119e9-210">d.</span></span> <span data-ttu-id="119e9-211">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="119e9-211">Click **Create**.</span></span>
 
### <a name="creating-a-teamseer-test-user"></a><span data-ttu-id="119e9-212">Skapa en testanvändare TeamSeer</span><span class="sxs-lookup"><span data-stu-id="119e9-212">Creating a TeamSeer test user</span></span>

<span data-ttu-id="119e9-213">tooenable Azure AD-användare toolog i tooTeamSeer, måste de vara etablerade i tooShiftPlanning.</span><span class="sxs-lookup"><span data-stu-id="119e9-213">tooenable Azure AD users toolog in tooTeamSeer, they must be provisioned in tooShiftPlanning.</span></span> <span data-ttu-id="119e9-214">Hello gäller TeamSeer är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="119e9-214">In hello case of TeamSeer, provisioning is a manual task.</span></span>

<span data-ttu-id="119e9-215">**tooprovision ett användarkonto, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="119e9-215">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="119e9-216">Logga in tooyour **TeamSeer** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="119e9-216">Log in tooyour **TeamSeer** company site as an administrator.</span></span>

2. <span data-ttu-id="119e9-217">Utför följande steg hello:</span><span class="sxs-lookup"><span data-stu-id="119e9-217">Perform hello following steps:</span></span>
   
    <span data-ttu-id="119e9-218">![HR Admin](./media/active-directory-saas-teamseer-tutorial/ic789640.png "HR Admin")</span><span class="sxs-lookup"><span data-stu-id="119e9-218">![HR Admin](./media/active-directory-saas-teamseer-tutorial/ic789640.png "HR Admin")</span></span>  
 
    <span data-ttu-id="119e9-219">a.</span><span class="sxs-lookup"><span data-stu-id="119e9-219">a.</span></span> <span data-ttu-id="119e9-220">Gå för**HR Admin \> användare**.</span><span class="sxs-lookup"><span data-stu-id="119e9-220">Go too**HR Admin \> Users**.</span></span>
  
    <span data-ttu-id="119e9-221">b.</span><span class="sxs-lookup"><span data-stu-id="119e9-221">b.</span></span> <span data-ttu-id="119e9-222">Klicka på **kör guiden nya användare och hello**.</span><span class="sxs-lookup"><span data-stu-id="119e9-222">Click **Run hello New User wizard**.</span></span>

3. <span data-ttu-id="119e9-223">I hello **användarinformation** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="119e9-223">In hello **User Details** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="119e9-224">![Användarinformation](./media/active-directory-saas-teamseer-tutorial/ic789641.png "användarinformation")</span><span class="sxs-lookup"><span data-stu-id="119e9-224">![User Details](./media/active-directory-saas-teamseer-tutorial/ic789641.png "User Details")</span></span>

    <span data-ttu-id="119e9-225">a.</span><span class="sxs-lookup"><span data-stu-id="119e9-225">a.</span></span> <span data-ttu-id="119e9-226">Typen hello **Förnamn**, **efternamn**, **användarnamn (e-postadress)** av en giltig AAD-konto som du vill tooprovision i toohello relaterade textrutor.</span><span class="sxs-lookup"><span data-stu-id="119e9-226">Type hello **First Name**, **Surname**, **User name (Email address)** of a valid AAD account you want tooprovision in toohello related textboxes.</span></span>
  
    <span data-ttu-id="119e9-227">b.</span><span class="sxs-lookup"><span data-stu-id="119e9-227">b.</span></span> <span data-ttu-id="119e9-228">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="119e9-228">Click **Next**.</span></span>

4. <span data-ttu-id="119e9-229">Följ hello på skärmen för att lägga till en ny användare och klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="119e9-229">Follow hello on-screen instructions for adding a new user, and click **Finish**.</span></span>

>[!NOTE]
><span data-ttu-id="119e9-230">Du kan använda något annat TeamSeer användarens konto skapas verktyg eller API: er som tillhandahålls av TeamSeer tooprovision användarkonton i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="119e9-230">You can use any other TeamSeer user account creation tools or APIs provided by TeamSeer tooprovision Azure AD user accounts.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="119e9-231">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="119e9-231">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="119e9-232">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooTeamSeer.</span><span class="sxs-lookup"><span data-stu-id="119e9-232">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooTeamSeer.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="119e9-234">**tooassign Britta Simon tooTeamSeer utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="119e9-234">**tooassign Britta Simon tooTeamSeer, perform hello following steps:**</span></span>

1. <span data-ttu-id="119e9-235">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="119e9-235">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="119e9-237">Välj i listan med program hello **TeamSeer**.</span><span class="sxs-lookup"><span data-stu-id="119e9-237">In hello applications list, select **TeamSeer**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_app.png) 

3. <span data-ttu-id="119e9-239">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="119e9-239">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="119e9-241">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="119e9-241">Click **Add** button.</span></span> <span data-ttu-id="119e9-242">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="119e9-242">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="119e9-244">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="119e9-244">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="119e9-245">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="119e9-245">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="119e9-246">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="119e9-246">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="119e9-247">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="119e9-247">Testing single sign-on</span></span>

<span data-ttu-id="119e9-248">Om du vill tootest dina inställningar för enkel inloggning, öppna hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="119e9-248">If you want tootest your single sign-on settings, open hello Access Panel.</span></span> <span data-ttu-id="119e9-249">Mer information om hello åtkomstpanelen finns [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="119e9-249">For more details about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="119e9-250">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="119e9-250">Additional resources</span></span>

* [<span data-ttu-id="119e9-251">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="119e9-251">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="119e9-252">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="119e9-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_203.png

