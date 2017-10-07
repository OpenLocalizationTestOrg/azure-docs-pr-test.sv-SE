---
title: "Självstudier: Azure Active Directory-integrering med upptar LMS | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och upptar LMS."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ba9f1b3d-a4a0-4ff7-b0e7-428e0ed92142
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: jeedes
ms.openlocfilehash: a140a78a3a9474a6372a3ad4fb8251bd2452c990
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-absorb-lms"></a><span data-ttu-id="96f13-103">Självstudier: Azure Active Directory-integrering med upptar LMS</span><span class="sxs-lookup"><span data-stu-id="96f13-103">Tutorial: Azure Active Directory integration with Absorb LMS</span></span>

<span data-ttu-id="96f13-104">I kursen får du lära dig hur toointegrate Absorb LMS med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="96f13-104">In this tutorial, you learn how toointegrate Absorb LMS with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="96f13-105">Integrera upptar LMS med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="96f13-105">Integrating Absorb LMS with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="96f13-106">Du kan styra i Azure AD som har åtkomst tooAbsorb LMS</span><span class="sxs-lookup"><span data-stu-id="96f13-106">You can control in Azure AD who has access tooAbsorb LMS</span></span>
- <span data-ttu-id="96f13-107">Du kan aktivera din användare tooautomatically get inloggade tooAbsorb LMS (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="96f13-107">You can enable your users tooautomatically get signed-on tooAbsorb LMS (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="96f13-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="96f13-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="96f13-109">Om du vill tooknow finns mer information om SaaS appintegrering med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="96f13-109">If you want tooknow more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="96f13-110">[Vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="96f13-110">[What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="96f13-111">Krav</span><span class="sxs-lookup"><span data-stu-id="96f13-111">Prerequisites</span></span>

<span data-ttu-id="96f13-112">tooconfigure Azure AD-integrering med upptar LMS måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="96f13-112">tooconfigure Azure AD integration with Absorb LMS, you need hello following items:</span></span>

- <span data-ttu-id="96f13-113">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="96f13-113">An Azure AD subscription</span></span>
- <span data-ttu-id="96f13-114">En upptar LMS enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="96f13-114">An Absorb LMS single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="96f13-115">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="96f13-115">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="96f13-116">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="96f13-116">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="96f13-117">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="96f13-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="96f13-118">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="96f13-118">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="96f13-119">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="96f13-119">Scenario description</span></span>
<span data-ttu-id="96f13-120">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="96f13-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="96f13-121">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="96f13-121">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="96f13-122">Att lägga till upptar LMS från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="96f13-122">Adding Absorb LMS from hello gallery</span></span>
2. <span data-ttu-id="96f13-123">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="96f13-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-absorb-lms-from-hello-gallery"></a><span data-ttu-id="96f13-124">Att lägga till upptar LMS från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="96f13-124">Adding Absorb LMS from hello gallery</span></span>
<span data-ttu-id="96f13-125">tooconfigure hello integrering av upptar LMS i tooAzure AD, behöver du tooadd Absorb LMS hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="96f13-125">tooconfigure hello integration of Absorb LMS in tooAzure AD, you need tooadd Absorb LMS from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="96f13-126">**tooadd Absorb LMS från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="96f13-126">**tooadd Absorb LMS from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="96f13-127">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="96f13-127">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="96f13-129">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="96f13-129">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="96f13-130">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="96f13-130">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="96f13-132">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="96f13-132">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="96f13-134">Skriv i sökrutan hello **upptar LMS**väljer **upptar LMS** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="96f13-134">In hello search box, type **Absorb LMS**, select **Absorb LMS** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Upptar LMS i hello resultatlistan](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="96f13-136">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="96f13-136">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="96f13-137">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med upptar LMS baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="96f13-137">In this section, you configure and test Azure AD single sign-on with Absorb LMS based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="96f13-138">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i upptar LMS är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="96f13-138">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Absorb LMS is tooa user in Azure AD.</span></span> <span data-ttu-id="96f13-139">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i upptar LMS toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="96f13-139">In other words, a link relationship between an Azure AD user and hello related user in Absorb LMS needs toobe established.</span></span>

<span data-ttu-id="96f13-140">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i upptar LMS.</span><span class="sxs-lookup"><span data-stu-id="96f13-140">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Absorb LMS.</span></span>

<span data-ttu-id="96f13-141">tooconfigure och testa Azure AD enkel inloggning med upptar LMS, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="96f13-141">tooconfigure and test Azure AD single sign-on with Absorb LMS, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="96f13-142">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="96f13-142">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="96f13-143">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="96f13-143">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="96f13-144">**[Skapa en testanvändare upptar LMS](#create-an-absorb-lms-test-user)**  -toohave en motsvarighet för Britta Simon i upptar LMS som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="96f13-144">**[Create an Absorb LMS test user](#create-an-absorb-lms-test-user)** - toohave a counterpart of Britta Simon in Absorb LMS that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="96f13-145">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="96f13-145">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="96f13-146">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="96f13-146">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="96f13-147">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="96f13-147">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="96f13-148">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt program skulle kunna ta emot LMS.</span><span class="sxs-lookup"><span data-stu-id="96f13-148">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Absorb LMS application.</span></span>

<span data-ttu-id="96f13-149">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med upptar LMS:**</span><span class="sxs-lookup"><span data-stu-id="96f13-149">**tooconfigure Azure AD single sign-on with Absorb LMS, perform hello following steps:**</span></span>

1. <span data-ttu-id="96f13-150">I hello Azure-portalen på hello **upptar LMS** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="96f13-150">In hello Azure portal, on hello **Absorb LMS** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="96f13-152">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="96f13-152">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_samlbase.png)

3. <span data-ttu-id="96f13-154">På hello **upptar LMS domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="96f13-154">On hello **Absorb LMS Domain and URLs** section, perform hello following steps:</span></span>

    ![Upptar LMS domän URL: er och enkel inloggning information](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_url.png)

    <span data-ttu-id="96f13-156">a.</span><span class="sxs-lookup"><span data-stu-id="96f13-156">a.</span></span> <span data-ttu-id="96f13-157">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.myabsorb.com/Account/SAML`</span><span class="sxs-lookup"><span data-stu-id="96f13-157">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.myabsorb.com/Account/SAML`</span></span>

    <span data-ttu-id="96f13-158">b.</span><span class="sxs-lookup"><span data-stu-id="96f13-158">b.</span></span> <span data-ttu-id="96f13-159">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.myabsorb.com/Account/SAML`</span><span class="sxs-lookup"><span data-stu-id="96f13-159">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<subdomain>.myabsorb.com/Account/SAML`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="96f13-160">Dessa värden är inte hello verkliga.</span><span class="sxs-lookup"><span data-stu-id="96f13-160">These values are not hello real.</span></span> <span data-ttu-id="96f13-161">Uppdatera dessa värden med hello faktiska identifierare och svars-URL.</span><span class="sxs-lookup"><span data-stu-id="96f13-161">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="96f13-162">Kontakta [upptar LMS klienten supportteamet](https://www.absorblms.com/support) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="96f13-162">Contact [Absorb LMS Client support team](https://www.absorblms.com/support) tooget these values.</span></span> 

4. <span data-ttu-id="96f13-163">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="96f13-163">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_certificate.png) 

6. <span data-ttu-id="96f13-165">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="96f13-165">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-absorblms-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="96f13-167">På hello **upptar LMS Configuration** klickar du på **konfigurera upptar LMS** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="96f13-167">On hello **Absorb LMS Configuration** section, click **Configure Absorb LMS** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="96f13-168">Kopiera hello **Sign-Out Webbadressen och SAML enkel inloggning Service** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="96f13-168">Copy hello **Sign-Out URL and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Upptar LMS konfiguration](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_configure.png) 

8. <span data-ttu-id="96f13-170">Logga in tooyour upptar LMS företagets webbplats som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="96f13-170">In a different web browser window, log in tooyour Absorb LMS company site as an administrator.</span></span>

9. <span data-ttu-id="96f13-171">Klicka på hello **ikon** på hello administratörsgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="96f13-171">Click hello **Account Icon** on hello admin interface.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-absorblms-tutorial/1.png)

10. <span data-ttu-id="96f13-173">Klicka på **portalinställningar**.</span><span class="sxs-lookup"><span data-stu-id="96f13-173">Click **Portal Settings**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-absorblms-tutorial/2.png)
    
11. <span data-ttu-id="96f13-175">Klicka på hello **användare** fliken.</span><span class="sxs-lookup"><span data-stu-id="96f13-175">Click hello **Users** tab.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-absorblms-tutorial/3.png)

