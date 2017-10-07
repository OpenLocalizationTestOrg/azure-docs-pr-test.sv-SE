---
title: "Självstudier: Azure Active Directory-integrering med Rally programvara | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Rally programvaran och Azure Active Directory."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ba25fade-e152-42dd-8377-a30bbc48c3ed
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: jeedes
ms.openlocfilehash: c75c8b98ce7fab19964c13de5ad7e19ef3ebd0e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-rally-software"></a><span data-ttu-id="f25b0-103">Självstudier: Azure Active Directory-integrering med Rally programvara</span><span class="sxs-lookup"><span data-stu-id="f25b0-103">Tutorial: Azure Active Directory integration with Rally Software</span></span>

<span data-ttu-id="f25b0-104">I kursen får du lära dig hur toointegrate Rally programvara med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="f25b0-104">In this tutorial, you learn how toointegrate Rally Software with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f25b0-105">Integrera Rally programvara med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="f25b0-105">Integrating Rally Software with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="f25b0-106">Du kan styra i Azure AD som har åtkomst tooRally programvara.</span><span class="sxs-lookup"><span data-stu-id="f25b0-106">You can control in Azure AD who has access tooRally Software.</span></span>
- <span data-ttu-id="f25b0-107">Du kan låta dina användare tooautomatically get inloggade tooRally programvara (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="f25b0-107">You can enable your users tooautomatically get signed-on tooRally Software (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="f25b0-108">Du kan hantera dina konton i en central plats - hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f25b0-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="f25b0-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f25b0-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f25b0-110">Krav</span><span class="sxs-lookup"><span data-stu-id="f25b0-110">Prerequisites</span></span>

<span data-ttu-id="f25b0-111">tooconfigure Azure AD-integrering med Rally programvara, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="f25b0-111">tooconfigure Azure AD integration with Rally Software, you need hello following items:</span></span>

- <span data-ttu-id="f25b0-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="f25b0-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f25b0-113">En Rally programvara enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="f25b0-113">A Rally Software single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f25b0-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="f25b0-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f25b0-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="f25b0-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f25b0-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="f25b0-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f25b0-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f25b0-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f25b0-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="f25b0-118">Scenario description</span></span>
<span data-ttu-id="f25b0-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="f25b0-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f25b0-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="f25b0-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f25b0-121">Lägga till Rally programvara från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="f25b0-121">Adding Rally Software from hello gallery</span></span>
2. <span data-ttu-id="f25b0-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f25b0-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-rally-software-from-hello-gallery"></a><span data-ttu-id="f25b0-123">Lägga till Rally programvara från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="f25b0-123">Adding Rally Software from hello gallery</span></span>
<span data-ttu-id="f25b0-124">tooconfigure hello integrering av Rally programvara i Azure AD, behöver du tooadd Rally programvara från hello galleriet tooyour lista över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="f25b0-124">tooconfigure hello integration of Rally Software into Azure AD, you need tooadd Rally Software from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="f25b0-125">**tooadd Rally programvara från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="f25b0-125">**tooadd Rally Software from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="f25b0-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="f25b0-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="f25b0-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="f25b0-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="f25b0-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="f25b0-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="f25b0-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f25b0-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="f25b0-133">Skriv i sökrutan hello **Rally programvara**väljer **Rally programvara** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="f25b0-133">In hello search box, type **Rally Software**, select **Rally Software** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Rally programvara i hello resultatlistan](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="f25b0-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f25b0-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="f25b0-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Rally programvara baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="f25b0-136">In this section, you configure and test Azure AD single sign-on with Rally Software based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="f25b0-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Rally programvaran är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f25b0-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Rally Software is tooa user in Azure AD.</span></span> <span data-ttu-id="f25b0-138">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i Rally programvara toobe upprätta.</span><span class="sxs-lookup"><span data-stu-id="f25b0-138">In other words, a link relationship between an Azure AD user and hello related user in Rally Software needs toobe established.</span></span>

<span data-ttu-id="f25b0-139">Tilldela hello värdet hello Rally programvaran **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="f25b0-139">In Rally Software, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="f25b0-140">tooconfigure och testa Azure AD enkel inloggning med Rally programvara, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="f25b0-140">tooconfigure and test Azure AD single sign-on with Rally Software, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="f25b0-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="f25b0-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="f25b0-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f25b0-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f25b0-143">**[Skapa en testanvändare Rally programvara](#create-a-rally-software-test-user)**  -toohave en motsvarighet för Britta Simon Rally programvara som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="f25b0-143">**[Create a Rally Software test user](#create-a-rally-software-test-user)** - toohave a counterpart of Britta Simon in Rally Software that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="f25b0-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="f25b0-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f25b0-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="f25b0-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="f25b0-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f25b0-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="f25b0-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i Rally programmet.</span><span class="sxs-lookup"><span data-stu-id="f25b0-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Rally Software application.</span></span>

<span data-ttu-id="f25b0-148">**Utför följande hello tooconfigure Azure AD enkel inloggning med Rally programvara:**</span><span class="sxs-lookup"><span data-stu-id="f25b0-148">**tooconfigure Azure AD single sign-on with Rally Software, perform hello following steps:**</span></span>

1. <span data-ttu-id="f25b0-149">I hello Azure-portalen på hello **Rally programvara** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="f25b0-149">In hello Azure portal, on hello **Rally Software** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="f25b0-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="f25b0-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_samlbase.png)

3. <span data-ttu-id="f25b0-153">På hello **Rally programvara domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="f25b0-153">On hello **Rally Software Domain and URLs** section, perform hello following steps:</span></span>

    ![Rally programvara domän och URL: er enkel inloggning information](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_url.png)

    <span data-ttu-id="f25b0-155">a.</span><span class="sxs-lookup"><span data-stu-id="f25b0-155">a.</span></span> <span data-ttu-id="f25b0-156">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<tenant-name>.rally.com`</span><span class="sxs-lookup"><span data-stu-id="f25b0-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenant-name>.rally.com`</span></span>

    <span data-ttu-id="f25b0-157">b.</span><span class="sxs-lookup"><span data-stu-id="f25b0-157">b.</span></span> <span data-ttu-id="f25b0-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<tenant-name>.rally.com`</span><span class="sxs-lookup"><span data-stu-id="f25b0-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<tenant-name>.rally.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="f25b0-159">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="f25b0-159">These values are not real.</span></span> <span data-ttu-id="f25b0-160">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="f25b0-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="f25b0-161">Kontakta [Rally Programvaruklienten supportteamet](https://help.rallydev.com/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="f25b0-161">Contact [Rally Software Client support team](https://help.rallydev.com/) tooget these values.</span></span> 
 


4. <span data-ttu-id="f25b0-162">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="f25b0-162">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_certificate.png) 

5. <span data-ttu-id="f25b0-164">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="f25b0-164">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-rally-software-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="f25b0-166">På hello **Rally programvarukonfiguration** klickar du på **konfigurera Rally programvara** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="f25b0-166">On hello **Rally Software Configuration** section, click **Configure Rally Software** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="f25b0-167">Kopiera hello **Sign-Out URL och SAML enhets-ID** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="f25b0-167">Copy hello **Sign-Out URL, and SAML Entity ID** from hello **Quick Reference section.**</span></span>

    ![Rally konfiguration av programvara](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_configure.png) 

7. <span data-ttu-id="f25b0-169">Logga in tooyour **Rally programvara** klient.</span><span class="sxs-lookup"><span data-stu-id="f25b0-169">Log in tooyour **Rally Software** tenant.</span></span>

8. <span data-ttu-id="f25b0-170">Klicka i hello verktygsfältet hello längst upp **installationsprogrammet**, och välj sedan **prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="f25b0-170">In hello toolbar on hello top, click **Setup**, and then select **Subscription**.</span></span>
   
    <span data-ttu-id="f25b0-171">![Prenumerationen](./media/active-directory-saas-rally-software-tutorial/ic769531.png "prenumeration")</span><span class="sxs-lookup"><span data-stu-id="f25b0-171">![Subscription](./media/active-directory-saas-rally-software-tutorial/ic769531.png "Subscription")</span></span>

9. <span data-ttu-id="f25b0-172">Klicka på hello **åtgärd** knappen.</span><span class="sxs-lookup"><span data-stu-id="f25b0-172">Click hello **Action** button.</span></span> <span data-ttu-id="f25b0-173">Välj **redigera prenumeration** på hello upp till höger i hello verktygsfält.</span><span class="sxs-lookup"><span data-stu-id="f25b0-173">Select **Edit Subscription** at hello top right side of hello toolbar.</span></span>

10. <span data-ttu-id="f25b0-174">På hello **prenumeration** dialogrutan sida, utför följande steg hello och klicka sedan på **spara och Stäng**:</span><span class="sxs-lookup"><span data-stu-id="f25b0-174">On hello **Subscription** dialog page, perform hello following steps, and then click **Save & Close**:</span></span>
   
    <span data-ttu-id="f25b0-175">![Autentisering](./media/active-directory-saas-rally-software-tutorial/ic769542.png "autentisering")</span><span class="sxs-lookup"><span data-stu-id="f25b0-175">![Authentication](./media/active-directory-saas-rally-software-tutorial/ic769542.png "Authentication")</span></span>
   
    <span data-ttu-id="f25b0-176">a.</span><span class="sxs-lookup"><span data-stu-id="f25b0-176">a.</span></span> <span data-ttu-id="f25b0-177">Välj **autentisering Rally eller SSO** autentisering listrutan.</span><span class="sxs-lookup"><span data-stu-id="f25b0-177">Select **Rally or SSO authentication** from Authentication dropdown.</span></span>

    <span data-ttu-id="f25b0-178">b.</span><span class="sxs-lookup"><span data-stu-id="f25b0-178">b.</span></span> <span data-ttu-id="f25b0-179">I hello **identitet providern URL** textruta klistra in hello värdet för **SAML enhets-ID**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f25b0-179">In hello **Identity provider URL** textbox, paste hello value of **SAML Entity ID**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="f25b0-180">c.</span><span class="sxs-lookup"><span data-stu-id="f25b0-180">c.</span></span> <span data-ttu-id="f25b0-181">I hello **SSO logga ut** textruta klistra in hello värdet för **Sign-Out URL**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f25b0-181">In hello **SSO Logout** textbox, paste hello value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

> [!TIP]
> <span data-ttu-id="f25b0-182">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="f25b0-182">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="f25b0-183">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="f25b0-183">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="f25b0-184">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f25b0-184">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="f25b0-185">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="f25b0-185">Create an Azure AD test user</span></span>

<span data-ttu-id="f25b0-186">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f25b0-186">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="f25b0-188">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="f25b0-188">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="f25b0-189">Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="f25b0-189">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-rally-software-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="f25b0-191">toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="f25b0-191">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-rally-software-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="f25b0-193">tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f25b0-193">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello webbinställningar](./media/active-directory-saas-rally-software-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="f25b0-195">I hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="f25b0-195">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello användardialogrutan](./media/active-directory-saas-rally-software-tutorial/create_aaduser_04.png)

    <span data-ttu-id="f25b0-197">a.</span><span class="sxs-lookup"><span data-stu-id="f25b0-197">a.</span></span> <span data-ttu-id="f25b0-198">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f25b0-198">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f25b0-199">b.</span><span class="sxs-lookup"><span data-stu-id="f25b0-199">b.</span></span> <span data-ttu-id="f25b0-200">I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f25b0-200">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="f25b0-201">c.</span><span class="sxs-lookup"><span data-stu-id="f25b0-201">c.</span></span> <span data-ttu-id="f25b0-202">Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="f25b0-202">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="f25b0-203">d.</span><span class="sxs-lookup"><span data-stu-id="f25b0-203">d.</span></span> <span data-ttu-id="f25b0-204">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="f25b0-204">Click **Create**.</span></span>
 
