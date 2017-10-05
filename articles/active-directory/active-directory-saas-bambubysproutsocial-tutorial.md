---
title: "Självstudier: Azure Active Directory-integrering med Bambu av brysselkål sociala | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Bambu av brysselkål sociala."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d2b9ddbc-cab7-40d6-aca1-5b171cab4199
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2017
ms.author: jeedes
ms.openlocfilehash: 985966d26f6ed0dcd4db47589abf94260ce62bf0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bambu-by-sprout-social"></a><span data-ttu-id="3004f-103">Självstudier: Azure Active Directory-integrering med Bambu av brysselkål sociala</span><span class="sxs-lookup"><span data-stu-id="3004f-103">Tutorial: Azure Active Directory integration with Bambu by Sprout Social</span></span>

<span data-ttu-id="3004f-104">I kursen får lära du att integrera Bambu av brysselkål sociala med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="3004f-104">In this tutorial, you learn how to integrate Bambu by Sprout Social with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3004f-105">Integrera Bambu av brysselkål sociala med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="3004f-105">Integrating Bambu by Sprout Social with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="3004f-106">Du kan styra i Azure AD som har åtkomst till Bambu av brysselkål sociala</span><span class="sxs-lookup"><span data-stu-id="3004f-106">You can control in Azure AD who has access to Bambu by Sprout Social</span></span>
- <span data-ttu-id="3004f-107">Du kan aktivera användarna att automatiskt hämta loggat in på Bambu av brysselkål sociala (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="3004f-107">You can enable your users to automatically get signed-on to Bambu by Sprout Social (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3004f-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="3004f-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="3004f-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3004f-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

<!--## Overview

To enable single sign-on with Bambu by Sprout Social, it must be configured to use Azure Active Directory as an identity provider. This guide provides information and tips on how to perform this configuration in Bambu by Sprout Social.

>[!Note]: 
>This embedded guide is brand new in the new Azure portal, and we’d love to hear your thoughts. Use the Feedback ? button at the top of the portal to provide feedback. The older guide for using the [Azure classic portal](https://manage.windowsazure.com) to configure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a><span data-ttu-id="3004f-110">Krav</span><span class="sxs-lookup"><span data-stu-id="3004f-110">Prerequisites</span></span>

<span data-ttu-id="3004f-111">För att konfigurera Azure AD-integrering med Bambu av brysselkål sociala, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="3004f-111">To configure Azure AD integration with Bambu by Sprout Social, you need the following items:</span></span>

- <span data-ttu-id="3004f-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="3004f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3004f-113">En Bambu av brysselkål sociala enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="3004f-113">A Bambu by Sprout Social single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3004f-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="3004f-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3004f-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="3004f-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3004f-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="3004f-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="3004f-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3004f-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3004f-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="3004f-118">Scenario description</span></span>
<span data-ttu-id="3004f-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="3004f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3004f-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="3004f-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3004f-121">Att lägga till Bambu av brysselkål sociala från galleriet</span><span class="sxs-lookup"><span data-stu-id="3004f-121">Adding Bambu by Sprout Social from the gallery</span></span>
2. <span data-ttu-id="3004f-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3004f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-bambu-by-sprout-social-from-the-gallery"></a><span data-ttu-id="3004f-123">Att lägga till Bambu av brysselkål sociala från galleriet</span><span class="sxs-lookup"><span data-stu-id="3004f-123">Adding Bambu by Sprout Social from the gallery</span></span>
<span data-ttu-id="3004f-124">Du måste lägga till Bambu av brysselkål sociala från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Bambu av brysselkål sociala i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3004f-124">To configure the integration of Bambu by Sprout Social into Azure AD, you need to add Bambu by Sprout Social from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="3004f-125">**Utför följande steg för att lägga till Bambu av brysselkål sociala från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="3004f-125">**To add Bambu by Sprout Social from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="3004f-126">I den  **[Azure Portal](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="3004f-126">In the **[Azure Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3004f-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="3004f-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="3004f-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="3004f-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="3004f-131">Klicka på **nytt program** knappen överst för att lägga till nya program.</span><span class="sxs-lookup"><span data-stu-id="3004f-131">Click **New application** button on the top of the dialog to add new application.</span></span>

    ![Program][3]

4. <span data-ttu-id="3004f-133">I sökrutan skriver **Bambu av brysselkål sociala**.</span><span class="sxs-lookup"><span data-stu-id="3004f-133">In the search box, type **Bambu by Sprout Social**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_bambubysproutsocial_search.png)

5. <span data-ttu-id="3004f-135">Välj i resultatpanelen **Bambu av brysselkål sociala**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="3004f-135">In the results panel, select **Bambu by Sprout Social**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_bambubysproutsocial_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3004f-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3004f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3004f-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Bambu av brysselkål sociala baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="3004f-138">In this section, you configure and test Azure AD single sign-on with Bambu by Sprout Social based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="3004f-139">Azure AD måste du känna till användaren i Bambu av brysselkål sociala motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="3004f-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Bambu by Sprout Social is to a user in Azure AD.</span></span> <span data-ttu-id="3004f-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Bambu av brysselkål sociala upprättas.</span><span class="sxs-lookup"><span data-stu-id="3004f-140">In other words, a link relationship between an Azure AD user and the related user in Bambu by Sprout Social needs to be established.</span></span>

<span data-ttu-id="3004f-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i Bambu av brysselkål sociala.</span><span class="sxs-lookup"><span data-stu-id="3004f-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Bambu by Sprout Social.</span></span>

<span data-ttu-id="3004f-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Bambu av brysselkål sociala, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="3004f-142">To configure and test Azure AD single sign-on with Bambu by Sprout Social, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="3004f-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="3004f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="3004f-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3004f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3004f-145">**[Skapa en Bambu av brysselkål sociala testanvändare](#creating-a-bambu-by-sprout-social-test-user)**  – har en motsvarighet för Britta Simon Bambu av brysselkål sociala som är kopplad till Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="3004f-145">**[Creating a Bambu by Sprout Social test user](#creating-a-bambu-by-sprout-social-test-user)** - to have a counterpart of Britta Simon in Bambu by Sprout Social that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="3004f-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="3004f-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3004f-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="3004f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3004f-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3004f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3004f-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i din Bambu av brysselkål sociala program.</span><span class="sxs-lookup"><span data-stu-id="3004f-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Bambu by Sprout Social application.</span></span>

<span data-ttu-id="3004f-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Bambu av brysselkål sociala:**</span><span class="sxs-lookup"><span data-stu-id="3004f-150">**To configure Azure AD single sign-on with Bambu by Sprout Social, perform the following steps:**</span></span>

1. <span data-ttu-id="3004f-151">I Azure-portalen på den **Bambu av brysselkål sociala** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="3004f-151">In the Azure portal, on the **Bambu by Sprout Social** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="3004f-153">På den **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** att aktivera enkel inloggning på.</span><span class="sxs-lookup"><span data-stu-id="3004f-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_bambubysproutsocial_samlbase.png)

3. <span data-ttu-id="3004f-155">På den **Bambu brysselkål sociala domänen och URL: er** avsnittet användaren behöver inte utföra några steg som appen före redan är integrerad med Azure.</span><span class="sxs-lookup"><span data-stu-id="3004f-155">On the **Bambu by Sprout Social Domain and URLs** section, the user does not have to perform any steps as the app is already pre-integrated with Azure.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_bambubysproutsocial_url.png)

4. <span data-ttu-id="3004f-157">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara XML-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="3004f-157">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_bambubysproutsocial_certificate.png) 

