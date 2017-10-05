---
title: "Självstudier: Azure Active Directory-integrering med 123ContactForm | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och 123ContactForm."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5211910a-ab96-4709-959a-524c4d57c43e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 3a99f0841c3e0d973168991f5dbee40e54c1d054
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-123contactform"></a><span data-ttu-id="0eeb4-103">Självstudier: Azure Active Directory-integrering med 123ContactForm</span><span class="sxs-lookup"><span data-stu-id="0eeb4-103">Tutorial: Azure Active Directory integration with 123ContactForm</span></span>

<span data-ttu-id="0eeb4-104">I kursen får lära du att integrera 123ContactForm med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="0eeb4-104">In this tutorial, you learn how to integrate 123ContactForm with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="0eeb4-105">Integrera 123ContactForm med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="0eeb4-105">Integrating 123ContactForm with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="0eeb4-106">Du kan styra i Azure AD som har åtkomst till 123ContactForm</span><span class="sxs-lookup"><span data-stu-id="0eeb4-106">You can control in Azure AD who has access to 123ContactForm</span></span>
- <span data-ttu-id="0eeb4-107">Du kan aktivera användarna att automatiskt hämta loggat in på 123ContactForm (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="0eeb4-107">You can enable your users to automatically get signed-on to 123ContactForm (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="0eeb4-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="0eeb4-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="0eeb4-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0eeb4-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0eeb4-110">Krav</span><span class="sxs-lookup"><span data-stu-id="0eeb4-110">Prerequisites</span></span>

<span data-ttu-id="0eeb4-111">För att konfigurera Azure AD-integrering med 123ContactForm, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="0eeb4-111">To configure Azure AD integration with 123ContactForm, you need the following items:</span></span>

- <span data-ttu-id="0eeb4-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="0eeb4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="0eeb4-113">En 123ContactForm enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="0eeb4-113">A 123ContactForm single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="0eeb4-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="0eeb4-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="0eeb4-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="0eeb4-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="0eeb4-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0eeb4-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="0eeb4-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="0eeb4-118">Scenario description</span></span>
<span data-ttu-id="0eeb4-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="0eeb4-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="0eeb4-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="0eeb4-121">Att lägga till 123ContactForm från galleriet</span><span class="sxs-lookup"><span data-stu-id="0eeb4-121">Adding 123ContactForm from the gallery</span></span>
2. <span data-ttu-id="0eeb4-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0eeb4-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-123contactform-from-the-gallery"></a><span data-ttu-id="0eeb4-123">Att lägga till 123ContactForm från galleriet</span><span class="sxs-lookup"><span data-stu-id="0eeb4-123">Adding 123ContactForm from the gallery</span></span>
<span data-ttu-id="0eeb4-124">Du måste lägga till 123ContactForm från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av 123ContactForm i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-124">To configure the integration of 123ContactForm into Azure AD, you need to add 123ContactForm from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="0eeb4-125">**Utför följande steg för att lägga till 123ContactForm från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="0eeb4-125">**To add 123ContactForm from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="0eeb4-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="0eeb4-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="0eeb4-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="0eeb4-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="0eeb4-133">I sökrutan skriver **123ContactForm**.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-133">In the search box, type **123ContactForm**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_search.png)

5. <span data-ttu-id="0eeb4-135">Välj i resultatpanelen **123ContactForm**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-135">In the results panel, select **123ContactForm**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="0eeb4-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0eeb4-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="0eeb4-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med 123ContactForm baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-138">In this section, you configure and test Azure AD single sign-on with 123ContactForm based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="0eeb4-139">Azure AD måste du känna till användaren i 123ContactForm motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-139">For single sign-on to work, Azure AD needs to know what the counterpart user in 123ContactForm is to a user in Azure AD.</span></span> <span data-ttu-id="0eeb4-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i 123ContactForm upprättas.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-140">In other words, a link relationship between an Azure AD user and the related user in 123ContactForm needs to be established.</span></span>

<span data-ttu-id="0eeb4-141">I 123ContactForm, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-141">In 123ContactForm, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="0eeb4-142">Om du vill konfigurera och testa Azure AD enkel inloggning med 123ContactForm, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="0eeb4-142">To configure and test Azure AD single sign-on with 123ContactForm, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="0eeb4-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="0eeb4-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="0eeb4-145">**[Skapa en testanvändare 123ContactForm](#creating-a-123contactform-test-user)**  – du har en motsvarighet för Britta Simon i 123ContactForm som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-145">**[Creating a 123ContactForm test user](#creating-a-123contactform-test-user)** - to have a counterpart of Britta Simon in 123ContactForm that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="0eeb4-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="0eeb4-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="0eeb4-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0eeb4-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="0eeb4-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt 123ContactForm program.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your 123ContactForm application.</span></span>

<span data-ttu-id="0eeb4-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med 123ContactForm:**</span><span class="sxs-lookup"><span data-stu-id="0eeb4-150">**To configure Azure AD single sign-on with 123ContactForm, perform the following steps:**</span></span>

1. <span data-ttu-id="0eeb4-151">I Azure-portalen på den **123ContactForm** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-151">In the Azure portal, on the **123ContactForm** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="0eeb4-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_samlbase.png)

3. <span data-ttu-id="0eeb4-155">På den **123ContactForm domän och URL: er** om du vill konfigurera programmet i **IDP initierade läge**, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="0eeb4-155">On the **123ContactForm Domain and URLs** section, If you wish to configure the application in **IDP initiated mode**, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-123contactform-tutorial/url1.png)

    <span data-ttu-id="0eeb4-157">a.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-157">a.</span></span> <span data-ttu-id="0eeb4-158">I den **identifierare** textruta Skriv en URL med följande mönster:`https://www.123contactform.com/saml/azure_ad/<tenant_id>/metadata`</span><span class="sxs-lookup"><span data-stu-id="0eeb4-158">In the **Identifier** textbox, type a URL using the following pattern: `https://www.123contactform.com/saml/azure_ad/<tenant_id>/metadata`</span></span>

    <span data-ttu-id="0eeb4-159">b.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-159">b.</span></span> <span data-ttu-id="0eeb4-160">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://www.123contactform.com/saml/azure_ad/<tenant_id>/acs`</span><span class="sxs-lookup"><span data-stu-id="0eeb4-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://www.123contactform.com/saml/azure_ad/<tenant_id>/acs`</span></span>

