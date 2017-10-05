---
title: "Självstudier: Azure Active Directory-integrering med FilesAnywhere | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och FilesAnywhere."
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
ms.date: 03/17/2017
ms.author: jeedes
ms.openlocfilehash: 4153056bd21006061c6ad8ff9cf3c17de9248628
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-filesanywhere"></a><span data-ttu-id="d808e-103">Självstudier: Azure Active Directory-integrering med FilesAnywhere</span><span class="sxs-lookup"><span data-stu-id="d808e-103">Tutorial: Azure Active Directory integration with FilesAnywhere</span></span>

<span data-ttu-id="d808e-104">I kursen får lära du att integrera FilesAnywhere med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="d808e-104">In this tutorial, you learn how to integrate FilesAnywhere with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d808e-105">Integrera FilesAnywhere med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="d808e-105">Integrating FilesAnywhere with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="d808e-106">Du kan styra i Azure AD som har åtkomst till FilesAnywhere</span><span class="sxs-lookup"><span data-stu-id="d808e-106">You can control in Azure AD who has access to FilesAnywhere</span></span>
- <span data-ttu-id="d808e-107">Du kan aktivera användarna att automatiskt hämta loggat in på FilesAnywhere (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="d808e-107">You can enable your users to automatically get signed-on to FilesAnywhere (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d808e-108">Du kan hantera dina konton i en central plats - till Azure-hanteringsportalen</span><span class="sxs-lookup"><span data-stu-id="d808e-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="d808e-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d808e-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d808e-110">Krav</span><span class="sxs-lookup"><span data-stu-id="d808e-110">Prerequisites</span></span>

<span data-ttu-id="d808e-111">För att konfigurera Azure AD-integrering med FilesAnywhere, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="d808e-111">To configure Azure AD integration with FilesAnywhere, you need the following items:</span></span>

- <span data-ttu-id="d808e-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="d808e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d808e-113">En FilesAnywhere enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="d808e-113">A FilesAnywhere single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="d808e-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="d808e-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="d808e-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="d808e-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d808e-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="d808e-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="d808e-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d808e-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="d808e-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="d808e-118">Scenario description</span></span>
<span data-ttu-id="d808e-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="d808e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d808e-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="d808e-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d808e-121">Att lägga till FilesAnywhere från galleriet</span><span class="sxs-lookup"><span data-stu-id="d808e-121">Adding FilesAnywhere from the gallery</span></span>
2. <span data-ttu-id="d808e-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d808e-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-filesanywhere-from-the-gallery"></a><span data-ttu-id="d808e-123">Att lägga till FilesAnywhere från galleriet</span><span class="sxs-lookup"><span data-stu-id="d808e-123">Adding FilesAnywhere from the gallery</span></span>
<span data-ttu-id="d808e-124">Du måste lägga till FilesAnywhere från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av FilesAnywhere i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d808e-124">To configure the integration of FilesAnywhere into Azure AD, you need to add FilesAnywhere from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="d808e-125">**Utför följande steg för att lägga till FilesAnywhere från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="d808e-125">**To add FilesAnywhere from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="d808e-126">I den  **[Azure-hanteringsportalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d808e-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d808e-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="d808e-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="d808e-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="d808e-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="d808e-131">Klicka på **Lägg till** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d808e-131">Click **Add** button on the top of the dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="d808e-133">I sökrutan skriver **FilesAnywhere**.</span><span class="sxs-lookup"><span data-stu-id="d808e-133">In the search box, type **FilesAnywhere**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_search.png)

5. <span data-ttu-id="d808e-135">Välj i resultatpanelen **FilesAnywhere**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="d808e-135">In the results panel, select **FilesAnywhere**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_addfromgallery.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d808e-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d808e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d808e-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med FilesAnywhere baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="d808e-138">In this section, you configure and test Azure AD single sign-on with FilesAnywhere based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d808e-139">Azure AD måste du känna till användaren i FilesAnywhere motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="d808e-139">For single sign-on to work, Azure AD needs to know what the counterpart user in FilesAnywhere is to a user in Azure AD.</span></span> <span data-ttu-id="d808e-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i FilesAnywhere upprättas.</span><span class="sxs-lookup"><span data-stu-id="d808e-140">In other words, a link relationship between an Azure AD user and the related user in FilesAnywhere needs to be established.</span></span>

<span data-ttu-id="d808e-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i FilesAnywhere.</span><span class="sxs-lookup"><span data-stu-id="d808e-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in FilesAnywhere.</span></span>

<span data-ttu-id="d808e-142">Om du vill konfigurera och testa Azure AD enkel inloggning med FilesAnywhere, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="d808e-142">To configure and test Azure AD single sign-on with FilesAnywhere, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="d808e-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="d808e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="d808e-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d808e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d808e-145">**[Skapa en testanvändare FilesAnywhere](#creating-a-filesanywhere-test-user)**  – du har en motsvarighet för Britta Simon i FilesAnywhere som är kopplad till Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="d808e-145">**[Creating a FilesAnywhere test user](#creating-a-filesanywhere-test-user)** - to have a counterpart of Britta Simon in FilesAnywhere that is linked to the Azure AD representation of her.</span></span>
3. <span data-ttu-id="d808e-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d808e-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
4. <span data-ttu-id="d808e-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="d808e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d808e-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d808e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d808e-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-hanteringsportalen och konfigurera enkel inloggning i ditt FilesAnywhere program.</span><span class="sxs-lookup"><span data-stu-id="d808e-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your FilesAnywhere application.</span></span>

<span data-ttu-id="d808e-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med FilesAnywhere:**</span><span class="sxs-lookup"><span data-stu-id="d808e-150">**To configure Azure AD single sign-on with FilesAnywhere, perform the following steps:**</span></span>

1. <span data-ttu-id="d808e-151">I Azure-hanteringsportalen på den **FilesAnywhere** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="d808e-151">In the Azure Management portal, on the **FilesAnywhere** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="d808e-153">På den **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** att aktivera enkel inloggning på.</span><span class="sxs-lookup"><span data-stu-id="d808e-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_samlbase.png)

3. <span data-ttu-id="d808e-155">På den **FilesAnywhere domän och URL: er** om du vill konfigurera programmet i **IDP initierade läge**:</span><span class="sxs-lookup"><span data-stu-id="d808e-155">On the **FilesAnywhere Domain and URLs** section, If you wish to configure the application in **IDP initiated mode**:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_filesanywhere_url.png)
    
    <span data-ttu-id="d808e-157">a.</span><span class="sxs-lookup"><span data-stu-id="d808e-157">a.</span></span> <span data-ttu-id="d808e-158">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://<company name>.filesanywhere.com/saml20.aspx?c=215`</span><span class="sxs-lookup"><span data-stu-id="d808e-158">In the **Reply URL** textbox, type a URL using the following pattern: `https://<company name>.filesanywhere.com/saml20.aspx?c=215`</span></span>
> [!NOTE]
> <span data-ttu-id="d808e-159">Observera att värdet **215** är en **clientid** och är bara ett exempel.</span><span class="sxs-lookup"><span data-stu-id="d808e-159">Please note that the value **215** is a **clientid** and is just an example.</span></span> <span data-ttu-id="d808e-160">Du måste ersätta den med det faktiska clientid-värdet.</span><span class="sxs-lookup"><span data-stu-id="d808e-160">You need to replace it with the actual clientid value.</span></span>

4. <span data-ttu-id="d808e-161">På den **FilesAnywhere domän och URL: er** om du vill konfigurera programmet i **SP initierade läge**, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="d808e-161">On the **FilesAnywhere Domain and URLs** section, If you wish to configure the application in **SP initiated mode**, perform the following steps:</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_filesanywhere_url1.png)

    <span data-ttu-id="d808e-163">a.</span><span class="sxs-lookup"><span data-stu-id="d808e-163">a.</span></span> <span data-ttu-id="d808e-164">Klicka på den **visa avancerade inställningar för URL: en** alternativet</span><span class="sxs-lookup"><span data-stu-id="d808e-164">Click on the **Show advanced URL settings** option</span></span>

    <span data-ttu-id="d808e-165">b.</span><span class="sxs-lookup"><span data-stu-id="d808e-165">b.</span></span> <span data-ttu-id="d808e-166">I den **logga URL** textruta Skriv en URL med följande mönster:`https://<sub domain>.filesanywhere.com/`</span><span class="sxs-lookup"><span data-stu-id="d808e-166">In the **Sign On URL** textbox, type a URL using the following pattern: `https://<sub domain>.filesanywhere.com/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d808e-167">Observera att detta inte är verkliga värden.</span><span class="sxs-lookup"><span data-stu-id="d808e-167">Please note that these are not the real values.</span></span> <span data-ttu-id="d808e-168">Du måste uppdatera dessa värden med de faktiska logga URL och Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="d808e-168">You have to update these values with the actual Sign On URL and Reply URL.</span></span> <span data-ttu-id="d808e-169">Kontakta [FilesAnywhere supportteamet](mailto:support@FilesAnywhere.com) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="d808e-169">Contact [FilesAnywhere support team](mailto:support@FilesAnywhere.com) to get these values.</span></span> 

5. <span data-ttu-id="d808e-170">FilesAnywhere program förväntar SAML-intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="d808e-170">FilesAnywhere Software application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="d808e-171">Konfigurera följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="d808e-171">Please configure the following claims for this application.</span></span> <span data-ttu-id="d808e-172">Du kan hantera värden för attributen från den ”**användarattribut**” avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="d808e-172">You can manage the values of these attributes from the "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="d808e-173">Följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="d808e-173">The following screenshot shows an example for this.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_filesanywhere_attribute.png)
    
    <span data-ttu-id="d808e-175">När användarna loggar med FilesAnywhere de hämta värdet för **clientid** -attributet från [FilesAnywhere team](mailto:support@FilesAnywhere.com).</span><span class="sxs-lookup"><span data-stu-id="d808e-175">When the users signs up with FilesAnywhere they get the value of **clientid** attribute from [FilesAnywhere team](mailto:support@FilesAnywhere.com).</span></span> <span data-ttu-id="d808e-176">Du måste lägga till ”klient-Id”-attribut med det unika värdet som tillhandahålls av FilesAnywhere.</span><span class="sxs-lookup"><span data-stu-id="d808e-176">You have to add the "Client Id" attribute with the unique value provided by FilesAnywhere.</span></span> <span data-ttu-id="d808e-177">Dessa attribut som visas ovan krävs.</span><span class="sxs-lookup"><span data-stu-id="d808e-177">All these attributes shown above are required.</span></span>
    > [!NOTE] 
    > <span data-ttu-id="d808e-178">Observera att värdet **2331** av **clientid** är bara ett exempel.</span><span class="sxs-lookup"><span data-stu-id="d808e-178">Please note that the value **2331** of **clientid** is just an example.</span></span> <span data-ttu-id="d808e-179">Du måste ange det faktiska värdet.</span><span class="sxs-lookup"><span data-stu-id="d808e-179">You need to provide the actual value.</span></span>


6. <span data-ttu-id="d808e-180">I den **användarattribut** avsnitt på den **enkel inloggning** dialogrutan Konfigurera attribut för SAML-token som visas i bilden ovan och utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="d808e-180">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image above and perform the following steps:</span></span>
    
    | <span data-ttu-id="d808e-181">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="d808e-181">Attribute Name</span></span> | <span data-ttu-id="d808e-182">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="d808e-182">Attribute Value</span></span> |
    | ---------------| --------------- |    
    | <span data-ttu-id="d808e-183">clientid</span><span class="sxs-lookup"><span data-stu-id="d808e-183">clientid</span></span> | <span data-ttu-id="d808e-184">*”uniquevalue”*</span><span class="sxs-lookup"><span data-stu-id="d808e-184">*"uniquevalue"*</span></span> |

    <span data-ttu-id="d808e-185">a.</span><span class="sxs-lookup"><span data-stu-id="d808e-185">a.</span></span> <span data-ttu-id="d808e-186">Klicka på **Lägg till attributet** att öppna den **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d808e-186">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_05.png)
    
    <span data-ttu-id="d808e-189">b.</span><span class="sxs-lookup"><span data-stu-id="d808e-189">b.</span></span> <span data-ttu-id="d808e-190">I den **namn** textruta ange attributets namn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="d808e-190">In the **Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="d808e-191">c.</span><span class="sxs-lookup"><span data-stu-id="d808e-191">c.</span></span> <span data-ttu-id="d808e-192">Från den **värdet** listan, ange det attributvärde som visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="d808e-192">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="d808e-193">d.</span><span class="sxs-lookup"><span data-stu-id="d808e-193">d.</span></span> <span data-ttu-id="d808e-194">Klicka på **Ok**</span><span class="sxs-lookup"><span data-stu-id="d808e-194">Click **Ok**</span></span>

7. <span data-ttu-id="d808e-195">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="d808e-195">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="d808e-197">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="d808e-197">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_certificate.png) 

9. <span data-ttu-id="d808e-199">På den **FilesAnywhere Configuration** klickar du på **konfigurera FilesAnywhere** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="d808e-199">On the **FilesAnywhere Configuration** section, click **Configure FilesAnywhere** to open **Configure sign-on** window.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_configure.png) 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_configuresignon.png)

10. <span data-ttu-id="d808e-202">För att få SSO konfigurationen klar för tillämpningsprogrammet FilesAnywhere slutet, kontakta [FilesAnywhere supportteamet](mailto:support@FilesAnywhere.com) och ge dem hämtade SAML tokensignering certifikat och enkel inloggning (SSO)-URL.</span><span class="sxs-lookup"><span data-stu-id="d808e-202">To get SSO configuration complete for your application at FilesAnywhere end, contact [FilesAnywhere support team](mailto:support@FilesAnywhere.com) and provide them the downloaded SAML token signing Certificate and Single Sign On (SSO) URL.</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d808e-203">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="d808e-203">Creating an Azure AD test user</span></span>
<span data-ttu-id="d808e-204">Syftet med det här avsnittet är att skapa en testanvändare i Azure Management portal kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d808e-204">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="d808e-206">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="d808e-206">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="d808e-207">I den **Azure-hanteringsportalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d808e-207">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-FilesAnywhere-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d808e-209">Gå till **användare och grupper** och på **alla användare** att visa en lista över användare.</span><span class="sxs-lookup"><span data-stu-id="d808e-209">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-FilesAnywhere-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d808e-211">Klicka på överst i dialogrutan **Lägg till** att öppna den **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d808e-211">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-FilesAnywhere-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d808e-213">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="d808e-213">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-FilesAnywhere-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d808e-215">a.</span><span class="sxs-lookup"><span data-stu-id="d808e-215">a.</span></span> <span data-ttu-id="d808e-216">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d808e-216">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d808e-217">b.</span><span class="sxs-lookup"><span data-stu-id="d808e-217">b.</span></span> <span data-ttu-id="d808e-218">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d808e-218">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d808e-219">c.</span><span class="sxs-lookup"><span data-stu-id="d808e-219">c.</span></span> <span data-ttu-id="d808e-220">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="d808e-220">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="d808e-221">d.</span><span class="sxs-lookup"><span data-stu-id="d808e-221">d.</span></span> <span data-ttu-id="d808e-222">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="d808e-222">Click **Create**.</span></span> 



### <a name="creating-a-filesanywhere-test-user"></a><span data-ttu-id="d808e-223">Skapa en testanvändare FilesAnywhere</span><span class="sxs-lookup"><span data-stu-id="d808e-223">Creating a FilesAnywhere test user</span></span>

<span data-ttu-id="d808e-224">Programmet stöder bara i tid användaretablering och authentication-användare kommer automatiskt att skapas i programmet.</span><span class="sxs-lookup"><span data-stu-id="d808e-224">Application supports Just in time user provisioning and after authentication users will be created in the application automatically.</span></span> 


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="d808e-225">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="d808e-225">Assigning the Azure AD test user</span></span>

<span data-ttu-id="d808e-226">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja sin åtkomst till FilesAnywhere.</span><span class="sxs-lookup"><span data-stu-id="d808e-226">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to FilesAnywhere.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="d808e-228">**Om du vill tilldela FilesAnywhere Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d808e-228">**To assign Britta Simon to FilesAnywhere, perform the following steps:**</span></span>

1. <span data-ttu-id="d808e-229">Öppna vyn program i Azure-hanteringsportalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="d808e-229">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="d808e-231">Välj i listan med program **FilesAnywhere**.</span><span class="sxs-lookup"><span data-stu-id="d808e-231">In the applications list, select **FilesAnywhere**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_app.png) 

3. <span data-ttu-id="d808e-233">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="d808e-233">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="d808e-235">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="d808e-235">Click **Add** button.</span></span> <span data-ttu-id="d808e-236">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d808e-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="d808e-238">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="d808e-238">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="d808e-239">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d808e-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d808e-240">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d808e-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    


### <a name="testing-single-sign-on"></a><span data-ttu-id="d808e-241">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d808e-241">Testing single sign-on</span></span>

<span data-ttu-id="d808e-242">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="d808e-242">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="d808e-243">När du klickar på panelen FilesAnywhere på åtkomstpanelen du bör få automatiskt loggat in på ditt FilesAnywhere program.</span><span class="sxs-lookup"><span data-stu-id="d808e-243">When you click the FilesAnywhere tile in the Access Panel, you should get automatically signed-on to your FilesAnywhere application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="d808e-244">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="d808e-244">Additional resources</span></span>

* [<span data-ttu-id="d808e-245">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d808e-245">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d808e-246">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d808e-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_203.png
