---
title: "Självstudier: Azure Active Directory-integrering med PurelyHR | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och PurelyHR."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 86a9c454-596d-4902-829a-fe126708f739
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2017
ms.author: jeedes
ms.openlocfilehash: bdc1748ed650cff36b1ef7d7330dd2a17b3bc760
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-purelyhr"></a><span data-ttu-id="58eeb-103">Självstudier: Azure Active Directory-integrering med PurelyHR</span><span class="sxs-lookup"><span data-stu-id="58eeb-103">Tutorial: Azure Active Directory integration with PurelyHR</span></span>

<span data-ttu-id="58eeb-104">I kursen får du lära dig hur toointegrate PurelyHR med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="58eeb-104">In this tutorial, you learn how toointegrate PurelyHR with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="58eeb-105">Integrera PurelyHR med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="58eeb-105">Integrating PurelyHR with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="58eeb-106">Du kan styra i Azure AD som har åtkomst till tooPurelyHR</span><span class="sxs-lookup"><span data-stu-id="58eeb-106">You can control in Azure AD who has access tooPurelyHR</span></span>
- <span data-ttu-id="58eeb-107">Du kan aktivera din användare tooautomatically get inloggade tooPurelyHR (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="58eeb-107">You can enable your users tooautomatically get signed-on tooPurelyHR (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="58eeb-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="58eeb-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="58eeb-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="58eeb-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="58eeb-110">Krav</span><span class="sxs-lookup"><span data-stu-id="58eeb-110">Prerequisites</span></span>

<span data-ttu-id="58eeb-111">tooconfigure Azure AD-integrering med PurelyHR, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="58eeb-111">tooconfigure Azure AD integration with PurelyHR, you need hello following items:</span></span>

- <span data-ttu-id="58eeb-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="58eeb-112">An Azure AD subscription</span></span>
- <span data-ttu-id="58eeb-113">En PurelyHR enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="58eeb-113">A PurelyHR single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="58eeb-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="58eeb-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="58eeb-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="58eeb-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="58eeb-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="58eeb-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="58eeb-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="58eeb-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="58eeb-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="58eeb-118">Scenario description</span></span>
<span data-ttu-id="58eeb-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="58eeb-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="58eeb-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="58eeb-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="58eeb-121">Att lägga till PurelyHR från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="58eeb-121">Adding PurelyHR from hello gallery</span></span>
2. <span data-ttu-id="58eeb-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="58eeb-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-purelyhr-from-hello-gallery"></a><span data-ttu-id="58eeb-123">Att lägga till PurelyHR från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="58eeb-123">Adding PurelyHR from hello gallery</span></span>
<span data-ttu-id="58eeb-124">tooconfigure hello integrering av PurelyHR i Azure AD, behöver du tooadd PurelyHR hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="58eeb-124">tooconfigure hello integration of PurelyHR into Azure AD, you need tooadd PurelyHR from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="58eeb-125">**tooadd PurelyHR från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="58eeb-125">**tooadd PurelyHR from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="58eeb-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="58eeb-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="58eeb-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="58eeb-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="58eeb-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="58eeb-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="58eeb-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="58eeb-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="58eeb-133">Skriv i sökrutan hello **PurelyHR**.</span><span class="sxs-lookup"><span data-stu-id="58eeb-133">In hello search box, type **PurelyHR**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_search.png)

5. <span data-ttu-id="58eeb-135">Markera hello resultat på panelen **PurelyHR**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="58eeb-135">In hello results panel, select **PurelyHR**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="58eeb-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="58eeb-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="58eeb-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med PurelyHR baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="58eeb-138">In this section, you configure and test Azure AD single sign-on with PurelyHR based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="58eeb-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i PurelyHR är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="58eeb-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in PurelyHR is tooa user in Azure AD.</span></span> <span data-ttu-id="58eeb-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i PurelyHR toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="58eeb-140">In other words, a link relationship between an Azure AD user and hello related user in PurelyHR needs toobe established.</span></span>

<span data-ttu-id="58eeb-141">I PurelyHR, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="58eeb-141">In PurelyHR, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="58eeb-142">tooconfigure och testa Azure AD enkel inloggning med PurelyHR, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="58eeb-142">tooconfigure and test Azure AD single sign-on with PurelyHR, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="58eeb-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="58eeb-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="58eeb-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="58eeb-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="58eeb-145">**[Skapa en testanvändare PurelyHR](#creating-a-purelyhr-test-user)**  -toohave en motsvarighet för Britta Simon i PurelyHR som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="58eeb-145">**[Creating a PurelyHR test user](#creating-a-purelyhr-test-user)** - toohave a counterpart of Britta Simon in PurelyHR that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="58eeb-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="58eeb-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="58eeb-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="58eeb-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="58eeb-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="58eeb-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="58eeb-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt PurelyHR program.</span><span class="sxs-lookup"><span data-stu-id="58eeb-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your PurelyHR application.</span></span>

<span data-ttu-id="58eeb-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med PurelyHR:**</span><span class="sxs-lookup"><span data-stu-id="58eeb-150">**tooconfigure Azure AD single sign-on with PurelyHR, perform hello following steps:**</span></span>

1. <span data-ttu-id="58eeb-151">I hello Azure-portalen på hello **PurelyHR** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="58eeb-151">In hello Azure portal, on hello **PurelyHR** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="58eeb-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="58eeb-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_samlbase.png)

3. <span data-ttu-id="58eeb-155">På hello **PurelyHR domän och URL: er** avsnittet, utför följande steg om du inte vill tooconfigure hello programmet hello **IDP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="58eeb-155">On hello **PurelyHR Domain and URLs** section, perform hello following steps if you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_url.png)
   
    <span data-ttu-id="58eeb-157">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyID>.purelyhr.com/sso-consume`</span><span class="sxs-lookup"><span data-stu-id="58eeb-157">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<companyID>.purelyhr.com/sso-consume`</span></span>