### <a name="create-a-rally-software-test-user"></a><span data-ttu-id="f25b0-205">Skapa en testanvändare Rally programvara</span><span class="sxs-lookup"><span data-stu-id="f25b0-205">Create a Rally Software test user</span></span>

<span data-ttu-id="f25b0-206">Azure AD-användare toobe kan toosign i, måste de vara etablerade toohello Rally program med hjälp av deras användarnamn för Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f25b0-206">For Azure AD users toobe able toosign in, they must be provisioned toohello Rally Software application using their Azure Active Directory user names.</span></span>

<span data-ttu-id="f25b0-207">**tooconfigure användaretablering, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="f25b0-207">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="f25b0-208">Logga in tooyour Rally programvara klient.</span><span class="sxs-lookup"><span data-stu-id="f25b0-208">Log in tooyour Rally Software tenant.</span></span>

2. <span data-ttu-id="f25b0-209">Gå för**installationsprogrammet \> användare**, och klicka sedan på **+ Lägg till ny**.</span><span class="sxs-lookup"><span data-stu-id="f25b0-209">Go too**Setup \> USERS**, and then click **+ Add New**.</span></span>
   
    <span data-ttu-id="f25b0-210">![Användare](./media/active-directory-saas-rally-software-tutorial/ic781039.png "användare")</span><span class="sxs-lookup"><span data-stu-id="f25b0-210">![Users](./media/active-directory-saas-rally-software-tutorial/ic781039.png "Users")</span></span>

