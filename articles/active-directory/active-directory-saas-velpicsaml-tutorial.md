---
title: "Självstudier: Azure Active Directory-integrering med Velpic SAML | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Velpic SAML."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: jeedes
ms.openlocfilehash: 613947d8fe95113382a2cdc0f79ce9eda85a0127
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-velpic-saml"></a><span data-ttu-id="98660-103">Självstudier: Azure Active Directory-integrering med Velpic SAML</span><span class="sxs-lookup"><span data-stu-id="98660-103">Tutorial: Azure Active Directory integration with Velpic SAML</span></span>

<span data-ttu-id="98660-104">I kursen får du lära dig hur toointegrate Velpic SAML med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="98660-104">In this tutorial, you learn how toointegrate Velpic SAML with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="98660-105">Integrera Velpic SAML med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="98660-105">Integrating Velpic SAML with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="98660-106">Du kan styra i Azure AD som har åtkomst tooVelpic SAML</span><span class="sxs-lookup"><span data-stu-id="98660-106">You can control in Azure AD who has access tooVelpic SAML</span></span>
- <span data-ttu-id="98660-107">Du kan aktivera din användare tooautomatically get inloggade tooVelpic SAML (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="98660-107">You can enable your users tooautomatically get signed-on tooVelpic SAML (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="98660-108">Du kan hantera dina konton i en central plats - hello Azure Management portal</span><span class="sxs-lookup"><span data-stu-id="98660-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="98660-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="98660-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="98660-110">Krav</span><span class="sxs-lookup"><span data-stu-id="98660-110">Prerequisites</span></span>

<span data-ttu-id="98660-111">tooconfigure Azure AD-integrering med Velpic SAML måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="98660-111">tooconfigure Azure AD integration with Velpic SAML, you need hello following items:</span></span>

- <span data-ttu-id="98660-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="98660-112">An Azure AD subscription</span></span>
- <span data-ttu-id="98660-113">En Velpic SAML enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="98660-113">A Velpic SAML single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="98660-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="98660-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="98660-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="98660-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="98660-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="98660-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="98660-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="98660-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="98660-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="98660-118">Scenario description</span></span>
<span data-ttu-id="98660-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="98660-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="98660-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="98660-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="98660-121">Att lägga till Velpic SAML från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="98660-121">Adding Velpic SAML from hello gallery</span></span>
2. <span data-ttu-id="98660-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="98660-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-velpic-saml-from-hello-gallery"></a><span data-ttu-id="98660-123">Att lägga till Velpic SAML från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="98660-123">Adding Velpic SAML from hello gallery</span></span>
<span data-ttu-id="98660-124">tooconfigure hello integrering av Velpic SAML i Azure AD, behöver du tooadd Velpic SAML hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="98660-124">tooconfigure hello integration of Velpic SAML into Azure AD, you need tooadd Velpic SAML from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="98660-125">**tooadd Velpic SAML från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="98660-125">**tooadd Velpic SAML from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="98660-126">I hello ** [Azure-hanteringsportalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="98660-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="98660-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="98660-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="98660-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="98660-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="98660-131">Klicka på **Lägg till** hello längst upp i hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="98660-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="98660-133">Skriv i sökrutan hello **Velpic SAML**.</span><span class="sxs-lookup"><span data-stu-id="98660-133">In hello search box, type **Velpic SAML**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_search.png)

5. <span data-ttu-id="98660-135">Markera hello resultat på panelen **Velpic SAML**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="98660-135">In hello results panel, select **Velpic SAML**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="98660-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="98660-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="98660-138">Du konfigurera och testa Azure AD enkel inloggning med Velpic SAML baserat på en testanvändare som kallas ”Britta Simon” i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="98660-138">In this section, you configure and test Azure AD single sign-on with Velpic SAML based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="98660-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Velpic SAML är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="98660-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Velpic SAML is tooa user in Azure AD.</span></span> <span data-ttu-id="98660-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Velpic SAML toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="98660-140">In other words, a link relationship between an Azure AD user and hello related user in Velpic SAML needs toobe established.</span></span>

<span data-ttu-id="98660-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Velpic SAML.</span><span class="sxs-lookup"><span data-stu-id="98660-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Velpic SAML.</span></span>

<span data-ttu-id="98660-142">tooconfigure och testa Azure AD enkel inloggning med Velpic SAML, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="98660-142">tooconfigure and test Azure AD single sign-on with Velpic SAML, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="98660-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on) ** -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="98660-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="98660-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user) ** -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="98660-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="98660-145">**[Skapa en testanvändare Velpic SAML](#creating-a-velpic-saml-test-user) ** -toohave en motsvarighet för Britta Simon i Velpic SAML som är länkade toohello Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="98660-145">**[Creating a Velpic SAML test user](#creating-a-velpic-saml-test-user)** - toohave a counterpart of Britta Simon in Velpic SAML that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="98660-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user) ** -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="98660-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="98660-147">**[Testa enkel inloggning](#testing-single-sign-on) ** -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="98660-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="98660-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="98660-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="98660-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure Management portal och konfigurera enkel inloggning i ditt Velpic SAML-program.</span><span class="sxs-lookup"><span data-stu-id="98660-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your Velpic SAML application.</span></span>

<span data-ttu-id="98660-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Velpic SAML:**</span><span class="sxs-lookup"><span data-stu-id="98660-150">**tooconfigure Azure AD single sign-on with Velpic SAML, perform hello following steps:**</span></span>

1. <span data-ttu-id="98660-151">I hello Azure Management portal på hello **Velpic SAML** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="98660-151">In hello Azure Management portal, on hello **Velpic SAML** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="98660-153">På hello **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** tooenable för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="98660-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_samlbase.png)

3. <span data-ttu-id="98660-155">Ange hello detaljer i hello **Velpic SAML domän och URL: er** avsnittet -</span><span class="sxs-lookup"><span data-stu-id="98660-155">Enter hello details in hello **Velpic SAML Domain and URLs** section-</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_url.png)

    <span data-ttu-id="98660-157">a.</span><span class="sxs-lookup"><span data-stu-id="98660-157">a.</span></span> <span data-ttu-id="98660-158">I hello **inloggnings-URL** textruta hello TYPVÄRDE som:`https://<sub-domain>.velpicsaml.net`</span><span class="sxs-lookup"><span data-stu-id="98660-158">In hello **Sign-on URL** textbox, type hello value as: `https://<sub-domain>.velpicsaml.net`</span></span>

    <span data-ttu-id="98660-159">b.</span><span class="sxs-lookup"><span data-stu-id="98660-159">b.</span></span> <span data-ttu-id="98660-160">I hello **identifierare** textruta klistra in hello **'Enkel inloggning på URL'** värde`https://auth.velpic.com/saml/v2/<entity-id>/login`</span><span class="sxs-lookup"><span data-stu-id="98660-160">In hello **Identifier** textbox, paste hello **‘Single sign on URL’** value `https://auth.velpic.com/saml/v2/<entity-id>/login`</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="98660-161">Observera att hello URL: en inloggning kommer att tillhandahållas av hello Velpic SAML-teamet och identifierarvärde blir tillgänglig när du konfigurerar hello SSO-plugin-programmet på Velpic SAML-sida.</span><span class="sxs-lookup"><span data-stu-id="98660-161">Please note that hello Sign on URL will be provided by hello Velpic SAML team and Identifier value will be available when you configure hello SSO Plugin on Velpic SAML side.</span></span> <span data-ttu-id="98660-162">Du måste toocopy som värdet från Velpic SAML appen på sidan och klistra in den här.</span><span class="sxs-lookup"><span data-stu-id="98660-162">You need toocopy that value from Velpic SAML application  page and paste it here.</span></span>

4. <span data-ttu-id="98660-163">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara hello XML-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="98660-163">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_certificate.png) 

5. <span data-ttu-id="98660-165">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="98660-165">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="98660-167">Öppna hello Velpic SAML-konfigurationsavsnittet, klicka på Konfigurera Velpic SAML tooopen konfigurera inloggning.</span><span class="sxs-lookup"><span data-stu-id="98660-167">On hello Velpic SAML Configuration section, click Configure Velpic SAML tooopen Configure sign-on window.</span></span> <span data-ttu-id="98660-168">Kopiera hello SAML enhets-ID från hello Snabbreferens avsnitt.</span><span class="sxs-lookup"><span data-stu-id="98660-168">Copy hello SAML Entity ID from hello Quick Reference section.</span></span>

7. <span data-ttu-id="98660-169">Logga in på webbplatsen Velpic SAML företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="98660-169">In a different web browser window, log into your Velpic SAML company site as an administrator.</span></span>

8. <span data-ttu-id="98660-170">Klicka på **hantera** fliken och gå för**integrering** avsnitt där du behöver tooclick på **plugin-program** knappen toocreate nytt plugin-program för inloggning.</span><span class="sxs-lookup"><span data-stu-id="98660-170">Click on **Manage** tab and go too**Integration** section where you need tooclick on **Plugins** button toocreate new plugin for Sign-In.</span></span>

    ![plugin-programmet](./media/active-directory-saas-velpicsaml-tutorial/velpic_1.png)

9. <span data-ttu-id="98660-172">Klicka på hello **'Lägga till plugin-programmet'** knappen.</span><span class="sxs-lookup"><span data-stu-id="98660-172">Click on hello **‘Add plugin’** button.</span></span>
    
    ![plugin-programmet](./media/active-directory-saas-velpicsaml-tutorial/velpic_2.png)

10. <span data-ttu-id="98660-174">Klicka på hello **SAML** panelen i hello lägga till Plugin-sida.</span><span class="sxs-lookup"><span data-stu-id="98660-174">Click on hello **SAML** tile in hello Add Plugin page.</span></span>
    
    ![plugin-programmet](./media/active-directory-saas-velpicsaml-tutorial/velpic_3.png)

11. <span data-ttu-id="98660-176">Ange hello av hello nytt SAML plugin-program och klicka på hello **'Add-** knappen.</span><span class="sxs-lookup"><span data-stu-id="98660-176">Enter hello name of hello new SAML plugin and click hello **‘Add’** button.</span></span>

    ![plugin-programmet](./media/active-directory-saas-velpicsaml-tutorial/velpic_4.png)

12. <span data-ttu-id="98660-178">Ange hello information på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="98660-178">Enter hello details as follows:</span></span>

    ![plugin-programmet](./media/active-directory-saas-velpicsaml-tutorial/velpic_5.png)

    <span data-ttu-id="98660-180">a.</span><span class="sxs-lookup"><span data-stu-id="98660-180">a.</span></span> <span data-ttu-id="98660-181">I hello **namn** textruta hello-typnamn för SAML-plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="98660-181">In hello **Name** textbox, type hello name of SAML plugin.</span></span>

    <span data-ttu-id="98660-182">b.</span><span class="sxs-lookup"><span data-stu-id="98660-182">b.</span></span> <span data-ttu-id="98660-183">I hello **utfärdar-URL** textruta klistra in hello **SAML enhets-ID** du kopierade från hello **konfigurera inloggning** fönster för hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="98660-183">In hello **Issuer URL** textbox, paste hello **SAML Entity ID** you copied from hello **Configure sign-on** window of hello Azure portal.</span></span>

    <span data-ttu-id="98660-184">c.</span><span class="sxs-lookup"><span data-stu-id="98660-184">c.</span></span> <span data-ttu-id="98660-185">I hello **providern Metadata Config** överför hello Metadata XML-fil som du hämtade från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="98660-185">In hello **Provider Metadata Config** upload hello Metadata XML file which you downloaded from Azure portal.</span></span>

    <span data-ttu-id="98660-186">d.</span><span class="sxs-lookup"><span data-stu-id="98660-186">d.</span></span> <span data-ttu-id="98660-187">Du kan också välja tooenable SAML precis i tid etablering genom att aktivera hello **'Automatiskt skapa nya användare'** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="98660-187">You can also choose tooenable SAML just in time provisioning by enabling hello **‘Auto create new users’** checkbox.</span></span> <span data-ttu-id="98660-188">Om en användare finns inte i Velpic och den här flaggan är inte aktiverad, misslyckas hello inloggningen från Azure.</span><span class="sxs-lookup"><span data-stu-id="98660-188">If a user doesn’t exist in Velpic and this flag is not enabled, hello login from Azure will fail.</span></span> <span data-ttu-id="98660-189">Om hello flaggan är aktiverad hello användaren kommer automatiskt att etableras i Velpic när hello inloggningen.</span><span class="sxs-lookup"><span data-stu-id="98660-189">If hello flag is enabled hello user will automatically be provisioned into Velpic at hello time of login.</span></span> 

    <span data-ttu-id="98660-190">e.</span><span class="sxs-lookup"><span data-stu-id="98660-190">e.</span></span> <span data-ttu-id="98660-191">Kopiera hello **enkel inloggning på URL: en** från hello textrutan och klistra in den i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="98660-191">Copy hello **Single sign on URL** from hello text box and paste it in hello Azure portal.</span></span>
    
    <span data-ttu-id="98660-192">f.</span><span class="sxs-lookup"><span data-stu-id="98660-192">f.</span></span> <span data-ttu-id="98660-193">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="98660-193">Click **Save**.</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="98660-194">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="98660-194">Creating an Azure AD test user</span></span>
<span data-ttu-id="98660-195">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure Management portal kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="98660-195">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="98660-197">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="98660-197">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="98660-198">I hello **Azure-hanteringsportalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="98660-198">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="98660-200">Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.</span><span class="sxs-lookup"><span data-stu-id="98660-200">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="98660-202">Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="98660-202">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="98660-204">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="98660-204">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="98660-206">a.</span><span class="sxs-lookup"><span data-stu-id="98660-206">a.</span></span> <span data-ttu-id="98660-207">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="98660-207">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="98660-208">b.</span><span class="sxs-lookup"><span data-stu-id="98660-208">b.</span></span> <span data-ttu-id="98660-209">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="98660-209">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="98660-210">c.</span><span class="sxs-lookup"><span data-stu-id="98660-210">c.</span></span> <span data-ttu-id="98660-211">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="98660-211">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="98660-212">d.</span><span class="sxs-lookup"><span data-stu-id="98660-212">d.</span></span> <span data-ttu-id="98660-213">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="98660-213">Click **Create**.</span></span>
 
### <a name="creating-a-velpic-saml-test-user"></a><span data-ttu-id="98660-214">Skapa en testanvändare Velpic SAML</span><span class="sxs-lookup"><span data-stu-id="98660-214">Creating a Velpic SAML test user</span></span>

<span data-ttu-id="98660-215">Det här steget krävs vanligtvis inte eftersom programmet hello stöder bara i tid användaretablering.</span><span class="sxs-lookup"><span data-stu-id="98660-215">This step is usually not required as hello application supports just in time user provisioning.</span></span> <span data-ttu-id="98660-216">Om hello automatisk användaretablering inte är aktiverat kan sedan skapa manuella användare göras som beskrivs nedan.</span><span class="sxs-lookup"><span data-stu-id="98660-216">If hello automatic user provisioning is not enabled then manual user creation can be done as described below.</span></span>

<span data-ttu-id="98660-217">Logga in på webbplatsen Velpic SAML företag som administratör och utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="98660-217">Log into your Velpic SAML company site as an administrator and perform following steps:</span></span>
    
1. <span data-ttu-id="98660-218">Klickar du på Hantera och gå tooUsers avsnittet, och klicka sedan på nya knappen tooadd användare.</span><span class="sxs-lookup"><span data-stu-id="98660-218">Click on Manage tab and go tooUsers section, then click on New button tooadd users.</span></span>

    ![Lägg till användare](./media/active-directory-saas-velpicsaml-tutorial/velpic_7.png)

2. <span data-ttu-id="98660-220">På hello **”skapa nya användare”** dialogrutan utför hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="98660-220">On hello **“Create New User”** dialog page, perform hello following steps.</span></span>

    ![Användaren](./media/active-directory-saas-velpicsaml-tutorial/velpic_8.png)
    
    <span data-ttu-id="98660-222">a.</span><span class="sxs-lookup"><span data-stu-id="98660-222">a.</span></span> <span data-ttu-id="98660-223">I hello **Förnamn** textruta typen hello förnamn Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="98660-223">In hello **First Name** textbox, type hello first name of Britta Simon.</span></span>

    <span data-ttu-id="98660-224">b.</span><span class="sxs-lookup"><span data-stu-id="98660-224">b.</span></span> <span data-ttu-id="98660-225">I hello **efternamn** textruta Skriv hello efternamn Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="98660-225">In hello **Last Name** textbox, type hello last name of Britta Simon.</span></span>

    <span data-ttu-id="98660-226">c.</span><span class="sxs-lookup"><span data-stu-id="98660-226">c.</span></span> <span data-ttu-id="98660-227">I hello **användarnamn** textruta typen hello användarnamn i Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="98660-227">In hello **User Name** textbox, type hello user name of Britta Simon.</span></span>

    <span data-ttu-id="98660-228">d.</span><span class="sxs-lookup"><span data-stu-id="98660-228">d.</span></span> <span data-ttu-id="98660-229">I hello **e-post** textruta typen hello e-postadress för Britta Simon konto.</span><span class="sxs-lookup"><span data-stu-id="98660-229">In hello **Email** textbox, type hello email address of Britta Simon account.</span></span>

    <span data-ttu-id="98660-230">e.</span><span class="sxs-lookup"><span data-stu-id="98660-230">e.</span></span> <span data-ttu-id="98660-231">Resten av hello information är valfritt, kan du fylla i den vid behov.</span><span class="sxs-lookup"><span data-stu-id="98660-231">Rest of hello information is optional, you can fill it if needed.</span></span>
    
    <span data-ttu-id="98660-232">f.</span><span class="sxs-lookup"><span data-stu-id="98660-232">f.</span></span> <span data-ttu-id="98660-233">Klicka på **SPARA**.</span><span class="sxs-lookup"><span data-stu-id="98660-233">Click **SAVE**.</span></span>  

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="98660-234">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="98660-234">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="98660-235">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att ge sina access tooVelpic SAML.</span><span class="sxs-lookup"><span data-stu-id="98660-235">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooVelpic SAML.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="98660-237">**tooassign Britta Simon tooVelpic SAML utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="98660-237">**tooassign Britta Simon tooVelpic SAML, perform hello following steps:**</span></span>

1. <span data-ttu-id="98660-238">I hello Azure Management portal öppnar du hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="98660-238">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="98660-240">Välj i listan med program hello **Velpic SAML**.</span><span class="sxs-lookup"><span data-stu-id="98660-240">In hello applications list, select **Velpic SAML**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_app.png) 

