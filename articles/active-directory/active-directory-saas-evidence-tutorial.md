---
title: "Självstudier: Azure Active Directory-integrering med Evidence.com | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Evidence.com."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: f9a7cb7c-ff67-40dc-872c-1fa35f9dd03b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: eed98c15dca8a6a77f0b5d407bb40ee0037fccad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-evidencecom"></a><span data-ttu-id="55342-103">Självstudier: Azure Active Directory-integrering med Evidence.com</span><span class="sxs-lookup"><span data-stu-id="55342-103">Tutorial: Azure Active Directory integration with Evidence.com</span></span>

<span data-ttu-id="55342-104">I kursen får du lära dig hur toointegrate Evidence.com med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="55342-104">In this tutorial, you learn how toointegrate Evidence.com with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="55342-105">Integrera Evidence.com med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="55342-105">Integrating Evidence.com with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="55342-106">Du kan styra i Azure AD som har åtkomst till tooEvidence.com.</span><span class="sxs-lookup"><span data-stu-id="55342-106">You can control in Azure AD who has access tooEvidence.com.</span></span>
- <span data-ttu-id="55342-107">Du kan låta dina användare tooautomatically get inloggade tooEvidence.com (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="55342-107">You can enable your users tooautomatically get signed-on tooEvidence.com (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="55342-108">Du kan hantera dina konton i en central plats - hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="55342-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="55342-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="55342-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="55342-110">Krav</span><span class="sxs-lookup"><span data-stu-id="55342-110">Prerequisites</span></span>

<span data-ttu-id="55342-111">tooconfigure Azure AD-integrering med Evidence.com, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="55342-111">tooconfigure Azure AD integration with Evidence.com, you need hello following items:</span></span>

- <span data-ttu-id="55342-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="55342-112">An Azure AD subscription</span></span>
- <span data-ttu-id="55342-113">En Evidence.com enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="55342-113">A Evidence.com single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="55342-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="55342-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="55342-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="55342-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="55342-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="55342-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="55342-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="55342-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="55342-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="55342-118">Scenario description</span></span>
<span data-ttu-id="55342-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="55342-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="55342-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="55342-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="55342-121">Att lägga till Evidence.com från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="55342-121">Adding Evidence.com from hello gallery</span></span>
2. <span data-ttu-id="55342-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="55342-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-evidencecom-from-hello-gallery"></a><span data-ttu-id="55342-123">Att lägga till Evidence.com från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="55342-123">Adding Evidence.com from hello gallery</span></span>
<span data-ttu-id="55342-124">tooconfigure hello integrering av Evidence.com i Azure AD, behöver du tooadd Evidence.com hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="55342-124">tooconfigure hello integration of Evidence.com into Azure AD, you need tooadd Evidence.com from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="55342-125">**tooadd Evidence.com från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="55342-125">**tooadd Evidence.com from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="55342-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="55342-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="55342-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="55342-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="55342-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="55342-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="55342-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="55342-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="55342-133">Skriv i sökrutan hello **Evidence.com**väljer **Evidence.com** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="55342-133">In hello search box, type **Evidence.com**, select **Evidence.com** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Evidence.com i hello resultatlistan](./media/active-directory-saas-evidence-tutorial/tutorial_evidence.com_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="55342-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="55342-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="55342-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Evidence.com baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="55342-136">In this section, you configure and test Azure AD single sign-on with Evidence.com based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="55342-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Evidence.com är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="55342-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Evidence.com is tooa user in Azure AD.</span></span> <span data-ttu-id="55342-138">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Evidence.com toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="55342-138">In other words, a link relationship between an Azure AD user and hello related user in Evidence.com needs toobe established.</span></span>

<span data-ttu-id="55342-139">I Evidence.com, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="55342-139">In Evidence.com, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="55342-140">tooconfigure och testa Azure AD enkel inloggning med Evidence.com, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="55342-140">tooconfigure and test Azure AD single sign-on with Evidence.com, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="55342-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="55342-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="55342-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="55342-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="55342-143">**[Skapa en testanvändare Evidence.com](#create-a-evidencecom-test-user)**  -toohave en motsvarighet för Britta Simon i Evidence.com som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="55342-143">**[Create a Evidence.com test user](#create-a-evidencecom-test-user)** - toohave a counterpart of Britta Simon in Evidence.com that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="55342-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="55342-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="55342-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="55342-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="55342-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="55342-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="55342-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Evidence.com program.</span><span class="sxs-lookup"><span data-stu-id="55342-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Evidence.com application.</span></span>

<span data-ttu-id="55342-148">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Evidence.com:**</span><span class="sxs-lookup"><span data-stu-id="55342-148">**tooconfigure Azure AD single sign-on with Evidence.com, perform hello following steps:**</span></span>

1. <span data-ttu-id="55342-149">I hello Azure-portalen på hello **Evidence.com** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="55342-149">In hello Azure portal, on hello **Evidence.com** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="55342-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="55342-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-evidence-tutorial/tutorial_evidence.com_samlbase.png)

3. <span data-ttu-id="55342-153">På hello **Evidence.com domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="55342-153">On hello **Evidence.com Domain and URLs** section, perform hello following steps:</span></span>

    ![URL: er och evidence.com domän med enkel inloggning information](./media/active-directory-saas-evidence-tutorial/tutorial_evidence.com_url.png)

    <span data-ttu-id="55342-155">a.</span><span class="sxs-lookup"><span data-stu-id="55342-155">a.</span></span> <span data-ttu-id="55342-156">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<yourtenant>.evidence.com`</span><span class="sxs-lookup"><span data-stu-id="55342-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<yourtenant>.evidence.com`</span></span>

    <span data-ttu-id="55342-157">b.</span><span class="sxs-lookup"><span data-stu-id="55342-157">b.</span></span> <span data-ttu-id="55342-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<yourtenant>.evidence.com`</span><span class="sxs-lookup"><span data-stu-id="55342-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<yourtenant>.evidence.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="55342-159">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="55342-159">These values are not real.</span></span> <span data-ttu-id="55342-160">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="55342-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="55342-161">Kontakta [Evidence.com klienten supportteamet](https://communities.taser.com/support/SupportContactUs?typ=LE) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="55342-161">Contact [Evidence.com Client support team](https://communities.taser.com/support/SupportContactUs?typ=LE) tooget these values.</span></span> 

4. <span data-ttu-id="55342-162">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="55342-162">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-evidence-tutorial/tutorial_evidence.com_certificate.png) 

5. <span data-ttu-id="55342-164">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="55342-164">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-evidence-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="55342-166">På hello **Evidence.com Configuration** klickar du på **konfigurera Evidence.com** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="55342-166">On hello **Evidence.com Configuration** section, click **Configure Evidence.com** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="55342-167">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="55342-167">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Evidence.com konfiguration](./media/active-directory-saas-evidence-tutorial/tutorial_evidence.com_configure.png) 

