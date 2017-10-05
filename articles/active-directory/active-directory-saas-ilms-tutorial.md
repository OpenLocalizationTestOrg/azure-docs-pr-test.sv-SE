---
title: "Självstudier: Azure Active Directory-integrering med iLMS | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och iLMS."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d6e11639-6cea-48c9-b008-246cf686e726
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/13/2017
ms.author: jeedes
ms.openlocfilehash: 22c72020200138e78835ed7dd2661f18b824c785
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ilms"></a><span data-ttu-id="2016a-103">Självstudier: Azure Active Directory-integrering med iLMS</span><span class="sxs-lookup"><span data-stu-id="2016a-103">Tutorial: Azure Active Directory integration with iLMS</span></span>

<span data-ttu-id="2016a-104">I kursen får lära du att integrera iLMS med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="2016a-104">In this tutorial, you learn how to integrate iLMS with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2016a-105">Integrera iLMS med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="2016a-105">Integrating iLMS with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="2016a-106">Du kan styra i Azure AD som har åtkomst till iLMS</span><span class="sxs-lookup"><span data-stu-id="2016a-106">You can control in Azure AD who has access to iLMS</span></span>
- <span data-ttu-id="2016a-107">Du kan aktivera användarna att automatiskt hämta loggat in på iLMS (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="2016a-107">You can enable your users to automatically get signed-on to iLMS (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2016a-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="2016a-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="2016a-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2016a-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2016a-110">Krav</span><span class="sxs-lookup"><span data-stu-id="2016a-110">Prerequisites</span></span>

<span data-ttu-id="2016a-111">För att konfigurera Azure AD-integrering med iLMS, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="2016a-111">To configure Azure AD integration with iLMS, you need the following items:</span></span>

- <span data-ttu-id="2016a-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="2016a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2016a-113">En iLMS enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="2016a-113">An iLMS single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="2016a-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="2016a-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="2016a-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="2016a-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2016a-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="2016a-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="2016a-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2016a-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2016a-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="2016a-118">Scenario description</span></span>
<span data-ttu-id="2016a-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="2016a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2016a-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="2016a-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2016a-121">Att lägga till iLMS från galleriet</span><span class="sxs-lookup"><span data-stu-id="2016a-121">Adding iLMS from the gallery</span></span>
2. <span data-ttu-id="2016a-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2016a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-ilms-from-the-gallery"></a><span data-ttu-id="2016a-123">Att lägga till iLMS från galleriet</span><span class="sxs-lookup"><span data-stu-id="2016a-123">Adding iLMS from the gallery</span></span>
<span data-ttu-id="2016a-124">Du måste lägga till iLMS från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av iLMS i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2016a-124">To configure the integration of iLMS into Azure AD, you need to add iLMS from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="2016a-125">**Utför följande steg för att lägga till iLMS från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="2016a-125">**To add iLMS from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="2016a-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="2016a-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="2016a-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="2016a-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="2016a-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="2016a-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="2016a-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2016a-131">To add new application, click **New application** button on the top of the dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="2016a-133">I sökrutan skriver **iLMS**.</span><span class="sxs-lookup"><span data-stu-id="2016a-133">In the search box, type **iLMS**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_search.png)

5. <span data-ttu-id="2016a-135">Välj i resultatpanelen **iLMS**, klicka på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="2016a-135">In the results panel, select **iLMS**, then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2016a-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2016a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="2016a-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med iLMS baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="2016a-138">In this section, you configure and test Azure AD single sign-on with iLMS based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="2016a-139">Azure AD måste du känna till användaren i iLMS motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="2016a-139">For single sign-on to work, Azure AD needs to know what the counterpart user in iLMS is to a user in Azure AD.</span></span> <span data-ttu-id="2016a-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i iLMS upprättas.</span><span class="sxs-lookup"><span data-stu-id="2016a-140">In other words, a link relationship between an Azure AD user and the related user in iLMS needs to be established.</span></span>

<span data-ttu-id="2016a-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i iLMS.</span><span class="sxs-lookup"><span data-stu-id="2016a-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in iLMS.</span></span>

<span data-ttu-id="2016a-142">Om du vill konfigurera och testa Azure AD enkel inloggning med iLMS, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="2016a-142">To configure and test Azure AD single sign-on with iLMS, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="2016a-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="2016a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="2016a-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2016a-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2016a-145">**[Skapa en testanvändare iLMS](#creating-an-ilms-test-user)**  – du har en motsvarighet för Britta Simon i iLMS som är kopplad till Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="2016a-145">**[Creating an iLMS test user](#creating-an-ilms-test-user)** - to have a counterpart of Britta Simon in iLMS that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="2016a-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="2016a-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2016a-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="2016a-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="2016a-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2016a-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="2016a-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt iLMS program.</span><span class="sxs-lookup"><span data-stu-id="2016a-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your iLMS application.</span></span>

<span data-ttu-id="2016a-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med iLMS:**</span><span class="sxs-lookup"><span data-stu-id="2016a-150">**To configure Azure AD single sign-on with iLMS, perform the following steps:**</span></span>

1. <span data-ttu-id="2016a-151">I Azure-portalen på den **iLMS** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="2016a-151">In the Azure portal, on the **iLMS** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="2016a-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="2016a-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_samlbase.png)

3. <span data-ttu-id="2016a-155">På den **iLMS domän och URL: er** avsnittet, utför följande steg om du vill konfigurera programmet i **IDP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="2016a-155">On the **iLMS Domain and URLs** section, perform the following steps if you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_url.png)

    <span data-ttu-id="2016a-157">a.</span><span class="sxs-lookup"><span data-stu-id="2016a-157">a.</span></span> <span data-ttu-id="2016a-158">I den **identifierare** textruta klistra in den **identifierare** värdet du kopiera från **tjänstleverantör** SAML-inställningarna i iLMS administrationsportal.</span><span class="sxs-lookup"><span data-stu-id="2016a-158">In the **Identifier** textbox, paste the **Identifier** value you copy from **Service Provider** section of SAML settings in iLMS admin portal.</span></span>

    <span data-ttu-id="2016a-159">b.</span><span class="sxs-lookup"><span data-stu-id="2016a-159">b.</span></span> <span data-ttu-id="2016a-160">I den **Reply URL** textruta klistra in den **slutpunkt (URL)** värdet du kopiera från **tjänstleverantör** SAML-inställningarna i iLMS administrationsportal med följande mönster`https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`</span><span class="sxs-lookup"><span data-stu-id="2016a-160">In the **Reply URL** textbox, paste the **Endpoint (URL)** value you copy from **Service Provider** section of SAML settings in iLMS admin portal having the following pattern `https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`</span></span>

    >[!Note]
    ><span data-ttu-id="2016a-161">Den här 123456 är en exempel-värdet för identifieraren.</span><span class="sxs-lookup"><span data-stu-id="2016a-161">This '123456' is an example value of identifier.</span></span>

4. <span data-ttu-id="2016a-162">Kontrollera **visa avancerade inställningar för URL: en**, om du vill konfigurera programmet i **SP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="2016a-162">Check **Show advanced URL settings**, if you wish to configure the application in **SP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_url1.png)

    <span data-ttu-id="2016a-164">I den **inloggnings-URL** textruta klistra in den **slutpunkt (URL)** värdet du kopiera från **tjänstleverantör** SAML inställningarna iLMS administrationsportalen som`https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`</span><span class="sxs-lookup"><span data-stu-id="2016a-164">In the **Sign-on URL** textbox, paste the **Endpoint (URL)** value you copy from **Service Provider** section of SAML settings in iLMS admin portal as `https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`</span></span>     

