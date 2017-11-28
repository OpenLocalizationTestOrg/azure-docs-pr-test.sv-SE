---
title: "Självstudier: Azure Active Directory-integrering med SkyDesk e-post | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och SkyDesk e-post."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a9d0bbcb-ddb5-473f-a4aa-028ae88ced1a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.openlocfilehash: 19c670a60f581a2be55b74eacdb5393a36e38be3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-skydesk-email"></a><span data-ttu-id="db26f-103">Självstudier: Azure Active Directory-integrering med SkyDesk e-post</span><span class="sxs-lookup"><span data-stu-id="db26f-103">Tutorial: Azure Active Directory integration with SkyDesk Email</span></span>

<span data-ttu-id="db26f-104">I kursen får du lära dig hur toointegrate SkyDesk e-post med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="db26f-104">In this tutorial, you learn how toointegrate SkyDesk Email with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="db26f-105">Integrera SkyDesk e-post med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="db26f-105">Integrating SkyDesk Email with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="db26f-106">Du kan styra i Azure AD som har åtkomst tooSkyDesk e-post</span><span class="sxs-lookup"><span data-stu-id="db26f-106">You can control in Azure AD who has access tooSkyDesk Email</span></span>
- <span data-ttu-id="db26f-107">Du kan aktivera din användare tooautomatically get inloggade tooSkyDesk e-post (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="db26f-107">You can enable your users tooautomatically get signed-on tooSkyDesk Email (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="db26f-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="db26f-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="db26f-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="db26f-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="db26f-110">Krav</span><span class="sxs-lookup"><span data-stu-id="db26f-110">Prerequisites</span></span>

<span data-ttu-id="db26f-111">tooconfigure Azure AD-integrering med SkyDesk e-post, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="db26f-111">tooconfigure Azure AD integration with SkyDesk Email, you need hello following items:</span></span>

- <span data-ttu-id="db26f-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="db26f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="db26f-113">En e-SkyDesk enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="db26f-113">A SkyDesk Email single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="db26f-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="db26f-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="db26f-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="db26f-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="db26f-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="db26f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="db26f-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="db26f-117">If you don't have an Azure AD trial environment, you can get a one-month trial here [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="db26f-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="db26f-118">Scenario description</span></span>
<span data-ttu-id="db26f-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="db26f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="db26f-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="db26f-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="db26f-121">Lägga till SkyDesk från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="db26f-121">Adding SkyDesk Email from hello gallery</span></span>
2. <span data-ttu-id="db26f-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="db26f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-skydesk-email-from-hello-gallery"></a><span data-ttu-id="db26f-123">Lägga till SkyDesk från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="db26f-123">Adding SkyDesk Email from hello gallery</span></span>
<span data-ttu-id="db26f-124">tooconfigure hello integrering av SkyDesk e-post i Azure AD, behöver du tooadd SkyDesk e-post från hello galleriet tooyour lista över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="db26f-124">tooconfigure hello integration of SkyDesk Email into Azure AD, you need tooadd SkyDesk Email from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="db26f-125">**tooadd SkyDesk e-post från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="db26f-125">**tooadd SkyDesk Email from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="db26f-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="db26f-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="db26f-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="db26f-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="db26f-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="db26f-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="db26f-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="db26f-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="db26f-133">Skriv i sökrutan hello **SkyDesk e-**.</span><span class="sxs-lookup"><span data-stu-id="db26f-133">In hello search box, type **SkyDesk Email**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_search.png)

5. <span data-ttu-id="db26f-135">Markera hello resultat på panelen **SkyDesk e-**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="db26f-135">In hello results panel, select **SkyDesk Email**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="db26f-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="db26f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="db26f-138">Du konfigurera och testa Azure AD enkel inloggning med SkyDesk e-post baserat på en testanvändare som kallas ”Britta Simon” i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="db26f-138">In this section, you configure and test Azure AD single sign-on with SkyDesk Email based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="db26f-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i SkyDesk e-post är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="db26f-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SkyDesk Email is tooa user in Azure AD.</span></span> <span data-ttu-id="db26f-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i SkyDesk e-post toobe upprätta.</span><span class="sxs-lookup"><span data-stu-id="db26f-140">In other words, a link relationship between an Azure AD user and hello related user in SkyDesk Email needs toobe established.</span></span>

<span data-ttu-id="db26f-141">I SkyDesk e-post, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="db26f-141">In SkyDesk Email, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="db26f-142">tooconfigure och testa Azure AD enkel inloggning med SkyDesk e-post, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="db26f-142">tooconfigure and test Azure AD single sign-on with SkyDesk Email, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="db26f-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="db26f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="db26f-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="db26f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="db26f-145">**[Skapa en testanvändare SkyDesk e-](#creating-a-skydesk-email-test-user)**  -toohave en motsvarighet för Britta Simon i SkyDesk e-post som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="db26f-145">**[Creating a SkyDesk Email test user](#creating-a-skydesk-email-test-user)** - toohave a counterpart of Britta Simon in SkyDesk Email that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="db26f-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="db26f-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="db26f-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="db26f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="db26f-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="db26f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="db26f-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i SkyDesk e-postprogrammet.</span><span class="sxs-lookup"><span data-stu-id="db26f-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your SkyDesk Email application.</span></span>

<span data-ttu-id="db26f-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med SkyDesk e-post:**</span><span class="sxs-lookup"><span data-stu-id="db26f-150">**tooconfigure Azure AD single sign-on with SkyDesk Email, perform hello following steps:**</span></span>

1. <span data-ttu-id="db26f-151">I hello Azure-portalen på hello **SkyDesk e-** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="db26f-151">In hello Azure portal, on hello **SkyDesk Email** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="db26f-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="db26f-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_samlbase.png)

3. <span data-ttu-id="db26f-155">På hello **SkyDesk e-postdomän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="db26f-155">On hello **SkyDesk Email Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_url.png)

    <span data-ttu-id="db26f-157">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://mail.skydesk.jp/portal/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="db26f-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://mail.skydesk.jp/portal/<companyname>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="db26f-158">hello-värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="db26f-158">hello value is not real.</span></span> <span data-ttu-id="db26f-159">Hello uppdateringsvärde med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="db26f-159">Update hello value with hello actual Sign-On URL.</span></span> <span data-ttu-id="db26f-160">Kontakta [SkyDesk e-postklient supportteamet](https://www.skydesk.sg/support/) tooget hello värde.</span><span class="sxs-lookup"><span data-stu-id="db26f-160">Contact [SkyDesk Email Client support team](https://www.skydesk.sg/support/) tooget hello value.</span></span> 
 
