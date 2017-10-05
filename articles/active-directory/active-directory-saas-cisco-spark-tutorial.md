---
title: "Självstudier: Azure Active Directory-integrering med Cisco Spark | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Cisco Spark."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c47894b1-f5df-4755-845d-f12f4c602dc4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: a0a221622afe1c801a331e2319f3a7ace3111dad
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-spark"></a><span data-ttu-id="659d8-103">Självstudier: Azure Active Directory-integrering med Cisco Spark</span><span class="sxs-lookup"><span data-stu-id="659d8-103">Tutorial: Azure Active Directory integration with Cisco Spark</span></span>

<span data-ttu-id="659d8-104">I kursen får lära du att integrera Cisco Spark med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="659d8-104">In this tutorial, you learn how to integrate Cisco Spark with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="659d8-105">Integrera Cisco Spark med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="659d8-105">Integrating Cisco Spark with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="659d8-106">Du kan styra i Azure AD som har åtkomst till Cisco Spark</span><span class="sxs-lookup"><span data-stu-id="659d8-106">You can control in Azure AD who has access to Cisco Spark</span></span>
- <span data-ttu-id="659d8-107">Du kan aktivera användarna att automatiskt hämta loggat in på Cisco Spark (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="659d8-107">You can enable your users to automatically get signed-on to Cisco Spark (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="659d8-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="659d8-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="659d8-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="659d8-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="659d8-110">Krav</span><span class="sxs-lookup"><span data-stu-id="659d8-110">Prerequisites</span></span>

<span data-ttu-id="659d8-111">För att konfigurera Azure AD-integrering med Cisco Spark, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="659d8-111">To configure Azure AD integration with Cisco Spark, you need the following items:</span></span>

- <span data-ttu-id="659d8-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="659d8-112">An Azure AD subscription</span></span>
- <span data-ttu-id="659d8-113">En Cisco-Spark enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="659d8-113">A Cisco Spark single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="659d8-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="659d8-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="659d8-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="659d8-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="659d8-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="659d8-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="659d8-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="659d8-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="659d8-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="659d8-118">Scenario description</span></span>
<span data-ttu-id="659d8-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="659d8-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="659d8-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="659d8-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="659d8-121">Att lägga till Cisco Spark från galleriet</span><span class="sxs-lookup"><span data-stu-id="659d8-121">Adding Cisco Spark from the gallery</span></span>
2. <span data-ttu-id="659d8-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="659d8-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-cisco-spark-from-the-gallery"></a><span data-ttu-id="659d8-123">Att lägga till Cisco Spark från galleriet</span><span class="sxs-lookup"><span data-stu-id="659d8-123">Adding Cisco Spark from the gallery</span></span>
<span data-ttu-id="659d8-124">Du måste lägga till Cisco Spark från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Cisco Spark i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="659d8-124">To configure the integration of Cisco Spark into Azure AD, you need to add Cisco Spark from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="659d8-125">**Utför följande steg för att lägga till Cisco Spark från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="659d8-125">**To add Cisco Spark from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="659d8-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="659d8-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="659d8-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="659d8-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="659d8-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="659d8-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="659d8-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="659d8-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="659d8-133">I sökrutan skriver **Cisco Spark**.</span><span class="sxs-lookup"><span data-stu-id="659d8-133">In the search box, type **Cisco Spark**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_search.png)

5. <span data-ttu-id="659d8-135">Välj i resultatpanelen **Cisco Spark**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="659d8-135">In the results panel, select **Cisco Spark**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="659d8-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="659d8-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="659d8-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Cisco Spark baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="659d8-138">In this section, you configure and test Azure AD single sign-on with Cisco Spark based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="659d8-139">Azure AD måste du känna till användaren i Cisco Spark motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="659d8-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Cisco Spark is to a user in Azure AD.</span></span> <span data-ttu-id="659d8-140">Med andra ord måste en länk mellan en Azure AD-användare och relaterade användaren i Cisco Spark upprättas.</span><span class="sxs-lookup"><span data-stu-id="659d8-140">In other words, a link relationship between an Azure AD user and the related user in Cisco Spark needs to be established.</span></span>

<span data-ttu-id="659d8-141">I Cisco Spark, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="659d8-141">In Cisco Spark, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="659d8-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Cisco Spark, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="659d8-142">To configure and test Azure AD single sign-on with Cisco Spark, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="659d8-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="659d8-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="659d8-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="659d8-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="659d8-145">**[Skapa en testanvändare Cisco Spark](#creating-a-cisco-spark-test-user)**  – du har en motsvarighet för Britta Simon i Cisco Spark som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="659d8-145">**[Creating a Cisco Spark test user](#creating-a-cisco-spark-test-user)** - to have a counterpart of Britta Simon in Cisco Spark that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="659d8-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="659d8-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="659d8-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="659d8-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="659d8-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="659d8-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="659d8-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i Cisco Spark-program.</span><span class="sxs-lookup"><span data-stu-id="659d8-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Cisco Spark application.</span></span>

<span data-ttu-id="659d8-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Cisco Spark:**</span><span class="sxs-lookup"><span data-stu-id="659d8-150">**To configure Azure AD single sign-on with Cisco Spark, perform the following steps:**</span></span>

1. <span data-ttu-id="659d8-151">I Azure-portalen på den **Cisco Spark** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="659d8-151">In the Azure portal, on the **Cisco Spark** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="659d8-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="659d8-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_samlbase.png)

