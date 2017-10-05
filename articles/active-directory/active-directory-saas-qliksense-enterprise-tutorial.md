---
title: "Självstudier: Azure Active Directory-integrering med Qlik mening Enterprise | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Qlik mening Enterprise."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8c27e340-2b25-47b6-bf1f-438be4c14f93
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: jeedes
ms.openlocfilehash: 4964634cd5aaf0dbb98c766f5e12700c4d118750
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-qlik-sense-enterprise"></a><span data-ttu-id="a8fc5-103">Självstudier: Azure Active Directory-integrering med Qlik mening Enterprise</span><span class="sxs-lookup"><span data-stu-id="a8fc5-103">Tutorial: Azure Active Directory integration with Qlik Sense Enterprise</span></span>

<span data-ttu-id="a8fc5-104">I kursen får lära du att integrera Qlik mening Enterprise med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="a8fc5-104">In this tutorial, you learn how to integrate Qlik Sense Enterprise with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a8fc5-105">Integrera Qlik mening Enterprise med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="a8fc5-105">Integrating Qlik Sense Enterprise with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="a8fc5-106">Du kan styra i Azure AD som har åtkomst till Qlik mening Enterprise.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-106">You can control in Azure AD who has access to Qlik Sense Enterprise.</span></span>
- <span data-ttu-id="a8fc5-107">Du kan aktivera användarna att automatiskt hämta loggat in på Qlik mening Enterprise (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-107">You can enable your users to automatically get signed-on to Qlik Sense Enterprise (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="a8fc5-108">Du kan hantera dina konton i en central plats - Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="a8fc5-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a8fc5-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a8fc5-110">Krav</span><span class="sxs-lookup"><span data-stu-id="a8fc5-110">Prerequisites</span></span>

<span data-ttu-id="a8fc5-111">För att konfigurera Azure AD-integrering med Qlik mening Enterprise, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="a8fc5-111">To configure Azure AD integration with Qlik Sense Enterprise, you need the following items:</span></span>

- <span data-ttu-id="a8fc5-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="a8fc5-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a8fc5-113">En Qlik mening Enterprise enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="a8fc5-113">A Qlik Sense Enterprise single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a8fc5-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a8fc5-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="a8fc5-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a8fc5-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a8fc5-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a8fc5-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a8fc5-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="a8fc5-118">Scenario description</span></span>
<span data-ttu-id="a8fc5-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a8fc5-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="a8fc5-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a8fc5-121">Att lägga till Qlik mening Enterprise från galleriet</span><span class="sxs-lookup"><span data-stu-id="a8fc5-121">Adding Qlik Sense Enterprise from the gallery</span></span>
2. <span data-ttu-id="a8fc5-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a8fc5-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-qlik-sense-enterprise-from-the-gallery"></a><span data-ttu-id="a8fc5-123">Att lägga till Qlik mening Enterprise från galleriet</span><span class="sxs-lookup"><span data-stu-id="a8fc5-123">Adding Qlik Sense Enterprise from the gallery</span></span>
<span data-ttu-id="a8fc5-124">Du måste lägga till Qlik mening Enterprise från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Qlik mening Enterprise i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-124">To configure the integration of Qlik Sense Enterprise into Azure AD, you need to add Qlik Sense Enterprise from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="a8fc5-125">**Utför följande steg för att lägga till Qlik mening Enterprise från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="a8fc5-125">**To add Qlik Sense Enterprise from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="a8fc5-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Azure Active Directory-knappen][1]

2. <span data-ttu-id="a8fc5-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="a8fc5-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-129">Then go to **All applications**.</span></span>

    ![Bladet Enterprise program][2]
    
3. <span data-ttu-id="a8fc5-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Knappen Nytt program][3]