4. <span data-ttu-id="58eeb-158">Kontrollera **visa avancerade inställningar för URL: en**, om du inte vill tooconfigure hello programmet i **SP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="58eeb-158">Check **Show advanced URL settings**, if you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_url1.png)
    
    <span data-ttu-id="58eeb-160">I hello **inloggnings-URL** textruta hello TYPVÄRDE med hello följande mönster:`https://<companyID>.purelyhr.com/sso-initiate`</span><span class="sxs-lookup"><span data-stu-id="58eeb-160">In hello **Sign-on URL** textbox, type hello value using hello following pattern: `https://<companyID>.purelyhr.com/sso-initiate`</span></span>
     
    > [!NOTE]
    > <span data-ttu-id="58eeb-161">Dessa värden är inte hello verkliga.</span><span class="sxs-lookup"><span data-stu-id="58eeb-161">These values are not hello real.</span></span> <span data-ttu-id="58eeb-162">Uppdatera dessa värden med hello faktiska Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="58eeb-162">Update these values with hello actual Reply URL and Sign-On URL.</span></span> <span data-ttu-id="58eeb-163">Kontakta [PurelyHR klienten supportteamet](http://support.purelyhr.com/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="58eeb-163">Contact [PurelyHR Client support team](http://support.purelyhr.com/) tooget these values.</span></span> 

5. <span data-ttu-id="58eeb-164">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="58eeb-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_certificate.png) 

6. <span data-ttu-id="58eeb-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="58eeb-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-purelyhr-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="58eeb-168">På hello **PurelyHR Configuration** klickar du på **konfigurera PurelyHR** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="58eeb-168">On hello **PurelyHR Configuration** section, click **Configure PurelyHR** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="58eeb-169">Kopiera hello **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="58eeb-169">Copy hello **SAML Entity ID and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_configure.png) 

