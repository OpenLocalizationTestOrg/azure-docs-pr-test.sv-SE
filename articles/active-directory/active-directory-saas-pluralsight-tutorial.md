---
title: "Självstudier: Azure Active Directory-integrering med Pluralsight | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Pluralsight."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4c3f07d2-4e1f-4ea3-9025-c663f1f2b7b4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: 62429643a108665544e42001d264046b5db1ec97
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-pluralsight"></a><span data-ttu-id="0ac8d-103">Självstudier: Azure Active Directory-integrering med Pluralsight</span><span class="sxs-lookup"><span data-stu-id="0ac8d-103">Tutorial: Azure Active Directory integration with Pluralsight</span></span>

<span data-ttu-id="0ac8d-104">I kursen får lära du att integrera Pluralsight med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="0ac8d-104">In this tutorial, you learn how to integrate Pluralsight with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="0ac8d-105">Integrera Pluralsight med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="0ac8d-105">Integrating Pluralsight with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="0ac8d-106">Du kan styra i Azure AD som har åtkomst till Pluralsight</span><span class="sxs-lookup"><span data-stu-id="0ac8d-106">You can control in Azure AD who has access to Pluralsight</span></span>
- <span data-ttu-id="0ac8d-107">Du kan aktivera användarna att automatiskt hämta loggat in på Pluralsight (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="0ac8d-107">You can enable your users to automatically get signed-on to Pluralsight (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="0ac8d-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="0ac8d-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="0ac8d-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0ac8d-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0ac8d-110">Krav</span><span class="sxs-lookup"><span data-stu-id="0ac8d-110">Prerequisites</span></span>

<span data-ttu-id="0ac8d-111">För att konfigurera Azure AD-integrering med Pluralsight, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="0ac8d-111">To configure Azure AD integration with Pluralsight, you need the following items:</span></span>

- <span data-ttu-id="0ac8d-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="0ac8d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="0ac8d-113">En Pluralsight enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="0ac8d-113">A Pluralsight single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="0ac8d-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="0ac8d-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="0ac8d-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="0ac8d-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="0ac8d-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0ac8d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="0ac8d-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="0ac8d-118">Scenario description</span></span>
<span data-ttu-id="0ac8d-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="0ac8d-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="0ac8d-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="0ac8d-121">Att lägga till Pluralsight från galleriet</span><span class="sxs-lookup"><span data-stu-id="0ac8d-121">Adding Pluralsight from the gallery</span></span>
2. <span data-ttu-id="0ac8d-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0ac8d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-pluralsight-from-the-gallery"></a><span data-ttu-id="0ac8d-123">Att lägga till Pluralsight från galleriet</span><span class="sxs-lookup"><span data-stu-id="0ac8d-123">Adding Pluralsight from the gallery</span></span>
<span data-ttu-id="0ac8d-124">Du måste lägga till Pluralsight från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Pluralsight i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-124">To configure the integration of Pluralsight into Azure AD, you need to add Pluralsight from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="0ac8d-125">**Utför följande steg för att lägga till Pluralsight från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="0ac8d-125">**To add Pluralsight from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="0ac8d-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="0ac8d-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="0ac8d-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="0ac8d-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="0ac8d-133">I sökrutan skriver **Pluralsight**.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-133">In the search box, type **Pluralsight**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_search.png)

5. <span data-ttu-id="0ac8d-135">Välj i resultatpanelen **Pluralsight**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-135">In the results panel, select **Pluralsight**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="0ac8d-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0ac8d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="0ac8d-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Pluralsight baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-138">In this section, you configure and test Azure AD single sign-on with Pluralsight based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="0ac8d-139">Azure AD måste du känna till användaren i Pluralsight motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Pluralsight is to a user in Azure AD.</span></span> <span data-ttu-id="0ac8d-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Pluralsight upprättas.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-140">In other words, a link relationship between an Azure AD user and the related user in Pluralsight needs to be established.</span></span>

<span data-ttu-id="0ac8d-141">I Pluralsight, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-141">In Pluralsight, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="0ac8d-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Pluralsight, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="0ac8d-142">To configure and test Azure AD single sign-on with Pluralsight, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="0ac8d-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="0ac8d-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="0ac8d-145">**[Skapa en testanvändare Pluralsight](#creating-a-pluralsight-test-user)**  – du har en motsvarighet för Britta Simon i Pluralsight som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-145">**[Creating a Pluralsight test user](#creating-a-pluralsight-test-user)** - to have a counterpart of Britta Simon in Pluralsight that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="0ac8d-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="0ac8d-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="0ac8d-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0ac8d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="0ac8d-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Pluralsight program.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Pluralsight application.</span></span>

<span data-ttu-id="0ac8d-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Pluralsight:**</span><span class="sxs-lookup"><span data-stu-id="0ac8d-150">**To configure Azure AD single sign-on with Pluralsight, perform the following steps:**</span></span>

1. <span data-ttu-id="0ac8d-151">I Azure-portalen på den **Pluralsight** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-151">In the Azure portal, on the **Pluralsight** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="0ac8d-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_samlbase.png)

3. <span data-ttu-id="0ac8d-155">På den **Pluralsight domän och URL: er** avsnittet, utför följande:</span><span class="sxs-lookup"><span data-stu-id="0ac8d-155">On the **Pluralsight Domain and URLs** section, perform the following:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_url.png)

    <span data-ttu-id="0ac8d-157">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<instance name>.pluralsight.com/sso/<company name>`</span><span class="sxs-lookup"><span data-stu-id="0ac8d-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<instance name>.pluralsight.com/sso/<company name>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="0ac8d-158">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-158">This value is not real.</span></span> <span data-ttu-id="0ac8d-159">Uppdatera det här värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="0ac8d-160">Kontakta [Pluralsight klienten supportteamet](mailto:support@pluralsight.com) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-160">Contact [Pluralsight Client support team](mailto:support@pluralsight.com) to get this value.</span></span> 
 


4. <span data-ttu-id="0ac8d-161">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_certificate.png) 

5. <span data-ttu-id="0ac8d-163">Syftet med det här avsnittet är att aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i Pluralsight-programmet.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-163">The objective of this section is to enable Azure AD single sign-on in the Azure portal and to configure SSO in the Pluralsight application.</span></span>

    <span data-ttu-id="0ac8d-164">Programmet Pluralsight förväntar SAML-intyg i ett specifikt format, vilket kräver att du kan lägga till anpassade attributmappning konfigurationen för SAML-token attribut.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-164">The Pluralsight application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your SAML token attributes configuration.</span></span> <span data-ttu-id="0ac8d-165">Följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-165">The following screenshot shows an example for this.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_attribute.png)

    >[!NOTE]
    ><span data-ttu-id="0ac8d-167">Du kan också lägga till den **”unikt ID”** attribut med lämpligt värde som EmployeeID eller något annat som passar för din organisation.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-167">You can also add the **"Unique ID"** attribute with the appropriate value like EmployeeID or something else which suits for your organization.</span></span> <span data-ttu-id="0ac8d-168">Observera att detta inte är det obligatoriska attributet; Du kan dock lägga till den för att identifiera unika användare.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-168">Also note that this is not the required attribute; however, you can add it to  identify the unique user.</span></span> 

6. <span data-ttu-id="0ac8d-169">Att lägga till de nödvändiga **SAML-token attribut**, utför följande steg för varje rad som visas i tabellen nedan:</span><span class="sxs-lookup"><span data-stu-id="0ac8d-169">To add the required **SAML token attributes**, for each row shown in the table below, perform the following steps:</span></span>
   
   | <span data-ttu-id="0ac8d-170">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="0ac8d-170">Attribute Name</span></span> | <span data-ttu-id="0ac8d-171">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="0ac8d-171">Attribute Value</span></span> |
   | ---| --- |
   | <span data-ttu-id="0ac8d-172">Förnamn</span><span class="sxs-lookup"><span data-stu-id="0ac8d-172">First Name</span></span> |<span data-ttu-id="0ac8d-173">User.givenName</span><span class="sxs-lookup"><span data-stu-id="0ac8d-173">user.givenname</span></span> |
   | <span data-ttu-id="0ac8d-174">Efternamn</span><span class="sxs-lookup"><span data-stu-id="0ac8d-174">Last Name</span></span> |<span data-ttu-id="0ac8d-175">User.surname</span><span class="sxs-lookup"><span data-stu-id="0ac8d-175">user.surname</span></span> |
   | <span data-ttu-id="0ac8d-176">E-post</span><span class="sxs-lookup"><span data-stu-id="0ac8d-176">Email</span></span> |<span data-ttu-id="0ac8d-177">User.Mail</span><span class="sxs-lookup"><span data-stu-id="0ac8d-177">user.mail</span></span> |
   
   <span data-ttu-id="0ac8d-178">a.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-178">a.</span></span> <span data-ttu-id="0ac8d-179">Klicka på **lägga till användarattribut** att öppna den **lägga till användaren Attribure** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-179">Click **add user attribute** to open the **Add User Attribure** dialog.</span></span>
    
     ![Konfigurera enkel inloggning](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_addattribute.png)
  
   <span data-ttu-id="0ac8d-181">b.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-181">b.</span></span> <span data-ttu-id="0ac8d-182">I den **attributnamn** textruta ange attributets namn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-182">In the **Attribute Name** textbox, type the attribute name shown for that row.</span></span>
  
   <span data-ttu-id="0ac8d-183">c.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-183">c.</span></span> <span data-ttu-id="0ac8d-184">Från den **attributvärdet** väljer du det attributvärde som visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-184">From the **Attribute Value** list, select the attribute value shown for that row.</span></span>
  
   <span data-ttu-id="0ac8d-185">d.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-185">d.</span></span> <span data-ttu-id="0ac8d-186">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-186">Click **Ok**.</span></span>    

7. <span data-ttu-id="0ac8d-187">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-187">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pluralsight-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="0ac8d-189">För att få SSO konfigurerats för ditt program, kontakta [Pluralsight professionella tjänster](mailTo:professionalservices@pluralsight.com) grupp- och ange den hämtade metadata.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-189">To get SSO configured for your application, contact [Pluralsight Professional Services](mailTo:professionalservices@pluralsight.com) team and provide the downloaded metadata file.</span></span>

> [!TIP]
> <span data-ttu-id="0ac8d-190">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="0ac8d-190">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="0ac8d-191">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-191">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="0ac8d-192">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="0ac8d-192">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="0ac8d-193">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="0ac8d-193">Creating an Azure AD test user</span></span>
<span data-ttu-id="0ac8d-194">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-194">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="0ac8d-196">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="0ac8d-196">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="0ac8d-197">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-197">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="0ac8d-199">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-199">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="0ac8d-201">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-201">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="0ac8d-203">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="0ac8d-203">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="0ac8d-205">a.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-205">a.</span></span> <span data-ttu-id="0ac8d-206">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-206">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="0ac8d-207">b.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-207">b.</span></span> <span data-ttu-id="0ac8d-208">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-208">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="0ac8d-209">c.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-209">c.</span></span> <span data-ttu-id="0ac8d-210">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-210">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="0ac8d-211">d.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-211">d.</span></span> <span data-ttu-id="0ac8d-212">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-212">Click **Create**.</span></span>
 
### <a name="creating-a-pluralsight-test-user"></a><span data-ttu-id="0ac8d-213">Skapa en testanvändare Pluralsight</span><span class="sxs-lookup"><span data-stu-id="0ac8d-213">Creating a Pluralsight test user</span></span>

<span data-ttu-id="0ac8d-214">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i Pluralsight.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-214">The objective of this section is to create a user called Britta Simon in Pluralsight.</span></span> <span data-ttu-id="0ac8d-215">Se tillsammans med [Pluralsight klienten supportteamet](mailto:support@pluralsight.com) att lägga till användare i Pluralsight-konto.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-215">Please work with [Pluralsight Client support team](mailto:support@pluralsight.com) to add the users in the Pluralsight account.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="0ac8d-216">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="0ac8d-216">Assigning the Azure AD test user</span></span>

<span data-ttu-id="0ac8d-217">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Pluralsight.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-217">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Pluralsight.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="0ac8d-219">**Om du vill tilldela Pluralsight Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="0ac8d-219">**To assign Britta Simon to Pluralsight, perform the following steps:**</span></span>

1. <span data-ttu-id="0ac8d-220">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-220">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="0ac8d-222">Välj i listan med program **Pluralsight**.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-222">In the applications list, select **Pluralsight**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_app.png) 

3. <span data-ttu-id="0ac8d-224">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-224">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="0ac8d-226">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-226">Click **Add** button.</span></span> <span data-ttu-id="0ac8d-227">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-227">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="0ac8d-229">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-229">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="0ac8d-230">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-230">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="0ac8d-231">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-231">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="0ac8d-232">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0ac8d-232">Testing single sign-on</span></span>

<span data-ttu-id="0ac8d-233">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-233">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="0ac8d-234">När du klickar på panelen Pluralsight på åtkomstpanelen du bör få automatiskt loggat in på ditt Pluralsight program.</span><span class="sxs-lookup"><span data-stu-id="0ac8d-234">When you click the Pluralsight tile in the Access Panel, you should get automatically signed-on to your Pluralsight application.</span></span> <span data-ttu-id="0ac8d-235">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="0ac8d-235">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0ac8d-236">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="0ac8d-236">Additional resources</span></span>

* [<span data-ttu-id="0ac8d-237">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0ac8d-237">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0ac8d-238">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="0ac8d-238">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_203.png