4. <span data-ttu-id="a8fc5-133">I sökrutan skriver **Qlik mening Enterprise**väljer **Qlik mening Enterprise** resultatet-panelen klickar **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-133">In the search box, type **Qlik Sense Enterprise**, select **Qlik Sense Enterprise** from result panel then click **Add** button to add the application.</span></span>

    ![Qlik mening i resultatlistan företag](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="a8fc5-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a8fc5-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="a8fc5-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Qlik mening Enterprise baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-136">In this section, you configure and test Azure AD single sign-on with Qlik Sense Enterprise based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a8fc5-137">Azure AD måste du känna till motsvarande användaren i Qlik mening företag till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Qlik Sense Enterprise is to a user in Azure AD.</span></span> <span data-ttu-id="a8fc5-138">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Qlik mening Enterprise upprättas.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-138">In other words, a link relationship between an Azure AD user and the related user in Qlik Sense Enterprise needs to be established.</span></span>

<span data-ttu-id="a8fc5-139">I Qlik mening Enterprise, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-139">In Qlik Sense Enterprise, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="a8fc5-140">Om du vill konfigurera och testa Azure AD enkel inloggning med Qlik mening Enterprise, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="a8fc5-140">To configure and test Azure AD single sign-on with Qlik Sense Enterprise, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="a8fc5-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="a8fc5-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a8fc5-143">**[Skapa en testanvändare Qlik mening Enterprise](#create-a-qlik-sense-enterprise-test-user)**  – du har en motsvarighet för Britta Simon i Qlik mening företag som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-143">**[Create a Qlik Sense Enterprise test user](#create-a-qlik-sense-enterprise-test-user)** - to have a counterpart of Britta Simon in Qlik Sense Enterprise that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="a8fc5-144">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a8fc5-145">**[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="a8fc5-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a8fc5-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="a8fc5-147">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i Qlik mening Enterprise-programmet.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Qlik Sense Enterprise application.</span></span>

<span data-ttu-id="a8fc5-148">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Qlik mening Enterprise:**</span><span class="sxs-lookup"><span data-stu-id="a8fc5-148">**To configure Azure AD single sign-on with Qlik Sense Enterprise, perform the following steps:**</span></span>

1. <span data-ttu-id="a8fc5-149">I Azure-portalen på den **Qlik mening Enterprise** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-149">In the Azure portal, on the **Qlik Sense Enterprise** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="a8fc5-151">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_samlbase.png)

3. <span data-ttu-id="a8fc5-153">På den **Qlik mening företagsdomänen och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="a8fc5-153">On the **Qlik Sense Enterprise Domain and URLs** section, perform the following steps:</span></span>

    ![URL: er och Qlik mening företagsdomänen enkel inloggning information](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_url.png)

    <span data-ttu-id="a8fc5-155">a.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-155">a.</span></span> <span data-ttu-id="a8fc5-156">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<Qlik Sense Fully Qualifed Hostname>:443//samlauthn/`</span><span class="sxs-lookup"><span data-stu-id="a8fc5-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<Qlik Sense Fully Qualifed Hostname>:443//samlauthn/`</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="a8fc5-157">Observera avslutande snedstreck i slutet av den här URI.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-157">Note the trailing slash at the end of this URI.</span></span> <span data-ttu-id="a8fc5-158">Det krävs.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-158">It is required.</span></span>
    
    <span data-ttu-id="a8fc5-159">b.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-159">b.</span></span> <span data-ttu-id="a8fc5-160">I den **identifierare** textruta Skriv en URL med följande mönster:</span><span class="sxs-lookup"><span data-stu-id="a8fc5-160">In the **Identifier** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `https://<Qlik Sense Fully Qualifed Hostname>.qlikpoc.com`|
    | `https://<Qlik Sense Fully Qualifed Hostname>.qliksense.com`|

    > [!NOTE] 
    > <span data-ttu-id="a8fc5-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-161">These values are not real.</span></span> <span data-ttu-id="a8fc5-162">Uppdatera dessa värden med de faktiska inloggnings-URL och identifierare, som beskrivs senare i den här kursen eller kontakta [Qlik mening Enterprise-klienten supportteamet](https://www.qlik.com/us/services/support) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-162">Update these values with the actual Sign-On URL and Identifier, Which are explained later in this tutorial or contact [Qlik Sense Enterprise Client support team](https://www.qlik.com/us/services/support) to get these values.</span></span> 

4. <span data-ttu-id="a8fc5-163">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-163">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Länken hämta certifikatet](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_certificate.png) 

5. <span data-ttu-id="a8fc5-165">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-165">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="a8fc5-167">Förbereda Federation Metadata XML-filen så att du kan ladda upp som Qlik mening servern.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-167">Prepare the Federation Metadata XML file so that you can upload that to Qlik Sense server.</span></span>
   
    > [!NOTE]
    > <span data-ttu-id="a8fc5-168">Innan du laddar upp IdP metadata till servern Qlik mening filen måste redigeras för att ta bort information för att säkerställa att programmet fungerar mellan Azure AD och Qlik mening server.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-168">Before uploading the IdP metadata to the Qlik Sense server, the file needs to be edited to remove information to ensure proper operation between Azure AD and Qlik Sense server.</span></span>
    
    ![QlikSense][qs24]
   
    <span data-ttu-id="a8fc5-170">a.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-170">a.</span></span> <span data-ttu-id="a8fc5-171">Öppna filen FederationMetaData.xml som du har hämtat från Azure-portalen i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-171">Open the FederationMetaData.xml file, which you have downloaded from Azure portal in a text editor.</span></span>
   
    <span data-ttu-id="a8fc5-172">b.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-172">b.</span></span> <span data-ttu-id="a8fc5-173">Sök efter värdet **RoleDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-173">Search for the value **RoleDescriptor**.</span></span>  <span data-ttu-id="a8fc5-174">Det finns fyra poster (två par inledande och avslutande elementtaggar).</span><span class="sxs-lookup"><span data-stu-id="a8fc5-174">There are four entries (two pairs of opening and closing element tags).</span></span>
   
    <span data-ttu-id="a8fc5-175">c.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-175">c.</span></span> <span data-ttu-id="a8fc5-176">Ta bort RoleDescriptor-taggar och all information mellan från filen.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-176">Delete the RoleDescriptor tags and all information in between from the file.</span></span>
   
    <span data-ttu-id="a8fc5-177">d.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-177">d.</span></span> <span data-ttu-id="a8fc5-178">Spara filen och spara den i närheten för användning senare i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-178">Save the file and keep it nearby for use later in this document.</span></span>

7. <span data-ttu-id="a8fc5-179">Navigera till Qlik meningsfullt Qlik Management Console (QMC) som en användare som kan skapa virtuella proxykonfigurationer.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-179">Navigate to the Qlik Sense Qlik Management Console (QMC) as a user who can create virtual proxy configurations.</span></span>

8. <span data-ttu-id="a8fc5-180">I QMC, klickar du på den **virtuella proxyservrar** menyalternativet.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-180">In the QMC, click on the **Virtual Proxies** menu item.</span></span>
   
    ![QlikSense][qs6] 

9. <span data-ttu-id="a8fc5-182">Längst ned på skärmen klickar du på den **Skapa nytt** knappen.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-182">At the bottom of the screen, click the **Create new** button.</span></span>
   
    ![QlikSense][qs7]

10. <span data-ttu-id="a8fc5-184">Den virtuella proxy redigera visas.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-184">The Virtual proxy edit screen appears.</span></span>  <span data-ttu-id="a8fc5-185">Är en meny för att göra konfigurationsalternativ visas till höger på skärmen.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-185">On the right side of the screen is a menu for making configuration options visible.</span></span>
   
    ![QlikSense][qs9]

11. <span data-ttu-id="a8fc5-187">Ange identitetsinformationen för den virtuella Azure-proxykonfigurationen med menyalternativet identifiering kontrolleras.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-187">With the Identification menu option checked, enter the identifying information for the Azure virtual proxy configuration.</span></span>
    
    ![QlikSense][qs8]  
    
    <span data-ttu-id="a8fc5-189">a.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-189">a.</span></span> <span data-ttu-id="a8fc5-190">Den **beskrivning** fältet är ett eget namn för den virtuella proxykonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-190">The **Description** field is a friendly name for the virtual proxy configuration.</span></span>  <span data-ttu-id="a8fc5-191">Ange ett värde för en beskrivning.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-191">Enter a value for a description.</span></span>
    
    <span data-ttu-id="a8fc5-192">b.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-192">b.</span></span> <span data-ttu-id="a8fc5-193">Den **prefixet** identifierar fältet virtuella proxy-slutpunkten för att ansluta till Qlik mening med Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-193">The **Prefix** field identifies the virtual proxy endpoint for connecting to Qlik Sense with Azure AD Single Sign-On.</span></span>  <span data-ttu-id="a8fc5-194">Ange ett unikt prefix-namn för den här virtuella proxy.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-194">Enter a unique prefix name for this virtual proxy.</span></span>
    
    <span data-ttu-id="a8fc5-195">c.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-195">c.</span></span> <span data-ttu-id="a8fc5-196">**Tidsgräns för inaktivitet (minuter)** är tidsgränsen för anslutningar via den här virtuella proxy.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-196">**Session inactivity timeout (minutes)** is the timeout for connections through this virtual proxy.</span></span>
    
    <span data-ttu-id="a8fc5-197">d.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-197">d.</span></span> <span data-ttu-id="a8fc5-198">Den **sessionsnamnet cookie-huvud** är cookie-namn som lagrar sessions-ID för en användare tar emot efter en lyckad autentisering Qlik mening sessionen.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-198">The **Session cookie header name** is the cookie name storing the session identifier for the Qlik Sense session a user receives after successful authentication.</span></span>  <span data-ttu-id="a8fc5-199">Det här namnet måste vara unikt.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-199">This name must be unique.</span></span>

12. <span data-ttu-id="a8fc5-200">Klicka på menyalternativet autentisering för att göra den synlig.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-200">Click on the Authentication menu option to make it visible.</span></span>  <span data-ttu-id="a8fc5-201">Autentisering visas.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-201">The Authentication screen appears.</span></span>
    
    ![QlikSense][qs10]
    
    <span data-ttu-id="a8fc5-203">a.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-203">a.</span></span> <span data-ttu-id="a8fc5-204">Den **anonym åtkomstläge** nedrullningsbara avgör om anonyma användare kan komma åt Qlik mening genom den virtuella proxyn.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-204">The **Anonymous access mode** drop down determines if anonymous users may access Qlik Sense through the virtual proxy.</span></span>  <span data-ttu-id="a8fc5-205">Standardalternativet är ingen anonym användare.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-205">The default option is No anonymous user.</span></span>
    
    <span data-ttu-id="a8fc5-206">b.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-206">b.</span></span> <span data-ttu-id="a8fc5-207">Den **autentiseringsmetod** nedrullningsbara styr autentiseringsschema virtuella proxy används.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-207">The **Authentication method** drop-down determines the authentication scheme the virtual proxy will use.</span></span>  <span data-ttu-id="a8fc5-208">Välj SAML från den nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-208">Select SAML from the drop-down list.</span></span>  <span data-ttu-id="a8fc5-209">Fler alternativ visas som ett resultat.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-209">More options appear as a result.</span></span>
    
    <span data-ttu-id="a8fc5-210">c.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-210">c.</span></span> <span data-ttu-id="a8fc5-211">I den **SAML URI värdfältet**, ange värdnamnet användare anger för att komma åt Qlik mening via den här virtuella SAML-proxyn.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-211">In the **SAML host URI field**, input the hostname users enter to access Qlik Sense through this SAML virtual proxy.</span></span>  <span data-ttu-id="a8fc5-212">Värdnamnet är URI: n för Qlik mening-servern.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-212">The hostname is the uri of the Qlik Sense server.</span></span>
    
    <span data-ttu-id="a8fc5-213">d.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-213">d.</span></span> <span data-ttu-id="a8fc5-214">I den **SAML enhets-ID**, ange samma värde som angetts för fältet SAML värden URI.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-214">In the **SAML entity ID**, enter the same value entered for the SAML host URI field.</span></span>
    
    <span data-ttu-id="a8fc5-215">e.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-215">e.</span></span> <span data-ttu-id="a8fc5-216">Den **SAML IdP metadata** är filen redigeras tidigare i den **redigera Federation Metadata från Azure AD-konfiguration** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-216">The **SAML IdP metadata** is the file edited earlier in the **Edit Federation Metadata from Azure AD Configuration** section.</span></span>  <span data-ttu-id="a8fc5-217">**Innan du laddar upp IdP-metadata i filen måste redigeras** att ta bort informationen om du vill säkerställa att programmet fungerar mellan Azure AD och Qlik mening server.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-217">**Before uploading the IdP metadata, the file needs to be edited** to remove information to ensure proper operation between Azure AD and Qlik Sense server.</span></span>  <span data-ttu-id="a8fc5-218">**Se anvisningarna ovan om filen är ännu som ska redigeras.**</span><span class="sxs-lookup"><span data-stu-id="a8fc5-218">**Please refer to the instructions above if the file has yet to be edited.**</span></span>  <span data-ttu-id="a8fc5-219">Om filen har redigerats klickar du på knappen Bläddra och välj den redigerade metadatafilen att överföra den till den virtuella proxykonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-219">If the file has been edited click on the Browse button and select the edited metadata file to upload it to the virtual proxy configuration.</span></span>
    
    <span data-ttu-id="a8fc5-220">f.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-220">f.</span></span> <span data-ttu-id="a8fc5-221">Ange namn eller schemat attributreferensen för SAML-attributet representerar den **UserID** Azure AD skickar till servern Qlik mening.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-221">Enter the attribute name or schema reference for the SAML attribute representing the **UserID** Azure AD sends to the Qlik Sense server.</span></span>  <span data-ttu-id="a8fc5-222">Referensinformation för schemat finns i Azure app skärmar efter konfiguration.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-222">Schema reference information is available in the Azure app screens post configuration.</span></span>  <span data-ttu-id="a8fc5-223">Om du vill använda namnattributet ange `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-223">To use the name attribute, enter `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span></span>
    
    <span data-ttu-id="a8fc5-224">g.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-224">g.</span></span> <span data-ttu-id="a8fc5-225">Ange värdet för den **användarkatalog** som ska anslutas till användarna när de autentiserar till Qlik mening server via Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-225">Enter the value for the **user directory** that will be attached to users when they authenticate to Qlik Sense server through Azure AD.</span></span>  <span data-ttu-id="a8fc5-226">Hårdkodad värden måste omges av **hakparentes []**.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-226">Hardcoded values must be surrounded by **square brackets []**.</span></span>  <span data-ttu-id="a8fc5-227">Om du vill använda ett attribut som skickas i Azure AD SAML-kontroll, ange namnet på attributet i den här textrutan **utan** hakparenteser.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-227">To use an attribute sent in the Azure AD SAML assertion, enter the name of the attribute in this text box **without** square brackets.</span></span>
    
    <span data-ttu-id="a8fc5-228">h.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-228">h.</span></span> <span data-ttu-id="a8fc5-229">Den **SAML Signeringsalgoritm** anger service provider (i det här fallet Qlik mening server) om certifikatsignering för den virtuella proxykonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-229">The **SAML signing algorithm** sets the service provider (in this case Qlik Sense server) certificate signing for the virtual proxy configuration.</span></span>  <span data-ttu-id="a8fc5-230">Om Qlik mening servern använder ett betrott certifikat som genereras med hjälp av Microsoft Enhanced RSA och AES Cryptographic Provider, ändra SAML Signeringsalgoritm till **SHA-256**.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-230">If Qlik Sense server uses a trusted certificate generated using Microsoft Enhanced RSA and AES Cryptographic Provider, change the SAML signing algorithm to **SHA-256**.</span></span>
    
    <span data-ttu-id="a8fc5-231">Jag.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-231">i.</span></span> <span data-ttu-id="a8fc5-232">Mappning med avsnittet SAML attribut kan för ytterligare attribut som grupper som ska skickas till Qlik mening för användning i säkerhetsregler.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-232">The SAML attribute mapping section allows for additional attributes like groups to be sent to Qlik Sense for use in security rules.</span></span>

13. <span data-ttu-id="a8fc5-233">Klicka på den **BELASTNINGSUTJÄMNING** att göra den synlig.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-233">Click on the **LOAD BALANCING** menu option to make it visible.</span></span>  <span data-ttu-id="a8fc5-234">Skärmbilden belastningsutjämning visas.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-234">The Load Balancing screen appears.</span></span>
    
    ![QlikSense][qs11]

14. <span data-ttu-id="a8fc5-236">Klicka på den **Lägg till ny servernoden** knappen, Välj motorn eller flera andra noder Qlik mening skickar sessioner för Utjämning av nätverksbelastning ändamål och klicka på den **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-236">Click on the **Add new server node** button, select engine node or nodes Qlik Sense will send sessions to for load balancing purposes, and click the **Add** button.</span></span>
    
    ![QlikSense][qs12]

15. <span data-ttu-id="a8fc5-238">Klicka på menyalternativet avancerade att göra den synlig.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-238">Click on the Advanced menu option to make it visible.</span></span> <span data-ttu-id="a8fc5-239">Skärmbilden visas.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-239">The Advanced screen appears.</span></span>
    
    ![QlikSense][qs13]
    
    <span data-ttu-id="a8fc5-241">Värden vitlista identifierar värdnamn som accepteras när du ansluter till servern Qlik mening.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-241">The Host white list identifies hostnames that are accepted when connecting to the Qlik Sense server.</span></span>  <span data-ttu-id="a8fc5-242">**Ange värdnamnet användarna anger vid anslutning till Qlik mening server.**</span><span class="sxs-lookup"><span data-stu-id="a8fc5-242">**Enter the hostname users will specify when connecting to Qlik Sense server.**</span></span> <span data-ttu-id="a8fc5-243">Värdnamnet är samma värde som URI: n för SAML-värden utan https://.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-243">The hostname is the same value as the SAML host uri without the https://.</span></span>

16. <span data-ttu-id="a8fc5-244">Klicka på den **tillämpa** knappen.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-244">Click the **Apply** button.</span></span>
    
    ![QlikSense][qs14]

17. <span data-ttu-id="a8fc5-246">Klicka på OK om du vill acceptera varningsmeddelandet om proxyservrar som är kopplad till virtuella proxyservern kommer att startas om.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-246">Click OK to accept the warning message that states proxies linked to the virtual proxy will be restarted.</span></span>
    
    ![QlikSense][qs15]

18. <span data-ttu-id="a8fc5-248">Associerade objekt menyn visas till höger på skärmen.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-248">On the right side of the screen, the Associated items menu appears.</span></span>  <span data-ttu-id="a8fc5-249">Klicka på den **proxyservrar** menyalternativet.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-249">Click on the **Proxies** menu option.</span></span>
    
    ![QlikSense][qs16]

19. <span data-ttu-id="a8fc5-251">Proxy visas.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-251">The proxy screen appears.</span></span>  <span data-ttu-id="a8fc5-252">Klicka på den **länk** längst ned att länka en proxy till virtuella proxy.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-252">Click the **Link** button at the bottom to link a proxy to the virtual proxy.</span></span>
    
    ![QlikSense][qs17]

20. <span data-ttu-id="a8fc5-254">Välj den proxy-nod som stöder den här virtuella proxyanslutningen och klicka på den **länk** knappen.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-254">Select the proxy node that will support this virtual proxy connection and click the **Link** button.</span></span>  <span data-ttu-id="a8fc5-255">När du länkar, visas proxy under associerade proxyservrar.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-255">After linking, the proxy will be listed under associated proxies.</span></span>
    
    ![QlikSense][qs18]
  
    ![QlikSense][qs19]

21. <span data-ttu-id="a8fc5-258">Uppdatera QMC meddelandet visas efter fem till tio sekunder.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-258">After about five to ten seconds, the Refresh QMC message will appear.</span></span>  <span data-ttu-id="a8fc5-259">Klicka på den **uppdatera QMC** knappen.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-259">Click the **Refresh QMC** button.</span></span>
    
    ![QlikSense][qs20]

22. <span data-ttu-id="a8fc5-261">När QMC uppdaterar, klicka på den **virtuella proxyservrar** menyalternativet.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-261">When the QMC refreshes, click on the **Virtual proxies** menu item.</span></span> <span data-ttu-id="a8fc5-262">Den nya SAML virtuella proxy posten visas i tabellen på skärmen.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-262">The new SAML virtual proxy entry is listed in the table on the screen.</span></span>  <span data-ttu-id="a8fc5-263">Klicka på posten virtuella proxy.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-263">Single click on the virtual proxy entry.</span></span>
    
    ![QlikSense][qs51]

23. <span data-ttu-id="a8fc5-265">Längst ned på skärmen aktiveras knappen ladda ned SP metadata.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-265">At the bottom of the screen, the Download SP metadata button will activate.</span></span>  <span data-ttu-id="a8fc5-266">Klicka på den **hämta SP metadata** för att spara metadata till en fil.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-266">Click the **Download SP metadata** button to save the metadata to a file.</span></span>
    
    ![QlikSense][qs52]

24. <span data-ttu-id="a8fc5-268">Öppna filen sp metadata.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-268">Open the sp metadata file.</span></span>  <span data-ttu-id="a8fc5-269">Se den **ID för entiteterna** transaktionen och **AssertionConsumerService** transaktionen.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-269">Observe the **entityID** entry and the **AssertionConsumerService** entry.</span></span>  <span data-ttu-id="a8fc5-270">Värdena är likvärdiga med den **identifierare** och **inloggning URL** i Azure AD tillämpningsprogrammets konfiguration.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-270">These values are equivalent to the **Identifier** and the **Sign on URL** in the Azure AD application configuration.</span></span> <span data-ttu-id="a8fc5-271">Klistra in dessa värden i den **Qlik mening företagsdomänen och URL: er** avsnitt i Azure AD-programkonfigurationen om de matchar inte varandra, och sedan bör du ersätta dem i guiden för konfiguration av Azure AD-App.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-271">Paste these values in the **Qlik Sense Enterprise Domain and URLs** section in the Azure AD application configuration if they are not matching, then you should replace them in the Azure AD App configuration wizard.</span></span>
    
    ![QlikSense][qs53]

> [!TIP]
> <span data-ttu-id="a8fc5-273">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="a8fc5-273">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="a8fc5-274">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-274">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="a8fc5-275">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a8fc5-275">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="a8fc5-276">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="a8fc5-276">Create an Azure AD test user</span></span>
<span data-ttu-id="a8fc5-277">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-277">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="a8fc5-279">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="a8fc5-279">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="a8fc5-280">I Azure-portalen i den vänstra rutan klickar du på den **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-280">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

   ![Azure Active Directory-knappen](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="a8fc5-282">Om du vill visa en lista över användare, gå till **användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-282">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

   ![”Användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="a8fc5-284">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i den **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-284">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

   ![Knappen Lägg till](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="a8fc5-286">I den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="a8fc5-286">In the **User** dialog box, perform the following steps:</span></span>

   ![Dialogrutan användare](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_04.png)

   <span data-ttu-id="a8fc5-288">a.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-288">a.</span></span> <span data-ttu-id="a8fc5-289">I den **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-289">In the **Name** box, type **BrittaSimon**.</span></span>

   <span data-ttu-id="a8fc5-290">b.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-290">b.</span></span> <span data-ttu-id="a8fc5-291">I den **användarnamn** Skriv användarens Britta Simon e-postadress.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-291">In the **User name** box, type the email address of user Britta Simon.</span></span>

   <span data-ttu-id="a8fc5-292">c.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-292">c.</span></span> <span data-ttu-id="a8fc5-293">Välj den **visa lösenordet** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-293">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

   <span data-ttu-id="a8fc5-294">d.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-294">d.</span></span> <span data-ttu-id="a8fc5-295">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-295">Click **Create**.</span></span>
 
### <a name="create-a-qlik-sense-enterprise-test-user"></a><span data-ttu-id="a8fc5-296">Skapa en testanvändare Qlik mening Enterprise</span><span class="sxs-lookup"><span data-stu-id="a8fc5-296">Create a Qlik Sense Enterprise test user</span></span>

<span data-ttu-id="a8fc5-297">I det här avsnittet kan du skapa en användare som kallas Britta Simon i Qlik mening Enterprise.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-297">In this section, you create a user called Britta Simon in Qlik Sense Enterprise.</span></span> <span data-ttu-id="a8fc5-298">Arbeta med [Qlik mening Enterprise-klienten supportteamet](https://www.qlik.com/us/services/support) att lägga till användare i Qlik mening Enterprise-plattformen.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-298">Work with [Qlik Sense Enterprise Client support team](https://www.qlik.com/us/services/support) to add the users in the Qlik Sense Enterprise platform.</span></span> <span data-ttu-id="a8fc5-299">Användare måste skapas och aktiveras innan du använder enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-299">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="a8fc5-300">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="a8fc5-300">Assign the Azure AD test user</span></span>

<span data-ttu-id="a8fc5-301">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Qlik mening Enterprise.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-301">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Qlik Sense Enterprise.</span></span>

![Tilldela rollen][200] 

<span data-ttu-id="a8fc5-303">**Om du vill tilldela Qlik mening Enterprise Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a8fc5-303">**To assign Britta Simon to Qlik Sense Enterprise, perform the following steps:**</span></span>

1. <span data-ttu-id="a8fc5-304">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-304">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="a8fc5-306">Välj i listan med program **Qlik mening Enterprise**.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-306">In the applications list, select **Qlik Sense Enterprise**.</span></span>

    ![Länken Qlik mening Enterprise i listan med program](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_app.png)  

3. <span data-ttu-id="a8fc5-308">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-308">In the menu on the left, click **Users and groups**.</span></span>

    ![Länken ”användare och grupper”][202]

4. <span data-ttu-id="a8fc5-310">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-310">Click **Add** button.</span></span> <span data-ttu-id="a8fc5-311">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-311">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Fönstret Lägg till tilldelning][203]

5. <span data-ttu-id="a8fc5-313">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-313">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="a8fc5-314">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-314">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a8fc5-315">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-315">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="a8fc5-316">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a8fc5-316">Test single sign-on</span></span>

<span data-ttu-id="a8fc5-317">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-317">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="a8fc5-318">När du klickar på panelen Qlik mening Enterprise på åtkomstpanelen du ska hämta automatiskt loggat in på ditt Qlik mening företagsprogram.</span><span class="sxs-lookup"><span data-stu-id="a8fc5-318">When you click the Qlik Sense Enterprise tile in the Access Panel, you should get automatically signed-on to your Qlik Sense Enterprise application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="a8fc5-319">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="a8fc5-319">Additional resources</span></span>

* [<span data-ttu-id="a8fc5-320">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a8fc5-320">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a8fc5-321">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a8fc5-321">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_203.png

[qs6]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_06.png
[qs7]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_07.png
[qs8]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_08.png
[qs9]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_09.png
[qs10]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_10.png
[qs11]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_11.png
[qs12]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_12.png
[qs13]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_13.png
[qs14]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_14.png
[qs15]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_15.png
[qs16]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_16.png
[qs17]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_17.png
[qs18]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_18.png
[qs19]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_19.png
[qs20]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_20.png
[qs24]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_24.png
[qs51]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_51.png
[qs52]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_52.png
[qs53]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_53.png

