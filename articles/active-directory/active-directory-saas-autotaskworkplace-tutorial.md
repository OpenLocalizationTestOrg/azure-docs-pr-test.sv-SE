---
title: "Självstudier: Azure Active Directory-integrering med Autotask arbetsplats | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Autotask arbetsplats."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: a9a7ff71-c389-4169-aafd-d7a505244797
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 45130162271b20860607497ff93c6a668c415233
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-autotask-workplace"></a><span data-ttu-id="cc776-103">Självstudier: Azure Active Directory-integrering med Autotask arbetsplats</span><span class="sxs-lookup"><span data-stu-id="cc776-103">Tutorial: Azure Active Directory integration with Autotask Workplace</span></span>

<span data-ttu-id="cc776-104">I kursen får lära du att integrera Autotask arbetsplats med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="cc776-104">In this tutorial, you learn how to integrate Autotask Workplace with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="cc776-105">Integrera Autotask arbetsplats med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="cc776-105">Integrating Autotask Workplace with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="cc776-106">Du kan styra i Azure AD som har åtkomst till arbetsplats Autotask</span><span class="sxs-lookup"><span data-stu-id="cc776-106">You can control in Azure AD who has access to Autotask Workplace</span></span>
- <span data-ttu-id="cc776-107">Du kan aktivera användarna att automatiskt hämta inloggade Autotask arbetsyta (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="cc776-107">You can enable your users to automatically get signed-on to Autotask Workplace (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="cc776-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="cc776-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="cc776-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="cc776-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cc776-110">Krav</span><span class="sxs-lookup"><span data-stu-id="cc776-110">Prerequisites</span></span>

<span data-ttu-id="cc776-111">För att konfigurera Azure AD-integrering med Autotask arbetsplats, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="cc776-111">To configure Azure AD integration with Autotask Workplace, you need the following items:</span></span>

- <span data-ttu-id="cc776-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="cc776-112">An Azure AD subscription</span></span>
- <span data-ttu-id="cc776-113">En Autotask arbetsplatsen enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="cc776-113">An Autotask Workplace single-sign on enabled subscription</span></span>
- <span data-ttu-id="cc776-114">Du måste vara administratör eller super administratör i arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="cc776-114">You must be an administrator or super administrator in Workplace.</span></span>
- <span data-ttu-id="cc776-115">Du måste ha ett administratörskontot i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cc776-115">You must have an administrator account in the Azure AD.</span></span>
- <span data-ttu-id="cc776-116">Användare som använder den här funktionen måste ha konton inom arbetsplats och Azure AD och deras e-postadresser för både måste matcha.</span><span class="sxs-lookup"><span data-stu-id="cc776-116">The users that will utilize this feature must have accounts within Workplace and the Azure AD, and their email addresses for both must match.</span></span>

> [!NOTE]
> <span data-ttu-id="cc776-117">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="cc776-117">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="cc776-118">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="cc776-118">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="cc776-119">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="cc776-119">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="cc776-120">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="cc776-120">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="cc776-121">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="cc776-121">Scenario description</span></span>
<span data-ttu-id="cc776-122">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="cc776-122">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="cc776-123">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="cc776-123">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="cc776-124">Att lägga till Autotask arbetsplats från galleriet</span><span class="sxs-lookup"><span data-stu-id="cc776-124">Adding Autotask Workplace from the gallery</span></span>
2. <span data-ttu-id="cc776-125">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cc776-125">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-autotask-workplace-from-the-gallery"></a><span data-ttu-id="cc776-126">Att lägga till Autotask arbetsplats från galleriet</span><span class="sxs-lookup"><span data-stu-id="cc776-126">Adding Autotask Workplace from the gallery</span></span>
<span data-ttu-id="cc776-127">Du måste lägga till Autotask arbetsplats från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Autotask arbetsplats i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cc776-127">To configure the integration of Autotask Workplace into Azure AD, you need to add Autotask Workplace from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="cc776-128">**Utför följande steg för att lägga till Autotask arbetsplats från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="cc776-128">**To add Autotask Workplace from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="cc776-129">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="cc776-129">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Azure Active Directory-knappen][1]

2. <span data-ttu-id="cc776-131">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="cc776-131">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="cc776-132">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="cc776-132">Then go to **All applications**.</span></span>

    ![Bladet Enterprise program][2]
    
3. <span data-ttu-id="cc776-134">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cc776-134">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Knappen Nytt program][3]

4. <span data-ttu-id="cc776-136">I sökrutan skriver **Autotask arbetsplats**väljer **Autotask arbetsplats** resultatet-panelen klickar **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="cc776-136">In the search box, type **Autotask Workplace**, select  **Autotask Workplace**  from result panel then click **Add** button to add the application.</span></span>

    ![Autotask arbetsplats i resultatlistan](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="cc776-138">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cc776-138">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="cc776-139">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Autotask arbetsplats baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="cc776-139">In this section, you configure and test Azure AD single sign-on with Autotask Workplace based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="cc776-140">Azure AD måste du känna till motsvarande användaren i Autotask arbetsplats till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="cc776-140">For single sign-on to work, Azure AD needs to know what the counterpart user in Autotask Workplace is to a user in Azure AD.</span></span> <span data-ttu-id="cc776-141">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Autotask arbetsplatsen upprättas.</span><span class="sxs-lookup"><span data-stu-id="cc776-141">In other words, a link relationship between an Azure AD user and the related user in Autotask Workplace needs to be established.</span></span>

<span data-ttu-id="cc776-142">I Autotask arbetsplats, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="cc776-142">In Autotask Workplace, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="cc776-143">Om du vill konfigurera och testa Azure AD enkel inloggning med Autotask arbetsplats, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="cc776-143">To configure and test Azure AD single sign-on with Autotask Workplace, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="cc776-144">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="cc776-144">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="cc776-145">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cc776-145">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="cc776-146">**[Skapa en testanvändare Autotask arbetsplats](#create-an-autotask-workplace-test-user)**  – du har en motsvarighet för Britta Simon Autotask arbetsplatsen som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="cc776-146">**[Create an Autotask Workplace test user](#create-an-autotask-workplace-test-user)** - to have a counterpart of Britta Simon in Autotask Workplace that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="cc776-147">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="cc776-147">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="cc776-148">**[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="cc776-148">**[Test Single Sign-On](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="cc776-149">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cc776-149">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="cc776-150">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Autotask arbetsplats.</span><span class="sxs-lookup"><span data-stu-id="cc776-150">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Autotask Workplace application.</span></span>

<span data-ttu-id="cc776-151">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Autotask arbetsplats:**</span><span class="sxs-lookup"><span data-stu-id="cc776-151">**To configure Azure AD single sign-on with Autotask Workplace, perform the following steps:**</span></span>

1. <span data-ttu-id="cc776-152">I Azure-portalen på den **Autotask arbetsplats** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="cc776-152">In the Azure portal, on the **Autotask Workplace** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="cc776-154">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="cc776-154">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_samlbase.png)

3. <span data-ttu-id="cc776-156">Om du vill konfigurera programmet i **IDP** initierade läge, utför följande steg på den **Autotask arbetsplats domän och URL: er** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="cc776-156">If you wish to configure the application in **IDP** initiated mode, perform the following steps on the **Autotask Workplace Domain and URLs** section:</span></span>

    ![Autotask arbetsplats domän URL: er och enkel inloggning information för IDP](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_url.png)

    <span data-ttu-id="cc776-158">a.</span><span class="sxs-lookup"><span data-stu-id="cc776-158">a.</span></span> <span data-ttu-id="cc776-159">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<subdomain>.awp.autotask.net/singlesignon/saml/metadata`</span><span class="sxs-lookup"><span data-stu-id="cc776-159">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.awp.autotask.net/singlesignon/saml/metadata`</span></span>

    <span data-ttu-id="cc776-160">b.</span><span class="sxs-lookup"><span data-stu-id="cc776-160">b.</span></span> <span data-ttu-id="cc776-161">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://<subdomain>.awp.autotask.net/singlesignon/saml/SSO`</span><span class="sxs-lookup"><span data-stu-id="cc776-161">In the **Reply URL** textbox, type a URL using the following pattern: `https://<subdomain>.awp.autotask.net/singlesignon/saml/SSO`</span></span>

4. <span data-ttu-id="cc776-162">Om du vill konfigurera programmet i **SP** initierade läge, kontrollera **visa avancerade inställningar för URL: en** och utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="cc776-162">If you wish to configure the application in **SP** initiated mode, check **Show advanced URL settings** and perform the following steps:</span></span>

    ![Autotask arbetsplats domän URL: er och enkel inloggning information för SP](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_url1.png)

    <span data-ttu-id="cc776-164">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<subdomain>.awp.autotask.net/loginsso`</span><span class="sxs-lookup"><span data-stu-id="cc776-164">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.awp.autotask.net/loginsso`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="cc776-165">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="cc776-165">These values are not real.</span></span> <span data-ttu-id="cc776-166">Uppdatera dessa värden med den faktiska identifierare Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="cc776-166">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="cc776-167">Kontakta [Autotask arbetsplats klienten supportteamet](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="cc776-167">Contact [Autotask Workplace Client support team](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) to get these values.</span></span> 

5. <span data-ttu-id="cc776-168">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="cc776-168">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Länken hämta certifikatet](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_certificate.png) 

6. <span data-ttu-id="cc776-170">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="cc776-170">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="cc776-172">Logga in på arbetsplatsen Online med administratörsautentiseringsuppgifter för den i ett annat webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="cc776-172">In a different web browser window, Log in to Workplace Online using the administrator credentials.</span></span>

    >[!Note]
    ><span data-ttu-id="cc776-173">När du konfigurerar IdP, måste en underdomän anges.</span><span class="sxs-lookup"><span data-stu-id="cc776-173">When configuring the IdP, a subdomain will need to be specified.</span></span> <span data-ttu-id="cc776-174">För att bekräfta rätt underdomänen, inloggning till arbetsplatsen Online.</span><span class="sxs-lookup"><span data-stu-id="cc776-174">To confirm the correct subdomain, login to Workplace Online.</span></span> <span data-ttu-id="cc776-175">Efter loggat in kan du anteckna underdomänen i URL-Adressen visas.</span><span class="sxs-lookup"><span data-stu-id="cc776-175">Once logged in, make note to the subdomain in the URL.</span></span>
    ><span data-ttu-id="cc776-176">Underdomänen ingår mellan ”https://” och ”.awp.autotask.net/” och bör vara oss eu, ca eller au.</span><span class="sxs-lookup"><span data-stu-id="cc776-176">The subdomain is the part between the “https://“ and “.awp.autotask.net/“ and should be us, eu, ca, or au.</span></span>

8. <span data-ttu-id="cc776-177">Gå till **Configuration** > **enkel inloggning** och utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="cc776-177">Go to **Configuration** > **Single Sign-On** and perform the following steps:</span></span>

    ![Autotask enkel inloggning konfiguration](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskssoconfig1.png)
 
    <span data-ttu-id="cc776-179">a.</span><span class="sxs-lookup"><span data-stu-id="cc776-179">a.</span></span> <span data-ttu-id="cc776-180">Välj den **XML-metadatafil** alternativ och sedan ladda upp den **XML-Metadata för** hämtas från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="cc776-180">Select the **XML Metadata File** option, and then upload the **Metadata XML** downloaded from Azure portal.</span></span>

    <span data-ttu-id="cc776-181">b.</span><span class="sxs-lookup"><span data-stu-id="cc776-181">b.</span></span> <span data-ttu-id="cc776-182">Klicka på **aktivera enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="cc776-182">Click **Enable SSO**.</span></span>
    
    ![Godkänn Autotask Single Sign-on konfiguration](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskssoconfig2.png)

    <span data-ttu-id="cc776-184">c.</span><span class="sxs-lookup"><span data-stu-id="cc776-184">c.</span></span> <span data-ttu-id="cc776-185">Välj den **jag bekräfta informationen är korrekt och den här IdP tillförlitliga** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="cc776-185">Select the **I confirm this information is correct and I trust this IdP** check box.</span></span>

    <span data-ttu-id="cc776-186">d.</span><span class="sxs-lookup"><span data-stu-id="cc776-186">d.</span></span> <span data-ttu-id="cc776-187">Klicka på **godkänna**.</span><span class="sxs-lookup"><span data-stu-id="cc776-187">Click **Approve**.</span></span>
     
>[!Note]
><span data-ttu-id="cc776-188">Om du behöver hjälp med att konfigurera Autotask arbetsplats finns [den här sidan](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) att få hjälp med ditt arbetsplatskonto.</span><span class="sxs-lookup"><span data-stu-id="cc776-188">If you require assistance with configuring Autotask Workplace, please see [this page](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) to get assistance with your Workplace account.</span></span>

> [!TIP]
> <span data-ttu-id="cc776-189">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="cc776-189">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="cc776-190">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="cc776-190">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="cc776-191">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="cc776-191">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="cc776-192">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="cc776-192">Create an Azure AD test user</span></span>

<span data-ttu-id="cc776-193">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cc776-193">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="cc776-195">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="cc776-195">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="cc776-196">I Azure-portalen i den vänstra rutan klickar du på den **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="cc776-196">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Azure Active Directory-knappen](./media/active-directory-saas-autotaskworkplace-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="cc776-198">Om du vill visa en lista över användare, gå till **användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="cc776-198">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![”Användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-autotaskworkplace-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="cc776-200">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i den **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cc776-200">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Knappen Lägg till](./media/active-directory-saas-autotaskworkplace-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="cc776-202">I den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="cc776-202">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogrutan användare](./media/active-directory-saas-autotaskworkplace-tutorial/create_aaduser_04.png)

    <span data-ttu-id="cc776-204">a.</span><span class="sxs-lookup"><span data-stu-id="cc776-204">a.</span></span> <span data-ttu-id="cc776-205">I den **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="cc776-205">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="cc776-206">b.</span><span class="sxs-lookup"><span data-stu-id="cc776-206">b.</span></span> <span data-ttu-id="cc776-207">I den **användarnamn** Skriv användarens Britta Simon e-postadress.</span><span class="sxs-lookup"><span data-stu-id="cc776-207">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="cc776-208">c.</span><span class="sxs-lookup"><span data-stu-id="cc776-208">c.</span></span> <span data-ttu-id="cc776-209">Välj den **visa lösenordet** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="cc776-209">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="cc776-210">d.</span><span class="sxs-lookup"><span data-stu-id="cc776-210">d.</span></span> <span data-ttu-id="cc776-211">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="cc776-211">Click **Create**.</span></span>

### <a name="create-an-autotask-workplace-test-user"></a><span data-ttu-id="cc776-212">Skapa en testanvändare Autotask arbetsplats</span><span class="sxs-lookup"><span data-stu-id="cc776-212">Create an Autotask Workplace test user</span></span>

<span data-ttu-id="cc776-213">I det här avsnittet skapar du en användare som kallas Britta Simon i Autotask.</span><span class="sxs-lookup"><span data-stu-id="cc776-213">In this section, you create a user called Britta Simon in Autotask.</span></span> <span data-ttu-id="cc776-214">Se tillsammans med [Autotask arbetsplats supportteamet](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) att lägga till användare i Autotask arbetsplats-plattformen.</span><span class="sxs-lookup"><span data-stu-id="cc776-214">Please work with [Autotask Workplace support team](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) to add the users in the Autotask Workplace platform.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="cc776-215">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="cc776-215">Assign the Azure AD test user</span></span>

<span data-ttu-id="cc776-216">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till arbetsplats Autotask.</span><span class="sxs-lookup"><span data-stu-id="cc776-216">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Autotask Workplace.</span></span>

![Tilldela rollen][200] 

<span data-ttu-id="cc776-218">**Om du vill tilldela Autotask arbetsplats Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="cc776-218">**To assign Britta Simon to Autotask Workplace, perform the following steps:**</span></span>

1. <span data-ttu-id="cc776-219">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="cc776-219">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="cc776-221">Välj i listan med program **Autotask arbetsplats**.</span><span class="sxs-lookup"><span data-stu-id="cc776-221">In the applications list, select **Autotask Workplace**.</span></span>

    ![Länken Autotask arbetsplats i listan med program](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_app.png) 

3. <span data-ttu-id="cc776-223">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="cc776-223">In the menu on the left, click **Users and groups**.</span></span>

    ![Länken ”användare och grupper”][202]

4. <span data-ttu-id="cc776-225">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="cc776-225">Click **Add** button.</span></span> <span data-ttu-id="cc776-226">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cc776-226">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Fönstret Lägg till tilldelning][203]

5. <span data-ttu-id="cc776-228">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="cc776-228">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="cc776-229">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cc776-229">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="cc776-230">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cc776-230">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="cc776-231">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cc776-231">Test single sign-on</span></span>

<span data-ttu-id="cc776-232">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="cc776-232">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="cc776-233">När du klickar på panelen Autotask arbetsplats på åtkomstpanelen du ska hämta automatiskt loggat in på ditt Autotask arbetsplats program.</span><span class="sxs-lookup"><span data-stu-id="cc776-233">When you click the Autotask Workplace tile in the Access Panel, you should get automatically signed-on to your Autotask Workplace application.</span></span>
<span data-ttu-id="cc776-234">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="cc776-234">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cc776-235">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="cc776-235">Additional resources</span></span>

* [<span data-ttu-id="cc776-236">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="cc776-236">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="cc776-237">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="cc776-237">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_203.png