4. <span data-ttu-id="db26f-161">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="db26f-161">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_certificate.png) 

5. <span data-ttu-id="db26f-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="db26f-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="db26f-165">På hello **SkyDesk e-postkonfigurationen** klickar du på **Konfigurera e-post från SkyDesk** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="db26f-165">On hello **SkyDesk Email Configuration** section, click **Configure SkyDesk Email** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="db26f-166">Kopiera hello **Sign-Out URL och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="db26f-166">Copy hello **Sign-Out URL, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_configure.png) 

7. <span data-ttu-id="db26f-168">tooenable SSO i **SkyDesk e-**, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="db26f-168">tooenable SSO in **SkyDesk Email**, perform hello following steps:</span></span>

    <span data-ttu-id="db26f-169">a.</span><span class="sxs-lookup"><span data-stu-id="db26f-169">a.</span></span> <span data-ttu-id="db26f-170">Inloggning tooyour SkyDesk e-postkonto som administratör.</span><span class="sxs-lookup"><span data-stu-id="db26f-170">Sign-on tooyour SkyDesk Email account as administrator.</span></span>

    <span data-ttu-id="db26f-171">b.</span><span class="sxs-lookup"><span data-stu-id="db26f-171">b.</span></span> <span data-ttu-id="db26f-172">Hello-menyn överst hello **installationsprogrammet**, och välj **Org**.</span><span class="sxs-lookup"><span data-stu-id="db26f-172">In hello menu on hello top, click **Setup**, and select **Org**.</span></span> 
    
      ![Konfigurera enkel inloggning](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_51.png)
  
    <span data-ttu-id="db26f-174">c.</span><span class="sxs-lookup"><span data-stu-id="db26f-174">c.</span></span> <span data-ttu-id="db26f-175">Klicka på **domäner** hello vänstra panelen.</span><span class="sxs-lookup"><span data-stu-id="db26f-175">Click on **Domains** from hello left panel.</span></span>
    
      ![Konfigurera enkel inloggning](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_53.png)

    <span data-ttu-id="db26f-177">d.</span><span class="sxs-lookup"><span data-stu-id="db26f-177">d.</span></span> <span data-ttu-id="db26f-178">Klicka på **Lägg till domän**.</span><span class="sxs-lookup"><span data-stu-id="db26f-178">Click on **Add Domain**.</span></span>
    
      ![Konfigurera enkel inloggning](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_54.png)

    <span data-ttu-id="db26f-180">e.</span><span class="sxs-lookup"><span data-stu-id="db26f-180">e.</span></span> <span data-ttu-id="db26f-181">Ange ditt domännamn och kontrollera hello domän.</span><span class="sxs-lookup"><span data-stu-id="db26f-181">Enter your Domain name, and then verify hello Domain.</span></span>
    
      ![Konfigurera enkel inloggning](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_55.png)

    <span data-ttu-id="db26f-183">f.</span><span class="sxs-lookup"><span data-stu-id="db26f-183">f.</span></span> <span data-ttu-id="db26f-184">Klicka på **SAML-autentisering** hello vänstra panelen.</span><span class="sxs-lookup"><span data-stu-id="db26f-184">Click on **SAML Authentication** from hello left panel.</span></span>
    
      ![Konfigurera enkel inloggning](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_52.png)

