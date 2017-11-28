---
title: "Självstudier: Azure Active Directory-integrering med UNIFI | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och UNIFI."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e1f49ee4-d2d4-4a82-9baf-0587ca1f20f6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: af7cc1167b5c0cff2a1f4cdaa8a2b93f5a718f81
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-unifi"></a><span data-ttu-id="9bf4e-103">Självstudier: Azure Active Directory-integrering med UNIFI</span><span class="sxs-lookup"><span data-stu-id="9bf4e-103">Tutorial: Azure Active Directory integration with UNIFI</span></span>

<span data-ttu-id="9bf4e-104">I kursen får du lära dig hur toointegrate UNIFI med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="9bf4e-104">In this tutorial, you learn how toointegrate UNIFI with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9bf4e-105">Integrera UNIFI med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="9bf4e-105">Integrating UNIFI with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="9bf4e-106">Du kan styra i Azure AD som har åtkomst till tooUNIFI</span><span class="sxs-lookup"><span data-stu-id="9bf4e-106">You can control in Azure AD who has access tooUNIFI</span></span>
- <span data-ttu-id="9bf4e-107">Du kan aktivera din användare tooautomatically get inloggade tooUNIFI (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="9bf4e-107">You can enable your users tooautomatically get signed-on tooUNIFI (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9bf4e-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="9bf4e-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="9bf4e-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9bf4e-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9bf4e-110">Krav</span><span class="sxs-lookup"><span data-stu-id="9bf4e-110">Prerequisites</span></span>

<span data-ttu-id="9bf4e-111">tooconfigure Azure AD-integrering med UNIFI, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="9bf4e-111">tooconfigure Azure AD integration with UNIFI, you need hello following items:</span></span>

- <span data-ttu-id="9bf4e-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="9bf4e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9bf4e-113">En UNIFI enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="9bf4e-113">A UNIFI single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9bf4e-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9bf4e-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="9bf4e-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9bf4e-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9bf4e-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9bf4e-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9bf4e-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="9bf4e-118">Scenario description</span></span>
<span data-ttu-id="9bf4e-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9bf4e-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="9bf4e-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9bf4e-121">Att lägga till UNIFI från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="9bf4e-121">Adding UNIFI from hello gallery</span></span>
2. <span data-ttu-id="9bf4e-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9bf4e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-unifi-from-hello-gallery"></a><span data-ttu-id="9bf4e-123">Att lägga till UNIFI från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="9bf4e-123">Adding UNIFI from hello gallery</span></span>
<span data-ttu-id="9bf4e-124">tooconfigure hello integrering av UNIFI i Azure AD, behöver du tooadd UNIFI hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-124">tooconfigure hello integration of UNIFI into Azure AD, you need tooadd UNIFI from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="9bf4e-125">**tooadd UNIFI från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="9bf4e-125">**tooadd UNIFI from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="9bf4e-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9bf4e-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="9bf4e-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="9bf4e-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="9bf4e-133">Skriv i sökrutan hello **UNIFI**.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-133">In hello search box, type **UNIFI**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_search.png)

5. <span data-ttu-id="9bf4e-135">Markera hello resultat på panelen **UNIFI**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-135">In hello results panel, select **UNIFI**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9bf4e-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9bf4e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9bf4e-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med UNIFI baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-138">In this section, you configure and test Azure AD single sign-on with UNIFI based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="9bf4e-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i UNIFI är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in UNIFI is tooa user in Azure AD.</span></span> <span data-ttu-id="9bf4e-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i UNIFI toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-140">In other words, a link relationship between an Azure AD user and hello related user in UNIFI needs toobe established.</span></span>

<span data-ttu-id="9bf4e-141">I UNIFI, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-141">In UNIFI, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="9bf4e-142">tooconfigure och testa Azure AD enkel inloggning med UNIFI, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="9bf4e-142">tooconfigure and test Azure AD single sign-on with UNIFI, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="9bf4e-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="9bf4e-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9bf4e-145">**[Skapa en UNIFI testanvändare](#creating-a-unifi-test-user)**  -toohave en motsvarighet för Britta Simon i UNIFI som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-145">**[Creating a UNIFI test user](#creating-a-unifi-test-user)** - toohave a counterpart of Britta Simon in UNIFI that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="9bf4e-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9bf4e-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9bf4e-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9bf4e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9bf4e-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt UNIFI program.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your UNIFI application.</span></span>

<span data-ttu-id="9bf4e-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med UNIFI:**</span><span class="sxs-lookup"><span data-stu-id="9bf4e-150">**tooconfigure Azure AD single sign-on with UNIFI, perform hello following steps:**</span></span>

1. <span data-ttu-id="9bf4e-151">I hello Azure-portalen på hello **UNIFI** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-151">In hello Azure portal, on hello **UNIFI** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="9bf4e-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_samlbase.png)

3. <span data-ttu-id="9bf4e-155">På hello **UNIFI domän och URL: er** om du vill tooconfigure hello programmet i **IDP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="9bf4e-155">On hello **UNIFI Domain and URLs** section, If you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_url1.png)

    <span data-ttu-id="9bf4e-157">I hello **identifierare** textruta hello TYPVÄRDE:`INVIEWlabs`</span><span class="sxs-lookup"><span data-stu-id="9bf4e-157">In hello **Identifier** textbox, type hello value: `INVIEWlabs`</span></span> 

4. <span data-ttu-id="9bf4e-158">Kontrollera **visa avancerade inställningar för URL: en**, om du inte vill tooconfigure hello programmet i **SP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="9bf4e-158">Check **Show advanced URL settings**, If you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_url2.png)

    <span data-ttu-id="9bf4e-160">I hello **inloggnings-URL** textruta typen hello URL:`https://app.discoverunifi.com/login`</span><span class="sxs-lookup"><span data-stu-id="9bf4e-160">In hello **Sign-on URL** textbox, type hello URL: `https://app.discoverunifi.com/login`</span></span>

