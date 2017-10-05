---
title: "Självstudier: Azure Active Directory-integrering med framför | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och fram."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 88270b6d-2571-434a-b139-b6ccc3a2b19f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: d936bc50a66ac2a3c17038ff08351edf9902c99f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-front"></a><span data-ttu-id="e42d4-103">Självstudier: Azure Active Directory-integrering med framför</span><span class="sxs-lookup"><span data-stu-id="e42d4-103">Tutorial: Azure Active Directory integration with Front</span></span>

<span data-ttu-id="e42d4-104">I kursen får lära du att integrera framför med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="e42d4-104">In this tutorial, you learn how to integrate Front with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e42d4-105">Integrera framför med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="e42d4-105">Integrating Front with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="e42d4-106">Du kan styra i Azure AD som har åtkomst till fram.</span><span class="sxs-lookup"><span data-stu-id="e42d4-106">You can control in Azure AD who has access to Front.</span></span>
- <span data-ttu-id="e42d4-107">Du kan aktivera användarna att automatiskt hämta inloggade längst fram (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="e42d4-107">You can enable your users to automatically get signed-on to Front (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="e42d4-108">Du kan hantera dina konton i en central plats - Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e42d4-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="e42d4-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e42d4-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e42d4-110">Krav</span><span class="sxs-lookup"><span data-stu-id="e42d4-110">Prerequisites</span></span>

<span data-ttu-id="e42d4-111">Om du vill konfigurera Azure AD-integrering med framför behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="e42d4-111">To configure Azure AD integration with Front, you need the following items:</span></span>

- <span data-ttu-id="e42d4-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="e42d4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e42d4-113">En framför enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="e42d4-113">A Front single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e42d4-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="e42d4-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e42d4-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="e42d4-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e42d4-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="e42d4-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e42d4-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e42d4-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e42d4-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="e42d4-118">Scenario description</span></span>
<span data-ttu-id="e42d4-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="e42d4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e42d4-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="e42d4-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e42d4-121">Att lägga till framför från galleriet</span><span class="sxs-lookup"><span data-stu-id="e42d4-121">Adding Front from the gallery</span></span>
2. <span data-ttu-id="e42d4-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e42d4-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-front-from-the-gallery"></a><span data-ttu-id="e42d4-123">Att lägga till framför från galleriet</span><span class="sxs-lookup"><span data-stu-id="e42d4-123">Adding Front from the gallery</span></span>
<span data-ttu-id="e42d4-124">Du måste lägga till framför från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av framför i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e42d4-124">To configure the integration of Front into Azure AD, you need to add Front from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="e42d4-125">**Utför följande steg för att lägga till framför från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="e42d4-125">**To add Front from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="e42d4-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="e42d4-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Azure Active Directory-knappen][1]

2. <span data-ttu-id="e42d4-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="e42d4-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="e42d4-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="e42d4-129">Then go to **All applications**.</span></span>

    ![Bladet Enterprise program][2]
    
3. <span data-ttu-id="e42d4-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e42d4-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Knappen Nytt program][3]

4. <span data-ttu-id="e42d4-133">I sökrutan skriver **främre**väljer **främre** resultatet-panelen klickar **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="e42d4-133">In the search box, type **Front**, select **Front** from result panel then click **Add** button to add the application.</span></span>

    ![Framför i resultatlistan](./media/active-directory-saas-front-tutorial/tutorial_front_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="e42d4-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e42d4-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="e42d4-136">I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med framför baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="e42d4-136">In this section, you configure and test Azure AD single sign-on with Front based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e42d4-137">Azure AD måste du känna till motsvarande användaren fram till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="e42d4-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Front is to a user in Azure AD.</span></span> <span data-ttu-id="e42d4-138">Med andra ord måste en länk mellan en Azure AD-användare och relaterade användaren fram upprättas.</span><span class="sxs-lookup"><span data-stu-id="e42d4-138">In other words, a link relationship between an Azure AD user and the related user in Front needs to be established.</span></span>

<span data-ttu-id="e42d4-139">Framför, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="e42d4-139">In Front, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="e42d4-140">Om du vill konfigurera och testa Azure AD enkel inloggning med framför, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="e42d4-140">To configure and test Azure AD single sign-on with Front, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="e42d4-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="e42d4-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="e42d4-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e42d4-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e42d4-143">**[Skapa en testanvändare framför](#create-a-front-test-user)**  – du har en motsvarighet för Britta Simon fram som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="e42d4-143">**[Create a Front test user](#create-a-front-test-user)** - to have a counterpart of Britta Simon in Front that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="e42d4-144">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e42d4-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e42d4-145">**[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="e42d4-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="e42d4-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e42d4-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="e42d4-147">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt program fram.</span><span class="sxs-lookup"><span data-stu-id="e42d4-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Front application.</span></span>

<span data-ttu-id="e42d4-148">**Utför följande steg för att konfigurera Azure AD enkel inloggning med framför:**</span><span class="sxs-lookup"><span data-stu-id="e42d4-148">**To configure Azure AD single sign-on with Front, perform the following steps:**</span></span>

1. <span data-ttu-id="e42d4-149">I Azure-portalen på den **främre** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="e42d4-149">In the Azure portal, on the **Front** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="e42d4-151">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e42d4-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-front-tutorial/tutorial_front_samlbase.png)

3. <span data-ttu-id="e42d4-153">På den **främre domän och URL: er** om du vill konfigurera programmet i **IDP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="e42d4-153">On the **Front Domain and URLs** section, If you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-front-tutorial/tutorial_front_url1.png)

    <span data-ttu-id="e42d4-155">a.</span><span class="sxs-lookup"><span data-stu-id="e42d4-155">a.</span></span> <span data-ttu-id="e42d4-156">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<companyname>.frontapp.com`</span><span class="sxs-lookup"><span data-stu-id="e42d4-156">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.frontapp.com`</span></span>

    <span data-ttu-id="e42d4-157">b.</span><span class="sxs-lookup"><span data-stu-id="e42d4-157">b.</span></span> <span data-ttu-id="e42d4-158">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://<companyname>.frontapp.com/sso/saml/callback`</span><span class="sxs-lookup"><span data-stu-id="e42d4-158">In the **Reply URL** textbox, type a URL using the following pattern: `https://<companyname>.frontapp.com/sso/saml/callback`</span></span>

4. <span data-ttu-id="e42d4-159">Kontrollera **visa avancerade inställningar för URL: en**, om du vill konfigurera programmet i **SP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="e42d4-159">Check **Show advanced URL settings**, If you wish to configure the application in **SP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-front-tutorial/tutorial_front_url2.png)

    <span data-ttu-id="e42d4-161">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<companyname>.frontapp.com`</span><span class="sxs-lookup"><span data-stu-id="e42d4-161">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.frontapp.com`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="e42d4-162">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="e42d4-162">These values are not real.</span></span> <span data-ttu-id="e42d4-163">Uppdatera dessa värden med den faktiska identifierare, Reply URL och inloggnings-URL som beskrivs senare i självstudiekursen eller kontakta [främre klienten supportteamet](mailto:support@frontapp.com) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="e42d4-163">Update these values with the actual Identifier, Reply URL, and Sign-On URL which are explained later in tutorial or contact [Front Client support team](mailto:support@frontapp.com) to get these values.</span></span> 

5. <span data-ttu-id="e42d4-164">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="e42d4-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-front-tutorial/tutorial_front_certificate.png) 

6. <span data-ttu-id="e42d4-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="e42d4-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-front-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="e42d4-168">På den **främre Configuration** klickar du på **konfigurera främre** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="e42d4-168">On the **Front Configuration** section, click **Configure Front** to open **Configure sign-on** window.</span></span> <span data-ttu-id="e42d4-169">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="e42d4-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-front-tutorial/tutorial_front_configure.png) 

8. <span data-ttu-id="e42d4-171">Inloggning till framför-klient som administratör.</span><span class="sxs-lookup"><span data-stu-id="e42d4-171">Sign-on to your Front tenant as an administrator.</span></span>

9. <span data-ttu-id="e42d4-172">Gå till **inställningar (kugghjulet ikonen längst ned i den vänstra sidopanelen) > Inställningar**.</span><span class="sxs-lookup"><span data-stu-id="e42d4-172">Go to **Settings (cog icon at the bottom of the left sidebar) > Preferences**.</span></span>
   
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-front-tutorial/tutorial_front_000.png)

