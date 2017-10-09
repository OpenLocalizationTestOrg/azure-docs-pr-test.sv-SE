---
title: "Självstudier: Azure Active Directory-integrering med ArcGIS Online | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och ArcGIS Online."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a9e132a4-29e7-48bf-beb9-4148e617c8a9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: jeedes
ms.openlocfilehash: f3dd55d798cf3256fb2758e011f33946baa405ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-arcgis-online"></a><span data-ttu-id="91aeb-103">Självstudier: Azure Active Directory-integrering med ArcGIS Online</span><span class="sxs-lookup"><span data-stu-id="91aeb-103">Tutorial: Azure Active Directory integration with ArcGIS Online</span></span>

<span data-ttu-id="91aeb-104">I kursen får du lära dig hur toointegrate ArcGIS Online med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="91aeb-104">In this tutorial, you learn how toointegrate ArcGIS Online with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="91aeb-105">Integrera ArcGIS Online med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="91aeb-105">Integrating ArcGIS Online with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="91aeb-106">Du kan styra i Azure AD som har åtkomst tooArcGIS Online</span><span class="sxs-lookup"><span data-stu-id="91aeb-106">You can control in Azure AD who has access tooArcGIS Online</span></span>
- <span data-ttu-id="91aeb-107">Du kan aktivera din användare tooautomatically get inloggade tooArcGIS Online (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="91aeb-107">You can enable your users tooautomatically get signed-on tooArcGIS Online (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="91aeb-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="91aeb-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="91aeb-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="91aeb-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

<!--## Overview

tooenable single sign-on with ArcGIS Online, it must be configured toouse Azure Active Directory as an identity provider. This guide provides information and tips on how tooperform this configuration in ArcGIS Online.

>[!Note]: 
>This embedded guide is brand new in hello new Azure portal, and we’d love toohear your thoughts. Use hello Feedback ? button at hello top of hello portal tooprovide feedback. hello older guide for using hello [Azure classic portal](https://manage.windowsazure.com) tooconfigure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a><span data-ttu-id="91aeb-110">Krav</span><span class="sxs-lookup"><span data-stu-id="91aeb-110">Prerequisites</span></span>

<span data-ttu-id="91aeb-111">tooconfigure Azure AD-integrering med ArcGIS Online, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="91aeb-111">tooconfigure Azure AD integration with ArcGIS Online, you need hello following items:</span></span>

- <span data-ttu-id="91aeb-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="91aeb-112">An Azure AD subscription</span></span>
- <span data-ttu-id="91aeb-113">En ArcGIS Online enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="91aeb-113">A ArcGIS Online single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="91aeb-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="91aeb-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="91aeb-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="91aeb-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="91aeb-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="91aeb-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="91aeb-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="91aeb-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="91aeb-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="91aeb-118">Scenario description</span></span>
<span data-ttu-id="91aeb-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="91aeb-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="91aeb-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="91aeb-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="91aeb-121">Att lägga till ArcGIS Online från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="91aeb-121">Adding ArcGIS Online from hello gallery</span></span>
2. <span data-ttu-id="91aeb-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="91aeb-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-arcgis-online-from-hello-gallery"></a><span data-ttu-id="91aeb-123">Att lägga till ArcGIS Online från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="91aeb-123">Adding ArcGIS Online from hello gallery</span></span>
<span data-ttu-id="91aeb-124">tooconfigure hello integrering av ArcGIS Online i Azure AD, behöver du tooadd ArcGIS Online hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="91aeb-124">tooconfigure hello integration of ArcGIS Online into Azure AD, you need tooadd ArcGIS Online from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="91aeb-125">**tooadd ArcGIS Online från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="91aeb-125">**tooadd ArcGIS Online from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="91aeb-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="91aeb-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="91aeb-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="91aeb-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="91aeb-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="91aeb-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="91aeb-131">Klicka på **nytt program** hello längst upp i hello dialogrutan tooadd nytt program.</span><span class="sxs-lookup"><span data-stu-id="91aeb-131">Click **New application** button on hello top of hello dialog tooadd new application.</span></span>

    ![Program][3]

4. <span data-ttu-id="91aeb-133">Skriv i sökrutan hello **ArcGIS Online**.</span><span class="sxs-lookup"><span data-stu-id="91aeb-133">In hello search box, type **ArcGIS Online**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_search.png)

5. <span data-ttu-id="91aeb-135">Markera hello resultat på panelen **ArcGIS Online**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="91aeb-135">In hello results panel, select **ArcGIS Online**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="91aeb-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="91aeb-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="91aeb-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med ArcGIS Online baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="91aeb-138">In this section, you configure and test Azure AD single sign-on with ArcGIS Online based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="91aeb-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i ArcGIS Online är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="91aeb-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in ArcGIS Online is tooa user in Azure AD.</span></span> <span data-ttu-id="91aeb-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i ArcGIS Online toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="91aeb-140">In other words, a link relationship between an Azure AD user and hello related user in ArcGIS Online needs toobe established.</span></span>

<span data-ttu-id="91aeb-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i ArcGIS Online.</span><span class="sxs-lookup"><span data-stu-id="91aeb-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in ArcGIS Online.</span></span>

<span data-ttu-id="91aeb-142">tooconfigure och testa Azure AD enkel inloggning med ArcGIS Online, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="91aeb-142">tooconfigure and test Azure AD single sign-on with ArcGIS Online, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="91aeb-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="91aeb-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="91aeb-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="91aeb-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="91aeb-145">**[Skapa en ArcGIS Online testanvändare](#creating-an-arcgis-online-test-user)**  -toohave en motsvarighet för Britta Simon i ArcGIS Online som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="91aeb-145">**[Creating an ArcGIS Online test user](#creating-an-arcgis-online-test-user)** - toohave a counterpart of Britta Simon in ArcGIS Online that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="91aeb-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="91aeb-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="91aeb-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="91aeb-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="91aeb-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="91aeb-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="91aeb-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet ArcGIS Online.</span><span class="sxs-lookup"><span data-stu-id="91aeb-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your ArcGIS Online application.</span></span>

<span data-ttu-id="91aeb-150">**Utför följande hello tooconfigure Azure AD enkel inloggning med ArcGIS Online:**</span><span class="sxs-lookup"><span data-stu-id="91aeb-150">**tooconfigure Azure AD single sign-on with ArcGIS Online, perform hello following steps:**</span></span>

1. <span data-ttu-id="91aeb-151">I hello Azure-portalen på hello **ArcGIS Online** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="91aeb-151">In hello Azure portal, on hello **ArcGIS Online** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="91aeb-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="91aeb-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_samlbase.png)

3. <span data-ttu-id="91aeb-155">På hello **ArcGIS Online domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="91aeb-155">On hello **ArcGIS Online Domain and URLs** section, perform hello following step:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_url.png)

    <span data-ttu-id="91aeb-157">I hello **inloggnings-URL** textruta hello TYPVÄRDE med hello följande mönster:`https://<company>.maps.arcgis.com`</span><span class="sxs-lookup"><span data-stu-id="91aeb-157">In hello **Sign-on URL** textbox, type hello value using hello following pattern: `https://<company>.maps.arcgis.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="91aeb-158">Det här värdet är inte hello verkliga.</span><span class="sxs-lookup"><span data-stu-id="91aeb-158">This value is not hello real.</span></span> <span data-ttu-id="91aeb-159">Uppdatera det här värdet med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="91aeb-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="91aeb-160">Kontakta [ArcGIS Online klienten supportteamet](http://support.esri.com/) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="91aeb-160">Contact [ArcGIS Online Client support team](http://support.esri.com/) tooget this value.</span></span> 

4. <span data-ttu-id="91aeb-161">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara hello XML-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="91aeb-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_certificate.png) 

5. <span data-ttu-id="91aeb-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="91aeb-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-arcgis-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="91aeb-165">Logga in på webbplatsen ArcGIS företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="91aeb-165">In a different web browser window, log into your ArcGIS company site as an administrator.</span></span>

7. <span data-ttu-id="91aeb-166">Klicka på **redigera inställningar för**.</span><span class="sxs-lookup"><span data-stu-id="91aeb-166">Click **EDIT SETTINGS**.</span></span>

    <span data-ttu-id="91aeb-167">![Redigera inställningar för](./media/active-directory-saas-arcgis-tutorial/ic784742.png "redigera inställningar")</span><span class="sxs-lookup"><span data-stu-id="91aeb-167">![Edit Settings](./media/active-directory-saas-arcgis-tutorial/ic784742.png "Edit Settings")</span></span>

8. <span data-ttu-id="91aeb-168">Klicka på **säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="91aeb-168">Click **Security**.</span></span>

    <span data-ttu-id="91aeb-169">![Säkerhet](./media/active-directory-saas-arcgis-tutorial/ic784743.png "säkerhet")</span><span class="sxs-lookup"><span data-stu-id="91aeb-169">![Security](./media/active-directory-saas-arcgis-tutorial/ic784743.png "Security")</span></span>

9. <span data-ttu-id="91aeb-170">Under **Enterprise inloggningar**, klickar du på **ange IDENTITETSLEVERANTÖR**.</span><span class="sxs-lookup"><span data-stu-id="91aeb-170">Under **Enterprise Logins**, click **SET IDENTITY PROVIDER**.</span></span>

    <span data-ttu-id="91aeb-171">![Enterprise-inloggningar](./media/active-directory-saas-arcgis-tutorial/ic784744.png "Enterprise-inloggningar")</span><span class="sxs-lookup"><span data-stu-id="91aeb-171">![Enterprise Logins](./media/active-directory-saas-arcgis-tutorial/ic784744.png "Enterprise Logins")</span></span>

10. <span data-ttu-id="91aeb-172">På hello **ange identitetsleverantör** konfigurationen utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="91aeb-172">On hello **Set Identity Provider** configuration page, perform hello following steps:</span></span>
   
    <span data-ttu-id="91aeb-173">![Ange identitetsleverantör](./media/active-directory-saas-arcgis-tutorial/ic784745.png "ange identitetsleverantören.")</span><span class="sxs-lookup"><span data-stu-id="91aeb-173">![Set Identity Provider](./media/active-directory-saas-arcgis-tutorial/ic784745.png "Set Identity Provider")</span></span>
   
    <span data-ttu-id="91aeb-174">a.</span><span class="sxs-lookup"><span data-stu-id="91aeb-174">a.</span></span> <span data-ttu-id="91aeb-175">I hello **namn** textruta Skriv ditt organisationsnamn.</span><span class="sxs-lookup"><span data-stu-id="91aeb-175">In hello **Name** textbox, type your organization’s name.</span></span>

    <span data-ttu-id="91aeb-176">b.</span><span class="sxs-lookup"><span data-stu-id="91aeb-176">b.</span></span> <span data-ttu-id="91aeb-177">För **Metadata för hello Enterprise identitetsleverantör ska anges med hjälp av**väljer **A filen**.</span><span class="sxs-lookup"><span data-stu-id="91aeb-177">For **Metadata for hello Enterprise Identity Provider will be supplied using**, select **A File**.</span></span>

    <span data-ttu-id="91aeb-178">c.</span><span class="sxs-lookup"><span data-stu-id="91aeb-178">c.</span></span> <span data-ttu-id="91aeb-179">tooupload metadata för hämtade filer, klickar du på **Välj fil**.</span><span class="sxs-lookup"><span data-stu-id="91aeb-179">tooupload your downloaded metadata file, click **Choose file**.</span></span>

    <span data-ttu-id="91aeb-180">d.</span><span class="sxs-lookup"><span data-stu-id="91aeb-180">d.</span></span> <span data-ttu-id="91aeb-181">Klicka på **SET IDENTITETSLEVERANTÖR**.</span><span class="sxs-lookup"><span data-stu-id="91aeb-181">Click **SET IDENTITY PROVIDER**.</span></span>

> [!TIP]
> <span data-ttu-id="91aeb-182">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="91aeb-182">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="91aeb-183">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="91aeb-183">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="91aeb-184">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="91aeb-184">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="91aeb-185">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="91aeb-185">Creating an Azure AD test user</span></span>
<span data-ttu-id="91aeb-186">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="91aeb-186">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="91aeb-188">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="91aeb-188">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="91aeb-189">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="91aeb-189">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-arcgis-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="91aeb-191">Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.</span><span class="sxs-lookup"><span data-stu-id="91aeb-191">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-arcgis-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="91aeb-193">Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="91aeb-193">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-arcgis-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="91aeb-195">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="91aeb-195">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-arcgis-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="91aeb-197">a.</span><span class="sxs-lookup"><span data-stu-id="91aeb-197">a.</span></span> <span data-ttu-id="91aeb-198">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="91aeb-198">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="91aeb-199">b.</span><span class="sxs-lookup"><span data-stu-id="91aeb-199">b.</span></span> <span data-ttu-id="91aeb-200">I hello **användarnamn** textruta typen hello **e-postadress** av Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="91aeb-200">In hello **User name** textbox, type hello **email address** of Britta Simon.</span></span>

    <span data-ttu-id="91aeb-201">c.</span><span class="sxs-lookup"><span data-stu-id="91aeb-201">c.</span></span> <span data-ttu-id="91aeb-202">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="91aeb-202">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="91aeb-203">d.</span><span class="sxs-lookup"><span data-stu-id="91aeb-203">d.</span></span> <span data-ttu-id="91aeb-204">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="91aeb-204">Click **Create**.</span></span>
 
### <a name="creating-an-arcgis-online-test-user"></a><span data-ttu-id="91aeb-205">Skapa en ArcGIS Online testanvändare</span><span class="sxs-lookup"><span data-stu-id="91aeb-205">Creating an ArcGIS Online test user</span></span>

<span data-ttu-id="91aeb-206">I ordning tooenable Azure AD-användare toolog till ArcGIS Online, måste de etableras i ArcGIS Online.</span><span class="sxs-lookup"><span data-stu-id="91aeb-206">In order tooenable Azure AD users toolog into ArcGIS Online, they must be provisioned into ArcGIS Online.</span></span>  
<span data-ttu-id="91aeb-207">Etablering är en manuell aktivitet i hello fallet ArcGIS online.</span><span class="sxs-lookup"><span data-stu-id="91aeb-207">In hello case of ArcGIS Online, provisioning is a manual task.</span></span>

<span data-ttu-id="91aeb-208">**tooprovision ett användarkonto, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="91aeb-208">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="91aeb-209">Logga in tooyour **ArcGIS** klient.</span><span class="sxs-lookup"><span data-stu-id="91aeb-209">Log in tooyour **ArcGIS** tenant.</span></span>

2. <span data-ttu-id="91aeb-210">Klicka på **bjuda in MEDLEMMAR**.</span><span class="sxs-lookup"><span data-stu-id="91aeb-210">Click **INVITE MEMBERS**.</span></span>
   
    <span data-ttu-id="91aeb-211">![Bjud in medlemmar](./media/active-directory-saas-arcgis-tutorial/ic784747.png "bjuda in medlemmar")</span><span class="sxs-lookup"><span data-stu-id="91aeb-211">![Invite Members](./media/active-directory-saas-arcgis-tutorial/ic784747.png "Invite Members")</span></span>

3. <span data-ttu-id="91aeb-212">Välj **lägga till medlemmar automatiskt utan att skicka ett e-postmeddelande**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="91aeb-212">Select **Add members automatically without sending an email**, and then click **NEXT**.</span></span>
   
    <span data-ttu-id="91aeb-213">![Lägga till medlemmar automatiskt](./media/active-directory-saas-arcgis-tutorial/ic784748.png "automatiskt lägga till medlemmar")</span><span class="sxs-lookup"><span data-stu-id="91aeb-213">![Add Members Automatically](./media/active-directory-saas-arcgis-tutorial/ic784748.png "Add Members Automatically")</span></span>

4. <span data-ttu-id="91aeb-214">På hello **medlemmar** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="91aeb-214">On hello **Members** dialog page, perform hello following steps:</span></span>
   
     <span data-ttu-id="91aeb-215">![Lägg till och granska](./media/active-directory-saas-arcgis-tutorial/ic784749.png "Lägg till och granska")</span><span class="sxs-lookup"><span data-stu-id="91aeb-215">![Add and review](./media/active-directory-saas-arcgis-tutorial/ic784749.png "Add and review")</span></span>
    
     <span data-ttu-id="91aeb-216">a.</span><span class="sxs-lookup"><span data-stu-id="91aeb-216">a.</span></span> <span data-ttu-id="91aeb-217">Ange hello **e-post**, **Förnamn**, och **efternamn** på en giltig AAD-konto som du vill tooprovision.</span><span class="sxs-lookup"><span data-stu-id="91aeb-217">Enter hello **Email**, **First Name**, and **Last Name** of a valid AAD account you want tooprovision.</span></span>
  
     <span data-ttu-id="91aeb-218">b.</span><span class="sxs-lookup"><span data-stu-id="91aeb-218">b.</span></span> <span data-ttu-id="91aeb-219">Klicka på **Lägg till och granska**.</span><span class="sxs-lookup"><span data-stu-id="91aeb-219">Click **ADD AND REVIEW**.</span></span>
5. <span data-ttu-id="91aeb-220">Granska hello data du har angett och klicka sedan på **lägga till MEDLEMMAR**.</span><span class="sxs-lookup"><span data-stu-id="91aeb-220">Review hello data you have entered, and then click **ADD MEMBERS**.</span></span>
   
    <span data-ttu-id="91aeb-221">![Lägg till medlem](./media/active-directory-saas-arcgis-tutorial/ic784750.png "Lägg till medlem")</span><span class="sxs-lookup"><span data-stu-id="91aeb-221">![Add member](./media/active-directory-saas-arcgis-tutorial/ic784750.png "Add member")</span></span>
        
    > [!NOTE]
    > <span data-ttu-id="91aeb-222">hello Azure Active Directory-konto innehavaren kommer ett e-postmeddelande och följ en länk tooconfirm sitt konto innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="91aeb-222">hello Azure Active Directory account holder will receive an email and follow a link tooconfirm their account before it becomes active.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="91aeb-223">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="91aeb-223">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="91aeb-224">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooArcGIS Online.</span><span class="sxs-lookup"><span data-stu-id="91aeb-224">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooArcGIS Online.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="91aeb-226">**tooassign Britta Simon tooArcGIS Online, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="91aeb-226">**tooassign Britta Simon tooArcGIS Online, perform hello following steps:**</span></span>

1. <span data-ttu-id="91aeb-227">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="91aeb-227">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="91aeb-229">Välj i listan med program hello **ArcGIS Online**.</span><span class="sxs-lookup"><span data-stu-id="91aeb-229">In hello applications list, select **ArcGIS Online**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_app.png) 

3. <span data-ttu-id="91aeb-231">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="91aeb-231">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="91aeb-233">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="91aeb-233">Click **Add** button.</span></span> <span data-ttu-id="91aeb-234">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="91aeb-234">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="91aeb-236">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="91aeb-236">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="91aeb-237">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="91aeb-237">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="91aeb-238">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="91aeb-238">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="91aeb-239">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="91aeb-239">Testing single sign-on</span></span>

<span data-ttu-id="91aeb-240">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="91aeb-240">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="91aeb-241">Du bör få automatiskt inloggade tooyour ArcGIS Online programmet när du klickar på hello ArcGIS Online panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="91aeb-241">When you click hello ArcGIS Online tile in hello Access Panel, you should get automatically signed-on tooyour ArcGIS Online application.</span></span>
<span data-ttu-id="91aeb-242">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="91aeb-242">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="91aeb-243">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="91aeb-243">Additional resources</span></span>

* [<span data-ttu-id="91aeb-244">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="91aeb-244">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="91aeb-245">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="91aeb-245">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_203.png

