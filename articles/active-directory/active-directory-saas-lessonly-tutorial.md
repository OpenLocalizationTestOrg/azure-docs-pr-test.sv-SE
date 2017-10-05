---
title: "Självstudier: Azure Active Directory-integrering med Lesson.ly | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Lesson.ly."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8c9dc6e6-5d85-4553-8a35-c7137064b928
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: fc1e1b2de0a138dbe88d794f802b002321948ab8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lessonly"></a><span data-ttu-id="335a8-103">Självstudier: Azure Active Directory-integrering med Lesson.ly</span><span class="sxs-lookup"><span data-stu-id="335a8-103">Tutorial: Azure Active Directory integration with Lesson.ly</span></span>

<span data-ttu-id="335a8-104">I kursen får lära du att integrera Lesson.ly med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="335a8-104">In this tutorial, you learn how to integrate Lesson.ly with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="335a8-105">Integrera Lesson.ly med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="335a8-105">Integrating Lesson.ly with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="335a8-106">Du kan styra i Azure AD som har åtkomst till Lesson.ly</span><span class="sxs-lookup"><span data-stu-id="335a8-106">You can control in Azure AD who has access to Lesson.ly</span></span>
- <span data-ttu-id="335a8-107">Du kan aktivera användarna att automatiskt hämta loggat in på Lesson.ly (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="335a8-107">You can enable your users to automatically get signed-on to Lesson.ly (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="335a8-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="335a8-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="335a8-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="335a8-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="335a8-110">Krav</span><span class="sxs-lookup"><span data-stu-id="335a8-110">Prerequisites</span></span>

<span data-ttu-id="335a8-111">För att konfigurera Azure AD-integrering med Lesson.ly, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="335a8-111">To configure Azure AD integration with Lesson.ly, you need the following items:</span></span>

- <span data-ttu-id="335a8-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="335a8-112">An Azure AD subscription</span></span>
- <span data-ttu-id="335a8-113">En Lesson.ly enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="335a8-113">A Lesson.ly single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="335a8-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="335a8-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="335a8-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="335a8-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="335a8-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="335a8-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="335a8-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="335a8-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="335a8-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="335a8-118">Scenario description</span></span>
<span data-ttu-id="335a8-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="335a8-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="335a8-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="335a8-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="335a8-121">Att lägga till Lesson.ly från galleriet</span><span class="sxs-lookup"><span data-stu-id="335a8-121">Adding Lesson.ly from the gallery</span></span>
2. <span data-ttu-id="335a8-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="335a8-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-lessonly-from-the-gallery"></a><span data-ttu-id="335a8-123">Att lägga till Lesson.ly från galleriet</span><span class="sxs-lookup"><span data-stu-id="335a8-123">Adding Lesson.ly from the gallery</span></span>
<span data-ttu-id="335a8-124">Du måste lägga till Lesson.ly från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Lesson.ly i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="335a8-124">To configure the integration of Lesson.ly into Azure AD, you need to add Lesson.ly from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="335a8-125">**Utför följande steg för att lägga till Lesson.ly från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="335a8-125">**To add Lesson.ly from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="335a8-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="335a8-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="335a8-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="335a8-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="335a8-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="335a8-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="335a8-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="335a8-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="335a8-133">I sökrutan skriver **Lesson.ly**.</span><span class="sxs-lookup"><span data-stu-id="335a8-133">In the search box, type **Lesson.ly**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_search.png)

5. <span data-ttu-id="335a8-135">Välj i resultatpanelen **Lesson.ly**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="335a8-135">In the results panel, select **Lesson.ly**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="335a8-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="335a8-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="335a8-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Lesson.ly baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="335a8-138">In this section, you configure and test Azure AD single sign-on with Lesson.ly based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="335a8-139">Azure AD måste du känna till användaren i Lesson.ly motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="335a8-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Lesson.ly is to a user in Azure AD.</span></span> <span data-ttu-id="335a8-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Lesson.ly upprättas.</span><span class="sxs-lookup"><span data-stu-id="335a8-140">In other words, a link relationship between an Azure AD user and the related user in Lesson.ly needs to be established.</span></span>

<span data-ttu-id="335a8-141">I Lesson.ly, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="335a8-141">In Lesson.ly, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="335a8-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Lesson.ly, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="335a8-142">To configure and test Azure AD single sign-on with Lesson.ly, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="335a8-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="335a8-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="335a8-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="335a8-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="335a8-145">**[Skapa en testanvändare Lesson.ly](#creating-a-lessonly-test-user)**  – du har en motsvarighet för Britta Simon i Lesson.ly som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="335a8-145">**[Creating a Lesson.ly test user](#creating-a-lessonly-test-user)** - to have a counterpart of Britta Simon in Lesson.ly that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="335a8-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="335a8-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="335a8-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="335a8-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="335a8-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="335a8-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="335a8-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Lesson.ly program.</span><span class="sxs-lookup"><span data-stu-id="335a8-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Lesson.ly application.</span></span>

<span data-ttu-id="335a8-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Lesson.ly:**</span><span class="sxs-lookup"><span data-stu-id="335a8-150">**To configure Azure AD single sign-on with Lesson.ly, perform the following steps:**</span></span>

1. <span data-ttu-id="335a8-151">I Azure-portalen på den **Lesson.ly** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="335a8-151">In the Azure portal, on the **Lesson.ly** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="335a8-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="335a8-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_samlbase.png)