10. <span data-ttu-id="e42d4-174">Klicka på **enkel inloggning** länk.</span><span class="sxs-lookup"><span data-stu-id="e42d4-174">Click **Single Sign On** link.</span></span>
   
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-front-tutorial/tutorial_front_001.png)

11. <span data-ttu-id="e42d4-176">Välj **SAML** i listrutan för **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="e42d4-176">Select **SAML** in the drop-down list of **Single Sign On**.</span></span>
   
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-front-tutorial/tutorial_front_002.png)

12. <span data-ttu-id="e42d4-178">I den **startpunkten** textruta ange värdet för **inloggning tjänst-URL för enkel** från guiden Konfigurera program för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e42d4-178">In the **Entry Point** textbox put the value of **Single Sign-on Service URL** from Azure AD application configuration wizard.</span></span>
    
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-front-tutorial/tutorial_front_003.png)

13. <span data-ttu-id="e42d4-180">Öppna din hämtade **Certificate(Base64)** fil i anteckningar, kopiera innehållet i den till Urklipp och klistra in den till den **signeringscertifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="e42d4-180">Open your downloaded **Certificate(Base64)** file in notepad, copy the content of it into your clipboard, and then paste it to the **Signing certificate** textbox.</span></span>
    
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-front-tutorial/tutorial_front_004.png)