7. <span data-ttu-id="55342-169">I ett separat webbläsarfönster inloggningen tooyour Evidence.com klient som administratör och navigera för**Admin** fliken</span><span class="sxs-lookup"><span data-stu-id="55342-169">In a separate web browser window, login tooyour Evidence.com tenant as an administrator and navigate too**Admin** Tab</span></span>

8. <span data-ttu-id="55342-170">Klicka på **Agency för enkel inloggning**</span><span class="sxs-lookup"><span data-stu-id="55342-170">Click on **Agency Single Sign On**</span></span>

9. <span data-ttu-id="55342-171">Välj **SAML utifrån för enkel inloggning**</span><span class="sxs-lookup"><span data-stu-id="55342-171">Select **SAML Based Single Sign On**</span></span>

10. <span data-ttu-id="55342-172">Kopiera hello **SAML enhets-ID**, **SAML enkel inloggning Tjänstwebbadress** och **Sign-Out URL** värden som visas i hello Azure-portalen och toohello motsvarande fält i Evidence.com.</span><span class="sxs-lookup"><span data-stu-id="55342-172">Copy hello **SAML Entity ID**, **SAML Single Sign-On Service URL** and **Sign-Out URL** values shown in hello Azure portal and toohello corresponding fields in Evidence.com.</span></span>

11. <span data-ttu-id="55342-173">Öppna filen hämtade Certificate(Base64) i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den toohello **säkerhetscertifikat** rutan.</span><span class="sxs-lookup"><span data-stu-id="55342-173">Open your downloaded Certificate(Base64) file in notepad, copy hello content of it into your clipboard, and then paste it toohello **Security Certificate** box.</span></span> 

12. <span data-ttu-id="55342-174">Spara hello konfigurationen i Evidence.com.</span><span class="sxs-lookup"><span data-stu-id="55342-174">Save hello configuration in Evidence.com.</span></span>

> [!TIP]
> <span data-ttu-id="55342-175">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="55342-175">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="55342-176">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="55342-176">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="55342-177">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="55342-177">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="55342-178">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="55342-178">Create an Azure AD test user</span></span>

<span data-ttu-id="55342-179">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="55342-179">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="55342-181">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="55342-181">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="55342-182">Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="55342-182">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-evidence-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="55342-184">toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="55342-184">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-evidence-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="55342-186">tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="55342-186">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello webbinställningar](./media/active-directory-saas-evidence-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="55342-188">I hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="55342-188">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello användardialogrutan](./media/active-directory-saas-evidence-tutorial/create_aaduser_04.png)

    <span data-ttu-id="55342-190">a.</span><span class="sxs-lookup"><span data-stu-id="55342-190">a.</span></span> <span data-ttu-id="55342-191">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="55342-191">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="55342-192">b.</span><span class="sxs-lookup"><span data-stu-id="55342-192">b.</span></span> <span data-ttu-id="55342-193">I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="55342-193">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="55342-194">c.</span><span class="sxs-lookup"><span data-stu-id="55342-194">c.</span></span> <span data-ttu-id="55342-195">Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="55342-195">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="55342-196">d.</span><span class="sxs-lookup"><span data-stu-id="55342-196">d.</span></span> <span data-ttu-id="55342-197">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="55342-197">Click **Create**.</span></span>
 