5. <span data-ttu-id="9bf4e-161">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-161">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_certificate.png) 

6. <span data-ttu-id="9bf4e-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-unifi-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="9bf4e-165">På hello **UNIFI Configuration** klickar du på **konfigurera UNIFI** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-165">On hello **UNIFI Configuration** section, click **Configure UNIFI** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="9bf4e-166">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="9bf4e-166">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_configure.png)

8. <span data-ttu-id="9bf4e-168">Inloggning i en annan webbläsarfönster tooyour **UNIFI** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-168">In a different web browser window, sign on tooyour **UNIFI** company site as administrator.</span></span>

9. <span data-ttu-id="9bf4e-169">Klicka på hello **användare**.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-169">Click on hello **Users**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-unifi-tutorial/app1.png) 

10. <span data-ttu-id="9bf4e-171">Klicka på hello **lägga till nya identitetsleverantör**.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-171">Click on hello **Add New Identity Provider**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-unifi-tutorial/app2.png)

11. <span data-ttu-id="9bf4e-173">I hello **lägga till identitetsleverantör** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9bf4e-173">In hello **Add Identity Provider** section, perform hello following steps:</span></span>   

    ![Konfigurera enkel inloggning](./media/active-directory-saas-unifi-tutorial/app3.png) 

    <span data-ttu-id="9bf4e-175">a.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-175">a.</span></span> <span data-ttu-id="9bf4e-176">I hello **providernamn** textruta hello-typnamn för hello identitetsleverantör...</span><span class="sxs-lookup"><span data-stu-id="9bf4e-176">In hello **Provider Name** textbox, type hello name of hello Identity Provider..</span></span>

    <span data-ttu-id="9bf4e-177">b.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-177">b.</span></span> <span data-ttu-id="9bf4e-178">I hello hello **providern URL** textruta klistra in hello **SAML inloggning tjänst-URL för enkel** -värde som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-178">In hello hello **Provider URL** textbox paste hello **SAML Single Sign-On Service URL** value, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="9bf4e-179">c.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-179">c.</span></span> <span data-ttu-id="9bf4e-180">Öppna hello certifikat som du har hämtat från hello Azure-portalen i anteckningar, ta bort hello **---BEGIN CERTIFICATE---** och **---END CERTIFICATE---** tagga och klistra in hello återstående innehåll i Hej **certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-180">Open hello Certificate that you have downloaded from hello Azure portal in notepad, remove hello **---BEGIN CERTIFICATE---** and **---END CERTIFICATE---** tag and then paste hello remaining content in hello **Certificate** textbox.</span></span>

    <span data-ttu-id="9bf4e-181">d.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-181">d.</span></span> <span data-ttu-id="9bf4e-182">Välj hello **är som standard Provider** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-182">Select hello **is Default Provider** checkbox.</span></span>

