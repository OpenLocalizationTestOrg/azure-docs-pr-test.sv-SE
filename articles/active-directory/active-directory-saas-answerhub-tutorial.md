---
title: "Självstudier: Azure Active Directory-integrering med AnswerHub | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och AnswerHub."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 818b91d7-01df-4b36-9706-f167c710a73c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 90b530da31abe7e6f18bfa2c5409f8ff1d4f1063
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-answerhub"></a><span data-ttu-id="b0752-103">Självstudier: Azure Active Directory-integrering med AnswerHub</span><span class="sxs-lookup"><span data-stu-id="b0752-103">Tutorial: Azure Active Directory integration with AnswerHub</span></span>

<span data-ttu-id="b0752-104">I kursen får du lära dig hur toointegrate AnswerHub med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="b0752-104">In this tutorial, you learn how toointegrate AnswerHub with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b0752-105">Integrera AnswerHub med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="b0752-105">Integrating AnswerHub with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="b0752-106">Du kan styra i Azure AD som har åtkomst till tooAnswerHub</span><span class="sxs-lookup"><span data-stu-id="b0752-106">You can control in Azure AD who has access tooAnswerHub</span></span>
- <span data-ttu-id="b0752-107">Du kan aktivera din användare tooautomatically get inloggade tooAnswerHub (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="b0752-107">You can enable your users tooautomatically get signed-on tooAnswerHub (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b0752-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="b0752-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="b0752-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b0752-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b0752-110">Krav</span><span class="sxs-lookup"><span data-stu-id="b0752-110">Prerequisites</span></span>

<span data-ttu-id="b0752-111">tooconfigure Azure AD-integrering med AnswerHub, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="b0752-111">tooconfigure Azure AD integration with AnswerHub, you need hello following items:</span></span>

- <span data-ttu-id="b0752-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="b0752-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b0752-113">En AnswerHub enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="b0752-113">An AnswerHub single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b0752-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="b0752-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b0752-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="b0752-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b0752-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="b0752-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b0752-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b0752-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b0752-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="b0752-118">Scenario description</span></span>
<span data-ttu-id="b0752-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="b0752-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b0752-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="b0752-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b0752-121">Att lägga till AnswerHub från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="b0752-121">Adding AnswerHub from hello gallery</span></span>
2. <span data-ttu-id="b0752-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b0752-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-answerhub-from-hello-gallery"></a><span data-ttu-id="b0752-123">Att lägga till AnswerHub från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="b0752-123">Adding AnswerHub from hello gallery</span></span>
<span data-ttu-id="b0752-124">tooconfigure hello integrering av AnswerHub i Azure AD, behöver du tooadd AnswerHub hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="b0752-124">tooconfigure hello integration of AnswerHub into Azure AD, you need tooadd AnswerHub from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="b0752-125">**tooadd AnswerHub från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b0752-125">**tooadd AnswerHub from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="b0752-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="b0752-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b0752-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="b0752-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="b0752-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="b0752-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="b0752-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b0752-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="b0752-133">Skriv i sökrutan hello **AnswerHub**.</span><span class="sxs-lookup"><span data-stu-id="b0752-133">In hello search box, type **AnswerHub**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_search.png)

5. <span data-ttu-id="b0752-135">Markera hello resultat på panelen **AnswerHub**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="b0752-135">In hello results panel, select **AnswerHub**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b0752-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b0752-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b0752-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med AnswerHub baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="b0752-138">In this section, you configure and test Azure AD single sign-on with AnswerHub based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b0752-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i AnswerHub är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b0752-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in AnswerHub is tooa user in Azure AD.</span></span> <span data-ttu-id="b0752-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i AnswerHub toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="b0752-140">In other words, a link relationship between an Azure AD user and hello related user in AnswerHub needs toobe established.</span></span>

<span data-ttu-id="b0752-141">I AnswerHub, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="b0752-141">In AnswerHub, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="b0752-142">tooconfigure och testa Azure AD enkel inloggning med AnswerHub, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="b0752-142">tooconfigure and test Azure AD single sign-on with AnswerHub, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="b0752-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="b0752-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="b0752-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b0752-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b0752-145">**[Skapa en testanvändare AnswerHub](#creating-an-answerhub-test-user)**  -toohave en motsvarighet för Britta Simon i AnswerHub som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="b0752-145">**[Creating an AnswerHub test user](#creating-an-answerhub-test-user)** - toohave a counterpart of Britta Simon in AnswerHub that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="b0752-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b0752-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b0752-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="b0752-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b0752-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b0752-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b0752-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt AnswerHub program.</span><span class="sxs-lookup"><span data-stu-id="b0752-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your AnswerHub application.</span></span>

<span data-ttu-id="b0752-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med AnswerHub:**</span><span class="sxs-lookup"><span data-stu-id="b0752-150">**tooconfigure Azure AD single sign-on with AnswerHub, perform hello following steps:**</span></span>

1. <span data-ttu-id="b0752-151">I hello Azure-portalen på hello **AnswerHub** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="b0752-151">In hello Azure portal, on hello **AnswerHub** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="b0752-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b0752-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_samlbase.png)

3. <span data-ttu-id="b0752-155">På hello **AnswerHub domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b0752-155">On hello **AnswerHub Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_url.png)

    <span data-ttu-id="b0752-157">a.</span><span class="sxs-lookup"><span data-stu-id="b0752-157">a.</span></span> <span data-ttu-id="b0752-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company>.answerhub.com`</span><span class="sxs-lookup"><span data-stu-id="b0752-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<company>.answerhub.com`</span></span>

    <span data-ttu-id="b0752-159">b.</span><span class="sxs-lookup"><span data-stu-id="b0752-159">b.</span></span> <span data-ttu-id="b0752-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company>.answerhub.com`</span><span class="sxs-lookup"><span data-stu-id="b0752-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<company>.answerhub.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b0752-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="b0752-161">These values are not real.</span></span> <span data-ttu-id="b0752-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="b0752-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="b0752-163">Kontakta [AnswerHub klienten supportteamet](mailto:success@answerhub.com) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="b0752-163">Contact [AnswerHub Client support team](mailto:success@answerhub.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="b0752-164">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="b0752-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_certificate.png) 

5. <span data-ttu-id="b0752-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="b0752-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-answerhub-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="b0752-168">På hello **AnswerHub Configuration** klickar du på **konfigurera AnswerHub** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="b0752-168">On hello **AnswerHub Configuration** section, click **Configure AnswerHub** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="b0752-169">Kopiera hello **Sign-Out URL och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="b0752-169">Copy hello **Sign-Out URL, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_configure.png) 

7. <span data-ttu-id="b0752-171">Logga in på webbplatsen AnswerHub företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="b0752-171">In a different web browser window, log into your AnswerHub company site as an administrator.</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="b0752-172">Om du behöver hjälp med att konfigurera AnswerHub kontaktar [Answerhubs supportteamet](mailto:success@answerhub.com.).</span><span class="sxs-lookup"><span data-stu-id="b0752-172">If you need help configuring AnswerHub, contact [AnswerHub's support team](mailto:success@answerhub.com.).</span></span>
   
8. <span data-ttu-id="b0752-173">Gå för**Administration**.</span><span class="sxs-lookup"><span data-stu-id="b0752-173">Go too**Administration**.</span></span>

9. <span data-ttu-id="b0752-174">Klicka på hello **användar- och** fliken.</span><span class="sxs-lookup"><span data-stu-id="b0752-174">Click hello **User and Group** tab.</span></span>

10. <span data-ttu-id="b0752-175">I navigeringsfönstret för hello på vänster sida, i hello hello **sociala inställningar** klickar du på **SAML installationsprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="b0752-175">In hello navigation pane on hello left side, in hello **Social Settings** section, click **SAML Setup**.</span></span>

11. <span data-ttu-id="b0752-176">Klicka på **IDP Config** fliken.</span><span class="sxs-lookup"><span data-stu-id="b0752-176">Click **IDP Config** tab.</span></span>

12. <span data-ttu-id="b0752-177">På hello **IDP Config** fliken, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b0752-177">On hello **IDP Config** tab, perform hello following steps:</span></span>

     <span data-ttu-id="b0752-178">![SAML-installationsprogrammet](./media/active-directory-saas-answerhub-tutorial/ic785172.png "SAML-installationen")</span><span class="sxs-lookup"><span data-stu-id="b0752-178">![SAML Setup](./media/active-directory-saas-answerhub-tutorial/ic785172.png "SAML Setup")</span></span>  
  
     <span data-ttu-id="b0752-179">a.</span><span class="sxs-lookup"><span data-stu-id="b0752-179">a.</span></span> <span data-ttu-id="b0752-180">I **IDP inloggnings-URL** textruta klistra in **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b0752-180">In **IDP Login URL** textbox, paste **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
  
     <span data-ttu-id="b0752-181">b.</span><span class="sxs-lookup"><span data-stu-id="b0752-181">b.</span></span> <span data-ttu-id="b0752-182">I **IDP logga ut URL** textruta klistra in **Sign-Out URL** värde som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b0752-182">In **IDP Logout URL** textbox, paste **Sign-Out URL** value which you have copied from Azure portal.</span></span>
     
     <span data-ttu-id="b0752-183">c.</span><span class="sxs-lookup"><span data-stu-id="b0752-183">c.</span></span> <span data-ttu-id="b0752-184">I **IDP identifierare namnformat** textruta ange hello användare identifierare värdet samma som väljs i Azure-portalen i **användarattribut** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="b0752-184">In **IDP Name Identifier Format** textbox, enter hello user Identifier value same as selected in Azure portal in **User Attributes** section.</span></span>
  
     <span data-ttu-id="b0752-185">d.</span><span class="sxs-lookup"><span data-stu-id="b0752-185">d.</span></span> <span data-ttu-id="b0752-186">Klicka på **nycklar och certifikat**.</span><span class="sxs-lookup"><span data-stu-id="b0752-186">Click **Keys and Certificates**.</span></span>

13. <span data-ttu-id="b0752-187">Utför följande steg hello hello nycklar och certifikat på fliken:</span><span class="sxs-lookup"><span data-stu-id="b0752-187">On hello Keys and Certificates tab, perform hello following steps:</span></span>
    
     <span data-ttu-id="b0752-188">![Nycklar och certifikat](./media/active-directory-saas-answerhub-tutorial/ic785173.png "nycklar och certifikat")</span><span class="sxs-lookup"><span data-stu-id="b0752-188">![Keys and Certificates](./media/active-directory-saas-answerhub-tutorial/ic785173.png "Keys and Certificates")</span></span>  
 
     <span data-ttu-id="b0752-189">a.</span><span class="sxs-lookup"><span data-stu-id="b0752-189">a.</span></span> <span data-ttu-id="b0752-190">Öppna din Base64-kodade certifikat som du har hämtat från Azure-portalen i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den toohello **IDP offentlig nyckel (x 509-Format)** textruta.</span><span class="sxs-lookup"><span data-stu-id="b0752-190">Open your base-64 encoded certificate which you have downloaded from Azure portal in notepad, copy hello content of it into your clipboard, and then paste it toohello **IDP Public Key (x509 Format)** textbox.</span></span>
  
     <span data-ttu-id="b0752-191">b.</span><span class="sxs-lookup"><span data-stu-id="b0752-191">b.</span></span> <span data-ttu-id="b0752-192">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="b0752-192">Click **Save**.</span></span>

14. <span data-ttu-id="b0752-193">På hello **IDP Config** klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="b0752-193">On hello **IDP Config** tab, click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="b0752-194">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="b0752-194">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="b0752-195">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="b0752-195">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="b0752-196">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b0752-196">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b0752-197">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="b0752-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="b0752-198">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b0752-198">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="b0752-200">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b0752-200">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="b0752-201">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="b0752-201">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-answerhub-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b0752-203">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="b0752-203">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-answerhub-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b0752-205">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b0752-205">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-answerhub-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b0752-207">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b0752-207">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-answerhub-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b0752-209">a.</span><span class="sxs-lookup"><span data-stu-id="b0752-209">a.</span></span> <span data-ttu-id="b0752-210">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b0752-210">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b0752-211">b.</span><span class="sxs-lookup"><span data-stu-id="b0752-211">b.</span></span> <span data-ttu-id="b0752-212">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b0752-212">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b0752-213">c.</span><span class="sxs-lookup"><span data-stu-id="b0752-213">c.</span></span> <span data-ttu-id="b0752-214">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="b0752-214">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="b0752-215">d.</span><span class="sxs-lookup"><span data-stu-id="b0752-215">d.</span></span> <span data-ttu-id="b0752-216">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="b0752-216">Click **Create**.</span></span>
 
### <a name="creating-an-answerhub-test-user"></a><span data-ttu-id="b0752-217">Skapa en testanvändare AnswerHub</span><span class="sxs-lookup"><span data-stu-id="b0752-217">Creating an AnswerHub test user</span></span>

<span data-ttu-id="b0752-218">tooenable Azure AD-användare toolog i tooAnswerHub, måste de etableras i AnswerHub.</span><span class="sxs-lookup"><span data-stu-id="b0752-218">tooenable Azure AD users toolog in tooAnswerHub, they must be provisioned into AnswerHub.</span></span>  
<span data-ttu-id="b0752-219">Hello gäller AnswerHub är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="b0752-219">In hello case of AnswerHub, provisioning is a manual task.</span></span>

<span data-ttu-id="b0752-220">**tooprovision ett användarkonto, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="b0752-220">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="b0752-221">Logga in tooyour **AnswerHub** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="b0752-221">Log in tooyour **AnswerHub** company site as administrator.</span></span>

2. <span data-ttu-id="b0752-222">Gå för**Administration**.</span><span class="sxs-lookup"><span data-stu-id="b0752-222">Go too**Administration**.</span></span>

3. <span data-ttu-id="b0752-223">Klicka på hello **användare och grupper** fliken.</span><span class="sxs-lookup"><span data-stu-id="b0752-223">Click hello **Users & Groups** tab.</span></span>

4. <span data-ttu-id="b0752-224">I navigeringsfönstret för hello på vänster sida, i hello hello **hantera användare** klickar du på **skapa eller importera användare**.</span><span class="sxs-lookup"><span data-stu-id="b0752-224">In hello navigation pane on hello left side, in hello **Manage Users** section, click **Create or import users**.</span></span>
   
   <span data-ttu-id="b0752-225">![Användare och grupper](./media/active-directory-saas-answerhub-tutorial/ic785175.png "användare och grupper")</span><span class="sxs-lookup"><span data-stu-id="b0752-225">![Users & Groups](./media/active-directory-saas-answerhub-tutorial/ic785175.png "Users & Groups")</span></span>

5. <span data-ttu-id="b0752-226">Typen hello **e-postadress**, **användarnamn** och **lösenord** av en giltig Azure Active Directory-konto som du vill använda tooprovision hello relaterade textrutor och klicka sedan på  **Spara**.</span><span class="sxs-lookup"><span data-stu-id="b0752-226">Type hello **Email address**, **Username** and **Password** of a valid Azure Active Directory account you want tooprovision into hello related textboxes, and then click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="b0752-227">Du kan använda något annat AnswerHub användarens konto skapas verktyg eller API: er som tillhandahålls av AnswerHub tooprovision AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="b0752-227">You can use any other AnswerHub user account creation tools or APIs provided by AnswerHub tooprovision AAD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="b0752-228">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="b0752-228">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="b0752-229">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooAnswerHub.</span><span class="sxs-lookup"><span data-stu-id="b0752-229">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAnswerHub.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="b0752-231">**tooassign Britta Simon tooAnswerHub utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b0752-231">**tooassign Britta Simon tooAnswerHub, perform hello following steps:**</span></span>

1. <span data-ttu-id="b0752-232">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="b0752-232">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="b0752-234">Välj i listan med program hello **AnswerHub**.</span><span class="sxs-lookup"><span data-stu-id="b0752-234">In hello applications list, select **AnswerHub**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_app.png) 

3. <span data-ttu-id="b0752-236">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="b0752-236">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="b0752-238">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="b0752-238">Click **Add** button.</span></span> <span data-ttu-id="b0752-239">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b0752-239">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="b0752-241">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="b0752-241">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="b0752-242">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b0752-242">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b0752-243">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b0752-243">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b0752-244">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b0752-244">Testing single sign-on</span></span>

<span data-ttu-id="b0752-245">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="b0752-245">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="b0752-246">Du bör få automatiskt inloggade tooyour AnswerHub programmet när du klickar på hello AnswerHub panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="b0752-246">When you click hello AnswerHub tile in hello Access Panel, you should get automatically signed-on tooyour AnswerHub application.</span></span>
<span data-ttu-id="b0752-247">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b0752-247">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b0752-248">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="b0752-248">Additional resources</span></span>

* [<span data-ttu-id="b0752-249">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b0752-249">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b0752-250">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b0752-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_203.png

