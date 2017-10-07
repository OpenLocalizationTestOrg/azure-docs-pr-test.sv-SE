---
title: "Självstudier: Azure Active Directory-integrering med Jitbit Helpdesk | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Jitbit Helpdesk."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 15ce27d4-0621-4103-8a34-e72c98d72ec3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 0f6c837bbb910c1e84f7ed9da676c5ab40f75338
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-jitbit-helpdesk"></a><span data-ttu-id="6a3c4-103">Självstudier: Azure Active Directory-integrering med Jitbit Helpdesk</span><span class="sxs-lookup"><span data-stu-id="6a3c4-103">Tutorial: Azure Active Directory integration with Jitbit Helpdesk</span></span>

<span data-ttu-id="6a3c4-104">I kursen får du lära dig hur toointegrate Jitbit Helpdesk med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="6a3c4-104">In this tutorial, you learn how toointegrate Jitbit Helpdesk with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6a3c4-105">Integrera Jitbit Helpdesk med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="6a3c4-105">Integrating Jitbit Helpdesk with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="6a3c4-106">Du kan styra i Azure AD som har åtkomst tooJitbit supportavdelningen</span><span class="sxs-lookup"><span data-stu-id="6a3c4-106">You can control in Azure AD who has access tooJitbit Helpdesk</span></span>
- <span data-ttu-id="6a3c4-107">Du kan aktivera din användare tooautomatically get inloggade tooJitbit supportavdelningen (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="6a3c4-107">You can enable your users tooautomatically get signed-on tooJitbit Helpdesk (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="6a3c4-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="6a3c4-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="6a3c4-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6a3c4-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6a3c4-110">Krav</span><span class="sxs-lookup"><span data-stu-id="6a3c4-110">Prerequisites</span></span>

<span data-ttu-id="6a3c4-111">tooconfigure Azure AD-integrering med Jitbit Helpdesk måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="6a3c4-111">tooconfigure Azure AD integration with Jitbit Helpdesk, you need hello following items:</span></span>

- <span data-ttu-id="6a3c4-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="6a3c4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6a3c4-113">En Jitbit Helpdesk enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="6a3c4-113">A Jitbit Helpdesk single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6a3c4-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6a3c4-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="6a3c4-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6a3c4-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6a3c4-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6a3c4-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6a3c4-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="6a3c4-118">Scenario description</span></span>
<span data-ttu-id="6a3c4-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6a3c4-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="6a3c4-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6a3c4-121">Att lägga till Jitbit Helpdesk från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="6a3c4-121">Adding Jitbit Helpdesk from hello gallery</span></span>
2. <span data-ttu-id="6a3c4-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6a3c4-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-jitbit-helpdesk-from-hello-gallery"></a><span data-ttu-id="6a3c4-123">Att lägga till Jitbit Helpdesk från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="6a3c4-123">Adding Jitbit Helpdesk from hello gallery</span></span>
<span data-ttu-id="6a3c4-124">tooconfigure hello integrering av Jitbit Helpdesk i Azure AD, behöver du tooadd Jitbit Helpdesk hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-124">tooconfigure hello integration of Jitbit Helpdesk into Azure AD, you need tooadd Jitbit Helpdesk from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="6a3c4-125">**tooadd Jitbit Helpdesk från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="6a3c4-125">**tooadd Jitbit Helpdesk from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="6a3c4-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="6a3c4-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="6a3c4-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="6a3c4-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="6a3c4-133">Skriv i sökrutan hello **Jitbit Helpdesk**.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-133">In hello search box, type **Jitbit Helpdesk**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_search.png)

5. <span data-ttu-id="6a3c4-135">Markera hello resultat på panelen **Jitbit Helpdesk**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-135">In hello results panel, select **Jitbit Helpdesk**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="6a3c4-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6a3c4-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="6a3c4-138">Du konfigurera och testa Azure AD enkel inloggning med Jitbit Helpdesk baserat på en testanvändare som kallas ”Britta Simon” i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-138">In this section, you configure and test Azure AD single sign-on with Jitbit Helpdesk based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="6a3c4-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Jitbit Helpdesk är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Jitbit Helpdesk is tooa user in Azure AD.</span></span> <span data-ttu-id="6a3c4-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Jitbit Helpdesk toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-140">In other words, a link relationship between an Azure AD user and hello related user in Jitbit Helpdesk needs toobe established.</span></span>

<span data-ttu-id="6a3c4-141">Tilldela hello värdet för hello i Jitbit Helpdesk **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-141">In Jitbit Helpdesk, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="6a3c4-142">tooconfigure och testa Azure AD enkel inloggning med Jitbit Helpdesk, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="6a3c4-142">tooconfigure and test Azure AD single sign-on with Jitbit Helpdesk, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="6a3c4-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="6a3c4-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6a3c4-145">**[Skapa en testanvändare Jitbit Helpdesk](#creating-a-jitbit-helpdesk-test-user)**  -toohave en motsvarighet för Britta Simon i Jitbit Helpdesk som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-145">**[Creating a Jitbit Helpdesk test user](#creating-a-jitbit-helpdesk-test-user)** - toohave a counterpart of Britta Simon in Jitbit Helpdesk that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="6a3c4-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6a3c4-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="6a3c4-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6a3c4-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="6a3c4-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Jitbit Helpdesk.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Jitbit Helpdesk application.</span></span>

<span data-ttu-id="6a3c4-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Jitbit Helpdesk:**</span><span class="sxs-lookup"><span data-stu-id="6a3c4-150">**tooconfigure Azure AD single sign-on with Jitbit Helpdesk, perform hello following steps:**</span></span>

1. <span data-ttu-id="6a3c4-151">I hello Azure-portalen på hello **Jitbit Helpdesk** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-151">In hello Azure portal, on hello **Jitbit Helpdesk** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="6a3c4-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_samlbase.png)

3. <span data-ttu-id="6a3c4-155">På hello **Jitbit supportavdelningen domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="6a3c4-155">On hello **Jitbit Helpdesk Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_url.png)

    <span data-ttu-id="6a3c4-157">a.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-157">a.</span></span> <span data-ttu-id="6a3c4-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:</span><span class="sxs-lookup"><span data-stu-id="6a3c4-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern:</span></span> 
    | |     
    | ----------------------------------------|
    | `https://<hostname>/helpdesk/User/Login`|
    | `https://<tenant-name>.Jitbit.com`|
    | |
    
    > [!NOTE] 
    > <span data-ttu-id="6a3c4-159">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-159">This value is not real.</span></span> <span data-ttu-id="6a3c4-160">Uppdatera det här värdet med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-160">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="6a3c4-161">Kontakta [Jitbit supportavdelningen klienten supportteamet](https://www.jitbit.com/support/) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-161">Contact [Jitbit Helpdesk Client support team](https://www.jitbit.com/support/) tooget this value.</span></span> 
    
    <span data-ttu-id="6a3c4-162">b.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-162">b.</span></span>  <span data-ttu-id="6a3c4-163">I hello **identifierare** textruta Skriv en URL som följande:`https://www.jitbit.com/web-helpdesk/`</span><span class="sxs-lookup"><span data-stu-id="6a3c4-163">In hello **Identifier** textbox, type a URL as following: `https://www.jitbit.com/web-helpdesk/`</span></span>

    
 


4. <span data-ttu-id="6a3c4-164">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_certificate.png) 