5. <span data-ttu-id="2016a-165">För att aktivera JIT-etablering, förväntar iLMS programmet SAML-intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="2016a-165">To enable JIT provisioning, iLMS application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="2016a-166">Konfigurera följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="2016a-166">Configure the following claims for this application.</span></span> <span data-ttu-id="2016a-167">Du kan hantera värden för attributen från den **användarattribut** avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="2016a-167">You can manage the values of these attributes from the **User Attributes** section on application integration page.</span></span> <span data-ttu-id="2016a-168">Följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="2016a-168">The following screenshot shows an example for this.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-ilms-tutorial/4.png)
    
    <span data-ttu-id="2016a-170">Skapa **avdelning, Region** och **Division** attribut och Lägg till namnet på attributen i iLMS.</span><span class="sxs-lookup"><span data-stu-id="2016a-170">Create **Department, Region** and **Division** attributes and add the name of these attributes in iLMS.</span></span> <span data-ttu-id="2016a-171">Dessa attribut som visas ovan krävs.</span><span class="sxs-lookup"><span data-stu-id="2016a-171">All these attributes shown above are required.</span></span>  

    > [!NOTE] 
    > <span data-ttu-id="2016a-172">Du måste aktivera **skapa Un-recognized användarkonto** i iLMS att mappa dessa attribut.</span><span class="sxs-lookup"><span data-stu-id="2016a-172">You have to enable **Create Un-recognized User Account** in iLMS to map these attributes.</span></span> <span data-ttu-id="2016a-173">Följ instruktionerna [här](http://support.inspiredelearning.com/customer/portal/articles/2204526) att få en uppfattning på attribut-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="2016a-173">Follow the instructions [here](http://support.inspiredelearning.com/customer/portal/articles/2204526) to get an idea on the attributes configuration.</span></span>

6. <span data-ttu-id="2016a-174">I den **användarattribut** avsnitt på den **enkel inloggning** dialogrutan Konfigurera attribut för SAML-token som visas i bilden ovan och utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="2016a-174">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image above and perform the following steps:</span></span>
    
    | <span data-ttu-id="2016a-175">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="2016a-175">Attribute Name</span></span> | <span data-ttu-id="2016a-176">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="2016a-176">Attribute Value</span></span> |
    | ---------------| --------------- |    
    | <span data-ttu-id="2016a-177">Division</span><span class="sxs-lookup"><span data-stu-id="2016a-177">division</span></span> | <span data-ttu-id="2016a-178">User.Department</span><span class="sxs-lookup"><span data-stu-id="2016a-178">user.department</span></span> |
    | <span data-ttu-id="2016a-179">Region</span><span class="sxs-lookup"><span data-stu-id="2016a-179">region</span></span> | <span data-ttu-id="2016a-180">User.state</span><span class="sxs-lookup"><span data-stu-id="2016a-180">user.state</span></span> |
    | <span data-ttu-id="2016a-181">Avdelning</span><span class="sxs-lookup"><span data-stu-id="2016a-181">department</span></span> | <span data-ttu-id="2016a-182">User.jobtitle</span><span class="sxs-lookup"><span data-stu-id="2016a-182">user.jobtitle</span></span> |

    <span data-ttu-id="2016a-183">a.</span><span class="sxs-lookup"><span data-stu-id="2016a-183">a.</span></span> <span data-ttu-id="2016a-184">Klicka på **Lägg till attributet** att öppna den **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2016a-184">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_05.png)
    
    <span data-ttu-id="2016a-187">b.</span><span class="sxs-lookup"><span data-stu-id="2016a-187">b.</span></span> <span data-ttu-id="2016a-188">I den **namn** textruta ange attributets namn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="2016a-188">In the **Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="2016a-189">c.</span><span class="sxs-lookup"><span data-stu-id="2016a-189">c.</span></span> <span data-ttu-id="2016a-190">Från den **värdet** listan, ange det attributvärde som visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="2016a-190">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="2016a-191">d.</span><span class="sxs-lookup"><span data-stu-id="2016a-191">d.</span></span> <span data-ttu-id="2016a-192">Klicka på **Ok**</span><span class="sxs-lookup"><span data-stu-id="2016a-192">Click **Ok**</span></span>

7. <span data-ttu-id="2016a-193">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara XML-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="2016a-193">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_certificate.png) 