4. <span data-ttu-id="0eeb4-161">Om du vill konfigurera programmet i **SP initierade läge**, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="0eeb4-161">If you wish to configure the application in **SP initiated mode**, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-123contactform-tutorial/url2.png)

    <span data-ttu-id="0eeb4-163">a.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-163">a.</span></span> <span data-ttu-id="0eeb4-164">Klicka på den **visa avancerade inställningar för URL: en** alternativet</span><span class="sxs-lookup"><span data-stu-id="0eeb4-164">Click the **Show advanced URL settings** option</span></span>

    <span data-ttu-id="0eeb4-165">b.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-165">b.</span></span> <span data-ttu-id="0eeb4-166">I den **logga URL** textruta Skriv en URL som:`https://www.123contactform.com/saml/azure_ad/<tenant_id>/sso`</span><span class="sxs-lookup"><span data-stu-id="0eeb4-166">In the **Sign On URL** textbox, type a URL as: `https://www.123contactform.com/saml/azure_ad/<tenant_id>/sso`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="0eeb4-167">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-167">These values are not real.</span></span> <span data-ttu-id="0eeb4-168">Du behöver uppdatera dessa värde från faktiska URL: er och identifierare som beskrivs senare i självstudierna.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-168">You'll need to update these value from actual URLs and Identifier which is explained later in the tutorial.</span></span>
    
5. <span data-ttu-id="0eeb4-169">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-169">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_certificate.png) 

6. <span data-ttu-id="0eeb4-171">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-171">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-123contactform-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="0eeb4-173">Konfigurera enkel inloggning på **123ContactForm** sida, gå till [https://www.123contactform.com/form-2709121/](https://www.123contactform.com/form-2709121/) och utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="0eeb4-173">To configure single sign-on on **123ContactForm** side, go to [https://www.123contactform.com/form-2709121/](https://www.123contactform.com/form-2709121/) and perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-123contactform-tutorial/submit.png) 

    <span data-ttu-id="0eeb4-175">a.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-175">a.</span></span> <span data-ttu-id="0eeb4-176">I den **e-post** textruta Skriv e-postadressen för användaren engångsfaktorautentisering</span><span class="sxs-lookup"><span data-stu-id="0eeb4-176">In the **Email** textbox, type the email of the user i.e</span></span> <span data-ttu-id="0eeb4-177">**BrittaSimon@Contoso.com**.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-177">**BrittaSimon@Contoso.com**.</span></span>

    <span data-ttu-id="0eeb4-178">b.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-178">b.</span></span> <span data-ttu-id="0eeb4-179">Klicka på **överför** och bläddra i XML-Metadata för filen som du har hämtat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-179">Click **Upload** and browse the Metadata XML file, which you have downloaded from Azure portal.</span></span>

    <span data-ttu-id="0eeb4-180">c.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-180">c.</span></span> <span data-ttu-id="0eeb4-181">Klicka på **skicka formuläret**.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-181">Click **SUBMIT FORM**.</span></span>

