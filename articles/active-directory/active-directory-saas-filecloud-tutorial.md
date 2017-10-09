---
title: "Självstudier: Azure Active Directory-integrering med FileCloud | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och FileCloud."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: f39f0ddd-b504-4562-971f-77b88d1e75fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: fe58d01f02d6ce99ee9e2f83e7dc72c4434e13b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-filecloud"></a><span data-ttu-id="a8437-103">Självstudier: Azure Active Directory-integrering med FileCloud</span><span class="sxs-lookup"><span data-stu-id="a8437-103">Tutorial: Azure Active Directory integration with FileCloud</span></span>

<span data-ttu-id="a8437-104">I kursen får du lära dig hur toointegrate FileCloud med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="a8437-104">In this tutorial, you learn how toointegrate FileCloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a8437-105">Integrera FileCloud med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="a8437-105">Integrating FileCloud with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="a8437-106">Du kan styra i Azure AD som har åtkomst till tooFileCloud.</span><span class="sxs-lookup"><span data-stu-id="a8437-106">You can control in Azure AD who has access tooFileCloud.</span></span>
- <span data-ttu-id="a8437-107">Du kan låta dina användare tooautomatically get inloggade tooFileCloud (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="a8437-107">You can enable your users tooautomatically get signed-on tooFileCloud (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="a8437-108">Du kan hantera dina konton i en central plats - hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a8437-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="a8437-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a8437-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a8437-110">Krav</span><span class="sxs-lookup"><span data-stu-id="a8437-110">Prerequisites</span></span>

<span data-ttu-id="a8437-111">tooconfigure Azure AD-integrering med FileCloud, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="a8437-111">tooconfigure Azure AD integration with FileCloud, you need hello following items:</span></span>

- <span data-ttu-id="a8437-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="a8437-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a8437-113">En FileCloud enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="a8437-113">A FileCloud single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a8437-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="a8437-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a8437-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="a8437-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a8437-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="a8437-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a8437-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a8437-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a8437-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="a8437-118">Scenario description</span></span>
<span data-ttu-id="a8437-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="a8437-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a8437-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="a8437-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a8437-121">Att lägga till FileCloud från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="a8437-121">Adding FileCloud from hello gallery</span></span>
2. <span data-ttu-id="a8437-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a8437-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-filecloud-from-hello-gallery"></a><span data-ttu-id="a8437-123">Att lägga till FileCloud från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="a8437-123">Adding FileCloud from hello gallery</span></span>
<span data-ttu-id="a8437-124">tooconfigure hello integrering av FileCloud i Azure AD, behöver du tooadd FileCloud hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="a8437-124">tooconfigure hello integration of FileCloud into Azure AD, you need tooadd FileCloud from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="a8437-125">**tooadd FileCloud från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a8437-125">**tooadd FileCloud from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="a8437-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a8437-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="a8437-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="a8437-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="a8437-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="a8437-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="a8437-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a8437-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="a8437-133">Skriv i sökrutan hello **FileCloud**väljer **FileCloud** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="a8437-133">In hello search box, type **FileCloud**, select **FileCloud** from result panel then click **Add** button tooadd hello application.</span></span>

    ![FileCloud i hello resultatlistan](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="a8437-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a8437-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="a8437-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med FileCloud baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="a8437-136">In this section, you configure and test Azure AD single sign-on with FileCloud based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a8437-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i FileCloud är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a8437-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in FileCloud is tooa user in Azure AD.</span></span> <span data-ttu-id="a8437-138">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i FileCloud toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="a8437-138">In other words, a link relationship between an Azure AD user and hello related user in FileCloud needs toobe established.</span></span>

<span data-ttu-id="a8437-139">I FileCloud, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="a8437-139">In FileCloud, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="a8437-140">tooconfigure och testa Azure AD enkel inloggning med FileCloud, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="a8437-140">tooconfigure and test Azure AD single sign-on with FileCloud, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="a8437-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="a8437-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="a8437-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a8437-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a8437-143">**[Skapa en testanvändare FileCloud](#create-a-filecloud-test-user)**  -toohave en motsvarighet för Britta Simon i FileCloud som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="a8437-143">**[Create a FileCloud test user](#create-a-filecloud-test-user)** - toohave a counterpart of Britta Simon in FileCloud that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="a8437-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a8437-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a8437-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="a8437-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="a8437-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a8437-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="a8437-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt FileCloud program.</span><span class="sxs-lookup"><span data-stu-id="a8437-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your FileCloud application.</span></span>

<span data-ttu-id="a8437-148">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med FileCloud:**</span><span class="sxs-lookup"><span data-stu-id="a8437-148">**tooconfigure Azure AD single sign-on with FileCloud, perform hello following steps:**</span></span>

1. <span data-ttu-id="a8437-149">I hello Azure-portalen på hello **FileCloud** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="a8437-149">In hello Azure portal, on hello **FileCloud** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="a8437-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a8437-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_samlbase.png)

3. <span data-ttu-id="a8437-153">På hello **FileCloud domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a8437-153">On hello **FileCloud Domain and URLs** section, perform hello following steps:</span></span>

    ![URL: er och FileCloud domän med enkel inloggning information](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_url.png)

    <span data-ttu-id="a8437-155">a.</span><span class="sxs-lookup"><span data-stu-id="a8437-155">a.</span></span> <span data-ttu-id="a8437-156">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.filecloudhosted.com`</span><span class="sxs-lookup"><span data-stu-id="a8437-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.filecloudhosted.com`</span></span>

    <span data-ttu-id="a8437-157">b.</span><span class="sxs-lookup"><span data-stu-id="a8437-157">b.</span></span> <span data-ttu-id="a8437-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.filecloudhosted.com/simplesaml/module.php/saml/sp/metadata.php/default-sp`</span><span class="sxs-lookup"><span data-stu-id="a8437-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.filecloudhosted.com/simplesaml/module.php/saml/sp/metadata.php/default-sp`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="a8437-159">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="a8437-159">These values are not real.</span></span> <span data-ttu-id="a8437-160">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="a8437-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="a8437-161">Kontakta [FileCloud klienten supportteamet](mailto:support@codelathe.com) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="a8437-161">Contact [FileCloud Client support team](mailto:support@codelathe.com) tooget these values.</span></span>

4. <span data-ttu-id="a8437-162">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="a8437-162">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_certificate.png) 

5. <span data-ttu-id="a8437-164">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="a8437-164">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-filecloud-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="a8437-166">På hello **FileCloud Configuration** klickar du på **konfigurera FileCloud** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="a8437-166">On hello **FileCloud Configuration** section, click **Configure FileCloud** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="a8437-167">Kopiera hello **SAML enhets-ID** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="a8437-167">Copy hello **SAML Entity ID** from hello **Quick Reference section.**</span></span>

    ![FileCloud konfiguration](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_configure.png) 

7. <span data-ttu-id="a8437-169">I en annan webbläsarfönstret, inloggning tooyour FileCloud klient som administratör.</span><span class="sxs-lookup"><span data-stu-id="a8437-169">In a different web browser window, sign-on tooyour FileCloud tenant as an administrator.</span></span>

8. <span data-ttu-id="a8437-170">Klicka på hello vänstra navigeringsfönstret **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="a8437-170">On hello left navigation pane, click **Settings**.</span></span> 
   
    ![Inställningar för avsnittet på App-sida](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_000.png)

9. <span data-ttu-id="a8437-172">Klicka på **SSO** för inställningar.</span><span class="sxs-lookup"><span data-stu-id="a8437-172">Click **SSO** tab on Settings section.</span></span> 
   
    ![Enkel inloggning fliken på App-sida](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_001.png)

10. <span data-ttu-id="a8437-174">Välj **SAML** som **standard SSO typen** på **inställningar för enkel inloggning (SSO)** panelen.</span><span class="sxs-lookup"><span data-stu-id="a8437-174">Select **SAML** as **Default SSO Type** on **Single Sign On (SSO) Settings** panel.</span></span>
   
    ![Enkel inloggning panelen i appen inställningar på klientsidan](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_002.png)

11. <span data-ttu-id="a8437-176">Klistra in **SAML enhets-ID**, som du har kopierat från Azure-portalen i hello **IdP slutpunkt URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="a8437-176">Paste **SAML Entity ID**, which you have copied from Azure portal into hello **IdP End Point URL** textbox.</span></span>

    ![IDP slutet punkt URL: en textruta](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_003.png)

12. <span data-ttu-id="a8437-178">Öppna metadatafilen hämtade i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den toohello **IdP metadata** textruta på **SAML inställningar** panelen.</span><span class="sxs-lookup"><span data-stu-id="a8437-178">Open your downloaded metadata file in notepad, copy hello content of it into your clipboard, and then paste it toohello **IdP Meta Data** textbox on **SAML Settings** panel.</span></span>

    ![IDP Meta dataavsnittet på App-sida](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_004.png)

13. <span data-ttu-id="a8437-180">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="a8437-180">Click **Save** button.</span></span>

> [!TIP]
> <span data-ttu-id="a8437-181">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="a8437-181">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="a8437-182">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="a8437-182">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="a8437-183">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a8437-183">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="a8437-184">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="a8437-184">Create an Azure AD test user</span></span>

<span data-ttu-id="a8437-185">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a8437-185">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="a8437-187">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a8437-187">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="a8437-188">Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="a8437-188">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-filecloud-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="a8437-190">toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="a8437-190">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-filecloud-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="a8437-192">tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a8437-192">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello webbinställningar](./media/active-directory-saas-filecloud-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="a8437-194">I hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a8437-194">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello användardialogrutan](./media/active-directory-saas-filecloud-tutorial/create_aaduser_04.png)

    <span data-ttu-id="a8437-196">a.</span><span class="sxs-lookup"><span data-stu-id="a8437-196">a.</span></span> <span data-ttu-id="a8437-197">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a8437-197">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a8437-198">b.</span><span class="sxs-lookup"><span data-stu-id="a8437-198">b.</span></span> <span data-ttu-id="a8437-199">I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a8437-199">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="a8437-200">c.</span><span class="sxs-lookup"><span data-stu-id="a8437-200">c.</span></span> <span data-ttu-id="a8437-201">Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="a8437-201">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="a8437-202">d.</span><span class="sxs-lookup"><span data-stu-id="a8437-202">d.</span></span> <span data-ttu-id="a8437-203">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="a8437-203">Click **Create**.</span></span>
 
### <a name="create-a-filecloud-test-user"></a><span data-ttu-id="a8437-204">Skapa en testanvändare FileCloud</span><span class="sxs-lookup"><span data-stu-id="a8437-204">Create a FileCloud test user</span></span>

<span data-ttu-id="a8437-205">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i FileCloud.</span><span class="sxs-lookup"><span data-stu-id="a8437-205">hello objective of this section is toocreate a user called Britta Simon in FileCloud.</span></span> <span data-ttu-id="a8437-206">FileCloud stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="a8437-206">FileCloud supports just-in-time provisioning, which is by default enabled.</span></span> <span data-ttu-id="a8437-207">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="a8437-207">There is no action item for you in this section.</span></span> <span data-ttu-id="a8437-208">En ny användare skapas under ett försök tooaccess FileCloud om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="a8437-208">A new user is created during an attempt tooaccess FileCloud if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="a8437-209">Om du behöver toocreate en användare manuellt, måste toocontact hello [FileCloud klienten supportteamet](mailto:support@codelathe.com).</span><span class="sxs-lookup"><span data-stu-id="a8437-209">If you need toocreate a user manually, you need toocontact hello [FileCloud Client support team](mailto:support@codelathe.com).</span></span> 

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="a8437-210">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="a8437-210">Assign hello Azure AD test user</span></span>

<span data-ttu-id="a8437-211">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooFileCloud.</span><span class="sxs-lookup"><span data-stu-id="a8437-211">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooFileCloud.</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="a8437-213">**tooassign Britta Simon tooFileCloud utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a8437-213">**tooassign Britta Simon tooFileCloud, perform hello following steps:**</span></span>

1. <span data-ttu-id="a8437-214">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="a8437-214">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="a8437-216">Välj i listan med program hello **FileCloud**.</span><span class="sxs-lookup"><span data-stu-id="a8437-216">In hello applications list, select **FileCloud**.</span></span>

    ![Hej FileCloud länken i listan med program hello](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_app.png)  

3. <span data-ttu-id="a8437-218">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="a8437-218">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202]

4. <span data-ttu-id="a8437-220">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="a8437-220">Click **Add** button.</span></span> <span data-ttu-id="a8437-221">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a8437-221">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="a8437-223">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="a8437-223">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="a8437-224">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a8437-224">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a8437-225">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a8437-225">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="a8437-226">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a8437-226">Test single sign-on</span></span>

<span data-ttu-id="a8437-227">hello syftet med det här avsnittet är tootest din Azure AD SSO konfiguration av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="a8437-227">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="a8437-228">Du bör få automatiskt inloggade tooyour FileCloud programmet när du klickar på hello FileCloud panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="a8437-228">When you click hello FileCloud tile in hello Access Panel, you should get automatically signed-on tooyour FileCloud application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a8437-229">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="a8437-229">Additional resources</span></span>

* [<span data-ttu-id="a8437-230">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a8437-230">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a8437-231">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a8437-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_203.png