3. <span data-ttu-id="659d8-155">På den **Cisco Spark domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="659d8-155">On the **Cisco Spark Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_url.png)

    <span data-ttu-id="659d8-157">a.</span><span class="sxs-lookup"><span data-stu-id="659d8-157">a.</span></span> <span data-ttu-id="659d8-158">I den **inloggnings-URL** textruta Skriv en URL som:`https://web.ciscospark.com/#/signin`</span><span class="sxs-lookup"><span data-stu-id="659d8-158">In the **Sign-on URL** textbox, type a URL as: `https://web.ciscospark.com/#/signin`</span></span>

    <span data-ttu-id="659d8-159">b.</span><span class="sxs-lookup"><span data-stu-id="659d8-159">b.</span></span> <span data-ttu-id="659d8-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://idbroker.webex.com/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="659d8-160">In the **Identifier** textbox, type a URL using the following pattern: `https://idbroker.webex.com/<companyname>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="659d8-161">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="659d8-161">This value is not real.</span></span> <span data-ttu-id="659d8-162">Uppdatera det här värdet med den faktiska identifieraren.</span><span class="sxs-lookup"><span data-stu-id="659d8-162">Update this value with the actual Identifier.</span></span> <span data-ttu-id="659d8-163">Kontakta [Cisco Spark klient supportteamet](https://support.ciscospark.com/) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="659d8-163">Contact [Cisco Spark Client support team](https://support.ciscospark.com/) to get this value.</span></span> 
 
4. <span data-ttu-id="659d8-164">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="659d8-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_certificate.png) 

5. <span data-ttu-id="659d8-166">Cisco Spark program förväntar SAML intyg som innehåller specifika attribut.</span><span class="sxs-lookup"><span data-stu-id="659d8-166">Cisco Spark application expects the SAML assertions to contain specific attributes.</span></span> <span data-ttu-id="659d8-167">Konfigurera följande attribut för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="659d8-167">Configure the following attributes  for this application.</span></span> <span data-ttu-id="659d8-168">Du kan hantera värden för attributen från den **användarattribut** avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="659d8-168">You can manage the values of these attributes from the **User Attributes** section on application integration page.</span></span> <span data-ttu-id="659d8-169">Följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="659d8-169">The following screenshot shows an example for this.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_07.png) 

6. <span data-ttu-id="659d8-171">I den **användarattribut** avsnitt på den **enkel inloggning** dialogrutan Konfigurera attribut för SAML-token som visas i bilden ovan och utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="659d8-171">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image above and perform the following steps:</span></span>
    
    | <span data-ttu-id="659d8-172">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="659d8-172">Attribute Name</span></span>  | <span data-ttu-id="659d8-173">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="659d8-173">Attribute Value</span></span> |
    | --------------- | -------------------- |    
    |   <span data-ttu-id="659d8-174">UID</span><span class="sxs-lookup"><span data-stu-id="659d8-174">uid</span></span>    | <span data-ttu-id="659d8-175">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="659d8-175">user.userprincipalname</span></span> |   

    <span data-ttu-id="659d8-176">a.</span><span class="sxs-lookup"><span data-stu-id="659d8-176">a.</span></span> <span data-ttu-id="659d8-177">Klicka på **Lägg till attributet** att öppna den **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="659d8-177">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-spark-tutorial/tutorial_attribute_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-spark-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="659d8-180">b.</span><span class="sxs-lookup"><span data-stu-id="659d8-180">b.</span></span> <span data-ttu-id="659d8-181">I den **namn** textruta ange attributets namn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="659d8-181">In the **Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="659d8-182">c.</span><span class="sxs-lookup"><span data-stu-id="659d8-182">c.</span></span> <span data-ttu-id="659d8-183">Från den **värdet** listan, ange det attributvärde som visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="659d8-183">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="659d8-184">d.</span><span class="sxs-lookup"><span data-stu-id="659d8-184">d.</span></span> <span data-ttu-id="659d8-185">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="659d8-185">Click **Ok**.</span></span>

