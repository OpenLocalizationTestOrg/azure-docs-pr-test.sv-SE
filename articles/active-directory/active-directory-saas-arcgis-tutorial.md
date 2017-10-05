---
title: "Självstudier: Azure Active Directory-integrering med ArcGIS Online | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och ArcGIS Online."
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
ms.openlocfilehash: df72270ca6443b456c079b22425f1660aa522389
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-arcgis-online"></a><span data-ttu-id="66254-103">Självstudier: Azure Active Directory-integrering med ArcGIS Online</span><span class="sxs-lookup"><span data-stu-id="66254-103">Tutorial: Azure Active Directory integration with ArcGIS Online</span></span>

<span data-ttu-id="66254-104">I kursen får lära du att integrera ArcGIS Online med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="66254-104">In this tutorial, you learn how to integrate ArcGIS Online with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="66254-105">Integrera ArcGIS Online med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="66254-105">Integrating ArcGIS Online with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="66254-106">Du kan styra i Azure AD som har åtkomst till ArcGIS Online</span><span class="sxs-lookup"><span data-stu-id="66254-106">You can control in Azure AD who has access to ArcGIS Online</span></span>
- <span data-ttu-id="66254-107">Du kan aktivera användarna att automatiskt hämta loggat in på ArcGIS Online (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="66254-107">You can enable your users to automatically get signed-on to ArcGIS Online (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="66254-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="66254-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="66254-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="66254-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

<!--## Overview

To enable single sign-on with ArcGIS Online, it must be configured to use Azure Active Directory as an identity provider. This guide provides information and tips on how to perform this configuration in ArcGIS Online.

>[!Note]: 
>This embedded guide is brand new in the new Azure portal, and we’d love to hear your thoughts. Use the Feedback ? button at the top of the portal to provide feedback. The older guide for using the [Azure classic portal](https://manage.windowsazure.com) to configure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a><span data-ttu-id="66254-110">Krav</span><span class="sxs-lookup"><span data-stu-id="66254-110">Prerequisites</span></span>

<span data-ttu-id="66254-111">För att konfigurera Azure AD-integrering med ArcGIS Online, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="66254-111">To configure Azure AD integration with ArcGIS Online, you need the following items:</span></span>

- <span data-ttu-id="66254-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="66254-112">An Azure AD subscription</span></span>
- <span data-ttu-id="66254-113">En ArcGIS Online enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="66254-113">A ArcGIS Online single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="66254-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="66254-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="66254-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="66254-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="66254-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="66254-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="66254-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="66254-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="66254-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="66254-118">Scenario description</span></span>
<span data-ttu-id="66254-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="66254-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="66254-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="66254-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="66254-121">Att lägga till ArcGIS Online från galleriet</span><span class="sxs-lookup"><span data-stu-id="66254-121">Adding ArcGIS Online from the gallery</span></span>
2. <span data-ttu-id="66254-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="66254-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-arcgis-online-from-the-gallery"></a><span data-ttu-id="66254-123">Att lägga till ArcGIS Online från galleriet</span><span class="sxs-lookup"><span data-stu-id="66254-123">Adding ArcGIS Online from the gallery</span></span>
<span data-ttu-id="66254-124">Du måste lägga till ArcGIS Online från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av ArcGIS Online i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="66254-124">To configure the integration of ArcGIS Online into Azure AD, you need to add ArcGIS Online from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="66254-125">**Om du vill lägga till ArcGIS Online från galleriet, utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="66254-125">**To add ArcGIS Online from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="66254-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="66254-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="66254-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="66254-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="66254-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="66254-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="66254-131">Klicka på **nytt program** knappen överst för att lägga till nya program.</span><span class="sxs-lookup"><span data-stu-id="66254-131">Click **New application** button on the top of the dialog to add new application.</span></span>

    ![Program][3]

4. <span data-ttu-id="66254-133">I sökrutan skriver **ArcGIS Online**.</span><span class="sxs-lookup"><span data-stu-id="66254-133">In the search box, type **ArcGIS Online**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_search.png)

5. <span data-ttu-id="66254-135">Välj i resultatpanelen **ArcGIS Online**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="66254-135">In the results panel, select **ArcGIS Online**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="66254-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="66254-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="66254-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med ArcGIS Online baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="66254-138">In this section, you configure and test Azure AD single sign-on with ArcGIS Online based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="66254-139">Azure AD måste du känna till användaren i ArcGIS Online motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="66254-139">For single sign-on to work, Azure AD needs to know what the counterpart user in ArcGIS Online is to a user in Azure AD.</span></span> <span data-ttu-id="66254-140">Med andra ord måste en länk mellan en Azure AD-användare och relaterade användaren i ArcGIS Online upprättas.</span><span class="sxs-lookup"><span data-stu-id="66254-140">In other words, a link relationship between an Azure AD user and the related user in ArcGIS Online needs to be established.</span></span>

<span data-ttu-id="66254-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i ArcGIS Online.</span><span class="sxs-lookup"><span data-stu-id="66254-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in ArcGIS Online.</span></span>

<span data-ttu-id="66254-142">Om du vill konfigurera och testa Azure AD enkel inloggning med ArcGIS Online, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="66254-142">To configure and test Azure AD single sign-on with ArcGIS Online, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="66254-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="66254-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="66254-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="66254-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="66254-145">**[Skapa en ArcGIS Online testanvändare](#creating-an-arcgis-online-test-user)**  – du har en motsvarighet för Britta Simon i ArcGIS Online som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="66254-145">**[Creating an ArcGIS Online test user](#creating-an-arcgis-online-test-user)** - to have a counterpart of Britta Simon in ArcGIS Online that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="66254-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="66254-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="66254-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="66254-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="66254-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="66254-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="66254-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet ArcGIS Online.</span><span class="sxs-lookup"><span data-stu-id="66254-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your ArcGIS Online application.</span></span>

<span data-ttu-id="66254-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med ArcGIS Online:**</span><span class="sxs-lookup"><span data-stu-id="66254-150">**To configure Azure AD single sign-on with ArcGIS Online, perform the following steps:**</span></span>

1. <span data-ttu-id="66254-151">I Azure-portalen på den **ArcGIS Online** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="66254-151">In the Azure portal, on the **ArcGIS Online** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="66254-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="66254-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_samlbase.png)

3. <span data-ttu-id="66254-155">På den **ArcGIS Online domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="66254-155">On the **ArcGIS Online Domain and URLs** section, perform the following step:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_url.png)

    <span data-ttu-id="66254-157">I den **inloggnings-URL** textruta Skriv det värde som använder följande mönster:`https://<company>.maps.arcgis.com`</span><span class="sxs-lookup"><span data-stu-id="66254-157">In the **Sign-on URL** textbox, type the value using the following pattern: `https://<company>.maps.arcgis.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="66254-158">Det här värdet är inte verkligt.</span><span class="sxs-lookup"><span data-stu-id="66254-158">This value is not the real.</span></span> <span data-ttu-id="66254-159">Uppdatera det här värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="66254-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="66254-160">Kontakta [ArcGIS Online klienten supportteamet](http://support.esri.com/) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="66254-160">Contact [ArcGIS Online Client support team](http://support.esri.com/) to get this value.</span></span> 

4. <span data-ttu-id="66254-161">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara XML-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="66254-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_certificate.png) 

5. <span data-ttu-id="66254-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="66254-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-arcgis-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="66254-165">Logga in på webbplatsen ArcGIS företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="66254-165">In a different web browser window, log into your ArcGIS company site as an administrator.</span></span>

7. <span data-ttu-id="66254-166">Klicka på **redigera inställningar för**.</span><span class="sxs-lookup"><span data-stu-id="66254-166">Click **EDIT SETTINGS**.</span></span>

    <span data-ttu-id="66254-167">![Redigera inställningar för](./media/active-directory-saas-arcgis-tutorial/ic784742.png "redigera inställningar")</span><span class="sxs-lookup"><span data-stu-id="66254-167">![Edit Settings](./media/active-directory-saas-arcgis-tutorial/ic784742.png "Edit Settings")</span></span>

8. <span data-ttu-id="66254-168">Klicka på **säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="66254-168">Click **Security**.</span></span>

    <span data-ttu-id="66254-169">![Säkerhet](./media/active-directory-saas-arcgis-tutorial/ic784743.png "säkerhet")</span><span class="sxs-lookup"><span data-stu-id="66254-169">![Security](./media/active-directory-saas-arcgis-tutorial/ic784743.png "Security")</span></span>

9. <span data-ttu-id="66254-170">Under **Enterprise inloggningar**, klickar du på **ange IDENTITETSLEVERANTÖR**.</span><span class="sxs-lookup"><span data-stu-id="66254-170">Under **Enterprise Logins**, click **SET IDENTITY PROVIDER**.</span></span>

    <span data-ttu-id="66254-171">![Enterprise-inloggningar](./media/active-directory-saas-arcgis-tutorial/ic784744.png "Enterprise-inloggningar")</span><span class="sxs-lookup"><span data-stu-id="66254-171">![Enterprise Logins](./media/active-directory-saas-arcgis-tutorial/ic784744.png "Enterprise Logins")</span></span>

10. <span data-ttu-id="66254-172">På den **ange identitetsleverantör** konfigurationen utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="66254-172">On the **Set Identity Provider** configuration page, perform the following steps:</span></span>
   
    <span data-ttu-id="66254-173">![Ange identitetsleverantör](./media/active-directory-saas-arcgis-tutorial/ic784745.png "ange identitetsleverantören.")</span><span class="sxs-lookup"><span data-stu-id="66254-173">![Set Identity Provider](./media/active-directory-saas-arcgis-tutorial/ic784745.png "Set Identity Provider")</span></span>
   
    <span data-ttu-id="66254-174">a.</span><span class="sxs-lookup"><span data-stu-id="66254-174">a.</span></span> <span data-ttu-id="66254-175">I den **namn** textruta Skriv ditt organisationsnamn.</span><span class="sxs-lookup"><span data-stu-id="66254-175">In the **Name** textbox, type your organization’s name.</span></span>

    <span data-ttu-id="66254-176">b.</span><span class="sxs-lookup"><span data-stu-id="66254-176">b.</span></span> <span data-ttu-id="66254-177">För **Metadata för Enterprise-identitetsleverantör ska anges med hjälp av**väljer **A filen**.</span><span class="sxs-lookup"><span data-stu-id="66254-177">For **Metadata for the Enterprise Identity Provider will be supplied using**, select **A File**.</span></span>

    <span data-ttu-id="66254-178">c.</span><span class="sxs-lookup"><span data-stu-id="66254-178">c.</span></span> <span data-ttu-id="66254-179">Om du vill överföra din hämtade metadatafil klickar du på **Välj fil**.</span><span class="sxs-lookup"><span data-stu-id="66254-179">To upload your downloaded metadata file, click **Choose file**.</span></span>

    <span data-ttu-id="66254-180">d.</span><span class="sxs-lookup"><span data-stu-id="66254-180">d.</span></span> <span data-ttu-id="66254-181">Klicka på **SET IDENTITETSLEVERANTÖR**.</span><span class="sxs-lookup"><span data-stu-id="66254-181">Click **SET IDENTITY PROVIDER**.</span></span>

> [!TIP]
> <span data-ttu-id="66254-182">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="66254-182">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="66254-183">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="66254-183">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="66254-184">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="66254-184">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="66254-185">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="66254-185">Creating an Azure AD test user</span></span>
<span data-ttu-id="66254-186">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="66254-186">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="66254-188">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="66254-188">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="66254-189">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="66254-189">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-arcgis-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="66254-191">Gå till **användare och grupper** och på **alla användare** att visa en lista över användare.</span><span class="sxs-lookup"><span data-stu-id="66254-191">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-arcgis-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="66254-193">Klicka på överst i dialogrutan **Lägg till** att öppna den **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="66254-193">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-arcgis-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="66254-195">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="66254-195">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-arcgis-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="66254-197">a.</span><span class="sxs-lookup"><span data-stu-id="66254-197">a.</span></span> <span data-ttu-id="66254-198">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="66254-198">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="66254-199">b.</span><span class="sxs-lookup"><span data-stu-id="66254-199">b.</span></span> <span data-ttu-id="66254-200">I den **användarnamn** textruta typ av **e-postadress** av Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="66254-200">In the **User name** textbox, type the **email address** of Britta Simon.</span></span>

    <span data-ttu-id="66254-201">c.</span><span class="sxs-lookup"><span data-stu-id="66254-201">c.</span></span> <span data-ttu-id="66254-202">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="66254-202">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="66254-203">d.</span><span class="sxs-lookup"><span data-stu-id="66254-203">d.</span></span> <span data-ttu-id="66254-204">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="66254-204">Click **Create**.</span></span>
 
### <a name="creating-an-arcgis-online-test-user"></a><span data-ttu-id="66254-205">Skapa en ArcGIS Online testanvändare</span><span class="sxs-lookup"><span data-stu-id="66254-205">Creating an ArcGIS Online test user</span></span>

<span data-ttu-id="66254-206">För att aktivera Azure AD-användare att logga in på ArcGIS Online, måste de etableras i ArcGIS Online.</span><span class="sxs-lookup"><span data-stu-id="66254-206">In order to enable Azure AD users to log into ArcGIS Online, they must be provisioned into ArcGIS Online.</span></span>  
<span data-ttu-id="66254-207">När det gäller ArcGIS Online är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="66254-207">In the case of ArcGIS Online, provisioning is a manual task.</span></span>

<span data-ttu-id="66254-208">**Utför följande steg om du vill konfigurera ett användarkonto:**</span><span class="sxs-lookup"><span data-stu-id="66254-208">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="66254-209">Logga in på ditt **ArcGIS** klient.</span><span class="sxs-lookup"><span data-stu-id="66254-209">Log in to your **ArcGIS** tenant.</span></span>

2. <span data-ttu-id="66254-210">Klicka på **bjuda in MEDLEMMAR**.</span><span class="sxs-lookup"><span data-stu-id="66254-210">Click **INVITE MEMBERS**.</span></span>
   
    <span data-ttu-id="66254-211">![Bjud in medlemmar](./media/active-directory-saas-arcgis-tutorial/ic784747.png "bjuda in medlemmar")</span><span class="sxs-lookup"><span data-stu-id="66254-211">![Invite Members](./media/active-directory-saas-arcgis-tutorial/ic784747.png "Invite Members")</span></span>

3. <span data-ttu-id="66254-212">Välj **lägga till medlemmar automatiskt utan att skicka ett e-postmeddelande**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="66254-212">Select **Add members automatically without sending an email**, and then click **NEXT**.</span></span>
   
    <span data-ttu-id="66254-213">![Lägga till medlemmar automatiskt](./media/active-directory-saas-arcgis-tutorial/ic784748.png "automatiskt lägga till medlemmar")</span><span class="sxs-lookup"><span data-stu-id="66254-213">![Add Members Automatically](./media/active-directory-saas-arcgis-tutorial/ic784748.png "Add Members Automatically")</span></span>

4. <span data-ttu-id="66254-214">På den **medlemmar** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="66254-214">On the **Members** dialog page, perform the following steps:</span></span>
   
     <span data-ttu-id="66254-215">![Lägg till och granska](./media/active-directory-saas-arcgis-tutorial/ic784749.png "Lägg till och granska")</span><span class="sxs-lookup"><span data-stu-id="66254-215">![Add and review](./media/active-directory-saas-arcgis-tutorial/ic784749.png "Add and review")</span></span>
    
     <span data-ttu-id="66254-216">a.</span><span class="sxs-lookup"><span data-stu-id="66254-216">a.</span></span> <span data-ttu-id="66254-217">Ange den **e-post**, **Förnamn**, och **efternamn** av en giltig AAD-konto som du vill etablera.</span><span class="sxs-lookup"><span data-stu-id="66254-217">Enter the **Email**, **First Name**, and **Last Name** of a valid AAD account you want to provision.</span></span>
  
     <span data-ttu-id="66254-218">b.</span><span class="sxs-lookup"><span data-stu-id="66254-218">b.</span></span> <span data-ttu-id="66254-219">Klicka på **Lägg till och granska**.</span><span class="sxs-lookup"><span data-stu-id="66254-219">Click **ADD AND REVIEW**.</span></span>
5. <span data-ttu-id="66254-220">Granska de data du har angett och klicka sedan på **lägga till MEDLEMMAR**.</span><span class="sxs-lookup"><span data-stu-id="66254-220">Review the data you have entered, and then click **ADD MEMBERS**.</span></span>
   
    <span data-ttu-id="66254-221">![Lägg till medlem](./media/active-directory-saas-arcgis-tutorial/ic784750.png "Lägg till medlem")</span><span class="sxs-lookup"><span data-stu-id="66254-221">![Add member](./media/active-directory-saas-arcgis-tutorial/ic784750.png "Add member")</span></span>
        
    > [!NOTE]
    > <span data-ttu-id="66254-222">Innehavaren för Azure Active Directory-konto ett e-postmeddelande och länken för att bekräfta sina konton innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="66254-222">The Azure Active Directory account holder will receive an email and follow a link to confirm their account before it becomes active.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="66254-223">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="66254-223">Assigning the Azure AD test user</span></span>

<span data-ttu-id="66254-224">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till ArcGIS Online.</span><span class="sxs-lookup"><span data-stu-id="66254-224">In this section, you enable Britta Simon to use Azure single sign-on by granting access to ArcGIS Online.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="66254-226">**Om du vill tilldela Britta Simon ArcGIS Online, utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="66254-226">**To assign Britta Simon to ArcGIS Online, perform the following steps:**</span></span>

1. <span data-ttu-id="66254-227">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="66254-227">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="66254-229">Välj i listan med program **ArcGIS Online**.</span><span class="sxs-lookup"><span data-stu-id="66254-229">In the applications list, select **ArcGIS Online**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_app.png) 

3. <span data-ttu-id="66254-231">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="66254-231">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="66254-233">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="66254-233">Click **Add** button.</span></span> <span data-ttu-id="66254-234">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="66254-234">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="66254-236">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="66254-236">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="66254-237">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="66254-237">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="66254-238">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="66254-238">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="66254-239">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="66254-239">Testing single sign-on</span></span>

<span data-ttu-id="66254-240">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="66254-240">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="66254-241">När du klickar på panelen ArcGIS Online på åtkomstpanelen du bör få automatiskt loggat in i tillämpningsprogrammet ArcGIS Online.</span><span class="sxs-lookup"><span data-stu-id="66254-241">When you click the ArcGIS Online tile in the Access Panel, you should get automatically signed-on to your ArcGIS Online application.</span></span>
<span data-ttu-id="66254-242">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="66254-242">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="66254-243">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="66254-243">Additional resources</span></span>

* [<span data-ttu-id="66254-244">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="66254-244">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="66254-245">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="66254-245">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



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

