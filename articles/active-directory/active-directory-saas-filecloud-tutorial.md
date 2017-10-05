---
title: "Självstudier: Azure Active Directory-integrering med FileCloud | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och FileCloud."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: f39f0ddd-b504-4562-971f-77b88d1e75fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: ad03516f684acc59912ffc57f6e0712828bd03f2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-filecloud"></a><span data-ttu-id="e2ab0-103">Självstudier: Azure Active Directory-integrering med FileCloud</span><span class="sxs-lookup"><span data-stu-id="e2ab0-103">Tutorial: Azure Active Directory integration with FileCloud</span></span>

<span data-ttu-id="e2ab0-104">I kursen får lära du att integrera FileCloud med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="e2ab0-104">In this tutorial, you learn how to integrate FileCloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e2ab0-105">Integrera FileCloud med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="e2ab0-105">Integrating FileCloud with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="e2ab0-106">Du kan styra i Azure AD som har åtkomst till FileCloud.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-106">You can control in Azure AD who has access to FileCloud.</span></span>
- <span data-ttu-id="e2ab0-107">Du kan aktivera användarna att automatiskt hämta loggat in på FileCloud (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-107">You can enable your users to automatically get signed-on to FileCloud (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="e2ab0-108">Du kan hantera dina konton i en central plats - Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="e2ab0-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e2ab0-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e2ab0-110">Krav</span><span class="sxs-lookup"><span data-stu-id="e2ab0-110">Prerequisites</span></span>

<span data-ttu-id="e2ab0-111">För att konfigurera Azure AD-integrering med FileCloud, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="e2ab0-111">To configure Azure AD integration with FileCloud, you need the following items:</span></span>

- <span data-ttu-id="e2ab0-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="e2ab0-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e2ab0-113">En FileCloud enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="e2ab0-113">A FileCloud single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e2ab0-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e2ab0-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="e2ab0-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e2ab0-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e2ab0-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e2ab0-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e2ab0-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="e2ab0-118">Scenario description</span></span>
<span data-ttu-id="e2ab0-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e2ab0-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="e2ab0-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e2ab0-121">Att lägga till FileCloud från galleriet</span><span class="sxs-lookup"><span data-stu-id="e2ab0-121">Adding FileCloud from the gallery</span></span>
2. <span data-ttu-id="e2ab0-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e2ab0-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-filecloud-from-the-gallery"></a><span data-ttu-id="e2ab0-123">Att lägga till FileCloud från galleriet</span><span class="sxs-lookup"><span data-stu-id="e2ab0-123">Adding FileCloud from the gallery</span></span>
<span data-ttu-id="e2ab0-124">Du måste lägga till FileCloud från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av FileCloud i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-124">To configure the integration of FileCloud into Azure AD, you need to add FileCloud from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="e2ab0-125">**Utför följande steg för att lägga till FileCloud från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="e2ab0-125">**To add FileCloud from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="e2ab0-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Azure Active Directory-knappen][1]

2. <span data-ttu-id="e2ab0-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="e2ab0-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-129">Then go to **All applications**.</span></span>

    ![Bladet Enterprise program][2]
    
3. <span data-ttu-id="e2ab0-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Knappen Nytt program][3]