8. <span data-ttu-id="2016a-195">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="2016a-195">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-iLMS-tutorial/tutorial_general_400.png)

9. <span data-ttu-id="2016a-197">I en annan webbläsarfönstret, logga in på ditt **iLMS administrationsportalen** som administratör.</span><span class="sxs-lookup"><span data-stu-id="2016a-197">In a different web browser window, log in to your **iLMS admin portal** as an administrator.</span></span>

10. <span data-ttu-id="2016a-198">Klicka på **SSO:SAML** under **inställningar** att öppna inställningarna för SAML och utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="2016a-198">Click **SSO:SAML** under **Settings** tab to open SAML settings and perform the following steps:</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-ilms-tutorial/1.png) 

    <span data-ttu-id="2016a-200">a.</span><span class="sxs-lookup"><span data-stu-id="2016a-200">a.</span></span> <span data-ttu-id="2016a-201">Expandera den **tjänstleverantör** avsnitt och kopiera den **identifierare** och **slutpunkt (URL)** värde.</span><span class="sxs-lookup"><span data-stu-id="2016a-201">Expand the **Service Provider** section and copy the **Identifier** and **Endpoint (URL)** value.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ilms-tutorial/2.png) 

    <span data-ttu-id="2016a-203">b.</span><span class="sxs-lookup"><span data-stu-id="2016a-203">b.</span></span> <span data-ttu-id="2016a-204">Under **identitetsleverantör** klickar du på **importera Metadata**.</span><span class="sxs-lookup"><span data-stu-id="2016a-204">Under **Identity Provider** section, click **Import Metadata**.</span></span>
    
    <span data-ttu-id="2016a-205">c.</span><span class="sxs-lookup"><span data-stu-id="2016a-205">c.</span></span> <span data-ttu-id="2016a-206">Välj den **Metadata** hämta filen från Azure-portalen från **SAML-signeringscertifikat** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="2016a-206">Select the **Metadata** file downloaded from Azure Portal from **SAML Signing Certificate** section.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_ssoconfig1.png) 

    <span data-ttu-id="2016a-208">d.</span><span class="sxs-lookup"><span data-stu-id="2016a-208">d.</span></span> <span data-ttu-id="2016a-209">Om du vill aktivera JIT etablering för att skapa iLMS datorkonton för un-känna igen användare, följa nedanstående steg:</span><span class="sxs-lookup"><span data-stu-id="2016a-209">If you want to enable JIT provisioning to create iLMS accounts for un-recognize users, follow below steps:</span></span>
        
       - <span data-ttu-id="2016a-210">Kontrollera **skapa icke godkänd användarkonto**.</span><span class="sxs-lookup"><span data-stu-id="2016a-210">Check **Create Un-recognized User Account**.</span></span>
       
       ![Konfigurera enkel inloggning](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_ssoconfig2.png)

       -  <span data-ttu-id="2016a-212">Mappa attribut i Azure AD med attribut i iLMS.</span><span class="sxs-lookup"><span data-stu-id="2016a-212">Map the attributes in Azure AD with the attributes in iLMS.</span></span> <span data-ttu-id="2016a-213">Ange namnet på attribut eller standardvärdet i attributkolumnen.</span><span class="sxs-lookup"><span data-stu-id="2016a-213">In the attribute column, specify the attributes name or the default value.</span></span>

    <span data-ttu-id="2016a-214">e.</span><span class="sxs-lookup"><span data-stu-id="2016a-214">e.</span></span> <span data-ttu-id="2016a-215">Gå till **affärsregler** fliken och utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="2016a-215">Go to **Business Rules** tab and perform the following steps:</span></span> 
        
       ![Konfigurera enkel inloggning](./media/active-directory-saas-ilms-tutorial/5.png)

       - <span data-ttu-id="2016a-217">Kontrollera **skapa Un-recognized regioner, avdelningar och avdelningar** att skapa regioner, avdelningar och avdelningar som inte redan finns vid tidpunkten för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="2016a-217">Check **Create Un-recognized Regions, Divisions and Departments** to create Regions, Divisions, and Departments that do not already exist at the time of Single Sign-on.</span></span>
        
       - <span data-ttu-id="2016a-218">Kontrollera **uppdatering användarens profil under inloggning** att ange om användarens profil uppdateras med varje enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="2016a-218">Check **Update User Profile During Sign-in** to specify whether the user’s profile is updated with each Single Sign-on.</span></span> 
        
       - <span data-ttu-id="2016a-219">Om den **”uppdatering tomma värden för ej obligatoriska fält i användarprofil”** alternativet är markerat, valfri profil fält som är tom vid inloggning kommer även att användarprofilen iLMS som innehåller tomma värden för fälten.</span><span class="sxs-lookup"><span data-stu-id="2016a-219">If the **“Update Blank Values for Non Mandatory Fields in User Profile”** option is checked, optional profile fields that are blank upon sign in will also cause the user’s iLMS profile to contain blank values for those fields.</span></span>
        
       - <span data-ttu-id="2016a-220">Kontrollera **skicka fel e-postmeddelande** och ange e-postadress för användaren där du vill ta emot e-postaviseringen fel.</span><span class="sxs-lookup"><span data-stu-id="2016a-220">Check **Send Error Notification Email** and enter the email of the user where you want to receive the error notification email.</span></span>

