---
title: "Självstudier: Azure Active Directory-integrering med RunMyProcess | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och RunMyProcess."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d31f7395-048b-4a61-9505-5acf9fc68d9b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: f02acda015aeb8d131d8e3ef88bf50c4e8e94750
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-runmyprocess"></a><span data-ttu-id="b7a1e-103">Självstudier: Azure Active Directory-integrering med RunMyProcess</span><span class="sxs-lookup"><span data-stu-id="b7a1e-103">Tutorial: Azure Active Directory integration with RunMyProcess</span></span>

<span data-ttu-id="b7a1e-104">I kursen får du lära dig hur toointegrate RunMyProcess med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="b7a1e-104">In this tutorial, you learn how toointegrate RunMyProcess with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b7a1e-105">Integrera RunMyProcess med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="b7a1e-105">Integrating RunMyProcess with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="b7a1e-106">Du kan styra i Azure AD som har åtkomst till tooRunMyProcess</span><span class="sxs-lookup"><span data-stu-id="b7a1e-106">You can control in Azure AD who has access tooRunMyProcess</span></span>
- <span data-ttu-id="b7a1e-107">Du kan aktivera din användare tooautomatically get inloggade tooRunMyProcess (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="b7a1e-107">You can enable your users tooautomatically get signed-on tooRunMyProcess (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b7a1e-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="b7a1e-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="b7a1e-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b7a1e-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b7a1e-110">Krav</span><span class="sxs-lookup"><span data-stu-id="b7a1e-110">Prerequisites</span></span>

<span data-ttu-id="b7a1e-111">tooconfigure Azure AD-integrering med RunMyProcess, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="b7a1e-111">tooconfigure Azure AD integration with RunMyProcess, you need hello following items:</span></span>

- <span data-ttu-id="b7a1e-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="b7a1e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b7a1e-113">En RunMyProcess enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="b7a1e-113">A RunMyProcess single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b7a1e-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b7a1e-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="b7a1e-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b7a1e-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b7a1e-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här:[utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b7a1e-117">If you don't have an Azure AD trial environment, you can get a one-month trial here:[Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b7a1e-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="b7a1e-118">Scenario description</span></span>
<span data-ttu-id="b7a1e-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b7a1e-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="b7a1e-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b7a1e-121">Att lägga till RunMyProcess från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="b7a1e-121">Adding RunMyProcess from hello gallery</span></span>
2. <span data-ttu-id="b7a1e-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b7a1e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-runmyprocess-from-hello-gallery"></a><span data-ttu-id="b7a1e-123">Att lägga till RunMyProcess från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="b7a1e-123">Adding RunMyProcess from hello gallery</span></span>
<span data-ttu-id="b7a1e-124">tooconfigure hello integrering av RunMyProcess i Azure AD, behöver du tooadd RunMyProcess hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-124">tooconfigure hello integration of RunMyProcess into Azure AD, you need tooadd RunMyProcess from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="b7a1e-125">**tooadd RunMyProcess från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b7a1e-125">**tooadd RunMyProcess from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="b7a1e-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b7a1e-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="b7a1e-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="b7a1e-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="b7a1e-133">Skriv i sökrutan hello **RunMyProcess**.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-133">In hello search box, type **RunMyProcess**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_search.png)

5. <span data-ttu-id="b7a1e-135">Markera hello resultat på panelen **RunMyProcess**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-135">In hello results panel, select **RunMyProcess**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b7a1e-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b7a1e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b7a1e-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med RunMyProcess baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-138">In this section, you configure and test Azure AD single sign-on with RunMyProcess based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b7a1e-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i RunMyProcess är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in RunMyProcess is tooa user in Azure AD.</span></span> <span data-ttu-id="b7a1e-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i RunMyProcess toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-140">In other words, a link relationship between an Azure AD user and hello related user in RunMyProcess needs toobe established.</span></span>

<span data-ttu-id="b7a1e-141">I RunMyProcess, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-141">In RunMyProcess, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="b7a1e-142">tooconfigure och testa Azure AD enkel inloggning med RunMyProcess, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="b7a1e-142">tooconfigure and test Azure AD single sign-on with RunMyProcess, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="b7a1e-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="b7a1e-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b7a1e-145">**[Skapa en testanvändare RunMyProcess](#creating-a-runmyprocess-test-user)**  -toohave en motsvarighet för Britta Simon i RunMyProcess som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-145">**[Creating a RunMyProcess test user](#creating-a-runmyprocess-test-user)** - toohave a counterpart of Britta Simon in RunMyProcess that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="b7a1e-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b7a1e-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b7a1e-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b7a1e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b7a1e-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt RunMyProcess program.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your RunMyProcess application.</span></span>

<span data-ttu-id="b7a1e-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med RunMyProcess:**</span><span class="sxs-lookup"><span data-stu-id="b7a1e-150">**tooconfigure Azure AD single sign-on with RunMyProcess, perform hello following steps:**</span></span>

1. <span data-ttu-id="b7a1e-151">I hello Azure-portalen på hello **RunMyProcess** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-151">In hello Azure portal, on hello **RunMyProcess** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="b7a1e-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_samlbase.png)

3. <span data-ttu-id="b7a1e-155">På hello **RunMyProcess domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b7a1e-155">On hello **RunMyProcess Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_url.png)

    <span data-ttu-id="b7a1e-157">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://live.runmyprocess.com/live/<tenant id>`</span><span class="sxs-lookup"><span data-stu-id="b7a1e-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://live.runmyprocess.com/live/<tenant id>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b7a1e-158">hello-värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-158">hello value is not real.</span></span> <span data-ttu-id="b7a1e-159">Hello uppdateringsvärde med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-159">Update hello value with hello actual Sign-On URL.</span></span> <span data-ttu-id="b7a1e-160">Kontakta [RunMyProcess klienten supportteamet](mailto:support@runmyprocess.com) tooget hello värde.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-160">Contact [RunMyProcess Client support team](mailto:support@runmyprocess.com) tooget hello value.</span></span> 

4. <span data-ttu-id="b7a1e-161">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-161">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_certificate.png) 

5. <span data-ttu-id="b7a1e-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="b7a1e-165">På hello **RunMyProcess Configuration** klickar du på **konfigurera RunMyProcess** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-165">On hello **RunMyProcess Configuration** section, click **Configure RunMyProcess** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="b7a1e-166">Kopiera hello **Sign-Out URL och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="b7a1e-166">Copy hello **Sign-Out URL, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_configure.png) 

7. <span data-ttu-id="b7a1e-168">I en annan webbläsarfönstret, inloggning tooyour RunMyProcess klient som administratör.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-168">In a different web browser window, sign-on tooyour RunMyProcess tenant as an administrator.</span></span>

8. <span data-ttu-id="b7a1e-169">I det vänstra navigeringsfönstret klickar du på **konto** och välj **Configuration**.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-169">In left navigation panel, click **Account** and select **Configuration**.</span></span>
   
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_001.png)