7. <span data-ttu-id="659d8-186">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="659d8-186">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="659d8-188">Logga in på [Cisco Molnhantering samarbete](https://admin.ciscospark.com/) med din fullständiga administratörsautentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="659d8-188">Sign in to [Cisco Cloud Collaboration Management](https://admin.ciscospark.com/) with your full administrator credentials.</span></span>

9. <span data-ttu-id="659d8-189">Välj **inställningar** och under den **autentisering** klickar du på **ändra**.</span><span class="sxs-lookup"><span data-stu-id="659d8-189">Select **Settings** and under the **Authentication** section, click **Modify**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_10.png)
    
10. <span data-ttu-id="659d8-191">Välj **integrera en identitetsleverantör med 3 part. (Avancerat)**  och gå till nästa sida.</span><span class="sxs-lookup"><span data-stu-id="659d8-191">Select **Integrate a 3rd-party identity provider. (Advanced)** and go to the next screen.</span></span>

11. <span data-ttu-id="659d8-192">På den **importera Idp Metadata** sidan antingen dra och släppa Azure AD-metadatafil till sidan eller Använd en webbläsare filalternativet Leta upp och ladda upp Azure AD-metadatafil.</span><span class="sxs-lookup"><span data-stu-id="659d8-192">On the **Import Idp Metadata** page, either drag and drop the Azure AD metadata file onto the page or use the file browser option to locate and upload the Azure AD metadata file.</span></span> <span data-ttu-id="659d8-193">Markera **kräver certifikat som signerats av en certifikatutfärdare i Metadata (säkrare)** och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="659d8-193">Then, select **Require certificate signed by a certificate authority in Metadata (more secure)** and click **Next**.</span></span> 
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_11.png)

12. <span data-ttu-id="659d8-195">Välj **SSO Testanslutningen**, och när en ny webbläsarflik öppnas autentisera med Azure AD genom att logga in.</span><span class="sxs-lookup"><span data-stu-id="659d8-195">Select **Test SSO Connection**, and when a new browser tab opens, authenticate with Azure AD by signing in.</span></span>

13. <span data-ttu-id="659d8-196">Gå tillbaka till den **Cisco samarbete Molnhantering** flik i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="659d8-196">Return to the **Cisco Cloud Collaboration Management** browser tab.</span></span> <span data-ttu-id="659d8-197">Om testet lyckades, väljer **det här testet lyckades. Aktivera enkel inloggning alternativet** och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="659d8-197">If the test was successful, select **This test was successful. Enable Single Sign-On option** and click **Next**.</span></span>

> [!TIP]
> <span data-ttu-id="659d8-198">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="659d8-198">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="659d8-199">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="659d8-199">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="659d8-200">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="659d8-200">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="659d8-201">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="659d8-201">Creating an Azure AD test user</span></span>
<span data-ttu-id="659d8-202">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="659d8-202">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="659d8-204">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="659d8-204">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="659d8-205">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="659d8-205">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="659d8-207">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="659d8-207">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="659d8-209">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="659d8-209">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="659d8-211">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="659d8-211">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="659d8-213">a.</span><span class="sxs-lookup"><span data-stu-id="659d8-213">a.</span></span> <span data-ttu-id="659d8-214">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="659d8-214">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="659d8-215">b.</span><span class="sxs-lookup"><span data-stu-id="659d8-215">b.</span></span> <span data-ttu-id="659d8-216">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="659d8-216">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="659d8-217">c.</span><span class="sxs-lookup"><span data-stu-id="659d8-217">c.</span></span> <span data-ttu-id="659d8-218">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="659d8-218">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="659d8-219">d.</span><span class="sxs-lookup"><span data-stu-id="659d8-219">d.</span></span> <span data-ttu-id="659d8-220">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="659d8-220">Click **Create**.</span></span>
 
### <a name="creating-a-cisco-spark-test-user"></a><span data-ttu-id="659d8-221">Skapa en testanvändare Cisco Spark</span><span class="sxs-lookup"><span data-stu-id="659d8-221">Creating a Cisco Spark test user</span></span>

<span data-ttu-id="659d8-222">I det här avsnittet skapar du en användare som kallas Britta Simon i Cisco Spark.</span><span class="sxs-lookup"><span data-stu-id="659d8-222">In this section, you create a user called Britta Simon in Cisco Spark.</span></span> <span data-ttu-id="659d8-223">I det här avsnittet skapar du en användare som kallas Britta Simon i Cisco Spark.</span><span class="sxs-lookup"><span data-stu-id="659d8-223">In this section, you create a user called Britta Simon in Cisco Spark.</span></span>

