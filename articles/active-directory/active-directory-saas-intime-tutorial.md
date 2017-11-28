---
title: "Självstudier: Azure Active Directory-integrering med InTime | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och InTime."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d4e2c6e1-ae5d-4d2c-8ffc-1b24534d376a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: jeedes
ms.openlocfilehash: 63652f0f098aeac95e89a2500b46a18440e34698
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-intime"></a><span data-ttu-id="3ae57-103">Självstudier: Azure Active Directory-integrering med InTime</span><span class="sxs-lookup"><span data-stu-id="3ae57-103">Tutorial: Azure Active Directory integration with InTime</span></span>

<span data-ttu-id="3ae57-104">I kursen får du lära dig hur toointegrate InTime med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="3ae57-104">In this tutorial, you learn how toointegrate InTime with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3ae57-105">Integrera InTime med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="3ae57-105">Integrating InTime with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="3ae57-106">Du kan styra i Azure AD som har åtkomst till tooInTime.</span><span class="sxs-lookup"><span data-stu-id="3ae57-106">You can control in Azure AD who has access tooInTime.</span></span>
- <span data-ttu-id="3ae57-107">Du kan låta dina användare tooautomatically get inloggade tooInTime (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="3ae57-107">You can enable your users tooautomatically get signed-on tooInTime (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="3ae57-108">Du kan hantera dina konton i en central plats - hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3ae57-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="3ae57-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3ae57-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3ae57-110">Krav</span><span class="sxs-lookup"><span data-stu-id="3ae57-110">Prerequisites</span></span>

<span data-ttu-id="3ae57-111">tooconfigure Azure AD-integrering med InTime, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="3ae57-111">tooconfigure Azure AD integration with InTime, you need hello following items:</span></span>

- <span data-ttu-id="3ae57-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="3ae57-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3ae57-113">En InTime enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="3ae57-113">A InTime single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3ae57-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="3ae57-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3ae57-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="3ae57-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3ae57-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="3ae57-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3ae57-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3ae57-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3ae57-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="3ae57-118">Scenario description</span></span>
<span data-ttu-id="3ae57-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="3ae57-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3ae57-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="3ae57-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3ae57-121">Att lägga till InTime från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="3ae57-121">Adding InTime from hello gallery</span></span>
2. <span data-ttu-id="3ae57-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3ae57-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-intime-from-hello-gallery"></a><span data-ttu-id="3ae57-123">Att lägga till InTime från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="3ae57-123">Adding InTime from hello gallery</span></span>
<span data-ttu-id="3ae57-124">tooconfigure hello integrering av InTime i Azure AD, behöver du tooadd InTime hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="3ae57-124">tooconfigure hello integration of InTime into Azure AD, you need tooadd InTime from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="3ae57-125">**tooadd InTime från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="3ae57-125">**tooadd InTime from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="3ae57-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="3ae57-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="3ae57-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="3ae57-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="3ae57-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="3ae57-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="3ae57-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3ae57-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="3ae57-133">Skriv i sökrutan hello **InTime**väljer **InTime** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="3ae57-133">In hello search box, type **InTime**, select **InTime** from result panel then click **Add** button tooadd hello application.</span></span>

    ![InTime i hello resultatlistan](./media/active-directory-saas-intime-tutorial/tutorial_intime_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="3ae57-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3ae57-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="3ae57-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med InTime baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="3ae57-136">In this section, you configure and test Azure AD single sign-on with InTime based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="3ae57-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i InTime är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3ae57-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in InTime is tooa user in Azure AD.</span></span> <span data-ttu-id="3ae57-138">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i InTime toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="3ae57-138">In other words, a link relationship between an Azure AD user and hello related user in InTime needs toobe established.</span></span>

<span data-ttu-id="3ae57-139">I InTime, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="3ae57-139">In InTime, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="3ae57-140">tooconfigure och testa Azure AD enkel inloggning med InTime, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="3ae57-140">tooconfigure and test Azure AD single sign-on with InTime, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="3ae57-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="3ae57-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="3ae57-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3ae57-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3ae57-143">**[Skapa en InTime testanvändare](#create-a-intime-test-user)**  -toohave en motsvarighet för Britta Simon i InTime som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="3ae57-143">**[Create a InTime test user](#create-a-intime-test-user)** - toohave a counterpart of Britta Simon in InTime that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="3ae57-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="3ae57-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3ae57-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="3ae57-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="3ae57-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3ae57-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="3ae57-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt InTime program.</span><span class="sxs-lookup"><span data-stu-id="3ae57-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your InTime application.</span></span>

<span data-ttu-id="3ae57-148">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med InTime:**</span><span class="sxs-lookup"><span data-stu-id="3ae57-148">**tooconfigure Azure AD single sign-on with InTime, perform hello following steps:**</span></span>

1. <span data-ttu-id="3ae57-149">I hello Azure-portalen på hello **InTime** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="3ae57-149">In hello Azure portal, on hello **InTime** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="3ae57-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="3ae57-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-intime-tutorial/tutorial_intime_samlbase.png)

3. <span data-ttu-id="3ae57-153">På hello **InTime domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="3ae57-153">On hello **InTime Domain and URLs** section, perform hello following steps:</span></span>

    ![URL: er och inTime domän med enkel inloggning information](./media/active-directory-saas-intime-tutorial/tutorial_intime_url.png)

    <span data-ttu-id="3ae57-155">a.</span><span class="sxs-lookup"><span data-stu-id="3ae57-155">a.</span></span> <span data-ttu-id="3ae57-156">I hello **inloggnings-URL** textruta typen hello URL:`https://intime6.intimesoft.com/mytime/login/login.xhtml`</span><span class="sxs-lookup"><span data-stu-id="3ae57-156">In hello **Sign-on URL** textbox, type hello URL: `https://intime6.intimesoft.com/mytime/login/login.xhtml`</span></span>

    <span data-ttu-id="3ae57-157">b.</span><span class="sxs-lookup"><span data-stu-id="3ae57-157">b.</span></span> <span data-ttu-id="3ae57-158">I hello **identifierare** textruta typen hello URL:`https://auth.intimesoft.com/auth/realms/master`</span><span class="sxs-lookup"><span data-stu-id="3ae57-158">In hello **Identifier** textbox, type hello URL: `https://auth.intimesoft.com/auth/realms/master`</span></span>

4. <span data-ttu-id="3ae57-159">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="3ae57-159">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-intime-tutorial/tutorial_intime_certificate.png) 

5. <span data-ttu-id="3ae57-161">Tillämpningsprogrammet InTime förväntar hello SAML intyg i ett specifikt format, vilket kräver att du tooadd attributet mappningar tooyour SAML-token attribut-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="3ae57-161">Your InTime application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour SAML token attributes configuration.</span></span> <span data-ttu-id="3ae57-162">hello följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="3ae57-162">hello following screenshot shows an example for this.</span></span> <span data-ttu-id="3ae57-163">Hej standardvärdet **användar-ID** är **user.userprincipalname** men InTime förväntar sig den här toobe som mappats till hello användarens e-postadress.</span><span class="sxs-lookup"><span data-stu-id="3ae57-163">hello default value of **User Identifier** is **user.userprincipalname** but InTime expects this toobe mapped with hello user's email address.</span></span> <span data-ttu-id="3ae57-164">Som du kan använda **user.mail** attribut hello listan eller använda hello rätt attribut-värde baserat på din organisationskonfiguration</span><span class="sxs-lookup"><span data-stu-id="3ae57-164">For that you can use **user.mail** attribute from hello list or use hello appropriate attribute value based on your organization configuration</span></span> 

    ![Konfigurera attribut](./media/active-directory-saas-intime-tutorial/tutorial_intime_attribute.png)

6. <span data-ttu-id="3ae57-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="3ae57-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-intime-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="3ae57-168">På hello **InTime Configuration** klickar du på **konfigurera InTime** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="3ae57-168">On hello **InTime Configuration** section, click **Configure InTime** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="3ae57-169">Kopiera hello **Sign-Out URL och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="3ae57-169">Copy hello **Sign-Out URL, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![InTime konfiguration](./media/active-directory-saas-intime-tutorial/tutorial_intime_configure.png) 

8. <span data-ttu-id="3ae57-171">tooconfigure enkel inloggning på **InTime** sida, behöver du toosend hello hämtas **XML-Metadata för**, **Sign-Out URL och SAML inloggning tjänst-URL för enkel** för[InTime supportteamet](mailto:hdollard@intimesoft.com).</span><span class="sxs-lookup"><span data-stu-id="3ae57-171">tooconfigure single sign-on on **InTime** side, you need toosend hello downloaded **Metadata XML**, **Sign-Out URL, and SAML Single Sign-On Service URL** too[InTime support team](mailto:hdollard@intimesoft.com).</span></span> <span data-ttu-id="3ae57-172">De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="3ae57-172">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="3ae57-173">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="3ae57-173">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="3ae57-174">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="3ae57-174">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="3ae57-175">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3ae57-175">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="3ae57-176">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="3ae57-176">Create an Azure AD test user</span></span>

<span data-ttu-id="3ae57-177">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3ae57-177">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="3ae57-179">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="3ae57-179">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="3ae57-180">Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="3ae57-180">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-intime-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="3ae57-182">toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="3ae57-182">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-intime-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="3ae57-184">tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3ae57-184">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello webbinställningar](./media/active-directory-saas-intime-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="3ae57-186">I hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="3ae57-186">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello användardialogrutan](./media/active-directory-saas-intime-tutorial/create_aaduser_04.png)

    <span data-ttu-id="3ae57-188">a.</span><span class="sxs-lookup"><span data-stu-id="3ae57-188">a.</span></span> <span data-ttu-id="3ae57-189">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3ae57-189">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3ae57-190">b.</span><span class="sxs-lookup"><span data-stu-id="3ae57-190">b.</span></span> <span data-ttu-id="3ae57-191">I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3ae57-191">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="3ae57-192">c.</span><span class="sxs-lookup"><span data-stu-id="3ae57-192">c.</span></span> <span data-ttu-id="3ae57-193">Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="3ae57-193">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="3ae57-194">d.</span><span class="sxs-lookup"><span data-stu-id="3ae57-194">d.</span></span> <span data-ttu-id="3ae57-195">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="3ae57-195">Click **Create**.</span></span>
 
### <a name="create-a-intime-test-user"></a><span data-ttu-id="3ae57-196">Skapa en InTime testanvändare</span><span class="sxs-lookup"><span data-stu-id="3ae57-196">Create a InTime test user</span></span>

<span data-ttu-id="3ae57-197">I det här avsnittet skapar du en användare som kallas Britta Simon i InTime.</span><span class="sxs-lookup"><span data-stu-id="3ae57-197">In this section, you create a user called Britta Simon in InTime.</span></span> <span data-ttu-id="3ae57-198">Arbeta med [InTime supportteamet](mailto:hdollard@intimesoft.com) tooadd hello användare i hello InTime plattform.</span><span class="sxs-lookup"><span data-stu-id="3ae57-198">Work with [InTime support team](mailto:hdollard@intimesoft.com) tooadd hello users in hello InTime platform.</span></span> <span data-ttu-id="3ae57-199">Användare måste skapas och aktiveras innan du använder enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="3ae57-199">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="3ae57-200">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="3ae57-200">Assign hello Azure AD test user</span></span>

<span data-ttu-id="3ae57-201">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooInTime.</span><span class="sxs-lookup"><span data-stu-id="3ae57-201">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooInTime.</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="3ae57-203">**tooassign Britta Simon tooInTime utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="3ae57-203">**tooassign Britta Simon tooInTime, perform hello following steps:**</span></span>

1. <span data-ttu-id="3ae57-204">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="3ae57-204">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="3ae57-206">Välj i listan med program hello **InTime**.</span><span class="sxs-lookup"><span data-stu-id="3ae57-206">In hello applications list, select **InTime**.</span></span>

    ![Hej InTime länken i listan med program hello](./media/active-directory-saas-intime-tutorial/tutorial_intime_app.png)  

3. <span data-ttu-id="3ae57-208">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="3ae57-208">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202]