### <a name="create-a-evidencecom-test-user"></a><span data-ttu-id="55342-198">Skapa en testanvändare Evidence.com</span><span class="sxs-lookup"><span data-stu-id="55342-198">Create a Evidence.com test user</span></span>

<span data-ttu-id="55342-199">För Azure AD-användare toobe kan toosign i, måste de etableras för åtkomst i hello Evidence.com program.</span><span class="sxs-lookup"><span data-stu-id="55342-199">For Azure AD users toobe able toosign in, they must be provisioned for access inside hello Evidence.com application.</span></span> <span data-ttu-id="55342-200">Det här avsnittet beskrivs hur toocreate Azure AD användarkonton i Evidence.com</span><span class="sxs-lookup"><span data-stu-id="55342-200">This section describes how toocreate Azure AD user accounts inside Evidence.com</span></span>

<span data-ttu-id="55342-201">**tooprovision ett användarkonto i Evidence.com:**</span><span class="sxs-lookup"><span data-stu-id="55342-201">**tooprovision a user account in Evidence.com:**</span></span>

1. <span data-ttu-id="55342-202">Logga in på webbplatsen Evidence.com företag som en administratör i ett webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="55342-202">In a web browser window, log into your Evidence.com company site as an administrator.</span></span>

2. <span data-ttu-id="55342-203">Navigera för**Admin** fliken.</span><span class="sxs-lookup"><span data-stu-id="55342-203">Navigate too**Admin** tab.</span></span>

3. <span data-ttu-id="55342-204">Klicka på **lägga till användare**.</span><span class="sxs-lookup"><span data-stu-id="55342-204">Click on **Add User**.</span></span>

4. <span data-ttu-id="55342-205">Klicka på hello **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="55342-205">Click hello **Add** button.</span></span>

5. <span data-ttu-id="55342-206">Hej **e-postadress** av hello läggs användare måste matcha hello användarnamnet för hello användare i Azure AD som du önskar toogive åtkomst.</span><span class="sxs-lookup"><span data-stu-id="55342-206">hello **Email Address** of hello added user must match hello username of hello users in Azure AD who you wish toogive access.</span></span> <span data-ttu-id="55342-207">Om hello användarnamn och e-postadress inte är hello samma värde i din organisation, kan du använda hello **Evidence.com > attribut > enkel inloggning** avsnitt i hello Azure portal toochange hello nameidenitifer skickas tooEvidence.com toobe hello e-postadress.</span><span class="sxs-lookup"><span data-stu-id="55342-207">If hello username and email address are not hello same value in your organization, you can use hello **Evidence.com > Attributes > Single Sign-On** section of hello Azure portal toochange hello nameidenitifer sent tooEvidence.com toobe hello email address.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="55342-208">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="55342-208">Assign hello Azure AD test user</span></span>

<span data-ttu-id="55342-209">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooEvidence.com.</span><span class="sxs-lookup"><span data-stu-id="55342-209">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooEvidence.com.</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="55342-211">**tooassign Britta Simon tooEvidence.com utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="55342-211">**tooassign Britta Simon tooEvidence.com, perform hello following steps:**</span></span>

1. <span data-ttu-id="55342-212">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="55342-212">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="55342-214">Välj i listan med program hello **Evidence.com**.</span><span class="sxs-lookup"><span data-stu-id="55342-214">In hello applications list, select **Evidence.com**.</span></span>

    ![Hej Evidence.com länken i listan med program hello](./media/active-directory-saas-evidence-tutorial/tutorial_evidence.com_app.png)  

3. <span data-ttu-id="55342-216">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="55342-216">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202]

4. <span data-ttu-id="55342-218">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="55342-218">Click **Add** button.</span></span> <span data-ttu-id="55342-219">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="55342-219">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="55342-221">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="55342-221">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="55342-222">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="55342-222">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="55342-223">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="55342-223">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="55342-224">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="55342-224">Test single sign-on</span></span>

<span data-ttu-id="55342-225">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="55342-225">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="55342-226">Du bör få automatiskt inloggade tooyour Evidence.com programmet när du klickar på hello Evidence.com panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="55342-226">When you click hello Evidence.com tile in hello Access Panel, you should get automatically signed-on tooyour Evidence.com application.</span></span>
<span data-ttu-id="55342-227">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="55342-227">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="55342-228">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="55342-228">Additional resources</span></span>

* [<span data-ttu-id="55342-229">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="55342-229">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="55342-230">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="55342-230">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-evidence-tutorial/tutorial_general_203.png