8. <span data-ttu-id="58eeb-171">tooconfigure enkel inloggning på **PurelyHR** , på webbplatsen för inloggning tootheir som administratör.</span><span class="sxs-lookup"><span data-stu-id="58eeb-171">tooconfigure single sign-on on **PurelyHR** side, login tootheir website as an administrator.</span></span>

9. <span data-ttu-id="58eeb-172">Öppna hello **instrumentpanelen** från hello alternativ i hello verktygsfältet och på **SSO-inställningarna**.</span><span class="sxs-lookup"><span data-stu-id="58eeb-172">Open hello **Dashboard** from hello options in hello toolbar and click **SSO Settings**.</span></span>

10. <span data-ttu-id="58eeb-173">Klistra in hello värden i hello rutor som beskrivs nedan-</span><span class="sxs-lookup"><span data-stu-id="58eeb-173">Paste hello values in hello boxes as described below-</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-purelyhr-tutorial/purelyhr-dashboard-sso-settings.png)    

    <span data-ttu-id="58eeb-175">a.</span><span class="sxs-lookup"><span data-stu-id="58eeb-175">a.</span></span> <span data-ttu-id="58eeb-176">Öppna hello **Certificate(Bas64)** hämtas från hello Azure-portalen i anteckningar och kopiera hello Certifikatvärde.</span><span class="sxs-lookup"><span data-stu-id="58eeb-176">Open hello **Certificate(Bas64)** downloaded from hello Azure portal in notepad and copy hello certificate value.</span></span> <span data-ttu-id="58eeb-177">Klistra in hello kopieras värdet till hello **X.509-certifikat** rutan.</span><span class="sxs-lookup"><span data-stu-id="58eeb-177">Paste hello copied value into hello **X.509 Certificate** box.</span></span>

    <span data-ttu-id="58eeb-178">b.</span><span class="sxs-lookup"><span data-stu-id="58eeb-178">b.</span></span> <span data-ttu-id="58eeb-179">I hello **Idp utfärdar-URL** klistra in hello **SAML enhets-ID** kopieras från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="58eeb-179">In hello **Idp Issuer URL** box, paste hello **SAML Entity ID** copied from hello Azure portal.</span></span>

    <span data-ttu-id="58eeb-180">c.</span><span class="sxs-lookup"><span data-stu-id="58eeb-180">c.</span></span> <span data-ttu-id="58eeb-181">I hello **Idp slutpunkts-URL** klistra in hello **SAML inloggning tjänst-URL för enkel** kopieras från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="58eeb-181">In hello **Idp Endpoint URL** box, paste hello **SAML Single Sign-On Service URL** copied from hello Azure portal.</span></span> 

    <span data-ttu-id="58eeb-182">d.</span><span class="sxs-lookup"><span data-stu-id="58eeb-182">d.</span></span> <span data-ttu-id="58eeb-183">Kontrollera hello **skapa automatiskt användare** kryssrutan tooenable automatisk användaretablering i PurelyHR.</span><span class="sxs-lookup"><span data-stu-id="58eeb-183">Check hello **Auto-Create Users** checkbox tooenable automatic user provisioning in PurelyHR.</span></span>

    <span data-ttu-id="58eeb-184">e.</span><span class="sxs-lookup"><span data-stu-id="58eeb-184">e.</span></span> <span data-ttu-id="58eeb-185">Klicka på **spara ändringar** toosave hello inställningar.</span><span class="sxs-lookup"><span data-stu-id="58eeb-185">Click **Save Changes** toosave hello settings.</span></span>