9. <span data-ttu-id="b7a1e-171">Gå för**autentiseringsmetod** avsnittet och utföra nedanstående steg:</span><span class="sxs-lookup"><span data-stu-id="b7a1e-171">Go too**Authentication method** section and perform below steps:</span></span>
   
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_002.png)

    <span data-ttu-id="b7a1e-173">a.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-173">a.</span></span> <span data-ttu-id="b7a1e-174">Som **metoden**väljer **SSO med Samlv2**.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-174">As **Method**, select **SSO with Samlv2**.</span></span> 

    <span data-ttu-id="b7a1e-175">b.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-175">b.</span></span> <span data-ttu-id="b7a1e-176">I hello **SSO omdirigering** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-176">In hello **SSO redirect** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="b7a1e-177">c.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-177">c.</span></span> <span data-ttu-id="b7a1e-178">I hello **logga ut omdirigering** textruta klistra in hello värdet för **Sign-Out URL**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-178">In hello **Logout redirect** textbox, paste hello value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="b7a1e-179">d.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-179">d.</span></span> <span data-ttu-id="b7a1e-180">I hello **Format för namn-Id** textruta hello TYPVÄRDE av **identifierare namnformat** som **urn: oasis: namn: tc: SAML:1.1:nameid-format: e-postadress**.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-180">In hello **Name Id Format** textbox, type hello value of **Name Identifier Format** as **urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress**.</span></span>

    <span data-ttu-id="b7a1e-181">e.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-181">e.</span></span> <span data-ttu-id="b7a1e-182">Kopiera hello innehållet hello hämtat certifikatfilen och klistra in den i hello **certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-182">Copy hello content of hello downloaded certificate file and then paste it into hello **Certificate** textbox.</span></span> 
 
    <span data-ttu-id="b7a1e-183">f.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-183">f.</span></span> <span data-ttu-id="b7a1e-184">Klicka på **spara** ikon.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-184">Click **Save** icon.</span></span>

> [!TIP]
> <span data-ttu-id="b7a1e-185">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="b7a1e-185">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="b7a1e-186">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-186">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="b7a1e-187">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b7a1e-187">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b7a1e-188">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="b7a1e-188">Creating an Azure AD test user</span></span>
<span data-ttu-id="b7a1e-189">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-189">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="b7a1e-191">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b7a1e-191">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="b7a1e-192">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-192">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b7a1e-194">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-194">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b7a1e-196">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-196">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b7a1e-198">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b7a1e-198">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b7a1e-200">a.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-200">a.</span></span> <span data-ttu-id="b7a1e-201">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-201">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b7a1e-202">b.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-202">b.</span></span> <span data-ttu-id="b7a1e-203">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-203">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b7a1e-204">c.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-204">c.</span></span> <span data-ttu-id="b7a1e-205">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-205">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="b7a1e-206">d.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-206">d.</span></span> <span data-ttu-id="b7a1e-207">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-207">Click **Create**.</span></span>
 