12. <span data-ttu-id="96f13-177">Utför följande steg tooaccess hello enkel inloggning konfigurationsfält hello:</span><span class="sxs-lookup"><span data-stu-id="96f13-177">Perform hello following steps tooaccess hello Single Sign-On configuration fields:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-absorblms-tutorial/4.png)

    <span data-ttu-id="96f13-179">a.</span><span class="sxs-lookup"><span data-stu-id="96f13-179">a.</span></span> <span data-ttu-id="96f13-180">Välj lämplig hello **läge**.</span><span class="sxs-lookup"><span data-stu-id="96f13-180">Select hello appropriate **Mode**.</span></span>

    <span data-ttu-id="96f13-181">b.</span><span class="sxs-lookup"><span data-stu-id="96f13-181">b.</span></span> <span data-ttu-id="96f13-182">Öppna hello certifikat som du har hämtat från hello Azure-portalen i anteckningar, ta bort hello **---BEGIN CERTIFICATE---** och **---END CERTIFICATE---** tagga och klistra in hello återstående innehåll i Hej **nyckeln** textruta.</span><span class="sxs-lookup"><span data-stu-id="96f13-182">Open hello Certificate that you have downloaded from hello Azure portal in notepad, remove hello **---BEGIN CERTIFICATE---** and **---END CERTIFICATE---** tag and then paste hello remaining content in hello **Key** textbox.</span></span>
    
    <span data-ttu-id="96f13-183">c.</span><span class="sxs-lookup"><span data-stu-id="96f13-183">c.</span></span> <span data-ttu-id="96f13-184">I hello **Id-egenskapen**, Välj hello lämpligt attribut som du har konfigurerat enligt hello användar-ID i hello Azure AD (till exempel, om hello userprinciplename är valt i Azure AD användarnamn här markeras.)</span><span class="sxs-lookup"><span data-stu-id="96f13-184">In hello **Id Property**, select hello appropriate attribute which you have configured as hello user identifier in hello Azure AD (For example, If hello userprinciplename is selected in Azure AD, then Username would be selected here.)</span></span>

    <span data-ttu-id="96f13-185">d.</span><span class="sxs-lookup"><span data-stu-id="96f13-185">d.</span></span> <span data-ttu-id="96f13-186">I hello **inloggnings-URL**, klistra in hello **”SAML inloggning tjänst-URL för enkel”** värde som du har kopierat från hello **konfigurera inloggning** fönster för hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="96f13-186">In hello **Login URL**, paste hello **“SAML Single Sign-On Service URL”** value you have copied from hello **Configure sign-on** window of hello Azure portal.</span></span>

    <span data-ttu-id="96f13-187">e.</span><span class="sxs-lookup"><span data-stu-id="96f13-187">e.</span></span> <span data-ttu-id="96f13-188">I hello **logga ut URL**, klistra in hello **”Sign-Out URL”** värde som du har kopierat från hello **konfigurera inloggning** fönster för hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="96f13-188">In hello **Logout URL**, paste hello **“Sign-Out URL”** value you have copied from hello **Configure sign-on** window of hello Azure portal.</span></span>