14. <span data-ttu-id="e42d4-182">På den **tjänstinställningar providern** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="e42d4-182">On the **Service provider settings** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-front-tutorial/tutorial_front_005.png)

    <span data-ttu-id="e42d4-184">a.</span><span class="sxs-lookup"><span data-stu-id="e42d4-184">a.</span></span> <span data-ttu-id="e42d4-185">Kopiera värdet för **enhets-ID** och klistrar in det i den **identifierare** TextBox-kontroll i **främre domän och URL: er** avsnitt i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e42d4-185">Copy the value of **Entity ID** and paste it into the **Identifier** textbox in **Front Domain and URLs** section in Azure portal.</span></span>

    <span data-ttu-id="e42d4-186">b.</span><span class="sxs-lookup"><span data-stu-id="e42d4-186">b.</span></span> <span data-ttu-id="e42d4-187">Kopiera värdet för **ACS URL** och klistrar in det i den **inloggnings-URL** TextBox-kontroll i **främre domän och URL: er** avsnitt i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e42d4-187">Copy the value of **ACS URL** and paste it into the **Sign-on URL** textbox in **Front Domain and URLs** section in Azure portal.</span></span>
    
15. <span data-ttu-id="e42d4-188">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="e42d4-188">Click **Save** button.</span></span>

> [!TIP]
> <span data-ttu-id="e42d4-189">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="e42d4-189">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="e42d4-190">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="e42d4-190">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="e42d4-191">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e42d4-191">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="e42d4-192">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="e42d4-192">Create an Azure AD test user</span></span>