3. <span data-ttu-id="335a8-155">På den **Lesson.ly domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="335a8-155">On the **Lesson.ly Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_url.png)

    <span data-ttu-id="335a8-157">a.</span><span class="sxs-lookup"><span data-stu-id="335a8-157">a.</span></span> <span data-ttu-id="335a8-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:</span><span class="sxs-lookup"><span data-stu-id="335a8-158">In the **Sign-on URL** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.lesson.ly/signin`|
    | `https://<companyname>.lessonly.com/signin`|

    >[!NOTE]
    ><span data-ttu-id="335a8-159">När refererar till en allmän namn som **companyname** måste ersättas med verkliga-namnet.</span><span class="sxs-lookup"><span data-stu-id="335a8-159">When referencing a generic name that **companyname** needs to be replaced by an actual name.</span></span>
    
    <span data-ttu-id="335a8-160">b.</span><span class="sxs-lookup"><span data-stu-id="335a8-160">b.</span></span> <span data-ttu-id="335a8-161">I den **identifierare** textruta Skriv en URL med följande mönster:</span><span class="sxs-lookup"><span data-stu-id="335a8-161">In the **Identifier** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.lesson.ly/auth/saml/metadata`|
    | `https://<companyname>.lessonly.com/auth/saml/metadata`|

    > [!NOTE] 
    > <span data-ttu-id="335a8-162">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="335a8-162">These values are not real.</span></span> <span data-ttu-id="335a8-163">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="335a8-163">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="335a8-164">Kontakta [Lesson.ly klienten supportteamet](mailto:dev@lessonly.com) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="335a8-164">Contact [Lesson.ly Client support team](mailto:dev@lessonly.com) to get these values.</span></span> 

4. <span data-ttu-id="335a8-165">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="335a8-165">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_certificate.png)

5. <span data-ttu-id="335a8-167">Programmet Lesson.ly förväntar SAML-intyg i ett specifikt format, vilket kräver att du kan lägga till attributmappningar till din **SAML-Token attribut** konfiguration. Följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="335a8-167">The Lesson.ly application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your **SAML Token Attributes** configuration.The following screenshot shows an example for this.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_06.png)
           
6. <span data-ttu-id="335a8-169">I den **användarattribut** avsnitt på den **enkel inloggning** dialogrutan Konfigurera attribut för SAML-token som visas i den föregående bilden och utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="335a8-169">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the preceding image and perform the following steps:</span></span>

    | <span data-ttu-id="335a8-170">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="335a8-170">Attribute Name</span></span>   | <span data-ttu-id="335a8-171">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="335a8-171">Attribute Value</span></span> |
    | ---------------  | ----------------|
    | <span data-ttu-id="335a8-172">urn: oid:2.5.4.42</span><span class="sxs-lookup"><span data-stu-id="335a8-172">urn:oid:2.5.4.42</span></span> |<span data-ttu-id="335a8-173">User.givenName</span><span class="sxs-lookup"><span data-stu-id="335a8-173">user.givenname</span></span> |
    | <span data-ttu-id="335a8-174">urn: oid:2.5.4.4</span><span class="sxs-lookup"><span data-stu-id="335a8-174">urn:oid:2.5.4.4</span></span>  |<span data-ttu-id="335a8-175">User.surname</span><span class="sxs-lookup"><span data-stu-id="335a8-175">user.surname</span></span> |
    | <span data-ttu-id="335a8-176">urn: oid:0.9.2342.19200300.1001.3</span><span class="sxs-lookup"><span data-stu-id="335a8-176">urn:oid:0.9.2342.19200300.1001.3</span></span> |<span data-ttu-id="335a8-177">User.Mail</span><span class="sxs-lookup"><span data-stu-id="335a8-177">user.mail</span></span> |

    <span data-ttu-id="335a8-178">a.</span><span class="sxs-lookup"><span data-stu-id="335a8-178">a.</span></span> <span data-ttu-id="335a8-179">Klicka på **Lägg till attributet** att öppna den **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="335a8-179">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lessonly-tutorial/tutorial_attribute_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lessonly-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="335a8-182">b.</span><span class="sxs-lookup"><span data-stu-id="335a8-182">b.</span></span> <span data-ttu-id="335a8-183">I den **namn** textruta ange attributets namn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="335a8-183">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="335a8-184">c.</span><span class="sxs-lookup"><span data-stu-id="335a8-184">c.</span></span> <span data-ttu-id="335a8-185">Från den **värdet** listan, ange det attributvärde som visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="335a8-185">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="335a8-186">d.</span><span class="sxs-lookup"><span data-stu-id="335a8-186">d.</span></span> <span data-ttu-id="335a8-187">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="335a8-187">Click **Ok**.</span></span>     