> [!TIP]
> <span data-ttu-id="9bf4e-183">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="9bf4e-183">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="9bf4e-184">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-184">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="9bf4e-185">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9bf4e-185">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9bf4e-186">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="9bf4e-186">Creating an Azure AD test user</span></span>
<span data-ttu-id="9bf4e-187">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-187">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="9bf4e-189">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="9bf4e-189">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="9bf4e-190">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-190">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-unifi-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9bf4e-192">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-192">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-unifi-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9bf4e-194">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-194">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-unifi-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9bf4e-196">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9bf4e-196">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-unifi-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9bf4e-198">a.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-198">a.</span></span> <span data-ttu-id="9bf4e-199">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-199">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9bf4e-200">b.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-200">b.</span></span> <span data-ttu-id="9bf4e-201">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-201">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9bf4e-202">c.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-202">c.</span></span> <span data-ttu-id="9bf4e-203">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-203">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="9bf4e-204">d.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-204">d.</span></span> <span data-ttu-id="9bf4e-205">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-205">Click **Create**.</span></span>
 
### <a name="creating-a-unifi-test-user"></a><span data-ttu-id="9bf4e-206">Skapa en testanvändare UNIFI</span><span class="sxs-lookup"><span data-stu-id="9bf4e-206">Creating a UNIFI test user</span></span>

<span data-ttu-id="9bf4e-207">I det här avsnittet skapar du en användare som kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-207">In this section, you create a user called Britta Simon.</span></span> <span data-ttu-id="9bf4e-208">**UNIFI** stöder automatisk användaretablering, så inga manuella steg krävs.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-208">**UNIFI** supports automatic user provisioning so no manual steps are required.</span></span> <span data-ttu-id="9bf4e-209">Användare skapas automatiskt efter en lyckad autentisering från hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-209">Users are created automatically after successful authentication from hello Azure AD.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="9bf4e-210">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="9bf4e-210">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="9bf4e-211">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooUNIFI.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-211">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooUNIFI.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="9bf4e-213">**tooassign Britta Simon tooUNIFI utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="9bf4e-213">**tooassign Britta Simon tooUNIFI, perform hello following steps:**</span></span>

1. <span data-ttu-id="9bf4e-214">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-214">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="9bf4e-216">Välj i listan med program hello **UNIFI**.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-216">In hello applications list, select **UNIFI**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_app.png) 

3. <span data-ttu-id="9bf4e-218">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-218">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="9bf4e-220">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-220">Click **Add** button.</span></span> <span data-ttu-id="9bf4e-221">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-221">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="9bf4e-223">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-223">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="9bf4e-224">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-224">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9bf4e-225">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-225">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="9bf4e-226">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9bf4e-226">Testing single sign-on</span></span>

<span data-ttu-id="9bf4e-227">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-227">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="9bf4e-228">Du bör få automatiskt inloggade tooyour UNIFI programmet när du klickar på hello UNIFI panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="9bf4e-228">When you click hello UNIFI tile in hello Access Panel, you should get automatically signed-on tooyour UNIFI application.</span></span>
<span data-ttu-id="9bf4e-229">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="9bf4e-229">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="9bf4e-230">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="9bf4e-230">Additional resources</span></span>

* [<span data-ttu-id="9bf4e-231">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9bf4e-231">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9bf4e-232">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9bf4e-232">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_203.png