11. <span data-ttu-id="2016a-221">Klicka på **spara** för att spara inställningarna.</span><span class="sxs-lookup"><span data-stu-id="2016a-221">Click **Save** button to save the settings.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ilms-tutorial/save.png)

> [!TIP]
> <span data-ttu-id="2016a-223">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="2016a-223">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="2016a-224">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="2016a-224">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="2016a-225">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="2016a-225">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
    
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2016a-226">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="2016a-226">Creating an Azure AD test user</span></span>
<span data-ttu-id="2016a-227">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2016a-227">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="2016a-229">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="2016a-229">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="2016a-230">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="2016a-230">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ilms-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="2016a-232">Gå till **användare och grupper** och på **alla användare** att visa en lista över användare.</span><span class="sxs-lookup"><span data-stu-id="2016a-232">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ilms-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2016a-234">Klicka på överst i dialogrutan **Lägg till** att öppna den **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2016a-234">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ilms-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2016a-236">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="2016a-236">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ilms-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="2016a-238">a.</span><span class="sxs-lookup"><span data-stu-id="2016a-238">a.</span></span> <span data-ttu-id="2016a-239">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2016a-239">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2016a-240">b.</span><span class="sxs-lookup"><span data-stu-id="2016a-240">b.</span></span> <span data-ttu-id="2016a-241">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="2016a-241">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="2016a-242">c.</span><span class="sxs-lookup"><span data-stu-id="2016a-242">c.</span></span> <span data-ttu-id="2016a-243">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="2016a-243">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="2016a-244">d.</span><span class="sxs-lookup"><span data-stu-id="2016a-244">d.</span></span> <span data-ttu-id="2016a-245">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="2016a-245">Click **Create**.</span></span>
 