13. <span data-ttu-id="96f13-189">Aktivera **endast tillåta SSO-inloggning**.</span><span class="sxs-lookup"><span data-stu-id="96f13-189">Enable **‘Only Allow SSO Login’**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-absorblms-tutorial/5.png)

14. <span data-ttu-id="96f13-191">Klicka på **”spara”.**</span><span class="sxs-lookup"><span data-stu-id="96f13-191">Click **"Save."**</span></span>

> [!TIP]
> <span data-ttu-id="96f13-192">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="96f13-192">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="96f13-193">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="96f13-193">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="96f13-194">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="96f13-194">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="96f13-195">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="96f13-195">Create an Azure AD test user</span></span>

<span data-ttu-id="96f13-196">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="96f13-196">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="96f13-198">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="96f13-198">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="96f13-199">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="96f13-199">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-absorblms-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="96f13-201">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="96f13-201">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-absorblms-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="96f13-203">Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="96f13-203">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![hello webbinställningar](./media/active-directory-saas-absorblms-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="96f13-205">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="96f13-205">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![hello användardialogrutan](./media/active-directory-saas-absorblms-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="96f13-207">a.</span><span class="sxs-lookup"><span data-stu-id="96f13-207">a.</span></span> <span data-ttu-id="96f13-208">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="96f13-208">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="96f13-209">b.</span><span class="sxs-lookup"><span data-stu-id="96f13-209">b.</span></span> <span data-ttu-id="96f13-210">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="96f13-210">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="96f13-211">c.</span><span class="sxs-lookup"><span data-stu-id="96f13-211">c.</span></span> <span data-ttu-id="96f13-212">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="96f13-212">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="96f13-213">d.</span><span class="sxs-lookup"><span data-stu-id="96f13-213">d.</span></span> <span data-ttu-id="96f13-214">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="96f13-214">Click **Create**.</span></span>

### <a name="create-an-absorb-lms-test-user"></a><span data-ttu-id="96f13-215">Skapa en testanvändare upptar LMS</span><span class="sxs-lookup"><span data-stu-id="96f13-215">Create an Absorb LMS test user</span></span>

<span data-ttu-id="96f13-216">tooenable Azure AD-användare toolog i tooAbsorb LMS, måste de vara etablerade i tooAbsorb LMS.</span><span class="sxs-lookup"><span data-stu-id="96f13-216">tooenable Azure AD users toolog in tooAbsorb LMS, they must be provisioned in tooAbsorb LMS.</span></span>  
<span data-ttu-id="96f13-217">Upptar LMS är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="96f13-217">For Absorb LMS, provisioning is a manual task.</span></span>

<span data-ttu-id="96f13-218">**tooprovision ett användarkonto, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="96f13-218">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="96f13-219">Logga in tooyour upptar LMS företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="96f13-219">Log in tooyour Absorb LMS company site as an administrator.</span></span>

2. <span data-ttu-id="96f13-220">Klicka på **användare** fliken.</span><span class="sxs-lookup"><span data-stu-id="96f13-220">Click **Users** tab.</span></span>

    ![Bjud in personer](./media/active-directory-saas-absorblms-tutorial/absorblms_users.png)

3. <span data-ttu-id="96f13-222">Klicka på **användare** under hello **användare** fliken.</span><span class="sxs-lookup"><span data-stu-id="96f13-222">Click **Users** under hello **Users** tab.</span></span>

    ![Bjud in personer](./media/active-directory-saas-absorblms-tutorial/absorblms_userssub.png)

4.  <span data-ttu-id="96f13-224">Välj **användare** från **Lägg till ny** listrutan.</span><span class="sxs-lookup"><span data-stu-id="96f13-224">Select **User** from **Add New** drop-down.</span></span>

    ![Bjud in personer](./media/active-directory-saas-absorblms-tutorial/absorblms_createuser.png)

5. <span data-ttu-id="96f13-226">På hello **Lägg till användare** utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="96f13-226">On hello **Add User** page, perform hello following steps:</span></span>

    ![Bjud in personer](./media/active-directory-saas-absorblms-tutorial/user.png)

    <span data-ttu-id="96f13-228">a.</span><span class="sxs-lookup"><span data-stu-id="96f13-228">a.</span></span> <span data-ttu-id="96f13-229">I hello **Förnamn** textruta typen hello förnamn som Britta.</span><span class="sxs-lookup"><span data-stu-id="96f13-229">In hello **First Name** textbox, type hello first name like Britta.</span></span>

    <span data-ttu-id="96f13-230">b.</span><span class="sxs-lookup"><span data-stu-id="96f13-230">b.</span></span> <span data-ttu-id="96f13-231">I hello **efternamn** textruta Skriv hello efternamn som Simon.</span><span class="sxs-lookup"><span data-stu-id="96f13-231">In hello **Last Name** textbox, type hello last name like Simon.</span></span>
    
    <span data-ttu-id="96f13-232">c.</span><span class="sxs-lookup"><span data-stu-id="96f13-232">c.</span></span> <span data-ttu-id="96f13-233">I hello **användarnamn** textruta Skriv hello användarnamn som Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="96f13-233">In hello **Username** textbox, type hello user name like Britta Simon.</span></span>

    <span data-ttu-id="96f13-234">d.</span><span class="sxs-lookup"><span data-stu-id="96f13-234">d.</span></span> <span data-ttu-id="96f13-235">I hello **lösenord** textruta hello lösenordstyp av Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="96f13-235">In hello **Password** textbox, type hello password of Britta Simon.</span></span>

    <span data-ttu-id="96f13-236">e.</span><span class="sxs-lookup"><span data-stu-id="96f13-236">e.</span></span> <span data-ttu-id="96f13-237">I hello **Bekräfta lösenord** textruta typen hello samma lösenord.</span><span class="sxs-lookup"><span data-stu-id="96f13-237">In hello **Confirm Password** textbox, type hello same password.</span></span>
    
    <span data-ttu-id="96f13-238">f.</span><span class="sxs-lookup"><span data-stu-id="96f13-238">f.</span></span> <span data-ttu-id="96f13-239">Gör det som **ACTIVE**.</span><span class="sxs-lookup"><span data-stu-id="96f13-239">Make it as **ACTIVE**.</span></span>   

6. <span data-ttu-id="96f13-240">Klicka på **”spara”.**</span><span class="sxs-lookup"><span data-stu-id="96f13-240">Click **"Save."**</span></span>
 
### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="96f13-241">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="96f13-241">Assign hello Azure AD test user</span></span>

<span data-ttu-id="96f13-242">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooAbsorb LMS.</span><span class="sxs-lookup"><span data-stu-id="96f13-242">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAbsorb LMS.</span></span>

![Tilldela hello användarroll][200]

<span data-ttu-id="96f13-244">**tooassign Britta Simon tooAbsorb LMS, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="96f13-244">**tooassign Britta Simon tooAbsorb LMS, perform hello following steps:**</span></span>

1. <span data-ttu-id="96f13-245">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="96f13-245">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="96f13-247">Välj i listan med program hello **upptar LMS**.</span><span class="sxs-lookup"><span data-stu-id="96f13-247">In hello applications list, select **Absorb LMS**.</span></span>

    ![hello upptar LMS länken i listan med program hello](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_app.png) 

3. <span data-ttu-id="96f13-249">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="96f13-249">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202] 

4. <span data-ttu-id="96f13-251">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="96f13-251">Click **Add** button.</span></span> <span data-ttu-id="96f13-252">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="96f13-252">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="96f13-254">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="96f13-254">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="96f13-255">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="96f13-255">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="96f13-256">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="96f13-256">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="96f13-257">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="96f13-257">Test single sign-on</span></span>

<span data-ttu-id="96f13-258">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="96f13-258">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="96f13-259">Klicka på hello upptar LMS panelen i hello åtkomstpanelen, får du automatiskt inloggade tooyour upptar LMS program.</span><span class="sxs-lookup"><span data-stu-id="96f13-259">Click hello Absorb LMS tile in hello Access Panel, you will get automatically signed-on tooyour Absorb LMS application.</span></span> <span data-ttu-id="96f13-260">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="96f13-260">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="96f13-261">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="96f13-261">Additional resources</span></span>

* [<span data-ttu-id="96f13-262">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="96f13-262">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="96f13-263">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="96f13-263">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_203.png