3. <span data-ttu-id="f25b0-211">Skriv hello namn i textrutan för hello ny användare och klicka sedan på **Lägg till med uppgifter**.</span><span class="sxs-lookup"><span data-stu-id="f25b0-211">Type hello name in hello New User textbox, and then click **Add with Details**.</span></span>

4. <span data-ttu-id="f25b0-212">I hello **skapa användare** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="f25b0-212">In hello **Create User** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="f25b0-213">![Skapa användare](./media/active-directory-saas-rally-software-tutorial/ic781040.png "skapa användare")</span><span class="sxs-lookup"><span data-stu-id="f25b0-213">![Create User](./media/active-directory-saas-rally-software-tutorial/ic781040.png "Create User")</span></span>

    <span data-ttu-id="f25b0-214">a.</span><span class="sxs-lookup"><span data-stu-id="f25b0-214">a.</span></span> <span data-ttu-id="f25b0-215">I hello **användarnamn** textruta hello-typnamn för användaren som **Brittsimon**.</span><span class="sxs-lookup"><span data-stu-id="f25b0-215">In hello **User Name** textbox, type hello name of user like **Brittsimon**.</span></span>
   
    <span data-ttu-id="f25b0-216">b.</span><span class="sxs-lookup"><span data-stu-id="f25b0-216">b.</span></span> <span data-ttu-id="f25b0-217">I **e-postadress** textruta ange hello e-postadress för användaren som  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="f25b0-217">In **E-mail Address** textbox, enter hello email of user like **brittasimon@contoso.com**.</span></span>

    <span data-ttu-id="f25b0-218">c.</span><span class="sxs-lookup"><span data-stu-id="f25b0-218">c.</span></span> <span data-ttu-id="f25b0-219">I **Förnamn** text Ange hello första namn för användaren som **Britta**.</span><span class="sxs-lookup"><span data-stu-id="f25b0-219">In **First Name** text box, enter hello first name of user like **Britta**.</span></span>

    <span data-ttu-id="f25b0-220">d.</span><span class="sxs-lookup"><span data-stu-id="f25b0-220">d.</span></span> <span data-ttu-id="f25b0-221">I **efternamn** text Ange hello efternamn för användaren som **Simon**.</span><span class="sxs-lookup"><span data-stu-id="f25b0-221">In **Last Name** text box, enter hello last name of user like **Simon**.</span></span>

    <span data-ttu-id="f25b0-222">e.</span><span class="sxs-lookup"><span data-stu-id="f25b0-222">e.</span></span> <span data-ttu-id="f25b0-223">Klicka på **spara och Stäng**.</span><span class="sxs-lookup"><span data-stu-id="f25b0-223">Click **Save & Close**.</span></span>

   >[!NOTE]
   ><span data-ttu-id="f25b0-224">Du kan använda alla andra Rally användaren konto skapas verktyg eller API: er som tillhandahålls av Rally programvara tooprovision användarkonton i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f25b0-224">You can use any other Rally Software user account creation tools or APIs provided by Rally Software tooprovision Azure AD user accounts.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="f25b0-225">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="f25b0-225">Assign hello Azure AD test user</span></span>