### <a name="creating-an-ilms-test-user"></a><span data-ttu-id="2016a-246">Skapa en testanvändare iLMS</span><span class="sxs-lookup"><span data-stu-id="2016a-246">Creating an iLMS test user</span></span>

<span data-ttu-id="2016a-247">Programmet stöder bara i tid användaretablering och authentication-användare skapas automatiskt i programmet.</span><span class="sxs-lookup"><span data-stu-id="2016a-247">Application supports Just in time user provisioning and after authentication users are created in the application automatically.</span></span> <span data-ttu-id="2016a-248">JIT fungerar om du har klickat på den **skapa Un-recognized användarkonto** kryssrutan under SAML Konfigurationsinställningen på administrationsportalen för iLMS.</span><span class="sxs-lookup"><span data-stu-id="2016a-248">JIT will work, if you have clicked the **Create Un-recognized User Account** checkbox during SAML configuration setting at iLMS admin portal.</span></span>

<span data-ttu-id="2016a-249">Följ nedanstående steg om du behöver skapa en användare manuellt:</span><span class="sxs-lookup"><span data-stu-id="2016a-249">If you need to create an user manually, then follow below steps :</span></span>

1. <span data-ttu-id="2016a-250">Logga in på webbplatsen iLMS företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="2016a-250">Log in to your iLMS company site as an administrator.</span></span>

2. <span data-ttu-id="2016a-251">Klicka på **”registrera användare”** under **användare** att öppna **registrera användaren** sidan.</span><span class="sxs-lookup"><span data-stu-id="2016a-251">Click **“Register User”** under **Users** tab to open **Register User** page.</span></span> 
   
   ![Lägga till medarbetare](./media/active-directory-saas-ilms-tutorial/3.png)