5. <span data-ttu-id="6a3c4-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="6a3c4-168">På hello **Jitbit supportavdelningen Configuration** klickar du på **konfigurera Jitbit supportavdelningen** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-168">On hello **Jitbit Helpdesk Configuration** section, click **Configure Jitbit Helpdesk** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="6a3c4-169">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="6a3c4-169">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_configure.png) 

7. <span data-ttu-id="6a3c4-171">Logga in på webbplatsen Jitbit Helpdesk företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-171">In a different web browser window, log into your Jitbit Helpdesk company site as an administrator.</span></span>

8. <span data-ttu-id="6a3c4-172">Klicka i hello verktygsfältet hello längst upp **Administration**.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-172">In hello toolbar on hello top, click **Administration**.</span></span>
   
    <span data-ttu-id="6a3c4-173">![Administration](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777681.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="6a3c4-173">![Administration](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777681.png "Administration")</span></span>

9. <span data-ttu-id="6a3c4-174">Klicka på **allmänna inställningar**.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-174">Click **General settings**.</span></span>
   
    <span data-ttu-id="6a3c4-175">![Användare, företag och behörigheter](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777680.png "användare, företag och behörigheter")</span><span class="sxs-lookup"><span data-stu-id="6a3c4-175">![Users, companies, and permissions](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777680.png "Users, companies, and permissions")</span></span>

10. <span data-ttu-id="6a3c4-176">I hello **autentiseringsinställningar** konfigurationen och utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="6a3c4-176">In hello **Authentication settings** configuration section, perform hello following steps:</span></span>
   
    <span data-ttu-id="6a3c4-177">![Autentiseringsinställningar](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777683.png "autentiseringsinställningar")</span><span class="sxs-lookup"><span data-stu-id="6a3c4-177">![Authentication settings](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777683.png "Authentication settings")</span></span>
    
    <span data-ttu-id="6a3c4-178">a.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-178">a.</span></span> <span data-ttu-id="6a3c4-179">Välj **aktivera SAML 2.0 enkel inloggning**, toosign i med enkel inloggning (SSO), **OneLogin**.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-179">Select **Enable SAML 2.0 single sign on**, toosign in using Single Sign-On (SSO), with **OneLogin**.</span></span>

    <span data-ttu-id="6a3c4-180">b.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-180">b.</span></span> <span data-ttu-id="6a3c4-181">I hello **slutpunkts-URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-181">In hello **EndPoint URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="6a3c4-182">c.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-182">c.</span></span> <span data-ttu-id="6a3c4-183">Öppna din **Base64-** kodat certifikat i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den toohello **X.509-certifikat** textruta</span><span class="sxs-lookup"><span data-stu-id="6a3c4-183">Open your **base-64** encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it toohello **X.509 Certificate** textbox</span></span>

    <span data-ttu-id="6a3c4-184">d.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-184">d.</span></span> <span data-ttu-id="6a3c4-185">Klicka på **spara ändringar**.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-185">Click **Save changes**.</span></span>

> [!TIP]
> <span data-ttu-id="6a3c4-186">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="6a3c4-186">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="6a3c4-187">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-187">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="6a3c4-188">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="6a3c4-188">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="6a3c4-189">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="6a3c4-189">Creating an Azure AD test user</span></span>
<span data-ttu-id="6a3c4-190">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-190">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="6a3c4-192">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="6a3c4-192">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="6a3c4-193">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-193">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jitbit-helpdesk-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="6a3c4-195">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-195">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jitbit-helpdesk-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="6a3c4-197">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-197">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jitbit-helpdesk-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="6a3c4-199">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="6a3c4-199">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jitbit-helpdesk-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="6a3c4-201">a.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-201">a.</span></span> <span data-ttu-id="6a3c4-202">I hello **namn** textruta typnamn som **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-202">In hello **Name** textbox, type name as **BrittaSimon**.</span></span>

    <span data-ttu-id="6a3c4-203">b.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-203">b.</span></span> <span data-ttu-id="6a3c4-204">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-204">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="6a3c4-205">c.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-205">c.</span></span> <span data-ttu-id="6a3c4-206">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-206">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="6a3c4-207">d.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-207">d.</span></span> <span data-ttu-id="6a3c4-208">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-208">Click **Create**.</span></span>
 
### <a name="creating-a-jitbit-helpdesk-test-user"></a><span data-ttu-id="6a3c4-209">Skapa en testanvändare Jitbit Helpdesk</span><span class="sxs-lookup"><span data-stu-id="6a3c4-209">Creating a Jitbit Helpdesk test user</span></span>

<span data-ttu-id="6a3c4-210">I ordning tooenable Azure AD-användare toolog till Jitbit Helpdesk, måste de etableras i Jitbit Helpdesk.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-210">In order tooenable Azure AD users toolog into Jitbit Helpdesk, they must be provisioned into Jitbit Helpdesk.</span></span>  <span data-ttu-id="6a3c4-211">Hello gäller Jitbit Helpdesk är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-211">In hello case of Jitbit Helpdesk, provisioning is a manual task.</span></span>

<span data-ttu-id="6a3c4-212">**tooprovision ett användarkonto, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="6a3c4-212">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="6a3c4-213">Logga in tooyour **Jitbit Helpdesk** klient.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-213">Log in tooyour **Jitbit Helpdesk** tenant.</span></span>

2. <span data-ttu-id="6a3c4-214">Hello-menyn överst hello **Administration**.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-214">In hello menu on hello top, click **Administration**.</span></span>
   
    <span data-ttu-id="6a3c4-215">![Administration](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777681.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="6a3c4-215">![Administration](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777681.png "Administration")</span></span>

3. <span data-ttu-id="6a3c4-216">Klicka på **användare och behörigheter**.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-216">Click **Users, companies and permissions**.</span></span>
   
    <span data-ttu-id="6a3c4-217">![Användare, företag och behörigheter](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777682.png "användare, företag och behörigheter")</span><span class="sxs-lookup"><span data-stu-id="6a3c4-217">![Users, companies, and permissions](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777682.png "Users, companies, and permissions")</span></span>

4. <span data-ttu-id="6a3c4-218">Klicka på **Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-218">Click **Add user**.</span></span>
   
    <span data-ttu-id="6a3c4-219">![Lägg till användare](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777685.png "Lägg till användare")</span><span class="sxs-lookup"><span data-stu-id="6a3c4-219">![Add user](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777685.png "Add user")</span></span>
   
5. <span data-ttu-id="6a3c4-220">I hello Skapa avsnittet, skriver du hello data på hello Azure AD-konto som du vill tooprovision på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="6a3c4-220">In hello Create section, type hello data of hello Azure AD account you want tooprovision as follows:</span></span>

    <span data-ttu-id="6a3c4-221">![Skapa](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777686.png "skapa")</span><span class="sxs-lookup"><span data-stu-id="6a3c4-221">![Create](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777686.png "Create")</span></span>
   
   <span data-ttu-id="6a3c4-222">a.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-222">a.</span></span> <span data-ttu-id="6a3c4-223">I hello **användarnamn** textruta typen **BrittaSimon**, hello användarnamn som hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-223">In hello **Username** textbox, type **BrittaSimon**, hello user name as in hello Azure portal.</span></span>

   <span data-ttu-id="6a3c4-224">b.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-224">b.</span></span> <span data-ttu-id="6a3c4-225">I hello **e-post** textruta e-post för hello användare som  **BrittaSimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="6a3c4-225">In hello **Email** textbox, type email of hello user like **BrittaSimon@contoso.com**.</span></span>

   <span data-ttu-id="6a3c4-226">c.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-226">c.</span></span> <span data-ttu-id="6a3c4-227">I hello **Förnamn** textruta Ange först namnet på hello användaren som **Britta**.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-227">In hello **First Name** textbox, type first name of hello user like **Britta**.</span></span>

   <span data-ttu-id="6a3c4-228">d.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-228">d.</span></span> <span data-ttu-id="6a3c4-229">I hello **efternamn** textruta Skriv Efternamn för hello användare som **Simon**.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-229">In hello **Last Name** textbox, type last name of hello user like **Simon**.</span></span>
   
   <span data-ttu-id="6a3c4-230">e.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-230">e.</span></span> <span data-ttu-id="6a3c4-231">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-231">Click **Create**.</span></span>

>[!NOTE]
><span data-ttu-id="6a3c4-232">Du kan använda något annat Jitbit Helpdesk användarens konto skapas verktyg eller API: er som tillhandahålls av Jitbit Helpdesk tooprovision användarkonton i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-232">You can use any other Jitbit Helpdesk user account creation tools or APIs provided by Jitbit Helpdesk tooprovision Azure AD user accounts.</span></span>
> 
        

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="6a3c4-233">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="6a3c4-233">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="6a3c4-234">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooJitbit supportavdelningen.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-234">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooJitbit Helpdesk.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="6a3c4-236">**tooassign Britta Simon tooJitbit supportavdelningen, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="6a3c4-236">**tooassign Britta Simon tooJitbit Helpdesk, perform hello following steps:**</span></span>

1. <span data-ttu-id="6a3c4-237">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-237">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="6a3c4-239">Välj i listan med program hello **Jitbit Helpdesk**.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-239">In hello applications list, select **Jitbit Helpdesk**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_app.png) 

3. <span data-ttu-id="6a3c4-241">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-241">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="6a3c4-243">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-243">Click **Add** button.</span></span> <span data-ttu-id="6a3c4-244">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-244">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="6a3c4-246">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-246">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="6a3c4-247">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-247">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6a3c4-248">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-248">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="6a3c4-249">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6a3c4-249">Testing single sign-on</span></span>

<span data-ttu-id="6a3c4-250">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-250">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="6a3c4-251">Du bör få inloggningssidan för Jitbit Helpdesk program när du klickar på hello Jitbit Helpdesk panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="6a3c4-251">When you click hello Jitbit Helpdesk tile in hello Access Panel, you should get login page of Jitbit Helpdesk application.</span></span>
<span data-ttu-id="6a3c4-252">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="6a3c4-252">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6a3c4-253">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="6a3c4-253">Additional resources</span></span>

* [<span data-ttu-id="6a3c4-254">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6a3c4-254">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6a3c4-255">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="6a3c4-255">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_203.png