4. <span data-ttu-id="3ae57-210">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="3ae57-210">Click **Add** button.</span></span> <span data-ttu-id="3ae57-211">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3ae57-211">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="3ae57-213">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="3ae57-213">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="3ae57-214">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3ae57-214">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3ae57-215">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3ae57-215">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="3ae57-216">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3ae57-216">Test single sign-on</span></span>

<span data-ttu-id="3ae57-217">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="3ae57-217">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="3ae57-218">När du klickar på hello InTime panelen i Hej åtkomstpanelen, du bör få hello inloggningssidan för InTime programmet.</span><span class="sxs-lookup"><span data-stu-id="3ae57-218">When you click hello InTime tile in hello Access Panel, you should get hello login page of your InTime application.</span></span> <span data-ttu-id="3ae57-219">Klicka på hello **inloggning** knappen, och sedan en serie IdPs ska visas i en lista över knappar.</span><span class="sxs-lookup"><span data-stu-id="3ae57-219">Click hello **Login** button, then a series of IdPs will be displayed on a list of buttons.</span></span> <span data-ttu-id="3ae57-220">Klicka på **IDP namnet** anges av [InTime supportteamet](mailto:hdollard@intimesoft.com) toologin till InTime programmet.</span><span class="sxs-lookup"><span data-stu-id="3ae57-220">click **IDP name** given by [InTime support team](mailto:hdollard@intimesoft.com) toologin into your InTime application.</span></span> <span data-ttu-id="3ae57-221">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="3ae57-221">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="3ae57-222">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="3ae57-222">Additional resources</span></span>

* [<span data-ttu-id="3ae57-223">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3ae57-223">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3ae57-224">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3ae57-224">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-intime-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-intime-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-intime-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-intime-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-intime-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-intime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-intime-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-intime-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-intime-tutorial/tutorial_general_203.png