8. <span data-ttu-id="db26f-186">På hello **SAML-autentisering** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="db26f-186">On hello **SAML Authentication** dialog page, perform hello following steps:</span></span>
   
      ![Konfigurera enkel inloggning](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_56.png)
   
    >[!NOTE]
    ><span data-ttu-id="db26f-188">toouse SAML-baserad autentisering, bör du antingen ha **verifierade domän** eller **portal URL** installationen.</span><span class="sxs-lookup"><span data-stu-id="db26f-188">toouse SAML based authentication, you should either have **verified domain** or **portal URL** setup.</span></span> <span data-ttu-id="db26f-189">Du kan ange hello portal URL: en med hello unikt namn.</span><span class="sxs-lookup"><span data-stu-id="db26f-189">You can set hello portal URL with hello unique name.</span></span>
    > 
    > 
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_57.png)

    <span data-ttu-id="db26f-191">a.</span><span class="sxs-lookup"><span data-stu-id="db26f-191">a.</span></span> <span data-ttu-id="db26f-192">I hello **inloggnings-URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="db26f-192">In hello **Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="db26f-193">b.</span><span class="sxs-lookup"><span data-stu-id="db26f-193">b.</span></span> <span data-ttu-id="db26f-194">I hello **logga ut** klistra in hello värdet för URL: en textruta **Sign-Out URL**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="db26f-194">In hello **Logout** URL textbox, paste hello value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="db26f-195">c.</span><span class="sxs-lookup"><span data-stu-id="db26f-195">c.</span></span> <span data-ttu-id="db26f-196">**Ändra lösenord URL** är valfritt så lämna det tomt.</span><span class="sxs-lookup"><span data-stu-id="db26f-196">**Change Password URL** is optional so leave it blank.</span></span>

    <span data-ttu-id="db26f-197">d.</span><span class="sxs-lookup"><span data-stu-id="db26f-197">d.</span></span> <span data-ttu-id="db26f-198">Klicka på **hämta nyckeln från filen** tooselect hämtade certifikatet från Azure-portalen och klicka sedan på **öppna** tooupload hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="db26f-198">Click on **Get Key From File** tooselect your downloaded certificate from Azure portal, and then click **Open** tooupload hello certificate.</span></span>

    <span data-ttu-id="db26f-199">e.</span><span class="sxs-lookup"><span data-stu-id="db26f-199">e.</span></span> <span data-ttu-id="db26f-200">Som **algoritmen**väljer **RSA**.</span><span class="sxs-lookup"><span data-stu-id="db26f-200">As **Algorithm**, select **RSA**.</span></span>

    <span data-ttu-id="db26f-201">f.</span><span class="sxs-lookup"><span data-stu-id="db26f-201">f.</span></span> <span data-ttu-id="db26f-202">Klicka på **Ok** toosave hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="db26f-202">Click **Ok** toosave hello changes.</span></span>