> [!TIP]
> <span data-ttu-id="58eeb-186">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="58eeb-186">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="58eeb-187">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="58eeb-187">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="58eeb-188">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="58eeb-188">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="58eeb-189">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="58eeb-189">Creating an Azure AD test user</span></span>
<span data-ttu-id="58eeb-190">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="58eeb-190">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="58eeb-192">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="58eeb-192">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="58eeb-193">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="58eeb-193">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-purelyhr-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="58eeb-195">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="58eeb-195">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-purelyhr-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="58eeb-197">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="58eeb-197">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-purelyhr-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="58eeb-199">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="58eeb-199">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-purelyhr-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="58eeb-201">a.</span><span class="sxs-lookup"><span data-stu-id="58eeb-201">a.</span></span> <span data-ttu-id="58eeb-202">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="58eeb-202">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="58eeb-203">b.</span><span class="sxs-lookup"><span data-stu-id="58eeb-203">b.</span></span> <span data-ttu-id="58eeb-204">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="58eeb-204">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="58eeb-205">c.</span><span class="sxs-lookup"><span data-stu-id="58eeb-205">c.</span></span> <span data-ttu-id="58eeb-206">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="58eeb-206">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="58eeb-207">d.</span><span class="sxs-lookup"><span data-stu-id="58eeb-207">d.</span></span> <span data-ttu-id="58eeb-208">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="58eeb-208">Click **Create**.</span></span>
 
### <a name="creating-a-purelyhr-test-user"></a><span data-ttu-id="58eeb-209">Skapa en testanvändare PurelyHR</span><span class="sxs-lookup"><span data-stu-id="58eeb-209">Creating a PurelyHR test user</span></span>

<span data-ttu-id="58eeb-210">tooenable Azure AD-användare toolog i tooPurelyHR, måste de etableras i PurelyHR.</span><span class="sxs-lookup"><span data-stu-id="58eeb-210">tooenable Azure AD users toolog in tooPurelyHR, they must be provisioned into PurelyHR.</span></span> <span data-ttu-id="58eeb-211">Etablering är en automatisk uppgift i PurelyHR, och inga manuella steg krävs när automatisk användaretablering är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="58eeb-211">In PurelyHR, provisioning is an automatic task and no manual steps are required when automatic user provisioning is enabled.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="58eeb-212">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="58eeb-212">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="58eeb-213">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooPurelyHR.</span><span class="sxs-lookup"><span data-stu-id="58eeb-213">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooPurelyHR.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="58eeb-215">**tooassign Britta Simon tooPurelyHR utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="58eeb-215">**tooassign Britta Simon tooPurelyHR, perform hello following steps:**</span></span>

1. <span data-ttu-id="58eeb-216">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="58eeb-216">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="58eeb-218">Välj i listan med program hello **PurelyHR**.</span><span class="sxs-lookup"><span data-stu-id="58eeb-218">In hello applications list, select **PurelyHR**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-purelyhr-tutorial/tutorial_purelyhr_app.png) 

3. <span data-ttu-id="58eeb-220">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="58eeb-220">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="58eeb-222">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="58eeb-222">Click **Add** button.</span></span> <span data-ttu-id="58eeb-223">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="58eeb-223">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="58eeb-225">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="58eeb-225">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="58eeb-226">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="58eeb-226">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="58eeb-227">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="58eeb-227">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="58eeb-228">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="58eeb-228">Testing single sign-on</span></span>

<span data-ttu-id="58eeb-229">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="58eeb-229">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="58eeb-230">Klicka på hello upptar LMS panelen i hello åtkomstpanelen, får du automatiskt inloggade tooyour upptar LMS program.</span><span class="sxs-lookup"><span data-stu-id="58eeb-230">Click hello Absorb LMS tile in hello Access Panel, you get automatically signed-on tooyour Absorb LMS application.</span></span>

<span data-ttu-id="58eeb-231">Mer information om hello åtkomstpanelen finns i.</span><span class="sxs-lookup"><span data-stu-id="58eeb-231">For more information about hello Access Panel, see.</span></span> <span data-ttu-id="58eeb-232">[Introduktion toohello åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="58eeb-232">[Introduction toohello Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="58eeb-233">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="58eeb-233">Additional resources</span></span>

* [<span data-ttu-id="58eeb-234">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="58eeb-234">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="58eeb-235">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="58eeb-235">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-purelyhr-tutorial/tutorial_general_203.png