5. <span data-ttu-id="3004f-159">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="3004f-159">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="3004f-161">På den **Bambu brysselkål sociala konfigurationen** klickar du på **konfigurera Bambu av brysselkål sociala** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="3004f-161">On the **Bambu by Sprout Social Configuration** section, click **Configure Bambu by Sprout Social** to open **Configure sign-on** window.</span></span> <span data-ttu-id="3004f-162">Kopiera den **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="3004f-162">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_bambubysproutsocial_configure.png) 

7. <span data-ttu-id="3004f-164">Konfigurera enkel inloggning på **Bambu av brysselkål sociala** sida, måste du skicka den hämtade **XML-Metadata för** och **SAML enkel inloggning Tjänstwebbadress** till [ Bambu av brysselkål sociala support](mailto:support@getbambu.com).</span><span class="sxs-lookup"><span data-stu-id="3004f-164">To configure single sign-on on **Bambu by Sprout Social** side, you need to send the downloaded **Metadata XML** and **SAML Single Sign-On Service URL** to [Bambu by Sprout Social support](mailto:support@getbambu.com).</span></span> <span data-ttu-id="3004f-165">De ska ange detta för att få SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="3004f-165">They will set this up in order to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="3004f-166">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="3004f-166">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="3004f-167">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klicka på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den  **Konfigurationen** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="3004f-167">After adding this app from the **Active Directory > Enterprise Applications** section, simply click on the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="3004f-168">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3004f-168">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

<!--### Next steps

To ensure users can sign-in to Bambu by Sprout Social after it has been configured to use Azure Active Directory, review the following tasks and topics:

