---
title: "Självstudier: Azure Active Directory-integrering med Amazon Web Services (AWS) | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Amazon Web Services (AWS)."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7561c20b-2325-4d97-887f-693aa383c7be
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 1b79572ace63f6174ce4fa014c49bf44bd728228
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-amazon-web-services-aws"></a><span data-ttu-id="9eb2a-103">Självstudier: Azure Active Directory-integrering med Amazon Web Services (AWS)</span><span class="sxs-lookup"><span data-stu-id="9eb2a-103">Tutorial: Azure Active Directory integration with Amazon Web Services (AWS)</span></span>

<span data-ttu-id="9eb2a-104">I kursen får du lära dig hur toointegrate Amazon Web Services (AWS) med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="9eb2a-104">In this tutorial, you learn how toointegrate Amazon Web Services (AWS) with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9eb2a-105">Integrera Amazon Web Services (AWS) med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="9eb2a-105">Integrating Amazon Web Services (AWS) with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="9eb2a-106">Du kan styra i Azure AD som har åtkomst tooAmazon Web Services (AWS)</span><span class="sxs-lookup"><span data-stu-id="9eb2a-106">You can control in Azure AD who has access tooAmazon Web Services (AWS)</span></span>
- <span data-ttu-id="9eb2a-107">Du kan aktivera din användare tooautomatically get inloggade tooAmazon Web Services (AWS) (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="9eb2a-107">You can enable your users tooautomatically get signed-on tooAmazon Web Services (AWS) (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9eb2a-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="9eb2a-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="9eb2a-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9eb2a-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

<!--## Overview

tooenable single sign-on with Amazon Web Services (AWS), it must be configured toouse Azure Active Directory as an identity provider. This guide provides information and tips on how tooperform this configuration in Amazon Web Services (AWS).

>[!Note]: 
>This embedded guide is brand new in hello new Azure portal, and we’d love toohear your thoughts. Use hello Feedback ? button at hello top of hello portal tooprovide feedback. hello older guide for using hello [Azure classic portal](https://manage.windowsazure.com) tooconfigure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a><span data-ttu-id="9eb2a-110">Krav</span><span class="sxs-lookup"><span data-stu-id="9eb2a-110">Prerequisites</span></span>

<span data-ttu-id="9eb2a-111">tooconfigure Azure AD-integrering med Amazon Web Services (AWS) behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="9eb2a-111">tooconfigure Azure AD integration with Amazon Web Services (AWS), you need hello following items:</span></span>

- <span data-ttu-id="9eb2a-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="9eb2a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9eb2a-113">Amazon Web Services (AWS) enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="9eb2a-113">Amazon Web Services (AWS) single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9eb2a-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9eb2a-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="9eb2a-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9eb2a-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="9eb2a-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9eb2a-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9eb2a-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="9eb2a-118">Scenario description</span></span>
<span data-ttu-id="9eb2a-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9eb2a-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="9eb2a-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9eb2a-121">Att lägga till Amazon Web Services (AWS) från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="9eb2a-121">Adding Amazon Web Services (AWS) from hello gallery</span></span>
2. <span data-ttu-id="9eb2a-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9eb2a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-amazon-web-services-aws-from-hello-gallery"></a><span data-ttu-id="9eb2a-123">Att lägga till Amazon Web Services (AWS) från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="9eb2a-123">Adding Amazon Web Services (AWS) from hello gallery</span></span>
<span data-ttu-id="9eb2a-124">tooconfigure hello integrering av Amazon Web Services (AWS) i Azure AD, behöver du tooadd Amazon Web Services (AWS) hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-124">tooconfigure hello integration of Amazon Web Services (AWS) into Azure AD, you need tooadd Amazon Web Services (AWS) from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="9eb2a-125">**tooadd Amazon Web Services (AWS) från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="9eb2a-125">**tooadd Amazon Web Services (AWS) from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="9eb2a-126">I hello  **[Azure Portal](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-126">In hello **[Azure Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9eb2a-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="9eb2a-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="9eb2a-131">Klicka på **Lägg till** hello längst upp i hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="9eb2a-133">Skriv i sökrutan hello **Amazon Web Services (AWS)**.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-133">In hello search box, type **Amazon Web Services (AWS)**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_search.png)

5. <span data-ttu-id="9eb2a-135">Markera hello resultat på panelen **Amazon Web Services (AWS)**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-135">In hello results panel, select **Amazon Web Services (AWS)**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9eb2a-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9eb2a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9eb2a-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Amazon Web Services (AWS) baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-138">In this section, you configure and test Azure AD single sign-on with Amazon Web Services (AWS) based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="9eb2a-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Amazon Web Services (AWS) är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Amazon Web Services (AWS) is tooa user in Azure AD.</span></span> <span data-ttu-id="9eb2a-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i Amazon Web Services (AWS) toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-140">In other words, a link relationship between an Azure AD user and hello related user in Amazon Web Services (AWS) needs toobe established.</span></span>

<span data-ttu-id="9eb2a-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Amazon Web Services (AWS).</span><span class="sxs-lookup"><span data-stu-id="9eb2a-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Amazon Web Services (AWS).</span></span>

<span data-ttu-id="9eb2a-142">tooconfigure och testa Azure AD enkel inloggning med Amazon Web Services (AWS), behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="9eb2a-142">tooconfigure and test Azure AD single sign-on with Amazon Web Services (AWS), you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="9eb2a-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="9eb2a-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9eb2a-145">**[Skapa en testanvändare Amazon Web Services](#creating-an-amazon-web-services-test-user)**  -toohave en motsvarighet för Britta Simon i Amazon Web Services (AWS) och som är länkade toohello Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-145">**[Creating an Amazon Web Services test user](#creating-an-amazon-web-services-test-user)** - toohave a counterpart of Britta Simon in Amazon Web Services (AWS) that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="9eb2a-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9eb2a-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9eb2a-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9eb2a-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9eb2a-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Amazon Web Services (AWS).</span><span class="sxs-lookup"><span data-stu-id="9eb2a-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Amazon Web Services (AWS) application.</span></span>

<span data-ttu-id="9eb2a-150">**tooconfigure Azure AD enkel inloggning med Amazon Web Services (AWS) utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="9eb2a-150">**tooconfigure Azure AD single sign-on with Amazon Web Services (AWS), perform hello following steps:**</span></span>

1. <span data-ttu-id="9eb2a-151">I hello Azure-portalen på hello **Amazon Web Services (AWS)** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-151">In hello Azure Portal, on hello **Amazon Web Services (AWS)** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="9eb2a-153">På hello **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_samlbase.png)

3. <span data-ttu-id="9eb2a-155">På hello **Amazon Web Services (AWS)-domän och URL: er** avsnittet, hello användaren har inte tooperform alla steg som hello app redan redan är integrerade med Azure.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-155">On hello **Amazon Web Services (AWS) Domain and URLs** section, hello user does not have tooperform any steps as hello app is already pre-integrated with Azure.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_url.png)

4. <span data-ttu-id="9eb2a-157">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara hello XML-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-157">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_certificate.png)

5. <span data-ttu-id="9eb2a-159">hello Amazon Web Services (AWS) program förväntar hello SAML intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-159">hello Amazon Web Services (AWS) Software application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="9eb2a-160">Konfigurera hello följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-160">Please configure hello following claims for this application.</span></span> <span data-ttu-id="9eb2a-161">Du kan hantera hello värden för attributen från hello ”**användarattribut**” avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-161">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="9eb2a-162">hello följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-162">hello following screenshot shows an example for this.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_attribute.png)

6. <span data-ttu-id="9eb2a-164">I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan Konfigurera SAML-token attribut som visas i hello bilden ovan och utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9eb2a-164">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image above and perform hello following steps:</span></span>
    
    | <span data-ttu-id="9eb2a-165">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="9eb2a-165">Attribute Name</span></span>  | <span data-ttu-id="9eb2a-166">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="9eb2a-166">Attribute Value</span></span> | <span data-ttu-id="9eb2a-167">namnområde</span><span class="sxs-lookup"><span data-stu-id="9eb2a-167">Namespace</span></span> |
    | --------------- | --------------- | --------------- |
    | <span data-ttu-id="9eb2a-168">RoleSessionName</span><span class="sxs-lookup"><span data-stu-id="9eb2a-168">RoleSessionName</span></span> | <span data-ttu-id="9eb2a-169">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="9eb2a-169">user.userprincipalname</span></span> | <span data-ttu-id="9eb2a-170">https://AWS.Amazon.com/SAML/Attributes</span><span class="sxs-lookup"><span data-stu-id="9eb2a-170">https://aws.amazon.com/SAML/Attributes</span></span> |
    | <span data-ttu-id="9eb2a-171">Roll</span><span class="sxs-lookup"><span data-stu-id="9eb2a-171">Role</span></span>            | <span data-ttu-id="9eb2a-172">User.assignedroles</span><span class="sxs-lookup"><span data-stu-id="9eb2a-172">user.assignedroles</span></span> |  <span data-ttu-id="9eb2a-173">https://AWS.Amazon.com/SAML/Attributes</span><span class="sxs-lookup"><span data-stu-id="9eb2a-173">https://aws.amazon.com/SAML/Attributes</span></span> |
    
    >[!TIP]
    ><span data-ttu-id="9eb2a-174">Du måste tooconfigure hello användaretablering i Azure AD toofetch alla hello roller från AWS-konsolen.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-174">You need tooconfigure hello user provisioning in Azure AD toofetch all hello roles from AWS Console.</span></span> <span data-ttu-id="9eb2a-175">Se hello etablering stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-175">Please refer hello provisioning steps below.</span></span>

    <span data-ttu-id="9eb2a-176">a.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-176">a.</span></span> <span data-ttu-id="9eb2a-177">Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-177">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_04.png)

    <span data-ttu-id="9eb2a-179">b.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-179">b.</span></span> <span data-ttu-id="9eb2a-180">I hello **namn** textruta hello attributnamn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-180">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="9eb2a-182">c.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-182">c.</span></span> <span data-ttu-id="9eb2a-183">Från hello **värdet** listan attributvärde för typ hello visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-183">From hello **Value** list, type hello attribute value shown for that row.</span></span> <span data-ttu-id="9eb2a-184">Lägg till hello Namespace-värde som anges ovan.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-184">Add hello Namespace value as given above.</span></span>
    
    <span data-ttu-id="9eb2a-185">d.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-185">d.</span></span> <span data-ttu-id="9eb2a-186">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-186">Click **Ok**.</span></span>

7. <span data-ttu-id="9eb2a-187">Klicka på **spara** knappen toosave hello inställningar på Azure.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-187">Click **Save** button toosave hello settings on Azure.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="9eb2a-189">I ett annat fönster i webbläsaren, inloggning tooyour Amazon Web Services (AWS) företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-189">In a different browser window, sign-on tooyour Amazon Web Services (AWS) company site as administrator.</span></span>

9. <span data-ttu-id="9eb2a-190">Klicka på **konsolen Home**.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-190">Click **Console Home**.</span></span>
   
    ![Konfigurera enkel inloggning][11]

10. <span data-ttu-id="9eb2a-192">Klicka på **IAM** från **säkerhet, identitet och efterlevnad** service.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-192">Click **IAM** from **Security, Identity & Compliance** service.</span></span>
   
    ![Konfigurera enkel inloggning][12]

11. <span data-ttu-id="9eb2a-194">Klicka på **identitetsleverantörer**, och klicka sedan på **skapa providern**.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-194">Click **Identity Providers**, and then click **Create Provider**.</span></span>
   
    ![Konfigurera enkel inloggning][13]

12. <span data-ttu-id="9eb2a-196">På hello **konfigurera Provider** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9eb2a-196">On hello **Configure Provider** dialog page, perform hello following steps:</span></span>
   
    ![Konfigurera enkel inloggning][14]
 
    <span data-ttu-id="9eb2a-198">a.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-198">a.</span></span> <span data-ttu-id="9eb2a-199">Som **providertyp**väljer **SAML**.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-199">As **Provider Type**, select **SAML**.</span></span>

    <span data-ttu-id="9eb2a-200">b.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-200">b.</span></span> <span data-ttu-id="9eb2a-201">I hello **providernamn** textruta skriver ett provider-namn (t.ex.: *trä*).</span><span class="sxs-lookup"><span data-stu-id="9eb2a-201">In hello **Provider Name** textbox, type a provider name (e.g.: *WAAD*).</span></span>

    <span data-ttu-id="9eb2a-202">c.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-202">c.</span></span> <span data-ttu-id="9eb2a-203">tooupload metadata för hämtade filer, klickar du på **Välj fil**.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-203">tooupload your downloaded metadata file, click **Choose File**.</span></span>

    <span data-ttu-id="9eb2a-204">d.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-204">d.</span></span> <span data-ttu-id="9eb2a-205">Klicka på **nästa steg**.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-205">Click **Next Step**.</span></span>

13. <span data-ttu-id="9eb2a-206">På hello **Kontrollera Providerinformationen** dialogrutan sidan, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-206">On hello **Verify Provider Information** dialog page, click **Create**.</span></span> 
    
    ![Konfigurera enkel inloggning][15]

14. <span data-ttu-id="9eb2a-208">Klicka på **roller**, och klicka sedan på **Skapa ny roll**.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-208">Click **Roles**, and then click **Create New Role**.</span></span> 
    
    ![Konfigurera enkel inloggning][16]

15. <span data-ttu-id="9eb2a-210">På hello **ange rollnamn** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9eb2a-210">On hello **Set Role Name** dialog, perform hello following steps:</span></span> 
    
    ![Konfigurera enkel inloggning][17] 

    <span data-ttu-id="9eb2a-212">a.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-212">a.</span></span> <span data-ttu-id="9eb2a-213">I hello **rollnamn** textruta, ange ett rollnamn (t.ex.: *TestUser*).</span><span class="sxs-lookup"><span data-stu-id="9eb2a-213">In hello **Role Name** textbox, type a role name (e.g.: *TestUser*).</span></span> 

    <span data-ttu-id="9eb2a-214">b.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-214">b.</span></span> <span data-ttu-id="9eb2a-215">Klicka på **nästa steg**.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-215">Click **Next Step**.</span></span>

16. <span data-ttu-id="9eb2a-216">På hello **Välj typ av roll** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9eb2a-216">On hello **Select Role Type** dialog, perform hello following steps:</span></span> 
    
    ![Konfigurera enkel inloggning][18] 

    <span data-ttu-id="9eb2a-218">a.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-218">a.</span></span> <span data-ttu-id="9eb2a-219">Välj **roll för identitets-providerns åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-219">Select **Role For Identity Provider Access**.</span></span> 

    <span data-ttu-id="9eb2a-220">b.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-220">b.</span></span> <span data-ttu-id="9eb2a-221">I hello **bevilja Web Single Sign-On (WebSSO) åtkomst tooSAML providers** klickar du på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-221">In hello **Grant Web Single Sign-On (WebSSO) access tooSAML providers** section, click **Select**.</span></span>

17. <span data-ttu-id="9eb2a-222">På hello **upprätta förtroende** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9eb2a-222">On hello **Establish Trust** dialog, perform hello following steps:</span></span>  
    
    ![Konfigurera enkel inloggning][19] 

    <span data-ttu-id="9eb2a-224">a.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-224">a.</span></span> <span data-ttu-id="9eb2a-225">Som SAML-provider väljer hello SAML-provider som du skapade tidigare (t.ex.: *trä*)</span><span class="sxs-lookup"><span data-stu-id="9eb2a-225">As SAML provider, select hello SAML provider you have created previously (e.g.: *WAAD*)</span></span>
  
    <span data-ttu-id="9eb2a-226">b.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-226">b.</span></span> <span data-ttu-id="9eb2a-227">Klicka på **nästa steg**.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-227">Click **Next Step**.</span></span>

18. <span data-ttu-id="9eb2a-228">På hello **Kontrollera rollen förtroende** dialogrutan klickar du på **nästa steg**.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-228">On hello **Verify Role Trust** dialog, click **Next Step**.</span></span>
    
    ![Konfigurera enkel inloggning][32]

19. <span data-ttu-id="9eb2a-230">På hello **koppla principen** dialogrutan klickar du på **nästa steg**.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-230">On hello **Attach Policy** dialog, click **Next Step**.</span></span>
    
    ![Konfigurera enkel inloggning][33]

20. <span data-ttu-id="9eb2a-232">På hello **granska** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9eb2a-232">On hello **Review** dialog, perform hello following steps:</span></span>
    
    ![Konfigurera enkel inloggning][34]
 
    <span data-ttu-id="9eb2a-234">a.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-234">a.</span></span> <span data-ttu-id="9eb2a-235">Klicka på **skapa roll**.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-235">Click **Create Role**.</span></span>

    <span data-ttu-id="9eb2a-236">b.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-236">b.</span></span> <span data-ttu-id="9eb2a-237">Skapa så många roller vid behov och mappa dem toohello identitetsleverantör.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-237">Create as many roles as needed and map them toohello Identity Provider.</span></span>

21. <span data-ttu-id="9eb2a-238">Nu konfigurera hello användaretablering toofetch alla hello roller från AWS</span><span class="sxs-lookup"><span data-stu-id="9eb2a-238">Now configure hello user provisioning toofetch all hello roles from AWS</span></span>

    <span data-ttu-id="9eb2a-239">a.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-239">a.</span></span> <span data-ttu-id="9eb2a-240">I hello AWS-konsolen logga in med ditt konto rot</span><span class="sxs-lookup"><span data-stu-id="9eb2a-240">In hello AWS Console login with your root account</span></span>

    <span data-ttu-id="9eb2a-241">b.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-241">b.</span></span> <span data-ttu-id="9eb2a-242">Klicka på ditt namn i hello övre högra hörnet och klicka sedan på hello **Mina säkerhetsreferenser** alternativet.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-242">In hello top right corner click your name and then click hello **My Security Credentials** option.</span></span> <span data-ttu-id="9eb2a-243">Då öppnas en skärm som en varning visas.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-243">This will open up a screen as a warning message.</span></span> <span data-ttu-id="9eb2a-244">Klicka på knappen hello **säkerhetsreferenser** knappen toopass hello-skärmen.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-244">Click hello button **Security Credentials** button toopass hello screen.</span></span>
        
       ![Konfigurera enkel inloggning][36]

       ![Konfigurera enkel inloggning][37]

    <span data-ttu-id="9eb2a-247">c.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-247">c.</span></span> <span data-ttu-id="9eb2a-248">I hello snabbtangenter avsnittet klickar du på hello **skapa nya åtkomstnyckel** knappen.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-248">In hello Access Keys section click hello **Create New Access Key** button.</span></span> <span data-ttu-id="9eb2a-249">Detta genererar hello åtkomst nyckel-ID och ett token värde.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-249">This generates hello Access Key ID and a token value.</span></span>
    
       ![Konfigurera enkel inloggning][38]

    <span data-ttu-id="9eb2a-251">d.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-251">d.</span></span> <span data-ttu-id="9eb2a-252">Kopiera båda dessa värden och även hämta den, så att du inte förlorar den.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-252">Copy both these values and also download it, so that you don't lose it.</span></span>

    <span data-ttu-id="9eb2a-253">e.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-253">e.</span></span> <span data-ttu-id="9eb2a-254">Klicka på hello Azure-portalen på sidan för hello Amazon Web Services (AWS) programmet integration, **etablering**.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-254">In hello Azure Portal, on hello Amazon Web Services (AWS) application integration page, click **Provisioning**.</span></span>
        
       ![Konfigurera enkel inloggning][35]

    <span data-ttu-id="9eb2a-256">f.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-256">f.</span></span> <span data-ttu-id="9eb2a-257">Ange hello etablering läge för**automatisk**</span><span class="sxs-lookup"><span data-stu-id="9eb2a-257">Set hello Provisioning mode too**Automatic**</span></span>
        
       ![Konfigurera enkel inloggning][39]

    <span data-ttu-id="9eb2a-259">g.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-259">g.</span></span> <span data-ttu-id="9eb2a-260">Nu i hello **clientsecret** och **hemlighet Token** klistra in hello motsvarande värden som du har kopierat från AWS-konsolen.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-260">Now in hello **clientsecret** and **Secret Token** paste hello corresponding values, which you have copied from AWS Console.</span></span>
    
    <span data-ttu-id="9eb2a-261">h.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-261">h.</span></span> <span data-ttu-id="9eb2a-262">Du kan klicka på hello **Testanslutningen** knappen tootest hello anslutningen.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-262">You can click hello **Test Connection** button tootest hello connectivity.</span></span> <span data-ttu-id="9eb2a-263">När det går bra kan du starta hello etablerar anslutningen.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-263">Once that is successful then you can start hello provisioning connector.</span></span>
       
       ![Konfigurera enkel inloggning][40]

    <span data-ttu-id="9eb2a-265">Jag.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-265">i.</span></span> <span data-ttu-id="9eb2a-266">Aktivera nu hello Status för etablering för**på**.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-266">Now enable hello Provisioning Status too**On**.</span></span> <span data-ttu-id="9eb2a-267">Detta startar hämtning hello roller från hello program.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-267">This starts fetching hello roles from hello application.</span></span>

       ![Konfigurera enkel inloggning][41]

    > [!NOTE]
    > <span data-ttu-id="9eb2a-269">Azure AD-etablering-tjänsten körs varje efter vissa tid toosync hello roller från AWS.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-269">Azure AD Provisioning service runs every after some time toosync hello roles from AWS.</span></span> <span data-ttu-id="9eb2a-270">Du bör se alla hello identitetsleverantör ansluten AWS roller till Azure AD och du kan använda dem när hello programmet toousers eller grupper.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-270">You should see all hello Identity Provider attached AWS roles into Azure AD and you can use them while assigning hello application toousers or groups.</span></span>

<!--### Next steps

tooensure users can sign-in tooAmazon Web Services (AWS) after it has been configured toouse Azure Active Directory, review hello following tasks and topics:

- User accounts must be pre-provisioned into Amazon Web Services (AWS) prior toosign-in. tooset this up, see Provisioning.
 
- Users must be assigned access tooAmazon Web Services (AWS) in Azure AD toosign-in. tooassign users, see Users.
 
- tooconfigure access polices for Amazon Web Services (AWS) users, see Access Policies.
 
- For additional information on deploying single sign-on toousers, see [this article](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9eb2a-271">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="9eb2a-271">Creating an Azure AD test user</span></span>
<span data-ttu-id="9eb2a-272">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-272">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="9eb2a-274">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="9eb2a-274">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="9eb2a-275">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-275">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9eb2a-277">Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-277">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9eb2a-279">Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-279">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9eb2a-281">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9eb2a-281">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9eb2a-283">a.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-283">a.</span></span> <span data-ttu-id="9eb2a-284">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-284">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9eb2a-285">b.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-285">b.</span></span> <span data-ttu-id="9eb2a-286">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-286">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9eb2a-287">c.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-287">c.</span></span> <span data-ttu-id="9eb2a-288">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-288">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="9eb2a-289">d.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-289">d.</span></span> <span data-ttu-id="9eb2a-290">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-290">Click **Create**.</span></span>
 
### <a name="creating-an-amazon-web-services-test-user"></a><span data-ttu-id="9eb2a-291">Skapa en testanvändare Amazon Web Services</span><span class="sxs-lookup"><span data-stu-id="9eb2a-291">Creating an Amazon Web Services test user</span></span>

<span data-ttu-id="9eb2a-292">I ordning tooenable Azure AD-användare toolog i tooAmazon Web Services (AWS), måste de etablerats i Amazon Web Services (AWS).</span><span class="sxs-lookup"><span data-stu-id="9eb2a-292">In order tooenable Azure AD users toolog in tooAmazon Web Services (AWS), they must be provisioned into Amazon Web Services (AWS).</span></span> <span data-ttu-id="9eb2a-293">I hello fallet för Amazon Web Services (AWS) är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-293">In hello case of Amazon Web Services (AWS), provisioning is a manual task.</span></span>

<span data-ttu-id="9eb2a-294">**tooprovision ett användarkonto, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="9eb2a-294">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="9eb2a-295">Logga in tooyour **Amazon Web Services (AWS)** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-295">Log in tooyour **Amazon Web Services (AWS)** company site as administrator.</span></span>

2. <span data-ttu-id="9eb2a-296">Klicka på hello **konsolen Home** ikon.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-296">Click hello **Console Home** icon.</span></span> 
   
    ![Konfigurera enkel inloggning][11]

3. <span data-ttu-id="9eb2a-298">Klicka på identitet och åtkomsthantering.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-298">Click Identity and Access Management.</span></span> 
   
    ![Konfigurera enkel inloggning][28]

4. <span data-ttu-id="9eb2a-300">I hello instrumentpanelen, klickar du på **användare**, och klicka sedan på **Create New Users**.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-300">In hello Dashboard, click **Users**, and then click **Create New Users**.</span></span> 
   
    ![Konfigurera enkel inloggning][29]

5. <span data-ttu-id="9eb2a-302">Dialogrutan Skapa användare hello utför hello följande:</span><span class="sxs-lookup"><span data-stu-id="9eb2a-302">On hello Create User dialog, perform hello following steps:</span></span> 
   
    ![Konfigurera enkel inloggning][30]   
    
    <span data-ttu-id="9eb2a-304">a.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-304">a.</span></span> <span data-ttu-id="9eb2a-305">I hello **ange användarnamn** textrutor, Skriv Brita Simon användarnamn (userprincipalname) i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-305">In hello **Enter User Names** textboxes, type Brita Simon's user name (userprincipalname) in Azure AD.</span></span>

    <span data-ttu-id="9eb2a-306">b.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-306">b.</span></span> <span data-ttu-id="9eb2a-307">Klicka på **skapa.**</span><span class="sxs-lookup"><span data-stu-id="9eb2a-307">Click **Create.**</span></span>
        
### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="9eb2a-308">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="9eb2a-308">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="9eb2a-309">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att ge sina access tooAmazon Web Services (AWS).</span><span class="sxs-lookup"><span data-stu-id="9eb2a-309">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooAmazon Web Services (AWS).</span></span>

![Tilldela användare][200] 

<span data-ttu-id="9eb2a-311">**tooassign Britta Simon tooAmazon Web Services (AWS), utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="9eb2a-311">**tooassign Britta Simon tooAmazon Web Services (AWS), perform hello following steps:**</span></span>

1. <span data-ttu-id="9eb2a-312">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-312">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="9eb2a-314">Välj i listan med program hello **Amazon Web Services (AWS)**.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-314">In hello applications list, select **Amazon Web Services (AWS)**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_app.png) 

3. <span data-ttu-id="9eb2a-316">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-316">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="9eb2a-318">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-318">Click **Add** button.</span></span> <span data-ttu-id="9eb2a-319">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-319">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="9eb2a-321">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-321">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="9eb2a-322">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-322">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9eb2a-323">På **Välj roll** fliken, väljer hello rätt roll för hello användare.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-323">On **Select Role** tab, select hello appropriate role for hello user.</span></span> <span data-ttu-id="9eb2a-324">Dessa roller visas med hello rollnamnet och identitetsnamn för providern.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-324">All these roles are shown with hello role name and identity provider name.</span></span> <span data-ttu-id="9eb2a-325">Det här sättet kan du enkelt identifiera hello roller från AWS.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-325">This way you can easily identify hello roles from AWS.</span></span>

8. <span data-ttu-id="9eb2a-326">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-326">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="9eb2a-327">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9eb2a-327">Testing single sign-on</span></span>

<span data-ttu-id="9eb2a-328">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-328">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="9eb2a-329">När du klickar på hello Amazon Web Services (AWS) panelen i hello åtkomstpanelen får automatiskt inloggade tooyour Amazon Web Services (AWS) program.</span><span class="sxs-lookup"><span data-stu-id="9eb2a-329">When you click hello Amazon Web Services (AWS) tile in hello Access Panel, you should get automatically signed-on tooyour Amazon Web Services (AWS) application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="9eb2a-330">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="9eb2a-330">Additional resources</span></span>

* [<span data-ttu-id="9eb2a-331">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9eb2a-331">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9eb2a-332">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9eb2a-332">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_203.png
[11]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795031.png
[12]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795032.png
[13]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795033.png
[14]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795034.png
[15]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795035.png
[16]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795022.png
[17]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795023.png
[18]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795024.png
[19]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795025.png
[20]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950351.png
[21]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_80.png
[22]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950352.png
[23]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_81.png
[24]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950353.png
[25]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_15.png

[28]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950321.png
[29]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795037.png
[30]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795038.png
[32]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950251.png
[33]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950252.png
[34]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950253.png
[35]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning.png
[36]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials.png
[37]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials_continue.png
[38]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_createnewaccesskey.png
[39]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_automatic.png
[40]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_testconnection.png
[41]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_on.png