3. <span data-ttu-id="98660-242">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="98660-242">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="98660-244">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="98660-244">Click **Add** button.</span></span> <span data-ttu-id="98660-245">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="98660-245">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="98660-247">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="98660-247">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="98660-248">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="98660-248">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="98660-249">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="98660-249">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="98660-250">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="98660-250">Testing single sign-on</span></span>

<span data-ttu-id="98660-251">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="98660-251">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

1. <span data-ttu-id="98660-252">När du klickar på hello Velpic SAML-panelen i hello åtkomstpanelen bör du hämta inloggningssidan för Velpic SAML-program.</span><span class="sxs-lookup"><span data-stu-id="98660-252">When you click hello Velpic SAML tile in hello Access Panel, you should get login page of Velpic SAML application.</span></span> <span data-ttu-id="98660-253">Du bör se hello **”logga in med Azure AD-** hello inloggningssidan-knappen.</span><span class="sxs-lookup"><span data-stu-id="98660-253">You should see hello **‘Log In With Azure AD’** button on hello sign in page.</span></span>

    ![plugin-programmet](./media/active-directory-saas-velpicsaml-tutorial/velpic_6.png)

2. <span data-ttu-id="98660-255">Klicka på hello **”logga in med Azure AD-** knappen toolog i tooVelpic med hjälp av Azure AD-kontot.</span><span class="sxs-lookup"><span data-stu-id="98660-255">Click on hello **‘Log In With Azure AD’** button toolog in tooVelpic using your Azure AD account.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="98660-256">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="98660-256">Additional resources</span></span>

* [<span data-ttu-id="98660-257">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="98660-257">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="98660-258">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="98660-258">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_203.png