8. <span data-ttu-id="0eeb4-182">På den **konfigurera Appinställningar i Microsoft Azure AD enkel inloggning -** utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="0eeb4-182">On the **Microsoft Azure AD - Single sign-on - Configure App Settings** perform the following steps:</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-123contactform-tutorial/url3.png)

    <span data-ttu-id="0eeb4-184">a.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-184">a.</span></span> <span data-ttu-id="0eeb4-185">Om du vill konfigurera programmet i **IDP initierade läge**, kopiera den **IDENTIFIERARE** värde för din instans och klistra in den i **identifierare** TextBox-kontroll i **123ContactForm domän och URL: er** avsnitt på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-185">If you wish to configure the application in **IDP initiated mode**, copy the **IDENTIFIER** value for your instance and paste it in **Identifier** textbox in **123ContactForm Domain and URLs** section on Azure portal.</span></span>
    
    <span data-ttu-id="0eeb4-186">b.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-186">b.</span></span> <span data-ttu-id="0eeb4-187">Om du vill konfigurera programmet i **IDP initierade läge**, kopiera den **REPLY URL** värde för din instans och klistra in den i **Reply URL** TextBox-kontroll i **123ContactForm domän och URL: er** avsnitt på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-187">If you wish to configure the application in **IDP initiated mode**, copy the **REPLY URL** value for your instance and paste it in **Reply URL** textbox in **123ContactForm Domain and URLs** section on Azure portal.</span></span>

    <span data-ttu-id="0eeb4-188">c.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-188">c.</span></span> <span data-ttu-id="0eeb4-189">Om du vill konfigurera programmet i **SP initierade läge**, kopiera den **SIGN ON URL** värde för din instans och klistra in den i **logga URL** TextBox-kontroll i **123ContactForm domän och URL: er** avsnitt på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-189">If you wish to configure the application in **SP initiated mode**, copy the **SIGN ON URL** value for your instance and paste it in **Sign On URL** textbox in **123ContactForm Domain and URLs** section on Azure portal.</span></span>

> [!TIP]
> <span data-ttu-id="0eeb4-190">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="0eeb4-190">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="0eeb4-191">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-191">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="0eeb4-192">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="0eeb4-192">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="0eeb4-193">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="0eeb4-193">Creating an Azure AD test user</span></span>
<span data-ttu-id="0eeb4-194">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-194">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="0eeb4-196">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="0eeb4-196">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="0eeb4-197">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-197">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-123contactform-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="0eeb4-199">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-199">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-123contactform-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="0eeb4-201">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-201">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-123contactform-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="0eeb4-203">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="0eeb4-203">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-123contactform-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="0eeb4-205">a.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-205">a.</span></span> <span data-ttu-id="0eeb4-206">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-206">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="0eeb4-207">b.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-207">b.</span></span> <span data-ttu-id="0eeb4-208">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-208">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="0eeb4-209">c.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-209">c.</span></span> <span data-ttu-id="0eeb4-210">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-210">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="0eeb4-211">d.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-211">d.</span></span> <span data-ttu-id="0eeb4-212">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-212">Click **Create**.</span></span>
 
### <a name="creating-a-123contactform-test-user"></a><span data-ttu-id="0eeb4-213">Skapa en testanvändare 123ContactForm</span><span class="sxs-lookup"><span data-stu-id="0eeb4-213">Creating a 123ContactForm test user</span></span>

<span data-ttu-id="0eeb4-214">Programmet stöder bara i tid användaretablering och authentication-användare kommer automatiskt att skapas i programmet.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-214">Application supports Just in time user provisioning and after authentication users will be created in the application automatically.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="0eeb4-215">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="0eeb4-215">Assigning the Azure AD test user</span></span>

<span data-ttu-id="0eeb4-216">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till 123ContactForm.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-216">In this section, you enable Britta Simon to use Azure single sign-on by granting access to 123ContactForm.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="0eeb4-218">**Om du vill tilldela 123ContactForm Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="0eeb4-218">**To assign Britta Simon to 123ContactForm, perform the following steps:**</span></span>

1. <span data-ttu-id="0eeb4-219">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-219">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="0eeb4-221">Välj i listan med program **123ContactForm**.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-221">In the applications list, select **123ContactForm**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_app.png) 

3. <span data-ttu-id="0eeb4-223">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-223">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="0eeb4-225">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-225">Click **Add** button.</span></span> <span data-ttu-id="0eeb4-226">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-226">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="0eeb4-228">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-228">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="0eeb4-229">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-229">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="0eeb4-230">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-230">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="0eeb4-231">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0eeb4-231">Testing single sign-on</span></span>

<span data-ttu-id="0eeb4-232">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-232">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="0eeb4-233">När du klickar på panelen 123ContactForm på åtkomstpanelen du bör få automatiskt loggat in på ditt 123ContactForm program.</span><span class="sxs-lookup"><span data-stu-id="0eeb4-233">When you click the 123ContactForm tile in the Access Panel, you should get automatically signed-on to your 123ContactForm application.</span></span>
<span data-ttu-id="0eeb4-234">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="0eeb4-234">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0eeb4-235">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="0eeb4-235">Additional resources</span></span>

* [<span data-ttu-id="0eeb4-236">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0eeb4-236">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0eeb4-237">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="0eeb4-237">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_203.png