3. <span data-ttu-id="2016a-253">På den **”registrera användare”** utför följande steg.</span><span class="sxs-lookup"><span data-stu-id="2016a-253">On the **“Register User”** page, perform the following steps.</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-ilms-tutorial/create_testuser_add.png)

    <span data-ttu-id="2016a-255">a.</span><span class="sxs-lookup"><span data-stu-id="2016a-255">a.</span></span> <span data-ttu-id="2016a-256">I den **Förnamn** textruta första typnamn Britta.</span><span class="sxs-lookup"><span data-stu-id="2016a-256">In the **First Name** textbox, type the first name Britta.</span></span>
   
    <span data-ttu-id="2016a-257">b.</span><span class="sxs-lookup"><span data-stu-id="2016a-257">b.</span></span> <span data-ttu-id="2016a-258">I den **efternamn** textruta anger efternamn Simon.</span><span class="sxs-lookup"><span data-stu-id="2016a-258">In the **Last Name** textbox, type the last name Simon.</span></span>

    <span data-ttu-id="2016a-259">c.</span><span class="sxs-lookup"><span data-stu-id="2016a-259">c.</span></span> <span data-ttu-id="2016a-260">I den **e-post-ID** textruta skriver Britta Simon konto e-postadress.</span><span class="sxs-lookup"><span data-stu-id="2016a-260">In the **Email ID** textbox, type the email address of Britta Simon account.</span></span>

    <span data-ttu-id="2016a-261">d.</span><span class="sxs-lookup"><span data-stu-id="2016a-261">d.</span></span> <span data-ttu-id="2016a-262">I den **Region** listrutan, Välj värdet för region.</span><span class="sxs-lookup"><span data-stu-id="2016a-262">In the **Region** dropdown, select the value for region.</span></span>

    <span data-ttu-id="2016a-263">e.</span><span class="sxs-lookup"><span data-stu-id="2016a-263">e.</span></span> <span data-ttu-id="2016a-264">I den **Division** listrutan, Välj värdet för delning.</span><span class="sxs-lookup"><span data-stu-id="2016a-264">In the **Division** dropdown, select the value for division.</span></span>

    <span data-ttu-id="2016a-265">f.</span><span class="sxs-lookup"><span data-stu-id="2016a-265">f.</span></span> <span data-ttu-id="2016a-266">I den **avdelning** listrutan, Välj värdet för avdelning.</span><span class="sxs-lookup"><span data-stu-id="2016a-266">In the **Department** dropdown, select the value for department.</span></span>

    <span data-ttu-id="2016a-267">g.</span><span class="sxs-lookup"><span data-stu-id="2016a-267">g.</span></span> <span data-ttu-id="2016a-268">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="2016a-268">Click **Save**.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="2016a-269">Du kan skicka e-post för registrering till användare genom att välja **skicka e-post från registrering** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="2016a-269">You can send registration mail to user by selecting **Send Registration Mail** checkbox.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="2016a-270">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="2016a-270">Assigning the Azure AD test user</span></span>

<span data-ttu-id="2016a-271">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning med sin åtkomst beviljas till iLMS.</span><span class="sxs-lookup"><span data-stu-id="2016a-271">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to iLMS.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="2016a-273">**Om du vill tilldela iLMS Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="2016a-273">**To assign Britta Simon to iLMS, perform the following steps:**</span></span>

1. <span data-ttu-id="2016a-274">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="2016a-274">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="2016a-276">Välj i listan med program **iLMS**.</span><span class="sxs-lookup"><span data-stu-id="2016a-276">In the applications list, select **iLMS**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_app.png) 

3. <span data-ttu-id="2016a-278">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="2016a-278">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="2016a-280">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="2016a-280">Click **Add** button.</span></span> <span data-ttu-id="2016a-281">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2016a-281">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="2016a-283">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="2016a-283">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="2016a-284">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2016a-284">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2016a-285">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2016a-285">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="2016a-286">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2016a-286">Testing single sign-on</span></span>

<span data-ttu-id="2016a-287">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="2016a-287">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="2016a-288">När du klickar på panelen iLMS på åtkomstpanelen du bör få automatiskt loggat in på ditt iLMS program.</span><span class="sxs-lookup"><span data-stu-id="2016a-288">When you click the iLMS tile in the Access Panel, you should get automatically signed-on to your iLMS application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2016a-289">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="2016a-289">Additional resources</span></span>

* [<span data-ttu-id="2016a-290">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2016a-290">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2016a-291">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="2016a-291">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_203.png

