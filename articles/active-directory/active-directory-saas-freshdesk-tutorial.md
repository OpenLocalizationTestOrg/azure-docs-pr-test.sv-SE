---
title: "Självstudier: Azure Active Directory-integrering med FreshDesk | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och FreshDesk."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c2a3e5aa-7b5a-4fe4-9285-45dbe6e8efcc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 577a5eb6d9b1bc03030a2b47f63d375869c903bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-freshdesk"></a><span data-ttu-id="f1d1f-103">Självstudier: Azure Active Directory-integrering med FreshDesk</span><span class="sxs-lookup"><span data-stu-id="f1d1f-103">Tutorial: Azure Active Directory integration with FreshDesk</span></span>

<span data-ttu-id="f1d1f-104">I kursen får du lära dig hur toointegrate FreshDesk med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="f1d1f-104">In this tutorial, you learn how toointegrate FreshDesk with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f1d1f-105">Integrera FreshDesk med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="f1d1f-105">Integrating FreshDesk with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="f1d1f-106">Du kan styra i Azure AD som har åtkomst till tooFreshDesk</span><span class="sxs-lookup"><span data-stu-id="f1d1f-106">You can control in Azure AD who has access tooFreshDesk</span></span>
- <span data-ttu-id="f1d1f-107">Du kan aktivera din användare tooautomatically get inloggade tooFreshDesk (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="f1d1f-107">You can enable your users tooautomatically get signed-on tooFreshDesk (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f1d1f-108">Du kan hantera dina konton i en central plats - hello Azure Management portal</span><span class="sxs-lookup"><span data-stu-id="f1d1f-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="f1d1f-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f1d1f-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f1d1f-110">Krav</span><span class="sxs-lookup"><span data-stu-id="f1d1f-110">Prerequisites</span></span>

<span data-ttu-id="f1d1f-111">tooconfigure Azure AD-integrering med FreshDesk, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="f1d1f-111">tooconfigure Azure AD integration with FreshDesk, you need hello following items:</span></span>

- <span data-ttu-id="f1d1f-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="f1d1f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f1d1f-113">En FreshDesk enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="f1d1f-113">A FreshDesk single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f1d1f-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f1d1f-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="f1d1f-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f1d1f-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="f1d1f-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f1d1f-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f1d1f-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="f1d1f-118">Scenario description</span></span>
<span data-ttu-id="f1d1f-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f1d1f-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="f1d1f-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f1d1f-121">Att lägga till FreshDesk från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="f1d1f-121">Adding FreshDesk from hello gallery</span></span>
2. <span data-ttu-id="f1d1f-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f1d1f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-freshdesk-from-hello-gallery"></a><span data-ttu-id="f1d1f-123">Att lägga till FreshDesk från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="f1d1f-123">Adding FreshDesk from hello gallery</span></span>
<span data-ttu-id="f1d1f-124">tooconfigure hello integrering av FreshDesk i Azure AD, behöver du tooadd FreshDesk hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-124">tooconfigure hello integration of FreshDesk into Azure AD, you need tooadd FreshDesk from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="f1d1f-125">**tooadd FreshDesk från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="f1d1f-125">**tooadd FreshDesk from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="f1d1f-126">I hello  **[Azure-hanteringsportalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f1d1f-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="f1d1f-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="f1d1f-131">Klicka på **Lägg till** hello längst upp i hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="f1d1f-133">Skriv i sökrutan hello **FreshDesk**.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-133">In hello search box, type **FreshDesk**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_search.png)

5. <span data-ttu-id="f1d1f-135">Markera hello resultat på panelen **FreshDesk**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-135">In hello results panel, select **FreshDesk**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f1d1f-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f1d1f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f1d1f-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med FreshDesk baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-138">In this section, you configure and test Azure AD single sign-on with FreshDesk based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="f1d1f-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i FreshDesk är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in FreshDesk is tooa user in Azure AD.</span></span> <span data-ttu-id="f1d1f-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i FreshDesk toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-140">In other words, a link relationship between an Azure AD user and hello related user in FreshDesk needs toobe established.</span></span>

<span data-ttu-id="f1d1f-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i FreshDesk.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in FreshDesk.</span></span>

<span data-ttu-id="f1d1f-142">tooconfigure och testa Azure AD enkel inloggning med FreshDesk, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="f1d1f-142">tooconfigure and test Azure AD single sign-on with FreshDesk, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="f1d1f-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="f1d1f-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f1d1f-145">**[Skapa en testanvändare FreshDesk](#creating-a-freshdesk-test-user)**  -toohave en motsvarighet för Britta Simon i FreshDesk som är länkade toohello Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-145">**[Creating a FreshDesk test user](#creating-a-freshdesk-test-user)** - toohave a counterpart of Britta Simon in FreshDesk that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="f1d1f-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f1d1f-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f1d1f-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f1d1f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f1d1f-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure Management portal och konfigurera enkel inloggning i ditt FreshDesk program.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your FreshDesk application.</span></span>

<span data-ttu-id="f1d1f-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med FreshDesk:**</span><span class="sxs-lookup"><span data-stu-id="f1d1f-150">**tooconfigure Azure AD single sign-on with FreshDesk, perform hello following steps:**</span></span>

1. <span data-ttu-id="f1d1f-151">I hello Azure Management portal på hello **FreshDesk** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-151">In hello Azure Management portal, on hello **FreshDesk** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="f1d1f-153">På hello **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** tooenable för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_samlbase.png)

3. <span data-ttu-id="f1d1f-155">På hello **FreshDesk domän och URL: er** avsnittet genom att ange hello **inloggnings-URL** som: `https://<tenant-name>.freshdesk.com` eller något annat värde Freshdesk har förslag.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-155">On hello **FreshDesk Domain and URLs** section, please enter hello **Sign-on URL** as: `https://<tenant-name>.freshdesk.com` or any other value Freshdesk has suggested.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_url.png)

    > [!NOTE] 
    > <span data-ttu-id="f1d1f-157">Observera att detta inte är det verkliga värdet.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-157">Please note that this is not the real value.</span></span> <span data-ttu-id="f1d1f-158">Du måste uppdatera värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-158">You have to update the value with the actual Sign-on URL.</span></span> <span data-ttu-id="f1d1f-159">Kontakta [FreshDesk klienten supportteamet](https://freshdesk.com/helpdesk-software?utm_source=Google-AdWords&utm_medium=Search-IND-Brand&utm_campaign=Search-IND-Brand&utm_term=freshdesk&device=c&gclid=COSH2_LH7NICFVUDvAodBPgBZg) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-159">Contact [FreshDesk Client support team](https://freshdesk.com/helpdesk-software?utm_source=Google-AdWords&utm_medium=Search-IND-Brand&utm_campaign=Search-IND-Brand&utm_term=freshdesk&device=c&gclid=COSH2_LH7NICFVUDvAodBPgBZg) to get this value.</span></span>  

4. <span data-ttu-id="f1d1f-160">På hello **SAML-signeringscertifikat** klickar du på **certifikat** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-160">On hello **SAML Signing Certificate** section, click **Certificate** and then save hello certificate on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_certificate.png) 

5. <span data-ttu-id="f1d1f-162">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-162">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-freshdesk-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="f1d1f-164">På hello **FreshDesk Configuration** klickar du på **konfigurera FreshDesk** tooopen inloggning konfigurera fönster.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-164">On hello **FreshDesk Configuration** section, click **Configure FreshDesk** tooopen Configure sign-on window.</span></span> <span data-ttu-id="f1d1f-165">Kopiera hello SAML inloggning tjänst-URL för enkel och Sign-Out URL från hello **Snabbreferens** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-165">Copy hello SAML Single Sign-On Service URL and Sign-Out URL from hello **Quick Reference** section.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_configure.png)

7. <span data-ttu-id="f1d1f-167">Logga in på webbplatsen Freshdesk företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-167">In a different web browser window, log into your Freshdesk company site as an administrator.</span></span>

8. <span data-ttu-id="f1d1f-168">Hello-menyn överst hello **Admin**.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-168">In hello menu on hello top, click **Admin**.</span></span>
   
   <span data-ttu-id="f1d1f-169">![Admin](./media/active-directory-saas-freshdesk-tutorial/IC776768.png "Admin")</span><span class="sxs-lookup"><span data-stu-id="f1d1f-169">![Admin](./media/active-directory-saas-freshdesk-tutorial/IC776768.png "Admin")</span></span>

9. <span data-ttu-id="f1d1f-170">I hello **allmänna inställningar** klickar du på **säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-170">In hello **General Settings** tab, click **Security**.</span></span>
   
   <span data-ttu-id="f1d1f-171">![Säkerhet](./media/active-directory-saas-freshdesk-tutorial/IC776769.png "säkerhet")</span><span class="sxs-lookup"><span data-stu-id="f1d1f-171">![Security](./media/active-directory-saas-freshdesk-tutorial/IC776769.png "Security")</span></span>

10. <span data-ttu-id="f1d1f-172">I hello **säkerhet** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="f1d1f-172">In hello **Security** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="f1d1f-173">![Enkel inloggning](./media/active-directory-saas-freshdesk-tutorial/IC776770.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="f1d1f-173">![Single Sign On](./media/active-directory-saas-freshdesk-tutorial/IC776770.png "Single Sign On")</span></span>
   
    <span data-ttu-id="f1d1f-174">a.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-174">a.</span></span> <span data-ttu-id="f1d1f-175">För **enkel inloggning (SSO)**väljer **på**.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-175">For **Single Sign On (SSO)**, select **On**.</span></span>

    <span data-ttu-id="f1d1f-176">b.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-176">b.</span></span> <span data-ttu-id="f1d1f-177">Välj **SAML SSO**.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-177">Select **SAML SSO**.</span></span>

    <span data-ttu-id="f1d1f-178">c.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-178">c.</span></span> <span data-ttu-id="f1d1f-179">Typen hello **SAML enkel inloggning Tjänstwebbadress** du kopierade från Azure-portalen i hello **inloggnings-URL för SAML** textruta.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-179">Type hello **SAML Single Sign-On Service URL** you copied from Azure portal into hello **SAML Login URL** textbox.</span></span>

    <span data-ttu-id="f1d1f-180">d.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-180">d.</span></span> <span data-ttu-id="f1d1f-181">Typen hello **Sign-Out URL** du kopierade från Azure-portalen i hello **logga ut URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-181">Type hello **Sign-Out URL**  you copied from Azure portal into hello **Logout URL** textbox.</span></span>

    <span data-ttu-id="f1d1f-182">e.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-182">e.</span></span> <span data-ttu-id="f1d1f-183">Kopiera hello **tumavtrycket** värde från hello hämtat certifikat från Azure-portalen och klistra in den i hello **säkerhet certifikat fingeravtryck** textruta.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-183">Copy hello **Thumbprint** value from hello downloaded certificate from Azure portal and paste it into hello **Security Certificate Fingerprint** textbox.</span></span>  
 
    >[!TIP]
    ><span data-ttu-id="f1d1f-184">Mer information finns i [hur tooretrieve en certifikatets tumavtrycksvärde](http://youtu.be/YKQF266SAxI).</span><span class="sxs-lookup"><span data-stu-id="f1d1f-184">For more details, see [How tooretrieve a certificate's thumbprint value](http://youtu.be/YKQF266SAxI).</span></span> 
    
    <span data-ttu-id="f1d1f-185">f.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-185">f.</span></span> <span data-ttu-id="f1d1f-186">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-186">Click **Save**.</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f1d1f-187">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="f1d1f-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="f1d1f-188">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure Management portal kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-188">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="f1d1f-190">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="f1d1f-190">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="f1d1f-191">I hello **Azure-hanteringsportalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-191">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-freshdesk-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f1d1f-193">Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-193">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-freshdesk-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f1d1f-195">Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-195">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-freshdesk-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f1d1f-197">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="f1d1f-197">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-freshdesk-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f1d1f-199">a.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-199">a.</span></span> <span data-ttu-id="f1d1f-200">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-200">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f1d1f-201">b.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-201">b.</span></span> <span data-ttu-id="f1d1f-202">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-202">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f1d1f-203">c.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-203">c.</span></span> <span data-ttu-id="f1d1f-204">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-204">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="f1d1f-205">d.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-205">d.</span></span> <span data-ttu-id="f1d1f-206">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-206">Click **Create**.</span></span>
 
### <a name="creating-a-freshdesk-test-user"></a><span data-ttu-id="f1d1f-207">Skapa en testanvändare FreshDesk</span><span class="sxs-lookup"><span data-stu-id="f1d1f-207">Creating a FreshDesk test user</span></span>

<span data-ttu-id="f1d1f-208">I ordning tooenable Azure AD-användare toolog i FreshDesk, måste de etableras i FreshDesk.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-208">In order tooenable Azure AD users toolog into FreshDesk, they must be provisioned into FreshDesk.</span></span>  
<span data-ttu-id="f1d1f-209">Hello gäller FreshDesk är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-209">In hello case of FreshDesk, provisioning is a manual task.</span></span>

<span data-ttu-id="f1d1f-210">**tooprovision användarkonton, utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="f1d1f-210">**tooprovision a user accounts, perform hello following steps:**</span></span>

1. <span data-ttu-id="f1d1f-211">Logga in tooyour **Freshdesk** klient.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-211">Log in tooyour **Freshdesk** tenant.</span></span>
2. <span data-ttu-id="f1d1f-212">Hello-menyn överst hello **Admin**.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-212">In hello menu on hello top, click **Admin**.</span></span>
   
   <span data-ttu-id="f1d1f-213">![Admin](./media/active-directory-saas-freshdesk-tutorial/IC776772.png "Admin")</span><span class="sxs-lookup"><span data-stu-id="f1d1f-213">![Admin](./media/active-directory-saas-freshdesk-tutorial/IC776772.png "Admin")</span></span>

3. <span data-ttu-id="f1d1f-214">I hello **allmänna inställningar** klickar du på **agenter**.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-214">In hello **General Settings** tab, click **Agents**.</span></span>
   
   <span data-ttu-id="f1d1f-215">![Agenter](./media/active-directory-saas-freshdesk-tutorial/IC776773.png "agenter")</span><span class="sxs-lookup"><span data-stu-id="f1d1f-215">![Agents](./media/active-directory-saas-freshdesk-tutorial/IC776773.png "Agents")</span></span>

4. <span data-ttu-id="f1d1f-216">Klicka på **ny Agent**.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-216">Click **New Agent**.</span></span>
   
    <span data-ttu-id="f1d1f-217">![Ny Agent](./media/active-directory-saas-freshdesk-tutorial/IC776774.png "ny Agent")</span><span class="sxs-lookup"><span data-stu-id="f1d1f-217">![New Agent](./media/active-directory-saas-freshdesk-tutorial/IC776774.png "New Agent")</span></span>

5. <span data-ttu-id="f1d1f-218">Utför följande hello på hello agentinformation dialogrutan:</span><span class="sxs-lookup"><span data-stu-id="f1d1f-218">On hello Agent Information dialog, perform hello following steps:</span></span>
   
   <span data-ttu-id="f1d1f-219">![Agentinformation](./media/active-directory-saas-freshdesk-tutorial/IC776775.png "agentinformation")</span><span class="sxs-lookup"><span data-stu-id="f1d1f-219">![Agent Information](./media/active-directory-saas-freshdesk-tutorial/IC776775.png "Agent Information")</span></span>
   
   <span data-ttu-id="f1d1f-220">a.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-220">a.</span></span> <span data-ttu-id="f1d1f-221">I hello **fullständiga namn** textruta hello-typnamn för hello Azure AD-konto som du vill tooprovision.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-221">In hello **Full Name** textbox, type hello name of hello Azure AD account you want tooprovision.</span></span>

   <span data-ttu-id="f1d1f-222">b.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-222">b.</span></span> <span data-ttu-id="f1d1f-223">I hello **e-post** textruta typen hello Azure AD e-postadress på hello Azure AD-konto som du vill tooprovision.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-223">In hello **Email** textbox, type hello Azure AD email address of hello Azure AD account you want tooprovision.</span></span>

   <span data-ttu-id="f1d1f-224">c.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-224">c.</span></span> <span data-ttu-id="f1d1f-225">I hello **rubrik** textruta ange hello rubrik på hello Azure AD-konto som du vill tooprovision.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-225">In hello **Title** textbox, type hello title of hello Azure AD account you want tooprovision.</span></span>

   <span data-ttu-id="f1d1f-226">d.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-226">d.</span></span> <span data-ttu-id="f1d1f-227">Välj **agenter rollen**, och klicka sedan på **tilldela**.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-227">Select **Agents role**, and then click **Assign**.</span></span>
       
   <span data-ttu-id="f1d1f-228">e.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-228">e.</span></span> <span data-ttu-id="f1d1f-229">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-229">Click **Save**.</span></span>     
   
    >[!NOTE]
    ><span data-ttu-id="f1d1f-230">hello Azure AD användare får ett e-postmeddelande som innehåller en länk tooconfirm hello-konto innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-230">hello Azure AD account holder will get an email that includes a link tooconfirm hello account before it is activated.</span></span> 
    > 
    
    >[!NOTE]
    ><span data-ttu-id="f1d1f-231">Du kan använda något annat Freshdesk användarens konto skapas verktyg eller API: er som tillhandahålls av Freshdesk tooprovision AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-231">You can use any other Freshdesk user account creation tools or APIs provided by Freshdesk tooprovision AAD user accounts.</span></span> <span data-ttu-id="f1d1f-232">tooFreshDesk.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-232">tooFreshDesk.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="f1d1f-233">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="f1d1f-233">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="f1d1f-234">I det här avsnittet Aktivera Britta Simon toouse Azure enkel inloggning genom att tilldela tooBox sin åtkomst.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-234">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooBox.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="f1d1f-236">**tooassign Britta Simon tooFreshDesk utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="f1d1f-236">**tooassign Britta Simon tooFreshDesk, perform hello following steps:**</span></span>

1. <span data-ttu-id="f1d1f-237">I hello Azure Management portal öppnar du hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-237">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="f1d1f-239">Välj i listan med program hello **FreshDesk**.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-239">In hello applications list, select **FreshDesk**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_app.png) 

3. <span data-ttu-id="f1d1f-241">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-241">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="f1d1f-243">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-243">Click **Add** button.</span></span> <span data-ttu-id="f1d1f-244">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-244">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="f1d1f-246">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-246">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="f1d1f-247">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-247">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f1d1f-248">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-248">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f1d1f-249">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f1d1f-249">Testing single sign-on</span></span>

<span data-ttu-id="f1d1f-250">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-250">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="f1d1f-251">Du bör få inloggningen sidan tooget inloggade tooyour FreshDesk programmet när du klickar på hello FreshDesk panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="f1d1f-251">When you click hello FreshDesk tile in hello Access Panel, you should get login page tooget signed-on tooyour FreshDesk application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f1d1f-252">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="f1d1f-252">Additional resources</span></span>

* [<span data-ttu-id="f1d1f-253">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f1d1f-253">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f1d1f-254">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="f1d1f-254">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_203.png

