---
title: "Självstudier: Azure Active Directory-integrering med ServiceChannel | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och ServiceChannel."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c3546eab-96b5-489b-a309-b895eb428053
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/3/2017
ms.author: jeedes
ms.openlocfilehash: 7e1dad18ff0ae9a9102b789b2cb32e7b96ed3d38
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-servicechannel"></a><span data-ttu-id="7a696-103">Självstudier: Azure Active Directory-integrering med ServiceChannel</span><span class="sxs-lookup"><span data-stu-id="7a696-103">Tutorial: Azure Active Directory integration with ServiceChannel</span></span>

<span data-ttu-id="7a696-104">I kursen får lära du att integrera ServiceChannel med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="7a696-104">In this tutorial, you learn how to integrate ServiceChannel with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7a696-105">Integrera ServiceChannel med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="7a696-105">Integrating ServiceChannel with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="7a696-106">Du kan styra i Azure AD som har åtkomst till ServiceChannel</span><span class="sxs-lookup"><span data-stu-id="7a696-106">You can control in Azure AD who has access to ServiceChannel</span></span>
- <span data-ttu-id="7a696-107">Du kan aktivera användarna att automatiskt hämta loggat in på ServiceChannel (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="7a696-107">You can enable your users to automatically get signed-on to ServiceChannel (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7a696-108">Du kan hantera dina konton i en central plats - till Azure-hanteringsportalen</span><span class="sxs-lookup"><span data-stu-id="7a696-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="7a696-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7a696-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7a696-110">Krav</span><span class="sxs-lookup"><span data-stu-id="7a696-110">Prerequisites</span></span>

<span data-ttu-id="7a696-111">För att konfigurera Azure AD-integrering med ServiceChannel, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="7a696-111">To configure Azure AD integration with ServiceChannel, you need the following items:</span></span>

- <span data-ttu-id="7a696-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="7a696-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7a696-113">En ServiceChannel enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="7a696-113">A ServiceChannel single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7a696-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="7a696-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7a696-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="7a696-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7a696-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="7a696-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="7a696-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7a696-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7a696-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="7a696-118">Scenario description</span></span>
<span data-ttu-id="7a696-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="7a696-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7a696-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="7a696-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7a696-121">Att lägga till ServiceChannel från galleriet</span><span class="sxs-lookup"><span data-stu-id="7a696-121">Adding ServiceChannel from the gallery</span></span>
2. <span data-ttu-id="7a696-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7a696-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-servicechannel-from-the-gallery"></a><span data-ttu-id="7a696-123">Att lägga till ServiceChannel från galleriet</span><span class="sxs-lookup"><span data-stu-id="7a696-123">Adding ServiceChannel from the gallery</span></span>
<span data-ttu-id="7a696-124">Du måste lägga till ServiceChannel från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av ServiceChannel i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7a696-124">To configure the integration of ServiceChannel into Azure AD, you need to add ServiceChannel from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="7a696-125">**Utför följande steg för att lägga till ServiceChannel från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="7a696-125">**To add ServiceChannel from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="7a696-126">I den  **[Azure-hanteringsportalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="7a696-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7a696-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="7a696-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="7a696-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="7a696-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="7a696-131">Klicka på **Lägg till** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7a696-131">Click **Add** button on the top of the dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="7a696-133">I sökrutan skriver **ServiceChannel**.</span><span class="sxs-lookup"><span data-stu-id="7a696-133">In the search box, type **ServiceChannel**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_000.png)

5. <span data-ttu-id="7a696-135">Välj i resultatpanelen **ServiceChannel**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="7a696-135">In the results panel, select **ServiceChannel**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_2.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="7a696-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7a696-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="7a696-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med ServiceChannel baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="7a696-138">In this section, you configure and test Azure AD single sign-on with ServiceChannel based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="7a696-139">Azure AD måste du känna till användaren i ServiceChannel motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="7a696-139">For single sign-on to work, Azure AD needs to know what the counterpart user in ServiceChannel is to a user in Azure AD.</span></span> <span data-ttu-id="7a696-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i ServiceChannel upprättas.</span><span class="sxs-lookup"><span data-stu-id="7a696-140">In other words, a link relationship between an Azure AD user and the related user in ServiceChannel needs to be established.</span></span>

<span data-ttu-id="7a696-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i ServiceChannel.</span><span class="sxs-lookup"><span data-stu-id="7a696-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in ServiceChannel.</span></span>

<span data-ttu-id="7a696-142">Om du vill konfigurera och testa Azure AD enkel inloggning med ServiceChannel, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="7a696-142">To configure and test Azure AD single sign-on with ServiceChannel, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="7a696-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="7a696-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="7a696-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7a696-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7a696-145">**[Skapa en testanvändare ServiceChannel](#creating-a-servicechannel-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7a696-145">**[Creating a ServiceChannel test user](#creating-a-servicechannel-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="7a696-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="7a696-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7a696-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="7a696-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="7a696-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7a696-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="7a696-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-hanteringsportalen och konfigurera enkel inloggning i ditt ServiceChannel-program.</span><span class="sxs-lookup"><span data-stu-id="7a696-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your ServiceChannel application.</span></span>

<span data-ttu-id="7a696-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med ServiceChannel:**</span><span class="sxs-lookup"><span data-stu-id="7a696-150">**To configure Azure AD single sign-on with ServiceChannel, perform the following steps:**</span></span>

1. <span data-ttu-id="7a696-151">I Azure-hanteringsportalen på den **ServiceChannel** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="7a696-151">In the Azure Management portal, on the **ServiceChannel** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="7a696-153">På den **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** att aktivera enkel inloggning på.</span><span class="sxs-lookup"><span data-stu-id="7a696-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_01.png)

3. <span data-ttu-id="7a696-155">På den **ServiceChannel domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="7a696-155">On the **ServiceChannel Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_urls.png)

    <span data-ttu-id="7a696-157">a.</span><span class="sxs-lookup"><span data-stu-id="7a696-157">a.</span></span> <span data-ttu-id="7a696-158">I den **identifierare** textruta Skriv värdet som:`http://adfs.<domain>.com/adfs/service/trust`</span><span class="sxs-lookup"><span data-stu-id="7a696-158">In the **Identifier** textbox, type the value as: `http://adfs.<domain>.com/adfs/service/trust`</span></span>

    <span data-ttu-id="7a696-159">b.</span><span class="sxs-lookup"><span data-stu-id="7a696-159">b.</span></span> <span data-ttu-id="7a696-160">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://<customer domain>.servicechannel.com/saml/acs`</span><span class="sxs-lookup"><span data-stu-id="7a696-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<customer domain>.servicechannel.com/saml/acs`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="7a696-161">Observera att detta inte är verkliga värden.</span><span class="sxs-lookup"><span data-stu-id="7a696-161">Please note that these are not the real values.</span></span> <span data-ttu-id="7a696-162">Du måste uppdatera dessa värden med de faktiska identifierare och Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="7a696-162">You have to update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="7a696-163">Här rekommenderar vi att du om du vill använda det unika värdet på strängen i identifieraren.</span><span class="sxs-lookup"><span data-stu-id="7a696-163">Here we suggest you to use the unique value of string in the Identifier.</span></span> <span data-ttu-id="7a696-164">Kontakta [ServiceChannel supportteamet](https://servicechannel.zendesk.com/hc/en-us) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="7a696-164">Contact [ServiceChannel support team](https://servicechannel.zendesk.com/hc/en-us) to get these values.</span></span>

4. <span data-ttu-id="7a696-165">Tillämpningsprogrammet ServiceChannel förväntar SAML-intyg i ett specifikt format, vilket kräver att du kan lägga till anpassade attributmappning konfigurationen för SAML-token attribut.</span><span class="sxs-lookup"><span data-stu-id="7a696-165">Your ServiceChannel application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your SAML token attributes configuration.</span></span> <span data-ttu-id="7a696-166">Följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="7a696-166">The following screenshot shows an example for this.</span></span> <span data-ttu-id="7a696-167">**NameIdentifier (användar-ID)** är bara obligatoriskt anspråk och standardvärdet är **user.userprincipalname** men ServiceChannel förväntar detta mappas med **user.mail**.</span><span class="sxs-lookup"><span data-stu-id="7a696-167">**NameIdentifier(User Identifier)** is the only mandatory claim and the default value is **user.userprincipalname** but ServiceChannel expects this to be mapped with **user.mail**.</span></span> <span data-ttu-id="7a696-168">Om du planerar att aktivera bara In Time användaretablering, bör du till följande anspråk som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="7a696-168">If you are planning to enable Just In Time user provisioning, then you should add the following claims as shown below.</span></span> <span data-ttu-id="7a696-169">**Rollen** anspråk måste mappas till **user.assignedroles** som innehåller rollen för användaren.</span><span class="sxs-lookup"><span data-stu-id="7a696-169">**Role** claim needs to be mapped to **user.assignedroles** which contains the role of the user.</span></span>  

    <span data-ttu-id="7a696-170">Du kan se ServiceChannel guide [här](https://servicechannel.zendesk.com/hc/en-us/articles/217514326-Azure-AD-Configuration-Example) för mer information om anspråk.</span><span class="sxs-lookup"><span data-stu-id="7a696-170">You can refer ServiceChannel guide [here](https://servicechannel.zendesk.com/hc/en-us/articles/217514326-Azure-AD-Configuration-Example) for more guidance on claims.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-servicechannel-tutorial/tutorial_servicechannel_attribute.png)

    > [!NOTE] 
    > <span data-ttu-id="7a696-172">Klicka på [här](http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/) veta hur du konfigurerar **rollen** i Azure AD</span><span class="sxs-lookup"><span data-stu-id="7a696-172">Please click [here](http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/) to know how to configure **Role** in Azure AD</span></span>

5. <span data-ttu-id="7a696-173">I **användarattribut** klickar du på **visa och redigera andra användarattribut** och ange attribut.</span><span class="sxs-lookup"><span data-stu-id="7a696-173">In **User Attributes** section, click **View and edit all other user attributes** and set the attributes.</span></span>

    | <span data-ttu-id="7a696-174">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="7a696-174">Attribute Name</span></span> | <span data-ttu-id="7a696-175">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="7a696-175">Attribute Value</span></span> |
    | --- | --- |    
    | <span data-ttu-id="7a696-176">Roll</span><span class="sxs-lookup"><span data-stu-id="7a696-176">Role</span></span>| <span data-ttu-id="7a696-177">User.assignedroles</span><span class="sxs-lookup"><span data-stu-id="7a696-177">user.assignedroles</span></span> |

    <span data-ttu-id="7a696-178">a.</span><span class="sxs-lookup"><span data-stu-id="7a696-178">a.</span></span> <span data-ttu-id="7a696-179">Klicka på **Lägg till attributet** att öppna den **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7a696-179">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-servicechannel-tutorial/tutorial_servicechannel_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-servicechannel-tutorial/tutorial_servicechannel_05.png)
    
    <span data-ttu-id="7a696-182">b.</span><span class="sxs-lookup"><span data-stu-id="7a696-182">b.</span></span> <span data-ttu-id="7a696-183">I den **namn** textruta ange attributets namn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="7a696-183">In the **Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="7a696-184">c.</span><span class="sxs-lookup"><span data-stu-id="7a696-184">c.</span></span> <span data-ttu-id="7a696-185">Från den **värdet** listan, ange det attributvärde som visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="7a696-185">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="7a696-186">d.</span><span class="sxs-lookup"><span data-stu-id="7a696-186">d.</span></span> <span data-ttu-id="7a696-187">Klicka på **Ok**</span><span class="sxs-lookup"><span data-stu-id="7a696-187">Click **Ok**</span></span>
    
6. <span data-ttu-id="7a696-188">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="7a696-188">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_05.png) 

7. <span data-ttu-id="7a696-190">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="7a696-190">Click **Save**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-servicechannel-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="7a696-192">På den **ServiceChannel Configuration** klickar du på **konfigurera ServiceChannel** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="7a696-192">On the **ServiceChannel Configuration** section, click **Configure ServiceChannel** to open **Configure sign-on** window.</span></span> <span data-ttu-id="7a696-193">Observera den **SAML Enitity ID** från den **Snabbreferens** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7a696-193">Please note the **SAML Enitity ID** from the **Quick Reference** section.</span></span>

9. <span data-ttu-id="7a696-194">Konfigurera enkel inloggning på **ServiceChannel** sida, måste du skicka den hämtade **certifikat (Base64)** och **SAML enhets-ID** till [ServiceChannel supportteamet](https://servicechannel.zendesk.com/hc/en-us).</span><span class="sxs-lookup"><span data-stu-id="7a696-194">To configure single sign-on on **ServiceChannel** side, you need to send the downloaded **certificate (Base64)** and **SAML Entity ID** to [ServiceChannel support team](https://servicechannel.zendesk.com/hc/en-us).</span></span> <span data-ttu-id="7a696-195">De ska ange detta för att få SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="7a696-195">They will set this up in order to have the SAML SSO connection set properly on both sides.</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="7a696-196">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="7a696-196">Creating an Azure AD test user</span></span>
<span data-ttu-id="7a696-197">Syftet med det här avsnittet är att skapa en testanvändare i Azure Management portal kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7a696-197">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="7a696-199">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="7a696-199">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="7a696-200">I den **Azure-hanteringsportalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="7a696-200">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-servicechannel-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7a696-202">Gå till **användare och grupper** och på **alla användare** att visa en lista över användare.</span><span class="sxs-lookup"><span data-stu-id="7a696-202">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-servicechannel-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7a696-204">Klicka på överst i dialogrutan **Lägg till** att öppna den **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7a696-204">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-servicechannel-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7a696-206">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="7a696-206">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-servicechannel-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7a696-208">a.</span><span class="sxs-lookup"><span data-stu-id="7a696-208">a.</span></span> <span data-ttu-id="7a696-209">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7a696-209">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7a696-210">b.</span><span class="sxs-lookup"><span data-stu-id="7a696-210">b.</span></span> <span data-ttu-id="7a696-211">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="7a696-211">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="7a696-212">c.</span><span class="sxs-lookup"><span data-stu-id="7a696-212">c.</span></span> <span data-ttu-id="7a696-213">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="7a696-213">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="7a696-214">d.</span><span class="sxs-lookup"><span data-stu-id="7a696-214">d.</span></span> <span data-ttu-id="7a696-215">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="7a696-215">Click **Create**.</span></span> 

### <a name="creating-a-servicechannel-test-user"></a><span data-ttu-id="7a696-216">Skapa en testanvändare ServiceChannel</span><span class="sxs-lookup"><span data-stu-id="7a696-216">Creating a ServiceChannel test user</span></span>

<span data-ttu-id="7a696-217">Programmet stöder bara i tid användaretablering och authentication-användare kommer automatiskt att skapas i programmet.</span><span class="sxs-lookup"><span data-stu-id="7a696-217">Application supports Just in time user provisioning and after authentication users will be created in the application automatically.</span></span> <span data-ttu-id="7a696-218">För fullständig användaretablering, kontakta [ServiceChannel-teamet](https://servicechannel.zendesk.com/hc/en-us)</span><span class="sxs-lookup"><span data-stu-id="7a696-218">For full user provisioning, please contact [ServiceChannel support team](https://servicechannel.zendesk.com/hc/en-us)</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="7a696-219">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="7a696-219">Assigning the Azure AD test user</span></span>

<span data-ttu-id="7a696-220">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja sin åtkomst till ServiceChannel.</span><span class="sxs-lookup"><span data-stu-id="7a696-220">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to ServiceChannel.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="7a696-222">**Om du vill tilldela ServiceChannel Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="7a696-222">**To assign Britta Simon to ServiceChannel, perform the following steps:**</span></span>

1. <span data-ttu-id="7a696-223">Öppna vyn program i Azure-hanteringsportalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="7a696-223">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="7a696-225">Välj i listan med program **ServiceChannel**.</span><span class="sxs-lookup"><span data-stu-id="7a696-225">In the applications list, select **ServiceChannel**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_app01.png) 

3. <span data-ttu-id="7a696-227">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="7a696-227">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="7a696-229">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="7a696-229">Click **Add** button.</span></span> <span data-ttu-id="7a696-230">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7a696-230">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="7a696-232">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="7a696-232">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="7a696-233">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7a696-233">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7a696-234">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7a696-234">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="7a696-235">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7a696-235">Testing single sign-on</span></span>

<span data-ttu-id="7a696-236">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="7a696-236">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="7a696-237">När du klickar på panelen ServiceChannel på åtkomstpanelen du bör få automatiskt loggat in på ditt ServiceChannel-program.</span><span class="sxs-lookup"><span data-stu-id="7a696-237">When you click the ServiceChannel tile in the Access Panel, you should get automatically signed-on to your ServiceChannel application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7a696-238">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="7a696-238">Additional resources</span></span>

* [<span data-ttu-id="7a696-239">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7a696-239">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7a696-240">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7a696-240">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_203.png