> [!TIP]
> <span data-ttu-id="db26f-203">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="db26f-203">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="db26f-204">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="db26f-204">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="db26f-205">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="db26f-205">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="db26f-206">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="db26f-206">Creating an Azure AD test user</span></span>
<span data-ttu-id="db26f-207">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="db26f-207">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="db26f-209">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="db26f-209">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="db26f-210">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="db26f-210">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="db26f-212">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="db26f-212">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="db26f-214">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="db26f-214">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="db26f-216">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="db26f-216">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="db26f-218">a.</span><span class="sxs-lookup"><span data-stu-id="db26f-218">a.</span></span> <span data-ttu-id="db26f-219">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="db26f-219">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="db26f-220">b.</span><span class="sxs-lookup"><span data-stu-id="db26f-220">b.</span></span> <span data-ttu-id="db26f-221">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="db26f-221">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="db26f-222">c.</span><span class="sxs-lookup"><span data-stu-id="db26f-222">c.</span></span> <span data-ttu-id="db26f-223">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="db26f-223">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="db26f-224">d.</span><span class="sxs-lookup"><span data-stu-id="db26f-224">d.</span></span> <span data-ttu-id="db26f-225">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="db26f-225">Click **Create**.</span></span>
 
### <a name="creating-a-skydesk-email-test-user"></a><span data-ttu-id="db26f-226">Skapa en testanvändare SkyDesk e-post</span><span class="sxs-lookup"><span data-stu-id="db26f-226">Creating a SkyDesk Email test user</span></span>

<span data-ttu-id="db26f-227">I det här avsnittet kan du skapa en användare som kallas Britta Simon i SkyDesk e-post.</span><span class="sxs-lookup"><span data-stu-id="db26f-227">In this section, you create a user called Britta Simon in SkyDesk Email.</span></span>

1. <span data-ttu-id="db26f-228">Klicka på **användaråtkomst** från vänster hello panelen i SkyDesk e-post och ange sedan ditt användarnamn.</span><span class="sxs-lookup"><span data-stu-id="db26f-228">Click on **User Access** from hello left panel in SkyDesk Email and then enter your username.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_58.png)

>[!NOTE] 
><span data-ttu-id="db26f-230">Om du behöver toocreate grupp användare, måste toocontact hello [SkyDesk e-postklient supportteamet](https://www.skydesk.sg/support/).</span><span class="sxs-lookup"><span data-stu-id="db26f-230">If you need toocreate bulk users, you need toocontact hello [SkyDesk Email Client support team](https://www.skydesk.sg/support/).</span></span>


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="db26f-231">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="db26f-231">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="db26f-232">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSkyDesk e-post.</span><span class="sxs-lookup"><span data-stu-id="db26f-232">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSkyDesk Email.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="db26f-234">**tooassign Britta Simon tooSkyDesk e-post, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="db26f-234">**tooassign Britta Simon tooSkyDesk Email, perform hello following steps:**</span></span>

1. <span data-ttu-id="db26f-235">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="db26f-235">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="db26f-237">Välj i listan med program hello **SkyDesk e-**.</span><span class="sxs-lookup"><span data-stu-id="db26f-237">In hello applications list, select **SkyDesk Email**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_app.png) 

3. <span data-ttu-id="db26f-239">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="db26f-239">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="db26f-241">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="db26f-241">Click **Add** button.</span></span> <span data-ttu-id="db26f-242">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="db26f-242">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="db26f-244">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="db26f-244">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="db26f-245">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="db26f-245">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="db26f-246">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="db26f-246">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="db26f-247">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="db26f-247">Testing single sign-on</span></span>

<span data-ttu-id="db26f-248">hello syftet med det här avsnittet är tootest din Azure AD SSO konfiguration av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="db26f-248">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="db26f-249">När du klickar på hello SkyDesk e-panelen i hello åtkomstpanelen får automatiskt inloggade tooyour SkyDesk e-postprogram.</span><span class="sxs-lookup"><span data-stu-id="db26f-249">When you click hello SkyDesk Email tile in hello Access Panel, you should get automatically signed-on tooyour SkyDesk Email application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="db26f-250">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="db26f-250">Additional resources</span></span>

* [<span data-ttu-id="db26f-251">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="db26f-251">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="db26f-252">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="db26f-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_203.png