1. <span data-ttu-id="659d8-224">Gå till den [Cisco Molnhantering samarbete](https://admin.ciscospark.com/) med din fullständiga administratörsautentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="659d8-224">Go to the [Cisco Cloud Collaboration Management](https://admin.ciscospark.com/) with your full administrator credentials.</span></span>

2. <span data-ttu-id="659d8-225">Klicka på **användare** och sedan **hantera användare**.</span><span class="sxs-lookup"><span data-stu-id="659d8-225">Click **Users** and then **Manage Users**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_12.png) 

3. <span data-ttu-id="659d8-227">I den **hantera användare** väljer **manuellt lägga till eller ändra användare** och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="659d8-227">In the **Manage User** window, select **Manually add or modify users** and click **Next**.</span></span>

4. <span data-ttu-id="659d8-228">Välj **namn och e-postadress**.</span><span class="sxs-lookup"><span data-stu-id="659d8-228">Select **Names and Email address**.</span></span> <span data-ttu-id="659d8-229">Fyll i textrutan på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="659d8-229">Then, fill out the textbox as follows:</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_13.png) 
    
    <span data-ttu-id="659d8-231">a.</span><span class="sxs-lookup"><span data-stu-id="659d8-231">a.</span></span> <span data-ttu-id="659d8-232">I den **Förnamn** textruta typen **Britta**.</span><span class="sxs-lookup"><span data-stu-id="659d8-232">In the **First Name** textbox, type **Britta**.</span></span> 
    
    <span data-ttu-id="659d8-233">b.</span><span class="sxs-lookup"><span data-stu-id="659d8-233">b.</span></span> <span data-ttu-id="659d8-234">I den **efternamn** textruta typen **Simon**.</span><span class="sxs-lookup"><span data-stu-id="659d8-234">In the **Last Name** textbox, type **Simon**.</span></span>
    
    <span data-ttu-id="659d8-235">c.</span><span class="sxs-lookup"><span data-stu-id="659d8-235">c.</span></span> <span data-ttu-id="659d8-236">I den **e-postadress** textruta typen  **britta.simon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="659d8-236">In the **Email address** textbox, type **britta.simon@contoso.com**.</span></span>

5. <span data-ttu-id="659d8-237">Klicka på plustecknet för att lägga till Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="659d8-237">Click the plus sign to add Britta Simon.</span></span> <span data-ttu-id="659d8-238">Klicka sedan på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="659d8-238">Then, click **Next**.</span></span>

6. <span data-ttu-id="659d8-239">I den **Lägg till tjänster för användare** -fönstret klickar du på **spara** och sedan **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="659d8-239">In the **Add Services for Users** window, click **Save** and then **Finish**.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="659d8-240">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="659d8-240">Assigning the Azure AD test user</span></span>

<span data-ttu-id="659d8-241">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Cisco Spark.</span><span class="sxs-lookup"><span data-stu-id="659d8-241">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Cisco Spark.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="659d8-243">**Om du vill tilldela Cisco Spark Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="659d8-243">**To assign Britta Simon to Cisco Spark, perform the following steps:**</span></span>

1. <span data-ttu-id="659d8-244">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="659d8-244">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="659d8-246">Välj i listan med program **Cisco Spark**.</span><span class="sxs-lookup"><span data-stu-id="659d8-246">In the applications list, select **Cisco Spark**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_app.png) 

3. <span data-ttu-id="659d8-248">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="659d8-248">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="659d8-250">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="659d8-250">Click **Add** button.</span></span> <span data-ttu-id="659d8-251">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="659d8-251">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="659d8-253">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="659d8-253">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="659d8-254">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="659d8-254">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="659d8-255">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="659d8-255">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="659d8-256">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="659d8-256">Testing single sign-on</span></span>

<span data-ttu-id="659d8-257">Syftet med det här avsnittet är att testa Azure AD SSO-konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="659d8-257">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="659d8-258">När du klickar på panelen Cisco Spark på åtkomstpanelen du bör få automatiskt loggat in på ditt Cisco Spark-program.</span><span class="sxs-lookup"><span data-stu-id="659d8-258">When you click the Cisco Spark tile in the Access Panel, you should get automatically signed-on to your Cisco Spark application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="659d8-259">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="659d8-259">Additional resources</span></span>

* [<span data-ttu-id="659d8-260">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="659d8-260">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="659d8-261">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="659d8-261">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_04.png
[10]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_060.png
[100]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_203.png