### <a name="creating-a-runmyprocess-test-user"></a><span data-ttu-id="b7a1e-208">Skapa en testanvändare RunMyProcess</span><span class="sxs-lookup"><span data-stu-id="b7a1e-208">Creating a RunMyProcess test user</span></span>

<span data-ttu-id="b7a1e-209">I ordning tooenable Azure AD-användare toolog i tooRunMyProcess, måste de etableras i RunMyProcess.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-209">In order tooenable Azure AD users toolog in tooRunMyProcess, they must be provisioned into RunMyProcess.</span></span> <span data-ttu-id="b7a1e-210">Hello gäller RunMyProcess är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-210">In hello case of RunMyProcess, provisioning is a manual task.</span></span>

<span data-ttu-id="b7a1e-211">**tooprovision ett användarkonto, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="b7a1e-211">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="b7a1e-212">Logga in tooyour RunMyProcess företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-212">Log in tooyour RunMyProcess company site as an administrator.</span></span>

2. <span data-ttu-id="b7a1e-213">Klicka på **konto** och välj **användare** i vänstra navigeringsfönstret och klicka på **ny användare**.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-213">Click **Account** and select **Users** in left navigation panel, then click **New User**.</span></span>
   
    <span data-ttu-id="b7a1e-214">![Ny användare](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_003.png "ny användare")</span><span class="sxs-lookup"><span data-stu-id="b7a1e-214">![New User](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_003.png "New User")</span></span>

3. <span data-ttu-id="b7a1e-215">I hello **användarinställningar** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b7a1e-215">In hello **User Settings** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="b7a1e-216">![Profilen](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_004.png "profil")</span><span class="sxs-lookup"><span data-stu-id="b7a1e-216">![Profile](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_004.png "Profile")</span></span> 
  
    <span data-ttu-id="b7a1e-217">a.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-217">a.</span></span> <span data-ttu-id="b7a1e-218">Typen hello **namn** och **e-post** av ett giltigt Azure AD-kontot som du vill använda tooprovision hello relaterade textrutor.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-218">Type hello **Name** and **E-mail** of a valid Azure AD account you want tooprovision into hello related textboxes.</span></span> 

    <span data-ttu-id="b7a1e-219">b.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-219">b.</span></span> <span data-ttu-id="b7a1e-220">Välj en **IDE språk**, **språk**, och **profil**.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-220">Select an **IDE language**, **Language**, and **Profile**.</span></span> 

    <span data-ttu-id="b7a1e-221">c.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-221">c.</span></span> <span data-ttu-id="b7a1e-222">Välj **skicka konto skapa e-toome**.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-222">Select **Send account creation e-mail toome**.</span></span> 

    <span data-ttu-id="b7a1e-223">d.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-223">d.</span></span> <span data-ttu-id="b7a1e-224">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-224">Click **Save**.</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="b7a1e-225">Du kan använda något annat RunMyProcess användarens konto skapas verktyg eller API: er som tillhandahålls av RunMyProcess tooprovision Azure Active Directory användarkonton.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-225">You can use any other RunMyProcess user account creation tools or APIs provided by RunMyProcess tooprovision Azure Active Directory user accounts.</span></span> 
    > 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="b7a1e-226">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="b7a1e-226">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="b7a1e-227">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooRunMyProcess.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-227">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooRunMyProcess.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="b7a1e-229">**tooassign Britta Simon tooRunMyProcess utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b7a1e-229">**tooassign Britta Simon tooRunMyProcess, perform hello following steps:**</span></span>

1. <span data-ttu-id="b7a1e-230">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-230">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="b7a1e-232">Välj i listan med program hello **RunMyProcess**.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-232">In hello applications list, select **RunMyProcess**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_app.png) 

3. <span data-ttu-id="b7a1e-234">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-234">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="b7a1e-236">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-236">Click **Add** button.</span></span> <span data-ttu-id="b7a1e-237">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-237">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="b7a1e-239">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-239">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="b7a1e-240">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-240">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b7a1e-241">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-241">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b7a1e-242">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b7a1e-242">Testing single sign-on</span></span>

<span data-ttu-id="b7a1e-243">hello syftet med det här avsnittet är tootest din Azure AD SSO konfiguration av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-243">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="b7a1e-244">Du bör få automatiskt inloggade tooyour RunMyProcess programmet när du klickar på hello RunMyProcess panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="b7a1e-244">When you click hello RunMyProcess tile in hello Access Panel, you should get automatically signed-on tooyour RunMyProcess application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b7a1e-245">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="b7a1e-245">Additional resources</span></span>

* [<span data-ttu-id="b7a1e-246">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b7a1e-246">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b7a1e-247">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b7a1e-247">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_203.png