<span data-ttu-id="f25b0-226">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooRally programvara.</span><span class="sxs-lookup"><span data-stu-id="f25b0-226">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooRally Software.</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="f25b0-228">**tooassign Britta Simon tooRally programvara, utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="f25b0-228">**tooassign Britta Simon tooRally Software, perform hello following steps:**</span></span>

1. <span data-ttu-id="f25b0-229">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="f25b0-229">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="f25b0-231">Välj i listan med program hello **Rally programvara**.</span><span class="sxs-lookup"><span data-stu-id="f25b0-231">In hello applications list, select **Rally Software**.</span></span>

    ![hello Rally programvarulänk i listan med program hello](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_app.png)  

3. <span data-ttu-id="f25b0-233">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="f25b0-233">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202]

4. <span data-ttu-id="f25b0-235">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="f25b0-235">Click **Add** button.</span></span> <span data-ttu-id="f25b0-236">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f25b0-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="f25b0-238">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="f25b0-238">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="f25b0-239">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f25b0-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f25b0-240">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f25b0-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="f25b0-241">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f25b0-241">Test single sign-on</span></span>

<span data-ttu-id="f25b0-242">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="f25b0-242">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="f25b0-243">Du bör få automatiskt inloggade tooyour Rally program när du klickar på hello Rally programvara panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="f25b0-243">When you click hello Rally Software tile in hello Access Panel, you should get automatically signed-on tooyour Rally Software application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f25b0-244">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="f25b0-244">Additional resources</span></span>

* [<span data-ttu-id="f25b0-245">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f25b0-245">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f25b0-246">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="f25b0-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_203.png

