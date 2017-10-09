---
title: "Självstudier: Azure Active Directory-integrering med Work.com | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Work.com."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 98e6739e-eb24-46bd-9dd3-20b489839076
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: dcdc51c884abd78c945b649de99f942d32373cf6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workcom"></a><span data-ttu-id="8bf17-103">Självstudier: Azure Active Directory-integrering med Work.com</span><span class="sxs-lookup"><span data-stu-id="8bf17-103">Tutorial: Azure Active Directory integration with Work.com</span></span>

<span data-ttu-id="8bf17-104">I kursen får du lära dig hur toointegrate Work.com med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="8bf17-104">In this tutorial, you learn how toointegrate Work.com with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8bf17-105">Integrera Work.com med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="8bf17-105">Integrating Work.com with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="8bf17-106">Du kan styra i Azure AD som har åtkomst till tooWork.com</span><span class="sxs-lookup"><span data-stu-id="8bf17-106">You can control in Azure AD who has access tooWork.com</span></span>
- <span data-ttu-id="8bf17-107">Du kan aktivera din användare tooautomatically get inloggade tooWork.com (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="8bf17-107">You can enable your users tooautomatically get signed-on tooWork.com (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8bf17-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="8bf17-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="8bf17-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8bf17-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8bf17-110">Krav</span><span class="sxs-lookup"><span data-stu-id="8bf17-110">Prerequisites</span></span>

<span data-ttu-id="8bf17-111">tooconfigure Azure AD-integrering med Work.com, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="8bf17-111">tooconfigure Azure AD integration with Work.com, you need hello following items:</span></span>

- <span data-ttu-id="8bf17-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="8bf17-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8bf17-113">En Work.com enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="8bf17-113">A Work.com single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8bf17-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="8bf17-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8bf17-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="8bf17-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8bf17-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="8bf17-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8bf17-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8bf17-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8bf17-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="8bf17-118">Scenario description</span></span>
<span data-ttu-id="8bf17-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="8bf17-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8bf17-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="8bf17-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8bf17-121">Lägg till Work.com från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="8bf17-121">Add Work.com from hello gallery</span></span>
2. <span data-ttu-id="8bf17-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8bf17-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-workcom-from-hello-gallery"></a><span data-ttu-id="8bf17-123">Lägg till Work.com från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="8bf17-123">Add Work.com from hello gallery</span></span>
<span data-ttu-id="8bf17-124">tooconfigure hello integrering av Work.com i Azure AD, behöver du tooadd Work.com hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="8bf17-124">tooconfigure hello integration of Work.com into Azure AD, you need tooadd Work.com from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="8bf17-125">**tooadd Work.com från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="8bf17-125">**tooadd Work.com from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="8bf17-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="8bf17-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8bf17-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="8bf17-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="8bf17-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="8bf17-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="8bf17-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8bf17-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="8bf17-133">Skriv i sökrutan hello **Work.com**väljer **Work.com** från resultatrutan Klicka **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="8bf17-133">In hello search box, type **Work.com**, select **Work.com** from results panel then click **Add** button tooadd hello application.</span></span>

    ![Lägg till från galleriet](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="8bf17-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8bf17-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="8bf17-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Work.com baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="8bf17-136">In this section, you configure and test Azure AD single sign-on with Work.com based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="8bf17-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Work.com är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8bf17-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Work.com is tooa user in Azure AD.</span></span> <span data-ttu-id="8bf17-138">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Work.com toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="8bf17-138">In other words, a link relationship between an Azure AD user and hello related user in Work.com needs toobe established.</span></span>

<span data-ttu-id="8bf17-139">I Work.com, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="8bf17-139">In Work.com, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="8bf17-140">tooconfigure och testa Azure AD enkel inloggning med Work.com, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="8bf17-140">tooconfigure and test Azure AD single sign-on with Work.com, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="8bf17-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="8bf17-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="8bf17-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8bf17-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8bf17-143">**[Skapa en testanvändare Work.com](#create-a-workcom-test-user)**  -toohave en motsvarighet för Britta Simon i Work.com som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="8bf17-143">**[Create a Work.com test user](#create-a-workcom-test-user)** - toohave a counterpart of Britta Simon in Work.com that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="8bf17-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="8bf17-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8bf17-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="8bf17-145">**[Test Single Sign-On](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="8bf17-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8bf17-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="8bf17-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Work.com program.</span><span class="sxs-lookup"><span data-stu-id="8bf17-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Work.com application.</span></span>

>[!NOTE]
><span data-ttu-id="8bf17-148">tooconfigure enkel inloggning, måste toosetup ett anpassat domännamn Work.com ännu.</span><span class="sxs-lookup"><span data-stu-id="8bf17-148">tooconfigure single sign-on, you need toosetup a custom Work.com domain name yet.</span></span> <span data-ttu-id="8bf17-149">Du behöver toodefine minst en domän namn, testa ditt domännamn och distribuera den tooyour hela organisationen.</span><span class="sxs-lookup"><span data-stu-id="8bf17-149">You need toodefine at least a domain name, test your domain name, and deploy it tooyour entire organization.</span></span>

<span data-ttu-id="8bf17-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Work.com:**</span><span class="sxs-lookup"><span data-stu-id="8bf17-150">**tooconfigure Azure AD single sign-on with Work.com, perform hello following steps:**</span></span>

1. <span data-ttu-id="8bf17-151">I hello Azure-portalen på hello **Work.com** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="8bf17-151">In hello Azure portal, on hello **Work.com** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="8bf17-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="8bf17-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![SAML-baserade inloggning](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_samlbase.png)

3. <span data-ttu-id="8bf17-155">På hello **Work.com domän och URL: er** avsnittet, utföra hello följande:</span><span class="sxs-lookup"><span data-stu-id="8bf17-155">On hello **Work.com Domain and URLs** section, perform hello following:</span></span>

    ![Avsnittet Work.com domän och URL: er](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_url.png)

    <span data-ttu-id="8bf17-157">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`http://<companyname>.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="8bf17-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `http://<companyname>.my.salesforce.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="8bf17-158">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="8bf17-158">This value is not real.</span></span> <span data-ttu-id="8bf17-159">Uppdatera det här värdet med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="8bf17-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="8bf17-160">Kontakta [Work.com klienten supportteamet](https://help.salesforce.com/articleView?id=000159855&type=3) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="8bf17-160">Contact [Work.com Client support team](https://help.salesforce.com/articleView?id=000159855&type=3) tooget this value.</span></span> 

4. <span data-ttu-id="8bf17-161">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="8bf17-161">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Signeringscertifikat för SAML-avsnitt](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_certificate.png) 

5. <span data-ttu-id="8bf17-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="8bf17-163">Click **Save** button.</span></span>

    ![Knappen Spara](./media/active-directory-saas-work-com-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="8bf17-165">På hello **Work.com Configuration** klickar du på **konfigurera Work.com** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="8bf17-165">On hello **Work.com Configuration** section, click **Configure Work.com** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="8bf17-166">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="8bf17-166">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Work.com konfigurationsavsnitt](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_configure.png) 
7. <span data-ttu-id="8bf17-168">Logga in tooyour Work.com klient som administratör.</span><span class="sxs-lookup"><span data-stu-id="8bf17-168">Log in tooyour Work.com tenant as administrator.</span></span>

8. <span data-ttu-id="8bf17-169">Gå för**installationsprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="8bf17-169">Go too**Setup**.</span></span>
   
    <span data-ttu-id="8bf17-170">![Installationsprogrammet](./media/active-directory-saas-work-com-tutorial/ic794108.png "installationen")</span><span class="sxs-lookup"><span data-stu-id="8bf17-170">![Setup](./media/active-directory-saas-work-com-tutorial/ic794108.png "Setup")</span></span>

9. <span data-ttu-id="8bf17-171">Hello vänstra navigeringsfönstret i hello **administrera** klickar du på **domänhantering** tooexpand hello Närliggande avsnitt och klickar sedan på **min domän** tooopen hello  **Min domän** sidan.</span><span class="sxs-lookup"><span data-stu-id="8bf17-171">On hello left navigation pane, in hello **Administer** section, click **Domain Management** tooexpand hello related section, and then click **My Domain** tooopen hello **My Domain** page.</span></span> 
   
    <span data-ttu-id="8bf17-172">![Min domän](./media/active-directory-saas-work-com-tutorial/ic767825.png "min domän")</span><span class="sxs-lookup"><span data-stu-id="8bf17-172">![My Domain](./media/active-directory-saas-work-com-tutorial/ic767825.png "My Domain")</span></span>

10. <span data-ttu-id="8bf17-173">tooverify som din domän har angetts korrekt, kontrollera att den är i ”**steg 4 distribueras tooUsers**” och granska din ”**Mina Domäninställningar**”.</span><span class="sxs-lookup"><span data-stu-id="8bf17-173">tooverify that your domain has been set up correctly, make sure that it is in “**Step 4 Deployed tooUsers**” and review your “**My Domain Settings**”.</span></span>
   
    <span data-ttu-id="8bf17-174">![Domän distribuerat tooUser](./media/active-directory-saas-work-com-tutorial/ic784377.png "domän distribueras tooUser")</span><span class="sxs-lookup"><span data-stu-id="8bf17-174">![Domain Deployed tooUser](./media/active-directory-saas-work-com-tutorial/ic784377.png "Domain Deployed tooUser")</span></span>

11. <span data-ttu-id="8bf17-175">Logga in tooyour Work.com klient.</span><span class="sxs-lookup"><span data-stu-id="8bf17-175">Log in tooyour Work.com tenant.</span></span>

12. <span data-ttu-id="8bf17-176">Gå för**installationsprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="8bf17-176">Go too**Setup**.</span></span>
    
    <span data-ttu-id="8bf17-177">![Installationsprogrammet](./media/active-directory-saas-work-com-tutorial/ic794108.png "installationen")</span><span class="sxs-lookup"><span data-stu-id="8bf17-177">![Setup](./media/active-directory-saas-work-com-tutorial/ic794108.png "Setup")</span></span>

13. <span data-ttu-id="8bf17-178">Expandera hello **säkerhetsåtgärder** -menyn och klicka sedan på **inställningar för enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="8bf17-178">Expand hello **Security Controls** menu, and then click **Single Sign-On Settings**.</span></span>
    
    <span data-ttu-id="8bf17-179">![Enkel inloggning inställningar](./media/active-directory-saas-work-com-tutorial/ic794113.png "enkel inloggning inställningar")</span><span class="sxs-lookup"><span data-stu-id="8bf17-179">![Single Sign-On Settings](./media/active-directory-saas-work-com-tutorial/ic794113.png "Single Sign-On Settings")</span></span>

14. <span data-ttu-id="8bf17-180">På hello **inställningar för enkel inloggning** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="8bf17-180">On hello **Single Sign-On Settings** dialog page, perform hello following steps:</span></span>
    
    <span data-ttu-id="8bf17-181">![SAML aktiverat](./media/active-directory-saas-work-com-tutorial/ic781026.png "SAML aktiverad")</span><span class="sxs-lookup"><span data-stu-id="8bf17-181">![SAML Enabled](./media/active-directory-saas-work-com-tutorial/ic781026.png "SAML Enabled")</span></span>
    
    <span data-ttu-id="8bf17-182">a.</span><span class="sxs-lookup"><span data-stu-id="8bf17-182">a.</span></span> <span data-ttu-id="8bf17-183">Välj **SAML aktiverat**.</span><span class="sxs-lookup"><span data-stu-id="8bf17-183">Select **SAML Enabled**.</span></span>
    
    <span data-ttu-id="8bf17-184">b.</span><span class="sxs-lookup"><span data-stu-id="8bf17-184">b.</span></span> <span data-ttu-id="8bf17-185">Klicka på **Ny**.</span><span class="sxs-lookup"><span data-stu-id="8bf17-185">Click **New**.</span></span>

15. <span data-ttu-id="8bf17-186">I hello **SAML enkel inloggning inställningar** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="8bf17-186">In hello **SAML Single Sign-On Settings** section, perform hello following steps:</span></span>
    
    <span data-ttu-id="8bf17-187">![SAML enkel inloggning inställningen](./media/active-directory-saas-work-com-tutorial/ic794114.png "SAML enkel inloggning inställningen")</span><span class="sxs-lookup"><span data-stu-id="8bf17-187">![SAML Single Sign-On Setting](./media/active-directory-saas-work-com-tutorial/ic794114.png "SAML Single Sign-On Setting")</span></span>
    
    <span data-ttu-id="8bf17-188">a.</span><span class="sxs-lookup"><span data-stu-id="8bf17-188">a.</span></span> <span data-ttu-id="8bf17-189">I hello **namn** textruta, ange ett namn för din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="8bf17-189">In hello **Name** textbox, type a name for your configuration.</span></span>  
       
    > [!NOTE]
    > <span data-ttu-id="8bf17-190">Att tillhandahålla ett värde för **namn** automatiskt fylla hello **API-namnet** textruta.</span><span class="sxs-lookup"><span data-stu-id="8bf17-190">Providing a value for **Name** does automatically populate hello **API Name** textbox.</span></span>
    
    <span data-ttu-id="8bf17-191">b.</span><span class="sxs-lookup"><span data-stu-id="8bf17-191">b.</span></span> <span data-ttu-id="8bf17-192">I **utfärdaren** textruta klistra in hello värdet för **SAML enhets-ID** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8bf17-192">In **Issuer** textbox, paste hello value of **SAML Entity ID** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="8bf17-193">c.</span><span class="sxs-lookup"><span data-stu-id="8bf17-193">c.</span></span> <span data-ttu-id="8bf17-194">tooupload hello hämtat certifikat från Azure-portalen klickar du på **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="8bf17-194">tooupload hello downloaded certificate from Azure portal, click **Browse**.</span></span>
    
    <span data-ttu-id="8bf17-195">d.</span><span class="sxs-lookup"><span data-stu-id="8bf17-195">d.</span></span> <span data-ttu-id="8bf17-196">I hello **enhets-Id** textruta typen `https://salesforce-work.com`.</span><span class="sxs-lookup"><span data-stu-id="8bf17-196">In hello **Entity Id** textbox, type `https://salesforce-work.com`.</span></span>
    
    <span data-ttu-id="8bf17-197">e.</span><span class="sxs-lookup"><span data-stu-id="8bf17-197">e.</span></span> <span data-ttu-id="8bf17-198">Som **SAML identitetstyp**väljer **Assertion innehåller hello Federation ID från hello användarobjektet**.</span><span class="sxs-lookup"><span data-stu-id="8bf17-198">As **SAML Identity Type**, select **Assertion contains hello Federation ID from hello User object**.</span></span>
    
    <span data-ttu-id="8bf17-199">f.</span><span class="sxs-lookup"><span data-stu-id="8bf17-199">f.</span></span> <span data-ttu-id="8bf17-200">Som **SAML identitet plats**väljer **identitet är i hello NameIdentfier elementet i hello ämne instruktionen**.</span><span class="sxs-lookup"><span data-stu-id="8bf17-200">As **SAML Identity Location**, select **Identity is in hello NameIdentfier element of hello Subject statement**.</span></span>
    
    <span data-ttu-id="8bf17-201">g.</span><span class="sxs-lookup"><span data-stu-id="8bf17-201">g.</span></span> <span data-ttu-id="8bf17-202">I **identitet providern inloggnings-URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8bf17-202">In **Identity Provider Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="8bf17-203">h.</span><span class="sxs-lookup"><span data-stu-id="8bf17-203">h.</span></span> <span data-ttu-id="8bf17-204">I **identitet providern logga ut URL** textruta klistra in hello värdet för **Sign-Out URL** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8bf17-204">In **Identity Provider Logout URL** textbox, paste hello value of **Sign-Out URL** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="8bf17-205">Jag.</span><span class="sxs-lookup"><span data-stu-id="8bf17-205">i.</span></span> <span data-ttu-id="8bf17-206">Som **providern initierade begära Tjänstbindning**väljer **HTTP Post**.</span><span class="sxs-lookup"><span data-stu-id="8bf17-206">As **Service Provider Initiated Request Binding**, select **HTTP Post**.</span></span>
    
    <span data-ttu-id="8bf17-207">j.</span><span class="sxs-lookup"><span data-stu-id="8bf17-207">j.</span></span> <span data-ttu-id="8bf17-208">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="8bf17-208">Click **Save**.</span></span>

16. <span data-ttu-id="8bf17-209">I din Work.com klassiska portalen på hello vänstra navigationsfönstret klickar du på **domänhantering** tooexpand hello Närliggande avsnitt och klickar sedan på **min domän** tooopen hello **min domän**sidan.</span><span class="sxs-lookup"><span data-stu-id="8bf17-209">In your Work.com classic portal, on hello left navigation pane, click **Domain Management** tooexpand hello related section, and then click **My Domain** tooopen hello **My Domain** page.</span></span> 
    
    <span data-ttu-id="8bf17-210">![Min domän](./media/active-directory-saas-work-com-tutorial/ic794115.png "min domän")</span><span class="sxs-lookup"><span data-stu-id="8bf17-210">![My Domain](./media/active-directory-saas-work-com-tutorial/ic794115.png "My Domain")</span></span>

17. <span data-ttu-id="8bf17-211">På hello **min domän** i hello sidan **inloggningen sidan anpassning** klickar du på **redigera**.</span><span class="sxs-lookup"><span data-stu-id="8bf17-211">On hello **My Domain** page, in hello **Login Page Branding** section, click **Edit**.</span></span>
    
    <span data-ttu-id="8bf17-212">![Inloggningssidan anpassning](./media/active-directory-saas-work-com-tutorial/ic767826.png "inloggningssidan anpassning")</span><span class="sxs-lookup"><span data-stu-id="8bf17-212">![Login Page Branding](./media/active-directory-saas-work-com-tutorial/ic767826.png "Login Page Branding")</span></span>

14. <span data-ttu-id="8bf17-213">På hello **inloggningen sidan anpassning** i hello sidan **Autentiseringstjänsten** avsnitt, hello namnet på din **SAML SSO inställningar** visas.</span><span class="sxs-lookup"><span data-stu-id="8bf17-213">On hello **Login Page Branding** page, in hello **Authentication Service** section, hello name of your **SAML SSO Settings** is displayed.</span></span> <span data-ttu-id="8bf17-214">Markera den och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="8bf17-214">Select it, and then click **Save**.</span></span>
    
    <span data-ttu-id="8bf17-215">![Inloggningssidan anpassning](./media/active-directory-saas-work-com-tutorial/ic784366.png "inloggningssidan anpassning")</span><span class="sxs-lookup"><span data-stu-id="8bf17-215">![Login Page Branding](./media/active-directory-saas-work-com-tutorial/ic784366.png "Login Page Branding")</span></span>

> [!TIP]
> <span data-ttu-id="8bf17-216">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="8bf17-216">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="8bf17-217">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="8bf17-217">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="8bf17-218">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8bf17-218">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="8bf17-219">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="8bf17-219">Create an Azure AD test user</span></span>
<span data-ttu-id="8bf17-220">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8bf17-220">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="8bf17-222">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="8bf17-222">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="8bf17-223">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="8bf17-223">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-work-com-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8bf17-225">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="8bf17-225">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Användare och grupper -> alla användare](./media/active-directory-saas-work-com-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8bf17-227">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8bf17-227">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Lägg till](./media/active-directory-saas-work-com-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8bf17-229">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="8bf17-229">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Dialogrutan Användarsida](./media/active-directory-saas-work-com-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8bf17-231">a.</span><span class="sxs-lookup"><span data-stu-id="8bf17-231">a.</span></span> <span data-ttu-id="8bf17-232">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8bf17-232">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8bf17-233">b.</span><span class="sxs-lookup"><span data-stu-id="8bf17-233">b.</span></span> <span data-ttu-id="8bf17-234">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8bf17-234">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8bf17-235">c.</span><span class="sxs-lookup"><span data-stu-id="8bf17-235">c.</span></span> <span data-ttu-id="8bf17-236">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="8bf17-236">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="8bf17-237">d.</span><span class="sxs-lookup"><span data-stu-id="8bf17-237">d.</span></span> <span data-ttu-id="8bf17-238">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="8bf17-238">Click **Create**.</span></span>
 
### <a name="create-a-workcom-test-user"></a><span data-ttu-id="8bf17-239">Skapa en testanvändare Work.com</span><span class="sxs-lookup"><span data-stu-id="8bf17-239">Create a Work.com test user</span></span>
<span data-ttu-id="8bf17-240">För Azure Active Directory användare toobe kan toosign i, måste de vara etablerade tooWork.com. Hello gäller Work.com är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="8bf17-240">For Azure Active Directory users toobe able toosign in, they must be provisioned tooWork.com. In hello case of Work.com, provisioning is a manual task.</span></span>

### <a name="tooconfigure-user-provisioning-perform-hello-following-steps"></a><span data-ttu-id="8bf17-241">tooconfigure användaretablering, utför följande steg hello:</span><span class="sxs-lookup"><span data-stu-id="8bf17-241">tooconfigure user provisioning, perform hello following steps:</span></span>
1. <span data-ttu-id="8bf17-242">Inloggning tooyour Work.com företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="8bf17-242">Sign on tooyour Work.com company site as an administrator.</span></span>

2. <span data-ttu-id="8bf17-243">Gå för**installationsprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="8bf17-243">Go too**Setup**.</span></span>
   
    <span data-ttu-id="8bf17-244">![Installationsprogrammet](./media/active-directory-saas-work-com-tutorial/IC794108.png "installationen")</span><span class="sxs-lookup"><span data-stu-id="8bf17-244">![Setup](./media/active-directory-saas-work-com-tutorial/IC794108.png "Setup")</span></span>
3. <span data-ttu-id="8bf17-245">Gå för**hantera användare \> användare**.</span><span class="sxs-lookup"><span data-stu-id="8bf17-245">Go too**Manage Users \> Users**.</span></span>
   
    <span data-ttu-id="8bf17-246">![Hantera användare](./media/active-directory-saas-work-com-tutorial/IC784369.png "hantera användare")</span><span class="sxs-lookup"><span data-stu-id="8bf17-246">![Manage Users](./media/active-directory-saas-work-com-tutorial/IC784369.png "Manage Users")</span></span>

4. <span data-ttu-id="8bf17-247">Klicka på **ny användare**.</span><span class="sxs-lookup"><span data-stu-id="8bf17-247">Click **New User**.</span></span>
   
    <span data-ttu-id="8bf17-248">![Alla användare](./media/active-directory-saas-work-com-tutorial/IC794117.png "alla användare")</span><span class="sxs-lookup"><span data-stu-id="8bf17-248">![All Users](./media/active-directory-saas-work-com-tutorial/IC794117.png "All Users")</span></span>

5. <span data-ttu-id="8bf17-249">Hello användare redigera avsnittet, utföra hello följa stegen i attribut för ett giltigt Azure AD-kontot som du vill använda tooprovision hello relaterade textrutor:</span><span class="sxs-lookup"><span data-stu-id="8bf17-249">In hello User Edit section, perform hello following steps, in attributes of a valid Azure AD account you want tooprovision into hello related textboxes:</span></span>
   
    <span data-ttu-id="8bf17-250">![Redigera användare](./media/active-directory-saas-work-com-tutorial/ic794118.png "Redigera användare")</span><span class="sxs-lookup"><span data-stu-id="8bf17-250">![User Edit](./media/active-directory-saas-work-com-tutorial/ic794118.png "User Edit")</span></span>
   
    <span data-ttu-id="8bf17-251">a.</span><span class="sxs-lookup"><span data-stu-id="8bf17-251">a.</span></span> <span data-ttu-id="8bf17-252">I hello **Förnamn** textruta typen hello **Förnamn** för hello användare **Britta**.</span><span class="sxs-lookup"><span data-stu-id="8bf17-252">In hello **First Name** textbox, type hello **first name** of hello user **Britta**.</span></span>
    
    <span data-ttu-id="8bf17-253">b.</span><span class="sxs-lookup"><span data-stu-id="8bf17-253">b.</span></span> <span data-ttu-id="8bf17-254">I hello **efternamn** textruta typen hello **efternamn** för hello användare **Simon**.</span><span class="sxs-lookup"><span data-stu-id="8bf17-254">In hello **Last Name** textbox, type hello **last name** of hello user **Simon**.</span></span>
    
    <span data-ttu-id="8bf17-255">c.</span><span class="sxs-lookup"><span data-stu-id="8bf17-255">c.</span></span> <span data-ttu-id="8bf17-256">I hello **Alias** textruta typen hello **namn** för hello användare **BrittaS**.</span><span class="sxs-lookup"><span data-stu-id="8bf17-256">In hello **Alias** textbox, type hello **name** of hello user **BrittaS**.</span></span>
    
    <span data-ttu-id="8bf17-257">d.</span><span class="sxs-lookup"><span data-stu-id="8bf17-257">d.</span></span> <span data-ttu-id="8bf17-258">I hello **e-post** textruta typen hello **e-postadress** för användare  **Brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="8bf17-258">In hello **Email** textbox, type hello **email address** of user **Brittasimon@contoso.com**.</span></span>
    
    <span data-ttu-id="8bf17-259">e.</span><span class="sxs-lookup"><span data-stu-id="8bf17-259">e.</span></span> <span data-ttu-id="8bf17-260">I hello **användarnamn** textruta, Skriv ett användarnamn för användaren som  **Brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="8bf17-260">In hello **User Name** textbox, type a user name of user like **Brittasimon@contoso.com**.</span></span>
    
    <span data-ttu-id="8bf17-261">f.</span><span class="sxs-lookup"><span data-stu-id="8bf17-261">f.</span></span> <span data-ttu-id="8bf17-262">I hello **smeknamn** textruta typ a **smeknamn** för användare **Simon**.</span><span class="sxs-lookup"><span data-stu-id="8bf17-262">In hello **Nick Name** textbox, type a **nick name** of user **Simon**.</span></span>
    
    <span data-ttu-id="8bf17-263">g.</span><span class="sxs-lookup"><span data-stu-id="8bf17-263">g.</span></span> <span data-ttu-id="8bf17-264">Välj **rollen**, **användarlicens**, och **profil**.</span><span class="sxs-lookup"><span data-stu-id="8bf17-264">Select **Role**, **User License**, and **Profile**.</span></span>
    
    <span data-ttu-id="8bf17-265">h.</span><span class="sxs-lookup"><span data-stu-id="8bf17-265">h.</span></span> <span data-ttu-id="8bf17-266">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="8bf17-266">Click **Save**.</span></span>  
      
    > [!NOTE]
    > <span data-ttu-id="8bf17-267">hello Azure AD användare får ett e-postmeddelande inklusive en länk tooconfirm hello innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="8bf17-267">hello Azure AD account holder will get an email including a link tooconfirm hello account before it becomes active.</span></span>
    > 
    > 

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="8bf17-268">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="8bf17-268">Assign hello Azure AD test user</span></span>

<span data-ttu-id="8bf17-269">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooWork.com.</span><span class="sxs-lookup"><span data-stu-id="8bf17-269">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooWork.com.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="8bf17-271">**tooassign Britta Simon tooWork.com utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="8bf17-271">**tooassign Britta Simon tooWork.com, perform hello following steps:**</span></span>

1. <span data-ttu-id="8bf17-272">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="8bf17-272">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="8bf17-274">Välj i listan med program hello **Work.com**.</span><span class="sxs-lookup"><span data-stu-id="8bf17-274">In hello applications list, select **Work.com**.</span></span>

    ![Work.com i appens lista](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_app.png) 

3. <span data-ttu-id="8bf17-276">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="8bf17-276">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="8bf17-278">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="8bf17-278">Click **Add** button.</span></span> <span data-ttu-id="8bf17-279">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8bf17-279">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="8bf17-281">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="8bf17-281">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="8bf17-282">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8bf17-282">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8bf17-283">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8bf17-283">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="8bf17-284">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8bf17-284">Test single sign-on</span></span>

<span data-ttu-id="8bf17-285">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="8bf17-285">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="8bf17-286">Du bör få automatiskt inloggade tooyour Work.com programmet när du klickar på hello Work.com panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="8bf17-286">When you click hello Work.com tile in hello Access Panel, you should get automatically signed-on tooyour Work.com application.</span></span>
<span data-ttu-id="8bf17-287">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="8bf17-287">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8bf17-288">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="8bf17-288">Additional resources</span></span>

* [<span data-ttu-id="8bf17-289">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8bf17-289">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8bf17-290">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="8bf17-290">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_203.png