- User accounts must be pre-provisioned into Bambu by Sprout Social prior to sign-in. To set this up, see Provisioning.
 
- Users must be assigned access to Bambu by Sprout Social in Azure AD to sign-in. To assign users, see Users.
 
- To configure access polices for Bambu by Sprout Social users, see Access Policies.
 
- For additional information on deploying single sign-on to users, see [this article](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3004f-169">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="3004f-169">Creating an Azure AD test user</span></span>
<span data-ttu-id="3004f-170">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3004f-170">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="3004f-172">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="3004f-172">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="3004f-173">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="3004f-173">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bambubysproutsocial-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3004f-175">Gå till **användare och grupper** och på **alla användare** att visa en lista över användare.</span><span class="sxs-lookup"><span data-stu-id="3004f-175">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bambubysproutsocial-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3004f-177">Klicka på överst i dialogrutan **Lägg till** att öppna den **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3004f-177">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bambubysproutsocial-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3004f-179">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="3004f-179">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bambubysproutsocial-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3004f-181">a.</span><span class="sxs-lookup"><span data-stu-id="3004f-181">a.</span></span> <span data-ttu-id="3004f-182">I den **namn** textruta typen **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="3004f-182">In the **Name** textbox, type **Britta Simon**.</span></span>

    <span data-ttu-id="3004f-183">b.</span><span class="sxs-lookup"><span data-stu-id="3004f-183">b.</span></span> <span data-ttu-id="3004f-184">I den **användarnamn** textruta typ av **e-postadress** av Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3004f-184">In the **User name** textbox, type the **email address** of Britta Simon.</span></span>

    <span data-ttu-id="3004f-185">c.</span><span class="sxs-lookup"><span data-stu-id="3004f-185">c.</span></span> <span data-ttu-id="3004f-186">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="3004f-186">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="3004f-187">d.</span><span class="sxs-lookup"><span data-stu-id="3004f-187">d.</span></span> <span data-ttu-id="3004f-188">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="3004f-188">Click **Create**.</span></span>
 
### <a name="creating-a-bambu-by-sprout-social-test-user"></a><span data-ttu-id="3004f-189">Skapa en Bambu av brysselkål sociala testanvändare</span><span class="sxs-lookup"><span data-stu-id="3004f-189">Creating a Bambu by Sprout Social test user</span></span>

<span data-ttu-id="3004f-190">Programmet stöder bara i tid användaretablering och authentication-användare kommer automatiskt att skapas i programmet.</span><span class="sxs-lookup"><span data-stu-id="3004f-190">Application supports Just in time user provisioning and after authentication users will be created in the application automatically.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="3004f-191">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="3004f-191">Assigning the Azure AD test user</span></span>

<span data-ttu-id="3004f-192">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja sin åtkomst till Bambu av brysselkål sociala.</span><span class="sxs-lookup"><span data-stu-id="3004f-192">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to Bambu by Sprout Social.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="3004f-194">**Om du vill tilldela Bambu av brysselkål sociala Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="3004f-194">**To assign Britta Simon to Bambu by Sprout Social, perform the following steps:**</span></span>

1. <span data-ttu-id="3004f-195">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="3004f-195">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="3004f-197">Välj i listan med program **Bambu av brysselkål sociala**.</span><span class="sxs-lookup"><span data-stu-id="3004f-197">In the applications list, select **Bambu by Sprout Social**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_bambubysproutsocial_app.png) 

3. <span data-ttu-id="3004f-199">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="3004f-199">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="3004f-201">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="3004f-201">Click **Add** button.</span></span> <span data-ttu-id="3004f-202">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3004f-202">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="3004f-204">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="3004f-204">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="3004f-205">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3004f-205">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3004f-206">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3004f-206">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3004f-207">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3004f-207">Testing single sign-on</span></span>

<span data-ttu-id="3004f-208">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="3004f-208">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="3004f-209">När du klickar på Bambu av brysselkål sociala panelen på åtkomstpanelen du bör få automatiskt loggat in på ditt Bambu av brysselkål sociala program.</span><span class="sxs-lookup"><span data-stu-id="3004f-209">When you click the Bambu by Sprout Social tile in the Access Panel, you should get automatically signed-on to your Bambu by Sprout Social application.</span></span> <span data-ttu-id="3004f-210">Mer information om åtkomstpanelen finns [introduktion till åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="3004f-210">For more details about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="3004f-211">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="3004f-211">Additional resources</span></span>

* [<span data-ttu-id="3004f-212">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3004f-212">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3004f-213">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3004f-213">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_203.png