4. <span data-ttu-id="e2ab0-133">I sökrutan skriver **FileCloud**väljer **FileCloud** resultatet-panelen klickar **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-133">In the search box, type **FileCloud**, select **FileCloud** from result panel then click **Add** button to add the application.</span></span>

    ![FileCloud i resultatlistan](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="e2ab0-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e2ab0-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="e2ab0-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med FileCloud baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-136">In this section, you configure and test Azure AD single sign-on with FileCloud based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e2ab0-137">Azure AD måste du känna till användaren i FileCloud motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-137">For single sign-on to work, Azure AD needs to know what the counterpart user in FileCloud is to a user in Azure AD.</span></span> <span data-ttu-id="e2ab0-138">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i FileCloud upprättas.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-138">In other words, a link relationship between an Azure AD user and the related user in FileCloud needs to be established.</span></span>

<span data-ttu-id="e2ab0-139">I FileCloud, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-139">In FileCloud, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="e2ab0-140">Om du vill konfigurera och testa Azure AD enkel inloggning med FileCloud, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="e2ab0-140">To configure and test Azure AD single sign-on with FileCloud, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="e2ab0-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="e2ab0-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e2ab0-143">**[Skapa en testanvändare FileCloud](#create-a-filecloud-test-user)**  – du har en motsvarighet för Britta Simon i FileCloud som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-143">**[Create a FileCloud test user](#create-a-filecloud-test-user)** - to have a counterpart of Britta Simon in FileCloud that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="e2ab0-144">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e2ab0-145">**[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="e2ab0-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e2ab0-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="e2ab0-147">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt FileCloud program.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your FileCloud application.</span></span>

<span data-ttu-id="e2ab0-148">**Utför följande steg för att konfigurera Azure AD enkel inloggning med FileCloud:**</span><span class="sxs-lookup"><span data-stu-id="e2ab0-148">**To configure Azure AD single sign-on with FileCloud, perform the following steps:**</span></span>

1. <span data-ttu-id="e2ab0-149">I Azure-portalen på den **FileCloud** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-149">In the Azure portal, on the **FileCloud** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="e2ab0-151">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_samlbase.png)

3. <span data-ttu-id="e2ab0-153">På den **FileCloud domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="e2ab0-153">On the **FileCloud Domain and URLs** section, perform the following steps:</span></span>

    ![URL: er och FileCloud domän med enkel inloggning information](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_url.png)

    <span data-ttu-id="e2ab0-155">a.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-155">a.</span></span> <span data-ttu-id="e2ab0-156">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<subdomain>.filecloudhosted.com`</span><span class="sxs-lookup"><span data-stu-id="e2ab0-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.filecloudhosted.com`</span></span>

    <span data-ttu-id="e2ab0-157">b.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-157">b.</span></span> <span data-ttu-id="e2ab0-158">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<subdomain>.filecloudhosted.com/simplesaml/module.php/saml/sp/metadata.php/default-sp`</span><span class="sxs-lookup"><span data-stu-id="e2ab0-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.filecloudhosted.com/simplesaml/module.php/saml/sp/metadata.php/default-sp`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="e2ab0-159">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-159">These values are not real.</span></span> <span data-ttu-id="e2ab0-160">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-160">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="e2ab0-161">Kontakta [FileCloud klienten supportteamet](mailto:support@codelathe.com) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-161">Contact [FileCloud Client support team](mailto:support@codelathe.com) to get these values.</span></span>

4. <span data-ttu-id="e2ab0-162">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-162">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Länken hämta certifikatet](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_certificate.png) 

5. <span data-ttu-id="e2ab0-164">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-164">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-filecloud-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e2ab0-166">På den **FileCloud Configuration** klickar du på **konfigurera FileCloud** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-166">On the **FileCloud Configuration** section, click **Configure FileCloud** to open **Configure sign-on** window.</span></span> <span data-ttu-id="e2ab0-167">Kopiera den **SAML enhets-ID** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="e2ab0-167">Copy the **SAML Entity ID** from the **Quick Reference section.**</span></span>

    ![FileCloud konfiguration](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_configure.png) 

7. <span data-ttu-id="e2ab0-169">I en annan webbläsarfönster inloggning till FileCloud-klient som administratör.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-169">In a different web browser window, sign-on to your FileCloud tenant as an administrator.</span></span>

8. <span data-ttu-id="e2ab0-170">I det vänstra navigeringsfönstret klickar du på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-170">On the left navigation pane, click **Settings**.</span></span> 
   
    ![Inställningar för avsnittet på App-sida](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_000.png)

9. <span data-ttu-id="e2ab0-172">Klicka på **SSO** för inställningar.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-172">Click **SSO** tab on Settings section.</span></span> 
   
    ![Enkel inloggning fliken på App-sida](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_001.png)

10. <span data-ttu-id="e2ab0-174">Välj **SAML** som **standard SSO typen** på **inställningar för enkel inloggning (SSO)** panelen.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-174">Select **SAML** as **Default SSO Type** on **Single Sign On (SSO) Settings** panel.</span></span>
   
    ![Enkel inloggning panelen i appen inställningar på klientsidan](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_002.png)

11. <span data-ttu-id="e2ab0-176">Klistra in **SAML enhets-ID**, som du har kopierat från Azure-portalen i den **IdP slutpunkt URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-176">Paste **SAML Entity ID**, which you have copied from Azure portal into the **IdP End Point URL** textbox.</span></span>

    ![IDP slutet punkt URL: en textruta](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_003.png)

12. <span data-ttu-id="e2ab0-178">Öppna hämtade metadatafilen i anteckningar, kopiera innehållet i den till Urklipp och klistra in den till den **IdP metadata** textruta på **SAML inställningar** panelen.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-178">Open your downloaded metadata file in notepad, copy the content of it into your clipboard, and then paste it to the **IdP Meta Data** textbox on **SAML Settings** panel.</span></span>

    ![IDP Meta dataavsnittet på App-sida](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_004.png)

13. <span data-ttu-id="e2ab0-180">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-180">Click **Save** button.</span></span>

> [!TIP]
> <span data-ttu-id="e2ab0-181">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="e2ab0-181">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="e2ab0-182">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-182">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="e2ab0-183">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e2ab0-183">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="e2ab0-184">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="e2ab0-184">Create an Azure AD test user</span></span>

<span data-ttu-id="e2ab0-185">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-185">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="e2ab0-187">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="e2ab0-187">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="e2ab0-188">I Azure-portalen i den vänstra rutan klickar du på den **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-188">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Azure Active Directory-knappen](./media/active-directory-saas-filecloud-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="e2ab0-190">Om du vill visa en lista över användare, gå till **användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-190">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![”Användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-filecloud-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="e2ab0-192">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i den **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-192">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Knappen Lägg till](./media/active-directory-saas-filecloud-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="e2ab0-194">I den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="e2ab0-194">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogrutan användare](./media/active-directory-saas-filecloud-tutorial/create_aaduser_04.png)

    <span data-ttu-id="e2ab0-196">a.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-196">a.</span></span> <span data-ttu-id="e2ab0-197">I den **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-197">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e2ab0-198">b.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-198">b.</span></span> <span data-ttu-id="e2ab0-199">I den **användarnamn** Skriv användarens Britta Simon e-postadress.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-199">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="e2ab0-200">c.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-200">c.</span></span> <span data-ttu-id="e2ab0-201">Välj den **visa lösenordet** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-201">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="e2ab0-202">d.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-202">d.</span></span> <span data-ttu-id="e2ab0-203">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-203">Click **Create**.</span></span>
 
### <a name="create-a-filecloud-test-user"></a><span data-ttu-id="e2ab0-204">Skapa en testanvändare FileCloud</span><span class="sxs-lookup"><span data-stu-id="e2ab0-204">Create a FileCloud test user</span></span>

<span data-ttu-id="e2ab0-205">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i FileCloud.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-205">The objective of this section is to create a user called Britta Simon in FileCloud.</span></span> <span data-ttu-id="e2ab0-206">FileCloud stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-206">FileCloud supports just-in-time provisioning, which is by default enabled.</span></span> <span data-ttu-id="e2ab0-207">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-207">There is no action item for you in this section.</span></span> <span data-ttu-id="e2ab0-208">En ny användare skapas under ett försök att komma åt FileCloud om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-208">A new user is created during an attempt to access FileCloud if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="e2ab0-209">Om du behöver skapa en användare manuellt, måste du kontakta den [FileCloud klienten supportteamet](mailto:support@codelathe.com).</span><span class="sxs-lookup"><span data-stu-id="e2ab0-209">If you need to create a user manually, you need to contact the [FileCloud Client support team](mailto:support@codelathe.com).</span></span> 

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="e2ab0-210">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="e2ab0-210">Assign the Azure AD test user</span></span>

<span data-ttu-id="e2ab0-211">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till FileCloud.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-211">In this section, you enable Britta Simon to use Azure single sign-on by granting access to FileCloud.</span></span>

![Tilldela rollen][200] 

<span data-ttu-id="e2ab0-213">**Om du vill tilldela FileCloud Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="e2ab0-213">**To assign Britta Simon to FileCloud, perform the following steps:**</span></span>

1. <span data-ttu-id="e2ab0-214">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-214">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="e2ab0-216">Välj i listan med program **FileCloud**.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-216">In the applications list, select **FileCloud**.</span></span>

    ![Länken FileCloud i listan med program](./media/active-directory-saas-filecloud-tutorial/tutorial_filecloud_app.png)  

3. <span data-ttu-id="e2ab0-218">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-218">In the menu on the left, click **Users and groups**.</span></span>

    ![Länken ”användare och grupper”][202]

4. <span data-ttu-id="e2ab0-220">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-220">Click **Add** button.</span></span> <span data-ttu-id="e2ab0-221">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-221">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Fönstret Lägg till tilldelning][203]

5. <span data-ttu-id="e2ab0-223">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-223">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="e2ab0-224">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-224">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e2ab0-225">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-225">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="e2ab0-226">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e2ab0-226">Test single sign-on</span></span>

<span data-ttu-id="e2ab0-227">Syftet med det här avsnittet är att testa Azure AD SSO-konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-227">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="e2ab0-228">När du klickar på panelen FileCloud på åtkomstpanelen du bör få automatiskt loggat in på ditt FileCloud program.</span><span class="sxs-lookup"><span data-stu-id="e2ab0-228">When you click the FileCloud tile in the Access Panel, you should get automatically signed-on to your FileCloud application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e2ab0-229">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="e2ab0-229">Additional resources</span></span>

* [<span data-ttu-id="e2ab0-230">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e2ab0-230">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e2ab0-231">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e2ab0-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-filecloud-tutorial/tutorial_general_203.png