7. <span data-ttu-id="335a8-188">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="335a8-188">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lessonly-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="335a8-190">På den **Lesson.ly Configuration** klickar du på **konfigurera Lesson.ly** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="335a8-190">On the **Lesson.ly Configuration** section, click **Configure Lesson.ly** to open **Configure sign-on** window.</span></span> <span data-ttu-id="335a8-191">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="335a8-191">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_configure.png)

9. <span data-ttu-id="335a8-193">Konfigurera enkel inloggning på **Lesson.ly** sida, måste du skicka den hämtade **Certificate(Base64)** och **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** till [Lesson.ly supportteamet](mailto:dev@lessonly.com).</span><span class="sxs-lookup"><span data-stu-id="335a8-193">To configure single sign-on on **Lesson.ly** side, you need to send the downloaded **Certificate(Base64)** and **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [Lesson.ly support team](mailto:dev@lessonly.com).</span></span>

> [!TIP]
> <span data-ttu-id="335a8-194">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="335a8-194">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="335a8-195">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="335a8-195">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="335a8-196">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="335a8-196">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="335a8-197">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="335a8-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="335a8-198">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="335a8-198">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="335a8-200">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="335a8-200">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="335a8-201">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="335a8-201">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lessonly-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="335a8-203">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="335a8-203">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lessonly-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="335a8-205">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="335a8-205">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lessonly-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="335a8-207">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="335a8-207">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lessonly-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="335a8-209">a.</span><span class="sxs-lookup"><span data-stu-id="335a8-209">a.</span></span> <span data-ttu-id="335a8-210">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="335a8-210">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="335a8-211">b.</span><span class="sxs-lookup"><span data-stu-id="335a8-211">b.</span></span> <span data-ttu-id="335a8-212">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="335a8-212">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="335a8-213">c.</span><span class="sxs-lookup"><span data-stu-id="335a8-213">c.</span></span> <span data-ttu-id="335a8-214">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="335a8-214">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="335a8-215">d.</span><span class="sxs-lookup"><span data-stu-id="335a8-215">d.</span></span> <span data-ttu-id="335a8-216">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="335a8-216">Click **Create**.</span></span>
 
### <a name="creating-a-lessonly-test-user"></a><span data-ttu-id="335a8-217">Skapa en testanvändare Lesson.ly</span><span class="sxs-lookup"><span data-stu-id="335a8-217">Creating a Lesson.ly test user</span></span>

<span data-ttu-id="335a8-218">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i Lesson.ly.</span><span class="sxs-lookup"><span data-stu-id="335a8-218">The objective of this section is to create a user called Britta Simon in Lesson.ly.</span></span> <span data-ttu-id="335a8-219">Lesson.LY stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="335a8-219">Lesson.ly supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="335a8-220">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="335a8-220">There is no action item for you in this section.</span></span> <span data-ttu-id="335a8-221">En ny användare skapas vid ett försök att komma åt Lesson.ly om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="335a8-221">A new user will be created during an attempt to access Lesson.ly if it doesn't exist yet.</span></span>

> [!NOTE]
> <span data-ttu-id="335a8-222">Om du behöver skapa en användare manuellt måste du kontakta den [Lesson.ly supportteamet](mailto:dev@lessonly.com).</span><span class="sxs-lookup"><span data-stu-id="335a8-222">If you need to create an user manually, you need to contact the [Lesson.ly support team](mailto:dev@lessonly.com).</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="335a8-223">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="335a8-223">Assigning the Azure AD test user</span></span>

<span data-ttu-id="335a8-224">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Lesson.ly.</span><span class="sxs-lookup"><span data-stu-id="335a8-224">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Lesson.ly.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="335a8-226">**Om du vill tilldela Lesson.ly Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="335a8-226">**To assign Britta Simon to Lesson.ly, perform the following steps:**</span></span>

1. <span data-ttu-id="335a8-227">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="335a8-227">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="335a8-229">Välj i listan med program **Lesson.ly**.</span><span class="sxs-lookup"><span data-stu-id="335a8-229">In the applications list, select **Lesson.ly**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lessonly-tutorial/tutorial_lesson.ly_app.png) 

3. <span data-ttu-id="335a8-231">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="335a8-231">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="335a8-233">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="335a8-233">Click **Add** button.</span></span> <span data-ttu-id="335a8-234">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="335a8-234">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="335a8-236">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="335a8-236">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="335a8-237">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="335a8-237">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="335a8-238">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="335a8-238">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="335a8-239">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="335a8-239">Testing single sign-on</span></span>

<span data-ttu-id="335a8-240">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="335a8-240">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="335a8-241">När du klickar på panelen Lesson.ly på åtkomstpanelen du bör få automatiskt loggat in på ditt Lesson.ly program.</span><span class="sxs-lookup"><span data-stu-id="335a8-241">When you click the Lesson.ly tile in the Access Panel, you should get automatically signed-on to your Lesson.ly application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="335a8-242">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="335a8-242">Additional resources</span></span>

* [<span data-ttu-id="335a8-243">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="335a8-243">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="335a8-244">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="335a8-244">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_203.png