<span data-ttu-id="e42d4-193">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e42d4-193">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="e42d4-195">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="e42d4-195">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="e42d4-196">I Azure-portalen i den vänstra rutan klickar du på den **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="e42d4-196">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Azure Active Directory-knappen](./media/active-directory-saas-front-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="e42d4-198">Om du vill visa en lista över användare, gå till **användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="e42d4-198">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![”Användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-front-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="e42d4-200">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i den **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e42d4-200">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Knappen Lägg till](./media/active-directory-saas-front-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="e42d4-202">I den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="e42d4-202">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogrutan användare](./media/active-directory-saas-front-tutorial/create_aaduser_04.png)

    <span data-ttu-id="e42d4-204">a.</span><span class="sxs-lookup"><span data-stu-id="e42d4-204">a.</span></span> <span data-ttu-id="e42d4-205">I den **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e42d4-205">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e42d4-206">b.</span><span class="sxs-lookup"><span data-stu-id="e42d4-206">b.</span></span> <span data-ttu-id="e42d4-207">I den **användarnamn** Skriv användarens Britta Simon e-postadress.</span><span class="sxs-lookup"><span data-stu-id="e42d4-207">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="e42d4-208">c.</span><span class="sxs-lookup"><span data-stu-id="e42d4-208">c.</span></span> <span data-ttu-id="e42d4-209">Välj den **visa lösenordet** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="e42d4-209">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="e42d4-210">d.</span><span class="sxs-lookup"><span data-stu-id="e42d4-210">d.</span></span> <span data-ttu-id="e42d4-211">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="e42d4-211">Click **Create**.</span></span>
 
### <a name="create-a-front-test-user"></a><span data-ttu-id="e42d4-212">Skapa en testanvändare framför</span><span class="sxs-lookup"><span data-stu-id="e42d4-212">Create a Front test user</span></span>

<span data-ttu-id="e42d4-213">I det här avsnittet skapar du en användare som kallas Britta Simon fram.</span><span class="sxs-lookup"><span data-stu-id="e42d4-213">In this section, you create a user called Britta Simon in Front.</span></span> <span data-ttu-id="e42d4-214">Arbeta med [främre klienten supportteamet](mailto:support@frontapp.com) att lägga till användare i fram-plattformen.</span><span class="sxs-lookup"><span data-stu-id="e42d4-214">Work with [Front Client support team](mailto:support@frontapp.com) to add the users in the Front platform.</span></span> <span data-ttu-id="e42d4-215">Användare måste skapas och aktiveras innan du använder enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e42d4-215">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="e42d4-216">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="e42d4-216">Assign the Azure AD test user</span></span>

<span data-ttu-id="e42d4-217">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till fram.</span><span class="sxs-lookup"><span data-stu-id="e42d4-217">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Front.</span></span>

![Tilldela rollen][200] 

<span data-ttu-id="e42d4-219">**Om du vill tilldela framför Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="e42d4-219">**To assign Britta Simon to Front, perform the following steps:**</span></span>

1. <span data-ttu-id="e42d4-220">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="e42d4-220">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="e42d4-222">Välj i listan med program **främre**.</span><span class="sxs-lookup"><span data-stu-id="e42d4-222">In the applications list, select **Front**.</span></span>

    ![Länken längst fram i listan med program](./media/active-directory-saas-front-tutorial/tutorial_front_app.png)  

3. <span data-ttu-id="e42d4-224">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="e42d4-224">In the menu on the left, click **Users and groups**.</span></span>

    ![Länken ”användare och grupper”][202]

4. <span data-ttu-id="e42d4-226">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="e42d4-226">Click **Add** button.</span></span> <span data-ttu-id="e42d4-227">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e42d4-227">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Fönstret Lägg till tilldelning][203]

5. <span data-ttu-id="e42d4-229">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="e42d4-229">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="e42d4-230">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e42d4-230">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e42d4-231">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e42d4-231">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="e42d4-232">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e42d4-232">Test single sign-on</span></span>

<span data-ttu-id="e42d4-233">Syftet med det här avsnittet är att testa din Azure AD-SSOconfiguration med åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="e42d4-233">The objective of this section is to test your Azure AD SSOconfiguration using the Access Panel.</span></span>

<span data-ttu-id="e42d4-234">När du klickar på panelen fram på åtkomstpanelen du ska hämta automatiskt loggat in på ditt program fram.</span><span class="sxs-lookup"><span data-stu-id="e42d4-234">When you click the Front tile in the Access Panel, you should get automatically signed-on to your Front application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="e42d4-235">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="e42d4-235">Additional resources</span></span>

* [<span data-ttu-id="e42d4-236">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e42d4-236">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e42d4-237">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e42d4-237">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-front-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-front-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-front-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-front-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-front-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-front-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-front-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-front-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-front-tutorial/tutorial_general_203.png

